# Shocker

* 疎通確認

  ```bash
  └─$ ping 10.10.10.56
  PING 10.10.10.56 (10.10.10.56) 56(84) bytes of data.
  64 bytes from 10.10.10.56: icmp_seq=9 ttl=63 time=219 ms
  ```

* ディレクトリ作成

  ```bash
  ┌──(kali㉿kali)-[~/Documents/HTB/Shocker]
  └─$ pwd                                                            
  /home/kali/Documents/HTB/Shocker
  ```

* nmap実施

  ```bash
  nmap -sV 10.10.10.56                                       
  Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-05 19:57 JST
  Nmap scan report for 10.10.10.56
  Host is up (0.19s latency).
  Not shown: 998 closed tcp ports (conn-refused)
  PORT     STATE SERVICE VERSION
  80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
  2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
  Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
  
  ```

* httpにアクセスしてみる。（かわいい）

  ![image-20220705195956365](C:\Users\nflabs-03\Documents\git\TIL\TIL\HackTheBox\img\image-20220705195956365.png)

* Apache httpd 2.4.18の脆弱性を確認する

  ```bash
  $ searchsploit Apache httpd 2.4.18
  Exploits: No Results                                                     
  Shellcodes: No Results 
  ```

* OpenSSHの脆弱性を確認する

  ```bash
  earchsploit OpenSSH 7.2p2
  ------------------------------------------------ ---------------------------------
   Exploit Title                                  |  Path
  ------------------------------------------------ ---------------------------------
  OpenSSH 2.3 < 7.7 - Username Enumeration        | linux/remote/45233.py
  OpenSSH 2.3 < 7.7 - Username Enumeration (PoC)  | linux/remote/45210.py
  OpenSSH 7.2p2 - Username Enumeration            | linux/remote/40136.py
  OpenSSH < 7.4 - 'UsePrivilegeSeparation Disable | linux/local/40962.txt
  OpenSSH < 7.4 - agent Protocol Arbitrary Librar | linux/remote/40963.txt
  OpenSSH < 7.7 - User Enumeration (2)            | linux/remote/45939.py
  OpenSSHd 7.2p2 - Username Enumeration           | linux/remote/40113.txt
  ------------------------------------------------ ---------------------------------
  Shellcodes: No Results
  ```

  * Username Enumerationできても特に有用なことがなさそうなので、UsePrivilegeSeparation DisableについてPocを試してみたい。
    上記の機能が有効に働かないことでroot権限を奪取することができるみたい。
  * [Exploit-DB](https://www.exploit-db.com/exploits/40962)
  * 

## 所感

* nmapを-sV＋-p-でやるとものすごく時間かかるのでやめる。