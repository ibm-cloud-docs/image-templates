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


# Importazione di un'immagine crittografata utilizzando l'API SoftLayer
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

Puoi utilizzare l'{{site.data.keyword.slapi_short}} per importare un'immagine crittografata da {{site.data.keyword.cos_full}}
e creare un template dell'immagine. Quando il tuo template dell'immagine viene creato, puoi utilizzarlo per eseguire il provisioning delle istanze.
{:shortdesc}

Per limitare l'accesso alle sole informazioni necessarie per completare l'attività di importazione, esegui l'autenticazione con un ID servizio. L'ID servizio deve avere accesso solo all'immagine crittografata nell'IBM Cloud Object Storage che desideri importare e all'istanza Key Protect in cui è memorizzata la tua chiave root.  

Il frammento python riportato di seguito mostra un esempio di come puoi accedere all'API
[SoftLayer_Virtual_Guest_Block_Device_Template_Group ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/)
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
      'crkCrn': 'crn:v1:bluemix:public:hs-crypto:us-south:a/0d06ba51fa0e431290956d1761da1b7b:5ef6cebe-26d7-4ef3-abdc-fb50f345780f:key:a9640391-aec5-4c86-8942-6e6c59bb40b5',
      'wrappedDek':'my wrapped DEK',
      }
ret = group_svc.createFromIcos(config)
print(ret)
```
{: codeblock}


Per ulteriori informazioni sull'individuazione dei valori necessari per importare l'immagine crittografata da {{site.data.keyword.cos_full_notm}}, vedi la tabella riportata di seguito.

| Campo    | Valore   |
| -------- | ------- |
| ibmApiKey | Specifica la chiave API di cui hai preso nota nel momento in cui l'hai creata. Se la chiave API viene persa, dovrai crearne una nuova. Per ulteriori informazioni, vedi [Gestione delle tue chiavi API](/docs/iam?topic=iam-userapikey#userapikey). |
| crkCrn | Specifica il [CRN (Cloud Resource Name)](/docs/overview?topic=overview-crn) per la chiave root che hai utilizzato per impacchettare la tua chiave di crittografia dei dati. Per individuare e copiare il CRN della tua chiave root, vai all'istanza del servizio [{{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/resources){: new_window}, passa il puntatore del mouse sulla tua chiave root, fai clic sui tre puntini **(...)** sull'estrema destra dello schermo, seleziona quindi l'opzione "Visualizza CRN" e fai clic sull'icona di copia. |
| wrappedDek | Specifica il testo crittografato associato alla tua chiave di crittografia dei dati impacchettata utilizzata per crittografare la tua immagine. Per ulteriori informazioni, vedi [Impacchettamento delle chiavi utilizzando l'API](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys). |
{: caption="Tabella 1. Valori necessari per importare l'immagine crittografata" caption-side="top"}
