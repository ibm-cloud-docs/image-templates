---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-23"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 映像模板 API 参考
{: #image-template-api-reference}

{{site.data.keyword.slapi_full}} 是供开发者和系统管理员与 {{site.data.keyword.cloud}} 后端系统进行直接交互的开发接口。{{site.data.keyword.slapi_short}} 支持 {{site.data.keyword.slportal_full}} 中的许多功能，这通常意味着如果某个交互可以在 {{site.data.keyword.slportal}} 中执行，那么也可以在此 API 中运行。由于在 API 中可以通过编程方式与 {{site.data.keyword.BluSoftlayer_full}} 环境的所有部分进行交互，所以 {{site.data.keyword.slapi_short}} 支持自动执行任务。例如，可以使用 *SoftLayer_Virtual_Guest/createObject* API 通过映像模板来部署虚拟服务器实例。
{:shortdesc}

{{site.data.keyword.slapi_short}} 是一种远程过程调用系统。每个调用都涉及向 API 端点发送数据以及接收所返回的结构化数据。通过 {{site.data.keyword.slapi_short}} 发送和接收数据时所使用的格式取决于您所选择的 API 实现。{{site.data.keyword.slapi_short}} 当前使用 SOAP、XML-RPC 或 REST 进行数据传输。

有关 {{site.data.keyword.slapi_short}} 和虚拟服务器 API 的更多信息，请参阅 {{site.data.keyword.sldn_full}} 中的以下资源：
* [{{site.data.keyword.slapi_short}} 概述 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/reference/softlayerapi/){: new_window}
* [{{site.data.keyword.slapi_short}} 入门 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/article/getting-started/){: new_window}
* [API 参考：SoftLayer_Virtual_Guest::createObject ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest/createObject/){: new_window}
* [API 参考：SoftLayer_Account::getBlockDeviceTemplateGroups ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/reference/services/SoftLayer_Account/getBlockDeviceTemplateGroups/){: new_window}
* [API 参考：SoftLayer_Virtual_Guest_Block_Device_Template_Group::getPublicImages ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getPublicImages/){: new_window}

有关 API 用法示例，请参阅以下资源：
* [How to create a virtual server from an image template with the {{site.data.keyword.slapi_short}} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://stackoverflow.com/questions/41138874/how-to-create-virtual-server-using-standard-template-softlayer-using-rest-api){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: Working with Virtual Servers ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/vs/){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: SoftLayer.image ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/image/){: new_window}
