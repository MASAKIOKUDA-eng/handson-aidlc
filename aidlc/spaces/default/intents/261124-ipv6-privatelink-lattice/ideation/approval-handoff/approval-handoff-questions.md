# Approval & Handoff — 質問

構想段階の成果物をまとめたうえで、要件定義（Inception）に進めてよいかを確認します。参照した文書は次のとおりです。

- `ideation/intent-capture/intent-statement.md`
- `ideation/intent-capture/stakeholder-map.md`
- `ideation/feasibility/feasibility-assessment.md`
- `ideation/feasibility/constraint-register.md`
- `ideation/feasibility/raid-log.md`

市場調査、範囲定義、チーム編成、ラフなモックアップの各ステージは、このプランでは実行していません。そのため、これらに関する質問は省いています。

---

## Q1. [プロダクトオーナー] 方式の最終決定者である顧客側の担当者とは、目的と範囲について合意できていますか？

目的と範囲は、インテントステートメントに記載したものです（PrivateLink と VPC Lattice を、遅延とセキュリティの観点で比較する検証シナリオの文書を作る。AWS 上での実行は行わない）。

A. 合意済み
B. この構想段階のまとめをもとに、週次報告で合意を取る
C. まだ合意していない
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q2. [プロダクトオーナー] 「IPv6 のみ」の前提が一方の方式で成り立たなかった場合、どう扱いますか？

実現性の確認で、PrivateLink の提供側や VPC Lattice が IPv6 のみの構成に対応しているかは、まだ確認できていません（RAID ログの R1、R2、課題 I1）。要件分析の前に公式ドキュメントで確認しますが、成り立たなかった場合の扱いを先に決めておきます。

A. 成り立たない部分だけ IPv4 を併用（デュアルスタック）し、その旨を明記したうえで比較する
B. 「IPv6 のみでは構成できない」こと自体を比較結果として記録し、その方式の検証手順は書かない
C. 顧客側の担当者に確認するまで保留し、要件分析では A と B の両案を併記する
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q3. [デリバリー] 文書を作成する体制はどうなっていますか？

期限は 1 週間以内です（制約 C-O1）。

A. 自分 1 人で作成する
B. 他のメンバーの支援を受けられる（誰の支援かを Other に書いてください）
C. 顧客側と共同で作成する
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q4. [デリバリー] 要件定義（Inception）に進めてよいですか？

RAID ログに記載したリスク（R1〜R5）と、その対応を確認したうえでお答えください。

A. Go: リスクと対応を確認したうえで、このまま進める
B. 条件付き Go: 未確認の点（IPv6 のみへの対応）を公式ドキュメントで確認できたら進める
C. No-go: いったん止める
D. Not yet defined（まだ決められない）
X. Other (please specify)

[Answer]: A
