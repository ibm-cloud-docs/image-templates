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
{:tip: .tip}

# Exporting an image to IBM Cloud Object Storage
{: #exporting-an-image-to-ibm-cloud-object-storage}

From the Image Templates page you can export an image template to an [{{site.data.keyword.cos_full}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage) account.
{:shortdesc}

The image export process takes a preexisting, private standard image template or an encrypted image template and coverts the image into an image file that is stored in a specified location on an {{site.data.keyword.cos_full_notm}} account.

*Note:* If you imported a VMDK image, you can export that image in VHD or VMDK format. Because of the differences between the image formats, there is a chance of data loss. To protect your data in the case of data loss, the original VHD file is retained.

## Before you begin
First, navigate to the device menu and ensure you have the correct account permissions to complete the tasks.

* Navigate to your console's device menu. For more information, see [Navigating to devices](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Ensure you have write access to {{site.data.keyword.cos_full_notm}}. Only the account owner, or a user with the **Manage Users** classic infrastructure permission, can adjust the permissions.

For more information about permissions, see [Classic infrastructure permissions](/docs/iam?topic=iam-infrapermission#infrapermission) and [Managing device access](/docs/vsi?topic=virtual-servers-managing-device-access).

If you plan to export this image template to {{site.data.keyword.cos_full_notm}}, make sure its name does not contain any characters that might be problematic in a web address. For example, ?, =, <, and other special characters might cause unwanted behavior if not URL-encoded.
{: tip}

## Exporting an image to IBM Cloud Object Storage

Use the following steps to export an image template to IBM Cloud Object Storage.

1. From the **Devices** menu, select **Manage > Images**.
2. Click **Actions** for the image template that you want to export and select **Export Image to IBM COS**. If an image template with the configuration that you want is not yet
available, see [Creating an Image Template](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template).
3. Complete the required fields (see table 1).
4. Click **OK** to export the image to the specified location in the {{site.data.keyword.cos_full_notm}} Account.

| Field | Value |
| ----- | ----- |
| File Name | Enter the file name for the image. |
| {{site.data.keyword.cos_full_notm}} | Select an {{site.data.keyword.cos_full_notm}} account. |
| Location | Select the specific geographic region where you want the image template stored. |
| Bucket | Select the {{site.data.keyword.cos_full_notm}} bucket where you want the image template stored. Only buckets that exist in the regional location that you selected are valid. You will receive an error message if you specify a bucket that doesn't exist in the selected location. |
| API Key | Specify the service ID API key that has write access to {{site.data.keyword.cos_full_notm}}. For more information, see [Managing service ID API keys](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys). |
{: caption="Table 1. Values for exporting an image to {{site.data.keyword.cos_full_notm}}" caption-side="top"}
| Target file type | Select the file type you want exported. If you imported a VMDK image, you have the option to export that image in VHD or VMDK format. If the file was originally in VHD format, you can only export as VHD. |

## Next Steps
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
Because each image is a different size and has different characteristics, the export process might take several minutes before it completes.

After you export an image, the image remains in the list of image templates, but it is also available as a file in the {{site.data.keyword.cos_full_notm}} location that is specified during the export process. You can download the image file from {{site.data.keyword.cos_full_notm}}. From the Resource List in {{site.data.keyword.cloud_notm}} console, access your Cloud Object Storage service instance. In the bucket where your image is stored, select the image files that you want to download and select **Download objects**.

When your image template is exported to IBM Cloud Object Storage, each block device (or disk) has its own associated file. For example, if your image file is named image.vhd, the first block device is exported as image-0.vhd. The second block device is exported as image-1.vhd, and so on.
{: tip}
