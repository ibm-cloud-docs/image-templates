---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Provisioning con un'immagine abilitata a cloud-init
{: #provisioning-wiht-a-cloud-init-enabled-image}

Quando ordini un server virtuale, molti sistemi operativi ora utilizzano un'immagine abilitata a cloud-init per ottimizzare il tempo di provisioning. Puoi anche importare
un'immagine personalizzata che hai abilitato per cloud-init.
{:shortdesc}

Ora, per impostazione predefinita, i seguenti sistemi operativi vengono configurati con un'immagine abilitata a cloud-init quando ordini un server virtuale senza componenti aggiuntivi. (I componenti aggiuntivi includono software aggiuntivo, script di post-provisioning e monitoraggio avanzato.)
* CentOS 7
* Debian 8, 9
* Ubuntu 16.04, 18.04
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

Quando ordini un server virtuale con un sistema operativo abilitato a cloud-init, puoi aggiungere i metadati o i dati utente con gli script di provisioning personalizzati. Nel campo Dati utente nel modulo dell'ordine, immetti i dati utente cloud-init facoltativi oppure i metadati facoltativi per il server.

## Importa un'immagine abilitata a cloud-init personalizzata

Se hai creato un'immagine personalizzata con cloud-init abilitato, puoi designarla come un'immagine cloud-init nella pagina Importa immagine
del {{site.data.keyword.slportal_full}}.

Per accedere alla pagina Importa immagine dei template dell'immagine e contrassegnare un'immagine come abilitata a cloud-init, segui questi passi:
1. Dal menu **Dispositivi**, seleziona **Gestisci** > **Immagini**.
2. Fai clic sulla scheda **Importa immagine**.
3. Completa le informazioni richieste per importare la tua immagine abilitata a cloud-init e seleziona la casella di spunta **Cloud init**
visualizzata accanto alla casella a discesa **Sistema operativo**. Per ulteriori informazioni sull'importazione delle immagini, vedi Importa un'immagine.

## Contrassegna un template dell'immagine come abilitato a cloud-init

Se hai un template dell'immagine VHD esistente che è abilitato a cloud-init, puoi designarlo come abilitato a cloud-init nella pagina dei
dettagli del template dell'immagine.

Per accedere a un template dell'immagine e contrassegnarlo come abilitato a cloud-init, completa i seguenti passi:
1. Dal menu **Dispositivi**, seleziona **Gestisci** > **Immagini**.
2. Dall'elenco dei template, fai clic sul nome del template dell'immagine che desideri aggiornare.
3. Sulla pagina Dettagli template dell'immagine, seleziona la casella di spunta **Abilitato** nell'intestazione **Cloud Init** e fai clic su **Aggiorna**.

## Utilizza un template dell'immagine creato da un server virtuale abilitato a cloud-init

Di norma, cloud-init viene eseguito solo una volta. Tuttavia, se esegui il provisioning di un server virtuale da un'immagine abilitata a cloud-init e successivamente crei
un template dell'immagine da tale server virtuale, viene registrato l'UUID. Se tale template dell'immagine viene utilizzato per creare un altro
server virtuale, cloud-init viene eseguito di nuovo.

## Crea template dell'immagine che sono abilitati a cloud-init

Per informazioni sulla configurazione delle immagini, vedi la
[documentazione di cloud-init ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloudinit.readthedocs.io/en/latest/).

Per informazioni sulle origini dati, vedi [Datasources ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html). Le immagini cloud-init {{site.data.keyword.cloud_notm}} vengono create per
l'ambiente utilizzando l'origine dati [Config Drive ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - Version 2 per fornire i metadati.

### Requisiti Linux
* Cloud-init versione 0.7.7 o superiore

### Requisiti Windows
* Servizio metadati Cloudbase-init per il supporto di reti pubbliche e private nell'infrastruttura {{site.data.keyword.cloud_notm}}. Il servizio aggiorna anche il portale del cliente con le credenziali del server virtuale Windows. Puoi accedere al servizio all'indirizzo
[https://github.com/softlayer/bluemix-cloudbase-init ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/softlayer/bluemix-cloudbase-init).
* Se stai utilizzando Vyatta nel tuo ambiente, devi configurarlo per consentire le chiamate API ai programmi di bilanciamento del carico dell'API. Per ulteriori informazioni, consulta [Brocade vRouter (Vyatta) Set up Guide for VMware Environments with File Storage ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/infrastructure/FileStorage?topic=FileStorage-configureVyatta#setting-up-brocade-vrouter-vyatta-for-vmware-environments-with-file-storage).
