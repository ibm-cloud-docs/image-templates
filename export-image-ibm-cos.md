---
copyright:
  years: 2014, 2018
lastupdated: "2018-10-02"
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# Exporting an image to IBM Cloud Object Storage
{: #exporting-to-ibm-cos}

From the Image Templates page you can export an image template to an [IBM Cloud Object Storage](/docs/services/cloud-object-storage/about-cos.html) account. 
{:shortdesc}

The image export process takes a preexisting, private standard image template or an encrypted image template and converts the image into an 
image file that is stored in a specified location on an IBM Cloud Object Storage account. 

Use the following steps to export an image template to IBM Cloud Object Storage.

1. Authenticate to {{site.data.keyword.slportal}} with a service ID that has write access to IBM Cloud Object Storage.
2. From the **Devices** menu in the [{{site.data.keyword.slportal_full}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/), select **Manage > Images**.
3. Click **Actions** for the image template that you want to export and select **Export Image to IBM COS**. If an image template with the configuration that you want is not yet 
available, see [Creating an Image Template](create-standard-image.html).
4. Complete the required fields (see table 1). 
5. Click **OK** to export the image to the specified location in the IBM Cloud Object Storage Account. 

| Field | Value |
| ----- | ----- |
| File Name | Enter the file name for the image. |
| {{site.data.keyword.cos_full_notm}} | Select an {{site.data.keyword.cos_full_notm}} account. |
| Location | Select the specific geographic region where you want the image template stored. | 
| Bucket | Select the {{site.data.keyword.cos_full_notm}} bucket where you want the image template stored. Only buckets that exist in the regional location that you selected are valid. You will receive an error message if you specify a bucket that doesn't exist in the selected location. |
| API Key | Specify the service ID API key that has write access to {{site.data.keyword.cos_full_notm}}. For more information, see [Managing service ID API keys](/docs/iam/serviceid_keys.html). |
{: caption="Table 1. Values for exporting an image to IBM Cloud Object Storage" caption-side="top"}

## Next Steps
Because each image is a different size and has different characteristics, the export process might take several minutes before it completes. 

After you export an image, the image remains in the list of image templates, but it is also available as a file in the IBM Cloud Object Storage location that is specified during the export process. 

When your image template is exported to IBM Cloud Object Storage, each block device (or disk) has its own associated file. For example, if your image file is named image.vhd, the first block device is exported as image-0.vhd. The second block device is exported as image-1.vhd, and so on. 
{: tip}

