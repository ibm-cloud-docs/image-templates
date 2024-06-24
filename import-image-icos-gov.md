---

copyright:
  years: 2019, 2021
lastupdated: "2021-05-20"

keywords:  fed, ic4g

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:note: .note}
{:screen: .screen}
{:external: target="_blank" .external}

# Import an image from IBM Cloud Object Storage to IBM Cloud for Government account
{: import-an-image-from-ibm-cloud-object-storage-to-ibm-cloud-for-government-account}

To import an image from {{site.data.keyword.cos_full}} to an {{site.data.keyword.ibmcloudgov_full_notm}} account, you need to use the {{site.data.keyword.slapi_short}}. For more information about the {{site.data.keyword.slapi_short}} and virtual server APIs, see the following resources in the {{site.data.keyword.sldn_full}}:
* [{{site.data.keyword.slapi_short}} Overview ](https://sldn.softlayer.com/reference/softlayerapi/){: external}
* [Getting Started with the {{site.data.keyword.slapi_short}}]((https://sldn.softlayer.com/article/getting-started/){: external}

## Importing an image
{: import-gov-image}

To import an image from {{site.data.keyword.cos_full_notm}} to an {{site.data.keyword.ibmcloudgov_full_notm}} account, submit a POST request to `https://api.usgov.softlayer.com/rest/v3.1/SoftLayer_Virtual_Guest_Block_Device_Template_Group/createFromIcos.json` with the following JSON in the request body.

The following JSON request body is a generic example.
{: note}

### Example JSON request body

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

