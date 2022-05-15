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

www     IN    A xxx.xxx.xxx.xxx
```

ゾーンファイルには必ずSOAレコードの記載が必要。SOAレコードとは  
権威を持つゾーンの開始点を示す[^1]  

**SERIAL**:シリアルナンバー。32ビットで示す。権威DNSサーバーが持つSerial Numberがセカンダリサーバーが持つそれより大きい場合、ゾーン転送が行われる。番号のフォーマットとして、一般的にはYYYMMDDnnが利用される。  

**REFRESH**:セカンダリサーバーがプライマリサーバーにゾーン情報を確認する時間間隔。32ビットで示す。  

**RETRY**:セカンダリサーバーがゾーン情報を更新できなかった場合に再更新する時間間隔。32ビットで示す。  

**EXPIRE**:ゾーン情報の更新ができない状態が続いた場合、セカンダリサーバ
のデータを継続利用できる最大時間。32ビットで示す。

**MINIMUM**:ネガティブキャッシュのTTL。32ビットで示す。ネガティブ
キャッシュとは、存在しないドメイン名であるという情報のキャッシュのこ
と。


SOAレコードの構造  
[^1][](https://jprs.jp/tech/material/rfc/RFC1034-ja.txt Page. 12).  

- ゾーンファイルの文法チェック  
```bash
nsd-checkzone example.test /etc/nsd/example.test.zone
```
※zone example.test is okと表示されれば成功

- nsd.confの修正
```bash
$ sudo vi /etc/nsd/nsd.conf
```
(https://www.nlnetlabs.nl/documentation/nsd/nsd.conf/)

```
server:
    ip-address:xxx.xxx.xxx.xxx
    do-pi6: no
zone:
    name: "example.test"
    zonefile: "example.test.zone"
```

- nsdを起動
nsd.confの文法チェック  
```bash
$ nsd-checkonf /etc/nsd/nsd.conf
```
nsdの起動  
```bash
$ sudo systemctl start nsd

- ゾーンファイル変更後の再反映
```bash
$ sudo nsd-control reload
$ sudo nsd-control reconfig
```