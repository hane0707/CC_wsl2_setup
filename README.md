# CC_wsl2_setup

Claude Code CLI を Windows (WSL2) 環境で活用するためのガイド集。

## ドキュメント

| ファイル | 内容 | 更新 |
|---|---|---|
| [claude-code-wsl2-setup.html](./claude-code-wsl2-setup.html) | WSL2 セットアップガイド — Windows Terminal + WSL2 (Ubuntu) + GitHub 連携の初期構築手順 | 2026-04-07 |
| [claude-code-security-settings.html](./claude-code-security-settings.html) | セキュリティ設定ガイド — permissions.deny・hooks・CLAUDE.md による安全な運用設定 (v1.4) | 2026-04-11 |
| [claude-code-plugin-analysis.html](./claude-code-plugin-analysis.html) | プラグイン比較分析 — superpowers / oh-my-claudecode / everything-claude-code / ralph の採用指針 | 2026-04-12 |
| [claude-code-2026-setup-proposal.html](./claude-code-2026-setup-proposal.html) | **業務セットアップ改善提案** — サンドボックスの利点を保ったまま「PR が作れない」「画面が見えない」を解消する。図解つき | 2026-08-02 |

## skills

配布用の skill テンプレート。`~/.claude/skills/` 配下にコピーして使う。

| skill | 内容 |
|---|---|
| [create-pr](./skills/create-pr/SKILL.md) | 非対話での PR 作成手順。`gh pr create` の対話 TUI による停止を回避し、`gh api` へのフォールバックを含む |

> `~/.claude/skills/` はサンドボックスが書き込みを拒否するため、`cp` では設置できない。
> エディタまたは Claude Code のファイル書き込みツール経由で配置すること。

## 環境

- Windows 11 + WSL2 (Ubuntu)
- Claude Code CLI 2.1.220
- GitHub (HTTPS + PAT 認証) / gh 2.91.0
- Opus 5 + Sonnet 5
