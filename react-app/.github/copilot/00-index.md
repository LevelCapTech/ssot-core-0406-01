# 00 Index — 文書索引（SSOT 参照入口）

このディレクトリは Copilot / 自動エージェント向けの **仕様の単一情報源（SSOT）** です。
必ず以下の順で参照してください。

## 参照順（優先度順）

1. [copilot-instructions.md](../copilot-instructions.md) — 規範層（短く強いルール）
2. [10-requirements.md](10-requirements.md) — 管理対象 / 非対象・受入条件
3. [20-architecture.md](20-architecture.md) — ディレクトリ構造・distribution_root・展開イメージ

## このリポジトリについて

本リポジトリ（`react-app/` セット）は **SSOT 完成物プロファイル** です。

- target repo にそのままコピーして展開できる最終状態のファイル群を管理する
- アプリケーションコードは含まない
- 1 セット = 1 完成物であり、複数セットの同時適用は禁止

## 使い方

- 仕様変更・追記は本ディレクトリに集約し、重複や分散を避ける
- 推測によるファイル追加は禁止。仕様に明示された構成のみを維持する
