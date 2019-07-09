---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}


# Imagevorlage erstellen
{: #creating-an-image-template}

Mit Imagevorlagen können Sie verschiedene Konfigurationsoptionen für {{site.data.keyword.virtualmachinesshort}} replizieren.
{:shortdesc}

Sie können zu jedem Zeitpunkt im Lebenszyklus eines virtuellen Servers eine Imagevorlage erstellen. Diese Imagevorlage können Sie anschließend verwenden, um Teile der Konfiguration in einem anderen virtuellen Server zu replizieren. Imagevorlagen können von jedem virtuellen Server erstellt werden, und zwar unabhängig von dessen Betriebssystem. Wenn Ihre Imagevorlage fertig erstellt worden ist, können Sie sie zum Erstellen eines weiteren virtuellen Servers verwenden.

## Vorbereitungen
Navigieren Sie zuerst zum Gerätemenü und stellen Sie sicher, dass Sie über die korrekten Kontoberechtigungen verfügen, um die Tasks auszuführen.

* Navigieren Sie zum Gerätemenü Ihrer Konsole. Weitere Informationen finden Sie unter [Zu Geräten navigieren](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Stellen Sie sicher, dass Sie über alle erforderlichen Kontoberechtigungen und Gerätezugriffe verfügen. Nur der Kontoeigner oder ein Benutzer mit der klassischen Infrastrukturberechtigung **Manage Users** (Benutzer verwalten) kann die Berechtigungen anpassen.

Weitere Informationen zu Berechtigungen finden Sie unter [Klassische Infrastrukturberechtigungen](/docs/iam?topic=iam-infrapermission#infrapermission) und [Gerätezugriff verwalten](/docs/vsi?topic=virtual-servers-managing-device-access).

## Imagevorlage erstellen

Führen Sie die folgenden Schritte aus, um eine Imagevorlage eines virtuellen Servers zu erstellen.

1. Wählen Sie im Menü **Einheiten** die Option **Einheitenliste** aus.
2. Klicken Sie auf den virtuellen Server, den Sie zum Erstellen einer Imagevorlage verwenden möchten.

  Prüfen Sie auf der Seite **Einheitendetails** den Inhalt der Registerkarte **Kennwörter**. Stellen Sie sicher, dass alle Kennwörter, die auf der Seite **Einheitendetails** aufgelistet sind, mit den tatsächlichen Betriebssystemkennwörtern und den Kennwörtern für sonstige Software-Add-ons übereinstimmen. Stimmen die Kennwörter nicht überein, funktionieren virtuelle Server, die mit diesem Image erstellt werden, nicht fehlerfrei.
  {:tip}

3. Wählen Sie im Menü **Aktionen** die Option **Imagevorlage erstellen** aus.
4. Geben Sie im Feld **Imagename** den neuen Namen für das Image ein.
5. Geben Sie alle notwendigen Angaben für das Image in das Feld **Anmerkung** ein.
6. Wählen Sie das Kontrollkästchen **Zustimmen** aus, wenn alle Informationen eingetragen wurden.
7. Klicken Sie zum Erstellen der Imagevorlage auf **Vorlage erstellen**.

## Nächste Schritte

Nachdem die Imagevorlage erstellt worden ist, können weitere virtuelle Server mithilfe der Vorlage erstellt werden. Die neuen virtuellen Server erhalten dabei dieselbe Konfiguration wie in der Imagevorlage enthalten ist.
