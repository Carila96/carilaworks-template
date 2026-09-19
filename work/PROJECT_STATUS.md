# Project Status

このファイルを、次の制作セッションが過去ログや分岐チャットを読み直さなくても再開できる「現在地の正本」として維持します。
作業終了時だけでなく、次の判断に影響する事実が変わった時点で随時更新してください。

## 最終更新

- 日時:
- 更新者 / セッション:

## 現在のbranch

- `main`

## 現在地

- 新作作成直後。`PROJECT_BRIEF.md` の確認待ち。

## 完了済み

- CARILA WORKS の初期Repository構造を生成。

## 現在の作業

- `work/CURRENT_TASK.md` を参照。

## 直近の重要変更

- なし。

## 次にやること

- `PROJECT_BRIEF.md` を読み、Harness文書を作品固有内容へ合わせる。

## ブロッカー / 未確定

- `docs/UNRESOLVED.md` を参照。

## 検証状況

- 未実施。

## 再開時の注意点 / Handoff

- まずこのファイルと `work/CURRENT_TASK.md`、関連Harness文書、実際のGit状態を確認する。
- lifecycle と制作進捗を混同しない。
- 公開操作はユーザー承認後にControlから行う。
- 過去チャットの全文再現を前提にしない。ここにない重要事項を会話記憶だけで補完しない。


## 2026-09-19 — Analytics / SEO baseline標準化

- CARILA WORKS標準として匿名AnalyticsとGoogle検索準備の確認項目をPlaybook / Checklistへ追加。
- PWAはChromium系appinstalledと、iOSを含むstandalone起動を区別して扱う。
- SEO baseline: title / description / canonical / OGP / lang / noindex / robots.txt / sitemap.xml / JSON-LD / 必要時hreflang。
- Analytics停止時に作品本体を止めないbest-effortを標準化。
- 個人情報や入力本文をAnalyticsへ保存しない。


## 2026-09-19 — 詳細Execution RulesのRepository正本化
- プロジェクト指示の文字数制約に依存しないよう、詳細作業規律を `docs/CARILA_WORKS_EXECUTION_RULES.md` へ永続化。
- GitHub connector-first、GitHub操作不能判定の強制ゲート、Mergeまでの標準フロー、途中停止禁止、中断復旧、PROJECT_STATUS運用、ユーザー指示忠実性、GitHub Actionsとの役割分離、完了報告を明文化。
- `AGENTS.md` のHarness読み順先頭へ追加し、CARILA WORKS作業では必須参照とした。
- `CARILA_WORKS_PLAYBOOK.md` は経験知・検討観点、Execution Rulesは実行手順の正本として役割を分離。
- 既存作品へのTemplate変更は無条件一括反映しない。

## 2026-09-19 — Execution Rules merge
- PR #11 `Persist CARILA WORKS execution rules` Merge済み。
- merge commit: `a0cfe707e12899bee37e3788235484e80ee08565`。


## 2026-09-19 — GitHub Tool Discovery Gate強化
- connectorが実際には利用可能なのに「connectorがない」「Mergeできない」と誤判定した再発事例を受け、Execution Rules / AGENTSを強化。
- GitHub関連依頼の最初の実行行動をTool Discovery Gateとし、現ターンでRepository read / file read / branch / update / PR / Merge相当actionの存在確認を必須化。
- 否定回答はTool Discovery + Repository Reality + Write Capability + 実connector errorの証拠が揃う場合のみ許可。
- container/git clone/DNS等の補助経路失敗は否定根拠として無効。
- Repository/projet instructionsだけではmodel runtimeを100%技術拘束できないため、完全なhard enforcementにはsystem/developer-level gateまたはpreflight付きtool wrapperが必要と明記。
