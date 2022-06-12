# パッケージ管理

## リポジトリアップデート

```bash  
sudo apt update 
```

* エラーが出る場合は以下を実行して再度リポジトリアップデート  

```bash
wget -q -O - https://archive.kali.org/archive-key.asc | apt-key add 
```

## パッケージインストール

```bash
sudo apt-get update
sudo apt^get install <パッケージ名>
```

## pyenvをインストールした際に事前にインストールしたツール

* こういったものを集めておくと環境構築時に参考になるのではないか。

    ```bash
    sudo apt-get update; sudo apt-get install make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev git
    ```

    * build-essential：ビルドに必要なライブラリ群
    * libssl-dev: opensslなどセキュアなソケット通信を行うためのライブラリ
    * libbz2-dev: データ圧縮/解答ソフトウェアbzip2のライブラリ
    * libreadline-dev: readline, コマンド実行履歴を保存・検索するためのReadlineライブラリとヘッダー  'https://qiita.com/tomtsutom0122/items/60f0ab22771a8c65247a'