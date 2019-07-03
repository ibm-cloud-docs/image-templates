---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}


# 使用您自己的操作系统许可证或预订
{: #using-your-own-os-license-or-subscription}

使用 VHD 映像创建映像模板时，可以选择通过 [Red Hat Cloud
Access ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) 预订来提供您自己的 RHEL 操作系统许可证，或者通过 Microsoft Enterprise Agreement 提供 Windows 许可证。
{:shortdesc}

如果在 {{site.data.keyword.cloud}} 中部署的映像指示您使用自己的许可证，那么存在以下支持条款：
* {{site.data.keyword.IBM_notm}} 提供对系统管理程序、供应实例、导入映像、重新引导映像、重装操作系统和捕获映像的支持。
* 您向其购买操作系统许可证的公司提供对映像本身的支持。{{site.data.keyword.IBM_notm}} 不提供对映像的支持。

为映像提供您自己的许可证时，以下限制适用于该映像：
* 映像是专用映像。不能公开共享。
* 映像不能包含任何软件附加组件。供应映像后，必须添加其他软件。

## 使用 Red Hat Cloud Access
有关对我们的 Red Hat Enterprise Linux 云提供者身份进行认证的更多信息，请参阅 [Infrastructure as a Service (Iaas) ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://access.redhat.com/ecosystem/cloud-provider/2262101)。

## 使用您自己的 Windows 许可证
支持以下操作系统：
* Windows Server 2016
* Windows Server 2012
* Windows Server 2012 R2

如果您对现有的 Windows 许可证资格或对了解报告需求有疑问，请联系 Microsoft 代表。如果创建的映像模板指定您使用自己的 Windows 许可证，那么必须在专用主机上供应该映像。如果使用的映像指示您使用自己的 Windows 许可证，那么无法供应自动分配给主机的公共实例或专用实例。此外，如果创建或更新的 Windows 映像模板指定您使用自己的许可证，那么 Windows 映像不能是 cloud-init 映像。

## 开始之前
首先，导航至设备菜单，并确保您有正确的帐户许可权来完成任务。

* 导航至控制台的设备菜单。有关更多信息，请参阅[导航至设备](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)。
* 确保您有任何必要的帐户许可权和设备访问权。仅帐户所有者（或具有**管理用户**经典基础架构许可权的用户）可以调整许可权。

有关许可权的更多信息，请参阅[经典基础架构许可权](/docs/iam?topic=iam-infrapermission#infrapermission)和[管理设备访问权](/docs/vsi?topic=virtual-servers-managing-device-access)。

## 导入指定您自己许可证的映像

可以导入 VHD 映像，并指定您将为操作系统提供自己的许可证或预订。


要访问“映像模板”的“导入映像”页面，并将 VHD 映像标记为使用您自己的许可证或预订，请完成以下步骤：
1. 从**设备**菜单中，选择**管理 > 映像**。
2. 单击**导入映像**选项卡。
3. 填写导入 VHD 映像所需的信息，然后选中**操作系统**下拉框旁边显示的**您的许可证**复选框。有关导入映像的更多信息，请参阅[准备和导入映像](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images)。

## 更新映像模板以指定用户提供的操作系统许可证

如果您有现有的 VHD 映像模板，那么可以指定要为操作系统提供自己的许可证或预订。

要访问映像模板并指定它使用您自己的现有许可证或预订，请完成以下步骤：
1. 从**设备**菜单中，选择**管理 > 映像**。
2. 从模板列表中，单击要更新的映像模板名称。
3. 在“映像模板详细信息”页面上，选中**操作系统许可证**标题下的**用户提供**复选框，然后单击**更新**。
