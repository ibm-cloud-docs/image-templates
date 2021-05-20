---

copyright:
  years: 2019, 2021
lastupdated: "2021-05-20"

keywords: VHD image file, encryption, encrypted image, image

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}


# Encrypting VHD images
{: #create-encrypted-image}

To use the E2E Encryption feature, you must encrypt your Virtual Hard Drive (VHD) image with the vhd-util tool before you import it into Image Templates to provision encrypted instances. Two levels of AES encryption are supported: AES 256 bit and AES 512 bit.
{:shortdesc}

## Encrypted VHD image requirements
{: #encrypted-image-reqs}

Encrypted VHD images must meet the following requirements:

* VHD formatted.
* Compatibility with the {{site.data.keyword.cloud}} Console infrastructure environment.
* Provisioned with a [supported OS](/docs/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images).
* Cloud-init enabled.
* Encrypted with [the vhd-util tool](/docs/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool).

## Encrypting your VHD image
{: #vhd-util-tool}

Follow these steps to create your encrypted VHD image:

1. Select a CentOS system version 7 or higher to encrypt your virtual disk image (VHD file) for {{site.data.keyword.cloud_notm}}. If you don't have access to physical hardware with CentOS installed, you can provision a virtual server instance with CentOS 7 inside {{site.data.keyword.cloud_notm}} by using either a public or dedicated host. The CentOS system used to encrypt VHD files need not be encrypted itself.

2. Download the encryption tool from {{site.data.keyword.cloud_notm}}, then use the best available option to encrypt your VHD.

   **Option 1** If your CentOS system is not running in {{site.data.keyword.cloud_notm}}, log in and connect to your customer [VPN](https://www.ibm.com/cloud/vpn-access). For more information about setting up a VPN, see ["Set up SSL VPN connections"](https://cloud.ibm.com/docs/iaas-vpn?topic=iaas-vpn-setup-ipsec-vpn). After you connect to your VPN, [go to the IBM Cloud download site ](http://downloads.service.softlayer.com/citrix/xen/){: external} and select the vhd-util tool RPM package file: `vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm` .

   **Option 2** If you can't download the RPM package file directly into your CentOS system, then connect to your customer VPN and download the file to the workstation that you're working on. You can then upload it to your CentOS system by using the secure copy (scp) command. If you're using a virtual server instance in {{site.data.keyword.cloud_notm}}, use the system’s public IP address for the upload by using the following command.

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

   **Option 3** If you choose to provision a CentOS virtual server instance inside {{site.data.keyword.cloud_notm}} in step 1, you can
   use the following curl command without connecting to your customer VPN:

   ```
   curl -O http://downloads.service.softlayer.com/citrix/xen/vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

3. Install the RPM by using the following command:

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. Identify and select the AES data encryption key (DEK) that you need to encrypt and decrypt your disk image, then write it into a keyfile. This DEK is the same base64-encoded DEK that you wrapped with your key management service-provided customer root key that is in [Preparing your environment](/docs/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment). Key material that is written into keyfiles must be [unwrapped](/docs/key-protect?topic=key-protect-cli-reference#kp-key-command) and not be encoded.

   Because the keyfile isn't base64-encoded, you can't print or view the keyfile content from the command line by using standard ASCII characters.
   {: tip}

   Use the following command to create keyfiles with either an **AES 256 bit** or an **AES 512 bit** encryption key:

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

   The first line of the output from the previous example command indicates that the keyfile named `aes512.dek` contains a 64-byte key. While the numbers that are listed on the second line are the SHA256 hashes or security hashes for the respective encryption keys. Output for files that contain an AES 256-bit encryption key indicates a 32 byte key.
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

The example command in **Step 7** creates a new encrypted VHD file that is named, “debian8-aes512.vhd”. It's encrypted with the AES 512-bit encryption key from the keyfile named “aes512.dek”. The SHA256 hash for its encryption is                                  `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda`.
