---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-20"

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

* An IBM key management service such as {{site.data.keyword.keymanagementservicelong_notm}} or {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} to secure your encryption keys (see Table 1).
* IBM {{site.data.keyword.iamshort}} (IAM) enables the Cloud Block Storage service to access your key management system and your root key that is used to wrap your data encryption key.
* {{site.data.keyword.cos_full_notm}} securely stores your encrypted image when you upload it.
* In {{site.data.keyword.cloud_notm}} console you can import your encrypted image and create an image template.
* With an encrypted image template available in the {{site.data.keyword.cloud_notm}} console infrastructure environment, you can provision encrypted virtual server instances.
* Finally, you can audit events that are associated with your encrypted virtual servers through [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).

## Encryption key management services

{{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} (now available in certain [regions](/docs/services/hs-crypto?topic=hs-crypto-regions#regions)) use a common key provider API to provide a consistent approach for managing encryption keys.  Behind the scenes, {{site.data.keyword.cloud_notm}} data centers provide a dedicated hardware security module (HSM) to protect your keys.  You may choose from the following options: 

| Key Management Service | HSM Encryption Certification |
| ----- | ----- |
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/concepts?topic=key-protect-getting-started-tutorial#getting-started-tutorial) | FIPS 140-2 *Level 2* compliance |
| [{{site.data.keyword.hscrypto}}](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) | FIPS 140-2 *Level 4* compliance |
{: caption="Table 1. Available key management service options" caption-side="top"}

## Preparing your environment

1. You must have an upgraded account to use E2E encryption for virtual servers. For more information, see [Switching to IBMid and linking accounts](/docs/account?topic=account-unifyingaccounts).
2. Use your key management service to create and manage keys.  The following example steps are specific to {{site.data.keyword.keymanagementserviceshort}}, but the general flow also applies to {{site.data.keyword.hscrypto}}. If you're using {{site.data.keyword.hscrypto}}, see the [documentation](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) for that service for corresponding instructions.
      1. Provision the [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision) service.
      2. [Create](/docs/services/key-protect?topic=key-protect-create-root-keys) or [import](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) a root key (CRK) in {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Optional**: If you choose, you can [create](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) or [import](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) a standard key for decryption.      
      4. [Set up the {{site.data.keyword.cloud_notm}} Key Protect CLI plug-in](/docs/services/key-protect?topic=key-protect-set-up-cli) so you can [wrap the data encryption key](/docs/services/key-protect?topic=key-protect-cli-reference#kp-wrap) that you intend to use to encrypt your VHD image with the root key. You need the cipher text that is associated with the wrapped data encryption key (WDEK) when you import your encrypted image to {{site.data.keyword.cloud_notm}} console.  
         
       Key Protect doesn't save additional authentication data (AAD), but you can still use AAD to further secure a key with up to              255 strings, each delimited by a comma and containing up to 255 characters.  If you supply AAD for key wrapping, save the data          to a secure location to ensure that you can access and provide the same AAD on future key unwrapping requests.
       {: tip}
      
3. From IBM {{site.data.keyword.iamshort}} (IAM), [authorize access](/docs/iam?topic=iam-serviceauth#create-auth) between your **Cloud Block Storage** (source service) and your **Key Management Service** (target service). If you import encrypted images from {{site.data.keyword.cos_full_notm}} must have an [access policy defined](/docs/iam?topic=iam-userroles#userroles) for your key management service in IAM.
4. In IBM Cloud Console, create  an instance of {{site.data.keyword.cos_full_notm}} and create a bucket to store data. For more information, see the [Getting started tutorial for {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)
      1. Create the {{site.data.keyword.cos_full_notm}} instance in the region where your key management service is provisioned.
      2. When you create the bucket, the **Resiliency** setting must be _Regional_.
      3. Optionally, when you create the bucket, you can [encrypt it with your key](/docs/services/cloud-object-storage?topic=cloud-object-storage-encryption#encryption-kp).   

## Preparing your encrypted images

1. Select an unencrypted image that works in the {{site.data.keyword.cloud_notm}} infrastructure environment that you want to encrypt. One option is to use an existing virtual server to [create an image template](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template). For more information, see [Work with an image template created from a cloud-init provisioned virtual server](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server). You can also use an existing VHD image. Ensure that the image meets [encrypted image requirements](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs).
2. If you're using an image template from {{site.data.keyword.slportal}}, [export the unencrypted image](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage) to {{site.data.keyword.cos_full_notm}}.
3. Download the image file from {{site.data.keyword.cos_full_notm}} to a secure local machine to encrypt the image. In your service dashboard, select the **Download** action to retrieve your object from storage. You can use the Aspera high-speed transfer plug-in to download images larger than 200 MB.
4. Use the vhd-util tool to [encrypt your VHD image](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image).
5. In {{site.data.keyword.cos_full_notm}}, navigate to your bucket and click **Add Objects** to [upload](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) the encrypted image. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB.

## Importing an encrypted image and ordering an instance

1. Using IBM {{site.data.keyword.iamshort}} (IAM), create a service ID to authenticate with when you import the encrypted image into {{site.data.keyword.cloud_notm}} console.
      1. Create a [service ID](/docs/iam?topic=iam-serviceids#serviceids).
      2. Assign an [access policy](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Assign access for these services: {{site.data.keyword.cos_full_notm}} and key management.
      3. Create an [API key for a service ID](/docs/iam?topic=iam-serviceidapikeys#create_service_key).
      4. For more information, see [Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/blog/introducing-ibm-cloud-iam-service-ids-api-keys){: new_window}.
2. From {{site.data.keyword.cloud_notm}} console, [import the encrypted image](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos) to the Image Templates page.
3. From the Image Templates page, you can use your encrypted image to [order](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template) a virtual server instance.
4. With an encrypted virtual server provisioned, you have the option to audit [virtual server events](/docs/vsi?topic=virtual-servers-at_events#at_events) through [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).
