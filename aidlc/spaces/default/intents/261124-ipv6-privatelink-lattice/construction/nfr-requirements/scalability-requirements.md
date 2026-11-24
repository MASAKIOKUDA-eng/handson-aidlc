# スケーラビリティの要件

入力: `inception/requirements-analysis/requirements.md`

この検証では、スケーラビリティの要件はありません。

- 負荷をかけた状態での測定は、要件分析の Q5 で対象外にしています。
- スケーラビリティという観点そのものも、インテントで対象外にしています（制約 C-S2）。
- ECS タスクは 1 つに固定します（`performance-requirements.md` NFR2.3）。オートスケーリングは設定しません。
