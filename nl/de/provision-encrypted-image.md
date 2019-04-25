---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-27"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# End-to-End-Verschlüsselung (E2E) für die Bereitstellung einer verschlüsselten Instanz verwenden
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

Das Feature für End-to-End-Verschlüsselung (E2E) ermöglicht Ihnen die Verwendung Ihres eigenen cloud-init-fähigen Betriebssystemimage, das Sie unter Verwendung eines Datenverschlüsselungsschlüssels, dessen Eigner Sie sind und den Sie verwalten, verschlüsselt haben. Nachdem Sie einige Schritte zur Konfiguration der Umgebung durchgeführt haben, können Sie Ihr verschlüsseltes Image in das Repository für Imagevorlagen importieren und für die Bereitstellung verschlüsselter virtueller Serverinstanzen verwenden. Durch die E2E-Verschlüsselung wird die Verschlüsselung ruhender Daten für den Speicher bereitgestellt, der bereitgestellten virtuellen Serverinstanzen zugeordnet ist.
{:shortdesc}

Bei der E2E-Verschlüsselung werden mehrere {{site.data.keyword.cloud}}-Komponenten zusammengebracht, um eine sichere Lösung für Ihre kritischen Informationen zu bieten.

* {{site.data.keyword.keymanagementservicefull_notm}} schützt Ihre Schlüssel mit nach FIPS 140, Sicherheitsstufe 2, zertifizierten cloudbasierten Hardwaresicherheitsmodulen (HSM), die vor dem Diebstahl von Informationen schützen.
* IBM {{site.data.keyword.iamshort}} (IAM) ermöglicht dem Cloud Block Storage-Service den Zugriff auf {{site.data.keyword.keymanagementserviceshort}} und Ihren Rootschlüssel, der für das Wrapping Ihres Datenverschlüsselungsschlüssels verwendet wird.
* {{site.data.keyword.cos_full_notm}} speichert Ihr verschlüsseltes Image sicher, wenn Sie es hochladen.
* In der {{site.data.keyword.cloud_notm}}-Konsole können Sie Ihr verschlüsseltes Image importieren und eine Imagevorlage erstellen.
* Wenn eine Vorlage für ein verschlüsseltes Image in der Infrastrukturumgebung der {{site.data.keyword.cloud_notm}}-Konsole verfügbar ist, können Sie verschlüsselte virtuelle Serverinstanzen bereitstellen.
* Zum Schluss haben Sie die Möglichkeit, über [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov) Audits für Ereignisse durchzuführen, die Ihren verschlüsselten virtuellen Servern zugeordnet sind.

## Umgebung vorbereiten

1. Um die E2E-Verschlüsselung nutzen zu können, müssen Sie über ein Konto verfügen, für das ein Upgrade durchgeführt wurde. Weitere Informationen finden Sie unter [Zur IBMid wechseln und Konten verknüpfen](/docs/account/softlayerlink.html).
2. Verwenden Sie {{site.data.keyword.keymanagementservicefull_notm}} zum Erstellen und Verwalten von Schlüsseln.
      1. Richten Sie den [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision)-Service ein.
      2. [Erstellen](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys) oder [importieren](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) Sie einen Rootschlüssel (CRK) in {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Optional**: Auf Wunsch können Sie einen Standardschlüssel für die Entschlüsselung [erstellen](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) oder [importieren](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys).
      4. [Konfigurieren Sie die Key Protect-API](/docs/services/key-protect?topic=key-protect-set-up-api#set-up-api), sodass Sie das Wrapping für den Schlüssel zur Datenverschlüsselung ausführen können, den Sie zum Verschlüsseln Ihres VHD-Image nutzen möchten.
      5. Führen Sie mit dem Rootschlüssel das [Wrapping für den Datenverschlüsselungsschlüssel](/docs/services/key-protect/wrap-keys.html#wrap-keys) aus. Beim Importieren Ihres verschlüsselten Image in die {{site.data.keyword.cloud_notm}}-Konsole benötigen Sie den verschlüsselten Text, der dem mit Key-Wrapping erstellten Datenverschlüsselungsschlüssel zugeordnet ist.
3. Verwenden Sie IBM {{site.data.keyword.iamshort}} (IAM), um zwischen **Cloud Block Storage** (Quellenservice) und **{{site.data.keyword.keymanagementservicelong_notm}}** (Zielservice) den [Zugriff zu autorisieren](/docs/iam?topic=iam-serviceauth#create-auth). Für Benutzer, die verschlüsselte Images aus {{site.data.keyword.cos_full_notm}} importieren, muss in IAM für {{site.data.keyword.keymanagementservicelong_notm}} eine [Zugriffsrichtlinie definiert](/docs/iam?topic=iam-userroles#userroles) sein.
4. Erstellen Sie in IBM Cloud Console eine Instanz von {{site.data.keyword.cos_full_notm}} und erstellen Sie ein Bucket zum Speichern von Daten. Weitere Information enthält das [Lernprogramm zur Einführung in {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial).
      1. Erstellen Sie die {{site.data.keyword.cos_full_notm}}-Instanz an demselben regionalen Standort, an dem auch Ihr {{site.data.keyword.keymanagementserviceshort}}-Service bereitgestellt wird.
      2. Beim Erstellen des Buckets muss als Einstellung für die Ausfallsicherheit**** der Wert _Regional_ festgelegt sein.
      3. Optional können Sie das Bucket bei seiner Erstellung [mit Ihrem eigenen Schlüssel](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-sse-kp#sse-kp) verschlüsseln.   

## Verschlüsselte Images vorbereiten

1. Wählen Sie ein nicht verschlüsseltes Image aus, dessen Ausführung in der {{site.data.keyword.cloud_notm}}-Infrastrukturumgebung erfolgt, die Sie verschlüsseln wollen. Eine Möglichkeit hierfür besteht darin, einen vorhandenen virtuellen Server zum [Erstellen einer Imagevorlage](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template) zu verwenden. Weitere Informationen finden Sie unter [Mit einer Imagevorlage arbeiten, die von einem mit "cloud-init" bereitgestellten virtuellen Server erstellt wurde](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server). Sie können jedoch auch ein vorhandenes VHD-Image verwenden. Stellen Sie sicher, dass das Image die [Anforderungen an verschlüsselte Images](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs) erfüllt.
2. Wenn Sie eine Imagevorlage von {{site.data.keyword.slportal}} verwenden, [exportieren Sie das nicht verschlüsselte Image](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage) zu {{site.data.keyword.cos_full_notm}}.
3. Laden Sie die Imagedatei von {{site.data.keyword.cos_full_notm}} auf eine sichere lokale Maschine herunter, um das Image zu verschlüsseln. Wählen Sie in Ihrem Service-Dashboard die Aktion **Herunterladen** aus, um Ihr Objekt aus dem Speicher abzurufen. Zum Herunterladen von Images mit einer Größe von über 200 MB können Sie das Aspera-Plug-in für Hochgeschwindigkeitsübertragung verwenden.
4. Verwenden Sie das Tool 'vhd-util', um [Ihr VHD-Image zu verschlüsseln](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image).
5. Navigieren Sie in {{site.data.keyword.cos_full_notm}} zu Ihrem Bucket und klicken Sie auf **Objekte hinzufügen**, um das verschlüsselte Image [hochzuladen](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload-data#upload-data). Zum Hochladen von Images mit einer Größe von über 200 MB können Sie das Aspera-Plug-in für Hochgeschwindigkeitsübertragung verwenden.

## Verschlüsseltes Image importieren und Instanz bestellen

1. Erstellen Sie mit IBM {{site.data.keyword.iamshort}} (IAM) eine Service-ID, mit der Sie sich authentifizieren können, wenn Sie das verschlüsselte Image in die {{site.data.keyword.cloud_notm}}-Konsole importieren.
      1. Erstellen Sie eine [Service-ID](/docs/iam?topic=iam-serviceids#serviceids).
      2. Weisen Sie eine [Zugriffsrichtlinie](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy) zu. Weisen Sie den folgenden Services Zugriff zu: {{site.data.keyword.cos_full_notm}} und {{site.data.keyword.keymanagementservicelong_notm}}.
      3. Erstellen Sie einen [API-Schlüssel für eine Service-ID](/docs/iam?topic=iam-serviceidapikeys#create_service_key).
      4. Weitere Informationen enthält der Blogbeitrag [Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window}.
2. Führen Sie in der {{site.data.keyword.cloud_notm}}-Konsole den [Import des verschlüsselten Image](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos) zur Seite "Imagevorlagen" aus.
3. Auf der Seite "Imagevorlagen" können Sie Ihr verschlüsseltes Image nun zum [Bestellen einer virtuellen Serverinstanz](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template) verwenden.
4. Wenn ein verschlüsselter virtueller Server eingerichtet ist, haben Sie die Möglichkeit, Audits für [Ereignisse bei virtuellen Servern](/docs/vsi?topic=virtual-servers-at_events#at_events) mit [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov) durchzuführen.
