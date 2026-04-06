# 10 Requirements — 管理対象・非対象・受入条件

## 目的

本セット（`react-app`）は、React アプリケーションを持つ target repo 向けの **SSOT 完成物プロファイル** を提供する。
AI エージェント（Copilot / Claude / GeminiCLI / Cursor / Codex）が参照する共通ルールと入口ファイルを一元管理する。

## 管理対象（In Scope）

本セットが管理するファイル・ディレクトリは以下のとおりです。

| パス | 対象 AI / 用途 |
| --- | --- |
| `AGENTS.md` | Codex 向け入口 |
| `CLAUDE.md` | Claude 向け入口 |
| `GEMINI.md` | GeminiCLI 向け入口 |
| `.agent.md` | CopilotCLI などの CLI 系サブエージェント向け補助入口 |
| `.github/copilot-instructions.md` | Copilot 共通入口 |
| `.github/copilot/00-index.md` | Copilot SSOT 参照入口（文書索引） |
| `.github/copilot/10-requirements.md` | 管理対象・非対象・受入条件（本ファイル） |
| `.github/copilot/20-architecture.md` | ディレクトリ構造・distribution_root・展開イメージ |
| `.cursor/rules/**` | Cursor 向けルール群 |

## 管理対象外（Out of Scope）

以下は本セットの管理対象外です。

- アプリケーションコード（`src/`、`public/`、`package.json` など）
- `.github/workflows/**`（CI/CD ワークフロー）
- `skills/`、`mcp/`、`schema/`、`policies/` 配下の内容
- 推測によるファイル追加

## セット構造の前提

- 1 セット = 1 完成物
- 複数セットの同時適用は禁止
- `distribution_root` を基準に target repo にそのまま展開可能であること
- AI 入口ファイルの責任は ssot-core が持つ
- path 衝突は禁止（`catalog-path-ownership-draft.md` に従う）

## 受入条件

- `distribution_root: ../../react-app` が `sets/react-app/set.yml` に定義されていること
- 上記「管理対象」に列挙されたすべてのファイルが `react-app/` 配下に存在すること
- アプリケーションコードが含まれていないこと
- `.github/workflows/**` が含まれていないこと
