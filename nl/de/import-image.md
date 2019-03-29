---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-17"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Images vorbereiten und importieren
{: #preparing-and-importing-images}

Über die Anzeige "Imagevorlagen" in {{site.data.keyword.slportal_full}} können Benutzer ein Image aus einer Serviceinstanz von [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage) importieren. Nachdem ein Image in ein Bucket in einer Serviceinstanz von {{site.data.keyword.cos_full_notm}} hochgeladen worden ist, können Sie es im {{site.data.keyword.slportal}} in das Repository für Imagevorlagen hochladen.
{:shortdesc}

Um Images von {{site.data.keyword.cos_full_notm}} hochladen zu können, müssen Sie über ein Konto verfügen, für das ein Upgrade durchgeführt wurde. Weitere Informationen finden Sie unter [Zur IBMid wechseln und Konten verknüpfen](/docs/account?topic=account-unifyingaccounts).
{: tip}

Nach dem Importieren von Images als Imagevorlagen können diese zum Bereitstellen oder Starten eines virtuellen Servers verwendet werden. Images, die aus einer Serviceinstanz von {{site.data.keyword.cos_full_notm}} importiert wurden, können entweder VHD-Images oder angepasste ISO-Images sein. VHD-Importe beschränken sich auf die folgenden 64-Bit-Betriebssysteme:  

* CentOS 6 und 7
* RedHat Enterprise Linux 6 und 7
* Ubuntu 14.04 und 16.04
* Microsoft Server Standard 2012, R2 2012 und 2016

VHD-Importe sind auf 100-GB-Datenträger beschränkt. VHDs müssen wie im folgenden Beispiel benannt werden: dateiname.vhd-0.vhd.

## Images in VHD-Format konvertieren
{: #convert-to-vhd}

Das Format 'VHD' (Virtual Hard Disk) ist das einzige Imageformat, das für virtuelle Server unterstützt wird. Um Images in das VHD-Format zu konvertieren, verwenden Sie die folgenden Informationen:

* Qemu-img 2.7.0 oder höher ist erforderlich
* Konvertieren Sie das Image mit dem folgenden Befehl:

  ```
  qemu-img convert -f <image format> <image name> -O vpc -o force_size <image name>
  ```
  {: pre}

* Beispielbefehl:

  ```
  qemu-img convert -f qcow2 test -O vpc -o force_size test
  ```
  {: pre}

Weitere Informationen finden Sie in der QEMU-Dokumentation zum [Konvertieren von Imageformaten ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats).

## ISO-Vorlagen
{: #iso-templates}

Nur unterstützte {{site.data.keyword.BluSoftlayer_notm}}-Betriebssysteme können zum Laden einer ISO-Vorlage in eine VSI verwendet werden. Eine Liste der unterstützten Betriebssysteme finden Sie hier: [https://www.ibm.com/cloud/server-software ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/server-software).

ISO-Images, die mit diesem Tool importiert werden, müssen bootfähig sein. Anderenfalls ist das Image für den Import nicht zulässig.

## Image für {{site.data.keyword.virtualmachinesshort}} konfigurieren
{: #config-image-vsi}

Um sicherzustellen, dass ein Image in der {{site.data.keyword.BluSoftlayer_notm}}-Infrastrukturumgebung erfolgreich bereitgestellt werden kann, müssen Images des virtuellen Servers gemäß den folgenden Spezifikationen konfiguriert werden:

* ***/boot*** muss die erste Partition sein.
* ***/boot*** und ***/*** muss ein ext3- oder ext4-Dateisystem sein.
* ***/etc***  und /root müssen sich auf derselben Partition befinden wie ***/***.
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :***, um die am System angehängte Auslagerungsplatte zu mounten.
* "wget" muss installiert werden.
* Die aktuellen Xen-Tools "xe-guest-utilities" müssen installiert sein. Führen Sie die folgenden Schritte aus:

    1. Laden Sie das XenServer-ISO-Image von Citrix herunter: [https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html)

    2. Hängen Sie das ISO-Image mit dem folgenden Befehl an:

        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}

    3. Suchen Sie das RPM für das ISO-Image, indem Sie den folgenden Befehl ausführen:

        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}

    4. Die Ausgabe listet ein RPM wie dieses hier auf:

        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}

     5. Sie können das RPM kopieren und dann das ISO-Image für xentools extrahieren. Führen Sie den folgenden Befehl aus, um eine Verzeichnisstruktur für das ISO-Image zu erstellen:

        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}

     6. In dieser erstellten Verzeichnisstruktur können Sie das ISO-Image *guest-tools* mounten. Suchen Sie *rpm/debs*, um xentools zu installieren. Beispiel-Verzeichnisstruktur:

        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}

Weitere Informationen zu cloud-init-fähige Images finden Sie unter [Bereitstellung mit einem cloud-init-fähigen Image](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image).

## Image auf {{site.data.keyword.cos_full_notm}} hochladen
{: #upload-to-ibm-cos}

Wenn Ihr Image bereitsteht, können Sie es auf {{site.data.keyword.cos_full_notm}} hochladen. Stellen Sie sicher, dass Sie an einem [regionalen Standort](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#select-regions-and-endpoints) ein Bucket verwenden.

1. Navigieren Sie in {{site.data.keyword.cos_full_notm}} zu Ihrem Bucket und klicken Sie auf **Objekte hinzufügen**, um das Image [hochzuladen](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data).
2. Verwenden Sie das Plug-in [Aspera](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#Aspera-high-speed-transfer) für die Hochgeschwindigkeitsübertragung, um die maximale Uploadgeschwindigkeit zum Hochladen Ihres Image nutzen zu können.

Sie können das SDK für Cloud Object Storage in Verbindung mit Aspera nutzen, um die Hochgeschwindigkeitsübertragung innerhalb Ihrer angepassten Anwendungen zu initiieren, wenn Sie Java, Python oder NodeJS verwenden. Weitere Informationen finden Sie unter [Bibliotheken und SDKs verwenden](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#sdk).
{: tip}


## Image aus IBM Cloud Object Storage importieren
{: #import-icos}

Führen Sie zum Importieren eines Image aus{{site.data.keyword.cos_full_notm}} die folgenden Schritte aus.

1. Öffnen Sie im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) die Seite **Imagevorlagen**, indem Sie die Optionen **Einheiten > Verwalten > Images** auswählen.
2. Klicken Sie auf die Registerkarte **Image von IBM COS importieren**, um das Importtool zu öffnen.
3. Füllen Sie die Pflichtangabefelder aus (siehe Tabelle 1).
4. Wenn der Vorgang für den Import aus {{site.data.keyword.cos_full_notm}} abgeschlossen ist, wird das Image auf der Seite "Imagevorlagen" angezeigt.

| Feld | Wert |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Wählen Sie die Serviceinstanz von {{site.data.keyword.cos_full_notm}} aus, in der das Image gespeichert ist, das Sie importieren möchten. |
| Standort | Wählen Sie die spezifische geografische Region aus, in der Ihr Image gespeichert ist. Sie können Images in die folgenden Regionen und die zugehörigen Rechenzentren importieren: US South (DAL13), US East (WDC07), EU Great Britain (LON02), EU Germany (FRA02), AP Japan (TOK02). Nachdem das Image in eines der aufgelisteten Rechenzentren importiert worden ist, können Sie es in ein anderes Rechenzentrum verschieben. |
| Bucket | Wählen Sie das {{site.data.keyword.cos_full_notm}}-Bucket aus, in dem Ihr Image gespeichert ist. Es sind nur Buckets gültig, die innerhalb des von Ihnen ausgewählten Standorts vorhanden sind. Wenn Sie ein Bucket auswählen, das nicht am ausgewählten Standort vorhanden ist, erhalten Sie eine Fehlernachricht.|
| Imagedatei | Wählen Sie in der {{site.data.keyword.cos_full_notm}}-Serviceinstanz die Imagedatei aus, die Sie importieren wollen. Es werden die Dateitypen VHD, ISO und RAW unterstützt. Wenn Sie ein verschlüsseltes Image importieren, muss das Image im Dateiformat RAW vorliegen und mit LUKS-Plattenverschlüsselung verschlüsselt sein. |
| Imagename | Geben Sie einen beschreibenden Namen für Ihr Image an. Dabei handelt es sich um das Image, das Sie für die Bereitstellung von virtuellen Serverinstanzen (VSIs) verwenden werden. |
| API-Schlüssel | Geben Sie den API-Schlüssel an, der den Zugriff auf Ihre {{site.data.keyword.cos_full_notm}}-Serviceinstanz ermöglicht. Wenn ein verschlüsseltes Image importiert wird, muss der API-Schlüssel auch Zugriff auf Key Protect haben. Der API-Schlüssel steht nur zum Zeitpunkt der Erstellung zum Kopieren oder Herunterladen zur Verfügung. Wenn der API-Schlüssel verloren geht, müssen Sie einen neuen API-Schlüssel erzeugen. Weitere Informationen finden Sie unter [Mit API-Schlüsseln arbeiten](/docs/iam?topic=iam-manapikey). |
| Betriebssystem | Wählen Sie das Betriebssystem aus, das in Ihrem Image enthalten ist. Bei verschlüsselten Images sind nur Linux-Betriebssysteme als Auswahl gültig. |
| Cloud-init | Wenn das Image, das Sie importieren, cloud-init-fähig ist, wählen Sie dieses Kontrollkästchen aus. Wenn Sie ein Image importieren, das ein cloud-init-fähiges Windows-Betriebssystem umfasst, und Sie wählen diese Option aus, können Sie nicht auch noch **Ihre Lizenz** angeben. Wenn Sie ein verschlüsseltes Image importieren, ist diese Option standardmäßig ausgewählt und kann nicht bearbeitet werden, da Ihr verschlüsseltes Image cloud-init-fähig sein muss. |
| Ihre Lizenz | Wenn Sie beabsichtigen, Ihre eigene Betriebssystemlizenz anzugeben, wählen Sie dieses Kontrollkästchen aus. Wenn Sie ein Image mit einem Windows-Betriebssystem importieren, können Sie diese Option auswählen, falls Sie vorhaben, das Image für die Bereitstellung von [dedizierte Hostinstanzen](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances) zu verwenden. Falls Ihre Version des Betriebssysteme Windows die Verwendung einer eigenen Lizenz nicht unterstützt, ist diese Option inaktiviert. Bei Windows-Images können Sie "Cloud-Init" nicht auswählen, wenn Sie angeben, dass Sie Ihre eigene Lizenz verwenden möchten. Wenn Sie ein verschlüsseltes Image mit Red Hat Enterprise Linux als Betriebssystem importieren, ist diese Option standardmäßig ausgewählt und kann nicht bearbeitet werden, da Ihr verschlüsseltes Image eine eigene Betriebssystemlizenz einschließen muss. |
| Bootmodus | Wählen Sie den Bootmodus für Ihr Image aus. Wenn für das von Ihnen angegebene Betriebssystem ein Standardbootmodus festgelegt ist, wird dieser hier automatisch ausgewählt. |
| Anmerkungen | Fügen Sie alle Anmerkungen in Bezug auf das Image hinzu, die für Benutzer hilfreich sein könnten. |
| Verschlüsselung | Die Auswahl für dieses Kontrollkästchen wird durch den Dateityp des Image bestimmt, das Sie zum Importieren auswählen. VHD- und ISO-Images geben an, dass die Imagedatei nicht verschlüsselt ist. Daher ist das Kontrollkästchen für VHD- und ISO-Images nicht ausgewählt. Eine Imagedatei im Dateiformat RAW weist darauf hin, dass es sich bei dem Image um ein verschlüsseltes Image handelt. Bei Angabe einer Imagedatei im Dateiformat RAW ist dieses Kontrollkästchen standardmäßig ausgewählt und kann nicht bearbeitet werden. |
{: caption="Tabelle 1. Werte für den Import eines Image aus IBM Cloud Object Storage" caption-side="top"}

In der folgenden Tabelle sind zusätzliche Felder angegeben, die nur für den Import verschlüsselter Images gelten. Weitere Informationen zu verschlüsselten Images finden Sie unter [End-to-End-Verschlüsselung (E2E) für die Bereitstellung einer verschlüsselten Instanz verwenden](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

Damit ein verschlüsseltes Image importiert werden kann, muss Ihr Konto über Zugriff auf das Feature für End-to-End-Verschlüsselung (E2E) verfügen. Wenn Ihr Konto für die End-to-End-Verschlüsselung (E2E) aktiviert werden soll, fordern Sie Unterstützung an.
{: tip}

| Feld | Wert |
| ----- | ----- |
| ID der {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz | Wenn ein verschlüsseltes Image importiert wird, muss sich Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz in derselben Region befinden wie Ihr {{site.data.keyword.cos_full_notm}}-Bucket. Über die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.cloud_notm}} können Sie die ID für Ihre Instanz von {{site.data.keyword.keymanagementserviceshort}} suchen. Weitere Informationen finden Sie unter [Instanz-ID abrufen](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID). |
| Durch Wrapping erstellter Datenverschlüsselungsschlüssel | Geben Sie beim Importieren eines verschlüsselten Image den verschlüsselten Text an, der dem Datenverschlüsselungsschlüssel zugeordnet ist, den Sie zum Verschlüsseln Ihres Image verwendet haben. Weitere Informationen finden Sie unter [Wrapping für Schlüssel über die API durchführen](/docs/services/key-protect?topic=key-protect-wrap-keys#api). |
| Rootschlüssel-ID | Geben Sie beim Importieren eines verschlüsselten Image die ID des Rootschlüssels an, der für das Key-Wrapping des Datenverschlüsselungsschlüssels verwendet wurde. Weitere Informationen finden Sie unter [Schlüssel anzeigen](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
{: caption="Tabelle 2. Werte für den Import eines verschlüsselten Image aus IBM Cloud Object Storage" caption-side="top"}

## Nächste Schritte

Nachdem der Importvorgang gestartet worden ist, sucht das System in der Serviceinstanz von {{site.data.keyword.cos_full_notm}} die Imagedatei im angegebenen Bucket. Die Imagedatei wird als Imagevorlage importiert, auf die dann auf der Seite "Imagevorlagen" zugegriffen werden kann. Wenn der Import abgeschlossen ist, kann das Image verwendet werden, um eine neue Einheit zu bestellen oder eine Einheit zu starten.
Die Imagevorlage kann außerdem jederzeit gelöscht werden. Die Dauer für den Imageimport variiert je nach Dateigröße, dauert aber in der Regel mehrere Minuten bis zu einer Stunde.

Nachdem ein Image in das Repository für Imagevorlagen importiert worden ist, können Sie es aus {{site.data.keyword.cos_full_notm}} löschen. Über die Seite **Imagevorlagen** können Sie auch weiterhin auf die Imagevorlage zugreifen und diese für die Bereitstellung virtueller Serverinstanzen verwenden.
