# ssot-core

このリポジトリは **SSOT（Single Source of Truth）** の中核リポジトリです。
AIエージェント（Copilot / Claude / GeminiCLI / Cursor / Codex）が参照する共通ルール・入口ファイル群を一元管理します。

---

## このリポジトリの目的

- 複数の案件リポジトリ（target repo）に配布する AI エージェント向け入口ファイルと共通ルールを、単一の管理場所に集約する
- target repo への配布は **PRベース** で行い、人間がレビュー・マージする
- AI 入口ファイルの最終責任者は本リポジトリ（`ssot-core`）とする

---

## 完成物プロファイルであること

本リポジトリが管理するのは **アプリケーションコードではなく「SSOT 完成物プロファイル」** です。

- 完成物プロファイルとは、target repo にそのままコピーして展開できる最終状態のファイル群を指す
- 展開後に追加の path remap や merge を要さず、AI エージェントが参照できる状態になっていること

---

## 管理対象

本リポジトリが管理するファイル・ディレクトリは以下のとおりです。

| パス | 対象 AI / 用途 | 担当 |
| --- | --- | --- |
| `AGENTS.md` | Codex 向け入口 | ssot-core |
| `CLAUDE.md` | Claude 向け入口 | ssot-core |
| `GEMINI.md` | GeminiCLI 向け入口 | ssot-core |
| `.agent.md` | CopilotCLI などの CLI 系サブエージェント向け補助入口 | ssot-core |
| `.github/copilot-instructions.md` | Copilot 共通入口 | ssot-core |
| `.github/copilot/00-index.md` | Copilot SSOT 参照入口（文書索引） | ssot-core |
| `.github/copilot/10-requirements.md` | 管理対象・非対象・受入条件 | ssot-core |
| `.github/copilot/20-architecture.md` | ディレクトリ構造・distribution_root・展開イメージ | ssot-core |
| `.cursor/rules/**` | Cursor 向けルール群 | ssot-core |

---

## 管理対象外

以下は本リポジトリの管理対象外です。

- アプリケーションコード（`src/`、`public/`、`package.json` など）
- `.github/workflows/**`（CI/CD ワークフロー）
- `skills/`、`mcp/`、`schema/`、`policies/` 配下の内容
- 推測によるファイル追加

---

## セット構造

```text
ssot-core/
├── sets/
│   └── react-app/
│       └── set.yml          ← distribution_root: ../../react-app を定義
├── react-app/               ← distribution_root（完成物ディレクトリ）
│   ├── AGENTS.md
│   ├── CLAUDE.md
│   ├── GEMINI.md
│   ├── .agent.md
│   ├── .github/
│   │   ├── copilot-instructions.md
│   │   └── copilot/
│   │       ├── 00-index.md
│   │       ├── 10-requirements.md
│   │       └── 20-architecture.md
│   └── .cursor/
│       └── rules/
│           └── ssot.mdc
└── README.md
```

- 1 セット = 1 完成物
- 複数セットの同時適用は禁止
- target repo は `ssot-bot.yml` で「何を使うか」だけを宣言する

---

## distribution_root 説明

`sets/<set-name>/set.yml` で `distribution_root` を指定します。

```yaml
version: 1
distribution_root: ../../react-app
```

- `distribution_root` は `set.yml` からの相対パスで解決される
- `distribution_root` が指すディレクトリを、target repo への展開起点とする
- `include` は使用しない（`ssot-core` の専用仕様に従う）
- `distribution_root` 配下の相対パスは、そのまま保持して target repo に展開される

---

## 配布方法（PRベース）

1. target repo の `ssot-bot.yml` に使用するセットと version を宣言する
2. `ssot-sync-controller` がセット定義を読み込み、desired state を生成する
3. `ssot-bot`（GitHub App）が対象 target repo に PR を作成する
4. 人間がレビュー・マージする

---

## ssot-sync-controller との関係

- `ssot-sync-controller` は本リポジトリの `set.yml` を読み込み、`distribution_root` 配下のファイルを target repo に PR 形式で配布する制御を担う
- path remap は行わない（catalog のパスをそのまま保持）
- 同期ロジックは `ssot-sync-controller` に中央集約されており、target repo にロジックは分散しない

---

## 他 repo との責務境界

| repo | 責務 |
| --- | --- |
| `ssot-core`（本リポジトリ） | AI 入口ファイル群の最終責任者 |
| `ssot-schema` | `.github/instructions/**` などの運用ルール系 SSOT |
| `ssot-policies` | セキュリティ・禁止事項などのポリシー系 SSOT |
| `skills-core` / `skills-domain` | スキル実体とセット定義 |
| `mcp-tools` / `mcp-server-core` | MCP tool 定義と実行基盤 |
| `ssot-sync-controller` | 中央同期ロジック（配布の制御） |
| `ssot-bot` | GitHub App 認証・GitHub 操作の実行主体 |
| target repos | 何を使うかを宣言するだけ（`ssot-bot.yml`） |

他 repo は `catalog-path-ownership-draft.md` の「対応する AI と主入口 path」および「補助入口 / 参照入口」に載る path と同じ target path に配置してはなりません。
