---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-02"

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
{: #exporting-to-ibm-cos}

在“映像模板”页面中，可以将映像模板导出到 [IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage) 帐户。
{:shortdesc}

映像导出过程采用预先存在的专用标准映像模板或加密映像模板，并将映像转换为存储在 IBM Cloud Object Storage 帐户上指定位置中的映像文件。

使用以下步骤将映像模板导出到 IBM Cloud Object Storage。

1. 使用对 IBM Cloud Object Storage 具有写访问权的服务标识向 {{site.data.keyword.slportal}} 进行认证。
2. 从 [{{site.data.keyword.slportal_full}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/) 的**设备**菜单中，选择**管理 > 映像**。
3. 对于要导出的映像模板，单击**操作**，然后选择**将映像导出到 IBM COS**。如果具有所需配置的映像模板尚不可用，请参阅[创建映像模板](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)。
4. 填写必填字段（请参阅表 1）。
5. 单击**确定**将映像导出到 IBM Cloud Object Storage 帐户中的指定位置。

|字段|值|
| ----- | ----- |
|文件名|输入映像的文件名。|
| {{site.data.keyword.cos_full_notm}} |选择 {{site.data.keyword.cos_full_notm}} 帐户。|
|位置|选择要存储映像模板的特定地理区域。|
|存储区|选择要存储映像模板的 {{site.data.keyword.cos_full_notm}} 存储区。只有在所选区域位置中存在的存储区才有效。如果指定的存储区在所选位置中不存在，那么您将收到错误消息。|
|API 密钥|指定对 {{site.data.keyword.cos_full_notm}} 具有写访问权的服务标识 API 密钥。有关更多信息，请参阅[管理服务标识 API 密钥](/docs/iam?topic=iam-serviceidapikeys)。|
{: caption="表 1. 用于将映像导出到 IBM Cloud Object Storage 的值" caption-side="top"}

## 后续步骤
由于每个映像大小不同且具有不同的特征，因此导出过程可能需要几分钟时间才能完成。

导出映像后，映像会保留在映像模板列表中，但也可以在导出过程中指定的 IBM Cloud Object Storage 位置中作为文件使用。

如果将映像模板导出到 IBM Cloud Object Storage，那么每个块设备（或磁盘）都会有其自己的关联文件。例如，如果映像文件命名为 image.vhd，那么第一个块设备将导出为 image-0.vhd。第二个块设备导出为 image-1.vhd，依此类推。
{: tip}
