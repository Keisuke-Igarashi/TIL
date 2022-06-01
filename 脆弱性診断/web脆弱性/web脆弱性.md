# web脆弱性

## おすすめのサイトなど
-(OWASP JAPAN代表的な脆弱性のサイト)[https://owasp.org/www-chapter-japan/]

## コンテンツ

* 脆弱性/脅威/対策で整理していく

## XSS

* 現在も脆弱性の届け出が一番多い
* JavaScriptを使って攻撃する

### 流れ
①フォーム入力などから脆弱性を確認する。以下コマンドを入れる
```
'>"><hr>
```
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

### 対策
・レスポンスするデータに対してサーバー側でエスケープ処理を施す
→phpだとhtmlspecialchars関数を利用する

```php
$value = htmlspecialchars(nl2br($row[$k]), ENT_QUOTES, "UTF-8");
```



## CSRF
Cross Site Request forgery
- forgery：偽造

### 脆弱性内容
* **ログインしている**ユーザが意図しないリクエスをWebアプリケーションが
受け付けてしまう脆弱性

### 脆弱性判断ポイント
1. hidden属性にトークンを持っていないまたは推測可能である場合
2. 購入、更新が行われる画面からパスワードの入力を求められない場合
3. refererを確認していない場合

### 攻略方法
掲示板等にパスワード変更画面や購入画面等に遷移するためのURLを
仕込んでおく。もしくはGETでサーバーが受信しているのであればクエリストリング
で値を変更するためのリクエストをサーバーに投げるなど


### 修正方法
学習未実施（Apgoatで別途)(6/2の演習中に実施)

### 対策

1. トークン
→リクエストのトークンで判断する

2. リファラー
→どのページから遷移してきたかを確認する

3. サーバ側で処理前にパスワード要求
→画面の設計変更が必要

'https://www.ipa.go.jp/files/000017316.pdf'
p31


