このプロジェクトは.devcontainerを使用して、下記の開発環境を整えられるようにするテンプレートプロジェクトです。

Dockerfileのbaseはdebian slim bookworm

インストールする最低限の機能
git
curl
mise

.mise.tomlを作成し、miseにより、node, python, claudecode, github-cli, ripgrep, jq, fzfのltsをinstall

compose.ymlで下記をmountすることでghコマンドでログイン済の状態にする
~/.ssh/authorized_keys:/home/vscode/.ssh/authorized_keys:cached
~/.config/gh/hosts.yml:/home/vscode/.config/gh/hosts.yml:cached

.devcontainerディレクトリの中には下記があることを想定
Dockerfile, compose.yml, devcontainer.json, README.md
