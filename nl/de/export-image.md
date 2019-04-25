---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:deprecated: .deprecated}
{:new_window: target="_blank"}

# Image nach OpenStack Swift exportieren
{: #exporting-an-image-to-openstack-swift}

Über die Seite "Imagevorlagen" können Sie eine Imagevorlage in ein Konto von [Object Storage OpenStack Swift](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-GettingStarted#getting-started-with-object-storage-openstack-swift) exportieren.
{:shortdesc}

Alle Instanzen dieses Service werden nicht mehr weiterentwickelt. Bereits vorhandene Konten können zwar verwendet werden, es können jedoch seit dem 10. Dezember 2018 keine neuen {{site.data.keyword.objectstorageshort}} OpenStack Swift-Konten mehr bereitgestellt werden. Seit 31. März 2019 unterstützt IBM Cloud das Import-/Exportfeature für Imagevorlagen in Verbindung mit Cloud Object Storage OpenStack Swift nicht mehr.
{: deprecated}

Beim Exportvorgang für ein Image wird eine bereits vorhandene private Standard-Imagevorlage in eine Imagedatei konvertiert, die dann an einer angegebenen Position in einem Konto von Object Storage OpenStack Swift gespeichert wird. Führen Sie die folgenden Schritte aus, um eine Imagevorlage zu exportieren:

1. Wählen Sie im [{{site.data.keyword.slportal_full}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) im Menü **Einheiten** die Optionen **Verwalten > Images** aus.
2. Klicken Sie für die Imagevorlage, die Sie exportieren möchten, auf **Aktionen** und wählen Sie **Image exportieren** aus. Falls noch keine Imagevorlage mit der gewünschten Konfiguration verfügbar ist, informieren Sie sich unter [Imagevorlage erstellen](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).
3. Geben Sie auf der Seite "Image exportieren" in das Feld **Dateiname** den Dateinamen des Image ein.
5. Wählen Sie aus der Dropdown-Liste **Konto** ein **Object Storage-Konto** aus.
6. Wählen Sie aus der Dropdown-Liste **Cluster** ein **Object Storage-Cluster** aus.
7. Wählen Sie aus der Dropdown-Liste **Container** einen **Object Storage-Container** aus.
8. Klicken Sie auf **Image exportieren**, um das Image an die angegebene Position im Object Storage-Konto zu exportieren. Klicken Sie auf **Schließen**, um die Aktion abzubrechen.

## Nächste Schritte

Nachdem Sie ein Image exportiert haben, wird dieses Image weiterhin in der Liste der Imagevorlagen aufgeführt, ist jetzt aber auch als Datei an der Position in Object Storage OpenStack Swift verfügbar, die beim Exportvorgang angegeben wurde. Weitere Informationen zum Anzeigen einer Datei, die in ein Object Storage-Konto exportiert wurde, finden Sie unter [Dateidetails anzeigen und bearbeiten](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-OSSSLPortal#viewing-and-editing-file-details). Da die Größe und die Eigenschaften für jedes Image variieren, kann der Exportprozess einige Minuten dauern, bevor er abgeschlossen wird. Die durchschnittliche Exportgeschwindigkeit beträgt 2 GB/Minute. Wenn das Image nach mehreren Minuten immer noch nicht im Konto von Object Storage OpenStack Swift verfügbar ist, sollten Sie [Unterstützung anfordern](/docs/get-support?topic=get-support-getting-customer-support).
