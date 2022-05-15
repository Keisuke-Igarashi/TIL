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
