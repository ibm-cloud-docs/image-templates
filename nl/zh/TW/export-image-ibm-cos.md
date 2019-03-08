---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-02"

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
{: #exporting-to-ibm-cos}

從「映像檔範本」頁面，您可以將映像檔範本匯出至 [IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage) 帳戶。
{:shortdesc}

映像檔匯出處理程序會取用預先存在的專用標準映像檔範本，或已加密的映像檔範本，然後將映像檔轉換成儲存在 IBM Cloud Object Storage 帳戶上之指定位置的映像檔。

請使用下列步驟，將映像檔範本匯出至 IBM Cloud Object Storage。

1. 使用對 IBM Cloud Object Storage 具有寫入權的服務 ID，向 {{site.data.keyword.slportal}} 進行鑑別。
2. 從 [{{site.data.keyword.slportal_full}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 的**裝置**功能表，選取**管理 > 映像檔**。
3. 針對您要匯出的映像檔範本按一下**動作**，然後選取**將映像檔匯出至 IBM COS**。如果具有您想要之配置的映像檔範本仍無法使用，請參閱[建立映像檔範本](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)。
4. 填寫必要欄位（請參閱表 1）。
5. 按一下**確定**，將映像檔匯出至 IBM Cloud Object Storage 帳戶中的指定位置。

| 欄位 | 值 |
| ----- | ----- |
| 檔名 | 輸入映像檔的檔名。 |
| {{site.data.keyword.cos_full_notm}} | 選取 {{site.data.keyword.cos_full_notm}} 帳戶。 |
| 位置 | 選取您要儲存映像檔範本的特定地理區域。 |
| 儲存區 | 選取您要儲存映像檔範本的 {{site.data.keyword.cos_full_notm}} 儲存區。只有存在於您所選地區位置中的儲存區才有效。如果您指定的儲存區不存在於選取的位置，則會收到錯誤訊息。|
| API 金鑰 | 指定具有 {{site.data.keyword.cos_full_notm}} 寫入權的服務 ID API 金鑰。如需相關資訊，請參閱[管理服務 ID API 金鑰](/docs/iam?topic=iam-serviceidapikeys)。|
{: caption="表 1. 用於將映像檔匯出至 IBM Cloud Object Storage 的值" caption-side="top"}

## 後續步驟
因為每個映像檔為不同的大小，且具備不同的性質，匯出處理程序可能需要數分鐘才能完成。

在匯出映像檔之後，映像檔會留在映像檔範本的清單中，但它也以檔案的方式提供於匯出處理程序期間指定的 IBM Cloud Object Storage 位置。

將映像檔範本匯出至 IBM Cloud Object Storage 時，每一個區塊裝置（或磁碟）都有它自己的關聯檔案。比方說，如果您的映像檔命名為 image.vhd，則第一個區塊裝置會匯出為 image-0.vhd。第二個區塊裝置會匯出為 image-1.vhd，依此類推。
{: tip}
