---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

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

Führen Sie die folgenden Schritte aus, um eine Imagevorlage eines virtuellen Servers zu erstellen.

1. Öffnen Sie das [{{site.data.keyword.slportal_full}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){: new_window}, indem Sie Ihre eindeutigen Berechtigungsnachweise eingeben.
2. Wählen Sie im Menü **Einheiten** die Option **Einheitenliste** aus.
3. Klicken Sie auf den virtuellen Server, den Sie zum Erstellen einer Imagevorlage verwenden möchten.

  Prüfen Sie auf der Seite **Einheitendetails** den Inhalt der Registerkarte **Kennwörter**. Stellen Sie sicher, dass alle Kennwörter, die auf der Seite **Einheitendetails** aufgelistet sind, mit den tatsächlichen Betriebssystemkennwörtern und den Kennwörtern für sonstige Software-Add-ons übereinstimmen. Stimmen die Kennwörter nicht überein, funktionieren virtuelle Server, die mit diesem Image erstellt werden, nicht fehlerfrei.
  {:tip}

4. Wählen Sie im Menü **Aktionen** die Option **Imagevorlage erstellen** aus.
5. Geben Sie im Feld **Imagename** den neuen Namen für das Image ein.
6. Geben Sie alle notwendigen Angaben für das Image in das Feld **Anmerkung** ein.
7. Wählen Sie das Kontrollkästchen **Zustimmen** aus, wenn alle Informationen eingetragen wurden.
8. Klicken Sie zum Erstellen der Imagevorlage auf **Vorlage erstellen**.

## Nächste Schritte

Nachdem die Imagevorlage erstellt worden ist, können weitere virtuelle Server mithilfe der Vorlage erstellt werden. Die neuen virtuellen Server erhalten dabei dieselbe Konfiguration wie in der Imagevorlage enthalten ist.
