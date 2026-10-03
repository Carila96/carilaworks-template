# CARILA WORKS Interaction Contract

この文書は、ChatGPT / AIがユーザーへ「押してください」「更新して大丈夫です」「公開できます」など実環境操作を依頼・許可する際の契約です。

## 原則

AIの文章上の判断を安全性の正本にしない。
Repository、実コード、実環境、機械的gate、実経路Acceptanceの証拠を優先する。

「実装した」「testが通った」「Merge済み」「CI green」「Production workflow SUCCESS」は、それぞれ重要な証拠だが、ユーザーが次の実操作を安全に行えることと同義ではない。

## User Action Evidence Gate

ユーザーへ実環境の更新・公開・移行・破壊的操作・課金を伴う操作を依頼する前に、対象操作について以下を確認する。

1. 対象runtime / source / configurationのidentityを特定できる。
2. 既知failure surfaceを列挙し、静的に確認可能な問題を残していない。
3. 変更による副作用・回帰先を監査している。
4. 対象操作と同一または安全側に厳しい実経路Acceptanceが成功している。
5. 外部control-planeのeventual consistency、retry、timeout、stale state等が関係する場合、その経路を実際に検証している。
6. rollback / fail-closed条件を確認している。
7. 実装に機械的READY/BLOCKED gateが存在する場合、現在runtimeのgateがREADYである。
8. runtime / source変更後は、過去runtimeのREADY証拠を使い回していない。

1つでも満たせない場合、AIは「大丈夫」「押してください」「公開してください」と案内してはならない。状態はBLOCKEDまたは未検証として扱い、可能な限り自分で調査・修正・検証を続ける。

## Mechanical Gate優先

対象システムに機械的な操作gateを実装できる場合、文章ルールだけで済ませず実装する。

- `READY`: 現在runtimeについて必要な実経路Acceptanceが成功しており、対象操作を許可できる。
- `BLOCKED`: 未検証、既知blockerあり、または必要証拠が不足している。
- `RUNNING`: 現在runtimeを検証中。ユーザー操作はまだ許可しない。
- `FAILED`: 検証失敗。ユーザー操作は許可しない。

runtimeや安全契約が変わった場合は、旧READYを自動的に失効させる設計を優先する。

AIは機械的gateを上書きしてはならない。AIが「大丈夫」と誤って判断しても、gateがBLOCKED/RUNNING/FAILEDならユーザー操作を通さない構造をAcceptanceとする。

## Same-path Acceptance

canaryやtestは「似た部品が動く」だけでは不十分な場合がある。

ユーザー操作が Queue、background job、edge propagation、Custom Domain、D1、Runtime Secret等を通るなら、重要なfailure surfaceについて同じ経路を通すAcceptanceを優先する。

モックだけで外部依存を代替し、実際の必須依存が未確認のままREADYにしない。

## 既知問題の扱い

既知failure surfaceを「今回はたまたま起きないだろう」として無視しない。
既知問題を完全除去できない場合は、少なくとも対象操作をBLOCKEDにする、retry/rollback/fail-closedで安全側へ倒す、またはユーザーへ残存リスクを明示する。

## ユーザーへの完了報告

ユーザー操作を案内する場合は、少なくとも次を区別して報告する。

- 実装済み
- 静的/自動test済み
- Production反映済み
- 実経路Acceptance済み
- Mechanical gate状態
- ユーザーが今行ってよい具体的操作

証拠が揃っていない工程を、先の工程まで完了したように表現しない。

## 再発時

READYと案内した後に、事前に想定可能だったfailure surfaceで失敗した場合は、単発バグとして終わらせない。

1. 当該バグを修正する。
2. なぜ事前監査・test・canaryが捕捉できなかったかを特定する。
3. User Action Evidence Gate / Acceptance / testを更新し、同種の誤案内を機械的に困難にする。
4. 他作品へ一般化できる内容はLearning Loopへ反映する。
