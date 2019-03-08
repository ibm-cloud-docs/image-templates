---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-30"

subcollection: image-templates

---


{:new_window: target="_blank"}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}

# Foire aux questions : Modèles d'image

## Qu'est-ce qu'un modèle d'image standard ?
{: faq}

Un modèle d'image standard est l'option d'imagerie {{site.data.keyword.virtualmachineslong}} pour {{site.data.keyword.BluSoftlayer_notm}}.
Il permet aux utilisateurs de prendre une image d'un serveur virtuel existant quel que soit son système d'exploitation et de l'utiliser pour créer un nouveau serveur virtuel.

## Qu'est-ce qu'un modèle ISO ?
{: faq}

Les modèles ISO sont un type de modèle spécifiquement réservé aux images ISO pouvant être utilisées pour initialiser une VSI. Il en existe deux versions : les modèles publics et les modèles privés. Les modèles ISO publics sont des modèles préconfigurés qui sont fournis par {{site.data.keyword.BluSoftlayer_notm}} et peuvent être utilisés par n'importe quel client. Les modèles ISO privés sont créés en important une image ISO stockée sur un compte {{site.data.keyword.objectstorageshort}}. Pour pouvoir être importée dans l'écran Modèles d'image, une image ISO doit permettre l'initialisation.

Seuls les systèmes d'exploitation pris en charge par {{site.data.keyword.BluSoftlayer_notm}} peuvent être utilisés pour charger un modèle ISO sur une VSI. Pour plus d'informations, voir [Systèmes d'exploitation pris en charge ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.softlayer.com/services/software/).
{:tip}

## Quelle est la différence entre une image publique et une image privée ?
{: faq}

Une image publique est une image qui peut être affichée et appliquée à un nouveau serveur virtuel par n'importe quel utilisateur {{site.data.keyword.BluSoftlayer_notm}}. {{site.data.keyword.BluSoftlayer_notm}} en crée actuellement comme solution pour les options de configuration sur différentes unités. Les clients peuvent également rendre des images publiques et les mettre à la disposition de tous les utilisateurs. Une image privée ne peut être affichée que par les utilisateurs autorisés, qui sont par défaut tous les utilisateurs de votre compte ; il est aussi possible de partager les images entre plusieurs comptes en modifiant les options de partage sur le {{site.data.keyword.slportal}}.

## Puis-je capturer et déployer des serveurs virtuels avec mon hyperviseur auto-géré ?
{: faq}

Seuls les serveurs mis à disposition par {{site.data.keyword.BluSoftlayer_notm}} peuvent être capturés et déployés. Les serveurs virtuels individuels que vous avez créés manuellement sur des unités personnelles ne peuvent pas être capturés, mis à disposition ou déployés.

## Puis-je partager mon modèle ISO privé avec d'autres clients ?
{: faq}

Les seuls modèles ISO qui sont publics sont ceux qui sont générés par {{site.data.keyword.BluSoftlayer_notm}}. Les modèles ISO privés sont spécifiques à un compte et ne peuvent pas être partagés entre clients via le {{site.data.keyword.slportal}}.

## Mon modèle ISO doit-il se trouver dans le même centre de données que mon serveur virtuel pour effectuer une initialisation à partir de l'image ?
{: faq}

Oui, les modèles ISO doivent être dans le même centre de données qu'un serveur virtuel pour s'initialiser à partir de l'image. Sinon, le modèle ISO
doit être copié dans le centre de données approprié. Dans ce cas, un avertissement s'affiche. Lorsqu'un modèle ISO est copié dans
un autre centre de données, des frais modiques sont facturés au compte de la même manière que pour la copie d'autres types de modèles d'image.

## Quelle est la différence entre les données physiques et la taille de volume ?
{: faq}

Le volume est l'espace disque disponible pour le stockage de fichiers, tandis que les données physiques sont constituées des fichiers stockés sur le disque.

## Qu'est-ce que la fonction d'importation/exportation d'image ?
{: faq}

La fonction d'importation/exportation d'image, qui se trouve sur la page Modèles d'image du {{site.data.keyword.slportal}}, permet de convertir les images VHD et ISO stockées dans un compte {{site.data.keyword.objectstorageshort}} en modèles d'image et vice versa. L'importation prend un fichier (VHD ou ISO) dans le conteneur d'un compte [{{site.data.keyword.objectstorageshort}}] et le convertit en modèle d'image. Le modèle d'image peut ensuite être utilisé pour initialiser ou charger une unité. Lorsque vous exportez une image, le modèle d'image est converti en un fichier (ou plusieurs fichiers si le modèle comporte plusieurs disques) qui est stocké à un emplacement spécifié dans le conteneur d'un compte {{site.data.keyword.objectstorageshort}}.

## Comment créer un modèle d'image pour l'intégralité de mon serveur et non uniquement pour mon unité principale ?
{: faq}

Pour créer un modèle d'image pour l'intégralité de votre serveur, consultez les instructions de la section [Création d'un modèle d'image](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).

Si vous choisissez d'exporter un modèle d'image vers IBM Cloud Object Storage, chaque unité par bloc (ou disque) a son propre fichier associé. Par exemple, si votre fichier image se nomme image.vhd, la première unité par bloc est exportée en tant qu'élément image-0.vhd. La deuxième unité par bloc est exportée en tant qu'élément image-1.vhd et ainsi de suite.
{: tip}
