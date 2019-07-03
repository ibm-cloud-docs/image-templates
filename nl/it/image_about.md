---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Informazioni sui template dell'immagine
{: #about-image-templates}

I template dell'immagine standard forniscono un'opzione di creazione dell'immagine per tutti i {{site.data.keyword.virtualmachinesshort}}, indipendentemente dal sistema operativo.
{:shortdesc}

Con i template dell'immagine standard, puoi acquisire un'immagine di un server virtuale esistente e crearne una nuova basata sull'immagine acquisita. Questi template non sono compatibili con i server bare metal.

## Come utilizzare i template dell'immagine
Le due azioni principali di ogni template dell'immagine sono creazione e distribuzione. Quando richiedi di creare un'immagine, il sistema automatizzato di {{site.data.keyword.cloud}} porta il tuo server offline. Mentre il server è offline, viene creato un backup compresso dei tuoi dati, vengono registrate le informazioni di configurazione e il template dell'immagine viene memorizzato nella SAN di {{site.data.keyword.cloud_notm}}. Durante la fase di distribuzione del template dell'immagine, il sistema automatizzato crea un nuovo server basato sui dati raccolti dall'immagine selezionata. Il processo di distribuzione effettua regolazioni per il volume, ripristina i dati copiati e quindi apporta le modifiche finali di configurazione (ad esempio, le configurazioni di rete) per il nuovo host.

## Immagini private

Le immagini private sono immagini create da un utente sull'account o immagini create su un altro account e condivise. Per impostazione predefinita, tutte le immagini che vengono create sono private.

## Immagini pubbliche

Le immagini pubbliche sono server virtuali preconfigurati che vengono pubblicati da {{site.data.keyword.cloud_notm}} o resi pubblici dai clienti e sono disponibili per l'uso da parte di tutti i clienti {{site.data.keyword.cloud_notm}}. I server virtuali di cui viene eseguito il provisioning tramite i template dell'immagine pubblica sono generalmente configurati per ottenere prestazioni ottimali, con precise combinazioni di diverse specifiche di configurazione.
