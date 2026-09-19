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
Repository作業開始時は原則connectorで以下を確認する。
1. Repository metadata
2. latest main
3. 対象branch
4. Open PR
5. 対象ファイル
6. mainとの差分
7. 必要時PRのstate / merged

書き込み依頼では、branch作成、file update、PR作成/更新、Mergeがconnectorで可能か実際に確認する。

「GitHubへ書き込めない」「PRを作れない」「Mergeできない」「この環境では実行不能」「ユーザー側で作業が必要」と答える前に、必ず同一ターン内でconnectorを直接確認する。connector未確認でGitHub操作不能と結論づけることを禁止する。

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
