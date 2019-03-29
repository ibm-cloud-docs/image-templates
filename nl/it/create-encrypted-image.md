---

copyright:
  years: 2018
lastupdated: "2018-09-19"

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


# Creazione di un'immagine crittografata
{: #creating-an-encrypted-image}

Come parte della funzione di crittografia E2E, puoi crittografare un'immagine da importare nel Template dell'immagine e utilizzarla per distribuire un'istanza del server virtuale crittografata.
{:shortdesc}

## Requisiti dell'immagine crittografata
{: #encrypted-image-reqs}

Un'immagine crittografata di tua creazione deve soddisfare i seguenti requisiti dell'immagine:

* L'immagine è compatibile con l'ambiente dell'infrastruttura {{site.data.keyword.cloud}} Console.
* L'immagine include un sistema operativo Linux come ad esempio CentOS, Debian, Red Hat Enterprise Linux o Ubuntu.
* L'immagine è abilitata a cloud-init.
* L'immagine è crittografata con la [crittografia del disco LUKS](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption).

## Utilizzo di QEMU e DM-Crypt per creare un'immagine RAW crittografata
{: #luks-disk-encryption}

Per crittografare la tua immagine, devi convertire un file immagine VHD fisso nel formato RAW. Quindi, utilizza QEMU e DM-Crypt per creare un nuovo file immagine con la crittografia del disco LUKS. Monta il file come un volume crittografato e copia il file immagine non crittografato nel volume crittografato.

Questa procedura ti guida attraverso queste attività:

* Convertire la tua immagine VHD dinamica in un'immagine VHD fissa.
* Convertire la tua immagine VHD fissa nel formato file RAW.
* Creare un file della chiave di crittografia dei dati che utilizzerai per crittografare l'unità.
* Creare un nuovo file RAW per contenere l'immagine e l'intestazione di crittografia LUKS.
* Formattare il file RAW con la crittografia del disco LUKS.
* Collegare il file RAW crittografato a un dispositivo a blocchi.
* Copiare l'immagine non crittografata in un dispositivo volume crittografato.

Devi disporre del privilegio per eseguire i comandi utilizzando l'autorizzazione utente `root`, tramite sudo, per montare e smontare i volumi crittografati sul tuo sistema. Puoi immettere il seguente comando per verificare i tuoi privilegi utente `root`:

```
sudo echo "Hello!"
```
{: pre}

Devi ricevere una risposta echo "Hello!" per completare questa attività di crittografia.

**Suggerimento**: devi completare questa attività di crittografia su un sistema su cui è in esecuzione un sistema operativo Linux e che abbia i seguenti pacchetti disponibili:
* qemu o qemu-image (in base al tuo sistema operativo Linux)
* cryptsetup

Completa i seguenti passi per crittografare la tua immagine.

1. Converti il tuo file immagine VHD dinamico in un file immagine VHD fisso. Un file VHD fisso previene il danneggiamento che potrebbe verificarsi se un file VHD dinamico viene convertito direttamente in un formato file RAW. Esegui il seguente comando per convertire il file immagine VHD dinamico in un file immagine VHD fisso:

  ```
  qemu-img convert -p -O vpc -o subformat=fixed <DYNAMIC_VHD_FILE> <FIXED_VHD_FILE>
  ```
  {: pre}

  Ad esempio, se il tuo file VHD dinamico è _Rhel_7.vhd-0.vhd_ e desideri che il file VHD fisso sia denominato _Rhel_7.fixed.vhd-0.vhd_, esegui il seguente comando:

  ```
  $ qemu-img convert -p -O vpc -o subformat=fixed Rhel_7.vhd-0.vhd Rhel_7.fixed.vhd-0.vhd
  ```
  {: screen}

  Il completamento di questo comando potrebbe richiedere minimo 30 minuti.
  {: tip}

2. Converti il file immagine VHD fisso nel formato file RAW utilizzando QEMU. Questa immagine deve avere un formato file RAW prima di poterla crittografare. Esegui il seguente comando QEMU:

  ```
  qemu-img convert -p -O raw <FIXED_VHD_FILE> <RAW_IMAGE_FILE>
  ```
  {: pre}

  Ad esempio, se il tuo file VHD fisso è _Rhel_7.fixed.vhd-0.vhd_ e desideri che il file di output RAW sia denominato _Rhel_7.raw-0.raw_, esegui il seguente comando:

  ```
  qemu-img convert -p -O raw Rhel_7.fixed.vhd-0.vhd Rhel_7.raw-0.raw
  ```
  {: screen}

  Il completamento di questo comando potrebbe richiedere minimo 30 minuti.
  {: tip}

3. Identifica la chiave di crittografia dei file che utilizzerai per crittografare e decrittografare l'unità. Questa chiave di crittografia dei dati è la stessa che hai impacchettato e specificato quando hai importato l'immagine crittografata in {{site.data.keyword.slportal}}. Crea un file che contenga la chiave di crittografia dei file che utilizzerai per crittografare e decrittografare l'unità. In questo file, la chiave deve essere spacchettata e deve avere un testo codificato base64. Base64 ti aiuta a garantire che non vengano inclusi spazi o interruzioni accidentali. La chiave di crittografia dei dati codificati base64 deve contenere minimo 32 caratteri o byte e massimo 512 caratteri o byte. Il materiale della chiave di crittografia dei dati deve trovarsi su una riga senza interruzioni o nuove righe. Ad esempio, utilizza il seguente comando per creare un file denominato `secret.dek` per memorizzare il tuo `unwrapped_key_material` relativo alla tua chiave di crittografia dei dati e codificarlo con base64:

  ```
  $ echo -n $(echo 'unwrapped_key_material' | base64) > secret.dek
  ```
  {: screen}

  Conserva questa chiave in un posto sicuro. Se perdi questa chiave, non sarai in grado di decrittografare il tuo disco.
  {: tip}

4. Identifica la dimensione corretta del nuovo file RAW da creare nel seguente passo per il disco crittografato, includendo l'aggiunta di un'intestazione LUKS. Il nuovo file RAW verrà utilizzato da _dmcrypt_ e _cryptsetup_ per conservare il contenuto del disco in un formato LUKS crittografato. La dimensione del nuovo file RAW deve essere la somma della dimensione del file RAW creato nel passo 2 e di un'intestazione LUKS di 4 MB costante. Per determinare la dimensione della tua immagine RAW esistente, esegui il seguente comando, dove _Rhel_7.raw-0.raw_ è il nome dell'immagine:

  ```
  ls -l Rhel_7.raw-0.raw
  ```
  {: pre}

  Vedrai un output simile al seguente:

  ```
  -rw-r--r--. 1 user user 42949017600 Mar 5 10:02 Rhel_7.raw-0.raw
  ```
  {: screen}

  Dove _42949017600_ è il numero di byte che vengono utilizzati dall'immagine RAW
      1. Aggiungi 4 MB (convertiti in byte) alla dimensione del file immagine RAW per includere l'intestazione LUKS. Ad esempio, 42949017600 + (4 x 1024 x 1024) = 42953211904 byte.
      2. Converti la somma della dimensione dell'immagine RAW e dell'intestazione LUKS in megabyte e arrotonda al megabyte successivo. Ad esempio, 42953211904 byte / 1024 byte / 1024 kilobyte = 40963.375 megabyte = **40964 MG**.

5. Crea un nuovo file RAW con il numero corretto di byte che diventerà l'immagine RAW crittografata. Utilizza il seguente comando _dd_ per creare il file RAW:

  ```
  dd if=/dev/zero of=<ENCRYPTED_RAW_FILENAME> bs=1M count=<TARGET_SIZE_IN_MEGABYTE> status=progress
  ```
  {: pre}

  Dove _ENCRYPTED_RAW_FILENAME_ è il file che diventerà l'immagine RAW crittografata e _TARGET_SIZE_IN_MEGABYTE_ è il numero che hai ottenuto nel passo 4, la dimensione dell'immagine RAW non crittografata più l'intestazione LUKS.

  Ad esempio, se desideri che il tuo nuovo file RAW che diventerà l'immagine crittografata sia _Rhel_7.encrypted.raw_ e che la sua dimensione finale sia _40964_ MB, esegui il seguente comando:

  ```
  $ dd if=/dev/zero of=Rhel_7.encrypted.raw bs=1M count=40964 status=progress
  ```
  {: screen}

6. Formatta il file RAW con la crittografia del disco LUKS utilizzando _dmcrypt_ e la tua chiave di crittografia dei dati (dal passo 3). Questo passo prepara il file per contenere l'immagine. Per formattare il file con la crittografia LUKS, esegui il seguente comando _cryptsetup_:

  ```
  cryptsetup -v luksFormat <ENCRYPTED_RAW_FILENAME> <DEK_FILENAME>
  ```
  {: pre}

  Dove _ENCRYPTED_RAW_FILENAME_ è il nome file del file RAW che desideri crittografare e _DEK_FILENAME_ è il nome del file in cui memorizzare la tua chiave di crittografia dei dati.

  Ad esempio, se il tuo file RAW è _Rhel_7.encrypted.raw_ e il file chiave è _secret.dek_, immetti:

  ```
  $ cryptsetup -v luksFormat Rhel_7.encrypted.raw secret.dek
  ```
  {: screen}

  Dopo aver eseguito il comando cryptsetup, riceverai la seguente richiesta. Si tratta di una richiesta prevista e devi rispondere YES per continuare.

  ```
  WARNING!
  ========
  This will overwrite data on Rhel_7.encrypted.raw irrevocably.
  Are you sure (Type uppercase yes): YES
  ```
  {: screen}

7. Collega il file RAW crittografato come un volume per associare un nuovo dispositivo a blocchi al sistema operativo. Utilizzando _cryptsetup_, esegui il seguente comando per aprire il dispositivo RAW crittografato utilizzando l'opzione luksOpen.

  Devi disporre del privilegio sudo per eseguire il seguente comando utilizzando l'autorizzazione utente `root`.
  {: tip}

  ```
  sudo cryptsetup -v luksOpen <ENCRYPTED_RAW_FILENAME> <VOLUME_NAME> --key-file <DEK_FILENAME>
  ```
  {: pre}

  Dove _ENCRYPTED_RAW_FILENAME_ è il nome file del file RAW crittografato, _VOLUME_NAME_ è il nome del dispositivo che _dmcrypt_ tenterà di creare e _DEK_FILENAME_ è il nome file della tua chiave di crittografia dei dati.

  Ad esempio, se il tuo file RAW crittografato è _Rhel_7.encrypted.raw_, il nome che vuoi utilizzare per il dispositivo a blocchi è _encryptedVolume_ e il file della chiave di crittografia dei dati è _secret.dek_, utilizza il seguente comando:

  ```
  $ sudo cryptsetup -v luksOpen Rhel_7.encrypted.raw encryptedVolume --key-file secret.dek
  ```
  {: screen}

  Una volta completato questo passo, in _/dev/mapper/_ viene creato un nuovo dispositivo a blocchi da cui puoi leggere e in cui puoi scrivere.

8. Conferma che il mapper volume LUKS è stato creato correttamente eseguendo il seguente comando:

  ```
  ls -l /dev/mapper/
  ```
  {: pre}

  Dovresti vedere un output simile al seguente esempio:

  ```
  total 0
  lrw-rw---- 1 root root        7 Mar 5 22:22 encryptedVolume -> ../dm-0
  ```
  {: screen}

9. Copia l'immagine non crittografata (dal passo 2) nel dispositivo volume crittografato (dal passo 7). Esegui il seguente comando _dd_ per copiare l'immagine non crittografata nel volume crittografato:

  ```
  sudo dd if=<RAW_IMAGE_FILE> of=/dev/mapper/<VOLUME_NAME>
  ```
  {: pre}

  Dove _RAW_IMAGE_FILE_ è il nome file dell'immagine RAW non crittografata (creato nel passo 2) e _VOLUME_NAME_ è il nome del dispositivo a blocchi del volume crittografato (creato nel passo 7).

  Ad esempio, se il tuo RAW_IMAGE_FILE è denominato _Rhel_7.raw-0.raw_ e il tuo VOLUME_NAME è _encryptedVolume_, esegui il seguente comando:

  ```
  $ sudo dd if=Rhel_7.raw-0.raw of=/dev/mapper/encryptedVolume status=progress
  ```
  {: screen}

  Il completamento di questo comando potrebbe richiedere minimo 30 minuti.
  {: tip}

10. Elimina permanentemente il mapper volume e chiudi la connessione LUKS al file di dati crittografato:

  ```
  sudo cryptsetup luksClose encryptedVolume
  ```
  {: pre}

  Dove _encryptedVolume_ è il nome del dispositivo a blocchi del volume crittografato.

  Ora il tuo _ENCRYPTED_RAW_FILENAME_ è inizializzato e puoi caricarlo in IBM Cloud Object Storage. Ad esempio, se il tuo file RAW crittografato è _Rhel_7.encrypted.raw_, carica tale immagine in IBM Cloud Object Storage.
