---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:pre: .pre}
{:screen: .screen}


# 이미지 준비 및 가져오기
{: #preparing-and-importing-images}

{{site.data.keyword.slportal_full}}의 이미지 템플리트 화면을 사용하여 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about) 서비스 인스턴스에서 이미지를 가져올 수 있습니다. 가상 하드 디스크(VHD) 또는 가상 머신 디스크(VMDK) 형식의 이미지를 가져올 수 있습니다. 가져오기 후 VMDK 이미지가 VHD로 변환됩니다. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스의 버킷에 이미지를 업로드하고 나면 {{site.data.keyword.slportal}}의 이미지 템플리트 저장소에 가져올 수 있습니다.
{:shortdesc}

{{site.data.keyword.cos_full_notm}}에서 이미지를 가져오려면 업그레이드된 계정이 있어야 합니다. 자세한 정보는 [IBM ID로 전환 및 계정 연결](/docs/account/softlayerlink.html)을 참조하십시오.
{: tip}

이 가져오기 기능을 사용하려면 {{site.data.keyword.cloud_notm}} 콘솔(cloud.ibm.com)을 통해 주문된 [IBM Cloud Object Storage 인스턴스](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision#provision-account)가 있어야 합니다.  control.softlayer.com의 IBM Cloud Object Storage는 지원되지 않습니다.
{: important}

이미지를 이미지 템플리트로 가져온 후에는 이를 사용하여 기존 가상 서버를 프로비저닝하거나 시작할 수 있습니다. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에서 가져온 이미지는 VHD, VMDK 또는 사용자 정의 ISO일 수 있습니다. VHD 및 VMDK 가져오기는 다음 64비트 운영 체제에서만 사용할 수 있습니다.  

* CentOS 6 및 7
* Microsoft Server Standard 2012, R2 2012 및 2016
* RedHat Enterprise Linux 6 및 7
* Ubuntu 14.04 및 16.04

가져오기는 100GB 디스크로 제한됩니다. filename.vhd-0.vhd 또는 filename.vmdk-0.vmdk 예제에 따라 이미지의 이름이 지정되어야 합니다.

## VHD로 이미지 변환
{: #convert-to-vhd}

VHD 및 VMDK 형식은 가상 서버에 지원되는 유일한 이미지 형식입니다. 이미지를 VMDK 이외의 형식에서 VHD로 변환하려면 다음 정보를 사용하십시오.

* Qemu-img 2.7.0 이상이 필요함
* 다음 명령으로 이미지 변환

  ```
  qemu-img convert -f <image format> <image name> -O vpc -o force_size <image name>
  ```
  {: pre}

* 예제 명령:

  ```
  qemu-img convert -f qcow2 test -O vpc -o force_size test
  ```
  {: pre}

자세한 정보는 QEMU 문서의 [이미지 형식 변환 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats)을
참조하십시오.

## ISO 템플리트
{: #iso-templates}

오직 {{site.data.keyword.BluSoftlayer_notm}} 지원 운영 체제만을 사용하여 ISO 템플리트를 VSI로 로드할 수 있습니다. 운영 체제 목록은
[https://www.ibm.com/cloud/server-software ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/server-software)에 있습니다.

이미지를 가져올 수 있으려면 이 도구를 사용하여 가져온 ISO가 부트 가능해야 합니다.

## {{site.data.keyword.virtualmachinesshort}}에 대한 이미지 구성
{: #config-image-vsi}

이미지가 {{site.data.keyword.BluSoftlayer_notm}} 인프라 환경에 성공적으로 배치될 수 있도록 하려면 다음 스펙에 따라 가상 서버 이미지를 구성해야 합니다.

* ***/boot***가 첫 번째 파티션이어야 함
* ***/boot*** 및 ***/***가 ext3 또는 ext4 파일 시스템이어야 함
* ***/etc*** 및 /root가 ***/***와 동일한 파티션에 있어야 함
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** 시스템에 연결된 스왑 디스크 마운트
* wget이 설치되어 있어야 함
* 최신 xe-guest-utilities Xen 도구가 설치되어 있어야 합니다. 다음 단계를 완료하십시오.

    1. Citrix([https://www.citrix.com/downloads/citrix-hypervisor/![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.citrix.com/downloads/citrix-hypervisor/))에서 XenServer ISO를 다운로드하십시오. 

    2. 다음 명령을 실행하여 ISO를 마운트하십시오.

        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}

    3. 다음 명령을 실행하여 ISO에 대한 RPM을 찾으십시오.

        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}

    4. 출력은 다음과 유사하게 RPM을 나열합니다.

        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}

     5. RPM을 복사한 후에 xentools에 대한 ISO를 추출할 수 있습니다. 다음 명령을 실행하여 ISO가 보관되는 디렉토리 구조를 작성하십시오.

        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}

     6. 결과 디렉토리 구조에서 *guest-tools* ISO를 마운트하고 xentools를 설치하기 위한 *rpm/debs*를 찾을 수 있습니다. 다음의 예제 디렉토리 구조를 참조하십시오.

        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}

cloud-init 사용 이미지에 대한 자세한 정보는 [cloud-init 사용 이미지로 프로비저닝](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#provisioning-with-a-cloud-init-enabled-image)을 참조하십시오.

## 암호화된 이미지 가져오기 준비
{: #preparing-to-import-an-encrypted-image}

자체 데이터 암호화 키로 암호화된 VHD 이미지를 가져오려면 [엔드 투 엔드(E2E) 암호화를 사용하여 암호화된 인스턴스 프로비저닝](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance)에 설명된 암호화 전제조건 및 지시사항을 완료했는지 확인하십시오.

vhd-util 도구를 사용하여 이미지를 암호화해야 하며, 이미지는 VHD 형식이어야 합니다. 자세한 정보는 [VHD 이미지 암호화](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image)를 참조하십시오. 
{: important}

## {{site.data.keyword.cos_full_notm}}에 이미지 업로드
{: #upload-to-ibm-cos}

이미지가 준비되면 {{site.data.keyword.cos_full_notm}}에 업로드할 수 있습니다. [지역 위치](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints)에서 버킷을 사용하십시오.

1. {{site.data.keyword.cos_full_notm}}에서 버킷으로 이동하고 **앱 오브젝트**를 클릭하여 이미지를 [업로드](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload)하십시오.
2. 최고 속도로 이미지를 업로드하려면 [Aspera](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera) 고속 전송 플러그인을 사용하십시오.

Java, Python 또는 NodeJS를 사용할 때 사용자 정의 애플리케이션에서 고속 전송을 시작하는 데 Aspera와 COS SDK를 사용할 수 있습니다. 자세한 정보는 [라이브러리와 SDK 사용](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera#aspera-sdk)을 참조하십시오.
{: tip}


## IBM Cloud Object Storage의 이미지 가져오기
{: #import-icos}

다음 단계를 완료하여 {{site.data.keyword.cos_full_notm}}에서 이미지를 가져오십시오.

1. 디바이스 메뉴로 이동하여 태스크를 완료하는 데 필요한 올바른 계정 권한을 가지고 있는지 확인하십시오. 

   * 콘솔의 디바이스 메뉴로 이동하십시오. 자세한 정보는 [디바이스로 이동](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)을 참조하십시오. 
   * 필요한 계정 권한 및 디바이스 액세스 권한을 가지고 있는지 확인하십시오. 계정 소유자(또는 **사용자 관리** 클래식 인프라 권한을 가진 사용자)만 권한을 조정할 수 있습니다. 

   권한에 대한 자세한 정보는 [클래식 인프라 권한](/docs/iam?topic=iam-    infrapermission#infrapermission) 및 [디바이스 액세스 관리](/docs/vsi?topic=virtual-servers-managing-device-access)를 참조하십시오. 

   암호화된 이미지를 가져오는 경우 {{site.data.keyword.cloud_notm}} 콘솔을 사용해야 합니다.
   {: important}
2. **디바이스 > 관리 > 이미지**를 선택하여 **이미지 템플리트** 페이지에 액세스하십시오. 
3. **IBM COS에서 이미지 가져오기** 탭을 클릭하여 가져오기 도구를 엽니다.
4. 필수 필드를 완료하십시오(표 1 참조).
5. {{site.data.keyword.cos_full_notm}}에서 가져오기를 완료하면 이미지 템플리트 페이지에 이미지가 표시됩니다.

| 필드 |값 |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | 가져오려는 이미지를 저장할 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 선택하십시오. |
| 위치 | 이미지가 저장된 특정 지역을 선택하십시오. 미국-남부(DAL13), 미국-동부(WDC07), EU-영국(LON02), EU-독일(FRA02), AP-일본 지역 및 연관된 데이터 센터에 이미지를 가져올 수 있습니다. 나열된 데이터 센터 중 하나에 이미지를 가져오고 나면 다른 데이터 센터로 이동할 수 있습니다. |
| 버킷 | 이미지를 저장할 {{site.data.keyword.cos_full_notm}} 버킷을 선택하십시오. 선택한 지역에 있는 버킷만 유효합니다. 선택한 위치에 없는 버킷을 선택하면 오류 메시지가 표시됩니다.|
| 이미지 파일 | {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에서 가져오려는 이미지 파일을 선택하십시오. 지원되는 파일 유형은 가상 하드 디스크(VHD), 가상 머신 디스크(VMDK) 및 ISO입니다. 암호화된 이미지를 가져오는 경우 이미지가 VHD 파일 형식이며 vhd-util 도구로 암호화되어야 합니다. |
| 이미지 이름 | 이미지의 설명적 이름을 지정하십시오. 가상 서버 인스턴스를 프로비저닝하는 데 사용할 이미지입니다. |
| API 키 | {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에 대한 액세스 권한을 제공하는 API 키를 지정하십시오. 암호화된 이미지를 가져올 때는 API 키가 키 관리 서비스 인스턴스에도 액세스할 수 있어야 합니다. API 키는 작성 시에만 복사하거나 다운로드할 수 있습니다. API 키가 유실되면 새 API 키를 작성해야 합니다. 자세한 정보는 [API 키에 관한 작업](/docs/iam?topic=iam-manapikey#manapikey)을 참조하십시오. |
| 운영 체제 | 이미지에 포함된 운영 체제를 선택하십시오. |
| Cloud-init | 가져올 이미지에 cloud-init가 사용된 경우 이 선택란을 선택하십시오. cloud-init 사용 Windows 운영 체제가 있는 이미지를 가져오는 중이고 이 옵션을 선택하는 경우 **사용자 라이센스**를 동시에 선택할 수 없습니다. 암호화된 이미지를 가져오는 경우 암호화된 이미지에 cloud-init이 사용되어야 하므로 이 옵션은 기본적으로 선택되며 편집할 수 없습니다. |
| 사용자 라이센스 | 고유 운영 체제 라이센스를 제공하려면 이 선택란을 선택하십시오. Windows 운영 체제가 있는 이미지를 가져오는 경우 이미지를 사용하여 [데디케이티드 호스트 인스턴스](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances)를 프로비저닝하려면 이 옵션을 선택할 수 있습니다. Windows 운영 체제 버전에서 고유 라이센스 사용을 지원하지 않으면 이 옵션은 사용되지 않습니다. Windows 이미지의 경우 고유 라이센스를 사용하도록 지정하면 Cloud init을 선택할 수 없습니다. Red Hat Enterprise Linux가 운영 체제로 포함된 암호화된 이미지를 가져오는 경우 암호화된 이미지에 고유 운영 체제 라이센스가 포함되어 있어야 하므로 이 옵션은 기본적으로 선택되며 편집할 수 없습니다. |
| 부트 모드 | 이미지의 부트 모드를 선택하십시오. 사용자가 지정하는 운영 체제의 기본 부트 모드가 설정되어 있으면 여기에서 부트 모드가 자동으로 선택됩니다. |
|참고 | 사용자에게 도움을 줄 수 있는 이미지와 관련된 참고사항을 추가하십시오. |
| 암호화 | vhd-util 도구를 사용하여 자체 데이터 암호화 키로 암호화한 이미지를 가져오는 경우 이 선택란을 선택하십시오. |
{: caption="표 1. IBM Cloud Object Storage에서 이미지를 가져오는 데 사용하는 값" caption-side="top"}

다음 표에서는 암호화된 이미지만 가져오는 데 적용할 수 있는 추가 필드를 보여줍니다. 암호화된 이미지에 관한 자세한 정보는 [엔드 투 엔드(E2E) 암호화를 사용하여 암호화된 인스턴스 프로비저닝](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance)을 참조하십시오.

<!--
To import an encrypted image, your account must have access to the End to End (E2E) Encryption feature. To enable your account for E2E Encryption, please contact [Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
{: tip}
-->

| 필드 |값 |
| ----- | ----- |
| 랩핑된 데이터 암호화 키 | 암호화된 이미지를 가져올 때 이미지를 암호화하는 데 사용한 데이터 암호화 키와 연관된 암호 텍스트를 지정하십시오. 자세한 정보는 [API를 사용하여 키 랩핑](/docs/services/key-protect?topic=key-protect-wrap-keys#api)을 참조하십시오. |
| 키 관리 서비스 인스턴스 | 드롭 다운 목록에서 사용자 계정의 키 관리 서비스 인스턴스를 선택하십시오. 키 관리 서비스 인스턴스는 데이터 암호화 키를 랩핑하는 데 사용한 고객 루트 키를 포함해야 합니다. |
| 키 이름 | 데이터 암호화 키를 랩핑하는 데 사용한 키 관리 서비스 인스턴스 내 루트 키의 이름을 선택하십시오. 자세한 정보는 [키 보기](/docs/services/key-protect?topic=key-protect-view-keys#view-keys)를 참조하십시오. |
{: caption="표 2. IBM Cloud Object Storage에서 암호화된 이미지를 가져오는 데 사용하는 값" caption-side="top"}

## 다음 단계

가져오기가 시작되고 나면 시스템에서 지정된 버킷의 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에서 이미지 파일을 찾습니다. 이미지 파일을 이미지 템플리트로 가져온 후에는
이미지 템플리트 페이지에서 액세스가 가능합니다. 가져오기가 완료되면 해당 이미지를 사용하여 새 디바이스를 주문하거나 기존 디바이스를 시작할 수 있습니다.
또한 이미지 템플리트는 언제든지 삭제할 수 있습니다. 이미지를 가져오는 시간은 파일 크기에 따라 다르지만, 대개 수 분에서 한 시간 정도 소요됩니다.

이미지를 이미지 템플리트 저장소에 가져오고 나면 {{site.data.keyword.cos_full_notm}}에서 삭제할 수 있습니다. 계속해서 **이미지 템플리트** 페이지에서 이미지 템플리트에 액세스하고 이를 사용하여 가상 서버 인스턴스를 프로비저닝할 수 있습니다. 
