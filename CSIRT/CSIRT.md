# **CSIRT**

1. CSIRTとは何か

    * 企業に設置されてサイバー攻撃に対応する組織
    * Computer Security Incident Response Team
    * 形骸化させずきちんと機能かすることが大事
    * インシデント
        * セキュリティインシデント＝セキュリティを脅かす事件、出来事
        * マルウェア感染、DDos攻撃、Webサイト改ざん、フィッシングサイト、スキャン行為など。どれをセキュリティインシデントとするかは組織によって変わる
            * 情報セキュリティインシデント（ＩＳＭＳ）
            * コンピューターセキュリティインシデント（JPCERT/CC)    

2. CSIRTはなぜ必要か

    * インシデント対応を前提とした体制を作るため

        1. インシデント増加
            1. ITの社会インフラ化
            2. グローバルに相互接続されるインターネット
            3. 組織化する攻撃者（サイバー攻撃のビジネス化・政治的メッセージの主張）

        2. インシデントは完全に防げない　→　なにか起きたときに対応しよう
            1. 攻撃手法の巧妙化
            2. 人間はミスをする

        3. インシデント対応が事業継続に必要
            - 業務の生産性への影響（ファイルが暗号化されてしまう）
            - 金銭的な被害
            - 社会的な信頼の低下
            - 自組織のノウハウ流出

3. CSIRTの特徴はなにがあるか

    1. 事故前提であらかじめ準備された体制
        * 従来のシステムは堅牢化で対応をしていた。裏口から侵入される可能性。
        * 完全に防ぐことはできないという考え方
        * 被害を減らすため。事前準備をすることで被害を最小限に

    2. 連携窓口

        【連携】
            * ほかの組織との連携も重要な機能
                
                * FIRST(Forum of Incident Response and Security Teams)
                    - 世界のCSIRTコミュニティ
                * NCA(日本シーサート協議会)
                * 日本のCSIRT
                    * JPCERT/CC（日本代表）
                    * NTT-CERT

        【窓口】
            * 外部からのインシデント報告を受け入れるため
            * 情報を一元的に管理するため

    3. 目的により様々な形態がある
        * 組織内CSIRT
            * 一般的なCSIRT
        * 国際連携CSIRT
            * 他国のCSIRTと連携する機能
            * CC=Cordination Center
        * コーディネーションセンター
            * 各子会社をまとめるような組織。持株など。直接対応は基本しない
        * 分析センター
            * インシデントの傾向分析やマルウェアの解析をし周知する
        * ベンダチーム
            * 自社製品の脆弱性に対してパッチをい作成し、注意喚起を行う
        * インシデントレスポンスプロバイダー
            * インシデント対応を有償で請け負う

    ＣＳＩＲＴ名称
        * CERT(Computer Emergency Response Team)
        * CSIRT(Computer Security Incident Response Team)
        * CIRT(Cyber Incident Response Team)
        * SIRT(Security Incident Response Team) ←Comはここ。コンピューターだけではない

4. CSIRTのサービスは何があるか

    * 啓発やトレーニング

    * 代表的なサービスフレームワーク

        * FIRST CSIRT Service Framework

            * 現代のCSIRTが提供するサービスを構造化したもの
            * FIRSTコミュニティに所属する専門家が作成

            * 構成  [参考サイト](https://www.nca.gr.jp/ttc/first_framework2_1.html)
                * サービスエリア→サービス→機能で定義される
                    ![](/CSIRT/img/%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%82%A8%E3%83%AA%E3%82%A2%E3%81%A8%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9.png)

            * Tips:CSIRTとSOC
                * SOCは検知が中心、CSIRTは対応・調整が中心
                    * 説明したCSIRTのサービスの一部をSOCが担う

            * 5つのサービスエリア
                * 情報セキュリティイベントマネジメント
                * 情報セキュリティインシデントマネジメント
                * 脆弱性管理
                * 状況把握：組織の活動に影響を与える可能性がある事象を識別、分析、理解し、伝達しインシデントを未然に防ぐ
                * 知識移転：組織のセキュリティレベルの向上を目指す

* 参考文献

    * CSIRT - 構築から運用まで- （書籍）
        * http://www.nttpub.co.jp/search/books/detail/100002401.html
    * JPCERT/CC CSIRTマテリアル
        * https://www.jpcert.or.jp/csirt_material/
    * FIRST CSIRT Service Framework
        * https://www.nca.gr.jp/ttc/first_framework2_1.html
    * セキュリティ組織の教科書
        * https://isog-j.org/output/2017/Textbook_soc-csirt_v2.html
    * CERT/CC CSIRTハンドブック（JPCERT/CC訳）
        * https://www.jpcert.or.jp/research/2007/CSIRT_Handbook.pdf

    