# ネットワーク設定があっているのにVMにpingやsshできないとき
```bash
sudo systemctl restart NetworkManager
```

# ARP Table 
```ip neigh```

# Routing Table
```ip route```

# IP 
```ifconfig```  
```ip a```  
```ping```  
目的のホストまでの経路を調査する  
```traceroute```   
```nslookup```  
```dig```  
```host```  
TCP/IPの通信状態・ポート番号を確認する  
```netstat```  
　　
　　
　　
  
**※グローバルIPはネットで検索できる**

# Setting Hosts Network
CUI Network Manager Command Line Interface  
```nmcli```  
- デバイスの設定内容確認  
```nmcli device```  
- インターフェース有効化(インターフェースとデバイスの自動接続を有効にする)
```nmcli connection modify ensXX connection.autoconnectyes```  

**デバイス(device)：物理インターフェース。ＮＩＣ分**  
**コネクション(connection)：論理インターフェース。ネットワークの集合体**  

- IPアドレスの変更/サブネットマスクの設定  
nmcli connection modify ensXX ipv4.method manual ipv4.address xxx.xxx.xxx.xxx/xx  
- デフォルトゲートウェイの設定  
nmcli connection modify ensXX ipv4.gateway xxx.xxx.xxx.x  
- DNSの設定  
nmcli connection modify ensXX ipv4.dns x.x.x.x  



GUI/TUI  
```nmtui```  
もしくは以下ファイルの修正
```/etc/sysconfig/network-scripts/ifcfg-ensXX  


## Ping Sweep
1. ①シェルスクリプトを利用した方法
```bash
for i in {1..254}; do ping xxx.yyy.zzz.$i -c 1 -W | grep ttl | cut -d'' -f 4 | tr -d ':' >> ip_list.txt & done
```

2. fpingを利用した方法
```bash
fping -r 1 -g xxx.yyy.zzz.0/24 | grep alive > fping.txt
```

## ping

- オプション
  - -v verbose
  - -i interval
  - -4 ipv4only

## whois

会社情報やドメインを登録した組織の情報などを確認できる。

-[web whois](https://www.whois.com/whois/)

```bash
whois microsoft.com
```

※OSINTなんかでもよく使うみたい

## dig

```bash
dig <domain> @<dns-server-ip>
```

## プロセスが動いているポート確認

```bash
lsof -i
```

# nc(netcat)

TCPやUDPのエコーサーバーを起動できる。ncatコマンドのシンボリックリンクとなっている。

接続先のサーバーで9000番でTCPまたはUDPポートが開いている前提で
```
nc beginnersbof.quals.beginners.seccon.jp 9000
```

# ネットワークインターフェース

コンピューターがネットワーク上にあるほかのコンピューターと通信する際に、パケットをやりとりする「境界」となるリソースのこと。

# ifconfig

'https://kotaro7750.hatenablog.com/entry/linux_nic'

* tun0, tun1：VPN接続している際、トンネリング情報として表示される。


```bash
┌──(kali㉿kali)-[~]
└─$ ifconfig                                                                                            
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:ffff:fe69:5903  prefixlen 64  scopeid 0x20<link>
        ether 02:42:ff:69:59:03  txqueuelen 0  (イーサネット)
        RX packets 8996  bytes 365688 (357.1 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 9018  bytes 31239191 (29.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.22.131  netmask 255.255.255.0  broadcast 192.168.22.255
        inet6 fe80::20c:29ff:feef:9680  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:ef:96:80  txqueuelen 1000  (イーサネット)
        RX packets 189  bytes 38132 (37.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 225  bytes 39148 (38.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 18  base 0x2000  

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.11.131  netmask 255.255.255.0  broadcast 192.168.11.255
        inet6 fe80::20c:29ff:feef:9676  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:ef:96:76  txqueuelen 1000  (イーサネット)
        RX packets 883217  bytes 368020531 (350.9 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1145061  bytes 338260609 (322.5 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (ローカルループバック)
        RX packets 120  bytes 39472 (38.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 120  bytes 39472 (38.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
        inet 10.18.86.217  netmask 255.255.128.0  destination 10.18.86.217
        inet6 fe80::bb7b:f76b:f91b:22b1  prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 500  (不明なネット)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 5  bytes 240 (240.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

tun1: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
        inet 10.18.86.217  netmask 255.255.128.0  destination 10.18.86.217
        inet6 fe80::139b:d099:4641:78ec  prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 500  (不明なネット)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2  bytes 96 (96.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

## portを解放するコマンド

```bash
┌──(venv)(kali㉿kali)-[~/Documents/git/myarchitecturaljourney_igarashikeisuke/Docker]
└─$ sudo lsof -i:3306
COMMAND    PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
mariadbd 91496 mysql   20u  IPv4 308075      0t0  TCP localhost:mysql (LISTEN)

┌──(venv)(kali㉿kali)-[~/Documents/git/myarchitecturaljourney_igarashikeisuke/Docker]
└─$ sudo kill 91496
```