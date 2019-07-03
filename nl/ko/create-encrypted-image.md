---

copyright:
  years: 2019
lastupdated: "2019-04-29"

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


# VHD 이미지 암호화 
{: #create-encrypted-image}

E2E 암호화 기능을 사용하려면 암호화된 인스턴스를 프로비저닝하기 위해 VHD 이미지를 이미지 템플리트로 가져오기 전에 vhd-util 도구를 사용하여 암호화해야 합니다. 두 가지 레벨의 AES 암호화(AES 256비트 및 AES 512비트)가 지원됩니다.
{:shortdesc}

## 암호화된 VHD 이미지 요구사항
{: #encrypted-image-reqs}

암호화된 VHD 이미지는 다음 요구사항을 충족해야 합니다.

* VHD 형식입니다.
* {{site.data.keyword.cloud}} 콘솔 인프라 환경과 호환 가능합니다.
* [지원되는 OS](/docs/infrastructure/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images)로 프로비저닝되었습니다.
* cloud-init이 사용으로 설정되어 있습니다.
* [vhd-util 도구](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool)로 암호화되었습니다.

## VHD 이미지 암호화
{: #vhd-util-tool}

암호화된 VHD 이미지를 작성하려면 다음 단계를 수행하십시오.

1. {{site.data.keyword.cloud_notm}}에 대한 가상 디스크 이미지(VHD 파일)를 암호화할 버전 7 이상을 실행 중인 CentOS 시스템을 선택하십시오. CentOS가 설치된 실제 하드웨어에 액세스할 수 없는 경우 공용 또는 전용 호스트를 사용하여 {{site.data.keyword.cloud_notm}} 내부에 CentOS 7이 있는 가상 서버 인스턴스를 프로비저닝할 수 있습니다. VHD 파일을 암호화하는 데 사용되는 CentOS 시스템 자체는 암호화될 필요가 없습니다.

2. CentOS 시스템에 로그인하고 고객 VPN에 연결한 후 [SoftLayer 다운로드 사이트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://downloads.service.softlayer.com/citrix/xen/){: new_window}로 이동하여  vhd-util 도구 RPM 패키지 파일 vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm을 선택하십시오.   

   RPM 패키지 파일을 CentOS 시스템에 직접 다운로드할 수 없는 경우 현재 작업 중인 워크스테이션에 해당 파일을 다운로드하십시오. 그런 다음 scp(secure copy) 명령을 사용하여 CentOS 시스템에 업로드할 수 있습니다. {{site.data.keyword.cloud_notm}}에서 가상 서버 인스턴스를 사용 중인 경우 다음 명령을 사용하여 이 업로드에 시스템의 공인 IP 주소를 사용하십시오.

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

3. 다음 명령을 사용하여 RPM을 설치하십시오.

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. 디스크 이미지를 암호화하고 복호화하는 데 필요한 AES 암호화 키를 식별하여 키 파일에 기록하십시오. 이 AES 암호화 키는 [환경 준비](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment)에서 키 관리 서비스가 제공한 고객 루트 키로 랩핑된 데이터 암호화 키와 동일합니다. 키 파일에 기록된 키 자료는 랩핑 해제되고 인코딩되지 않아야 합니다. 

   data_key는 키 파일 내부에서 base64로 인코딩되지 않았으므로 표준 ASCII 문자를 사용하여 명령행에서 키 파일 컨텐츠를 인쇄하거나 볼 수 없습니다. 
   {: tip}

   다음 명령을 사용하여 **AES 256비트** 또는 **AES 512비트** 암호화 키로 키 파일을 작성하십시오. 
   
   ```
   echo <data_key> | base64 -d - > <keyfile_name>
   ```
   {: pre} 

   예제 명령:

   ```
   echo Nrfen98EpMxF2B+wdgLfagzrqvgUZfMK4vL2T0NsT20ihrsNC9pUUHtizF6218pze8RLCgQ6kwxuE58IWLzgDA== | base64 -d - > aes512.dek
   ```
   {: screen}

5. 다음 명령을 사용하여 이전 단계에서 작성한 키 파일을 확인하십시오.

   ```
   vhd-util key -C -k <keyfile_name>
   ```
   {: pre}

   예제 명령 및 출력:

   ```
   vhd-util key -C -k aes512.dek
   vhd_util_read_key: using keyfile aes512.dek, Size (bytes) 64
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

   이전 예제 명령의 출력에서 첫 번째 행은 `aes512.dek`라는 키 파일에 64바이트 키가 포함되어 있음을 표시하며 두 번째 행에 나열된 숫자는 각 암호화 키에 대한 SHA256 해시 또는 보안 해시입니다. AES 256비트 암호화 키를 포함하는 파일의 출력에는 32바이트 키가 표시됩니다.
   {: tip} 

6. 다음 명령을 사용하여 VHD 파일의 암호화된 사본을 작성하십시오. `target_vhd`는 `source_vhd`의 암호화된 버전을 포함하는 파일의 이름을 나타냅니다.

   ```
   vhd-util copy -n <source_vhd> -N <target_vhd> -k <keyfile_name>
   ```
   {: pre}    

   예제 명령:

   ```
   vhd-util copy -n debian8-ne.vhd -N debian8-aes512.vhd -k aes512.dek
   ```
   {: screen}

7. 다음 명령을 사용하여 VHD 파일이 암호화되었는지 확인하십시오.

   ```
   vhd-util key -p -n <vhd_filename>
   ```
   {: pre}

   예제 명령 및 출력:

   ```
   vhd-util key -p -n debian8-aes512.vhd
   0000000000000000000000000000000000000000000000000000000000000000
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

VHD 파일이 암호화된 경우 이전 예제에 표시된 대로 출력에 두 개의 해시 값이 표시됩니다. 첫 번째 해시는 모두 0입니다. 두 번째 해시는 AES 암호화 키가 VHD를 암호화하고 복호화하는 데 사용하는 SHA256 해시입니다. VHD 파일의 SHA256 해시가 **5단계**에 표시된 해시와 동일한지 확인하십시오.

**7단계**의 예제 명령은 "debian8-aes512.vhd"라는 암호화된 새 VHD 파일을 작성합니다. 이 파일은 "aes512.dek"라는 키 파일의 AES 512비트 암호화 키로 암호화됩니다. 해당 암호화에 대한 SHA256 해시는 `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda`입니다.
