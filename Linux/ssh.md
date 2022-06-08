# SSH

## SSH接続できないとき

* sshの設定を確認する
    * セキュリティ的にあまりよくないけどwindows→linuxでパスワード認証するときは、
    　passwordAuthenticationがyesになっているかを確認

    　devian系は/etc/ssh/sshd_config

## 認証方式
1. パスワード認証方式
2. 公開鍵認証方式
3. ワンタイムパスワード認証方式

## パスワード認証
```
ssh ユーザ名@<対象ホストのIP>
ssh -l msfadmin <対象ホストのIP>
```

## 公開鍵認証
公開鍵のほうが安全。
①クライアントが秘密鍵と公開鍵を作成し、接続先に公開鍵を送る  
②秘密鍵を利用して署名を作成し、署名のみを接続先に送る  
 接続先では公開鍵を使って署名を検証しログイン許可する。  

- SSH鍵作成
```bash
$ ssh-keygen -t rsa -b 4096
```
-b: 鍵のbit長を指定
RSAの公開鍵暗号では、安全な鍵長は2048bitと定義されているが、
量子コンピューティングにイノベーションが起これば一遍する可能性あり
https://www.nict.go.jp/publication/NICT-News/1303/01.html

- 管理ホストから対象ホストへ公開鍵を転送
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub root@xxx.xxx.xxx.xxx
```
※転送先のホストのユーザの~/.ssh配下にauthorized_keysが生成される

- 管理ホストから対象ホストへssh接続する
```bash
ssh -i ~/.ssh/id_rsa root@192.168.11.103
```

### SSHのログ
- /var/log/auth.log or /var/log/secure

### SSHの設定ファイル
パスワード認証がyesになっているかはここを確認
```
/etc/ssh/ssh_config
```

### SSHの再起動

Debian(Ubuntu, kali)
```bash
sudo /etc/init.d/ssh restart
```
