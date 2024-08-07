---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-29"

keywords: image templates

subcollection: image-templates

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with image templates
{: #getting-started-with-image-templates}

With {{site.data.keyword.cloud}} Classic infrastructure image templates, you can capture an image of a virtual server to quickly replicate its configuration with minimal changes in the order process. Image templates provide an imaging option for all {{site.data.keyword.virtualmachinesshort}}, regardless of operating system. Image templates are not compatible with bare metal servers.
{: shortdesc}

Image templates are not intended for backing up your data.
{: note}

## How image templates work
{: #how-image-templates-work}

The two main actions for any image template are create and deploy. When you request to create an image, the automated system of {{site.data.keyword.cloud}} takes your server offline. While the server is offline, a compressed backup of your data is created, the configuration information is recorded, and the image template is stored on the {{site.data.keyword.cloud_notm}} SAN. During the deployment stage of the image template, the automated system constructs a new server that is based on the data that is gathered from the selected image. The deployment process makes adjustments for volume, restores the copied data, and then makes final configuration changes (for example, network configurations) for the new host.

## Private images
{: #private-images}

Private images are images that are created by users on their own accounts or images that are created on another account and shared. By default, all the images that you create are private.

## Public images
{: #public-images}

Public images are pre-configured virtual servers that are posted by {{site.data.keyword.cloud_notm}} or made public by customers and are available for use by all {{site.data.keyword.cloud_notm}} customers. Virtual servers that are provisioned through public image templates are generally configured for optimal performance, with specific combinations of different configuration specs.

{{site.data.content.iso-operating-systems}}

## Creating an image template
{: #creating-template}

Complete the following steps to create an image template of a virtual server.

Image template automation doesn't support software add-ons. You need to remove the add-on software to create an image template.
{: important}

1. Go to the console device menu and make sure that you have the correct account permissions to complete the tasks.
2. From the **Devices** menu, select **Device List**.
3. Click the virtual server that you want to use to create an image template.
4. Check the **Passwords** tab of the **Device Details** page. Make sure that any passwords that are listed on the **Device Details** page match the actual operating system passwords and any other software add-on passwords. If passwords do not match, virtual servers that are created from this image template fail.
5. From the **Actions** menu, select **Create Image Template** and complete the fields.

For more information about creating image templates, see [**Creating an image template**](/docs/image-templates?topic=image-templates-creating-an-image-template).

## Next steps
{: #next-steps}

After the image template is created, you can [order more virtual servers](/docs/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template) based on the template. Your new virtual servers have the same configurations that are included in the image template.
