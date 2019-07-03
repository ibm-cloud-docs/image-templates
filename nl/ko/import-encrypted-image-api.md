---

copyright:
  years: 2018
lastupdated: "2019-05-13"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:python: .ph data-hd-programlang='python'}
{:table: .aria-labeledby="caption"}


# SoftLayer API를 사용하여 암호화된 이미지 가져오기
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

{{site.data.keyword.slapi_short}}를 사용하여 {{site.data.keyword.cos_full}}에서 암호화된 이미지를 가져오고
이미지 템플리트를 작성할 수 있습니다. 이미지 템플리트가 작성되면 인스턴스를 프로비저닝하는 데 사용할 수 있습니다.
{:shortdesc}

가져오기 태스크를 완료하는 데 필요한 정보에만 액세스하도록 제한하려면 서비스 ID로 인증하십시오. 서비스 ID는 IBM Cloud Object Storage에서 가져올 암호화된 이미지와 루트 키가 저장된 키 보호 인스턴스에만 액세스할 수 있어야 합니다.  

다음 python 스니펫에서는 [SoftLayer_Virtual_Guest_Block_Device_Template_Group ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) API에 액세스하고
_createFromIcos_ 메소드를 사용하여 이미지 템플리트를
작성하는 방법의 예를 보여줍니다.

```python
import SoftLayer

client = SoftLayer.create_client_from_env(username='<user>',
                        api_key='<api_key>',
                        endpoint_url='https://api.softlayer.com/rest/v3',
                        timeout=240)
group_svc = client['Virtual_Guest_Block_Device_Template_Group']
config = {'name':'my_encrypted_image',
      'note':'my note',
      'operatingSystemReferenceCode':'REDHAT_7_64',
      'uri':'cos://<region_name>/<bucket_name>/xx123.Rhel_7_encrypted.raw',
      'bootMode':'my boot mode',
      'cloud-init': True,
      'byol': True,
      'encrypted': True,
      'ibmApiKey':'<api_key>',
      'crkCrn': 'crn:v1:bluemix:public:hs-crypto:us-south:a/0d06ba51fa0e431290956d1761da1b7b:5ef6cebe-26d7-4ef3-abdc-fb50f345780f:key:a9640391-aec5-4c86-8942-6e6c59bb40b5',
      'wrappedDek':'my wrapped DEK',
      }
ret = group_svc.createFromIcos(config)
print(ret)
```
{: codeblock}


{{site.data.keyword.cos_full_notm}}에서 암호화된 이미지를 가져오는 데 필요한 값을 찾는 데 관한 자세한 정보는 다음 표를 참조하십시오.

| 필드    |값   |
| -------- | ------- |
| ibmApiKey | 작성할 때 참고한 API 키를 지정합니다. API 키가 유실되면 새 API 키를 작성해야 합니다. 자세한 정보는 [API 키 관리](/docs/iam?topic=iam-userapikey#userapikey)를 참조하십시오. |
| crkCrn | 데이터 암호화 키를 랩핑할 때 사용한 루트 키에 대한 [CRN(Cloud Resource Name)](/docs/overview?topic=overview-crn)을 지정하십시오. 루트 키 CRN을 찾아서 복사하려면 [{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/resources){: new_window}로 이동하여 루트 키 위에 마우스를 놓고 화면의 맨 오른쪽에 있는 생략 기호**(...)**를 클릭한 다음 "CRN 보기" 옵션을 선택하고 복사 아이콘을 클릭하십시오. |
| wrappedDek | 이미지를 암호화하는 데 사용한 랩핑된 데이터 암호화 키와 연관되는 암호 텍스트를 지정합니다. 자세한 정보는 [API를 사용하여 키 랩핑](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys)을 참조하십시오. |
{: caption="표 1. 암호화된 이미지를 가져오는 데 필요한 값" caption-side="top"}
