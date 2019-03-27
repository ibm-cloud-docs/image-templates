---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-27"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Using End to End (E2E) Encryption to provision an encrypted instance
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

The End to End (E2E) Encryption feature enables you to bring your own encrypted, cloud-init enabled operating system image that you've encrypted by using a data encryption key that you own and control. After completing some environment setup, you can import your encrypted image to the image template repository and use it to provision encrypted virtual server instances. E2E encryption provides data-at-rest encryption for the storage that is associated with provisioned virtual server instances.
{:shortdesc}

copyright:
  years: 2014, 2019
lastupdated: "2019-03-27"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Using End to End (E2E) Encryption to provision an encrypted instance
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

The End to End (E2E) Encryption feature enables you to bring your own encrypted, cloud-init enabled operating system image that you've encrypted by using a data encryption key that you own and control. After completing some environment setup, you can import your encrypted image to the image template repository and use it to provision encrypted virtual server instances. E2E encryption provides data-at-rest encryption for the storage that is associated with provisioned virtual server instances.
{:shortdesc}

E2E Encryption brings together several {{site.data.keyword.cloud}} components to provide a secure solution for your critical information.

* {{site.data.keyword.keymanagementservicefull_notm}} secures your keys with FIPS 140-2 Level 2 certified cloud-based hardware security modules (HSMs) that protect against the theft of information.
* IBM {{site.data.keyword.iamshort}} (IAM) enables the Cloud Block Storage service to access {{site.data.keyword.keymanagementserviceshort}} and your root key that is used to wrap your data encryption key.
* {{site.data.keyword.cos_full_notm}} securely stores your encrypted image when you upload it.
* In {{site.data.keyword.slportal}} you can import your encrypted image and create an image template.
* With an encrypted image template available in the {{site.data.keyword.cloud_notm}} Console infrastructure environment, you can provision encrypted virtual server instances.
* Finally, you have the option to audit events that are associated with your encrypted virtual servers through [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).

## Preparing your environment

1. You must have an upgraded account to use E2E encryption for virtual servers. For more informaton, see [Switching to IBMid and linking accounts](/docs/account/softlayerlink.html).
2. Use {{site.data.keyword.keymanagementservicefull_notm}} to create and manage keys.
      1. Provision the [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision) service.
      2. [Create](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys) or [import](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) a root key (CRK) in {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Optional**: If you choose, you can [create](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) or [import](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) a standard key for decryption.
      4. [Set up the Key Protect API](/docs/services/key-protect?topic=key-protect-set-up-api#set-up-api) so that you can wrap the data encryption key that you intend to use to encrypt your VHD image.
      5. [Wrap the data encryption key](/docs/services/key-protect/wrap-keys.html#wrap-keys) with the root key. You will need the cipher text that is associated with the wrapped data encryption key (WDEK) when you import your encrypted image to {{site.data.keyword.slportal}}.
3. From IBM {{site.data.keyword.iamshort}} (IAM), [authorize access](/docs/iam?topic=iam-serviceauth#create-auth) between **Cloud Block Storage** (source service) and **{{site.data.keyword.keymanagementservicelong_notm}}** (target service). Users who import encrypted images from {{site.data.keyword.cos_full_notm}} must have an [access policy defined](/docs/iam?topic=iam-userroles#userroles) for {{site.data.keyword.keymanagementservicelong_notm}} in IAM.
4. In IBM Cloud Console, create  an instance of {{site.data.keyword.cos_full_notm}} and create a bucket to store data. For more information, see [Getting started tutorial for {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial).
      1. Create the {{site.data.keyword.cos_full_notm}} instance in the same regional location as your {{site.data.keyword.keymanagementserviceshort}} service is provisioned.
      2. When you create the bucket, the **Resiliency** setting must be _Regional_.
      3. Optionally, when you create the bucket, you can [encrypt it with your key](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-sse-kp#sse-kp).   

## Preparing your encrypted images

1. Select an unencrypted image that works in the {{site.data.keyword.cloud_notm}} infrastructure environment that you want to encrypt. One option is to use an existing virtual server to [create an image template](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template). For more information, see [Work with an image template created from a cloud-init provisioned virtual server](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server). You can also use an existing VHD image. Ensure that the image meets [encrypted image requirements](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs).
2. If you are using an image template from {{site.data.keyword.slportal}}, [export the unencrypted image](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage) to {{site.data.keyword.cos_full_notm}}.
3. Download the image file from {{site.data.keyword.cos_full_notm}} to a secure local machine to encrypt the image. In your service dashboard, select the **Download** action to retrieve your object from storage. You can use the Aspera high-speed transfer plug-in to download images larger than 200 MB.
4. Use the vhd-util tool to [encrypt your VHD image](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image).
5. In {{site.data.keyword.cos_full_notm}}, navigate to your bucket and click **Add Objects** to [upload](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload-data#upload-data) the encrypted image. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB.

## Importing an encrypted image and ordering an instance

1. Using IBM {{site.data.keyword.iamshort}} (IAM), create a service ID to authenticate with when you import the encrypted image into {{site.data.keyword.slportal}}.
      1. Create a [service ID](/docs/iam?topic=iam-serviceids#serviceids).
      2. Assign an [access policy](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Assign access for these services: {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.keymanagementservicelong_notm}}.
      3. Create an [API key for a service ID](/docs/iam?topic=iam-serviceidapikeys#create_service_key).
      4. For more information, see [Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window}.
2. From {{site.data.keyword.slportal}}, [import the encrypted image](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos) to the Image Templates page.
3. From the Image Templates page, you can use your encrypted image to [order](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template) a virtual server instance.
4. With an encrypted virtual server provisioned, you have the option to audit [virtual server events](/docs/vsi?topic=virtual-servers-at_events#at_events) through [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).
