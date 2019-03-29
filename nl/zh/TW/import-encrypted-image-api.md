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
{:python: .ph data-hd-programlang='python'}
{:table: .aria-labeledby="caption"}


# 使用 SoftLayer API 匯入已加密的映像檔
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

您可以使用 {{site.data.keyword.slapi_short}}，從 {{site.data.keyword.cos_full}} 匯入已加密的映像檔，並建立映像檔範本。當建立映像檔範本時，您可以使用它來佈建實例。
{:shortdesc}

若限制只能存取完成匯入作業所需的資訊，請使用服務 ID 進行鑑別。服務 ID 應該只能存取 IBM Cloud Object Storage 中要匯入的已加密映像檔，以及儲存主要金鑰的 Key Protect 實例。  

下列 python Snippet 顯示的範例說明如何存取 [SoftLayer_Virtual_Guest_Block_Device_Template_Group ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer.github.io/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) API，並使用 _createFromIcos_ 方法來建立映像檔範本。

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
      'rootKeyId':'my root key ID',
      'wrappedDek':'my wrapped DEK',
      'keyProtectId':'<key_protect_instance_id>',
      }
ret = group_svc.createFromIcos(config)
print(ret)
```
{: codeblock}


如需尋找從 {{site.data.keyword.cos_full_notm}} 匯入已加密映像檔所需之值的相關資訊，請參閱下表。

| 欄位 | 值 |
| -------- | ------- |
| ibmApiKey | 指定您建立 API 金鑰時記下的 API 金鑰。如果 API 金鑰遺失，您必須建立新的 API 金鑰。如需相關資訊，請參閱[管理 API 金鑰](/docs/iam?topic=iam-userapikey)。|
| rootKeyId | 指定用來包裝資料加密金鑰的主要金鑰 ID。如需相關資訊，請參閱[檢視金鑰](/docs/services/key-protect?topic=key-protect-view-keys#view-keys)。|
| wrappedDek | 指定與您包裝之資料加密金鑰相關聯的密碼文字，而此金鑰是用來加密您的映像檔。如需相關資訊，請參閱[使用 API 包裝金鑰](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys)。|
| keyProtectId | 您可以使用 {{site.data.keyword.cloud_notm}} CLI 來尋找 {{site.data.keyword.keymanagementserviceshort}} 實例 ID。如需相關資訊，請參閱[擷取實例 ID](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID)。|
{: caption="表 1. 匯入已加密映像檔所需的值" caption-side="top"}
