---
name: create-pr
description: Use when creating a GitHub pull request from Claude Code - avoids the interactive TUI of `gh pr create` that hangs in non-interactive shells, with a `gh api` fallback. Triggers on "PRを作って", "プルリクを出して", "create a PR", "open a pull request".
---

# 非対話での PR 作成

## なぜこの手順が必要か

`gh pr create` を引数なしで実行すると、タイトル入力・本文エディタ起動・最終確認の
対話 UI が起動する。Claude Code の Bash はキーボード入力を受け付けないため、
ここで停止または失敗する。**サンドボックスやネットワークが原因ではない。**

そのため **対話が発生しないようフラグを全て明示する**ことが唯一の要件になる。

## 手順

### 1. 前提を確認する

```bash
git rev-parse --abbrev-ref HEAD    # 現在のブランチ
git status --short                  # 未コミットの変更
gh auth status                      # 認証とスコープ（repo が必要）
```

デフォルトブランチ（`main` / `master`）上にいる場合は PR を作れない。
先に作業ブランチへ切り替える。

```bash
git checkout -b <type>/<short-description>
```

### 2. コミットする

コミットメッセージは日本語で記述する。

### 3. push する

```bash
git push origin "$(git rev-parse --abbrev-ref HEAD)"
```

**`-u` / `--set-upstream` は使わない。** サンドボックスは `.git/config` を保護対象と
しており、そのロックファイルのパス（`.git/config.lock`）に `/dev/null` をマウントして
書き込みを封じている。そのため `-u` を付けると push 自体は成功するのに
`unable to write upstream branch configuration` で異常終了し、失敗したように見える。

upstream 設定が必要な場合は、サンドボックス外（ユーザー自身のターミナル）で実行する。

`--force` は使わない（`permissions.deny` で禁止されている）。

### 4. PR 本文をファイルに書き出す

本文を `--body` にインラインで渡すと、改行やバッククォートのエスケープで壊れやすい。
必ずファイル経由で渡す。

```bash
cat > "$(git rev-parse --git-dir)/PR_BODY.md" <<'EOF'
## 概要

<何を、なぜ変えたか>

## 変更内容

- <箇条書き>

## 確認方法

- <レビュアーが検証できる手順>
EOF
```

`.git/` 配下に置くため、リポジトリを汚さない。

### 5. PR を作成する（本命）

**すべてのフラグを明示すること。1つでも欠けると対話に入る。**

```bash
gh pr create \
  --base main \
  --head "$(git rev-parse --abbrev-ref HEAD)" \
  --title "<タイトル>" \
  --body-file "$(git rev-parse --git-dir)/PR_BODY.md"
```

必須フラグ:

| フラグ | 省略するとどうなるか |
|---|---|
| `--title` | タイトルを対話で聞かれる |
| `--body` または `--body-file` | エディタが起動する |
| `--base` | 対象ブランチを対話で聞かれる場合がある |
| `--head` | 通常は推測されるが、明示すると確実 |

`--web` は**使わない**（WSL2 のサンドボックス内から Windows のブラウザを起動できない）。

### 6. 失敗した場合のフォールバック

`gh` 自体が使えない場合、REST API を直接叩く。`gh api` は対話 UI を持たない。

```bash
gh api -X POST "/repos/<owner>/<repo>/pulls" \
  -f head="<branch>" \
  -f base=main \
  -f title="<タイトル>" \
  -f body="$(cat "$(git rev-parse --git-dir)/PR_BODY.md")"
```

### 7. 結果を確認する

```bash
gh pr view --json number,url,state --jq '{number,url,state}'
```

URL が返れば成功。**URL を確認するまで「PR を作成した」と報告しない。**

## トラブルシューティング

| 症状 | 原因と対処 |
|---|---|
| コマンドが応答しない／固まる | フラグの指定漏れ。手順5の必須フラグを再確認する |
| `must be on a branch named differently than the default branch` | デフォルトブランチ上にいる。作業ブランチを切る |
| `No commits between X and Y` | push した差分がない。コミットと push を確認する |
| `HTTP 403` / `Resource not accessible` | トークンのスコープ不足。`gh auth status` で `repo` を確認する |
| 接続エラー・タイムアウト | `sandbox.network.allowedDomains` に `*.github.com` があるか確認する |
| `unable to write upstream branch configuration` | `-u` を付けたことが原因。**push 自体は成功している。** `git ls-remote --heads origin <branch>` で確認し、`-u` を外して再実行する |
| `could not lock config file .git/config` | 同上。`.git/config.lock` はサンドボックスがマウントした `/dev/null` であり、削除も書き込みもできない（正常な保護動作） |

## 検証記録

2026-08-02、本環境（Claude Code 2.1.220 / gh 2.91.0 / WSL2 Ubuntu）にて実測。

- `POST /repos/.../pulls` の到達性を確認（422 応答＝通信・認証・権限はすべて正常）
- 本手順に従い **PR #1 の作成に成功**。`gh pr create` が停止する原因が
  ネットワークやサンドボックスではなく、フラグ省略時の対話 UI であることを確定
- `git push -u` は `.git/config` 保護により失敗するが、push 自体は成功することを確認
- `.git/` 直下へのファイル書き込みは可能（`PR_BODY.md` の置き場所として使える）
