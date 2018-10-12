---
copyright:
  years: 2014, 2018
lastupdated: "2018-10-12"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Preparing and importing images
{: #preparing-and-importing-images}

The Image Templates screen in the {{site.data.keyword.slportal_full}} allows users to import an image from an [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/about-cos.html) service instance. After an image is uploaded to a bucket in an {{site.data.keyword.cos_full_notm}} service instance, you can import it to the image templates repository in the {{site.data.keyword.slportal_notm}}.
{:shortdesc}

You must have an upgraded account to import images from {{site.data.keyword.cos_full_notm}. For more informaton, see [Switching to IBMid and linking accounts](/docs/account/softlayerlink.html).
{: tip}

After images are imported as an image template, they can be used to provision or start an existing virtual server. Images that are imported from an {{site.data.keyword.cos_full_notm}} service instance can be either VHDs or custom ISOs. VHD imports are restricted to the following 64-bit operating systems:

* CentOS 6 and 7
* RedHat Enterprise Linux 6 and 7
* Ubuntu 14.04, and 16.04
* Microsoft Server Standard 2012, R2 2012, and 2016

VHD imports are limited to 100 GB disks. VHDs must be named according to the following example: filename.vhd-0.vhd.

## Converting images to VHD
{: #convert-to-vhd}

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
{: #iso-templates}

Only {{site.data.keyword.BluSoftlayer_notm}} Supported Operating Systems can be used to load an ISO Template onto a VSI. A list of 
Supported Operating Systems can be found here: [https://www.ibm.com/cloud/server-software ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/server-software)

ISOs that are imported by using this tool must be bootable in order for the image to be eligible for import.

## Configuring an Image for {{site.data.keyword.virtualmachinesshort}}
{: #config-image-vsi}

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

## Uploading an image to {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

When your image is ready, you can upload it to {{site.data.keyword.cos_full_notm}}. Make sure to use a bucket with a regional location that also supports importing images. (You can import images to the following regions: US-South, US-East, EU-Great Britain, and EU-Germany.) 

1. In {{site.data.keyword.cos_full_notm}}, navigate to your bucket and click **Add Objects** to [upload](/docs/services/cloud-object-storage/basics/upload.html#uploading-data) the image. 
2. Use the [Aspera](/docs/services/cloud-object-storage/basics/aspera.html#Aspera-high-speed-transfer) high-speed transfer plug-in for the fastest upload speeds of your image.

You can use COS SDK with Aspera to initiate high-speed transfer within your custom applications when using Java, Python, or NodeJS. For more information, see [Using Libraries and SDKs](/docs/services/cloud-object-storage/basics/aspera.html#sdk).
{: tip}


## Importing an Image from IBM Cloud Object Storage 
{: #import-icos}

Complete the following steps to import an image from {{site.data.keyword.cos_full_notm}}. 

1. In the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/), access the **Image Templates** page by selecting **Devices > Manage > Images**.
2. Click the **Import Image from IBM COS** tab to open the Import tool.
3. Complete the required fields (see Table 1). 
4. When the import is complete from {{site.data.keyword.cos_full_notm}}, the image appears on the Image Templates page. 

| Field | Value |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Select the {{site.data.keyword.cos_full_notm}} service instance where the image that you want to import is stored. |
| Location | Select the specific geographic region where your image is stored. You can import images into the following regions and associated data centers: US-South (DAL13), US-East (WDC07), EU-Great Britain (LON02), EU-Germany (FRA02). After the image is imported to one of the data centers that are listed, you can move it to another data center. | 
| Bucket | Select the {{site.data.keyword.cos_full_notm}} bucket where your image is stored. Only buckets that exist in the regional  location that you selected are valid. You will receive an error message if you select a bucket that doesn't exist in the selected location.|
| Image File | Select the image file in the {{site.data.keyword.cos_full_notm}} service instance that you want to import. Supported file types are VHD, ISO, and RAW. If you are importing an encrypted image, the image must be in the RAW file format and encrypted with LUKS disk encryption. |
| Image Name | Specify a descriptive name for your image. This is the image that you will use to provision virtual server instances. |
| API Key | Specify the API key that gives access to your {{site.data.keyword.cos_full_notm}} service instance. When importing an encrypted image, the API Key must also have access to Key Protect. The API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key. For more information, see [Working with API keys](/docs/iam/apikeys.html). |
| Operating System | Select the operating system that is included in your image. For encrypted images, only Linux operating systems are valid selections. |
| Cloud-init | If the image that you are importing is cloud-init enabled, select this check box. If you are importing an image that has a cloud-init enabled Windows operating system and you select this option, you cannot also specify **Your License**. If you are importing an encrypted image, this option is selected by default and not editable because your encrypted image must be cloud-init enabled. |
| Your License | If you plan to provide your own operating system license, select this check box. If you are importing an image with a Windows operating system, you can select this option if you plan to use the image to provision [dedicated host instances](/docs/vsi/vsi_dedicated_host.html#dedicated-hosts-and-dedicated-instances). If your version of Windows operating system does not support using your own license, this option is disabled. For Windows images, you cannot select Cloud Init if you specify that you will use your own license. If you are importing an encrypted image with Red Hat Enterprise Linux as your operating system, this option is selected by default and not editable because your encrypted image must include its own operating system license. |
| Boot Mode | Select the boot mode for your image. If a default boot mode is set for the operating system that you specify, that boot mode is selected here automatically. |
| Notes | Add any notes related to the image that might be helpful to users. |
| Encryption | The selection for this check box is determined by the file type of the image that you select to import. VHD and ISO images indicate that the image file is not encrypted. Thus, the check box is not selected for VHD and ISO images. A RAW image file indicates that the image is an encrypted image. If a RAW image file is specified, this check box is selected by default and not editable. |
{: caption="Table 1. Values for importing an image from IBM Cloud Object Storage" caption-side="top"}

The following table shows additional fields that are applicable to importing encrypted images only. For more information about encrypted images, see [Using End to End (E2E) Encryption to provision an encrypted instance](provision-encrypted-image.html).

To import an encrypted image, your account must have access to the End to End (E2E) Encryption feature. To enable your account for E2E Encryption, please contact Support. 
{: tip}

| Field | Value |
| ----- | ----- |
| {{site.data.keyword.keymanagementserviceshort}} Service Instance ID | When importing an encrypted image, your {{site.data.keyword.keymanagementserviceshort}} service instance must be in the same region as your {{site.data.keyword.cos_full_notm}} bucket. You can use the {{site.data.keyword.cloud_notm}} CLI to find your {{site.data.keyword.keymanagementserviceshort}} instance ID. For more information, see [Retrieving your instance ID](/docs/services/key-protect/access-api.html#retrieve-instance-ID). |
| Wrapped Data Encryption Key | When importing an encrypted image, specify the cipher text that is associated with the data encryption key that you used to encrypt your image. For more information, see [Wrapping keys by using the API](/docs/services/key-protect/wrap-keys.html#api). |
| Root Key ID | When importing an encrypted image, specify the ID of the root key that was used to wrap the data encryption key. For more information, see [Viewing keys](/docs/services/key-protect/view-keys.html#view-keys). |
{: caption="Table 2. Values for importing an encrypted image from IBM Cloud Object Storage" caption-side="top"}

## Next Steps

After the import begins, the system locates the image file in the {{site.data.keyword.cos_full_notm}} service instance in the specified bucket. The image file is imported as an image template that is then accessible on 
the Image Templates page. After the import completes, the image can be used to order a new device or to start an existing device. 
Additionally, the image template can be deleted at any time. Image import times vary based on file size, but generally take several minutes to an hour.

After an image is imported into the image template repository, you can delete it from {{site.data.keyword.cos_full_notm}}. You can continue   accessing the image template from the **Image Templates** page and using it to provision virtual server instances. 

