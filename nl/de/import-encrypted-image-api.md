---

copyright:
  years: 2018
lastupdated: "2019-05-13"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:python: .ph data-hd-programlang='python'}
{:table: .aria-labeledby="caption"}


# Verschlüsseltes Image mit der SoftLayer-API importieren
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

Mit der {{site.data.keyword.slapi_short}} können Sie ein verschlüsseltes Image aus {{site.data.keyword.cos_full}} importieren und eine Imagevorlage erstellen. Nachdem Ihre Imagevorlage erstellt worden ist, können Sie sie zur Bereitstellung (Einrichtung) von Instanzen verwenden.
{:shortdesc}

Um den Zugriff auf nur diejenigen Informationen zu beschränken, die für die Durchführung der Importtask nötig sind, authentifizieren Sie sich mit einer Service-ID. Die Service-ID sollte nur Zugriff auf das verschlüsselte Image in IBM Cloud Object Storage, das Sie importieren möchten, und auf die Key Protect-Instanz haben, in der Ihr Rootschlüssel gespeichert ist.  

Das folgende Python-Snippet veranschaulicht anhand eines Beispiels, wie Sie auf die API
[SoftLayer_Virtual_Guest_Block_Device_Template_Group ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) API zugreifen und eine Imagevorlage mit der Methode _createFromIcos_ erstellen können.

```python
import SoftLayer

client = SoftLayer.create_client_from_env(username='<user>',
                        api_key='<api_key>',
                        endpoint_url='https://api.softlayer.com/rest/v3',
                        timeout=240)
group_svc = client['Virtual_Guest_Block_Device_Template_Group']
config = {'name':'my_encrypted_image',
      'note':'my note',
      'operatingSystemReferenceCode':'REDHAT_7_64',
      'uri':'cos://<region_name>/<bucket_name>/xx123.Rhel_7_encrypted.raw',
      'bootMode':'my boot mode',
      'cloud-init': True,
      'byol': True,
      'encrypted': True,
      'ibmApiKey':'<api_key>',
      'crkCrn': 'crn:v1:bluemix:public:hs-crypto:us-south:a/0d06ba51fa0e431290956d1761da1b7b:5ef6cebe-26d7-4ef3-abdc-fb50f345780f:key:a9640391-aec5-4c86-8942-6e6c59bb40b5',
      'wrappedDek':'my wrapped DEK',
      }
ret = group_svc.createFromIcos(config)
print(ret)
```
{: codeblock}


Weitere Informationen dazu, wie Sie Werte finden, die zum Importieren des verschlüsselten Image von {{site.data.keyword.cos_full_notm}} benötigt werden, enthält die folgende Tabelle.

| Feld    | Wert   |
| -------- | ------- |
| ibmApiKey | Geben Sie den API-Schlüssel an, den Sie bei seiner Erstellung notiert haben. Wenn der API-Schlüssel verloren geht, müssen Sie einen neuen API-Schlüssel erzeugen. Weitere Informationen finden Sie unter [API-Schlüssel verwalten](/docs/iam?topic=iam-userapikey#userapikey). |
| crkCrn | Geben Sie den [CRN (Cloudressourcennamen)](/docs/overview?topic=overview-crn) für den Rootschlüssel an, den Sie für das Wrapping des Datenverschlüsselungsschlüssels verwendet haben. Um den Rootschlüssel-CRN zu suchen und zu kopieren, rufen Sie die [{{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/resources){: new_window} auf, bewegen den Mauszeiger über den Rootschlüssel, klicken auf die Auslassungspunkte **(...)** auf der rechten Seite des Bildschirms und wählen dann die Option "CRN anzeigen" aus; klicken Sie anschließend auf das Kopiersymbol.  |
| wrappedDek | Geben Sie den verschlüsselten Text an, der Ihrem mit Key-Wrapping erstellten Datenverschlüsselungsschlüssel zugeordnet ist, den Sie zum Verschlüsseln Ihres Image verwendet haben. Weitere Informationen finden Sie unter [Wrapping für Schlüssel über die API durchführen](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys). |
{: caption="Tabelle 1. Werte, die zum Importieren verschlüsselter Images erforderlich sind" caption-side="top"}
