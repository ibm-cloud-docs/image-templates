---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-27"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 匯出映像檔

從「映像檔範本」頁面，您可以將映像檔範本匯出到 Object Storage 帳戶。
{:shortdesc}

映像檔匯出處理程序會取用預先存在的專用標準映像檔範本，然後將映像檔轉換成儲存在 Object Storage 帳戶上之指定位置的映像檔。請使用下列步驟來匯出映像檔範本。

1. 從[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 的**裝置**功能表，選取**管理 > 映像檔**。
2. 針對您要匯出的映像檔範本按一下**動作**，然後選取**匯出映像檔**。如果具有您想要之配置的映像檔範本仍無法使用，請參閱[建立標準映像檔](create-standard-image.html)。
3. 在「匯出映像檔」頁面上，在**檔名**欄位中輸入映像檔的檔名。
5. 從**帳戶**下拉清單，選取 **Object Storage 帳戶**。
6. 從**叢集**下拉清單，選取 **Object Storage 叢集**。
7. 從**容器**下拉清單，選取 **Object Storage 容器**。
8. 按一下**匯出映像檔**，將映像檔匯出到 Object Storage 帳戶中的指定位置。按一下**關閉**即可取消動作。

## 後續步驟

匯出映像檔之後，映像檔會留在映像檔範本的清單中，但它也以檔案的方式提供於匯出處理程序期間指定的 Object Storage 位置。如需檢視匯出到 Object Storage 帳戶之檔案的相關資訊，請參閱[檢視及編輯 Object Storage 檔案詳細資料](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html)。因為每個映像檔為不同的大小，且具備不同的性質，匯出處理程序可能需要數分鐘才能完成。平均值匯出速度是每分鐘 2 GB。如果數分鐘過去，映像檔仍然無法在 Object Storage 帳戶上使用，[請與支援中心聯絡](/docs/get-support/howtogetsupport.html)。

