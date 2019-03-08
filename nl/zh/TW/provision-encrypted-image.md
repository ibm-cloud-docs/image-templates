---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-21"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# 使用端對端 (E2E) 加密來佈建已加密的實例

「端對端 (E2E) 加密」功能可讓您自帶已加密且已啟用 cloud-init 的作業系統映像檔，而您先前已使用您所擁有及控制的資料加密金鑰來加密此映像檔。在完成部分環境設定之後，您可以將已加密的映像檔匯入至映像檔範本儲存庫，並使用它來佈建已加密的虛擬伺服器實例。E2E 加密可對與佈建的虛擬伺服器實例相關聯的儲存空間，提供靜態資料加密。若要存取此功能，請與「支援中心」聯絡。
{:shortdesc}

「E2E 加密」會同時帶來數個 {{site.data.keyword.cloud}} 元件，以提供重要資訊的安全解決方案。

* {{site.data.keyword.keymanagementservicefull_notm}} 利用 FIPS 140-2 Level 2 認證的雲端型硬體安全模組 (HSM) 來保護金鑰，以防止資訊遭竊。
* IBM {{site.data.keyword.iamshort}} (IAM) 可讓 Cloud Block Storage 服務存取 {{site.data.keyword.keymanagementserviceshort}}，以及用來包裝資料加密金鑰的主要金鑰。
* {{site.data.keyword.cos_full_notm}} 可在您上傳已加密的映像檔時安全地儲存它。
* 在 {{site.data.keyword.slportal}} 中，您可以匯入已加密的映像檔，並建立映像檔範本。
* 搭配可在「{{site.data.keyword.cloud_notm}} 主控台」基礎架構環境中使用的已加密映像檔範本，您可以佈建已加密的虛擬伺服器實例。
* 最後，您可以選擇透過 Activity Tracker 來審核與已加密虛擬伺服器相關聯的事件。

## 準備您的環境

1. 您必須具有已升級的帳戶，才能對虛擬伺服器使用 E2E 加密。如需相關資訊，請參閱[切換至 IBM ID 及鏈結帳戶](/docs/account?topic=account-unifyingaccounts)。
2. 使用 {{site.data.keyword.keymanagementservicefull_notm}} 來建立及管理金鑰。
      1. 佈建 [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision) 服務。
      2. 在 {{site.data.keyword.keymanagementservicelong_notm}} 中[建立](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys)或[匯入](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys)主要金鑰 (CRK)。
      3. **選用**：如果您願意，則可以[建立](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys)或[匯入](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys)標準金鑰進行解密。
      4. 取得 [{{site.data.keyword.keymanagementserviceshort}} API 的存取權](/docs/services/key-protect?topic=key-protect-set-up-api#set-up-api)，讓您可以包裝資料加密金鑰。
      5. [包裝資料加密金鑰](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys)與主要金鑰。將已加密的映像檔匯入至 {{site.data.keyword.slportal}} 時，您將需要與包裝的資料加密金鑰 (WDEK) 相關聯的密碼文字。
3. 從 IBM {{site.data.keyword.iamshort}} (IAM) 中，[授與](/docs/iam?topic=iam-serviceauth#create-an-authorization) **Cloud Block Storage**（來源服務）與 {{site.data.keyword.keymanagementservicelong_notm}}（目標服務）之間的存取權。從 {{site.data.keyword.cos_full_notm}} 匯入已加密映像檔的使用者，必須在 IAM 中針對 {{site.data.keyword.keymanagementservicelong_notm}} [定義存取原則](/docs/iam?topic=iam-userroles)。
4. 在「IBM Cloud 主控台」中，建立 {{site.data.keyword.cos_full_notm}} 的實例，並建立儲存區來儲存資料。如需相關資訊，請參閱[開始使用 {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-console-#getting-started-console-)。
      1. 在佈建您的 {{site.data.keyword.keymanagementserviceshort}} 服務時，在相同地區位置中建立 {{site.data.keyword.cos_full_notm}} 實例。
      2. 當您建立儲存區時，**備援**設定必須是_地區_。
      3. 建立儲存區時，您可以選擇性地[使用您的金鑰將其加密](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-manage-encryption#sse-kp)。   

## 準備已加密的映像檔

1. 選取在您要加密之 {{site.data.keyword.cloud_notm}} 基礎架構環境中運作的未加密映像檔。其中一個選項是使用現有的虛擬伺服器來[建立自訂映像檔](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)。如需相關資訊，請參閱[使用從 cloud-init 佈建的虛擬伺服器建立的映像檔範本](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-a-standard-image-created-from-a-cloud-init-provisioned-virtual-server)。您也可以使用現有的 VHD 映像檔。請確定映像檔符合[已加密的映像檔需求](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#encrypted-image-reqs)。
2. 如果您是從 {{site.data.keyword.slportal}} 使用映像檔範本，請[將未加密的映像檔](/docs/infrastructure/image-templates?topic=image-templates-exporting-to-ibm-cos)匯出至 {{site.data.keyword.cos_full_notm}}。
3. 將映像檔從 {{site.data.keyword.cos_full_notm}} 下載至安全的本端機器來加密映像檔。在服務儀表板中，選取**下載**動作，從儲存空間中擷取您的物件。您可以使用 Aspera 高速傳輸外掛程式，來下載大於 200 MB 的映像檔。
4. 使用 LUKS 磁碟加密格式，藉由使用 QEMU 和 DMCrypt 來[加密您的映像檔](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption)。
5. 在 {{site.data.keyword.cos_full_notm}} 中，導覽至您的儲存區，然後按一下**新增物件**來[上傳](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data)已加密的映像檔。您可以使用 Aspera 高速傳輸外掛程式，來上傳大於 200 MB 的映像檔。

## 匯入已加密的映像檔並訂購實例

1. 使用 IBM {{site.data.keyword.iamshort}} (IAM)，在將已加密的映像檔匯入至 {{site.data.keyword.slportal}} 時，建立鑑別時使用的服務 ID。
      1. 建立[服務 ID](/docs/iam?topic=iam-serviceids#serviceids)。
      2. 指派[存取原則](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy)。指派這些服務的存取權：{{site.data.keyword.cos_full_notm}} 和 {{site.data.keyword.keymanagementservicelong_notm}}。
      3. 建立[服務 ID 的 API 金鑰](/docs/iam?topic=iam-serviceidapikeys#creating-an-api-key-for-a-service-id)。
      4. 如需相關資訊，請參閱[簡介 {{site.data.keyword.cloud_notm}} IAM 服務 ID 和 API 金鑰![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window}。
2. 從 {{site.data.keyword.slportal}} 中，[將已加密的映像檔匯入](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images#import-icos)至「映像檔範本」頁面。（您也可以[使用 {{site.data.keyword.slapi_short}}](/docs/infrastructure/image-templates?topic=image-templates-importing-an-encrypted-image-by-using-the-softlayer-api) 來匯入已加密的映像檔。）
3. 從「映像檔範本」頁面中，您可以使用已加密的映像檔，來[訂購](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template)虛擬伺服器實例。
4. 使用佈建的已加密虛擬伺服器時，您可以選擇透過 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov) 審核[虛擬伺服器事件](/docs/vsi?topic=virtual-servers-at_events#at_events)。
