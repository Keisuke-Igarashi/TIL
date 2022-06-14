# **Flask**

## インストール可能なプロジェクトの作成

* setup.py：プロジェクトとプロジェクトに所属するファイルの記述

    ```python
    from setuptools import find_packages, setup

    setup(
        name='flaskr',      
        version='1.0.0',
        packages=find_packages(),
        include_package_data=True,  # staticやtemplateの登録に必要
        zip_safe=False,
        install_requires=[
            'flask',
        ],
    )
    ```

* MANIFEST.in：include_package_dataが何かを伝えるファイル

    ```python
    include flaskr/schema.sql   # sqlを含める
    graft flaskr/static         # staticディレクトリにあるすべてを含める
    graft flaskr/templates      # templateディレクトリにあるすべてを含める
    global-exclude *.pyc        # 除外
    ```
    * https://packaging.python.org/tutorials/packaging-projects/

<br />

* プロジェクトのインストール

    いまのディレクトリからsetup.pyを見つけ出し、それを編集可能(editable)または開発(development)モードでインストールするように伝えるコマンド。編集可能モードは、ローカルのコードを変更したときに、プロジェクトに関するメタデータを変更した場合だけ再インストールが必要なようにする設定。

    ```python
    pip install -e .
    ```

    * プロジェクトがインストールされたことを確認
        
        ```py
        $ pip list

        Package        Version   Location
        -------------- --------- ----------------------------------
        click          6.7
        Flask          1.0
        flaskr         1.0.0     /home/user/Projects/flask-tutorial
        itsdangerous   0.24
        Jinja2         2.10
        MarkupSafe     1.0
        pip            9.0.3
        setuptools     39.0.1
        Werkzeug       0.14.1
        wheel          0.30.
        ```

* プロジェクトの実行の方法

    ディレクトリの指定はなくどこからでも
    実行ができる状態となる。

    ```bash
    $ export FLASK_APP=flaskr
    $ export FLASK_ENV=development
    $ flask run
    ```

* WSGI：Web Server Gateway Interface

    pythonにおいてwebサーバーとWebアプリケーション（あるいはWebアプリケーションフレームワーク)を接続するための標準化されたインターフェース定義のこと


* 本番環境での実行について

    開発サーバーであるflask runを利用しており、本番環境への適用は非推奨である。本来であれば別途HTTPサーバーを立ててFlaskと連携させる設定をするべき。

    https://msiz07-flask-docs-ja.readthedocs.io/ja/latest/cli.html#run-the-development-server

    ![](/python/img/flask_server.png)
    

## FlaskからHTML→Javascriptにデータを送る方法

* Flask側の記載：render_templateで連携する
    ```python
    return render_template('search.html', archs = result2)
    ```

* HTML側の記載：jinjaの文法で呼び出す。json形式で呼び出しをする

    ```html
    <script type="text/javascript">
        const archs = {{ archs | tojson }} 
        // console.log( {{ archs | tojson }} )
    </script>
    <script type="text/javascript" src="{{ url_for('static', filename='/js/leaflet_tuto.js') }}"></script>
    ```

* js側の記載：Array型の変数としてarchsをグローバル変数として利用可能となる。

    ```js
    const archs_js = archs 
    ```

### チュートリアルメモ

- flaskでのデバックモード実行の方法が不明。breakポイント入れているし、デバックモードにはしている
解決。VScodeのデバックメニュー（左のメニューに虫と再生ボタンがある）から、launch.jsonに/flaskr/__init__.pyを指定して実行する。

- register画面でうまくエラーメッセージが表示されない
解決。flash()のインデントがうまくできていなかった。

- registerでDB登録がうまくできていない。
5/23 1H悩んだけど解決できず...明日また頑張る。