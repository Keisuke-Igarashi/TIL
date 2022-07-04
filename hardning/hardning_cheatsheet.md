# ハードニングチートシート

## 体制役割分担

* OS・アプリ・監視に分ける

## SSHログイン

* bastionサーバーssh

  ```bash
  ssh ec2-user@35.79.107.238 -i .\Documents\team1.pem
  ```

* lampサーバーssh

  ```bash
  ssh admin@10.0.1.100
  ```

## ポート確認

```
nmap ipアドレス
```



## ログ確認

* IDSのログ確認

  ```bash
  # アラートログ閲覧
  tail -f /var/log/suricata/fast.log
  tail -f /var/log/suricata/eve.json
  
  # シグネチャファイル確認
  cat /var/lib/suricata/rules/suricata.rules | grep xxxxx
  ```


* アクセスログの確認

  ```bash
  # アクセスログ確認(httpd)
  cat /var/log/httpd/app_access.log | grep xxxxx
  ```

* セキュアログの確認

  ```bash
  cat /var/log/secure | grep Accepted
  cat /var/log/secure | grep Failed
  cat /var/log/secure | grep invalid
  ```

* ftpログの確認

  * ftpのログイン試行はsecureに記載される

## OS堅牢化

[参考サイト](https://securecompliance.co/linux-server-hardening-checklist/)

* ルートユーザ以外の日常遣いのユーザを作る

  * ユーザ情報の確認

    ```bash
    cat /etc/passwd | grep ftp
    ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    
    # UIDが0になっているユーザがいないか確認する
    awk -F: '($3 == "0") {print}' /etc/passwd
    ```

    * ftp：ユーザ名
    * x ：パスワードを表すからむだが通常は「x」1文字であらわされる。古い環境だと暗号化されたパスワードであることもある
    * 14：ユーザIDが記述される。
    * 50：グループIDが記述される。
    * FTP User：任意のコメント
    * /var/ftp：ユーザのホームディレクトリを指す。ディレクトリが存在しない場合「/」がデフォルトとなる
    * /sbin/nologin：ユーザが仕様するシェルを設定する。ログインをしないユーザーには「/sbin/nologin」が割り当てられる

    [参考サイト](https://eng-entrance.com/linux-user-show)

    ```bash
    useradd ユーザ名
    useradd keisuke
    cat /etc/passwd | grep keisuke
    keisuke:x:1001:1001::home/keisuke:/bin/bash
    
    # オプション指定例
    useradd -s /bin/bash -m ユーザー名
    
    # デフォルト値の確認
    useradd -D
    ```

    | 短いオプション  | 長いオプション          | 意味                                                       |
    | --------------- | ----------------------- | ---------------------------------------------------------- |
    | -m              | --create-home           | ユーザのホームディレクトリが存在しない場合作成する         |
    | -M              | --no-create-home        | ユーザのホームディレクトリを作成しない                     |
    | -b              | --base-dir BASE_DIR     | ホームディレクトリのベースとなるディレクトリ(/home)など    |
    | -d ディレクトリ | --home-dir ディレクトリ | ユーザーのホームディレクトリ(通常はユーザー名と同じにする) |
    | -k ディレクトリ | --skel ディレクトリ     | ひな形ディレクおt理(デフォルトは/etc/skel)を指定する       |

    

    [ほかにもたくさんあったので続きは参考サイトへ](https://atmarkit.itmedia.co.jp/ait/articles/1811/02/news035.html)

    

  + ユーザ情報の登録

    + ubuntuの場合

      ```bash
      adduser
      ```

    * linuxの場合

      ```bash
      useradd
      ```

      ※linuxにもadduserコマンドがあるがuseraddへのシンボリックリンク

      

  + パスワードの登録(root専用コマンド)

    ```bash
    passwd ユーザ名
    
    # passwd設定前
    sudo passwd -S keisuke
    keisuke LK 2022-07-03 0 9999 7 -1 (Password locked.)
    
    # passwdの設定
    sudo passwd keisuke
    ```

    + 「passwd」は、「passwd」のみでユーザ自身のパスワードを変更し、rootユーザは「passwed ユーザ名」で指定したユーザーのパスワード変更が可能

    + [オプション](https://atmarkit.itmedia.co.jp/ait/articles/1612/05/news021.html)

    + -l  ユーザ名 オプション：指定したユーザーをロックする

    + -u ユーザ名 オプション：指定したユーザーのロックを解除する

    + -d ユーザ名 オプション：パスワードを削除する

    + -s ユーザ名 オプション：パスワードの状態を表示する

      

* rootユーザでのログインの無効化

  * rootのログインを無効化するために一般ユーザーにwheelグループという特別グループに追加をしていく

    ```bash
    # 一般ユーザにsudo権限を与える
    sudo su -
    usermod -G wheel ユーザ名
    # 所属グループを確認する
    groups ユーザ名 
    ```

  * rootログインの禁止設定

    ```bash
    # ssh-configの変更
    sudo vi /etc/ssh/sshd_config
    
    # PermitRootLoginを変更
    変更前：# PermitRootLogin yes
    変更後：PermitRootLogin no
    
    # sshdを再起動
    sudo systemctl restart sshd.service
    ```

​			[参考サイト](https://pointsandlines.jp/server-infra/linux-no-root-login)

* 必要のないサービスを停止する(init.d and xinetd)

  * 「/etc/init.d」ディレクトリ

    * デーモンなどの起動スクリプトが設置されているディレクトリ

      ```bash
      # apacheを起動する場合
      /etc/init.d/httpd start
      ```

  * xinetd
    * 詳細はそこまで調べられていないので割愛。サイトの記事を少し読んだので掲載
    * [参考サイト](https://blog.mosuke.tech/entry/2015/01/02/013658/)

* 不要なアプリを利用不可にする(ftp.telnet,x11)

  * vsftpd

    * FTPサーバー

    * Very Secure FTP Daemon

    * CentOSで標準のFTPサーバーとして採用

    * vsftpd.conf (/etc/vsftpd/vsftpd.conf)

    * [参考サイト - Redhatドキュメント](https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/6/html/deployment_guide/s2-ftp-vsftpd-conf)

    * anonymousの設定をOFFにする

      ```bash
      anonymous_enable=NO
      ```

    * 恐らく推奨はされないがポート番号の変更も有効な手段か

      ```bash
      listen_port = 10021
      ```

    * ログインできるユーザを制限

      ```bash
      # userlistを利用する
      userlist_enable=YES
      ```

      * ユーザリストも/etc/vsftpd/にある(デフォルトではブラックリストのはず)

    * [参考サイト](https://jyn.jp/vsftpd-security/)

* firewallをセットアップする

  * [centos - firewalld](https://github.com/Keisuke-Igarashi/TIL/blob/main/Linux/firewall_centos.md)

* IDSとIPSを利用する

* ログファイルのバックアップを取得する

* SELinuxを利用可能にする

* すべてのパスワードに複雑なパスワードを設定する

* すべてのアカウントでパスワードが設定されていることを確認する

  ```bash
  awk -F: '($2 == "") {print}' /etc/shadow
  ```

* rootユーザ以外のアカウントがUIDが０となっていないことを確認する

  ```bash
  awk -F: '($3 == "0") {print}' /etc/passwd
  ```

* SSHのロックダウンを実施する

  [参考サイト](https://hackers-high.com/linux/ssh-config-for-beginners/)

  

  * 公開鍵・秘密鍵認証を用いる

  * rootでのログインを禁止する

  * パスワード認証を禁止する

    ```bash
    # rootでのログイン禁止
    PermitRootLogin no
    
    # 公開鍵認証を有効化
    PubkeyAuthentication yes
    
    # パスワード認証を無効化
    PasswordAuthentication no
    ```

  * 利用しないIPアドレスは無効化する

    * 

  * IPアドレスのホワイトリストを利用する

    * 

  * 2FAを利用可能にする

* すべてのクーロンタスクをchmod700にする