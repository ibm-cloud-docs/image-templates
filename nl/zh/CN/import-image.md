---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:pre: .pre}
{:screen: .screen}


# 准备和导入映像
{: #preparing-and-importing-images}

{{site.data.keyword.slportal_full}} 中的“映像模板”屏幕允许您从 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about) 服务实例导入映像。您可以导入虚拟硬盘 (VHD) 或虚拟机磁盘 (VMDK) 格式的映像。导入后，VMDK 映像将转换为 VHD。映像上传到 {{site.data.keyword.cos_full_notm}} 服务实例中的存储区后，可以将其导入到 {{site.data.keyword.slportal}} 中的映像模板存储库。
{:shortdesc}

您必须具有已升级的帐户才能从 {{site.data.keyword.cos_full_notm}} 导入映像。 有关更多信息，请参阅[切换到 IBM 标识和链接帐户](/docs/account/softlayerlink.html)。
{: tip}

必须通过 {{site.data.keyword.cloud_notm}} 控制台 (cloud.ibm.com) 订购了 [IBM Cloud Object Storage 实例](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision#provision-account)，才能使用此导入功能。从 control.softlayer.com 订购的 IBM Cloud Object Storage 不受支持。
{: important}

将映像作为映像模板导入后，可以使用这些映像来供应或启动现有的虚拟服务器。从 {{site.data.keyword.cos_full_notm}} 服务实例导入的映像可以是 VHD、VMDK 或定制 ISO。VHD 和 VMDK 导入仅限于以下 64 位操作系统：  

* CentOS 6 和 7
* Microsoft Server Standard 2012、R2 2012 和 2016
* RedHat Enterprise Linux 6 和 7
* Ubuntu 14.04 和 16.04

导入限制为 100 GB 磁盘。映像必须按照以下示例命名：filename.vhd-0.vhd 或 filename.vmdk-0.vmdk。

## 将映像转换为 VHD
{: #convert-to-vhd}

VHD 和 VMDK 格式是虚拟服务器唯一支持的映像格式。要将映像从非 VMDK 的任何格式转换为 VHD，请使用以下信息：

* 需要 Qemu-img 2.7.0 或更高版本
* 使用以下命令转换映像：

  ```
  qemu-img convert -f <image format> <image name> -O vpc -o force_size <image name>
  ```
  {: pre}

* 示例命令：

  ```
  qemu-img convert -f qcow2 test -O vpc -o force_size test
  ```
  {: pre}

有关更多信息，请参阅 QEMU 文档中的 [Converting image formats ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats)。

## ISO 模板
{: #iso-templates}

仅 {{site.data.keyword.BluSoftlayer_notm}} 支持的操作系统可用于将 ISO 模板装入到 VSI 上。支持的操作系统的列表可以在此处找到：[https://www.ibm.com/cloud/server-software ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/server-software)

使用此工具导入的 ISO 必须是可引导的，这样映像才有资格进行导入。

## 为{{site.data.keyword.virtualmachinesshort}}配置映像
{: #config-image-vsi}

要确保映像可以成功地部署在 {{site.data.keyword.BluSoftlayer_notm}} 基础架构环境中，必须将虚拟服务器映像配置为以下规范：

* ***/boot*** 必须是第一个分区
* ***/boot*** 和 ***/*** 必须是 ext3 或 ext4 文件系统
* ***/etc*** 和 /root 必须与 ***/*** 位于同一分区上
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** 用于装载连接到系统的交换磁盘
* 必须安装 wget
* 必须安装最新的 xe-guest-utilities Xen tools。完成以下步骤：

    1. 从 Citrix 下载 XenServer ISO：[https://www.citrix.com/downloads/citrix-hypervisor/ ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.citrix.com/downloads/citrix-hypervisor/)

    2. 通过运行以下命令来装载该 ISO：

        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}

    3. 通过运行以下命令来找到该 ISO 的 RPM：

        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}

    4. 输出会列出类似于以下内容的 RPM：

        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}

     5. 可以复制该 RPM，然后抽取 xentools 的 ISO。运行以下命令来创建包含该 ISO 的目录结构：

        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}

     6. 从生成的目录结构中，可以装载 *guest-tools* ISO，并找到 *rpm/debs* 来安装 xentools。请参阅以下示例目录结构：

        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}

有关启用了 cloud-init 的映像的更多信息，请参阅[使用启用了 cloud-init 的映像进行供应](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#provisioning-with-a-cloud-init-enabled-image)。

## 准备导入加密映像
{: #preparing-to-import-an-encrypted-image}

如果计划导入使用您自己的数据加密密钥加密的 VHD 映像，请确保完成[使用端到端 (E2E) 加密来供应加密实例](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance)中描述的加密先决条件和指示信息。

必须使用 vhd-util 工具来加密映像，映像必须为 VHD 格式。有关更多信息，请参阅[加密 VHD 映像](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image)。
{: important}

## 将映像上传到 {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

映像准备就绪后，可以将其上传到 {{site.data.keyword.cos_full_notm}}。确保使用[区域位置](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints)中的存储区。

1. 在 {{site.data.keyword.cos_full_notm}} 中，浏览至存储区，然后单击**添加对象**以[上传](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload)映像。
2. 使用 [Aspera](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera) 高速传输插件能够以最快速度上传映像。

使用 Java、Python 或 NodeJS 时，可以将 COS SDK 与 Aspera 配合使用，以在定制应用程序中启动高速传输。有关更多信息，请参阅[使用库和 SDK](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera#aspera-sdk)。
{: tip}


## 从 IBM Cloud Object Storage 导入映像
{: #import-icos}

要从 {{site.data.keyword.cos_full_notm}} 导入映像，请完成以下步骤。

1. 导航至设备菜单，并确保您有正确的帐户许可权来完成任务。

   * 导航至控制台的设备菜单。有关更多信息，请参阅[导航至设备](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)。
   * 确保您有任何必要的帐户许可权和设备访问权。仅帐户所有者（或具有**管理用户**经典基础架构许可权的用户）可以调整许可权。

   有关许可权的更多信息，请参阅[经典基础架构许可权](/docs/iam?topic=iam-    infrapermission#infrapermission)和[管理设备访问权](/docs/vsi?topic=virtual-servers-managing-device-access)。

   如果要导入加密映像，必须使用 {{site.data.keyword.cloud_notm}} 控制台。
   {: important}
2. 选择**设备 > 管理 > 映像**以访问**映像模板**页面。
3. 单击**从 IBM COS 导入映像**选项卡以打开“导入”工具。
4. 填写必填字段（请参阅表 1）。
5. 从 {{site.data.keyword.cos_full_notm}} 完成导入后，映像会显示在“映像模板”页面上。

|字段|值|
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} |选择存储要导入的映像的 {{site.data.keyword.cos_full_notm}} 服务实例。|
|位置|选择存储映像的特定地理区域。您可以将映像导入以下区域和关联的数据中心：美国南部 (DAL13)、美国东部 (WDC07)、欧洲-英国 (LON02)、欧洲-德国 (FRA02) 和亚太地区-日本。将映像导入到列出的其中一个数据中心之后，可以将其移至其他数据中心。|
|存储区|选择存储映像的 {{site.data.keyword.cos_full_notm}} 存储区。只有在所选区域位置中存在的存储区才有效。如果选择的存储区在所选位置中不存在，那么您将收到错误消息。|
|映像文件|选择 {{site.data.keyword.cos_full_notm}} 服务实例中要导入的映像文件。支持的文件类型为 VHD（虚拟硬盘）、VMDK（虚拟机磁盘）和 ISO。 如果要导入加密映像，那么该映像必须为 VHD 文件格式，并且使用 vhd-util 工具加密。|
|映像名称|指定映像的描述性名称。这是将用于供应虚拟服务器实例的映像。|
|API 密钥|指定用于授予对 {{site.data.keyword.cos_full_notm}} 服务实例的访问权的 API 密钥。导入加密映像时，API 密钥还必须有权访问您的密钥管理服务实例。API 密钥仅在创建时可用于复制或下载。如果 API 密钥丢失，必须创建新的 API 密钥。有关更多信息，请参阅[使用 API 密钥](/docs/iam?topic=iam-manapikey#manapikey)。|
|操作系统|选择映像中包含的操作系统。|
|Cloud-init|如果要导入的映像启用了 cloud-init，请选中此复选框。如果要导入的映像使用启用了 cloud-init 的 Windows 操作系统，并且您选择了此选项，那么无法同时选择**您的许可证**。如果要导入加密映像，那么缺省情况下此选项已选中且不可编辑，因为加密映像必须启用 cloud-init。|
|您的许可证|如果计划提供您自己的操作系统许可证，请选中此复选框。要导入包含 Windows 操作系统的映像时，如果计划使用该映像来供应[专用主机实例](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances)，那么可以选中此选项。如果 Windows 操作系统的版本不支持使用您自己的许可证，那么会禁用此选项。对于 Windows 映像，如果指定将使用自己的许可证，那么无法选中 Cloud Init。如果要导入将 Red Hat Enterprise Linux 用作操作系统的加密映像，那么缺省情况下此选项已选中且不可编辑，因为加密映像必须包含其自己的操作系统许可证。|
|引导方式|选择映像的引导方式。如果为指定的操作系统设置了缺省引导方式，那么在此处会自动选择该引导方式。|
|注释|添加可能对用户有用的与映像相关的任何注释。|
|加密|如果要导入通过 vhd-util 工具使用您自己的数据加密密钥加密的映像，请选中此复选框。|
{: caption="表 1. 用于从 IBM Cloud Object Storage 导入映像的值" caption-side="top"}

下表显示了仅适用于导入加密映像的其他字段。有关加密映像的更多信息，请参阅[使用端到端 (E2E) 加密来供应加密实例](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance)。

<!--
To import an encrypted image, your account must have access to the End to End (E2E) Encryption feature. To enable your account for E2E Encryption, please contact [Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
{: tip}
-->

|字段|值|
| ----- | ----- |
|包装的数据加密密钥|导入加密映像时，请指定与用于加密映像的数据加密密钥关联的密文。有关更多信息，请参阅[使用 API 包装密钥](/docs/services/key-protect?topic=key-protect-wrap-keys#api)。|
|密钥管理服务实例|从下拉列表选择您的帐户中的密钥管理服务实例。密钥管理服务实例必须包含用于打包数据加密密钥的客户根密钥。|
|密钥名称|选择密钥管理服务实例中用于打包数据加密密钥的根密钥的名称。有关更多信息，请参阅[查看密钥](/docs/services/key-protect?topic=key-protect-view-keys#view-keys)。|
{: caption="表 2. 用于从 IBM Cloud Object Storage 导入加密映像的值" caption-side="top"}

## 后续步骤

导入开始后，系统会在 {{site.data.keyword.cos_full_notm}} 服务实例的指定存储区中找到映像文件。映像文件会作为映像模板导入，然后可在“映像模板”页面上对其进行访问。导入完成后，映像可用于订购新设备或启动现有设备。此外，可以随时删除映像模板。映像导入时间根据文件大小而变化，但一般需要几分钟到 1 小时。

将映像导入映像模板存储库后，可以从 {{site.data.keyword.cos_full_notm}} 中删除该映像。您可以在**映像模板**页面中继续访问映像模板，并将其用于供应虚拟服务器实例。
