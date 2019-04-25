---

copyright:
  years: 2019
lastupdated: "2019-04-01"

keywords:  fed, ic4g

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:note: .note}
{:screen: .screen}
{:new_window: target="_blank"}

# Importa un'immagine da IBM Cloud Object Storage a un account IBM Cloud for Government
{: import-an-image-from-ibm-cloud-object-storage-to-ibm-cloud-for-government-account}

Per importare un'immagine da {{site.data.keyword.cos_full}} a un account {{site.data.keyword.ibmcloudgov_full_notm}}, devi utilizzare {{site.data.keyword.slapi_short}}. Per ulteriori informazioni sulla {{site.data.keyword.slapi_short}} e sulle API del server virtuale, consulta le seguenti risorse in
{{site.data.keyword.sldn_full}}:
* [Panoramica di {{site.data.keyword.slapi_short}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://softlayer.github.io/reference/softlayerapi/){: new_window}
* [Introduzione a {{site.data.keyword.slapi_short}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://softlayer.github.io/article/getting-started/){: new_window}

## Importazione di un'immagine
{: import-gov-image}

Per importare un'immagine da {{site.data.keyword.cos_full_notm}} a un account {{site.data.keyword.ibmcloudgov_full_notm}}, inoltra una richiesta POST a https://api.usgov.softlayer.com/rest/v3.1/SoftLayer_Virtual_Guest_Block_Device_Template_Group/createFromIcos.json con il seguente JSON nel corpo della richiesta.

Il corpo della richiesta JSON riportato di seguito è un esempio generico.
{:note}

### Corpo della richiesta JSON di esempio

```
{
    "parameters": [
        {
            "name": "IMAGE_NAME",
            "note": "NOTE_FOR_IMAGE",
            "operatingSystemReferenceCode": "UBUNTU_16_64",
            "uri": "cos://us-south/my-bucket-name/my-image-object-name.vhd",
            "isEncrypted": 0,
            "ibmAccessKey": "MY_HMAC_ACCESS_KEY",
            "ibmSecretKey": "MY_HMAC_SECRET_KEY"
        }
    ]
}
```
