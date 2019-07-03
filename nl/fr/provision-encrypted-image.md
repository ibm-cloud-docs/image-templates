---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-13"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Utilisation du chiffrement de bout en bout pour la mise à disposition d'une instance chiffrée
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

La fonction de chiffrement de bout en bout vous permet d'utiliser votre propre image de système d'exploitation de type cloud-init que vous avez chiffrée en utilisant une clé de chiffrement de données qui vous appartient et que vous contrôlez. Une fois la configuration d'environnement effectuée, vous pouvez importer l'image chiffrée dans le référentiel de modèles d'image et l'utiliser pour mettre à disposition des instances de serveur virtuel chiffrées. Le chiffrement de bout en bout met à disposition le chiffrement des données au repos pour le stockage associé aux instances de serveur virtuel mises à disposition.
{:shortdesc}

Le chiffrement de bout en bout regroupe plusieurs composants {{site.data.keyword.cloud}} afin de fournir une solution sécurisée pour vos informations critiques.

* Un service de gestion de clés IBM tel qu'{{site.data.keyword.keymanagementservicelong_notm}} ou {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} pour sécuriser les clés de chiffrement (voir le tableau 1).
* IBM {{site.data.keyword.iamshort}} (IAM) permet au service Cloud Block Storage d'accéder à votre système de gestion de clés et à votre clé racine utilisée pour l'encapsulage de votre clé de chiffrement de données.
* {{site.data.keyword.cos_full_notm}} stocke de manière sécurisée votre image chiffrée lors de son téléchargement.
* Dans la console {{site.data.keyword.cloud_notm}}, vous pouvez importer votre image chiffrée et créer un modèle d'image.
* Avec un modèle d'image chiffré dans l'environnement d'infrastructure de la console {{site.data.keyword.cloud_notm}}, vous pouvez mettre à disposition des instances de serveur virtuel chiffrées.
* Enfin, vous pouvez effectuer l'audit des événements associés à vos serveurs virtuels chiffrés en utilisant [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov). 

## Services de gestion de clés de chiffrement

{{site.data.keyword.keymanagementserviceshort}} et {{site.data.keyword.hscrypto}} (désormais disponibles dans certaines [régions](/docs/services/hs-crypto?topic=hs-crypto-regions#regions)) utilisent une API de fournisseur de clé commune pour assurer une approche cohérente de la gestion des clés de chiffrement. En coulisse, des centre de données {{site.data.keyword.cloud_notm}} fournissent un module de sécurité dédié (HSM) afin de protéger vos clés. Vous disposez des choix suivants : 

| Service de gestion de clés | Certificat de chiffrement HSM |
| ----- | ----- |
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/concepts?topic=key-protect-getting-started-tutorial#getting-started-tutorial) | Conformité FIPS 140-2 *Niveau 2* |
| [{{site.data.keyword.hscrypto}}](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) | Conformité FIPS 140-2 *Niveau 4* |
{: caption="Tableau 1. Options de service de gestion de clés disponibles" caption-side="top"}

## Préparation de votre environnement

1. Vous devez avoir un compte mis à niveau pour utiliser le chiffrement de bout en bout pour les serveurs virtuels. Pour plus d'informations, voir [Passage à l'IBMid et liaison des comptes](/docs/account/softlayerlink.html).
2. Utilisez votre service de gestion de clés pour créer et gérer des clés. Les exemples d'étape suivants sont spécifiques à {{site.data.keyword.keymanagementserviceshort}}, mais le déroulement général s'applique également à {{site.data.keyword.hscrypto}}. Si vous utilisez {{site.data.keyword.hscrypto}}, voir la [documentation](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) sur ce service pour connaître les instructions correspondantes.
      1. Mettez à disposition le service [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision).
      2. [Créez](/docs/services/key-protect?topic=key-protect-create-root-keys) ou [importez](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) une clé racine (CRK) dans {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Facultatif** : Si vous le souhaitez, vous pouvez [créer](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) ou [importer](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) une clé standard pour le déchiffrement.      
      4. [Configurez le plug-in d'interface CLI {{site.data.keyword.cloud_notm}} Key Protect](/docs/services/key-protect?topic=key-protect-set-up-cli) afin de pouvoir [encapsuler la clé de chiffrement de données](/docs/services/key-protect?topic=key-protect-cli-reference#kp-wrap) que vous prévoyez d'utiliser pour chiffrer votre image VHD avec la clé racine. Vous avez besoin du texte chiffré associé à la clé de chiffrement de données encapsulée (WDEK) lorsque vous importez votre image chiffrée dans la console {{site.data.keyword.cloud_notm}}.   
         
       Key Protect ne sauvegarde pas les données d'authentification supplémentaires (AAD), mais vous pouvez néanmoins utiliser les AAD pour sécuriser davantage une clé, avec jusqu'à 255 chaînes, chacune délimitée par une virgule et contenant jusqu'à 255 caractères. Si vous fournissez des AAD pour l'encapsulage de clé, sauvegardez les données à un emplacement sécurisé afin d'être sûr de pouvoir accéder à et fournir ces mêmes AAD dans les futures demandes de désencapsulage de clé.
       {: tip}
      
3. Depuis IBM {{site.data.keyword.iamshort}} (IAM), [autorisez l'accès](/docs/iam?topic=iam-serviceauth#create-auth) entre votre **Cloud Block Storage** (service source) et votre **service de gestion de clés** (service cible). Si vous importez des images chiffrées depuis {{site.data.keyword.cos_full_notm}}, vous devez disposer d'une [politique d'accès définie](/docs/iam?topic=iam-userroles#userroles) pour votre service de gestion de clés dans IAM.
4. Dans IBM Cloud Console, créez une instance d'{{site.data.keyword.cos_full_notm}} et créez un compartiment pour le stockage de données. Pour plus d'informations, voir le [tutoriel d'initiation pour {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)
      1. Créez l'instance {{site.data.keyword.cos_full_notm}} dans la région où votre service de gestion de clés est mis à disposition. 
      2. Lorsque vous créez le compartiment, le paramètre **Résilience** doit avoir la valeur _Régional_.
      3. Lorsque vous créez le compartiment, vous pouvez si vous le souhaitez [le chiffrer avec votre clé](/docs/services/cloud-object-storage?topic=cloud-object-storage-encryption#encryption-kp).   

## Préparation de vos images chiffrées

1. Sélectionnez une image non chiffrée que vous souhaitez chiffrer fonctionnant dans l'environnement d'infrastructure {{site.data.keyword.cloud_notm}}. Pour cela, vous pouvez utiliser un serveur virtuel existant pour [créer un modèle d'image](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template). Pour plus d'informations, voir [Utilisation d'un modèle d'image créé à partir d'un serveur virtuel mis à disposition par cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server). Vous pouvez également utiliser une image VHD existante. Vérifiez que l'image respecte les [exigences requises pour les images chiffrées](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs).
2. Si vous utilisez un modèle d'image provenant de {{site.data.keyword.slportal}}, [exportez l'image déchiffrée](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage) dans {{site.data.keyword.cos_full_notm}}.
3. Téléchargez le fichier image à partir d'{{site.data.keyword.cos_full_notm}} sur une machine locale sécurisée afin de chiffrer l'image. Dans votre tableau de bord de service, sélectionnez l'action **Télécharger** pour extraire votre objet du stockage. Vous pouvez utiliser le plug-in de transfert à haut débit Aspera pour télécharger des images de plus de 200 Mo.
4. Utilisez l'outil vhd-util pour [chiffrer votre image VHD](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image).
5. Dans {{site.data.keyword.cos_full_notm}}, accédez à votre compartiment puis cliquez sur **Ajouter un objet** pour [télécharger](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) l'image chiffrée. Vous pouvez utiliser le plug-in de transfert à haut débit Aspera pour télécharger des images de plus de 200 Mo.

## Importation d'une image chiffrée et commande d'une instance

1. En utilisant IBM {{site.data.keyword.iamshort}} (IAM), créez un ID de service pour l'authentification lors de l'importation de l'image chiffrée dans la console {{site.data.keyword.cloud_notm}}.
      1. Créez un [ID de service](/docs/iam?topic=iam-serviceids#serviceids).
      2. Affectez une [règle d'accès](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Affectez l'accès pour les services {{site.data.keyword.cos_full_notm}} et la gestion des clés. 
      3. Créez une [clé d'API pour un ID de service](/docs/iam?topic=iam-serviceidapikeys#create_service_key).
      4. Pour plus d'informations, voir [Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/blog/introducing-ibm-cloud-iam-service-ids-api-keys){: new_window}.
2. Dans la console {{site.data.keyword.cloud_notm}}, [importez l'image chiffrée](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos) dans la page Modèles d'image.
3. Sur la page Modèles d'image, vous pouvez utiliser votre image chiffrée pour [commander](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template) une instance de serveur virtuel.
4. Avec un serveur virtuel chiffré mis à disposition, vous pouvez effectuer l'audit des [événements de serveur virtuel](/docs/vsi?topic=virtual-servers-at_events#at_events) en utilisant [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).
