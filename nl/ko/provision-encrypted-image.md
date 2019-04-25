---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-27"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# 엔드 투 엔드(E2E) 암호화를 사용하여 암호화된 인스턴스 프로비저닝
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

엔드 투 엔드(E2E) 암호화 기능을 사용하면 사용자가 소유하고 제어하는 데이터 암호화 키를 사용하여 암호화한 고유한 cloud-init 사용 운영 체제 이미지를 가져올 수 있습니다. 일부 환경 설정을 완료하고 나면 암호화된 이미지를 이미지 템플리트 저장소에 가져와서 암호화된 가상 서버 인스턴스를 프로비저닝하는 데 사용할 수 있습니다. E2E 암호화에서는 프로비저닝된 가상 서버 인스턴스와 연관된 스토리지의 저장 데이터 암호화를 제공합니다.
{:shortdesc}

E2E 암호화에서는 여러 {{site.data.keyword.cloud}} 컴포넌트를 결합하여 중요한 정보의 보안 솔루션을 제공합니다.

* {{site.data.keyword.keymanagementservicefull_notm}}에서는 정보 도용을 보호하는 FIPS 140-2 레벨 2 인증 클라우드 기반 하드웨어 보안 모듈(HSM)을 통해 키를 보호합니다.
* IBM IAM({{site.data.keyword.iamshort}})을 사용하면 Cloud Block Storage 서비스를 통해 {{site.data.keyword.keymanagementserviceshort}} 및 데이터 암호화 키를 랩핑하는 데 사용하는 루트 키에 액세스할 수 있습니다.
* {{site.data.keyword.cos_full_notm}}에서는 암호화된 이미지를 업로드할 때 해당 이미지를 안전하게 저장합니다.
* {{site.data.keyword.cloud_notm}} 콘솔에서 암호화된 이미지를 가져오고 이미지 템플리트를 작성할 수 있습니다.
* {{site.data.keyword.cloud_notm}} 콘솔 인프라 환경에서 사용 가능한 암호화된 이미지 템플리트를 사용하면 암호화된 가상 서버 인스턴스를 프로비저닝할 수 있습니다.
* 마지막으로 [활동 트래커](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov)를 통해 암호화된 가상 서버와 연관된 이벤트를 감사하는 옵션이 있습니다.

## 환경 준비

1. 가상 서버에 E2E 암호화를 사용하려면 업그레이드된 계정이 있어야 합니다. 자세한 정보는 [IBM ID로 전환 및 계정 연결](/docs/account/softlayerlink.html)을 참조하십시오.
2. {{site.data.keyword.keymanagementservicefull_notm}}를 사용하여 키를 작성하고 관리하십시오.
      1. [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision) 서비스를 프로비저닝하십시오.
      2. {{site.data.keyword.keymanagementservicelong_notm}}에서 루트 키(CRK)를 [작성](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys)하거나 [가져오기](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys)를 수행하십시오.
      3. **선택사항**: 선택하는 경우 복호화할 표준 키를 [작성](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys)하거나 [가져오기](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys)를 수행할 수 있습니다.
      4. VHD 이미지를 암호화하는 데 사용할 데이터 암호화 키를 랩핑할 수 있도록 [ Key Protect API를 설정](/docs/services/key-protect?topic=key-protect-set-up-api#set-up-api)하십시오.
      5. 루트 키로 [데이터 암호화 키를 랩핑](/docs/services/key-protect/wrap-keys.html#wrap-keys)하십시오. 암호화된 이미지를 {{site.data.keyword.cloud_notm}} 콘솔에 가져올 때 랩핑된 데이터 암호화 키(WDEK)와 연관된 암호 텍스트가 필요합니다.
3. IBM IAM({{site.data.keyword.iamshort}})에서 **Cloud Block Storage**(소스 서비스)와 **{{site.data.keyword.keymanagementservicelong_notm}}**(대상 서비스) 사이의 [액세스 권한을 부여](/docs/iam?topic=iam-serviceauth#create-auth)하십시오. {{site.data.keyword.cos_full_notm}}에서 암호화된 이미지를 가져오는 사용자는 IAM에서 {{site.data.keyword.keymanagementservicelong_notm}}용으로 [정의된 액세스 정책](/docs/iam?topic=iam-userroles#userroles)이 있어야 합니다.
4. IBM Cloud Console에서 {{site.data.keyword.cos_full_notm}}의 인스턴스를 작성하고 데이터를 저장할 버킷을 작성하십시오. 자세한 정보는 [{{site.data.keyword.cos_full_notm}} 시작하기 튜토리얼](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial)을 참조하십시오.
      1. {{site.data.keyword.keymanagementserviceshort}} 서비스가 프로비저닝된 위치와 동일한 지역 위치에 {{site.data.keyword.cos_full_notm}} 인스턴스를 작성하십시오.
      2. 버킷을 작성할 때 **복원성** 설정은 _지역_이어야 합니다.
      3. 선택적으로 버킷을 작성할 때 [키를 사용하여 암호화](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-sse-kp#sse-kp)할 수 있습니다.   

## 암호화된 이미지 준비

1. 암호화할 {{site.data.keyword.cloud_notm}} 인프라 환경에서 작동하는 암호화되지 않은 이미지를 선택하십시오. 한 가지 옵션은 기존 가상 서버를 사용하여 [이미지 템플리트를 작성](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template)하는 것입니다. 자세한 정보는 [cloud-init 프로비저닝된 가상 서버에서 작성한 이미지 템플리트로 작업](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server)을 참조하십시오. 기존 VHD 이미지도 사용할 수 있습니다. 이미지가 [암호화된 이미지 요구사항](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs)을 충족하는지 확인하십시오.
2. {{site.data.keyword.slportal}}의 이미지 템플리트를 사용하는 경우 {{site.data.keyword.cos_full_notm}}에 [암호화되지 않은 이미지 내보내기](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage)를 수행하십시오.
3. 이미지를 암호화하려면 {{site.data.keyword.cos_full_notm}}에서 안전한 로컬 시스템에 이미지를 다운로드하십시오. 서비스 대시보드에서 **다운로드** 조치를 선택하여 스토리지에서 오브젝트를 검색하십시오. Aspera 고속 전송 플러그인을 사용하여 200MB보다 큰 이미지를 다운로드할 수 있습니다.
4. vhd-util 도구를 사용하여 [VHD 이미지를 암호화](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image)하십시오.
5. {{site.data.keyword.cos_full_notm}}에서 버킷으로 이동하고 **오브젝트 추가**를 클릭하여 암호화된 이미지를 [업로드](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload-data#upload-data)하십시오. Aspera 고속 전송 플러그인을 사용하여 200MB보다 큰 이미지를 업로드할 수 있습니다.

## 암호화된 이미지 가져오기 및 인스턴스 주문

1. IBM IAM({{site.data.keyword.iamshort}})을 사용하여 암호화된 이미지를 {{site.data.keyword.cloud_notm}} 콘솔에 가져올 때 인증할 서비스 ID를 작성하십시오.
      1. [서비스 ID](/docs/iam?topic=iam-serviceids#serviceids)를 작성하십시오.
      2. [액세스 정책](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy)을 지정하십시오. {{site.data.keyword.cos_full_notm}} 및 {{site.data.keyword.keymanagementservicelong_notm}} 서비스의 액세스를 지정하십시오.
      3. [서비스 ID의 API 키](/docs/iam?topic=iam-serviceidapikeys#create_service_key)를 작성하십시오.
      4. 자세한 정보는 [{{site.data.keyword.cloud_notm}} IAM 서비스 ID 및 API 키 소개![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window}를 참조하십시오.
2. {{site.data.keyword.cloud_notm}} 콘솔에서 이미지 템플리트 페이지로 [암호화된 이미지를 가져오십시오](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos).
3. 이미지 템플리트 페이지에서 암호화된 이미지를 사용하여 가상 서버 인스턴스를 [주문](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template)할 수 있습니다.
4. 암호화된 가상 서버를 프로비저닝하면 [활동 트래커](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov)를 통해 [가상 서버 이벤트](/docs/vsi?topic=virtual-servers-at_events#at_events)를 감사하는 옵션이 있습니다.
