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


# Eigene Betriebssystemlizenz oder eigenes Abonnement verwenden
{: #using-your-own-os-license-or-subscription}

Wenn Sie eine Imagevorlage mit einem VHD-Image erstellen, können Sie Ihre eigene RHEL-Betriebssystemlizenz über das [Red Hat Cloud
Access ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) Abonnement oder eine Windows-Lizenz über die Microsoft Enterprise-Vereinbarung bereitstellen.
{:shortdesc}

Wenn Sie in der {{site.data.keyword.cloud}} ein Image bereitstellen, die angibt, dass Sie Ihre eigene Lizenz verwenden, gelten die folgenden Support-Bedingungen:
* {{site.data.keyword.IBM_notm}} bietet Unterstützung für Hypervisoren, das Bereitstellen von Instanzen, das Importieren von Images, das Neustarten von Images, das erneute Laden des Betriebssystems und das Erfassen von Images.
* Das Unternehmen, von dem Sie die Lizenz des Betriebssystems erwerben, unterstützt das Image selbst. {{site.data.keyword.IBM_notm}} bietet keine Unterstützung für das Image an.

Wenn Sie eine eigene Lizenz für ein Image bereitstellen, gelten für das Image die folgenden Einschränkungen:
* Das Image ist kein privates Image. Es kann nicht öffentlich zugänglich gemacht werden.
* Das Image kann keine Software-Add-ons enthalten. Weitere Software kann nur hinzugefügt werden, nachdem das Image bereitgestellt wurde.

## Red Hat Cloud Access verwenden
Weitere Informationen zu unserer Zertifizierung als Red Hat Enterprise Linux-Cloud-Provider finden Sie unter [Infrastructure as a Service (Iaas) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://access.redhat.com/ecosystem/cloud-provider/2262101).

## Ihre eigene Windows-Lizenz verwenden
Die folgenden Betriebssysteme werden unterstützt:
* Windows Server 2016
* Windows Server 2012
* Windows Server 2012 R2

Wenden Sie sich an Ihren Microsoft-Ansprechpartner, wenn Sie Fragen zu Ihrer Windows-Lizenzberechtigung oder zu den Berichtsanforderungen haben. Wenn Sie eine Imagevorlage erstellen, die angibt, dass Sie Ihre eigene Windows-Lizenz verwenden, müssen Sie dieses Image auf einem dedizierten Host bereitstellen. Sie können keine öffentliche und keine dedizierte Instanz bereitstellen, die einem Host automatisch zugewiesen wird, wenn Sie ein Image verwenden, das angibt, dass Sie Ihre eigene Windows-Lizenz verwenden. Das Windows-Image kann außerdem kein Image des Typs "cloud-init" sein, wenn Sie eine Windows-Imagevorlage erstellen oder aktualisieren, das angibt, dass Sie Ihre eigene Lizenz verwenden.

## Vorbereitungen
Navigieren Sie zuerst zum Gerätemenü und stellen Sie sicher, dass Sie über die korrekten Kontoberechtigungen verfügen, um die Tasks auszuführen.

* Navigieren Sie zum Gerätemenü Ihrer Konsole. Weitere Informationen finden Sie unter [Zu Geräten navigieren](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Stellen Sie sicher, dass Sie über alle erforderlichen Kontoberechtigungen und Gerätezugriffe verfügen. Nur der Kontoeigner oder ein Benutzer mit der klassischen Infrastrukturberechtigung **Manage Users** (Benutzer verwalten) kann die Berechtigungen anpassen.

Weitere Informationen zu Berechtigungen finden Sie unter [Klassische Infrastrukturberechtigungen](/docs/iam?topic=iam-infrapermission#infrapermission) und [Gerätezugriff verwalten](/docs/vsi?topic=virtual-servers-managing-device-access).

## Ein Image importieren, das Ihre eigene Lizenz angibt

Sie können ein VHD-Image importieren und angeben, dass Sie für das Betriebssystem eine eigene Lizenz oder ein Abonnement bereitstellen.

Führen Sie die folgenden Schritte aus, um auf die Seite "Image importieren" mit den Imagevorlagen zuzugreifen und ein VHD-Image zu markieren, das Sie für Ihre eigene Lizenz oder Ihr eigenes Abonnement verwenden möchten:
1. Wählen Sie im Menü **Einheiten** die Optionen **Verwalten > Images** aus.
2. Klicken Sie auf die Registerkarte **Image importieren**.
3. Geben Sie die erforderlichen Informationen zum Importieren Ihres VHD-Image an. Wählen Sie das Kontrollkästchen **Ihre Lizenz** aus, das neben der Dropdown-Liste **Betriebssystem** angezeigt wird. Weitere Informationen zum Importieren von Images finden Sie unter [Images vorbereiten und importieren](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images).

## Eine Imagevorlage aktualisieren, um eine vom Benutzer bereitgestellte Betriebssystemlizenz anzugeben

Wenn Sie eine vorhandene VHD-Imagevorlage haben, können Sie angeben, dass Sie eine eigene Lizenz oder ein Abonnement für das Betriebssystem bereitstellen möchten.

Führen Sie die folgenden Schritte aus, um auf eine Imagevorlage zuzugreifen und anzugeben, dass sie Ihre eigene Lizenz oder Ihr Abonnement verwendet:
1. Wählen Sie im Menü **Einheiten** die Optionen **Verwalten > Images** aus.
2. Klicken Sie in der Liste der Vorlagen auf den Namen der Imagevorlage, die Sie aktualisieren möchten.
3. Klicken Sie auf der Seite "Details der Imagevorlage" auf das Kontrollkästchen **Vom Benutzer zur Verfügung gestellt** unter der Überschrift **Betriebssystemlizenz**. Klicken Sie auf **Aktualisieren**.
