# DNS設定
- dnsサーバの設定確認  
```bash
/etc/resolv.conf
```  

- 設定後再起動  
```bash
systemctl restart systemd-resolved
```  

# DNSサーバーインストール
- リポジトリアップデート
```bash  
sudo apt update 
```

- エラーが出る場合は以下を実行して再度リポジトリアップデート  
```bash
wget -q -O - https://archive.kali.org/archive-key.asc | apt-key add 
```

- nsdのインストール  
```bash
sudo apt install nsd
sudo apt list nsd
```

- ゾーンファイル作成  
```bash
sudo touch /etc/nsd/(ドメイン名).zone
sudo touch /etc/nsd/example.test.zone
sudo vi /etc/nsd/example.test.zone
```

- ゾーンファイルの中身  
```
@ IN SOA ts.example.test. mail.example.test. (
    YYYYMMDDxx  ; SERIAL
    28800       ; REFRESH
    7200        ; RETRY
    864000      ; EXPIRE
    86400       ; MINIMUM
)

www     IN    A 192.168.11.220
```

SOAレコードの構造  
[https://jprs.jp/tech/material/rfc/RFC1034-ja.txt](https://jprs.jp/tech/material/rfc/RFC1034-ja.txt Page. 12)



