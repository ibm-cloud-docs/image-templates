---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}

# Avvio di una VSI da un'immagine
{: #booting-a-vsi-from-an-image}

La funzione Avvio da immagine avvia un'istanza del server virtuale (VSI) utilizzando un template ISO importato da un account
Object Storage.
{:shortdesc}

L'avvio di un server virtuale da un'immagine porta online il dispositivo in modo sicuro cosicché possano essere risolti i problemi. Nella maggior parte dei casi, la funzione di avvio da immagine consente di risolvere i problemi in un ambiente senza rischiare una perdita di dati significativa che potrebbe verificarsi con un ricaricamento del sistema operativo. Sebbene la perdita di dati significativa sia meno probabile rispetto al ricaricamento del sistema operativo, ti consigliamo di eseguire il backup del dispositivo prima di iniziare l'operazione di avvio.

Utilizza i seguenti passi per avviare un server virtuale da un'immagine.

1. Esegui il backup di tutti i dati del dispositivo.
2. Dal menu **Dispositivi** nel [{{site.data.keyword.slportal_full}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/), seleziona **Elenco dispositivi**.
3. Da Elenco dispositivi, fai clic sul server virtuale da cui desideri avviare un template ISO.
4. Nella pagina Dettagli del dispositivo del server virtuale, seleziona **Azioni > Carica da immagine**.
5. Fai clic su **Avvia da questa immagine** per l'immagine desiderata

  Passa da immagini pubbliche a immagini private e viceversa utilizzando la funzione del selettore a discesa sopra l'elenco di immagini.
  {:tip}

6. Fai clic su **Carica da immagine** per avviare il dispositivo utilizzando l'immagine selezionata. Fai clic su **Chiudi** per annullare l'azione.

## Passi successivi

Una volta che hai iniziato il processo di avvio, l'immagine viene disattivata e poi avviata utilizzando l'immagine selezionata. Il tempo di avvio varia poiché
variano la dimensione e il tipo di ciascuna immagine. Una volta avviato il server utilizzando l'immagine selezionata, puoi accedervi come un dispositivo avviato regolarmente ma configurato in base all'immagine. Riavvia il server virtuale per disattivare e riportare il dispositivo al suo stato originale.
