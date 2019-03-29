---

copyright:
  years: 2018
lastupdated: "2018-08-09"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 使用 Activity Tracker 來審核虛擬伺服器事件
{: #auditing-virtual-server-events-with-activity-tracker}

您可以使用 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov)，在虛擬伺服器實例的整個生命週期加以審核。您必須具有 Activity Tracker 的實例，且其已在美國南部佈建超值服務方案。超值方案可讓您存取 Kibana 儀表板，其中具有其他過濾及搜尋審核日誌的選項。

在 {{site.data.keyword.cloud}} 帳戶擁有者的帳戶日誌區段中，可以看到日誌。帳戶擁有者可以檢視下列日誌：
* 由帳戶擁有者觸發的事件。
* 由與帳戶鏈結的使用者觸發的事件。

您可以透過 Activity Tracker 審核下列虛擬伺服器實例事件：
* 佈建虛擬伺服器實例
* 停用專用或公用介面的埠
* 啟用專用或公用介面的埠
* 設定埠速度
* 建立映像檔範本
* 關閉虛擬伺服器實例電源
* 開啟虛擬伺服器實例電源
* 重新啟動虛擬伺服器實例
* 重新命名虛擬伺服器實例
* 進入虛擬伺服器實例的救援模式
* 將安全群組新增至虛擬伺服器實例的公用或專用介面
* 從虛擬伺服器實例的公用或專用介面中移除安全群組
* 從映像檔載入虛擬伺服器實例
* 從映像檔啟動虛擬伺服器實例
* 取消虛擬伺服器實例裝置

對於與網路介面相關聯的事件，審核日誌不會區分事件是發生在公用介面還是專用介面上。
{: tip}
