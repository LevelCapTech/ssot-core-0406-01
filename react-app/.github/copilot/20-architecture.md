# 20 Architecture — ディレクトリ構造・distribution_root・展開イメージ

## このセットの位置づけ

本セット（`react-app`）は **SSOT 完成物プロファイル** です。
`ssot-core` リポジトリ内の `react-app/` ディレクトリが `distribution_root` として定義されており、
target repo へそのままコピーして展開できる最終状態のファイル群を管理します。

## ssot-core 内のディレクトリ構造

```text
ssot-core/
├── sets/
│   └── react-app/
│       └── set.yml          ← distribution_root: ../../react-app を定義
├── react-app/               ← distribution_root（完成物ディレクトリ）
│   ├── AGENTS.md            ← Codex 向け入口
│   ├── CLAUDE.md            ← Claude 向け入口
│   ├── GEMINI.md            ← GeminiCLI 向け入口
│   ├── .agent.md            ← CLI 系サブエージェント向け補助入口
│   ├── .github/
│   │   ├── copilot-instructions.md   ← Copilot 共通入口
│   │   └── copilot/
│   │       ├── 00-index.md           ← 文書索引
│   │       ├── 10-requirements.md    ← 管理対象・非対象
│   │       └── 20-architecture.md   ← 本ファイル
│   └── .cursor/
│       └── rules/
│           └── ssot.mdc              ← Cursor 向けルール
└── README.md
```

## set.yml の定義

```yaml
version: 1
distribution_root: ../../react-app
```

- `distribution_root` は `sets/react-app/set.yml` からの相対パスで解決される
- `distribution_root` が指すディレクトリを、target repo への展開起点とする
- `include` は使用しない（`ssot-core` の専用仕様に従う）

## 展開イメージ

`react-app/` 配下のファイルは、target repo のルートを起点としてそのまま配置されます。

| ssot-core 内のパス | target repo へのコピー先 |
| --- | --- |
| `react-app/AGENTS.md` | `AGENTS.md` |
| `react-app/CLAUDE.md` | `CLAUDE.md` |
| `react-app/GEMINI.md` | `GEMINI.md` |
| `react-app/.agent.md` | `.agent.md` |
| `react-app/.github/copilot-instructions.md` | `.github/copilot-instructions.md` |
| `react-app/.github/copilot/00-index.md` | `.github/copilot/00-index.md` |
| `react-app/.github/copilot/10-requirements.md` | `.github/copilot/10-requirements.md` |
| `react-app/.github/copilot/20-architecture.md` | `.github/copilot/20-architecture.md` |
| `react-app/.cursor/rules/**` | `.cursor/rules/**` |

## ssot-sync-controller との関係

- ssot-sync-controller は `set.yml` の `distribution_root` を読み込み、対象ディレクトリを target repo に PR 形式で配布する
- path remap は行わない（catalog のパスをそのまま保持）
- 配布は PRベースで行われ、人間がレビュー・マージする

## 他 repo との責務境界

| repo | 責務 |
| --- | --- |
| `ssot-core`（本リポジトリ） | AI 入口ファイル群の最終責任者 |
| `ssot-schema` | `.github/instructions/**` などの運用ルール系 SSOT |
| `ssot-policies` | セキュリティ・禁止事項などのポリシー系 SSOT |
| `skills-core` / `skills-domain` | スキル実体とセット定義 |
| `mcp-tools` / `mcp-server-core` | MCP tool 定義と実行基盤 |
| `ssot-sync-controller` | 中央同期ロジック（配布の制御） |
| target repos | 何を使うかを宣言するだけ（`ssot-bot.yml`） |
