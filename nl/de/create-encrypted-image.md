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


# Verschlüsseltes Image erstellen
{: #creating-an-encrypted-image}

Als Teil der E2E-Verschlüsselungsfunktion können Sie ein Image, das in die Imagevorlagen importiert werden soll, verschlüsseln und es zur Bereitstellung einer verschlüsselten virtuellen Serverinstanz verwenden.
{:shortdesc}

## Voraussetzungen für verschlüsselte Images
{: #encrypted-image-reqs}

Ein von Ihnen erstelltes verschlüsseltes Image muss die folgenden Voraussetzungen für Images erfüllen:

* Das Image ist mit der Infrastrukturumgebung der {{site.data.keyword.cloud}}-Konsole kompatibel.
* Das Image enthält ein Linux-Betriebssystem wie CentOS, Debian, Red Hat Enterprise Linux oder Ubuntu.
* Das Image ist cloud-init-fähig.
* Das Image ist mit [LUKS-Verschlüsselung für Platten](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption) verschlüsselt.

## QEMU und DM-Crypt zum Erstellen eines verschlüsselten RAW-Images verwenden
{: #luks-disk-encryption}

Zum Verschlüsseln Ihres Images müssen Sie eine VHD-Imagedatei mit fester Größe in das Dateiformat RAW konvertieren. Dann verwenden Sie QEMU und DM-Crypt, um eine neue Imagedatei mit LUKS-Plattenverschlüsselung (LUKS - Linux Unified Key Setup-on-disk-format) zu erstellen. Im Anschluss hängen Sie die Datei als verschlüsselten Datenträger an und kopieren die nicht verschlüsselte Imagedatei in den verschlüsselten Datenträger.

Bei dieser Prozedur erfahren Sie Schritt für Schritt, wie Sie die folgenden Tasks ausführen:

* Ihr VHD-Image mit dynamischer Größe in ein VHD-Image mit fester Größe konvertieren
* Ihr VHD-Image mit fester Größe in RAW-Dateiformat konvertieren
* Datei mit dem Datenverschlüsselungsschlüssel erstellen, die Sie zum Verschlüsseln des Laufwerks verwenden werden
* Neue RAW-Datei erstellen, die das Image sowie den LUKS-Verschlüsselungsheader enthalten wird
* RAW-Datei mit LUKS-Plattenverschlüsselung formatieren
* Verschlüsselte RAW-Datei an eine Blockeinheit anhängen
* Das nicht verschlüsselte Image in die verschlüsselte Datenträgereinheit kopieren

Sie müssen über die Berechtigung zum Ausführen von Befehlen mit der Benutzerberechtigung `root` über sudo verfügen, um verschlüsselte Datenträger über eine Mountoperation an Ihr System anhängen bzw. von diesem abhängen zu können. Durch Absetzen des folgenden Befehls können Sie Ihre `root`-Berechtigung überprüfen:

```
sudo echo "Hello!"
```
{: pre}

Damit diese Verschlüsselungstask als erfolgreich abgeschlossen gilt, muss die Antwort "Hello!" als Echo zurückgegeben werden.

**Tipp**: Sie müssen diese Verschlüsselungstask auf einem System durchführen, auf dem als Betriebssystem Linux ausgeführt wird und die folgenden Pakete verfügbar sind:
* qemu oder qemu-image (je nach dem von Ihnen verwendeten Betriebssystem Linux)
* cryptsetup

Verschlüsseln Sie Ihr Image, indem Sie die folgenden Schritte ausführen.

1. Konvertieren Sie Ihre VHD-Imagedatei mit dynamischer Größe in eine VHD-Imagedatei mit fester Größe. Eine VHD-Datei mit fester Größe verhindert Beschädigungen, die auftreten können, wenn eine VHD-Datei mit dynamischer Größe direkt in das Dateiformat RAW konvertiert wird. Führen Sie den folgenden Befehl aus, um die VHD-Imagedatei mit dynamischer Größe in eine VHD-Imagedatei mit fester Größe zu konvertieren:

  ```
  qemu-img convert -p -O vpc -o subformat=fixed <VHD-DATEI_MIT_DYNAMISCHER_GRÖSSE> <VHD-DATEI_MIT_FESTER_GRÖSSE>
  ```
  {: pre}

  Wenn Ihre VHD-Datei mit fester Größe den Namen _Rhel_7.vhd-0.vhd_ hat, die VHD-Datei mit fester Größe jedoch den Namen _Rhel_7.fixed.vhd-0.vhd_ erhalten soll, führen Sie den folgenden Befehl aus:

  ```
  $ qemu-img convert -p -O vpc -o subformat=fixed Rhel_7.vhd-0.vhd Rhel_7.fixed.vhd-0.vhd
  ```
  {: screen}

  Die Ausführung dieses Befehls kann 30 Minuten oder länger in Anspruch nehmen.
  {: tip}

2. Konvertieren Sie die Datei für Ihre VHD-Imagedatei mit fester Größe in das Dateiformat RAW. Verwenden Sie dazu QEMU. Das Image muss das Dateiformat RAW aufweisen, bevor es verschlüsselt werden kann. Führen Sie den folgenden QEMU-Befehl aus:

  ```
  qemu-img convert -p -O raw <VHD-DATEI_MIT_FESTER_GRÖSSE> <RAW-IMAGEDATEI>
  ```
  {: pre}

  Wenn Ihre VHD-Datei mit fester Größe zum Beispiel den Namen _Rhel_7.fixed.vhd-0.vhd_ hat, die RAW-Ausgabedatei jedoch den Namen _Rhel_7.raw-0.raw_ erhalten soll, führen Sie den folgenden Befehl aus:

  ```
  qemu-img convert -p -O raw Rhel_7.fixed.vhd-0.vhd Rhel_7.raw-0.raw
  ```
  {: screen}

  Die Ausführung dieses Befehls kann 30 Minuten oder länger in Anspruch nehmen.
  {: tip}

3. Geben Sie den Datenverschlüsselungsschlüssel an, den Sie zum Ver- und Entschlüsseln des Laufwerks verwenden werden. Bei diesem Datenverschlüsselungsschlüssel handelt es sich um denselben Schlüssel, den Sie per Key-Wrapping erstellen und angeben, wenn Sie das verschlüsselte Image in {{site.data.keyword.slportal}} importieren. Erstellen Sie eine Datei, die den Datenverschlüsselungsschlüssel enthält, den Sie zum Ver- und Entschlüsseln des Laufwerks verwenden werden. In dieser Datei muss das Key-Wrapping für den Schlüssel aufgehoben sein und der Schlüssel muss als Text in Base64-Codierung vorliegen. Die Base64-Codierung hilft sicherzustellen, dass keine unbeabsichtigten Leerzeichen oder Umbrüche enthalten sind. Der mit Base64-Codierung codiert Datenverschlüsselungsschlüssel muss mindestens aus 32 Zeichen oder Byte bestehen und darf nicht mehr als 512 Zeichen oder Byte aufweisen. Die Informationen für den Datenverschlüsselungsschlüssel müssen sich durchgängig in einer Zeile ohne Umbrüche oder Zeilenvorschubzeichen befinden. Verwenden Sie zum Beispiel den nachfolgend aufgeführten Befehl, um eine Datei namens `secret.dek` zum Speichern von `unwrapped_key_material` (d. h. den Schlüsselinformationen ohne Key-Wrapping) für Ihren Datenverschlüsselungsschlüssel zu erstellen und sie mit Base64 zu codieren:

  ```
  $ echo -n $(echo 'unwrapped_key_material' | base64) > secret.dek
  ```
  {: screen}

  Bewahren Sie diesen Schlüssel sorgfältig an einem sicheren Ort auf. Wenn Sie diesen Schlüssel verlieren, können Sie die Platte nicht entschlüsseln.
  {: tip}

4. Geben Sie die richtige Größe für die neue RAW-Datei an, die im folgenden Schritt für die verschlüsselte Platte erstellt wird. Berücksichtigen Sie bei der Berechnung die Größe eines LUKS-Headers, der hinzugefügt wird. _DM-Crypt_ und _Cryptsetup_ verwenden die neue RAW-Datei zum Speichern des Platteninhalts in LUKS-Verschlüsselung (LUKS - Linux Unified Key Setup-on-disk-format). Die Größe der neuen RAW-Datei errechnet sich als die Summe der Größe der in Schritt 2 erstellten RAW-Datei und dem konstanten Wert von 4 MB für den LUKS-Header. Sie können die Größe Ihres vorhandenen RAW-Image durch Ausführen des folgenden Befehls ermitteln, wobei _Rhel_7.raw-0.raw_ der Name des Image ist:

  ```
  ls -l Rhel_7.raw-0.raw
  ```
  {: pre}

  Es wird eine Ausgabe ähnlich der folgenden angezeigt:

  ```
  -rw-r--r--. 1 user user 42949017600 Mar 5 10:02 Rhel_7.raw-0.raw
  ```
  {: screen}

  Dabei steht _42949017600_ für die Anzahl von Byte, die das RAW-Image belegt.
      1. Rechnen Sie (in Byte umgerechnet) 4 MB zur Größe der RAW-Imagedatei hinzu, um den LUKS-Header zu berücksichtigen. Beispiel: 42949017600 + (4 x 1024 x 1024) = 42953211904 Byte.
      2. Rechnen Sie die Summe der RAW-Imagegröße und des LUKS-Headers in Megabyte um und runden Sie auf das nächste Megabyte auf. Beispiel: 42953211904 Byte / 1024 Byte / 1024 Kilobyte = 40963.375 Megabyte = **40964 MB**.

5. Erstellen Sie eine neue RAW-Datei mit der richtigen Bytegröße, die künftig als verschlüsseltes RAW-Image verwendet wird. Verwenden Sie zum Erstellen der RAW-Datei den folgenden _dd_-Befehl:

  ```
  dd if=/dev/zero of=<NAME_DER_VERSCHLÜSSELTEN_RAW-DATEI> bs=1M count=<ZIELGRÖSSE_IN_MEGABYTE> status=progress
  ```
  {: pre}

  Dabei gibt _NAME_DER_VERSCHLÜSSELTEN_RAW-DATEI_ die Datei an, die zum verschlüsselten RAW-Image wird, und _ZIELGRÖSSE_IN_MEGABYTE_ ist die Zahl, die Sie in Schritt 4 errechnet haben, d. h. die Summe aus der Größe des nicht verschlüsselten RAW-Image und dem LUKS-Header.

  Wenn zum Beispiel Ihre neue RAW-Datei, die zum verschlüsselten Image wird, die Datei _Rhel_7.encrypted.raw_ mit der Zielgröße von _40964_ MB sein soll, führen Sie den folgenden Befehl aus:

  ```
  $ dd if=/dev/zero of=Rhel_7.encrypted.raw bs=1M count=40964 status=progress
  ```
  {: screen}

6. Formatieren Sie die RAW-Datei mit LUKS-Plattenverschlüsselung. Verwenden Sie dazu _dmcrypt_ und Ihren Datenverschlüsselungsschlüssel (aus Schritt 3). Dieser Schritt bereitet die Datei vor, die dann das Image enthalten wird. Formatieren Sie die Datei mit LUKS-Verschlüsselung, indem Sie den folgenden _cryptsetup_-Befehl ausführen:

  ```
  cryptsetup -v luksFormat <NAME_DER_VERSCHLÜSSELTEN_RAW-DATEI> <NAME_DER_DVS-DATEI>
  ```
  {: pre}

  Dabei gibt _NAME_DER_VERSCHLÜSSELTEN_RAW-DATEI_ den Namen der RAW-Datei an, die verschlüsselt werden soll, und _NAME_DER_DVS-DATEI_ steht für den Namen der Datei, in der Ihr Datenverschlüsselungsschlüssel (DVS) gespeichert ist.

  Wenn Ihre RAW-Datei zum Beispiel _Rhel_7.encrypted.raw_ und die Schlüsseldatei _secret.dek_ heißt, muss die entsprechende Eingabe wie folgt aussehen:

  ```
  $ cryptsetup -v luksFormat Rhel_7.encrypted.raw secret.dek
  ```
  {: screen}

  Nach der Ausführung des Befehls 'cryptsetup' wird die folgende Eingabeaufforderung angezeigt. Diese Eingabeaufforderung wird erwartet und zum Fortfahren müssen Sie durch die Eingabe von YES antworten.

  ```
  WARNING!
  ========
  This will overwrite data on Rhel_7.encrypted.raw irrevocably.
  Are you sure (Type uppercase yes): YES
  ```
  {: screen}

7. Hängen Sie die verschlüsselte RAW-Datei als Datenträger an, um dem Betriebssystem eine Blockeinheit zuordnen zu können. Führen Sie mit _cryptsetup_ den folgenden Befehl aus, um die verschlüsselte RAW-Einheit mit der Option 'luksOpen' zu öffnen.

  Sie müssen über die Berechtigung 'sudo' verfügen, um den folgenden Befehl unter Verwendung der Benutzerberechtigung `root` ausführen zu können.
  {: tip}

  ```
  sudo cryptsetup -v luksOpen <NAME_DER_VERSCHLÜSSELTEN_RAW-DATEI> <DATENTRÄGERNAME> --key-file <NAME_DER_DVS-DATEI>
  ```
  {: pre}

  Dabei gibt _NAME_DER_VERSCHLÜSSELTEN_RAW-DATEI_ den Namen der verschlüsselten RAW-Datei an und _DATENTRÄGERNAME_ steht für den Namen der Einheit, die mit _dmcrypt_ erstellt werden soll, während _NAME_DER_DVS-DATEI_ für den Namen der Datei mit dem Datenverschlüsselungsschlüssel steht.

  Wenn Ihre verschlüsselte RAW-Datei zum Beispiel _Rhel_7.encrypted.raw_ heißt, Sie der Blockeinheit den Namen _encryptedVolume_ geben wollen und die Datei mit dem Datenverschlüsselungsschlüssel den Namen _secret.dek_ trägt, müssen Sie den folgenden Befehl verwenden:

  ```
  $ sudo cryptsetup -v luksOpen Rhel_7.encrypted.raw encryptedVolume --key-file secret.dek
  ```
  {: screen}

  Wenn dieser Schritt ausgeführt worden ist, wird unter _/dev/mapper/_ eine neue Blockeinheit erstellt, aus der Sie Daten lesen und in die Sie Daten schreiben können.

8. Stellen Sie sicher, dass der LUKS-Datenträgermapper erfolgreich erstellt wurde. Führen Sie dazu den folgenden Befehl aus:

  ```
  ls -l /dev/mapper/
  ```
  {: pre}

  Die angezeigte Ausgabe sollte dem folgenden Beispiel ähnlich sehen:

  ```
  total 0
  lrw-rw---- 1 root root        7 Mar 5 22:22 encryptedVolume -> ../dm-0
  ```
  {: screen}

9. Kopieren Sie das nicht verschlüsselte Image (aus Schritt 2) in die verschlüsselte Datenträgereinheit (aus Schritt 7). Kopieren Sie das nicht verschlüsselte Image mit dem folgenden _dd_-Befehl auf den verschlüsselten Datenträger:

  ```
  sudo dd if=<RAW-IMAGEDATEI> of=/dev/mapper/<DATENTRÄGERNAME>
  ```
  {: pre}

  Dabei gibt _RAW-IMAGEDATEI_ den Namen des (in Schritt 2 erstellten) nicht verschlüsselten RAW-Image an und _DATENTRÄGERNAME_ steht für den Namen der (in Schritt 7 erstellten) verschlüsselten Datenträgerblockeinheit.

  Wenn Ihre RAW-IMAGEDATEI zum Beispiel den Namen _Rhel_7.raw-0.raw_ hat und DATENTRÄGERNAME den Wert _encryptedVolume_ hat, müssen Sie den folgenden Befehl ausführen:

  ```
  $ sudo dd if=Rhel_7.raw-0.raw of=/dev/mapper/encryptedVolume status=progress
  ```
  {: screen}

  Die Ausführung dieses Befehls kann 30 Minuten oder länger in Anspruch nehmen.
  {: tip}

10. Zerstören Sie den Datenträgermapper und schließen Sie die LUKS-Verbindung zu der verschlüsselten Datendatei wie folgt:

  ```
  sudo cryptsetup luksClose encryptedVolume
  ```
  {: pre}

  Dabei steht _encryptedVolume_ für den Namen der verschlüsselten Datenträgerblockeinheit.

  Ihre Datei '_NAME_DER_VERSCHLÜSSELTEN_RAW-DATEI_' ist nun initialisiert und Sie können sie in IBM Cloud Object Storage hochladen. Wenn Ihre verschlüsselte RAW-Datei zum Beispiel _Rhel_7.encrypted.raw_ heißt, sollten Sie dieses Image in IBM Cloud Object Storage hochladen.
