---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-31"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# 準備及匯入映像檔

[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 中的「映像檔範本」畫面，允許使用者從以 Swift 為基礎的 [Object Storage](/docs/infrastructure/objectstorage-swift/index.html) 帳戶上傳現有的映像檔。
{:shortdesc}

映像檔匯入為映像檔範本之後，可以用來佈建或啟動現有的虛擬伺服器。從 Object Storage 帳戶匯入的映像檔可以是 VHD 或自訂 ISO。VHD 匯入僅限於下列 64 位元作業系統：

* CentOS 6 及 7
* RedHat Enterprise Linux 6 及 7
* Ubuntu 14.04 及 16.04
* Microsoft Server Standard 2012、R2 2012 及 2016

VHD 匯入僅限於 100 GB 磁碟。VHD 必須根據下列範例來命名：filename.vhd-0.vhd。

## 將映像檔轉換成 VHD

VHD 格式是 {{site.data.keyword.BluVirtServers_full}} 唯一支援的映像檔格式。若要將映像檔轉換成 VHD，請使用下列資訊：

* 需要 Qemu-img 2.7.0 或更新版本
* 使用下列指令轉換映像檔： 
 
  ```
  qemu-img convert -f <image format> <image name> -O vpc -o force_size <image name>
  ```
  {: pre}
   
* 範例指令：
   
  ```
  qemu-img convert -f qcow2 test -O vpc -o force_size test
  ```
  {: pre}

如需相關資訊，請參閱 QEMU 文件中的 [Converting image formats ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats)。

## ISO 範本

只有 {{site.data.keyword.BluSoftlayer_notm}} 支援的作業系統可以用來將 ISO 範本匯入到 VSI。支援的作業系統清單可以在這裡找到：[http://www.softlayer.com/services/software/ ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.softlayer.com/services/software/)

使用此工具匯入的 ISO 必須可開機，映像檔才有匯入的資格。

## 配置 {{site.data.keyword.virtualmachinesshort}} 的映像檔

為了確保映像檔可以順利部署在 {{site.data.keyword.BluSoftlayer_notm}} 基礎架構環境，虛擬伺服器映像檔必須配置成下列規格：

* ***/boot*** 必須是第一個分割區
* ***/boot*** 和 ***/*** 必須是 ext3 或 ext4 檔案系統
* ***/etc***  和 /root 必須在與 ***/*** 相同的分割區上
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** 以裝載連接到系統的交換磁碟
* 必須安裝 wget
* 必須安裝最新的 xe-guest-utilities Xen 工具。請完成下列步驟：
    
    1. 從 Citrix 下載 XenServer ISO：[https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html)
    
    2. 執行下列指令裝載 ISO： 
    
        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}
    
    3. 執行下列指令找到 ISO 的 RPM：
    
        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}
    
    4. 輸出會列出類似如下的 RPM： 
    
        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}
     
     5. 您可以複製 RPM，然後擷取 ISO 中的 xentools。執行下列指令，以建立儲存 ISO 的目錄結構：
    
        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}
    
     6. 從結果的目錄結構，您可以裝載 *guest-tools* ISO 並找到 *rpm/debs* 以安裝 xentools。請參閱下列範例目錄結構：
     
        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}
    
如需啟用 cloud-init 之映像檔的相關資訊，請參閱[使用啟用 cloud-init 的映像檔進行佈建](image_cloud-init.html)。

## 匯入映像檔

請完成下列步驟，以在「客戶入口網站」中匯入映像檔。

1. 從 {{site.data.keyword.objectstorageshort}} 帳戶找到並記錄映像檔的下列詳細資料。如需相關資訊，請參閱[檢視及編輯 Object Storage 檔案詳細資料](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html)。
  * 帳戶名稱
  * 叢集
  * 容器
  * 映像檔檔名
2. 在[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 中，選取**裝置 > 管理 > 映像檔**來存取**映像檔範本**頁面。
3. 按一下**匯入映像檔**標籤，以開啟「匯入」工具。
4. 從**帳戶**下拉清單，針對您要匯入的映像檔選取 **{{site.data.keyword.objectstorageshort}} 帳戶**。
5. 從**叢集**下拉清單，針對您要匯入的映像檔選取 **{{site.data.keyword.objectstorageshort}} 叢集**。
6. 從**容器**下拉清單，針對您要匯入的映像檔選取 **{{site.data.keyword.objectstorageshort}} 容器**。
7. 從**映像檔**下拉清單，選取 {{site.data.keyword.objectstorageshort}} 中列出的**映像檔檔名**。
8. 在**映像檔名稱**欄位，輸入新映像檔範本的映像檔名稱。
9. 在**附註**文字框，輸入任何適用的附註。
10. 從**作業系統**下拉清單，選取映像檔的作業系統。

  如果要匯入的映像檔是自訂 ISO，則會停用「作業系統」下拉清單。只有在匯入牽涉到 VHD 時才需要此步驟。
{:tip}

11. 如果您匯入的映像檔已啟用 cloud-init，請選取 **Cloud Init** 勾選框。如需相關資訊，請參閱[使用啟用 cloud-init 的映像檔進行佈建](image_cloud-init.html)。        
12. 如果您計劃提供自己的作業系統授權，請選取**授權**勾選框。如需相關資訊，請參閱[使用 Red Hat Cloud Access](use-red-hat-cloud-access.html)。
13. 按一下**匯入**，以將映像檔匯入到「映像檔範本」畫面。按一下**取消**即可取消動作。

## 後續步驟

匯入開始之後，系統會在 {{site.data.keyword.objectstorageshort}} 帳戶中，使用指定路徑（帳戶 > 叢集 > 容器 > 映像檔）找到映像檔。映像檔會匯入成為映像檔範本，然後便可在「映像檔範本」頁面上存取該映像檔範本。匯入完成之後，映像檔可以用來訂購新的裝置或啟動現有的裝置。此外，可以隨時刪除映像檔。映像檔匯入時間會隨著檔案大小而改變，但一般會需要數分鐘到一小時。

