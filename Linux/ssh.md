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

### teratermでの公開鍵認証設定

[公開鍵認証によるSSH接続 - Tera Termの使い方](https://webkaru.net/linux/tera-term-ssh-login-public-key/)

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

### ubuntuのSSH設定(sshdのインストールが必要)

* openssl-serverのインストール

    ```bash
    sudo apt install openssh-server
    ```

* [参考サイト](https://kaworu.jpn.org/ubuntu/Ubuntu%E3%81%ABssh%E3%81%A7%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%88%E3%81%86%E3%81%ABopenssh-server%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B)



### ポートフォーワーディング

* ダイナミックポートフォーワーディング

  SSHサーバにダイナミックポートフォワーディングを行い、SOCKSプロキシに対応しているアプリケーションで接続することで、SSHサーバからのみ接続ができるサーバに接続することができる。

  ```powershell
  ssh -D $PROXY_PORT $USER_NAME@$SSHサーバーホスト -i <秘密鍵>
  ```

* SOCKS5プロキシの設定（クライアント端末で設定）

  ![image-20220705083816009](C:\Users\nflabs-03\Documents\git\TIL\TIL\Linux\img\image-20220705083816009.png)

* [参考サイト](https://blog.icttoracon.net/2020/02/21/%E3%83%80%E3%82%A4%E3%83%8A%E3%83%9F%E3%83%83%E3%82%AF%E3%83%9D%E3%83%BC%E3%83%88%E3%83%95%E3%82%A9%E3%83%AF%E3%83%BC%E3%83%87%E3%82%A3%E3%83%B3%E3%82%B0%EF%BC%88socks%E3%83%97%E3%83%AD%E3%82%AD/)