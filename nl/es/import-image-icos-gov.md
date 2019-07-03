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

# Importar una imagen de IBM Cloud Object Storage a una cuenta de IBM Cloud for Government
{: import-an-image-from-ibm-cloud-object-storage-to-ibm-cloud-for-government-account}

Para importar una imagen de {{site.data.keyword.cos_full}} a una cuenta de {{site.data.keyword.ibmcloudgov_full_notm}}, debe utilizar la {{site.data.keyword.slapi_short}}. Para obtener más información sobre la {{site.data.keyword.slapi_short}} y las API del servidor virtual, consulte los recursos siguientes en {{site.data.keyword.sldn_full}}:
* [Visión general de {{site.data.keyword.slapi_short}} ![icono de enlace externo](../icons/launch-glyph.svg "icono de enlace externo")](https://sldn.softlayer.com/reference/softlayerapi/){: new_window}
* [Iniciación a la {{site.data.keyword.slapi_short}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://sldn.softlayer.com/article/getting-started/){: new_window}

## Importación de una imagen
{: import-gov-image}

Para importar una imagen de {{site.data.keyword.cos_full_notm}} a una cuenta de {{site.data.keyword.ibmcloudgov_full_notm}}, envíe una solicitud POST a https://api.usgov.softlayer.com/rest/v3.1/SoftLayer_Virtual_Guest_Block_Device_Template_Group/createFromIcos.json con el siguiente JSON en el cuerpo de solicitud.

El cuerpo de solicitud JSON siguiente es un ejemplo genérico.
{:note}

### Cuerpo de solicitud JSON de ejemplo

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

