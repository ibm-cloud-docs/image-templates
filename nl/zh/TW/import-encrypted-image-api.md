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


# 使用 SoftLayer API 匯入已加密的映像檔
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

您可以使用 {{site.data.keyword.slapi_short}}，從 {{site.data.keyword.cos_full}} 匯入已加密的映像檔，並建立映像檔範本。當建立映像檔範本時，您可以使用它來佈建實例。
{:shortdesc}

若限制只能存取完成匯入作業所需的資訊，請使用服務 ID 進行鑑別。服務 ID 應該只能存取 IBM Cloud Object Storage 中要匯入的已加密映像檔，以及儲存根金鑰的 Key Protect 實例。  

下列 python Snippet 顯示的範例說明如何存取 [SoftLayer_Virtual_Guest_Block_Device_Template_Group ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) API，並使用 _createFromIcos_ 方法來建立映像檔範本。

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


如需尋找從 {{site.data.keyword.cos_full_notm}} 匯入已加密映像檔所需之值的相關資訊，請參閱下表。

| 欄位 | 值 |
| -------- | ------- |
| ibmApiKey | 指定您建立 API 金鑰時記下的 API 金鑰。如果 API 金鑰遺失，您必須建立新的 API 金鑰。如需相關資訊，請參閱[管理 API 金鑰](/docs/iam?topic=iam-userapikey#userapikey)。|
| crkCrn | 針對用來包裝資料加密金鑰的客戶根金鑰指定[雲端資源名稱 (CRN)](/docs/overview?topic=overview-crn)。若要尋找並複製根金鑰 CRN，請移至 [{{site.data.keyword.keymanagementserviceshort}} 服務實例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/resources){: new_window}、將游標移至根金鑰上方、按一下畫面最右側的省略符號 **(...)**，然後選取「檢視 CRN」選項並按一下複製圖示。|
| wrappedDek | 指定與您包裝之資料加密金鑰相關聯的密碼文字，而此金鑰是用來加密您的映像檔。如需相關資訊，請參閱[使用 API 包裝金鑰](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys)。|
{: caption="表 1. 匯入已加密映像檔所需的值" caption-side="top"}
