## windows資格情報
Gitのパスワード等が格納されている。  

検索窓　→　資格情報で検索　→　windows資格情報

## 環境変数の表示
```PowerShell
dir env:path |  % {$_.Value -replace ";","`r`n"}

#コメントで教えていただいた方法を以下に記載。
#その１
$env:Path.Split(";")
```