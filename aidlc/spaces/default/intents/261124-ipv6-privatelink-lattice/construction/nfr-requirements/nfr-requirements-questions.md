# NFR Requirements — 質問

要件定義（`inception/requirements-analysis/requirements.md`）で未決とした数値（OQ5）と、要件のレビューで指摘された点（R-03〜R-06）を決めるための質問です。

---

## Q1. [品質] 遅延測定のリクエスト数と繰り返し回数はどうしますか？（FR4.4）

A. ウォームアップ 100 件のあと、本測定 1,000 件を 1 セットとし、3 セット繰り返す
B. ウォームアップ 100 件のあと、本測定 10,000 件を 1 セットとし、5 セット繰り返す
C. ウォームアップなしで、本測定 1,000 件を 1 セットだけ行う
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q2. [品質] 2 方式の測定順と時間帯をどうそろえますか？（FR4.3）

A. セットごとに方式を交互に測る（PrivateLink → Lattice → Lattice → PrivateLink …）。全セットを同じ日の連続した時間内に行う
B. 1 リクエストごとに方式を交互に切り替える
C. 方式ごとにまとめて測り、日を変えてもう一度測る
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q3. [品質] 測定環境のサイズはどうしますか？（NFR2）

A. EC2 は汎用の中程度のサイズ（例: m7i.large）1 台、ECS タスクは 0.5 vCPU / 1 GB を 1 つ
B. EC2 はネットワーク性能の高いサイズ（例: c7i.xlarge）1 台、ECS タスクは 1 vCPU / 2 GB を 2 つ
C. 文書では指定せず、実施者が決めた値を記録欄に書く
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q4. [セキュリティ] アクセス制御の「拒否されるケース」に使う呼び出し元はどう用意しますか？（R-05）

A. 利用側 VPC に、別のセキュリティグループを付けた EC2 をもう 1 台置く
B. 3 つ目の VPC（IPv6 のみ）を用意し、そこに EC2 を置く
C. A と B の両方を用意する
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q5. [セキュリティ] 実施者の権限（最小権限）について、文書に何を書きますか？（R-06）

A. 検証に必要な操作をまとめた IAM ポリシーの例を文書に載せる
B. 「検証用アカウントで管理者に近い権限で実施する」ことを前提として明記し、ポリシー例は載せない
C. 権限については書かない
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q6. [アーキテクト] 呼び出し先の名前（DNS 名）と TLS 証明書はどうそろえますか？（R-03）

HTTPS では、呼び出しに使う名前と証明書の名前が一致している必要があります。IPv6 のみの環境では、名前が IPv6 アドレス（AAAA レコード）で引けることも必要です。

A. 両方式とも、自分で決めたドメイン名（Route 53 のプライベートホストゾーン）で呼び出し、その名前の証明書を使う
B. 各方式が自動で付ける DNS 名をそのまま使い、証明書の検証は省く（測定用と割り切る）
C. PrivateLink はプライベート DNS 名、Lattice はカスタムドメインを使う
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A
