---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}


# Utilizzo della tua licenza del sistema operativo o sottoscrizione
{: #using-your-own-os-license-or-subscription}

Quando crei un template dell'immagine con un'immagine VHD, puoi scegliere di fornire la tua licenza del sistema operativo RHEL tramite la sottoscrizione [Red Hat Cloud
Access ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) o una licenza Windows tramite Microsoft Enterprise Agreement.
{:shortdesc}

Se distribuisci un'immagine in {{site.data.keyword.cloud}} che indica che stai utilizzando la tua licenza, esistono i seguenti termini di supporto:
* {{site.data.keyword.IBM_notm}} fornisce il supporto per gli hypervisor, per il provisioning delle istanze, per l'importazione delle immagini, per il riavvio di un'immagine, per il ricaricamento di un sistema operativo e per l'acquisizione di un'immagine.
* La società da cui hai acquistato la licenza del sistema operativo fornisce il supporto per l'immagine stessa. {{site.data.keyword.IBM_notm}} non fornisce il supporto per l'immagine.

Quando fornisci la tua licenza per un'immagine, all'immagine vengono applicate le seguenti limitazioni:
* L'immagine è un'immagine privata. Non può essere condivisa pubblicamente.
* L'immagine non può avere componenti aggiuntivi software inclusi. Il software aggiuntivo deve essere aggiunto dopo aver eseguito il provisioning dell'immagine.

## Utilizzo di Red Hat Cloud Access
Per ulteriori informazioni sulla nostra certificazione come provider cloud Red Hat Enterprise Linux, vedi [Infrastructure as a Service (Iaas) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://access.redhat.com/ecosystem/cloud-provider/2262101).

## Utilizzo della tua licenza Windows
Sono supportati i seguenti sistemi operativi:
* Windows Server 2016
* Windows Server 2012
* Windows Server 2012 R2

Se hai domande sull'idoneità della tua licenza Windows esistente o sulla comprensione dei requisiti di notifica, contatta il tuo rappresentante Microsoft. Quando crei un template dell'immagine che specifica che stai utilizzando la tua licenza Windows, devi eseguire il provisioning di tale immagine su un host dedicato. Non puoi eseguire il provisioning di un'istanza pubblica o di un'istanza dedicata assegnata automaticamente a un host quando utilizzi un'immagine che indica che stai utilizzando la tua licenza Windows. Inoltre, quando crei o aggiorni un template dell'immagine Windows che specifica che stai utilizzando la tua licenza, l'immagine Windows non può essere un'immagine cloud-init.

## Prima di cominciare
Innanzitutto, passa al menu del dispositivo e assicurati di disporre delle autorizzazioni di account corrette per completare le attività.

* Passa al menu del dispositivo della tua console. Per ulteriori informazioni, vedi [Passaggio ai dispositivi](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Assicurati di disporre delle autorizzazioni di account e dell'accesso al dispositivo necessari. Solo il proprietario dell'account o un utente con l'autorizzazione dell'infrastruttura classica **Gestisci utenti** possono modificare le autorizzazioni.

Per ulteriori informazioni sulle autorizzazioni, vedi [Autorizzazioni dell'infrastruttura classica](/docs/iam?topic=iam-infrapermission#infrapermission) e [Gestione accesso dispositivo](/docs/vsi?topic=virtual-servers-managing-device-access).

## Importazione di un'immagine che designa la tua licenza

Puoi importare un'immagine VHD e specificare che stai fornendo la tua licenza o sottoscrizione per il sistema operativo.

Per accedere alla pagina Importa immagine dei template dell'immagine e contrassegnare un'immagine VHD per utilizzare la tua licenza o sottoscrizione, completa i seguenti passi:
1. Dal menu **Dispositivi**, seleziona **Gestisci > Immagini**.
2. Fai clic sulla scheda **Importa immagine**.
3. Completa le informazioni richieste per importare la tua immagine VHD e seleziona la casella di spunta **La tua licenza** visualizzata accanto
alla casella a discesa **Sistema operativo**. Per ulteriori informazioni sull'importazione delle immagini, vedi [Preparazione e importazione delle immagini](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images).

## Aggiornamento di un template dell'immagine per specificare una licenza del sistema operativo fornita dall'utente

Se hai un template dell'immagine VHD esistente, puoi specificare che desideri fornire la tua licenza o sottoscrizione per il sistema operativo.

Per accedere a un template dell'immagine e designare che utilizzi la tua licenza o sottoscrizione esistente, completa i seguenti passi:
1. Dal menu **Dispositivi**, seleziona **Gestisci > Immagini**.
2. Dall'elenco dei template, fai clic sul nome del template dell'immagine che desideri aggiornare.
3. Sulla pagina Dettagli template dell'immagine, seleziona la casella di spunta **Fornito dall'utente** sotto l'intestazione **Licenza SO** e fai clic su **Aggiorna**.
