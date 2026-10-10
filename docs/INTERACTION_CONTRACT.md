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

## Fix-until-verified Loop

不具合修正の完了単位は、ユーザーが指摘した1行・1関数・1画面ではなく、**その症状を生む因果経路全体**とする。

修正依頼を受けたら、少なくとも次を一続きで扱う。

1. ユーザーが観測した症状を再現または証拠化する。
2. 入力・保存・状態判定・非同期処理・描画・操作・副作用・永続化・再読込など、症状までの因果経路を列挙する。
3. 最初に見つかった原因だけで止めず、同じ経路上に矛盾、stale state、race、二重処理、cache、旧契約、fallback漏れが残っていないか監査する。
4. 原因を修正する。
5. 「修正箇所が存在する」testではなく、**元の症状が起きた順序・状態遷移・経路**を再現する回帰test / Acceptanceを追加する。
6. test / build / static checkだけで完了扱いせず、可能な範囲で実runtimeへ反映し、同じ入口から期待する最終状態まで確認する。
7. 途中で別の問題を自分で発見した場合、ユーザーの再指摘を待たず、同じ作業の未完了事項として原因調査→修正→再検証を続ける。
8. 最終状態が確認できるまで `BLOCKED` / `RUNNING` とし、「直った」「完了」「押してください」と言わない。

外部認証、本人確認、決済本人操作などAIが越えられない境界だけが最後に残った場合は、それ以前の機械検証を完了した上で、その**1点だけ**をユーザーへ依頼する。

## Causal Boundary Audit

「言われた箇所だけ直す」ことを防ぐため、修正前後に対象機能の境界を横断して確認する。

例:
- form入力 → API保存 → DB → dirty判定 → UI再取得 → DOM描画 → button state → action実行 → activity記録 → dirty解消
- click → Queue投入 → consumer → external API → retry / rollback → D1 commit → UI通知
- OAuth開始 → redirect URI生成 → provider → callback → state/nonce検証 → session → logout/relogin

ユーザーの症状が最後の段階にある場合でも、直前の1関数だけを原因範囲と決め打ちしない。

特に以下は横断監査対象とする。
- async render順序 / MutationObserver / stale closure / race condition
- cache / asset version / Service Worker / browser state
- persisted stateとDOM stateの不一致
- source identity / runtime identity / config identityのずれ
- retryによる二重処理、自己Mutation、polling loop
- hidden UIや旧compatibility pathへの依存
- testが実際の時系列ではなく文字列・関数存在だけを見ていないか

## UI generation rule

非同期描画されたUIへ後付けenhancementを行う場合、`projectId` やrouteだけを「処理済み」のidentityにしない。

- target elementがまだ存在しない段階で初期化完了をcacheしない。
- 同じentityでも再描画でDOM nodeが交換されたら新しいUI generationとして扱う。
- enhancement自身が起こしたtext/attribute mutationを再初期化triggerとして無限loopさせない。
- async request中にDOM generationが変わった場合、旧generation向けresponseを新しいDOMへ適用しない。
- race修正のtestは、`module load → target未描画 → async render → observer → target処理` の順序そのものを検証する。

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

静的な「関数がある」「文字列がある」「routeがある」だけのtestは、時系列・状態遷移・render timingがfailure surfaceならAcceptanceとして不足する。
その場合は、ユーザーが踏む順序を再現するtestまたは実経路検証を追加する。

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

「修正済み」と「ユーザー症状の解消確認済み」も区別する。後者を確認できていない場合、前者だけを根拠に「直った」と表現しない。

## 再発時

READYと案内した後に、事前に想定可能だったfailure surfaceで失敗した場合は、単発バグとして終わらせない。

1. 当該バグを修正する。
2. なぜ事前監査・test・canaryが捕捉できなかったかを特定する。
3. User Action Evidence Gate / Acceptance / testを更新し、同種の誤案内を機械的に困難にする。
4. 他作品へ一般化できる内容はLearning Loopへ反映する。
5. 同じユーザーに再試行を依頼する前に、追加で機械検証できる範囲を全て消化する。
