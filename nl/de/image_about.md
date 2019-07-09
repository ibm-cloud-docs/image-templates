---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Informationen zu Imagevorlagen
{: #about-image-templates}

Standard-Imagevorlagen stellen eine Option für die Imageerstellung für alle {{site.data.keyword.virtualmachinesshort}} unabhängig vom Betriebssystem bereit.
{:shortdesc}

Sie können mit Standard-Imagevorlagen ein Image eines virtuellen Servers erfassen und auf der Grundlage dieses erfassten Image einen neuen virtuellen Server erstellen. Standard-Imagevorlagen sind nicht kompatibel mit Bare-Metal-Servern.

## Funktionsweise von Imagevorlagen
Die beiden Hauptaktionen für jede Imagevorlage sind das Erstellen und das Bereitstellen. Wenn Sie das Erstellen eines Image anfordern, schaltet das automatisierte System der {{site.data.keyword.cloud}} Ihren Server offline. Während der Server offline ist, wird eine komprimierte Sicherung Ihrer Daten erstellt, die Konfigurationsdaten werden aufgezeichnet und die Imagevorlage wird im SAN der {{site.data.keyword.cloud_notm}} gespeichert. In der Bereitstellungsphase der Imagevorlage erstellt das automatisierte System einen neuen Server mithilfe der aus dem ausgewählten Image gesammelten Daten. Im Bereitstellungsprozess wird das Volumen angepasst, die kopierten Daten werden wiederhergestellt und schließlich werden Konfigurationsänderungen (z. B. Netzkonfigurationen) für den neuen Host vorgenommen.

## Private Images

Private Images sind Images, die von einem Benutzer auf dem Konto erstellt werden oder Images, die auf einem anderen Konto erstellt und gemeinsam genutzt werden. Standardmäßig sind alle Images, die erstellt werden, private Images.

## Öffentliche Images

Öffentliche Images sind vorkonfigurierte virtuelle Server, die von {{site.data.keyword.cloud_notm}} gepostet oder von Kunden öffentlich gemacht werden und für alle {{site.data.keyword.cloud_notm}}-Kunden zur Verfügung stehen. Virtuelle Server, die über öffentliche Imagevorlagen bereitgestellt werden, sind im Allgemeinen mit bestimmten Kombinationen einer Vielfalt von Konfigurationsspezifikationen für optimale Leistung konfiguriert.
