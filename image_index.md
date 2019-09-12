---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-09-12"

keywords: image templates

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:note: .note}
{:new_window: target="_blank"}

# Getting started with image templates
{: #getting-started-with-image-templates}

With {{site.data.keyword.cloud}} image templates, you can capture an image of a virtual server to quickly replicate its configuration with minimal changes in the order process.
{:shortdesc}

Image templates are not intended for use as a backup strategy.
{:note}


## Creating an image template
{: #creating-template}

Complete the following steps to create an image template of a virtual server.

1. Navigate to your console's device menu and ensure you have the correct account permissions to complete the tasks.
2. From the **Devices** menu, select **Device List**.
3. Click the virtual server that you want to use to create an image template.

  Check the **Passwords** tab of the **Device Details** page. Ensure that any passwords listed on the **Device Details** page match the actual operating system passwords and any other software add-on passwords. If passwords do not match, virtual servers that are created from this image template fail.
  {:tip}

4. From the **Actions** menu, select **Create Image Template** and complete the required fields.

For more information, see [**Creating an image template**](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).

## Next steps
{: #next-steps}

After the image template is created, you can [order more virtual servers](/docs/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template) based on the template.  Your new virtual servers have the same configurations that are included in the image template.
