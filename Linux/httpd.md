### ソースコードからのダウンロード
- インストールフォルダ
```bash
cd /usr/local/etc
```

- 関連パッケージのインストール
```bash
yum install -y apr-devel apr-util-devel pcre-devel gcc libxml2-devel libcurl-devel libjpeg-devel libpng-devel libXpm-devel libwebp-devel wget unzip perl php73-php-pdo php73-php-mysqlnd
```
これには本当に必要なモノ以外も含まれている

- httpdのソースコードを公式サイトからダウンロード
```
# curl -OL https://dlcdn.apache.org//httpd/httpd-2.4.53.tar.gz
```
その時の最新バージョンを

- ダウロードしたソースコードはファイル圧縮されているため展開
```
# tar -zxvf httpd-2.4.53.tar.gz
```

- 解凍したソースコードのディレクトリへ移動
```
# cd httpd-2.4.53
```

- ソースコードのコンパイルに必要なMakefileの作成
```
# ./configure
```

※Makefileとはmakeコマンドに実行させたい手順を記述したテキスト(文字)
形式のファイル。
参考URL: https://ewords.jp/w/make%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89.html
作成したMakefileを使ってソースコードからのコンパイルとインストールを
実施する
```
# make
# make install
```

インストールが完了したらhttpdのバージョンの確認
```
# ./httpd -v
```

httpd.service をユニットファイル名として作成し編集
```
# vi /etc/systemd/system/httpd.service
```

入力内容
```
[Unit]
Description=The Apache 2.4.53 HTTP Server
After=network.target remote-fs.target nss-lookup.target
[Service]
Type=forking
EnvironmentFile=/usr/local/apache2/conf/httpd.conf
ExecStart=/usr/local/apache2/bin/apachectl -k start
ExecReload=/usr/local/apache2/bin/apachectl -k graceful
ExecStop=/usr/local/apache2/bin/apachectl -k stop
PrivateTmp=true
[Install]
WantedBy=multi-user.target
```

httpdの自動起動を有効化
```
# systemctl enable httpd.service
```
httpdの起動
```
# systemctl start httpd.service
```
httpdのステータスの確認
```
# systemctl status httpd.service
```