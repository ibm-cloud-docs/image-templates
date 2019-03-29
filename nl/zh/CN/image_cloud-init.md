---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# 使用启用了 cloud-init 的映像进行供应
{: #provisioning-wiht-a-cloud-init-enabled-image}

订购虚拟服务器时，许多操作系统现在都使用启用了 cloud-init 的映像来优化供应时间。您还可以导入启用了 cloud-init 的定制映像。
{:shortdesc}

订购不带附加组件的虚拟服务器时，以下操作系统现在缺省为启用了 cloud-init 的映像。（附加组件包括其他软件、供应后脚本和高级监视。）
* CentOS 7
* Debian 8 和 9
* Ubuntu 16.04 和 18.04
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

使用启用了 cloud-init 的操作系统来订购虚拟服务器时，可以使用定制供应脚本添加用户数据或元数据。在订购表单上的“用户数据”字段中，输入服务器的可选 cloud-init 用户数据或可选元数据。

## 导入启用了 cloud-init 的定制映像

如果已创建启用了 cloud-init 的定制映像，那么可以在 {{site.data.keyword.slportal_full}} 的“导入映像”页面上将其指定为 cloud-init 映像。

要访问“映像模板”的“导入映像”页面并将映像标记为启用了 cloud-init，请完成以下步骤：
1. 从**设备**菜单中，选择**管理** > **映像**。
2. 单击**导入映像**选项卡。
3. 填写导入启用了 cloud-init 的映像所需的信息，然后选中**操作系统**下拉框旁边显示的 **Cloud init** 复选框。有关导入映像的更多信息，请参阅“导入映像”。

## 将映像模板标记为启用了 cloud-init

如果您有启用了 cloud-init 的现有 VHD 映像模板，那么可以在映像模板的详细信息页面上将其指定为启用了 cloud-init。

要访问映像模板并将其标记为启用了 cloud-init，请完成以下步骤：
1. 从**设备**菜单中，选择**管理** > **映像**。
2. 从模板列表中，单击要更新的映像模板名称。
3. 在“映像模板详细信息”页面上，选中 **Cloud Init** 标题下的**已启用**复选框，然后单击**更新**。

## 使用从 cloud-init 供应的虚拟服务器创建的映像模板

Cloud-init 通常只运行一次。但是，如果基于启用了 cloud-init 的映像来供应虚拟服务器，随后从该虚拟服务器创建映像模板，那么会记录 UUID。如果使用该映像模板来创建其他虚拟服务器，cloud-init 会再次运行。

## 创建启用了 cloud-init 的映像模板

有关配置映像的信息，请参阅 [cloud-init 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloudinit.readthedocs.io/en/latest/)。

有关数据源的信息，请参阅 [Datasources ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html)。将使用 [Config Drive ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - V2 数据源来提供元数据，从而为环境创建 {{site.data.keyword.cloud_notm}} cloud-init 映像。

### Linux 需求
* Cloud-init V0.7.7 或更高版本

### Windows 需求
* Cloudbase-init Metadata Service，用于在 {{site.data.keyword.cloud_notm}} 基础架构中支持公用和专用网络。该服务还使用 Windows 虚拟服务器凭证来更新客户门户网站。您可以通过以下网址访问该服务：[https://github.com/softlayer/bluemix-cloudbase-init ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/softlayer/bluemix-cloudbase-init)。
* 如果在环境中使用的是 Vyatta，那么必须配置 Vyatta 以允许对 API 负载均衡器进行 API 调用。[针对使用 File Storage 的 VMware 环境的 Brocade vRouter (Vyatta) 设置指南 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标 ")](/docs/infrastructure/FileStorage?topic=FileStorage-configureVyatta#setting-up-brocade-vrouter-vyatta-for-vmware-environments-with-file-storage)。
