---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-13"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# 使用端到端 (E2E) 加密来供应加密实例
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

通过“端到端 (E2E) 加密”功能，您能够自带启用了 cloud-init 的加密操作系统映像，该映像是使用您自己拥有并控制的数据加密密钥进行加密的。完成某些环境设置后，可以将加密映像导入到映像模板存储库，并将其用于供应加密虚拟服务器实例。E2E 加密为与供应的虚拟服务器实例关联的存储器提供静态数据加密。
{:shortdesc}

E2E 加密将多个 {{site.data.keyword.cloud}} 组件整合在一起，为关键信息提供安全的解决方案。

* IBM 密钥管理服务（如 {{site.data.keyword.keymanagementservicelong_notm}} 或 {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}）可保护加密密钥的安全（请参阅表 1）。
* 通过 IBM {{site.data.keyword.iamshort}} (IAM)，Cloud Block Storage 服务能够访问您的密钥管理系统以及用于包装数据加密密钥的根密钥。
* {{site.data.keyword.cos_full_notm}} 可安全地存储上传的加密映像。
* 在 {{site.data.keyword.cloud_notm}} 控制台中，可以导入加密映像并创建映像模板。
* 通过 {{site.data.keyword.cloud_notm}} 控制台基础架构环境中提供的加密映像模板，可以供应加密虚拟服务器实例。
* 最后，可以通过 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov) 来审计与加密虚拟服务器关联的事件。

## 加密密钥管理服务

{{site.data.keyword.keymanagementserviceshort}} 和 {{site.data.keyword.hscrypto}}（目前在某些[区域](/docs/services/hs-crypto?topic=hs-crypto-regions#regions)中可用）使用公用密钥提供者 API 来提供一致的加密密钥管理方法。{{site.data.keyword.cloud_notm}} 数据中心在后台提供专用硬件安全模块 (HSM) 来保护您的密钥。您可以从以下选项中选择： 

|密钥管理服务|HSM 加密证书|
| ----- | ----- |
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/concepts?topic=key-protect-getting-started-tutorial#getting-started-tutorial) |FIPS 140-2 *2 级*合规性|
| [{{site.data.keyword.hscrypto}}](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) |FIPS 140-2 *4 级*合规性|
{: caption="表 1. 可用的密钥管理服务选项" caption-side="top"}

## 准备环境

1. 您必须具有已升级的帐户才能对虚拟服务器使用 E2E 加密。有关更多信息，请参阅[切换到 IBM 标识和链接帐户](/docs/account/softlayerlink.html)。

2. 使用密钥管理服务来创建和管理密钥。以下示例步骤特定于 {{site.data.keyword.keymanagementserviceshort}}，但一般流程也适用于 {{site.data.keyword.hscrypto}}。如果您使用的是 {{site.data.keyword.hscrypto}}，请参阅该服务的[文档](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started)以获取相应的指示信息。
      1. 供应 [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision) 服务。
      2. 在 {{site.data.keyword.keymanagementservicelong_notm}}中[创建](/docs/services/key-protect?topic=key-protect-create-root-keys)或[导入](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys)根密钥 (CRK)。
      3. **可选**：如果选择，可以[创建](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys)或[导入](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys)用于解密的标准密钥。      
      4. [设置 {{site.data.keyword.cloud_notm}} Key Protect CLI 插件](/docs/services/key-protect?topic=key-protect-set-up-cli)，以便您可以[打包数据加密密钥](/docs/services/key-protect?topic=key-protect-cli-reference#kp-wrap)（您打算将此密钥与根密钥搭配使用来加密 VHD 映像）。将加密映像导入到 {{site.data.keyword.cloud_notm}} 控制台时，您需要使用与打包的数据加密密钥 (WDEK) 关联的密文。  
         
       Key Protect 不会保存附加认证数据 (AAD)，但您仍可以使用 AAD 进一步保护最多包含 255 个字符串的密钥，其中每个字符串都以逗号分隔并包含最多 255 个字符。如果提供 AAD 进行密钥打包，请将数据保存到安全位置，以确保您可以针对未来的密钥解包请求访问并提供相同的 AAD。
       {: tip}
      
3. 通过 IBM {{site.data.keyword.iamshort}} (IAM)，在您的 **Cloud Block Storage**（源服务）和您的**密钥管理服务**（目标服务）之间[授予访问权](/docs/iam?topic=iam-serviceauth#create-auth)。如果从 {{site.data.keyword.cos_full_notm}} 导入加密映像，您必须在 IAM 中为密钥管理服务[定义访问策略](/docs/iam?topic=iam-userroles#userroles)。
4. 在 IBM Cloud 控制台中，创建 {{site.data.keyword.cos_full_notm}} 实例并创建存储区以存储数据。有关更多信息，请参阅 [{{site.data.keyword.cos_full_notm}} 入门教程](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)
      1. 在供应密钥管理服务的区域中创建 {{site.data.keyword.cos_full_notm}} 实例。
      2. 创建存储区时，**弹性**设置必须为_区域_。
      3. （可选）创建存储区时，可以[使用密钥加密存储区](/docs/services/cloud-object-storage?topic=cloud-object-storage-encryption#encryption-kp)。   

## 准备加密映像

1. 选择要加密的在 {{site.data.keyword.cloud_notm}} 基础架构环境中工作的未加密映像。一个选项是使用现有虚拟服务器来[创建映像模板](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template)。有关更多信息，请参阅[使用从 cloud-init 供应的虚拟服务器创建的映像模板](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server)。您还可以使用现有 VHD 映像。确保映像满足[加密映像需求](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs)。
2. 如果要在 {{site.data.keyword.slportal}} 中使用映像模板，请[导出未加密映像](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage)至 {{site.data.keyword.cos_full_notm}}。
3. 将映像文件从 {{site.data.keyword.cos_full_notm}} 下载到安全的本地计算机以加密映像。在服务仪表板中，选择**下载**操作以从存储器中检索对象。可以使用 Aspera 高速传输插件来下载大于 200 MB 的映像。
4. 使用 vhd-util 工具[加密 VHD 映像](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image)。
5. 在 {{site.data.keyword.cos_full_notm}} 中，浏览至存储区，然后单击**添加对象**以[上传](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload)加密映像。可以使用 Aspera 高速传输插件来上传大于 200 MB 的映像。

## 导入加密映像并订购实例

1. 在将加密映像导入到 {{site.data.keyword.cloud_notm}} 控制台时，使用 IBM {{site.data.keyword.iamshort}} (IAM) 来创建要用于认证的服务标识。
      1. 创建[服务标识](/docs/iam?topic=iam-serviceids#serviceids)。
      2. 分配[访问策略](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy)。分配对以下服务的访问权：{{site.data.keyword.cos_full_notm}} 和密钥管理。
      3. [为服务标识创建 API 密钥](/docs/iam?topic=iam-serviceidapikeys#create_service_key)。
      4. 有关更多信息，请参阅 [Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/blog/introducing-ibm-cloud-iam-service-ids-api-keys){: new_window}。
2. 从 {{site.data.keyword.cloud_notm}} 控制台，在“映像模板”页面中[导入加密映像](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos)。
3. 在“映像模板”页面中，可以使用加密映像来[订购](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template)虚拟服务器实例。
4. 在供应加密虚拟服务器后，可以选择通过 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov) 来审计[虚拟服务器事件](/docs/vsi?topic=virtual-servers-at_events#at_events)。
