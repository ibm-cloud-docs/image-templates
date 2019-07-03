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


# Preparazione e importazione delle immagini
{: #preparing-and-importing-images}

La schermata Template dell'immagine nel {{site.data.keyword.slportal_full}} ti consente di importare un'immagine da un'istanza del servizio [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about). Puoi importare immagini che hanno formato VHD (Virtual Hard Disk) o VMDK (Virtual Machine Disk). Dopo l'importazione, le immagini VMDK vengono convertite in VHD. Una volta caricata un'immagine in un bucket in un'istanza del servizio {{site.data.keyword.cos_full_notm}}, puoi importarla nel repository dei template dell'immagine nel {{site.data.keyword.slportal}}.
{:shortdesc}

Devi disporre di un account aggiornato per importare le immagini da {{site.data.keyword.cos_full_notm}}. Per ulteriori informazioni, vedi [Passaggio all'ID IBM e collegamento degli account](/docs/account/softlayerlink.html).
{: tip}

Devi disporre di un'[istanza IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision#provision-account) ordinata tramite la console {{site.data.keyword.cloud_notm}} (cloud.ibm.com) per utilizzare questa funzione di importazione.  IBM Cloud Object Storage da control.softlayer.com non è supportato.
{: important}

Una volta importate le immagini come un template dell'immagine, possono essere utilizzate per eseguire il provisioning o per avviare un server virtuale esistente. Le immagini importate da un'istanza del servizio {{site.data.keyword.cos_full_notm}} possono essere VHD, VMDK o ISO personalizzati. Le importazioni VHD e VMDK sono limitate ai seguenti sistemi operativi a 64 bit:  

* CentOS 6 e 7
* Microsoft Server Standard 2012, R2 2012 e 2016
* RedHat Enterprise Linux 6 e 7
* Ubuntu 14.04 e 16.04

Le importazioni sono limitate a dischi da 100 GB. Le immagini devono essere denominate in base al seguente esempio: filename.vhd-0.vhd oppure filename.vmdk-0.vmdk

## Conversione delle immagini in VHD
{: #convert-to-vhd}

I formati VHD e VMDK sono gli unici formati immagine supportati per i server virtuali. Per convertire le immagini in VHD da qualsiasi formato diverso da VMDK, utilizza le seguenti informazioni:

* È necessario Qemu-img 2.7.0 o più recente
* Converti l'immagine con il seguente comando:

  ```
  qemu-img convert -f <image format> <image name> -O vpc -o force_size <image name>
  ```
  {: pre}

* Comando di esempio:

  ```
  qemu-img convert -f qcow2 test -O vpc -o force_size test
  ```
  {: pre}

Per ulteriori informazioni, vedi [Converting image formats ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats)
nella documentazione QEMU.

## Template ISO
{: #iso-templates}

Per caricare un Template ISO in una VSI, possono essere utilizzati solo sistemi operativi supportati {{site.data.keyword.BluSoftlayer_notm}}. Un elenco
dei sistemi operativi supportati è disponibile qui: [https://www.ibm.com/cloud/server-software ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/server-software)

Gli ISO importati utilizzando questo strumento devono essere avviabili affinché l'immagine sia idonea per l'importazione.

## Configurazione di un'immagine per {{site.data.keyword.virtualmachinesshort}}
{: #config-image-vsi}

Per assicurarti che un'immagine possa essere distribuita correttamente nell'ambiente dell'infrastruttura {{site.data.keyword.BluSoftlayer_notm}}, le immagini del server virtuale devono essere configurate con le seguenti specifiche:

* ***/boot*** deve essere la prima partizione
* ***/boot*** e ***/*** devono essere il file system ext3 o ext4
* ***/etc***  e /root devono trovarsi nella stessa partizione di ***/***
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** per montare il disco swap collegato al sistema
* wget deve essere installato
* Devono essere installati gli strumenti Xen xe-guest-utilities più recenti. Completa i seguenti passi:

    1. Scarica l'ISO di XenServer da Citrix: [https://www.citrix.com/downloads/citrix-hypervisor/![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.citrix.com/downloads/citrix-hypervisor/)

    2. Monta l'ISO eseguendo il seguente comando:

        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}

    3. Individua l'RPM per l'ISO eseguendo il seguente comando:

        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}

    4. L'output elenca un RPM simile al seguente:

        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}

     5. Puoi copiare l'RPM e poi estrarre l'ISO per xentools. Esegui il seguente comando per creare una struttura di directory che ospiti l'ISO:

        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}

     6. Dalla struttura di directory risultante, puoi montare l'ISO *guest-tools* e individuare *rpm/debs* per installare xentools. Esamina la struttura di directory di esempio riportata di seguito:

        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}

Per ulteriori informazioni sulle immagini abilitate a cloud-init, vedi [Provisioning con un'immagine abilitata a cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#provisioning-with-a-cloud-init-enabled-image).

## Preparazione all'importazione di un'immagine crittografata
{: #preparing-to-import-an-encrypted-image}

Se intendi importare un'immagine VHD crittografata con la tua chiave di crittografia dei dati, assicurati di completare i prerequisiti e le istruzioni per la crittografia descritti in [Utilizzo della crittografia End to End (E2E) per eseguire il provisioning di un'istanza crittografata](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

Devi utilizzare lo strumento vhd-util per crittografare la tua immagine che deve essere in formato VHD. Per ulteriori informazioni, vedi [Crittografia delle immagini VHD](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image). 
{: important}

## Caricamento di un'immagine in {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

Quando la tua immagine è pronta, puoi caricarla in {{site.data.keyword.cos_full_notm}}. Assicurati di utilizzare un bucket in un'[ubicazione regionale](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints).

1. In {{site.data.keyword.cos_full_notm}}, passa al tuo bucket e fai clic su **Aggiungi oggetti** per [caricare](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) l'immagine.
2. Utilizza il plug-in di trasferimento ad alta velocità [Aspera](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera) per velocità di caricamento più rapide della tua immagine.

Puoi utilizzare COS SDK con Aspera per inizializzare il trasferimento ad alta velocità all'interno delle tue applicazioni personalizzate quando si utilizza Java, Python o NodeJS. Per ulteriori informazioni, vedi [Using Libraries and SDKs](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera#aspera-sdk).
{: tip}


## Importazione di un'immagine da IBM Cloud Object Storage
{: #import-icos}

Completa i seguenti passi per importare un'immagine da {{site.data.keyword.cos_full_notm}}.

1. Passa al menu del dispositivo e assicurati di disporre delle autorizzazioni di account corrette per completare le attività.

   * Passa al menu del dispositivo della tua console. Per ulteriori informazioni, vedi [Passaggio ai dispositivi](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
   * Assicurati di disporre delle autorizzazioni di account e dell'accesso al dispositivo necessari. Solo il proprietario dell'account o un utente con l'autorizzazione dell'infrastruttura classica **Gestisci utenti** possono modificare le autorizzazioni.

   Per ulteriori informazioni sulle autorizzazioni, vedi [Autorizzazioni dell'infrastruttura classica](/docs/iam?topic=iam-    infrapermission#infrapermission) e [Gestione accesso dispositivo](/docs/vsi?topic=virtual-servers-managing-device-access).

   Se sta importando un'immagine crittografata, devi utilizzare la console {{site.data.keyword.cloud_notm}}.
   {: important}
2. Accedi alla pagina **Template dell'immagine** selezionando **Dispositivi > Gestisci > Immagini**.
3. Fai clic sulla scheda **Importa immagine da IBM COS** per aprire lo strumento di importazione.
4. Completa i campi obbligatori (vedi Tabella 1).
5. Una volta completata l'importazione da {{site.data.keyword.cos_full_notm}}, l'immagine compare nella pagina Template dell'immagine.

| Campo | Valore |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Seleziona l'istanza del servizio {{site.data.keyword.cos_full_notm}} in cui è memorizzata l'immagine che desideri importare. |
| Ubicazione | Seleziona la regione geografica specifica in cui è memorizzata la tua immagine. Puoi importare le immagini nelle seguenti regioni e data center associati: Stati Uniti Sud (DAL13), Stati Uniti Est (WDC07), UE-Gran Bretagna (LON02), UE-Germania (FRA02), AP-Giappone. Una volta importata l'immagine in uno dei data center elencati, puoi spostarla in un altro data center. |
| Bucket | Seleziona il bucket {{site.data.keyword.cos_full_notm}} in cui è memorizzata la tua immagine. Sono validi solo i bucket presenti nell'ubicazione regionale che hai selezionato. Riceverai un messaggio di errore se selezioni un bucket che non è presente nell'ubicazione selezionata.|
| File immagine | Seleziona il file immagine nell'istanza del servizio {{site.data.keyword.cos_full_notm}} che desideri importare. I tipi di file supportati sono VHD (Virtual Hard Disk), VMDK (Virtual Machine Disk) e ISO. Se stai importando un'immagine crittografata, l'immagine deve essere in formato file VHD e crittografata con lo strumento vhd-util. |
| Nome immagine | Specifica un nome descrittivo per la tua immagine. Si tratta dell'immagine che utilizzerai per eseguire il provisioning delle istanze del server virtuale. |
| Chiave API | Specifica la chiave API che ti concede l'accesso alla tua istanza del servizio {{site.data.keyword.cos_full_notm}}. Quando importi un'immagine crittografata, anche la chiave API deve avere l'accesso all'istanza del servizio di gestione chiavi. La chiave API è disponibile solo per essere copiata o scaricata al momento della creazione. Se la chiave API viene persa, dovrai crearne una nuova. Per ulteriori informazioni, vedi [Gestione delle chiavi API](/docs/iam?topic=iam-manapikey#manapikey). |
| Sistema operativo | Seleziona il sistema operativo incluso nella tua immagine. |
| Cloud-init | Se l'immagine che stai importando è abilitata a cloud-init, seleziona questa casella di spunta. Se stai importando un'immagine che ha un sistema operativo Windows abilitato a cloud-init e selezioni questa opzione, non puoi selezionare contemporaneamente **La tua licenza**. Se stai importando un'immagine crittografata, questa opzione è selezionata per impostazione predefinita e non è modificabile in quanto la tua immagine crittografata deve essere abilitata a cloud-init. |
| La tua licenza | Se intendi fornire la tua licenza di sistema operativo, seleziona questa casella di spunta. Se stai importando un'immagine con un sistema operativo Windows, puoi selezionare questa opzione se intendi utilizzare l'immagine per eseguire il provisioning di [istante host dedicate](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances). Se la tua versione del sistema operativo Windows non supporta l'utilizzo della tua licenza, questa opzione è disabilitata. Per le immagini Windows, non puoi selezionare Cloud Init se specifichi che utilizzerai la tua licenza. Se stai importando un'immagine crittografata con Red Hat Enterprise Linux come sistema operativo, questa opzione è selezionata per impostazione predefinita e non è modificabile in quanto la tua immagine crittografata deve includere la sua licenza di sistema operativo. |
| Modalità di avvio | Seleziona la modalità di avvio della tua immagine. Se è impostata una modalità di avvio predefinita per il sistema operativo che hai specificato, la modalità di avvio viene selezionata automaticamente qui. |
| Note | Aggiungi le note relative all'immagine che ritieni possano essere utili agli utenti. |
| Crittografia | Se stai importando un'immagine crittografata con la tua chiave di crittografia dei dati utilizzando lo strumento vhd-util, seleziona questa casella di spunta. |
{: caption="Tabella 1. Valori per l'importazione di un'immagine da IBM Cloud Object Storage" caption-side="top"}

La seguente tabella mostra i campi aggiuntivi applicabili solo all'importazione delle immagini crittografate. Per ulteriori informazioni sulle immagini crittografate, vedi [Utilizzo della crittografia End to End (E2E) per eseguire il provisioning di un'istanza crittografata](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

<!--
To import an encrypted image, your account must have access to the End to End (E2E) Encryption feature. To enable your account for E2E Encryption, please contact [Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
{: tip}
-->

| Campo | Valore |
| ----- | ----- |
| Chiave di crittografia dei dati impacchettata (WDEK) | Quando importi un'immagine crittografata, specifica il testo crittografato associato alla chiave di crittografia dei dati che hai utilizzato per crittografare la tua immagine. Per ulteriori informazioni, vedi [Impacchettamento delle chiavi utilizzando l'API](/docs/services/key-protect?topic=key-protect-wrap-keys#api). |
| Istanza del servizio di gestione chiavi | Seleziona un'istanza del servizio di gestione chiavi nel tuo account dall'elenco a discesa. L'istanza del servizio di gestione chiavi deve includere la chiave root del cliente che hai utilizzato per impacchettare la tua chiave di crittografia dei dati. |
| Nome chiave | Seleziona il nome della chiave root all'interno dell'istanza del servizio di gestione chiavi che hai utilizzato per impacchettare la tua chiave di crittografia dei dati. Per ulteriori informazioni, vedi [Visualizzazione delle chiavi](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
{: caption="Tabella 2. Valori per l'importazione di un'immagine crittografata da IBM Cloud Object Storage" caption-side="top"}

## Passi successivi

Una volta avviata l'importazione, il sistema individua il file immagine nell'istanza di servizio {{site.data.keyword.cos_full_notm}} nel bucket specificato. Il file immagine viene importato come un template dell'immagine che è quindi accessibile
nella pagina Template dell'immagine. Una volta completata l'importazione, l'immagine può essere utilizzata per ordinare un nuovo dispositivo o avviarne uno esistente.
Inoltre, il template dell'immagine può essere eliminato in qualsiasi momento. I tempi di importazione dell'immagine variano a seconda della dimensione del file, ma generalmente durano da diversi minuti a un'ora.

Una volta importata un'immagine nel repository dei template dell'immagine, puoi eliminarla da {{site.data.keyword.cos_full_notm}}. Puoi continuare ad accedere al template dell'immagine dalla tua pagina **Template dell'immagine** e utilizzarlo per eseguire il provisioning delle istanze del server virtuale.
