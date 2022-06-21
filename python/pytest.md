# **pytest**

1. ターミナルで仮想環境を切り替える

* poetryの場合
    
    ```bash
    poetry shell
    ```

2. pytestがインストールされていることを確認

* poetryの場合

    ```bash
    poetry show | grep pytest
    ```

    * インストールされていない場合は以下コマンドでインストール

        ```bash
        poetry add pytest
        ```

3. pytestコードの記述

    * ファイル名は'test_'で始める
    * テスト対象の関数をimportで取り込む
    * 何をしているかわかるようtest_テスト対象の関数名"とする
    * assertの後ろにテストしたい式を記述する

## parametrize

* 一括テストの実施時に失敗するケースでテストが終了しないようにするために、pytestではparametrizeを用いる。parametrizeはpytestで利用できるデコレータで、変数と結果をパラメータとしてテストコードに渡すことで、複数のテストを1つのテストコードで実行することができる。

    * テスト対象
    ```python
    def multiple(x, y):
    return x * y
    ```

    * テストコード
    ```python
    import pytest
    from multiple import multiple
    @pytest.mark.parametrize(('x', 'y', 'expected'),[
        (2, 2, 5),
        (2, 3, 6),
        (4, 2, 8)
    ])

    def test_multiple(x,y, expected):
        assert multiple(x, y) == expected
    ```

    * テスト実行

    ```bash
    pytest test_multiple.py -v
    ```

## custom markers

* 特定の試験のみを実行する

* @pytest.mark.[ラベル名]でテストをラベル付けできる

* [公式ドキュメント](https://docs.pytest.org/en/stable/example/markers.html)

* aとラベル付けされたテストのみを実行

    ```bash
    pytest test_custom.py -m "a"
    ```

* aとbの両方のラベルがついているテストのみ実行

    ```bash
    pytest test_custom.py -m "a and b"
    ```

* aとラベル付けされたテスト以外を実行

    ```bash
    pytest test_custom -m "not a"
    ```

## skip

* 特定の関数をスキップする場合は以下ラベル付け

    ```python
    @pytest.mark.skip(reason='Fixing')
    ```

    * 条件を付けてスキップを行う場合

        ```python
        @pytest.mark.skipif(スキップする条件、reason='skipの理由')
        ```

## フィクスチャ

* 再現可能な結果が得られるように、初期状態（前提条件）をセットアップする機能

    * 必要なオブジェクトを生成する
    * ファイル、ネットワーク接続などのリソースを準備する

    * @pytest.fixtureで定義し、テイスト関数でパラメータとして受け取る

    * テストの実行手順。フィクスチャの対象は1.と4.である

        1. **Arrange；テストのための準備**
        2. Act：テスト実行
        3. Assert：結果の確認
        4. **Cleanup：テスト前の状態に戻す**

    * フィクスチャの構造

        ```python
        @pytest.fixture
        def txt():
            # 前処理

            yield ... # テスト関数に何らかの値を渡す

            # 後処理
        ```


        * fixtureの例
        ```python
        import pytest
        import sqlite3
        from get_minage import get_minage

        @pytest.fixture
        def db_con():
            db_con = sqlite3.connect('users.db')

            yield db_con

            db_con.close()

        def test_get_minage(db_con):
            assert get_minage(db_con) == 23
        ```

    * フィクスチャの連携

        * フィクスチャから別のフィクスチャを呼び出せる

        ```python
        import pytest
        import random, string


        from combination import factorial

        # ランダムな文字列の生成fixture
        @pytest.fixture
        def randam():
            yield 'feiifjojfiejifje'

        # 文字列の長さを取得するfixture
        @pytest.fixture
        def length(randam):

            leng = len(randam)
            yield leng

        # テスト関数の呼び出し
        def test_factorial(length):
            assert factorial(length) == 20922789888000
        ```

## テンポラリファイルの作成

* 以下モジュールを利用して/tmpファイルに一時ファイルの作成が可能

    ```python
    @pytest.fixture
    def txt(tmpdir) -> str:
        tmpfile = tmpdir.join('numbers.txt')

        with tmpfile.open('W') as f:
            for n in [2,5,4,3,1]:
                f.write('{}\n'.format(n))

        yield str(tmpfile)

        tempfile.remove()
    ```

    * tmpdirを@pytest.fixtureをデコレータとした関数の引数に渡すと、その関数の内部でtmpdirというオブジェクトが使えるようになる。tmpdirの中身はpy.path.localというオブジェクトである。

    * [tmpdirに関するドキュメント](https://docs.pytest.org/en/stable/reference.html#tmpdir)
    * [py.pathに関するドキュメント](https://py.readthedocs.io/en/latest/path.html)