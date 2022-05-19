# Windowsでのセットアップについて

1. 公式サイトからインストールする。PATHに追加チェックボックスをONにしておく。
2. インストール後コマンドプロンプトで以下実行
```
python --version
```
ここでpythonのバージョンが表示されればインストールが成功しているが、
なぜかversionが表示されず"python"とだけ表示される状態になってしまった。

ネットで検索したところ、アプリ実行エイリアスが悪さをしているということで、  
ホーム→設定→アプリ→アプリ実行エイリアスでpythonの設定をＯＦＦにしたところ、  
無事バージョンの表示ができた。

# venv環境の作成について

1. VScodeにて、pythonプロジェクトのディレクトリを開いておく。
2. pythonでvenv環境を作る
```
python --version
python -m venv venv
./venv/Script/activate
```