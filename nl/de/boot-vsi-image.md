---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}

# VSI aus einem Image starten
{: #booting-a-vsi-from-an-image}

Mit der Funktion "Aus Image starten" wird eine virtuelle Serverinstanz (VSI) gestartet, indem eine ISO-Vorlage verwendet wird, die von einem Object Storage-Konto importiert wurde.
{:shortdesc}

Das Booten eines virtuellen Servers aus einem Image stellt die Einheit auf sichere Weise online, sodass Probleme gelöst werden können. In den meisten Fällen ermöglicht die Funktion "Aus Image starten" die Fehlerbehebung in einer Umgebung, ohne dass ein erheblicher Datenverlust durch ein erneutes Laden des Betriebssystems auftreten kann. Obwohl ein signifikanter Datenverlust weniger wahrscheinlich ist als beim Neuladen eines Betriebssystems, wird empfohlen, vor dem Initiieren des Bootvorgangs ein Backup der Einheit durchzuführen.

## Vorbereitungen
Navigieren Sie zuerst zum Gerätemenü und stellen Sie sicher, dass Sie über die korrekten Kontoberechtigungen verfügen, um die Tasks auszuführen.

* Navigieren Sie zum Gerätemenü Ihrer Konsole. Weitere Informationen finden Sie unter [Zu Geräten navigieren](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Stellen Sie sicher, dass Sie über alle erforderlichen Kontoberechtigungen und Gerätezugriffe verfügen. Nur der Kontoeigner oder ein Benutzer mit der klassischen Infrastrukturberechtigung **Manage Users** (Benutzer verwalten) kann die Berechtigungen anpassen.

Weitere Informationen zu Berechtigungen finden Sie unter [Klassische Infrastrukturberechtigungen](/docs/iam?topic=iam-infrapermission#infrapermission) und [Gerätezugriff verwalten](/docs/vsi?topic=virtual-servers-managing-device-access).

## VSI aus einem Image starten

Führen Sie die folgenden Schritte durch, um einen virtuellen Server aus einem Image zu starten:

1. Sichern Sie alle in der Einheit gespeicherten Daten.
2. Wählen Sie im Menü **Einheiten** die Option **Einheitenliste** aus.
3. Klicken Sie in der Einheitenliste auf den virtuellen Server, den Sie aus einer ISO-Vorlage starten möchten.
4. Wählen Sie auf der Seite "Einheitendetails" für den virtuellen Server **Aktionen > Aus Image starten** aus.
5. Klicken Sie für das gewünschte Image auf **Aus diesem Image starten**.

  Wechseln Sie zwischen den öffentlichen und privaten Images, indem Sie die Dropdown-Funktion über der Imageliste verwenden.
  {:tip}

6. Klicken Sie auf **Aus Image starten**, um die Einheit mit dem ausgewählten Image zu booten. Klicken Sie auf **Schließen**, um die Aktion abzubrechen.

## Nächste Schritte

Nachdem Sie den Bootprozess initiiert haben, wird das Image ausgeschaltet und anschließend mit dem ausgewählten Image gestartet. Die Bootzeit variiert, da Größe und Typ jedes Images variieren. Nachdem der virtuelle Server mit dem ausgewählten Image gestartet wurde, kann er als regelmäßig gestartete Einheit aufgerufen werden, ist jedoch entsprechend dem Image konfiguriert. Starten Sie den virtuellen Server neu, um die Einheit herunterzufahren und in ihren ursprünglichen Zustand zurückzusetzen.
