# devcontainer-template

.devcontainerを使用して統一された開発環境を構築するためのテンプレートプロジェクトです。

## 概要

このテンプレートは、Dockerベースの開発環境を簡単にセットアップできるように設計されています。Debian slim bookwormをベースとし、開発に必要な最低限のツールが事前にインストールされます。

## 含まれるツール

### 基本ツール
- git
- curl
- mise (バージョン管理ツール)

### mise経由でインストールされるツール (LTS版)
- node
- python
- claudecode
- github-cli
- ripgrep
- jq
- fzf

## セットアップ

### 前提条件
- Docker
- Docker Compose
- VS Code + Dev Containers拡張機能

### 使用方法

1. このテンプレートをクローンまたはダウンロード
2. プロジェクトルートで`.devcontainer`ディレクトリが作成されていることを確認
3. VS Codeでプロジェクトを開く
4. コマンドパレット（Ctrl+Shift+P）から「Dev Containers: Reopen in Container」を実行

### SSH・GitHub CLI設定

GitHub CLIとSSHキーを自動でマウントするため、以下のファイルが必要です：

```yaml
# compose.ymlで自動マウント
~/.ssh/authorized_keys:/home/vscode/.ssh/authorized_keys:cached
~/.config/gh/hosts.yml:/home/vscode/.config/gh/hosts.yml:cached
```

## ディレクトリ構成

```
.devcontainer/
├── Dockerfile
├── compose.yml
├── devcontainer.json
└── README.md
```

## カスタマイズ

- `.mise.toml`でツールのバージョンを変更
- `Dockerfile`で追加パッケージのインストール
- `devcontainer.json`でVS Code拡張機能の追加
