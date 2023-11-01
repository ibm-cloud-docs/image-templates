---

copyright:
  years: 2014, 2023
lastupdated: "2023-11-01"

keywords: image template, image, import image, {{site.data.keyword.cloud}}, {{site.data.keyword.cloud_notm}}

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:screen: .screen}
{:external: target="_blank" .external}


# Preparing and importing images
{: #preparing-and-importing-images}

You use the Image Templates screen in the {{site.data.keyword.cloud}} console to import an image from an [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage) service instance. You can import images to classic infrastructure that are in Virtual Hard Disk (VHD), Virtual Machine Disk (VMDK) or custom ISO 9660 format.  After an image is uploaded to a bucket in an {{site.data.keyword.cos_full_notm}} service instance, you can import it to the image templates repository in the {{site.data.keyword.cloud_notm}} console.  VMDK images are converted to VHD after the import.
{: shortdesc}

You must have an upgraded account to import images from {{site.data.keyword.cos_full_notm}}. For more information, see [Switching to IBMid and linking accounts](/docs/account?topic=account-account-getting-started).
{: tip}

These instructions are specific to importing images to the image templates repository for classic infrastructure. These instructions do not apply to {{site.data.keyword.vpc_short}} infrastructure. 
{: note}

You must have an [IBM Cloud Object Storage instance](/docs/cloud-object-storage?topic=cloud-object-storage-provision#provision-account) that was ordered through the {{site.data.keyword.cloud_notm}} console to use this import feature. IBM Cloud Object Storage from control.softlayer.com is not supported.
{: important}

After images are imported as an image template, they can be used to provision or start an existing virtual server.  VHD and VMDK image imports are restricted to the following 64-bit operating systems:  

* CentOS 7 and 8
* Debian 9 and 10
* Windows Server Standard 2012, 2012 R2, 2016, and 2019
* RedHat Enterprise Linux 7 and 8
* Ubuntu 18.04 and Ubuntu 20.04

Imports are limited to 100 GB disks. Only the boot volume is imported, not secondary disks. Imported images must be named according to the following example: filename.vhd-0.vhd or filename.vmdk-0.vmdk

## ISO Templates
{: #iso-templates}

{{site.data.keyword.cloud_notm}} Supported Operating Systems can be used only to load an ISO Template onto a virtual server instance. For a list of supported operating systems, see [Supported operating systems for IBM Cloud virtual servers](/docs/bare-metal?topic=bare-metal-about-software&locale=en#supported-operating-systems-for-ibm-cloud-virtual-servers)

ISOs that are imported by using this tool must be bootable in order for the image to be eligible for import.

## Configuring unencrypted custom images for {{site.data.keyword.virtualmachinesshort}}
{: #config-image-vsi}

### Configuring a Linux custom image
{: #linux-custom-image}

To make sure that an image can be successfully deployed in the {{site.data.keyword.BluSoftlayer_notm}} infrastructure environment, virtual server images must be configured to the following specifications:

* ***/boot*** must be mounted to the first partition
* ***/boot*** and ***/*** must be mounted to separate partitions, and each must be formatted with the ext3 or ext4 file system
* ***/etc***  and ***/root*** must be on the same partition as ***/***
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** to mount the swap disk that is attached to the system

    The Linux boot volume does not support LVM.
    {: note}
* wget must be installed

    Guest additions are used for reporting performance and health of virtual server instances. Without performance and health information, virtual server instances might be marked for offline migrations when a critical maintenance is needed.
    {: note}

    Don't disable or remove the default guest additions that are installed on {{site.data.keyword.BluSoftlayer_notm}} supplied images.
    {: important}

* The latest supported guest tools must be installed. Complete the following steps to install guest tools.
    1. Download the [Linux Guest Tool package](http://downloads.service.networklayer.com/citrix/xen/CitrixHypervisor-LinuxGuestTools-7.20.0-1.tar.gz){: external}.

    2. Extract the package and follow the directions that are in the `README.txt`.

* Make sure that your image is cloud-init enabled.

    - For more information about cloud-init, see the [cloud-init documentation](https://cloudinit.readthedocs.io/en/latest/){: external}.
    - For more information about cloud-init enabled images, see [Provisioning with a cloud-init enabled image](/docs/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#provisioning-with-a-cloud-init-enabled-image).
    - For more information about cloud-init data sources, see [Data sources](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html){: external}. {{site.data.keyword.cloud_notm}} cloud-init images are created for the environment by using the [Config drive](https://cloudinit.readthedocs.io/en/latest/reference/datasources/configdrive.html){: external} Version 2 data source to supply the metadata.
    - Linux images require Cloud-init version 0.7.7 or greater.

### Next steps
{: #next-steps-guest-addtions}

If you want to make sure that the guest additions are working from the hypervisor point of view, you can [create a case](/docs/get-support?topic=get-support-open-case) and ask for XenTools virtual server validation.

### Configuring a Windows custom image
{: #windows-custom-image}

Complete the following steps to make sure that your own Windows custom image can be successfully deployed in the {{site.data.keyword.cloud_notm}} infrastructure environment.

1. Begin with a single image file in VHD format. Disable its firewall so that it moves smoothly through provisioning.

2. Install the latest supported guest tools by using following steps.
    - Log in to the IBM Cloud VPN by using your {{site.data.keyword.cloud}} credentials and download the latest supported guest tools [here](http://downloads.service.networklayer.com/citrix/xen/citrix-vm-tools-9.1.5.zip){: external}.
    - Extract the compressed .zip file.
    - Go to the Windows Installer, for example, _managementagent64_, and double-click to open the **Citrix XenServer Windows Management Agent Setup** wizard. Complete the installation wizard to install XenServer Tools.

    Guest additions are important for reporting performance and health of virtual server instances. Without performance and health information, virtual server instances might be marked for offline migrations when a critical maintenance is needed.
    {: note}

    Don't disable or remove the default guest additions that are installed on {{site.data.keyword.BluSoftlayer_notm}} supplied images.
    {: important}

    If you want to make sure that the guest additions are working from the hypervisor point of view, you can [create a case](/docs/get-support?topic=get-support-open-case) and ask for XenTools virtual server validation.

3. Make sure that your image is cloud-init enabled.
    * For more information, see [cloudbase-init’s documentation](https://cloudbase-init.readthedocs.io/en/latest/index.html){: external}.
    * For more information about data sources, see [Data sources](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html){: external}. {{site.data.keyword.cloud_notm}} cloud-init images are created for the
environment by using the [Config Drive](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html){: external} Version 2 data source to supply the metadata.

4. Windows images require the Cloudbase-init Metadata Service for public and private network support in {{site.data.keyword.cloud_notm}} infrastructure. You can access the service [here](https://github.com/softlayer/bluemix-cloudbase-init){: external}.

## Preparing to import an encrypted image
{: #preparing-to-import-an-encrypted-image}

If you plan to import a VHD image that's encrypted with your own data encryption key, make sure that you complete the prerequisites and instructions for encryption that are described in [Using End to End (E2E) Encryption to provision an encrypted instance](/docs/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

You must use the vhd-util tool to encrypt your image, which must be in VHD format. For more information, see [Encrypting VHD images](/docs/image-templates?topic=image-templates-create-encrypted-image).
{: important}

## Uploading an image to {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

When your image is ready, you can upload it to {{site.data.keyword.cos_full_notm}}. Make sure to use a bucket in a [regional location](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints).

1. In {{site.data.keyword.cos_full_notm}}, go to your bucket and select **Upload > Files** to [upload](/docs/cloud-object-storage?topic=cloud-object-storage-upload) the image.
2. Use the [Aspera](/docs/cloud-object-storage?topic=cloud-object-storage-aspera) high-speed transfer plug-in for the fastest upload speeds of your image.

You can use COS SDK with Aspera to initiate high-speed transfer within your custom applications when you use Java, Python, or NodeJS. For more information, see [Using Libraries and SDKs](/docs/cloud-object-storage?topic=cloud-object-storage-aspera#aspera-sdk).
{: tip}


## Importing an Image from IBM Cloud Object Storage
{: #import-icos}

Complete the following steps to import an image from {{site.data.keyword.cos_full_notm}}.

1. Go to the device menu and make sure that you have the correct account permissions to complete the tasks.

   * Go to your console's device menu. For more information, see [Navigating to devices](/docs/image-templates?topic=virtual-servers-navigating-devices).
   * Make sure that you have any necessary account permissions and device access. Only the account owner, or a user with the **Manage Users** classic infrastructure permission, can adjust the permissions.

   For more information about permissions, see [Classic infrastructure permissions](/docs/account?topic=account-infrapermission#infrapermission) and [Managing device access](/docs/virtual-servers?topic=virtual-servers-managing-device-access).

   You might need to check your Windows firewall settings to ensure that you can import an image from {{site.data.keyword.cos_full_notm}}.
   {: tip}
2. Access the **Image Templates** page by selecting **Devices > Manage > Images**.
3. Click the **Import Custom Image** link to open the Import tool.
4. Complete the required fields (see Table 1).
5. When the import is complete from {{site.data.keyword.cos_full_notm}}, the image appears on the Image Templates page.

| Field | Value |
| ----- | ----- |
| Image Name | Specify a descriptive name for your new image. This image is the image that you use to provision virtual server instances. |
| IBM Cloud API Key | Specify the API key that gives access to your {{site.data.keyword.cos_full_notm}} service instance. This key is the object storage service _Service Credentials_ API key. When you import an encrypted image, the API Key must also have access to your key management service instance. The API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key. For more information, see [Working with API keys](/docs/account?topic=account-manapikey#manapikey). |
| Notes | Add any notes that are related to the image that might be helpful to users. |
|COS service instance | Select the {{site.data.keyword.cos_full_notm}} service instance where the image that you want to import is stored.|
| Location | Select the specific geographic region where your image is stored. You can import images into the following regions and associated data centers: US-South (DAL13), US-East (WDC07), EU-Great Britain (LON02), EU-Germany (FRA02), AP-Japan, and AU-Sydney. After the image is imported to one of the data centers that are listed, you can move it to another data center. |
| Bucket | Select the {{site.data.keyword.cos_full_notm}} bucket where your image is stored. Only buckets that exist in the regional location that you selected are valid. You receive an error message if you select a bucket that doesn't exist in the selected location.|
| Name | Select the image file in the {{site.data.keyword.cos_full_notm}} service instance that you want to import. Supported file types are VHD (Virtual Hard Disk), VMDK (Virtual Machine Disk), and ISO. If you are importing an encrypted image, the image must be in the VHD file format and encrypted with the vhd-util tool. |
| Operating System | Select the operating system that is included in your image. |
| Boot Mode | Select the boot mode for your image. If a default boot mode is set for the operating system that you specify, that boot mode is selected here automatically. |
| Your License | If you plan to provide your own operating system license, select this checkbox. If you are importing an image with a Windows operating system, you can select this option if you plan to use the image to provision [dedicated host instances](/docs/virtual-servers?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances). If your version of Windows operating system does not support the use your own license option, this option is disabled. For Windows images, you cannot select Cloud Init if you specify that you use your own license. If you are importing an encrypted image with Red Hat Enterprise Linux as your operating system, this option is selected by default and not editable because your encrypted image must include its own operating system license. |
| Cloud-init | If the image that you are importing is cloud-init enabled, select this checkbox. If you are importing an image that is on a cloud-init enabled Windows operating system and you select this option, you cannot concurrently select **Your License**. If you are importing an encrypted image, this option is selected by default and not editable because your encrypted image must be cloud-init enabled. |
| Encryption | If you are importing an image that you have encrypted with your own data encryption key by using the vhd-util tool, select this checkbox. |

{: caption="Table 1. Values for importing a custom image from IBM Cloud Object Storage" caption-side="top"}

The following table shows extra fields that are applicable only to importing encrypted images by using the  {{site.data.keyword.cloud_notm}} console. For more information about encrypted images, see [Using End to End (E2E) Encryption to provision an encrypted instance](/docs/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

<!--
To import an encrypted image, your account must have access to the End to End (E2E) Encryption feature. To enable your account for E2E Encryption, contact [Support](/docs/get-support?topic=get-support-using-avatar#getting-customer-support).
{: tip}
-->

| Field | Value |
| ----- | ----- |
| Wrapped Data Encryption Key | When you import an encrypted image, specify the ciphertext that is associated with the wrapped data encryption key (DEK) that is produced by wrapping the DEK you used to encrypt your image. For more information, see **Wrapping keys** using either the [{{site.data.keyword.keymanagementservicelong}} API](/docs/key-protect?topic=key-protect-cli-reference#kp-key-command) or the [{{site.data.keyword.cloud}} {{site.data.keyword.hscrypto}} API](/docs/hs-crypto?topic=hs-crypto-wrap-keys#wrap-keys-api). |
| Key Management Service Instance | Select a key management service instance in your account from the drop-down list (either the Key Protect service or the Hyper Protect Crypto service). The key management service instance must include the customer root key that you used to wrap your data encryption key. |
| Key Name | Select the name of the root key within the key management service instance that you used to wrap your data encryption key. For more information, see [Viewing keys in Key Protect](/docs/key-protect?topic=key-protect-view-keys#view-keys) or [Viewing keys in Hyper Protect Crypto](/docs/key-protect?topic=key-protect-view-keys#view-keys). |
{: caption="Table 2. Values for importing an encrypted image from IBM Cloud Object Storage" caption-side="top"}

## Next Steps
{: #ns-image-prep}

After the import begins, the system locates the image file in the {{site.data.keyword.cos_full_notm}} service instance in the specified bucket. The image file is imported as an image template that is then accessible on
the Image Templates page. After the import completes, the image can be used to order a new device or to start an existing device.
Additionally, you can delete an image template at any time. Image import times vary based on file size, but generally take several minutes to an hour.

The information that you provided to import encrypted image files from IBM Cloud Object Storage is stored along with the image file. Your information is then used to create an encrypted image template that you can use when you order a new device. You don't need to reenter this information when you order new devices from encrypted image templates.

After an image is imported into the image template repository, you can delete it from {{site.data.keyword.cos_full_notm}}. You can continue accessing the image template from your **Image Templates** page and by using it to provision virtual server instances.
