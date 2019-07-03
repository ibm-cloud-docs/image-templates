---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# A propos des modèles d'image
{: #about-image-templates}

Les modèles d'image standard fournissent une option d'image pour tous les serveurs {{site.data.keyword.virtualmachinesshort}}, quel que soit le système d'exploitation.
{:shortdesc}

Les modèles d'image standard vous permettent de capturer une image d'un serveur virtuel existant et d'en créer un nouveau en fonction de l'image capturée. Les modèles d'image standard ne sont pas compatibles avec les serveurs Bare Metal.

## Mode de fonctionnement des modèles d'image
Les deux principales actions des modèles d'image sont la création et le déploiement. Lorsque vous demandez la création d'une image, le système automatisé de {{site.data.keyword.cloud}} fait passer votre serveur hors ligne. Lorsque le serveur est hors ligne, une sauvegarde compressée de vos données est créée, les informations de configuration sont enregistrées et le modèle d'image est stocké dans le réseau SAN de {{site.data.keyword.cloud_notm}}. Lors du déploiement du modèle d'image, le système automatisé crée un nouveau serveur en fonction des données rassemblées à partir de l'image sélectionné. Le processus de déploiement effectue des réglages de volume, restaure les données copiées puis réalise des modifications de configuration finales (configurations réseau, par exemple) pour le nouvel hôte.

## Images privées

Les images privées sont des images créées par un utilisateur sur le compte, ou des images créées sur un autre compte et partagées. Par défaut, toutes les images créées sont privées.

## Images publiques

Les images publiques sont des serveurs virtuels préconfigurés qui sont publiés par {{site.data.keyword.cloud_notm}} ou rendus publics par des clients et mis à la disposition de tous les clients {{site.data.keyword.cloud_notm}}. Les serveurs virtuels qui sont mis à disposition
via des modèles d'image publique sont généralement configurés pour des performances optimales, avec une combinaison spécifique de caractéristiques.
