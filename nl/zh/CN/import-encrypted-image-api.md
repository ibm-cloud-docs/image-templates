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


# 使用 SoftLayer API 导入加密映像
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

您可以使用 {{site.data.keyword.slapi_short}} 从 {{site.data.keyword.cos_full}} 导入加密映像，然后创建映像模板。创建映像模板后，可以将其用于供应实例。
{:shortdesc}

要将访问权仅限于完成导入任务所需的信息，请使用服务标识进行认证。服务标识应该只有权访问 IBM Cloud Object Storage 中要导入的加密映像以及存储根密钥的 Key Protect 实例。  

以下 Python 片段示例说明了可以如何访问 [SoftLayer_Virtual_Guest_Block_Device_Template_Group ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) API，并使用 _createFromIcos_ 方法来创建映像模板。

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


有关查找从 {{site.data.keyword.cos_full_notm}} 导入加密映像所需的值的更多信息，请参阅下表。

|字段|值|
| -------- | ------- |
|ibmApiKey|指定在创建 API 密钥时记下的 API 密钥。如果 API 密钥丢失，必须创建新的 API 密钥。有关更多信息，请参阅[管理 API 密钥](/docs/iam?topic=iam-userapikey#userapikey)。|
| crkCrn |指定用于打包数据加密密钥的根密钥的[云资源名称 (CRN)](/docs/overview?topic=overview-crn)。要找到并复制您的根密钥 CRN，请转至 [{{site.data.keyword.keymanagementserviceshort}} 服务实例 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/resources){: new_window}，将鼠标悬停在根密钥上，单击屏幕最右侧的省略号 **(...)**，然后选择“查看 CRN”选项并单击复制图标。|
|wrappedDek|指定与用于加密映像的已包装数据加密密钥关联的密文。有关更多信息，请参阅[使用 API 包装密钥](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys)。|
{: caption="表 1. 导入加密映像所需的值" caption-side="top"}
