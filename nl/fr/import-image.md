---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-31"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Préparation et importation d'images

L'écran Modèles d'image du [portail client ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/) permet à des utilisateurs de télécharger une image existante à partir d'un compte [Object Storage](/docs/infrastructure/objectstorage-swift/index.html) basé sur Swift.
{:shortdesc}

Une fois que les images sont importées en tant que modèle d'image, elles peuvent être utilisées pour mettre à disposition ou démarrer un serveur virtuel existant. Les images importées d'un compte Object Storage peuvent être des images VHD ou des images ISO personnalisées. L'importation d'images VHD est limitée aux systèmes d'exploitation 64 bits suivants :

* CentOS 6 et 7
* RedHat Enterprise Linux 6 et 7
* Ubuntu 14.04 et 16.04
* Microsoft Server Standard 2012, R2 2012 et 2016

Les importations d'image VHD sont limitées aux disques de 100 Go. Les images VHD doivent être nommées conformément à l'exemple suivant : nomfichier.vhd-0.vhd.

## Conversion d'images au format VHD

Le format VHD est le seul format d'image pris en charge pour les {{site.data.keyword.BluVirtServers_full}}. Pour convertir des images au format VHD, utilisez les informations suivantes :

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

Seuls les systèmes d'exploitation pris en charge par {{site.data.keyword.BluSoftlayer_notm}} peuvent être utilisés pour charger un modèle ISO sur une VSI. Leur liste est disponible à l'adresse suivante : [http://www.softlayer.com/services/software/ ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.softlayer.com/services/software/)

Les images ISO importées avec cet outil doivent permettre l'initialisation pour être éligibles à l'importation. 

## Configuration d'une image pour des {{site.data.keyword.virtualmachinesshort}}

Pour vérifier qu'une image peut être déployée dans l'environnement d'infrastructure {{site.data.keyword.BluSoftlayer_notm}}, vous devez configurer les images de serveur virtuel comme suit : 

* ***/boot*** doit être la première partition
* ***/boot*** et ***/*** doivent être un système de fichiers ext3 ou ext4
* ***/etc***  et /root doivent être sur la même partition que ***/***
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** pour monter le disque de permutation connecté au système
* wget doit être installé
* Les outils Xen xe-guest-utilities doivent être installés. Procédez comme suit :
    
    1. Téléchargez l'image ISO XenServer à partir de Citrix : [https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html)
    
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
    
Pour plus d'informations sur les images cloud-init, voir [Mise à disposition avec une image cloud-init](image_cloud-init.html).

## Importation d'une image

Procédez comme suit pour importer une image dans le portail client.

1. Localisez et notez les détails suivants pour l'image provenant du compte {{site.data.keyword.objectstorageshort}}. Pour plus d'informations, voir [Affichage et modification des détails du fichier de stockage d'objets](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html).
  * Nom de compte
  * Cluster
  * Conteneur
  * Nom de fichier de l'image
2. Dans le [portail client ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/), accédez à la page **Modèles d'image** en sélectionnant **Unités > Gérer > Images**.
3. Cliquez sur l'onglet **Importer image** pour ouvrir l'outil d'importation. 
4. Sélectionnez le **compte {{site.data.keyword.objectstorageshort}}** de l'image à importer dans la liste déroulante **Compte**. 
5. Sélectionnez le **cluster {{site.data.keyword.objectstorageshort}}** de l'image à importer dans la liste déroulante **Cluster**. 
6. Sélectionnez le **conteneur {{site.data.keyword.objectstorageshort}}** de l'image à importer dans la liste déroulante **Conteneur**. 
7. Sélectionnez le **nom de fichier de l'image** tel qu'il figure dans {{site.data.keyword.objectstorageshort}} dans la liste déroulante **Fichier image**.
8. Saisissez le nom d'image du nouveau modèle d'image dans la zone **Nom de l'image**.
9. Saisissez des remarques applicables dans la zone de texte **Remarques**.
10. Sélectionnez le système d'exploitation de l'image dans la liste déroulante **Système d'exploitation**.

  La liste déroulante Système d'exploitation est désactivée si l'image à importer est une image ISO personnalisée. Cette étape n'est nécessaire que pour l'importation d'une image VHD.
{:tip}

11. Si l'image que vous importez est de type cloud-init, cochez la case **Cloud Init**. Pour plus d'informations, voir [Mise à disposition avec une image cloud-init](image_cloud-init.html).        
12. Si vous prévoyez de fournir votre propre licence de système d'exploitation, cochez la case **Votre licence**. Pour plus d'informations, voir [Utilisation de Red Hat Cloud Access](use-red-hat-cloud-access.html).
13. Cliquez sur **Importer** pour importer l'image dans l'écran Modèles d'image. Cliquez sur **Annuler** pour annuler l'action.

## Etapes suivantes

Une fois que l'importation commence, le système localise le fichier image dans le compte {{site.data.keyword.objectstorageshort}} en utilisant le chemin indiqué (Compte > Cluster > Conteneur > Fichier image). Le fichier image est importé en tant que modèle d'image qui est ensuite accessible sur la page Modèles d'image. Une fois l'importation terminée, l'image peut être utilisée pour commander une nouvelle unité ou pour démarrer une unité existante. En outre, l'image peut être supprimée à tout moment. La durée d'importation de l'image varie en fonction de la taille du fichier, mais l'opération prend généralement de quelques minutes à une heure. 

