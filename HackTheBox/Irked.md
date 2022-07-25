# Irked

## 偵察

```bash
sudo nmap -sC -sV -O -oA initial 10.10.10.117
```

-sC：　--script=defaultと同様

```bash
PORT    STATE SERVICE    VERSION
22/tcp  open  tcpwrapped
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
80/tcp  open  tcpwrapped
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.10 (Debian)
111/tcp open  tcpwrapped
```

```bash
sudo nmap -p 22,80,111 -sV -oA full-scripts 10.10.10.117
```

-sV：サービスとバージョン情報を表示
-oA：代表的な3フォーマットでファイル出力する

```bash
$ sudo nmap -p 22,80,111 -sV -oA full-scripts 10.10.10.117
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
80/tcp  open  http    Apache httpd 2.4.10 ((Debian))
111/tcp open  rpcbind 2-4 (RPC #100000)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## 調査

* 80ポートをブラウザで

![image-20220725224702807](img/image-20220725224702807.png)

IRC：Internet Relay Chat (サーバーを介してクライアントとクライアントが会話をする枠組み)

* ディレクトリ探索

  ```
  sudo dirb http://10.10.10.117/
  ```

* サービスの脆弱性検索

  ```bash
  └─$ sudo searchsploit OpenSSH 6.7p1
  --------------------------------------------- ---------------------------------
   Exploit Title                               |  Path
  --------------------------------------------- ---------------------------------
  OpenSSH 2.3 < 7.7 - Username Enumeration     | linux/remote/45233.py
  OpenSSH 2.3 < 7.7 - Username Enumeration (Po | linux/remote/45210.py
  OpenSSH < 7.4 - 'UsePrivilegeSeparation Disa | linux/local/40962.txt
  OpenSSH < 7.4 - agent Protocol Arbitrary Lib | linux/remote/40963.txt
  OpenSSH < 7.7 - User Enumeration (2)         | linux/remote/45939.py
  --------------------------------------------- ---------------------------------
  Shellcodes: No Results
  ```

  ```bash
  sudo searchsploit rpcbind 2-4
  Exploits: No Results
  Shellcodes: No Results
  ```

  

明日WriteUP見ながら完成させる。
