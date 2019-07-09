---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

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

# Image nach IBM Cloud Object Storage exportieren
{: #exporting-an-image-to-ibm-cloud-object-storage}

Über die Seite 'Imagevorlagen' können Sie eine Imagevorlage in ein Konto von [{{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about) exportieren.
{:shortdesc}

Beim Exportvorgang für ein Image wird eine bereits vorhandene Vorlage für private Standardimages oder eine Vorlage für verschlüsselte Images in eine Imagedatei konvertiert, die dann an einer angegebenen Position in einem Konto von {{site.data.keyword.cos_full_notm}} gespeichert wird.

*Hinweis:* Wenn Sie ein VMDK-Image importiert haben, können Sie dieses Image im VHD- oder VMDK-Format exportieren. Aufgrund der Unterschiede zwischen den Imageformaten kann es zu Datenverlust kommen. Zum Schutz der Daten im Fall eines Datenverlusts wird die ursprüngliche VHD-Datei aufbewahrt.

## Vorbereitungen
Navigieren Sie zuerst zum Gerätemenü und stellen Sie sicher, dass Sie über die korrekten Kontoberechtigungen verfügen, um die Tasks auszuführen.

* Navigieren Sie zum Gerätemenü Ihrer Konsole. Weitere Informationen finden Sie unter [Zu Geräten navigieren](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Stellen Sie sicher, dass Sie über Schreibzugriff für {{site.data.keyword.cos_full_notm}} verfügen. Nur der Kontoeigner oder ein Benutzer mit der klassischen Infrastrukturberechtigung **Manage Users** (Benutzer verwalten) kann die Berechtigungen anpassen.

Weitere Informationen zu Berechtigungen finden Sie unter [Klassische Infrastrukturberechtigungen](/docs/iam?topic=iam-infrapermission#infrapermission) und [Gerätezugriff verwalten](/docs/vsi?topic=virtual-servers-managing-device-access).

Wenn Sie vorhaben, diese Imagevorlage nach {{site.data.keyword.cos_full_notm}} zu exportieren, müssen Sie sicherstellen, dass der Name keine Zeichen enthält, die in einer Webadresse zu Problemen führen können. So können z. B. ?, =, < und andere Sonderzeichen können zu unerwünschtem Verhalten führen, wenn sie nicht URL-codiert sind.
{: tip}

## Image nach IBM Cloud Object Storage exportieren

Führen Sie zum Exportieren einer Imagevorlage zu IBM Cloud Object Storage die folgenden Schritte aus.

1. Wählen Sie im Menü **Einheiten** die Optionen **Verwalten > Images** aus.
2. Klicken Sie für die Imagevorlage, die Sie exportieren wollen, auf **Aktionen**, und wählen Sie **Image zu IBM COS exportieren** aus. Falls noch keine Imagevorlage mit der gewünschten Konfiguration verfügbar ist, informieren Sie sich unter [Imagevorlage erstellen](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template).
3. Füllen Sie die Pflichtangabefelder aus (siehe Tabelle 1).
4. Klicken Sie auf **OK**, um das Image an die angegebene Position im {{site.data.keyword.cos_full_notm}}-Konto zu exportieren.

| Feld | Wert |
| ----- | ----- |
| Dateiname | Geben Sie den Dateinamen für das Image ein. |
| {{site.data.keyword.cos_full_notm}} | Wählen Sie ein {{site.data.keyword.cos_full_notm}}-Konto aus. |
| Standort | Wählen Sie die spezifische geografische Region aus, an der Ihr Image gespeichert werden soll. |
| Bucket | Wählen Sie das {{site.data.keyword.cos_full_notm}}-Bucket aus, in dem Ihr Image gespeichert werden soll. Es sind nur Buckets gültig, die innerhalb des von Ihnen ausgewählten Standorts vorhanden sind. Wenn Sie ein Bucket angeben, das nicht am ausgewählten Standort vorhanden ist, erhalten Sie eine Fehlernachricht. |
| API-Schlüssel | Geben Sie API-Schlüssel für die Service-ID an, die für {{site.data.keyword.cos_full_notm}} über Schreibzugriff verfügt. Weitere Informationen finden Sie unter [API-Schlüssel für Service-IDs verwalten](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys). |
{: caption="Tabelle 1. Werte für den Export eines Image nach {{site.data.keyword.cos_full_notm}}" caption-side="top"}
| Zieldateityp | Wählen Sie den Dateityp aus, den Sie exportieren möchten. Wenn Sie ein VMDK-Image importiert haben, können Sie dieses Image im VHD- oder VMDK-Format exportieren. Wenn die Datei ursprünglich im VHD-Format vorlag, ist der Export nur im VHD-Format möglich. |

## Nächste Schritte
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
Da die Größe und die Eigenschaften für jedes Image variieren, kann der Exportprozess einige Minuten dauern, bevor er abgeschlossen wird.

Nachdem Sie ein Image exportiert haben, wird dieses Image weiterhin in der Liste der Imagevorlagen aufgeführt, ist jetzt aber auch als Datei an der Position in {{site.data.keyword.cos_full_notm}} verfügbar, die beim Exportvorgang angegeben wurde. Sie können die Imagedatei von {{site.data.keyword.cos_full_notm}} herunterladen. Greifen Sie über die Ressourcenliste in der {{site.data.keyword.cloud_notm}}-Konsole auf Ihre Cloud Object Storage-Serviceinstanz zu. Wählen Sie in dem Bucket, in dem Ihr Image gespeichert ist, die Imagedateien aus, die Sie herunterladen möchten, und wählen Sie **Objekte herunterladen** aus.

Wenn Sie eine Imagevorlage zu IBM Cloud Object Storage exportieren, so ist jeder Blockeinheit (oder Platte) eine eigene Datei zugeordnet. Wenn Ihre Imagedatei beispielsweise den Namen "image.vhd" hat, so wird die erste Blockeinheit als "image-0.vhd" exportiert. Die zweite Blockeinheit wird dann mit dem Namen "image-1.vhd" exportiert usw.
{: tip}
