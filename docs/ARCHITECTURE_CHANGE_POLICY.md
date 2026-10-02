# Architecture Change Policy

この文書は、CARILA WORKS作品の不具合修正・機能追加・運用改善で、場当たり的な追加実装を避けるための共通設計ゲートです。

## 原則

局所的に動く修正より、作品全体のSource of Truth、既存経路、依存、運用コストを壊さない修正を優先する。同じ目的の経路を安易に増やさず、可能なら既存のcanonical pathを修正・置換して1つへ収束させる。

## 実装前に確認する7項目

1. **Source of Truth** — 同じ状態・設定・結果を複数箇所で管理していないか。
2. **Caller / Route Map** — 対象を呼ぶUI、API、worker、queue、workflow、script、test、fallbackを確認したか。
3. **Existing Path First** — 新しいroute・script・workflow・adapterを作る前に既存経路で解決できないか。
4. **Legacy / Dead Path** — 旧実装、compat shim、古い定数、未使用route、重複workflow、古いfixtureが残っていないか。
5. **Failure Surface** — timeout、retry、二重実行、stale state、partial success、rollbackを確認したか。
6. **Cost Surface** — GitHub Actions、外部API、DB、Queue、storage等の実行回数・費用が増えないか。
7. **Module Boundary** — 巨大ファイルへ条件分岐を継ぎ足すより、責務を分離した方が安全か。

## 完了前に必ず記録するDELTA

共通基盤や運用に関わる変更では、PR・完了報告・必要なstatus文書で次を確認する。

```text
DEPENDENCY DELTA: NONE | <changes>
ROUTE DELTA: NONE | <added/removed/replaced routes>
ACTIONS DELTA: NONE | <workflow/run-trigger changes>
LEGACY CLEANUP: NONE | <removed/replaced legacy paths>
```

旧経路を残す場合は、互換性等の具体的理由と削除条件を残す。「念のため」で恒久残存させない。

## GitHub Actions

- Repository操作はGitHub connectorを優先し、Actionsを編集・commit・PR・Mergeの代替にしない。
- ActionsはCI/build/test/deploy/schedule等、runnerが必要な処理だけに使う。
- 調査途中の仮説検証でActionsを反復しない。非Actions検証を先に行い、Actionsは最終ゲートにする。
- Draft中の細かな更新でCIを起動しない。Ready / Merge候補になってから必要な最終CIを行う。
- bot commitが別workflowを連鎖発火しないか確認する。
- schedule頻度は必要成果物の頻度から設計し、batch可能な処理を細切れrunにしない。
- docs / Harness / status / metadataだけの変更でdeployを起動しない。

## リファクタリング

構造改善は機能変更と無関係な大規模書き換えを一度に行わない。既存test・contractを保持したまま責務単位で段階的に抽出し、各段階で挙動不変を確認する。大きなmoduleが変更のたびに別機能へ波及する場合は、局所パッチを続けるよりmodule境界の整理を優先課題として記録する。
