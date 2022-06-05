# wireshark

## フィルターの方法

'https://www.infraexpert.com/info/wireshark5.html'

IPアドレスを含む
```
ip.addr == xxx.xxx.xxx.xxx
```

送信元IPアドレスを指定
```
ip.src == xxx.xxx.xxx.xxx
```

宛先IPアドレスを指定
```
ip.dst == xxx.xxx.xxx.xxx
```

TCP ポート番号
```
tcp. port == 80
```