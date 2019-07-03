---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# 將映像檔匯出至 IBM Cloud Object Storage
{: #exporting-an-image-to-ibm-cloud-object-storage}

從「映像檔範本」頁面，您可以將映像檔範本匯出到  [{{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about) 帳戶。
{:shortdesc}

映像檔匯出處理程序會取用預先存在的專用標準映像檔範本，或已加密的映像檔範本，然後將映像檔轉換成儲存在 {{site.data.keyword.cos_full_notm}} 帳戶上之指定位置的映像檔。

*附註：* 如果您匯入 VMDK 映像檔，您可以使用 VHD 或 VMDK 格式匯出映像檔。由於映像檔格式之間的差異，可能會遺失資料。若要在資料遺失的情況下保護您的資料，要保留原始 VHD 檔案。

## 開始之前
首先，請導覽至裝置功能表，並確保您具備完成作業的正確帳戶許可權。

* 導覽至主控台的裝置功能表。如需相關資訊，請參閱[導覽至裝置](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)。
* 確保您具備 {{site.data.keyword.cos_full_notm}} 的寫入權。只有帳戶擁有者及具備**管理使用者**標準基礎架構許可權的使用者，才能調整許可權。

如需許可權的相關資訊，請參閱[標準基礎架構許可權](/docs/iam?topic=iam-infrapermission#infrapermission)及[管理裝置存取權](/docs/vsi?topic=virtual-servers-managing-device-access)。

如果您計劃將這個映像檔範本匯出至 {{site.data.keyword.cos_full_notm}}，請確定其名稱未包含可能在網址中造成問題的任何字元。例如，?、=、< 及其他特殊字元若未經過 URL 編碼，則會導致不想要的行為。
{: tip}

## 將映像檔匯出至 IBM Cloud Object Storage

請使用下列步驟，將映像檔範本匯出至 IBM Cloud Object Storage。

1. 從**裝置**功能表，選取**管理 > 映像檔**。
2. 針對您要匯出的映像檔範本按一下**動作**，然後選取**將映像檔匯出至 IBM COS**。如果具有您想要之配置的映像檔範本仍無法使用，請參閱[建立映像檔範本](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template)。
3. 填寫必要欄位（請參閱表 1）。
4. 按一下**確定**，將映像檔匯出至 {{site.data.keyword.cos_full_notm}} 帳戶中的指定位置。

| 欄位 | 值 |
| ----- | ----- |
| 檔名 | 輸入映像檔的檔名。 |
| {{site.data.keyword.cos_full_notm}} | 選取 {{site.data.keyword.cos_full_notm}} 帳戶。 |
| 位置 | 選取您要儲存映像檔範本的特定地理區域。 |
| 儲存區 | 選取您要儲存映像檔範本的 {{site.data.keyword.cos_full_notm}} 儲存區。只有存在於您所選地區位置中的儲存區才有效。如果您指定的儲存區不存在於選取的位置，則會收到錯誤訊息。|
| API 金鑰 | 指定具有 {{site.data.keyword.cos_full_notm}} 寫入權的服務 ID API 金鑰。如需相關資訊，請參閱[管理服務 ID API 金鑰](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys)。|
{: caption="表 1. 用於將映像檔匯出至 {{site.data.keyword.cos_full_notm}} 的值" caption-side="top"}
| 目標檔類型 | 選取您要匯出的檔案類型。如果您已匯入 VMDK 映像檔，您可以使用 VHD 或 VMDK 格式匯出映像檔。如果檔案最初是 VHD 格式，則只能匯出為 VHD。|

## 後續步驟
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
因為每個映像檔為不同的大小，且具備不同的性質，匯出處理程序可能需要數分鐘才能完成。

匯出映像檔之後，映像檔會留在映像檔範本的清單中，但它也以檔案的方式提供於匯出處理程序期間指定的 {{site.data.keyword.cos_full_notm}} 位置。您也可以從 {{site.data.keyword.cos_full_notm}} 下載映像檔。請從 {{site.data.keyword.cloud_notm}} 主控台的「資源清單」中存取 Cloud Object Storage 服務實例。在儲存映像檔的儲存區中，請選取您要下載的映像檔並選取**下載物件**。

將映像檔範本匯出至 IBM Cloud Object Storage 時，每一個區塊裝置（或磁碟）都有它自己的關聯檔案。比方說，如果您的映像檔命名為 image.vhd，則第一個區塊裝置會匯出為 image-0.vhd。第二個區塊裝置會匯出為 image-1.vhd，依此類推。
{: tip}
