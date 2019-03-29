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

# API-Referenz für Imagevorlagen
{: #image-template-api-reference}

Die {{site.data.keyword.slapi_full}} ist die Entwicklungsschnittstelle, über die Entwickler und Systemadministratoren die Möglichkeit zur direkten Interaktion mit dem Back-End-System von {{site.data.keyword.cloud}} erhalten. Die {{site.data.keyword.slapi_short}} ist die treibende Kraft für viele Features im {{site.data.keyword.slportal_full}}, was in der Regel bedeutet, dass eine Interaktion, die im {{site.data.keyword.slportal}} möglich ist, auch in der API durchgeführt werden kann. Da Sie programmgesteuert mit allen Teilen der {{site.data.keyword.BluSoftlayer_full}}-Umgebung innerhalb der API interagieren können, ermöglicht die {{site.data.keyword.slapi_short}} die Automatisierung von Tasks. So können Sie zum Beispiel die API *SoftLayer_Virtual_Guest/createObject* verwenden, um eine virtuelle Serverinstanz (VSI) aus einer Imagevorlage bereitzustellen.
{:shortdesc}

{{site.data.keyword.slapi_short}} ist ein System für Remote Procedure Call. Bei jedem Aufruf werden Daten an einen API-Endpunkt gesendet und im Gegenzug strukturierte Daten empfangen. Welches Format zum Senden und Empfangen von Daten mit der {{site.data.keyword.slapi_short}} verwendet wird, richtet sich danach, welche Implementierung der API Sie auswählen. Die {{site.data.keyword.slapi_short}} verwendet derzeit SOAP, XML-RPC oder REST für die Datenübertragung.

Weitere Informationen zur {{site.data.keyword.slapi_short}} und den APIs für virtuelle Server enthalten die folgenden Ressourcen im {{site.data.keyword.sldn_full}}:
* [Übersicht über {{site.data.keyword.slapi_short}} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/reference/softlayerapi/){: new_window}
* [Einführung in {{site.data.keyword.slapi_short}} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/article/getting-started/){: new_window}
* [API-Referenz: SoftLayer_Virtual_Guest::createObject ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest/createObject/){: new_window}
* [API-Referenz: SoftLayer_Account::getBlockDeviceTemplateGroups ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/reference/services/SoftLayer_Account/getBlockDeviceTemplateGroups/){: new_window}
* [API-Referenz: SoftLayer_Virtual_Guest_Block_Device_Template_Group::getPublicImages ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getPublicImages/){: new_window}

Beispiele zur Verwendung der API finden Sie in den folgenden Ressourcen:
* [Vorgehensweise zum Erstellen eines virtuellen Servers aus einer Imagevorlage mit der {{site.data.keyword.slapi_short}} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://stackoverflow.com/questions/41138874/how-to-create-virtual-server-using-standard-template-softlayer-using-rest-api){: new_window}
* [{{site.data.keyword.slapi_short}} - Python-Client: Mit virtuellen Servern arbeiten ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://softlayer-python.readthedocs.io/en/latest/cli/vs.html){: new_window}
* [{{site.data.keyword.slapi_short}} - Python-Client: SoftLayer.image ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/image/){: new_window}
