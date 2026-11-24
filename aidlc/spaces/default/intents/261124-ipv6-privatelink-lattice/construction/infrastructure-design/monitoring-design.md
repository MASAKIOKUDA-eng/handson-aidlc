# 監視の設計

入力: `construction/nfr-requirements/observability-requirements.md`、`construction/nfr-requirements/performance-requirements.md`

短時間の検証なので、ダッシュボードやアラートは作りません（本ステージ Q3）。記録は、検証シナリオの 8 章で行います。

## Metrics & KPIs

| Metric | Source | Threshold | Why it matters |
|--------|--------|-----------|----------------|
| 1 件ごとの往復時間（time_total） | curl の `-w` 出力 | 合否なし（値を記録） | 遅延の比較そのもの（FR4） |
| 新しく張った接続の数（num_connects） | curl の `-w` 出力 | 2 行目以降はすべて 0 | 接続を使い回せているかの確認（NFR レビューの R-03） |
| エラー件数 | HTTP ステータスが 200 以外、またはタイムアウト | 1 セット 10 件まで | 超えたらそのセットは測り直す（NFR1.3） |
| ターゲットの状態 | ターゲットグループのヘルスチェック | healthy | 測定を始める条件（NFR3.4） |

## Alerts

| Alert | Condition | Severity | Routes to |
|-------|-----------|----------|-----------|
| なし | - | - | 実施者が手順の中で目で確認する |

## SLIs / SLOs

| SLI | SLO target | Measurement window |
|-----|-----------|--------------------|
| 該当なし（運用するサービスではない） | - | - |

## Logs & Tracing

- 測定の生データは、セットごとに `raw_<方式>_<セット番号>.txt` として保存します。
- 呼び出し元の識別は、アプリの応答（JSON）を記録欄に貼り付けます（NFR5.2）。
- トレーシングは行いません。
