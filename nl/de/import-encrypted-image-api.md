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
{:python: .ph data-hd-programlang='python'}
{:table: .aria-labeledby="caption"}


# Verschlüsseltes Image mit der SoftLayer-API importieren

Mit der {{site.data.keyword.slapi_short}} können Sie ein verschlüsseltes Image aus {{site.data.keyword.cos_full}} importieren und eine Imagevorlage erstellen. Nachdem Ihre Imagevorlage erstellt worden ist, können Sie sie zur Bereitstellung (Einrichtung) von Instanzen verwenden.
{:shortdesc}

Um den Zugriff auf nur diejenigen Informationen zu beschränken, die für die Durchführung der Importtask nötig sind, authentifizieren Sie sich mit einer Service-ID. Die Service-ID sollte nur Zugriff auf das verschlüsselte Image in IBM Cloud Object Storage, das Sie importieren möchten, und auf die Key Protect-Instanz haben, in der Ihr Rootschlüssel gespeichert ist.  

Das folgende Python-Snippet veranschaulicht anhand eines Beispiels, wie Sie auf die API
[SoftLayer_Virtual_Guest_Block_Device_Template_Group ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) API zugreifen und eine Imagevorlage mit der Methode _createFromIcos_ erstellen können.

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
      'rootKeyId':'my root key ID',
      'wrappedDek':'my wrapped DEK',
      'keyProtectId':'<key_protect_instance_id>',
      }
ret = group_svc.createFromIcos(config)
print(ret)
```
{: codeblock}


Weitere Informationen dazu, wie Sie Werte finden, die zum Importieren des verschlüsselten Image von {{site.data.keyword.cos_full_notm}} benötigt werden, enthält die folgende Tabelle. 

| Feld    | Wert   |
| -------- | ------- |
| ibmApiKey | Geben Sie den API-Schlüssel an, den Sie bei seiner Erstellung notiert haben. Wenn der API-Schlüssel verloren geht, müssen Sie einen neuen API-Schlüssel erzeugen. Weitere Informationen finden Sie unter [API-Schlüssel verwalten](/docs/iam?topic=iam-userapikey). |
| rootKeyId | Geben Sie die ID des Rootschlüssels an, der für das Key-Wrapping des Datenverschlüsselungsschlüssels verwendet wurde. Weitere Informationen finden Sie unter [Schlüssel anzeigen](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
| wrappedDek | Geben Sie den verschlüsselten Text an, der Ihrem mit Key-Wrapping erstellten Datenverschlüsselungsschlüssel zugeordnet ist, den Sie zum Verschlüsseln Ihres Image verwendet haben. Weitere Informationen finden Sie unter [Wrapping für Schlüssel über die API durchführen](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys). |
| keyProtectId | Über die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.cloud_notm}} können Sie die ID für Ihre Instanz von {{site.data.keyword.keymanagementserviceshort}} suchen. Weitere Information finden Sie unter [Instanz-ID abrufen](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID). |
{: caption="Tabelle 1. Werte, die zum Importieren verschlüsselter Images erforderlich sind " caption-side="top"}
