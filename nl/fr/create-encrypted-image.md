---

copyright:
  years: 2019
lastupdated: "2019-03-27"

keywords: VHD image file, encryption, encrypted image, image

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}


# Chiffrement d'images VHD 
{: #create-encrypted-image}

Pour utiliser la fonction de chiffrement de bout en bout, vous devez chiffrer votre image VHD avec l'outil vhd-util avant de l'importer dans Modèles d'image pour mettre à disposition les instances chiffrées. Deux niveaux de chiffrement AES sont pris en charge : AES 256 bits et AES 512 bits.
{:shortdesc}

## Exigences liées aux images VHD chiffrées
{: #encrypted-image-reqs}

Les images VHD chiffrées doivent respecter les exigences suivantes : 

* Format VHD. 
* Compatibilité avec l'environnement d'infrastructure de la console {{site.data.keyword.cloud}}. 
* Mise à disposition avec un [système d'exploitation pris en charge](/docs/infrastructure/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images). 
* Activation pour cloud-init. 
* Chiffrement avec [l'outil vhd-util](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool). 

## Chiffrement de votre image VHD
{: #vhd-util-tool}

Procédez comme suit pour créer votre image VHD chiffrée : 

1. Sélectionnez un système CentOS exécutant la version 7 ou une version ultérieure pour chiffrer votre image de disque virtuel (fichier VHD) pour {{site.data.keyword.cloud_notm}}. Si vous n'avez pas accès à un matériel physique sur lequel CentOS est installé, vous pouvez mettre à disposition
une instance de serveur virtuel avec CentOS 7 dans {{site.data.keyword.cloud_notm}} en utilisant un hôte public ou dédié. Il n'est pas nécessaire que
le système CentOS utilisé pour le chiffrement des fichiers VHD soit lui-même chiffré. 

2. Connectez-vous à votre système CentOS et au réseau privé virtuel (VPN) de votre client, puis [accédez au site de téléchargement SoftLayer ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://downloads.service.softlayer.com/citrix/xen/){: new_window} et sélectionnez le fichier de package RPM de l'outil vhd-util : vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm   

   Si vous ne pouvez pas télécharger le fichier de package RPM directement dans votre système CentOS, téléchargez le fichier sur le poste de travail sur lequel vous travaillez actuellement. Vous pouvez ensuite l'envoyer par téléchargement vers votre système CentOS à l'aide de la commande de copie sécurisée (scp). Si vous utilisez une instance de serveur
virtuel dans {{site.data.keyword.cloud_notm}}, utilisez l'adresse IP publique du système pour cet envoi par téléchargement à l'aide de la commande suivante : 

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

3. Installez le package RPM à l'aide de la commande suivante : 

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. Identifiez la clé de chiffrement AES dont vous avez besoin pour chiffrer et déchiffrer votre image de disque et écrivez-la dans un fichier de clés. La clé de chiffrement AES est la même clé de chiffrement de données que celle que vous avez encapsulée avec la clé racine du client Key Protect
dans [Préparation de votre environnement](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment). La clé qui est écrite dans les fichiers de clés doit être désencapsulée et non codée.  

   La clé de données n'étant pas codée en base64 dans les fichiers de clés, vous ne pouvez pas imprimer, ni afficher le contenu du fichier de clés à partir de la ligne de commande en utilisant des caractères ASCII standard.
   {: tip}

   Utilisez la commande suivante pour créer des fichiers de clés avec une clé de chiffrement **AES 256 bits** ou **AES 512 bits** :  
   
   ```
   echo <clé_données> | base64 -d - > <nom_fichier de clés>
   ```
   {: pre} 

   Exemple de commande :

   ```
   echo Nrfen98EpMxF2B+wdgLfagzrqvgUZfMK4vL2T0NsT20ihrsNC9pUUHtizF6218pze8RLCgQ6kwxuE58IWLzgDA== | base64 -d - > aes512.dek
   ```
   {: screen}

5. Utilisez la commande suivante pour vérifier les fichiers de clés que vous avez créés lors de l'étape précédente : 

   ```
   vhd-util key -C -k <nom_fichier de clés>
   ```
   {: pre}

   Exemple de commande avec sortie : 

   ```
   vhd-util key -C -k aes512.dek
   vhd_util_read_key: using keyfile aes512.dek, Size (bytes) 64
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

   La première ligne de la sortie provenant de l'exemple de commande précédent indique que le fichier de clés nommé `aes512.dek` contient une clé de 64 octets, tandis que les numéros répertoriés sur la deuxième ligne sont les hachages SHA256 ou les hachages de sécurité pour les clés de chiffrement respectives. La sortie pour les fichiers contenant une clé de chiffrement AES 256 bits correspond à une clés de 32 octets.
   {: tip} 

6. Utilisez la commande suivante pour créer des copies chiffrées de vos fichiers VHD. `vhd_cible` représente le nom du fichier contenant la version chiffrée de `vhd_source`. 

   ```
   vhd-util copy -n <vhd_source> -N <vhd_cible> -k <nom_fichier de clés>
   ```
   {: pre}    

   Exemple de commande :

   ```
   vhd-util copy -n debian8-ne.vhd -N debian8-aes512.vhd -k aes512.dek
   ```
   {: screen}

7. Vérifiez que les fichiers VHD sont chiffrés à l'aide de la commande suivante : 

   ```
   vhd-util key -p -n <nom_fichier_vhd>
   ```
   {: pre}

   Exemple de commande avec sortie : 

   ```
   vhd-util key -p -n debian8-aes512.vhd
   0000000000000000000000000000000000000000000000000000000000000000
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

Si le fichier VHD est chiffré, la sortie comporte deux valeurs de hachage, comme illustré dans l'exemple précédent. Le premier hachage est composé uniquement de zéros. Le deuxième est le hachage SHA256 que la clé de chiffrement AES utilise pour chiffrer et déchiffrer le fichier VHD. Assurez-vous que les hachages SHA256 pour les fichiers VHD sont identiques aux hachages indiqués à l'**Etape 5**. 

L'exemple de commande figurant à l'**Etape 7** crée un nouveau fichier VHD chiffré, appelé "debian8-aes512.vhd". Il est chiffré avec la clé de chiffrement AES 512 bits provenant du fichier de clés nommé "aes512.dek". Le hachage SHA256 pour son chiffrement est `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda`. 
