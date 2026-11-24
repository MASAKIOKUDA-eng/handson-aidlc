# Feasibility & Constraints — 質問

インテントステートメント（`ideation/intent-capture/intent-statement.md`）をもとに、検証シナリオの実現性と制約を確認します。

---

## Q1. [アーキテクト] 「IPv6 を前提とした VPC」とは、どの構成を指しますか？

PrivateLink と VPC Lattice のどちらも、IPv6 への対応範囲（IPv6 のみの構成で使えるか、デュアルスタックが必要か）が比較結果を左右します。

A. IPv6 のみ（IPv6-only サブネット。IPv4 を使わない）
B. デュアルスタック（IPv6 を主に使い、IPv4 も併用する）
C. A と B の両方のパターンを比較対象にする
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q2. [アーキテクト] 呼び出し元（クライアント側）は何で動いていますか？

呼び出し元の種類によって、遅延の測り方と、呼び出し元を識別するための情報の付け方が変わります。

A. 呼び出し元も Amazon ECS のタスク
B. Amazon EC2 インスタンス
C. AWS Lambda 関数
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: B

## Q3. [アーキテクト] 呼び出される側の ECS サービスの起動タイプはどれですか？

A. AWS Fargate
B. Amazon EC2
C. 両方を対象にする
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q4. [アーキテクト] 2 つの VPC は、同じ AWS アカウントにありますか？

アカウントをまたぐかどうかで、アクセス制御の設定方法（共有の仕組み、許可の出し方）が変わります。

A. 同じアカウント内の 2 つの VPC
B. 別々のアカウントの VPC
C. 両方のパターンを比較対象にする
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q5. [アーキテクト] 検証を想定するリージョンはどこですか？

サービスや機能の提供状況はリージョンによって異なる場合があるため、前提として決めておきます。

A. 東京リージョン（ap-northeast-1）
B. 大阪リージョン（ap-northeast-3）
C. 米国のリージョン（us-east-1 など）
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q6. [セキュリティ] 「提供側から呼び出し元を識別できるか」で、何を識別したいですか？

識別のレベルによって、比較する仕組みと合否の基準が変わります。

A. 呼び出し元のネットワーク上の情報（送信元 IP アドレス、VPC、エンドポイントなど）
B. 呼び出し元の認証された身元（IAM ロールなどのプリンシパル）
C. A と B の両方を比較する
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q7. [コンプライアンス] この検証や将来のシステムに関わる規制・社内基準はありますか？

A. 特になし
B. 業界の規制がある（PCI DSS、FISC など。Other に具体的に書いてください）
C. 顧客や自社のセキュリティ基準がある
D. Not identified（まだ確認していない）
X. Other (please specify)

[Answer]: A

## Q8. [デリバリー] 検証シナリオの文書を仕上げる期限はありますか？

A. 1 週間以内
B. 1 か月以内
C. 特に期限はない
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A
