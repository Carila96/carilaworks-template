# CARILA WORKS Execution Rules

この文書はCARILA WORKS作品の企画・制作・修正における詳細実行ルールの正本です。プロジェクト指示は要約のみを保持し、詳細はこの文書、AGENTS.md、実際のGit状態を参照します。

## 基本原則
- 会話は作業場所であり正本ではない。仕様・判断・現在地・未完了事項はRepositoryへ残す。
- 実コード・Git状態・Repository文書を会話記憶より優先する。
- GitHubへ接続可能なら、ChatGPT自身が調査・修正・検証・文書更新・PR・Mergeまで進める。
- ユーザーが明示的に求めない限り、Codex向け文章や「これを渡してください」で止めない。
- GitHub操作の第一選択は認証済みGitHub connector / GitHub API経路とする。
- container、git clone、shell、browser、DNS、local networkは補助経路であり、その失敗をGitHub全体の操作不能と判断しない。
- GitHub Actionsは通常の編集・branch・PR・Mergeには使わず、CI/build/test/deploy/schedule等workflow実行が必要な用途に限定する。

## GitHub操作の強制ゲート

### 0. Tool Discovery Gate（最優先）
GitHubに関係する依頼では、最初の実行行動としてGitHub connectorの利用可能ツールを確認する。少なくともRepository read、file read、branch作成、file update、PR作成、Mergeに対応するconnector actionが利用可能かを確認する。

この確認が終わる前に、GitHub操作可否についてユーザー向けの結論を返してはならない。

特に以下を禁止する:
- 「connectorがない」と推測で言う
- container / git clone / shell / browser等を先に試し、その失敗からGitHub不可と判断する
- 利用可能tool一覧を確認せず「このセッションではMergeできない」と言う
- 過去ターンでconnectorを使えていたにもかかわらず、現ターンで再確認せず利用不能と断定する

GitHub connectorの存在確認は「記憶」や「画面に見えているtool説明」ではなく、現ターンのtool discovery / 利用可能action確認を根拠にする。

### 1. Repository Reality Gate
Tool Discovery Gate通過後、Repository作業開始時はconnectorで以下を確認する。
1. Repository metadata
2. latest main
3. 対象branch
4. Open PR
5. 対象ファイル
6. mainとの差分
7. 必要時PRのstate / merged

### 2. Write Capability Gate
書き込み依頼では、branch作成、file update、PR作成/更新、Mergeがconnectorで可能か実際に確認する。read成功だけでwrite可否を推測しない。

### 3. Negative Claim Evidence Gate
「GitHubへ書き込めない」「branchを作れない」「PRを作れない」「Mergeできない」「connectorがない」「この環境では実行不能」「ユーザー側で作業が必要」と答える場合は、同一ターン内で以下の証拠が必要:
1. Tool Discovery Gateを実行済み
2. Repository Reality Gateを実行済み
3. 書き込み依頼ならWrite Capability Gateを実行済み
4. 実際に失敗したconnector actionとエラー内容がある

上記4条件を満たさない否定回答は禁止する。未確認の場合は「できない」と言わず、connector確認を続行する。

git cloneやDNS等が失敗しても、その経路だけの失敗として扱う。connectorが使えるならconnectorへ切り替え、作業を継続する。

container/local gitはlocal build、package install、test runner、大量解析、binary生成、connector非対応操作等が必要な場合に使う。最終Git状態、PR、Merge確認はconnectorを優先する。

## 標準制作フロー
1. connectorでRepository/Git状態確認
2. Harness文書確認
3. 現在地と依頼内容照合
4. 原因調査または実装
5. test/build/静的解析/動作確認
6. PROJECT_STATUS等更新
7. branch反映
8. PR作成/更新
9. Merge可否確認
10. 問題なければMerge
11. connectorでlatest main / PR state / merged再確認
12. 完了報告
13. ユーザーがCARILA WORKS Controlから公開

ChatGPT担当は原則Mergeまで。Production公開はユーザーがControlから行う。ChatGPTは勝手にProduction公開や別deploy経路の新設を行わない。

## 途中で止めない
実装依頼では調査結果、修正案、コード例だけを返して終了しない。可能なら「原因特定→修正→テスト→文書更新→PR→Merge」まで進める。
「次にできます」「Codexへ渡してください」「このコードを貼ってください」「ユーザー側でGitHubへ反映してください」で止まらない。
補助経路1つの失敗を理由に停止しない。本当に進められない場合のみ、実際に試した操作と不足する権限・機能・情報を具体的に示す。推測で「できない」と判断しない。

## 作業開始時確認
実装前に最低限以下を確認する。
- AGENTS.md
- CARILA_WORKS_PLAYBOOK.md
- docs/CARILA_WORKS_EXECUTION_RULES.md
- PROJECT_BRIEF.md
- docs/CONSTITUTION.md
- docs/REQUIREMENTS.md
- docs/DECISIONS.md
- docs/UNRESOLVED.md
- docs/INTERACTION_CONTRACT.md（存在時）
- evals/ACCEPTANCE.md（存在時）
- work/ROADMAP.md
- work/CURRENT_TASK.md
- work/PROJECT_STATUS.md
- work/PROJECT_CHECKLIST.md
- branch/latest main/Open PR/mainとの差分/実コード/必要時PR state・merged

作品固有の読み順にも従う。引き継ぎ文だけを最新状態と断定せずRepository実状態を取り直す。文書と実コードが矛盾する場合は事実に合わせて文書も修正する。


## PROJECT_STATUS.md
work/PROJECT_STATUS.mdを作品の現在地の正本とする。作業終了時だけでなく、branch変更、実装完了、原因特定、方針確定、test/build結果、PR、Merge、仕様変更、blocker発生・解消、次作業変更、重要判断、移行前など、次セッションの判断が変わる時点で随時更新する。

最低限、最終更新、現在branch、現在地、完了済み、現在作業、直近重要変更、PR/Merge状況、検証状況、blocker/未確定、次の具体作業、再開時注意/Handoffを保持する。逐語録は保存しない。次の担当AIが過去チャットなしで再開できる粒度にする。Merge後は可能な限り同一作業内でPR/Merge状況もmain実態へ合わせる。

## 新しいチャット・分岐再開
新規チャット、分岐、別セッションでは、ユーザーに再説明を求める前にconnectorでRepositoryから復元する。
確認順:
1. work/PROJECT_STATUS.md
2. work/CURRENT_TASK.md
3. 関連Harness
4. latest main
5. 対象branch
6. Open PR
7. PR state / merged
8. 実コード
9. mainとの差分

その上で前回の続きから開始する。新チャットだけを理由にCodex用引き継ぎ文を作らない。ユーザー提供の引き継ぎ文も最新状態とは断定せずGitHubで確認する。

## 長いチャットからの移行
チャットが重い、表示不安定、context上限接近等の場合は分岐だけに依存しない。移行前に実Git状態確認、PROJECT_STATUS更新、必要に応じCURRENT_TASK/DECISIONS/UNRESOLVED/REQUIREMENTS更新、GitHub反映、latest main/Open PR/PR state/merged再確認を行い、新規チャットからRepositoryだけで再開可能にする。

## ストリーミング・ツール中断時
ストリーミング、tool実行、返答表示が中断しても作業失敗・未完了と推測しない。回答再開前にconnectorでlatest main、対象branch、mainとの差分、Open PR、対象PR state/mergedを再取得する。
PR作成時に422、already exists、validation failed等が出ても即失敗としない。同一head branchの既存PRを検索し、Open/Closed/Merged、main反映済みかを確認する。
会話上の発言よりGitHub実状態を正とする。

## 既存作品
Template更新前の既存作品にも、このAI副業プロジェクト内で作業する場合は同じ制作フローを適用する。PROJECT_STATUSがあれば最新化し、なければ既存構造とAGENTSを確認して仕様を壊さず現在地永続化を整える。Template変更を既存作品へ無条件一括反映しない。既存作品でもGitHub操作の標準経路はconnectorとする。

## Learning Loop
他作品にも再利用可能なbug、原因、回避策、設計原則、test観点、UI/運用/deploy失敗、GitHub connector/container/Actions経路の失敗は既存Learning Loopへ記録する。作品固有事情と一般化知見を分ける。一経路の失敗をシステム全体の不能と誤認した事例は再発防止対象とする。

## 判断の優先順位
1. 実コード・Git状態・実環境
2. docs/REQUIREMENTS.md等の確定仕様
3. work/PROJECT_STATUS.md
4. docs/DECISIONS.md
5. その他Repository文書
6. 現在会話
7. 過去会話記憶

Git状態はGitHub上の実状態を優先し、古いlocal cloneを最新GitHubより優先しない。現在会話で明示的仕様変更があれば新指示を反映し、Repository文書も更新する。

## ユーザー指示への忠実性
具体的修正指示はまずその内容をそのまま実装する。

禁止:
- 指定変更を実装せず別解へ置換する
- 依頼外UI変更やrefactorを混ぜる
- 指定変更を弱め、見た目上ほぼ分からない状態で完了扱いする
- 実装前に提案だけ返して終了する
- 求められていない画像生成、mock生成、Codex文章へすり替える
- 「まず試す」と言いながらRepositoryを変更せず終了する

「これだけ変更」「それ以外を変えない」は厳守する。視覚変更はコード上に値があるだけで完了とせず、ユーザー指定の視覚差が実際に見えることをAcceptanceとする。変更量が小さすぎて実機上ほぼ判別できない場合は要求を満たしたと扱わない。対象外のUI、機能、data、挙動を変更しない。

## GitHub connectorとActionsの役割分離
GitHub connectorの主用途:
- Repository確認
- file read/update
- branch
- commit相当
- PR作成/確認
- Merge
- latest main確認

GitHub Actionsの主用途:
- CI
- automated test
- build
- scheduled job
- deploy workflow
- 定期収集/投稿
- その他runner上のworkflow

通常修正やPR/Mergeのため不要なActions workflowを新設しない。Actions利用量削減時はconnectorで直接可能なRepository操作をActionsへ回さない。ただしcommit/PRを契機に既存workflowが自動起動する場合、そのActions消費は別問題として扱う。

## 完了報告
予定ではなく実結果を報告する。必要に応じ、原因、修正、検証、branch、PR、Merge、latest main、PROJECT_STATUS更新、ユーザー次操作を簡潔に示す。
Merge後の通常操作はCARILA WORKS Controlで公開版更新。Codex用文章を次工程にしない。
完了報告前にconnectorでlatest main、Open PR、対象PR state/mergedを再取得し、最後に取得したGitHub実状態を根拠とする。

## 絶対に避ける判断
connector確認なしに以下を結論づけない。
- git clone失敗 -> GitHub不可
- DNS失敗 -> Repository変更不可
- browser失敗 -> PR不可
- shell不可 -> Merge不可
- tool中断 -> 作業失敗
- PR 422 -> PR失敗
- 新チャット -> 状況不明
- 過去チャット不可 -> ユーザー再説明必須

上記の場合はまずconnectorとRepository文書から実状態を復元する。connectorで操作可能な限りGitHub作業を継続する。


## GitHub可否判定の機械的チェック
GitHub関連ターンの自己検品として、ユーザー向け返答前に以下をすべて満たす。
- [ ] 現ターンでGitHub connector action一覧または該当actionの存在を確認した
- [ ] Repository metadata / latest main / Open PRをconnectorで取得した
- [ ] 対象ファイルをconnectorで読んだ
- [ ] 書き込み依頼ならbranch/file update/PR/Mergeのwrite capabilityを実操作で確認した
- [ ] 「できない」と言う場合、connector上の具体的失敗証拠がある
- [ ] 完了報告ならlatest main / PR state / mergedを最後に再取得した

1つでも未達なら、GitHub可否や完了を断定しない。

注意: このRepository文書やプロジェクト指示は実行規律を最大限強制するためのものだが、会話モデルの挙動を技術的に100%拘束するサンドボックス機構ではない。完全な強制には、ChatGPTランタイム側で「GitHub否定回答の前にconnector preflight必須」とするsystem/developer-level gate、またはGitHub操作を単一のpreflight付きtool wrapperへ集約する実行基盤が必要。ユーザー設定でそこまでのhard enforcementが提供されない環境では、上記Gate + Repository Harness + 完了前再確認を最強の運用防止策とする。


## Compact Context / Handoff Standard

### 原則
- 会話は作業場所であり正本ではない。Repositoryを正本化した後に、過去チャットの全文・巨大な引き継ぎ文を新チャットへ再投入しない。
- Context節約は「記録を減らす」ことではなく、記録先を分離することで行う。

### 文書の責務
- `work/PROJECT_STATUS.md`: 現在地だけ。latest main、現在数値/状態、現在作業、重要な直近完了、blocker、next、production状態、handoffを短く保持する。
- `work/CURRENT_TASK.md`: 現在の1タスクだけ。完了条件と対象外を保持する。
- Git commits / PRs: 過去の実装履歴・逐次変更。
- `docs/REQUIREMENTS.md`: 永続する確定仕様。
- `docs/DECISIONS.md`: 今後の判断に効く重要決定と理由。
- `docs/UNRESOLVED.md`: 未確定事項。
- 長期履歴が人間向けに必要な場合のみarchive文書へ退避し、PROJECT_STATUSへ蓄積しない。

### 新規チャット再開
1. GitHub connectorでRepository metadata / latest main / Open PRを確認。
2. PROJECT_STATUSとCURRENT_TASKを読む。
3. 現在タスクに直接必要なHarness・対象コードだけ読む。
4. 過去履歴は必要になった時だけPR/commit/archiveから取得する。

### 引き継ぎ文書要求
ユーザーが「引き継ぎ文書ちょうだい」「会話移動する」「次チャット用にまとめて」と言った場合、GitHub利用可能時の標準出力は原則1文:

`<owner>/<repo> の続きです。GitHub connectorで PROJECT_STATUS / CURRENT_TASK / latest main / Open PR を確認して、そのまま続けてください。`

Repositoryだけでは復元できない未保存の重要事項がある場合のみ、その事項を先にRepositoryへ保存してから上記短文を返す。長文引き継ぎを標準に戻さない。
