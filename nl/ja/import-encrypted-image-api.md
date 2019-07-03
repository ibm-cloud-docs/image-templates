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


# SoftLayer API を使用した暗号化イメージのインポート
{: #importing-an-encrypted-image-by-using-the-softlayer-api}

{{site.data.keyword.slapi_short}} を使用して、暗号化されたイメージを {{site.data.keyword.cos_full}} からインポートし、イメージ・テンプレートを作成できます。 イメージ・テンプレートを作成したら、そのテンプレートを使用してインスタンスをプロビジョンできます。
{:shortdesc}

インポート作業の実行に必要な情報だけにアクセスを制限するために、サービス ID で認証を受けます。 そのサービス ID でアクセスできるのは、IBM Cloud オブジェクト・ストレージ内のインポート対象の暗号化イメージと、ルート鍵が保管されている Key Protect インスタンスだけにする必要があります。  

以下の Python スニペットは、[SoftLayer_Virtual_Guest_Block_Device_Template_Group ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest_Block_Device_Template_Group/) API にアクセスし、_createFromIcos_ メソッドを使用してイメージ・テンプレートを作成する方法の例を示しています。

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


暗号化イメージを {{site.data.keyword.cos_full_notm}} からインポートするために必要な値の詳細については、以下の表を参照してください。

| フィールド    | 値   |
| -------- | ------- |
| ibmApiKey | API キーの作成時にメモした API キーを指定します。 API キーを紛失した場合は、新しい API キーを作成する必要があります。 詳しくは、[API キーの管理](/docs/iam?topic=iam-userapikey#userapikey)を参照してください。 |
| crkCrn | データ暗号化鍵のラップに使用したルート鍵に [Cloud Resource Name (CRN)](/docs/overview?topic=overview-crn) を指定します。ルート鍵 CRN を見つけてコピーするには、[{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/resources){: new_window}に移動し、ルート鍵の上にマウスを合わせ、画面の右端の方にある省略符号 **(...)** をクリックして、「CRN の表示 (View CRN)」オプションを選択してコピー・アイコンをクリックします。|
| wrappedDek | イメージの暗号化に使用したデータ暗号化鍵をラップしたものに関連付けられた暗号テキストを指定します。 詳しくは、[鍵のラッピング](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys)を参照してください。 |
{: caption="表 1. 暗号化イメージをインポートするために必要な値" caption-side="top"}
