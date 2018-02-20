---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-27"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Ein Image exportieren

Sie können über die Seite "Imagevorlagen" eine Imagevorlage zu einem Object Storage-Konto exportieren.
{:shortdesc}

Der Image-Exportprozess verwendet eine vorhandene, private Standardimagevorlage und konvertiert das Image in eine Imagedatei, die am angegebenen Speicherort im Object Storage-Konto gespeichert wird. Führen Sie die folgenden Schritte aus, um eine Imagevorlage zu exportieren:

1. Wählen Sie im Menü **Einheiten** im [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) die Option **Verwalten > Images** aus.
2. Klicken Sie für die Imagevorlage, die Sie exportieren möchten, auf **Aktionen** und wählen Sie **Image exportieren** aus. Wenn eine Imagevorlage mit der gewünschten Konfiguration noch nicht verfügbar ist, siehe [Ein Standardimage erstellen](create-standard-image.html).
3. Geben Sie auf der Seite "Image exportieren" in das Feld **Dateiname** den Dateiennamen des Images ein.
5. Wählen Sie aus der Dropdown-Liste **Konto** ein **Object Storage-Konto** aus.
6. Wählen Sie aus der Dropdown-Liste **Cluster** ein **Object Storage-Cluster** aus.
7. Wählen Sie aus der Dropdown-Liste **Container** einen **Object Storage-Container** aus.
8. Klicken Sie auf **Image exportieren**, um das Image an die angegebene Position im Object Storage-Konto zu exportieren. Klicken Sie auf **Schließen**, um die Aktion abzubrechen. 

## Nächste Schritte

Nachdem das Image exportiert wurde, wird das Image in der Liste der Imagevorlagen weiterhin angezeigt. Es ist jedoch auch als Datei in der Object Storage-Position verfügbar, die beim Exportieren angegeben wurde. Weitere Informationen zum Anzeigen einer Datei, die zu einem Object Storage-Konto exportiert wurde, finden Sie in [Object Storage-Dateiinformationen anzeigen und bearbeiten](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html). Da die Größe und die Eigenschaften für jedes Image variieren, kann der Exportprozess einige Minuten dauern, bevor er abgeschlossen wird. Die durchschnittliche Exportgeschwindigkeit beträgt 2 GB/Minute. Wenn mehrere Minuten verstrichen sind und das Image immer noch nicht im Object Storage-Konto verfügbar ist, [fordern Sie Unterstützung an](/docs/get-support/howtogetsupport.html).

