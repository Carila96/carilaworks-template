# Playbook Candidates

この文書は、この作品の制作中に得た「別作品でも再利用できそうな知見」を一時保存する候補箱です。`CARILA_WORKS_PLAYBOOK.md` 本体へ直接昇格させる前に、作品固有事情から一般化できるかを確認します。

## ルール

- Codex / AI は、制作中に再利用可能な失敗・バグ原因・回避策・設計原則・検品観点を見つけたら、ユーザーから指示がなくてもここへ候補として追記する。
- 単発の作品固有事情、個人的な好み、未検証の推測は候補化しない。
- 同じ内容が既に `CARILA_WORKS_PLAYBOOK.md` またはこのファイルにある場合は重複追加しない。
- product philosophy、課金方針、公開方針など作品やCARILA WORKSの思想を変える内容は、勝手に共通ルール化せずユーザー確認対象とする。
- 候補をPlaybookへ昇格したら、ここでは `PROMOTED` と記録する。却下・作品固有と判断した場合は `REJECTED` と理由を残す。
- 候補が残っている場合、作業完了報告や引き継ぎ時に「Playbook候補あり」と明示する。

## Status

- `CANDIDATE` — 共通化候補。未判定。
- `PROMOTED` — CARILA WORKS Playbookへ反映済み。
- `REJECTED` — 共通化しないと判断済み。

## 候補

### 2026-10-03: Eventual consistencyは最初のidentity probeから待つ
- Status: CANDIDATE
- 発見した事象: Custom Domainのcontrol-plane recordが新candidateを指した後でもedge routing反映前はadapter-owned health probeがHTTP 530になり得る。後段root probeに十分な待機時間があっても、最初のidentity probeが短いとそこへ到達せずrollbackする。また、rollback後にrelease全体を作り直すgeneric retryはcandidate/domainを再生成し、propagation待ちをゼロからやり直す副作用がある。
- 原因 / 背景: eventual consistency対策を後段probeには入れていたが、最初のidentity/control probeと待機budgetを揃えていなかった。failure familyを区別せず5xx全般をwhole-release retry対象にしていた。
- 一般化した知見: control-plane成功からedge readinessを検証する多段releaseでは、最初のidentity probeから同一resource identityを保持して十分なbounded propagation windowを設ける。edgeの一時応答にはcache-bustも検討する。十分なwindowを使い切った後にresourceをtear down/recreateするretryは収束をリセットし得るため、failure family別にretry classifierを持つ。テストは実装構文ではなく、待機budget・identity retention・rollback/retry side effectをassertする。
- どんな作品で有効か: CDN / edge / Custom Domain / DNS / Queueなどeventual consistencyを伴うdeploy・Preview・Production更新。
- 根拠 / 確認方法: C-LINKs初回Productionの`HEALTH_CHECK / HTTP 530`、Control PR #221、branch verification run `37107738523`（208 tests PASS / check PASS）、Production run #211 `37107947468`（新runtime fingerprintのsame-candidate safety gate READY、workflow SUCCESS）。
- Playbook反映先候補: Release / deployのeventual consistency、retry policy、CI_LAST_GATE / failure-surface audit。
