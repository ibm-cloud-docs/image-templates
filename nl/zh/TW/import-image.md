---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-17"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# 準備及匯入映像檔
{: #preparing-and-importing-images}

{{site.data.keyword.slportal_full}}中的「映像檔範本」畫面容許使用者從 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage) 服務實例匯入映像檔。將映像檔上傳至 {{site.data.keyword.cos_full_notm}} 服務實例中的儲存區之後，您可以將它匯入至 {{site.data.keyword.slportal}}中的映像檔範本儲存庫。
{:shortdesc}

您必須具有已升級的帳戶，才能從 {{site.data.keyword.cos_full_notm}} 匯入影像。如需相關資訊，請參閱[切換至 IBM ID 及鏈結帳戶](/docs/account?topic=account-unifyingaccounts)。
{: tip}

映像檔匯入為映像檔範本之後，可以用來佈建或啟動現有的虛擬伺服器。從 {{site.data.keyword.cos_full_notm}} 帳戶匯入的映像檔可以是 VHD 或自訂 ISO。VHD 匯入僅限於下列 64 位元作業系統：  

* CentOS 6 及 7
* RedHat Enterprise Linux 6 及 7
* Ubuntu 14.04 及 16.04
* Microsoft Server Standard 2012、R2 2012 及 2016

VHD 匯入僅限於 100 GB 磁碟。VHD 必須根據下列範例來命名：filename.vhd-0.vhd。

## 將映像檔轉換成 VHD
{: #convert-to-vhd}

VHD 格式是虛擬伺服器唯一支援的映像檔格式。若要將映像檔轉換成 VHD，請使用下列資訊：

* 需要 Qemu-img 2.7.0 或更新版本
* 使用下列指令轉換映像檔：

  ```
  qemu-img convert -f <image format> <image name> -O vpc -o force_size <image name>
  ```
  {: pre}

* 範例指令：

  ```
  qemu-img convert -f qcow2 test -O vpc -o force_size test
  ```
  {: pre}

如需相關資訊，請參閱 QEMU 文件中的 [Converting image formats ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats)。

## ISO 範本
{: #iso-templates}

只有 {{site.data.keyword.BluSoftlayer_notm}} 支援的作業系統可以用來將 ISO 範本匯入到 VSI。「支援的作業系統」清單可以在這裡找到：[https://www.ibm.com/cloud/server-software ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/server-software)

使用此工具匯入的 ISO 必須可開機，映像檔才有匯入的資格。

## 配置 {{site.data.keyword.virtualmachinesshort}} 的映像檔
{: #config-image-vsi}

為了確保映像檔可以順利部署在 {{site.data.keyword.BluSoftlayer_notm}} 基礎架構環境，虛擬伺服器映像檔必須配置成下列規格：

* ***/boot*** 必須是第一個分割區
* ***/boot*** 和 ***/*** 必須是 ext3 或 ext4 檔案系統
* ***/etc***  和 /root 必須在與 ***/*** 相同的分割區上
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** 以裝載連接到系統的交換磁碟
* 必須安裝 wget
* 必須安裝最新的 xe-guest-utilities Xen 工具。請完成下列步驟：

    1. 從 Citrix 下載 XenServer ISO：[https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html)

    2. 執行下列指令裝載 ISO：

        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}

    3. 執行下列指令找到 ISO 的 RPM：

        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}

    4. 輸出會列出類似如下的 RPM：

        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}

     5. 您可以複製 RPM，然後擷取 ISO 中的 xentools。執行下列指令，以建立儲存 ISO 的目錄結構：

        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}

     6. 從結果的目錄結構，您可以裝載 *guest-tools* ISO 並找到 *rpm/debs* 以安裝 xentools。請參閱下列範例目錄結構：

        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}

如需啟用 cloud-init 之映像檔的相關資訊，請參閱[使用啟用 cloud-init 的映像檔進行佈建](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image)。

## 將映像檔上傳至 {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

當您的映像檔備妥時，您可以將它上傳至 {{site.data.keyword.cos_full_notm}}。請確定使用[地區位置](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#select-regions-and-endpoints)中的儲存區。

1. 在 {{site.data.keyword.cos_full_notm}} 中，導覽至您的儲存區，然後按一下**新增物件**來[上傳](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data)映像檔。
2. 使用 [Aspera](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#Aspera-high-speed-transfer) 高速傳輸外掛程式，以擁有映像檔最快的上傳速度。

您可以使用 COS SDK 與 Aspera 搭配，在使用 Java、Python 或 NodeJS 時，於自訂應用程式內起始高速傳輸。如需相關資訊，請參閱[使用程式庫與 SDK](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#sdk)。
{: tip}


## 從 IBM Cloud Object Storage 匯入映像檔
{: #import-icos}

請完成下列步驟，從 {{site.data.keyword.cos_full_notm}} 匯入映像檔。

1. 在 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 中，選取**裝置 > 管理 > 映像檔**來存取**映像檔範本**頁面。
2. 按一下**從 IBM COS 匯入映像檔**標籤，以開啟「匯入」工具。
3. 填寫必要欄位（請參閱「表 1」）。
4. 從 {{site.data.keyword.cos_full_notm}} 完成匯入時，映像檔會出現在「映像檔範本」頁面上。

| 欄位 | 值 |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | 選取 {{site.data.keyword.cos_full_notm}} 服務實例，其中儲存您要匯入的映像檔。|
| 位置 | 選取要儲存映像檔的特定地理區域。您可以將映像檔匯入至下列地區及關聯的資料中心：US-South (DAL13)、US-East (WDC07)、EU-Great Britain (LON02)、EU-Germany (FRA02)、AP-Japan (TOK02)。將映像檔匯入至其中一個列出的資料中心之後，您可以將它移至另一個資料中心。|
| 儲存區 | 選取要儲存映像檔的 {{site.data.keyword.cos_full_notm}} 儲存區。只有存在於您所選地區位置中的儲存區才有效。如果您選取的儲存區不存在於選取的位置，則會收到錯誤訊息。|
| 映像檔 | 選取 {{site.data.keyword.cos_full_notm}} 服務實例中您要匯入的映像檔。支援檔案類型為 VHD、ISO 和 RAW。如果您是匯入已加密的映像檔，則映像檔必須採用 RAW 檔案格式，並使用 LUKS 磁碟加密進行加密。|
| 映像檔名稱 | 指定映像檔的敘述性名稱。這是您將用來佈建虛擬伺服器實例的映像檔。|
| API 金鑰 | 指定提供 {{site.data.keyword.cos_full_notm}} 服務實例之存取權的 API 金鑰。匯入已加密的映像檔時，API 金鑰也必須具有 Key Protect 的存取權。只在建立時才能複製或下載 API 金鑰。如果 API 金鑰遺失，您必須建立新的 API 金鑰。如需相關資訊，請參閱[使用 API 金鑰](/docs/iam?topic=iam-manapikey)。|
| 作業系統 | 選取映像檔中包含的作業系統。若為已加密的映像檔，只有 Linux 作業系統才是有效的選項。|
| Cloud-init | 如果您匯入的映像檔已啟用 cloud-init，請選取此勾選框。如果您匯入的映像檔具有已啟用 cloud-init 的 Windows 作業系統，而且您選取此選項，則也無法指定**您的授權**。如果您是匯入已加密的映像檔，則依預設會選取此選項，且因為已加密的映像檔必須已啟用 cloud-init，所以無法編輯。|
| 您的授權 | 如果您計劃提供自己的作業系統授權，請選取此勾選框。如果匯入的映像檔具有 Windows 作業系統，則在您計劃使用映像檔來佈建[專用主機實例](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances)時，可以選取此選項。如果您的 Windows 作業系統版本不支援使用您自己的授權，則會停用此選項。對於 Windows 映像檔，如果您指定將使用自己的授權，則無法選取 Cloud Init。如果匯入的已加密映像檔具有 Red Hat Enterprise Linux 作為作業系統，則依預設會選取此選項，且因為已加密的映像檔必須包含自己的作業系統授權，所以無法編輯。|
| 開機模式 | 為您的映像檔選取開機模式。如果已為您指定的作業系統設定預設開機模式，則會在這裡自動選取該開機模式。|
| 附註 | 新增任何與映像檔相關且可能有助於使用者的附註。|
| 加密 | 這個勾選框的選項是由您選取要匯入之映像檔的檔案類型所決定。VHD 和 ISO 映像檔指出映像檔未加密。因此，不會針對 VHD 和 ISO 映像檔選取勾選框。RAW 映像檔指出映像檔是已加密的映像檔。如果指定了 RAW 映像檔，則依預設會選取此勾選框，且不可編輯。|
{: caption="表 1. 用於從 IBM Cloud Object Storage 匯入映像檔的值" caption-side="top"}

下表顯示僅適用於匯入已加密映像檔的其他欄位。如需已加密映像檔的相關資訊，請參閱[使用端對端 (E2E) 加密來佈建已加密的實例](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance)。

若要匯入已加密的映像檔，您的帳戶必須有權存取「端對端 (E2E) 加密」功能。若要啟用您的帳戶進行「E2E 加密」，請與「支援中心」聯絡。
{: tip}

| 欄位 | 值 |
| ----- | ----- |
| {{site.data.keyword.keymanagementserviceshort}} 服務實例 ID | 匯入已加密的映像檔時，您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例必須位於與 {{site.data.keyword.cos_full_notm}} 儲存區相同的地區中。 您可以使用 {{site.data.keyword.cloud_notm}} CLI 來尋找 {{site.data.keyword.keymanagementserviceshort}} 實例 ID。如需相關資訊，請參閱[擷取實例 ID](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID)。|
| 包裝的資料加密金鑰 | 匯入已加密的映像檔時，請指定與資料加密金鑰相關聯的密碼文字，而此金鑰是用來加密您的映像檔。如需相關資訊，請參閱[使用 API 包裝金鑰](/docs/services/key-protect?topic=key-protect-wrap-keys#api)。|
| 主要金鑰 ID | 匯入已加密的映像檔時，請指定用來包裝資料加密金鑰的主要金鑰 ID。如需相關資訊，請參閱[檢視金鑰](/docs/services/key-protect?topic=key-protect-view-keys#view-keys)。|
{: caption="表 2. 用於從 IBM Cloud Object Storage 匯入已加密映像檔的值" caption-side="top"}

## 後續步驟

匯入開始之後，系統會在所指定儲存區的 {{site.data.keyword.cos_full_notm}} 服務實例中尋找映像檔。映像檔會匯入成為映像檔範本，然後便可在「映像檔範本」頁面上存取該映像檔範本。匯入完成之後，映像檔可以用來訂購新的裝置或啟動現有的裝置。此外，也可以隨時刪除映像檔範本。映像檔匯入時間會隨著檔案大小而改變，但一般會需要數分鐘到一小時。

將映像檔匯入至映像檔範本儲存庫之後，您可以從 {{site.data.keyword.cos_full_notm}} 中刪除它。您可以繼續從**映像檔範本**頁面存取映像檔範本，並使用它來佈建虛擬伺服器實例。
