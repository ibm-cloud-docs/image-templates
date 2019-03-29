---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-21"

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

La fonction de chiffrement de bout en bout vous permet d'utiliser votre propre image de système d'exploitation de type cloud-init que vous avez chiffrée en utilisant une clé de chiffrement de données qui vous appartient et que vous contrôlez. Une fois la configuration d'environnement effectuée, vous pouvez importer l'image chiffrée dans le référentiel de modèles d'image et l'utiliser pour mettre à disposition des instances de serveur virtuel chiffrées. Le chiffrement de bout en bout met à disposition le chiffrement des données au repos pour le stockage associé aux instances de serveur virtuel mises à disposition. Pour pouvoir accéder à cette fonction, contactez le support.
{:shortdesc}

Le chiffrement de bout en bout regroupe plusieurs composants {{site.data.keyword.cloud}} afin de fournir une solution sécurisée pour vos informations critiques.

* {{site.data.keyword.keymanagementservicefull_notm}} sécurise vos clés avec des modules HSM (hardware security module) basés sur le cloud et certifiés FIPS 140-2 niveau 2 qui vous protègent contre le vol d'informations.
* IBM {{site.data.keyword.iamshort}} (IAM) permet au service Cloud Block Storage d'accéder à {{site.data.keyword.keymanagementserviceshort}} et à votre clé racine utilisée pour l'encapsulage de votre clé de chiffrement de données.
* {{site.data.keyword.cos_full_notm}} stocke de manière sécurisée votre image chiffrée lors de son téléchargement.
* Dans {{site.data.keyword.slportal}}, vous pouvez importer votre image chiffrée et créer un modèle d'image.
* Avec un modèle d'image chiffré dans l'environnement d'infrastructure {{site.data.keyword.cloud_notm}} Console, vous pouvez mettre à disposition des instances de serveur virtuel chiffrées.
* Pour finir, vous pouvez effectuer l'audit des événements associés à vos serveurs virtuels chiffrés en utilisant Activity Tracker.

## Préparation de votre environnement

1. Vous devez avoir un compte mis à niveau pour utiliser le chiffrement de bout en bout pour les serveurs virtuels. Pour plus d'informations, voir [Passage à l'IBMid et liaison des comptes](/docs/account?topic=account-unifyingaccounts).
2. Utilisez {{site.data.keyword.keymanagementservicefull_notm}} pour créer et gérer les clés.
      1. Mettez à disposition le service [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision).
      2. [Créez](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys) ou [importez](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) une clé racine (CRK) dans {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Facultatif** : Si vous le souhaitez, vous pouvez [créer](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) ou [importer](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) une clé standard pour le déchiffrement.
      4. Obtenez [l'accès à l'API {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-set-up-api#set-up-api) afin de pouvoir encapsuler la clé de chiffrement de données.
      5. [Encapsulez la clé de chiffrement de données](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys) avec la clé racine. Vous avez besoin du texte chiffré associé à la clé de chiffrement de données encapsulée (WDEK) lorsque vous importez votre image chiffrée dans le portail {{site.data.keyword.slportal}}.
3. A partir d'IBM {{site.data.keyword.iamshort}} (IAM), [autorisez l'accès](/docs/iam?topic=iam-serviceauth#create-an-authorization) entre **Cloud Block Storage** (service source) et {{site.data.keyword.keymanagementservicelong_notm}} (service cible). Les utilisateurs qui importent des images chiffrées à partir d'{{site.data.keyword.cos_full_notm}} doivent disposer d'une [règle d'accès définie](/docs/iam?topic=iam-userroles) pour {{site.data.keyword.keymanagementservicelong_notm}} dans IAM.
4. Dans IBM Cloud Console, créez une instance d'{{site.data.keyword.cos_full_notm}} et créez un compartiment pour le stockage de données. Pour plus d'informations, voir [Initiation à {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-console-#getting-started-console-).
      1. Créez l'instance {{site.data.keyword.cos_full_notm}} au même emplacement régional que celui où votre service {{site.data.keyword.keymanagementserviceshort}} est mis à disposition.
      2. Lorsque vous créez le compartiment, le paramètre **Résilience** doit avoir la valeur _Régional_.
      3. Lorsque vous créez le compartiment, vous pouvez si vous le souhaitez [le chiffrer avec votre clé](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-manage-encryption#sse-kp).   

## Préparation de vos images chiffrées

1. Sélectionnez une image non chiffrée que vous souhaitez chiffrer fonctionnant dans l'environnement d'infrastructure {{site.data.keyword.cloud_notm}}. Pour cela, vous pouvez utiliser un serveur virtuel existant pour [créer une image personnalisée](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template). Pour plus d'informations, voir [Utilisation d'un modèle d'image créé à partir d'un serveur virtuel mis à disposition par cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-a-standard-image-created-from-a-cloud-init-provisioned-virtual-server). Vous pouvez également utiliser une image VHD existante. Vérifiez que l'image respecte les [exigences requises pour les images chiffrées](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#encrypted-image-reqs).
2. Si vous utilisez un modèle d'image du portail {{site.data.keyword.slportal}}, [exportez l'image non chiffrée](/docs/infrastructure/image-templates?topic=image-templates-exporting-to-ibm-cos) dans {{site.data.keyword.cos_full_notm}}.
3. Téléchargez le fichier image à partir d'{{site.data.keyword.cos_full_notm}} sur une machine locale sécurisée afin de chiffrer l'image. Dans votre tableau de bord de service, sélectionnez l'action **Télécharger** pour extraire votre objet du stockage. Vous pouvez utiliser le plug-in de transfert à haut débit Aspera pour télécharger des images de plus de 200 Mo.
4. Utilisez le format de chiffrement de disque LUKS pour [chiffrer votre image](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption) en utilisant QEMU et DMCrypt.
5. Dans {{site.data.keyword.cos_full_notm}}, accédez à votre compartiment puis cliquez sur **Ajouter un objet** pour [télécharger](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data) l'image chiffrée. Vous pouvez utiliser le plug-in de transfert à haut débit Aspera pour télécharger des images de plus de 200 Mo.

## Importation d'une image chiffrée et commande d'une instance

1. En utilisant IBM {{site.data.keyword.iamshort}} (IAM), créez un ID de service pour l'authentification lors de l'importation de l'image chiffrée dans {{site.data.keyword.slportal}}.
      1. Créez un [ID de service](/docs/iam?topic=iam-serviceids#serviceids).
      2. Affectez une [règle d'accès](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Affectez l'accès pour les services {{site.data.keyword.cos_full_notm}} et {{site.data.keyword.keymanagementservicelong_notm}}.
      3. Créez une [clé d'API pour un ID de service](/docs/iam?topic=iam-serviceidapikeys#creating-an-api-key-for-a-service-id).
      4. Pour plus d'informations, voir [Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window}.
2. Dans le portail {{site.data.keyword.slportal}}, [importez l'image chiffrée](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images#import-icos) sur la page Modèles d'image. (Vous pouvez également importer l'image chiffrée en [utilisant l'API {{site.data.keyword.slapi_short}}](/docs/infrastructure/image-templates?topic=image-templates-importing-an-encrypted-image-by-using-the-softlayer-api).)
3. Sur la page Modèles d'image, vous pouvez utiliser votre image chiffrée pour [commander](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template) une instance de serveur virtuel.
4. Avec un serveur virtuel chiffré mis à disposition, vous pouvez effectuer l'audit des [événements de serveur virtuel](/docs/vsi?topic=virtual-servers-at_events#at_events) en utilisant [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov).
