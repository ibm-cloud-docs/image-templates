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

# IBM Cloud オブジェクト・ストレージから IBM Cloud for Government アカウントへのイメージのインポート
{: import-an-image-from-ibm-cloud-object-storage-to-ibm-cloud-for-government-account}

{{site.data.keyword.cos_full}} から {{site.data.keyword.ibmcloudgov_full_notm}} アカウントにイメージをインポートするには、{{site.data.keyword.slapi_short}} を使用する必要があります。{{site.data.keyword.slapi_short}} と仮想サーバー API について詳しくは、{{site.data.keyword.sldn_full}} の以下の資料を参照してください。
* [{{site.data.keyword.slapi_short}} の概要 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/reference/softlayerapi/){: new_window}
* [{{site.data.keyword.slapi_short}} 入門![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/article/getting-started/){: new_window}

## イメージのインポート
{: import-gov-image}

{{site.data.keyword.cos_full_notm}} から {{site.data.keyword.ibmcloudgov_full_notm}} アカウントにイメージをインポートするには、以下の JSON を要求本体に含めて、POST 要求を https://api.usgov.softlayer.com/rest/v3.1/SoftLayer_Virtual_Guest_Block_Device_Template_Group/createFromIcos.json に送信します。

以下の JSON 要求本体は汎用例です。
{:note}

### JSON 要求本体の例

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
