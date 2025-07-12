# CLAUDE.md - devcontainer-template

## プロジェクト概要

このプロジェクトは.devcontainerを使用したDockerベース開発環境のテンプレートです。

## 技術スタック

- **Base Image**: Debian slim bookworm
- **Package Manager**: apt (システムレベル), mise (開発ツール)
- **Container Technology**: Docker + Docker Compose
- **IDE Integration**: VS Code Dev Containers

## 開発方針

### ファイル構成
- `.devcontainer/` 配下に全ての設定ファイルを配置
- `Dockerfile`, `compose.yml`, `devcontainer.json`, `README.md` を含む
- `.mise.toml` でツールバージョン管理

### セキュリティ
- ホストのSSHキーとGitHub CLI設定をマウント
- 読み取り専用でキャッシュ可能な設定

### バージョン管理
- 全てのツールはLTS版を使用
- miseによる統一されたバージョン管理

## 開発時の注意点

1. **ファイル変更時**: .devcontainerディレクトリ内のファイルを変更した場合は、コンテナの再ビルドが必要
2. **ポートマッピング**: 新しいサービスを追加する際は、compose.ymlでポート設定を追加
3. **ツール追加**: 新しいツールは`.mise.toml`に追加してmise経由でインストール

## テスト・検証

1. コンテナが正常にビルドできることを確認
2. 全ての指定ツールがインストールされていることを確認
3. GitHub CLIが認証済み状態で使用できることを確認