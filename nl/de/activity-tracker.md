---

copyright:
  years: 2018
lastupdated: "2018-08-09"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Audits für Ereignisse bei virtuellen Servern mit Activity Tracker durchführen

Durch Verwenden von [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov) können Sie für eine virtuelle Serverinstanz (VSI) im Verlauf ihres gesamten Lebenszyklus Audits durchführen. Dazu müssen Sie über eine Instanz von Activity Tracker mit dem in 'Vereinigte Staaten (Süden)' bereitgestellten Premium-Serviceplan verfügen. Durch den Premium-Plan erhalten Sie Zugriff auf das Kibana-Dashboard, das weitere Optionen zum Filtern und Durchsuchen von Auditprotokollen umfasst. 

Die Protokolle sind für den Eigner des {{site.data.keyword.cloud}}-Kontos im Abschnitt für Kontoprotokolle sichtbar. Der Kontoeigner hat Einsicht in die folgenden Protokolle: 
* Ereignisse, die vom Eigner des Kontos ausgelöst wurden.
* Ereignisse, die von Benutzern ausgelöst wurden, die mit dem Konto verknüpft sind.

Sie können für die folgenden Ereignisse für virtuelle Serverinstanzen über Activity Tracker Audits durchführen: 
* Bereitstellen/Einrichten einer virtuellen Serverinstanz
* Inaktivieren eines Ports für eine private oder eine öffentliche Schnittstelle
* Aktivieren eines Ports für eine private oder eine öffentliche Schnittstelle
* Festlegen der Portgeschwindigkeit
* Erstellen einer Imagevorlage
* Ausschalten einer virtuellen Serverinstanz
* Einschalten einer virtuellen Serverinstanz
* Durchführen eines Warmstarts für eine virtuelle Serverinstanz
* Umbenennen einer virtuellen Serverinstanz
* Wechseln in den Rescue-Modus für eine virtuelle Serverinstanz
* Hinzufügen einer Sicherheitsgruppe zu einer öffentlichen oder privaten Schnittstelle einer virtuellen Serverinstanz
* Entfernen einer Sicherheitsgruppe aus einer öffentlichen oder privaten Schnittstelle einer virtuellen Serverinstanz
* Laden einer virtuellen Serverinstanz aus einem Image
* Booten einer virtuellen Serverinstanz aus einem Image
* Stornieren einer VSI-Einheit

Bei Ereignissen, die einer Netzschnittstelle zugeordnet sind, unterscheidet das Auditprotokoll nicht, ob das Ereignis auf einer öffentlichen oder einer privaten Schnittstelle aufgetreten ist.
{: tip}
