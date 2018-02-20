---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-31"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Images vorbereiten und importieren

Die Anzeige "Imagevorlagen" im [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) ermöglicht Benutzern, ein vorhandenes Image von einem Swift-basierten [Object Storage](/docs/infrastructure/objectstorage-swift/index.html)-Konto hochzuladen.
{:shortdesc}

Nach dem Importieren von Images als Imagevorlagen können sie zum Bereitstellen oder Starten eines virtuellen Servers verwendet werden. Images, die aus einem Object Storage-Konto importiert wurden, können entweder VHDs oder angepasste ISO-Images sein. VHD-Importe beschränken sich auf die folgenden 64-Bit-Betriebssysteme:

* CentOS 6 und 7
* RedHat Enterprise Linux 6 und 7
* Ubuntu 14.04 und 16.04
* Microsoft Server Standard 2012, R2 2012 und 2016

VHD-Importe sind auf 100-GB-Datenträger beschränkt. VHDs müssen wie im folgenden Beispiel benannt werden: dateiname.vhd-0.vhd.

## Images in das VHD-Format konvertieren

Das VHD-Format ist das einzige Supportimageformat für {{site.data.keyword.BluVirtServers_full}}. Um Images in das VHD-Format zu konvertieren, verwenden Sie die folgenden Informationen:

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

Nur unterstützte {{site.data.keyword.BluSoftlayer_notm}}-Betriebssysteme können zum Laden einer ISO-Vorlage in eine VSI verwendet werden. Die Liste unterstützter Betriebssysteme finden Sie hier: [http://www.softlayer.com/services/software/ ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.softlayer.com/services/software/)

ISO-Images, die mit diesem Tool importiert werden, müssen bootfähig sein. Anderenfalls ist das Image für den Import nicht zulässig. 

## Ein Image für {{site.data.keyword.virtualmachinesshort}} konfigurieren

Um sicherzustellen, dass ein Image in der {{site.data.keyword.BluSoftlayer_notm}}-Infrastrukturumgebung erfolgreich bereitgestellt werden kann, müssen Images des virtuellen Servers gemäß den folgenden Spezifikationen konfiguriert werden:

* ***/boot*** muss die erste Partition sein.
* ***/boot*** und ***/*** muss ein ext3- oder ext4-Dateisystem sein.
* ***/etc***  und /root müssen sich auf derselben Partition befinden wie ***/***.
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :***, um die am System angehängte Auslagerungsplatte zu mounten.
* "wget" muss installiert werden.
* Die aktuellen Xen-Tools "xe-guest-utilities" müssen installiert sein. Führen Sie die folgenden Schritte aus:
    
    1. Laden Sie das XenServer-ISO-Image von Citrix herunter: [https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html)
    
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
    
Weitere Informationen zu cloud-init-fähige Images finden Sie unter [Bereitstellung mit einem cloud-init-fähigen Image](image_cloud-init.html).

## Ein Image importieren

Führen Sie die folgenden Schritte aus, um ein Image in das Kundenportal zu importieren.

1. Suchen Sie die folgenden Details für das Image aus dem {{site.data.keyword.objectstorageshort}}-Konto und notieren Sie sich. Weitere Informationen finden Sie unter [Object Storage-Dateiinformationen anzeigen und bearbeiten](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html).
  * Kontoname
  * Cluster
  * Container
  * Image-Dateiname
2. Greifen Sie im [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) auf die Seite **Imagevorlagen** zu, indem Sie **Einheiten > Verwalten > Images** auswählen.
3. Klicken Sie auf die Registerkarte **Image importieren**, um das Importtool zu öffnen.
4. Wählen Sie das **{{site.data.keyword.objectstorageshort}}-Konto** über die Dropdown-Liste **Konto** für das Image aus, das Sie importieren möchten.
5. Wählen Sie das **{{site.data.keyword.objectstorageshort}}-Cluster** über die Dropdown-Liste **Cluster** für das Image aus, das Sie importieren möchten.
6. Wählen Sie den **{{site.data.keyword.objectstorageshort}}-Container** über die Dropdown-Liste **Container** für das Image aus, das Sie importieren möchten.
7. Wählen Sie den **Image-Dateinamen** aus der Dropdown-Liste **Image File** aus, der in {{site.data.keyword.objectstorageshort}} aufgeführt ist.
8. Geben Sie in das Feld **Imagename** einen Namen für die neue Imagevorlage ein.
9. Geben Sie die gewünschten Anmerkungen in das Textfeld **Anmerkungen** ein.
10. Wählen Sie aus der Dropdown-Liste **Betriebssystem** das Betriebssystem des Images aus.

  Die Dropdown-Liste "Betriebssystem" ist inaktiviert, wenn das zu importierende Image ein angepasstes ISO-Image ist. Dieser Schritt ist nur erforderlich, wenn der Import ein VHD-Image enthält.
  {:tip}

11. Wenn das Image, das Sie importieren möchten, cloud-init-fähig ist, wählen Sie das Kontrollkästchen **Cloud-Init** aus. Weitere Informationen finden Sie unter [Bereitstellung mit einem cloud-init-fähigen Image](image_cloud-init.html).        
12. Wenn Sie Ihre eigene Betriebssystemlizenz bereitstellen, wählen Sie das Kontrollkästchen **Ihre Lizenz** aus. Weitere Informationen finden Sie unter [Red Hat Cloud Access verwenden](use-red-hat-cloud-access.html). 
13. Klicken Sie auf **Importieren**, um das Image in die Anzeige "Imagevorlagen" zu importieren. Klicken Sie auf **Abbrechen**, um den Vorgang abzubrechen.

## Nächste Schritte

Sobald der Import gestartet ist, wird nach der Imagedatei im {{site.data.keyword.objectstorageshort}}-Konto unter dem angegebenen Pfad (Konto > Cluster > Container > Imagedatei) gesucht. Die Imagedatei wird als Imagevorlage importiert, auf die dann auf der Seite "Imagevorlagen" zugegriffen werden kann. Wenn der Import abgeschlossen ist, kann das Image verwendet werden, um eine neue Einheit zu bestellen oder eine Einheit zu starten.
Ein Image kann außerdem jederzeit gelöscht werden. Die Dauer für den Imageimport variiert je nach Dateigröße, dauert aber in der Regel mehrere Minuten bis zu einer Stunde.

