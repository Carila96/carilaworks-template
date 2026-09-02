# Project Checklist

`CARILA_WORKS_PLAYBOOK.md` の共通経験を、この作品でどう扱うか記録します。目的は「未検討のまま忘れること」を防ぐことです。

## Status

各項目は必要に応じて以下で管理します。

- `UNREVIEWED` — まだ検討していない。残タスク確認時に候補として再提示する。
- `ADOPTED` — 採用することを決めた。Requirements / Current Taskへ具体化する。
- `LATER` — 今はやらないが後で再検討する。残タスクから消さない。
- `NOT_NEEDED` — ユーザーがこの作品では不要と判断した。
- `DONE` — 採用した内容の実装・確認まで完了した。

会話に出なくなった、時間が経った、AIが重要でないと判断した、という理由だけで `NOT_NEEDED` や `DONE` に変更してはいけません。

## 企画・対象ユーザー

- 対象ユーザー: UNREVIEWED
- 中核価値・継続利用理由: UNREVIEWED
- 主対象端末: UNREVIEWED
- 年齢・利用対象制約: UNREVIEWED

## アカウント・ユーザー管理

- ログイン / 匿名利用: UNREVIEWED
- 複数端末同期: UNREVIEWED
- 退会 / アカウント削除: UNREVIEWED
- 権限管理: UNREVIEWED

## データ

- 保存対象 / 保存先: UNREVIEWED
- backup / restore: UNREVIEWED
- export / import: UNREVIEWED
- データ削除 / 保存期間: UNREVIEWED
- migration: UNREVIEWED
- Source of Truth: UNREVIEWED

## 収益・コスト

- 収益方式: UNREVIEWED
- 有料 / 無料の境界: UNREVIEWED
- 決済・解約・返金: UNREVIEWED
- API / hosting / DB等の継続コスト: UNREVIEWED

## 法務

- 利用規約: UNREVIEWED
- プライバシーポリシー: UNREVIEWED
- Cookie / analytics説明: UNREVIEWED
- 年齢確認: UNREVIEWED
- 外部素材・API利用条件: UNREVIEWED

## セキュリティ

- Secret管理: UNREVIEWED
- 入力値検証: UNREVIEWED
- 認証 / 認可: UNREVIEWED
- API濫用 / rate limit: UNREVIEWED
- Web典型脆弱性: UNREVIEWED
- ログへの機密情報混入: UNREVIEWED

## UI・互換性

- smartphone実機相当: UNREVIEWED
- iPhone Safari: UNREVIEWED
- viewport / safe area / keyboard: UNREVIEWED
- loading / empty / long content: UNREVIEWED
- 二重操作 / 戻る操作: UNREVIEWED

## エラー・障害

- 通信失敗 / timeout: UNREVIEWED
- retry / 二重登録: UNREVIEWED
- 外部API停止: UNREVIEWED
- 部分失敗 / rollback: UNREVIEWED
- ユーザー向けerror表示: UNREVIEWED

## 品質・テスト

- 正常系: UNREVIEWED
- 境界値 / 異常系: UNREVIEWED
- regression: UNREVIEWED
- build / typecheck / lint / tests: UNREVIEWED
- 作品固有Acceptance: UNREVIEWED

## 運用・公開

- 障害検知 / analytics: UNREVIEWED
- 管理画面 / 問い合わせ: UNREVIEWED
- version / migration運用: UNREVIEWED
- favicon / metadata / OGP: UNREVIEWED
- production / HTTPS / debug残存: UNREVIEWED
- サービス終了時対応: UNREVIEWED

## 残タスクを聞かれた時の扱い

`UNREVIEWED` と `LATER` のうち、現在の制作段階と作品特性に関係する項目を、実装タスクとは分けて「次に検討する候補」として提示する。全項目を一度に列挙して会話を埋めず、優先度の高いものから提示する。
