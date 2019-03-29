---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-17"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Préparation et importation d'images
{: #preparing-and-importing-images}

L'écran Modèles d'image du portail {{site.data.keyword.slportal_full}} permet aux utilisateurs d'importer une image à partir d'une instance de service [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage). Une fois qu'une image est téléchargée dans un compartiment d'une instance de service {{site.data.keyword.cos_full_notm}}, vous pouvez l'importer dans le référentiel des modèles d'image du portail {{site.data.keyword.slportal}}.
{:shortdesc}

Vous devez avoir un compte mis à niveau pour pouvoir importer des images à partir d'{{site.data.keyword.cos_full_notm}}. Pour plus d'informations, voir [Passage à l'IBMid et liaison des comptes](/docs/account?topic=account-unifyingaccounts).
{: tip}

Une fois que les images sont importées en tant que modèle d'image, elles peuvent être utilisées pour mettre à disposition ou démarrer un serveur virtuel existant. Les images importées à partir d'une instance de service {{site.data.keyword.cos_full_notm}} peuvent être des images ISO personnalisées ou VHD. L'importation d'images VHD est limitée aux systèmes d'exploitation 64 bits suivants :  

* CentOS 6 et 7
* RedHat Enterprise Linux 6 et 7
* Ubuntu 14.04 et 16.04
* Microsoft Server Standard 2012, R2 2012 et 2016

Les importations d'image VHD sont limitées aux disques de 100 Go. Les images VHD doivent être nommées conformément à l'exemple suivant : nomfichier.vhd-0.vhd.

## Conversion d'images au format VHD
{: #convert-to-vhd}

Le format VHD est le seul format d'image pris en charge pour les serveurs virtuels. Pour convertir des images au format VHD, utilisez les informations suivantes :

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

    1. Téléchargez l'image ISO XenServer à partir de Citrix : [https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html)

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

Pour plus d'informations sur les images cloud-init, voir [Mise à disposition avec une image cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image).

## Téléchargement d'une image dans {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

Une fois que votre image est prête, vous pouvez la télécharger dans {{site.data.keyword.cos_full_notm}}. Pensez à utiliser un compartiment dans un [emplacement régional](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#select-regions-and-endpoints).

1. Dans {{site.data.keyword.cos_full_notm}}, accédez à votre compartiment puis cliquez sur **Ajouter un objet** pour [télécharger](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data) l'image.
2. Utilisez le plug-in de transfert à haut débit [Aspera](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#Aspera-high-speed-transfer) pour
bénéficier de vitesses de téléchargement élevées pour votre image.

Vous pouvez utiliser le kit SDK COS avec Aspera pour initier un transfert à haut débit dans vos applications personnalisées lors de l'utilisation de Java, Python ou de NodeJS. Pour plus d'informations, voir la rubrique relative à l'[utilisation des bibliothèques et des kits SDK](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#sdk).
{: tip}


## Importation d'une image à partir d'IBM Cloud Object Storage
{: #import-icos}

Procédez comme suit pour importer une image à partir d'{{site.data.keyword.cos_full_notm}}.

1. Dans le portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/), accédez à la page **Modèles d'image** en sélectionnant **Unités > Gérer > Images**.
2. Cliquez sur l'onglet d'importation d'image à partir d'IBM COS**** pour ouvrir l'outil d'importation.
3. Renseignez les zones suivantes (voir le tableau 1).
4. Une fois l'importation à partir d'{{site.data.keyword.cos_full_notm}} terminée, l'image s'affiche sur la page Modèles d'image.

| Zone | Valeur |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Sélectionnez l'instance de service {{site.data.keyword.cos_full_notm}} où l'image que vous souhaitez importer est stockée. |
| Emplacement | Sélectionnez la région géographique spécifique où votre image est stockée. Vous pouvez importer des images dans les régions suivantes et les centres de données associés : Etats-Unis-Sud (DAL13), Etats-Unis-Est (WDC07), Europe-Grande Bretagne (LON02), Europe-Allemagne (FRA02), Asie-Japon (TOK02). Une fois que l'image est importée dans un des centres de données répertorié, vous pouvez la déplacer dans un autre centre de données. |
| Compartiment | Sélectionnez le compartiment {{site.data.keyword.cos_full_notm}} où votre image est stockée. Seuls les compartiments qui existent dans l'emplacement régional sélectionné sont valides. Un message s'affiche si vous sélectionnez un compartiment qui n'existe pas à l'emplacement sélectionné.|
| Fichier image | Sélectionnez dans l'instance de service {{site.data.keyword.cos_full_notm}} le fichier image à importer. Les types de fichier pris en charge sont VHD, ISO et RAW. Si vous importez une image chiffrée, elle doit être au format de fichier RAW et être chiffré avec la méthode de chiffrement de disque LUKS. |
| Nom de l'image | Indiquez un nom descriptif de votre image. Il s'agit de l'image utilisée pour la mise à disposition d'instances de serveur virtuel. |
| Clé d'API | Indiquez la clé d'API qui donne accès à votre instance de service {{site.data.keyword.cos_full_notm}}. Lors de l'importation d'une image chiffrée, la clé d'API doit également avoir accès à Key Protect. La clé d'API est disponible uniquement pour être copiée ou téléchargée lors de la création. Si la clé d'API est perdue, vous devez en créer une autre. Pour plus d'informations, voir [Gestion des clés d'API](/docs/iam?topic=iam-manapikey). |
| Système d'exploitation | Sélectionnez le système d'exploitation inclus dans votre image. Pour les images chiffrées, vous ne pouvez sélectionner que des systèmes d'exploitation Linux. |
| Cloud-init | Si l'image que vous importez est de type cloud-init, sélectionnez cette case à cocher. Si vous importez une image ayant un système d'exploitation Windows de type cloud-init et que vous sélectionnez cette option, vous ne pouvez pas sélectionner **Votre licence**. Si vous importez une image chiffrée, cette option est sélectionnée par défaut (non modifiable) car votre image chiffrée doit être de type cloud-init. |
| Votre licence | Si vous prévoyez de fournir votre propre licence de système d'exploitation, sélectionnez cette case à cocher Si vous importez une image avec un système d'exploitation Windows, vous pouvez sélectionner cette option si vous prévoyez d'utiliser l'image pour mettre à disposition [des instances d'hôte dédiées](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances). Si votre version de système d'exploitation Windows ne prend pas en charge l'utilisation de votre propre licence, cette option est désactivée. Pour les images Windows, vous ne pouvez pas sélectionner Cloud Init si vous indiquez que vous allez utiliser votre propre licence. Si vous importez une image chiffrée avec Red Hat Enterprise Linux comme système d'exploitation, cette option est sélectionnée par défaut (non modifiable) car votre image chiffrée doit inclure sa propre licence de système d'exploitation. |
| Mode d'amorçage | Sélectionnez le mode d'amorçage pour votre image. Si un mode par défaut est défini pour le système d'exploitation indiqué, le mode d'amorçage est sélectionné ici automatiquement. |
| Remarques | Ajoutez des remarques relatives à l'image pouvant être utiles aux utilisateurs. |
| Chiffrement | La sélection de cette case à cocher est déterminée par le type de fichier de l'image que vous choisissez d'importer. Les images VHD et ISO indiquent que le fichier image n'est pas chiffré. Par conséquent, la case à cocher n'est pas sélectionnée pour les images VHD et ISO. Un fichier image RAW indique que l'image est chiffrée. Si un fichier image RAW est spécifié, cette case à cocher est sélectionnée par défaut et cela n'est pas modifiable. |
{: caption="Tableau 1. Valeurs pour l'importation d'une image à partir d'IBM Cloud Object Storage" caption-side="top"}

Le tableau suivant présente les zones supplémentaires applicables à l'importation d'images chiffrées uniquement. Pour plus d'informations sur les images chiffrées, voir [Utilisation du chiffrement de bout en bout pour la mise à disposition d'une instance chiffrée](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

Pour importer une image chiffrée, votre compte doit avoir accès à la fonction de chiffrement de bout en bout. Pour activer votre compte pour ce type de chiffrement, veuillez contacter le support.
{: tip}

| Zone | Valeur |
| ----- | ----- |
| ID d'instance de service {{site.data.keyword.keymanagementserviceshort}} | Lors de l'importation d'une image chiffrée, votre instance de service {{site.data.keyword.keymanagementserviceshort}} doit être dans la même région que votre compartiment {{site.data.keyword.cos_full_notm}}. Vous pouvez utiliser l'interface CLI {{site.data.keyword.cloud_notm}} pour trouver votre ID d'instance {{site.data.keyword.keymanagementserviceshort}}. Pour plus d'informations, voir [Extraction de l'ID d'instance](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID). |
| Clé de chiffrement de données encapsulée | Lors de l'importation d'une image chiffrée, indiquez le texte chiffré associée à la clé de chiffrement de données utilisée pour le chiffrement de votre image. Pour plus d'informations, voir [Encapsulage de clés](/docs/services/key-protect?topic=key-protect-wrap-keys#api). |
| ID de la clé racine | Lors de l'importation d'une image chiffrée, indiquez l'ID de la clé racine utilisée pour l'encapsulation de la clé de chiffrement de données. Pour plus d'informations, voir la rubrique relative à l'[affichage des clés](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
{: caption="Tableau 2. Valeurs pour l'importation d'une image chiffrée à partir d'IBM Cloud Object Storage" caption-side="top"}

## Etapes suivantes

Une fois que l'importation commence, le système recherche le fichier image dans l'instance de service {{site.data.keyword.cos_full_notm}} du compartiment indiqué. Le fichier image est importé en tant que modèle d'image qui est ensuite accessible sur la page Modèles d'image. Une fois l'importation terminée, l'image peut être utilisée pour commander une nouvelle unité ou pour démarrer une unité existante.
De plus, le modèle d'image peut être supprimé à tout moment. La durée d'importation de l'image varie en fonction de la taille du fichier, mais l'opération prend généralement de quelques minutes à une heure.

Une fois qu'une image est importée dans le référentiel de modèles d'image, vous pouvez la supprimer dans {{site.data.keyword.cos_full_notm}}. Vous pouvez toujours accéder au modèle d'image sur la page **Modèles d'image** et l'utiliser pour mettre à disposition des instances de serveur virtuel.
