---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}

# VSI aus einem Image starten

Mit der Funktion "Aus Image starten" wird eine virtuelle Serverinstanz (VSI) gestartet, indem eine ISO-Vorlage verwendet wird, die von einem Object Storage-Konto importiert wurde.
{:shortdesc}

Das Booten eines virtuellen Servers aus einem Image stellt die Einheit auf sichere Weise online, sodass Probleme gelöst werden können. In den meisten Fällen ermöglicht die Funktion "Aus Image starten" die Fehlerbehebung in einer Umgebung, ohne dass ein erheblicher Datenverlust durch ein erneutes Laden des Betriebssystems auftreten kann. Obwohl ein signifikanter Datenverlust weniger wahrscheinlich ist als beim Neuladen eines Betriebssystems, wird empfohlen, vor dem Initiieren des Bootvorgangs ein Backup der Einheit durchzuführen.

Führen Sie die folgenden Schritte durch, um einen virtuellen Server aus einem Image zu starten:

1. Sichern Sie alle in der Einheit gespeicherten Daten.
2. Wählen Sie im [{{site.data.keyword.slportal_full}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) im Menü **Einheiten** die Option **Einheitenliste** aus.
3. Klicken Sie in der Einheitenliste auf den virtuellen Server, den Sie aus einer ISO-Vorlage starten möchten.
4. Wählen Sie auf der Seite "Einheitendetails" für den virtuellen Server **Aktionen > Aus Image starten** aus.
5. Klicken Sie für das gewünschte Image auf **Aus diesem Image starten**.

  Wechseln Sie zwischen den öffentlichen und privaten Images, indem Sie die Dropdown-Funktion über der Imageliste verwenden.
  {:tip}

6. Klicken Sie auf **Aus Image starten**, um die Einheit mit dem ausgewählten Image zu booten. Klicken Sie auf **Schließen**, um die Aktion abzubrechen.

## Nächste Schritte

Nachdem Sie den Bootprozess initiiert haben, wird das Image ausgeschaltet und anschließend mit dem ausgewählten Image gestartet. Die Bootzeit variiert, da Größe und Typ jedes Images variieren. Nachdem der virtuelle Server mit dem ausgewählten Image gestartet wurde, kann er als regelmäßig gestartete Einheit aufgerufen werden, ist jedoch entsprechend dem Image konfiguriert. Starten Sie den virtuellen Server neu, um die Einheit herunterzufahren und in ihren ursprünglichen Zustand zurückzusetzen.
