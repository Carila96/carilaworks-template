# CARILA WORKS 作品制作ルール

このリポジトリは CARILA WORKS Control から作成される作品リポジトリです。作業前に `README.md`、`PROJECT_BRIEF.md`、`CARILA_WORKS_PLAYBOOK.md`、`carila-project.json`、既存コードと、より狭い範囲の `AGENTS.md` を確認してください。

## Harness の読み順

長期制作で文脈・決定・現在地を失わないため、依頼に関係する Harness 文書を確認してください。

1. `PROJECT_BRIEF.md` — 企画から渡された作品要件の原文。勝手に要約して置き換えない。
2. `CARILA_WORKS_PLAYBOOK.md` — CARILA WORKS全体で蓄積した制作経験と、毎作品で一度は検討したい論点。
3. `docs/CONSTITUTION.md` — 作品の目的、守る価値、変えてはいけない原則。
4. `docs/REQUIREMENTS.md` — ユーザーと合意済みの確定仕様。
5. `docs/DECISIONS.md` — 重要な決定と、その理由・代替案。
6. `docs/UNRESOLVED.md` — 未確定事項。ここにある内容を推測で確定しない。
7. `work/ROADMAP.md` — 現在の制作計画と段階。
8. `work/CURRENT_TASK.md` — 今回取り組む範囲、完了条件、対象外。
9. `work/PROJECT_CHECKLIST.md` — Playbook項目をこの作品でどう扱うか。
10. `work/PLAYBOOK_CANDIDATES.md` — この作品で得た、他作品へ還元できそうな経験値候補。
11. `work/PROJECT_STATUS.md` — 現在地、完了済み、次の作業、引き継ぎ情報。
12. `evals/ACCEPTANCE.md` — 完成と判定するための検品条件。

すべてを毎回書き換えない。今回の変更によって事実が変わった文書だけを、実装と同じ変更の中で更新する。

## 制作

- `PROJECT_BRIEF.md` を企画から渡された作品要件の正として扱い、`docs/REQUIREMENTS.md` を合意済み仕様の正として扱う。両者が矛盾する場合は勝手に解釈せずユーザーへ確認する。
- 不明な仕様を推測で補わない。安全に取り消せる細部を仮置きする場合は、その事実を `docs/UNRESOLVED.md` または `work/CURRENT_TASK.md` に明記する。
- 重要な設計・仕様判断を行った場合は、結論だけでなく理由を `docs/DECISIONS.md` に残す。
- 依頼に必要な最小限の変更を行い、実装と資料を一致させ、関連するテスト、ビルド、静的解析を実行する。
- 作業開始時は `work/CURRENT_TASK.md` に今回の目的・完了条件・対象外を合わせ、完了時は `work/PROJECT_STATUS.md` を次回そのまま再開できる状態へ更新する。
- 完成を主張する前に `evals/ACCEPTANCE.md` の該当項目を確認する。未確認項目がある場合は未確認と報告する。
- React、Vite、Next.js など特定のフレームワークや hosting provider を前提にしない。必要になった時点で、作品要件に合う構成を選ぶ。

## Playbook と提案

- `CARILA_WORKS_PLAYBOOK.md` は実装命令ではなく、検討漏れを防ぐための共通知識である。作品に不要な機能を機械的に追加しない。
- 新作開始時と、企画・制作の節目で `work/PROJECT_CHECKLIST.md` を確認する。
- ユーザーが「次に何をやる？」「残タスクは？」「他に決めることある？」と聞いた場合、実装中タスクだけでなく `PROJECT_CHECKLIST` の `UNREVIEWED` / `LATER` から、現在の段階に関係する項目を優先順位付きで提案する。
- ユーザーが明確に「今回は不要」と判断しない限り、項目を会話から消えたことだけで `NOT_NEEDED` にしない。
- ユーザーが後回しにした項目は `LATER` とし、適切な段階で再提示する。
- 採用された項目は `ADOPTED` とした上で `docs/REQUIREMENTS.md` または `work/CURRENT_TASK.md` に具体化し、完了後に `DONE` とする。
- すべての未検討項目を一度に列挙して会話を圧迫しない。今決める価値が高いものを先に提案し、残りはChecklistに保持する。

## 経験値の自動候補化

- 制作中に「別作品でも起こり得る失敗」「再利用できるバグ回避策」「設計上の再発防止策」「有効な検品観点」を見つけた場合、ユーザーから指示がなくても `work/PLAYBOOK_CANDIDATES.md` へ `CANDIDATE` として記録する。
- 候補は作品固有の事象そのものではなく、別作品でも使える形へ一般化して記録する。
- 未検証の推測、単なる好み、一作品だけの特殊事情は候補化しない。
- product philosophy、課金方針、公開方針などCARILA WORKSや作品の思想を変える内容は、候補には残しても勝手に共通ルールへ昇格させない。
- 同じ候補を重複登録しない。既にPlaybookにある内容なら新規候補にしない。
- 作業完了時に未処理の `CANDIDATE` がある場合は、通常の完了報告に加えて「Playbook候補あり」と明示する。
- Playbook本体への昇格は、候補が十分一般化され、根拠があり、既存ルールと競合しないことを確認してから行う。思想・方針を変える内容はユーザー確認を優先する。

## Repository capability と release

- release adapter を作品側の判断で固定しない。Control が Repository の構成から capability と adapter を判定する。
- 最小 static 作品はルートの `index.html` を公開ソースとする（`STATIC_SOURCE`）。
- build 型へ移行する場合は `dist`、`build`、または `out` に成果物を生成し、Control が参照できるよう成果物を commit する（`STATIC_BUILD_OUTPUT`）。採用した出力先は実態に合わせる。
- Worker 型へ移行する場合は Wrangler の設定および entrypoint を含む Cloudflare Workers の Repository 契約を満たす（`WORKER_APP`）。
- Vercel 固有の設定や API を標準契約として追加しない。作品要件として明示された場合に限り検討する。

## lifecycle と manifest

- `WORKING`、`PUBLIC`、`COMING_SOON`、`MAINTENANCE`、`ARCHIVED` などの lifecycle を Codex の判断で変更しない。
- lifecycle の唯一の正は Control の D1 である。公開・非公開の変更は Control で行う。
- 制作工程は lifecycle と別概念として扱う。`work/ROADMAP.md` や `work/PROJECT_STATUS.md` の進捗表現を lifecycle の代用にしない。
- `carila-project.json` は Repository 契約と、Control が管理する metadata の mirror である。manifest の `status` を変更して Control の公開状態を操作しない。
- `releaseAdapterType` は互換性のための metadata mirror であり、作品側が技術方式を指定または固定する設定ではない。
- `outputDirectory` は build 成果物の出力先が実際に必要になるまで設定しない。
- `schemaVersion` は明示された移行なしに変更せず、作品固有情報は Control による作成時の置換値を維持する。

## セキュリティと外部操作

- Secret、API key、access token、password、秘密鍵、個人情報を Git に保存しない。秘密値は Git 管理外の環境変数または承認された Secret 管理機能を使う。
- 公開、deploy、release、DNS・subdomain の変更、課金、破壊的操作など外部状態を変える操作は、対象と影響を説明し、ユーザーの明示的な承認を得てから行う。
- 生成物（Control が契約上必要とする commit 済み build 成果物を除く）、local 設定、機密情報を commit しない。

## Harness 改善

- 同種の失敗、仕様の取り違え、引き継ぎ漏れが再発した場合は、その場の修正だけで終わらせず、どの Harness 文書・Playbook項目・ルール・検品条件を直せば再発を防げるか確認する。
- Harness 自体の変更は作品仕様の変更と区別し、既存の確定仕様や lifecycle を暗黙に変更しない。
