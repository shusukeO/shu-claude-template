[日本語](#japanese) | [English](#english)

---

# Japanese

# Claude Code セットアップテンプレート

新しいリポジトリでClaude Codeを使い始める際のセットアップテンプレートです。カスタムスキル、MCPサーバー設定を含み、プロジェクトに合わせてカスタマイズして使用できます。

## 目的

- 新しいプロジェクトで素早くClaude Code環境をセットアップする
- 汎用的なカスタムスキルをテンプレートとして提供する
- チーム内でスキルを共有、標準化する
- MCPサーバーの設定例を提供する

## クイックスタート

このリポジトリの中身をプロジェクトのルートにコピーします。

```bash
cp -r /path/to/shu-claude-template/* /path/to/your-project/
```

## 使い方

スキルは `/スキル名` で呼び出せます。

```
/init-project   # プロジェクト初期化
/review-diff    # コードレビュー
/sync-docs      # ドキュメント同期
/lint-ja        # 日本語ライティング
/msg            # ステージ済み変更からコミットメッセージを用意
/msg-all        # 全変更をステージしてコミットメッセージを用意
```

`lint-ja` は日本語のドキュメントを書く場面で Claude が自動的に読み込みます。それ以外のスキルは副作用があるため、ユーザーが `/` で呼び出したときだけ動きます。

## 含まれるファイル

### テンプレートファイル

| ファイル                         | 説明                                   |
| -------------------------------- | -------------------------------------- |
| `CLAUDE.md.template`             | プロジェクト説明ファイルのテンプレート |
| `.claude/settings.json.template` | MCPサーバー設定のテンプレート          |

### スキル

| スキル         | 説明                                                                   |
| -------------- | ---------------------------------------------------------------------- |
| `init-project` | プロジェクト初期化（CLAUDE.md作成、構造把握）                          |
| `review-diff`  | mainブランチとの差分を独立したサブエージェントでレビューし、問題点を指摘する |
| `sync-docs`    | ドキュメント（CLAUDE.md, README.md等）と実装の差異をチェック、修正する |
| `lint-ja`      | 日本語ドキュメントを自然な文体で作成、編集するためのガイドライン       |
| `msg`          | ステージ済みの変更からコミットメッセージを生成し、クリップボードにコピーする |
| `msg-all`      | 全変更をステージしてコミットメッセージを生成し、クリップボードにコピーする   |

### ドキュメント管理

| ディレクトリ | 説明                   |
| ------------ | ---------------------- |
| `docs/todo/` | 未完了のタスクと計画     |
| `docs/done/` | 完了したタスクのアーカイブ |

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
    └── skills/                 # カスタムスキル（1スキル1ディレクトリ）
        ├── init-project/SKILL.md   # プロジェクト初期化
        ├── lint-ja/SKILL.md        # 日本語ライティングガイド
        ├── msg/SKILL.md            # コミットメッセージ生成（ステージ済み）
        ├── msg-all/SKILL.md        # コミットメッセージ生成（全変更）
        ├── review-diff/SKILL.md    # コードレビュー
        └── sync-docs/SKILL.md      # ドキュメント同期
```

## MCPサーバー設定について

`.claude/settings.json.template` には以下のMCPサーバー設定例が含まれています。

| サーバー   | 説明                                                       |
| ---------- | ---------------------------------------------------------- |
| Serena     | コードベースのシンボル解析（関数やクラスの定義を高速に検索） |
| Context7   | ライブラリの最新ドキュメント参照                           |
| Playwright | ブラウザ操作と E2E テスト                                  |

## カスタマイズ例

### review-diff のカスタマイズ

プロジェクト固有のレビュー観点を追加する場合、`.claude/skills/review-diff/SKILL.md` を編集します。

```markdown
# 追加するレビュー観点の例

- DDD設計への準拠
- React/Vue等フレームワーク固有のベストプラクティス
- プロジェクト固有のコーディング規約
```

### sync-docs のカスタマイズ

プロジェクト固有のドキュメント構成に合わせてチェック対象を編集します。

## 新しいスキルの追加

1. `.claude/skills/<スキル名>/SKILL.md` を作成する。ディレクトリ名がスキル名になる（例: `my-skill/SKILL.md` は `/my-skill`）
2. 先頭に YAML フロントマターを書く
3. 本文に Markdown 形式でプロンプトを記述する

フロントマターの例は以下の通りです。

```markdown
---
name: my-skill
description: 何をするか、いつ使うか。Claude はこの文を見て自動的に読み込むかを判断する
argument-hint: [引数のヒント]
allowed-tools: Read, Grep, Bash(git diff:*)
---
```

主なフィールドの意味は以下の通りです。

- `description` は常にコンテキストに載る唯一の部分なので、用途と使う場面のキーワードを入れる
- `allowed-tools` は列挙したツールの許可プロンプトを省略する。他のツールを禁止するわけではない
- `` !`command` `` と書くと、Claude に渡す前にコマンドが実行され、出力が埋め込まれる。`git diff` の結果などはこの形で渡す
- `context: fork` を付けると、独立したサブエージェントで実行され、結果だけが元の会話に返る。大きな差分を読むレビューなど、元の会話のコンテキストを消費したくないスキルに付ける

詳細は [Claude Code のスキルのドキュメント](https://code.claude.com/docs/ja/skills) を参照してください。

## 注意事項

- スキルは汎用的に作られているため、プロジェクトに合わせて適宜修正してください
- 特定の技術スタック（React, Python等）を使用する場合は、該当するベストプラクティスをスキルに追加することを推奨します

## 参考

- lint-ja スキルは [textlint-rule-preset-ai-writing](https://github.com/textlint-ja/textlint-rule-preset-ai-writing) を参考に作成されています
- lint-ja の「AI が書いた文章に出やすい単語」は [textlint-rule-preset-ai-words-ja](https://github.com/p1ass/textlint-rule-preset-ai-words-ja#%E6%A4%9C%E5%87%BA%E3%81%99%E3%82%8B%E5%8D%98%E8%AA%9E) の検出単語を参考にしています
- lint-ja の文章規範（段落構成、論証の厳密さ、演出の抑制など）は [日本語技術文書の文章規範（k16shikano）](https://gist.github.com/k16shikano/fd287c3133457c4fd8f5601d34aa817d) を参考にしています

## ライセンス

MIT License

---

# English

# Claude Code Setup Template

A setup template for starting Claude Code in new repositories. Includes custom skills and MCP server configurations that can be customized for your project.

## Purpose

- Quickly set up Claude Code environment in new projects
- Provide generic custom skills as templates
- Share and standardize skills within teams
- Provide MCP server configuration examples

## Quick Start

Copy the contents of this repository to your project root.

```bash
cp -r /path/to/shu-claude-template/* /path/to/your-project/
```

## Usage

Skills can be invoked with `/skill-name`.

```
/init-project   # Project initialization
/review-diff    # Code review
/sync-docs      # Document synchronization
/lint-ja        # Japanese writing guidelines
/msg            # Prepare a commit message from staged changes
/msg-all        # Stage all changes and prepare a commit message
```

Claude loads `lint-ja` automatically when writing Japanese documents. The other skills have side effects, so they run only when invoked by the user with `/`.

## Included Files

### Template Files

| File                             | Description                       |
| -------------------------------- | --------------------------------- |
| `CLAUDE.md.template`             | Project description file template |
| `.claude/settings.json.template` | MCP server configuration template |

### Skills

| Skill          | Description                                                              |
| -------------- | ------------------------------------------------------------------------ |
| `init-project` | Project initialization (creates CLAUDE.md, analyzes structure)           |
| `review-diff`  | Reviews the diff from main in an isolated subagent and reports issues    |
| `sync-docs`    | Checks and fixes discrepancies between docs (CLAUDE.md, README.md, etc.) |
| `lint-ja`      | Guidelines for writing natural Japanese documents                        |
| `msg`          | Generates a commit message from staged changes and copies it to the clipboard |
| `msg-all`      | Stages all changes, generates a commit message and copies it to the clipboard |

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
    └── skills/                 # Custom skills (one directory per skill)
        ├── init-project/SKILL.md   # Project initialization
        ├── lint-ja/SKILL.md        # Japanese writing guide
        ├── msg/SKILL.md            # Commit message (staged changes)
        ├── msg-all/SKILL.md        # Commit message (all changes)
        ├── review-diff/SKILL.md    # Code review
        └── sync-docs/SKILL.md      # Document sync
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

To add project-specific review criteria, edit `.claude/skills/review-diff/SKILL.md`.

```markdown
# Example additional review criteria

- DDD design compliance
- Framework-specific best practices (React/Vue, etc.)
- Project-specific coding standards
```

### Customizing sync-docs

Edit the check targets according to your project's document structure.

## Adding New Skills

1. Create `.claude/skills/<skill-name>/SKILL.md`. The directory name becomes the skill name (e.g., `my-skill/SKILL.md` becomes `/my-skill`)
2. Start the file with YAML frontmatter
3. Write the prompt in Markdown format

Example frontmatter:

```markdown
---
name: my-skill
description: What it does and when to use it. Claude reads this to decide whether to load the skill automatically
argument-hint: [argument hint]
allowed-tools: Read, Grep, Bash(git diff:*)
---
```

Key fields:

- `description` is the only part always kept in context, so include the purpose and trigger keywords
- `allowed-tools` skips permission prompts for the listed tools. It does not block other tools
- `` !`command` `` runs the command before the prompt reaches Claude and embeds the output. Pass things like `git diff` results this way
- `context: fork` runs the skill in an isolated subagent and returns only the result to the main conversation. Use it for skills that read a lot, such as reviewing a large diff, so they do not consume the main context

See the [Claude Code skills documentation](https://code.claude.com/docs/en/skills) for details.

## Notes

- Skills are designed to be generic; customize them as needed for your project
- When using specific tech stacks (React, Python, etc.), consider adding relevant best practices to the skills

## References

- The lint-ja skill is based on [textlint-rule-preset-ai-writing](https://github.com/textlint-ja/textlint-rule-preset-ai-writing)
- The word list in lint-ja (words that AI-generated Japanese tends to use) is based on the detected words in [textlint-rule-preset-ai-words-ja](https://github.com/p1ass/textlint-rule-preset-ai-words-ja#%E6%A4%9C%E5%87%BA%E3%81%99%E3%82%8B%E5%8D%98%E8%AA%9E)
- The writing norms in lint-ja (paragraph structure, rigorous argumentation, restraint in dramatization, etc.) are based on [Japanese Technical Writing Norms by k16shikano](https://gist.github.com/k16shikano/fd287c3133457c4fd8f5601d34aa817d)

## License

MIT License
