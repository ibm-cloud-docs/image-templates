---

copyright:
  years: 2014, 2026
lastupdated: "2026-10-19"

keywords:

subcollection: image-templates

---

{{site.data.keyword.attribute-definition-list}}

# Deleting an image template
{: #deleting-an-image-template}

Standard image templates can be deleted at any time after they are created.
{: shortdesc}

## Before you begin
{: #byb-delete-template}

First, go to the device menu and make sure that you have the correct account permissions to complete the tasks.

* Go to your console's device menu.
* Make sure that you have any necessary account permissions and device access. Only the account owner, or a user with the **Manage Users** classic infrastructure permission, can adjust the permissions.

For more information about permissions, see {Managing device access](/docs/virtual-servers?topic=virtual-servers-managing-device-access).

## Deleting an image template
{: #deleting-an-image-template_steps}

Upon deletion, image templates cannot be retrieved and are wiped permanently from all data centers. Use the following steps to delete an image template.

1. From the **Devices** menu, select **Manage > Images**.
2. Click **Actions** for the image template that you want to delete and select **Delete**.
3. Select **Yes** on the confirmation box to complete the deletion. Select **No** to cancel the deletion.

After an image template is deleted, the template is no longer available to use. All devices that were created by using the template remain available and fully functional. However, no additional devices can be created by using the deleted template. Image templates cannot be recovered after they are deleted.
