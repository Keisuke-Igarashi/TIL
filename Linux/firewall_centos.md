# firewalldのZoneについて
ネットワークインターフェースごとに決める通信の許可・拒否のルールセット
|名前|説明|
----|----
|public|sshとdhcp-6vclientのみ許可パブリックネットワーク向け|
|drop|外部からのすべての通信を破棄|
|block|内部から外部に向けた通信で、外部からかえってきたもののみ許可。それ以外は拒否|

# Firewallの設定

* 状態確認

  ```bash
  systemctl status firewalld
  firewall-cmd --state
  ```

* 設定状況の確認

  ```bash
  # zoneの確認
  firewall-cmd --get-active-zone
  # zoneファイルの場所
  ls /usr/lib/firewalld/zones
  ```

* 利用可能なサービス

  ```bash
  firewall-cmd --get-services
  firewall-cmd --get-icmptypes
  ```

- dropテーブルの設定一覧を確認
```bash
firewall-cmd –-zone=drop --list-all
```
- publicテーブルの設定一覧を確認
```bash
firewall-cmd --zone=public --list-all
```
- HTTP通信を恒久的に許可
```bash
firewall-cmd --zone=public --add-service=http --permanent
```
- 設定を反映させる
```bash
firewall-cmd --reload
```

* IPアドレスの拒否設定

  ```bash
  firewall-cmd --zone=drop --permanent --add-source=10.0.100.103
  ```

  