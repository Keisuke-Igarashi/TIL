# Sunday

rating:1.4 → 異様に低いけどなぜ

## 調査

* ping確認

```
ping 10.10.10.76
PING 10.10.10.76 (10.10.10.76) 56(84) bytes of data.
64 bytes from 10.10.10.76: icmp_seq=1 ttl=254 time=75.7 ms
```

* nmap

  ```bash
  nmap -sV 10.10.10.76
  Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-21 09:29 JST
  Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
  Nmap done: 1 IP address (0 hosts up) scanned in 3.71 seconds
  ```

  ```bash
  nmap --script vuln -oA vuln-scan 10.10.10.76
  Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-21 09:30 JST
  Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
  Nmap done: 1 IP address (0 hosts up) scanned in 13.72 seconds
  ```

  

  * nmapスキャンがブロックされる（ステルススキャンを使ってみる）

  ```bash
  udo nmap -sS 10.10.10.76
  [sudo] kali のパスワード:
  Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-21 09:36 JST
  Nmap scan report for 10.10.10.76
  Host is up (0.10s latency).                                                           
  Not shown: 997 closed tcp ports (reset)                                               
  PORT    STATE SERVICE
  79/tcp  open  finger
  111/tcp open  rpcbind
  515/tcp open  printer
  ```
  
  

