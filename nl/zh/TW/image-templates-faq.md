---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-30"

keywords:

subcollection: image-templates

---


{:new_window: target="_blank"}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}

# 常見問題：映像檔範本
{: #faq-image-templates}

## 何謂標準映像檔範本？
{: faq}

「標準映像檔」範本是 {{site.data.keyword.BluSoftlayer_notm}} 的 {{site.data.keyword.virtualmachineslong}} 映像選項。
標準映像檔範本允許使用者擷取現有虛擬伺服器的映像檔（不論其作業系統為何），然後根據映像檔建立新的虛擬伺服器。

## 何謂 ISO 範本？
{: faq}

「ISO 範本」是為了可用來進行 VSI 開機的 ISO 而特別保留的範本類型。ISO 範本提供兩種版本：公用及專用。公用 ISO 範本是預先配置的範本，由 {{site.data.keyword.BluSoftlayer_notm}} 提供並可供任何客戶使用。專用 ISO 範本的建立是透過匯入儲存在 {{site.data.keyword.objectstorageshort}} 帳戶上之 ISO 的映像檔。若要讓 ISO 匯入至「映像檔範本」畫面，該 ISO 必須是可開機的。

只有 {{site.data.keyword.BluSoftlayer_notm}} 支援的作業系統可以用來將 ISO 範本匯入到 VSI。如需相關資訊，請參閱[支援的作業系統 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.softlayer.com/services/software/)。
{:tip}

## 公用映像檔與專用映像檔之間的差異為何？
{: faq}

公用映像檔是可以由任何 {{site.data.keyword.BluSoftlayer_notm}} 使用者檢視及套用到新虛擬伺服器的映像檔。{{site.data.keyword.BluSoftlayer_notm}} 目前會建立公用映像檔作為不同裝置上的配置選項解決方案。客戶也可以讓映像檔成為公用且可供任何使用者使用。專用映像檔是只能由獲授權之使用者檢視的映像檔。獲授權的使用者預設為您帳戶上的任何使用者；不過，映像檔也可在多個帳戶之間，藉由更新 {{site.data.keyword.slportal}} 中的共用選項來共用。

## 我可以用自行管理的 Hypervisor 擷取及部署虛擬伺服器嗎？
{: faq}

只能擷取和部署由 {{site.data.keyword.BluSoftlayer_notm}} 佈建的伺服器。您在個人裝置上手動建立的個別虛擬伺服器，無法加以擷取、佈建或部署。

## 是否可以與其他客戶共用我的專用 ISO 範本？
{: faq}

唯一公開給所有客戶的 ISO 範本，是由 {{site.data.keyword.BluSoftlayer_notm}} 產生的範本。專用 ISO 範本是帳戶所特有，無法透過 {{site.data.keyword.slportal}} 在客戶之間共用。

## 我的 ISO 範本是否需要與我的虛擬伺服器在相同的資料中心裡，以便完成從映像檔開機？
{: faq}

是的，ISO 範本必須與虛擬伺服器在相同的資料中心裡，才能從映像檔開機。如果不在相同的資料中心，ISO 範本必須複製到適當的資料中心，以便完成動作。如果發生此狀況，會出現一則關於交易的警告。當 ISO 範本複製到不同的資料中心時，會向帳戶收取一筆小額的費用，方式就和複製其他類型的映像檔範本所套用的費用一樣。

## 實體資料與磁區大小之間的差異為何？
{: faq}

磁區是可用來儲存檔案的磁碟空間，而實體資料則是由儲存在磁碟上的實際檔案所組成。

## 何謂映像檔匯入/匯出特性？
{: faq}

映像檔匯入/匯出特性（位於 {{site.data.keyword.slportal}} 的「映像檔範本」頁面）允許將 {{site.data.keyword.objectstorageshort}} 帳戶上儲存的 VHD 和 ISO，轉換成映像檔範本，反之亦然。當您匯入映像檔時，特定的檔案（VHD 或 ISO）會從指定的 [{{site.data.keyword.objectstorageshort}}] 帳戶容器讀取，並轉換成映像檔範本。然後映像檔範本便可以用來開機或載入裝置。當您匯出映像檔時，映像檔範本會轉換成檔案（若範本有多個磁碟則是數個檔案），該檔案儲存在 {{site.data.keyword.objectstorageshort}} 帳戶容器上的指定位置。

## 如何為整個伺服器建立映像檔範本，而不只是我的主要磁碟機？
{: faq}

若要建立整個伺服器的映像檔範本，請參閱[建立映像檔範本](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)中的指示。

如果您選擇要將映像檔範本匯出至 IBM Cloud Object Storage，則每一個區塊裝置（或磁碟）都有它自己的關聯檔案。比方說，如果您的映像檔命名為 image.vhd，則第一個區塊裝置會匯出為 image-0.vhd。第二個區塊裝置會匯出為 image-1.vhd，依此類推。
{: tip}
