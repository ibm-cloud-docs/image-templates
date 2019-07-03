---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

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

# Esportazione di un'immagine in IBM Cloud Object Storage
{: #exporting-an-image-to-ibm-cloud-object-storage}

Dalla pagina Template dell'immagine, puoi esportare un template dell'immagine in un account [{{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about).
{:shortdesc}

Il processo di esportazione dell'immagine prende un template dell'immagine standard privato preesistente o un template dell'immagine crittografata
e converte l'immagine in un file immagine memorizzato in una specifica ubicazione in un account {{site.data.keyword.cos_full_notm}} 

*Nota:* se hai importato un'immagine VMDK, puoi esportare tale immagine in formato VHD o VMDK. A causa delle differenze tra i formati di immagine, è possibile che si verifichi una perdita di dati. Per proteggere i tuoi dati in caso di perdita di dati, viene conservato il file VHD originale.

## Prima di cominciare
Innanzitutto, passa al menu del dispositivo e assicurati di disporre delle autorizzazioni di account corrette per completare le attività.

* Passa al menu del dispositivo della tua console. Per ulteriori informazioni, vedi [Passaggio ai dispositivi](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Assicurati di disporre dell'accesso in scrittura a {{site.data.keyword.cos_full_notm}}. Solo il proprietario dell'account o un utente con l'autorizzazione dell'infrastruttura classica **Gestisci utenti** possono modificare le autorizzazioni.

Per ulteriori informazioni sulle autorizzazioni, vedi [Autorizzazioni dell'infrastruttura classica](/docs/iam?topic=iam-infrapermission#infrapermission) e [Gestione accesso dispositivo](/docs/vsi?topic=virtual-servers-managing-device-access).

Se intendi esportare questo template dell'immagine in {{site.data.keyword.cos_full_notm}}, assicurati che il suo nome non contenga alcun carattere che possa essere problematico in un indirizzo web. Ad esempio, ?, =, <, e altri caratteri speciali potrebbero causare una modalità di funzionamento indesiderata, senza una codifica URL.
{: tip}

## Esportazione di un'immagine in IBM Cloud Object Storage

Utilizza i seguenti passi per esportare un template dell'immagine in IBM Cloud Object Storage.

1. Dal menu **Dispositivi**, seleziona **Gestisci > Immagini**.
2. Fai clic su **Azioni** per il template dell'immagine che desideri esportare e seleziona **Esporta immagine in IBM COS**. Se un template dell'immagine con la configurazione che desideri non è ancora
disponibile, vedi [Creazione di un template dell'immagine](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template).
3. Completare i campi obbligatori (vedi tabella 1).
4. Fai clic su **OK** per esportare l'immagine nell'ubicazione specificata nell'account {{site.data.keyword.cos_full_notm}} 

| Campo | Valore |
| ----- | ----- |
| Nome file | Immetti il nome file per l'immagine. |
| {{site.data.keyword.cos_full_notm}} | Seleziona un account {{site.data.keyword.cos_full_notm}}. |
| Ubicazione | Seleziona la regione geografica specifica in cui desideri memorizzare il template dell'immagine. |
| Bucket | Seleziona il bucket {{site.data.keyword.cos_full_notm}} in cui desideri memorizzare il template dell'immagine. Sono validi solo i bucket presenti nell'ubicazione regionale che hai selezionato. Riceverai un messaggio di errore se specifichi un bucket che non è presente nell'ubicazione selezionata. |
| Chiave API | Specifica la chiave API dell'ID servizio che dispone dell'accesso in scrittura a {{site.data.keyword.cos_full_notm}}. Per ulteriori informazioni, vedi [Gestione delle chiavi API di un ID servizio](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys). |
{: caption="Tabella 1. Valori per l'esportazione di un'immagine in {{site.data.keyword.cos_full_notm}}" caption-side="top"}
| Tipo di file di destinazione | Seleziona il tipo di file che desideri esportare. Se hai importato un'immagine VMDK, hai la possibilità di esportare tale immagine in formato VHD o VMDK. Se il file era originariamente in formato VHD, puoi esportarlo solo come VHD. |

## Passi successivi
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
Poiché ciascuna immagine ha dimensione e caratteristiche diverse,
il completamento del processo di esportazione potrebbe richiedere diversi minuti.

Una volta che hai esportato un'immagine, rimane nell'elenco dei template dell'immagine, ma è disponibile anche come file nell'ubicazione {{site.data.keyword.cos_full_notm}} specificata durante il processo di esportazione. Puoi scaricare il file immagine da {{site.data.keyword.cos_full_notm}}. Dall'elenco risorse nella console {{site.data.keyword.cloud_notm}}, accedi all'istanza del servizio Cloud Object Storage. Nel bucket dove è archiviata la tua immagine, seleziona i file immagine che vuoi scaricare e seleziona **Scarica oggetti**.

Quando il tuo template dell'immagine viene esportato in IBM Cloud Object Storage, ciascun dispositivo a blocchi (o disco) ha il suo proprio file associato. Ad esempio, se il tuo file immagine è denominato image.vhd, il primo dispositivo a blocchi viene esportato come image-0.vhd. Il secondo dispositivo a blocchi viene esportato come image-1.vhd, e così via.
{: tip}
