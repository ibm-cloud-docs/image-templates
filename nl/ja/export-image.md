---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:deprecated: .deprecated}
{:new_window: target="_blank"}

# OpenStack Swift へのイメージのエクスポート
{: #exporting-an-image-to-openstack-swift}

「イメージ・テンプレート」ページから、[Object Storage OpenStack Swift](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-GettingStarted#getting-started-with-object-storage-openstack-swift) アカウントにイメージ・テンプレートをエクスポートできます。
{:shortdesc}

このサービスのすべてのインスタンスは非推奨です。既存のアカウントは使用できますが、2018 年 12 月 10 日より後に新しい {{site.data.keyword.objectstorageshort}} OpenStack Swift アカウントをプロビジョンすることはできません。2019 年 3 月 31 日で、IBM Cloud は Cloud Object Storage OpenStack Swift でのイメージ・テンプレートのインポート/エクスポート機能のサポートを終了します。
{: deprecated}

このイメージのエクスポート・プロセスでは、プライベートの既存の標準イメージ・テンプレートを選択し、そのイメージを Object Storage OpenStack Swift アカウント上の指定した場所に保管されるイメージ・ファイルに変換します。 イメージ・テンプレートをエクスポートするには、以下の手順を使用します。

1. [{{site.data.keyword.slportal_full}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/) の**「デバイス」**メニューから、**「管理」>「イメージ」**を選択します。
2. エクスポートするイメージ・テンプレートに対する**「アクション」**をクリックし、**「イメージのエクスポート」**を選択します。 必要な構成のイメージ・テンプレートがまだ存在しない場合は、[イメージ・テンプレートの作成](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)を参照してください。
3. 「イメージのエクスポート」ページで、**「ファイル名」**フィールドにイメージの名前を入力します。
5. **「アカウント」**ドロップダウン・リストから**「オブジェクト・ストレージ・アカウント」**を選択します。
6. **「クラスター」**ドロップダウン・リストから**「オブジェクト・ストレージ・クラスター」**を選択します。
7. **「コンテナー」**ドロップダウン・リストから**「オブジェクト・ストレージ・コンテナー」**を選択します。
8. **「イメージのエクスポート」**をクリックして、オブジェクト・ストレージ・アカウントの指定の場所にイメージをエクスポートします。 操作を取り消すには、**「閉じる」**をクリックします。

## 次のステップ

エクスポートしたイメージは、イメージ・テンプレートのリストに残りますが、エクスポート・プロセスで指定した Object Storage OpenStack Swift の場所にもファイルとして存在するようになります。 オブジェクト・ストレージ・アカウントにエクスポートされたファイルの表示方法について詳しくは、[ファイルの詳細の表示および編集](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-OSSSLPortal#viewing-and-editing-file-details)を参照してください。 イメージごとにサイズと特性が異なるため、エクスポート・プロセスが完了するまでに数分間かかることがあります。 平均的なエクスポート速度は、2 GB / 分です。 数分経過してもイメージが Object Storage OpenStack Swift アカウントに表示されない場合は、[サポートにお問い合わせください](/docs/get-support?topic=get-support-getting-customer-support)。
