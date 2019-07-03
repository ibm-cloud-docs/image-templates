---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-13"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# エンド・ツー・エンド (E2E) の暗号化を使用した暗号化インスタンスのプロビジョン
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

エンドツーエンド (E2E) の暗号化機能を使用すると、ユーザーは、自分が所有および管理するデータ暗号化鍵で既に暗号化した cloud-init 対応の暗号化オペレーティング・システム・イメージを持ち込むことができます。 環境のセットアップが完了したら、暗号化イメージをイメージ・テンプレート・リポジトリーにインポートし、そのイメージを使用して、暗号化された仮想サーバー・インスタンスをプロビジョンできます。 E2E 暗号化を使用する場合は、プロビジョンした仮想サーバー・インスタンスに関連付けたストレージに対して保存データ暗号化機能を利用できます。
{:shortdesc}

E2E 暗号化は、複数の {{site.data.keyword.cloud}} コンポーネントを結集して、重要な情報を保護するためのソリューションを実現しています。

* 暗号鍵を保護するための {{site.data.keyword.keymanagementservicelong_notm}} または {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} などの IBM 鍵管理サービス (表 1 を参照)。
* IBM {{site.data.keyword.iamshort}} (IAM): Cloud のブロック・ストレージ・サービスが、鍵管理システムと、データ暗号化鍵のラップに使用されたルート鍵とにアクセスできるようにします。
* {{site.data.keyword.cos_full_notm}}: アップロードされた暗号化イメージを安全に保管します。
* {{site.data.keyword.cloud_notm}} コンソールで、ユーザーは暗号化イメージをインポートしてイメージ・テンプレートを作成できます。
* {{site.data.keyword.cloud_notm}} コンソール・インフラストラクチャー環境: ユーザーは、ここで利用できる暗号化イメージのテンプレートを使用して、暗号化された仮想サーバー・インスタンスをプロビジョンできます。
* 最後に、[アクティビティー・トラッカー](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov)を使用して、暗号化された仮想サーバーに関連するイベントを監査することができます。

## 暗号鍵管理サービス

{{site.data.keyword.keymanagementserviceshort}} および {{site.data.keyword.hscrypto}} (現在、特定の[地域](/docs/services/hs-crypto?topic=hs-crypto-regions#regions)で使用可能) は、共通鍵プロバイダー API を使用して、暗号化鍵を管理するための一貫したアプローチを提供します。裏では、{{site.data.keyword.cloud_notm}} データ・センターは、鍵を保護するための専用ハードウェア・セキュリティー・モジュール (HSM) を提供します。以下のオプションから選択できます。 

|鍵管理サービス| HSM 暗号化証明 |
| ----- | ----- |
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/concepts?topic=key-protect-getting-started-tutorial#getting-started-tutorial) | FIPS 140-2 *レベル 2* コンプライアンス |
| [{{site.data.keyword.hscrypto}}](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) | FIPS 140-2 *レベル 4* コンプライアンス |
{: caption="表 1. 使用可能な鍵管理サービスのオプション" caption-side="top"}

## 環境の準備

1. 仮想サーバーに E2E 暗号化を使用するには、アップグレードしたアカウントが必要です。 詳しくは、[IBMid への切り替えとアカウントのリンク](/docs/account/softlayerlink.html)を参照してください。

2. 鍵管理サービスを使用して、鍵を作成して管理します。次の手順例は {{site.data.keyword.keymanagementserviceshort}} に固有のものですが、一般的なフローは {{site.data.keyword.hscrypto}} にも適用されます。{{site.data.keyword.hscrypto}} を使用している場合、対応する指示については、そのサービスの[資料](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started)を参照してください。
      1. [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision) サービスをプロビジョンします。
      2. {{site.data.keyword.keymanagementservicelong_notm}} でルート鍵 (CRK) を[作成](/docs/services/key-protect?topic=key-protect-create-root-keys)するか[インポート](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys)します。
      3. **オプション**: 復号用の標準鍵を[作成](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys)するか[インポート](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys)することを選択できます。      
      4. ルート鍵を使用して VHD イメージを暗号化するために使用する[データ暗号化鍵をラップ](/docs/services/key-protect?topic=key-protect-cli-reference#kp-wrap)できるように、[{{site.data.keyword.cloud_notm}}Key Protect CLI プラグインをセットアップします](/docs/services/key-protect?topic=key-protect-set-up-cli)。暗号化イメージを {{site.data.keyword.cloud_notm}} コンソールにインポートするときには、このラップしたデータ暗号化鍵 (WDEK) に関連付けられた暗号テキストが必要になります。  
         
       Key Protect は追加認証データ (AAD) を保存しませんが、最大 255 の文字列 (それぞれの文字列がコンマで区切られ、それぞれの文字列が最大 255 文字) でより安全に鍵を保護するために AAD を引き続き使用することができます。鍵のラップ用に AAD を提供する場合は、同じ AAD にアクセスすることや、将来の鍵のアンラップ要求時に同じ AAD を提供することが確実にできるようにするため、データを安全な場所に保存してください。{: tip}
      
3. IBM {{site.data.keyword.iamshort}} (IAM) から、**Cloud ブロック・ストレージ** (ソース・サービス) と**鍵管理サービス** (ターゲット・サービス) の間の[アクセスを許可](/docs/iam?topic=iam-serviceauth#create-auth)します。暗号化イメージを {{site.data.keyword.cos_full_notm}} からインポートする場合、鍵管理サービスに対する[アクセス・ポリシー](/docs/iam?topic=iam-userroles#userroles)を IAM で定義しておく必要があります。
4. IBM Cloud コンソールで、{{site.data.keyword.cos_full_notm}} のインスタンスを作成し、データを保管するバケットを作成します。 詳しくは、[{{site.data.keyword.cos_full_notm}} の入門チュートリアル](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)を参照してください。
      1. 鍵管理サービスをプロビジョンしたのと同じ地域に {{site.data.keyword.cos_full_notm}} インスタンスを作成します。
      2. バケットを作成する際には、**「回復力」**の設定を_「Regional」_にする必要があります。
      3. オプションで、バケットを作成する際に、[鍵を使用して暗号化](/docs/services/cloud-object-storage?topic=cloud-object-storage-encryption#encryption-kp)することもできます。   

## 暗号化イメージの準備

1. {{site.data.keyword.cloud_notm}} インフラストラクチャー環境で機能する、暗号化対象の非暗号化イメージを選択します。 既存の仮想サーバーを使用して[イメージ・テンプレートを作成する](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template)という選択肢もあります。 詳しくは、[cloud-init でプロビジョンした仮想サーバーから作成したイメージ・テンプレートを使用する方法](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server)を参照してください。 既存の VHD イメージを使用することもできます。 イメージが[暗号化イメージの要件](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs)を満たしていることを確認してください。
2. {{site.data.keyword.slportal}}にあるイメージ・テンプレートを使用する場合は、{{site.data.keyword.cos_full_notm}} に[その非暗号化イメージをエクスポート](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage)します。
3. {{site.data.keyword.cos_full_notm}} から安全なローカル・マシンにイメージ・ファイルをダウンロードし、そのイメージを暗号化します。 サービス・ダッシュボードで、**「ダウンロード」**アクションを選択してストレージからオブジェクトを取得します。 200 MB を超えるイメージをダウンロードするときには、Aspera 高速転送プラグインを使用すると良いでしょう。
4. vhd-util ツールを使用して、[VHD イメージを暗号化します](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image)。
5. {{site.data.keyword.cos_full_notm}} で、バケットにナビゲートし、**「オブジェクトの追加」**をクリックして、暗号化したイメージを[アップロード](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload)します。 200 MB を超えるイメージをアップロードするときには、Aspera 高速転送プラグインを使用すると良いでしょう。

## 暗号化イメージのインポートとインスタンスの注文

1. IBM {{site.data.keyword.iamshort}} (IAM) を使用して、暗号化イメージを {{site.data.keyword.cloud_notm}} コンソールにインポートする際の認証に使用するサービス ID を作成します。
      1. [サービス ID](/docs/iam?topic=iam-serviceids#serviceids) を作成します。
      2. [アクセス・ポリシー](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy)を割り当てます。 {{site.data.keyword.cos_full_notm}} と 鍵管理のサービスに対するアクセス権限を割り当てます。
      3. [サービス ID の API キー](/docs/iam?topic=iam-serviceidapikeys#create_service_key)を作成します。
      4. 詳しくは、[Introducing {{site.data.keyword.cloud_notm}} IAM Service IDs and API Keys ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/blog/introducing-ibm-cloud-iam-service-ids-api-keys){: new_window} を参照してください。
2. {{site.data.keyword.cloud_notm}} コンソールで、「イメージ・テンプレート」ページに[暗号化イメージをインポート](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos)します。
3. 「イメージ・テンプレート」ページから、暗号化イメージを使用して、仮想サーバー・インスタンスを[注文](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template)できます。
4. 暗号化された仮想サーバーをプロビジョンしたら、[アクティビティー・トラッカー](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov)を使用して、[仮想サーバー・イベント](/docs/vsi?topic=virtual-servers-at_events#at_events)を監査できます。
