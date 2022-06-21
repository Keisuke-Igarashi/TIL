# **tcpdump**

* ネットワークトラフィックをダンプ出力する

* [参考サイト](https://xtech.nikkei.com/it/article/COLUMN/20140512/556024/) 

    ```bash
    tmpdump [オプション] [EXPRESSION]
    ```

        * EXPRESSION：条件式。条件式にマッチしたパケットのみ表示する


    ```bash
    sudo tcpdump -i tun0 icmp
    ```

        * -i INTERFACE オプション：ネットワーク・インターフェースを監視する
        * icmp：プロトコルを条件式として指定。icmpのみを表示する

        