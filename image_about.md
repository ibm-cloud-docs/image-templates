---

copyright:
  years: 2014, 2021
lastupdated: "2019-06-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# About image templates
{: #about-image-templates}

Standard image templates provide an imaging option for all {{site.data.keyword.virtualmachinesshort}}, regardless of operating system.
{: shortdesc}

With standard image templates, you can capture an image of an existing virtual server and create a new one based on the captured image. Standard image templates are not compatible with bare metal servers.

## How Image Templates Work
The two main actions for any image template are create and deploy. When you request to create an image, the automated system of {{site.data.keyword.cloud}} takes your server offline. While the server is offline, a compressed backup of your data is created, the configuration information is recorded, and the image template is stored on the {{site.data.keyword.cloud_notm}} SAN. During the deployment stage of the image template, the automated system constructs a new server that is based on the data that is gathered from the selected image. The deployment process makes adjustments for volume, restores the copied data, and then makes final configuration changes (for example, network configurations) for the new host.

## Private Images

Private images are images that are created by a user on the account or images that are created on another account and shared. By default, all images that are created are private.

## Public Images

Public images are pre-configured virtual servers that are posted by {{site.data.keyword.cloud_notm}} or made public by customers and are available for use by all {{site.data.keyword.cloud_notm}} customers. Virtual servers that are provisioned through public image templates are generally configured for optimal performance, with specific combinations of different configuration specs.
