---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-02"

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
{: #exporting-to-ibm-cos}

이미지 템플리트 페이지에서 이미지 템플리트를 [IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage) 계정으로 내보낼 수 있습니다.
{:shortdesc}

이미지 내보내기 프로세스는 기존의 개인용 표준 이미지 템플리트 또는 암호화된 이미지 템플리트를 가져와서 해당 이미지를
IBM Cloud Object Storage 계정의 지정된 위치에 저장된 이미지 파일로 변환합니다.

다음 단계를 사용하여 IBM Cloud Object Storage에 이미지 템플리트를 내보내십시오.

1. IBM Cloud Object Storage에 대한 쓰기 액세스 권한이 있는 서비스 ID로 {{site.data.keyword.slportal}}을 인증합니다.
2. [{{site.data.keyword.slportal_full}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)의 **디바이스** 메뉴에서 **관리 > 이미지**를 선택하십시오.
3. 내보낼 이미지 템플리트의 **조치**를 클릭하고 **IBM COS에 이미지 내보내기**를 선택하십시오. 원하는 구성의 이미지 템플리트를 아직 사용할 수 없는 경우에는
[이미지 템플리트 작성](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)을 참조하십시오. 
4. 필수 필드를 완료하십시오(표 1 참조).
5. **확인**을 클릭하여 IBM Cloud Object Storage 계정의 지정된 위치로 이미지를 내보내십시오. 

| 필드 |값 |
| ----- | ----- |
| 파일 이름 | 이미지의 파일 이름을 입력하십시오. |
| {{site.data.keyword.cos_full_notm}} | {{site.data.keyword.cos_full_notm}} 계정을 선택하십시오. |
| 위치 | 이미지 템플리트를 저장할 특정 지역을 선택하십시오. |
| 버킷 | 이미지 템플리트를 저장할 {{site.data.keyword.cos_full_notm}} 버킷을 선택하십시오. 선택한 지역에 있는 버킷만 유효합니다. 선택한 위치에 없는 버킷을 지정하면 오류 메시지가 표시됩니다.|
| API 키 | {{site.data.keyword.cos_full_notm}}에 대한 쓰기 액세스 권한이 있는 서비스 ID API 키를 지정하십시오. 자세한 정보는 [서비스 ID API 키 관리](/docs/iam?topic=iam-serviceidapikeys)를 참조하십시오. |
{: caption="표 1. IBM Cloud Object Storage에 이미지를 내보내는 데 사용하는 값" caption-side="top"}

## 다음 단계
각 이미지의 크기 및 특성이 서로 다르므로 내보내기 프로세스를 완료하려면 수 분이 걸릴 수 있습니다. 

이미지를 내보낸 후에 이미지는 이미지 템플리트의 목록에 그대로 남아있지만, 내보내기 프로세스 중에 지정된 IBM Cloud Object Storage 위치에 있는 파일로 이미지를 사용할 수도 있습니다. 

이미지 템플리트를 IBM Cloud Object Storage에 내보낼 때 각 블록 디바이스(또는 디스크)에는 연관된 고유 파일이 있습니다. 예를 들어 이미지 파일의 이름을 image.vhd로 지정하면 첫 번째 블록 디바이스를 image-0.vhd로 내보냅니다. 두 번째 블록 디바이스는 image-1.vhd로 내보내는 식입니다.
{: tip}
