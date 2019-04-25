---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-16"

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

「イメージ・テンプレート」ページから、[IBM Cloud オブジェクト・ストレージ](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage)のアカウントにイメージ・テンプレートをエクスポートできます。
{:shortdesc}

このイメージのエクスポート・プロセスでは、プライベートの既存の標準イメージ・テンプレート、または暗号化イメージのテンプレートを選択し、そのイメージを、IBM Cloud オブジェクト・ストレージ・アカウント上の指定された場所に保管されるイメージ・ファイルに変換します。

*注:* VMDK イメージをインポートした場合、そのイメージを VHD または VMDK 形式でエクスポートできます。イメージ形式の間に違いがあるため、データが損失する可能性があります。データ損失の際にデータを保護するために、元の VHD ファイルが保存されます。

IBM Cloud オブジェクト・ストレージにイメージ・テンプレートをエクスポートするには、以下の手順を使用します。

1. IBM Cloud オブジェクト・ストレージに対する書き込みアクセス権限を持つサービス ID を使用して、{{site.data.keyword.slportal}}の認証を受けます。
2. [{{site.data.keyword.slportal_full}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/) の**「デバイス」**メニューから、**「管理」>「イメージ」**を選択します。
3. エクスポートするイメージ・テンプレートの**「アクション」**をクリックし、**「IBM COS にイメージをエクスポート (Export Image to IBM COS)」**を選択します。 必要な構成のイメージ・テンプレートがまだ存在しない場合は、[イメージ・テンプレートの作成](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template)を参照してください。
4. 必須フィールドに入力します (表 1 を参照)。
5. **「OK」**をクリックして、IBM Cloud オブジェクト・ストレージ・アカウントの指定した場所にイメージをエクスポートします。

| フィールド | 値 |
| ----- | ----- |
| ファイル名 | イメージのファイル名を入力します。 |
| {{site.data.keyword.cos_full_notm}} | {{site.data.keyword.cos_full_notm}} アカウントを選択します。 |
| 場所 | イメージ・テンプレートを保管する特定の地理的領域を選択します。 |
| バケット | イメージ・テンプレートを保管する {{site.data.keyword.cos_full_notm}} バケットを選択します。 選択した地域の場所に存在するバケットだけが選択可能です。 選択した場所に存在しないバケットを指定すると、エラー・メッセージを受け取ります。 |
| API キー | {{site.data.keyword.cos_full_notm}} に対する書き込みアクセス権限を持つサービス ID の API キーを指定します。 詳しくは、[サービス ID の API キーの管理](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys)を参照してください。 |
{: caption="表 1. IBM Cloud オブジェクト・ストレージにイメージをエクスポートするための値" caption-side="top"}
| ターゲット・ファイル・タイプ | エクスポートするファイル・タイプを選択します。VMDK イメージをインポートした場合、そのイメージを VHD または VMDK 形式でエクスポートする選択肢があります。ファイルが元は VHD 形式であった場合、VHD としてのみエクスポートできます。|

## 次のステップ
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
イメージごとにサイズと特性が異なるため、エクスポート・プロセスが完了するまでに数分間かかることがあります。

エクスポートしたイメージは、イメージ・テンプレートのリストに残りますが、エクスポート・プロセスで指定した IBM Cloud オブジェクト・ストレージの場所にもファイルとしても存在するようになります。イメージ・ファイルは {{site.data.keyword.cos_full_notm}} からダウンロードできます。サービス・ダッシュボードで、**「ダウンロード」**アクションを選択してストレージからオブジェクトを取得します。 200 MB を超えるイメージをダウンロードするときには、Aspera 高速転送プラグインを使用すると良いでしょう。

イメージ・テンプレートを IBM Cloud オブジェクト・ストレージにエクスポートすると、各ブロック・デバイス (またはディスク) に独自のファイルが関連付けられます。 例えば、イメージ・ファイルの名前が image.vhd の場合、1 つ目のブロック・デバイスは image-0.vhd としてエクスポートされます。 2 つ目のブロック・デバイスは image-1.vhd としてエクスポートされる、というように続きます。
{: tip}
