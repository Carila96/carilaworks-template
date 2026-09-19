# Current Task

## 目的
CARILA WORKS全体の詳細実行ルールをRepository側へ永続化し、短いプロジェクト指示に依存せず新規チャット/新規作品から再現できる状態にする。

## 今回やること
- docs/CARILA_WORKS_EXECUTION_RULES.md を追加し、GitHub connector-first、Mergeまで、途中停止禁止、中断復旧、ユーザー指示忠実性、Actions役割分離、完了報告を詳細化。
- AGENTS.md のHarness読み順先頭へexecution rulesを追加し、詳細ルールの正本として参照。
- CARILA_WORKS_PLAYBOOK.md からexecution rulesへリンク。
- PROJECT_STATUSを更新。
- PR作成・Merge。

## 完了条件
- 詳細実行ルールがmainへMerge済み。
- AGENTS.mdから必須参照される。
- Playbookと役割が分離されている。
- latest main / PR mergedをGitHub connectorで確認済み。

## 今回やらないこと
- 既存作品へのTemplate変更の一括反映。
- Production公開操作。

## 関連する確定仕様
- CARILA WORKS標準制作フロー。
