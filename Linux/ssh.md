- SSH鍵作成
```bash
$ ssh-keygen -t rsa -b 4096
```
-b: 鍵のbit長を指定
RSAの公開鍵暗号では、安全な鍵長は2048bitと定義されているが、
量子コンピューティングにイノベーションが起これば一遍する可能性あり
https://www.nict.go.jp/publication/NICT-News/1303/01.html

- 管理ホストから対象ホストへ公開鍵を転送
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub root@xxx.xxx.xxx.xxx
```
※転送先のホストのユーザの~/.ssh配下にauthorized_keysが生成される

- 管理ホストから対象ホストへssh接続する
```bash
ssh -i ~/.ssh/id_rsa root@192.168.11.103
```
