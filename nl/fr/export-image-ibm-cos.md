---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-02"

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
{: #exporting-to-ibm-cos}

Sur la page Modèle d'image, vous pouvez exporter un modèle d'image dans un compte [IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage).
{:shortdesc}

Le processus d'exportation d'image utilise un modèle d'image privé préexistant ou un modèle d'image chiffré et convertit l'image en
un fichier image stocké à un emplacement défini dans un compte IBM Cloud Object Storage.

Procédez comme suit pour exporter un modèle d'image dans IBM Cloud Object Storage.

1. Authentifiez-vous sur le portail {{site.data.keyword.slportal}} avec un ID de service ayant des droits en écriture dans IBM Cloud Object Storage.
2. Dans le menu **Unités** du portail [{{site.data.keyword.slportal_full}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/), sélectionnez **Gérer > Images**.
3. Cliquez sur **Actions** pour le modèle d'image à exporter puis sélectionnez l'option d'exportation d'image vers IBM COS****. Si un modèle d'image avec la configuration souhaitée n'est pas
encore disponible, voir [Création d'un modèle d'image](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).
4. Renseignez les zones requises (voir le tableau 1).
5. Cliquez sur **OK** pour exporter l'image à l'emplacement indiqué dans le compte IBM Cloud Object Storage.

| Zone | Valeur |
| ----- | ----- |
| Nom de fichier | Entrez le nom de fichier de l'image. |
| {{site.data.keyword.cos_full_notm}} | Sélectionnez un compte {{site.data.keyword.cos_full_notm}}. |
| Emplacement | Sélectionnez la région géographique spécifique où vous souhaitez que le modèle d'image soit stocké. |
| Compartiment | Sélectionnez le compartiment {{site.data.keyword.cos_full_notm}} où vous souhaitez que le modèle d'image soit stocké. Seuls les compartiments qui existent à l'emplacement régional sélectionné sont valides. Un message d'erreur s'affiche si vous indiquez un compartiment qui n'existe pas à l'emplacement sélectionné. |
| Clé d'API | Indiquez la clé d'API d'ID de service disposant de droits d'accès à {{site.data.keyword.cos_full_notm}}. Pour plus d'informations, voir [Gestion des clés d'API d'ID de service](/docs/iam?topic=iam-serviceidapikeys). |
{: caption="Tableau 1. Valeurs pour l'exportation d'une image dans IBM Cloud Object Storage" caption-side="top"}

## Etapes suivantes
Chaque image ayant une taille et des caractéristiques différentes, le processus d'exportation risque de prendre plusieurs minutes.

Après l'exportation d'une image, cette dernière reste dans la liste des modèles d'image, mais est également disponible sous forme de fichier à l'emplacement IBM Cloud Object Storage spécifié lors du processus d'exportation.

Lorsque votre modèle d'image est exporté dans IBM Cloud Object Storage, chaque unité par bloc (ou disque) dispose de son propre fichier associé. Par exemple, si votre fichier image se nomme image.vhd, la première unité par bloc est exportée en tant qu'élément image-0.vhd. La deuxième unité par bloc est exportée en tant qu'élément image-1.vhd et ainsi de suite.
{: tip}
