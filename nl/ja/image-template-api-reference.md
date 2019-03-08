---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-23"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# イメージ・テンプレート API のリファレンス
{: #api-reference}

{{site.data.keyword.slapi_full}} は、開発者やシステム管理者が {{site.data.keyword.cloud}} のバックエンド・システムと直接対話できる開発用インターフェースです。{{site.data.keyword.slportal_full}}の多くの機能は、{{site.data.keyword.slapi_short}} によって駆動しています。つまり、一般には、{{site.data.keyword.slportal}}で行える対話は、API でも行うことができます。API だけでプログラムから {{site.data.keyword.BluSoftlayer_full}} 環境のあらゆる部分と対話できるので、{{site.data.keyword.slapi_short}} は作業の自動化を可能にします。例えば、*SoftLayer_Virtual_Guest/createObject* API を使用すれば、イメージ・テンプレートから仮想サーバー・インスタンスをデプロイできます。
{:shortdesc}

{{site.data.keyword.slapi_short}} は、リモート・プロシージャー・コールのシステムです。呼び出しごとに、API エンドポイントにデータを送信し、応答として返された構造化データを受信します。{{site.data.keyword.slapi_short}} とのデータの送受信に使用する形式は、選択した API の実装仕様によって異なります。{{site.data.keyword.slapi_short}} は現在、データ伝送に SOAP、XML-RPC、または REST を使用しています。

{{site.data.keyword.slapi_short}} と仮想サーバー API について詳しくは、{{site.data.keyword.sldn_full}} の以下の資料を参照してください。
* [{{site.data.keyword.slapi_short}} の概要 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/reference/softlayerapi/){: new_window}
* [{{site.data.keyword.slapi_short}} 入門![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/article/getting-started/){: new_window}
* [API リファレンス: SoftLayer_Virtual_Guest::createObject ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest/createObject/){: new_window}
* [API リファレンス: SoftLayer_Account::getBlockDeviceTemplateGroups ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/reference/services/SoftLayer_Account/getBlockDeviceTemplateGroups/){: new_window}
* [API リファレンス: SoftLayer_Virtual_Guest_Block_Device_Template_Group::getPublicImages ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getPublicImages/){: new_window}

API の使用例については、以下の資料を参照してください。
* [{{site.data.keyword.slapi_short}}を使用してイメージ・テンプレートから仮想サーバーを作成する方法 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://stackoverflow.com/questions/41138874/how-to-create-virtual-server-using-standard-template-softlayer-using-rest-api){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: Working with Virtual Servers ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://softlayer-python.readthedocs.io/en/latest/cli/vs.html){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: SoftLayer.image ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/image/){: new_window}
