---
copyright:
  years: 2017
lastupdated: "2017-09-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}


# Using Red Hat Cloud Access 

When you create an image template with a VHD image, you can select to provide your own operating system license through the [Red Hat Cloud 
Access ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) subscription.
{:shortdesc}

If you deploy an image in {{site.data.keyword.BluSoftlayer_full}} that indicates you are using your own license through Red Hat Cloud Access, the following support terms exist:
* IBM provides support for hypervisors, provisioning instances, importing images, rebooting an image, reloading an OS, and capturing an image.
* Red Hat provides support for the image itself. IBM does not provide support for the image.

For more information about our certification as a Red Hat Enterprise Linux cloud provider, see [Infrastructure as a Service (Iaas) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://access.redhat.com/ecosystem/cloud-provider/2262101).

## Importing an image that designates your own license

You can import a VHD image and specify that you will provide your own license or subscription for the operating system.

To access the Import Image page of Image Templates and mark a VHD image to use your own RHEL operating system license, complete the following steps:
1. From the **Devices** menu select **Manage > Images**.
2. Click the **Import Image** tab.
3. Complete the required information for importing your VHD image, and select the **Your License** checkbox that’s shown near the **Operating System** 
dropdown box. For more information about importing images, see [Preparing and importing images](import-image.html).

## Updating an image template to specify a user provided OS license

If you have an existing VHD image template, you can specify that you want to provide your own license or subscription for the operating system.

To access an image template and designate that it use your own existing RHEL license, complete the following steps:
1. From the **Devices** menu select **Manage > Images**.
2. From the list of templates, click the image template name that you want to update.
3. On the Image Template Details page, select the **User Provided** checkbox under the **OS License** heading, and click **Update**.

