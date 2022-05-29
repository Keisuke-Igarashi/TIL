# SMB

* Server Message Block Protocol

windowsマシン間でファイル共有やプリンタ共有を行うためのプロトコル  
windowマシンだけでなく、linuxにも対応している  
→ Samba : SMB protcol をサポートするオープンソース。Unix systemでリリースされた。

* TCP/IPを用いてサーバーと接続をする

## 特徴

- ポート番号：137-139,445
- バージョン
    -SMB v1
    -SMB v2
    -SVB v3 ★通信暗号化機能あり。推奨

- 脆弱性多い（WannaCryもSMBを利用）

## アクセス方法

- smbclientコマンド
対話的に操作できる

```
# smbclient //<対象のホストのIP>/<ディレクトリパス> -U <ユーザ名>
```

```
smbclient //[IP]/[SHARE]
-U [name] : to specify the user
-p [port] : to specify the port
```


- mountコマンド
ローカルのディレクトリと同様に操作できる

先に同期用のディレクトリを作成
```
# mkdir -p /mnt/windows
```

マウント実施
```
# mount -t cifs -o username=xxxxxx,password=xxxxx,vers=V.V //192.168.YYY.XXX/xxxxx /mnt/windows/
```

### SMBのログ

- /var/log/samba/log.<machine名>
- ログレベルを10段階で設定できる

### よく利用するNSE
- バージョン特定
    - smb-os-dicvovery

### enmu4linux
SMBから情報を列挙するためのツール

```
enum4linux -h
```

```
enum4linux <対象のホストIP>

ユーザ情報
```
enum4linux -U <対象のホストIP>
```