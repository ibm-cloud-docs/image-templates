---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}


# Creazione di un template dell'immagine
{: #creating-an-image-template}

Con i template dell'immagine, puoi replicare diverse opzioni di configurazione relative a {{site.data.keyword.virtualmachinesshort}}.
{:shortdesc}

In qualsiasi momento del ciclo di vita di un server virtuale, puoi creare un template dell'immagine. Quindi, puoi utilizzarlo per replicare velocemente parti della sua configurazione in un altro server virtuale. Puoi creare template dell'immagine da qualsiasi server virtuale, indipendentemente dal suo sistema operativo. Una volta completato il tuo template dell'immagine, puoi utilizzarlo per creare un altro server virtuale.

## Prima di cominciare
Innanzitutto, passa al menu del dispositivo e assicurati di disporre delle autorizzazioni di account corrette per completare le attività.

* Passa al menu del dispositivo della tua console. Per ulteriori informazioni, vedi [Passaggio ai dispositivi](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Assicurati di disporre delle autorizzazioni di account e dell'accesso al dispositivo necessari. Solo il proprietario dell'account o un utente con l'autorizzazione dell'infrastruttura classica **Gestisci utenti** possono modificare le autorizzazioni.

Per ulteriori informazioni sulle autorizzazioni, vedi [Autorizzazioni dell'infrastruttura classica](/docs/iam?topic=iam-infrapermission#infrapermission) e [Gestione accesso dispositivo](/docs/vsi?topic=virtual-servers-managing-device-access).

## Creazione di un template dell'immagine

Completa i seguenti passi per creare un template dell'immagine di un server virtuale.

1. Dal menu **Dispositivi**, seleziona **Elenco dispositivi**.
2. Fai clic sul server virtuale che desideri utilizzare per creare un template dell'immagine.

  Controlla la scheda **Password** della pagina **Dettagli dispositivo**. Assicurati che le password elencate nella pagina **Dettagli dispositivo** corrispondano alle password effettive del sistema operativo e di qualsiasi altro componente aggiuntivo software. Se le password non corrispondono, i server virtuali creati da questo template dell'immagine presenteranno un malfunzionamento.
  {:tip}

3. Dal menu **Azioni**, seleziona **Crea template dell'immagine**.
4. Immetti il nuovo nome dell'immagine nel campo **Nome immagine**.
5. Immetti le note necessarie per l'immagine nel campo **Nota**.
6. Seleziona la casetta di spunta **Accetto** dopo aver immesso tutte le informazioni.
7. Fai clic su **Crea template** per creare il template dell'immagine.

## Passi successivi

Una volta creato il template dell'immagine, utilizzandolo puoi creare altri server virtuali. I nuovi
server virtuali hanno le stesse configurazioni incluse nel template dell'immagine.
