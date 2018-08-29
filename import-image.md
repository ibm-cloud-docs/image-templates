---
copyright:
  years: 2014, 2018
lastupdated: "2018-08-29"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Preparing and importing images

The Image Templates screen in the {{site.data.keyword.slportal_full}} allows users to upload an existing image from an [Object Storage OpenStack Swift](/docs/infrastructure/objectstorage-swift/index.html) account. 
{:shortdesc}

After images are imported as an image template, they can be used to provision or start an existing virtual server. Images that are imported from an Object Storage account can be either VHDs or custom ISOs. VHD imports are restricted to the following 64-bit operating systems:

* CentOS 6 and 7
* RedHat Enterprise Linux 6 and 7
* Ubuntu 14.04, and 16.04
* Microsoft Server Standard 2012, R2 2012, and 2016

VHD imports are limited to 100 GB disks. VHDs must be named according to the following example: filename.vhd-0.vhd.

## Converting images to VHD

VHD format is the only supported image format for virtual servers. To convert images to VHD, use the following information:

* Qemu-img 2.7.0 or newer is required
* Convert the image with the following command: 
 
  ```
  qemu-img convert -f <image format> <image name> -O vpc -o force_size <image name>
  ```
  {: pre}
   
* Example command:
   
  ```
  qemu-img convert -f qcow2 test -O vpc -o force_size test
  ```
  {: pre}

For more information, see [Converting image formats ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) in the QEMU
documentation.

## ISO Templates

Only {{site.data.keyword.BluSoftlayer_notm}} Supported Operating Systems can be used to load an ISO Template onto a VSI. A list of 
Supported Operating Systems can be found here: [http://www.softlayer.com/services/software/ ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.softlayer.com/services/software/)

ISOs that are imported by using this tool must be bootable in order for the image to be eligible for import.

## Configuring an Image for {{site.data.keyword.virtualmachinesshort}}

To ensure that an image can be successfully deployed in the {{site.data.keyword.BluSoftlayer_notm}} infrastructure environment, virtual server images must be configured to the following specifications:

* ***/boot*** must be first partition
* ***/boot*** and ***/*** must be ext3 or ext4 file system
* ***/etc***  and /root must be on the same partition as ***/***
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** to mount the swap disk that is attached to the system
* wget must be installed
* Latest xe-guest-utilities Xen tools must be installed. Complete the following steps:
    
    1. Download the XenServer ISO from Citrix: [https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html)
    
    2. Mount the ISO by running the following command: 
    
        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}
    
    3. Locate the RPM for the ISO by running the following command:
    
        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}
    
    4. The output lists an RPM similar to: 
    
        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}
     
     5. You can copy the RPM and then extract the ISO for xentools. Run the following command to create a directory structure that houses the ISO:
    
        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}
    
     6. From the resulting directory structure, you can mount the *guest-tools* ISO and locate *rpm/debs* to install xentools. See the following example directory structure:
     
        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}
    
For more information about cloud-init enabled images, see [Provisioning with a cloud-init enabled image](image_cloud-init.html).

## Importing an Image

Complete the following steps to import an image in the {{site.data.keyword.slportal}}.

1. Locate and record the following details for the image from the {{site.data.keyword.objectstorageshort}} account.  For more information, see [Viewing and Editing Object Storage File Details](/docs/infrastructure/objectstorage-swift/interacting-in-portal.html#viewing-and-editing-file-details).
  * Account Name
  * Cluster
  * Container
  * Image Filename
2. In the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/), access the **Image Templates** page by selecting **Devices > Manage > Images**.
3. Click the **Import Image** tab to open the Import tool.
4. Select the **{{site.data.keyword.objectstorageshort}} Account** for the image that you want to import from the **Account** drop-down list.
5. Select the **{{site.data.keyword.objectstorageshort}} Cluster** for the image that you want to import from the **Cluster** drop-down list.
6. Select the **{{site.data.keyword.objectstorageshort}} Container** for the image that you want to import from the **Container** drop-down list.
7. Select the **Image Filename** as it is listed in {{site.data.keyword.objectstorageshort}} from the **Image File** drop-down list.
8. Enter the image name for the new image template in the **Image Name** field.
9. Enter any applicable notes in the **Notes** text box.
10. Select the image's operating system from the **Operating System** drop down list.

  The Operating System drop down list is disabled if the image for import is a custom ISO. This step is only required when the import     involves a VHD.
  {:tip}

11. If the image that you are importing is cloud-init enabled, select the **Cloud Init** check box. For more information, see [Provisioning with a cloud-init enabled image](image_cloud-init.html).        
12. If you plan to provide your own operating system license, select the **Your License** check box. For more information, see [Using your own OS license or subscription](use-red-hat-cloud-access.html).
13. Click **Import** to import the image to the Image Templates screen. Click **Cancel** to cancel the action.

## Importing an Image from IBM Cloud Object Storage 
{: #import-icos}

Complete the following steps to import an encrypted image from {{site.data.keyword.cos_full_notm}}. 

This task is currently part of the End to End (E2E) Encryption feature. To gain access to this feature, please contact Support.
{: tip}

To limit access to only the information that is needed to complete the import task, authenticate to {{site.data.keyword.slportal}} with a service ID. The service ID should have access only to the encrypted image in IBM Cloud Object Storage that you want to import and the Key Protect instance where your root key is stored.

1. In the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/), access the **Image Templates** page by selecting **Devices > Manage > Images**.
2. Click the **Import Image from IBM COS** tab to open the Import tool.
3. Complete the required fields (see Table 1). You must provide the {{site.data.keyword.keymanagementserviceshort}} Instance ID, the wrapped data encryption key (WDEK), and the root key ID. The {{site.data.keyword.keymanagementserviceshort}} information is needed to access the image when it is provisioned. 
4. When the import is complete from {{site.data.keyword.cos_full_notm}}, the encrypted image appears on the Image Templates page. 

Currently it is not supported to export an encrypted image template to object storage. Additionally, a virtual server instance that is provisioned from an encrypted image template does not support the following actions: Create image template, Migrate instance, and Migrate to SAN. 
{: tip}

| Field | Value |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Select the {{site.data.keyword.cos_full_notm}} account for the image that you want to import. |
| Location | Select the specific geographic region where your image is stored. | 
| Bucket | Select the {{site.data.keyword.cos_full_notm}} bucket where your image is stored. |
| Image File | Select the encrypted image file in the {{site.data.keyword.cos_full_notm}} that you want to import. The image must be in the RAW file format and encrypted with LUKS disk encryption. |
| Image Name | Specify a descriptive name for your encrypted image. This is the image that you will use to provision virtual server instances. |
| Encryption | This check box is selected by default and not editable. Encrypted images are currently the only type of image that can be imported from {{site.data.keyword.cos_full_notm}}. |
| Operating System | Select the operating system that is included in your encrypted image. |
| Cloud-init | This option is selected by default and not editable because your encrypted image must be cloud-init enabled. |
| Your License | This option is selected by default and not editable because your encrypted image must include its own operating system license. |
| Boot Mode | Select the boot mode for your image. |
| Notes | Add any notes related to the image that might be helpful to users. |
| {{site.data.keyword.keymanagementserviceshort}} Service Instance ID | You can use the {{site.data.keyword.cloud_notm}} CLI to find your {{site.data.keyword.keymanagementserviceshort}} instance ID. For more information, see [Retrieving your instance ID](/docs/services/keymgmt/keyprotect_authentication.html#retrieve_instance_ID). |
| Wrapped Data Encryption Key | Specify the cipher text that is associated with the data encryption key that you used to encrypt your image. For more information, see [Wrapping keys by using the API](/docs/services/keymgmt/keyprotect_wrap_keys.html). |
| Root Key ID | Specify the ID of the root key that was used to wrap the data encryption key. For more information, see [Viewing keys](/docs/services/keymgmt/keyprotect_view_keys.html). |
| API Key | Specify the API key that you noted when you created it. The API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key. For more information, see [Managing your API keys](/docs/iam/userid_keys.html). |
{: caption="Table 1. Values for importing an image from IBM Cloud Object Storage" caption-side="top"}


## Next Steps

After the import begins, the system locates the image file in the {{site.data.keyword.objectstorageshort}} account by using the 
specified path (Account > Cluster > Container > Image File). The image file is imported as an image template that is then accessible on 
the Image Templates page. After the import completes, the image can be used to order a new device or to start an existing device. 
Additionally, the image can be deleted at any time. Image import times vary based on file size, but generally take several minutes to an hour.

