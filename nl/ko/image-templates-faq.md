---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-30"

subcollection: image-templates

---


{:new_window: target="_blank"}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}

# FAQ: 이미지 템플리트

## 표준 이미지 템플리트란 무엇입니까?
{: faq}

표준 이미지 템플리트는 {{site.data.keyword.BluSoftlayer_notm}}의 {{site.data.keyword.virtualmachineslong}} 이미지 작성 옵션입니다.
표준 이미지 템플리트를 사용하여 사용자는 해당 운영 체제와 무관하게 기존 가상 서버의 이미지를 캡처하고, 해당 이미지 기반의
새 가상 서버를 작성할 수 있습니다.

## ISO 템플리트란 무엇입니까?
{: faq}

ISO 템플리트는 VSI 부팅에 사용될 수 있는, ISO용으로 특별히 예약된 템플리트의 유형입니다. ISO 템플리트는 두 가지 버전(공용 및 개인용)으로 사용 가능합니다. 공용 ISO 템플리트는 {{site.data.keyword.BluSoftlayer_notm}}에 의해 제공되고 임의의 고객이 사용할 수 있는 사전 구성된 템플리트입니다. 개인용 ISO 템플리트는 {{site.data.keyword.objectstorageshort}} 계정에 저장된 ISO 이미지를 가져옴으로써 작성됩니다. ISO를 이미지 템플리트 화면으로 가져오려면 ISO가 부팅 가능해야 합니다.

오직 {{site.data.keyword.BluSoftlayer_notm}} 지원 운영 체제만을 사용하여 ISO 템플리트를 VSI로 로드할 수 있습니다. 자세한 정보는 [지원 운영 체제 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.softlayer.com/services/software/)를 참조하십시오.
{:tip}

## 공용 이미지와 개인용 이미지 간의 차이점은 무엇입니까?
{: faq}

공용 이미지는 임의의 {{site.data.keyword.BluSoftlayer_notm}} 사용자가 보고, 새 가상 서버에 적용할 수 있는 이미지입니다. {{site.data.keyword.BluSoftlayer_notm}}에서는 현재 다양한 디바이스의 구성 옵션에 대한 솔루션으로
공용 이미지를 작성합니다. 또한 고객은 이미지를 공용화하고 이를 임의의 사용자가 사용하도록 허용할 수 있습니다. 개인용 이미지는 권한 부여된 사용자만 볼 수 있는
이미지입니다. 권한 부여된 사용자는 기본적으로 해당 계정의 모든 사용자입니다.
그러나 {{site.data.keyword.slportal}}에서 공유 옵션을 업데이트하여 여러 계정 간에 이미지를 공유할 수도 있습니다.

## 자체 관리되는 내 하이퍼바이저로 가상 서버를 캡처하고 배치할 수 있습니까?
{: faq}

{{site.data.keyword.BluSoftlayer_notm}}에서 프로비저닝하는 서버만 캡처 및 배치가 가능합니다. 개인 디바이스에서 수동으로 작성된 개별 가상 서버의 캡처, 프로비저닝 또는 배치는 허용되지 않습니다.

## 내 개인용 ISO 템플리트를 다른 고객과 공유할 수 있습니까?
{: faq}

모든 고객에게 공용화된 유일한 ISO 템플리트는 {{site.data.keyword.BluSoftlayer_notm}}에서 생성한 템플리트입니다. 개인용 ISO 템플리트는 계정마다 고유하며, {{site.data.keyword.slportal}}을 통해 고객 간에 공유될 수 없습니다.

## 이미지에서 부팅을 완료하려면 내 ISO 템플리트가 내 가상 서버와 동일한 데이터 센터에 있어야 합니까?
{: faq}

예. 이미지에서 부팅하려면 ISO 템플리트가 가상 서버와 동일한 데이터 센터에 있어야 합니다. 동일한 데이터 센터에 있지 않은 경우
조치를 완료하려면 ISO 템플리트를 적합한 데이터 센터로 복사해야 합니다. 이러한 상황이 발생하면
트랜잭션 관련 경고가 나타납니다. ISO 템플리트가 다른 데이터 센터로 복사된 경우
기타 유형의 이미지 템플리트 복사에 대해 요금이 적용되는 것과 동일한 방식으로 계정에 소액의 요금이 부과됩니다.

## 실제 데이터 및 볼륨 크기 간의 차이는 무엇입니까?
{: faq}

볼륨은 파일 저장에 사용할 수 있는 디스크 공간이고, 실제 데이터는 디스크에 저장된 실제 파일로 구성됩니다.

## 이미지 가져오기/내보내기 기능이란 무엇입니까?
{: faq}

{{site.data.keyword.slportal}}의 이미지 템플리트 페이지에 있는 이미지 가져오기/내보내기 기능을 사용하면 이미지 템플리트로 변환될 수 있도록 {{site.data.keyword.objectstorageshort}} 계정에 저장된 VHD 및 ISO의 변환이 허용되고 역방향으로의 변환도 허용됩니다. 이미지를 가져오는 경우 특정 파일(VHD 또는 ISO)의 소스는 지정된 [{{site.data.keyword.objectstorageshort}}] 계정의 컨테이너이며, 해당 파일은 이미지 템플리트로 변환됩니다. 그러면 이미지 템플리트를 사용하여 디바이스를 부팅하거나 로드할 수 있습니다. 이미지를 내보내는 경우 이미지 템플리트는 {{site.data.keyword.objectstorageshort}} 계정의 컨테이너에서 지정된 위치에 저장된 파일(또는 템플리트에 복수의 디스크가 있는 경우에는 여러 개의 파일)로 변환됩니다.

## 기본 드라이브만이 아니라 전체 서버용으로 이미지 템플리트를 작성하는 방법은 무엇입니까?
{: faq}

전체 서버의 이미지 템플리트를 작성하려면 [이미지 템플리트 작성](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)의 지시사항을 참조하십시오.

이미지 템플리트를 IBM Cloud Object Storage로 내보내도록 선택하면 각 블록 디바이스(또는 디스크)에는 연관된 고유 파일이 있습니다. 예를 들어 이미지 파일의 이름을 image.vhd로 지정하면 첫 번째 블록 디바이스를 image-0.vhd로 내보냅니다. 두 번째 블록 디바이스는 image-1.vhd로 내보내는 식입니다.
{: tip}
