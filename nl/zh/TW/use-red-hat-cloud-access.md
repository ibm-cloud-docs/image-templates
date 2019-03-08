---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-04"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}


# 使用您自己的 OS 授權或訂閱

當您使用 VHD 映像檔建立映像檔範本時，可以選擇透過 [Red Hat Cloud Access ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) 訂閱提供自己的 RHEL 作業系統授權，或透過 Microsoft 企業合約提供 Windows 授權。
{:shortdesc}

如果您在 {{site.data.keyword.BluSoftlayer_full}} 部署了一個映像檔，指出您正在使用自己的授權，則會存在下列支援條款：
* {{site.data.keyword.IBM_notm}} 提供 Hypervisor、佈建實例、匯入映像檔、重新開機映像檔、重新載入 OS 及擷取映像檔等支援。
* 您向其購買作業系統授權的公司則提供對於映像檔本身的支援。{{site.data.keyword.IBM_notm}} 不提供映像檔的支援。

當您自行提供映像檔的授權時，下列限制適用於該映像檔：
* 映像檔是專用映像檔。它無法公開地共用。
* 映像檔無法包含任何軟體增益集。其他軟體必須在佈建映像檔之後新增。

## 使用 Red Hat Cloud Access
如需我們身為 Red Hat Enterprise Linux 雲端提供者的憑證相關資訊，請參閱 [Infrastructure as a Service (Iaas) ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://access.redhat.com/ecosystem/cloud-provider/2262101)。

## 使用您自己的 Windows 授權
支援下列作業系統：
* Windows Server 2016
* Windows Server 2012
* Windows Server 2012 R2

如果您對於現有 Windows 授權資格有問題，或是不瞭解報告需求，請與您的 Microsoft 代表聯絡。當您建立映像檔範本，指定您使用自己的 Windows 授權，則必須在專用主機上佈建該映像檔。當您使用的映像檔指出您是使用自己的 Windows 授權時，您無法佈建公用實例或自動指派給主機的專用實例。此外，當您建立或更新 Windows 映像檔範本，而它指定您使用自己的授權，Windows 映像檔便不得為 cloud-init 映像檔。

## 匯入自行指定授權的映像檔

您可以匯入 VHD 映像檔，然後指定您要針對作業系統提供自己的授權或訂閱。

若要存取映像檔範本的「匯入映像檔」頁面，並將 VHD 映像檔標示為使用您自己的授權或訂閱，請完成下列步驟：
1. 從**裝置**功能表，選取**管理 > 映像檔**。
2. 按一下**匯入映像檔**標籤。
3. 完成匯入  VHD 映像檔的必要資訊，然後選取接近**作業系統**下拉方框處顯示的**授權**勾選框。如需匯入映像檔的相關資訊，請參閱[準備及匯入映像檔](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images)。

## 更新映像檔範本以指定使用者提供的 OS 授權

如果您有現有 VHD 映像檔範本，可以指定您要針對作業系統提供自己的授權或訂閱。

若要存取映像檔範本並指定它使用您自己的現有授權或訂閱，請完成下列步驟：
1. 從**裝置**功能表，選取**管理 > 映像檔**。
2. 從範本清單按一下您要更新的映像檔範本名稱。
3. 在「映像檔範本詳細資料」頁面上，選取 **OS 授權**標題底下的**使用者提供**勾選框，然後按一下**更新**。
