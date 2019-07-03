---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}


# 자체 OS 라이센스 또는 구독 사용
{: #using-your-own-os-license-or-subscription}

VHD 이미지로 이미지 템플리트를 작성하는 경우 [Red Hat Cloud Access![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) 구독을 통한 자체 RHEL 운영 체제 라이센스 또는
Microsoft Enterprise Agreement를 통한 Windows 라이센스를 제공하도록 선택할 수 있습니다.
{:shortdesc}

자체 라이센스를 사용 중임을 표시하는 이미지를 {{site.data.keyword.cloud}}에 배치하는 경우에는 다음의 지원 조항이 존재합니다.
* {{site.data.keyword.IBM_notm}}에서는 하이퍼바이저, 인스턴스 프로비저닝, 이미지 가져오기, 이미지 재부팅, OS 재로드 및 이미지 캡처에 대한 지원을 제공합니다.
* 운영 체제 라이센스의 구입처인 회사는 이미지 자체에 대한 지원을 제공합니다. {{site.data.keyword.IBM_notm}}에서는 이미지에 대한 지원을 제공하지 않습니다.

이미지에 대한 자체 라이센스를 제공하는 경우에는 다음의 제한사항이 이미지에 적용됩니다.
* 이미지는 개인용 이미지입니다. 이는 공용으로 공유될 수 없습니다.
* 이미지에는 소프트웨어 추가 기능이 포함될 수 없습니다. 추가적인 소프트웨어는 이미지가 프로비저닝된 후에 추가되어야 합니다.

## Red Hat Cloud Access 사용
Red Hat Enterprise Linux 클라우드 제공자로서의 인증에 대한 자세한 정보는 [IaaS(Infrastructure as a Service) ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://access.redhat.com/ecosystem/cloud-provider/2262101)를 참조하십시오.

## 자체 Windows 라이센스 사용
다음과 같은 운영 체제가 지원됩니다.
* Windows Server 2016
* Windows Server 2012
* Windows Server 2012 R2

보고 요구사항 파악 또는 기존 Windows 라이센스 적격성에 대해 질문이 있으면 Microsoft 담당자에게 문의하십시오. 자체 Windows 라이센스를 사용 중임을 지정하는 이미지 템플리트를 작성하는 경우에는 전용 호스트에서 해당 이미지를 프로비저닝해야 합니다. 자체 Windows 라이센스를 사용 중임을 표시하는 이미지를 사용하는 경우에는 호스트에 자동 지정된 전용 인스턴스 또는 공용 인스턴스를 프로비저닝할 수 없습니다. 또한 자체 라이센스를 사용 중임을 지정하는 Windows 이미지 템플리트를 작성하거나 업데이트하는 경우 Windows 이미지는 cloud-init 이미지가 될 수 없습니다.

## 시작하기 전에
먼저 디바이스 메뉴로 이동하여 태스크를 완료하는 데 필요한 올바른 계정 권한을 가지고 있는지 확인하십시오. 

* 콘솔의 디바이스 메뉴로 이동하십시오. 자세한 정보는 [디바이스로 이동](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)을 참조하십시오. 
* 필요한 계정 권한 및 디바이스 액세스 권한을 가지고 있는지 확인하십시오. 계정 소유자(또는 **사용자 관리** 클래식 인프라 권한을 가진 사용자)만 권한을 조정할 수 있습니다. 

권한에 대한 자세한 정보는 [클래식 인프라 권한](/docs/iam?topic=iam-infrapermission#infrapermission) 및 [디바이스 액세스 관리](/docs/vsi?topic=virtual-servers-managing-device-access)를 참조하십시오. 

## 자체 라이센스를 지정하는 이미지 가져오기

VHD 이미지를 가져온 후에는 운영 체제에 대해 자체 라이센스 또는 구독을 제공 중임을 지정할 수 있습니다.

이미지 템플리트의 이미지 가져오기 페이지에 액세스하고 자체 라이센스 또는 구독을 사용할 VHD 이미지를 표시하려면 다음 단계를 완료하십시오.
1. **디바이스** 메뉴에서 **관리 > 이미지**를 선택하십시오.
2. **이미지 가져오기** 탭을 클릭하십시오.
3. VHD 이미지 가져오기에 대한 필수 정보를 채우고, **운영 체제** 드롭 다운 상자 근처에 표시된
**라이센스** 선택란을 선택하십시오. 이미지 가져오기에 대한 자세한 정보는 [이미지 준비 및 가져오기](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images)를 참조하십시오.

## 사용자 제공된 OS 라이센스 지정을 위한 이미지 템플리트 업데이트

기존 VHD 이미지 템플리트가 있는 경우에는 운영 체제에 대한 자체 라이센스 또는 구독을 제공하고자 함을 지정할 수 있습니다.

이미지 템플리트에 액세스하고, 기존의 자체 라이센스 또는 구독을 사용함을 지정하려면 다음 단계를 완료하십시오.
1. **디바이스** 메뉴에서 **관리 > 이미지**를 선택하십시오.
2. 템플리트의 목록에서 업데이트할 이미지 템플리트 이름을 클릭하십시오.
3. 이미지 템플리트 세부사항 페이지의 **OS 라이센스** 표제 아래에서 **사용자 제공됨** 선택란을 선택하고, **업데이트**를 클릭하십시오.
