---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}


# 创建映像模板
{: #creating-an-image-template}

通过映像模板，可以为{{site.data.keyword.virtualmachinesshort}}复制各种配置选项。
{:shortdesc}

在虚拟服务器的生命周期内，您随时可以创建映像模板。然后，可以使用该模板在其他虚拟服务器中快速复制其部分配置。您可以从任何虚拟服务器创建映像模板，而不考虑其操作系统。映像模板创建完成后，可以将其用于创建其他虚拟服务器。

## 开始之前
首先，导航至设备菜单，并确保您有正确的帐户许可权来完成任务。

* 导航至控制台的设备菜单。有关更多信息，请参阅[导航至设备](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)。
* 确保您有任何必要的帐户许可权和设备访问权。仅帐户所有者（或具有**管理用户**经典基础架构许可权的用户）可以调整许可权。

有关许可权的更多信息，请参阅[经典基础架构许可权](/docs/iam?topic=iam-infrapermission#infrapermission)和[管理设备访问权](/docs/vsi?topic=virtual-servers-managing-device-access)。

## 创建映像模板

要创建虚拟服务器的映像模板，请完成以下步骤。

1. 从**设备**菜单中，选择**设备列表**。
2. 单击要用于创建映像模板的虚拟服务器。

  检查**设备详细信息**页面的**密码**选项卡。确保**设备详细信息**页面上列出的任何密码都与实际操作系统密码和其他任何软件附加组件密码相匹配。如果密码不匹配，那么基于此映像模板创建的虚拟服务器会失败。
  {:tip}

3. 从**操作**菜单中，选择**创建映像模板**。
4. 在**映像名称**字段中，输入映像的新名称。
5. 在**注释**字段中，输入该映像的任何必要注释。
6. 输入了所有信息后，选中**同意**复选框。
7. 单击**创建模板**以创建映像模板。

## 后续步骤

创建映像模板后，可以使用该模板来创建更多的虚拟服务器。新的虚拟服务器具有的配置与映像模板中包含的配置相同。
