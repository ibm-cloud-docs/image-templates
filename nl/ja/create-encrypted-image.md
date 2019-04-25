---

copyright:
  years: 2019
lastupdated: "2019-03-27"

keywords: VHD image file, encryption, encrypted image, image

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}


# VHD イメージの暗号化 
{: #create-encrypted-image}

E2E Encryption 機能を使用するには、VHD イメージを、暗号化インスタンスをプロビジョンするためのイメージ・テンプレートにインポートする前に、vhd-util ツールを使用して暗号化する必要があります。2 つのレベルの AES 暗号化がサポートされています。それらは AES 256-bit と AES 512-bit です。
{:shortdesc}

## 暗号化 VHD イメージの要件
{: #encrypted-image-reqs}

暗号化 VHD イメージは、以下の要件を満たしている必要があります。

* VHD がフォーマット済みであること。
* {{site.data.keyword.cloud}} Console インフラストラクチャー環境と互換性があること。
* [サポートされる OS](/docs/infrastructure/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images) でプロビジョンされていること。
* Cloud-init が有効であること。
* [vhd-util ツール](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool)を使用して暗号化されていること。

## VHD イメージの暗号化
{: #vhd-util-tool}

以下の手順に従って、暗号化 VHD イメージを作成します。

1. バージョン 7 以上を実行している CentOS システムを選択し、{{site.data.keyword.cloud_notm}} のための仮想ディスク・イメージ (VHD ファイル) を暗号化します。CentOS のインストールされた物理ハードウェアへのアクセス権限がない場合、パブリックまたは専用のホストを使用して、{{site.data.keyword.cloud_notm}} 内の CentOS 7 がある仮想サーバー・インスタンスをプロビジョンできます。VHD ファイルの暗号化に使用された CentOS システム自体は、暗号化しないことが必要です。

2. CentOS システムにログインしてカスタマー VPN に接続してから、[SoftLayer ダウンロード・サイト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://downloads.service.softlayer.com/citrix/xen/){: new_window} に移動して、vhd-util ツール RPM パッケージ・ファイル vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm を選択します。   

   RPM パッケージ・ファイルを CentOS システムに直接ダウンロードできない場合は、そのファイルを現在作業中のワークステーションにダウンロードします。その後、セキュア・コピー (scp) コマンドを使用して、それを CentOS システムにアップロードします。{{site.data.keyword.cloud_notm}} で仮想サーバー・インスタンスを使用している場合、以下のコマンドを使用して、このアップロード用にシステムのパブリック IP アドレスを使用します。

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

3. 以下のコマンドを使用して、RPM をインストールします。

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. 暗号化する必要のある AES 暗号鍵を識別し、ディスク・イメージを復号して鍵ファイルに書き込みます。この AES 暗号鍵は、[環境の準備](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment)で Key Protect カスタマー・ルート鍵と共にラップしたものと同じデータ暗号化鍵です。鍵ファイルに書き込む鍵素材は、アンラップしたもので、エンコードしていないものであることが必要です。 

   鍵ファイル内で data_key は base64 エンコードではないので、標準 ASCII 文字を使用してコマンド・ラインから鍵ファイルの内容を印刷したり表示したりすることはできません。
   {: tip}

   以下のコマンドを使用して、**AES 256-bit** または **AES 512-bit** の暗号鍵により、鍵ファイルを作成します。 
   
   ```
   echo <data_key> | base64 -d - > <keyfile_name>
   ```
   {: pre} 

   コマンド例:

   ```
   echo Nrfen98EpMxF2B+wdgLfagzrqvgUZfMK4vL2T0NsT20ihrsNC9pUUHtizF6218pze8RLCgQ6kwxuE58IWLzgDA== | base64 -d - > aes512.dek
   ```
   {: screen}

5. 以下のコマンドを使用して、直前のステップで作成した鍵ファイルを検証します。

   ```
   vhd-util key -C -k <keyfile_name>
   ```
   {: pre}

   以下はコマンドと出力の例です。

   ```
   vhd-util key -C -k aes512.dek
   vhd_util_read_key: using keyfile aes512.dek, Size (bytes) 64
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

   直前のコマンド例からの出力で、最初の行は `aes512.dek` という名前の鍵ファイルに 64-byte 鍵が含まれることを示し、2 番目の行にリストされた数字は対応する暗号鍵の SHA256 ハッシュまたはセキュリティー・ハッシュです。
AES 256-bit 暗号鍵を含むファイルからの出力は 32-byte 鍵を示します。
   {: tip} 

6. 以下のコマンドを使用して、VHD ファイルの暗号化コピーを作成します。`target_vhd` は、`source_vhd` の暗号化バージョンを含むファイルの名前を表します。

   ```
   vhd-util copy -n <source_vhd> -N <target_vhd> -k <keyfile_name>
   ```
   {: pre}    

   コマンド例:

   ```
   vhd-util copy -n debian8-ne.vhd -N debian8-aes512.vhd -k aes512.dek
   ```
   {: screen}

7. 以下のコマンドを使用して、VHD ファイルが暗号化されていることを検証します。

   ```
   vhd-util key -p -n <vhd_filename>
   ```
   {: pre}

   以下はコマンドと出力の例です。

   ```
   vhd-util key -p -n debian8-aes512.vhd
   0000000000000000000000000000000000000000000000000000000000000000
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

VHD ファイルが暗号化されている場合は、直前の例に示されているように、出力に 2 つのハッシュ値が含まれます。最初のハッシュはすべてゼロです。2 番目のハッシュは、AES 暗号鍵が VHD の暗号化と復号に使用する SHA256 ハッシュです。VHD ファイル用の SHA256 ハッシュが**ステップ 5** に示されているハッシュと同じであることを確認してください。

**ステップ 7** にあるコマンド例は、「debian8-aes512.vhd」という名前の新しい暗号化 VHD ファイルを作成します。これは、「aes512.dek」という名前の鍵ファイルにある AES 512-bit 暗号鍵によって暗号化されます。この暗号化の SHA256 ハッシュは `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda` です。
