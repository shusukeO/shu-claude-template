---
description: ステージ済みの変更だけからコミットメッセージを用意する（git add もコミットもしない）
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(printf:*), Bash(pbcopy:*)
model: haiku
---

# msg

以下のコンテキストは収集済み。**`git add` は実行しないこと**（ステージ済みの変更だけが対象）。
**追加で git status / git diff / git log を実行しないこと。**

## コンテキスト

- ステージ済みファイル: !`git status --short`
- ステージ済み差分: !`git diff --cached`
- 最近のコミットスタイル: !`git log --oneline -10`

## タスク

ステージ済み差分が空なら「ステージ済みの変更はありません」と報告して終了する。

差分がある場合、最近のコミットスタイル（`feat:` / `fix:` などのプレフィックス + 日本語、1 行）に
合わせたコミットメッセージを生成し、**1 回の Bash 呼び出し**で以下を実行する：

```bash
printf '%s\n' "メッセージ" > .git/MERGE_MSG && printf '%s' "メッセージ" | pbcopy
```

最後に、生成したメッセージと
「VSCode のコミットメッセージ欄を確認してコミットボタンを押してください（表示されない場合は Cmd+V）」
の案内だけを簡潔に表示する。変更内容の長い説明は不要。

## 禁止事項

- **`git add` を実行しないこと**（未ステージの変更はそのまま残す）
- **`git commit` / `git push` は絶対に実行しないこと**（コミットはユーザーが VSCode で行う）
