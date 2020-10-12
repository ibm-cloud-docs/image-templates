---

copyright:
  years: 2014, 2020
lastupdated: "2020-02-04"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Provisioning with a cloud-init enabled image
{: #provisioning-with-a-cloud-init-enabled-image}

When you order a virtual server, many operating systems now use a cloud-init enabled image to optimize provisioning time. You can also import
a customized image that you've enabled for cloud-init.
{:shortdesc}

The following operating systems now default to a cloud-init enabled image when you order a virtual server without add-ons. (Add-ons include additional software, post-provisioning scripts, and advanced monitoring.)
* CentOS 6, 7
* Debian 8, 9
* Red Hat Enterprise Linux 6.x, 7.x
* Ubuntu 16.04, 18.04
* Windows Server 2012, 2012 R2, 2016, 2019

When you order a virtual server with a cloud-init enabled operating system, you can add user data or metadata with custom provisioning scripts. In the User Data field on the order form, enter optional cloud-init user data or optional metadata for the server.

## Before you begin
{: #byb-cloud-init-imt}

First, navigate to the device menu and ensure you have the correct account permissions to complete the tasks.

* Navigate to your console's device menu. For more information, see [Navigating to devices](/docs/image-templates?topic=virtual-servers-navigating-devices).
* Ensure you have any necessary account permissions and device access. Only the account owner, or a user with the **Manage Users** classic infrastructure permission, can adjust the permissions.

For more information about permissions, see [Classic infrastructure permissions](/docs/account?topic=account-infrapermission#infrapermission) and [Managing device access](/docs/virtual-servers?topic=virtual-servers-managing-device-access).

## Import a customized cloud-init enabled image

If you’ve created a customized image that’s cloud-init enabled, you can designate it as a cloud-init image on the Import Image page of
the {{site.data.keyword.slportal_full}}.

To access the Import Image page of Image Templates and mark an image as cloud-init enabled, complete the following steps:
1. From the **Devices** menu select **Manage** > **Images**.
2. Click the **Import Image** tab.
3. Complete the required information for importing your cloud-init enabled image, and select the **Cloud-init** checkbox that’s shown near the **Operating System** dropdown box. For more information about importing images, see [Import an Image](/docs/image-templates?topic=image-templates-preparing-and-importing-images#import-icos).

## Mark an image template as cloud-init enabled

If you have an existing VHD image template that is cloud-init enabled, you can designate it as cloud-init enabled on the details page of
the image template.

To access an image template and mark it as cloud-init enabled, complete the following steps:
1. From the **Devices** menu select **Manage** > **Images**.
2. From the list of templates, click the image template name that you want to update.
3. On the Image Template Details page, select the **Enabled** checkbox under the **Cloud-init** heading, and click **Update**.


If your image is encrypted, the **Cloud-init** checkbox is already selected by default since encrypted images must be cloud-init enabled.
{: note}

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
* Cloud-init image configured to use SSH for logging in to your virtual server instance.

  The following example cloud.cfg file shows settings used in a Red Hat Enterprise Linux 7 deployment. These settings differ from the     default settings in the cloud-init installation package on Red Hat Enterprise Linux 7.

  ```
  # cat /etc/cloud/cloud.cfg
  # Configure the Datasource for both instances
  datasource_list: [ ConfigDrive ]
 
  user: root
  ssh_pwauth: True
  disable_root: False
  manage_etc_hosts: True
 
  system_info:
   # This will affect which distro class gets used
   distro: rhel
   # Other config here will be given to the distro class and/or path classes
   paths:
      cloud_dir: /var/lib/cloud/
      templates_dir: /etc/cloud/templates/

  cloud_init_modules:
   - migrator
   - seed_random
   - bootcmd
   - write-files
   - disk_setup
   - mounts
   - ca-certs
   - rsyslog
   - users-groups
   - ssh

  cloud_config_modules:
  # Emit the cloud config ready event
  # this can be used by upstart jobs for 'start on cloud-config'.
   - snap_config
   - locale
   - set-passwords
   - ntp
   - timezone
   - disable-ec2-metadata
   - set_hostname
   - update_hostname
   - update_etc_hosts
   - runcmd

  # The modules that run in the 'final' stage
  cloud_final_modules:
   - snappy
   - package-update-upgrade-install
   - fan
   - lxd
   - puppet
   - chef
   - salt-minion
   - mcollective
   - rightscale_userdata
   - scripts-vendor
   - scripts-per-once
   - scripts-per-boot
   - scripts-per-instance
   - scripts-user
   - ssh-authkey-fingerprints
   - keys-to-console
   - phone-home
   - final-message
   - power-state-change
  ``` 
  {: pre}

### Windows requirements

* The Cloudbase-init Metadata Service is required for public and private networks support in {{site.data.keyword.cloud_notm}} infrastructure. You can access the service at
[https://github.com/softlayer/bluemix-cloudbase-init ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/softlayer/bluemix-cloudbase-init).
* If you are using a Vyatta in your environment, you must configure the Vyatta to allow API calls to API load balancers. For more information, see [Brocade vRouter (Vyatta) Set up Guide for VMware Environments with File Storage](/docs/virtual-router-appliance?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges#load-balancer-ips).
* Run the following commands to sysprep the image:
  
  ```
  C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\SetSetupComplete.cmd
  ```
  {: pre}
  
  ```
  C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:"C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\Unattend.xml"
  ```
  {: pre}
