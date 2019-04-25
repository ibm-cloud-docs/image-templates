---

copyright:
  years: 2019
lastupdated: "2019-04-01"

keywords:  fed, ic4g

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:note: .note}
{:screen: .screen}
{:new_window: target="_blank"}

# IBM Cloud Object Storage에서 IBM Cloud for Government 계정으로 이미지 가져오기
{: import-an-image-from-ibm-cloud-object-storage-to-ibm-cloud-for-government-account}

{{site.data.keyword.cos_full}}에서 {{site.data.keyword.ibmcloudgov_full_notm}} 계정으로 이미지를 가져오려면 {{site.data.keyword.slapi_short}}를 사용해야 합니다. {{site.data.keyword.slapi_short}} 및 가상 서버 API에 관한 자세한 정보는 {{site.data.keyword.sldn_full}}의
다음 리소스를 참조하십시오.
* [{{site.data.keyword.slapi_short}} 개요 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/reference/softlayerapi/){: new_window}
* [{{site.data.keyword.slapi_short}} 시작하기 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/article/getting-started/){: new_window}

## 이미지 가져오기
{: import-gov-image}

{{site.data.keyword.cos_full_notm}}에서 {{site.data.keyword.ibmcloudgov_full_notm}} 계정으로 이미지를 가져오려면 요청 본문에서 다음 JSON을 사용하여 https://api.usgov.softlayer.com/rest/v3.1/SoftLayer_Virtual_Guest_Block_Device_Template_Group/createFromIcos.json에 POST 요청을 제출하십시오.

JSON 요청 본문의 일반 예제는 다음과 같습니다.
{:note}

### 예제 JSON 요청 본문

```
{
    "parameters": [
        {
            "name": "IMAGE_NAME",
            "note": "NOTE_FOR_IMAGE",
            "operatingSystemReferenceCode": "UBUNTU_16_64",
            "uri": "cos://us-south/my-bucket-name/my-image-object-name.vhd",
            "isEncrypted": 0,
            "ibmAccessKey": "MY_HMAC_ACCESS_KEY",
            "ibmSecretKey": "MY_HMAC_SECRET_KEY"
        }
    ]
}
```
