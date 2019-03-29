---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Mise à disposition avec une image cloud-init
{: #provisioning-wiht-a-cloud-init-enabled-image}

Lorsque vous commandez un serveur virtuel, de nombreux systèmes d'exploitation utilisent désormais une image cloud-init pour optimiser le temps de mise à disposition. Vous pouvez également importer une image personnalisée que vous avez activée pour cloud-init.
{:shortdesc}

Les systèmes d'exploitation suivants utilisent maintenant par défaut une image de type cloud-init lorsque vous commandez un serveur virtuel sans modules complémentaires. (Les modules complémentaires incluent des logiciels supplémentaires, des scripts postérieurs à la mise à disposition et la surveillance avancée.)
* CentOS 7
* Debian 8, 9
* Ubuntu 16.04, 18.04
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

Lorsque vous commandez un serveur virtuel avec un système d'exploitation cloud-init, vous pouvez ajouter des données utilisateur ou des métadonnées avec des scripts de mise à disposition personnalisés. Dans la zone de données utilisateur du formulaire de commande, saisissez éventuellement des données utilisateur cloud-init ou des métadonnées pour le serveur.

## Importation d'une image cloud-init personnalisée

Si vous avez créé une image personnalisée de type cloud-init, vous pouvez la désigner en tant qu'image cloud-init sur la page Importer image du
portail {{site.data.keyword.slportal_full}}.

Pour accéder à la page Importer image des modèles d'image et marquer une image en tant que cloud-init, procédez comme suit :
1. Dans le menu **Unités**, sélectionnez **Gérer** > **Images**.
2. Cliquez sur l'onglet **Importer image**.
3. Complétez les informations requises pour l'importation de votre image cloud-init et cochez la case **Cloud init** qui s'affiche en regard de la liste déroulante **Système d'exploitation**. Pour plus d'informations sur l'importation d'images, voir Importation d'une image.

## Marquage d'un modèle d'image en tant que cloud-init

Si vous disposez d'un modèle d'image VHD existant activé pour cloud-init, vous pouvez le désigner en tant que cloud-init sur la page des détails du modèle d'image.

Pour accéder à un modèle d'image et le marquer en tant que cloud-init, procédez comme suit :
1. Dans le menu **Unités**, sélectionnez **Gérer** > **Images**.
2. Dans la liste des modèles, cliquez sur le nom du modèle d'image à mettre à jour.
3. Sur la page Détails du Modèle d'Image, cochez la case **Activé** sous l'en-tête **Cloud Init**, puis cliquez sur **Mise à jour**.

## Utilisation d'un modèle d'image créé à partir d'un serveur virtuel mis à disposition de type cloud-init

Cloud-init s'exécute en général une seule fois. Toutefois, si vous mettez à disposition un serveur virtuel à partir d'une image de type cloud-init et que vous créez ensuite
un modèle d'image à partir de ce serveur virtuel, l'identificateur unique universel est enregistré. Si ce modèle d'image est utilisé pour créer un autre
serveur virtuel, cloud-init s'exécute à nouveau.

## Création de modèles d'image de type cloud-init

Pour plus d'informations sur la configuration des images, consultez la [documentation cloud-init ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloudinit.readthedocs.io/en/latest/).

Pour plus d'informations sur les sources de données, voir [Sources de données ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html). Les images cloud-init {{site.data.keyword.cloud_notm}} sont créées pour l'environnement à l'aide de la source de données [Config Drive ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - Version 2 pour fournir les métadonnées.

### Configuration requise pour Linux
* Cloud-init version 0.7.7 ou suivante

### Configuration requise pour Windows
* Support Cloudbase-init Metadata Service pour réseau public et privé dans l'infrastructure {{site.data.keyword.cloud_notm}}. Le service met également à jour le portail client avec les informations d'identification du serveur virtuel Windows. Vous pouvez accéder au service sur le site
[https://github.com/softlayer/bluemix-cloudbase-init ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/softlayer/bluemix-cloudbase-init).
* Si vous utilisez Vyatta dans votre environnement, vous devez configurer Vyatta pour autoriser les appels d'API vers les équilibreurs de charge d'API. Pour plus d'informations, voir [Configuration de Brocade vRouter (Vyatta) pour les environnements VMware avec File Storage ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/infrastructure/FileStorage?topic=FileStorage-configureVyatta#setting-up-brocade-vrouter-vyatta-for-vmware-environments-with-file-storage).
