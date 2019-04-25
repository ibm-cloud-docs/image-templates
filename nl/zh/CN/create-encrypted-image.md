---

copyright:
  years: 2019
lastupdated: "2019-03-27"

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


# 加密 VHD 映像 
{: #create-encrypted-image}

要使用 E2E 加密功能，必须先使用 vhd-util 工具加密 VHD 映像，再将其导入“映像模板”以供应加密的实例。支持两种级别的 AES 加密：AES 256 位和 AES 512 位。
{:shortdesc}

## 加密 VHD 映像需求
{: #encrypted-image-reqs}

加密 VHD 映像必须满足以下需求：

* VHD 格式。
* 与 {{site.data.keyword.cloud}} 控制台基础架构环境兼容。
* 使用[支持的操作系统](/docs/infrastructure/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images)供应。
* 已启用 Cloud-init。
* 使用 [vhd-util 工具](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool)进行了加密。

## 加密 VHD 映像
{: #vhd-util-tool}

执行这些步骤以创建加密 VHD 映像：

1. 选择运行 V7 或更高版本的 CentOS 系统，为 {{site.data.keyword.cloud_notm}} 加密虚拟磁盘映像（VHD 文件）。如果您无法访问安装了 CentOS 的物理硬件，那么可以使用公共或专用主机在 {{site.data.keyword.cloud_notm}} 中使用 CentOS 7 供应虚拟服务器实例。用于加密 VHD 文件的 CentOS 系统本身不需要加密。

2. 登录到 CentOS 系统并连接到客户 VPM，然后[转至 SoftLayer 下载站点 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://downloads.service.softlayer.com/citrix/xen/){: new_window}，并选择 vhd-util 工具 RPM 包文件：vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm   

   如果无法将 RPM 包文件直接下载到 CentOS 系统，请将此文件下载到您当前工作所在的工作站。然后，使用安全复制 (scp) 命令，将其上传到 CentOS 系统。如果使用的是 {{site.data.keyword.cloud_notm}} 中的虚拟服务器实例，请使用以下命令，通过系统的公共 IP 地址执行此上传操作。

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

3. 使用以下命令安装 RPM：

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. 确定加密和解密磁盘映像并将其写入密钥文件所需的 AES 加密密钥。此 AES 加密密钥是在[准备您的环境](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment)中使用 Key Protect 客户根密钥打包的相同数据加密密钥。写入密钥文件的密钥材料必须解包，并且不得进行编码。 

   因为 data_key 在密钥文件中不是 Base64 编码的，所以无法使用标准 ASCII 字符从命令行打印或查看密钥文件内容。
   {: tip}

   使用以下命令，通过 **AES 256 位**或 **AES 512 位**加密密钥创建密钥文件： 
   
   ```
   echo <data_key> | base64 -d - > <keyfile_name>
   ```
   {: pre} 

   示例命令：

   ```
   echo Nrfen98EpMxF2B+wdgLfagzrqvgUZfMK4vL2T0NsT20ihrsNC9pUUHtizF6218pze8RLCgQ6kwxuE58IWLzgDA== | base64 -d - > aes512.dek
   ```
   {: screen}

5. 使用以下命令，验证在先前步骤中创建的密钥文件：

   ```
   vhd-util key -C -k <keyfile_name>
   ```
   {: pre}

   示例命令及输出：

   ```
   vhd-util key -C -k aes512.dek
   vhd_util_read_key: using keyfile aes512.dek, Size (bytes) 64
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

   先前示例命令的输出的第一行指示名为 `aes512.dek` 的密钥文件包含一个 64 字节的密钥，而第二行上列出的数字是相应加密密钥的 SHA256 散列或安全性散列。包含 AES 256 位加密密钥的文件的输出将指示一个 32 字节的密钥。
   {: tip} 

6. 使用以下命令，创建 VHD 文件的加密副本。`target_vhd` 表示包含加密版本 `source_vhd` 的文件的名称。

   ```
   vhd-util copy -n <source_vhd> -N <target_vhd> -k <keyfile_name>
   ```
   {: pre}    

   示例命令：

   ```
   vhd-util copy -n debian8-ne.vhd -N debian8-aes512.vhd -k aes512.dek
   ```
   {: screen}

7. 使用以下命令验证 VHD 文件是否已加密。

   ```
   vhd-util key -p -n <vhd_filename>
   ```
   {: pre}

   示例命令及输出：

   ```
   vhd-util key -p -n debian8-aes512.vhd
   0000000000000000000000000000000000000000000000000000000000000000
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

如果 VHD 文件已加密，那么会在输出中看到两个散列值，如先前示例中所示。第一个散列全部为零。第二个散列为 SHA256 散列，由 AES 加密密钥用于加密和解密 VHD。确保 VHD 文件的 SHA256 散列与**步骤 5** 中显示的散列相同。

**步骤 7** 中的示例命令创建名为“debian8-aes512.vhd”的新的加密 VHD 文件。此文件使用来自名为“aes512.dek”的密钥文件的 AES 512 位加密密钥进行加密。用于加密的 SHA256 散列为 `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda`。
