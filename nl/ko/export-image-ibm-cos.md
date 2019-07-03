---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

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

# IBM Cloud Object Storage에 이미지 내보내기
{: #exporting-an-image-to-ibm-cloud-object-storage}

이미지 템플리트 페이지에서 이미지 템플리트를 [{{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about) 계정에 내보낼 수 있습니다.
{:shortdesc}

이미지 내보내기 프로세스는 기존의 개인용 표준 이미지 템플리트 또는 암호화된 이미지 템플리트를 가져와서 해당 이미지를 {{site.data.keyword.cos_full_notm}} 계정의 지정된 위치에 저장되는 이미지 파일로 변환합니다. 

*참고:* VMDK 이미지를 가져온 경우 해당 이미지를 VHD 또는 VMDK 형식으로 내보낼 수 있습니다. 이미지 형식 간의 차이로 인해 데이터가 손실될 가능성이 있습니다. 데이터 손실이 발생하는 경우 데이터를 보호하기 위해 원래 VHD 파일이 보존됩니다.

## 시작하기 전에
먼저 디바이스 메뉴로 이동하여 태스크를 완료하는 데 필요한 올바른 계정 권한을 가지고 있는지 확인하십시오. 

* 콘솔의 디바이스 메뉴로 이동하십시오. 자세한 정보는 [디바이스로 이동](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)을 참조하십시오. 
* {{site.data.keyword.cos_full_notm}}에 대한 쓰기 액세스 권한이 있는지 확인하십시오. 계정 소유자(또는 **사용자 관리** 클래식 인프라 권한을 가진 사용자)만 권한을 조정할 수 있습니다. 

권한에 대한 자세한 정보는 [클래식 인프라 권한](/docs/iam?topic=iam-infrapermission#infrapermission) 및 [디바이스 액세스 관리](/docs/vsi?topic=virtual-servers-managing-device-access)를 참조하십시오. 

이 이미지 템플리트를 {{site.data.keyword.cos_full_notm}}에 내보낼 계획인 경우에는 웹 주소에서 문제가 될 수 있는 문자가 이름에 포함되지 않게 하십시오. 예를 들어, URL 인코딩되지 않은 경우 ?, =, < 및 기타 특수 문자를 사용하면 원하지 않는 작동이 발생할 수 있습니다.
{: tip}

## IBM Cloud Object Storage에 이미지 내보내기

다음 단계를 사용하여 IBM Cloud Object Storage에 이미지 템플리트를 내보내십시오.

1. **디바이스** 메뉴에서 **관리 > 이미지**를 선택하십시오. 
2. 내보낼 이미지 템플리트의 **조치**를 클릭하고 **IBM COS에 이미지 내보내기**를 선택하십시오. 원하는 구성의 이미지 템플리트를 아직 사용할 수 없는 경우에는
[이미지 템플리트 작성](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template)을 참조하십시오.
3. 필수 필드를 완료하십시오(표 1 참조).
4. **확인**을 클릭하여 {{site.data.keyword.cos_full_notm}} 계정의 지정된 위치로 이미지를 내보내십시오. 

| 필드 |값 |
| ----- | ----- |
| 파일 이름 | 이미지의 파일 이름을 입력하십시오. |
| {{site.data.keyword.cos_full_notm}} | {{site.data.keyword.cos_full_notm}} 계정을 선택하십시오. |
| 위치 | 이미지 템플리트를 저장할 특정 지역을 선택하십시오. |
| 버킷 | 이미지 템플리트를 저장할 {{site.data.keyword.cos_full_notm}} 버킷을 선택하십시오. 선택한 지역에 있는 버킷만 유효합니다. 선택한 위치에 없는 버킷을 지정하면 오류 메시지가 표시됩니다. |
| API 키 | {{site.data.keyword.cos_full_notm}}에 대한 쓰기 액세스 권한이 있는 서비스 ID API 키를 지정하십시오. 자세한 정보는 [서비스 ID API 키 관리](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys)를 참조하십시오. |
{: caption="표 1. {{site.data.keyword.cos_full_notm}}에 이미지를 내보내는 데 사용하는 값" caption-side="top"}
| 대상 파일 유형 | 내보낼 파일 유형을 선택하십시오. VMDK 이미지를 가져온 경우 해당 이미지를 VHD 또는 VMDK 형식으로 내보내는 옵션이 있습니다. 파일이 원래 VHD 형식이면 VHD로만 내보낼 수 있습니다. |

## 다음 단계
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
각 이미지의 크기 및 특성이 서로 다르므로 내보내기 프로세스를 완료하려면 수 분이 걸릴 수 있습니다.

이미지를 내보낸 후 해당 이미지는 이미지 템플리트의 목록에 남아 있지만 해당 이미지를 내보내기 프로세스 중에 지정된 {{site.data.keyword.cos_full_notm}} 위치에 있는 파일로 사용할 수도 있습니다. {{site.data.keyword.cos_full_notm}}에서 이미지 파일을 다운로드할 수 있습니다. {{site.data.keyword.cloud_notm}} 콘솔의 리소스 목록에서 Cloud Object Storage 서비스 인스턴스에 액세스하십시오. 이미지가 저장되는 버킷에서 다운로드할 이미지 파일을 선택한 후 **오브젝트 다운로드**를 선택하십시오. 

이미지 템플리트를 IBM Cloud Object Storage에 내보낼 때 각 블록 디바이스(또는 디스크)에는 연관된 고유 파일이 있습니다. 예를 들어 이미지 파일의 이름을 image.vhd로 지정하면 첫 번째 블록 디바이스를 image-0.vhd로 내보냅니다. 두 번째 블록 디바이스는 image-1.vhd로 내보내는 식입니다.
{: tip}
