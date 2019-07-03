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

# Importation d'une image à partir d'IBM Cloud Object Storage dans un compte IBM Cloud for Government
{: import-an-image-from-ibm-cloud-object-storage-to-ibm-cloud-for-government-account}

Pour importer une image à partir d'{{site.data.keyword.cos_full}} dans un compte {{site.data.keyword.ibmcloudgov_full_notm}}, vous devez utiliser l'API {{site.data.keyword.slapi_short}}. Pour plus d'informations sur l'API {{site.data.keyword.slapi_short}} et les API de serveur virtuel, consultez les ressources suivantes dans {{site.data.keyword.sldn_full}} :
* [{{site.data.keyword.slapi_short}} Overview ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://sldn.softlayer.com/reference/softlayerapi/){: new_window}
* [Getting Started with the {{site.data.keyword.slapi_short}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://sldn.softlayer.com/article/getting-started/){: new_window}

## Importation d'une image
{: import-gov-image}

Pour importer une image d'{{site.data.keyword.cos_full_notm}} dans un compte {{site.data.keyword.ibmcloudgov_full_notm}}, envoyez une demande POST à https://api.usgov.softlayer.com/rest/v3.1/SoftLayer_Virtual_Guest_Block_Device_Template_Group/createFromIcos.json avec le code JSON suivant dans le corps de la demande.

Le corps de demande JSON suivant est un exemple générique.
{:note}

### Exemple de corps de demande JSON

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

