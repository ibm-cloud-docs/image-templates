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


# Utilizzo della crittografia End to End (E2E) per eseguire il provisioning di un'istanza crittografata
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

La funzione di crittografia End to End (E2E) ti consente di utilizzare la tua immagine di sistema operativo abilitata a cloud-init, crittografata, che hai crittografato utilizzando una chiave di crittografia dei dati che possiedi e controlli. Una volta completate alcune parti di configurazione, puoi importare la tua immagine crittografata nel repository dei template dell'immagine e utilizzarla per eseguire il provisioning delle istanze del server virtuale crittografato. La crittografia E2E fornisce la crittografia per i dati inattivi per l'archiviazione associata alle istanze del server virtuale di cui è stato eseguito il provisioning. Per ottenere l'accesso a questa funzione, contatta il supporto.
{:shortdesc}

La crittografia E2E racchiude diversi componenti {{site.data.keyword.cloud}} per fornire una soluzione sicura per le tue informazioni critiche.

* {{site.data.keyword.keymanagementservicefull_notm}} protegge le tue chiavi con gli HSM (hardware security module) basati sul cloud certificati da FIPS 140-2 Level 2 che proteggono dal furto di informazioni.
* IBM {{site.data.keyword.iamshort}} (IAM) consente al servizio Cloud Block Storage di accedere a {{site.data.keyword.keymanagementserviceshort}} e alla tua chiave root utilizzata per impacchettare la tua chiave di crittografia dei dati.
* {{site.data.keyword.cos_full_notm}} memorizza in modo sicuro la tua immagine crittografata quando la carichi.
* In {{site.data.keyword.slportal}} puoi importare la tua immagine crittografata e creare un template dell'immagine.
* Con un template dell'immagine crittografata disponibile nell'ambiente dell'infrastruttura {{site.data.keyword.cloud_notm}} Console, puoi eseguire il provisioning delle istanze del server virtuale crittografato.
* Infine, hai la possibilità di controllare gli eventi associati ai server virtuali crittografati tramite il Programma di traccia dell'attività.

## Preparazione del tuo ambiente

1. Devi disporre di un account aggiornato per utilizzare la crittografia E2E per i server virtuali. Per ulteriori informazioni, vedi [Passaggio all'ID IBM e collegamento degli account](/docs/account?topic=account-unifyingaccounts).
2. Utilizza {{site.data.keyword.keymanagementservicefull_notm}} per creare e gestire le chiavi.
      1. Esegui il provisioning del servizio [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision).
      2. [Crea](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys) o [importa](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) una chiave root (CRK) in {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Facoltativo**: se vuoi, puoi [creare](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) o [importare](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) una chiave standard per la decrittografia.
      4. Ottieni l'[accesso all'API {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-set-up-api#set-up-api) in modo che tu possa impacchettare la chiave di crittografia dei dati.
      5. [Impacchetta la chiave di crittografia dei dati](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys) con la chiave root. Avrai bisogno del testo crittografato associato alla chiave di crittografia dei dati impacchettata (WDEK) quando importi la tua immagine crittografata in {{site.data.keyword.slportal}}.
3. Da IBM {{site.data.keyword.iamshort}} (IAM), [autorizza l'accesso](/docs/iam?topic=iam-serviceauth#create-an-authorization) tra **Cloud Block Storage** (servizio di origine) e {{site.data.keyword.keymanagementservicelong_notm}} (servizio di destinazione). Gli utenti che hanno importato immagini crittografate da {{site.data.keyword.cos_full_notm}} devono avere una [politica di accesso definita](/docs/iam?topic=iam-userroles) per {{site.data.keyword.keymanagementservicelong_notm}} in IAM.
4. In IBM Cloud Console, crea un'istanza di {{site.data.keyword.cos_full_notm}} e crea un bucket per memorizzare i dati. Per ulteriori informazioni, vedi [Introduzione a {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-console-#getting-started-console-).
      1. Crea l'istanza {{site.data.keyword.cos_full_notm}} nella stessa ubicazione regionale in cui è stato eseguito il provisioning del tuo servizio {{site.data.keyword.keymanagementserviceshort}}.
      2. Quando crei il bucket, l'impostazione **Resilienza** deve essere _Regionale_.
      3. Facoltativamente, quando crei il bucket, puoi [crittografarlo con la tua chiave](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-manage-encryption#sse-kp).   

## Preparazione delle tue immagini crittografate

1. Seleziona un'immagine non crittografata che funzioni nell'ambiente dell'infrastruttura {{site.data.keyword.cloud_notm}} che desideri crittografare. Una delle opzioni consiste nell'utilizzare un server virtuale esistente per [creare un'immagine personalizzata](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template). Per ulteriori informazioni, vedi [Utilizza un template dell'immagine creato da un server virtuale abilitato a cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-a-standard-image-created-from-a-cloud-init-provisioned-virtual-server). Puoi anche utilizzare un'immagine VHD esistente. Assicurati che l'immagine soddisfi i [requisiti dell'immagine crittografata](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#encrypted-image-reqs).
2. Se stai utilizzando un template dell'immagine da {{site.data.keyword.slportal}}, [esporta l'immagine non crittografata](/docs/infrastructure/image-templates?topic=image-templates-exporting-to-ibm-cos) in {{site.data.keyword.cos_full_notm}}.
3. Scarica il file immagine da {{site.data.keyword.cos_full_notm}} in una macchina locale sicura per crittografare l'immagine. Nel tuo dashboard di servizio, seleziona l'azione **Scarica** per richiamare il tuo oggetto dall'archiviazione. Puoi utilizzare il plug-in di trasferimento ad alta velocità Aspera per scaricare immagini più grandi di 200 MB.
4. Utilizza il formato di crittografia del disco LUKS per [crittografare la tua immagine](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption) utilizzando QEMU e DMCrypt.
5. In {{site.data.keyword.cos_full_notm}}, passa al tuo bucket e fai clic su **Aggiungi oggetti** per [caricare](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data) l'immagine crittografata. Puoi utilizzare il plug-in di trasferimento ad alta velocità Aspera per caricare immagini più grandi di 200 MB.

## Importazione di un'immagine crittografata e ordine di un'istanza

1. Utilizzando IBM {{site.data.keyword.iamshort}} (IAM), crea un ID servizio con cui eseguire l'autenticazione quando importi l'immagine crittografata nel {{site.data.keyword.slportal}}.
      1. Crea un [ID servizio](/docs/iam?topic=iam-serviceids#serviceids).
      2. Assegna una [politica di accesso](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Assegna l'accesso per questi servizi: {{site.data.keyword.cos_full_notm}} e {{site.data.keyword.keymanagementservicelong_notm}}.
      3. Crea una [chiave API per un ID servizio](/docs/iam?topic=iam-serviceidapikeys#creating-an-api-key-for-a-service-id).
      4. Per ulteriori informazioni, vedi [Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window}.
2. Dal {{site.data.keyword.slportal}}, [importa l'immagine crittografata](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images#import-icos) nella pagina Template dell'immagine. (Puoi anche importare l'immagine crittografata [utilizzando {{site.data.keyword.slapi_short}}](/docs/infrastructure/image-templates?topic=image-templates-importing-an-encrypted-image-by-using-the-softlayer-api).)
3. Dalla pagina Template delle immagini, puoi utilizzare la tua immagine crittografata per [ordinare](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template) un'istanza del server virtuale.
4. Con un server virtuale crittografato di cui è stato eseguito il provisioninig, puoi controllare gli [eventi del server virtuale](/docs/vsi?topic=virtual-servers-at_events#at_events) tramite il [Programma di traccia dell'attività](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov).
