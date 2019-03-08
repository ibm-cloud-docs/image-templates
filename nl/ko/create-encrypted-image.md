---

copyright:
  years: 2018
lastupdated: "2018-09-19"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}


# 암호화된 이미지 작성

E2E 암호화 기능의 일부로 이미지 템플리트에 가져올 이미지를 암호화한 다음 암호화된 Virtual Server 인스턴스를 배치하는 데 사용할 수 있습니다.
{:shortdesc}

## 암호화된 이미지 요구사항
{: #encrypted-image-reqs}

사용자가 작성하는 암호화된 이미지는 다음 이미지 요구사항을 충족해야 합니다.

* 이미지가 {{site.data.keyword.cloud}} 콘솔 인프라 환경과 호환 가능합니다.
* 이미지에 CentOS, Debian, Red Hat Enterprise Linux 또는 Ubuntu와 같은 Linux 운영 체제가 포함되어 있습니다.
* 이미지에 cloud-init이 사용으로 설정되어 있습니다.
* 이미지가 [LUKS 디스크 암호화](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption)로 암호화되었습니다.

## QEMU 및 DM-Crypt를 사용하여 암호화된 RAW 이미지 작성
{: #luks-disk-encryption}

이미지를 암호화하려면 고정 VHD 이미지 파일을 RAW 형식으로 변환해야 합니다. 그런 다음 QEMU와 DM-Crypt를 사용하여 LUKS 디스크 암호화로 새 이미지를 작성하십시오. 파일을 암호화된 볼륨으로 마운트하고 암호화되지 않은 이미지 파일을 암호화된 볼륨에 복사하십시오.

이 프로시저에서는 다음 태스크를 단계별로 안내합니다.

* 동적 VHD 이미지를 고정 VHD 이미지로 변환합니다.
* 고정 VHD 이미지를 RAW 파일 형식으로 변환합니다.
* 드라이브를 암호화하는 데 사용할 데이터 암호화 키 파일을 작성합니다.
* LUKS 암호화 헤더뿐 아니라 이미지를 포함하는 새 RAW 파일을 작성합니다.
* LUKS 디스크 암호화를 사용하여 RAW 파일을 포맷합니다.
* 암호화된 RAW 파일을 블록 디바이스에 첨부합니다.
* 암호화되지 않은 이미지를 암호화된 볼륨 디바이스에 복사합니다.

시스템에서 암호화된 볼륨을 마운트하고 마운트 해제하려면 sudo를 통한 `root` 사용자 권한을 사용하여 명령을 실행할 권한이 있어야 합니다. 다음 명령을 실행하여 `root` 사용자 권한을 확인할 수 있습니다.

```
sudo echo "Hello!"
```
{: pre}

이 암호화 태스크를 완료하려면 응답으로 "Hello!"가 다시 표시되어야 합니다.

**팁**: Linux 운영 체제를 실행 중이고 다음 패키지를 사용할 수 있는 시스템에서 이 암호화 태스크를 완료해야 합니다.
* qemu 또는 qemu-image(Linux 운영 체제에 따라 다름)
* cryptsetup

다음 단계를 완료하여 이미지를 암호화하십시오.

1. 동적 VHD 이미지 파일을 고정 VHD 이미지 파일로 변환합니다. 고정 VHD 파일을 사용하면 동적 VHD를 직접 RAW 파일 형식으로 변환하는 경우 발생할 수 있는 손상을 방지합니다. 다음 명령을 실행하여 동적 VHD 이미지 파일을 고정 VHD 이미지 파일로 변환하십시오.

  ```
  qemu-img convert -p -O vpc -o subformat=fixed <DYNAMIC_VHD_FILE> <FIXED_VHD_FILE>
  ```
  {: pre}

  예를 들어 동적 VHD 파일이 _Rhel_7.vhd-0.vhd_이고 고정 VHD 파일의 이름을 _Rhel_7.fixed.vhd-0.vhd_로 지정하려는 경우 다음 명령을 실행하십시오.

  ```
  $ qemu-img convert -p -O vpc -o subformat=fixed Rhel_7.vhd-0.vhd Rhel_7.fixed.vhd-0.vhd
  ```
  {: screen}

  이 명령은 완료하는 데 30분 이상 걸릴 수 있습니다.
  {: tip}

2. QEMU를 사용하여 고정 VHD 이미지 파일을 RAW 파일 형식으로 변환합니다. 이미지가 RAW 파일 형식이어야 암호화할 수 있습니다. 다음 QEMU 명령을 실행하십시오.

  ```
  qemu-img convert -p -O raw <FIXED_VHD_FILE> <RAW_IMAGE_FILE>
  ```
  {: pre}

  예를 들어 고정 VHD 파일이 _Rhel_7.fixed.vhd-0.vhd_이고 RAW 출력 파일의 이름을 _Rhel_7.raw-0.raw_로 지정하려면 다음 명령을 실행하십시오.

  ```
  qemu-img convert -p -O raw Rhel_7.fixed.vhd-0.vhd Rhel_7.raw-0.raw
  ```
  {: screen}

  이 명령은 완료하는 데 30분 이상 걸릴 수 있습니다.
  {: tip}

3. 드라이브를 암호화하고 복호화하는 데 사용할 데이터 암호화 키를 식별합니다. 이 데이터 암호화 키는 암호화된 이미지를 {{site.data.keyword.slportal}}에 가져올 때 지정하고 랩핑하는 키와 같습니다. 드라이브를 암호화하고 복호화하는 데 사용할 데이터 암호화 키를 포함하는 파일을 작성합니다. 이 파일에서 키는 랩핑이 해제되고 base64 인코딩 텍스트 형식이어야 합니다. Base64를 사용하면 우발적으로 공백이 포함되거나 중단되는 경우가 없습니다. base64 인코딩 데이터 암호화 키는 최소 32자 또는 32바이트여야 하며 최대 512자 또는 512바이트여야 합니다. 데이터 암호화 키 자료는 행 바꾸기와 줄 바꾸기가 없이 한 줄로 표시되어야 합니다. 예를 들어 데이터 암호화 키의 `unwrapped_key_material`을 저장하고 base64로 인코딩하려면 다음 명령을 사용하여 `secret.dek`라는 파일을 작성하십시오.

  ```
  $ echo -n $(echo 'unwrapped_key_material' | base64) > secret.dek
  ```
  {: screen}

  이 파일을 안전하게 보관하십시오. 이 키를 유실하면 디스크를 복호화할 수 없습니다.
  {: tip}

4. LUKS 헤더 추가를 처리하는 다음 단계에서 암호화된 디스크용으로 작성할 올바른 크기의 새 RAW 파일을 식별하십시오. 새 RAW 파일은 _dmcrypt_와 _cryptsetup_에서 암호화된 LUKS 형식으로 디스크 컨텐츠를 보유하는 데 사용합니다. 새 RAW 파일의 크기는 2단계에서 작성한 RAW 파일의 크기와 상수 4MB LUKS 헤더의 합계여야 합니다. 기존 RAW 이미지의 크기를 판별하려면 다음 명령을 실행하십시오. 여기서 _Rhel_7.raw-0.raw_는 이미지 이름입니다.

  ```
  ls -l Rhel_7.raw-0.raw
  ```
  {: pre}

  다음과 비슷한 출력이 표시됩니다.

  ```
  -rw-r--r--. 1 user user 42949017600 Mar 5 10:02 Rhel_7.raw-0.raw
  ```
  {: screen}

  여기서 _42949017600_은 RAW 이미지에서 사용 중인 바이트 수입니다.
      1. LUK 헤더를 처리하도록 RAW 이미지 파일 크기에 4MB(바이트로 변환됨)를 추가하십시오. 예를 들어, 42949017600 + (4 x 1024 x 1024) = 42953211904바이트입니다.
      2. RAW 이미지 크기와 LUKS 헤더의 합계를 메가바이트로 변환하고 다음 메가바이트로 반올림하십시오. 예를 들어, 42953211904바이트 / 1024바이트 / 1024킬로바이트 = 40963.375메가바이트 = **40964MG**입니다.

5. 암호화된 RAW 이미지가 될 새 RAW 파일을 올바른 수의 바이트로 작성하십시오. 다음 _dd_ 명령을 사용하여 RAW 파일을 작성하십시오.

  ```
  dd if=/dev/zero of=<ENCRYPTED_RAW_FILENAME> bs=1M count=<TARGET_SIZE_IN_MEGABYTE> status=progress
  ```
  {: pre}

  여기서, _ENCRYPTED_RAW_FILENAME_은 암호화된 RAW 이미지가 될 파일이고 _TARGET_SIZE_IN_MEGABYTE_는 4단계에서 얻은 숫자, 즉 암호화되지 않은 RAW 이미지와 LUKS 헤더의 크기입니다.

  예를 들어 암호화된 이미지가 될 새 RAW 파일을 _Rhel_7.encrypted.raw_로 지정하고 대상 크기를 _40964_MB로 지정하려면 다음 명령을 실행하십시오.

  ```
  $ dd if=/dev/zero of=Rhel_7.encrypted.raw bs=1M count=40964 status=progress
  ```
  {: screen}

6. _dmcrypt_를 사용하여 LUKS 디스크 암호화와 데이터 암호화 키(3단계에서 확보)를 사용하여 RAW 파일을 형식화합니다. 이 단계에서는 이미지를 포함할 파일을 준비합니다. LUKS 암호화로 파일을 포맷하려면 다음 _cryptsetup_ 명령을 실행하십시오.

  ```
  cryptsetup -v luksFormat <ENCRYPTED_RAW_FILENAME> <DEK_FILENAME>
  ```
  {: pre}

  여기서, _ENCRYPTED_RAW_FILENAME_은 암호화할 RAW 파일의 파일 이름이고 _DEK_FILENAME_은 데이터 암호화 키가 저장된 파일의 이름입니다.

  예를 들어, RAW 파일이 _Rhel_7.encrypted.raw_이고 키 파일이 _secret.dek_이면 다음을 입력하십시오.

  ```
  $ cryptsetup -v luksFormat Rhel_7.encrypted.raw secret.dek
  ```
  {: screen}

  cryptsetup 명령을 실행하고 나면 다음 프롬프트가 표시됩니다. 이 프롬프트가 표시되는 것은 정상이며 계속하려면 YES로 응답해야 합니다.

  ```
WARNING!
  ========
  This will overwrite data on Rhel_7.encrypted.raw irrevocably.
  Are you sure (Type uppercase yes): YES
  ```
  {: screen}

7. 암호화된 RAW 파일을 볼륨으로 첨부하여 새 블록 디바이스를 운영 체제에 연관시킵니다. luksOpen 옵션을 사용하여 암호화된 RAW 디바이스를 열려면 _cryptsetup_을 사용하여 다음 명령을 실행하십시오.

  `root` 사용자 권한을 사용하여 다음 명령을 실행하려면 sudo 권한이 있어야 합니다.
  {: tip}

  ```
  sudo cryptsetup -v luksOpen <ENCRYPTED_RAW_FILENAME> <VOLUME_NAME> --key-file <DEK_FILENAME>
  ```
  {: pre}

  여기서, _ENCRYPTED_RAW_FILENAME_은 암호화된 RAW 파일의 파일 이름이고 _VOLUME_NAME_은 _dmcrypt_에서 작성하려고 하는 디바이스의 이름이며 _DEK_FILENAME_은 데이터 암호화 키의 파일 이름입니다.

  예를 들어 암호화된 RAW 파일이 _Rhel_7.encrypted.raw_이고 블록 디바이스의 이름을 _encryptedVolume_으로 지정하려고 하며 데이터 암호화 키 파일이 _secret.dek_이면 다음 명령을 사용합니다.

  ```
  $ sudo cryptsetup -v luksOpen Rhel_7.encrypted.raw encryptedVolume --key-file secret.dek
  ```
  {: screen}

  이 단계가 완료되면 _/dev/mapper/_에서 읽고 쓸 수 있는 새 블록 디바이스가 작성됩니다.

8. 다음 명령을 실행하여 LUKS 볼륨 맵퍼가 작성되었는지 확인하십시오.

  ```
  ls -l /dev/mapper/
  ```
  {: pre}

  다음 예와 비슷한 출력이 표시되어야 합니다.

  ```
  total 0
  lrw-rw---- 1 root root        7 Mar 5 22:22 encryptedVolume -> ../dm-0
  ```
  {: screen}

9. 암호화되지 않은 이미지(2단계에서 확보)를 암호화된 볼륨 디바이스(7단계에서 확보)에 복사하십시오. 다음 _dd_ 명령을 실행하여 암호화되지 않은 이미지를 암호화된 볼륨에 복사하십시오.

  ```
  sudo dd if=<RAW_IMAGE_FILE> of=/dev/mapper/<VOLUME_NAME>
  ```
  {: pre}

  여기서, _RAW_IMAGE_FILE_은 암호화되지 않은 RAW 이미지(2단계에서 작성됨)의 파일 이름이고 _VOLUME_NAME_은 암호화된 볼륨 블록 디바이스의 이름(7단계에서 작성됨)입니다.

  예를 들어 RAW_IMAGE_FILE의 이름을 _Rhel_7.raw-0.raw_로 지정하고 VOLUME_NAME이 _encryptedVolume_이면 다음 명령을 실행하십시오.

  ```
  $ sudo dd if=Rhel_7.raw-0.raw of=/dev/mapper/encryptedVolume status=progress
  ```
  {: screen}

  이 명령은 완료하는 데 30분 이상 걸릴 수 있습니다.
  {: tip}

10. 다음을 수행하여 볼륨 맵퍼를 영구 삭제하고 암호화된 데이터 파일과의 LUKS 연결을 종료하십시오.

  ```
  sudo cryptsetup luksClose encryptedVolume
  ```
  {: pre}

  여기서, _encryptedVolume_은 암호화된 볼륨 블록 디바이스의 이름입니다.

  이제 _ENCRYPTED_RAW_FILENAME_이 초기화되므로 IBM Cloud Object Storage에 업로드할 수 있습니다. 예를 들어, 암호화된 RAW 파일이 _Rhel_7.encrypted.raw_이면 해당 이미지를 IBM Cloud Object Storage에 업로드하십시오.
