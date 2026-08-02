# CC_wsl2_setup

Claude Code CLI を Windows (WSL2) 環境で活用するためのガイド集。

## ドキュメント

| ファイル | 内容 | 更新 |
|---|---|---|
| [claude-code-wsl2-setup.html](./claude-code-wsl2-setup.html) | **セットアップ手順書（メイン）** — WSL2 + サンドボックス + GitHub 連携 + ブラウザ確認環境の構築手順 (v2.0) | 2026-08-03 |
| [claude-code-security-settings.html](./claude-code-security-settings.html) | セキュリティ設定ガイド — permissions.deny・hooks・CLAUDE.md による安全な運用設定 (v1.4) | 2026-04-11 |
| [claude-code-plugin-analysis.html](./claude-code-plugin-analysis.html) | プラグイン比較分析 — superpowers / oh-my-claudecode / everything-claude-code / ralph の採用指針 | 2026-04-12 |
| [claude-code-2026-setup-proposal.html](./claude-code-2026-setup-proposal.html) | 改善提案・検証記録 — 各設定を選んだ理由、実測データ、段階的な自律化のロードマップ。図解つき | 2026-08-03 |

> **実際にセットアップするときは [claude-code-wsl2-setup.html](./claude-code-wsl2-setup.html) を見てください。**
> 提案書のほうは「なぜその設定にしたのか」の背景資料です。

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
