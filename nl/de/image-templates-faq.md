---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-30"

subcollection: image-templates

---


{:new_window: target="_blank"}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}

# Häufig gestellte Fragen (FAQs): Imagevorlagen

## Was ist eine Standard-Imagevorlage?
{: faq}

Eine Standard-Imagevorlage ist die {{site.data.keyword.virtualmachineslong}}-Imagingoption für {{site.data.keyword.BluSoftlayer_notm}}.
Standard-Imagevorlagen ermöglichen unabhängig vom Betriebssystem die Imageerfassung für einen vorhandenen virtuellen Server und das Erstellen eines neuen virtuellen Servers basierend auf dem erfassten Image.

## Was ist eine ISO-Imagevorlage?
{: faq}

Die ISO-Imagevorlage ist eine Vorlage speziell für ISO-Images, die zum Booten einer VSI verwendet werden können. ISO-Vorlagen sind in zwei Versionen verfügbar: öffentlich und privat. Öffentliche ISO-Vorlagen sind vorkonfigurierte Vorlagen, die von {{site.data.keyword.BluSoftlayer_notm}} bereitgestellt werden und von jedem Kunden genutzt werden können. Private ISO-Vorlagen werden erstellt, indem ein Image von einem ISO-Image importiert wird, das für ein {{site.data.keyword.objectstorageshort}}-Konto gespeichert ist. Damit eine ISO-Vorlage in die Anzeige "Imagevorlagen" importiert werden kann, muss die ISO-Datei bootfähig sein.

Nur unterstützte {{site.data.keyword.BluSoftlayer_notm}}-Betriebssysteme können zum Laden einer ISO-Vorlage in eine VSI verwendet werden. Weitere Informationen finden Sie unter [Unterstützte Betriebssysteme ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.softlayer.com/services/software/).
{:tip}

## Worin besteht der Unterschied zwischen einem öffentlichen und einem privaten Image?
{: faq}

Ein öffentliches Image ist ein Image, das von jedem {{site.data.keyword.BluSoftlayer_notm}}-Benutzer angezeigt und auf einen neuen virtuellen Server angewendet werden kann. {{site.data.keyword.BluSoftlayer_notm}} erstellt im Moment öffentliche Images als Lösung für Konfigurationsoptionen auf verschiedenen Einheiten. Kunden können Images auch öffentlich zugänglich und für jeden Benutzer verfügbar machen. Ein privates Image ist ein Image, das nur von berechtigten Benutzern angezeigt werden kann. Berechtigte Benutzer sind standardmäßig alle Benutzer in Ihrem Konto. Images können aber auch zwischen mehreren Konten gemeinsam genutzt werden, wenn die Freigabeoptionen im {{site.data.keyword.slportal}} aktualisiert werden..

## Können virtuelle Server mit einem selbst verwalteten Hypervisor erfasst und bereitstellt werden?
{: faq}

Nur Server, deren Bereitstellung durch {{site.data.keyword.BluSoftlayer_notm}} erfolgt, können erfasst und implementiert werden. Einzelne virtuelle Server, die Sie auf persönlichen Einheiten manuell erstellt haben, können nicht erfasst, bereitgestellt oder implementiert werden.

## Können private ISO-Vorlagen mit anderen Kunden gemeinsam genutzt werden?
{: faq}

Die einzigen ISO-Vorlagen, die für alle Kunden öffentlich zugänglich werden, sind von {{site.data.keyword.BluSoftlayer_notm}} generierte Vorlagen. Private ISO-Vorlagen sind für das jeweilige Konto spezifisch und können nicht über {{site.data.keyword.slportal}} mit anderen Kunden gemeinsam genutzt werden.

## Muss sich die ISO-Vorlage im selben Rechenzentrum befinden wie der virtuelle Server, um vom Image zu booten?
{: faq}

Ja, die ISO-Vorlage muss dich im selben Rechenzentrum befinden wie der virtuelle Server, um vom Image zu booten. Wenn es sich nicht im selben Rechenzentrum befindet, muss die ISO-Vorlage in das entsprechende Rechenzentrum kopiert werden, damit die Aktion abgeschlossen wird. Wenn diese Situation eintritt, wird für diese Transaktion eine Warnung angezeigt. Wenn eine ISO-Vorlage in ein anderes Rechenzentrum kopiert wird, wird dem Konto eine geringe Gebühr in Rechnung gestellt, die der Gebühr für das Kopieren anderer Typen von Imagevorlagen entspricht.

## Worin besteht der Unterschied zwischen physischen Daten und Datenträgergröße?
{: faq}

Der Datenträger ist der Festplattenspeicher, der zum Speichern von Dateien zur Verfügung steht. Physische Daten hingegen sind die Dateien, die auf dem Datenträger gespeichert sind.

## Was ist die Funktion zum Importieren/Exportieren von Images?
{: faq}

Die Funktion zum Importieren/Exportieren von Images befindet sich auf der Seite "Imagevorlagen" im {{site.data.keyword.slportal}}. Mit ihr können VHD- und ISO-Images, die in einem {{site.data.keyword.objectstorageshort}}-Konto gespeichert sind, in Imagevorlagen und umgekehrt konvertiert werden. Wenn Sie ein Image importieren, wird eine bestimmte Datei (VHD oder ISO) aus dem Container eines angegebenen Kontos [{{site.data.keyword.objectstorageshort}}] abgerufen und in eine Imagevorlage konvertiert. Die Imagevorlage kann dann verwendet werden, um eine Einheit zu booten oder zu laden. Wenn Sie ein Image exportieren, wird die Imagevorlage in eine Datei (oder in mehrere Dateien, wenn die Vorlage mehrere Platten enthält) konvertiert. Diese Datei befindet sich an einer bestimmten Position auf dem {{site.data.keyword.objectstorageshort}}-Konto-Container.

## Wie erstelle ich eine Imagevorlage für meinen gesamten Server und nicht nur für mein primäres Laufwerk?
{: faq}

Wenn Sie eine Imagevorlage für Ihren gesamten Server erstellen wollen, lesen Sie die Anweisungen in [Imagevorlage erstellen](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template). 

Wenn Sie eine Imagevorlage in IBM Cloud Object Storage exportieren, so ist jeder Blockeinheit (oder Platte) eine eigene Datei zugeordnet. Wenn Ihre Imagedatei beispielsweise den Namen "image.vhd" hat, so wird die erste Blockeinheit als "image-0.vhd" exportiert. Die zweite Blockeinheit wird dann mit dem Namen "image-1.vhd" exportiert usw.
{: tip}
