# CC_wsl2_setup

Windows (WSL2) 環境で Claude Code を業務利用するためのセットアップ資料。

## セットアップする

**[claude-code-wsl2-setup.html](./claude-code-wsl2-setup.html) を開いてください。これ一つで完結します。**

WSL2 の導入から、サンドボックス設定、CLAUDE.md、プラグイン、GitHub 連携、
AI に画面を確認させるブラウザ環境まで、必要な手順をすべて含みます。

進捗チェックボックス付きなので、中断しても続きから再開できます。

## 資料

| ファイル | 内容 | いつ読むか |
|---|---|---|
| [claude-code-wsl2-setup.html](./claude-code-wsl2-setup.html) | **セットアップ手順書** | セットアップするとき。**これだけで完結する** |
| [claude-code-threat-model.html](./claude-code-threat-model.html) | 脅威モデルと設定の対応表 | なぜここまでやるのか納得したいとき。設定に抜け漏れがないか監査したいとき |
| [claude-code-design-notes.html](./claude-code-design-notes.html) | 設計判断と検証記録 | なぜこの構成を選んだのか、何を却下したのかを確認したいとき |

補足2本はいずれもセットアップには不要です。

## skills

配布用の skill テンプレート。`~/.claude/skills/` 配下に配置して使う。

| skill | 内容 |
|---|---|
| [create-pr](./skills/create-pr/SKILL.md) | 非対話での PR 作成手順。`gh pr create` の対話 UI による停止を回避し、`gh api` へのフォールバックを含む |

> `~/.claude/skills/` はサンドボックスが書き込みを拒否するため `cp` では設置できない。
> エディタか、Claude Code のファイル書き込みツール経由で配置すること。

## archive/

過去の検討資料。手順書に統合済みのため、通常は参照する必要はない。

| ファイル | 統合先 |
|---|---|
| claude-code-security-settings.html | 手順書（CLAUDE.md / プラグイン / hooks / メンテナンス） |
| claude-code-2026-setup-proposal.html | claude-code-design-notes.html |
| claude-code-plugin-analysis.html | 手順書（プラグイン導入 STEP） |

## 環境

- Windows 11 + WSL2 (Ubuntu)
- Claude Code CLI 2.1.220 / gh 2.91.0
- Opus 5 + Sonnet 5
