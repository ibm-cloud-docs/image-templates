---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-02"

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

Dalla pagina Template dell'immagine, puoi esportare un template dell'immagine in un account [IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage).
{:shortdesc}

Il processo di esportazione dell'immagine prende un template dell'immagine standard privato preesistente o un template dell'immagine crittografato
e converte l'immagine in un file immagine memorizzato in una specifica ubicazione in un account IBM Cloud Object Storage.

Utilizza i seguenti passi per esportare un template dell'immagine in IBM Cloud Object Storage.

1. Esegui l'autenticazione a {{site.data.keyword.slportal}} con un ID servizio con accesso in scrittura a IBM Cloud Object Storage.
2. Dal menu **Dispositivi** nel [{{site.data.keyword.slportal_full}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/), seleziona **Gestisci > Immagini**.
3. Fai clic su **Azioni** per il template dell'immagine che desideri esportare e seleziona **Esporta immagine in IBM COS**. Se un template dell'immagine con la configurazione che desideri non è ancora
disponibile, vedi [Creazione di un template dell'immagine](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).
4. Completare i campi obbligatori (vedi tabella 1).
5. Fai clic su **OK** per esportare l'immagine nell'ubicazione desidera nell'Account IBM Cloud Object Storage.

| Campo | Valore |
| ----- | ----- |
| Nome file | Immetti il nome file per l'immagine. |
| {{site.data.keyword.cos_full_notm}} | Seleziona un account {{site.data.keyword.cos_full_notm}}. |
| Ubicazione | Seleziona la regione geografica specifica in cui desideri memorizzare il template dell'immagine. |
| Bucket | Seleziona il bucket {{site.data.keyword.cos_full_notm}} in cui desideri memorizzare il template dell'immagine. Sono validi solo i bucket presenti nell'ubicazione regionale che hai selezionato. Riceverai un messaggio di errore se specifichi un bucket che non è presente nell'ubicazione selezionata. |
| Chiave API | Specifica la chiave API dell'ID servizio che dispone dell'accesso in scrittura a {{site.data.keyword.cos_full_notm}}. Per ulteriori informazioni, vedi [Gestione delle chiavi API di un ID servizio](/docs/iam?topic=iam-serviceidapikeys). |
{: caption="Tabella 1. Valori per l'esportazione di un'immagine in IBM Cloud Object Storage" caption-side="top"}

## Passi successivi
Poiché ciascuna immagine ha dimensione e caratteristiche diverse,
il completamento del processo di esportazione potrebbe richiedere diversi minuti.

Una volta che hai esportato un'immagine, rimane nell'elenco dei template dell'immagine, ma è disponibile anche come file nell'ubicazione IBM Cloud Object Storage specificata durante il processo di esportazione.

Quando il tuo template dell'immagine viene esportato in IBM Cloud Object Storage, ciascun dispositivo a blocchi (o disco) ha il suo proprio file associato. Ad esempio, se il tuo file immagine è denominato image.vhd, il primo dispositivo a blocchi viene esportato come image-0.vhd. Il secondo dispositivo a blocchi viene esportato come image-1.vhd, e così via.
{: tip}
