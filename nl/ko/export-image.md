---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# OpenStack Swift에 이미지 내보내기
{: #exporting-an-image-to-openstack-swift}

이미지 템플리트 페이지에서 이미지 템플리트를 [Object Storage OpenStack Swift](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-GettingStarted#getting-started-with-object-storage-openstack-swift) 계정에 내보낼 수 있습니다.
{:shortdesc}

이미지 내보내기 프로세스는 기존의 개인용 표준 이미지 템플리트를 가져와서 해당 이미지를 Object Storage
OpenStack Swift 계정의 지정된 위치에 저장된 이미지 파일로 변환합니다. 이미지 템플리트를 내보내려면 다음 단계를 수행하십시오.

1. [{{site.data.keyword.slportal_full}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)의 **디바이스** 메뉴에서 **관리 > 이미지**를 선택하십시오.
2. 내보낼 이미지 템플리트에 대한 **조치**를 클릭하고 **이미지 내보내기**를 선택하십시오. 원하는 구성의 이미지 템플리트를 아직 사용할 수 없는 경우에는
[이미지 템플리트 작성](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)을 참조하십시오.
3. 이미지 내보내기 페이지의 **파일 이름** 필드에 이미지의 파일 이름을 입력하십시오.
5. **계정** 드롭 다운 목록에서 **Object Storage 계정**을 선택하십시오.
6. **클러스터** 드롭 다운 목록에서 **Object Storage 클러스터**를 선택하십시오.
7. **컨테이너** 드롭 다운 목록에서 **Object Storage 컨테이너**를 선택하십시오.
8. **이미지 내보내기**를 클릭하여 Object Storage 계정의 지정된 위치로 이미지를 내보내십시오. 조치를 취소하려면 **닫기**를
클릭하십시오.

## 다음 단계

이미지를 내보낸 후에 이미지는 이미지 템플리트의 목록에 그대로 남아있지만, 내보내기 프로세스 중에 지정된 Object Storage OpenStack Swift 위치에 있는 파일로 이미지를 사용할 수도 있습니다. Object Storage 계정으로 내보낸 파일 보기에 대한 자세한 정보는
[파일 세부사항 보기 및 편집](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-OSSSLPortal#viewing-and-editing-file-details)을 참조하십시오. 각 이미지의 크기 및 특성이 서로 다르므로 내보내기 프로세스를 완료하려면
수 분이 걸릴 수 있습니다. 평균 내보내기 속도는 2GB/분입니다. 수 분이 경과한 후에도 Object Storage OpenStack Swift 계정에서 이미지를 사용할 수 없는 경우에는
[지원 팀에 문의](/docs/get-support?topic=get-support-getting-customer-support)하십시오.
