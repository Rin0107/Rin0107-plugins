# rinpe-plugins

Claude Code 用のプラグインマーケットプレイスです。

## インストール

### 1. マーケットプレイスの追加

```shell
/plugin marketplace add Rin0107/claude-plugins
```

### 2. プラグインのインストール

```shell
/plugin install pr@Rin0107-plugins
```

## ディレクトリ構造

```shell
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json    # マーケットプレイスカタログ
├── plugins/
│   └── a-plugin/
│       ├── .claude-plugin/
│       │   └── plugin.json # プラグインマニフェスト
│       ├── skills/
|       ├── agents/
|       └── ...
└── README.md
```

## プラグインの追加

新しいプラグインを追加するには:

1. `plugins/<plugin-name>/` ディレクトリを作成
2. `.claude-plugin/plugin.json` マニフェストを追加
3. スキル、フック、MCP サーバーなどを追加
4. `.claude-plugin/marketplace.json` の `plugins` 配列にエントリを追加

see: [プラグインマーケットプレイスの作成と配布](https://code.claude.com/docs/ja/plugin-marketplaces)
