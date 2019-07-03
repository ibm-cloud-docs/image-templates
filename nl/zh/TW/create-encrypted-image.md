---

copyright:
  years: 2019
lastupdated: "2019-04-29"

keywords: VHD image file, encryption, encrypted image, image

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}


# 加密 VHD 映像檔 
{: #create-encrypted-image}

若要使用「E2E 加密」特性，您必須先使用 vhd-util 工具加密 VHD 映像檔，然後再將其匯入「映像檔範本」以佈建加密實例。支援兩種 AES 加密層次：AES 256 位元及 AES 512 位元。
{:shortdesc}

## 加密 VHD 映像檔需求
{: #encrypted-image-reqs}

加密 VHD 映像檔必須符合下列需求：

* 格式化為 VHD。
* 與「{{site.data.keyword.cloud}} 主控台」基礎架構環境相容。
* 使用[受支援 OS](/docs/infrastructure/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images) 佈建。
* 已啟用 Cloud-init。
* 使用 [vhd-util 工具](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool)加密。

## 加密您的 VHD 映像檔
{: #vhd-util-tool}

遵循這些步驟建立您的加密 VHD 映像檔：

1. 選取執行第 7 版或更高版本的 CentOS 系統，以加密 {{site.data.keyword.cloud_notm}} 的虛擬磁碟映像檔（VHD 檔）。如果您無法存取已安裝 CentOS 的實體硬體，您可以使用公用或專用主機在 {{site.data.keyword.cloud_notm}} 內部佈建具有 CentOS 7 的虛擬伺服器實例。用來加密 VHD 檔案的 CentOS 系統本身不需加密。

2. 登入 CentOS 系統並連接至您的客戶 VPN，然後[前往 SoftLayer 下載網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://downloads.service.softlayer.com/citrix/xen/){: new_window}，並選取 vhd-util 工具 RPM 套件檔：vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm   

   如果無法將 RPM 套件檔直接下載至 CentOS 系統，請將檔案下載到目前正在執行的工作站。然後，您可以使用安全副本 (scp) 指令將其上傳至 CentOS 系統。如果您在 {{site.data.keyword.cloud_notm}} 使用虛擬伺服器實例，請使用下列指令並使用系統的公用 IP 位址進行此上傳。

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

3. 使用下列指令安裝 RPM：

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. 識別加密及解密磁碟映像檔所需的 AES 加密金鑰，並將其寫入金鑰檔。此 AES 加密金鑰與您在[準備您的環境](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment)中使用金鑰管理服務提供之客戶根金鑰包裝的資料加密金鑰相同。寫入金鑰檔的金鑰資料必須解除包裝，且不能經過編碼。 

   因為 data_key 不是在金鑰檔中進行 base64 編碼，因此無法使用標準 ASCII 字元列印或檢視金鑰檔內容。
   {: tip}

   使用下列指令，以 **AES 256 位元**或 **AES 512 位元**加密金鑰建立金鑰檔： 
   
   ```
   echo <data_key> | base64 -d - > <keyfile_name>
   ```
   {: pre} 

   範例指令：

   ```
   echo Nrfen98EpMxF2B+wdgLfagzrqvgUZfMK4vL2T0NsT20ihrsNC9pUUHtizF6218pze8RLCgQ6kwxuE58IWLzgDA== | base64 -d - > aes512.dek
   ```
   {: screen}

5. 使用下列指令驗證您在上一步建立的金鑰檔：

   ```
   vhd-util key -C -k <keyfile_name>
   ```
   {: pre}

   含輸出的範例指令：

   ```
   vhd-util key -C -k aes512.dek
   vhd_util_read_key: using keyfile aes512.dek, Size (bytes) 64
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

   上一個範例指令輸出的第一行表示名為 `aes512.dek` 的金鑰檔包含 64 位元組的金鑰，而第二行列出的數字是 SHA256 雜湊或各個加密金鑰的安全雜湊值。包含 AES 256 位元加密金鑰的檔案輸出將指出 32 位元組的金鑰。
   {: tip} 

6. 使用下列指令來建立 VHD 檔案的已加密副本。`target_vhd` 表示包含 `source_vhd` 加密版本的檔案名稱。

   ```
   vhd-util copy -n <source_vhd> -N <target_vhd> -k <keyfile_name>
   ```
   {: pre}    

   範例指令：

   ```
   vhd-util copy -n debian8-ne.vhd -N debian8-aes512.vhd -k aes512.dek
   ```
   {: screen}

7. 使用下列指令驗證已加密 VHD 檔案。

   ```
   vhd-util key -p -n <vhd_filename>
   ```
   {: pre}

   含輸出的範例指令：

   ```
   vhd-util key -p -n debian8-aes512.vhd
   0000000000000000000000000000000000000000000000000000000000000000
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

如果 VHD 檔案已加密，則會在輸出中看到兩個雜湊值，如上一個範例所顯示。第一個雜湊全都是零。第二個雜湊是 AES 加密金鑰使用來加密及解密 VHD 的 SHA256 雜湊。確保 VHD 檔案的 SHA256 雜湊與**步驟 5** 中顯示的雜湊相同。

在**步驟 7** 中的範例指令，會建立名為 "debian8-aes512.vhd" 的新加密 VHD 檔案。它使用名為 "aes512.dek" 之金鑰檔中的 AES 512 位元加密金鑰進行加密。其加密的 SHA256 雜湊為 `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda`。
