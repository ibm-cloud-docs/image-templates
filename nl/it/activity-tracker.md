---

copyright:
  years: 2018
lastupdated: "2018-08-09"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Controllo degli eventi del server virtuale con il Programma di traccia dell'attività
{: #auditing-virtual-server-events-with-activity-tracker}

Puoi controllare un'istanza del server virtuale per tutto il suo ciclo di vita utilizzando il [Programma di traccia dell'attività](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov). Devi avere un'istanza del Programma di traccia dell'attività con il piano di servizio premium fornito negli Stati Uniti Sud. Il piano premium ti fornisce l'accesso al dashboard Kibana che contiene molte più opzioni per il filtraggio e la ricerca nei log di controllo.

I log sono visibili nella sezione dei log dell'account del proprietario dell'account {{site.data.keyword.cloud}}. Il proprietario dell'account può visualizzare i seguenti log:
* Eventi attivati dal proprietario dell'account.
* Eventi attivati dagli utenti collegati con l'account.

Tramite il Programma di traccia dell'attività, puoi controllare i seguenti eventi dell'istanza del server virtuale:
* Provisioning di un'istanza del server virtuale
* Disabilitazione di una porta per un'interfaccia privata o pubblica
* Abilitazione di una porta per un'interfaccia privata o pubblica
* Impostazione della velocità della porta
* Creazione di un template dell'immagine
* Disattivazione di un'istanza del server virtuale
* Attivazione di un'istanza del server virtuale
* Riavvio di un'istanza del server virtuale
* Ridenominazione di un'istanza del server virtuale
* Attivazione della modalità di rischio per un'istanza del server virtuale
* Aggiunta di un gruppo di sicurezza a un'interfaccia pubblica o privata di un'istanza del server virtuale
* Rimozione di un gruppo di sicurezza da un'interfaccia pubblica o privata di un'istanza del server virtuale
* Caricamento di un'istanza del server virtuale da un'immagine
* Avvio di un'istanza del server virtuale da un'immagine
* Annullamento del dispositivo dell'istanza del server virtuale

Per gli eventi associati a un'interfaccia di rete, il log di controllo non distingue se l'evento si è verificato in un'interfaccia pubblica o in una privata.
{: tip}
