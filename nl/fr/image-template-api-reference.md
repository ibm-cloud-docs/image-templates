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

# Référence d'API de modèle d'image
{: #api-reference}

L'API {{site.data.keyword.slapi_full}} constitue l'interface de développement permettant aux développeurs et aux administrateurs système de gérer l'interaction avec le système dorsal d'{{site.data.keyword.cloud}}. Un grand nombre de fonctions du portail {{site.data.keyword.slportal_full}} sont également disponibles dans l'API {{site.data.keyword.slapi_short}}. Autrement dit, si une interaction est possible dans le portail {{site.data.keyword.slportal}}, elle peut également être exécutée dans l'API. Etant donné que vous pouvez interagir à l'aide d'un programme avec l'ensemble de l'environnement {{site.data.keyword.BluSoftlayer_full}} dans l'API, l'API {{site.data.keyword.slapi_short}} vous permet d'automatiser les tâches. Par exemple, vous pouvez utiliser
l'API *SoftLayer_Virtual_Guest/createObject* pour déployer une instance de serveur virtuel à partir d'un modèle d'image.
{:shortdesc}

L'API {{site.data.keyword.slapi_short}} est un système d'appel de procédure distante (RPC). Chaque appel implique l'envoi de données vers un noeud final d'API et la
réception de données structurées en retour. Le format utilisé pour l'envoi et la réception de données avec l'API {{site.data.keyword.slapi_short}} dépend de l'implémentation choisie pour l'API . L'API {{site.data.keyword.slapi_short}} utilise actuellement SOAP, XML-RPC ou REST pour la transmission des données.

Pour plus d'informations sur l'API {{site.data.keyword.slapi_short}} et les API de serveur virtuel, consultez les ressources suivantes dans {{site.data.keyword.sldn_full}} :
* [{{site.data.keyword.slapi_short}} Overview ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer.github.io/reference/softlayerapi/){: new_window}
* [Getting Started with the {{site.data.keyword.slapi_short}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer.github.io/article/getting-started/){: new_window}
* [API Reference: SoftLayer_Virtual_Guest::createObject ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest/createObject/){: new_window}
* [API Reference: SoftLayer_Account::getBlockDeviceTemplateGroups ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer.github.io/reference/services/SoftLayer_Account/getBlockDeviceTemplateGroups/){: new_window}
* [API Reference: SoftLayer_Virtual_Guest_Block_Device_Template_Group::getPublicImages ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getPublicImages/){: new_window}

Pour des exemples d'utilisation d'API, voir les ressources suivantes :
* [How to create a virtual server from an image template with the {{site.data.keyword.slapi_short}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://stackoverflow.com/questions/41138874/how-to-create-virtual-server-using-standard-template-softlayer-using-rest-api){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: Working with Virtual Servers ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://softlayer-python.readthedocs.io/en/latest/cli/vs.html){: new_window}
* [{{site.data.keyword.slapi_short}} Python Client: SoftLayer.image ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/image/){: new_window}
