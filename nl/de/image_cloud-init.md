---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Bereitstellung mit einem cloud-init-fähigen Image
{: #provisioning-with-a-cloud-init-enabled-image}

Wenn Sie einen virtuellen Server bestellen, verwenden viele Betriebssysteme jetzt ein cloud-init-fähiges Image, um die Bereitstellungszeit zu optimieren. Sie können auch ein angepasstes Image importieren, das Sie für den Typ "cloud-init" aktiviert haben.
{:shortdesc}

Die folgenden Betriebssysteme verwenden jetzt standardmäßig ein cloud-init-fähiges Image, wenn Sie einen virtuellen Server ohne Add-ons bestellen. (Add-ons umfassten zusätzliche Software, Scripts nach der Bereitstellung und erweiterte Überwachung.)
* CentOS 7
* Debian 8, 9
* Red Hat Enterprise Linux 7.x
* Ubuntu 16.04, 18.04
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

Wenn Sie einen virtuellen Server mit einem cloud-init-fähigen Betriebssystem bestellen, können Sie Benutzer- oder Metadaten mit angepassten Bereitstellungsscripts hinzufügen. Geben Sie im Bestellformular im Benutzerdaten-Feld optionale cloud-init-Benutzerdaten oder optionale Metadaten für den Server ein.

## Vorbereitungen
Navigieren Sie zuerst zum Gerätemenü und stellen Sie sicher, dass Sie über die korrekten Kontoberechtigungen verfügen, um die Tasks auszuführen.

* Navigieren Sie zum Gerätemenü Ihrer Konsole. Weitere Informationen finden Sie unter [Zu Geräten navigieren](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Stellen Sie sicher, dass Sie über alle erforderlichen Kontoberechtigungen und Gerätezugriffe verfügen. Nur der Kontoeigner oder ein Benutzer mit der klassischen Infrastrukturberechtigung **Manage Users** (Benutzer verwalten) kann die Berechtigungen anpassen.

Weitere Informationen zu Berechtigungen finden Sie unter [Klassische Infrastrukturberechtigungen](/docs/iam?topic=iam-infrapermission#infrapermission) und [Gerätezugriff verwalten](/docs/vsi?topic=virtual-servers-managing-device-access).

## Ein angepasstes cloud-init-fähiges Image importieren

Wenn Sie ein angepasstes Image erstellt haben, das cloud-init-fähig ist, können Sie dieses Image im {{site.data.keyword.slportal_full}} auf der Seite "Image importieren" als cloud-init-fähiges Image angeben.

Führen Sie die folgenden Schritte aus, um auf die Seite "Image importieren" mit Imagevorlagen zuzugreifen und ein Image als cloud-init-fähig zu markieren:
1. Wählen Sie im Menü **Einheiten** die Optionen **Verwalten** > **Images** aus.
2. Klicken Sie auf die Registerkarte **Image importieren**.
3. Geben Sie die erforderlichen Informationen zum Importieren Ihres cloud-init-fähigen Image an. Wählen Sie das Kontrollkästchen **Cloud-init** aus, das neben der Dropdown-Liste **Betriebssystem** angezeigt wird. Weitere Informationen zum Importieren von Images finden Sie unter "Images vorbereiten und importieren".

## Eine Imagevorlage als cloud-init-fähig markieren

Wenn Sie eine VHD-Imagevorlage haben, die cloud-init-fähig ist, können Sie sie auf der Detailseite der Image-Vorlage als cloud-init-fähig angeben.

Führen Sie die folgenden Schritte aus, um auf eine Imagevorlage zuzugreifen und sie als cloud-init-fähig zu markieren:
1. Wählen Sie im Menü **Einheiten** die Optionen **Verwalten** > **Images** aus.
2. Klicken Sie in der Liste der Vorlagen auf den Namen der Imagevorlage, die Sie aktualisieren möchten.
3. Klicken Sie auf der Seite "Details der Imagevorlage" auf das Kontrollkästchen **Aktiviert** unter der Überschrift **Cloud-Init**. Klicken Sie auf **Aktualisieren**.

## Mit einer Imagevorlage arbeiten, die aus einem mit "cloud-init" eingerichteten virtuellen Server erstellt wurde

"Cloud-init" wird in der Regel nur einmal ausgeführt. Wenn Sie jedoch einen virtuellen Server über ein cloud-init-fähiges Image bereitstellen und anschließend eine Imagevorlage aus diesem virtuellen Server erstellen, wird die UUID aufgezeichnet. Wenn diese Imagevorlage dann zum Erstellen eines anderen virtuellen Servers verwendet wird, wird "cloud-init" erneut ausgeführt.

## Cloud-init-fähige Imagevorlagen erstellen

Informationen zum Konfigurieren von Images finden Sie in der [Cloud-init-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloudinit.readthedocs.io/en/latest/).

Informationen zu Datenquellen finden Sie unter [Datenquellen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html). Cloud-init-fähige {{site.data.keyword.cloud_notm}}-Images werden für die Umgebung erstellt,
indem die Datenquelle [Config Drive ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - Version 2 verwendet wird, um die Metadaten bereitzustellen.

### Linux-Voraussetzungen
* Cloud-init-Version 0.7.7 oder höher

### Windows-Voraussetzungen
* Cloudbase-init Metadata Service für die Unterstützung öffentlicher und privater Netze in der {{site.data.keyword.cloud_notm}}-Infrastruktur. Mit diesem Service wird auch das Kundenportal mit den Berechtigungsnachweisen für den virtuellen Windows-Server aktualisiert. Sie haben Zugriff auf den Service unter [https://github.com/softlayer/bluemix-cloudbase-init ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/softlayer/bluemix-cloudbase-init).
* Wenn Sie in Ihrer Umgebung ein Vyatta-Gerät verwenden, müssen Sie das Vyatta-Gerät so konfigurieren, dass API-Aufrufe an die API-Lastausgleichsfunktion zulässig sind. Weitere Informationen finden Sie unter [Brocade vRouter (Vyatta) für VMware-Umgebungen mit File Storage einrichten](/docs/infrastructure/virtual-router-appliance?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#load-balancer-ips).
