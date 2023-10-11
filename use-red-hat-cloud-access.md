---

copyright:
  years: 2014, 2023
lastupdated: "2023-10-11"

keywords:

subcollection: image-templates

---

{{site.data.keyword.attribute-definition-list}}


# Using your own OS license or subscription
{: #using-your-own-os-license-or-subscription}

When you create an image template with a Virtual Hard Disk (VHD) image, you can opt to provide your own RHEL operating system license through the [Red Hat Cloud Access](https://www.redhat.com/en/technologies/cloud-computing/cloud-access){: external} subscription or a Windows license through the Microsoft Enterprise Agreement.
{: shortdesc}

If you deploy an image in {{site.data.keyword.cloud}} that indicates you are using your own license, the following support terms exist:
* {{site.data.keyword.IBM_notm}} supports hypervisors, provisioning instances, importing images, rebooting an image, reloading an OS, and capturing an image.
* The company from which you buy the operating system license supports the image itself.

When you provide your own license for an image, the following restrictions apply to the image:
* The image is a private image. It can't be shared publicly.
* The image can't have any software add-ons included. Add-ons must be installed after the image is provisioned.

## Using Red Hat Cloud Access
{: #using-rhel-cloud-access}

For more information about our certification as a Red Hat Enterprise Linux cloud provider, see [Infrastructure as a Service (IaaS)](https://access.redhat.com/ecosystem/cloud-provider/2262101){: external}.

## Using your own Windows license
{: #using-your-own-windows-license}

The following Windows versions are supported:

* Windows Server 2022
* Windows Server 2019
* Windows Server 2016

If you have questions about your existing Windows license eligibility or reporting requirements, contact your Microsoft representative. When you create an image template by using your own Windows license, you must provision that image on a dedicated host. You can't provision a public instance or a dedicated instance that is auto-assigned to a host when you use an image under your own Windows license. Additionally, when you create or update a Windows image template under your own license, the Windows image can't be a cloud-init image.

## Before you begin
{: #byb-redhat-imt}

First, go to the device menu and make sure that you have the correct account permissions to complete the tasks.

* Go to your console's device menu. For more information, see [Navigating to devices](/docs/image-templates?topic=virtual-servers-navigating-devices).
* Make sure that you have any necessary account permissions and device access. Only the account owner, or a user with the **Manage Users** classic infrastructure permission, can adjust the permissions.

For more information about permissions, see [Classic infrastructure permissions](/docs/account?topic=account-infrapermission#infrapermission) and [Managing device access](/docs/virtual-servers?topic=virtual-servers-managing-device-access).

## Importing an image that designates your own license
{: #importing-that-mage-designates-your-own-license}

You can import a VHD image and specify that you are providing your own license or subscription for the operating system.

To access the Import Image page of Image Templates and mark a VHD image to use your own license or subscription, complete the following steps:
1. From the **Devices** menu, select **Manage > Images**.
2. Click the **Import Image** tab.
3. Complete the required information for importing your VHD image, and select the **Your License** checkbox that is shown near the **Operating System** dropdown box. For more information about importing images, see [Preparing and importing images](/docs/image-templates?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images).

## Updating an image template to specify a user provided OS license
{: #update-image-template-user-os-license}

If you have an existing VHD image template, you can specify that you want to provide your own license or subscription for the operating system.

To access an image template and signal that it uses your own existing license or subscription, complete the following steps:
1. From the **Devices** menu, select **Manage > Images**.
2. From the list of templates, click the image template name that you want to update.
3. On the Image Template Details page, select the **User Provided** checkbox under the **OS License** heading, and click **Update**.
