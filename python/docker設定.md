# **PythonのDocker設定**

[参考サイト](https://docs.docker.com/language/python/build-images/)

* サンプルアプリケーションを作成する

    ```sh
    cd /path/to/python-docker
    pip3 install Flask
    pip3 freeze | grep Flask >> requirements.txt
    touch app.py
    ```

    * app.pyの中身

        ```python
        from flask import Flask
        app = Flask(__name__)

        @app.route('/')
        def hello_world():
            return 'Hello, Docker!'
        ```

    * アプリケーションの実行

        ```bash
        python -m flask run
        ```

* Dockerfileの作成

    * syntax：parserのバージョンを伝える

        ```docker
        # syntax=docker/dockerfile:1
        ```

        * 1を指定することで最新のparserを選択してくれるので推奨

    * pythonのイメージを取得する

        ```docker
        FROM python:3.8-slim-buster
        ```

    * WORKDIR：Dockerfile内に記載したコマンドの実行ディレクトリを作成

        ```docker
        WORKDIR /app
        ```

    * COPY：dockerイメージにファイルをコピーする

        * 第一引数：イメージにコピーしたいファイルの指定
        * 第二引数：どこにファイルをコピーしたいかの指定

        ```docker
        COPY irequirements.txt requirements.txt
        ```

        ※appにコピーする

    * RUN：コマンド実行指示

        ```docker
        RUN pip install -r requirements.txt
        ```

    * ソースファイルをコピー

        ```docker
        COPY . .
        ```

    * 環境変数を指定

        ```docker
        ENV FLASK_APP majapp
        ENV FLASK_ENV development
        ```

        * CMD export FLASK_APP = majappで実行してみたがこれはダメだった。

    * CMD：コンテナ起動時に実行するコマンドを指定

        ```docker
        CMD [ "python", "-m" , "flask", "run", "--host=0.0.0.0"]
        ```

        * --host=0.0.0.0：外部から参照可能にするために必要な設定

    * Dockerfileの全容

        ```docker
        # syntax=docker/dockerfile:1

        FROM python:3.8-slim-buster

        WORKDIR /app

        COPY requirements.txt requirements.txt
        RUN pip3 install -r requirements.txt

        COPY . .

        CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0"]
        ```

* Dockerfileをビルドしてイメージを作成

    ```docker
    docker build --tag python-docker .
    ```

    * --tagオプション：イメージに名前をつける

[参考サイト(Docker build以降)](https://docs.docker.com/language/python/run-containers/)

* コンテナの起動

    ```bash
    docker run python-docker
    ```

    * 実行結果
        ```bash
        ┌──(kali㉿kali)-[~/Documents/python-docker]
        └─$ sudo docker run python-docker
        [sudo] kali のパスワード:
        * Environment: production
        WARNING: This is a development server. Do not use it in a production deployment.
        Use a production WSGI server instead.
        * Debug mode: off
        * Running on all addresses (0.0.0.0)
        WARNING: This is a development server. Do not use it in a production deployment.
        * Running on http://127.0.0.1:5000
        * Running on http://172.17.0.2:5000 (Press CTRL+C to quit)
        ```

        * RESTサーバーが立ち上がった状態となる

    * curlで呼び出ししてみる。
    
        ```bash
        $ curl localhost:5000
        curl: (7) Failed to connect to localhost port 5000 after 0 ms: 接続を拒否されました
        ```

        * コンテナをisolationで起動しているため上記の動きとなる

    
    * publshモードでローカルネットワーク内で起動する

        ```bush
        docker run --publish 8000:5000 python-docker
        ```

        * --publish(-p for short)：[host port]：[container port]

        * 実行結果

            ```bash
            ┌──(kali㉿kali)-[~]
            └─$ curl localhost:8000
            Hello, Docker!
            ```

    * ditachモードで起動する

        ```bash
        docker run -d -p 8000:5000 python-docker
        ```

       **これでdockerホストのOSのIPアドレスにポート番号8000番でアクセスすることでflaskをホストOSから起動することができるようになる**

       ```
       http://<DockerホストOSのIPアドレス>:8000
       ```




[データベース設定以降の参考サイト](https://docs.docker.com/language/python/develop/)

* データベース設定

    * データ格納用、設定値格納用のボリュームを作成する

        ```bash
        docker volume create mysql
        docker volume create mysql_config
        ```

        * ボリュームのリスト表示

            ```bash
            docker volume ls
            ```

        * ボリュームの詳細

            ```bash
            docker volume inspect ボリューム名
            ```

    * DBと接続するためのネットワークを作成する

        ユーザ定義ネットワークを作江氏することでDNSの機能が使えるようになる。

        ```bash
        docker network create mysqlnet
        ```

    * MySQLをコンテナ上で起動し、ボリュームとネットワークにattachする

        ```bash
        docker run --rm -d -v mysql:/var/lib/mysql \
        -v mysql_config:/etc/mysql -p 3306:3306 \
        --network mysqlnet \
        --name mysqldb \
        -e MYSQL_ROOT_PASSWORD=p@ssw0rd1 \
        mysql
        ```

        * -v or --volume オプション

            * 第一引数：ボリューム名
            * 第二引数：コンテナのどのディレクトリにマウントするか
            * 第三引数：roなどのオプション

             ![](/python/img/docker_volume.png)

    * MySQLへの接続

        ```bash
        docker exec -ti mysqldb mysql -u root -p
        ```

    * アプリケーションと接続する

        * app.pyを編集する

        ```python
        import mysql.connector
        import json
        from flask import Flask

        app = Flask(__name__)

        @app.route('/')
        def hello_world():
        return 'Hello, Docker!'

        @app.route('/widgets')
        def get_widgets():
        mydb = mysql.connector.connect(
            host="mysqldb",
            user="root",
            password="p@ssw0rd1",
            database="inventory"
        )
        cursor = mydb.cursor()


        cursor.execute("SELECT * FROM widgets")

        row_headers=[x[0] for x in cursor.description] #this will extract row headers

        results = cursor.fetchall()
        json_data=[]
        for result in results:
            json_data.append(dict(zip(row_headers,result)))

        cursor.close()

        return json.dumps(json_data)

        @app.route('/initdb')
        def db_init():
        mydb = mysql.connector.connect(
            host="mysqldb",
            user="root",
            password="p@ssw0rd1"
        )
        cursor = mydb.cursor()

        cursor.execute("DROP DATABASE IF EXISTS inventory")
        cursor.execute("CREATE DATABASE inventory")
        cursor.close()

        mydb = mysql.connector.connect(
            host="mysqldb",
            user="root",
            password="p@ssw0rd1",
            database="inventory"
        )
        cursor = mydb.cursor()

        cursor.execute("DROP TABLE IF EXISTS widgets")
        cursor.execute("CREATE TABLE widgets (name VARCHAR(255), description VARCHAR(255))")
        cursor.close()

        return 'init database'

        if __name__ == "__main__":
        app.run(host ='0.0.0.0')
        ```

    * ライブラリを追加する

        ```bash
        pip3 install mysql-connector-python
        pip3 freeze | grep mysql-connector-python >> requirements.txt
        ```

    * イメージをビルドする

        ```bash
        docker build --tag python-docker-dev .
        ```

    * コンテナをデータベースのネットワークに追加して起動する

        ```bash
        docker run \
        --rm -d \
        --network mysqlnet \
        --name rest-server \
        -p 8000:5000 \
        python-docker-dev
        ```

    * 実行する

        ```bash
        curl http://localhost:8000/initdb
        curl http://localhost:8000/widgets

* docker-composeを利用する

    * docker-compose.dev.yml
    
        ```yml
        version: '3.8'

        services:
        web:
        build:
        context: .
        ports:
        - 8000:5000
        volumes:
        - ./:/app

        mysqldb:
        image: mysql
        ports:
        - 3306:3306
        environment:
        - MYSQL_ROOT_PASSWORD=p@ssw0rd1
        volumes:
        - mysql:/var/lib/mysql
        - mysql_config:/etc/mysql

        volumes:
        mysql:
        mysql_config:
        ```

        * [docker-compose.ymlの記載方法](https://docs.docker.com/compose/compose-file/)

        * docker_compose時のnetwork設定について

            * [参考サイト](https://docs.docker.jp/compose/networking.html)

                同一のcompose.yml内で指定したサービスは同一のbridgeネットワーク内に設定される。

    * mysqlの初期化について

        * [参考サイト](https://qiita.com/moaikids/items/f7c0db2c98425094ef10)


    * コンテナを実行

        ```bash
         docker-compose -f docker-compose.dev.yml up --build
         ```

