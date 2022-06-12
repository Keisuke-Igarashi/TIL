# **poetry**

### pythonのライブラリ管理ツール

* venvとは違いプロジェクトごとにライブラリの依存関係も管理する
* ライブラリの依存関係だけでなく、ビルド環境一式を設定ファイルとして記述することが可能で、ビルドや公開を簡単に行える

![](/python/img/poetry_concept.png)

### pyproject.toml
    
* 現在のプロジェクトに必要なライブラリの一覧が記載されたファイル
* ユーザが指定したライブラリ名とバージョン文字列を記載

### poetry.lock

* pyproject.tomlに記載した条件にしたがって、ある時点での依存関係の解決結果を書き込んでおくファイル
* 初回のpoetry install時に初めて作成される

### ダウンロード

* 以下でpythonファイルを実行しダウンロードする

    ```bash
    curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
    ```

* 環境変数設定を保存する
    
    ```bash
    $ source ~/.profile
    $ which poetry
    $ poetry completions bash | sudo tee /etc/bash_completion.d/poetry.bash-completion
    ```
    * 最後のコマンドはbashにてTAB補完機能を有効化する設定。必須ではない。



### 機能

* new：プロジェクトのひな形を作成する

    ```
    poetry new [project name]
    ```

    ![](/python/img/poetry_project.png)

* init： pyproject.tomlを新規作成するコマンド

    * プロジェクトのひな形の必要がない場合に利用
    * 対話形式で項目を入力
    * オプションを付けると直接指定が可能

        ```bash
        poetry init --dev-dependency pytest
        ```

* install: pyproject.tomlの読み込み、依存関係の解決およびインストール

    ```bash
    poetry install
    ```

    * --no-devオプション：開発用ライブラリをインストールしない

        ```bash
        poetry install --no-dev
        ```

        ※開発用のライブラリとは、pyproject.tomlの[tool.poetry.dev-dependecies]に記載されたライブラリ

        * poetry.lockの有無による挙動の違い
            
            * poetry.lockがある場合：ロックファイルに記述されているバージョンを厳密に適用

            * poetry.lockがない場合：コマンド実行時に依存関係の解決、インストールを行いロックファイルを作成

* update：プロジェクトの依存関係を解決しライブラリを最新に更新

    ```bash
    poetry update
    ```

    * ライブラリのみのアップデートも可能
    
        ```bash
        poetry update ruquests selenium
        ```


* show：インストール済みライブラリの一覧を表示
    
    ```bash
    $ poetry show
    $ poetry show pytest    //詳細を表示
    ```

    * --no-devオプション：開発用の依存関係を列挙しない
    * **--treeオプション**：依存関係を木構造で列挙する
    * --latestオプション：最新バージョンを表示する
    * --outdated：　古くなったパッケージの最新バージョンだけを表示する

    * 特定のライブラリの確認

        ```bash
        poetry show pytest
        ```

* add : 必要なライブラリを**pyproject.toml**に追加および**インストール**

    ```bash
    $ poetry add flask requests
    $ poetry add --dev requests //開発用の依存関係として追加
    $ poetry add --dev requests //バージョンの制約を追加することが可能
    ```

    * バージョン制約の例

        * 「poetry add "ライブラリ名>=1.1.5"」:1.1.5以上のバージョン
        * 「poetry add ライブラリ名@^1.1.5」：1.1.5以上2.0.0未満のバージョン
        * 「poetry add ライブラリ名@latest」：最新のバージョン
  
<br />

* remove：インストールされているライブラリを削除

    ```bash
    $ poetry Ubuntu remove selenium
    ```


* shell：作成した仮想環境の中でシェルを実行

    * 仮想環境の中でシェルを起動

        ```bash
        poetry shell
        ```
    * 仮想環境を終了
        
        ```bash
        exit
        ```

    * 仮想環境が作成されるディレクトリは以下のコマンドで確認可能

        ```bash
        $ poetry config virtualenvs.path
        ```

    * プロジェクトの仮想環境の中でコマンドを実行するだけの場合、runコマンドも利用可能

        ```bash
        $ poetry run python sample_pca.py
        ```

        


