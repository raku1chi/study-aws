[ホーム](../README.md) > [Phase 1: 基礎](README.md) > 主要サービスツアー

# 主要サービスツアー

> **この章のゴール**
> - AWS の主要サービスをカテゴリに分類し、それぞれを「一言で」説明できる
> - 問題文のユースケースから、適切なサービスを選べる
> - 廃止・名称変更・新規受付終了になったサービスを知り、古い情報に惑わされない
>
> **対応試験**: CLF-C02（第3分野: クラウドテクノロジーとサービス — タスク 3.1、3.3〜3.8）
> **目安時間**: 読む 200分 / 確認問題 40分

## この章の全体像

AWS には非常に多くのサービスがありますが、CLF-C02 で問われるのは「そのサービスが何をするものか」と「どんな場面で使うか」です。細かい設定を覚える必要はありません。まず、典型的な Web システムで主要なサービスがどこに位置するかを見てみましょう。

```mermaid
flowchart LR
  User["利用者"] -->|"名前解決"| R53["Route 53<br/>(DNS)"]
  User -->|"HTTPS"| CF["CloudFront + WAF<br/>(配信と防御)"]
  CF --> S3["S3<br/>(画像・静的ファイル)"]
  subgraph VPC["VPC（東京リージョン）"]
    ALB["Application Load Balancer"]
    EC2A["EC2<br/>(AZ a)"]
    EC2C["EC2<br/>(AZ c)"]
    RDS["RDS / Aurora<br/>(データベース)"]
    EC["ElastiCache<br/>(キャッシュ)"]
  end
  CF --> ALB
  ALB --> EC2A
  ALB --> EC2C
  EC2A --> RDS
  EC2C --> RDS
  EC2A --> EC
  CW["CloudWatch<br/>(監視)"] -.-> EC2C
```

この章の表では、各サービスを **一言で**（何をするか）、**ユースケース**（どんな場面で使うか）、**CLF のキーワード**（問題文や選択肢に出やすい言葉）の観点で紹介します。サービスの状態は次の表記で示します（2026年9月時点）。

- 【新規受付終了】既存の利用者は使い続けられるが、新しい利用者は使い始められない（メンテナンス段階）
- 【サポート終了予定】サービスの終了日が発表されている
- 【名称変更】新しい名前で提供されている

## 1. AWS を操作する方法

| 方法 | 一言で | 使いどころ |
|---|---|---|
| AWS マネジメントコンソール | Web ブラウザで操作する画面 | 初めての操作、状態の確認 |
| AWS CLI | コマンドラインから操作するツール | 繰り返しの作業、スクリプトによる自動化 |
| AWS SDK | プログラム（Python、Java、JavaScript など）から AWS の API を呼ぶためのライブラリ | アプリケーションからの操作 |
| AWS CloudShell | ブラウザから使えるシェル。AWS CLI が導入済みで、追加料金なし | 手元に環境を用意せずに CLI を使う |
| Infrastructure as Code（IaC） | テンプレートやコードでインフラを定義する | AWS CloudFormation や AWS CDK による、再現性のある構築 |

コンソール、CLI、SDK のどれを使っても、最終的には同じ AWS の API を呼び出しています。そのため、どの方法で操作しても AWS CloudTrail に記録されます。オンプレミスとの接続手段（インターネット、AWS Site-to-Site VPN、AWS Direct Connect）は 5 節で扱います。

## 2. コンピューティング

| サービス | 一言で | ユースケース | CLF のキーワード |
|---|---|---|---|
| Amazon EC2 | 仮想サーバー（IaaS） | OS やミドルウェアを自由に選ぶ Web・業務サーバー | インスタンスタイプ、AMI、OS の管理は利用者 |
| Amazon EC2 Auto Scaling | EC2 インスタンスの台数を自動で増減 | 需要の変動への対応、故障したインスタンスの自動置き換え | 弾力性、スケールアウト / スケールイン |
| Elastic Load Balancing（ELB） | 複数のターゲットに通信を分散 | 複数の AZ の EC2 インスタンスへの振り分け | ALB（HTTP/HTTPS）、NLB（TCP/UDP・高性能）、GWLB（仮想アプライアンス） |
| AWS Lambda | イベントに応じてコードを実行（FaaS） | ファイルのアップロード時の処理、API のバックエンド | サーバーレス、実行時間に応じた課金、最大実行時間 15 分 |
| Amazon ECS | AWS 独自のコンテナオーケストレーション | コンテナ化したアプリケーションの実行 | タスク、サービス |
| Amazon EKS | マネージドな Kubernetes | Kubernetes の標準的なツールで運用したい場合 | Kubernetes |
| AWS Fargate | コンテナ用のサーバーレスな実行環境 | サーバー（EC2）を管理せずにコンテナを実行 | ECS や EKS と組み合わせて使う |
| Amazon ECR | コンテナイメージのレジストリ | イメージの保存、脆弱性スキャン | Docker イメージ |
| Amazon ECS Express Mode | Fargate 上のサービス、ALB、自動スケーリング、HTTPS の URL を 1 ステップで作成 | コンテナ化した Web アプリを手早く公開 | 2025年11月提供開始。App Runner の推奨後継 |
| AWS Elastic Beanstalk | コードをアップロードするだけで実行環境を自動構築（PaaS） | Java、.NET、PHP、Node.js、Python、Ruby、Go、Docker のアプリのデプロイ | インフラ管理の負担を減らしつつ、基盤の設定も変更できる |
| Amazon Lightsail | 月額料金のシンプルな仮想サーバー | 小規模な Web サイト、WordPress、開発環境 | シンプル、料金を予測しやすい |
| AWS Batch | バッチ処理のジョブを実行する基盤 | 大量の計算ジョブ（シミュレーション、解析） | ジョブキュー、スポットインスタンスとの組み合わせ |
| AWS Outposts | AWS のインフラをオンプレミスに設置 | 超低レイテンシー、データの所在要件 | ハイブリッド（[05 章](05-global-infrastructure.md)） |
| AWS App Runner | 【新規受付終了】ソースコードやコンテナから Web アプリを自動デプロイ | 既存の利用者のみ | 2026年4月30日に新規受付終了。後継は ECS Express Mode |

> [!TIP]
> **試験のポイント**: 「OS を細かく制御したい」→ EC2、「サーバーを管理せずにイベントでコードを実行」→ Lambda、「サーバーを管理せずにコンテナを実行」→ Fargate、「コードをアップロードするだけ」→ Elastic Beanstalk、「月額料金のシンプルなサーバー」→ Lightsail です。EC2 の購入オプション（オンデマンド、Savings Plans、リザーブドインスタンス、スポットインスタンス）は [08 章](08-billing-pricing-support.md) で学びます。

> [!WARNING]
> **ひっかけ注意**: Lambda の 1 回の実行は最大 15 分です。何時間もかかる処理には、Amazon ECS（AWS Fargate）や AWS Batch を使います。

## 3. ストレージ

| サービス | 一言で | ユースケース | CLF のキーワード |
|---|---|---|---|
| Amazon S3 | 容量無制限のオブジェクトストレージ | 静的 Web サイト、バックアップ、データレイク | バケット、オブジェクト、99.999999999%（イレブンナイン）の耐久性、ストレージクラス、ライフサイクル |
| Amazon EBS | EC2 インスタンス用のブロックストレージ | OS のディスク、データベースのデータ | AZ 単位、スナップショット |
| インスタンスストア | EC2 のホストに物理的に接続された一時的なブロックストレージ | キャッシュ、一時データ | インスタンスを停止・終了するとデータが消える |
| Amazon EFS | 複数の Linux サーバーから同時に使える共有ファイルストレージ | Web コンテンツの共有、コンテナの永続ストレージ | NFS、容量が自動で伸縮、マルチ AZ |
| Amazon FSx | 高機能なファイルシステムのフルマネージド版 | Windows のファイル共有、HPC、NetApp ONTAP からの移行 | FSx for Windows File Server（SMB、Active Directory 連携）、FSx for Lustre（HPC）、FSx for NetApp ONTAP、FSx for OpenZFS |
| AWS Storage Gateway | オンプレミスとクラウドのストレージをつなぐハイブリッドストレージ | オンプレミスから S3 をファイル共有として使う、テープバックアップの置き換え | S3 File Gateway、Volume Gateway、Tape Gateway（FSx File Gateway は新規利用不可） |
| AWS Backup | 複数のサービスのバックアップを一元管理 | EC2、EBS、RDS、DynamoDB、EFS などのバックアップ方針を統一 | バックアッププラン、クロスリージョンコピー |
| AWS Elastic Disaster Recovery | サーバーを AWS に継続的に複製し、災害時に復旧 | オンプレミスや他のクラウドのサーバーの災害対策 | 低コストの DR、短い RPO / RTO |

S3 では、アクセス頻度に応じて **ストレージクラス** を選ぶことでコストを最適化できます。

| ストレージクラス | 向いているデータ |
|---|---|
| S3 Standard | 頻繁にアクセスするデータ |
| S3 Intelligent-Tiering | アクセスパターンが不明、または変化するデータ（自動で適切な階層へ移動） |
| S3 Standard-IA | アクセスは少ないが、必要なときはすぐに取り出したいデータ |
| S3 One Zone-IA | 再作成できるデータ（1 つの AZ だけに保存するため安価） |
| S3 Glacier Instant Retrieval | めったにアクセスしないが、ミリ秒で取り出したいアーカイブ |
| S3 Glacier Flexible Retrieval | 取り出しに数分〜数時間かかってもよいアーカイブ |
| S3 Glacier Deep Archive | 最も安価。取り出しに半日程度かかってもよい長期保存（コンプライアンス目的など） |
| S3 Express One Zone | 1 つの AZ で、1 桁ミリ秒の高い性能が必要なデータ |

> [!TIP]
> **試験のポイント**: ストレージの種類で選びます。**ブロック** → EBS、**ファイル** → EFS（Linux / NFS）や FSx（Windows / SMB など）、**オブジェクト** → S3 です。「複数の EC2 インスタンスから同時にマウントしたい（Linux）」→ EFS、「Windows のファイル共有」→ FSx for Windows File Server、「アクセス頻度が予測できない」→ S3 Intelligent-Tiering です。

> [!WARNING]
> **ひっかけ注意**: 元祖の S3 Glacier の「ボールト」を直接使う方式は新規受付を終了していますが、**S3 の Glacier ストレージクラスは引き続き利用できます**。「長期アーカイブには S3 Glacier Deep Archive」という考え方は変わりません。

## 4. データベース

| サービス | 一言で | ユースケース | CLF のキーワード |
|---|---|---|---|
| Amazon RDS | マネージドなリレーショナルデータベース | 業務システムや Web アプリの DB | MySQL、PostgreSQL、MariaDB、Oracle、SQL Server、Db2。マルチ AZ、リードレプリカ、自動バックアップ |
| Amazon Aurora | クラウド向けに設計された高性能なリレーショナル DB | 高いスループットと可用性が必要な DB | MySQL / PostgreSQL 互換、3 つの AZ に 6 つのコピー、Aurora Serverless |
| Amazon Aurora DSQL | サーバーレスの分散 SQL データベース | 複数のリージョンでのアクティブ / アクティブ構成 | PostgreSQL 互換、2025年5月 GA |
| Amazon DynamoDB | フルマネージドでサーバーレスな NoSQL データベース | 大規模な Web・モバイル・ゲーム、セッション管理 | キーバリュー、1 桁ミリ秒の応答、自動スケーリング、グローバルテーブル |
| Amazon ElastiCache | インメモリのキャッシュ | DB の読み取り負荷の軽減、セッションの保存 | Valkey / Redis OSS / Memcached、マイクロ秒単位の応答 |
| Amazon MemoryDB | 耐久性のあるインメモリデータベース | 超高速で、かつデータを失えない用途 | Valkey / Redis OSS 互換 |
| Amazon DocumentDB（MongoDB 互換） | ドキュメントデータベース | JSON ドキュメントを扱うアプリ、MongoDB からの移行 | MongoDB 互換 |
| Amazon Neptune | グラフデータベース | SNS のつながり、レコメンデーション、不正検知 | ノードとリレーションシップ |
| Amazon Keyspaces（for Apache Cassandra） | Cassandra 互換のサーバーレス DB | Cassandra からの移行 | CQL |
| Amazon Timestream | 時系列データベース | IoT のセンサー値、運用メトリクス | Timestream for InfluxDB（Timestream for LiveAnalytics は 2025年6月から新規受付終了） |

> [!TIP]
> **試験のポイント**: 「リレーショナル、複雑な結合、トランザクション」→ RDS / Aurora、「スキーマが柔軟、大規模、ミリ秒単位の応答、サーバーレス」→ DynamoDB、「読み取りの高速化、キャッシュ」→ ElastiCache、「大量データの分析（データウェアハウス）」→ Amazon Redshift（9 節）です。また、EC2 に自分で DB をインストールすると OS や DB のパッチ適用、バックアップは利用者の責任になりますが、RDS ならこれらを AWS に任せられます。

> [!WARNING]
> **ひっかけ注意**: 古い教材で台帳データベースとして登場する Amazon QLDB は、2025年7月31日にサービスを終了しました。

## 5. ネットワークとコンテンツ配信

| サービス | 一言で | ユースケース | CLF のキーワード |
|---|---|---|---|
| Amazon VPC | AWS 上の、論理的に分離されたプライベートネットワーク | サブネットの設計、インターネットからの分離 | サブネット、ルートテーブル、インターネットゲートウェイ、NAT ゲートウェイ、セキュリティグループ、ネットワーク ACL |
| Amazon Route 53 | DNS とドメイン登録 | ドメイン名の名前解決、ヘルスチェックによるフェイルオーバー | ルーティングポリシー（シンプル、加重、レイテンシー、フェイルオーバー、位置情報など） |
| Amazon CloudFront | CDN（コンテンツ配信ネットワーク） | 静的・動的コンテンツの高速配信 | エッジロケーション、キャッシュ |
| AWS Global Accelerator | AWS のグローバルネットワーク経由で、アプリへの通信を高速化 | 世界中からの TCP / UDP 通信、固定 IP アドレスが必要な場合 | エニーキャストの固定 IP アドレス、リージョン間の迅速なフェイルオーバー |
| AWS Direct Connect | オンプレミスと AWS をつなぐ専用線 | 大容量で安定したハイブリッド接続 | 一貫したネットワーク性能、インターネットを経由しない |
| AWS Site-to-Site VPN | インターネット経由の IPsec VPN | 素早く安価にオンプレミスと接続、Direct Connect のバックアップ | 暗号化トンネル |
| AWS Client VPN | 個人の端末から AWS に接続する VPN | リモートワーク | OpenVPN ベースのクライアント |
| AWS Transit Gateway | 多数の VPC とオンプレミスをつなぐハブ | ハブ&スポーク型のネットワーク | VPC ピアリングの網の目を解消 |
| VPC ピアリング | 2 つの VPC を 1 対 1 で接続 | 少数の VPC 間の通信 | 推移的なルーティングはできない |
| VPC エンドポイント | インターネットを経由せずに、AWS のサービスや他の VPC のサービスへ接続 | プライベートなサブネットから S3 や DynamoDB を使う | インターフェイスエンドポイント（AWS PrivateLink）、ゲートウェイエンドポイント（S3・DynamoDB） |

> [!TIP]
> **試験のポイント**: 「一貫した帯域幅、インターネットを経由しない専用接続」→ Direct Connect、「短期間・低コストで暗号化された接続」→ Site-to-Site VPN、「世界中の利用者に、コンテンツをキャッシュして配信」→ CloudFront です。

> [!WARNING]
> **ひっかけ注意**: Direct Connect の通信は、既定では暗号化されません。暗号化が必要なら、Direct Connect の上に VPN を組み合わせるなどの方法をとります。

## 6. セキュリティ・ID・コンプライアンス

詳しくは [07 章](07-security-and-compliance.md) で学ぶので、ここでは役割だけを押さえます。

| サービス | 一言で | CLF のキーワード |
|---|---|---|
| AWS IAM | AWS へのアクセスを制御（ユーザー、グループ、ロール、ポリシー） | 最小権限、MFA、追加料金なし |
| AWS IAM Identity Center（旧 AWS SSO） | 複数のアカウントや業務アプリへのシングルサインオン | 社員のアクセスを一元管理、許可セット |
| Amazon Cognito | 自社の Web・モバイルアプリの利用者向けの認証 | サインアップ / サインイン、ソーシャルログイン |
| AWS Directory Service | マネージドな Microsoft Active Directory | AWS Managed Microsoft AD、AD Connector |
| AWS KMS | 暗号鍵の作成と管理 | S3、EBS、RDS などの暗号化、キーポリシー |
| AWS CloudHSM | 専用のハードウェアセキュリティモジュール（HSM） | シングルテナント、鍵を完全に自己管理 |
| AWS Secrets Manager | DB のパスワードなど、シークレットの保管と自動ローテーション | 認証情報のハードコードをなくす |
| AWS Certificate Manager（ACM） | SSL/TLS 証明書の発行と管理 | ELB や CloudFront の HTTPS 化、自動更新 |
| AWS WAF | Web アプリケーションファイアウォール | SQL インジェクション、クロスサイトスクリプティング（XSS）、レート制限 |
| AWS Shield | DDoS 攻撃からの保護 | Standard は全利用者に自動・無料で適用。Advanced は有料で、高度な保護と専門チームの支援 |
| AWS Firewall Manager | 組織全体のファイアウォールルールを一元管理 | AWS Organizations と連携、WAF やセキュリティグループのルールを一括適用 |
| AWS Network Firewall | VPC 用のマネージドなネットワークファイアウォール | VPC の境界での通信の検査 |
| Amazon GuardDuty | ログを機械学習などで分析する脅威検出 | 不審な API 呼び出し、マルウェア、暗号資産のマイニング |
| Amazon Inspector | 脆弱性の自動スキャン | EC2、コンテナイメージ、Lambda の脆弱性（CVE） |
| Amazon Macie | S3 内の機密データ（個人情報など）を検出 | 機械学習、個人を特定できる情報（PII） |
| Amazon Detective | セキュリティの調査と根本原因の分析 | 検出結果の深掘り、関係の可視化 |
| AWS Security Hub | セキュリティの検出結果を集約し、優先順位を付ける | 2025年12月に統合版が GA。従来のセキュリティ標準のチェック機能は AWS Security Hub CSPM |
| AWS Artifact | AWS のコンプライアンスレポートと契約をダウンロード | SOC、ISO、PCI などの第三者監査レポート、セルフサービス |
| AWS Audit Manager | 【新規受付終了】監査用の証跡（エビデンス）を自動収集 | 2026年3月に新規受付の終了を発表 |

## 7. 管理とガバナンス

| サービス | 一言で | CLF のキーワード |
|---|---|---|
| Amazon CloudWatch | メトリクス、ログ、アラームによる監視 | CPU 使用率のアラーム、ダッシュボード、CloudWatch Logs |
| AWS CloudTrail | AWS の API 呼び出しを記録（誰が、いつ、何をしたか） | 監査、操作の追跡、イベント履歴（90 日）、証跡 |
| AWS Config | リソースの設定の変更履歴を記録し、ルールで評価 | 構成の履歴、Config ルール、コンプライアンスの評価 |
| AWS Systems Manager | サーバーの運用管理をまとめたサービス | Session Manager（SSH を開けずに接続）、Patch Manager、Run Command、Parameter Store、Automation（Change Manager と Incident Manager は新規受付終了） |
| AWS CloudFormation | テンプレート（JSON / YAML）でインフラをコードとして定義（IaC） | スタック、同じ環境を何度でも再現 |
| AWS Organizations | 複数のアカウントを一元管理 | 一括請求（コンソリデーティッドビリング）、OU、SCP |
| AWS Control Tower | ベストプラクティスに沿ったマルチアカウント環境を自動構築 | ランディングゾーン、コントロール（ガードレール） |
| AWS Service Catalog | 承認済みの構成（製品）をカタログ化し、利用者がセルフサービスで起動 | ガバナンス、標準化 |
| AWS Trusted Advisor | ベストプラクティスに沿っているかを自動でチェック | コスト最適化、パフォーマンス、セキュリティ、耐障害性、サービスクォータ、運用上の優秀性。使えるチェックはサポートプランで異なる |
| AWS Health Dashboard | AWS のサービスの障害や、予定されたメンテナンスを通知 | 自分のアカウントに影響するイベント |
| AWS Compute Optimizer | 使用状況を分析し、リソースのサイズを推奨 | ライトサイジング、EC2、EBS、Lambda |
| AWS Well-Architected Tool | Well-Architected のセルフレビュー | 6 つの柱、追加料金なし |
| AWS License Manager | ソフトウェアライセンスの使用状況を管理 | BYOL、ライセンス違反の防止 |

> [!TIP]
> **試験のポイント**: 監視系の 3 つは必ず区別できるようにしましょう。
> - 「CPU 使用率などの **性能** を監視してアラームを出す」→ **CloudWatch**
> - 「**誰が・いつ・どの API を** 呼び出したか」→ **CloudTrail**
> - 「リソースの **設定が** どう変わったか、ルールに **準拠** しているか」→ **Config**

コスト管理のツール（AWS Cost Explorer、AWS Budgets、AWS Pricing Calculator など）は [08 章](08-billing-pricing-support.md) で扱います。

## 8. アプリケーション統合

| サービス | 一言で | ユースケース | CLF のキーワード |
|---|---|---|---|
| Amazon SQS | フルマネージドなメッセージキュー | コンポーネント間の疎結合、処理の平準化 | 標準キュー / FIFO キュー、最大メッセージサイズ 1 MiB |
| Amazon SNS | Pub/Sub 型の通知サービス | メール・SMS・プッシュ通知、SQS や Lambda への一斉配信（ファンアウト） | トピック、サブスクリプション |
| Amazon EventBridge | イベントバスとスケジューラー | AWS のサービスや SaaS のイベントを条件に応じて振り分ける、定期実行 | ルール、イベントバス、EventBridge Scheduler |
| AWS Step Functions | ワークフローのオーケストレーション | 複数の Lambda 関数を、順番・分岐・再試行付きで実行 | ステートマシン、視覚的なワークフロー |
| Amazon MQ | マネージドなメッセージブローカー（Apache ActiveMQ / RabbitMQ） | 既存システムを業界標準のプロトコルのまま移行 | JMS、AMQP、MQTT などの標準プロトコル |
| Amazon API Gateway | API の作成・公開・管理 | Lambda と組み合わせたサーバーレス API | REST / HTTP / WebSocket API、スロットリング |
| AWS AppSync | マネージドな GraphQL API | Web・モバイルアプリのデータ取得、リアルタイムの更新 | GraphQL |
| Amazon AppFlow | SaaS と AWS の間のデータ連携 | Salesforce などのデータを S3 に転送 | ノーコード、SaaS 連携 |

> [!TIP]
> **試験のポイント**: 「コンポーネントを **疎結合** にしたい」「処理を **キュー** にためる」→ SQS、「1 つのメッセージを複数の宛先へ（**ファンアウト**）」→ SNS、「イベントの内容に応じて **ルーティング**」→ EventBridge、「複数の処理を **順番に・条件分岐付きで** 実行」→ Step Functions です。

> [!WARNING]
> **ひっかけ注意**: SQS の最大メッセージサイズは、2025年8月に 256 KiB から **1 MiB** に拡大されました。古い教材や試験問題では「256 KB」と書かれている場合があります。

## 9. 分析

| サービス | 一言で | ユースケース | CLF のキーワード |
|---|---|---|---|
| Amazon Athena | S3 上のデータに標準 SQL で直接クエリ | ログのアドホックな分析 | サーバーレス、スキャンしたデータ量に応じた課金 |
| Amazon Redshift | データウェアハウス | 大量データの集計、BI | 列指向、Redshift Serverless |
| Amazon EMR | ビッグデータ処理の基盤（Apache Spark、Hadoop など） | 大規模な ETL、機械学習の前処理 | マネージドな Hadoop / Spark |
| AWS Glue | サーバーレスなデータ統合（ETL）とデータカタログ | データの抽出・変換・ロード、メタデータの管理 | クローラー、AWS Glue Data Catalog |
| Amazon Kinesis Data Streams | ストリーミングデータをリアルタイムに収集 | クリックストリーム、IoT のデータ | リアルタイム、シャード |
| Amazon Data Firehose（旧 Kinesis Data Firehose） | ストリーミングデータを S3、Redshift、OpenSearch Service などへ配信 | ログを S3 に自動で保存 | フルマネージド、ほぼリアルタイム |
| Amazon Managed Service for Apache Flink（旧 Kinesis Data Analytics） | ストリーミングデータをリアルタイムに処理 | 異常検知、リアルタイムの集計 | Apache Flink |
| Amazon MSK | マネージドな Apache Kafka | Kafka を使う既存システムの移行 | Kafka |
| Amazon OpenSearch Service（旧 Amazon Elasticsearch Service） | 検索とログ分析 | 全文検索、ログの可視化 | OpenSearch、ダッシュボード |
| Amazon Quick Suite（旧 Amazon QuickSight を含む） | BI（ダッシュボード・可視化）と、業務向けの生成 AI 機能をまとめたサービス | 経営ダッシュボード、データに基づく調査 | 【名称変更】2025年10月に QuickSight が Quick Suite へ再編（BI 機能は Amazon Quick Sight） |
| AWS Lake Formation | データレイクの構築と、アクセス権限の一元管理 | S3 のデータレイクへのきめ細かなアクセス制御 | データレイク、ガバナンス |
| AWS Data Exchange | サードパーティのデータを検索・購読 | 市場データや気象データの取得 | データ製品 |
| AWS Clean Rooms | 生データを互いに見せずに、複数の組織で共同分析 | 広告主とメディアの共同分析 | プライバシー保護 |

> [!TIP]
> **試験のポイント**: 「S3 のデータにサーバーレスで SQL」→ Athena、「データウェアハウス」→ Redshift、「ETL」「データカタログ」→ Glue、「リアルタイムのストリーミングデータ」→ Kinesis、「BI ダッシュボード」→ Quick Sight（旧 QuickSight）、「Hadoop / Spark」→ EMR です。試験では旧名称の QuickSight で出題される可能性があります。

## 10. AI / 機械学習

AWS の AI サービスは、**生成 AI とアシスタント**（基盤モデルを使ったアプリやエージェント）、**学習済みの AI サービス**（機械学習の知識がなくても API を呼ぶだけで使える）、**ML プラットフォーム**（独自のモデルを構築する）の 3 つの層に分けると整理しやすくなります。

| サービス | 一言で | ユースケース | CLF のキーワード |
|---|---|---|---|
| Amazon Bedrock | さまざまな企業の基盤モデルを API で使える、フルマネージドな生成 AI サービス | チャットボット、要約、社内文書を使った回答（RAG） | 基盤モデル（Anthropic Claude、Amazon Nova、Meta Llama など）、Knowledge Bases、Guardrails、インフラ管理不要 |
| Amazon Bedrock AgentCore | AI エージェントを安全に本番運用するための基盤 | 社内システムを操作する AI エージェント | Runtime、Memory、Gateway、Identity など（2025年10月 GA）。従来の Bedrock Agents は「Agents Classic」として新規受付終了 |
| Amazon SageMaker AI（旧 Amazon SageMaker） | 機械学習モデルの構築・学習・デプロイの基盤 | 独自モデルの開発 | 【名称変更】ノートブック、学習、推論エンドポイント、SageMaker Canvas（ノーコード） |
| Amazon Q | AWS の生成 AI アシスタント | AWS の使い方の質問、開発や業務の支援 | コンソールやチャットアプリの Amazon Q は継続。Amazon Q Developer は 2026年5月に新規サインアップを停止し、IDE プラグインは 2027年4月30日にサポート終了予定（後継は Kiro）。Amazon Q Business は 2026年6月に新規受付の終了を発表 |
| Kiro | AI エージェントを組み込んだ開発環境（IDE） | 仕様からの開発、コード生成 | 2025年11月 GA |
| Amazon Rekognition | 画像・動画の分析 | 顔の検出と比較、物体の検出、不適切なコンテンツの検出 | コンピュータビジョン |
| Amazon Comprehend | 自然言語処理（NLP） | 感情分析、キーフレーズや固有表現の抽出、言語の判定 | テキスト分析、個人情報の検出 |
| Amazon Transcribe | 音声をテキストに変換（文字起こし） | コールセンターの録音の文字起こし、字幕 | 音声認識 |
| Amazon Polly | テキストを音声に変換（音声合成） | 読み上げ、音声案内 | 音声合成 |
| Amazon Translate | 機械翻訳 | Web サイトやチャットの多言語化 | ニューラル機械翻訳 |
| Amazon Textract | 文書から文字、表、フォームを抽出 | 請求書や申込書のデータ化 | OCR を超えた、文書の構造の理解 |
| Amazon Lex | 音声やテキストの会話インターフェイス（チャットボット） | 問い合わせ対応ボット、Amazon Connect との連携 | インテント、Alexa と同じ深層学習技術 |
| Amazon Personalize | 機械学習によるレコメンデーション | EC サイトの「おすすめ商品」 | パーソナライズ |
| Amazon Kendra | 【新規受付終了】機械学習を使ったエンタープライズ検索 | 社内文書の自然言語検索 | 2026年6月に新規受付の終了を発表。既存の利用者のみ |

> [!TIP]
> **試験のポイント**: 名前と機能の対応が狙われます。**Transcribe** は「書き起こす（transcribe）」で音声 → テキスト、**Polly** はおしゃべりなオウム（ポリー）でテキスト → 音声、**Textract** は「テキストを抽出（extract）」、**Rekognition** は「認識（recognition）」で画像・動画です。「インフラを管理せずに、基盤モデルで生成 AI アプリを作りたい」→ Bedrock、「独自の ML モデルを構築・学習したい」→ SageMaker AI です。

> [!WARNING]
> **ひっかけ注意**: 試験や古い教材では、「社内文書の検索」に Amazon Kendra、「業務向けの生成 AI アシスタント」に Amazon Q Business、「エージェント」に Agents for Amazon Bedrock が正解として登場する可能性があります。いずれも現在は新規受付を終了しているため、新しく構成する場合は Amazon Bedrock Knowledge Bases（RAG）や Amazon Bedrock AgentCore などを検討します。
