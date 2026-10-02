# CARILA WORKS Playbook

> 実行手順・GitHub操作・中断復旧・指示忠実性の詳細ルールは `docs/CARILA_WORKS_EXECUTION_RULES.md` を参照する。このPlaybookは経験知・検討観点の正本であり、実行手順の正本ではない。

この文書は、作品ごとの仕様書ではありません。CARILA WORKSでアプリ・ゲーム・Webサービスを作るたびに得た「次回以降も一度は検討したい論点」「よくある失敗」「再発防止策」を蓄積する共通の経験値帳です。

## 使い方

- 新作では、ここにある項目を最初からすべて実装しない。
- 企画・制作が進み「次に何を詰めるか」「残タスクは何か」となった時に、作品に関係する未検討項目を提案する。
- ユーザーが明確に「今回は不要」と判断した項目だけ、その作品では `NOT_NEEDED` として扱う。
- 何も判断されていない項目を、会話から消えたことを理由に完了・不要扱いしない。
- 後回しにした項目は `LATER` として残し、適切な段階で再提示する。
- 実装することが決まった項目は作品側の `docs/REQUIREMENTS.md` や `work/CURRENT_TASK.md` へ落とす。
- 重要な判断理由は `docs/DECISIONS.md` に残す。
- 新しい失敗や有効だった再発防止策が見つかったら、このPlaybookへ一般化して追加する。

## 企画・対象ユーザー

- 誰が使うのか。自分専用、限定公開、不特定多数のどれか。
- ユーザーが最初に得る価値は何か。
- 継続して使う理由は何か。
- 使わなくなる理由・面倒になる要因は何か。
- PC / smartphone / tablet のどれを主対象にするか。
- 年齢制限や利用対象の制約が必要か。

## アカウント・ユーザー管理

- ログインは必要か。
- 匿名利用で成立するか。
- 複数端末同期が必要か。
- アカウント作成・退会・削除が必要か。
- password reset、session期限、重複アカウントをどう扱うか。
- 管理者と一般ユーザーの権限差が必要か。

## データ

- 何を保存するか。
- 保存先はlocal / browser / server / DBのどれか。
- 消えると困るデータか。
- backup / restore が必要か。
- export / import が必要か。
- データ削除機能が必要か。
- 保存期間を決める必要があるか。
- schema変更時のmigrationをどうするか。
- 同じ状態を複数箇所で管理してSource of Truthが分裂していないか。

## 収益・コスト

- 無料、広告、買い切り、月額、従量課金、寄付など、収益方式を検討したか。
- 無料版と有料版の差をどうするか。
- 決済失敗、解約、返金、二重課金への対応が必要か。
- API、DB、hosting、storage等の継続コストを把握しているか。
- ユーザー増加時にコストが急増する箇所はないか。
- 収益性だけでなく、自走性・維持コスト・寿命・他サービスとの分散効果を見たか。


## Growth・Distribution・収益化

公開済み作品は「作れたか」だけでなく、次の5段階のどこが最大ボトルネックかを確認する。

1. **Discovery** — 未来の顧客の視界に入っているか
2. **Appeal** — 見た瞬間に「試したい理由」が伝わるか
3. **Activation** — 最初の価値体験まで迷わず進めるか
4. **Monetization** — 価値を支払いへ変換する導線があるか
5. **Retention** — 再訪・継続利用する理由があるか

Distributionは1つのSNSだけに依存せず、作品に合う以下の3経路を一度は検討する。

- **Push** — X / Threads / 動画 / community / creator outreachなど、こちらから届ける経路。
- **Pull** — SEO / marketplace / directory / discovery platformなど、探している人に見つかる経路。
- **Loop** — share / referral / 公開結果 / UGCなど、利用者が次の利用者を連れてくる経路。

収益化対象の作品では、可能なら「露出 → 興味 → 初回体験 → Checkout → Purchase」を分離して計測する。
CARILA WORKS Controlへ匿名集計を自動連携する場合は、任意の共通契約 `GET /api/carila-business-metrics`（schemaVersion 1.0）を利用できる。未対応でも作品の公開を妨げない。
公開Metricsにはraw event、個人情報、Secret、非公開の売上額を含めない。売上額の自動集約が必要な場合は別途認証付き経路を設計する。

買い切り・低単価商品は有効な収益化実験になり得るが、価格を下げることをDiscovery不足の代替にしない。価値が一度で完結する、成果物をすぐ受け取れる、subscription理由が弱い等の条件で検討する。


## Analytics・検索発見性

公開作品では、可能な限り作品固有の思い込みではなく実測で改善判断できるよう、匿名利用計測を検討する。

CARILA WORKS標準の匿名Analyticsを使う場合は、Controlが配信する共通clientを利用し、最低限以下を観測候補とする。

- page_view
- 流入元（UTM / referrer）
- session / 再訪
- PWA standalone起動
- install event（取得可能browserのみ）
- 外部link click
- share
- 作品固有の主要Activation event

個人情報、email、氏名、入力本文、健康情報等をAnalytics eventへ混ぜない。
visitor / session IDを利用する場合も、Control側へraw IDを永続保存せず匿名集計目的に限定する。
計測基盤が停止しても作品本体を止めないbest-effort設計を優先する。

PWAの「ホーム画面に追加」はbrowser差がある。
Chromium系では `appinstalled` を取得できる場合があるが、iOS Safariでは追加操作そのものをWeb側で直接取得できない。
iOSでは `navigator.standalone` / `display-mode: standalone` による「ホーム画面追加後の実起動」を利用指標とする。

検索流入を作る作品では、最低限以下を一度確認する。

- 意図が伝わる `<title>`
- 検索結果向けdescription
- production canonical URL
- OGP / Twitter Card
- `<html lang>`
- noindexが意図せず残っていない
- `robots.txt`
- `sitemap.xml`
- 内容に合うJSON-LD構造化データ
- 日本語 / 英語等の多言語ページが独立URLを持つ場合はhreflang

SEOはmeta tagを置くだけで完了扱いせず、「誰が何を検索した時にこの作品へ来るのか」という検索意図とページ内容を一致させる。
大量の低品質keywordページを作ることをSEO施策としない。

## 法務・公開ルール

- 利用規約が必要か。
- プライバシーポリシーが必要か。
- Cookieやanalyticsの説明・同意が必要か。
- 個人情報・健康情報・位置情報など慎重に扱う情報があるか。
- 年齢確認が必要か。
- 課金する場合に必要な表示や説明があるか。
- 外部素材、画像、音源、フォント、APIの利用条件を満たしているか。

## セキュリティ

- Secret、API key、tokenをclient側やGitへ出していないか。
- 入力値を信用しすぎていないか。
- 認証だけでなく認可も確認しているか。
- 他人のデータをID変更だけで参照・更新できないか。
- APIの濫用、連打、bot、rate limitを考える必要があるか。
- XSS、CSRF、injection、open redirectなどWebの典型的問題を確認したか。
- 公開範囲が意図せず広がる経路がないか。
- エラーログにSecretや個人情報を残していないか。

## UI・端末互換性

- smartphoneの実機幅で確認したか。
- iPhone Safari / Chrome系browserで主要画面を確認したか。
- keyboard表示、safe area、viewport高さ、orientation変更で崩れないか。
- JSで画面サイズを過剰に計算せず、CSSで解決できる配置を優先できないか。
- loading中、空データ、長文、非常に大きい数値で崩れないか。
- buttonの二重押しや戻る操作で状態が壊れないか。
- touch targetが小さすぎないか。

## エラー・障害

- 通信失敗時に何が起きるか。
- API timeout時に永遠に待たないか。
- retryして安全な処理か。二重登録にならないか。
- 外部API停止時に最低限使える範囲はあるか。
- DB書き込み途中で失敗した時に中途半端な状態が残らないか。
- destructive operationに確認・rollback・backupが必要か。
- ユーザーへ技術的すぎないエラー表示ができるか。

## 品質・テスト

- 中核機能の正常系を確認したか。
- empty / invalid / maximum / duplicate 等の境界値を確認したか。
- 既存機能を壊していないか。
- build / typecheck / lint / tests のうち利用可能なものを実行したか。
- 実装者自身が作ったコードだけを読んで「問題なし」と判定していないか。
- 公開前に作品固有のAcceptanceを満たしているか。

## 運用・監視

- 障害に気付く方法があるか。
- analyticsや利用状況の計測が必要か。
- 管理画面が必要か。
- 問い合わせ先や問題報告導線が必要か。
- version更新、DB migration、古いclientとの互換性をどう扱うか。
- 外部サービス終了・料金改定時の代替策が必要か。
- サービス終了時にユーザーデータをどうするか。
- 自動deployがある場合、docs / Harness / metadataだけの変更まで再deploy対象になっていないか。deployに影響するpathと影響しないpathを区別できるか。

## 公開前チェック

- favicon、title、description、OGP等が必要か。
- production URL、domain、HTTPSが正しいか。
- Preview用設定やdebug表示が本番に残っていないか。
- test data、dummy user、不要なconsole logが残っていないか。
- 公開範囲と検索エンジンindex可否が意図通りか。
- 利用規約・privacy・問い合わせ等、必要と判断した公開情報へ到達できるか。

## Repository操作能力の確認

- **経験:** containerの `git clone`、DNS、browser、raw URL等の一経路が失敗しただけで「このセッションではGitHub操作できない」と判断すると、実際には認証済みGitHub connectorが利用可能でも作業を止めてしまう。
- **一般化:** Repository操作可否は、その操作を担う認証済みGitHub connector / API経路を直接試して判定する。別経路の失敗をGitHub全体の失敗へ一般化しない。
- **必須確認:** 「GitHub操作不可」「Merge不可」と報告する前に、Repository metadata、default branch最新commit、Open PR、対象ファイルreadを確認する。書き込み依頼では通常の作業branch作成等のsafe writeも実際に試す。
- **blocked判定:** connector/API側の認証・権限・service failureが確認できた場合に限りblockedとし、失敗した操作とエラーをRepositoryのstatus/handoffへ記録する。
- **目的:** 会話内の決意ではなく、次のAI・別セッション・別作品でも同じ確認手順を再現できるようにする。

## 経験値の追加ルール

新しい項目を追加するときは、単発の作品固有事情ではなく、今後の別作品でも再発し得る形へ一般化する。

記録例:

- **経験:** iOS SafariでJS取得の高さを使った中央配置が初回表示でずれた。
- **一般化:** viewport依存配置はまずCSSの `position: fixed; inset: 0`、flex/grid、safe-area等で解決できないか確認し、JSによる高さ計算を最後の手段にする。

- **経験:** lifecycleをRepository manifestとControl DBの双方から変更可能にすると状態競合が起こり得る。
- **一般化:** 重要状態には単一のSource of Truthを定め、mirror側から状態遷移を発生させない。

- **経験:** Harnessや進捗文書の更新だけでも、pathを区別しないpush webhookではPreview deployが発火し得る。
- **一般化:** 自動deployのtriggerは「Repositoryが変わったか」ではなく「実行成果物へ影響するpathが変わったか」で判定し、docs・運用metadataだけの更新は不要なdeployから除外する。


## 無記名投票BOXから得た共通知見（2026-09-11）

- **経験:** unit / contract / runtime testが通っていても、公開後の実ユーザー経路でだけ失敗する箇所が残った。
- **一般化:** 公開を伴う作品では、中核フローをproduction URLから実際に1件通し、保存先・後段処理・最終状態まで確認するProduction E2Eを公開完了条件に含める。コード上のテスト成功だけで「運用可能」と判定しない。

- **経験:** GitHub Actionsのcronを指定時刻どおりに実行される前提にすると、自動収集が予定時刻を過ぎても動かないことがあった。
- **一般化:** 外部schedulerは遅延・欠落し得る前提で設計する。時刻ぴったりを業務保証にせず、重要処理には複数回poll、別trigger、再実行可能性、未処理backlogを持たせる。自動化の正しさは「指定時刻」ではなく「最終的に取りこぼさず処理されること」で評価する。

- **経験:** 一般ユーザーデータと公式収集データを同一DBに置くと、将来の誤参照・誤収集事故の境界が弱くなる。
- **一般化:** 用途・同意・公開範囲が異なるデータ群は、必要ならtable分離だけでなくDB / binding自体を物理分離する。専用storageが未設定の時に別storageへfallbackせず、fail closedで停止する。

- **経験:** 自動処理を複数段に分けた際、途中状態が見えないと「どこで止まったか」を追えない。
- **一般化:** 自動pipelineは可能な範囲で raw / reviewed / pending / processed / result のように段階ごとの証跡を残し、入力・判定・送信・結果を後から追跡できるようにする。再実行時はidempotencyと重複防止を組み込む。

- **経験:** service-to-service認証で長期固定Secretを増やさず、GitHub Actions OIDCで限定された自動ingest / exportを構成できた。
- **一般化:** 外部実行基盤が短命identityを発行できる場合は、長期API keyの追加よりOIDC等の短命credentialを優先する。issuer / audience / repository / refなどを検証し、許可主体を最小化する。

- **経験:** OGPはmeta tagを実装しただけでは不十分で、X側のcacheにより古い画像が残った。
- **一般化:** OGP / Twitter Card等はproduction URLを実SNSまたはcrawler相当で確認する。画像URLは明示的な寸法・MIMEを持たせ、差し替え時はversioned asset URL等でcache更新を制御できるようにする。

- **経験:** 共有URL自体が再入場手段になる匿名サービスでは、通常の「ホームへ戻る」がアクセス喪失につながり得た。
- **一般化:** magic link / share URL / recovery codeなど「失うと戻れない情報」がアクセス権を担う作品では、離脱・logout・端末移行前に保存確認とcopy導線を用意する。内部IDや管理Secretは通常画面へ露出させない。

- **経験:** Worker appのpreview health確認で、static asset fallbackが `/health` を飲み込み、正常なWorkerを異常扱いした。
- **一般化:** deploy後health checkはadapter / app typeに合わせる。存在しない固定route一つだけを成功条件にせず、Worker存在・HTTP到達・期待HTML / API応答など、その配信方式で成立する観測点を使う。

- **経験:** 自動投稿・自動収集は「動いた瞬間」だけでなく、停止指示まで継続運転できることが価値だった。
- **一般化:** 長期自動運転機能は、手動操作なしで次周期へ進むこと、設定を勝手に再計算しないこと、停止方法が明確であること、外部依存が切れた時に復旧地点を特定できることまでを完成条件に含める。


## API route / 認証境界から得た共通知見（2026-09-13）

- **経験:** 自動化側が新しいProduction API routeを呼び始めた時点で、Productionがまだ旧版だったため404になった。コードとworkflowが正しくても、callerとdeployed runtimeのversion差で失敗する。
- **一般化:** 新しいAPI routeを自動処理から呼ぶ変更では、「route実装をmainへ入れる」ことと「Productionにそのrouteが存在する」ことを別条件として扱う。自動callerは404を恒久障害と決めつけず、pending/backlogを保持して安全に再試行できるようにし、Production更新後に回復できることを検証する。
- **検品:** 自動workflowを有効化する前後に、対象Production URLで新routeの存在確認を行い、旧版Productionを叩いた場合にもデータを失わず再実行できることを確認する。

- **経験:** 認証付き画面へのroot redirectやhealth check契約の不一致で、正常なappでも401/403を返し、deployや監視が失敗扱いになることがあった。
- **一般化:** 401/403は「Secretが間違っている」と即断せず、①health checkが未認証で到達すべきrouteか、②redirect後にprotected routeへ入っていないか、③service-to-service credentialのissuer/audience/repository/refが一致するか、④人間向け認証と機械向けhealth/API認証を混同していないかを順番に切り分ける。公開入口・health endpoint・protected admin/APIの認証契約を明文化する。
- **検品:** Production E2Eでは200だけでなく、期待routeの404、未認証healthの401/403、redirect chainを明示的に検査し、誤ったroute/auth契約を公開前に検出する。

- **経験:** push triggerだけのinbox処理では、一時的な404/401/5xxやscheduler欠落でpendingが残ったままになる可能性がある。
- **一般化:** 長期自動運転pipelineは push/event trigger に加えて低頻度のreconciliation pollを持ち、pending滞留、直近失敗後の後続成功有無、最終成功時刻を監視する。失敗履歴そのものではなく「回復していない失敗」を異常と判定する。


## HaloPaletteから得た共通知見（2026-09-18）

- **経験:** 同一アプリにカテゴリ・ブランド・媒体違いの派生画面を追加した際、見た目を似せても管理カード、複数選択、確定単位、遷移先などの操作が分岐し、ユーザーがカテゴリごとに学び直す状態になった。
- **一般化:** データ集合だけが違う派生画面は、追加前に共通Interaction Contractを定義する。タップ後の遷移、複数選択可否、確定単位、管理カード、検索、破壊操作までAcceptanceで固定し、カテゴリ固有差はデータ・属性表示へ閉じ込める。

- **経験:** 既存画面へ後付けscriptで挙動を重ねた結果、古いlistenerが先に発火したり、capture/bubble順序の競合で「コードはあるのに実機挙動が変わらない」状態が起きた。
- **一般化:** 段階的UI migrationやenhancementではDOM差分だけでなくイベント伝播順まで検品する。旧挙動を無効化する必要があるなら、元handlerを削除するか、所有するpointer/eventだけを明示して処理し、二重実装を長期化させない。

- **経験:** 2D用のpointer capture補正がpointerup/cancelを無条件に消費し、同じDOMを使う3D dragの終了処理まで止めた。
- **一般化:** 同一DOMで複数gesture/modeを共存させる場合、listenerは「自分が開始・追跡したpointer」だけをconsumeする。pointerdown→move→up/cancelの全経路で、他モードのlifecycleを遮断しないことを実機Acceptanceに含める。

- **経験:** pointermoveで回転stateを更新していてもdrag中にrenderしていないと、指には追従せず、release後に蓄積したstateだけが遅れて動いて見えた。
- **一般化:** drag/pan/rotateの完成条件はstate更新ではなく「gesture中の各frameに描画が反映されること」。慣性はrelease後の補助であり、live responseの代替にしない。

- **経験:** canvas上の1,000〜2,000超の小さな半透明背景点を毎frame配列化・depth sort・個別animationすると、モバイルで体感ラグが出た。
- **一般化:** 視覚寄与が小さい装飾点群は毎frame sortを避けて直接描画し、depth順が意味を持つ主オブジェクトだけsortする。静的な装飾よりgestureのフレームレートを優先する。

- **経験:** モバイル管理カードで「削除」と「閉じる」が近接すると誤操作リスクが高く、横幅を持て余した縦積みUIも操作効率が悪かった。
- **一般化:** destructive actionとdismiss actionは空間的・視覚的に分離する。dismissはheaderの×、deleteはカード下端など役割ごとに固定し、状態操作は2列等で横幅を活用する。モバイルでは見た目の整列より誤タップ回避を優先する。


## CARILA WORKS Control release基盤から得た共通知見（2026-10-02）

- **経験:** Cloudflare Static Assets配下で拡張子なしの予約health pathを使うと、HTML handling / asset routingの影響を受け、Worker側でrouteを実装していても404になる余地が残った。
- **一般化:** Control-ownedの機械判定用artifactは、アプリ固有routeと衝突しない予約名かつ拡張子付きの固定asset（例: `/__carila-control-health.json`）として持たせる。Worker-first / asset-firstのどちらでも同一candidate identityを返せる構造にし、app固有`/health`を共通deploy契約へ昇格させない。

- **経験:** static作品だけ「HTTP 2xxなら正常」、Worker Appだけcandidate identity検証、のようにadapterごとに成功条件が分岐すると、片側だけ誤判定や古いartifact判定が残った。
- **一般化:** automatic adapterのProduction切替では、Domain record → exact Worker → Control-owned candidate/adapter identity → browser-facing rootの順に共通検証する。配信方式固有差はadapter名だけに閉じ、成功判定の骨格を共通化する。

- **経験:** runtimeのadapter revisionとGitHub Actions側のexpected revisionを別々にhard-codeした結果、片方だけ更新される契約driftが起こり得た。
- **一般化:** version / schema / adapter revisionは単一Source of Truthから派生させる。CI/CDの検証値を手で二重管理せず、runtime sourceまたは生成artifactから読み取る。

- **経験:** Queue jobを`RUNNING`へした後にconsumerが中断すると、永続的に更新中扱いになる。またduplicate deliveryをそのままackすると、実処理が終わっていないjobを失う。
- **一般化:** background jobにはlease / stale timeout / idempotent acquisitionを持たせる。処理中leaseを取得できないduplicateはackせず遅延retryし、stale leaseだけを再取得可能にする。UI側もstale jobを永久ロックとして扱わない。

- **経験:** ブラウザ側とQueue consumer側の両方で同じRepository capability再判定をすると、GitHub API callと判定経路が重複し、結果不一致の余地が増えた。
- **一般化:** correctnessに関わる再判定はserver / queue側のauthoritative pathへ集約し、browserは操作要求と状態表示に限定する。同期fallbackも同じserver helperを通す。

- **経験:** 一時的なGET失敗1回でpollを終了したり、loading overlayをfinally以外で閉じる実装では、処理自体が継続していてもUIが「止まった」ように見えた。
- **一般化:** read-only通信だけをbounded retryし、mutationは自動retryしない。長時間jobのpollはbackoffしながら継続し、loading / overlay cleanupは必ずfinallyで行う。

- **経験:** optional webhook / secret-vault capabilityのSecret未設定までProduction deploy全体の必須条件にすると、利用していない補助機能のために正常なControl更新まで停止した。
- **一般化:** required dependencyとoptional capabilityをCIでも分離する。未設定optional機能はnoticeに留め、共通正常系を止めない。

- **経験:** 毎回Custom Domain PUTを実行すると、既に正しい設定でも外部control-plane mutationを増やし、障害面とAPI消費を広げる。
- **一般化:** external control-plane変更はread-before-writeで現状態を確認し、desired stateと一致する場合はno-opにする。無意味なmutationを「念のため」で繰り返さない。


### バグが連続する時の回帰調査ルール

- **経験:** 同じユーザー操作が何度も失敗しているのに、hash / MIME / fallback / retryなど観測できた症状ごとに小さな修正を重ねると、共通のRelease契約そのものの不整合を見逃しやすい。個々のCIやcanaryがgreenでも、実作品E2Eが通らなければ原因は未解決である。
- **一般化:** 同一導線で2回以上「修正後も再発」した時点でforward patchを一旦止め、last-known-good、変更期間、外部状態、adapter契約、実作品E2Eを横断したregression auditへ切り替える。症状の近くではなく、入力→変換→外部API→配信→実ブラウザまでの全経路を1枚の契約として見直す。

- **経験:** テストで`ASSETS.fetch()`等の外部依存を「正常に動くmock」として注入していたため、実際にはその依存自体が壊れていてもunit/integration testがgreenになった。さらに実装変更後にsource-shape assertionが古いまま残り、Production runをtest段階で止めた。
- **一般化:** 外部依存をmockするテストだけで「実配信可能」を証明しない。少なくとも1本は「その依存が無い／壊れている」条件か、生成artifact単体でユーザー-facing rootが成立する条件をAcceptanceへ入れる。実装契約を変えた時は、同じ契約を参照する全testをMerge前に検索・更新し、source文字列一致ではなく実際の生成artifact / metadata / responseを検証する。

- **経験:** Actions停止中に複数のRelease変更がmainへ蓄積し、後からまとめてProductionへ到達したため、どの変更が回帰原因かを特定しにくくなった。
- **一般化:** Productionへ出せない期間に基盤変更を積む場合、各変更の「未Production検証」を明示し、最後に成功したProduction revisionを記録する。復旧時は蓄積変更を一括で正常と仮定せず、last-known-goodからの差分と実作品E2Eを段階的に検証する。

- **経験:** 一時rollbackは原因切り分けに有効だったが、必要なQueue / release_jobs / Push等まで恒久廃止したように見える危険があった。
- **一般化:** rollbackは「安定coreの復元」と「機能廃止」を分けて記録する。復元後に実E2Eを通し、その上で隔離した機能を1系統ずつ戻し、各段階でAcceptanceを通す。rollbackしただけで元の設計目的を破棄しない。


### CI_LAST_GATE — CI/Production Actionは調査の最後に1回だけ

- **経験:** 目の前の失敗だけを直してruntimeをMergeし、その都度CI/Production Actionを走らせると、次の既知リスクを同じ事前調査で潰せたにもかかわらずActionsを消費し、ユーザーの再試行回数も増えた。
- **一般化:** 同一導線の障害修正では、CIを「調査ツール」にしない。まずGitHub connectorとローカル/静的検証で、入力→capability判定→source取得→build/adapter→binding/secret→upload→routing→edge propagation→health/root→manifest/D1反映→cleanup/rollback→UIまでのfailure surfaceを列挙し、既知の穴を1 branchへまとめて潰す。
- **強制ゲート:** runtime変更をmainへMergeしてCI/Production Actionを発火してよいのは、(1) 対象導線と正常実績作品との差分比較、(2) 変更契約を参照する全test/fixture/constant検索、(3) optional/required依存、外部control-planeのeventual consistency、timeout/retry、rollback、stale state、manifest/D1 driftの確認、(4) 追加で静的に潰せる既知リスクが残っていないこと、を確認した後だけとする。
- **運用:** CI失敗で新しい既知問題が出ても即座に次のMergeをしない。ログを起点にもう一度全参照を監査し、次の一回へまとめる。docs/test-onlyでruntime deploy不要な変更はProduction Actionを発火させない。
- **完了条件:** 「CIがgreen」ではなく実作品E2Eまで通ること。ただし実作品E2E前にも、コードから予測できるfailure classは可能な限り先回りして潰し、ユーザーに何度も同じボタンを押させない。


### 実作品404回帰の解決パターン — Control greenではなく実E2Eを正本にする

- **事例:** CARILA WORKS ControlはProduction deploy / health / canaryがgreenでも、C-LINKsの実Previewはcandidate rootで404を返し続けた。最終的に、最後にProduction実績のある同期Release coreへ戻して構造を安定させ、small Worker Appのstatic assetsをgenerated adapterへinlineしてnative ASSETS依存を外し、さらにworkers.devのcontrol-plane enabledと実edge到達を別状態として扱ってexact candidate adapter到達後にroot HTMLを確認することで、実PreviewのURL作成・QR生成・履歴・短縮redirectまで成功した。
- **学び:** CI/canary/Control healthのgreenは「Control自身がdeployできた」証拠であって、作品E2Eの代替ではない。Release基盤の完了条件は、代表実作品で user click → source取得 → adapter生成 → binding/secret → upload → workers.dev/custom domain → browser-facing root/API → manifest/D1反映まで通ること。
- **学び:** 外部platformのcontrol-plane成功をdata-plane成功と同一視しない。Cloudflareではroute enabled直後にedgeが未反映な時間があり得るため、exact candidate identityを持つreserved probeでdata-plane readinessを確認してからrootを判定する。
- **学び:** mockされたASSETS等がgreenでも、実platform bindingが壊れれば作品は失敗する。小規模artifactでは外部bindingを必須依存から外せるなら外し、生成artifact単体でrootを返せるAcceptanceを持つ。
- **学び:** 一時rollbackで安定coreを取り戻した場合、隔離したQueue / release_jobs / Push等の目的まで破棄しない。実作品E2Eがgreenになった地点を新しいbaselineとし、その上へ機能を1系統ずつ再導入する。
- **学び:** 同じ障害でユーザーに再試行を繰り返させない。既知failure surfaceを静的に洗い切り、CI_LAST_GATEを通した後だけProduction Actionを1回起動し、その後の1クリックをAcceptanceに使う。
