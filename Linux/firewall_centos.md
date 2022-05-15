# firewalldのZoneについて
ネットワークインターフェースごとに決める通信の許可・拒否のルールセット
|名前|説明|
----|----
|public|sshとdhcp-6vclientのみ許可パブリックネットワーク向け|
|drop|外部からのすべての通信を破棄|
|block|内部から外部に向けた通信で、外部からかえってきたもののみ許可。それ以外は拒否|

# Firewallの設定

- dropテーブルの設定一覧を確認
```
# firewall-cmd –-zone=drop --list-all
```
- publicテーブルの設定一覧を確認
```
# firewall-cmd --zone=public --list-all
```
- HTTP通信を恒久的に許可
```
# firewall-cmd --zone=public --add-service=http --permanent
```
- 設定を反映させる
```
# firewall-cmd --reload
```