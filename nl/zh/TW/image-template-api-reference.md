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

# 映像檔範本 API 參考資料
{: #image-template-api-reference}

{{site.data.keyword.slapi_full}} 是讓開發人員及系統管理者直接與 {{site.data.keyword.cloud}} 後端系統互動的開發介面。{{site.data.keyword.slapi_short}} 提供 {{site.data.keyword.slportal_full}} 中的許多功能，一般表示，如果可以在 {{site.data.keyword.slportal}} 中進行互動，同樣也可以在 API 中執行。因為您可以在 API 內透過程式設計方式與 {{site.data.keyword.BluSoftlayer_full}} 環境的所有部分進行互動，所以 {{site.data.keyword.slapi_short}} 可讓您將作業自動化。例如，您可以使用 *SoftLayer_Virtual_Guest/createObject* API，從映像檔範本部署虛擬伺服器實例。
{:shortdesc}

{{site.data.keyword.slapi_short}} 是一種「遠端程序呼叫」系統。每一個呼叫需要向 API 端點傳送資料，並從其接收結構化資料。透過 {{site.data.keyword.slapi_short}} 傳送及接收資料時使用何種格式，視您選擇哪一個 API 實作而定。{{site.data.keyword.slapi_short}} 目前使用 SOAP、XML-RPC 或 REST 進行資料傳輸。

如需 {{site.data.keyword.slapi_short}} 及虛擬伺服器 API 的相關資訊，請參閱 {{site.data.keyword.sldn_full}} 中的下列資源：
* [{{site.data.keyword.slapi_short}} Overview ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer.github.io/reference/softlayerapi/){: new_window}
* [Getting Started with the {{site.data.keyword.slapi_short}} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer.github.io/article/getting-started/){: new_window}
* [API Reference: SoftLayer_Virtual_Guest::createObject ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest/createObject/){: new_window}
* [API Reference: SoftLayer_Account::getBlockDeviceTemplateGroups ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer.github.io/reference/services/SoftLayer_Account/getBlockDeviceTemplateGroups/){: new_window}
* [API Reference: SoftLayer_Virtual_Guest_Block_Device_Template_Group::getPublicImages ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getPublicImages/){: new_window}

如需 API 使用範例，請參閱下列資源：
* [How to create a virtual server from an image template with the {{site.data.keyword.slapi_short}} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/questions/41138874/how-to-create-virtual-server-using-standard-template-softlayer-using-rest-api){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: Working with Virtual Servers ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://softlayer-python.readthedocs.io/en/latest/cli/vs.html){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: SoftLayer.image ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/image/){: new_window}
