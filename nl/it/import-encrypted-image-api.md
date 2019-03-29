---

copyright:
  years: 2018
lastupdated: "2018-08-09"

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


# Importazione di un'immagine crittografata utilizzando l'API SoftLayer
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

Puoi utilizzare l'{{site.data.keyword.slapi_short}} per importare un'immagine crittografata da {{site.data.keyword.cos_full}}
e creare un template dell'immagine. Quando il tuo template dell'immagine viene creato, puoi utilizzarlo per eseguire il provisioning delle istanze.
{:shortdesc}

Per limitare l'accesso alle sole informazioni necessarie per completare l'attività di importazione, esegui l'autenticazione con un ID servizio. L'ID servizio deve avere accesso solo all'immagine crittografata nell'IBM Cloud Object Storage che desideri importare e all'istanza Key Protect in cui è memorizzata la tua chiave root.  

Il frammento python riportato di seguito mostra un esempio di come puoi accedere all'API
[SoftLayer_Virtual_Guest_Block_Device_Template_Group ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/)
e di come puoi utilizzare il metodo _createFromIcos_ per creare un template dell'immagine.

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


Per ulteriori informazioni sull'individuazione dei valori necessari per importare l'immagine crittografata da {{site.data.keyword.cos_full_notm}}, vedi la tabella riportata di seguito.

| Campo    | Valore   |
| -------- | ------- |
| ibmApiKey | Specifica la chiave API di cui hai preso nota nel momento in cui l'hai creata. Se la chiave API viene persa, dovrai crearne una nuova. Per ulteriori informazioni, vedi [Gestione delle tue chiavi API](/docs/iam?topic=iam-userapikey). |
| rootKeyId | Specifica l'ID della chiave root che è stata utilizzata per impacchettare la chiave di crittografia dei dati. Per ulteriori informazioni, vedi [Visualizzazione delle chiavi](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
| wrappedDek | Specifica il testo crittografato associato alla tua chiave di crittografia dei dati impacchettata utilizzata per crittografare la tua immagine. Per ulteriori informazioni, vedi [Impacchettamento delle chiavi utilizzando l'API](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys). |
| keyProtectId | Puoi utilizzare la CLI {{site.data.keyword.cloud_notm}} per trovare il tuo ID dell'istanza {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi [Richiamo del tuo ID dell'istanza](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID). |
{: caption="Tabella 1. Valori necessari per importare l'immagine crittografata" caption-side="top"}
