# インフラ仕様（検証環境）

入力: `construction/nfr-requirements/` の全文書と `inception/requirements-analysis/requirements.md`。NFR Design、Domain Design、Functional Design はプランで省略しているため、構成は NFR Requirements から直接導いています。

検証シナリオの本文は [docs/verification-scenario-privatelink-vs-lattice.md](../../../../../../../docs/verification-scenario-privatelink-vs-lattice.md) です。この文書は、その設計上の判断をまとめたものです。

## Deployment

| Facet | Choice | Rationale |
|-------|--------|-----------|
| 計算基盤 | 呼び出し先は Fargate 上の ECS（nginx のタスク 1 つ）。呼び出し元は EC2 2 台（許可用と拒否用） | 制約 C-T2、C-T3。拒否用は NFR Q4 |
| ネットワーク | 同じアカウントの 2 つの VPC。IPv6 のみで、Egress-only インターネットゲートウェイを使う。例外は NLB のサブネット（デュアルスタック） | C-T1、決定 D-16 |
| アベイラビリティーゾーン | すべて 1 つの AZ | NFR3.3 |
| ストレージ | なし | 状態を持たない検証 |
| 環境 | 検証用の環境 1 つ | 文書のみの成果物 |
| IaC | 作らない。コンソールまたは CLI の手順として書く | 要件分析 AS1、本ステージ Q1 |
| サイズ | EC2 は m7i.large、タスクは 0.5 vCPU / 1 GB × 1 | NFR2.3 |

## Infrastructure Services

| Service | Role | Configuration | Notes |
|---------|------|---------------|-------|
| AWS PrivateLink（エンドポイントサービス） | 経路 1 | IPv6 に対応させ、承認を必須にする | NFR4.7 |
| NLB | load-balancer | 内部向け、デュアルスタック、TLS 443。ターゲットグループは IPv6 の 2 つ（8080 は PPv2 あり、8081 はなし） | NFR4.8、NFR4.9 |
| インターフェイスエンドポイント | 経路 1 の入口 | IPv6 のみのサブネットに置き、IP アドレスタイプは IPv6。`sg-endpoint` を付ける | NFR4.5 |
| Amazon VPC Lattice | 経路 2 | サービスネットワークとサービス（カスタムドメイン、HTTPS 443）。ターゲットグループは IPv6（8081）。VPC の関連付けに `sg-lattice-assoc` を付ける | NFR4.5〜4.7、4.10 |
| Route 53 プライベートホストゾーン | dns | `pl.<YOUR_DOMAIN>`（AAAA エイリアス）、`lattice.<YOUR_DOMAIN>`（CNAME） | NFR4.3 |
| ACM | 証明書 | 上の 2 つの名前 | NFR4.3 |
| ECR | イメージ置き場 | デュアルスタックエンドポイントから取得する | NFR1.2 |

## 判断の記録（代替案との比較）

| 判断 | 採用 | 見送った案と理由 |
|------|------|------------------|
| PrivateLink のターゲットグループを 2 つにする | 8080（PPv2 あり）と 8081 の 2 つを用意し、リスナーの転送先を切り替える | 1 つのターゲットグループで PPv2 の設定を切り替える案: 切り替えの待ち時間が読みにくく、手順も増えるため |
| 測定に curl を使う | curl の設定ファイルで 1 プロセスから送る | hey や oha: IPv4 だけの取得元に依存するため |
| セキュリティ上の影響 | 拒否用 EC2 には、別のセキュリティグループと別の IAM ロールを付ける。認証ポリシーの許可は、許可用のロールに限る | 同じロールを使う案: 許可と拒否を区別できないため（NFR レビューの R-01） |

## Shared Infrastructure

該当しません（Unit は 1 つです）。
