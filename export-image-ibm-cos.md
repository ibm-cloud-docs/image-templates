---

copyright:
  years: 2014, 2021
lastupdated: "2021-05-20"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:important: .important}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# Exporting an image to IBM Cloud Object Storage
{: #exporting-an-image-to-ibm-cloud-object-storage}

From the Image Templates page, you can export an image template to an [{{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage) account.
{:shortdesc}

The image export process takes a preexisting, private standard image template, or an encrypted image template and coverts the image into an image file that is stored in a specified location on an {{site.data.keyword.cos_full_notm}} account.

If you imported a VMDK image, you can export that image in VHD or VMDK format. Because of the differences between the image formats, a chance of data loss exists. To protect your data if data loss occurs, the original VHD file is retained.
{: note}

## Before you begin
{: #byb-export-icos}

First, go to the device menu and make sure that you have the correct account permissions to complete the tasks.

* Go to your console's device menu. For more information, see [Navigating to devices](/docs/image-templates?topic=virtual-servers-navigating-devices).
* Make sure that you have write access to {{site.data.keyword.cos_full_notm}}. Only the account owner, or a user with the **Manage Users** classic infrastructure permission, can adjust the permissions.

For more information about permissions, see [Classic infrastructure permissions](/docs/account?topic=account-infrapermission#infrapermission) and [Managing device access](/docs/virtual-servers?topic=virtual-servers-managing-device-access).

If you plan to export this image template to {{site.data.keyword.cos_full_notm}}, make sure the image template name does not contain any characters that can be problematic in a web address. For example, ?, =, <, and other special characters might cause unwanted behavior if not URL-encoded.
{: tip}

## Exporting an image to IBM Cloud Object Storage
{: #exporting-an-image-to-ibm-cloud-object-storage_steps}

Use the following steps to export an image template to IBM Cloud Object Storage.

1. From the **Devices** menu, select **Manage > Images**.
2. Click **Actions** for the image template that you want to export and select **Export Image to IBM COS**. If an image template with the configuration that you want is not yet
available, see [Creating an Image Template](/docs/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template).
3. Complete the required fields (see table 1).
4. Click **OK** to export the image to the specified location in the {{site.data.keyword.cos_full_notm}} Account.

| Field | Value |
| ----- | ----- |
| File Name | Enter the file name for the image. |
| {{site.data.keyword.cos_full_notm}} | Select an {{site.data.keyword.cos_full_notm}} account. |
| Location | Select the specific geographic region where you want the image template stored. |
| Bucket | Select the {{site.data.keyword.cos_full_notm}} bucket where you want the image template stored. Only buckets that exist in the regional location that you selected are valid. An error message is displayed if you specify a bucket that doesn't exist in the selected location. |
| API Key | Specify the service ID API key that has write access to {{site.data.keyword.cos_full_notm}}. For more information, see [Managing service ID API keys](/docs/account?topic=account-serviceidapikeys#serviceidapikeys). |
{: caption="Table 1. Values for exporting an image to {{site.data.keyword.cos_full_notm}}" caption-side="top"}
| Target file type | Select the file type that you want to export. If you imported a VMDK image, you can export that image in VHD or VMDK format. If the file was originally in VHD format, you can export only as VHD. |

## Next Steps
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
Because each image is a different size and has different characteristics, the export process might take several minutes before it completes.

After you export an image, the image remains in the list of image templates, but it is also available as a file in the {{site.data.keyword.cos_full_notm}} location that is specified during the export process. You can download the image file from {{site.data.keyword.cos_full_notm}}. From the Resource List in {{site.data.keyword.cloud_notm}} console, access your Cloud Object Storage service instance. In the bucket where your image is stored, select the image files that you want to download and select **Download objects**.

When your image template is exported to IBM Cloud Object Storage, each block device (or disk) has its own associated file. For example, if your image file is named image.vhd, the first block device is exported as image-0.vhd. The second block device is exported as image-1.vhd, and so on.
{: tip}
