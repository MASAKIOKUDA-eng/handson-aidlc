# 決定記録（構想段階）

構想段階（Ideation）で決まった事項の記録です。出典は各ステージの質問ファイルです。

| # | 決定事項 | 決定内容 | 出典 |
|---|----------|----------|------|
| D-01 | 検証の目的 | 新しく作るサービス間通信の方式を、PrivateLink と VPC Lattice のどちらにするか決める判断材料を作る | `intent-capture-questions.md` Q1 |
| D-02 | 文書の読み手 | 主に作成者自身が使い、顧客など作成者以外の人も実施できる水準で書く | `intent-capture-questions.md` Q2、Q11 |
| D-03 | 完成の条件 | 作成者以外の人が、この文書だけで検証を実施できる | `intent-capture-questions.md` Q3 |
| D-04 | 意思決定者 | 範囲・優先度と方式の最終決定は、顧客側の担当者が行う | `intent-capture-questions.md` Q5、Q6、Q10 |
| D-05 | 報告 | 週 1 回、進捗を報告する | `intent-capture-questions.md` Q7、Q9 |
| D-06 | 対象外の観点 | 遅延とセキュリティ以外の観点（可用性、スケーラビリティ、運用のしやすさなど）は扱わない | `intent-capture-questions.md` Q8 |
| D-07 | IP アドレスの構成 | IPv6 のみ | `feasibility-questions.md` Q1 |
| D-08 | 呼び出し元 | Amazon EC2 インスタンス | `feasibility-questions.md` Q2 |
| D-09 | 呼び出し先 | AWS Fargate 上の Amazon ECS サービス | `feasibility-questions.md` Q3 |
| D-10 | VPC の配置 | 同じアカウント内の 2 つの VPC | `feasibility-questions.md` Q4 |
| D-11 | リージョン | 東京（ap-northeast-1） | `feasibility-questions.md` Q5 |
| D-12 | 識別の対象 | ネットワーク上の情報（送信元 IP アドレス、VPC、エンドポイントなど） | `feasibility-questions.md` Q6 |
| D-13 | 規制 | 該当なし | `feasibility-questions.md` Q7 |
| D-14 | 期限 | 1 週間以内 | `feasibility-questions.md` Q8 |
| D-15 | 範囲の合意 | 顧客側の担当者と合意済み | `approval-handoff-questions.md` Q1 |
| D-16 | IPv6 のみが成り立たない場合 | 成り立たない部分だけ IPv4 を併用（デュアルスタック）し、そのことを明記して比較する | `approval-handoff-questions.md` Q2 |
| D-17 | 作成体制 | 作成者 1 人 | `approval-handoff-questions.md` Q3 |
| D-18 | Go / No-go | Go | `approval-handoff-questions.md` Q4 |
| D-19 | ワークフローのプラン | express をもとにしたカスタムプラン（文書のみ。コード生成・デプロイ系のステージは省略） | プラン承認時の決定 |
