[ホーム](../README.md) > Phase 2: アソシエイト

# Phase 2: アソシエイト ― 目標: SAA-C03

AWS の主要サービスを「仕組み」と「使い分け」のレベルまで理解し、**要件から標準的なアーキテクチャを設計できる** ようになるフェーズです。
AWS Certified Solutions Architect – Associate（SAA-C03）の合格を目指します。

SAA-C03 は SAP-C03 の土台です。Phase 3 では本フェーズの知識を前提に「組織全体」「複数リージョン」「ハイブリッド」へと視野を広げるため、ここで学ぶ定石は確実に身につけてください。

## このフェーズのゴール

- IAM のポリシー評価ロジックを説明し、クロスアカウントアクセスを設計できる
- VPC をゼロから設計でき、通信経路（ルートテーブル・SG・NACL・エンドポイント）を説明できる
- コンピューティング・ストレージ・データベースを要件に応じて選定できる
- 疎結合・サーバーレス・コンテナの設計パターンを使い分けられる
- 高可用性と DR の戦略を RPO/RTO とコストから選べる
- 「最もコスト効率が高い」「運用上のオーバーヘッドが最も少ない」といった制約から最適解を選べる
- SAA-C03 の模擬試験で 80% 以上を取れる

## 章の一覧

SAA-C03 のドメイン: **D1** セキュアなアーキテクチャの設計（30%）/ **D2** 弾力性に優れたアーキテクチャの設計（26%）/ **D3** 高性能なアーキテクチャの設計（24%）/ **D4** コストを最適化したアーキテクチャの設計（20%）

| # | 章 | 目安時間 | 主なドメイン | ハンズオン |
|---|---|---|---|---|
| 1 | [IAM 徹底解説](01-iam.md) | 5 時間 | D1 | [Lab 01](../04-labs/lab01-secure-account.md) |
| 2 | [VPC とネットワーク設計](02-vpc.md) | 7 時間 | D1, D2 | [Lab 02](../04-labs/lab02-vpc-ec2.md) |
| 3 | [Amazon EC2](03-ec2.md) | 5 時間 | D3, D4 | [Lab 02](../04-labs/lab02-vpc-ec2.md) |
| 4 | [Elastic Load Balancing と Auto Scaling](04-elb-auto-scaling.md) | 5 時間 | D2, D3 | [Lab 03](../04-labs/lab03-alb-auto-scaling.md) |
| 5 | [Amazon S3](05-s3.md) | 6 時間 | D1, D3, D4 | [Lab 04](../04-labs/lab04-s3-cloudfront.md) |
| 6 | [ブロック・ファイルストレージ](06-ebs-efs-fsx.md) | 4 時間 | D3, D4 | ― |
| 7 | [データベース](07-databases.md) | 7 時間 | D2, D3 | [Lab 05](../04-labs/lab05-rds-multi-az.md) |
| 8 | [Route 53・CloudFront・Global Accelerator](08-route53-cloudfront.md) | 5 時間 | D2, D3 | [Lab 04](../04-labs/lab04-s3-cloudfront.md) |
| 9 | [アプリケーション統合](09-application-integration.md) | 5 時間 | D2 | [Lab 09](../04-labs/lab09-event-driven.md) |
| 10 | [サーバーレス](10-serverless.md) | 6 時間 | D2, D3 | [Lab 06](../04-labs/lab06-serverless-api.md) |
| 11 | [コンテナ](11-containers.md) | 4 時間 | D2, D3 | ― |
| 12 | [セキュリティサービス](12-security-services.md) | 6 時間 | D1 | ― |
| 13 | [監視と運用管理](13-monitoring-management.md) | 5 時間 | D2, D3 | [Lab 07](../04-labs/lab07-cloudformation-iac.md), [Lab 08](../04-labs/lab08-cloudwatch-monitoring.md) |
| 14 | [データ分析サービス](14-analytics.md) | 4 時間 | D3 | ― |
| 15 | [高可用性と災害対策](15-high-availability-dr.md) | 4 時間 | D2 | ― |
| 16 | [移行とハイブリッド接続](16-migration-hybrid.md) | 4 時間 | D2, D3 | ― |
| 17 | [AWS Well-Architected Framework](17-well-architected.md) | 3 時間 | 全ドメイン | ― |
| 18 | [コスト最適化](18-cost-optimization.md) | 4 時間 | D4 | ― |
| 19 | [SAA-C03 試験対策](19-saa-exam-prep.md) | 3 時間 | 全ドメイン | ― |

> [!TIP]
> 1〜8 章（IAM・VPC・EC2・ELB・S3・ストレージ・DB・DNS/CDN）は SAA の出題の中心であり、Phase 3 でも前提になります。
> ここは時間をかけて、ハンズオンまで必ず実施してください。

## 模擬試験

- [SAA-C03 模擬試験 第 1 回（65 問 / 130 分）](../05-practice-exams/saa-mock-exam-1.md)
- [SAA-C03 模擬試験 第 2 回（65 問 / 130 分）](../05-practice-exams/saa-mock-exam-2.md)

第 1 回は 19 章を読み終えた直後に、第 2 回は弱点を復習した後の仕上げに使うのがおすすめです。

## 完了の目安

- [ ] 全章の確認問題で 80% 以上正解できる
- [ ] Lab 02〜09 を完了し、後片付けまで済ませた
- [ ] SAA-C03 模擬試験（第 1 回・第 2 回）で 80% 以上
- [ ] SAA-C03 に合格した

---
[← Phase 1: 基礎](../01-foundations/README.md) | [目次](../README.md) | [Phase 3: プロフェッショナル →](../03-professional/README.md)
