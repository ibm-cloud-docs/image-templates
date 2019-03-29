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


# Preparazione e importazione delle immagini
{: #preparing-and-importing-images}

La schermata Template dell'immagine nel {{site.data.keyword.slportal_full}} consente agli utenti di importare un'immagine da un'istanza del servizio [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage). Una volta caricata un'immagine in un bucket in un'istanza del servizio {{site.data.keyword.cos_full_notm}}, puoi importarla nel repository dei template dell'immagine nel {{site.data.keyword.slportal}}.
{:shortdesc}

Devi disporre di un account aggiornato per importare le immagini da {{site.data.keyword.cos_full_notm}}. Per ulteriori informazioni, vedi [Passaggio all'ID IBM e collegamento degli account](/docs/account?topic=account-unifyingaccounts).
{: tip}

Una volta importate le immagini come un template dell'immagine, possono essere utilizzate per eseguire il provisioning o per avviare un server virtuale esistente. Le immagini che vengono importate da un'istanza del servizio {{site.data.keyword.cos_full_notm}} possono essere VHD o ISO personalizzati. Le importazioni VHD sono limitate ai seguenti sistemi operativi a 64 bit:  

* CentOS 6 e 7
* RedHat Enterprise Linux 6 e 7
* Ubuntu 14.04 e 16.04
* Microsoft Server Standard 2012, R2 2012 e 2016

Le importazioni VHD sono limitate a dischi da 100 GB. I VHD devono essere denominati in base al seguente esempio: filename.vhd-0.vhd.

## Conversione delle immagini in VHD
{: #convert-to-vhd}

Il formato VHD è l'unico formato immagine supportato per i server virtuali. Per convertire le immagini in VHD, utilizza le seguenti informazioni:

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

    1. Scarica l'ISO di XenServer da Citrix: [https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html)

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

Per ulteriori informazioni sulle immagini abilitate a cloud-init, vedi [Provisioning con un'immagine abilitata a cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image).

## Caricamento di un'immagine in {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

Quando la tua immagine è pronta, puoi caricarla in {{site.data.keyword.cos_full_notm}}. Assicurati di utilizzare un bucket in un'[ubicazione regionale](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#select-regions-and-endpoints).

1. In {{site.data.keyword.cos_full_notm}}, passa al tuo bucket e fai clic su **Aggiungi oggetti** per [caricare](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data) l'immagine.
2. Utilizza il plug-in di trasferimento ad alta velocità [Aspera](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#Aspera-high-speed-transfer) per velocità di caricamento più rapide della tua immagine.

Puoi utilizzare COS SDK con Aspera per inizializzare il trasferimento ad alta velocità all'interno delle tue applicazioni personalizzate quando si utilizza Java, Python o NodeJS. Per ulteriori informazioni, vedi [Using Libraries and SDKs](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#sdk).
{: tip}


## Importazione di un'immagine da IBM Cloud Object Storage
{: #import-icos}

Completa i seguenti passi per importare un'immagine da {{site.data.keyword.cos_full_notm}}.

1. Nel [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/), accedi alla pagina **Template dell'immagine** selezionando **Dispositivi > Gestisci > Immagini**.
2. Fai clic sulla scheda **Importa immagine da IBM COS** per aprire lo strumento di importazione.
3. Completa i campi obbligatori (vedi Tabella 1).
4. Una volta completata l'importazione da {{site.data.keyword.cos_full_notm}}, l'immagine compare nella pagina Template dell'immagine.

| Campo | Valore |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Seleziona l'istanza del servizio {{site.data.keyword.cos_full_notm}} in cui è memorizzata l'immagine che desideri importare. |
| Ubicazione | Seleziona la regione geografica specifica in cui è memorizzata la tua immagine. Puoi importare le immagini nelle seguenti regioni e data center associati: Stati Uniti Sud (DAL13), Stati Uniti Est (WDC07), UE-Gran Bretagna (LON02), UE-Germania (FRA02), AP-Giappone (TOK02). Una volta importata l'immagine in uno dei data center elencati, puoi spostarla in un altro data center. |
| Bucket | Seleziona il bucket {{site.data.keyword.cos_full_notm}} in cui è memorizzata la tua immagine. Sono validi solo i bucket presenti nell'ubicazione regionale che hai selezionato. Riceverai un messaggio di errore se selezioni un bucket che non è presente nell'ubicazione selezionata.|
| File immagine | Seleziona il file immagine nell'istanza del servizio {{site.data.keyword.cos_full_notm}} che desideri importare. I tipi di file supportati sono VHD, ISO e RAW. Se stai importando un'immagine crittografata, l'immagine deve avere un formato file RAW ed essere crittografata con la crittografia del disco LUKS. |
| Nome immagine | Specifica un nome descrittivo per la tua immagine. Si tratta dell'immagine che utilizzerai per eseguire il provisioning delle istanze del server virtuale. |
| Chiave API | Specifica la chiave API che ti concede l'accesso alla tua istanza del servizio {{site.data.keyword.cos_full_notm}}. Quando importi un'immagine crittografata, anche la chiave API deve avere l'accesso a Key Protect. La chiave API è disponibile solo per essere copiata o scaricata al momento della creazione. Se la chiave API viene persa, dovrai crearne una nuova. Per ulteriori informazioni, vedi [Gestione delle chiavi API](/docs/iam?topic=iam-manapikey). |
| Sistema operativo | Seleziona il sistema operativo incluso nella tua immagine. Per le immagini crittografate, solo i sistemi operativi Linux costituiscono selezioni valide. |
| Cloud-init | Se l'immagine che stai importando è abilitata a cloud-init, seleziona questa casella di spunta. Se stai importando un'immagine che ha un sistema operativo Windows abilitato a cloud-init e selezioni questa opzione, non puoi specificare anche **La tua licenza**. Se stai importando un'immagine crittografata, questa opzione è selezionata per impostazione predefinita e non è modificabile in quanto la tua immagine crittografata deve essere abilitata a cloud-init. |
| La tua licenza | Se intendi fornire la tua licenza di sistema operativo, seleziona questa casella di spunta. Se stai importando un'immagine con un sistema operativo Windows, puoi selezionare questa opzione se intendi utilizzare l'immagine per eseguire il provisioning di [istante host dedicate](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances). Se la tua versione del sistema operativo Windows non supporta l'utilizzo della tua licenza, questa opzione è disabilitata. Per le immagini Windows, non puoi selezionare Cloud Init se specifichi che utilizzerai la tua licenza. Se stai importando un'immagine crittografata con Red Hat Enterprise Linux come sistema operativo, questa opzione è selezionata per impostazione predefinita e non è modificabile in quanto la tua immagine crittografata deve includere la sua licenza di sistema operativo. |
| Modalità di avvio | Seleziona la modalità di avvio della tua immagine. Se è impostata una modalità di avvio predefinita per il sistema operativo che hai specificato, la modalità di avvio viene selezionata automaticamente qui. |
| Note | Aggiungi le note relative all'immagine che ritieni possano essere utili agli utenti. |
| Crittografia | La selezione per questa casella di spunta viene determinata dal tipo di file dell'immagine che hai selezionato per l'importazione. Le immagini VHD e ISO indicano che il file immagine non è crittografato. Quindi, la casella di spunta non è selezionata per le immagini VHD e ISO. Un file immagine RAW indica che l'immagine è un'immagine crittografata. Se viene specificato un file immagine RAW, questa casella di spunta è selezionata per impostazione predefinita e non è modificabile. |
{: caption="Tabella 1. Valori per l'importazione di un'immagine da IBM Cloud Object Storage" caption-side="top"}

La seguente tabella mostra i campi aggiuntivi applicabili solo all'importazione delle immagini crittografate. Per ulteriori informazioni sulle immagini crittografate, vedi [Utilizzo della crittografia End to End (E2E) per eseguire il provisioning di un'istanza crittografata](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

Per importare un'immagine crittografata, il tuo account deve avere accesso alla funzione di crittografia End to End (E2E). Per abilitare il tuo account per la crittografia E2E, contatta il supporto.
{: tip}

| Campo | Valore |
| ----- | ----- |
| ID istanza del servizio {{site.data.keyword.keymanagementserviceshort}} | Quando importi un'immagine crittografata, la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}} deve trovarsi nella stessa regione del tuo bucket {{site.data.keyword.cos_full_notm}}. Puoi utilizzare la CLI {{site.data.keyword.cloud_notm}} per trovare il tuo ID istanza {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi [Richiamo del tuo ID dell'istanza](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID). |
| Chiave di crittografia dei dati impacchettata (WDEK) | Quando importi un'immagine crittografata, specifica il testo crittografato associato alla chiave di crittografia dei dati che hai utilizzato per crittografare la tua immagine. Per ulteriori informazioni, vedi [Impacchettamento delle chiavi utilizzando l'API](/docs/services/key-protect?topic=key-protect-wrap-keys#api). |
| ID chiave root | Quando importi un'immagine crittografata, specifica l'ID della chiave root che è stato utilizzato per impacchettare la chiave di crittografia dei dati. Per ulteriori informazioni, vedi [Visualizzazione delle chiavi](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
{: caption="Tabella 2. Valori per l'importazione di un'immagine crittografata da IBM Cloud Object Storage" caption-side="top"}

## Passi successivi

Una volta avviata l'importazione, il sistema individua il file immagine nell'istanza di servizio {{site.data.keyword.cos_full_notm}} nel bucket specificato. Il file immagine viene importato come un template dell'immagine che è quindi accessibile
nella pagina Template dell'immagine. Una volta completata l'importazione, l'immagine può essere utilizzata per ordinare un nuovo dispositivo o avviarne uno esistente.
Inoltre, il template dell'immagine può essere eliminato in qualsiasi momento. I tempi di importazione dell'immagine variano a seconda della dimensione del file, ma generalmente durano da diversi minuti a un'ora.

Una volta importata un'immagine nel repository dei template dell'immagine, puoi eliminarla da {{site.data.keyword.cos_full_notm}}. Puoi continuare ad accedere al template dell'immagine dalla pagina **Template dell'immagine** e utilizzarlo per eseguire il provisioning delle istanze del server virtuale.
