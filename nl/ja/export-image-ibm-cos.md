---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# IBM Cloud オブジェクト・ストレージへのイメージのエクスポート
{: #exporting-an-image-to-ibm-cloud-object-storage}

「イメージ・テンプレート」ページから、イメージ・テンプレートを [{{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about) アカウントにエクスポートすることができます。{:shortdesc}

このイメージのエクスポート・プロセスでは、プライベートの既存の標準イメージ・テンプレート、または暗号化イメージのテンプレートを選択し、そのイメージを、{{site.data.keyword.cos_full_notm}} アカウント上の指定された場所に保管されるイメージ・ファイルに変換します。

*注:* VMDK イメージをインポートした場合、そのイメージを VHD または VMDK 形式でエクスポートできます。 イメージ形式の間に違いがあるため、データが損失する可能性があります。 データ損失の際にデータを保護するために、元の VHD ファイルが保存されます。

## 開始する前に
まず、デバイス・メニューにナビゲートして、タスクを完了するための正しいアカウント権限があることを確認します。

* コンソールのデバイス・メニューにナビゲートします。詳しくは、[デバイスへのナビゲート](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices)を参照してください。
* {{site.data.keyword.cos_full_notm}} に対する書き込み権限があることを確認してください。アカウントの所有者、または**ユーザーの管理**クラシック・インフラストラクチャー権限を持つユーザーのみが権限を調整できます。

権限について詳しくは、[クラシック・インフラストラクチャー権限](/docs/iam?topic=iam-infrapermission#infrapermission)および[デバイス・アクセス権限の管理](/docs/vsi?topic=virtual-servers-managing-device-access)を参照してください。

このイメージ・テンプレートを {{site.data.keyword.cos_full_notm}} にエクスポートする予定の場合は、その名前に Web アドレスで問題となる可能性のある文字が含まれていないことを確認してください。例えば、?、=、<、および他の特殊文字は URL エンコードされていない場合、望ましくない動作を引き起こす可能性があります。
{: tip}

## IBM Cloud オブジェクト・ストレージへのイメージのエクスポート

IBM Cloud オブジェクト・ストレージにイメージ・テンプレートをエクスポートするには、以下の手順を使用します。

1. **「デバイス」**メニューから、**「管理」>「イメージ」**を選択します。
2. エクスポートするイメージ・テンプレートの**「アクション」**をクリックし、**「IBM COS にイメージをエクスポート (Export Image to IBM COS)」**を選択します。 必要な構成のイメージ・テンプレートがまだ存在しない場合は、[イメージ・テンプレートの作成](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template)を参照してください。
3. 必須フィールドに入力します (表 1 を参照)。
4. **「OK」**をクリックして、{{site.data.keyword.cos_full_notm}} アカウントの指定した場所にイメージをエクスポートします。

| フィールド | 値 |
| ----- | ----- |
| ファイル名 | イメージのファイル名を入力します。 |
| {{site.data.keyword.cos_full_notm}} | {{site.data.keyword.cos_full_notm}} アカウントを選択します。 |
| 場所 | イメージ・テンプレートを保管する特定の地理的領域を選択します。 |
| バケット | イメージ・テンプレートを保管する {{site.data.keyword.cos_full_notm}} バケットを選択します。 選択した地域の場所に存在するバケットだけが選択可能です。 選択した場所に存在しないバケットを指定すると、エラー・メッセージを受け取ります。 |
| API キー | {{site.data.keyword.cos_full_notm}} に対する書き込みアクセス権限を持つサービス ID の API キーを指定します。 詳しくは、[サービス ID の API キーの管理](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys)を参照してください。 |
{: caption="表 1. {{site.data.keyword.cos_full_notm}} にイメージをエクスポートするための値" caption-side="top"}
| ターゲット・ファイル・タイプ | エクスポートするファイル・タイプを選択します。 VMDK イメージをインポートした場合、そのイメージを VHD または VMDK 形式でエクスポートする選択肢があります。 ファイルが元は VHD 形式であった場合、VHD としてのみエクスポートできます。 |

## 次のステップ
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
イメージごとにサイズと特性が異なるため、エクスポート・プロセスが完了するまでに数分間かかることがあります。

エクスポートしたイメージは、イメージ・テンプレートのリストに残りますが、エクスポート・プロセスで指定した {{site.data.keyword.cos_full_notm}} の場所にもファイルとしても存在するようになります。イメージ・ファイルは {{site.data.keyword.cos_full_notm}} からダウンロードできます。 {{site.data.keyword.cloud_notm}} コンソールの「リソース・リスト」から、Cloud Object Storage サービス・インスタンスにアクセスします。イメージが保管されているバケットで、ダウンロードするイメージ・ファイルを選択してから、**「オブジェクトのダウンロード (Download objects)」**を選択します。

イメージ・テンプレートを IBM Cloud オブジェクト・ストレージにエクスポートすると、各ブロック・デバイス (またはディスク) に独自のファイルが関連付けられます。 例えば、イメージ・ファイルの名前が image.vhd の場合、1 つ目のブロック・デバイスは image-0.vhd としてエクスポートされます。 2 つ目のブロック・デバイスは image-1.vhd としてエクスポートされる、というように続きます。
{: tip}
