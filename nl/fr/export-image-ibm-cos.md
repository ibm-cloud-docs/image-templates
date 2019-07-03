---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# Exportation d'une image dans IBM Cloud Object Storage
{: #exporting-an-image-to-ibm-cloud-object-storage}

La page Modèles d'image permet d'exporter un modèle d'image vers un compte [{{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about).
{:shortdesc}

Le processus d'exportation d'image utilise un modèle d'image privé préexistant ou un modèle d'image chiffré et convertit l'image en un fichier image stocké à un emplacement défini dans un compte {{site.data.keyword.cos_full_notm}}. 

*Remarque :* si vous avez importé une image VMDK, vous pouvez l'exporter au format VHD ou VMDK. En raison des différences entre les formats d'image, il existe un risque de perte de données. Pour protéger vos données en cas de perte de données, le fichier VHD d'origine est conservé.

## Avant de commencer
Tout d'abord, accédez au menu Unité et assurez-vous de disposer des droits de compte appropriés pour exécuter les tâches. 

* Accédez au menu Unité de votre console. Pour plus d'informations, voir [Accès aux unités](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Vérifiez que vous disposez de l'accès en écriture sur {{site.data.keyword.cos_full_notm}}. Seul le propriétaire de compte ou un utilisateur disposant de droit d'infrastructure classique **Gérer les utilisateurs** peut modifier les droits. 

Pour plus d'informations sur les droits, voir [Droits d'infrastructure classique](/docs/iam?topic=iam-infrapermission#infrapermission) et [Gestion de l'accès aux unités](/docs/vsi?topic=virtual-servers-managing-device-access).

Si vous prévoyez d'exporter ce modèle d'image vers {{site.data.keyword.cos_full_notm}}, assurez-vous que son nom ne comporte pas de caractères pouvant poser problème dans une adresse Web. Par exemple, ?, =, <, et d'autres caractères spéciaux risquent de provoquer un comportement non souhaité s'il ne sont pas codés dans l'URL.
{: tip}

## Exportation d'une image dans IBM Cloud Object Storage

Procédez comme suit pour exporter un modèle d'image dans IBM Cloud Object Storage.

1. Dans le menu **Unités**, sélectionnez **Gérer > Images**.
2. Cliquez sur **Actions** pour le modèle d'image à exporter puis sélectionnez l'option d'exportation d'image vers IBM COS****. Si un modèle d'image avec la configuration souhaitée
n'est pas encore disponible, voir [Création d'un modèle d'image](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template).
3. Renseignez les zones requises (voir le tableau 1).
4. Cliquez sur **OK** pour exporter l'image à l'emplacement spécifié du compte {{site.data.keyword.cos_full_notm}}.

| Zone | Valeur |
| ----- | ----- |
| Nom de fichier | Entrez le nom de fichier de l'image. |
| {{site.data.keyword.cos_full_notm}} | Sélectionnez un compte {{site.data.keyword.cos_full_notm}}. |
| Emplacement | Sélectionnez la région géographique spécifique où vous souhaitez que le modèle d'image soit stocké. |
| Compartiment | Sélectionnez le compartiment {{site.data.keyword.cos_full_notm}} où vous souhaitez que le modèle d'image soit stocké. Seuls les compartiments qui existent à l'emplacement régional sélectionné sont valides. Un message d'erreur s'affiche si vous indiquez un compartiment qui n'existe pas à l'emplacement sélectionné. |
| Clé d'API | Indiquez la clé d'API d'ID de service disposant de droits d'accès à {{site.data.keyword.cos_full_notm}}. Pour plus d'informations, voir [Gestion des clés d'API d'ID de service](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys). |
{: caption="Tableau 1. Valeurs pour l'exportation d'une image vers {{site.data.keyword.cos_full_notm}}" caption-side="top"}
| Type du fichier cible | Sélectionnez le type de fichier à exporter. Si vous avez importé une image VMDK, vous pouvez l'exporter au format VHD ou VMDK. Si le fichier d'origine était au format VHD, vous ne pouvez l'exporter qu'au format VHD. |

## Etapes suivantes
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
Chaque image ayant une taille et des caractéristiques différentes, le processus d'exportation risque de prendre plusieurs minutes.

Après l'exportation d'une image, cette dernière reste dans la liste des modèles d'image, mais est également disponible sous forme de fichier à l'emplacement {{site.data.keyword.cos_full_notm}} spécifié lors du processus d'exportation. Vous pouvez télécharger le fichier image à partir d'{{site.data.keyword.cos_full_notm}}. Depuis la liste de ressources sur la console {{site.data.keyword.cloud_notm}}, accédez à votre instance de service Cloud Object Storage. Dans le compartiment où est stockée votre image, sélectionnez les fichiers image à télécharger, puis sélectionnez l'option de **téléchargement d'objets**.

Lorsque votre modèle d'image est exporté dans IBM Cloud Object Storage, chaque unité par bloc (ou disque) dispose de son propre fichier associé. Par exemple, si votre fichier image se nomme image.vhd, la première unité par bloc est exportée en tant qu'élément image-0.vhd. La deuxième unité par bloc est exportée en tant qu'élément image-1.vhd et ainsi de suite.
{: tip}
