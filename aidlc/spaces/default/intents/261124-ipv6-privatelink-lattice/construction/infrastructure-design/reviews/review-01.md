## Review

**Verdict:** NOT-READY
**Reviewer:** aidlc-architecture-reviewer-agent
**Date:** 2026-11-24T18:19:55Z
**Iteration:** 1
**Verdict Dictated:** no

### Findings

| ID | Severity | Location | Finding | Required action | Status |
|---|---|---|---|---|---|
| R-01 | Major | docs/verification-scenario-privatelink-vs-lattice.md > 5.2 / 5.3 / 6 / 7.1; aidlc/spaces/default/intents/261124-ipv6-privatelink-lattice/construction/infrastructure-design/cicd-pipeline.md > 元に戻す方法 | 5.2 で Lattice サービスの認証タイプを NONE から AWS_IAM に変えるが、戻す手順がない。cicd-pipeline.md の「戻す対象」にも認証タイプが入っていない。そのまま進むと、5.3 の「承認後 = 200」、6 章の識別確認、7 章の遅延測定で、署名なしの curl が 403 になる。AWS_IAM のまま署名付きで測ると、署名のコストが Lattice の遅延に混ざり、PrivateLink との比較が公平でなくなる。 | 5.2 の最後に、認証タイプを NONE に戻す手順と、認証ポリシーを外す手順を追加する。cicd-pipeline.md の「元に戻す方法」の対象に認証タイプを加える。7.1 の開始条件に、両方式とも認証なしで測ることを明記する。 | New |
| R-02 | Major | docs/verification-scenario-privatelink-vs-lattice.md > 4.1 手順 2 / 手順 3 / 手順 4、3.1 | IPv6 のみの環境から AWS サービスに届くための前提が書かれていない。許可用 EC2 は「Session Manager で接続できる」ことが期待結果だが、SSM のデュアルスタックエンドポイントと IPv6 での名前解決の確認がない。Fargate のイメージ取得（ECR と、その背後の S3）とログ送信（CloudWatch Logs）も、ECR 以外のエンドポイントは触れていない。Egress-only IGW だけで足りるかを確認していないため、4.1 の時点で止まるおそれがある。 | 4.1 に、IPv6 のみで使う AWS サービスの一覧（SSM、ECR、S3、CloudWatch Logs）と、各サービスのデュアルスタックエンドポイントの設定を追加する。確認できない点は「要確認」とし、3 章の事前確認に加える。タスク実行ロールとインスタンスロールに必要なポリシーも書く。 | New |
| R-03 | Major | docs/verification-scenario-privatelink-vs-lattice.md > 4.2 手順 4、5.3 | 4.2 手順 4 で接続を承認した後に、5.3 で「承認前」と「拒否後」の状態を確認する流れになっている。承認済みのエンドポイントを承認前に戻す手順はない。PrivateLink では、拒否（reject）した接続は同じエンドポイントのまま再承認できないため、新しいエンドポイントの作成と、DNS エイリアスの付け替えが必要になる。現状の手順どおりだと、承認前の状態は確認できない。 | 5.3 を、「承認前 → 承認後 → 拒否後」の順にする。承認前の確認は 4.2 手順 4 の承認をまだしない状態で行うよう、手順を並べ替える。拒否後は、新しいエンドポイントを作ってやり直す手順（5.3 の後で、遅延測定用に作り直す分も含む）を書く。cicd-pipeline.md の元に戻す方法にも反映する。 | New |
| R-04 | Minor | docs/verification-scenario-privatelink-vs-lattice.md > 9 IAM ポリシー例 | 許可用・拒否用 EC2 のロール（`pl-lattice-allow-role`、`pl-lattice-deny-role`）と、ECS のタスク実行ロールに付けるポリシーが書かれていない。Lattice 認証ポリシーで許可するロール ARN の書き方の例もない。 | 9 章に、2 つの EC2 ロールとタスク実行ロールの必要最小限のポリシーと、認証ポリシーの例を追加する。 | New |
| R-05 | Minor | aidlc/spaces/default/intents/261124-ipv6-privatelink-lattice/construction/infrastructure-design/cicd-pipeline.md > 実施の順番と判断点 の 1 行目 | Fargate が IPv6 のみに対応していない場合の分岐が、Lattice と同じ「デュアルスタックにして進む」になっている。ただし 3.1 の確認では、Fargate の失敗時の対応が書かれていない。 | 3.1 に、対応していない場合の手順（提供側の ECS サブネットをデュアルスタックにして 8.4 に記録する）を追加する。 | New |

### Validation Tool Results

| Tool | Result | Interpretation |
|---|---|---|
| (なし) | 該当なし | このステージには検証ツールが指定されていない。成果物は文書のみのため、AWS 上での実行確認は行っていない。 |

### Summary

構成と判断点は筋が通っているが、実施者がそのまま手順を追うと、認証タイプが戻らず測定が成り立たない点（Lattice の AWS_IAM）、承認状態を巻き戻せない点（PrivateLink）、IPv6 のみの環境で必要な AWS サービスの前提が欠けている点の 3 つで、手順が止まるか比較が公平でなくなる。これらを直してから承認することを勧める。monitoring-design.md と infrastructure-specification.md には、特に指摘すべき点はない。
