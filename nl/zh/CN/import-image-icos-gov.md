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

# 将映像从 IBM Cloud Object Storage 导入 IBM Cloud for Government 帐户
{: import-an-image-from-ibm-cloud-object-storage-to-ibm-cloud-for-government-account}

要将映像从 {{site.data.keyword.cos_full}} 导入 {{site.data.keyword.ibmcloudgov_full_notm}} 帐户，必须使用 {{site.data.keyword.slapi_short}}。有关 {{site.data.keyword.slapi_short}} 和虚拟服务器 API 的更多信息，请参阅 {{site.data.keyword.sldn_full}} 中的以下资源：
* [{{site.data.keyword.slapi_short}} 概述 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/reference/softlayerapi/){: new_window}
* [{{site.data.keyword.slapi_short}} 入门 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/article/getting-started/){: new_window}

## 导入映像
{: import-gov-image}

要将映像从 {{site.data.keyword.cos_full_notm}} 导入 {{site.data.keyword.ibmcloudgov_full_notm}} 帐户，请在请求主体中使用以下 JSON 向 https://api.usgov.softlayer.com/rest/v3.1/SoftLayer_Virtual_Guest_Block_Device_Template_Group/createFromIcos.json 提交 POST 请求。

以下 JSON 请求主体为通用示例。
{:note}

### 示例 JSON 请求主体

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

