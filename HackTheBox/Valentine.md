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

  
