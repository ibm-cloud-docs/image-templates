---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Provisioning with a cloud-init enabled image

When you order a virtual server, many operating systems now use a cloud-init enabled image to optimize provisioning time. You can also import
a customized image that you've enabled for cloud-init.
{:shortdesc}

The following operating systems now default to a cloud-init enabled image when you order a virtual server without add-ons. (Add-ons include additional software, post-provisioning scripts, and advanced monitoring.)
* CentOS 7
* Debian 8, 9
* Ubuntu 16.04, 18.04
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

When you order a virtual server with a cloud-init enabled operating system, you can add user data or metadata with custom provisioning scripts. In the User Data field on the order form, enter optional cloud-init user data or optional metadata for the server.

## Import a customized cloud-init enabled image

If you’ve created a customized image that’s cloud-init enabled, you can designate it as a cloud-init image on the Import Image page of
the {{site.data.keyword.slportal_full}}.

To access the Import Image page of Image Templates and mark an image as cloud-init enabled, complete the following steps:
1. From the **Devices** menu select **Manage** > **Images**.
2. Click the **Import Image** tab.
3. Complete the required information for importing your cloud-init enabled image, and select the **Cloud init** checkbox that’s shown near
the **Operating System** dropdown box. For more information about importing images, see Import an Image.

## Mark an image template as cloud-init enabled

If you have an existing VHD image template that is cloud-init enabled, you can designate it as cloud-init enabled on the details page of
the image template.

To access an image template and mark it as cloud-init enabled, complete the following steps:
1. From the **Devices** menu select **Manage** > **Images**.
2. From the list of templates, click the image template name that you want to update.
3. On the Image Template Details page, select the **Enabled** checkbox under the **Cloud Init** heading, and click **Update**.

## Work with an image template created from a cloud-init provisioned virtual server

Cloud-init typically only runs once. However, if you provision a virtual server from a cloud-init enabled image and subsequently create
an image template from that virtual server, the UUID is recorded. If that image template is used to create another
virtual server, cloud-init runs again.

## Create image templates that are cloud-init enabled

For information about configuring images, see
[cloud-init documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloudinit.readthedocs.io/en/latest/).

For information about datasources, see [Datasources ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html). {{site.data.keyword.cloud_notm}} cloud-init images are created for the
environment by using the [Config Drive ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - Version 2 datasource to supply the metadata.

### Linux requirements
* Cloud-init version 0.7.7 or greater

### Windows requirements
* Cloudbase-init Metadata Service for public and private network support in {{site.data.keyword.cloud_notm}} infrastructure. The service  also updates the Customer Portal with the Windows virtual server credentials. You can access the service at
[https://github.com/softlayer/bluemix-cloudbase-init ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/softlayer/bluemix-cloudbase-init).
* If you are using a Vyatta in your environment, you must configure the Vyatta to allow API calls to API load balancers. For more information, see [Brocade vRouter (Vyatta) Set up Guide for VMware Environments with File Storage ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/infrastructure/FileStorage?topic=FileStorage-configureVyatta#setting-up-brocade-vrouter-vyatta-for-vmware-environments-with-file-storage).
