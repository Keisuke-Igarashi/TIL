# **ログ分析入門**

* builtinコマンド

    シェルに埋め込まれているコマンドで実行ファイルを必要としない。必要とする場合は、外部コマンドと呼ぶ。bashの場合はbuiltinと入力しタブを3回押すことでコマンドの一覧を確認することが可能。

* 行数のカウント(wc)

    ```bash
    wc -l access.log
    6821 access.log
    ```

* ファイルの先頭を表示(head)

    ```bash
    head <ファイル名>
    head -n 20 <ファイル名>
    ```

* ファイルの末尾を表示(tail)

    ```bash
    tail <ファイル名>
    tail -n 20 <ファイル名>
    tail -f <ファイル名>    //ログをリアルタイムで監視
    less +F　//tail -f と同様の挙動
    ```

* 指定した文字列が含まれる行を抽出（grep)

    ```bash
    grep <検索したい文字列> <ファイル名>
    grep -r <検索したい文字列> <ファイル名> //ディレクトリ配下のすべて
    cat access.log | grep GET //useless use of cat(UUoC)と呼ばれパフォーマンスの観点から非推奨
    ```

    * OR検索

        ```bash
        cat <ファイル名> | grep -e <検索したい文字列> -e <検索したい文字列2>
        ```

    * AND検索

        ```bash
        cat <ファイル名> | grep <検索したい文字列> | grep <検索したい文字列2>
        ```

    * NOT検索

        ```bash
        cat <ファイル名> | grep -v <検索したい文字列>
        ```

* awkコマンド

    * access.logの各行を空白で区切り、1番目の文字列を表示（IPの表示）

        ```bash
        cat access.log | awk '{print $1}'
        ```

    * access.logの各行を.で区切り、3番目の文字列を表示（第三オクテットの表示）

        ```bash
        cat access.log | awk -F "." '{print $3}'
        ```

    * access.logの各行を空白で区切り、6番目から8番目の文字列を表示

        ```bash
        cat access.log | awk '{print $6,$7,$8}'
        ```

    * access.logの各行を[と]で区切り、2番目の文字列を表示（時刻の表示）

        ```bash
        cat access.log | awk -F '[[¥]]' '{print $2}'
        ```

            * -Fオプションで複数文字列を指定する場合は、"[]"でくくる
            * []を指定したい場合は、後ろの]が検索文字であることを示すために、\でエスケープする必要がある点に注意
            * []で区切るので、[]よりも前は$1、[]より後は$3、該当箇所が$2となる

    * access.logのGETを含む各行を“GET”と“HTTP"で区切り、2番目の文字列を表示（GETリクエストのパスの表示）
    
        ```bash
        cat access.log | grep GET | awk -F 'GET|HTTP' '{print $2}'
        ```

        * |で区切ることでOR指定ができる

* cutコマンド

    ```bash
    cat <ファイル名> | cut -d "<区切り文字>" -f <何番目>
    cat <ファイル名> | cut -d "<区切り文字>" -f n-m //n番目-m番目
    cat <ファイル名> | cut -d "<区切り文字>" -f n- //n番目以降
    ```

* sortコマンド

    ```bash
    cat <ファイル名> | LC_COLLATE=C sort
    cat access.log | sort -n    //左側の文字から数値として並び変える
    ```

    * ※ sortは環境変数（LC_COLLATE）によって結果が異なる
    * LC_COLLATEをC（デフォルトロケール）とする

* 文字列の重複削除、カウント(uniq)

    * ログ分析においては、cut、sort、uniqをパイプで繋ぐことで、特定の情報の重複を排除したリストを作ることができる

    ```bash
    cat access.log | cut -d " " -f 1 | sort -n | uniq
    ```

    * !!!!!!!!!!!!sort -n してからuniqにしないといけない。塊の重複をなくすという動作のため。同じ文字で2つ以上の塊があった場合に重複削除をキチンとできない!!!!!!!!!!!

    * access.logから通信元のIPアドレスが多い順のリストを作る

        ```bash
        cat access.log | cut -d " " -f 1 | sort -n | uniq -c | sort -nr
        ```
        * uniq に-c を付けることで出現回数を出力
        * さらにsort を繋ぐことで、出現回数順に並び替え
        * sortに-r を付けると逆順に表示
        * 特定の情報のランキングを作ることができる

* 結果を標準出力しつつファイルへ出力（tee）

    ```bash
    ```

    * access.logから通信元のIPアドレスが多い順のリストを作る際に、cutコマンドとsortコマンドがどのような出力をしているかを調べる
        
        ```bash
        cat access.log | cut -d " " -f 1 | tee cut.log | sort -n |
        tee sort.log | uniq -c | sort -nr
        ```

* awkとcutの違い

    確かにcutコマンドではfromという単語が出ていますね
    リダイレクトを用いて次の3つのファイルを作成し、どの部分で変化が出ているのかを調べてみましょう。
    cat auth.log | grep "Invalid user" > orig
    cat auth.log | grep "Invalid user" | cut -d " " -f 10 > cut
    cat auth.log | grep "Invalid user" | awk '{print $10}' > awk
    そして、cutとawkの差分を調べてみます
    $ diff awk cut -u | less
    --- awk   2022-06-29 13:01:36.317547714 +0900
    +++ cut   2022-06-29 13:01:47.901134323 +0900
    @@ -83,3193 +83,3193 @@
    83行目周辺で変化があるようです。origの83行目を見てみましょう。（diffコマンドは変化がない部分も含めて表示します。実際に変化があるのは86行目です。）
    $ sed -n 83,93p go
    Nov 30 23:59:15 ip-172-31-27-153 sshd[23376]: Invalid user xbian from 221.208.245.210
    Nov 30 23:59:19 ip-172-31-27-153 sshd[23380]: Invalid user pi from 221.208.245.210
    Nov 30 23:59:21 ip-172-31-27-153 sshd[23382]: Invalid user vyatta from 221.208.245.210
    Dec  1 01:04:03 ip-172-31-27-153 sshd[23430]: Invalid user support from 180.210.234.87
    Dec  1 01:04:07 ip-172-31-27-153 sshd[23432]: Invalid user operator from 180.210.234.87
    Dec  1 01:04:12 ip-172-31-27-153 sshd[23436]: Invalid user cgi from 180.210.234.87
    Dec  1 01:04:18 ip-172-31-27-153 sshd[23440]: Invalid user ftpuser from 180.210.234.87
    Dec  1 01:04:42 ip-172-31-27-153 sshd[23457]: Invalid user oracle from 180.210.234.87
    Dec  1 01:04:45 ip-172-31-27-153 sshd[23459]: Invalid user nagios from 180.210.234.87
    Dec  1 01:04:47 ip-172-31-27-153 sshd[23461]: Invalid user svn from 180.210.234.87
    Dec  1 01:04:52 ip-172-31-27-153 sshd[23463]: Invalid user mysql from 180.210.234.87
    Dec 1が怪しいことが分かりました。確かに、Dec 1はスペース文字が2文字入っていますね。
    このことから、awkコマンドは連続した空白を取り出していることが分かります。一方で、cutコマンドは一文字のみを取り出していることが分かります。


* ログの概要を調査
    1. 記録されているログの件数

    ```bash
    student@it-system:~/Desktop/work/Server$ cat access.log | wc -l
    6821
    ```

    2. ログが記録されている期間の最初の時間と最後の時間

    * 最初の時間
        
        ```bash
        student@it-system:~/Desktop/work/Server$ head -n 1 access.log | awk '{print $4}'
        [21/Oct/2021:02:31:20
        ```

    * 最後の時間

        ```bash
        student@it-system:~/Desktop/work/Server$ tail -n 1 access.log | awk -F '[[\]]' '{print $2}'
        27/Oct/2021:02:33:03 -0700
        ```

* ログ全体の特徴を調査

    3. 最も記録されているUser-Agent

        ```bash
        student@it-system:~/Desktop/work/Server$ cat access.log | cut -d " " -f 12- | uniq -c | sort -rn | head -n 10
        195 "Mozilla/5.0 (compatible; VSAgent/2.0)" 
        113 "python-requests/2.26.0" 
        82 "Mozilla/5.0 (compatible; Adsbot/3.1; +https://seostar.co/robot/)" 
        73 "Mozilla/5.0 (compatible; AhrefsBot/7.0; +http://ahrefs.com/robot/)" 
        71 "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.109 Safari/537.36" 
        68 "Mozilla/5.0 (compatible; AhrefsBot/7.0; +http://ahrefs.com/robot/)" 
        65 "Mozilla/5.0 (compatible; BLEXBot/1.0; +http://webmeup-crawler.com/)" 
        60 "Mozilla/5.0 (compatible; AhrefsBot/7.0; +http://ahrefs.com/robot/)" 
        59 "Mozilla/5.0 (compatible; AhrefsBot/7.0; +http://ahrefs.com/robot/)" 
        54 "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.87 Safari/537.36" 
        ```

    4. HTTPステータスコードが404のうち、アクセスが多いパス

        ```bash(sortしてからuniq -cする必要がある。繰り返されているところだけ重複削除になるため)
        student@it-system:~/Desktop/work/Server$ cat access.log | grep 404 | cut -d ' ' -f 6-7 | uniq -c | sort -rn | head -n 5
        20 "GET /css/style.css HTTP/1.1"
        7 "GET /css/style.css HTTP/1.1"
        7 "GET /css/style.css HTTP/1.1"
        5 "GET /css/style.css HTTP/1.1"
        5 "GET /css/style.css HTTP/1.1"
        ```

        * 正解(grep 404をする前にawkで絞り込んだ方が良かったかも)
        ```bash
        cat access.log | awk '{print $9,$7}' |grep 404|awk '{print $2}'|sort|uniq -c|sort -rn
        ```

* 特定の通信が記録されたログを調査

    5. 37.0.11[.]67からアクセスがあった時間

        ```bash
        student@it-system:~/Desktop/work/Server$ cat access.log | grep 37.0.11[.]67 | cut -d ' ' -f 4-5
        [26/Oct/2021:07:15:40 -0700]
        [26/Oct/2021:07:15:50 -0700]
        ```

* ログの概要を調査
    1. 記録されているログの件数(≠認証試行の回数)

        ```bash
        student@it-system:~/Desktop/work/Server$ wc -l auth.log 
        86839 auth.log
        ```

    2. ログが記録されている最初の時間と最後の時間

        ```bash
        student@it-system:~/Desktop/work/Server$ head -n 1 auth.log 
        Nov 30 06:39:00 ip-172-31-27-153 CRON[21882]: pam_unix(cron:session): session closed for user root
        student@it-system:~/Desktop/work/Server$ tail -n 1 auth.log 
        Dec 31 22:27:48 ip-172-31-27-153 sshd[8003]: Connection closed by 218.2.0.133 [preauth]
        ```

        解答
        ```bash
        head auth.log -n 1 ; tail auth.log -n 1
        ```


* ログ全体の特徴を調査

    3. 無効なユーザの認証が行われた回数

        ```bash
        student@it-system:~/Desktop/work/Server$ cat auth.log | grep "Invalid user" | wc -l
        12250
        ```

    4. ログインに失敗したユーザのTop10

        ```bash
        student@it-system:~/Desktop/work/Server$ cat auth.log | grep "Invalid user" | awk '{print $ 8}' | sort | uniq -c | sort -rn | head -n 10
        1939 admin
        933 test
        515 guest
        449 oracle
        418 ftp
        392 ftpuser
        340 nagios
        339 D-Link
        335 debug
        327 log
        ```

    5. ログインに失敗したIPアドレスのTop10

        ```bash
        student@it-system:~/Desktop/work/Server$ cat auth.log | grep "Invalid user" |cut -d " " -f 8-10 | sort | uniq -c | grep -v user | sort -rn | head -n 10
        96 test from 188.87.35.25
        86 test from 78.129.223.28
        65 nagios from 188.87.35.25
        46 zabbix from 188.87.35.25
        46 admin from 222.161.209.92
        45 guest from 188.87.35.25
        32 nagios from 78.129.223.28
        32 guest from 78.129.223.28
        25 ftp from 222.161.209.92
        22 oracle from 222.161.209.92
        ```

        解答(あとで復習(★))
        ```bash
        grep "Invalid user" auth.log | awk '{print $10}' | sort -n | uniq -c | sort -nr | head -n 10
        ```

## ネットワークログ(WireShark)

* wiresharkでpcapの情報を参照する
　
    ```bash
    wireshark dns_http.pcap
    ```

    ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/wireshark_packet.png)

* 「表示-時刻表示形式」から時間のフォーマット変更をすること

* パケットフィルタ

    * 一覧表をここに作成する(★)

    ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/wireshark_filter.png)
    ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/wireshark_filter2.png)

* 対象のレコードを右クリック→追跡→TCPストリームなどで特定のpayloadsを取り出すことができる

    ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/wireshark_stream.png)

        * TCPのセッションが複数ある場合は、Streamのプルダウンで切り替えることが可能。ストリームのプルダウンを切り替えると裏で対象のパケット一覧がwireshark上に表示される。

        * TCPストリームでエンコードされていた場合、HTTPストリームではデコード後の通信内容を表示することができる。



* パケットからファイルを抽出

    * ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/wireshark_export.png)

* Statistic(統計)

    * Resolved Addresses（解決したアドレス） ：DNSで名前解決されたアドレスを表示。たくさん出るので目的のIPなどでフィルタすること
    * Protocol Hierarchy（プロトコル階層）：利用されているプロトコル情報を階層で表示可能。調査の最初に実施して利用されているプロトコルを網羅的に確認すること
    * Conversations(対話)：対象のpcapがどの端末同士で通信されているか。調査の最初に確認する
    * endpoints(終端):IPアドレスごとの表示


* pcap：ftp.pcap
    * クライアントのIPアドレス

        192.168.22.132

    * 送信、もしくは受信しているファイルの内容

        sample_put.html

* pcap：ssh.pcap

    * クライアントのIPアドレス
 
         192.168.22.132

    * 通信の開始時間と終了時間

        * 3wayhandshake

* pcapを解析して、どのような攻撃が実行されているか分析してください
* 分析する項目は以下です
    * 攻撃名:SQLインジェクション
    * 攻撃を行っているIPアドレス：192.168.22.135
    * 攻撃された時間（日本時間）: Date: Mon, 31 Jan 2022 08:37:27 GMT
    * 攻撃の成否: 成功   
    * 攻撃が成功している場合は窃取された情報：ユーザIDとパスワード

* 攻撃が分析できた方はアクセスログと比較して、取得できる情報の違いを
確認してください
    * pcap：1.pcap
    * アクセスログ：1.log
    アクセスログはメソッドやGETでのSQLの痕跡のみしか確認できない


* 分析する項目は以下です
    * 攻撃名：OSコマンドインジェクション
    →webshellが正解。別途学習する。（'https://tex2e.github.io/blog/linux/selinux-and-php-backdoor')
    * 攻撃を行っているIPアドレス：192.168.22.135
    * 攻撃された時間（日本時間）:
    17:49:48
    * 攻撃の成否：成功
    * 攻撃が成功している場合は窃取された情報：ncコマンドのディレクトリ。その後192.168.22.129に対してポート5567でサービスが張られており、この後バッシュ操作等される可能性あり。なおwww-dataユーザは/bin/shの実行権限あり

    *  phpでサーバーが起動している場合、phpのsystem()関数でコマンド実行ができる。

        ```php
        # Take input from the url paramter. shell.php?cmd=whoami
        <?php system($_GET['cmd']); ?>
        ```

            * $_GET['<パラメータ名>']でクエリから値を取得し、system()関数に渡す
            * system関数の説明：https://www.php.net/manual/ja/function.system.php
            * 参考にしたサイト：'https://sushant747.gitbooks.io/total-oscp-guide/content/webshell.html'

* 攻撃が分析できた方はアクセスログと比較して、取得できる情報の違いを
確認してください
    * pcap：2.pcap
    * アクセスログ：2.log

    ログからだと実行されたコマンドしかわからない

* 分析する項目は以下です
    *攻撃名：log4j
    *攻撃を行っているIPアドレス:172.17.0.1
    *攻撃が始まった時間:14:17:25
    *攻撃の成否:？？
    *攻撃が成功している場合は窃取された情報：exploit.classが被害者端末からGETリクエストされている
* 攻撃が分析できた方はアクセスログと比較して、取得できる情報の違いを
確認してください
    * pcap：3.pcap
    * アクセスログ：3.log

    * https://blogs.jpcert.or.jp/ja/2021/12/log4j-cve-2021-44228.html



## IDSログ分析

* OSSであるsuricataを利用

* suricataの実行コマンド

    ```bash
    sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
    ```

* fast.logから攻撃を特定する。

    ```bash
    cat fast.log
    ```

    ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/fastlog.png)

    * fast.logは実行した際のカレントディレクトリに作成される。なので必ず調査対象の.pcapファイル直下で実行するなりすること。

* シグネチャを確認する。

    * suricataのシグネチャが登録されているルールファイルの場所

        ```bash
        /var/lib/suricata/rules/suricata.rules
        ```

        * シグネチャIDで絞り込みして確認
        ```bash
        sudo cat /var/lib/suricata/rules/suricata.rules | grep "sid:2006546"
        ```

    * シグネチャ内容の確認

        * シグネチャのフォーマット

            ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/signature_format.png)

            * action：シグネチャが一致した時の動作を定義  
            * Header：確認する通信のプロトコルやIPアドレス、ポート番号、通信の方向を定義
            * Options: ルールの詳細を定義

        * Actionの種類

            |アクション|説明|
            ----|----
            |alert|アラートを生成|
            |pass|パケットの検査を中止し通信を許可|
            |drop|パケットを破棄し、アラートを生成|
            |reject|一致したパケットの送信者にTCP RSTもしくはICMP ureachable errorを送信|

            * IDSは検知のみのため基本alertのみ使用する
            * IPSとしての運用は遮断等も行う

        * Headerの詳細

            * 対応しているプロトコル

                * tcp/udp/icmp/ip
                * L7プロトコル：http,dns,smb,rdp

            * source and destination

                * $HOME_NET：自分のネットワークの範囲を示す
                * $EXTERNAL_NET：自分以外のネットワークの範囲をします

                |演算子|説明|例|
                ----|----|----
                |.../...|IPアドレスの範囲(CIDR表記)|192.0.2.0/24|
                |!|否定/例外|!192.0.2.1|
                |[..., ...]|グループ|[192.0.2.1, 192.0.2.2]|

            * ports

                * ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/signature_header_port.png)

            * Direction

                ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/signature_header_direction.png)

        * Optionsの詳細

            * format

                ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/signature_option_format.png)

            * keywordの種類

                ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/signature_options_keyword.png)

    * suricata.rulesの公式ドキュメント

        - 'https://suricata.readthedocs.io/en/suricata-6.0.2/rules/meta.html'
        - 'https://suricata.readthedocs.io/en/suricata-6.0.2/rules/payload-keywords.html'
        - 'https://dev.classmethod.jp/articles/networkfirewall-suricata/#toc-5'

    

    * 演習

    * 検知したシグネチャ名
        ET WEB_SPECIFIC_APPS [PT OPEN] Drupalgeddon2

    * 行われている攻撃の種類や対象のシステムを調査（Google等を活用）
        CVE-2018-7600
        https://jvndb.jvn.jp/ja/contents/2018/JVNDB-2018-003541.html
        https://www.intellilink.co.jp/column/vulner/2018/042400.aspx


    * 攻撃を行ってるホスト（IPアドレス、ポート番号）
    * 攻撃を受けているホスト（IPアドレス、ポート番号）
    
        54.93.114.243:58639 -> 172.20.0.2:80


## インシデントログ分析演習

* webサーバのIPアドレス：172.16.10.11
    
    * Apache 2.4.6 + PHP 7.4.16 + WordPress 5.7.1
    * wordPressにはプラグインをインストール

        * Contact Form 7 (6.4):'https://ja.wordpress.org/plugins/contact-form-7/'
        * WP File Manager(6.0):'https://ja.wordpress.org/plugins/wp-file-manager/'

* dbサーバーのIPアドレス：172.16.10.12
* jumpサーバー：172.16.10.101
* Local Maintenance :172.16.10.1

* Firewall

|SrcAddr|SrcPort|Dest Addr|DestPort|Action|備考|
----|----|----|----|----|----
|1|\*|\*|172.16.10.101|22|ACCEPT|メンテナンス用|
|2|\*|\*|172.16.10.11|80|ACCEPT|Webサーバ用|
|99|\*|\*|\*|\*|DROP|マッチしなかったらDrop|

* インシデント分析の流れ

    1. アラートを出したIDSログの分析

        * アラートごとに以下をまとめる

            * 最後にアラートが発生した時間
            * 通信元、通信先
            * シグネチャ名
            * ピックアップした理由（何がセキュリティ上の問題なのか）

        * fast.log：簡易なアラートの情報
        * eve.json：JSON形式の詳細なアラート情報
            EveBoxを用いて解析していく（演習ではlubuntu上にコンテナ起動して利用)

            ```bash
            CONTAINER ID   IMAGE                                                 COMMAND                  CREATED        STATUS                    PORTS                                                 NAMES
            82cf46a141ec   jasonish/evebox                                       "/docker-entrypoint.…"   3 months ago   Exited (0) 3 months ago                                                         adoring_hugle
            2d8ef82e9526   jasonish/evebox                                       "/docker-entrypoint.…"   3 months ago   Up 28 hours               0.0.0.0:5636->5636/tcp, :::5636->5636/tcp             evebox
            55545a21a123   docker.elastic.co/elasticsearch/elasticsearch:8.1.0   "/bin/tini -- /usr/l…"   3 months ago   Up 28 hours               0.0.0.0:9200->9200/tcp, :::9200->9200/tcp, 9300/tcp   elasticsearch
            ```

        * 分析のヒント

            * フィルタを利用して絞り込む
            * 通常の通信と比べる
            * 攻撃の流れを意識する

                * スキャン後に攻撃が来る傾向が多い。スキャンはフィルタしてもよい。

            * 通信の結果を確認する

                * 404は省ける。20x,30x系を特に確認したい。

            * 環境情報を思い出す

        * EveBox

            ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/eve_box.jpg.png)

            * inbox：ここで不要なものをarchiveする
            * Escalated：お気に入りに入れたものだけ表示する。ここに解析用のログを貯める
            * Alert：全量のログが見える
            * Events：1つ1つ表示する
            * Reports：統計（レポート）を表示する



        


    2. インシデントの概要を推定
    3. インシデントを時系列で詳細分析

        * SSHログの調査

            * 成功：Accepted
                
                ```bash
                grep Accepted 2021-04-20_jump_secure.log
                ```

            * 失敗：Failed/invalid
                
                ```bash
                cat 2021-04-20_jump_secure.log | grep Failed
                ```

            * **プロセスIDを取得しておき、sshログで日時（開始-終了)時間を調査する**

                ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/ssh_processid.png)

    
        * ディスクイメージ（今回はmountで確認）

            * mount

            ```bash
            $ sudo mkdir /mnt/jump
            $ sudo mount -o ro,noexec,nodev,offset=2793406464
            /dev/sdb /mnt/jump
            $ sudo mkdir /mnt/web
            $ sudo mount -o ro,noexec,nodev,offset=1074790400
            /dev/sdc /mnt/web
            ```

                * -o: オプション
                    * ro: 読み取り専用
                    * noexec: バイナリの実行を不許可（マルウェアの誤実行を禁止）
                    * nodev: スペシャルデバイスファイルのオープンを不許可
                    * offset: 読み込む位置（バイト）

        * Linuxで調査時に確認するファイル

            ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/loganalys_linuxfile.png)

        * fileのタイムスタンプ

            ![](/%E3%83%AD%E3%82%B0%E5%88%86%E6%9E%90%E5%85%A5%E9%96%80/img/linux_file_mac.png)

        * tab補完に関して質問

                    
            五十嵐 恵介
            6時間前
            @sensei
            1点質問させてください。
            sudo cat root/.ba
            等と入力し、tab補完しようと思ったのですがされませんでした。.で始まるファイルはtab補完が聞かない等あるのでしょうか？もしわかれば教えていただきたいです。
            ブックマークに追加しました


            Yusuke Nishida
            6時間前
            シェルはユーザ権限で動いています。/rootはその他のユーザに権限が与えられていないため、ユーザ権限で動いているシェルは/rootの中身を見ることができません。
            タブキーによる補完はシェルが行っているため、タブ補完が効かないものと思われます。したがって、
            sudo -i
            でシェルをroot権限で動かせば、タブ補完はできるものと思います。
            :benkyouninarimasu:
            1

            ブックマークに追加しました


            Yusuke Nishida
            6時間前
            例えば、cd はビルトインコマンドです。
            $ type cd
            cd はシェル組み込み関数です
            cdコマンドを用いて/root へアクセスしようとすると、アクセスできません。
            $ cd /root
            bash: cd: /root: 許可がありません
            当然、lsもできません。
            $ ls /root
            ls: ディレクトリ '/root' を開くことが出来ません: 許可がありません
            したがって、タブ補完しようにもシェルがファイル一覧を取得できないのです。
            sudoを付けているのだから行けるのでは？と思われるかもですが、sudoは外部コマンドです。
            $ type sudo
            sudo はハッシュされています (/usr/bin/sudo)
            また、lsも外部コマンドです。
            $ type -a ls
            ls は `ls --color=auto' のエイリアスです
            ls は /usr/bin/ls です
            ls は /bin/ls です
            したがって、sudoは/usr/bin/sudo を起動しているだけであり、sudo lsも/usr/bin/sudo /usr/bin/ls を実行しているだけです。一方で、cdやタブキーによる補完はシェルの機能なため、外部コマンドではないです。ゆえに、sudoのパラメータとして渡すこともできません。
            ゆえに、シェルがアクセス可能な権限しかタブ補完はできないのです