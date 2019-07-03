---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# 将映像导出到 IBM Cloud Object Storage
{: #exporting-an-image-to-ibm-cloud-object-storage}

在“映像模板”页面中，可以将映像模板导出到 [{{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about) 帐户。
{:shortdesc}

映像导出过程采用预先存在的专用标准映像模板或加密映像模板，并将映像转换为存储在 {{site.data.keyword.cos_full_notm}} 帐户上指定位置中的映像文件。

*注：*如果导入了 VMDK 映像，那么可将此映像导出为 VHD 或 VMDK 格式。由于映像格式差异，可能会丢失数据。为了保护数据以防止数据丢失，将保留原始 VHD 文件。

## 开始之前
首先，导航至设备菜单，并确保您有正确的帐户许可权来完成任务。

* 导航至控制台的设备菜单。有关更多信息，请参阅[导航至设备](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)。
* 确保您有 {{site.data.keyword.cos_full_notm}} 的写访问权。仅帐户所有者（或具有**管理用户**经典基础架构许可权的用户）可以调整许可权。

有关许可权的更多信息，请参阅[经典基础架构许可权](/docs/iam?topic=iam-infrapermission#infrapermission)和[管理设备访问权](/docs/vsi?topic=virtual-servers-managing-device-access)。

如果计划将此映像模板导出到 {{site.data.keyword.cos_full_notm}}，请确保其名称不包含可能使 Web 地址出现问题的任何字符。例如，如果未采用 URL 编码，?、=、< 和其他特殊字符可能导致出现不需要的行为。
{: tip}

## 将映像导出到 IBM Cloud Object Storage

使用以下步骤将映像模板导出到 IBM Cloud Object Storage。

1. 从**设备**菜单中，选择**管理 > 映像**。
2. 对于要导出的映像模板，单击**操作**，然后选择**将映像导出到 IBM COS**。如果具有所需配置的映像模板尚不可用，请参阅[创建映像模板](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template)。
3. 填写必填字段（请参阅表 1）。
4. 单击**确定**将映像导出到 {{site.data.keyword.cos_full_notm}} 帐户中的指定位置。

|字段|值|
| ----- | ----- |
|文件名|输入映像的文件名。|
| {{site.data.keyword.cos_full_notm}} |选择 {{site.data.keyword.cos_full_notm}} 帐户。|
|位置|选择要存储映像模板的特定地理区域。|
|存储区|选择要存储映像模板的 {{site.data.keyword.cos_full_notm}} 存储区。只有在所选区域位置中存在的存储区才有效。如果指定的存储区在所选位置中不存在，那么您将收到错误消息。|
|API 密钥|指定对 {{site.data.keyword.cos_full_notm}} 具有写访问权的服务标识 API 密钥。有关更多信息，请参阅[管理服务标识 API 密钥](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys)。|
{: caption="表 1. 用于将映像导出到 {{site.data.keyword.cos_full_notm}} 的值" caption-side="top"}
| 目标文件类型 | 选择要导出的文件类型。如果导入了 VMDK 映像，那么可选择将此映像导出为 VHD 或 VMDK 格式。如果此文件最初为 VHD 映像，那么仅可将其导出为 VHD。|

## 后续步骤
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
由于每个映像大小不同且具有不同的特征，因此导出过程可能需要几分钟时间才能完成。

导出映像后，映像会保留在映像模板列表中，但也可以在导出过程中指定的 {{site.data.keyword.cos_full_notm}} 位置中作为文件使用。您可以从 {{site.data.keyword.cos_full_notm}} 下载映像文件。从 {{site.data.keyword.cloud_notm}} 控制台的“资源列表”中，访问您的 Cloud Object Storage 服务实例。在存储映像的存储区中，选择您要下载的映像文件，然后选择**下载对象**。

如果将映像模板导出到 IBM Cloud Object Storage，那么每个块设备（或磁盘）都会有其自己的关联文件。例如，如果映像文件命名为 image.vhd，那么第一个块设备将导出为 image-0.vhd。第二个块设备导出为 image-1.vhd，依此类推。
{: tip}
