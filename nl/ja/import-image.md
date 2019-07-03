---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:pre: .pre}
{:screen: .screen}


# イメージの準備およびインポート
{: #preparing-and-importing-images}

{{site.data.keyword.slportal_full}}の「イメージ・テンプレート」画面を使用して、[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about) サービス・インスタンスからイメージをインポートできます。 Virtual Hard Disk (VHD) または Virtual Machine Disk (VMDK) 形式のイメージをインポートできます。 インポートの後に、VMDK のイメージは VHD に変換されます。 {{site.data.keyword.cos_full_notm}} サービス・インスタンスのバケットにイメージをアップロードしたら、{{site.data.keyword.slportal}}のイメージ・テンプレート・リポジトリーにそのイメージをインポートできます。
{:shortdesc}

{{site.data.keyword.cos_full_notm}} からイメージをインポートするには、アップグレードしたアカウントが必要です。 詳しくは、[IBMid への切り替えとアカウントのリンク](/docs/account/softlayerlink.html)を参照してください。
{: tip}

このインポート機能を使用するには、{{site.data.keyword.cloud_notm}} コンソール (cloud.ibm.com) から [IBM Cloud オブジェクト・ストレージのインスタンス](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision#provision-account)を注文する必要があります。  control.softlayer.com にある IBM Cloud オブジェクト・ストレージはサポートされていません。
{: important}

イメージがイメージ・テンプレートとしてインポートされた後、それらを使用して、既存の仮想サーバーをプロビジョンしたり開始したりすることができます。 {{site.data.keyword.cos_full_notm}} サービス・インスタンスからインポートできるイメージは、VHD、VMDK、またはカスタム ISO のいずれかです。 VHD および VMDK のインポートは、以下の 64 ビットのオペレーティング・システムに制限されています。  

* CentOS 6 および 7
* Microsoft Server Standard 2012、R2 2012、および 2016
* RedHat Enterprise Linux 6 および 7
* Ubuntu 14.04、および 16.04

インポートは 100 GB ディスクに制限されています。 イメージは、「filename.vhd-0.vhd or filename.vmdk-0.vmdk」の例に従って命名する必要があります。

## イメージを VHD に変換
{: #convert-to-vhd}

VHD 形式と VMDK 形式だけが、仮想サーバーのイメージ形式としてサポートされています。 イメージを VMDK 以外の任意の形式から VHD に変換するには、以下の情報を使用します。

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

    1. Citrix ([https://www.citrix.com/downloads/citrix-hypervisor ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.citrix.com/downloads/citrix-hypervisor/)) から XenServer ISO をダウンロードします。

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

cloud-init 対応イメージについては、[cloud-init 対応イメージでのプロビジョニング](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#provisioning-with-a-cloud-init-enabled-image)を参照してください。

## 暗号化イメージのインポートの準備
{: #preparing-to-import-an-encrypted-image}

独自のデータ暗号化鍵によって暗号化した VHD イメージをインポートする場合には、[エンド・ツー・エンド (E2E) の暗号化を使用した暗号化インスタンスのプロビジョン](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance)で説明されている暗号化の前提条件を満たし、その指示に従っていることを確認してください。

vhd-util ツールを使用してイメージ (VHD 形式でなければならない) を暗号化する必要があります。 詳しくは、[VHD イメージの暗号化](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image)を参照してください。 
{: important}

## {{site.data.keyword.cos_full_notm}} へのイメージのアップロード
{: #upload-to-ibm-cos}

イメージの準備ができたら、{{site.data.keyword.cos_full_notm}} にアップロードできます。 必ず、[地域の場所](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints)のバケットを使用してください。

1. {{site.data.keyword.cos_full_notm}} で、バケットにナビゲートし、**「オブジェクトの追加」**をクリックして、イメージを[アップロード](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload)します。
2. [Aspera](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera) 高速転送プラグインを使用すると、イメージのアップロード速度が向上します。

Java、Python、または NodeJS を使用する場合は、COS SDK と Aspera を使用してカスタム・アプリケーション内部で高速転送を開始できます。 詳しくは、[ライブラリーと SDK の使用方法](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera#aspera-sdk)を参照してください。
{: tip}


## IBM Cloud オブジェクト・ストレージからのイメージのインポート
{: #import-icos}

{{site.data.keyword.cos_full_notm}} からイメージをインポートするには、以下の手順を実行します。

1. デバイス・メニューにナビゲートして、タスクを完了するための正しいアカウント権限があることを確認します。

   * コンソールのデバイス・メニューにナビゲートします。詳しくは、[デバイスへのナビゲート](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)を参照してください。
   * 必要なアカウント権限およびデバイス・アクセス権限があることを確認します。アカウントの所有者、または**ユーザーの管理**クラシック・インフラストラクチャー権限を持つユーザーのみが権限を調整できます。

   権限について詳しくは、[クラシック・インフラストラクチャー権限](/docs/iam?topic=iam-    infrapermission#infrapermission)および[デバイス・アクセス権限の管理](/docs/vsi?topic=virtual-servers-managing-device-access)を参照してください。

   暗号化イメージをインポートする場合は、{{site.data.keyword.cloud_notm}} コンソールを使用する必要があります。
   {: important}
2. **「デバイス」>「管理」>「イメージ」**を選択して、**「イメージ・テンプレート」**ページにアクセスします。
3. **「IBM COS からのイメージのインポート (Import Image from IBM COS)」**タブをクリックして、インポート・ツールを開きます。
4. 必須フィールドに入力します (表 1 を参照)。
5. {{site.data.keyword.cos_full_notm}} からのインポートが完了すると、「イメージ・テンプレート」ページにイメージが表示されます。

| フィールド | 値 |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | インポート対象のイメージが保管されている {{site.data.keyword.cos_full_notm}} サービス・インスタンスを選択します。 |
| 場所 | イメージが保管されている特定の地理的領域を選択します。 イメージのインポート先になるのは、米国南部 (DAL13)、米国東部 (WDC07)、EU - 英国 (LON02)、EU - ドイツ (FRA02)、アジア太平洋 - 日本の地域および関連データ・センターです。 リストされたデータ・センターのいずれかにイメージをインポートした後に、別のデータ・センターに移動できます。 |
| バケット | イメージが保管されている {{site.data.keyword.cos_full_notm}} バケットを選択します。 選択した地域の場所に存在するバケットだけが選択可能です。 選択した場所に存在しないバケットを選択すると、エラー・メッセージを受け取ります。|
| イメージ・ファイル | {{site.data.keyword.cos_full_notm}} サービス・インスタンス内のインポート対象のイメージ・ファイルを選択します。 サポートされるファイル・タイプは、VHD (Virtual Hard Disk)、VMDK (Virtual Machine Disk)、および ISO です。 暗号化イメージをインポートする場合は、vhd-util ツールを使用して暗号化された VHD ファイル形式のイメージでなければなりません。 |
| イメージ名 | イメージの記述名を指定します。 これが、仮想サーバー・インスタンスをプロビジョンするときに使用するイメージになります。 |
| API キー | {{site.data.keyword.cos_full_notm}} サービス・インスタンスにアクセスできる API キーを指定します。 暗号化イメージをインポートするときには、この API キーで鍵管理サービス・インスタンスにもアクセスできなければなりません。API キーのコピーまたはダウンロードは、作成時にしか実行できません。 API キーを紛失した場合は、新しい API キーを作成する必要があります。 詳しくは、[API キーの操作](/docs/iam?topic=iam-manapikey#manapikey)を参照してください。 |
| オペレーティング・システム | イメージに含まれているオペレーティング・システムを選択します。 |
| Cloud-init | インポートするイメージが cloud-init に対応している場合は、このチェック・ボックスを選択します。 cloud-init 対応の Windows オペレーティング・システムが含まれているイメージのインポート時にこのオプションを選択した場合、**「ご使用のライセンス」**を同時に選択することはできません。 暗号化イメージをインポートする場合は、cloud-init に対応したイメージでなければならないので、このオプションはデフォルトで選択されます。編集はできません。 |
| 自分のライセンス (Your License) | ユーザーのオペレーティング・システム・ライセンスを持ち込む場合は、このチェック・ボックスを選択します。 Windows オペレーティング・システムのイメージをインポートし、そのイメージを使用して[専用ホスト・インスタンス](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances)をプロビジョンする場合に、このオプションを選択できます。 Windows オペレーティング・システムのバージョンがライセンスの持ち込みをサポートしていない場合は、このオプションは無効になります。 Windows イメージの場合、ライセンス持ち込みを指定すると、Cloud Init は選択できなくなります。 Red Hat Enterprise Linux オペレーティング・システムの暗号化イメージをインポートする場合は、持ち込みのオペレーティング・システム・ライセンスが含まれた暗号化イメージでなければならないので、このオプションはデフォルトで選択されます。編集はできません。 |
| ブート・モード | イメージのブート・モードを選択します。 指定したオペレーティング・システムにデフォルトのブート・モードが設定されている場合は、そのブート・モードが自動的に選択されます。 |
| メモ | ユーザーの役に立つようにイメージに関するメモを追加します。 |
| 暗号化 | vhd-util ツールを使用して独自のデータ暗号化鍵により暗号化したイメージをインポートしている場合、このチェック・ボックスを選択します。 |
{: caption="表 1. IBM Cloud オブジェクト・ストレージからイメージをインポートするための値" caption-side="top"}

以下の表に、暗号化イメージをインポートする場合にのみ適用される追加のフィールドを示します。 暗号化イメージについて詳しくは、[エンド・ツー・エンド (E2E) の暗号化を使用した暗号化インスタンスのプロビジョン](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance) を参照してください。

<!--
To import an encrypted image, your account must have access to the End to End (E2E) Encryption feature. To enable your account for E2E Encryption, please contact [Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
{: tip}
-->

| フィールド | 値 |
| ----- | ----- |
| ラップされたデータ暗号化鍵 | 暗号化イメージをインポートするときには、イメージの暗号化に使用したデータ暗号化鍵に関連付けられた暗号テキストを指定します。 詳しくは、[鍵のラッピング](/docs/services/key-protect?topic=key-protect-wrap-keys#api)を参照してください。 |
|鍵管理サービス・インスタンス| ドロップダウン・リストからアカウント内の鍵管理サービス・インスタンスを選択します。鍵管理サービス・インスタンスには、データ暗号化鍵のラップに使用したカスタマー・ルート鍵が含まれている必要があります。|
| 鍵の名前 | データ暗号化鍵のラップに使用した鍵管理サービス・インスタンス内にあるルート鍵の名前を選択します。詳しくは、[鍵の表示](/docs/services/key-protect?topic=key-protect-view-keys#view-keys)を参照してください。 |
{: caption="表 2. IBM Cloud オブジェクト・ストレージから暗号化イメージをインポートするための値" caption-side="top"}

## 次のステップ

インポートが始まると、システムは、{{site.data.keyword.cos_full_notm}} サービス・インスタンスの指定されたバケットで、該当するイメージ・ファイルを見つけます。 イメージ・ファイルがイメージ・テンプレートとしてインポートされ、「イメージ・テンプレート」ページで利用できるようになります。 インポートが完了したら、そのイメージを新規デバイスの注文や既存デバイスの開始に使用できます。
また、このイメージ・テンプレートはいつでも削除できます。 イメージのインポートにかかる時間はファイル・サイズによって異なりますが、通常、数分間から 1 時間かかります。

イメージ・テンプレート・リポジトリー内にインポートしたイメージは、{{site.data.keyword.cos_full_notm}} から削除してもかまいません。 削除した後も**「イメージ・テンプレート」**ページからそのイメージ・テンプレートにアクセスし、仮想サーバー・インスタンスのプロビジョンに使用することができます。
