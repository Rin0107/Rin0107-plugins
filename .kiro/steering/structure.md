# Project Structure

## Organization Philosophy

プラグイン隔離パターン。各プラグインは独立したディレクトリに自己完結し、マーケットプレイスカタログが一元的にインデックスする。開発プロセスは `.kiro/` 配下の仕様駆動フレームワークで管理。

## Directory Patterns

### マーケットプレイスルート
**Location**: `/`
**Purpose**: プロジェクト全体のマニフェストとドキュメント
**Example**: `.claude-plugin/marketplace.json`, `README.md`, `CLAUDE.md`

### プラグインパッケージ
**Location**: `/plugins/<plugin-name>/`
**Purpose**: 個別プラグインの自己完結パッケージ
**Example**: `/plugins/pr/` には `.claude-plugin/plugin.json` と `skills/` を含む

### スキル実装
**Location**: `/plugins/<plugin-name>/skills/<skill-name>/`
**Purpose**: 個別スキルの定義ファイル
**Example**: `/plugins/pr/skills/pr-create/SKILL.md`

### 仕様管理
**Location**: `/.kiro/specs/<feature-name>/`
**Purpose**: 機能単位の仕様ドキュメント（requirements, design, tasks）
**Example**: `/.kiro/specs/pr-plugin/` に `spec.json`, `requirements.md`, `design.md`, `tasks.md`

### プロジェクトステアリング
**Location**: `/.kiro/steering/`
**Purpose**: プロジェクト全体のパターンと方針（AI のプロジェクトメモリ）

## Naming Conventions

- **プラグイン名**: kebab-case（例: `pr`, `my-plugin`）
- **スキル名**: kebab-case、プラグイン名をプレフィックス（例: `pr-create`）
- **マニフェスト**: `plugin.json`（プラグイン）、`marketplace.json`（カタログ）
- **スキルファイル**: `SKILL.md`（大文字）
- **仕様ファイル**: `spec.json`, `requirements.md`, `design.md`, `tasks.md`

## Plugin Structure Pattern

新しいプラグインは以下のパターンに従う:

```
plugins/<plugin-name>/
├── .claude-plugin/
│   └── plugin.json          # プラグインマニフェスト
├── skills/
│   └── <skill-name>/
│       └── SKILL.md         # スキル定義
└── agents/                  # （必要な場合のみ）
    └── <agent-name>/
        └── AGENT.md
```

## Code Organization Principles

1. **プラグイン隔離**: 各プラグインは他のプラグインに依存しない
2. **スキル中心**: 機能はスキルとして提供し、ユーザーが直接呼び出せる形式を優先
3. **マニフェスト必須**: プラグインには必ず `plugin.json` を配置
4. **仕様先行**: 新機能追加時は `.kiro/specs/` で仕様を定義してから実装

---
_Document patterns, not file trees. New files following patterns shouldn't require updates_
