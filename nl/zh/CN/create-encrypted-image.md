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


# 创建加密映像

作为 E2E 加密功能的一部分，您可以对映像加密以导入到映像模板，然后将该映像用于部署加密虚拟服务器实例。
{:shortdesc}

## 加密映像需求
{: #encrypted-image-reqs}

创建的加密映像必须满足以下映像需求：

* 映像与 {{site.data.keyword.cloud}} 控制台基础架构环境兼容。
* 映像包含 Linux 操作系统，例如 CentOS、Debian、Red Hat Enterprise Linux 或 Ubuntu。
* 映像已启用 cloud-init。
* 映像已使用 [LUKS 磁盘加密](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption)进行加密。

## 使用 QEMU 和 DM-Crypt 创建加密的 RAW 映像
{: #luks-disk-encryption}

要对映像加密，必须将静态 VHD 映像文件转换为 RAW 格式。然后，使用 QEMU 和 DM-Crypt 通过 LUKS 磁盘加密来创建新的映像文件。将文件安装为加密卷，然后将未加密的映像文件复制到加密卷中。

此过程将全程指导您完成以下任务：

* 将动态 VHD 映像转换为静态 VHD 映像。
* 将静态 VHD 映像转换为 RAW 文件格式。
* 创建将用于加密驱动器的数据加密密钥文件。
* 创建新的 RAW 文件，以包含映像以及 LUKS 加密头。
* 使用 LUKS 磁盘加密来设置 RAW 文件的格式。
* 将加密的 RAW 文件连接到块设备。
* 将未加密的映像复制到加密卷设备中。

您必须具有相应特权来使用 `root` 用户权限通过 sudo 运行命令，以在系统上安装和卸载加密卷。可以发出以下命令来验证 `root` 用户特权：

```
sudo echo "Hello!"
```
{: pre}

您必须在回复中收到“Hello!”回传信息才能完成此加密任务。

**提示**：您必须在运行 Linux 操作系统且有以下包可用的系统上完成此加密任务：
* qemu 或 qemu-image（取决于您的 Linux 操作系统）
* cryptsetup

要对映像加密，请完成以下步骤。

1. 将动态 VHD 映像文件转换为静态 VHD 映像文件。静态 VHD 文件可防止在动态 VHD 直接转换为 RAW 文件格式时可能发生的损坏。运行以下命令以将动态 VHD 映像文件转换为静态 VHD 映像文件：

  ```
  qemu-img convert -p -O vpc -o subformat=fixed <DYNAMIC_VHD_FILE> <FIXED_VHD_FILE>
  ```
  {: pre}

  例如，如果动态 VHD 文件为 _Rhel_7.vhd-0.vhd_，并且您希望将静态 VHD 文件命名为 _Rhel_7.fixed.vhd-0.vhd_，请运行以下命令：

  ```
  $ qemu-img convert -p -O vpc -o subformat=fixed Rhel_7.vhd-0.vhd Rhel_7.fixed.vhd-0.vhd
  ```
  {: screen}

  此命令可能需要 30 分钟或更长时间才能完成。
  {: tip}

2. 使用 QEMU 将静态 VHD 映像文件转换为 RAW 文件格式。映像必须为 RAW 文件格式后，才能对其进行加密。运行以下 QEMU 命令：

  ```
  qemu-img convert -p -O raw <FIXED_VHD_FILE> <RAW_IMAGE_FILE>
  ```
  {: pre}

  例如，如果静态 VHD 文件为 _Rhel_7.fixed.vhd-0.vhd_，并且您希望 RAW 输出文件的名称为 _Rhel_7.raw-0.raw_，请运行以下命令：

  ```
  qemu-img convert -p -O raw Rhel_7.fixed.vhd-0.vhd Rhel_7.raw-0.raw
  ```
  {: screen}

  此命令可能需要 30 分钟或更长时间才能完成。
  {: tip}

3. 确定将用于加密和解密驱动器的数据加密密钥。此数据加密密钥是将加密映像导入到 {{site.data.keyword.slportal}} 时包装并指定的密钥。创建一个文件，其中包含将用于加密和解密驱动器的数据加密密钥。在此文件中，必须对密钥解包，并且密钥格式必须为 Base64 编码文本。Base64 有助于确保不包含意外空格或换行符。Base64 编码的数据加密密钥必须至少包含 32 个字符或字节，并且至多包含 512 个字符或字节。数据加密密钥资料必须位于一行上，不包含任何换行符。例如，使用以下命令来创建名为 `secret.dek` 的文件，以存储数据加密密钥的 `unwrapped_key_material`，并使用 Base64 对其进行编码：

  ```
  $ echo -n $(echo 'unwrapped_key_material' | base64) > secret.dek
  ```
  {: screen}

  妥善保存此密钥。如果丢失此密钥，将无法解密磁盘。
  {: tip}

4. 确定要在以下步骤中为加密磁盘创建的新 RAW 文件的正确大小，计算时要加上 LUKS 头。新的 RAW 文件将由 _dmcrypt_ 和 _cryptsetup_ 用于保存加密 LUKS 格式的磁盘内容。新 RAW 文件的大小必须等于步骤 2 中创建的 RAW 文件的大小与常量 4 MB LUKS 头的总和。要确定现有 RAW 映像的大小，请运行以下命令，其中 _Rhel_7.raw-0.raw_ 是映像名称：

  ```
  ls -l Rhel_7.raw-0.raw
  ```
  {: pre}

  您将看到类似于以下内容的输出：

  ```
  -rw-r--r--. 1 user user 42949017600 Mar 5 10:02 Rhel_7.raw-0.raw
  ```
  {: screen}

  其中，_42949017600_ 是 RAW 映像使用的字节数
      1. 将 LUKS 头大小 4 MB（转换为字节数）与 RAW 映像文件大小相加。例如，42949017600 + (4 x 1024 x 1024) = 42953211904 字节。
      2. 将 RAW 映像大小与 LUKS 头的总和转换为兆字节，并四舍五入为最近的整数兆字节。例如，42953211904 字节 / 1024 字节 / 1024 千字节 = 40963.375 兆字节 = **40964 MG**。

5. 创建具有正确字节数的新 RAW 文件，此文件将成为加密的 RAW 映像。使用以下 _dd_ 命令来创建 RAW 文件：

  ```
  dd if=/dev/zero of=<ENCRYPTED_RAW_FILENAME> bs=1M count=<TARGET_SIZE_IN_MEGABYTE> status=progress
  ```
  {: pre}

  其中，_ENCRYPTED_RAW_FILENAME_ 是将成为加密 RAW 映像的文件，_TARGET_SIZE_IN_MEGABYTE_ 是在步骤 4 中获得的数字，等于未加密的 RAW 映像加上 LUKS 头的大小。

  例如，如果希望新的 RAW 文件成为加密映像 _Rhel_7.encrypted.raw_，并且其目标大小为 _40964_ MB，请运行以下命令：

  ```
  $ dd if=/dev/zero of=Rhel_7.encrypted.raw bs=1M count=40964 status=progress
  ```
  {: screen}

6. 使用 _dmcrypt_ 和数据加密密钥（步骤 3 中）通过 LUKS 磁盘加密来设置 RAW 文件的格式。此步骤将准备文件以包含映像。要使用 LUKS 加密来设置文件格式，请运行以下 _cryptsetup_ 命令：

  ```
  cryptsetup -v luksFormat <ENCRYPTED_RAW_FILENAME> <DEK_FILENAME>
  ```
  {: pre}

  其中，_ENCRYPTED_RAW_FILENAME_ 是要加密的 RAW 文件的文件名，_DEK_FILENAME_ 是存储数据加密密钥的文件的名称。

  例如，如果 RAW 文件为 _Rhel_7.encrypted.raw_，并且密钥文件为 _secret.dek_，请输入：

  ```
  $ cryptsetup -v luksFormat Rhel_7.encrypted.raw secret.dek
  ```
  {: screen}

  运行 cryptsetup 命令后，您将收到以下提示。此提示是预期行为，您必须使用 YES 来响应才能继续。

  ```
  WARNING!
  ========
  This will overwrite data on Rhel_7.encrypted.raw irrevocably.
  Are you sure (Type uppercase yes): YES
  ```
  {: screen}

7. 将加密的 RAW 文件作为卷进行连接，以将新的块设备与操作系统相关联。使用 _cryptsetup_，运行以下命令以使用 luksOpen 选项打开加密的 RAW 设备。

  您必须具有 sudo 特权，才能使用 `root` 用户权限来运行以下命令。
  {: tip}

  ```
  sudo cryptsetup -v luksOpen <ENCRYPTED_RAW_FILENAME> <VOLUME_NAME> --key-file <DEK_FILENAME>
  ```
  {: pre}

  其中，_ENCRYPTED_RAW_FILENAME_ 是加密的 RAW 文件的文件名，_VOLUME_NAME_ 是 _dmcrypt_ 将尝试创建的设备的名称，_DEK_FILENAME_ 是数据加密密钥的文件名。

  例如，如果加密的 RAW 文件为 _Rhel_7.encrypted.raw_，那么要将块设备命名为 _encryptedVolume_，并且数据加密密钥文件为 _secret.dek_，请使用以下命令：

  ```
  $ sudo cryptsetup -v luksOpen Rhel_7.encrypted.raw encryptedVolume --key-file secret.dek
  ```
  {: screen}

  此步骤完成后，将创建新的块设备，您可以在 _/dev/mapper/_ 下对其进行读取和写入。

8. 通过运行以下命令，确认是否已成功创建 LUKS 卷映射器：

  ```
  ls -l /dev/mapper/
  ```
  {: pre}

  您应该会看到类似于以下示例的输出：

  ```
  total 0
  lrw-rw---- 1 root root        7 Mar 5 22:22 encryptedVolume -> ../dm-0
  ```
  {: screen}

9. 将未加密的映像（步骤 2 中）复制到加密卷设备（步骤 7 中）。运行以下 _dd_ 命令以将未加密的映像复制到加密卷：

  ```
  sudo dd if=<RAW_IMAGE_FILE> of=/dev/mapper/<VOLUME_NAME>
  ```
  {: pre}

  其中，_RAW_IMAGE_FILE_ 是未加密 RAW 映像（在步骤 2 中创建）的文件名，_VOLUME_NAME_ 是加密卷块设备（在步骤 7 中创建）的名称。

  例如，如果 RAW_IMAGE_FILE 名为 _Rhel_7.raw-0.raw_，并且 VOLUME_NAME 为 _encryptedVolume_，请运行以下命令：

  ```
  $ sudo dd if=Rhel_7.raw-0.raw of=/dev/mapper/encryptedVolume status=progress
  ```
  {: screen}

  此命令可能需要 30 分钟或更长时间才能完成。
  {: tip}

10. 销毁卷映射器，并关闭与加密数据文件的 LUKS 连接：

  ```
  sudo cryptsetup luksClose encryptedVolume
  ```
  {: pre}

  其中，_encryptedVolume_ 是加密卷块设备的名称。

  现在，_ENCRYPTED_RAW_FILENAME_ 已初始化，您可以将其上传到 IBM Cloud Object Storage。例如，如果加密的 RAW 文件为 _Rhel_7.encrypted.raw_，请将该映像上传到 IBM Cloud Object Storage。
