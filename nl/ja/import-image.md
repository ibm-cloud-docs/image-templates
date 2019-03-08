---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-17"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# イメージの準備およびインポート
{: #preparing-and-importing-images}

ユーザーは、{{site.data.keyword.slportal_full}}の「イメージ・テンプレート」画面を使用して、[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage) サービス・インスタンスからイメージをインポートできます。{{site.data.keyword.cos_full_notm}} サービス・インスタンスのバケットにイメージをアップロードしたら、{{site.data.keyword.slportal}}のイメージ・テンプレート・リポジトリーにそのイメージをインポートできます。
{:shortdesc}

{{site.data.keyword.cos_full_notm}} からイメージをインポートするには、アップグレードしたアカウントが必要です。詳しくは、[IBM ID への切り替えとアカウントのリンク](/docs/account?topic=account-unifyingaccounts)を参照してください。
{: tip}

イメージがイメージ・テンプレートとしてインポートされた後、それらを使用して、既存の仮想サーバーをプロビジョンしたり開始したりすることができます。 {{site.data.keyword.cos_full_notm}} サービス・インスタンスからインポートできるイメージは、VHD またはカスタム ISO のいずれかです。VHD のインポートは、以下の 64 ビットのオペレーティング・システムに制限されています。  

* CentOS 6 および 7
* RedHat Enterprise Linux 6 および 7
* Ubuntu 14.04、および 16.04
* Microsoft Server Standard 2012、R2 2012、および 2016

VHD のインポートは 100 GB ディスクに制限されています。 VHD は、「filename.vhd-0.vhd」の例に従って命名する必要があります。

## イメージを VHD に変換
{: #convert-to-vhd}

VHD 形式は、仮想サーバーのイメージ形式として唯一サポートされる形式です。イメージを VHD に変換するには、以下の情報を使用します。

* Qemu-img 2.7.0 以降が必要です。
* 以下のコマンドを使用してイメージを変換します。

  ```
  qemu-img convert -f <image format> <image name> -O vpc -o force_size <image name>
  ```
  {: pre}

* コマンド例:

  ```
  qemu-img convert -f qcow2 test -O vpc -o force_size test
  ```
  {: pre}

詳細情報については、QEMU の資料の中の [Converting image formats ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) を参照してください。

## ISO テンプレート
{: #iso-templates}

ISO テンプレートを VSI にロードするために使用できるのは、{{site.data.keyword.BluSoftlayer_notm}} がサポートするオペレーティング・システムだけです。 サポート対象オペレーティング・システムのリストは、[https://www.ibm.com/cloud/server-software ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/server-software) にあります。

イメージがインポートに適格となるためには、このツールを使用してインポートされる ISO がブート可能でなければなりません。

## {{site.data.keyword.virtualmachinesshort}}用にイメージを構成
{: #config-image-vsi}

{{site.data.keyword.BluSoftlayer_notm}} インフラストラクチャー環境にイメージを正常にデプロイできるようにするには、以下の仕様に合わせて仮想サーバー・イメージを構成する必要があります。

* ***/boot*** が最初のパーティションでなければならない
* ***/boot*** と ***/*** は、ext3 または ext4 のファイル・システムでなければならない
* ***/etc*** および /root は、***/*** と同じパーティションになければならない
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** システムに接続されている swap ディスクをマウントする
* wget がインストールされていなければならない
* 最新の xe-guest-utilities Xen Tools がインストールされている必要がある。 以下の手順を実行します。

    1. Citrix ([https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html)) から XenServer ISO をダウンロードします。

    2. 以下のコマンドを実行して ISO をマウントします。

        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}

    3. 以下のコマンドを実行して ISO の RPM を見つけます。

        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}

    4. 以下のように出力に RPM がリストされます。

        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}

     5. RPM をコピーし、次に xentools 用の ISO を抽出できます。 以下のコマンドを実行して、ISO を入れるディレクトリー構造を作成します。

        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}

     6. 結果として作成されたディレクトリー構造から *guest-tools* ISO をマウントし、xentools をインストールするための *rpm/debs* を見つけることができます。 以下のディレクトリー構造の例を参照してください。

        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}

cloud-init 対応イメージについては、[cloud-init 対応イメージでのプロビジョニング](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image)を参照してください。

## {{site.data.keyword.cos_full_notm}} へのイメージのアップロード
{: #upload-to-ibm-cos}

イメージの準備ができたら、{{site.data.keyword.cos_full_notm}} にアップロードできます。必ず、[地域の場所](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#select-regions-and-endpoints)のバケットを使用してください。

1. {{site.data.keyword.cos_full_notm}} で、バケットにナビゲートし、**「オブジェクトの追加」**をクリックして、イメージを[アップロード](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data)します。
2. [Aspera](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#Aspera-high-speed-transfer) 高速転送プラグインを使用すると、イメージのアップロード速度が向上します。

Java、Python、または NodeJS を使用する場合は、COS SDK と Aspera を使用してカスタム・アプリケーション内部で高速転送を開始できます。詳しくは、[ライブラリーと SDK の使用方法](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#sdk)を参照してください。
{: tip}


## IBM Cloud オブジェクト・ストレージからのイメージのインポート
{: #import-icos}

{{site.data.keyword.cos_full_notm}} からイメージをインポートするには、以下の手順を実行します。

1. [{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/) で**「デバイス」>「管理」>「イメージ」**を選択して、**「イメージ・テンプレート」**ページにアクセスします。
2. **「IBM COS からのイメージのインポート (Import Image from IBM COS)」**タブをクリックして、インポート・ツールを開きます。
3. 必須フィールドに入力します (表 1 を参照)。
4. {{site.data.keyword.cos_full_notm}} からのインポートが完了すると、「イメージ・テンプレート」ページにイメージが表示されます。

| フィールド | 値 |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | インポート対象のイメージが保管されている {{site.data.keyword.cos_full_notm}} サービス・インスタンスを選択します。|
| 場所 | イメージが保管されている特定の地理的領域を選択します。イメージのインポート先になるのは、米国南部 (DAL13)、米国東部 (WDC07)、EU - 英国 (LON02)、EU - ドイツ (FRA02)、アジア太平洋 - 日本 (TOK02) の地域および関連データ・センターです。リストされたデータ・センターのいずれかにイメージをインポートした後に、別のデータ・センターに移動できます。|
| バケット | イメージが保管されている {{site.data.keyword.cos_full_notm}} バケットを選択します。選択した地域の場所に存在するバケットだけが選択可能です。選択した場所に存在しないバケットを選択すると、エラー・メッセージを受け取ります。|
| イメージ・ファイル | {{site.data.keyword.cos_full_notm}} サービス・インスタンス内のインポート対象のイメージ・ファイルを選択します。サポートされるファイル・タイプは、VHD、ISO、および RAW です。暗号化イメージをインポートする場合は、LUKS ディスク暗号化を使用して暗号化された RAW ファイル形式のイメージでなければなりません。|
| イメージ名 | イメージの記述名を指定します。これが、仮想サーバー・インスタンスをプロビジョンするときに使用するイメージになります。|
| API キー | {{site.data.keyword.cos_full_notm}} サービス・インスタンスにアクセスできる API キーを指定します。暗号化イメージをインポートするときには、この API キーで Key Protect にもアクセスできなければなりません。API キーのコピーまたはダウンロードは、作成時にしか実行できません。API キーを紛失した場合は、新しい API キーを作成する必要があります。詳しくは、[API キーの操作](/docs/iam?topic=iam-manapikey)を参照してください。|
| オペレーティング・システム | イメージに含まれているオペレーティング・システムを選択します。暗号化イメージを使用する場合、有効な選択肢は Linux オペレーティング・システムのみです。|
| Cloud-init | インポートするイメージが cloud-init に対応している場合は、このチェック・ボックスを選択します。cloud-init 対応の Windows オペレーティング・システムが含まれているイメージのインポート時にこのオプションを選択した場合、**「自分のライセンス (Your License)」**は指定できなくなります。暗号化イメージをインポートする場合は、cloud-init に対応したイメージでなければならないので、このオプションはデフォルトで選択されます。編集はできません。|
| 自分のライセンス (Your License) | ユーザーのオペレーティング・システム・ライセンスを持ち込む場合は、このチェック・ボックスを選択します。Windows オペレーティング・システムのイメージをインポートし、そのイメージを使用して[専用ホスト・インスタンス](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances)をプロビジョンする場合に、このオプションを選択できます。Windows オペレーティング・システムのバージョンがライセンスの持ち込みをサポートしていない場合は、このオプションは無効になります。Windows イメージの場合、ライセンス持ち込みを指定すると、Cloud Init は選択できなくなります。Red Hat Enterprise Linux オペレーティング・システムの暗号化イメージをインポートする場合は、持ち込みのオペレーティング・システム・ライセンスが含まれた暗号化イメージでなければならないので、このオプションはデフォルトで選択されます。編集はできません。|
| ブート・モード | イメージのブート・モードを選択します。指定したオペレーティング・システムにデフォルトのブート・モードが設定されている場合は、そのブート・モードが自動的に選択されます。|
| メモ | ユーザーの役に立つようにイメージに関するメモを追加します。|
| 暗号化 | このチェック・ボックスが選択されるかどうかは、インポート対象として選択したイメージのファイル・タイプによって決まります。VHD イメージと ISO イメージは、イメージ・ファイルが暗号化されていないことを意味します。したがって、VHD イメージと ISO イメージの場合は、このチェック・ボックスは選択されません。RAW イメージ・ファイルは、イメージが暗号化されていることを意味します。RAW イメージ・ファイルを指定すると、このチェック・ボックスがデフォルトで選択されます。編集はできません。|
{: caption="表 1. IBM Cloud オブジェクト・ストレージからイメージをインポートするための値" caption-side="top"}

以下の表に、暗号化イメージをインポートする場合にのみ適用される追加のフィールドを示します。暗号化イメージについて詳しくは、[エンド・ツー・エンド (E2E) の暗号化を使用した暗号化インスタンスのプロビジョン](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance) を参照してください。

暗号化イメージをインポートするためには、エンド・ツー・エンド (E2E) の暗号化機能を利用できるアカウントが必要です。アカウントで E2E 暗号化を利用できるようにするには、サポートにお問い合わせください。
{: tip}

| フィールド | 値 |
| ----- | ----- |
| {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンス ID | 暗号化イメージをインポートする場合には、{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスが {{site.data.keyword.cos_full_notm}} バケットと同じ地域になければなりません。{{site.data.keyword.keymanagementserviceshort}} インスタンス ID は、{{site.data.keyword.cloud_notm}} CLI を使用して調べられます。詳しくは、[インスタンス ID の取得](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID)を参照してください。|
| ラップされたデータ暗号化鍵 | 暗号化イメージをインポートするときには、イメージの暗号化に使用したデータ暗号化鍵に関連付けられた暗号テキストを指定します。詳しくは、[鍵のラッピング](/docs/services/key-protect?topic=key-protect-wrap-keys#api)を参照してください。|
| ルート鍵 ID | 暗号化イメージをインポートするときには、データ暗号化鍵のラップに使用したルート鍵の ID を指定します。詳しくは、[鍵の表示](/docs/services/key-protect?topic=key-protect-view-keys#view-keys)を参照してください。|
{: caption="表 2. IBM Cloud オブジェクト・ストレージから暗号化イメージをインポートするための値" caption-side="top"}

## 次のステップ

インポートが始まると、システムは、{{site.data.keyword.cos_full_notm}} サービス・インスタンスの指定されたバケットで、該当するイメージ・ファイルを見つけます。イメージ・ファイルがイメージ・テンプレートとしてインポートされ、「イメージ・テンプレート」ページで利用できるようになります。インポートが完了したら、そのイメージを新規デバイスの注文や既存デバイスの開始に使用できます。
また、このイメージ・テンプレートはいつでも削除できます。イメージのインポートにかかる時間はファイル・サイズによって異なりますが、通常、数分間から 1 時間かかります。

イメージ・テンプレート・リポジトリー内にインポートしたイメージは、{{site.data.keyword.cos_full_notm}} から削除してもかまいません。削除した後も**「イメージ・テンプレート」 **ページからそのイメージ・テンプレートにアクセスし、仮想サーバー・インスタンスのプロビジョンに使用することができます。
