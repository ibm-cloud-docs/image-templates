---

copyright:
  years: 2018
lastupdated: "2018-08-09"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 활동 트래커로 가상 서버 이벤트 감사
{: #auditing-virtual-server-events-with-activity-tracker}

[활동 트래커](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov)를 사용하여 라이프사이클 동안 가상 서버 인스턴스를 감사할 수 있습니다. 미국 남부에 프로비저닝된 프리미엄 서비스 플랜과 함께 활동 트래커 인스턴스가 있어야 합니다. 프리미엄 플랜에서는 감사 로그를 필터링하고 검색하는 추가 옵션이 있는 Kibana 대시보드에 액세스할 수 있습니다.

로그는 {{site.data.keyword.cloud}} 계정의 소유자를 위한 계정 로그 섹션에 표시됩니다. 계정 소유자는 다음 로그를 볼 수 있습니다.
* 계정 소유자가 트리거한 이벤트.
* 계정과 연결된 사용자가 트리거한 이벤트.

활동 트래커를 통해 다음 가상 서버 인스턴스 이벤트를 감사할 수 있습니다.
* 가상 서버 인스턴스 프로비저닝
* 사설 또는 공용 인터페이스의 포트를 사용 안함으로 설정
* 사설 또는 공용 인터페이스의 포트를 사용으로 설정
* 포트 속도 설정
* 이미지 템플리트 작성
* 가상 서버 인스턴스 전원 끄기
* 가상 서버 인스턴스 전원 켜기
* 가상 서버 인스턴스 다시 부팅
* 가상 서버 인스턴스 이름 바꾸기
* 가상 서버 인스턴스의 복구 모드 시작
* 가상 서버 인스턴스의 공용 또는 사설 인터페이스에 보안 그룹 추가
* 가상 서버 인스턴스의 공용 또는 사설 인터페이스에서 보안 그룹 제거
* 이미지에서 가상 서버 인스턴스 로드
* 이미지에서 가상 서버 인스턴스 부팅
* 가상 서버 인스턴스 디바이스 취소

네트워크 인터페이스와 연관된 이벤트의 경우 감사 로그에서는 이벤트가 발생한 인터페이스가 공용인지 사설인지 구분하지 않습니다.
{: tip}
