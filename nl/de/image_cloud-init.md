---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Bereitstellung mit einem cloud-init-fähigen Image

Wenn Sie einen virtuellen Server bestellen, verwenden viele Betriebssysteme jetzt ein cloud-init-fähiges Image, um die Bereitstellungszeit zu optimieren. Sie können auch ein angepasstes Image importieren, das Sie für den Typ "cloud-init" aktiviert haben. {:shortdesc}

Die folgenden Betriebssysteme verwenden jetzt standardmäßig ein cloud-init-fähiges Image, wenn Sie einen virtuellen Server bestellen. Sie müssen daher keine zusätzliche Software, keine Scripts nach der Bereitstellung oder keine erweiterte Überwachung auswählen.
* CentOS 7
* Debian 8
* Ubuntu 16
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

Wenn Sie einen virtuellen Server mit einem cloud-init-fähigen Betriebssystem bestellen, können Sie Benutzer- oder Metadaten mit angepassten Bereitstellungsscripts hinzufügen. Geben Sie im Bestellformular im Benutzerdaten-Feld optionale cloud-init-Benutzerdaten oder optionale Metadaten für den Server ein. 

## Ein angepasstes cloud-init-fähiges Image importieren

Wenn Sie ein angepasstes Image erstellt haben, das cloud-init-fähig ist, können Sie es im Kundenportal auf der Seite "Image importieren" als cloud-init-Image festlegen.

Führen Sie die folgenden Schritte aus, um auf die Seite "Image importieren" mit Imagevorlagen zuzugreifen und ein Image als cloud-init-fähig zu markieren: 
1. Wählen Sie im Menü **Einheiten** die Option **Verwalten** > **Images** aus.
2. Klicken Sie auf die Registerkarte **Image importieren**.
3. Geben Sie die erforderlichen Informationen zum Importieren Ihres cloud-init-fähigen Images an. Wählen Sie das Kontrollkästchen **Cloud-init** aus, das neben der Dropdown-Liste **Betriebssystem** angezeigt wird. Weitere Informationen zum Importieren von Images finden Sie unter "Images vorbereiten und importieren".

## Eine Imagevorlage als cloud-init-fähig markieren

Wenn Sie eine VHD-Imagevorlage haben, die cloud-init-fähig ist, können Sie sie auf der Detailseite der Image-Vorlage als cloud-init-fähig angeben.

Führen Sie die folgenden Schritte aus, um auf eine Imagevorlage zuzugreifen und sie als cloud-init-fähig zu markieren:
1. Wählen Sie im Menü **Einheiten** die Option **Verwalten** > **Images** aus.
2. Klicken Sie in der Liste der Vorlagen auf den Namen der Imagevorlage, die Sie aktualisieren möchten.
3. Klicken Sie auf der Seite "Details der Imagevorlage" auf das Kontrollkästchen **Aktiviert** unter der Überschrift **Cloud-Init**. Klicken Sie auf **Aktualisieren**.

## Mit Standard-Imagevorlagen arbeiten, die von einem cloud-init-bereitgestellten virtuellen Server erstellt wurden

"Cloud-init" wird in der Regel nur einmal ausgeführt. Wenn Sie jedoch einen virtuellen Server über ein cloud-init-fähiges Image bereitstellen und dann eine Standard-Imagevorlage von diesem virtuellen Server erstellen, wird die UUID aufgezeichnet. Wenn diese Standard-Imagevorlage zum Erstellen eines anderen virtuellen Servers verwendet wird, wird "cloud-init" erneut ausgeführt.

## Cloud-init-fähige Imagevorlagen erstellen

Informationen zum Konfigurieren von Images finden Sie in der [Cloud-init-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloudinit.readthedocs.io/en/latest/).

Informationen zu Datenquellen finden Sie unter [Datenquellen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html). Cloud-init-fähige {{site.data.keyword.cloud}}-Images werden für die Umgebung erstellt, indem die Datenquelle ["Config Drive ![Symbol für externen Link](../../icons/launch-glyph.svg "External link icon")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - Version 2" verwendet wird, um die Metadaten bereitzustellen.

### Linux-Voraussetzungen
* Cloud-init-Version 0.7.7 oder höher

### Windows-Voraussetzungen
* Cloudbase-init Metadata Service für die Unterstützung öffentlicher und privater Netze in der {{site.data.keyword.cloud_notm}}-Infrastruktur. Mit diesem Service wird auch das Kundenportal mit den Berechtigungsnachweisen für den virtuellen Windows-Server aktualisiert. Sie haben Zugriff auf den Service unter [https://github.com/softlayer/bluemix-cloudbase-init ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/softlayer/bluemix-cloudbase-init).
* Wenn Sie in Ihrer Umgebung ein Vyatta-Gerät verwenden, müssen Sie das Vyatta-Gerät so konfigurieren, dass API-Aufrufe an die API-Lastausgleichsfunktion zulässig sind. Weitere Informationen finden Sie unter [Brocade vRouter (Vyatta)-Handbuch zum Einrichten von VMware-Umgebungen mit Endurance oder Performance Storage ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://knowledgelayer.softlayer.com/content/brocade-vrouter-vyatta-set-guide-vmware-environments-endurance-or-performance-storage).

