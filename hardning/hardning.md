# **ハードニング演習**

* Hardning：脆弱性を減らすことでシステムを堅牢（セキュア）にすること

* 衛る=まもる

* ハードニング競技：クローラーが常時監視し適切な対応がなされているかを評価する

* 発生する攻撃

    * App
    * Middle
    * OS

* 評価方法

    * 攻撃結果とWebサイト稼働状況をもとにスコアリング

* 流れ

    * 作戦会議
    * 堅牢化
    * 振り返り

* 演習環境

    ![](/hardning/ensyukankyo.png)

    ![](/hardning/lampenviroment.png)

    * ドメイン名:app.teamN.nflabs
    * admin password ※
    * testuser testpass ※
    ※絶対変えない


    * bastionサーバー：nmapの実行が可能
    
    * lampサーバー：衛るべきサーバー
    
    * スコアサーバー：http://10.0.100.201/score/score.php?game_id=
    
    * みはりサイト
    http://app.team1.nflabs

* コマンドチートシート

    * bastionサーバーssh

        ```bash
        ssh ec2-user@35.79.107.238 -i .\Documents\team1.pem
        ```

    * lampサーバーssh

        ```bash
        ssh admin@10.0.1.100
        ```

    * IDSのログ確認

        ```bash
        


* 対処方法