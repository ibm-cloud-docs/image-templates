---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-19"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Exportation d'une image dans OpenStack Swift

La page Modèles d'image vous permet d'exporter un modèle d'image vers un compte [Object Storage OpenStack Swift](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-GettingStarted#getting-started-with-object-storage-openstack-swift).
{:shortdesc}

Le processus d'exportation d'image utilise un modèle d'image standard privé préexistant et convertit l'image en fichier
image stocké à un emplacement spécifié dans un compte Object Storage OpenStack Swift. Procédez comme suit pour exporter un modèle d'image.

1. Dans le menu **Unités** du portail [{{site.data.keyword.slportal_full}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/), sélectionnez **Gérer > Images**.
2. Cliquez sur **Actions** pour le modèle d'image à exporter et sélectionnez **Exporter image**. Si un modèle d'image avec la configuration souhaitée n'est pas
encore disponible, voir [Création d'un modèle d'image](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).
3. Sur la page Exporter image, saisissez le nom de fichier de l'image dans la zone **Nom de fichier**.
5. Dans la liste déroulante **Compte**, sélectionnez un **Compte de stockage d'objets**.
6. Dans la liste déroulante **Cluster**, sélectionnez un **Cluster de stockage d'objets**.
7. Dans la liste déroulante **Conteneur**, sélectionnez un **Conteneur de stockage d'objets**.
8. Cliquez sur **Exporter image** pour exporter l'image vers l'emplacement indiqué dans le compte de stockage d'objets. Cliquez sur **Fermer** pour annuler l'action.

## Etapes suivantes

Après l'exportation d'une image, cette dernière reste dans la liste des modèles d'image mais est également disponible en tant que fichier à l'emplacement Object Storage OpenStack Swift spécifié lors du processus d'exportation. Pour plus d'informations sur l'affichage d'un fichier
exporté vers un compte de stockage d'objet, voir la rubrique relative à l'[affichage et l'édition des détails de fichier](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-OSSSLPortal#viewing-and-editing-file-details). Chaque image ayant une taille et des caractéristiques différentes, le processus d'exportation risque de prendre plusieurs minutes. La vitesse d'exportation
moyenne est de 2 Go/minute. Si l'image n'est toujours pas disponible sur le compte
Object Storage OpenStack Swift, [prenez contact avec le support](/docs/get-support?topic=get-support-getting-customer-support).
