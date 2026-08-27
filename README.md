# CARILA WORKS Project Template

CARILA WORKS Control が新しい作品 Repository を作成するための、hosting provider・framework 非依存の最小金型です。完成作品ではありません。

Control は作成時に `README.md`、`PROJECT_BRIEF.md`、`carila-project.json` の placeholder を作品固有情報へ置換・設定します。その後、Codex で要件に沿って制作し、変更を `main` へ merge すると Preview が自動更新されます。Control で Preview を確認し、公開操作は Control から行います。

## 初期状態

- ルートの最小 `index.html` により `STATIC_SOURCE` として判定可能
- lifecycle は安全な初期値 `WORKING`
- public、preview、production URL および subdomain は未設定
- framework、build tool、release adapter、hosting provider は未固定

## 制作フロー

1. Control がこの金型から作品 Repository を作り、作品固有情報と brief を設定する。
2. `PROJECT_BRIEF.md` と `AGENTS.md` を確認し、Codex で作品を制作する。
3. Pull Request を review して `main` へ merge する。
4. 自動更新された Preview を Control で確認する。
5. lifecycle と公開先を Control で管理し、承認後に公開する。

`carila-project.json` は Repository 契約および Control metadata の mirror です。manifest の編集によって公開状態を変更したり、release 方式を指定したりしません。

## 発展できる構成

- **STATIC_SOURCE**: ルートの `index.html` をそのまま使用する。
- **STATIC_BUILD_OUTPUT**: 必要な framework/build tool を作品側で導入し、`dist`、`build`、または `out` の commit 済み成果物を提供する。
- **WORKER_APP**: Wrangler 設定と Worker entrypoint を追加し、Cloudflare Workers の契約を満たす。

Control が Repository 構成から capability と release adapter を判定します。手動 deploy や Vercel を標準前提にはしません。
