---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# 使用啟用 cloud-init 的映像檔進行佈建
{: #provisioning-with-a-cloud-init-enabled-image}

當您訂購虛擬伺服器時，許多作業系統現在使用啟用 cloud-init 的映像檔來最佳化佈建時間。您也可以匯入您已啟用 cloud-init 的自訂映像檔。
{:shortdesc}

當您訂購沒有附加程式的虛擬伺服器時，下列作業系統現在預設為啟用 cloud-init 的映像檔。（附加程式包括其他軟體、佈建後 Script 及進階監視。）
* CentOS 7
* Debian 8, 9
* Red Hat Enterprise Linux 7.x
* Ubuntu 16.04、18.04
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

當您訂購具有啟用 cloud-init 之作業系統的虛擬伺服器時，您可以使用自訂佈建 Script 新增使用者資料或 meta 資料。在訂單表格上的「使用者資料」欄位中，輸入伺服器的選用性 cloud-init 使用者資料或選用性 meta 資料。

## 開始之前
首先，請導覽至裝置功能表，並確保您具備完成作業的正確帳戶許可權。

* 導覽至主控台的裝置功能表。如需相關資訊，請參閱[導覽至裝置](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)。
* 確保您具備所有必要的帳戶許可權及裝置存取權。只有帳戶擁有者及具備**管理使用者**標準基礎架構許可權的使用者，才能調整許可權。

如需許可權的相關資訊，請參閱[標準基礎架構許可權](/docs/iam?topic=iam-infrapermission#infrapermission)及[管理裝置存取權](/docs/vsi?topic=virtual-servers-managing-device-access)。

## 匯入啟用 cloud-init 的自訂映像檔

如果建立了啟用 cloud-init 的自訂映像檔，您可以在 {{site.data.keyword.slportal_full}}的「匯入映像檔」頁面上，將其指定為 cloud-init 映像檔。

若要存取映像檔範本的「匯入映像檔」頁面，並將映像檔標示為啟用 cloud-init，請完成下列步驟：
1. 從**裝置**功能表，選取**管理** > **映像檔**。
2. 按一下**匯入映像檔**標籤。
3. 完成匯入啟用 cloud-init 之映像檔的必要資訊，然後選取接近**作業系統**下拉方框處顯示的 **Cloud init** 勾選框。如需匯入映像檔的相關資訊，請參閱「匯入映像檔」。

## 將映像檔範本標示為啟用 cloud-init

如果您有啟用 cloud-init 的現有 VHD 映像檔範本，可以在映像檔範本的詳細資料頁面上將它指定為啟用 cloud-init。

若要存取映像檔範本並將它標示為啟用 cloud-init，請完成下列步驟：
1. 從**裝置**功能表，選取**管理** > **映像檔**。
2. 從範本清單按一下您要更新的映像檔範本名稱。
3. 在「映像檔範本詳細資料」頁面上，選取 **Cloud Init** 標題底下的**已啟用**勾選框，然後按一下**更新**。

## 使用從 cloud-init 佈建的虛擬伺服器建立的映像檔範本

Cloud-init 一般只執行一次。不過，如果您從啟用 cloud-init 的映像檔佈建虛擬伺服器，並且後來從該虛擬伺服器建立映像檔範本，則會記錄 UUID。如果該映像檔範本用來建立另一部虛擬伺服器，則 cloud-init 會再次執行。

## 建立啟用 cloud-init 的映像檔範本

如需配置映像檔的相關資訊，請參閱 [cloud-init 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloudinit.readthedocs.io/en/latest/)。

如需資料來源的相關資訊，請參閱[資料來源 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html)。要為環境建立 {{site.data.keyword.cloud_notm}} cloud-init 映像檔時，請使用 [Config Drive ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - 第 2 版資料來源以提供 meta 資料。

### Linux 需求
* Cloud-init 0.7.7 版或更新版本

### Windows 需求
* {{site.data.keyword.cloud_notm}} 基礎架構中的 Cloudbase-init Metadata Service 公用及專用網路支援。服務也會以 Windows 虛擬伺服器認證更新「客戶入口網站」。您可以存取服務，網址是 [https://github.com/softlayer/bluemix-cloudbase-init ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/softlayer/bluemix-cloudbase-init)。
* 如果您在環境中使用 Vyatta，必須配置 Vyatta 以允許對 API 負載平衡器的 API 呼叫。如需相關資訊，請參閱 [Brocade vRouter (Vyatta) Set up Guide for VMware Environments with File Storage](/docs/infrastructure/virtual-router-appliance?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#load-balancer-ips)。
