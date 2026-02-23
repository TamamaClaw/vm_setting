# vm_setting

Ubuntu VM 初期セットアップ用 Ansible Playbook

## 含まれる設定

| ロール | 内容 |
|--------|------|
| `ssh` | openssh-server インストール・起動 |
| `tailscale` | Tailscale VPN インストール・起動 |
| `japanese` | 日本語キーボード・ibus-mozc・フォント |
| `terminal` | ターミナルにgitブランチ表示 (bash/zsh) |
| `browsers` | Google Chrome & Brave Browser |
| `tools` | curl / gh / git / jq 等の基本ツール |
| `mise` | mise ランタイムマネージャー |

## 使い方

```bash
# 1. インベントリを作成
cp inventory.ini.example inventory.ini
# → VM の IP / ユーザーを編集

# 2. ansible をインストール
pip3 install ansible

# 3. チェックモード (実際には何も変えない)
ansible-playbook -i inventory.ini playbook.yml --check

# 4. 本番実行
ansible-playbook -i inventory.ini playbook.yml

# 5. タグ指定 (特定ロールだけ)
ansible-playbook -i inventory.ini playbook.yml --tags ssh,tools
ansible-playbook -i inventory.ini playbook.yml --tags terminal
```

## 変数カスタマイズ

`group_vars/all.yml` を編集:

```yaml
# Tailscale 自動接続 (任意)
tailscale_auth_key: "tskey-auth-xxxx"

# mise でインストールするランタイム (任意)
mise_tools:
  - { name: node, version: "22" }
  - { name: python, version: "3.12" }
```

## ディレクトリ構成

```
vm_setting/
├── playbook.yml
├── inventory.ini.example
├── group_vars/
│   └── all.yml
└── roles/
    ├── ssh/
    ├── tailscale/
    ├── japanese/
    ├── terminal/
    ├── browsers/
    ├── tools/
    └── mise/
```
