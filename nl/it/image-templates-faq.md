---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-30"

keywords:

subcollection: image-templates

---


{:new_window: target="_blank"}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}

# FAQ: template dell'immagine
{: #faq-image-templates}

## Cos'è un template dell'immagine standard?
{: faq}

Un template dell'immagine standard costituisce l'opzione di creazione dell'immagine di {{site.data.keyword.virtualmachineslong}} per {{site.data.keyword.BluSoftlayer_notm}}.
I template dell'immagine standard consentono agli utenti di acquisire un'immagine di un server virtuale esistente indipendentemente dal suo sistema
operativo e di creare un nuovo server virtuale basato sull'immagine.

## Cos'è un template ISO?
{: faq}

Il template ISO è un tipo di template riservato specificatamente per gli ISO che possono essere utilizzati per avviare una VSI. I template ISO sono disponibili in due versioni: pubblici e privati. I template ISO pubblici sono template preconfigurati forniti da {{site.data.keyword.BluSoftlayer_notm}} e possono essere utilizzati da qualsiasi cliente. I template ISO privati vengono creati importando un'immagine di un ISO memorizzato in un account {{site.data.keyword.objectstorageshort}}. Per poter importare un ISO nella schermata Template dell'immagine, è necessario che l'ISO sia avviabile.

Per caricare un template ISO in una VSI, possono essere utilizzati solo sistemi operativi supportati {{site.data.keyword.BluSoftlayer_notm}}. Per ulteriori informazioni, vedi [Supported Operating Systems ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://www.softlayer.com/services/software/).
{:tip}

## Qual è la differenza tra un'immagine pubblica e una privata?
{: faq}

Un'immagine pubblica è un'immagine che può essere esaminata e applicata a un nuovo server virtuale da qualsiasi utente {{site.data.keyword.BluSoftlayer_notm}}. Attualmente {{site.data.keyword.BluSoftlayer_notm}}
crea immagini pubbliche come una soluzione per le opzioni di configurazione su diversi dispositivi. I clienti possono anche creare immagini pubbliche e renderle disponibili agli utenti. Un'immagine privata è un'immagine che può
essere esaminata solo dagli utenti autorizzati. Per impostazione predefinita, gli utenti autorizzati sono gli utenti presenti nel tuo account; tuttavia
le immagini possono essere anche condivise tra più account aggiornando le opzioni di condivisione nel {{site.data.keyword.slportal}}.

## Posso acquisire e distribuire i server virtuali con il mio hypervisor autogestito?
{: faq}

Puoi acquisire e distribuire solo i server di cui è stato eseguito il provisioning tramite {{site.data.keyword.BluSoftlayer_notm}}. Non puoi acquisire, eseguire il provisioning o distribuire i singoli server virtuali che hai creato manualmente sui dispositivi personali.

## Posso condividere il mio template ISO privato con altri clienti?
{: faq}

Gli unici template ISO resi pubblici a tutti i clienti sono i template generati da {{site.data.keyword.BluSoftlayer_notm}}. I template ISO privati sono specifici dell'account e non possono essere condivisi tra i clienti tramite il {{site.data.keyword.slportal}}.

## Il mio template ISO deve trovarsi nello stesso data center del mio server virtuale per completare un avvio dall'immagine?
{: faq}

Sì, i template ISO devono trovarsi nello stesso data center di un server virtuale per eseguire l'avvio dall'immagine. Nel caso in cui non si
trovi nello stesso data center, il template ISO deve essere copiato nel data center appropriato per completare l'azione. Se si verifica questa situazione,
riceverai un messaggio di avvertenza relativo alla transazione. Quando un template ISO viene copiato in un data center diverso, sull'account viene addebitato
un piccolo costo con la stessa procedura che viene applicata per la copia di altri tipi di template dell'immagine.

## Qual è la differenza tra i dati fisici e la dimensione del volume?
{: faq}

Il volume è lo spazio su disco disponibile per memorizzare i file, mentre i dati fisici sono i file effettivi memorizzati sul disco.

## Cos'è la funzione di importazione/esportazione dell'immagine?
{: faq}

La funzione di importazione/esportazione dell'immagine che si trova nella pagina Template dell'immagine nel {{site.data.keyword.slportal}} consente la conversione di VHD e di ISO memorizzati in un account {{site.data.keyword.objectstorageshort}} in template dell'immagine e viceversa. Quando importi un'immagine, viene originato uno file specifico (VHD o ISO) da un contenitore dell'account [{{site.data.keyword.objectstorageshort}}] specificato e viene convertito in template dell'immagine. Il template dell'immagine può essere utilizzato per avviare o caricare un dispositivo. Quando esporti un'immagine, il template dell'immagine viene convertito in un file (o in diversi file se il template dispone di più dischi) memorizzato in un'ubicazione specificata in un contenitore dell'account {{site.data.keyword.objectstorageshort}}.

## Come creo un template dell'immagine per tutto il mio server e non solo per la mia unità primaria?
{: faq}

Per creare un template dell'immagine per tutto il tuo server, vedi le istruzioni presenti in [Creazione di un template dell'immagine](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).

Se scegli di esportare un template dell'immagine in IBM Cloud Object Storage, ciascun dispositivo a blocchi (o disco) ha il suo proprio file associato. Ad esempio, se il tuo file immagine è denominato image.vhd, il primo dispositivo a blocchi viene esportato come image-0.vhd. Il secondo dispositivo a blocchi viene esportato come image-1.vhd, e così via.
{: tip}
