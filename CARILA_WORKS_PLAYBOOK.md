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
