---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# cloud-init 사용 이미지로 프로비저닝

가상 서버를 주문하는 경우 대부분의 운영 체제는 이제 cloud-init 사용 이미지를 사용하여
프로비저닝 시간을 최적화합니다. cloud-init에 대해 사용 가능한, 사용자 정의된 이미지를 가져올 수도 있습니다.
{:shortdesc}

다음 운영 체제는 이제 추가 기능 없이 Virtual Server를 주문할 때 cloud-init 사용 이미지로 기본값이 지정됩니다. (추가 기능에는 추가 소프트웨어, 사후 프로비저닝 스크립트 및 고급 모니터링이 포함됩니다.)
* CentOS 7
* Debian 8, 9
* Ubuntu 16.04, 18.04
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

cloud-init 사용 운영 체제의 가상 서버를 주문하는 경우에는 사용자 정의 프로비저닝 스크립트의 사용자 데이터 또는 메타데이터를 추가할 수 있습니다. 주문 양식의 사용자 데이터 필드에 서버에 대한 선택적 cloud-init 사용자 데이터 또는 선택적 메타데이터를 입력하십시오.

## 사용자 정의된 cloud-init 사용 이미지 가져오기

사용자 정의된 cloud-init 사용 이미지를 작성한 경우에는 {{site.data.keyword.slportal_full}}의 이미지 가져오기 페이지에서 해당 이미지를
cloud-init 이미지로 지정할 수 있습니다. 

이미지 템플리트의 이미지 가져오기 페이지에 액세스하여 cloud-init 사용으로 이미지를 표시하려면 다음 단계를 완료하십시오.
1. **디바이스** 메뉴에서 **관리** > **이미지**를 선택하십시오.
2. **이미지 가져오기** 탭을 클릭하십시오.
3. cloud-init 사용 이미지 가져오기에 대한 필수 정보를 채우고 **운영 체제** 드롭 다운 상자 근처에 표시된
**Cloud init** 선택란을 선택하십시오. 이미지 가져오기에 대한 자세한 정보는 이미지 가져오기를 참조하십시오.

## 이미지 템플리트를 cloud-init 사용으로 표시

기존 cloud-init 사용 VHD 이미지 템플리트가 있는 경우에는 이미지 템플리트의 세부사항 페이지에서 이를 cloud-init 사용으로
지정할 수 있습니다.

이미지 템플리트에 액세스하고 이를 cloud-init 사용으로 표시하려면 다음 단계를 완료하십시오.
1. **디바이스** 메뉴에서 **관리** > **이미지**를 선택하십시오.
2. 템플리트의 목록에서 업데이트할 이미지 템플리트 이름을 클릭하십시오.
3. 이미지 템플리트 세부사항 페이지의 **Cloud Init** 표제 아래에서 **사용** 선택란을 선택하고, **업데이트**를 클릭하십시오.

## cloud-init 프로비저닝된 Virtual Server에서 작성된 이미지 템플리트 작업

Cloud-init는 일반적으로 한 번만 실행됩니다. 그러나 cloud-init 사용 이미지에서 Virtual Server를
프로비저닝한 후에 해당 Virtual Server에서 이미지 템플리트를 작성하는 경우에는 UUID가 기록됩니다. 해당 이미지 템플리트를 사용하여
다른 Virtual Server를 작성하는 경우에는 cloud-init이 다시 실행됩니다.

## cloud-init 사용 이미지 템플리트 작성

이미지 구성에 대한 정보는
[cloud-init 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloudinit.readthedocs.io/en/latest/)를 참조하십시오.

데이터 소스에 대한 정보는 [데이터 소스 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html)를 참조하십시오. {{site.data.keyword.cloud_notm}} cloud-init 이미지는 메타데이터를 제공하기 위해 [구성 드라이브
![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - 버전 2 데이터 소스를 사용하여 환경에 맞게 작성됩니다.

### Linux 요구사항
* Cloud-init 버전 0.7.7 이상

### Windows 요구사항
* 공용 및 사설 네트워크에 대한 Cloudbase-init 메타데이터 서비스는 {{site.data.keyword.cloud_notm}} 인프라에서 지원됩니다. 또한 이 서비스는 Windows 가상 서버 인증 정보로 고객 포털을 업데이트합니다. 
[https://github.com/softlayer/bluemix-cloudbase-init ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/softlayer/bluemix-cloudbase-init)에서 해당 서비스에 액세스할 수 있습니다.
* 사용자 환경에서 Vyatta를 사용하는 경우에는 API 로드 밸런서에 대한 API 호출을 허용하도록 Vyatta를 구성해야 합니다. 자세한 정보는 [File Storage가 있는 VMware 환경의 Brocade vRouter(Vyatta) 설정 안내서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/infrastructure/FileStorage?topic=FileStorage-configureVyatta#setting-up-brocade-vrouter-vyatta-for-vmware-environments-with-file-storage)를 참조하십시오.
