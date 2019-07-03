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

# Referência de API de modelo de imagem
{: #image-template-api-reference}

O {{site.data.keyword.slapi_full}} é a interface de desenvolvimento que fornece aos desenvolvedores e administradores do sistema a interação direta com o sistema backend do {{site.data.keyword.cloud}}. O {{site.data.keyword.slapi_short}} impulsiona muitos dos recursos no {{site.data.keyword.slportal_full}}, o que geralmente significa que, se uma interação é possível no {{site.data.keyword.slportal}}, ela também pode ser executada na API. Como você pode interagir programaticamente com todas as partes do ambiente do {{site.data.keyword.BluSoftlayer_full}} dentro da API, o {{site.data.keyword.slapi_short}} permite automatizar tarefas. Por exemplo, é possível usar
a API *SoftLayer_Virtual_Guest/createObject* para implementar uma instância de servidor virtual por meio de um modelo de imagem.
{:shortdesc}

O {{site.data.keyword.slapi_short}} é um sistema de Chamada de Procedimento Remoto. Cada chamada envolve enviar dados para um terminal de API e receber dados estruturados em retorno. O formato usado para enviar e receber dados com o {{site.data.keyword.slapi_short}} depende de qual implementação da API você escolher. O {{site.data.keyword.slapi_short}} usa atualmente SOAP, XML-RPC ou REST para transmissão de dados.

Para obter mais informações sobre o {{site.data.keyword.slapi_short}} e as APIs de servidor virtual, consulte os recursos a seguir no {{site.data.keyword.sldn_full}}:
* [{{site.data.keyword.slapi_short}} Visão geral ![Ícone de linkexterno](../icons/launch-glyph.svg "Ícone de link externo")](https://sldn.softlayer.com/reference/softlayerapi/){: new_window}

* [Introdução ao {{site.data.keyword.slapi_short}} ![Ícone de linkexterno](../icons/launch-glyph.svg "Ícone de link externo")](https://sldn.softlayer.com/article/getting-started/){: new_window}

* [Referência de API: SoftLayer_Virtual_Guest::createObject ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de linkexterno")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest/createObject/){: new_window}

* [Referência de API: SoftLayer_Account::getBlockDeviceTemplateGroups ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://sldn.softlayer.com/reference/services/SoftLayer_Account/getBlockDeviceTemplateGroups/){: new_window}
* [Referência de API: SoftLayer_Virtual_Guest_Block_Device_Template_Group::getPublicImages ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getPublicImages/){: new_window}

Para obter exemplos de uso da API, veja os recursos a seguir:
* [Como criar um servidor virtual por meio de um modelo de imagem com o {{site.data.keyword.slapi_short}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://stackoverflow.com/questions/41138874/how-to-create-virtual-server-using-standard-template-softlayer-using-rest-api){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: Trabalhando com Virtual Servers ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/vs/){: new_window}
* [Cliente {{site.data.keyword.slapi_short}} Python: SoftLayer.image ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/image/){: new_window}
