# CARILA WORKS Resource Efficiency Rules

この文書は、GitHub Actions / Cloudflare Workers / Cron Triggers / D1 / Queue等の無駄な利用と不要な構造複雑化を防ぐための共通ルールです。

## 原則
1. **1つの責務に1つのowner**
   - cleanup、refill、reconcile、snapshot、notification等を複数のcron/request/workflowで重複実行しない。
   - 既存ownerが壊れた場合、新fallback回路を増やす前に既存ownerを修復する。

2. **event-drivenをpollingより優先**
   - push/webhook/queue/message等の確実なeventがある処理へ、同じ目的の定期pollingを追加しない。
   - cronは「時間経過そのもの」が条件の処理、または許容遅延内のself-healに限定する。

3. **release metadataを業務triggerにしない**
   - `carila-project.json` はControlがrelease URL/status等を書き戻すmetadataである。
   - manifest更新を、無関係なCI、外部API実検証、automation snapshot、business batchのpush triggerへ含めない。
   - manifest内容そのものを検証するworkflowだけ例外とする。

4. **Draft中はActions CIを使わない**
   - 作業状態はbranch/Draft PRへ随時保存する。
   - Draft中はlocal/containerのsyntax/unit/contract/build等で検証する。
   - Ready for reviewで最終CIを自動起動し、PASS後にMergeする。

5. **CronはProductionだけが所有**
   - Preview WorkerへProductionと同じCron Triggerを作らない。
   - Previewは手動/実機確認用runtimeであり、定期business jobの二重実行を起こさない。

6. **D1 maintenanceをhot request pathへ置かない**
   - retention cleanup、全件scan、schema discovery/backfill、集計再構築等を通常のread/write requestごとに実行しない。
   - 定期cleanupはcron等の単一ownerへ寄せる。
   - schema変更はmigrationを第一選択とする。一時的な互換guardが必要ならper-isolate cache等で繰り返しqueryを避け、移行完了後に削除する。

7. **Queueを増やす前に既存Queueで表現できないか確認**
   - Queueはbrowser lifecycleから切り離す重い非同期処理に使う。
   - 単純な短時間処理や既存release Queueで扱える処理のために新Queue/state machineを作らない。

8. **外部API実検証をMerge CIの常用条件にしない**
   - provider側の一時障害・0件・rate limitでコードのMerge可否を不安定にしない。
   - PR gateはdeterministic mock/contract testを基本とする。
   - live provider testはmanual diagnostic、または明確なE2E環境で必要時だけ実行する。

## 新規cron/workflow/D1/Queue追加前チェック
- [ ] 同じ責務を持つ既存ownerがない
- [ ] event-drivenでは実現できない理由がある
- [ ] 実行頻度はSLAに必要な最小頻度である
- [ ] PreviewとProductionで二重実行されない
- [ ] release metadataだけで誤発火しない
- [ ] D1 read/writeが通常request数に不必要に比例しない
- [ ] 既存Queue/DB/workflowへ安全に統合できないことを確認した
- [ ] failure時に別fallback回路を増殖させず、同じownerでrecoveryできる
- [ ] Actions / Cron / D1 / QueueのDELTAをPR本文またはPROJECT_STATUSへ記録した

## 監査時の判断順
1. 実際に何回起動しているか
2. 起動ごとに何をread/write/callしているか
3. その処理に時間条件が本当に必要か
4. 同じ責務を別経路でも実行していないか
5. 削除・統合してもRecovery SLAとデータ整合性を維持できるか

ファイル数やコード行数だけで「重い」と判断しない。料金・quota・障害リスクへ直接つながるruntime pathを先に最適化する。
