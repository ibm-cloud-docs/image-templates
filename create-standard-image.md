---
copyright:
  years: 2017
lastupdated: "2017-09-18"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}


# Creating a standard image

With standard image templates, you can replicate various configuration options for {{site.data.keyword.virtualmachinesshort}}. 
{:shortdesc}

At any point during the life of a virtual server, you can create a standard image of a device and use it to quickly replicate portions of its 
configuration in another virtual server. Standard images can be taken of any virtual server, regardless of its operating system, but can be used only to create another virtual server. 

Complete the following steps to create a standard image of a virtual server.

1. Access the [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/) by using your unique credentials.
2. From the **Devices** menu, select **Device List**.
3. Click the virtual server that you want to use to create a standard image.
4. From the **Actions** menu, select **Create Image Template**.
5. Enter the new name for the standard image in the **Image Name** field.
6. Enter any necessary notes for the image in the **Note** field.
7. Select the **Agree** check box when all information is entered.
8. Click **Create Template** to create the Standard Image Template.

## Next steps

After the standard image template is created, more virtual servers can be created by using the template. The new 
virtual servers will have the same configurations that are included in the image template.

