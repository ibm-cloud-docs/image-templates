---

copyright:
  years: 2014, 2021
lastupdated: "2021-05-20"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}

# Booting a virtual server from an image
{: #booting-a-vsi-from-an-image}

The Boot from Image feature starts a virtual server instance by using an ISO template that is
imported from an Object Storage Account.
{:shortdesc}

Booting a virtual server from an image brings the device online safely so that issues can be resolved. In most cases, the boot from image feature that allows for troubleshooting to occur in an environment without risking significant data loss that might be experienced from an OS reload. Though significant data loss is less likely than with an OS reload, it is recommended that you back up the device before you start the boot from image process.

## Before you begin
{: #byb-boot-vsi}

First, go to the device menu and make sure that you have the correct account permissions to complete the tasks.

* Go to your console's device menu. For more information, see [Navigating to devices](/docs/image-templates?topic=virtual-servers-navigating-devices).
* Make sure that you have any necessary account permissions and device access. Only the account owner, or a user with the **Manage Users** classic infrastructure permission, can adjust the permissions.

For more information about permissions, see [Classic infrastructure permissions](/docs/account?topic=account-infrapermission#infrapermission) and [Managing device access](/docs/virtual-servers?topic=virtual-servers-managing-device-access).

## Booting a virtual server instance from an image
{: #booting-a-vsi-from-an-image_steps}

Use the following steps to start a virtual server from an image.

1. Back up all data on the device.
2. From the **Devices** menu, select **Device List**.
3. From the Device List, click the virtual server that you want to start from an ISO template.
4. On the Device Details page for the virtual server, select **Actions > Boot from Image**.
5. Click **Boot from this Image** for the wanted image

  Toggle between public and private images by using the drop-down selection feature.
  {:tip}

6. Click **Boot from Image** to boot the device by using the selected image. Click **Close** to cancel the action.

## Next Steps
{: #ns-boot-vsi}

After you initiate the boot process, the image is powered off and then started by using the selected image. Boot time varies, as the size and type of
each image varies. After the virtual server is started by using the selected image, it can be accessed as a regularly booted device, but is configured according to the image. Restart the virtual server to power down and return the device to its original state.
