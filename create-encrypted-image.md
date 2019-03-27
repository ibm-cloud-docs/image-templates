---

copyright:
  years: 2019
lastupdated: "2019-03-27"

keywords: VHD image file, encryption, encrypted image, image

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}


# Encrypting VHD images 
{: #create-encrypted-image}

To use the E2E Encryption feature, you must encrypt your VHD image with the vhd-util tool before importing it into Image Templates for provisioning encrypted instances. Two levels of AES encryption are supported: AES 256-bit and AES 512-bit.
{:shortdesc}

## Encrypted VHD image requirements
{: #encrypted-image-reqs}

Encrypted VHD images must meet the following requirements:

* VHD formatted.
* Compatibility with the {{site.data.keyword.cloud}} Console infrastructure environment.
* Provisioned with a [supported OS](/docs/infrastructure/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images).
* Cloud-init enabled.
* Encrypted with [the vhd-util tool](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool).

## Encrypting your VHD image
{: #vhd-util-tool}

Follow these steps to create your encrypted VHD image:

1. Select a CentOS system running version 7 or higher to encrypt your virtual disk image (VHD file) for {{site.data.keyword.cloud_notm}}. If you do not have access to physical hardware with CentOS installed, you can provision a virtual server instance with CentOS 7 inside {{site.data.keyword.cloud_notm}} by using either a public or dedicated host. The CentOS system used to encrypt VHD files need not be encrypted itself.

2. Log in to your CentOS system and connect to your customer VPN, then [go to the Softlayer download site ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://downloads.service.softlayer.com/citrix/xen/){: new_window} and select the vhd-util tool RPM package file: vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm   

   If you cannot download the RPM package file directly into your CentOS system, then download the file to the workstation you are          currently working on. You can then upload it to your CentOS system by using the secure copy (scp) command. If you are using a virtual    server instance in {{site.data.keyword.cloud_notm}}, use the system’s public IP address for this upload by using the following          command.

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

3. Install the RPM by using the following command:

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. Identify the AES encryption key that you need to encrypt and decrypt your disk image and write it into a keyfile. This AES encryption key is the same data encryption key that you wrapped with the Key Protect customer root key in [Preparing your environment](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment). Key material that is written into keyfiles must be unwrapped and not be encoded. 

   Because the data_key is not base64 encoded inside the keyfiles, you cannot print or view the keyfile content from the command line      by using standard ASCII characters. 
   {: tip}

   Use the following command to create keyfiles with either an **AES 256-bit** or an **AES 512-bit** encryption key: 
   
   ```
   echo <data_key> | base64 -d - > <keyfile_name>
   ```
   {: pre} 

   Example command:

   ```
   echo Nrfen98EpMxF2B+wdgLfagzrqvgUZfMK4vL2T0NsT20ihrsNC9pUUHtizF6218pze8RLCgQ6kwxuE58IWLzgDA== | base64 -d - > aes512.dek
   ```
   {: screen}

5. Use the following command to verify the keyfiles that you created in the previous step:

   ```
   vhd-util key -C -k <keyfile_name>
   ```
   {: pre}

   Example command with output:

   ```
   vhd-util key -C -k aes512.dek
   vhd_util_read_key: using keyfile aes512.dek, Size (bytes) 64
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

   The first line of the output from the previous example command indicates that the keyfile named `aes512.dek` contains a 64-byte key,    while the numbers listed on the second line are the SHA256 hashes or security hashes for the respective encryption keys. Output for      files containing an AES 256-bit encryption key will indicate a 32-byte key.
   {: tip} 

6. Use the following command to create encrypted copies of your VHD files. `target_vhd` represents the name of the file that contains the encrypted version of `source_vhd`.

   ```
   vhd-util copy -n <source_vhd> -N <target_vhd> -k <keyfile_name>
   ```
   {: pre}    

   Example command:

   ```
   vhd-util copy -n debian8-ne.vhd -N debian8-aes512.vhd -k aes512.dek
   ```
   {: screen}

7. Verify that the VHD files are encrypted by using the following command.

   ```
   vhd-util key -p -n <vhd_filename>
   ```
   {: pre}

   Example command with output:

   ```
   vhd-util key -p -n debian8-aes512.vhd
   0000000000000000000000000000000000000000000000000000000000000000
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

If the VHD file is encrypted, you see two hash values in the output as shown in the previous example. The first hash is all zeros. The second hash is the SHA256 hash that the AES encryption key uses to encrypt and decrypt the VHD. Make sure that the SHA256 hashes for the VHD files are the same as the hashes shown in **Step 5**.

The example command in **Step 7** creates a new encrypted VHD file that is named, “debian8-aes512.vhd”. It is encrypted with the AES 512-bit encryption key from the keyfile named “aes512.dek”. The SHA256 hash for its encryption is                                  `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda`.
