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

# Importar uma imagem do IBM Cloud Object Storage para a conta do IBM Cloud for Government
{: import-an-image-from-ibm-cloud-object-storage-to-ibm-cloud-for-government-account}

Para importar uma imagem do {{site.data.keyword.cos_full}} para uma conta do {{site.data.keyword.ibmcloudgov_full_notm}}, deve-se usar o {{site.data.keyword.slapi_short}}. Para obter mais informações sobre o {{site.data.keyword.slapi_short}} e as APIs de servidor virtual, consulte os recursos a seguir no {{site.data.keyword.sldn_full}}:
* [{{site.data.keyword.slapi_short}} Visão geral ![Ícone de linkexterno](../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer.github.io/reference/softlayerapi/){: new_window}

* [Introdução ao {{site.data.keyword.slapi_short}} ![Ícone de linkexterno](../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer.github.io/article/getting-started/){: new_window}


## Importando uma imagem
{: import-gov-image}

Para importar uma imagem do {{site.data.keyword.cos_full_notm}} para uma conta do {{site.data.keyword.ibmcloudgov_full_notm}}, envie uma solicitação de POST para https://api.usgov.softlayer.com/rest/v3.1/SoftLayer_Virtual_Guest_Block_Device_Template_Group/createFromIcos.json com o JSON a seguir no corpo da solicitação.

O corpo da solicitação JSON a seguir é um exemplo genérico.
{:note}

### Exemplo de corpo da solicitação JSON

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
