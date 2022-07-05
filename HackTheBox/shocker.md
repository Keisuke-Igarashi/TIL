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

* 

## 所感

* nmapを-sV＋-p-でやるとものすごく時間かかるのでやめる。