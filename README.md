[🇯🇵 日本語](#japanese) | [🇺🇸 English](#english)

---

# Japanese

# Claude Code セットアップテンプレート

新しいリポジトリでClaude Codeを使い始める際のセットアップテンプレートです。カスタムコマンド、MCPサーバー設定を含み、プロジェクトに合わせてカスタマイズして使用できます。

## 目的

- 新しいプロジェクトで素早くClaude Code環境をセットアップする
- 汎用的なカスタムコマンドをテンプレートとして提供する
- チーム内でコマンドを共有・標準化する
- MCPサーバーの設定例を提供する

## クイックスタート

このリポジトリの中身をプロジェクトのルートにコピーします。

```bash
cp -r /path/to/shu-claude-template/* /path/to/your-project/
```

## 使い方

コマンドは `/コマンド名` で呼び出せます。

```
/init-project   # プロジェクト初期化
/review-diff    # コードレビュー
/sync-docs      # ドキュメント同期
/lint-ja        # 日本語ライティング
```

## 含まれるファイル

### テンプレートファイル

| ファイル                         | 説明                                   |
| -------------------------------- | -------------------------------------- |
| `CLAUDE.md.template`             | プロジェクト説明ファイルのテンプレート |
| `.claude/settings.json.template` | MCPサーバー設定のテンプレート          |

### コマンド

| コマンド       | 説明                                                                   |
| -------------- | ---------------------------------------------------------------------- |
| `init-project` | プロジェクト初期化（CLAUDE.md作成、構造把握）                          |
| `review-diff`  | mainブランチとの差分をレビューし、問題点を指摘・修正する               |
| `sync-docs`    | ドキュメント（CLAUDE.md, README.md等）と実装の差異をチェック・修正する |
| `lint-ja`      | 日本語ドキュメントを自然な文体で作成・編集するためのガイドライン       |

### ドキュメント管理

| ディレクトリ | 説明                   |
| ------------ | ---------------------- |
| `docs/todo/` | 未完了タスク・計画     |
| `docs/done/` | 完了タスク・アーカイブ |

## ディレクトリ構造

```
.
├── README.md
├── CLAUDE.md.template          # CLAUDE.mdのテンプレート
├── docs/                       # ドキュメント管理
│   ├── todo/                   # 未完了タスク
│   │   └── .gitkeep
│   └── done/                   # 完了タスク
│       └── .gitkeep
└── .claude/
    ├── settings.json.template  # MCPサーバー設定テンプレート
    └── commands/               # カスタムコマンド
        ├── init-project.md     # プロジェクト初期化
        ├── lint-ja.md          # 日本語ライティングガイド
        ├── review-diff.md      # コードレビュー
        └── sync-docs.md        # ドキュメント同期
```

## MCPサーバー設定について

`.claude/settings.json.template` には以下のMCPサーバー設定例が含まれています。

| サーバー   | 説明                                                       |
| ---------- | ---------------------------------------------------------- |
| Serena     | コードベースのシンボル解析（関数・クラスの定義を高速検索） |
| Context7   | ライブラリの最新ドキュメント参照                           |
| Playwright | ブラウザ操作・E2Eテスト                                    |

## カスタマイズ例

### review-diff のカスタマイズ

プロジェクト固有のレビュー観点を追加する場合、`.claude/commands/review-diff.md`を編集します。

```markdown
# 追加するレビュー観点の例

- DDD設計への準拠
- React/Vue等フレームワーク固有のベストプラクティス
- プロジェクト固有のコーディング規約
```

### sync-docs のカスタマイズ

プロジェクト固有のドキュメント構成に合わせてチェック対象を編集します。

## 新しいコマンドの追加

1. `.claude/commands/`ディレクトリに新しい`.md`ファイルを作成
2. ファイル名がコマンド名になる（例: `my-command.md` → `/my-command`）
3. Markdown形式でプロンプトを記述

## 注意事項

- コマンドは汎用的に作られているため、プロジェクトに合わせて適宜修正してください
- 特定の技術スタック（React, Python等）を使用する場合は、該当するベストプラクティスをコマンドに追加することを推奨します

## 参考

- lint-ja コマンドは [textlint-rule-preset-ai-writing](https://github.com/textlint-ja/textlint-rule-preset-ai-writing) を参考に作成されています

## ライセンス

MIT License

---

# English

# Claude Code Setup Template

A setup template for starting Claude Code in new repositories. Includes custom commands and MCP server configurations that can be customized for your project.

## Purpose

- Quickly set up Claude Code environment in new projects
- Provide generic custom commands as templates
- Share and standardize commands within teams
- Provide MCP server configuration examples

## Quick Start

Copy the contents of this repository to your project root.

```bash
cp -r /path/to/shu-claude-template/* /path/to/your-project/
```

## Usage

Commands can be invoked with `/command-name`.

```
/init-project   # Project initialization
/review-diff    # Code review
/sync-docs      # Document synchronization
/lint-ja        # Japanese writing guidelines
```

## Included Files

### Template Files

| File                             | Description                       |
| -------------------------------- | --------------------------------- |
| `CLAUDE.md.template`             | Project description file template |
| `.claude/settings.json.template` | MCP server configuration template |

### Commands

| Command        | Description                                                              |
| -------------- | ------------------------------------------------------------------------ |
| `init-project` | Project initialization (creates CLAUDE.md, analyzes structure)           |
| `review-diff`  | Reviews diff from main branch, identifies and fixes issues               |
| `sync-docs`    | Checks and fixes discrepancies between docs (CLAUDE.md, README.md, etc.) |
| `lint-ja`      | Guidelines for writing natural Japanese documents                        |

### Document Management

| Directory    | Description              |
| ------------ | ------------------------ |
| `docs/todo/` | Incomplete tasks/plans   |
| `docs/done/` | Completed tasks/archives |

## Directory Structure

```
.
├── README.md
├── CLAUDE.md.template          # CLAUDE.md template
├── docs/                       # Document management
│   ├── todo/                   # Incomplete tasks
│   │   └── .gitkeep
│   └── done/                   # Completed tasks
│       └── .gitkeep
└── .claude/
    ├── settings.json.template  # MCP server config template
    └── commands/               # Custom commands
        ├── init-project.md     # Project initialization
        ├── lint-ja.md          # Japanese writing guide
        ├── review-diff.md      # Code review
        └── sync-docs.md        # Document sync
```

## MCP Server Configuration

`.claude/settings.json.template` includes the following MCP server configuration examples.

| Server     | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| Serena     | Codebase symbol analysis (fast search for functions/classes) |
| Context7   | Latest library documentation reference                       |
| Playwright | Browser automation / E2E testing                             |

## Customization Examples

### Customizing review-diff

To add project-specific review criteria, edit `.claude/commands/review-diff.md`.

```markdown
# Example additional review criteria

- DDD design compliance
- Framework-specific best practices (React/Vue, etc.)
- Project-specific coding standards
```

### Customizing sync-docs

Edit the check targets according to your project's document structure.

## Adding New Commands

1. Create a new `.md` file in the `.claude/commands/` directory
2. The filename becomes the command name (e.g., `my-command.md` → `/my-command`)
3. Write the prompt in Markdown format

## Notes

- Commands are designed to be generic; customize them as needed for your project
- When using specific tech stacks (React, Python, etc.), consider adding relevant best practices to the commands

## References

- The lint-ja command is based on [textlint-rule-preset-ai-writing](https://github.com/textlint-ja/textlint-rule-preset-ai-writing)

## License

MIT License
