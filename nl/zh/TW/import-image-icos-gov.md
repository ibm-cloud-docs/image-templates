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

# 從 IBM Cloud Object Storage 將映像檔匯入至 IBM Cloud for Government 帳戶
{: import-an-image-from-ibm-cloud-object-storage-to-ibm-cloud-for-government-account}

若要從 {{site.data.keyword.cos_full}} 匯入映像檔至 {{site.data.keyword.ibmcloudgov_full_notm}} 帳戶，您必須使用 {{site.data.keyword.slapi_short}}。如需 {{site.data.keyword.slapi_short}} 及虛擬伺服器 API 的相關資訊，請參閱 {{site.data.keyword.sldn_full}} 中的下列資源：
* [{{site.data.keyword.slapi_short}} Overview ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://sldn.softlayer.com/reference/softlayerapi/){: new_window}
* [Getting Started with the {{site.data.keyword.slapi_short}} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://sldn.softlayer.com/article/getting-started/){: new_window}

## 匯入映像檔
{: import-gov-image}

若要從 {{site.data.keyword.cos_full_notm}} 匯入映像檔至 {{site.data.keyword.ibmcloudgov_full_notm}} 帳戶，請將 POST 要求提交至 https://api.usgov.softlayer.com/rest/v3.1/SoftLayer_Virtual_Guest_Block_Device_Template_Group/createFromIcos.json，並且在要求內文中使用下列 JSON。

下列 JSON 要求內文是通用範例。
{:note}

### JSON 要求內文範例

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

