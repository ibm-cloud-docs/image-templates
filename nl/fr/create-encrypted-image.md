---

copyright:
  years: 2018
lastupdated: "2018-09-19"

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


# Création d'une image chiffrée
{: #creating-an-encrypted-image}

Dans le cadre de la fonction de chiffrement de bout en bout, vous pouvez chiffrer une image à importer dans Modèles d'image et l'utiliser pour déployer une instance de serveur virtuel chiffrée.
{:shortdesc}

## Exigences liées aux images chiffrées
{: #encrypted-image-reqs}

Lorsque vous créez une image chiffrée, les exigences suivantes doivent être respectées :

* L'image doit être compatible avec l'environnement d'infrastructure {{site.data.keyword.cloud}} Console.
* L'image doit inclure un système d'exploitation Linux, tels que CentOS, Debian, Red Hat Enterprise Linux ou Ubuntu.
* L'image doit être de type cloud-init.
* L'image doit être chiffrée via la méthode de [chiffrement de disque LUKS](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption).

## Utilisation de QEMU et de DM-Crypt pour créer une image RAW chiffrée
{: #luks-disk-encryption}

Pour chiffrer votre image, vous devez convertir un fichier image VHD fixe au format RAW. Utilisez ensuite QEMU et DM-Crypt pour créer un fichier image en utilisant la méthode de chiffrement de disque LUKS. Montez le fichier en tant que volume chiffré puis copiez le fichier image non chiffré dans le volume chiffré.

Cette procédure vous guide pour effectuer les tâches suivantes :

* Convertir votre image VHD dynamique en image VHD fixe.
* Convertir votre image VHD fixe au format de fichier RAW.
* Créer un fichier de clé de chiffrement de données que vous allez utiliser pour chiffrer l'unité.
* Créer un fichier RAW dans lequel placer l'image ainsi que l'en-tête de chiffrement LUKS.
* Formater le fichier RAW avec la méthode de chiffrement de disque LUKS.
* Associer le fichier RAW chiffré à une unité par bloc.
* Copier l'image non chiffrée dans l'unité de volume chiffrée.

Vous devez être autorisé à exécuter des commandes en utilisant les droits d'accès utilisateur `root`, via sudo, afin de monter et de démonter des volumes chiffrés sur votre système. Vous pouvez émettre la commande suivante pour vérifier vos privilèges d'utilisateur `root` :

```
sudo echo "Hello!"
```
{: pre}

Pour terminer cette tâche de chiffrement, "Hello!" doit être renvoyé en réponse.

**Conseil** : Vous devez effectuer cette tâche de chiffrement sur un système sous Linux et disposant des packages suivants :
* qemu ou qemu-image (en fonction de votre système d'exploitation Linux)
* cryptsetup

Procédez comme suit pour chiffrer votre image.

1. Convertissez votre fichier image VHD dynamique en fichier image VHD fixe. Un fichier VHD fixe empêche la corruption pouvant survenir si un élément VHD dynamique est converti directement au format de fichier RAW. Exécutez la commande suivante pour convertir le fichier image VHD dynamique en fichier image VHD fixe :

  ```
  qemu-img convert -p -O vpc -o subformat=fixed <DYNAMIC_VHD_FILE> <FIXED_VHD_FILE>
  ```
  {: pre}

  Par exemple, si votre fichier VHD dynamique s'appelle _Rhel_7.vhd-0.vhd_ et que vous souhaitez que le fichier VHD fixe soit nommé _Rhel_7.fixed.vhd-0.vhd_, exécutez la commande suivante :

  ```
  $ qemu-img convert -p -O vpc -o subformat=fixed Rhel_7.vhd-0.vhd Rhel_7.fixed.vhd-0.vhd
  ```
  {: screen}

  L'exécution de cette commande peut durer 30 minutes ou plus.
  {: tip}

2. Convertissez votre fichier image VHD fixe au format de fichier RAW en utilisant QEMU. Pour pouvoir être chiffrée, l'image doit être au format de fichier RAW. Exécutez la commande QEMU suivante :

  ```
  qemu-img convert -p -O raw <FIXED_VHD_FILE> <RAW_IMAGE_FILE>
  ```
  {: pre}

  Par exemple, si votre fichier VHD fixe se nomme _Rhel_7.fixed.vhd-0.vhd_ et que vous souhaitez que le fichier de sortie RAW soit nommé _Rhel_7.raw-0.raw_, exécutez la commande suivante :

  ```
  qemu-img convert -p -O raw Rhel_7.fixed.vhd-0.vhd Rhel_7.raw-0.raw
  ```
  {: screen}

  L'exécution de cette commande peut durer 30 minutes ou plus.
  {: tip}

3. Identifiez la clé de chiffrement de données que vous allez utiliser pour le chiffrement et le déchiffrement de l'unité. Il s'agit de la clé encapsulée et spécifiée lors de l'importation de l'image chiffrée dans le portail {{site.data.keyword.slportal}}. Créez un fichier contenant la clé de chiffrement de données utilisée pour le chiffrement et le déchiffrement de l'unité. Dans ce fichier, la clé doit être désencapsulée et codée en base64. Ainsi, il est garanti qu'aucun espace ou retour à la ligne accidentel n'est inclus. La clé de chiffrement de données codée en base64 doit inclure au moins 32 caractères ou octets et au maximum 512 caractères ou octets. Le matériel de clé de chiffrement de données doit se trouver sur une seule ligne. Par exemple, utilisez la commande suivante pour créer un fichier nommé `secret.dek` dans lequel stocker votre élément `unwrapped_key_material` pour votre clé de chiffrement de données et coder ce dernier avec base64 :

  ```
  $ echo -n $(echo 'unwrapped_key_material' | base64) > secret.dek
  ```
  {: screen}

  Conservez précieusement cette clé. Si vous la perdez, vous ne pourrez plus déchiffrer votre disque.
  {: tip}

4. Identifiez la taille correcte du nouveau fichier RAW à créer à l'étape suivante pour le disque chiffré, en prenant en compte l'ajout d'un en-tête LUKS. Le nouveau fichier RAW sera utilisé par _dmcrypt_ et _cryptsetup_ pour inclure le contenu du disque à un format LUKS chiffré. La taille du nouveau fichier RAW doit correspondre à la somme de la taille du fichier RAW créé à l'étape 2 plus un en-tête LUKS constant de 4 Mo. Pour déterminer la taille de votre image RAW existante, exécutez la commande suivante où _Rhel_7.raw-0.raw_ correspond au nom de l'image :

  ```
  ls -l Rhel_7.raw-0.raw
  ```
  {: pre}

  Le résultat est similaire à l'exemple suivant :

  ```
  -rw-r--r--. 1 user user 42949017600 Mar 5 10:02 Rhel_7.raw-0.raw
  ```
  {: screen}

  Où _42949017600_ correspond au nombre d'octets utilisés par l'image RAW
      1. Ajoutez 4 Mo (convertis en octets) à la taille du fichier image RAW pour prendre en compte l'en-tête LUKS. Par exemple, 42949017600 + (4 x 1024 x 1024) = 42953211904 octets.
      2. Convertissez la somme de la taille de l'image RAW et de l'en-tête LUKS en mégaoctets et arrondissez au mégaoctet suivant. Par exemple, 42953211904 octets / 1024 octets / 1024 kilooctets = 40963,375 mégaoctets = **40964 Mo**.

5. Créez un fichier RAW avec le nombre correct d'octets qui deviendra l'image RAW chiffrée. Utilisez la commande _dd_ pour créer le fichier RAW :

  ```
  dd if=/dev/zero of=<ENCRYPTED_RAW_FILENAME> bs=1M count=<TARGET_SIZE_IN_MEGABYTE> status=progress
  ```
  {: pre}

  Où _ENCRYPTED_RAW_FILENAME_ est le fichier qui deviendra l'image RAW chiffrée et _TARGET_SIZE_IN_MEGABYTE_ est le chiffre obtenu à l'étape 4 correspondant à la taille de l'image RAW non chiffrée plus l'en-tête LUKS.

  Par exemple, si vous souhaitez que votre nouveau fichier RAW qui deviendra l'image chiffrée soit nommé _Rhel_7.encrypted.raw_ et que sa taille cible soit _40964_ Mo, exécutez la commande suivante :

  ```
  $ dd if=/dev/zero of=Rhel_7.encrypted.raw bs=1M count=40964 status=progress
  ```
  {: screen}

6. Formatez le fichier RAW avec la méthode de chiffrement par disque LUKS en utilisant _dmcrypt_ et votre clé de chiffrement de données (étape 3). Cette étape permet de préparer le fichier pour qu'il contienne l'image. Pour formater le fichier avec le chiffrement LUKS, exécutez la commande _cryptsetup_ :

  ```
  cryptsetup -v luksFormat <ENCRYPTED_RAW_FILENAME> <DEK_FILENAME>
  ```
  {: pre}

  Où _ENCRYPTED_RAW_FILENAME_ correspond au nom du fichier RAW à chiffrer et _DEK_FILENAME_ au nom du fichier dans lequel votre clé de chiffrement de données est stockée.

  Par exemple, si votre fichier RAW se nomme _Rhel_7.encrypted.raw_ et que le fichier de clés s'appelle _secret.dek_, entrez :

  ```
  $ cryptsetup -v luksFormat Rhel_7.encrypted.raw secret.dek
  ```
  {: screen}

  Après l'exécution de la commande cryptsetup, l'invite suivante s'affiche. Cette dernière est attendue et vous devez répondre en indiquant YES pour continuer.

  ```
  WARNING!
  ========
  This will overwrite data on Rhel_7.encrypted.raw irrevocably.
  Are you sure (Type uppercase yes): YES
  ```
  {: screen}

7. Connectez le fichier RAW chiffré en tant que volume pour associer une nouvelle unité par bloc au système d'exploitation. En utilisant _cryptsetup_, exécutez la commande suivante pour ouvrir l'unité RAW chiffrée avec l'option luksOpen.

  Vous devez disposer du privilège sudo pour exécuter la commande suivante en utilisant les droits d'accès utilisateur `root`.
  {: tip}

  ```
  sudo cryptsetup -v luksOpen <ENCRYPTED_RAW_FILENAME> <VOLUME_NAME> --key-file <DEK_FILENAME>
  ```
  {: pre}

  Où _ENCRYPTED_RAW_FILENAME_ correspond au nom du fichier RAW chiffré, _VOLUME_NAME_ au nom de l'unité que _dmcrypt_ tente de créer et _DEK_FILENAME_ au nom de fichier de votre clé de chiffrement de données.

  Par exemple, si votre fichier RAW chiffré se nomme _Rhel_7.encrypted.raw_, que vous souhaitez nommer l'unité par bloc _encryptedVolume_ et que le fichier de clé de chiffrement de données s'appelle _secret.dek_, utilisez la commande suivante :

  ```
  $ sudo cryptsetup -v luksOpen Rhel_7.encrypted.raw encryptedVolume --key-file secret.dek
  ```
  {: screen}

  Une fois cette étape terminée, une nouvelle unité par bloc est créée. Cette dernière est accessible en lecture et en écriture sous _/dev/mapper/_.

8. Confirmez que l'outil de mappage de volume LUKS a été créé en exécutant la commande suivante :

  ```
  ls -l /dev/mapper/
  ```
  {: pre}

  Un résultat similaire à l'exemple suivant doit s'afficher :

  ```
  total 0
  lrw-rw---- 1 root root        7 Mar 5 22:22 encryptedVolume -> ../dm-0
  ```
  {: screen}

9. Copiez l'image non chiffrée (provenant de l'étape 2) dans l'unité de volume chiffrée (provenant de l'étape 7). Exécutez la commande _dd_ pour copier l'image non chiffrée dans le volume chiffré :

  ```
  sudo dd if=<RAW_IMAGE_FILE> of=/dev/mapper/<VOLUME_NAME>
  ```
  {: pre}

  Où _RAW_IMAGE_FILE_ correspond au nom du fichier RAW non chiffré (créé à l'étape 2) et _VOLUME_NAME_ au nom de l'unité par bloc du volume chiffré (créée à l'étape 7).

  Par exemple, si votre élément RAW_IMAGE_FILE se nomme _Rhel_7.raw-0.raw_ et que votre élément VOLUME_NAME s'appelle _encryptedVolume_, vous exécutez la commande suivante :

  ```
  $ sudo dd if=Rhel_7.raw-0.raw of=/dev/mapper/encryptedVolume status=progress
  ```
  {: screen}

  L'exécution de cette commande peut durer 30 minutes ou plus.
  {: tip}

10. Supprimez l'outil de mappage de volume et fermez la connexion LUKS au fichier de données chiffré :

  ```
  sudo cryptsetup luksClose encryptedVolume
  ```
  {: pre}

  Où _encryptedVolume_ correspond au nom de l'unité par bloc du volume chiffré.

  Votre élément _ENCRYPTED_RAW_FILENAME_ est désormais initialisé et vous pouvez le télécharger dans IBM Cloud Object Storage. Par exemple, si votre fichier RAW chiffré se nomme _Rhel_7.encrypted.raw_, téléchargez cette image dans IBM Cloud Object Storage.
