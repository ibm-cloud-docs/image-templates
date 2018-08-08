---
copyright:
  years: 2014, 2018
lastupdated: "2018-08-08"
---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}


# Creating an image template

With image templates, you can replicate various configuration options for {{site.data.keyword.virtualmachinesshort}}. 
{:shortdesc}

At any point during the life of a virtual server, you can create an image template of a device and use it to quickly replicate portions of its configuration in another virtual server. Image templates can be taken of any virtual server, regardless of its operating system, but can be used only to create another virtual server. 

Complete the following steps to create an image template of a virtual server.

1. Access the [{{site.data.keyword.slportal_full}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){: new_window} by using your unique credentials.
2. From the **Devices** menu, select **Device List**.
3. Click the virtual server that you want to use to create an image template.

  Check the **Passwords** tab of the **Device Details** page. Ensure that any passwords listed on this page match the actual operating system passwords and any other software add-on passwords. If passwords do not match, virtual servers that are created from this image template will fail.
  {:tip}
  
4. From the **Actions** menu, select **Create Image Template**.
5. Enter the new name for the image in the **Image Name** field.
6. Enter any necessary notes for the image in the **Note** field.
7. Select the **Agree** check box when all information is entered.
8. Click **Create Template** to create the image template.

## Next steps

After the image template is created, more virtual servers can be created by using the template. The new 
virtual servers will have the same configurations that are included in the image template.

