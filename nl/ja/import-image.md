---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-31"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# イメージの準備およびインポート

[カスタマー・ポータル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/) の「イメージ・テンプレート」画面により、ユーザーは、Swift ベースの[オブジェクト・ストレージ](/docs/infrastructure/objectstorage-swift/index.html)・アカウントから既存のイメージをアップロードすることができます。
{:shortdesc}

イメージがイメージ・テンプレートとしてインポートされた後、それらを使用して、既存の仮想サーバーをプロビジョンしたり開始したりすることができます。オブジェクト・ストレージ・アカウントからインポートされるイメージは、VHD またはカスタム ISO のいずれかになります。VHD のインポートは、以下の 64 ビットのオペレーティング・システムに制限されています。

* CentOS 6 および 7
* RedHat Enterprise Linux 6 および 7
* Ubuntu 14.04、および 16.04
* Microsoft Server Standard 2012、R2 2012、および 2016

VHD のインポートは 100 GB ディスクに制限されています。VHD は、「filename.vhd-0.vhd」の例に従って命名する必要があります。

## イメージを VHD に変換

VHD フォーマットは、{{site.data.keyword.BluVirtServers_full}}で唯一サポートされるイメージ・フォーマットです。イメージを VHD に変換するには、以下の情報を使用します。

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

ISO テンプレートを VSI にロードするために使用できるのは、{{site.data.keyword.BluSoftlayer_notm}} がサポートするオペレーティング・システムだけです。サポート対象オペレーティング・システムのリストは、[http://www.softlayer.com/services/software/ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.softlayer.com/services/software/) にあります。

イメージがインポートに適格となるためには、このツールを使用してインポートされる ISO がブート可能でなければなりません。

## {{site.data.keyword.virtualmachinesshort}}用にイメージを構成

{{site.data.keyword.BluSoftlayer_notm}} インフラストラクチャー環境にイメージを正常にデプロイできるようにするには、以下の仕様に合わせて仮想サーバー・イメージを構成する必要があります。

* ***/boot*** が最初のパーティションでなければならない
* ***/boot*** と ***/*** は、ext3 または ext4 のファイル・システムでなければならない
* ***/etc*** および /root は、***/*** と同じパーティションになければならない
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** システムに接続されている swap ディスクをマウントする
* wget がインストールされていなければならない
* 最新の xe-guest-utilities Xen Tools がインストールされている必要がある。以下の手順を実行します。
    
    1. Citrix ([https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html)) から XenServer ISO をダウンロードします。
    
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
     
     5. RPM をコピーし、次に xentools 用の ISO を抽出できます。以下のコマンドを実行して、ISO を入れるディレクトリー構造を作成します。
    
        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}
    
     6. 結果として作成されたディレクトリー構造から *guest-tools* ISO をマウントし、xentools をインストールするための *rpm/debs* を見つけることができます。以下のディレクトリー構造の例を参照してください。
     
        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}
    
cloud-init 対応イメージについては、[cloud-init 対応イメージでのプロビジョニング](image_cloud-init.html)を参照してください。

## イメージのインポート

カスタマー・ポータルでイメージをインポートするには、以下の手順を実行します。

1. {{site.data.keyword.objectstorageshort}}・アカウントからイメージに関する以下の詳細を見つけて記録します。詳細情報については、[Viewing and Editing Object Storage File Details ](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html)を参照してください。
  * アカウント名
  * クラスター
  * コンテナー
  * イメージ・ファイル名
2. [カスタマー・ポータル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/) で**「デバイス」>「管理」>「イメージ」**を選択して、**「イメージ・テンプレート」**ページにアクセスします。
3. **「イメージのインポート」**タブをクリックして、インポート・ツールを開きます。
4. **「アカウント」**ドロップダウン・リストから、インポートするイメージの**「{{site.data.keyword.objectstorageshort}}・アカウント」**を選択します。
5. **「クラスター」**ドロップダウン・リストから、インポートするイメージの**「{{site.data.keyword.objectstorageshort}}・クラスター」**を選択します。
6. **「コンテナー」**ドロップダウン・リストから、インポートするイメージの**「{{site.data.keyword.objectstorageshort}}・コンテナー」**を選択します。
7. **「イメージ・ファイル」**ドロップダウン・リストから、{{site.data.keyword.objectstorageshort}}にリストされている**「イメージ・ファイル名」**を選択します。
8. **「イメージ名」**フィールドに、新規イメージ・テンプレートのイメージ名を入力します。
9. **「メモ」**テキスト・ボックスに、該当するメモを入力します。
10. **「オペレーティング・システム」**ドロップダウン・リストから、イメージのオペレーティング・システムを選択します。

  インポートするイメージがカスタム ISO の場合、「オペレーティング・システム」ドロップダウン・リストは使用不可になっています。このステップは、インポートで VHD が使用される場合にのみ必要です。
  {:tip}

11. インポートするイメージが cloud-init 対応の場合は、**「Cloud Init」**チェック・ボックスを選択します。詳細情報については、[cloud-init 対応イメージでのプロビジョニング](image_cloud-init.html)を参照してください。        
12. ユーザー自身のオペレーティング・システム・ライセンスを提供することを予定している場合は、**「自分のライセンス (Your License)」**チェック・ボックスを選択します。詳細情報については、[Red Hat Cloud Access の使用](use-red-hat-cloud-access.html)を参照してください。
13. **「インポート」**をクリックして、イメージを「イメージ・テンプレート」画面にインポートします。アクションを取り消すには、**「キャンセル」**をクリックします。

## 次のステップ

インポートが開始されると、システムは、指定されたパス(「アカウント」>「クラスター」>「コンテナー」>「イメージ・ファイル) を使用して、{{site.data.keyword.objectstorageshort}}・アカウント内でイメージ・ファイルを見つけます。イメージ・ファイルはイメージ・テンプレートとしてインポートされ、次に「イメージ・テンプレート」ページからアクセス可能になります。インポートが完了すると、イメージは、新しいデバイスのオーダーや既存のデバイスの開始に使用できます。さらに、このイメージはいつでも削除できます。イメージのインポートにかかる時間はファイル・サイズによって異なりますが、通常、数分間から 1 時間かかります。

