# pwnable BeginnerBof


![](/CTF/img/Beginnerbof_question.png)

* Pwnってこういうのだけじゃないらしいですが，多分これだけでもできればすごいと思います．

```
nc beginnersbof.quals.beginners.seccon.jp 9000
```

* Pwnableとは

PwnableとはCTFのジャンルの1つで、プログラムの脆弱性をつき、本来アクセスできないメモリ領域にアクセスして操作し、フラグを取得する形の問題である。

脆弱性のあるプログラムのある、ないしは実行しているサーバーにsshやncで接続し、
クラックしていく。

## pownableの流れ（詳細セキュリティコンテストよりp468あたり）

1. 動作環境を調べる
2. プログラムの動作を把握する
    * 脆弱性を見つける
3. 脆弱性をつく
    * 緩和機構を回避する
4. 任意の動作を実行させる
    * シェルを立ち上げる
    * ファイルを読みだす


## 実施内容

* とりあえずコマンドを実行してみる。

    * 素直に実行→Helloと返却してくれた
    
    ```bash
    └─# nc beginnersbof.quals.beginners.seccon.jp 9000                                  
    How long is your name?
    10
    What's your name?
    keisuke
    Hello keisuke
    ```

    * 指定した文字数よりも長い文字列を送信→リアクションなし

    ```bash
    └─# nc beginnersbof.quals.beginners.seccon.jp 9000                                  
    How long is your name?
    4
    What's your name?
    keisuke
    ```

これ以上なにもできない。涙