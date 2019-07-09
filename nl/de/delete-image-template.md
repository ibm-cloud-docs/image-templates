---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Imagevorlage löschen
{: #deleting-an-image-template}

Standard-Imagevorlagen können nach ihrer Erstellung jederzeit wieder gelöscht werden.
{:shortdesc}

## Vorbereitungen
Navigieren Sie zuerst zum Gerätemenü und stellen Sie sicher, dass Sie über die korrekten Kontoberechtigungen verfügen, um die Tasks auszuführen.

* Navigieren Sie zum Gerätemenü Ihrer Konsole. Weitere Informationen finden Sie unter [Zu Geräten navigieren](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Stellen Sie sicher, dass Sie über alle erforderlichen Kontoberechtigungen und Gerätezugriffe verfügen. Nur der Kontoeigner oder ein Benutzer mit der klassischen Infrastrukturberechtigung **Manage Users** (Benutzer verwalten) kann die Berechtigungen anpassen.

Weitere Informationen zu Berechtigungen finden Sie unter [Klassische Infrastrukturberechtigungen](/docs/iam?topic=iam-infrapermission#infrapermission) und [Gerätezugriff verwalten](/docs/vsi?topic=virtual-servers-managing-device-access).

## Imagevorlage löschen
)
Nach dem Löschen können die Imagevorlagen nicht mehr abgerufen werden und sind aus allen Rechenzentren permanent gelöscht. Führen Sie die folgenden Schritte aus, um eine Imagevorlage zu löschen:

1. Wählen Sie im Menü **Einheiten** die Optionen **Verwalten > Images** aus.
2. Klicken Sie für die Imagevorlage, die Sie löschen möchten, auf **Aktionen**, und wählen Sie **Löschen** aus.
3. Wähle Sie im Bestätigungsfenster **Ja** aus, um das Löschen zu bestätigen. Wählen Sie **Nein**, um das Löschen abzubrechen.

Nachdem eine Imagevorlage gelöscht wurde, ist sie nicht mehr verfügbar. Alle Einheiten, die mit der Vorlage erstellt wurden, bleiben verfügbar und voll funktionsfähig. Es können jedoch keine weiteren Einheiten mit der gelöschten Vorlage erstellt werden. Imagevorlagen können nach dem Löschen nicht mehr wiederhergestellt werden.
