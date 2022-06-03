# OSコマンドインジェクション

* シェルを呼び出す機能のある関数を利用しているサイトで起きる
    * PHP：exec(), passthru(), proc_open(),shell_exec(), system()
    * perl：open(), system()

## 対策
* シェルを起動できる関数の利用を避ける

## 脆弱性の検査方法

* pingを実施してみる。

```windows
& /windows/system32/ping -n 2 127.0.0.1
```

![](/%E8%84%86%E5%BC%B1%E6%80%A7%E8%A8%BA%E6%96%AD/web%E8%84%86%E5%BC%B1%E6%80%A7/OScommandInjection.md)

