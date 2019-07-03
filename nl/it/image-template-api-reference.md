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

# Riferimento API del template dell'immagine
{: #image-template-api-reference}

{{site.data.keyword.slapi_full}} è l'interfaccia di sviluppo che consente agli sviluppatori e agli amministratori di sistema di
interagire direttamente con il sistema di backend di {{site.data.keyword.cloud}}. {{site.data.keyword.slapi_short}} potenzia molte delle funzioni disponibili nel
{{site.data.keyword.slportal_full}}, in genere ciò significa che se un'interazione è possibile nel {{site.data.keyword.slportal}},
può essere eseguita anche nell'API. Poiché puoi interagire a livello di programmazione con tutte le parti dell'ambiente {{site.data.keyword.BluSoftlayer_full}}
all'interno dell'API, {{site.data.keyword.slapi_short}} ti consente di automatizzare le attività. Ad esempio, puoi utilizzare l'API
*SoftLayer_Virtual_Guest/createObject* per distribuire un'istanza del server virtuale da un template dell'immagine.
{:shortdesc}

{{site.data.keyword.slapi_short}} è un sistema di chiamata di procedura remota. Ogni chiamata prevede l'invio di dati verso un endpoint API e la successiva ricezione dei dati
strutturati. Il formato utilizzato per inviare e ricevere i dati con {{site.data.keyword.slapi_short}} dipende da quale
implementazione API scegli. {{site.data.keyword.slapi_short}} attualmente utilizza SOAP, XML-RPC o REST per la trasmissione dei dati.

Per ulteriori informazioni sulla {{site.data.keyword.slapi_short}} e sulle API del server virtuale, consulta le seguenti risorse in
{{site.data.keyword.sldn_full}}:
* [Panoramica di {{site.data.keyword.slapi_short}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://sldn.softlayer.com/reference/softlayerapi/){: new_window}
* [Introduzione a {{site.data.keyword.slapi_short}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://sldn.softlayer.com/article/getting-started/){: new_window}
* [Riferimento API: SoftLayer_Virtual_Guest::createObject ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest/createObject/){: new_window}
* [Riferimento API: SoftLayer_Account::getBlockDeviceTemplateGroups ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://sldn.softlayer.com/reference/services/SoftLayer_Account/getBlockDeviceTemplateGroups/){: new_window}
* [Riferimento API: SoftLayer_Virtual_Guest_Block_Device_Template_Group::getPublicImages ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getPublicImages/){: new_window}

Per gli esempi di utilizzo dell'API, consulta le seguenti risorse:
* [How to create a virtual server from an image template with the {{site.data.keyword.slapi_short}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://stackoverflow.com/questions/41138874/how-to-create-virtual-server-using-standard-template-softlayer-using-rest-api){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: Working with Virtual Servers ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/vs/){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: SoftLayer.image ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/image/){: new_window}
