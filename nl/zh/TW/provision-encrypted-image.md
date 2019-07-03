---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-13"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# 使用端對端 (E2E) 加密來佈建已加密的實例
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

「端對端 (E2E) 加密」特性可讓您自帶已加密且已啟用 cloud-init 的作業系統映像檔，而您先前已使用您所擁有及控制的資料加密金鑰來加密此映像檔。在完成部分環境設定之後，您可以將已加密的映像檔匯入至映像檔範本儲存庫，並使用它來佈建已加密的虛擬伺服器實例。E2E 加密可對與佈建的虛擬伺服器實例相關聯的儲存空間，提供靜態資料加密。
{:shortdesc}

「E2E 加密」會同時帶來數個 {{site.data.keyword.cloud}} 元件，以提供重要資訊的安全解決方案。

* {{site.data.keyword.keymanagementservicelong_notm}} 或 {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} 之類的 IBM 金鑰管理服務可保護加密金鑰的安全（請參閱表 1）。
* IBM {{site.data.keyword.iamshort}} (IAM) 可讓 Cloud Block Storage 服務存取金鑰管理系統，以及用來包裝資料加密金鑰的根金鑰。
* {{site.data.keyword.cos_full_notm}} 可在您上傳已加密的映像檔時安全地儲存它。
* 在 {{site.data.keyword.cloud_notm}} 主控台中，您可以匯入已加密的映像檔，並建立映像檔範本。
* 搭配可在「{{site.data.keyword.cloud_notm}} 主控台」基礎架構環境中使用的已加密映像檔範本，您可以佈建已加密的虛擬伺服器實例。
* 最後，您可以透過 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov) 來審核與已加密虛擬伺服器相關聯的事件。

## 加密金鑰管理服務

{{site.data.keyword.keymanagementserviceshort}} 及 {{site.data.keyword.hscrypto}}（現在於特定[地區](/docs/services/hs-crypto?topic=hs-crypto-regions#regions)提供）使用一般金鑰提供者 API，提供一致的方法來管理加密金鑰。{{site.data.keyword.cloud_notm}} 資料中心在幕後提供專用的 Hardware Security Module (HSM) 來保護您的金鑰。您可以從下列選項進行選擇： 

| 金鑰管理服務 | HSM 加密憑證 |
| ----- | ----- |
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/concepts?topic=key-protect-getting-started-tutorial#getting-started-tutorial) | FIPS 140-2 *Level 2* 相符性 |
| [{{site.data.keyword.hscrypto}}](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) | FIPS 140-2 *Level 4* 相符性 |
{: caption="表 1. 可用的金鑰管理服務選項" caption-side="top"}

## 準備您的環境

1. 您必須具有已升級的帳戶，才能對虛擬伺服器使用 E2E 加密。如需相關資訊，請參閱[切換至 IBM ID 及鏈結帳戶](/docs/account/softlayerlink.html)。

2. 使用金鑰管理服務來建立及管理金鑰。下列範例步驟是 {{site.data.keyword.keymanagementserviceshort}} 的特定步驟，但一般流程也適用於 {{site.data.keyword.hscrypto}}。如果您是使用 {{site.data.keyword.hscrypto}}，請參閱該服務的[文件](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started)，以取得相對應的指示。
      1. 佈建 [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision) 服務。
      2. 在 {{site.data.keyword.keymanagementservicelong_notm}} 中[建立](/docs/services/key-protect?topic=key-protect-create-root-keys)或[匯入](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys)根金鑰 (CRK)。
      3. **選用**：如果您願意，則可以[建立](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys)或[匯入](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys)標準金鑰進行解密。      
      4. [設定 {{site.data.keyword.cloud_notm}} Key Protect CLI 外掛程式](/docs/services/key-protect?topic=key-protect-set-up-cli)，如此您就可以[包裝資料加密金鑰](/docs/services/key-protect?topic=key-protect-cli-reference#kp-wrap)，您打算使用該資料加密金鑰以根金鑰加密 VHD 映像檔。將已加密的映像檔匯入至 {{site.data.keyword.cloud_notm}} 主控台時，您將需要與包裝的資料加密金鑰 (WDEK) 相關聯的密碼文字。  
         
       Key Protect 不會儲存其他鑑別資料 (AAD)，您仍可使用 AAD 針對最多包含 255 個字串的金鑰進一步保護其安全，每個字串皆以逗號分隔且最多包含 255 個字元。如果您提供 AAD 以進行金鑰包裝，請將資料儲存到安全位置，以確保您能夠針對未來的金鑰解除包裝要求存取及提供相同的 AAD。
       {: tip}
      
3. 從 IBM {{site.data.keyword.iamshort}} (IAM) 中，[授與](/docs/iam?topic=iam-serviceauth#create-auth) **Cloud Block Storage**（來源服務）與 **金鑰管理服務**（目標服務）之間的存取權。如果您從 {{site.data.keyword.cos_full_notm}} 匯入已加密的映像檔，則必須在 IAM 中針對金鑰管理服務[定義存取原則](/docs/iam?topic=iam-userroles#userroles)。
4. 在「IBM Cloud 主控台」中，建立 {{site.data.keyword.cos_full_notm}} 的實例，並建立儲存區來儲存資料。如需相關資訊，請參閱[開始使用 {{site.data.keyword.cos_full_notm}} 的入門指導教學](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)。
      1. 在佈建您的金鑰管理服務時，在相同地區位置中建立 {{site.data.keyword.cos_full_notm}} 實例。
      2. 當您建立儲存區時，**備援**設定必須是_地區_。
      3. 建立儲存區時，您可以選擇性地[使用您的金鑰將其加密](/docs/services/cloud-object-storage?topic=cloud-object-storage-encryption#encryption-kp)。   

## 準備已加密的映像檔

1. 選取在您要加密之 {{site.data.keyword.cloud_notm}} 基礎架構環境中運作的未加密映像檔。其中一個選項是使用現有的虛擬伺服器來[建立映像檔範本](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template)。如需相關資訊，請參閱[使用從 cloud-init 佈建的虛擬伺服器建立的映像檔範本](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server)。您也可以使用現有的 VHD 映像檔。請確定映像檔符合[已加密的映像檔需求](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs)。
2. 如果您是從 {{site.data.keyword.slportal}} 使用映像檔範本，請[將未加密的映像檔](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage)匯出至 {{site.data.keyword.cos_full_notm}}。
3. 將映像檔從 {{site.data.keyword.cos_full_notm}} 下載至安全的本端機器來加密映像檔。在服務儀表板中，選取**下載**動作，從儲存空間中擷取您的物件。您可以使用 Aspera 高速傳輸外掛程式，來下載大於 200 MB 的映像檔。
4. 使用 vhd-util 工具來[加密您的 VHD 映像檔](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image)。
5. 在 {{site.data.keyword.cos_full_notm}} 中，導覽至您的儲存區，然後按一下**新增物件**來[上傳](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload)已加密的映像檔。您可以使用 Aspera 高速傳輸外掛程式，來上傳大於 200 MB 的映像檔。

## 匯入已加密的映像檔並訂購實例

1. 使用 IBM {{site.data.keyword.iamshort}} (IAM)，在將已加密的映像檔匯入至 {{site.data.keyword.cloud_notm}} 主控台時，建立鑑別時使用的服務 ID。
      1. 建立[服務 ID](/docs/iam?topic=iam-serviceids#serviceids)。
      2. 指派[存取原則](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy)。指派這些服務的存取權：{{site.data.keyword.cos_full_notm}} 及金鑰管理。
      3. 建立[服務 ID 的 API 金鑰](/docs/iam?topic=iam-serviceidapikeys#create_service_key)。
      4. 如需相關資訊，請參閱[簡介 {{site.data.keyword.cloud_notm}} IAM 服務 ID 和 API 金鑰![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/blog/introducing-ibm-cloud-iam-service-ids-api-keys){: new_window}。
2. 從 {{site.data.keyword.cloud_notm}} 主控台中，[將已加密的映像檔匯入](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos)至「映像檔範本」頁面。
3. 從「映像檔範本」頁面中，您可以使用已加密的映像檔，來[訂購](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template)虛擬伺服器實例。
4. 使用佈建的已加密虛擬伺服器時，您可以選擇透過 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov) 審核[虛擬伺服器事件](/docs/vsi?topic=virtual-servers-at_events#at_events)。
