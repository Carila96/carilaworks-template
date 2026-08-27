# CARILA WORKS 作品制作ルール

このリポジトリは CARILA WORKS Control から作成される作品リポジトリです。作業前に `README.md`、`PROJECT_BRIEF.md`、`carila-project.json`、既存コードと、より狭い範囲の `AGENTS.md` を確認してください。

## 制作

- `PROJECT_BRIEF.md` を作品要件の正として扱い、不明な仕様を推測で補わない。
- 依頼に必要な最小限の変更を行い、実装と資料を一致させ、関連するテスト、ビルド、静的解析を実行する。
- React、Vite、Next.js など特定のフレームワークや hosting provider を前提にしない。必要になった時点で、作品要件に合う構成を選ぶ。

## Repository capability と release

- release adapter を作品側の判断で固定しない。Control が Repository の構成から capability と adapter を判定する。
- 最小 static 作品はルートの `index.html` を公開ソースとする（`STATIC_SOURCE`）。
- build 型へ移行する場合は `dist`、`build`、または `out` に成果物を生成し、Control が参照できるよう成果物を commit する（`STATIC_BUILD_OUTPUT`）。採用した出力先は実態に合わせる。
- Worker 型へ移行する場合は Wrangler の設定および entrypoint を含む Cloudflare Workers の Repository 契約を満たす（`WORKER_APP`）。
- Vercel 固有の設定や API を標準契約として追加しない。作品要件として明示された場合に限り検討する。

## lifecycle と manifest

- `WORKING`、`PUBLIC`、`COMING_SOON`、`MAINTENANCE`、`ARCHIVED` などの lifecycle を Codex の判断で変更しない。
- lifecycle の唯一の正は Control の D1 である。公開・非公開の変更は Control で行う。
- `carila-project.json` は Repository 契約と、Control が管理する metadata の mirror である。manifest の `status` を変更して Control の公開状態を操作しない。
- `releaseAdapterType` は互換性のための metadata mirror であり、作品側が技術方式を指定または固定する設定ではない。
- `outputDirectory` は build 成果物の出力先が実際に必要になるまで設定しない。
- `schemaVersion` は明示された移行なしに変更せず、作品固有情報は Control による作成時の置換値を維持する。

## セキュリティと外部操作

- Secret、API key、access token、password、秘密鍵、個人情報を Git に保存しない。秘密値は Git 管理外の環境変数または承認された Secret 管理機能を使う。
- 公開、deploy、release、DNS・subdomain の変更、課金、破壊的操作など外部状態を変える操作は、対象と影響を説明し、ユーザーの明示的な承認を得てから行う。
- 生成物（Control が契約上必要とする commit 済み build 成果物を除く）、local 設定、機密情報を commit しない。
