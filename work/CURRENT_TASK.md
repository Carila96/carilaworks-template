# Current Task

## 目的
GitHub connectorを使えるのに「connectorがない」「Mergeできない」と誤判定する再発を、CARILA WORKS Harness上で可能な限り強く防止する。

## 今回やること
- Execution RulesへTool Discovery Gateを追加。
- GitHub可否の否定回答前に、現ターンでconnector action存在確認を必須化。
- Repository Reality Gate / Write Capability Gate / Negative Claim Evidence Gateへ分離。
- AGENTS.mdへ最初の実行行動としてTool Discovery Gateを追加。
- 返答前の機械的self-checkを追加。
- text instructionだけでは100% hard enforcementできず、完全強制にはruntime/system/tool wrapperが必要であることを明記。
- PR #13 Merge済み。merge commit: `82af9bcb81917c2c1f317b508c25b4f06d84bda3`。

## 完了条件
- mainへMerge済み。
- AGENTSとExecution Rulesの両方にTool Discovery Gateが存在。
- 「connectorがない」「Mergeできない」の否定回答にlive connector evidenceが必須化。
- latest main / PR mergedをconnectorで再確認済み。

## 今回やらないこと
- 既存作品へのTemplate変更一括反映。
- ChatGPT製品runtimeのsystem-level変更（Repositoryからは変更不能）。
