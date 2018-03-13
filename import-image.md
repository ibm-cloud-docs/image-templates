---
copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Preparing and importing images

The Image Templates screen in the {{site.data.keyword.slportal_full}} allows users to upload an existing image from a Swift-based [Object Storage](/docs/infrastructure/objectstorage-swift/index.html) account. 
{:shortdesc}

After images are imported as an image template, they can be used to provision or start an existing virtual server. Images that are imported from an Object Storage account can be either VHDs or custom ISOs. VHD imports are restricted to the following 64-bit operating systems:

* CentOS 6 and 7
* RedHat Enterprise Linux 6 and 7
* Ubuntu 14.04, and 16.04
* Microsoft Server Standard 2012, R2 2012, and 2016

VHD imports are limited to 100 GB disks. VHDs must be named according to the following example: filename.vhd-0.vhd.

## Converting images to VHD

VHD format is the only support image format for {{site.data.keyword.BluVirtServers_full}}. To convert images to VHD, use the following information:

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

1. Locate and record the following details for the image from the {{site.data.keyword.objectstorageshort}} account.  For more information, see [Viewing and Editing Object Storage File Details](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html).
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
12. If you plan to provide your own operating system license, select the **Your License** check box. For more information, see [Using Red Hat Cloud Access](use-red-hat-cloud-access.html).
13. Click **Import** to import the image to the Image Templates screen. Click **Cancel** to cancel the action.

## Next Steps

After the import begins, the system locates the image file in the {{site.data.keyword.objectstorageshort}} account by using the 
specified path (Account > Cluster > Container > Image File). The image file is imported as an image template that is then accessible on 
the Image Templates page. After the import completes, the image can be used to order a new device or to start an existing device. 
Additionally, the image can be deleted at any time. Image import times vary based on file size, but generally take several minutes to an hour.

