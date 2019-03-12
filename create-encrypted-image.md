---

copyright:
  years: 2018
lastupdated: "2018-09-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}


# Creating an encrypted image
{: #create-encrypted-image}

As part of the E2E Encryption feature, you can encrypt an image to import into Image Templates and use it to deploy an encrypted virtual server instance.
{:shortdesc}

## Encrypted image requirements
{: #encrypted-image-reqs}

An encrypted image that you create must meet the following image requirements:

* Image is compatible with the {{site.data.keyword.cloud}} Console infrastructure environment.
* Image includes a Linux operating system such as CentOS, Debian, Red Hat Enterprise Linux, or Ubuntu.
* Image is cloud-init enabled.
* Image is encrypted with [LUKS disk encryption](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption).

## Using QEMU and DM-Crypt to create an encrypted RAW image
{: #luks-disk-encryption}

To encrypt your image, you must convert a fixed VHD image file to RAW format. Then, use QEMU and DM-Crypt to create a new image file with LUKS disk encryption. Mount the file as an encrypted volume, and copy the unencrypted image file into the encrypted volume.

This procedure walks you through the following tasks:

* Convert your dynamic VHD image to a fixed VHD image.
* Convert your fixed VHD image to RAW file format.
* Create a data encryption key file that you will use to encrypt the drive.
* Create a new RAW file to contain the image as well as the LUKS encryption header.
* Format the RAW file with LUKS disk encryption.
* Attach the encrypted RAW file to a block device.
* Copy the unencrypted image into the encrypted volume device.

You must have the privilege to run commands using `root` user authority, via sudo, to mount and unmount encrypted volumes on your system. You can issue the following command to verify your `root` user privileges:

```
sudo echo "Hello!"
```
{: pre}

You must receive "Hello!" echoed back in reply to complete this encryption task.

**Tip**: You must complete this encryption task on a system that is running a Linux operating system and has the following packages available:
* qemu or qemu-image (depending on your Linux operating system)
* cryptsetup

Complete the following steps to encrypt your image.

1. Convert your dynamic VHD image file to a fixed VHD image file. A fixed VHD file prevents corruption that might occur if a dynamic VHD is converted directly to RAW file format. Run the following command to convert the dynamic VHD image file to a fixed VHD image file:

  ```
  qemu-img convert -p -O vpc -o subformat=fixed <DYNAMIC_VHD_FILE> <FIXED_VHD_FILE>
  ```
  {: pre}

  For example, if your dynamic VHD file is _Rhel_7.vhd-0.vhd_ and you want the fixed VHD file to be named _Rhel_7.fixed.vhd-0.vhd_, run the following command:

  ```
  $ qemu-img convert -p -O vpc -o subformat=fixed Rhel_7.vhd-0.vhd Rhel_7.fixed.vhd-0.vhd
  ```
  {: screen}

  This command might take 30 minutes or more to complete.
  {: tip}

2. Convert your fixed VHD image file to RAW file format by using QEMU. The image must be in RAW file format before you can encrypt it. Run the following QEMU command:

  ```
  qemu-img convert -p -O raw <FIXED_VHD_FILE> <RAW_IMAGE_FILE>
  ```
  {: pre}

  For example, if your fixed VHD file is _Rhel_7.fixed.vhd-0.vhd_, and you want the RAW output file named _Rhel_7.raw-0.raw_, run the following command:

  ```
  qemu-img convert -p -O raw Rhel_7.fixed.vhd-0.vhd Rhel_7.raw-0.raw
  ```
  {: screen}

  This command might take 30 minutes or more to complete.
  {: tip}

3. Identify the data encryption key that you will use to encrypt and decrypt the drive. This data encryption key is the same key that you wrap and specify when you import the encrypted image to {{site.data.keyword.slportal}}. Create a file that contains the data encryption key that you'll use to encrypt and decrypt the drive. In this file, the key must be unwrapped and in base64 encoded text. Base64 helps ensure that no accidental spaces or breaks are included. The base64 encoded data encryption key must have a minimum of 32 characters or bytes, and a maximum of 512 characters or bytes. The data encryption key material must be on one line with no line breaks and no newline. For example, use the following command to create a file called `secret.dek` to store your `unwrapped_key_material` for your data encryption key and encode it with base64:

  ```
  $ echo -n $(echo 'unwrapped_key_material' | base64) > secret.dek
  ```
  {: screen}

  Keep this key safe. If you lose this key, you won't be able to decrypt your disk.
  {: tip}

4. Identify the correct size of the new RAW file to create in the following step for the encrypted disk, accounting for the addition of a LUKS header. The new RAW file will be used by _dmcrypt_ and _cryptsetup_ to hold the disk content in an encrypted LUKS format. The size of the new RAW file must be the sum of the size of the RAW file created in step 2 and a constant 4 MB LUKS header. To determine the size of your existing RAW image, run the following command, where _Rhel_7.raw-0.raw_ is the image name:

  ```
  ls -l Rhel_7.raw-0.raw
  ```
  {: pre}

  You will see output similar to the following:

  ```
  -rw-r--r--. 1 user user 42949017600 Mar 5 10:02 Rhel_7.raw-0.raw
  ```
  {: screen}

  Where _42949017600_ is the number of bytes that the RAW image is using
      1. Add 4 MB (converted to bytes) to the RAW image file size to account for the LUKS header. For example, 42949017600 + (4 x 1024 x 1024) = 42953211904 bytes.
      2. Convert the sum of the RAW image size and the LUKS header to megabytes and round up to the next megabyte. For example, 42953211904 bytes / 1024 bytes / 1024 kilobytes = 40963.375 megabytes = **40964 MG**.

5. Create a new RAW file with the correct number of bytes that will become the encrypted RAW image. Use the following _dd_ command to create the RAW file:

  ```
  dd if=/dev/zero of=<ENCRYPTED_RAW_FILENAME> bs=1M count=<TARGET_SIZE_IN_MEGABYTE> status=progress
  ```
  {: pre}

  Where _ENCRYPTED_RAW_FILENAME_ is the file that will become the encrypted RAW image and _TARGET_SIZE_IN_MEGABYTE_ is the number you got from step 4, the size of the unencrypted RAW image plus the LUKS header.

  For example, if you want your new RAW file that will become the encrypted image to be _Rhel_7.encrypted.raw_, and its target size is _40964_ MB, you run the following command:

  ```
  $ dd if=/dev/zero of=Rhel_7.encrypted.raw bs=1M count=40964 status=progress
  ```
  {: screen}

6. Format the RAW file with LUKS disk encryption by using _dmcrypt_ and your data encryption key (from step 3). This step prepares the file to contain the image. To format the file with LUKS encryption, run the following _cryptsetup_ command:

  ```
  cryptsetup -v luksFormat <ENCRYPTED_RAW_FILENAME> <DEK_FILENAME>
  ```
  {: pre}

  Where _ENCRYPTED_RAW_FILENAME_ is the file name of the RAW file that you want to encrypt and _DEK_FILENAME_ is the name of the file where your data encryption key is stored.

  For example, if your RAW file is _Rhel_7.encrypted.raw_, and the key file is _secret.dek_, you enter:

  ```
  $ cryptsetup -v luksFormat Rhel_7.encrypted.raw secret.dek
  ```
  {: screen}

  After running the cryptsetup command, you will receive the following prompt. This prompt is expected, and you must respond with YES to continue.

  ```
  WARNING!
  ========
  This will overwrite data on Rhel_7.encrypted.raw irrevocably.
  Are you sure (Type uppercase yes): YES
  ```
  {: screen}

7. Attach the encrypted RAW file as a volume to associate a new block device to the operating system. Using _cryptsetup_, run the following command to open the encrypted RAW device using the luksOpen option.

  You must have sudo privilege to run the following command using `root` user authority.
  {: tip}

  ```
  sudo cryptsetup -v luksOpen <ENCRYPTED_RAW_FILENAME> <VOLUME_NAME> --key-file <DEK_FILENAME>
  ```
  {: pre}

  Where _ENCRYPTED_RAW_FILENAME_ is the file name of the encrypted RAW file, _VOLUME_NAME_ is the name of the device that _dmcrypt_ will attempt to create, and _DEK_FILENAME_ is the file name of your data encryption key.

  For example, if your encrypted RAW file is _Rhel_7.encrypted.raw_, you want to name the block device _encryptedVolume_, and the data encryption key file is _secret.dek_, you use the following command:

  ```
  $ sudo cryptsetup -v luksOpen Rhel_7.encrypted.raw encryptedVolume --key-file secret.dek
  ```
  {: screen}

  When this step is complete, a new block device is created that you can read from and write to under _/dev/mapper/_.

8. Confirm that the LUKS volume mapper was created successfully by running the following command:

  ```
  ls -l /dev/mapper/
  ```
  {: pre}

  You should see output that looks similar to the following example:

  ```
  total 0
  lrw-rw---- 1 root root        7 Mar 5 22:22 encryptedVolume -> ../dm-0
  ```
  {: screen}

9. Copy the unencrypted image (from step 2) into the encrypted volume device (from step 7). Run the following _dd_ command to copy the unencrypted image to the encrypted volume:

  ```
  sudo dd if=<RAW_IMAGE_FILE> of=/dev/mapper/<VOLUME_NAME>
  ```
  {: pre}

  Where _RAW_IMAGE_FILE_ is the file name of the unencrypted RAW image (created in step 2) and _VOLUME_NAME_ is the name of the encrypted volume block device name (created in step 7).

  For example, if your RAW_IMAGE_FILE is named _Rhel_7.raw-0.raw_, and your VOLUME_NAME is _encryptedVolume_, you run the following command:

  ```
  $ sudo dd if=Rhel_7.raw-0.raw of=/dev/mapper/encryptedVolume status=progress
  ```
  {: screen}

  This command might take 30 minutes or more to complete.
  {: tip}

10. Destroy the volume mapper and close the LUKS connection to the encrypted data file:

  ```
  sudo cryptsetup luksClose encryptedVolume
  ```
  {: pre}

  Where _encryptedVolume_ is the name of the encrypted volume block device.

  Your _ENCRYPTED_RAW_FILENAME_ is now initialized and you can upload it to IBM Cloud Object Storage. For example, if your encrypted RAW file is _Rhel_7.encrypted.raw_, upload that image to IBM Cloud Object Storage.
