---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-27"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Exportation d'une image

La page Modèles d'image vous permet d'exporter un modèle d'image vers un compte Object Storage.
{:shortdesc}

Le processus d'exportation d'image prend un modèle d'image standard privé préexistant et convertit l'image en fichier image stocké à un emplacement spécifique sur un compte Object Storage. Procédez comme suit pour exporter un modèle d'image. 

1. Dans le menu **Unités** du [portail client ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/), sélectionnez **Gérer > Images**.
2. Cliquez sur **Actions** pour le modèle d'image à exporter et sélectionnez **Exporter image**.Si un modèle d'image avec la configuration souhaitée n'est pas encore disponible, voir [Création d'une image standard](create-standard-image.html).
3. Sur la page Exporter image, saisissez le nom de fichier de l'image dans la zone **Nom de fichier**.
5. Dans la liste déroulante **Compte**, sélectionnez un **Compte de stockage d'objets**.
6. Dans la liste déroulante **Cluster**, sélectionnez un **Cluster de stockage d'objets**.
7. Dans la liste déroulante **Conteneur**, sélectionnez un **Conteneur de stockage d'objets**.
8. Cliquez sur **Exporter image** pour exporter l'image vers l'emplacement indiqué dans le compte de stockage d'objets. Cliquez sur **Fermer** pour annuler l'action.

## Etapes suivantes

Après l'exportation d'une image, cette dernière reste dans la liste des modèles d'image, mais est également disponible sous forme de fichier dans l'emplacement Object Storage indiqué lors du processus d'exportation. Pour plus d'informations sur l'affichage d'un fichier exporté vers un compte de stockage d'objets,
voir [Affichage et édition des détails d'un fichier de stockage
d'objets](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html). Chaque image ayant une taille et des caractéristiques différentes, le processus d'exportation risque de prendre plusieurs minutes. La vitesse d'exportation
moyenne est de 2 Go/minute. Si l'image n'est toujours pas disponible sur le compte de stockage d'objets au bout de plusieurs minutes, [prenez contact avec le support](/docs/get-support/howtogetsupport.html).

