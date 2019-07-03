---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:pre: .pre}
{:screen: .screen}


# Préparation et importation d'images
{: #preparing-and-importing-images}

L'écran Modèles d'image du portail {{site.data.keyword.slportal_full}} vous permet d'importer une image à partir d'une instance de service [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about). Vous pouvez importer des images qui sont au format VHD (disque dur virtuel) ou VMDK (disque de machine virtuelle). Après l'importation, les images VMDK sont converties au format VHD. Une fois qu'une image est téléchargée dans un compartiment d'une instance de service {{site.data.keyword.cos_full_notm}}, vous pouvez l'importer dans le référentiel des modèles d'image du portail {{site.data.keyword.slportal}}.
{:shortdesc}

Vous devez avoir un compte mis à niveau pour pouvoir importer des images à partir d'{{site.data.keyword.cos_full_notm}}. Pour plus d'informations, voir [Passage à l'IBMid et liaison des comptes](/docs/account/softlayerlink.html).
{: tip}

Vous devez disposer d'une [instance IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision#provision-account) commandée via la console {{site.data.keyword.cloud_notm}} (cloud.ibm.com) pour utiliser cette fonction d'importation.  IBM Cloud Object Storage provenant de control.softlayer.com n'est pas pris en charge.
{: important}

Une fois que les images sont importées en tant que modèle d'image, elles peuvent être utilisées pour mettre à disposition ou démarrer un serveur virtuel existant. Les images importées à partir d'une instance de service {{site.data.keyword.cos_full_notm}} peuvent être des images ISO personnalisées, VHD ou VMDK. Les importations VHD et VMDK sont limitées aux systèmes d'exploitation 64 bits suivants :  

* CentOS 6 et 7
* Microsoft Server Standard 2012, R2 2012 et 2016
* RedHat Enterprise Linux 6 et 7
* Ubuntu 14.04 et 16.04

Les importations sont limitées aux disques de 100 Go. Les images doivent être nommées selon l'exemple suivant : nom_fichier.vhd-0.vhd ou nom_fichier.vmdk-0.vmdk

## Conversion d'images au format VHD
{: #convert-to-vhd}

Les formats VHD et VMDK sont les seuls formats d'image pris en charge pour les serveurs virtuels. Pour convertir des images au format VHD à partir de n'importe quel format autre que VMDK, utilisez les informations suivantes :

* Qemu-img 2.7.0 ou version ultérieure est requis
* Convertissez l'image avec la commande suivante :

  ```
  qemu-img convert -f <format image> <nom image> -O vpc -o force_size <nom image>
  ```
  {: pre}

* Exemple de commande :

  ```
  qemu-img convert -f qcow2 test -O vpc -o force_size test
  ```
  {: pre}

Pour plus d'informations, voir [Conversion de formats d'image ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) dans la documentation
QEMU.

## Modèles ISO
{: #iso-templates}

Seuls les systèmes d'exploitation pris en charge par {{site.data.keyword.BluSoftlayer_notm}} peuvent être utilisés pour charger un modèle ISO sur une VSI. Une liste des
systèmes d'exploitation pris en charge est disponible sur la page [https://www.ibm.com/cloud/server-software ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/server-software)

Les images ISO importées avec cet outil doivent permettre l'initialisation pour être éligibles à l'importation.

## Configuration d'une image pour des {{site.data.keyword.virtualmachinesshort}}
{: #config-image-vsi}

Pour vérifier qu'une image peut être déployée dans l'environnement d'infrastructure {{site.data.keyword.BluSoftlayer_notm}}, vous devez configurer les images de serveur virtuel comme suit :

* ***/boot*** doit être la première partition
* ***/boot*** et ***/*** doivent être un système de fichiers ext3 ou ext4
* ***/etc***  et /root doivent être sur la même partition que ***/***
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** pour monter le disque de permutation connecté au système
* wget doit être installé
* Les outils Xen xe-guest-utilities doivent être installés. Procédez comme suit :

    1. Téléchargez l'image ISO XenServer depuis Citrix : [https://www.citrix.com/downloads/citrix-hypervisor/![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.citrix.com/downloads/citrix-hypervisor/)

    2. Montez l'image ISO en exécutant la commande suivante :

        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}

    3. Localisez le package RPM de l'image ISO en exécutant la commande suivante :

        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}

    4. La sortie répertorie un package RPM comme suit :

        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}

     5. Vous pouvez copier le package RPM, puis extraire l'image ISO de xentools. Exécutez la commande suivante pour créer une structure de répertoire contenant l'image ISO :

        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}

     6. A partir de la structure de répertoire générée, vous pouvez monter l'image ISO *guest-tools* et localiser *rpm/debs* pour installer xentools. Consultez l'exemple de structure de répertoire suivant :

        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}

Pour plus d'informations sur les images cloud-init, voir [Mise à disposition avec une image cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#provisioning-with-a-cloud-init-enabled-image).

## Préparation de l'importation d'une image chiffrée
{: #preparing-to-import-an-encrypted-image}

Si vous prévoyez d'importer une image VHD qui est chiffrée avec votre propre clé de chiffrement de données, vérifiez que vous remplissez les conditions prérequises et que vous suivez les instructions de chiffrement décrites dans [Utilisation du chiffrement de bout en bout pour la mise à disposition d'une instance chiffrée](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

Vous devez utiliser l'outil vhd-util pour chiffrer votre image, qui doit être au format VHD. Pour plus d'informations, voir [Chiffrement d'images VHD](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image). 
{: important}

## Téléchargement d'une image dans {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

Une fois que votre image est prête, vous pouvez la télécharger dans {{site.data.keyword.cos_full_notm}}. Pensez à utiliser un compartiment dans un [emplacement régional](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints).

1. Dans {{site.data.keyword.cos_full_notm}}, accédez à votre compartiment puis cliquez sur **Ajouter un objet** pour [télécharger](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) l'image.
2. Utilisez le plug-in de transfert à haut débit [Aspera](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera) pour
bénéficier de vitesses de téléchargement élevées pour votre image.

Vous pouvez utiliser le kit SDK COS avec Aspera pour initier un transfert à haut débit dans vos applications personnalisées lors de l'utilisation de Java, Python ou de NodeJS. Pour plus d'informations, voir la rubrique relative à l'[utilisation des bibliothèques et des kits SDK](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera#aspera-sdk).
{: tip}


## Importation d'une image à partir d'IBM Cloud Object Storage
{: #import-icos}

Procédez comme suit pour importer une image à partir d'{{site.data.keyword.cos_full_notm}}.

1. Accédez au menu Unité et assurez-vous de disposer des droits de compte appropriés pour exécuter les tâches. 

   * Accédez au menu Unité de votre console. Pour plus d'informations, voir [Accès aux unités](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
   * Vérifiez que vous disposez des droits de compte et accès aux unités requis. Seul le propriétaire de compte ou un utilisateur disposant de droit d'infrastructure classique **Gérer les utilisateurs** peut modifier les droits. 

   Pour plus d'informations sur les droits, voir [Droits d'infrastructure classique](/docs/iam?topic=iam-    infrapermission#infrapermission) et [Gestion de l'accès aux unités](/docs/vsi?topic=virtual-servers-managing-device-access).

   Si vous importez une image chiffrée, vous devez utiliser la console {{site.data.keyword.cloud_notm}}.
   {: important}
2. Accédez à la page **Modèles d'image** en sélectionnant **Unités > Gérer > Images**.
3. Cliquez sur l'onglet d'importation d'image à partir d'IBM COS**** pour ouvrir l'outil d'importation.
4. Renseignez les zones suivantes (voir le tableau 1).
5. Une fois l'importation à partir d'{{site.data.keyword.cos_full_notm}} terminée, l'image s'affiche sur la page Modèles d'image.

| Zone | Valeur |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Sélectionnez l'instance de service {{site.data.keyword.cos_full_notm}} où l'image que vous souhaitez importer est stockée. |
| Emplacement | Sélectionnez la région géographique spécifique où votre image est stockée. Vous pouvez importer des images dans les régions suivantes et les centres de données associés : Sud des Etats-Unis (DAL13), Est des Etats-Unis (WDC07), Europe-Grande Bretagne (LON02), Europe-Allemagne (FRA02), Asie-Japon. Une fois que l'image est importée dans un des centres de données répertorié, vous pouvez la déplacer dans un autre centre de données. |
| Compartiment | Sélectionnez le compartiment {{site.data.keyword.cos_full_notm}} où votre image est stockée. Seuls les compartiments qui existent dans l'emplacement régional sélectionné sont valides. Un message s'affiche si vous sélectionnez un compartiment qui n'existe pas à l'emplacement sélectionné.|
| Fichier image | Sélectionnez dans l'instance de service {{site.data.keyword.cos_full_notm}} le fichier image à importer. Les types de fichiers pris en charge sont VHD (Disque dur virtuel), VMDK (Disque de machine virtuelle) et ISO. Si vous importez une image chiffrée, elle doit être au format de fichier VHD et être chiffrée avec l'outil vhd-util. |
| Nom de l'image | Indiquez un nom descriptif de votre image. Il s'agit de l'image utilisée pour la mise à disposition d'instances de serveur virtuel. |
| Clé d'API | Indiquez la clé d'API qui donne accès à votre instance de service {{site.data.keyword.cos_full_notm}}. Lors de l'importation d'une image chiffrée, la clé d'API doit également avoir accès à votre instance de service de gestion de clés. La clé d'API est disponible uniquement pour être copiée ou téléchargée lors de la création. Si la clé d'API est perdue, vous devez en créer une autre. Pour plus d'informations, voir [Gestion des clés d'API](/docs/iam?topic=iam-manapikey#manapikey). |
| Système d'exploitation | Sélectionnez le système d'exploitation inclus dans votre image. |
| Cloud-init | Si l'image que vous importez est de type cloud-init, sélectionnez cette case à cocher. Si vous importez une image ayant un système d'exploitation Windows de type cloud-init et que vous sélectionnez cette option, vous ne pouvez pas sélectionner simultanément **Votre licence**. Si vous importez une image chiffrée, cette option est sélectionnée par défaut (non modifiable) car votre image chiffrée doit être de type cloud-init. |
| Votre licence | Si vous prévoyez de fournir votre propre licence de système d'exploitation, sélectionnez cette case à cocher Si vous importez une image avec un système d'exploitation Windows, vous pouvez sélectionner cette option si vous prévoyez d'utiliser l'image pour mettre à disposition [des instances d'hôte dédiées](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances). Si votre version de système d'exploitation Windows ne prend pas en charge l'utilisation de votre propre licence, cette option est désactivée. Pour les images Windows, vous ne pouvez pas sélectionner Cloud Init si vous indiquez que vous allez utiliser votre propre licence. Si vous importez une image chiffrée avec Red Hat Enterprise Linux comme système d'exploitation, cette option est sélectionnée par défaut (non modifiable) car votre image chiffrée doit inclure sa propre licence de système d'exploitation. |
| Mode d'amorçage | Sélectionnez le mode d'amorçage pour votre image. Si un mode par défaut est défini pour le système d'exploitation indiqué, le mode d'amorçage est sélectionné ici automatiquement. |
| Remarques | Ajoutez des remarques relatives à l'image pouvant être utiles aux utilisateurs. |
| Chiffrement | Si vous importez une image que vous avez chiffrée avec votre propre clé de chiffrement de données à l'aide de l'outil vhd-util, cochez cette case. |
{: caption="Tableau 1. Valeurs pour l'importation d'une image à partir d'IBM Cloud Object Storage" caption-side="top"}

Le tableau suivant présente les zones supplémentaires applicables à l'importation d'images chiffrées uniquement. Pour plus d'informations sur les images chiffrées, voir [Utilisation du chiffrement de bout en bout pour la mise à disposition d'une instance chiffrée](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

<!--
To import an encrypted image, your account must have access to the End to End (E2E) Encryption feature. To enable your account for E2E Encryption, please contact [Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
{: tip}
-->

| Zone | Valeur |
| ----- | ----- |
| Clé de chiffrement de données encapsulée | Lors de l'importation d'une image chiffrée, indiquez le texte chiffré associée à la clé de chiffrement de données utilisée pour le chiffrement de votre image. Pour plus d'informations, voir [Encapsulage de clés](/docs/services/key-protect?topic=key-protect-wrap-keys#api). |
| Instance de service de gestion de clés (KMS) | Sélectionnez une instance de service de gestion de clés dans votre compte à partir de la liste déroulante. L'instance du service de gestion de clés doit inclure la clé racine client que vous avez utilisée pour encapsuler votre clé de chiffrement de données. |
| Nom de la clé | Sélectionnez le nom de la clé racine dans l'instance de service de gestion de clés utilisée pour encapsuler votre clé de chiffrement de données. Pour plus d'informations, voir la rubrique relative à l'[affichage des clés](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
{: caption="Tableau 2. Valeurs pour l'importation d'une image chiffrée à partir d'IBM Cloud Object Storage" caption-side="top"}

## Etapes suivantes

Une fois que l'importation commence, le système recherche le fichier image dans l'instance de service {{site.data.keyword.cos_full_notm}} du compartiment indiqué. Le fichier image est importé en tant que modèle d'image qui est ensuite accessible sur la page Modèles d'image. Une fois l'importation terminée, l'image peut être utilisée pour commander une nouvelle unité ou pour démarrer une unité existante.
De plus, le modèle d'image peut être supprimé à tout moment. La durée d'importation de l'image varie en fonction de la taille du fichier, mais l'opération prend généralement de quelques minutes à une heure.

Une fois qu'une image est importée dans le référentiel de modèles d'image, vous pouvez la supprimer dans {{site.data.keyword.cos_full_notm}}. Vous pouvez toujours accéder au modèle d'image sur la page **Modèles d'image** et l'utiliser pour mettre à disposition des instances de serveur virtuel.
