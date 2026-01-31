# Technology Stack

## Architecture

プラグインベースの配布アーキテクチャ。各プラグインは独立したパッケージとして自己完結し、Claude Code のマーケットプレイスシステムを通じてインストール可能。ランタイム依存なし。

## Core Technologies

- **形式**: JSON（マニフェスト・設定） + Markdown（スキル定義・ドキュメント）
- **プラットフォーム**: Claude Code プラグインシステム
- **配布**: Claude Code マーケットプレイス（`/plugin marketplace add`）
- **バージョン管理**: Git / GitHub

## Key Libraries

ランタイム依存ライブラリなし。純粋な配布パッケージとして機能する。

## Development Standards

### マニフェスト形式

プラグインマニフェスト（`plugin.json`）:
```json
{
  "name": "plugin-name",
  "description": "What it does",
  "version": "X.Y.Z"
}
```

マーケットプレイスカタログ（`marketplace.json`）:
```json
{
  "name": "marketplace-name",
  "owner": { "name": "owner-name" },
  "metadata": { "pluginRoot": "./plugins" },
  "plugins": [{ "name": "plugin-name" }]
}
```

### スキル定義形式

スキルは YAML front-matter 付き Markdown（`SKILL.md`）で定義:
```markdown
---
description: One-line description
user-invocable: true
---

[Full description]

## Steps
[Numbered implementation steps]

## Guidelines
[Key rules and constraints]
```

### Code Quality

- 仕様駆動開発（Kiro フレームワーク）による 3 段階承認ゲート
- 日本語でのドキュメント・仕様記述（EARS キーワードは英語維持）

## Development Environment

### Required Tools

- Git
- GitHub CLI (`gh`)
- Claude Code CLI

### Common Commands

```bash
# プラグインインストール（ユーザー向け）:
/plugin marketplace add Rin0107/claude-plugins
/plugin install pr@Rin0107-plugins

# 開発フロー（開発者向け）:
/kiro:spec-init "feature description"
/kiro:spec-requirements {feature}
/kiro:spec-design {feature}
/kiro:spec-tasks {feature}
/kiro:spec-impl {feature}
```

## Key Technical Decisions

1. **マニフェストファースト**: コード前にメタデータ定義を行い、プラグインの意図を明確化
2. **Markdown ベーススキル**: スキルロジックを Markdown で記述し、Claude Code が解釈実行
3. **ゼロ依存**: npm パッケージ等の外部依存なし、配布の簡素化を優先
4. **Spec-Driven ガバナンス**: 全機能開発に仕様フェーズを必須化

---
_Document standards and patterns, not every dependency_
