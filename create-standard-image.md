---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-26"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}


# Creating an image template
{: #creating-an-image-template}

With image templates, you can replicate various configuration options for {{site.data.keyword.virtualmachinesshort}}.
{:shortdesc}

At any point during the life of a virtual server, you can create an image template. Then, you can use it to quickly replicate portions of its configuration in another virtual server. You can create image templates from any virtual server, regardless of its operating system. When your image template is complete, you can use it to create another virtual server.

## Before you begin
First, navigate to the device menu and ensure you have the correct account permissions to complete the tasks.

* Navigate to your console's device menu. For more information, see [Navigating to devices](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Ensure you have any necessary account permissions and device access. Only the account owner, or a user with the **Manage Users** classic infrastructure permission, can adjust the permissions.

For more information about permissions, see [Classic infrastructure permissions](/docs/iam?topic=iam-infrapermission#infrapermission) and [Managing device access](/docs/vsi?topic=virtual-servers-managing-device-access).

## Creating an image template
{: #creating-an-image-template_steps}

Complete the following steps to create an image template of a virtual server.

1. From the **Devices** menu, select **Device List**.
2. Click the virtual server that you want to use to create an image template.

  Check the **Passwords** tab of the **Device Details** page. Ensure that any passwords listed on the **Device Details** page match the actual operating system passwords and any other software add-on passwords. If passwords do not match, virtual servers that are created from this image template fail.
  {:tip}

3. From the **Actions** menu, select **Create Image Template**.
4. Enter the new name for the image in the **Image Name** field.
5. Enter any necessary notes for the image in the **Note** field.
6. Select the **Agree** check box when all information is entered.
7. Click **Create Template** to create the image template.

## Next steps

After the image template is created, more virtual servers can be created by using the template. The new
virtual servers have the same configurations that are included in the image template.
