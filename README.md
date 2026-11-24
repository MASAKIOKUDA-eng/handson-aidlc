# handson-aidlc

**AI-DLC を入れた場合（検証 B）** のハンズオンです。

同じ依頼文から検証シナリオを作り、[not-handson-aidlc](https://github.com/MASAKIOKUDA-eng/not-handson-aidlc)（AI-DLC を入れなかった場合、検証 A）と比べます。
比べるのは成果物の出来ではなく、**進め方と記録の残り方** です。

## 検証の目的

AI-DLC（AI 駆動開発ライフサイクル）を使うと、検証シナリオ作成の進め方と記録がどう変わるかを、同じ題材・同じ条件で確かめます。

## 題材

IPv6 を前提とした VPC で、別の VPC にある Amazon ECS のサービスを呼び出す経路として、AWS PrivateLink と Amazon VPC Lattice を比較する検証シナリオ（スピードとセキュリティの 2 観点）。
**成果物は検証シナリオの文書までとし、AWS 上での実行は行いません。**

## 比較の条件

| 条件 | A（not-handson-aidlc） | B（このリポジトリ） |
|------|------------------------|---------------------|
| ツール | Claude Code（AI-DLC なし） | Claude Code + [aidlc-workflows](https://github.com/awslabs/aidlc-workflows) 2.11.3（バージョンを固定） |
| モデル | A・B で同じものを使い、評価シートに記録する | 同左 |
| 最初の依頼文 | [prompts/initial-request.md](prompts/initial-request.md) を一字一句そのまま | 同左 |
| 参考資料 | 渡さない | 渡さない |
| 作業時間の上限 | 60 分 | 60 分 |
| 記録するもの | 会話のログ（`/export`）、成果物 | 監査ログ、状態ファイル、成果物 |

## ファイル構成

```
.
├── prompts/initial-request.md   A・B 共通の最初の依頼文
├── docs/procedure.md            この検証（B）の手順（aidlc-workflows の導入を含む）
├── docs/evaluation-sheet.md     A・B 共通の評価シート
├── .claude/, aidlc/             aidlc config が作る（作業中にできる）
└── records/                     /export の会話ログ（作業後に置く）
```

AI-DLC の成果物・状態ファイル・監査ログは、aidlc-workflows の仕様どおり `aidlc/spaces/default/intents/<YYMMDD>-<label>/` に作られます。

## 公開前の確認

このリポジトリは非公開で作業します。公開する前に、監査ログ（`audit/`）と成果物にアカウント ID、メールアドレス、端末のユーザー名やパスなどが含まれていないかを確認し、含まれていれば伏せ字にします。監査ログには人の入力がそのまま記録されます。
