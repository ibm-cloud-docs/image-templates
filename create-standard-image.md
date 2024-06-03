---

copyright:
  years: 2014, 2024
lastupdated: "2024-06-03"

keywords:

subcollection: image-templates

---

{{site.data.keyword.attribute-definition-list}}

# Creating an image template
{: #creating-an-image-template}

With image templates, you can replicate various configuration options for {{site.data.keyword.virtualmachinesshort}}.
{: shortdesc}

At any point during the life of a virtual server, you can create an image template. Then, you can use it to quickly replicate portions of its configuration in another virtual server. You can create image templates from any virtual server, regardless of its operating system. When your image template is complete, you can use it to create another virtual server.

{{site.data.content.iso-operating-systems}}

## Before you begin
{: #byb-create_template}

First, go to the device menu and make sure that you have the correct account permissions to complete the tasks.

* Make sure that you have any necessary account permissions and device access. Only the account owner, or a user with the **Manage users** Classic infrastructure permission, can adjust the permissions. For more information about permissions, see [Classic infrastructure permissions](/docs/account?topic=account-infrapermission#infrapermission) and [Managing device access](/docs/virtual-servers?topic=virtual-servers-managing-device-access).

You can't create an image template if {{site.data.keyword.cloud}}-provided third-party software is installed. You need to remove the third-party software applications before you create an image template.
{: note}

## Creating an image template
{: #creating-an-image-template_steps}

Complete the following steps to create an image template of a virtual server.

1. From the **Devices** menu, select **Device List**.
2. Click the virtual server that you want to use to create an image template.

    Check the **Passwords** tab of the **Device Details** page. Make sure that any passwords that are listed on the **Device Details** page match the actual operating system passwords and any other software add-on passwords. If passwords don't match, virtual servers that are created from this image template fail.
    {: tip}

3. From the **Actions** menu, select **Create Image Template**.
4. Enter the new name for the image in the **Image Name** field.
5. Enter any necessary notes for the image in the **Note** field.
6. Select the **Agree** checkbox when all information is entered.
7. Click **Create Template** to create the image template.

Your server is offline until the imaging process completes. The image template processing time varies based on the resources that are available on the physical host and how much data is captured in the image template. 
{: note}

## Next steps
{: #ns-create_template}

After the image template is created, you can create more virtual servers by using the template. The new
virtual servers have the same configurations that are included in the image template.
