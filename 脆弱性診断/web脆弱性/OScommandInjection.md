# OSコマンドインジェクション

* シェルを呼び出す機能のある関数を利用しているサイトで起きる
    * PHP：exec(), passthru(), proc_open(),shell_exec(), system()
    * perl：open(), system()

## 対策
* シェルを起動できる関数の利用を避ける

## 脆弱性の検査方法
