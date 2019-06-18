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


# Importing an encrypted image by using the SoftLayer API
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

You can use the {{site.data.keyword.slapi_short}} to import an encrypted image from {{site.data.keyword.cos_full}}
and create an image template. When your image template is created, you can use it to provision instances.
{:shortdesc}

To limit access to only the information that is needed to complete the import task, authenticate with a service ID. The service ID should have access only to the encrypted image in IBM Cloud Object Storage that you want to import and the Key Protect instance where your root key is stored.  

The following python snippet shows an example of how you can access the
[SoftLayer_Virtual_Guest_Block_Device_Template_Group ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) API and use the
_createFromIcos_ method to create an image template.

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


For more information about locating values that are needed to import the encrypted image from {{site.data.keyword.cos_full_notm}}, see the following table.

| Field    | Value   |
| -------- | ------- |
| ibmApiKey | Specify the API key that you noted when you created it. If the API key is lost, you must create a new API key. For more information, see [Managing your API keys](/docs/iam?topic=iam-userapikey#userapikey). |
| crkCrn | Specify the [Cloud Resource Name (CRN)](/docs/overview?topic=overview-crn) for the root key that you used to wrap your data encryption key.  To locate and copy your root key CRN, go to the [{{site.data.keyword.keymanagementserviceshort}} service instance ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/resources){: new_window}, hover over your root key, click the ellipsis **(...)** on the far right side of the screen, then select the "View CRN" option and click the copy icon.  |
| wrappedDek | Specify the cipher text that is associated with your wrapped data encryption key that you used to encrypt your image. For more information, see [Wrapping keys by using the API](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys). |
{: caption="Table 1. Values needed for importing encrypted image" caption-side="top"}
