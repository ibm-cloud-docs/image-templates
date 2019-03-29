---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

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

Completa i seguenti passi per creare un template dell'immagine di un server virtuale.

1. Accedi al [{{site.data.keyword.slportal_full}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){: new_window} utilizzando le tue credenziali univoche.
2. Dal menu **Dispositivi**, seleziona **Elenco dispositivi**.
3. Fai clic sul server virtuale che desideri utilizzare per creare un template dell'immagine.

  Controlla la scheda **Password** della pagina **Dettagli dispositivo**. Assicurati che le password elencate nella pagina **Dettagli dispositivo** corrispondano alle password effettive del sistema operativo e di qualsiasi altro componente aggiuntivo software. Se le password non corrispondono, i server virtuali creati da questo template dell'immagine presenteranno un malfunzionamento.
  {:tip}

4. Dal menu **Azioni**, seleziona **Crea template dell'immagine**.
5. Immetti il nuovo nome dell'immagine nel campo **Nome immagine**.
6. Immetti le note necessarie per l'immagine nel campo **Nota**.
7. Seleziona la casetta di spunta **Accetto** dopo aver immesso tutte le informazioni.
8. Fai clic su **Crea template** per creare il template dell'immagine.

## Passi successivi

Una volta creato il template dell'immagine, utilizzandolo puoi creare altri server virtuali. I nuovi
server virtuali hanno le stesse configurazioni incluse nel template dell'immagine.
