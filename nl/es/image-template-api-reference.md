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

# Referencia de API de plantilla de imagen
{: #image-template-api-reference}

{{site.data.keyword.slapi_full}} es la interfaz de desarrollo que proporciona a los desarrolladores y a los administradores del sistema la interacción directa con el sistema de fondo de {{site.data.keyword.cloud}}. {{site.data.keyword.slapi_short}} alimenta muchas de las características del {{site.data.keyword.slportal_full}}, lo que normalmente significa que si una acción se puede ejecutar en el {{site.data.keyword.slportal}}, también se puede ejecutar en la API. Puesto que puede interactuar mediante programación con todas las partes del entorno de {{site.data.keyword.BluSoftlayer_full}} dentro de la API, {{site.data.keyword.slapi_short}} le permite automatizar las tareas. Por ejemplo, puede utilizar la API *SoftLayer_Virtual_Guest/createObject* para desplegar una instancia de servidor virtual a partir de una plantilla de imagen.
{:shortdesc}

La {{site.data.keyword.slapi_short}} es un sistema de llamada a procedimiento remoto. Cada llamada implica el envío de datos a un punto final de API y la recepción de datos estructurados. El formato utilizado para enviar y recibir datos con la {{site.data.keyword.slapi_short}} depende de la implementación de la API que se elija. Actualmente la {{site.data.keyword.slapi_short}} utiliza SOAP, XML-RPC o REST para la transmisión de datos.

Para obtener más información sobre la {{site.data.keyword.slapi_short}} y las API del servidor virtual, consulte los recursos siguientes en {{site.data.keyword.sldn_full}}:
* [Visión general de {{site.data.keyword.slapi_short}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://sldn.softlayer.com/reference/softlayerapi/){: new_window}
* [Iniciación a la {{site.data.keyword.slapi_short}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://sldn.softlayer.com/article/getting-started/){: new_window}
* [Referencia de API: SoftLayer_Virtual_Guest::createObject ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest/createObject/){: new_window}
* [Referencia de API: SoftLayer_Account::getBlockDeviceTemplateGroups ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://sldn.softlayer.com/reference/services/SoftLayer_Account/getBlockDeviceTemplateGroups/){: new_window}
* [Referencia de API: SoftLayer_Virtual_Guest_Block_Device_Template_Group::getPublicImages ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getPublicImages/){: new_window}

Para ver ejemplos de uso de la API, consulte los siguientes recursos:
* [Cómo crear un servidor virtual a partir de una plantilla de imagen con la {{site.data.keyword.slapi_short}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://stackoverflow.com/questions/41138874/how-to-create-virtual-server-using-standard-template-softlayer-using-rest-api){: new_window}
* [Cliente Python de la {{site.data.keyword.slapi_short}}: Trabajar con servidores virtuales ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/vs/){: new_window}
* [Cliente Python de la {{site.data.keyword.slapi_short}}: SoftLayer.image ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/image/){: new_window}
