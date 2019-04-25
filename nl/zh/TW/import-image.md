---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-27"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:pre: .pre}
{:screen: .screen}


# 準備及匯入映像檔
{: #preparing-and-importing-images}

在 {{site.data.keyword.slportal_full}} 中的「映像檔範本」畫面容許您從 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage) 服務實例匯入映像檔。您可以匯入虛擬硬碟 (VHD) 或虛擬機器硬碟 (VMDK) 格式的映像檔。匯入之後，VMDK 映像檔會轉換為 VHD。將映像檔上傳至 {{site.data.keyword.cos_full_notm}} 服務實例中的儲存區之後，您可以將它匯入至 {{site.data.keyword.slportal}}中的映像檔範本儲存庫。
{:shortdesc}

您必須具有已升級的帳戶，才能從 {{site.data.keyword.cos_full_notm}} 匯入影像。如需相關資訊，請參閱[切換至 IBM ID 及鏈結帳戶](/docs/account/softlayerlink.html)。
{: tip}

您必須透過 {{site.data.keyword.cloud_notm}} 主控台 (cloud.ibm.com) 訂購 [IBM Cloud Object Storage 實例](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-order-storage#creating-a-new-ibm-cloud-platform-account)，以使用此匯入特性。不支援 control.softlayer.com 的 IBM Cloud Object Storage。
{: important}

映像檔匯入為映像檔範本之後，可以用來佈建或啟動現有的虛擬伺服器。從 {{site.data.keyword.cos_full_notm}} 服務實例匯入的映像檔可以是 VHD、VMDK 或自訂 ISO。VHD 及 VMDK 匯入僅限於下列 64 位元作業系統：  

* CentOS 6 及 7
* RedHat Enterprise Linux 6 及 7
* Ubuntu 14.04 及 16.04
* Microsoft Server Standard 2012、R2 2012 及 2016

匯入僅限於 100 GB 磁碟。映像檔必須根據下列範例來命名：filename.vhd-0.vhd 或 filename.vmdk-0.vmdk

## 將映像檔轉換成 VHD
{: #convert-to-vhd}

VHD 及 VMDK 格式是虛擬伺服器唯一支援的映像檔格式。若要將映像檔從 VMDK 之外的格式轉換為 VHD，請使用下列資訊：

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

如需啟用 cloud-init 之映像檔的相關資訊，請參閱[使用啟用 cloud-init 的映像檔進行佈建](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#provisioning-with-a-cloud-init-enabled-image)。

## 準備匯入已加密映像檔
{: #preparing-to-import-an-encrypted-image}

如果計畫匯入使用專屬資料加密金鑰加密的 VHD 映像檔，請確保完成[使用端對端 (E2E) 加密來佈建已加密的實例](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance)中所述的加密必要條件及指示。

您必須使用 vhd-util 工具來加密您的映像檔，其必須是 VHD 格式。如需相關資訊，請參閱[加密 VHD 映像檔](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image)。
{: important}

## 將映像檔上傳至 {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

當您的映像檔備妥時，您可以將它上傳至 {{site.data.keyword.cos_full_notm}}。請確定使用[地區位置](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints)中的儲存區。

1. 在 {{site.data.keyword.cos_full_notm}} 中，導覽至您的儲存區，然後按一下**新增物件**來[上傳](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload-data#upload-data)映像檔。
2. 使用 [Aspera](/docs/services/cloud-object-storage?topic=cloud-object-storage-use-aspera-high-speed-transfer#use-aspera-high-speed-transfer) 高速傳輸外掛程式，以擁有映像檔最快的上傳速度。

您可以使用 COS SDK 與 Aspera 搭配，在使用 Java、Python 或 NodeJS 時，於自訂應用程式內起始高速傳輸。如需相關資訊，請參閱[使用程式庫與 SDK](/docs/services/cloud-object-storage?topic=cloud-object-storage-use-aspera-high-speed-transfer#sdk)。
{: tip}


## 從 IBM Cloud Object Storage 匯入映像檔
{: #import-icos}

請完成下列步驟，從 {{site.data.keyword.cos_full_notm}} 匯入映像檔。

1. 在[{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 或 [{{site.data.keyword.cloud_notm}} 主控台 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/classic)中，存取**映像檔範本**頁面，方法是選取**裝置 > 管理 > 映像檔**。 

   如果您正在匯入已加密的映像檔，您必須使用 {{site.data.keyword.cloud_notm}} 主控台。
   {: important}

2. 按一下**從 IBM COS 匯入映像檔**標籤，以開啟「匯入」工具。
3. 填寫必要欄位（請參閱「表 1」）。
4. 從 {{site.data.keyword.cos_full_notm}} 完成匯入時，映像檔會出現在「映像檔範本」頁面上。

| 欄位 | 值 |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | 選取 {{site.data.keyword.cos_full_notm}} 服務實例，其中儲存您要匯入的映像檔。|
| 位置 | 選取要儲存映像檔的特定地理區域。您可以將映像檔匯入至下列地區及關聯的資料中心：US-South (DAL13)、US-East (WDC07)、EU-Great Britain (LON02)、EU-Germany (FRA02)、AP-Japan。將映像檔匯入至其中一個列出的資料中心之後，您可以將它移至另一個資料中心。|
| 儲存區 | 選取要儲存映像檔的 {{site.data.keyword.cos_full_notm}} 儲存區。只有存在於您所選地區位置中的儲存區才有效。如果您選取的儲存區不存在於選取的位置，則會收到錯誤訊息。|
| 映像檔 | 選取 {{site.data.keyword.cos_full_notm}} 服務實例中您要匯入的映像檔。支援檔案類型為 VHD（虛擬硬碟）、VMDK（虛擬機器磁碟）及 ISO。如果您是匯入已加密映像檔，則該映像檔必須是 VHD 檔案格式，且使用 vhd-util 工具加密。|
| 映像檔名稱 | 指定映像檔的敘述性名稱。這是您將用來佈建虛擬伺服器實例的映像檔。|
| API 金鑰 | 指定提供 {{site.data.keyword.cos_full_notm}} 服務實例之存取權的 API 金鑰。匯入已加密的映像檔時，API 金鑰也必須具有 Key Protect 的存取權。只在建立時才能複製或下載 API 金鑰。如果 API 金鑰遺失，您必須建立新的 API 金鑰。如需相關資訊，請參閱[使用 API 金鑰](/docs/iam?topic=iam-manapikey#manapikey)。|
| 作業系統 | 選取映像檔中包含的作業系統。|
| Cloud-init | 如果您匯入的映像檔已啟用 cloud-init，請選取此勾選框。如果您匯入的映像檔具有已啟用 cloud-init 的 Windows 作業系統，而且您選取此選項，則無法同時選取**您的授權**。如果您是匯入已加密的映像檔，則依預設會選取此選項，且因為已加密的映像檔必須已啟用 cloud-init，所以無法編輯。|
| 您的授權 | 如果您計劃提供自己的作業系統授權，請選取此勾選框。如果匯入的映像檔具有 Windows 作業系統，則在您計劃使用映像檔來佈建[專用主機實例](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances)時，可以選取此選項。如果您的 Windows 作業系統版本不支援使用您自己的授權，則會停用此選項。對於 Windows 映像檔，如果您指定將使用自己的授權，則無法選取 Cloud Init。如果匯入的已加密映像檔具有 Red Hat Enterprise Linux 作為作業系統，則依預設會選取此選項，且因為已加密的映像檔必須包含自己的作業系統授權，所以無法編輯。|
| 開機模式 | 為您的映像檔選取開機模式。如果已為您指定的作業系統設定預設開機模式，則會在這裡自動選取該開機模式。|
| 附註 | 新增任何與映像檔相關且可能有助於使用者的附註。|
| 加密 | 如果您要使用 vhd-util 工具匯入使用專屬資料加密金鑰加密的映像檔，請選取此勾選框。|
{: caption="表 1. 用於從 IBM Cloud Object Storage 匯入映像檔的值" caption-side="top"}

下表顯示僅適用於匯入已加密映像檔的其他欄位。如需已加密映像檔的相關資訊，請參閱[使用端對端 (E2E) 加密來佈建已加密的實例](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance)。

<!--
To import an encrypted image, your account must have access to the End to End (E2E) Encryption feature. To enable your account for E2E Encryption, please contact [Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
{: tip}
-->

| 欄位 | 值 |
| ----- | ----- |
| 包裝的資料加密金鑰 | 匯入已加密的映像檔時，請指定與資料加密金鑰相關聯的密碼文字，而此金鑰是用來加密您的映像檔。如需相關資訊，請參閱[使用 API 包裝金鑰](/docs/services/key-protect?topic=key-protect-wrap-keys#api)。|
| {{site.data.keyword.keymanagementserviceshort}} 服務實例 | 在您的帳戶中，從下拉清單中選取 Key Protect 實例。Key Protect 實例必須包含用來包裝資料加密金鑰的客戶根金鑰。|
| 金鑰名稱 |在 {{site.data.keyword.keymanagementserviceshort}} 服務實例內選取您用來包裝資料加密金鑰的根金鑰名稱。如需相關資訊，請參閱[檢視金鑰](/docs/services/key-protect?topic=key-protect-view-keys#view-keys)。|
{: caption="表 2. 用於從 IBM Cloud Object Storage 匯入已加密映像檔的值" caption-side="top"}

## 後續步驟

匯入開始之後，系統會在所指定儲存區的 {{site.data.keyword.cos_full_notm}} 服務實例中尋找映像檔。映像檔會匯入成為映像檔範本，然後便可在「映像檔範本」頁面上存取該映像檔範本。匯入完成之後，映像檔可以用來訂購新的裝置或啟動現有的裝置。此外，也可以隨時刪除映像檔範本。映像檔匯入時間會隨著檔案大小而改變，但一般會需要數分鐘到一小時。

將映像檔匯入至映像檔範本儲存庫之後，您可以從 {{site.data.keyword.cos_full_notm}} 中刪除它。您可以繼續從**映像檔範本**頁面存取映像檔範本，並使用它來佈建虛擬伺服器實例。
