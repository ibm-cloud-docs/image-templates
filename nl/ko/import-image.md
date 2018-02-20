---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-31"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# 이미지 준비 및 가져오기

[고객 포털 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)의 이미지 템플리트 화면을 사용하여 사용자는 Swift 기반 [Object Storage](/docs/infrastructure/objectstorage-swift/index.html) 계정에서 기존 이미지를 업로드할 수 있습니다.
{:shortdesc}

이미지를 이미지 템플리트로 가져온 후에는 이를 사용하여 기존 가상 서버를 프로비저닝하거나 시작할 수 있습니다. Object Storage 계정에서 가져온 이미지는 VHD 또는 사용자 정의 ISO일 수 있습니다. VHD 가져오기는 다음 64비트 운영 체제에서만 사용할 수 있습니다.

* CentOS 6 및 7
* RedHat Enterprise Linux 6 및 7
* Ubuntu 14.04 및 16.04
* Microsoft Server Standard 2012, R2 2012 및 2016

VHD 가져오기는 100GB 디스크로 제한됩니다. VHD는 다음 예에 따라 이름이 지정되어야 합니다. 예: filename.vhd-0.vhd.

## VHD로 이미지 변환

VHD 형식은 {{site.data.keyword.BluVirtServers_full}}에 대한 유일한 지원 이미지 형식입니다. 이미지를 VHD로 변환하려면 다음 정보를 사용하십시오. 

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

오직 {{site.data.keyword.BluSoftlayer_notm}} 지원 운영 체제만을 사용하여 ISO 템플리트를 VSI로 로드할 수 있습니다. 지원 운영 체제의 목록은
다음에서 찾을 수 있습니다. [http://www.softlayer.com/services/software/ ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.softlayer.com/services/software/)

이미지를 가져올 수 있으려면 이 도구를 사용하여 가져온 ISO가 부트 가능해야 합니다. 

## {{site.data.keyword.virtualmachinesshort}}에 대한 이미지 구성

이미지가 {{site.data.keyword.BluSoftlayer_notm}} 인프라 환경에 성공적으로 배치될 수 있도록 하려면 다음 스펙에 따라 가상 서버 이미지를 구성해야 합니다. 

* ***/boot***가 첫 번째 파티션이어야 함
* ***/boot*** 및 ***/***가 ext3 또는 ext4 파일 시스템이어야 함
* ***/etc*** 및 /root가 ***/***와 동일한 파티션에 있어야 함
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** 시스템에 연결된 스왑 디스크 마운트
* wget이 설치되어 있어야 함
* 최신 xe-guest-utilities Xen 도구가 설치되어 있어야 합니다. 다음 단계를 완료하십시오. 
    
    1. Citrix에서 XenServer ISO를 다운로드하십시오. [https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html)
    
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
    
cloud-init 사용 이미지에 대한 자세한 정보는 [cloud-init 사용 이미지로 프로비저닝](image_cloud-init.html)을 참조하십시오. 

## 이미지 가져오기

고객 포털의 이미지를 가져오려면 다음 단계를 완료하십시오. 

1. {{site.data.keyword.objectstorageshort}} 계정에서 이미지에 대한 다음 세부사항을 찾아서 기록하십시오. 자세한 정보는 [Object Storage 파일 세부사항 보기 및 편집](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html)을 참조하십시오. 
  * 계정 이름
  * 클러스터
  * 컨테이너
  * 이미지 파일 이름
2. [고객 포털 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)에서 **디바이스 > 관리 > 이미지**를 선택하여 **이미지 템플리트** 페이지에 액세스하십시오. 
3. **이미지 가져오기** 탭을 클릭하여 가져오기 도구를 여십시오. 
4. **계정** 드롭 다운 목록에서 가져올 이미지에 대해 **{{site.data.keyword.objectstorageshort}} 계정**을 선택하십시오. 
5. **클러스터** 드롭 다운 목록에서 가져올 이미지에 대해 **{{site.data.keyword.objectstorageshort}} 클러스터**를 선택하십시오. 
6. **컨테이너** 드롭 다운 목록에서 가져올 이미지에 대해 **{{site.data.keyword.objectstorageshort}} 컨테이너**를 선택하십시오. 
7. **이미지 파일** 드롭 다운 목록에서 {{site.data.keyword.objectstorageshort}}에 나열된 **이미지 파일 이름**을 선택하십시오. 
8. **이미지 이름** 필드에 새 이미지 템플리트에 대한 이미지 이름을 입력하십시오. 
9. **참고** 텍스트 상자에 적용 가능한 참고사항을 입력하십시오. 
10. **운영 체제** 드롭 다운 목록에서 이미지의 운영 체제를 선택하십시오. 

  가져올 이미지가 사용자 정의 ISO인 경우에는 운영 체제 드롭 다운 목록이 사용되지 않습니다. 이 단계는 가져오기에 VHD가 포함된 경우에만 필요합니다.
  {:tip}

11. 가져오는 이미지가 cloud-init 사용 이미지인 경우에는 **Cloud Init** 선택란을 선택하십시오. 자세한 정보는 [cloud-init 사용 이미지로 프로비저닝](image_cloud-init.html)을 참조하십시오.         
12. 자체 운영 체제 라이센스를 제공하려면 **라이센스** 선택란을 선택하십시오. 자세한 정보는 [Red Hat Cloud Access 사용](use-red-hat-cloud-access.html)을 참조하십시오. 
13. **가져오기**를 클릭하여 이미지 템플리트 화면으로 이미지를 가져오십시오. 조치를 취소하려면 **취소**를 클릭하십시오. 

## 다음 단계

가져오기가 시작된 후에 시스템은 특정 경로(계정 > 클러스터 > 컨테이너 > 이미지 파일)를
사용하여 {{site.data.keyword.objectstorageshort}} 계정에서 이미지 파일을 찾습니다. 이미지 파일을 이미지 템플리트로 가져온 후에는
이미지 템플리트 페이지에서 액세스가 가능합니다. 가져오기가 완료되면 해당 이미지를 사용하여 새 디바이스를 주문하거나 기존 디바이스를 시작할 수 있습니다.
또한 이미지는 언제든지 삭제가 가능합니다. 이미지를 가져오는 시간은 파일 크기에 따라 다르지만, 대개 수 분에서 한 시간 정도 소요됩니다. 

