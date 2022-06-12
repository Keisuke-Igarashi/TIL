# **pyenv**

* pythonの複数バージョンを使い分けるコマンドラインツール

## **インストール**

* すべてのpythonが```${pyenv root}/versions```配下にインストールされる

* pyenvインストールに必要なライブラリを事前インストールする

    ```bash
    sudo apt-get update; sudo apt-get install make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev git
    ```

* build-essential：ビルドに必要なライブラリ群
* libssl-dev: opensslなどセキュアなソケット通信を行うためのライブラリ
* libbz2-dev: データ圧縮/解答ソフトウェアbzip2のライブラリ
* libreadline-dev: readline, コマンド実行履歴を保存・検索するためのReadlineライブラリとヘッダー  'https://qiita.com/tomtsutom0122/items/60f0ab22771a8c65247a'


* gitからチェックアウトする(home/.pyenvがおすすめ)
    
    ```bash
    git clone https://github.com/pyenv/pyenv.git ~/.pyenv
    ```

* 環境変数を設定する

    ```bash
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
    echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(pyenv init -)"' >> ~/.bashrc
    ```

    * ~/.profileにも同様の設定をする

    ```bash
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile
    echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile
    echo 'eval "$(pyenv init -)"' >> ~/.profile
    ```

    * shellを再起動する
    
    ```bash
    exec "$SHELL"
    ```

    * pyenvのインストール確認
    
    ```bash
    pyenv -v
    pyenv 2.3.1-17-g9f2cba3d
    ```

* pythonのインストール

    ```
    pyenv install 3.8.5
    
    testuser@ubuntu:~/.pyenv$ pyenv versions
    * system (set by /home/testuser/.pyenv/version)
    3.8.5
    ```

* pythonバージョンの切り替え

    * カレントディレクトリのpythonのバージョンを切り替える

        ```bash
        $ pyenv local 3.8.5
        $ pyenv versions
        system
        * 3.8.5 (set by /home/testuser/.pyenv/.python-version)
        ```

    * グローバル環境のpythonのバージョンを切り替える
        
        ```bash
        pyenv global 3.8.5
        ```

    * 現在のシェルでPyhtonのバージョンを切り替える
        
        ```bash
        pyenv shell 3.8.5
        ```

    * pyenv localコマンドを実行すると、カレントディレクトリに".python-versions"というファイルが作成され、指定したバージョンが書き込まれる。

    * 設定を解除する場合上書きするか以下コマンドを実行する

        ```bash
        pyenv local --unset
        ```

        