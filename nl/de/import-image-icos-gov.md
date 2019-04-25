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

# Image aus IBM Cloud Object Storage in IBM Cloud for Government-Konto importieren
{: import-an-image-from-ibm-cloud-object-storage-to-ibm-cloud-for-government-account}

Um ein Image aus {{site.data.keyword.cos_full}} in ein {{site.data.keyword.ibmcloudgov_full_notm}}-Konto zu importieren, müssen Sie die {{site.data.keyword.slapi_short}} verwenden. Weitere Informationen zur {{site.data.keyword.slapi_short}} und den APIs für virtuelle Server enthalten die folgenden Ressourcen im {{site.data.keyword.sldn_full}}:
* [Übersicht über {{site.data.keyword.slapi_short}} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/reference/softlayerapi/){: new_window}
* [Einführung in {{site.data.keyword.slapi_short}} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/article/getting-started/){: new_window}

## Image importieren
{: import-gov-image}

Um ein Image aus {{site.data.keyword.cos_full_notm}} in ein {{site.data.keyword.ibmcloudgov_full_notm}}-Konto zu importieren, müssen Sie mit dem folgenden JSON-Anforderungshauptteil eine POST-Anforderung bei https://api.usgov.softlayer.com/rest/v3.1/SoftLayer_Virtual_Guest_Block_Device_Template_Group/createFromIcos.json einreichen.

Bei dem folgenden JSON-Anforderungshauptteil handelt es sich um ein allgemeines Beispiel.
{:note}

### Beispiel für JSON-Anforderungshauptteil

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
