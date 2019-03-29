---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-04"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}


# Utilisation de votre propre licence de système d'exploitation ou abonnement
{: #using-your-own-os-license-or-subscription}

Lorsque vous créez un modèle d'image avec une image VHD, vous pouvez choisir de fournir votre propre licence de système d'exploitation RHEL via l'abonnement à [Red Hat Cloud Access ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) ou une licence Windows via l'abonnement Accord Entreprise Microsoft.
{:shortdesc}

Si vous déployez une image dans {{site.data.keyword.BluSoftlayer_full}} qui indique que vous utilisez votre propre licence, les conditions de support suivantes s'appliquent :
* {{site.data.keyword.IBM_notm}} fournit du support pour les hyperviseurs, la mise à disposition d'instances, l'importation d'images, la réinitialisation d'une image, le rechargement d'un système d'exploitation et la capture d'une image.
* La société auprès de laquelle vous achetez la licence du système d'exploitation fournit du support pour l'image elle-même. {{site.data.keyword.IBM_notm}} ne fournit pas de support pour l'image.

Lorsque vous fournissez votre propre licence pour une image, les restrictions suivantes s'appliquent à l'image :
* L'image est privée. Elle ne peut pas être partagée publiquement.
* Aucun module complémentaire logiciel ne peut être inclus dans l'image. Des logiciels supplémentaires doivent être ajoutés après la mise à disposition de l'image.

## Utilisation de Red Hat Cloud Access
Pour plus d'informations sur notre certification en tant que fournisseur de cloud Red Hat Enterprise Linux, voir [Infrastructure sous forme de services ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://access.redhat.com/ecosystem/cloud-provider/2262101).

## Utilisation de votre propre licence Windows
Les systèmes d'exploitation suivants sont pris en charge :
* Windows Server 2016
* Windows Server 2012
* Windows Server 2012 R2

Si vous avez des questions concernant l'éligibilité de votre licence Windows existante, ou pour comprendre les exigences de génération de rapports, contactez votre interlocuteur Microsoft. Lorsque vous créez un modèle d'image qui indique que vous utilisez votre propre licence Windows, vous devez mettre à disposition cette image sur un hôte dédié. Vous ne pouvez pas mettre à disposition une instance publique ou une instance dédiée qui est automatiquement affectée à un hôte, lorsque vous utilisez une image qui indique que vous utilisez votre propre licence Windows. En outre, lorsque vous créez ou mettez à jour un modèle d'image Windows qui indique que vous utilisez votre propre licence, l'image Windows ne peut pas être de type cloud-init.

## Importation d'une image désignant votre propre licence

Vous pouvez importer une image VHD et indiquer que vous fournissez votre propre licence ou abonnement pour le système d'exploitation.

Pour accéder à la page Importer image des modèles d'image et marquer une image VHD pour qu'elle utilise votre propre licence ou abonnement, procédez comme suit :
1. Dans le menu **Unités**, sélectionnez **Gérer > Images**.
2. Cliquez sur l'onglet **Importer image**.
3. Complétez les informations requises pour l'importation de votre image VHD et cochez la case **Votre licence** qui s'affiche en regard de la liste déroulante **Système d'exploitation**. Pour plus d'informations sur l'importation d'images, voir [Préparation et importation d'images](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images).

## Mise à jour d'un modèle d'image pour spécifier une licence de système d'exploitation fournie par un utilisateur

Si vous disposez déjà d'un modèle d'image VHD, vous pouvez indiquer que vous voulez fournir votre propre licence ou abonnement pour le système d'exploitation.

Pour accéder à un modèle d'image et indiquer qu'il utilise votre propre licence ou abonnement existant, procédez comme suit :
1. Dans le menu **Unités**, sélectionnez **Gérer > Images**.
2. Dans la liste des modèles, cliquez sur le nom du modèle d'image à mettre à jour.
3. Sur la page Détails du Modèle d'Image, cochez la case **Fourni par l'utilisateur** sous l'en-tête **Licence de système d'exploitation**, puis cliquez sur **Mise à jour**.
