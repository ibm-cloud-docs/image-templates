---

copyright:
  years: 2018
lastupdated: "2018-09-19"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}


# 暗号化イメージの作成

E2E 暗号化機能の一部として、イメージを暗号化してイメージ・テンプレートにインポートし、そのイメージを使用して、暗号化された仮想サーバー・インスタンスをデプロイすることができます。
{:shortdesc}

## 暗号化イメージの要件
{: #encrypted-image-reqs}

以下のイメージ要件を満たす暗号化イメージを作成する必要があります。

* イメージは {{site.data.keyword.cloud}} コンソール・インフラストラクチャー環境に対応している。
* イメージには Linux オペレーティング・システム (CentOS、Debian、Red Hat Enterprise Linux、Ubuntu など) が含まれている。
* イメージは cloud-init に対応している。
* イメージは [LUKS ディスク暗号化](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption)によって暗号化されている。

## QEMU および DM-Crypt を使用して暗号化 RAW イメージを作成する
{: #luks-disk-encryption}

イメージを暗号化するには、容量固定 VHD イメージ・ファイルを RAW 形式に変換する必要があります。その後、QEMU および DM-Crypt を使用して、LUKS ディスク暗号化を適用した新規イメージ・ファイルを作成します。そのファイルを暗号化ボリュームとしてマウントし、非暗号化イメージ・ファイルをその暗号化ボリュームにコピーします。

この手順では、以下の作業について説明します。

* 容量可変 VHD イメージを容量固定 VHD イメージに変換する。
* 容量固定 VHD イメージを RAW ファイル形式に変換する。
* ドライブの暗号化に使用するデータ暗号化鍵ファイルを作成する。
* イメージと LUKS 暗号化ヘッダーを入れるための新規 RAW ファイルを作成する。
* その RAW ファイルを LUKS ディスク暗号化を使用してフォーマットする。
* 暗号化した RAW ファイルをブロック・デバイスに接続する。
* 非暗号化イメージを暗号化ボリューム・デバイスにコピーする。

システムで暗号化ボリュームをマウントおよびアンマウントするために、sudo で `root` ユーザー権限を使用してコマンドを実行する特権が必要です。以下のコマンドを実行すると、自分の `root` ユーザー特権を確認できます。

```
sudo echo "Hello!"
```
{: pre}

この暗号化作業を実行するには、応答として "Hello!" がエコー出力されなければなりません。

**ヒント**: この暗号化作業は、Linux オペレーティング・システムを実行していて、以下のパッケージを利用できるシステムで実行する必要があります。
* qemu または qemu-image (Linux オペレーティング・システムに応じてどちらか)
* cryptsetup

イメージを暗号化するには、以下の手順を実行します。

1. 容量可変 VHD イメージ・ファイルを容量固定 VHD イメージ・ファイルに変換します。容量可変 VHD を直接 RAW ファイル形式に変換すると破損する可能性があるので、容量固定 VHD ファイルに変換します。以下のコマンドを実行して、容量可変 VHD イメージ・ファイルを容量固定 VHD イメージ・ファイルに変換します。

  ```
  qemu-img convert -p -O vpc -o subformat=fixed <DYNAMIC_VHD_FILE> <FIXED_VHD_FILE>
  ```
  {: pre}

  例えば、容量可変 VHD ファイルが _Rhel_7.vhd-0.vhd_ である場合に、容量固定 VHD ファイルの名前を _Rhel_7.fixed.vhd-0.vhd_ にするには、以下のコマンドを実行します。

  ```
  $ qemu-img convert -p -O vpc -o subformat=fixed Rhel_7.vhd-0.vhd Rhel_7.fixed.vhd-0.vhd
  ```
  {: screen}

  このコマンドは、完了するまで 30 分以上かかる場合があります。
  {: tip}

2. QEMU を使用して、容量固定 VHD イメージ・ファイルを RAW ファイル形式に変換します。イメージを暗号化するには、その前にイメージを RAW ファイル形式にしておく必要があります。以下の QEMU コマンドを実行します。

  ```
  qemu-img convert -p -O raw <FIXED_VHD_FILE> <RAW_IMAGE_FILE>
  ```
  {: pre}

  例えば、容量固定 VHD ファイルが _Rhel_7.fixed.vhd-0.vhd_ である場合に、RAW 出力ファイルの名前を _Rhel_7.raw-0.raw_ にするには、以下のコマンドを実行します。

  ```
  qemu-img convert -p -O raw Rhel_7.fixed.vhd-0.vhd Rhel_7.raw-0.raw
  ```
  {: screen}

  このコマンドは、完了するまで 30 分以上かかる場合があります。
  {: tip}

3. ドライブの暗号化および復号に使用するデータ暗号化鍵を決定します。このデータ暗号化鍵が、暗号化イメージを {{site.data.keyword.slportal}}にインポートするときにラップして指定する鍵になります。ドライブの暗号化および復号に使用するデータ暗号化鍵が入ったファイルを作成します。このファイル内には、ラップしていない base64 エンコードのテキスト形式の鍵を入れる必要があります。Base64 は、スペースや改行が誤って含められないようにするのに役立ちます。この base64 エンコードのデータ暗号化鍵は、32 文字またはバイト以上、512 文字またはバイト以下でなければなりません。データ暗号化鍵の素材は 1 行で指定します。改行を含めてはいけません。例えば、`secret.dek` というファイルを作成し、データ暗号化鍵 `unwrapped_key_material` を入れ、base64 でエンコードするには、以下のコマンドを使用します。

  ```
  $ echo -n $(echo 'unwrapped_key_material' | base64) > secret.dek
  ```
  {: screen}

  この鍵を安全な場所に保管します。この鍵を紛失すると、ディスクを復号できなくなります。
  {: tip}

4. 次のステップでは、暗号化ディスク用に作成する新規 RAW ファイルの正確なサイズを、LUKS ヘッダーが追加されることを考慮に入れて決定します。_dmcrypt_ および _cryptsetup_ は、この新規 RAW ファイルを使用して、ディスクの内容を LUKS 暗号化形式で保持します。この新規 RAW ファイルのサイズは、ステップ 2 で作成した RAW ファイルのサイズと、LUKS ヘッダー 4 MB (一定) の合計になる必要があります。既存の RAW イメージのサイズを調べるには、以下のコマンドを実行します。ここで _Rhel_7.raw-0.raw_ はイメージ名です。

  ```
  ls -l Rhel_7.raw-0.raw
  ```
  {: pre}

  次のような出力が表示されます。

  ```
  -rw-r--r--. 1 user user 42949017600 Mar 5 10:02 Rhel_7.raw-0.raw
  ```
  {: screen}

  _42949017600_ が、RAW イメージで使用されているバイト数です。
      1. LUKS ヘッダーを考慮し、RAW イメージ・ファイル・サイズに 4 MB (バイトに変換) を追加します。例: 42949017600 + (4 x 1024 x 1024) = 42953211904 バイト。
      2. RAW イメージ・サイズと LUKS ヘッダーの合計をメガバイトに変換し、直近の整数メガバイトに切り上げます。例: 42953211904 バイト / 1024 バイト / 1024 キロバイト = 40963.375 メガバイト = **40964 MG**。

5. 正確なバイト数を指定して、暗号化 RAW イメージとなる新規 RAW ファイルを作成します。以下の _dd_ コマンドを使用して、RAW ファイルを作成してください。

  ```
  dd if=/dev/zero of=<ENCRYPTED_RAW_FILENAME> bs=1M count=<TARGET_SIZE_IN_MEGABYTE> status=progress
  ```
  {: pre}

  ここで、_ENCRYPTED_RAW_FILENAME_ は、暗号化 RAW イメージとなるファイルです。_TARGET_SIZE_IN_MEGABYTE_ は、ステップ 4 で求めた数値、つまり非暗号化 RAW イメージに LUKS ヘッダーを加えたサイズです。

  例えば、暗号化イメージになる新規 RAW ファイルを _Rhel_7.encrypted.raw_ とし、そのターゲット・サイズが _40964_ MB である場合は、以下のコマンドを実行します。

  ```
  $ dd if=/dev/zero of=Rhel_7.encrypted.raw bs=1M count=40964 status=progress
  ```
  {: screen}

6. _dmcrypt_ およびデータ暗号化鍵 (ステップ 3 で作成) を使用して、RAW ファイルを LUKS ディスク暗号化でフォーマットします。このステップにより、イメージをファイルに入れる準備が整います。LUKS 暗号化を使用してファイルをフォーマットするには、以下の _cryptsetup_ コマンドを実行してください。

  ```
  cryptsetup -v luksFormat <ENCRYPTED_RAW_FILENAME> <DEK_FILENAME>
  ```
  {: pre}

  ここで、_ENCRYPTED_RAW_FILENAME_ は、暗号化する RAW ファイルのファイル名です。_DEK_FILENAME_ は、データ暗号化鍵が入っているファイルの名前です。

  例えば、RAW ファイルが _Rhel_7.encrypted.raw_、鍵ファイルが _secret.dek_ の場合は、以下のように入力します。

  ```
  $ cryptsetup -v luksFormat Rhel_7.encrypted.raw secret.dek
  ```
  {: screen}

  この cryptsetup コマンドを実行すると、以下のプロンプトが出されます。これは予期されるプロンプトです。YES と入力して応答し、続行してください。

  ```
  WARNING!
  ========
  This will overwrite data on Rhel_7.encrypted.raw irrevocably.
  Are you sure (Type uppercase yes): YES
  ```
  {: screen}

7. 暗号化 RAW ファイルをボリュームとして接続して、新規ブロック・デバイスをオペレーティング・システムと関連付けます。_cryptsetup_ を使用して luksOpen オプションで暗号化 RAW デバイスを開く以下のコマンドを実行してください。

  以下のコマンドは `root` ユーザー権限を使用して実行するので、sudo 特権が必要です。
  {: tip}

  ```
  sudo cryptsetup -v luksOpen <ENCRYPTED_RAW_FILENAME> <VOLUME_NAME> --key-file <DEK_FILENAME>
  ```
  {: pre}

  ここで、_ENCRYPTED_RAW_FILENAME_ は、暗号化 RAW ファイルのファイル名です。_VOLUME_NAME_ は、_dmcrypt_ で作成するデバイスの名前です。_DEK_FILENAME_ は、データ暗号化鍵のファイル名です。

  例えば、暗号化 RAW ファイルが _Rhel_7.encrypted.raw_、ブロック・デバイスの名前が _encryptedVolume_、データ暗号化鍵ファイルが _secret.dek_ である場合は、以下のコマンドを使用します。

  ```
  $ sudo cryptsetup -v luksOpen Rhel_7.encrypted.raw encryptedVolume --key-file secret.dek
  ```
  {: screen}

  このステップが完了すると、読み書き可能な新規ブロック・デバイスが _/dev/mapper/_ の下に作成されます。

8. 以下のコマンドを実行して、LUKS ボリューム・マッパーが正常に作成されたことを確認します。

  ```
  ls -l /dev/mapper/
  ```
  {: pre}

  以下の例のような出力が表示されます。

  ```
  total 0
  lrw-rw---- 1 root root        7 Mar 5 22:22 encryptedVolume -> ../dm-0
  ```
  {: screen}

9. 非暗号化イメージ (ステップ 2 で作成) を暗号化ボリューム・デバイス (ステップ 7 で作成) にコピーします。以下の _dd_ コマンドを実行して、非暗号化イメージを暗号化ボリュームにコピーしてください。

  ```
  sudo dd if=<RAW_IMAGE_FILE> of=/dev/mapper/<VOLUME_NAME>
  ```
  {: pre}

  ここで、_RAW_IMAGE_FILE_ は非暗号化 RAW イメージ (ステップ 2 で作成) のファイル名で、_VOLUME_NAME_ は暗号化ボリューム・ブロック・デバイス (ステップ 7 で作成) の名前です。

  例えば、RAW_IMAGE_FILE の名前が _Rhel_7.raw-0.raw_ で、VOLUME_NAME が _encryptedVolume_ である場合は、以下のコマンドを実行します。

  ```
  $ sudo dd if=Rhel_7.raw-0.raw of=/dev/mapper/encryptedVolume status=progress
  ```
  {: screen}

  このコマンドは、完了するまで 30 分以上かかる場合があります。
  {: tip}

10. ボリューム・マッパーを破棄し、暗号化データ・ファイルへの LUKS 接続を閉じます。

  ```
  sudo cryptsetup luksClose encryptedVolume
  ```
  {: pre}

  ここで、_encryptedVolume_ は暗号化ボリュームのブロック・デバイスの名前です。

  これで、_ENCRYPTED_RAW_FILENAME_ が初期化され、IBM Cloud オブジェクト・ストレージにアップロードできるようになりました。例えば、暗号化 RAW ファイルが _Rhel_7.encrypted.raw_ であれば、そのイメージを IBM Cloud オブジェクト・ストレージにアップロードします。
