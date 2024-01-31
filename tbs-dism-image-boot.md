---

copyright:
  years: 2024
lastupdated: "2024-01-31"

keywords: question about booting from DISM Windows image

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why won't my Windows DISM created ISO image boot?
{: #troubleshoot-dism-boot}
{: troubleshoot}
{: support} 

You are attempting to use an ISO image created with the Windows Deployment Image Servicing and Management (DISM) tool to boot a virtual server, but rather than using the DISM ISO, the virtual server is booting from the native operating system disk.  
{: shortdesc}

You used the Windows DISM tool to create an ISO image, then used the **Boot from image** option to start your virtual server. Your virtual server boots the native operating system disk instead of the DISM ISO as expected. 
{: tsSymptoms}

DISM created ISOs have a "press any key" prompt which requires intervention at the KVM for the ISO to be booted. If you do not press a key within 5 seconds, the virtual server boots the normal operating system, bypassing the ISO.
{: tsCauses}

You can remove the "press any key" prompt from a DISM created ISO by completing the following steps:
{: tsResolve}

1. Open the ISO with an ISO editing application, for example, PowerISO, UltraISO, etc.
1. Delete `bootfix.bin`.
1. Replace `EFISYS.BIN` with `efisys_noprompt.bin`. You can find `efisys_noprompt.bin` on most Windows installations by searching.
1. Save the ISO.
1. Upload the modified ISO to {{site.data.keyword.cos_full}}, and then import the image from {{site.data.keyword.cos_full_notm}}. For more information, see [Preparing and importing images](/docs/image-templates?topic=image-templates-preparing-and-importing-images). 
1. Try the [Boot from an image](/docs/image-templates?topic=image-templates-booting-a-vsi-from-an-image) task again. 

