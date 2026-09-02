# CARILA WORKS Project Template

CARILA WORKS Control が新しい作品 Repository を作成するための、hosting provider・framework 非依存の金型です。完成作品ではありません。

Control は作成時に `README.md`、`PROJECT_BRIEF.md`、`carila-project.json` の placeholder を作品固有情報へ置換・設定します。その後、Codex で要件に沿って制作し、変更を `main` へ merge すると Preview が自動更新されます。Control で Preview を確認し、公開操作は Control から行います。

## 初期状態

- ルートの最小 `index.html` により `STATIC_SOURCE` として判定可能
- lifecycle は安全な初期値 `WORKING`
- repository、preview、production URL および subdomain は空文字で未設定
- framework、build tool、release adapter、hosting provider は未固定
- 長期制作の文脈を維持する CARILA WORKS Harness を同梱

## Harness

作品制作では `AGENTS.md` を入口として、役割の異なる文書を分離して管理します。

```text
AGENTS.md
PROJECT_BRIEF.md
carila-project.json

docs/
  CONSTITUTION.md
  REQUIREMENTS.md
  DECISIONS.md
  UNRESOLVED.md

work/
  ROADMAP.md
  CURRENT_TASK.md
  PROJECT_STATUS.md

evals/
  ACCEPTANCE.md
```

- `PROJECT_BRIEF.md`: 企画から渡された作品要件の原文
- `CONSTITUTION.md`: 作品の目的・守る価値・不変原則
- `REQUIREMENTS.md`: 合意済みの確定仕様
- `DECISIONS.md`: 重要な決定と理由
- `UNRESOLVED.md`: 未確定事項・確認待ち
- `ROADMAP.md`: 制作工程
- `CURRENT_TASK.md`: 今回の作業範囲と完了条件
- `PROJECT_STATUS.md`: 現在地と次回への引き継ぎ
- `ACCEPTANCE.md`: 完成判定の検品条件

Harness文書は全部を毎回更新するものではありません。実装によって事実が変わった文書だけを更新し、仕様・決定・未確定事項・現在地を混在させないことを目的とします。

## 制作フロー

1. Control がこの金型から作品 Repository を作り、作品固有情報と brief を設定する。
2. `AGENTS.md` と `PROJECT_BRIEF.md` を読み、Harnessを作品固有内容へ合わせる。
3. `CURRENT_TASK.md` に今回の範囲と完了条件を置き、Codexで制作する。
4. 実装と同時に、変更された事実に対応するHarness文書を更新する。
5. `ACCEPTANCE.md` と利用可能なテストで検品する。
6. `PROJECT_STATUS.md` を次回そのまま再開できる状態へ更新する。
7. Pull Request を review して `main` へ merge する。
8. 自動更新された Preview を Control で確認する。
9. lifecycle と公開先を Control で管理し、承認後に公開する。

`carila-project.json` は Repository 契約および Control metadata の mirror です。manifest の編集によって公開状態を変更したり、release 方式を指定したりしません。

## 発展できる構成

- **STATIC_SOURCE**: ルートの `index.html` をそのまま使用する。
- **STATIC_BUILD_OUTPUT**: 必要な framework/build tool を作品側で導入し、`dist`、`build`、または `out` の commit 済み成果物を提供する。
- **WORKER_APP**: Wrangler 設定と Worker entrypoint を追加し、Cloudflare Workers の契約を満たす。

Control が Repository 構成から capability と release adapter を判定します。手動 deploy や Vercel を標準前提にはしません。
