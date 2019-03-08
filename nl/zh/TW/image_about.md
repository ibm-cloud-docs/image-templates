---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-03"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 關於映像檔範本

標準映像檔範本為所有 {{site.data.keyword.BluVirtServers_short}} 標準映像選項，不論其作業系統為何。
{:shortdesc}

使用標準映像檔範本，您可以擷取現有虛擬伺服器的映像檔，然後根據擷取到的映像檔建立新的虛擬伺服器。標準映像檔範本與裸機伺服器不相容。

## 映像檔範本的運作方式
任何映像檔範本的兩個主要動作是建立和部署。當您要求建立映像檔時，{{site.data.keyword.BluSoftlayer_full}} 的自動系統會讓您的伺服器離線。伺服器離線之後，會建立資料的壓縮備份、記錄配置資訊，並將映像檔範本儲存在 {{site.data.keyword.BluSoftlayer_notm}} SAN 上。在映像檔範本的部署階段期間，自動系統會建構以從所選取映像檔收集之資料為基礎的新伺服器。部署程序會針對資料量進行調整、還原複製的資料，然後針對新主機進行最後的配置變更（例如網路配置）。

## 專用映像檔

專用映像檔是由帳戶上的使用者所建立的映像檔，或是在另一個帳戶建立然後共用的映像檔。依預設，所建立的所有映像檔都是專用映像檔。

## 公用映像檔

公用映像檔是預先配置的虛擬伺服器，由 {{site.data.keyword.BluSoftlayer_notm}} 張貼或由客戶公開，且可供所有 {{site.data.keyword.BluSoftlayer_notm}} 客戶使用。透過公用映像檔範本佈建的虛擬伺服器，其配置一般都會有最佳效能，並具有不同配置規格的特定組合。
