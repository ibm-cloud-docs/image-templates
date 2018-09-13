---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Using End to End (E2E) Encryption to provision an encrypted instance 

The End to End (E2E) Encryption feature enables you to bring your own encrypted, cloud-init enabled operating system image that you've encrypted by using a data encryption key that you own and control. After completing some environment setup, you can import your encrypted image to the image template repository and use it to provision virtual server instances on a dedicated host. E2E encryption provides data-at-rest encryption for your boot disk on the provisioned virtual server instances. To gain access to this limited availability feature, please contact Support. 
{:shortdesc}

E2E Encryption brings together several {{site.data.keyword.cloud}} components to provide a secure solution for your critical information.

* {{site.data.keyword.keymanagementservicefull_notm}} secures your keys with FIPS 140-2 Level 2 certified cloud-based hardware security modules (HSMs) that protect against the theft of information. 
* IBM {{site.data.keyword.iamshort}} (IAM) enables the Disk Encryption for {{site.data.keyword.cloud_notm}} {{site.data.keyword.virtualmachinesshort}} service to access {{site.data.keyword.keymanagementserviceshort}} and your root key that is used to wrap your data encryption key.
* {{site.data.keyword.cos_full_notm}} securely stores your encrypted image when you upload it. 
* In {{site.data.keyword.slportal}} you can import your encrypted image and create an image template. 
* With an encrypted image template available in the {{site.data.keyword.cloud_notm}} Console infrastructure environment, you can provision dedicated virtual server instances. A dedicated host ensures single tenancy. 
* Finally, you can audit events that are associated with your encrypted virtual servers through Activity Tracker.

## Preparing your environment

1. Ensure that you are using a [dedicated host](/docs/vsi/vsi_provision_dedicated.html) with GPU capability in the DAL13 data center.
2. You must have an upgraded account to use E2E encryption for virtual servers. For more informaton, see [Switching to IBMid and linking accounts](/docs/account/softlayerlink.html). 
3. Use {{site.data.keyword.keymanagementservicefull_notm}} to create and manage keys.
      1. Provision the [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/keymgmt/keyprotect_provision.html) service. 
      2. [Create](/docs/services/keymgmt/keyprotect_create_root.html) or [import](/docs/services/keymgmt/keyprotect_import_root.html) a root key (CRK) in {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Optional**: If you choose, you can [create](/docs/services/keymgmt/keyprotect_create_standard.html) or [import](/docs/services/keymgmt/keyprotect_import_standard.html) a standard key for decryption. 
      4. Obtain [access to the {{site.data.keyword.keymanagementserviceshort}} API](/docs/services/keymgmt/keyprotect_authentication.html#access-api) so that you can wrap the data encryption key.
      5. [Wrap the data encryption key (WDEK)](/docs/services/keymgmt/keyprotect_wrap_keys.html) with the root key. You will need the cipher text that is associated with the WDEK when you import your encrypted image to {{site.data.keyword.slportal}}.
4. From IBM {{site.data.keyword.iamshort}} (IAM), [authorize access](/docs/iam/authorizations.html#create-an-authorization) between Disk Encryption for {{site.data.keyword.cloud_notm}} {{site.data.keyword.virtualmachinesshort}} (source service) and {{site.data.keyword.keymanagementservicelong_notm}} (target service). Users who import encrypted images from {{site.data.keyword.cos_full_notm}} must have an [access policy defined](/docs/iam/users_roles.html) for {{site.data.keyword.keymanagementservicelong_notm}} in IAM. 
5. In IBM Cloud Console, create  an instance of {{site.data.keyword.cos_full_notm}}, create a bucket to store data and [encrypt it with your key](/docs/services/cloud-object-storage/basics/encryption.html#sse-kp). When you create the bucket, the **Resiliency** setting must be _Regional_, and the **Location** setting must be _us-south_.  
 
## Preparing your encrypted images
 
1. Select an unencrypted image that works in the {{site.data.keyword.cloud_notm}} infrastructure environment that you want to encrypt. One option is to use an existing virtual server to [create a custom image](/docs/infrastructure/image-templates/create-standard-image.html). For more information, see [Work with a standard image created from a cloud-init provisioned virtual server](/docs/infrastructure/image-templates/image_cloud-init.html#work-with-a-standard-image-created-from-a-cloud-init-provisioned-virtual-server). You can also use an existing VHD image. Ensure that the image meets [encrypted image requirements](create-encrypted-image.html#encrypted-image-reqs). 
2. If you are using an image template from {{site.data.keyword.slportal}}, [export the unencrypted image](/docs/infrastructure/image-templates/export-image.html) to {{site.data.keyword.cos_short}} OpenStack Swift.
3. [Download the image file](/docs/infrastructure/objectstorage-swift/interacting-in-portal.html#viewing-and-editing-file-details) from {{site.data.keyword.cos_short}} OpenStack Swift to a secure local machine to encrypt the image. If your image is more than 20 GB, you might need to use a temporary URL that the UI provides to download the image. 
4. Use the LUKS disk encryption format to [encrypt your image](create-encrypted-image.html#luks-disk-encryption) by using QEMU and DMCrypt. 
5. In {{site.data.keyword.cos_full_notm}}, navigate to your bucket and click **Add Objects** to [upload](/docs/services/cloud-object-storage/basics/upload.html#uploading-data) the encrypted image. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB.
 
## Importing an encrypted image and ordering an instance 
 
1. Using IBM {{site.data.keyword.iamshort}} (IAM), create a service ID to authenticate with when you import the encrypted image into {{site.data.keyword.slportal}}. 
      1. Create a [service ID](/docs/iam/serviceid.html#serviceids).
      2. Assign an [access policy](/docs/iam/serviceidaccess.html#serviceidpolicy). Assign access for these services: {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.keymanagementservicelong_notm}}.
      3. Create an [API key for a service ID](/docs/iam/serviceid_keys.html#creating-an-api-key-for-a-service-id).
      4. For more information, see [Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window}.
2. From {{site.data.keyword.slportal}}, [import the encrypted image](import-image.html#import-icos) to the Image Templates page. (You can also import the encrypted image by [using the {{site.data.keyword.slapi_short}}](import-encrypted-image-api.html).)
3. From the Image Templates page, you can use your encrypted image to [order](order-vsi-from-image-template.html) a dedicated virtual server instance. 
4. With an encrypted virtual server provisioned, you have the option to audit [virtual server events](/docs/vsi/vsi_activity_tracker_events.html#at_events) through [Activity Tracker](/docs/services/cloud-activity-tracker/activity_tracker_ov.html). 
