---

copyright:
  years: 2019
lastupdated: "2019-04-29"

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


# Crittografia delle immagini VHD 
{: #create-encrypted-image}

Per utilizzare la funzione di crittografia E2E, devi crittografare la tua immagine VHD con lo strumento vhd-util prima di importarla nel template dell'immagine per eseguire il provisioning delle istanze crittografate. Sono supportati due livelli di crittografia AES: AES a 256 bit e AES a 512 bit.
{:shortdesc}

## Requisiti dell'immagine VHD crittografata
{: #encrypted-image-reqs}

Le immagini VHD crittografate devono soddisfare i seguenti requisiti:

* Formato VHD.
* Compatibilità con l'ambiente dell'infrastruttura {{site.data.keyword.cloud}} Console.
* Eseguito il provisioning con un [sistema operativo supportato](/docs/infrastructure/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images).
* Abilitato a Cloud-init.
* Eseguita crittografia con [lo strumento vhd-util](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool).

## Crittografia della tua immagine VHD
{: #vhd-util-tool}

Segui questi passi per creare la tua immagine VHD crittografata:

1. Seleziona un sistema CentOS su cui è in esecuzione la versione 7 o successiva per crittografare la tua immagine del disco virtuale (file VHD) per {{site.data.keyword.cloud_notm}}. Se non hai accesso all'hardware fisico con CentOS installato, puoi eseguire il provisioning di un'istanza del server virtuale con CentOS 7 in {{site.data.keyword.cloud_notm}} utilizzando un host pubblico o dedicato. Il sistema CentOS utilizzato per crittografare i file VHD non ha bisogno di essere crittografato.

2. Accedi al tuo sistema CentOS e collegati alla tua VPN del cliente, quindi [vai al sito di download di SoftLayer ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://downloads.service.softlayer.com/citrix/xen/){: new_window} e seleziona il file del pacchetto RPM dello strumento vhd-util: vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm   

   Se non puoi scaricare il file del pacchetto RPM direttamente nel tuo sistema CentOS, scarica il file nella workstation su cui stai attualmente lavorando. Puoi quindi caricarlo nel tuo sistema CentOS utilizzando il comando secure copy (scp). Se stai utilizzando un'istanza del server virtuale in {{site.data.keyword.cloud_notm}}, utilizza l'indirizzo IP pubblico del sistema per questo caricamento utilizzando il seguente comando.

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

3. Installa RPM utilizzando il seguente comando:

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. Identifica la chiave di crittografia AES di cui hai bisogno per crittografare e decrittografare la tua immagine del disco e scrivila in un keyfile. Questa chiave di crittografia AES è la stessa chiave di crittografia dei dati che hai impacchettato con la chiave root del cliente fornita dal servizio di gestione delle chiavi in [Preparazione del tuo ambiente](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment). Il materiale della chiave scritto nei keyfile deve essere spacchettato e non codificato. 

   Poiché data_key non è codificato base64 all'interno dei keyfile, non puoi stampare o visualizzare il contenuto del keyfile dalla riga di comando utilizzando i caratteri ASCII standard. 
   {: tip}

   Utilizza il seguente comando per creare i keyfile con una chiave di crittografia **AES a 256 bit** o **AES a 512 bit**: 
   
   ```
   echo <data_key> | base64 -d - > <keyfile_name>
   ```
   {: pre} 

   Comando di esempio:

   ```
   echo Nrfen98EpMxF2B+wdgLfagzrqvgUZfMK4vL2T0NsT20ihrsNC9pUUHtizF6218pze8RLCgQ6kwxuE58IWLzgDA== | base64 -d - > aes512.dek
   ```
   {: screen}

5. Utilizza il seguente comando per verificare i keyfile che hai creato nel passo precedente:

   ```
   vhd-util key -C -k <keyfile_name>
   ```
   {: pre}

   Comando di esempio con output:

   ```
   vhd-util key -C -k aes512.dek
   vhd_util_read_key: using keyfile aes512.dek, Size (bytes) 64
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

   La prima riga dell'output dal comando di esempio precedente indica che il keyfile denominato `aes512.dek` contiene una chiave a 64 byte, mentre i numeri elencati nella seconda riga sono hash SHA256 o hash di sicurezza per le rispettive chiavi di crittografia. L'output per i file che contengono una chiave di crittografia AES a 256 bit indicherà una chiave a 32 byte.
   {: tip} 

6. Utilizza il seguente comando per creare copie crittografate dei tuoi file VHD. `target_vhd` rappresenta il nome del file che contiene la versione crittografata di `source_vhd`.

   ```
   vhd-util copy -n <source_vhd> -N <target_vhd> -k <keyfile_name>
   ```
   {: pre}    

   Comando di esempio:

   ```
   vhd-util copy -n debian8-ne.vhd -N debian8-aes512.vhd -k aes512.dek
   ```
   {: screen}

7. Verifica che i file VHD vengano crittografati utilizzando il seguente comando.

   ```
   vhd-util key -p -n <vhd_filename>
   ```
   {: pre}

   Comando di esempio con output:

   ```
   vhd-util key -p -n debian8-aes512.vhd
   0000000000000000000000000000000000000000000000000000000000000000
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

Se il file VHD è crittografato, vedrai due valori hash nell'output come mostrato nell'esempio precedente. Il primo hash sono tutti zero. Il secondo hash è l'hash SHA256 utilizzato dalla chiave di crittografia AES per crittografare e decrittografare VHD. Assicurati che gli hash SHA256 per i file VHD siano gli stessi di quelli mostrati nel **Passo 5**.

Il comando di esempio nel **Passo 7** crea un nuovo file VHD crittografato denominato, “debian8-aes512.vhd”. Viene crittografato con la chiave di crittografia AES a 512 bit proveniente dal keyfile denominato “aes512.dek”. L'hash SHA256 per la sua crittografia è                                  `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda`.
