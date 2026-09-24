# CARILA WORKS 作品制作ルール

このリポジトリは CARILA WORKS Control から作成される作品リポジトリです。作業前に `README.md`、`PROJECT_BRIEF.md`、`CARILA_WORKS_PLAYBOOK.md`、`carila-project.json`、既存コードと、より狭い範囲の `AGENTS.md` を確認してください。

## Harness の読み順

長期制作で文脈・決定・現在地を失わないため、依頼に関係する Harness 文書を確認してください。

1. `docs/CARILA_WORKS_EXECUTION_RULES.md` — GitHub操作、制作フロー、再開、中断復旧、指示忠実性、完了報告の詳細実行ルール。CARILA WORKS作業では必ず従う。
2. `PROJECT_BRIEF.md` — 企画から渡された作品要件の原文。勝手に要約して置き換えない。
3. `CARILA_WORKS_PLAYBOOK.md` — CARILA WORKS全体で蓄積した制作経験と、毎作品で一度は検討したい論点。
4. `docs/CONSTITUTION.md` — 作品の目的、守る価値、変えてはいけない原則。
5. `docs/REQUIREMENTS.md` — ユーザーと合意済みの確定仕様。
6. `docs/DECISIONS.md` — 重要な決定と、その理由・代替案。
7. `docs/UNRESOLVED.md` — 未確定事項。ここにある内容を推測で確定しない。
8. `work/ROADMAP.md` — 現在の制作計画と段階。
9. `work/CURRENT_TASK.md` — 今回取り組む範囲、完了条件、対象外。
10. `work/PROJECT_CHECKLIST.md` — Playbook項目をこの作品でどう扱うか。
11. `work/PLAYBOOK_CANDIDATES.md` — この作品で得た、他作品へ還元できそうな経験値候補。
12. `work/PROJECT_STATUS.md` — 現在地、完了済み、次の作業、引き継ぎ情報。
13. `evals/ACCEPTANCE.md` — 完成と判定するための検品条件。

すべてを毎回書き換えない。今回の変更によって事実が変わった文書だけを、実装と同じ変更の中で更新する。

## セッション継続と現在地の永続化

- チャット履歴や分岐チャットを長期記憶の正本にしない。制作の現在地は `work/PROJECT_STATUS.md` を正本としてRepositoryへ永続化する。
- `work/PROJECT_STATUS.md` は作業終了時だけでなく、ブランチ変更、実装完了、重要な原因特定、テスト結果、方針変更、ブロッカー発生・解消など「次の制作セッションの判断が変わる事実」が発生した時点で随時更新する。
- 新しいチャット・Codexセッション・別AIへ移った場合は、過去ログを再現しようとする前に `work/PROJECT_STATUS.md`、`work/CURRENT_TASK.md`、関連Harness文書、実際のGit状態を確認し、Repositoryの記録から現在地を復元する。
- チャットが長くなったことを理由に分岐だけへ依存しない。継続性が不安定になった場合は、まず現在地をRepositoryへ記録してから完全な新規チャットへ移行できる状態にする。
- `work/PROJECT_STATUS.md` には最低限、現在のbranch、現在地、完了済み、現在の作業、直近の重要変更、検証状況、ブロッカー/未確定、次にやる具体的な作業、再開時の注意点を保持する。
- 記録は会話の逐語録にしない。次の担当AIが過去チャットを読まなくても、何が事実として完了し、何が未完了で、次に何を確認・実行すべきか判断できる粒度にする。

## 実行ルールの正本

- GitHub操作、作業継続、再開、中断復旧、ユーザー指示への忠実性、Actionsとの役割分離、完了報告の詳細は `docs/CARILA_WORKS_EXECUTION_RULES.md` を正本とする。
- このAGENTS.mdの要約と詳細ルールが競合する場合は、より新しい実Git状態と確定仕様を確認したうえで `docs/CARILA_WORKS_EXECUTION_RULES.md` を優先して適用する。
- 詳細ルールを読まずに、container/git clone等の補助経路だけでGitHub操作可否を判断してはならない。

## GitHub Tool Discovery Gate

GitHubに関係する依頼では、最初の実行行動として認証済みGitHub connectorの利用可能actionを確認する。
Repository read / file read / branch / file update / PR / Merge相当のactionが現ターンで利用可能か確認する前に、「connectorがない」「GitHub操作不可」「Merge不可」と結論づけてはならない。
GitHub可否の否定回答には、同一ターン内のconnector discovery、Repository確認、必要なwrite actionの実失敗証拠を必須とする。
container / git clone / DNS / shell / browserの失敗はこのGateの代替にならない。

## GitHub 操作可否の判定

CARILA WORKSの実装・修正タスクで、GitHub操作が必要な場合は「使えない」と推測で判断しない。特に、container / shell / git clone / DNS / browser / web / raw URL など一つの経路が失敗しただけで、GitHub全体を利用不能と扱わない。

GitHub操作不能と報告する前に、対象Repositoryに対して認証済みのGitHub connector / GitHub API経路で、最低限以下を直接確認する。

1. Repository metadataを取得できるか。
2. default branchの最新commitを取得できるか。
3. Open PRを取得できるか。
4. 対象ファイルを1つ以上読めるか。
5. 書き込みを伴う依頼では、通常の作業branch作成など安全なwrite操作を実際に試し、権限拒否かどうかを確認する。

判定ルール:
- 上記のGitHub直接経路が成功している限り、containerや別経路の失敗を理由に「GitHub操作不可」「Merge不可」と報告しない。
- 書き込み権限が必要な作業では、read成功だけでなくwrite経路まで確認してから可否を判断する。
- 本当にblockedの場合は、失敗した経路・操作・権限エラーを具体的に `work/PROJECT_STATUS.md` へ残す。
- 複数のGitHub経路がある場合、Repository操作の正本は認証済みGitHub connector / GitHub API経路とする。
- 実装依頼でGitHub操作が可能なら、原因特定→修正→検証→文書更新→PR→Mergeまでを一続きで進める。
- ツール呼び出し・ストリーミング・表示が途中で中断した後は、見えている会話状態から作業の成否を推測しない。ユーザーへ「未完了」「失敗」「Mergeされていない」と答える前に、latest main、対象branchとの差分、Open PR、対象PRのstate/mergedをGitHub connectorで再取得して実状態を確認する。
- PR作成が422等で失敗した場合は、同じhead branchの既存PRを検索して状態を確認する。既存PRがMerge済みなら新規作成せず、そのMergeを正として扱う。

## 制作

- `PROJECT_BRIEF.md` を企画から渡された作品要件の正として扱い、`docs/REQUIREMENTS.md` を合意済み仕様の正として扱う。両者が矛盾する場合は勝手に解釈せずユーザーへ確認する。
- 不明な仕様を推測で補わない。安全に取り消せる細部を仮置きする場合は、その事実を `docs/UNRESOLVED.md` または `work/CURRENT_TASK.md` に明記する。
- 重要な設計・仕様判断を行った場合は、結論だけでなく理由を `docs/DECISIONS.md` に残す。
- 依頼に必要な最小限の変更を行い、実装と資料を一致させ、関連するテスト、ビルド、静的解析を実行する。
- 作業開始時は `work/CURRENT_TASK.md` に今回の目的・完了条件・対象外を合わせ、完了時は `work/PROJECT_STATUS.md` を次回そのまま再開できる状態へ更新する。
- 完成を主張する前に `evals/ACCEPTANCE.md` の該当項目を確認する。未確認項目がある場合は未確認と報告する。
- React、Vite、Next.js など特定のフレームワークや hosting provider を前提にしない。必要になった時点で、作品要件に合う構成を選ぶ。

## ChatGPT Work の提案・分業

- CARILA WORKSの全作品で、依頼の一部または全部が通常ChatよりChatGPT Workに明確に適するかを毎回判断する。対象例は、大量のWeb横断調査、複数サイト比較、長時間のCloud Browser作業、フォーム入力、外部サービスをまたぐ多段階実務、調査から成果物作成までの長い作業である。
- Workが有効な場合は、ユーザーが気付いていなくても**「この部分はWork向き」**と明示し、何が楽になるかを短く説明する。
- その際、ユーザーがWorkへそのまま貼れる**具体的な依頼文をコードブロックで提示する**。抽象的に「Workを使ってください」で止めない。
- 通常ChatからWorkへ自動切替できるような表現はしない。現在の環境で実行可能な作業はそのまま進め、Workが有利な部分だけを切り分ける。
- Repository調査・実装・テスト・PR・Mergeは引き続きGitHub connectorを第一選択とし、WorkをGitHub作業の代替にしない。
- このルールは既存作品だけでなく、CARILA WORKS Templateから今後作成する全作品へ継承する。

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
- Worker 型の `compatibility_date` は、**UTC基準で未来日にならない固定日**を設定する。JST等のローカル日付の「今日」をそのまま採用しない。新規設定・更新時は `new Date().toISOString().slice(0,10)` 以下であることをテストし、Cloudflareへ送る前に失敗させる。
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


## Compact handoff / context budget
- `work/PROJECT_STATUS.md` は追記型の日誌ではなく、**現在地の短いスナップショット**として維持する。過去のPR・commit・作業ログを無制限に追記しない。
- `work/CURRENT_TASK.md` は現在の1タスクだけを記述し、完了した過去タスクはGit/PR履歴へ任せる。
- 過去の実装詳細はGit commits / PRs、永続仕様はREQUIREMENTS、重要判断はDECISIONS、未確定はUNRESOLVEDへ分離する。
- 新規チャット再開時はまず `PROJECT_STATUS` → `CURRENT_TASK` → latest main / Open PR を確認し、その後**現在タスクに必要な文書・コードだけ**読む。履歴文書の全読込を標準にしない。
- ユーザーが「引き継ぎ文書ちょうだい」「会話移動する」「次チャット用にまとめて」と依頼した場合、GitHubが利用可能なら長文の履歴再掲を避け、原則として次の最短形式を返す:
  `<owner>/<repo> の続きです。GitHub connectorで PROJECT_STATUS / CURRENT_TASK / latest main / Open PR を確認して、そのまま続けてください。`
- Repository名が確定している場合は実Repository名を入れる。現在の作業で特別な一時制約がある場合のみ、短い追記を加える。
