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


# Utilizzo della crittografia End to End (E2E) per eseguire il provisioning di un'istanza crittografata
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

La funzione di crittografia End to End (E2E) ti consente di utilizzare la tua immagine di sistema operativo abilitata a cloud-init, crittografata, che hai crittografato utilizzando una chiave di crittografia dei dati che possiedi e controlli. Una volta completate alcune parti di configurazione, puoi importare la tua immagine crittografata nel repository dei template dell'immagine e utilizzarla per eseguire il provisioning delle istanze del server virtuale crittografato. La crittografia E2E fornisce la crittografia per i dati inattivi per l'archiviazione associata alle istanze del server virtuale di cui è stato eseguito il provisioning.
{:shortdesc}

La crittografia E2E racchiude diversi componenti {{site.data.keyword.cloud}} per fornire una soluzione sicura per le tue informazioni critiche.

* Un servizio di gestione delle chiavi IBM come {{site.data.keyword.keymanagementservicelong_notm}} o {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} per proteggere le tue chiavi di crittografia (vedi la Tabella 1).
* IBM IAM ({{site.data.keyword.iamshort}}) consente al servizio Cloud Block Storage di accedere al tuo sistema di gestione delle chiavi e alla tua chiave root che viene utilizzata per impacchettare la tua chiave di crittografia dei dati.
* {{site.data.keyword.cos_full_notm}} memorizza in modo sicuro la tua immagine crittografata quando la carichi.
* Nella console {{site.data.keyword.cloud_notm}} puoi importare la tua immagine crittografata e creare un template dell'immagine.
* Con un template dell'immagine crittografata disponibile nell'ambiente dell'infrastruttura della console {{site.data.keyword.cloud_notm}}, puoi eseguire il provisioning delle istanze del server virtuale crittografato.
* Infine, puoi controllare gli eventi associati ai tuoi server virtuali crittografati mediante l'[Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).

## Servizi di gestione delle chiavi di crittografia

{{site.data.keyword.keymanagementserviceshort}} e {{site.data.keyword.hscrypto}} (ora disponibili in specifiche [regioni](/docs/services/hs-crypto?topic=hs-crypto-regions#regions)) utilizzano un'API di provider di chiavi comune per fornire un approccio congruente per la gestione delle chiavi di crittografia. Dietro le quinte, i data center {{site.data.keyword.cloud_notm}} forniscono un modulo di sicurezza hardware (HSM, hardware security module) dedicato per proteggere le tue chiavi.  Puoi scegliere dalle seguenti opzioni: 

| Servizio di gestione delle chiavi | Certificazione di crittografia HSM |
| ----- | ----- |
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/concepts?topic=key-protect-getting-started-tutorial#getting-started-tutorial) | Conformità FIPS 140-2 *Livello 2* |
| [{{site.data.keyword.hscrypto}}](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) | Conformità FIPS 140-2 *Livello 4* |
{: caption="Tabella 1. Opzioni del servizio di gestione delle chiavi disponibili" caption-side="top"}

## Preparazione del tuo ambiente

1. Devi disporre di un account aggiornato per utilizzare la crittografia E2E per i server virtuali. Per ulteriori informazioni, vedi [Passaggio all'ID IBM e collegamento degli account](/docs/account/softlayerlink.html).
2. Utilizza il tuo servizio di gestione delle chiavi per creare e gestire le chiavi.  La seguente procedura di esempio è specifica per {{site.data.keyword.keymanagementserviceshort}} ma il flusso generale si applica anche a {{site.data.keyword.hscrypto}}. Se stai utilizzando {{site.data.keyword.hscrypto}}, vedi la [documentazione](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) per tale servizio per le istruzioni corrispondenti.
      1. Esegui il provisioning del servizio [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision).
      2. [Crea](/docs/services/key-protect?topic=key-protect-create-root-keys) o [importa](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) una chiave root (CRK) in {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Facoltativo**: se vuoi, puoi [creare](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) o [importare](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) una chiave standard per la decrittografia.      
      4. [Configura il plug-in della CLI {{site.data.keyword.cloud_notm}} Key Protect](/docs/services/key-protect?topic=key-protect-set-up-cli) in modo da potere [impacchettare la chiave di crittografia dei dati](/docs/services/key-protect?topic=key-protect-cli-reference#kp-wrap) che intendi utilizzare per crittografare la tua immagine VHD con la chiave root. Hai bisogno del testo crittografato associato alla chiave di crittografia dei dati impacchettata (WDEK) quando importi la tua immagine crittografata nella console {{site.data.keyword.cloud_notm}}.  
         
       Key Protect non salva i dati di autenticazione aggiuntivi (AAD, additional authentication data) ma puoi comunque utilizzare gli AAD per proteggere ulteriormente una chiave con un massimo di 255 stringhe, ciascuna delimitata da una virgola e contenente fino a 255 caratteri. Se fornisci gli AAD per l'impacchettamento della chiave, salva i dati in un'ubicazione protetta per assicurati che tu possa accedere e fornire le stesse AAD alle future richieste di spacchettamento delle chiavi.
       {: tip}
      
3. Da IBM IAM ({{site.data.keyword.iamshort}}), [autorizza l'accesso](/docs/iam?topic=iam-serviceauth#create-auth) tra il tuo **Cloud Block Storage** (servizio di origine) e il tuo **Key Management Service** (servizio di destinazione). Se importi le immagini crittografate da {{site.data.keyword.cos_full_notm}} devi disporre di una [politica di accesso definita](/docs/iam?topic=iam-userroles#userroles) per il tuo servizio di gestione delle chiavi in IAM.
4. In IBM Cloud Console, crea un'istanza di {{site.data.keyword.cos_full_notm}} e crea un bucket per memorizzare i dati. Per ulteriori informazioni, vedi l'[Esercitazione introduttiva per {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)
      1. Crea l'istanza {{site.data.keyword.cos_full_notm}} nella regione in cui è stato eseguito il provisioning del tuo servizio di gestione delle chiavi.
      2. Quando crei il bucket, l'impostazione **Resilienza** deve essere _Regionale_.
      3. Facoltativamente, quando crei il bucket, puoi [crittografarlo con la tua chiave](/docs/services/cloud-object-storage?topic=cloud-object-storage-encryption#encryption-kp).   

## Preparazione delle tue immagini crittografate

1. Seleziona un'immagine non crittografata che funzioni nell'ambiente dell'infrastruttura {{site.data.keyword.cloud_notm}} che desideri crittografare. Una delle opzioni consiste nell'utilizzare un server virtuale esistente per [creare un template dell'immagine](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template). Per ulteriori informazioni, vedi [Utilizza un template dell'immagine creato da un server virtuale abilitato a cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server). Puoi anche utilizzare un'immagine VHD esistente. Assicurati che l'immagine soddisfi i [requisiti dell'immagine crittografata](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs).
2. Se stai utilizzando un template dell'immagine da {{site.data.keyword.slportal}}, [esporta l'immagine decrittografata](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage) in {{site.data.keyword.cos_full_notm}}.
3. Scarica il file immagine da {{site.data.keyword.cos_full_notm}} in una macchina locale sicura per crittografare l'immagine. Nel tuo dashboard di servizio, seleziona l'azione **Scarica** per richiamare il tuo oggetto dall'archiviazione. Puoi utilizzare il plug-in di trasferimento ad alta velocità Aspera per scaricare immagini più grandi di 200 MB.
4. Utilizza lo strumento vhd-util per [crittografare la tua immagine VHD](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image).
5. In {{site.data.keyword.cos_full_notm}}, passa al tuo bucket e fai clic su **Aggiungi oggetti** per [caricare](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) l'immagine crittografata. Puoi utilizzare il plug-in di trasferimento ad alta velocità Aspera per caricare immagini più grandi di 200 MB.

## Importazione di un'immagine crittografata e ordine di un'istanza

1. Utilizzando IBM {{site.data.keyword.iamshort}} (IAM), crea un ID servizio con cui eseguire l'autenticazione quando importi l'immagine crittografata nella console {{site.data.keyword.cloud_notm}}.
      1. Crea un [ID servizio](/docs/iam?topic=iam-serviceids#serviceids).
      2. Assegna una [politica di accesso](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Assegna l'accesso per questi servizi: {{site.data.keyword.cos_full_notm}} e gestione delle chiavi.
      3. Crea una [chiave API per un ID servizio](/docs/iam?topic=iam-serviceidapikeys#create_service_key).
      4. Per ulteriori informazioni, vedi [Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/blog/introducing-ibm-cloud-iam-service-ids-api-keys){: new_window}.
2. Dalla console {{site.data.keyword.cloud_notm}}, [importa l'immagine crittografata](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos) nella pagina Template dell'immagine.
3. Dalla pagina Template delle immagini, puoi utilizzare la tua immagine crittografata per [ordinare](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template) un'istanza del server virtuale.
4. Con un server virtuale crittografato di cui è stato eseguito il provisioning, puoi controllare gli [eventi del server virtuale](/docs/vsi?topic=virtual-servers-at_events#at_events) tramite l'[Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).
