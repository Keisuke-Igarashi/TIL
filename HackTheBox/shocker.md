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
  nmap -sV -p- 10.10.10.56
  ```

  