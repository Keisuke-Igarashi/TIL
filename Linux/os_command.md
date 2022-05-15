- カーネルのバージョン確認  
```uname -r```  

- OSの確認方法  
etcディレクトリの下にリリース情報がある場合が多い  
```cat /etc/centos-release```  
詳細情報  
```cat /proc/version```  

- サービス自動起動設定
```bash
systemctl enable mariadb
```