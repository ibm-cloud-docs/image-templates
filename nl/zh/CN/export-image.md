---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:deprecated: .deprecated}
{:new_window: target="_blank"}

# 将映像导出到 OpenStack Swift
{: #exporting-an-image-to-openstack-swift}

在“映像模板”页面中，可以将映像模板导出到 [Object Storage OpenStack Swift](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-GettingStarted#getting-started-with-object-storage-openstack-swift) 帐户。
{:shortdesc}

此服务的所有实例已不推荐使用。可以使用现有帐户，但在 2018 年 12 月 10 日后无法供应新的 {{site.data.keyword.objectstorageshort}} OpenStack Swift 帐户。从 2019 年 3 月 31 日开始，IBM Cloud 不再支持 Cloud Object Storage OpenStack Swift 的映像模板导入/导出功能。
{: deprecated}

映像导出过程采用预先存在的专用标准映像模板，并将映像转换为存储在 Object Storage OpenStack Swift 帐户上指定位置中的映像文件。使用以下步骤导出映像模板。

1. 从 [{{site.data.keyword.slportal_full}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/) 的**设备**菜单中，选择**管理 > 映像**。
2. 对于要导出的映像模板，单击**操作**，然后选择**导出映像**。如果具有所需配置的映像模板尚不可用，请参阅[创建映像模板](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)。
3. 在“导出映像”页面上的**文件名**字段中，输入映像的文件名。
5. 从**帐户**下拉列表中，选择**对象存储器帐户**。
6. 从**集群**下拉列表中，选择**对象存储器集群**。
7. 从**容器**下拉列表中，选择**对象存储器容器**。
8. 单击**导出映像**将映像导出到对象存储器帐户中的指定位置。单击**关闭**以取消该操作。

## 后续步骤

导出映像后，映像会保留在映像模板列表中，但也可以在导出过程中指定的 Object Storage OpenStack Swift 位置中作为文件使用。有关查看已导出到 Object Storage OpenStack Swift 帐户的文件的更多信息，请参阅[查看和编辑文件详细信息](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-OSSSLPortal#viewing-and-editing-file-details)。由于每个映像大小不同且具有不同的特征，因此导出过程可能需要几分钟时间才能完成。平均导出速度为 2 GB/分钟。如果数分钟后，映像在 Object Storage OpenStack Swift 帐户上仍然不可用，请[联系支持人员](/docs/get-support?topic=get-support-getting-customer-support)。
