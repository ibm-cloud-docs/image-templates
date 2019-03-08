---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-21"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# エンド・ツー・エンド (E2E) の暗号化を使用した暗号化インスタンスのプロビジョン

エンド・ツー・エンド (E2E) の暗号化機能を使用すると、ユーザーは、自分が所有および管理するデータ暗号化鍵で既に暗号化した cloud-init 対応のオペレーティング・システム・イメージを持ち込むことができます。環境のセットアップが完了したら、暗号化イメージをイメージ・テンプレート・リポジトリーにインポートし、そのイメージを使用して、暗号化された仮想サーバー・インスタンスをプロビジョンできます。E2E 暗号化を使用する場合は、プロビジョンした仮想サーバー・インスタンスに関連付けたストレージに対して保存データ暗号化機能を利用できます。この機能を利用するには、サポートにお問い合わせください。
{:shortdesc}

E2E 暗号化は、複数の {{site.data.keyword.cloud}} コンポーネントを結集して、重要な情報を保護するためのソリューションを実現しています。

* {{site.data.keyword.keymanagementservicefull_notm}}: FIPS 140-2 レベル 2 認定を受けた、情報盗難防止用のクラウド・ベースのハードウェア・セキュリティー・モジュール (HSM) を使用してユーザーの鍵を保護します。
* IBM {{site.data.keyword.iamshort}} (IAM): Cloud のブロック・ストレージ・サービスが、{{site.data.keyword.keymanagementserviceshort}} と、データ暗号化鍵のラップに使用されたルート鍵とにアクセスできるようにします。
* {{site.data.keyword.cos_full_notm}}: アップロードされた暗号化イメージを安全に保管します。
* {{site.data.keyword.slportal}}: ここで、ユーザーは暗号化イメージをインポートしてイメージ・テンプレートを作成できます。
* {{site.data.keyword.cloud_notm}} コンソール・インフラストラクチャー環境: ここで、ユーザーは暗号化イメージのテンプレートを使用して、暗号化された仮想サーバー・インスタンスをプロビジョンできます。
* 最後に、アクティビティー・トラッカーを使用して、暗号化された仮想サーバーに関連するイベントを監査することもできます。

## 環境の準備

1. 仮想サーバーに E2E 暗号化を使用するには、アップグレードしたアカウントが必要です。詳しくは、[IBM ID への切り替えとアカウントのリンク](/docs/account?topic=account-unifyingaccounts)を参照してください。
2. {{site.data.keyword.keymanagementservicefull_notm}} を使用して、鍵を作成して管理します。
      1. [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision) サービスをプロビジョンします。
      2. {{site.data.keyword.keymanagementservicelong_notm}} でルート鍵 (CRK) を[作成](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys)するか[インポート](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys)します。
      3. **オプション**: 復号用の標準鍵を[作成](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys)するか[インポート](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys)することを選択できます。
      4. データ暗号化鍵をラップできるように、[{{site.data.keyword.keymanagementserviceshort}} API へのアクセス](/docs/services/key-protect?topic=key-protect-set-up-api#set-up-api)権を取得します。
      5. ルート鍵を使用して、[データ暗号化鍵のラッピング](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys)を行います。暗号化イメージを {{site.data.keyword.slportal}}にインポートするときには、このラップしたデータ暗号化鍵 (WDEK) に関連付けられた暗号テキストが必要になります。
3. IBM {{site.data.keyword.iamshort}} (IAM) から、**Cloud ブロック・ストレージ** (ソース・サービス) と {{site.data.keyword.keymanagementservicelong_notm}} (ターゲット・サービス) の間の[アクセスを許可](/docs/iam?topic=iam-serviceauth#create-an-authorization)します。暗号化イメージを {{site.data.keyword.cos_full_notm}} からインポートするユーザーのために、{{site.data.keyword.keymanagementservicelong_notm}} に対する [アクセス・ポリシー](/docs/iam?topic=iam-userroles)を IAM で定義しておく必要があります。
4. IBM Cloud コンソールで、{{site.data.keyword.cos_full_notm}} のインスタンスを作成し、データを保管するバケットを作成します。詳しくは、[Getting started with {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-console-#getting-started-console-) を参照してください。
      1. {{site.data.keyword.keymanagementserviceshort}} サービスをプロビジョンしたのと同じ地域の場所に {{site.data.keyword.cos_full_notm}} インスタンスを作成します。
      2. バケットを作成する際には、**「回復力」**の設定を_「Regional」_にする必要があります。
      3. オプションで、バケットを作成する際に、[鍵を使用して暗号化](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-manage-encryption#sse-kp)することもできます。   

## 暗号化イメージの準備

1. {{site.data.keyword.cloud_notm}} インフラストラクチャー環境で機能する、暗号化対象の非暗号化イメージを選択します。既存の仮想サーバーを使用して[カスタム・イメージを作成](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)するという選択肢もあります。詳しくは、[cloud-init でプロビジョンした仮想サーバーから作成したイメージ・テンプレートを使用する方法](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-a-standard-image-created-from-a-cloud-init-provisioned-virtual-server)を参照してください。既存の VHD イメージを使用することもできます。イメージが[暗号化イメージの要件](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#encrypted-image-reqs)を満たしていることを確認してください。
2. {{site.data.keyword.slportal}}にあるイメージ・テンプレートを使用する場合は、{{site.data.keyword.cos_full_notm}} に[その非暗号化イメージをエクスポート](/docs/infrastructure/image-templates?topic=image-templates-exporting-to-ibm-cos)します。
3. {{site.data.keyword.cos_full_notm}} から安全なローカル・マシンにイメージ・ファイルをダウンロードし、そのイメージを暗号化します。サービス・ダッシュボードで、**「ダウンロード」**アクションを選択してストレージからオブジェクトを取得します。200 MB を超えるイメージをダウンロードするときには、Aspera 高速転送プラグインを使用すると良いでしょう。
4. QEMU および DMCrypt で LUKS ディスク暗号化形式を使用して[イメージを暗号化](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption)します。
5. {{site.data.keyword.cos_full_notm}} で、バケットにナビゲートし、**「オブジェクトの追加」**をクリックして、暗号化したイメージを[アップロード](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data)します。200 MB を超えるイメージをアップロードするときには、Aspera 高速転送プラグインを使用すると良いでしょう。

## 暗号化イメージのインポートとインスタンスの注文

1. IBM {{site.data.keyword.iamshort}} (IAM) を使用して、暗号化イメージを {{site.data.keyword.slportal}} にインポートする際の認証に使用するサービス ID を作成します。
      1. [サービス ID](/docs/iam?topic=iam-serviceids#serviceids) を作成します。
      2. [アクセス・ポリシー](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy)を割り当てます。{{site.data.keyword.cos_full_notm}} と {{site.data.keyword.keymanagementservicelong_notm}} のサービスに対するアクセス権限を割り当てます。
      3. [サービス ID の API キー](/docs/iam?topic=iam-serviceidapikeys#creating-an-api-key-for-a-service-id)を作成します。
      4. 詳しくは、[Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window} を参照してください。
2. {{site.data.keyword.slportal}}で、「イメージ・テンプレート」ページに[暗号化イメージをインポート](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images#import-icos)します([{{site.data.keyword.slapi_short}} を使用して](/docs/infrastructure/image-templates?topic=image-templates-importing-an-encrypted-image-by-using-the-softlayer-api)、暗号化イメージをインポートすることもできます)。
3. 「イメージ・テンプレート」ページから、暗号化イメージを使用して、仮想サーバー・インスタンスを[注文](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template)できます。
4. 暗号化された仮想サーバーをプロビジョンしたら、[アクティビティー・トラッカー](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov)を使用して、[仮想サーバー・イベント](/docs/vsi?topic=virtual-servers-at_events#at_events)を監査できます。
