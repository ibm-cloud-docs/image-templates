---

copyright:
  years: 2018
lastupdated: "2018-09-19"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}


# 建立已加密映像檔

作為「E2E 加密」功能的一部分，您可以加密要匯入至「映像檔範本」的映像檔，並使用它來部署已加密的虛擬伺服器實例。
{:shortdesc}

## 已加密映像檔需求
{: #encrypted-image-reqs}

您建立的已加密映像檔必須符合下列映像檔需求：

* 映像檔與「{{site.data.keyword.cloud}} 主控台」基礎架構環境相容。
* 映像檔包括 Linux 作業系統，例如 CentOS、Debian、Red Hat Enterprise Linux 或 Ubuntu。
* 映像檔已啟用 cloud-init。
* 映像檔是使用 [LUKS 磁碟加密](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption)進行加密。

## 使用 QEMU 及 DM-Crypt 來建立已加密的 RAW 映像檔
{: #luks-disk-encryption}

若要加密您的映像檔，您必須將固定 VHD 映像檔轉換為 RAW 格式。然後，使用 QEMU 及 DM-Crypt 來建立以 LUKS 磁碟加密的新映像檔。將檔案裝載為加密磁區，並將未加密的映像檔複製到已加密磁區。

此程序會引導您完成下列作業：

* 將動態 VHD 映像檔轉換為固定 VHD 映像檔。
* 將固定 VHD 映像檔轉換為 RAW 檔案格式。
* 建立您將用來加密磁碟機的資料加密金鑰檔。
* 建立新的 RAW 檔案，以包含映像檔及 LUKS 加密標頭。
* 將具有 LUKS 磁碟加密的 RAW 檔案格式化。
* 將已加密的 RAW 檔案附加至區塊裝置。
* 將未加密的映像檔複製到已加密的磁區裝置。

您必須具有專用權，可透過 sudo 使用 `root` 使用者權限執行指令，才能在系統上裝載和卸載已加密磁區。您可以發出下列指令來驗證 `root` 使用者專用權：

```
sudo echo "Hello!"
```
{: pre}

您必須在回覆中收到 "Hello!" 回應，才能完成此加密作業。

**提示**：您必須在執行 Linux 作業系統且提供下列套件的系統上，完成此加密作業：
* qemu 或 qemu-image（視您的 Linux 作業系統而定）
* cryptsetup

請完成下列步驟來加密您的映像檔。

1. 將動態 VHD 映像檔轉換為固定 VHD 映像檔。固定 VHD 檔案可防止動態 VHD 直接轉換為 RAW 檔案格式時可能發生的毀損。請執行下列指令，將動態 VHD 映像檔轉換為固定 VHD 映像檔：

  ```
  qemu-img convert -p -O vpc -o subformat=fixed <DYNAMIC_VHD_FILE> <FIXED_VHD_FILE>
  ```
  {: pre}

  比方說，如果動態 VHD 檔案是 _Rhel_7.vhd-0.vhd_，且您要將固定 VHD 檔案命名為 _Rhel_7.fixed.vhd-0.vhd_，請執行下列指令：

  ```
  $ qemu-img convert -p -O vpc -o subformat=fixed Rhel_7.vhd-0.vhd Rhel_7.fixed.vhd-0.vhd
  ```
  {: screen}

  此指令可能需要 30 分鐘以上才能完成。
  {: tip}

2. 使用 QEMU 將固定 VHD 映像檔轉換為 RAW 檔案格式。映像檔必須採用 RAW 檔案格式，然後您才能將其加密。請執行下列 QEMU 指令：

  ```
  qemu-img convert -p -O raw <FIXED_VHD_FILE> <RAW_IMAGE_FILE>
  ```
  {: pre}

  比方說，如果固定 VHD 檔案是 _Rhel_7.fixed.vhd-0.vhd_，且您要將 RAW 輸出檔命名為 _Rhel_7.raw-0.raw_，請執行下列指令：

  ```
  qemu-img convert -p -O raw Rhel_7.fixed.vhd-0.vhd Rhel_7.raw-0.raw
  ```
  {: screen}

  此指令可能需要 30 分鐘以上才能完成。
  {: tip}

3. 識別您將用來加密和解密磁碟機的資料加密金鑰。此資料加密金鑰是您將已加密映像檔匯入至 {{site.data.keyword.slportal}} 時，所包裝及指定的相同金鑰。建立一個檔案，其中包含您將用來加密及解密磁碟機的資料加密金鑰。在此檔案中，必須將金鑰解除包裝，並置於 base64 編碼文字中。Base64 可協助確保不會包括任何意外的空格或換行。Base64 編碼資料加密金鑰最少必須有 32 個字元或位元組，且最多可以有 512 個字元或位元組。資料加密金鑰資料必須在同一行上，沒有換行及新行字元。例如，使用下列指令來建立一個稱為 `secret.dek` 的檔案，為資料加密金鑰儲存 `unwrapped_key_material`，並使用 base64 將其編碼：

  ```
  $ echo -n $(echo 'unwrapped_key_material' | base64) > secret.dek
  ```
  {: screen}

  將此金鑰放在安全之處。如果遺失此金鑰，將無法解密您的磁碟。
  {: tip}

4. 識別要在下列步驟為已加密磁碟建立之新 RAW 檔案的正確大小，這會計入 LUKS 標頭的長度。_dmcrypt_ 和 _cryptsetup_ 將使用新的 RAW 檔案，以已加密的 LUKS 格式保留磁碟內容。新 RAW 檔案的大小必須是在步驟 2 中建立之 RAW 檔案的大小，與固定 4 MB LUKS 標頭的總和。若要判斷現有 RAW 映像檔的大小，請執行下列指令，其中 _Rhel_7.raw-0.raw_ 是映像檔名稱：

  ```
  ls -l Rhel_7.raw-0.raw
  ```
  {: pre}

  您將會看到如下輸出：

  ```
  -rw-r--r--. 1 user user 42949017600 Mar 5 10:02 Rhel_7.raw-0.raw
  ```
  {: screen}

  其中 _42949017600_ 是 RAW 映像檔正在使用的位元組數
      1. 將 4 MB（已轉換為位元組）新增至 RAW 映像檔大小，來計入 LUKS 標頭的長度。例如，42949017600 + (4 x 1024 x 1024) = 42953211904 個位元組。
      2. 將 RAW 映像檔大小及 LUKS 標頭的總和轉換為 MB，並捨入至下一個 MB。例如，42953211904 個位元組 / 1024 個位元組 / 1024 KB = 40963.375 MB = **40964 MB**.

5. 建立位元組數正確的新 RAW 檔案，其會變成已加密的 RAW 映像檔。請使用下列 _dd_ 指令來建立 RAW 檔案：

  ```
  dd if=/dev/zero of=<ENCRYPTED_RAW_FILENAME> bs=1M count=<TARGET_SIZE_IN_MEGABYTE> status=progress
  ```
  {: pre}

  其中 _ENCRYPTED_RAW_FILENAME_ 是將變成已加密 RAW 映像檔的檔案，而 _TARGET_SIZE_IN_MEGABYTE_ 是您從步驟 4 取得的數目，這是未加密 RAW 映像檔加上 LUKS 標頭的大小。

  比方說，如果您想要將變成已加密映像檔的新 RAW 檔案成為 _Rhel_7.encrypted.praw_，且其目標大小是 _40964_ MB，請執行下列指令：

  ```
  $ dd if=/dev/zero of=Rhel_7.encrypted.raw bs=1M count=40964 status=progress
  ```
  {: screen}

6. 使用 _dmcrypt_ 和資料加密金鑰（來自步驟 3），將具有 LUKS 磁碟加密的 RAW 檔案格式化。此步驟會準備檔案以包含映像檔。若要將具有 LUKS 加密的檔案格式化，請執行下列 _cryptsetup_ 指令：

  ```
  cryptsetup -v luksFormat <ENCRYPTED_RAW_FILENAME> <DEK_FILENAME>
  ```
  {: pre}

  其中 _ENCRYPTED_RAW_FILENAME_ 是您要加密之 RAW 檔案的檔名，而 _DEK_FILENAME_ 是儲存資料加密金鑰的檔案名稱。

  比方說，如果您的 RAW 檔案是 _Rhel_7.encrypted.raw_，而金鑰檔是 _secre.dek_，請輸入：

  ```
  $ cryptsetup -v luksFormat Rhel_7.encrypted.raw secret.dek
  ```
  {: screen}

  在執行 cryptsetup 指令之後，您會收到下列提示。此為預期提示，您必須回應 YES 以繼續。

  ```
  WARNING!
  ========
  This will overwrite data on Rhel_7.encrypted.raw irrevocably.
  Are you sure (Type uppercase yes): YES
  ```
  {: screen}

7. 將已加密的 RAW 檔案附加為磁區，以使新的區塊裝置與作業系統產生關聯。使用 _cryptsetup_，執行下列指令，以使用 luksOpen 選項來開啟已加密的 RAW 裝置。

  您必須具有 sudo 專用權，才能使用 `root` 使用者權限執行下列指令。
  {: tip}

  ```
  sudo cryptsetup -v luksOpen <ENCRYPTED_RAW_FILENAME> <VOLUME_NAME> --key-file <DEK_FILENAME>
  ```
  {: pre}

  其中 _ENCRYPTED_RAW_FILENAME_ 是已加密 RAW 檔案的檔名、_VOLUME_NAME_ 是 _dmcrypt_ 將嘗試建立的裝置名稱，而 _DEK_FILENAME_ 是資料加密金鑰的檔名。

  比方說，如果您的已加密 RAW 檔案是 _Rhel_7.encrypted.raw_、您想要將區塊裝置命名為 _encryptedVolume_，而且資料加密金鑰檔是 _secret.dek_，請使用下列指令：

  ```
  $ sudo cryptsetup -v luksOpen Rhel_7.encrypted.raw encryptedVolume --key-file secret.dek
  ```
  {: screen}

  當此步驟完成時，會建立一個新的區塊裝置，您可以在 _/dev/mapper/_ 下讀取並寫入其中。

8. 執行下列指令，確認已順利建立 LUKS 磁區對映程式：

  ```
  ls -l /dev/mapper/
  ```
  {: pre}

  您應該會看到類似下列範例的輸出：

  ```
  total 0
  lrw-rw---- 1 root root        7 Mar 5 22:22 encryptedVolume -> ../dm-0
  ```
  {: screen}

9. 將未加密的映像檔（來自步驟 2）複製到已加密的磁區裝置（來自步驟 7）。請執行下列 _dd_ 指令，將未加密的映像檔複製到已加密磁區：

  ```
  sudo dd if=<RAW_IMAGE_FILE> of=/dev/mapper/<VOLUME_NAME>
  ```
  {: pre}

  其中 _RAW_IMAGE_FILE_ 是未加密 RAW 映像檔（已在步驟 2 中建立）的檔名，而 _VOLUME_NAME_ 是已加密磁區區塊裝置（已在步驟 7 中建立）的名稱。

  比方說，如果您的 RAW_IMAGE_FILE 命名為 _Rhel_7.raw-0.raw_，且您的 VOLUME_NAME 是 _encryptedVolume_，請執行下列指令：

  ```
  $ sudo dd if=Rhel_7.raw-0.raw of=/dev/mapper/encryptedVolume status=progress
  ```
  {: screen}

  此指令可能需要 30 分鐘以上才能完成。
  {: tip}

10. 破壞磁區對映程式，並關閉 LUKS 與已加密資料檔的連線：

  ```
  sudo cryptsetup luksClose encryptedVolume
  ```
  {: pre}

  其中 _encryptedVolume_ 是已加密磁區區塊裝置的名稱。

  您的 _ENCRYPTED_RAW_FILENAME_ 現在已起始設定，您可以將它上傳至 IBM Cloud Object Storage。比方說，如果您的已加密 RAW 檔案是 _Rhel_7.encrypted.raw_，請將該映像檔上傳至 IBM Cloud Object Storage。
