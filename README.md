# vm_setting

Ubuntu VM 初期セットアップ用 Ansible Playbook集

## クイックスタート

```bash
# 1. リポジトリをクローン
git clone https://github.com/TamamaClaw/vm_setting.git
cd vm_setting

# 2. インベントリを作成 (パスワードを設定)
cp inventory.ini.example inventory.local
vi inventory.local   # ansible_become_password を編集

# 3. VM全体セットアップ
ansible-playbook playbook.yml

# 4. Dataikuだけインストール
ansible-playbook dataiku.yml
```

## チェックモード (dry-run)

```bash
ansible-playbook playbook.yml --check
```

## タグ指定

```bash
ansible-playbook playbook.yml --tags ssh,tools
```

## 設定のカスタマイズ

`group_vars/all.yml` で共通変数を上書き:

```yaml
# SSH設定
ssh_port: 2232
ssh_authorized_keys:
  - "ssh-ed25519 AAAA... your@host"

# miseツール
mise_tools:
  - { name: node, version: "22" }
  - { name: python, version: "3.12" }

# Tailscale
tailscale_auth_key: "tskey-auth-xxxx"
```

## ロール一覧

| ロール | 内容 |
|--------|------|
| ssh | openssh-server, ポート2232, 公開鍵認証のみ |
| tailscale | Tailscaleインストール・設定 |
| japanese | 日本語ロケール・タイムゾーン設定 |
| terminal | zsh/tmux/fzf/bat/eza |
| browsers | Chromium/Firefox |
| tools | git/curl/jq/htop等 |
| mise | Node.js/Python等ランタイム管理 |
| dataiku | Dataiku DSS (Docker) |
