# CARILA WORKS Project Template

CARILA WORKS Control が新しい作品 Repository を作成するための、hosting provider・framework 非依存の金型です。完成作品ではありません。

Control は作成時に `README.md`、`PROJECT_BRIEF.md`、`carila-project.json` の placeholder を作品固有情報へ置換・設定します。その後、Codex で要件に沿って制作し、変更を `main` へ merge すると Preview が自動更新されます。Control で Preview を確認し、公開操作は Control から行います。

## 初期状態

- ルートの最小 `index.html` により `STATIC_SOURCE` として判定可能
- lifecycle は安全な初期値 `WORKING`
- repository、preview、production URL および subdomain は空文字で未設定
- framework、build tool、release adapter、hosting provider は未固定
- 長期制作の文脈を維持する CARILA WORKS Harness を同梱
- 過去の制作経験を次の作品へ持ち越す `CARILA_WORKS_PLAYBOOK.md` と、作品ごとの検討状況を保持する `work/PROJECT_CHECKLIST.md` を同梱
- 制作中に得た再利用可能な知見を自動候補化する `work/PLAYBOOK_CANDIDATES.md` を同梱

## Harness

作品制作では `AGENTS.md` を入口として、役割の異なる文書を分離して管理します。

```text
AGENTS.md
PROJECT_BRIEF.md
CARILA_WORKS_PLAYBOOK.md
carila-project.json

docs/
  CONSTITUTION.md
  REQUIREMENTS.md
  DECISIONS.md
  UNRESOLVED.md

work/
  ROADMAP.md
  CURRENT_TASK.md
  PROJECT_CHECKLIST.md
  PLAYBOOK_CANDIDATES.md
  PROJECT_STATUS.md

evals/
  ACCEPTANCE.md
```

- `PROJECT_BRIEF.md`: 企画から渡された作品要件の原文
- `CARILA_WORKS_PLAYBOOK.md`: CARILA WORKS全体で蓄積した制作経験・検討候補・再発防止策
- `CONSTITUTION.md`: 作品の目的・守る価値・不変原則
- `REQUIREMENTS.md`: 合意済みの確定仕様
- `DECISIONS.md`: 重要な決定と理由
- `UNRESOLVED.md`: 未確定事項・確認待ち
- `ROADMAP.md`: 制作工程
- `CURRENT_TASK.md`: 今回の作業範囲と完了条件
- `PROJECT_CHECKLIST.md`: Playbookの各論点をこの作品で採用・後回し・不要・完了のどれとして扱うか
- `PLAYBOOK_CANDIDATES.md`: この作品で得た、他作品にも還元できそうな知見の候補箱
- `PROJECT_STATUS.md`: 現在地と次回への引き継ぎ
- `ACCEPTANCE.md`: 完成判定の検品条件

Harness文書は全部を毎回更新するものではありません。実装によって事実が変わった文書だけを更新し、仕様・決定・未確定事項・現在地を混在させないことを目的とします。

## Playbook の考え方

Playbookは「全部実装するチェックリスト」ではなく、「一度は検討して、不要なら不要と明示するための経験値帳」です。

- 未検討の項目は `UNREVIEWED` のまま保持する。
- 今はやらない項目は `LATER` として保持する。
- ユーザーがこの作品では不要と判断した項目だけ `NOT_NEEDED` とする。
- 採用した項目は `ADOPTED`、実装・確認完了後は `DONE` とする。
- 「次に何をやる？」「残タスクは？」と聞かれた時は、通常の実装タスクに加えて、現在の制作段階に関係する `UNREVIEWED` / `LATER` を提案候補として出す。

## 経験値の学習ループ

- Codex / AI は制作中に、別作品でも再利用できる失敗・バグ原因・回避策・設計原則・検品観点を見つけたら、ユーザーから指示がなくても `work/PLAYBOOK_CANDIDATES.md` へ候補として残す。
- 候補は作品固有事情そのものではなく、別作品でも使える形へ一般化する。
- 単なる好み、未検証の推測、一作品だけの特殊事情は候補にしない。
- product philosophyや課金・公開方針など思想を変える内容は勝手に共通ルール化しない。
- 候補は `CANDIDATE` → `PROMOTED` / `REJECTED` で処理し、共通化に値するものだけ `CARILA_WORKS_PLAYBOOK.md` へ昇格する。
- 未処理候補がある場合、作業完了・引き継ぎ時に「Playbook候補あり」と報告する。

## 制作フロー

1. Control がこの金型から作品 Repository を作り、作品固有情報と brief を設定する。
2. `AGENTS.md`、`PROJECT_BRIEF.md`、`CARILA_WORKS_PLAYBOOK.md` を読み、Harnessを作品固有内容へ合わせる。
3. `PROJECT_CHECKLIST.md` で、現在の段階で検討すべき共通論点を確認する。
4. `CURRENT_TASK.md` に今回の範囲と完了条件を置き、Codexで制作する。
5. 実装と同時に、変更された事実に対応するHarness文書を更新する。
6. 制作中に再利用可能な学びが出たら `PLAYBOOK_CANDIDATES.md` へ自動候補化する。
7. 企画・制作の節目ではChecklistの未検討・後回し項目から次の検討候補を提案する。
8. `ACCEPTANCE.md` と利用可能なテストで検品する。
9. `PROJECT_STATUS.md` を次回そのまま再開できる状態へ更新する。
10. Pull Request を review して `main` へ merge する。
11. 自動更新された Preview を Control で確認する。
12. lifecycle と公開先を Control で管理し、承認後に公開する。

`carila-project.json` は Repository 契約および Control metadata の mirror です。manifest の編集によって公開状態を変更したり、release 方式を指定したりしません。

## 発展できる構成

- **STATIC_SOURCE**: ルートの `index.html` をそのまま使用する。
- **STATIC_BUILD_OUTPUT**: 必要な framework/build tool を作品側で導入し、`dist`、`build`、または `out` の commit 済み成果物を提供する。
- **WORKER_APP**: Wrangler 設定と Worker entrypoint を追加し、Cloudflare Workers の契約を満たす。

Control が Repository 構成から capability と release adapter を判定します。手動 deploy や Vercel を標準前提にはしません。
