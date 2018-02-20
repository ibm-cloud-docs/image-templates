---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# cloud-init 対応イメージでのプロビジョニング

仮想サーバーをオーダーする場合、現在、多くのオペレーティング・システムでは、cloud-init 対応イメージを使用してプロビジョニング時間の最適化が行われています。また、cloud-init 対応のカスタマイズ・イメージをインポートすることもできます。
{:shortdesc}

現在、以下のオペレーティング・システムでは、追加のソフトウェア、ポストプロビジョニング・スクリプト、または拡張モニタリングを選択せずに仮想サーバーをオーダーするとき、cloud-init 対応イメージがデフォルトになります。
* CentOS 7
* Debian 8
* Ubuntu 16
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

cloud-init 対応のオペレーティング・システムで仮想サーバーをオーダーするとき、カスタム・プロビジョニング・スクリプトを使用してユーザー・データまたはメタデータを追加することができます。オーダー・フォームの「ユーザー・データ」フィールドに、サーバーのオプション cloud-init ユーザー・データまたはオプション・メタデータを入力します。 

## カスタマイズされた cloud-init 対応イメージのインポート

cloud-init 対応のカスタマイズ・イメージを作成した場合は、カスタム・ポータルの「イメージのインポート」ページで、そのイメージを cloud-init イメージに指定することができます。

イメージ・テンプレートの「イメージのインポート」ページにアクセスし、イメージに cloud-init 対応のマークを付けるには、以下の手順を実行します。
1. **「デバイス」**メニューから、**「管理」** > **「イメージ」**を選択します。
2. **「イメージのインポート」**タブをクリックします。
3. cloud-init 対応イメージをインポートするために必要な情報を入力し、**「オペレーティング・システム」**ドロップダウン・ボックスの近くに表示されている**「Cloud init」**チェック・ボックスを選択します。イメージのインポートについて詳しくは、「イメージのインポート」を参照してください。

## イメージ・テンプレートを cloud-init 対応としてマーク付け

cloud-init 対応の、既存の VHD イメージ・テンプレートがある場合は、イメージ・テンプレートの詳細ページで、それを cloud-init 対応に指定できます。

イメージ・テンプレートにアクセスし、それに cloud-init 対応のマークを付けるには、以下の手順を実行します。
1. **「デバイス」**メニューから、**「管理」** > **「イメージ」**を選択します。
2. テンプレートのリストから、更新するイメージ・テンプレート名をクリックします。
3. 「イメージ・テンプレートの詳細」ページで、**「Cloud Init」**ヘッダーの下の**「有効」**チェック・ボックスを選択し、**「更新」**をクリックします。

## cloud-init でプロビジョンされた仮想サーバーから作成された標準イメージの処理

Cloud-init が実行されるのは、通常、1 回のみです。ただし、cloud-init 対応イメージから仮想サーバーをプロビジョンし、その後、その仮想サーバーから標準イメージ・テンプレートを作成した場合、UUID が記録されます。その標準イメージ・テンプレートを使用して別の仮想サーバーを作成すると、cloud-init が再度実行されます。

## cloud-init 対応のイメージ・テンプレートの作成

イメージの構成については、[cloud-init の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloudinit.readthedocs.io/en/latest/) を参照してください。

データ・ソースについて詳しくは、[Datasources ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html) を参照してください。{{site.data.keyword.cloud}} cloud-init イメージは、メタデータを提供するために [Config Drive ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - バージョン 2 データ・ソースを使用して、環境用に作成されます。

### Linux 要件
* Cloud-init バージョン 0.7.7 以上

### Windows 要件
* {{site.data.keyword.cloud_notm}} インフラストラクチャー内に、パブリック・ネットワークおよびプライベート・ネットワークをサポートするための Cloudbase-init メタデータ・サービス。このサービスは、Windows 仮想サーバーの資格情報を使用してカスタマー・ポータルの更新も行います。このサービスには、[https://github.com/softlayer/bluemix-cloudbase-init ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/softlayer/bluemix-cloudbase-init) からアクセスできます。
* ご使用環境で Vyatta を使用している場合は、API ロード・バランサーへの API 呼び出しを許可するように Vyatta を構成する必要があります。詳細情報については、[Brocade vRouter (Vyatta) Set up Guide for VMware Environments with Endurance or Performance Storage ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://knowledgelayer.softlayer.com/content/brocade-vrouter-vyatta-set-guide-vmware-environments-endurance-or-performance-storage) を参照してください。

