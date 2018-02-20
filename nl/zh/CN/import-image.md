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


# 准备和导入映像

[客户门户网站 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/) 中的“映像模板”屏幕支持用户从基于 Swift 的[对象存储器](/docs/infrastructure/objectstorage-swift/index.html)帐户上传现有映像。
{:shortdesc}

将映像作为映像模板导入后，可以使用这些映像来供应或启动现有的虚拟服务器。通过对象存储器帐户导入的映像可以是 VHD 或定制 ISO。VHD 导入仅限于以下 64 位操作系统：

* CentOS 6 和 7
* RedHat Enterprise Linux 6 和 7
* Ubuntu 14.04 和 16.04
* Microsoft Server Standard 2012、R2 2012 和 2016

VHD 导入限制为 100 GB 磁盘。VHD 必须按照以下示例命名：filename.vhd-0.vhd。

## 将映像转换为 VHD

VHD 格式是 {{site.data.keyword.BluVirtServers_full}} 唯一支持的映像格式。要将映像转换为 VHD，请使用以下信息：

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

仅 {{site.data.keyword.BluSoftlayer_notm}} 支持的操作系统可用于将 ISO 模板装入到 VSI 上。支持的操作系统的列表可以在此处找到：[http://www.softlayer.com/services/software/ ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.softlayer.com/services/software/)

使用此工具导入的 ISO 必须是可引导的，这样映像才有资格进行导入。

## 为{{site.data.keyword.virtualmachinesshort}}配置映像

要确保映像可以成功地部署在 {{site.data.keyword.BluSoftlayer_notm}} 基础架构环境中，必须将虚拟服务器映像配置为以下规范：

* ***/boot*** 必须是第一个分区
* ***/boot*** 和 ***/*** 必须是 ext3 或 ext4 文件系统
* ***/etc*** 和 /root 必须与 ***/*** 位于同一分区上
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** 用于装载连接到系统的交换磁盘
* 必须安装 wget
* 必须安装最新的 xe-guest-utilities Xen tools。完成以下步骤：
    
    1. 从 Citrix 下载 XenServer ISO：[https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html)
    
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
     
     5. 可以复制该 RPM，然后从该 ISO 中抽取 xentools。运行以下命令创建包含该 ISO 的目录结构：
    
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
    
有关启用了 cloud-init 的映像的更多信息，请参阅[使用启用了 cloud-init 的映像进行供应](image_cloud-init.html)。

## 导入映像

要在客户门户网站中导入映像，请完成以下步骤。

1. 在{{site.data.keyword.objectstorageshort}}帐户中找到并记录映像的以下详细信息。有关更多信息，请参阅[查看和编辑对象存储器文件详细信息](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html)。
  * 帐户名称
  * 集群
  * 容器
  * 映像文件名
2. 在[客户门户网站 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/) 中，通过选择**设备 > 管理 > 映像**来访问**映像模板**页面。
3. 单击**导入映像**选项卡以打开“导入”工具。
4. 从**帐户**下拉列表中，选择要导入的映像的**{{site.data.keyword.objectstorageshort}}帐户**。
5. 从**集群**下拉列表中，选择要导入的映像的**{{site.data.keyword.objectstorageshort}}集群**。
6. 从**容器**下拉列表中，选择要导入的映像的**{{site.data.keyword.objectstorageshort}}容器**。
7. 从**映像文件**下拉列表中，选择**映像文件名**，如{{site.data.keyword.objectstorageshort}}中所列。
8. 在**映像名称**字段中，输入新映像模板的映像名称。
9. 在**注释**文本框中，输入任何适用的注释。
10. 从**操作系统**下拉列表中，选择映像的操作系统。

  如果要导入的映像是定制 ISO，那么“操作系统”下拉列表处于禁用状态。仅当导入涉及 VHD 时，才需要执行此步骤。
  {:tip}

11. 如果要导入的映像启用了 cloud-init，请选中 **Cloud Init** 复选框。有关更多信息，请参阅[使用启用了 cloud-init 的映像进行供应](image_cloud-init.html)。        
12. 如果计划提供您自己的操作系统许可证，请选中**您的许可证**复选框。有关更多信息，请参阅[使用 Red Hat Cloud Access](use-red-hat-cloud-access.html)。
13. 单击**导入**将映像导入到“映像模板”屏幕。单击**取消**以取消该操作。

## 后续步骤

导入开始后，系统会使用指定的路径（“帐户”>“集群”>“容器”>“映像文件”）在{{site.data.keyword.objectstorageshort}}帐户中找到映像文件。映像文件会作为映像模板导入，然后可在“映像模板”页面上对其进行访问。导入完成后，映像可用于订购新设备或启动现有设备。此外，可以随时删除映像。映像导入时间根据文件大小而变化，但一般需要几分钟到 1 小时。

