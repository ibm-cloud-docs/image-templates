---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-23"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 이미지 템플리트 API 참조
{: #image-template-api-reference}

{{site.data.keyword.slapi_full}}는 개발자와 시스템 관리자가 {{site.data.keyword.cloud}} 백엔드 시스템과 직접 상호작용하는 데 사용할 수 있는 개발 인터페이스입니다. {{site.data.keyword.slapi_short}}에서는 {{site.data.keyword.slportal_full}}의
많은 기능을 제공합니다. 즉, 일반적으로 {{site.data.keyword.slportal}}에서 상호작용이 가능하면 API에서도
실행할 수 있다는 의미입니다. API에서 {{site.data.keyword.BluSoftlayer_full}} 환경의 모든 부분과 프로그래밍 방식으로
상호작용할 수 있으므로 {{site.data.keyword.slapi_short}}를 사용하면 태스크를 자동화할 수 있습니다. 예를 들어
*SoftLayer_Virtual_Guest/createObject* API를 사용하여 이미지 템플리트에서 Virtual Server 인스턴스를 배치할 수 있습니다.
{:shortdesc}

{{site.data.keyword.slapi_short}}는 원격 프로시저 호출(RPC) 시스템입니다. 각 호출에는 API 엔드포인트로 데이터를 보내고
구조화된 데이터를 받는 과정이 포함됩니다. {{site.data.keyword.slapi_short}}를 사용하여 데이터를 송수신하는 데 사용하는
형식은 선택하는 API 구현에 따라 달라집니다. 현재 {{site.data.keyword.slapi_short}}에서는 데이터 전송에 SOAP, XML-RPC 또는 REST를 사용합니다.

{{site.data.keyword.slapi_short}} 및 Virtual Server API에 관한 자세한 정보는 {{site.data.keyword.sldn_full}}의
다음 리소스를 참조하십시오.
* [{{site.data.keyword.slapi_short}} 개요 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/reference/softlayerapi/){: new_window}
* [{{site.data.keyword.slapi_short}} 시작하기 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/article/getting-started/){: new_window}
* [API 참조: SoftLayer_Virtual_Guest::createObject ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest/createObject/){: new_window}
* [API 참조: SoftLayer_Account::getBlockDeviceTemplateGroups ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/reference/services/SoftLayer_Account/getBlockDeviceTemplateGroups/){: new_window}
* [API 참조: SoftLayer_Virtual_Guest_Block_Device_Template_Group::getPublicImages ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getPublicImages/){: new_window}

API 사용 예는 다음 리소스를 참조하십시오.
* [{{site.data.keyword.slapi_short}}를 사용하여 이미지 템플리트에서 Virtual Server를 작성하는 방법 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://stackoverflow.com/questions/41138874/how-to-create-virtual-server-using-standard-template-softlayer-using-rest-api){: new_window}
* [{{site.data.keyword.slapi_short}} Python 클라이언트: Virtual Server 작업 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://softlayer-python.readthedocs.io/en/latest/cli/vs.html){: new_window}
* [{{site.data.keyword.slapi_short}} Python 클라이언트: SoftLayer.image ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer-api-python-client.readthedocs.io/en/latest/api/managers/image/){: new_window}
