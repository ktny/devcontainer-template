# .devcontainer 設定

このディレクトリには、VS Code Dev Containersで使用するDockerベース開発環境の設定ファイルが含まれています。

## ファイル構成

### Dockerfile
- **ベースイメージ**: `debian:bookworm-slim`
- **ユーザー**: `vscode` (UID: 1000)
- **インストール済みツール**:
  - git
  - curl 
  - ca-certificates
  - sudo
  - openssh-client
  - mise (ツールバージョン管理)

### compose.yml
Docker Composeサービス設定:
- **サービス名**: `devcontainer`
- **ボリュームマウント**:
  - プロジェクトルート → `/workspace` (cached)
  - `~/.ssh/authorized_keys` → `/home/vscode/.ssh/authorized_keys` (cached)
  - `~/.config/gh/hosts.yml` → `/home/vscode/.config/gh/hosts.yml` (cached)
- **コマンド**: `sleep infinity` (開発時の継続実行)

### devcontainer.json
VS Code Dev Container設定:
- **名前**: "Dev Container Template"
- **拡張機能**:
  - JSON Language Features
  - Tomorrow Kit Theme
  - GitLens
- **postCreateCommand**: `mise install && mise doctor`
- **追加マウント**: Git設定ファイル

## 前提条件

### ホスト環境に必要なファイル
1. **SSH設定**:
   ```
   ~/.ssh/authorized_keys
   ```

2. **GitHub CLI設定**:
   ```
   ~/.config/gh/hosts.yml
   ```

3. **Git設定**:
   ```
   ~/.gitconfig
   ```

## 使用方法

1. VS Codeでプロジェクトを開く
2. コマンドパレット（Ctrl+Shift+P）を開く
3. "Dev Containers: Reopen in Container" を実行
4. 初回起動時は自動的に:
   - Dockerイメージビルド
   - mise install実行
   - VS Code拡張機能インストール

## トラブルシューティング

### コンテナビルドエラー
```bash
# キャッシュクリアして再ビルド
docker compose -f .devcontainer/compose.yml build --no-cache
```

### mise installエラー
```bash
# コンテナ内で手動実行
mise doctor
mise install --verbose
```

### 権限エラー
```bash
# ホスト側でファイル権限確認
ls -la ~/.ssh/authorized_keys
ls -la ~/.config/gh/hosts.yml
```

## カスタマイズ

### 追加ツールのインストール
1. `.mise.toml` に追加したいツールを記載
2. コンテナを再ビルド

### VS Code拡張機能追加
1. `devcontainer.json` の `extensions` 配列に拡張機能IDを追加
2. コンテナを再ビルド

### 環境変数設定
1. `compose.yml` に `environment` セクションを追加
2. または `.env` ファイルを作成

## セキュリティ

- SSH設定とGitHub CLI設定は読み取り専用でマウント
- 本番環境では使用しない開発専用設定
- 機密情報は環境変数やシークレット管理ツールを使用
