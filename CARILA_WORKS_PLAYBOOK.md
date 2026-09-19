# CARILA WORKS Playbook

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
