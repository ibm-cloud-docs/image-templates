---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}


# 이미지 템플리트 작성
{: #creating-an-image-template}

이미지 템플리트를 사용하면 {{site.data.keyword.virtualmachinesshort}}에 대한 다양한 구성 옵션을 복제할 수 있습니다.
{:shortdesc}

Virtual Server 수명 중 언제든 이미지 템플리트를 작성할 수 있습니다. 그러면 다른 Virtual Server에서 구성의 일부를 신속하게 복제하는 데 사용할 수 있습니다. 운영 체제와 상관없이 모든 Virtual Server에서 이미지 템플리트를 작성할 수 있습니다. 이미지 템플리트가 완료되면 다른 Virtual Server를 작성하는 데 사용할 수 있습니다.

다음 단계를 완료하여 Virtual Server의 이미지 템플리트를 작성하십시오.

1. 고유 인증 정보를 사용하여 [{{site.data.keyword.slportal_full}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){: new_window}에 액세스하십시오.
2. **디바이스** 메뉴에서 **디바이스 목록**을 선택하십시오.
3. 이미지 템플리트를 작성하는 데 사용할 Virtual Server를 클릭하십시오.

  **디바이스 세부사항** 페이지의 **비밀번호** 탭을 확인하십시오. **디바이스 세부사항** 페이지에 나열된 비밀번호가 실제 운영 체제 비밀번호 및 기타 소프트웨어 추가 기능 비밀번호와 일치하는지 확인하십시오. 비밀번호가 일치하지 않으면 이 이미지 템플리트에서 작성하는 Virtual Server가 실패합니다.
  {:tip}

4. **조치** 메뉴에서 **이미지 템플리트 작성**을 선택하십시오.
5. **이미지 이름** 필드에 이미지의 새 이름을 입력하십시오.
6. **참고** 필드에 이미지에 대한 필수 참고사항을 입력하십시오.
7. 모든 정보가 입력되면 **동의** 선택란을 선택하십시오.
8. **템플리트 작성**을 클릭하여 이미지 템플리트를 작성하십시오.

## 다음 단계

이미지 템플리트가 작성된 후에는 템플리트를 사용하여 추가로 Virtual Server를 작성할 수 있습니다. 새 Virtual Server는
이미지 템플리트에 포함된 구성과 동일하게 구성됩니다.
