---
description: Run spec-init through spec-tasks automatically using sub-agents for each phase
user-invocable: true
---

cc-sdd（Claude Code Spec-Driven Development）の仕様策定フェーズ（spec-init → spec-requirements → spec-design → spec-tasks）を、各工程専門の sub-agent を使って自動実行するオーケストレーションスキル。

ユーザーはプロジェクト説明を引数として渡すだけで、仕様ドキュメント一式（requirements.md, design.md, tasks.md）が段階的に自動生成される。

各 sub-agent の詳細な手順は `agents/` 配下の AGENT.md に定義されている:
- `agents/spec-init/AGENT.md` — 仕様初期化
- `agents/spec-requirements/AGENT.md` — 要件生成
- `agents/spec-design/AGENT.md` — 設計生成
- `agents/spec-tasks/AGENT.md` — タスク生成

## Steps

### 1. Phase 1: spec-init（仕様初期化）

Task ツールで `subagent_type: "general-purpose"` の sub-agent を起動する。

プロンプト:
- `agents/spec-init/AGENT.md` の内容を読み取り、そのままプロンプトとして使用する
- プロンプトの冒頭に以下を追加する:
  ```
  プロジェクト説明: {ユーザーが渡したプロジェクト説明}
  ```

sub-agent の結果から **feature name** を取得する。

### 2. Phase 1 完了検証

`.kiro/specs/{feature-name}/spec.json` を Read ツールで読み取り、以下を検証する:

- ファイルが存在すること
- `phase` が `"initialized"` であること

検証に成功したら進捗を通知する: `[1/4] spec-init 完了`

**検証に失敗した場合**: エラーを報告し、後続フェーズを停止する。

### 3. Phase 2: spec-requirements（要件生成）

Task ツールで `subagent_type: "general-purpose"` の sub-agent を起動する。

プロンプト:
- `agents/spec-requirements/AGENT.md` の内容を読み取り、そのままプロンプトとして使用する
- プロンプトの冒頭に以下を追加する:
  ```
  feature name: {Phase 1 で取得した feature name}
  ```

### 4. Phase 2 完了検証

`.kiro/specs/{feature-name}/spec.json` を Read ツールで読み取り、以下を検証する:

- `phase` が `"requirements-generated"` であること
- `approvals.requirements.generated` が `true` であること

検証に成功したら進捗を通知する: `[2/4] spec-requirements 完了`

**検証に失敗した場合**: エラーを報告し、後続フェーズを停止する。

### 5. Phase 3: spec-design（設計生成）

Task ツールで `subagent_type: "general-purpose"` の sub-agent を起動する。

プロンプト:
- `agents/spec-design/AGENT.md` の内容を読み取り、そのままプロンプトとして使用する
- プロンプトの冒頭に以下を追加する:
  ```
  feature name: {feature name}
  ```

### 6. Phase 3 完了検証

`.kiro/specs/{feature-name}/spec.json` を Read ツールで読み取り、以下を検証する:

- `phase` が `"design-generated"` であること
- `approvals.design.generated` が `true` であること

検証に成功したら進捗を通知する: `[3/4] spec-design 完了`

**検証に失敗した場合**: エラーを報告し、後続フェーズを停止する。

### 7. Phase 4: spec-tasks（タスク生成）

Task ツールで `subagent_type: "general-purpose"` の sub-agent を起動する。

プロンプト:
- `agents/spec-tasks/AGENT.md` の内容を読み取り、そのままプロンプトとして使用する
- プロンプトの冒頭に以下を追加する:
  ```
  feature name: {feature name}
  ```

### 8. Phase 4 完了検証

`.kiro/specs/{feature-name}/spec.json` を Read ツールで読み取り、以下を検証する:

- `phase` が `"tasks-generated"` であること
- `approvals.tasks.generated` が `true` であること

検証に成功したら進捗を通知する: `[4/4] spec-tasks 完了`

**検証に失敗した場合**: エラーを報告し、後続フェーズを停止する。

### 9. 完了サマリー

全 4 フェーズが正常に完了した場合、以下のサマリーをユーザーに提示する:

```
✅ spec-omakase 完了

## Feature: {feature name}

## 生成ファイル:
- `.kiro/specs/{feature-name}/spec.json` — メタデータ
- `.kiro/specs/{feature-name}/requirements.md` — 要件ドキュメント
- `.kiro/specs/{feature-name}/design.md` — 設計ドキュメント
- `.kiro/specs/{feature-name}/research.md` — リサーチログ
- `.kiro/specs/{feature-name}/tasks.md` — 実装タスク

## Next Step:
/kiro:spec-impl {feature-name}
```

## Guidelines

- 各フェーズは必ず spec-init → spec-requirements → spec-design → spec-tasks の順序で実行する
- 各フェーズの完了検証で失敗した場合は、即座に後続フェーズを停止し、エラーが発生したフェーズ名とエラー内容をユーザーに報告する
- sub-agent は `subagent_type: "general-purpose"` で起動する
- 各 sub-agent のプロンプトは `agents/` 配下の対応する AGENT.md を Read ツールで読み取って使用する
- AGENT.md 内の `{feature-name}` は Phase 1 で取得した実際の feature name に置換する
- `{ユーザーが渡したプロジェクト説明}` は、このスキルに渡された引数に置換する
- テンプレートやステアリングファイルが不在の場合、各 sub-agent がファイルパスを含む警告を出力するが、可能な限り処理を継続する
