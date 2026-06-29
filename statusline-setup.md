# Claude Code ステータスライン設定

コンテキストの残りトークン割合をステータスラインに表示する設定。

表示例:

```
Opus 4.8 (1M) · svsfmlp-web (main) · 85%
```

- 構成: モデル名 · カレントディレクトリ名 (git ブランチ) · 残り割合
- 残り割合の色: シアン=40%超 / 黄=16〜40% / 赤=15%以下。取得できないときは `--%`

## 前提

- `jq` が必要（`brew install jq`）
- git ブランチ表示は git リポジトリ内でのみ有効（任意）

## セットアップ手順（新しい PC）

1. `~/.claude/statusline.sh` を下記内容で作成する
2. 実行権限を付与する: `chmod +x ~/.claude/statusline.sh`
3. `~/.claude/settings.json` に `statusLine` を追記する
4. Claude Code を再起動する

## ファイル

### `~/.claude/statusline.sh`

```bash
#!/bin/bash
# Claude Code ステータスライン: モデル名・ディレクトリ・コンテキスト残り割合を表示
# 例: Opus 4.8 (1M) · svsfmlp-web (main) · 85%
input=$(cat)

model=$(printf '%s' "$input" | jq -r '.model.display_name // "Claude"')
cur_dir=$(printf '%s' "$input" | jq -r '.workspace.current_dir // .cwd // ""')
dir_name=$(basename "$cur_dir" 2>/dev/null)

# git ブランチ（リポジトリ内なら）
branch=$(git -C "$cur_dir" rev-parse --abbrev-ref HEAD 2>/dev/null)

# 残りコンテキスト割合（remaining_percentage を優先、null ならトークン数から算出）
remaining=$(printf '%s' "$input" | jq -r '
  if .context_window.remaining_percentage != null then
    .context_window.remaining_percentage
  elif (.context_window.total_input_tokens != null
        and .context_window.context_window_size != null
        and .context_window.context_window_size > 0) then
    (100 - (.context_window.total_input_tokens / .context_window.context_window_size * 100))
  else
    empty
  end' | cut -d. -f1)

# 残量に応じた色（dark-daltonized 向けにシアン/黄/赤）
if [ -z "$remaining" ]; then
  pct="--%"
else
  if [ "$remaining" -le 15 ]; then
    color=$'\033[31m'   # 赤: 残りわずか
  elif [ "$remaining" -le 40 ]; then
    color=$'\033[33m'   # 黄: 注意
  else
    color=$'\033[36m'   # シアン: 余裕あり
  fi
  reset=$'\033[0m'
  pct="${color}${remaining}%${reset}"
fi

# 組み立て（存在する要素だけ · で連結）
line="$model"
[ -n "$dir_name" ] && line="$line · $dir_name"
[ -n "$branch" ] && line="$line ($branch)"
line="$line · $pct"

printf '%s' "$line"
```

### `~/.claude/settings.json`（追記する部分）

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline.sh"
  }
}
```

## 仕組み

ステータスラインのコマンドには標準入力で JSON が渡される。コンテキスト情報は `context_window` に含まれ、`remaining_percentage` と `used_percentage` は Claude Code 側で事前計算済み。`context_window_size` は通常 200000、1M コンテキストでは 1000000。スクリプトはローカル実行のため API トークンを消費しない。

## カスタマイズ

- 数字だけにする: 組み立て部分を `line="$pct"` に変更する
- 色を消す: `color` と `reset` の代入を空文字にする
- 色のしきい値: `-le 15` / `-le 40` の数値を変更する

公式ドキュメント: https://code.claude.com/docs/en/statusline.md
