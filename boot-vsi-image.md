---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}

# Booting a VSI from an image
{: #boot-vsi-image}

The Boot from Image feature starts a virtual server instance (VSI) by using an ISO template that is
imported from an Object Storage Account.
{:shortdesc}

Booting a virtual server from an image brings the device online safely so that issues can be resolved. In most cases, the boot from image feature allows for troubleshooting to occur in an environment without risking significant data loss that might be experienced from an OS reload. Though significant data loss is less likely than with an OS reload, it is recommended that you back up the device before initiating the boot.

Use the following steps to start a virtual server from an image.

1. Back up all data on the device.
2. From the **Devices** menu in the [{{site.data.keyword.slportal_full}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/), select **Device List**.
3. From the Device List, click the virtual server that you want to start from an ISO template.
4. On the Device Details page for the virtual server, select **Actions > Boot from Image**.
5. Click **Boot from this Image** for the desired image

  Toggle between public and private images by using the drop-down feature above the image list.
  {:tip}

6. Click **Boot from Image** to boot the device by using the selected image. Click **Close** to cancel the action.

## Next Steps

After you initiate the boot process, the image is powered off and then started by using the selected image. Boot time varies, as the size and type of
each image varies. After the virtual server is started by using the selected image, it can be accessed as a regularly booted device, but is configured according to the image. Restart the virtual server to power down and return the device to its original state.
