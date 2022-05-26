# SQL

## DBSMの三大機能
- メタデータ管理
データベース、スキーマ、テーブルなどがどこにあるのか
- 質問処理
SQLなどの解釈機能
- トランザクション管理

## SQLスタイルガイド
`https://www.sqlstyle.guide/ja/`

## 集計
### SELECT
#### LIKE
- %：1文字**以上**の任意の文字列
- _：1文字の任意の文字列  

以下でTから始まり7文字の名前となる人を検索することができる
```
SELECT * FROM customer WHERE last_name like 'T______'
```

#### COUNT
COUNT(カラム名)でカラムの行数を検索できる

```
SELECT COUNT(*) FROM customer;
```

#### GROUPBY
```
SELECT <カラム名> FROM <テーブル名> GROUP BY <カラム名>;
```
グループ化することでデータ値の集計が可能
- COUNT(総数)
- SUM(総和)
- AVG(平均)
- MAX
- MIN

```
SELECT <カラム名>, COUNT(<カラム名>) FROM <テーブル名> GROUP BY <カラム名>;
```
- 複数のカラム名を指定した場合は、そのカラムのデータの組み合わせが同じものでグループ化される

#### LIMIT
```
SELECT
	rental_duration, 
	COUNT(film_id) 
 FROM film 
 GROUP BY rental_duration
 LIMIT 3;
 ```

 #### ORDER BY
 ```
 SELECT
	rental_duration, 
	COUNT(film_id) 
 FROM film 
 GROUP BY rental_duration
 ORDER BY rental_duration DESC;
 ```
 - ASC：昇順
 - DESC：降順


## 結合
### INNER JOIN (内部結合)

```
SELECT
	c.customer_id,
	c.store_id,
	c.first_name,
	c.last_name,
	a.address,
	a.address2
FROM customer as c
JOIN address as a
ON c.address_id = a.address_id
ORDER BY customer_id DESC;
```

### OUTER JOIN（外部結合）
    - LEFT OUTER JOIN (左外部結合)  
    左がベース
    - RIGHT OUTER JOIN (右外部結合)  
    右がベース（左にないものはNULLになる）
    - FULL OUTER JOIN (完全外部結合)  
    左右どちらもお互いにないものはNULLで表示
    UNIONALL？？

### CROSS JOIN(直積)
### NATURAL JOIN(自然結合)

### UNION
- 複数のSELECT分により取得したレコードを縦に結合し、１つの結果として取得する
- 取得するカラムの型を合わせる必要がある
→カラムをＮＵＬＬで合わせるテクニックも覚える
 特に脆弱性攻撃の際に、カラム名が数個しかわからない場合にＮＵＬＬを指定してあげることで該当の項目を取得してくることができる

```
SELECT
	store_id,
	last_name
FROM customer
UNION
SELECT
	film_id,
	NULL
FROM film
ORDER BY store_id ASC;
```

#### UNION:結果でかえってくるレコードの重複を許さない
#### UNION ALL :結果で返ってくるレコードの重複を許す

## 操作

### CREATE TABLE
```
CREATE TABLE product(
	id INTEGER NOT NULL PRIMARY KEY,
	name TEXT NOT NULL,
	price INTEGER NOT NULL,
	description TEXT);
```

### INSERT
```
INSERT INTO
product (id, name, price ,description) VALUES
	(1,'Cheese',100,'Delicious dairy'),
	(2,'Bread',200,'Freshly baked');
```

### DELETE
- 条件には主キーを指定することが多いが別の条件の
指定も可能
- 条件をつけない場合は全レコードが削除される
```
DELETE FROM product WHERE id = 2;
```

### DROP TABLE
```
DROP TABLE product;
```

### 参考：SQLの種類
#### DDL(Data Definition Language)
ーデータベースの構造を定義する
- CREATE,DROP
#### DML(Data Manipulation Language)
- テーブル内のデータの検索・追加・更新・削除を行う
- SELECT,INSERT,UPDATE,DELETE
#### DCL(Data Control Language)
- データベースのアクセス制御を行う
- GRANT(アクセス権限付与),REVOKE(アクセス権限取り消し)

## 副問い合わせ
```
-- SELECT
-- 	cus.customer_id,
-- 	cus.first_name,
-- 	cus.last_name,
-- 	B.rental_id_count,
-- 	B.is_match
-- FROM customer AS cus
-- JOIN
-- (
-- SELECT
-- 	c.customer_id,
-- 	COUNT(r.rental_id) AS rental_id_count,
-- 	c.is_match AS is_match
-- FROM campaign AS c
-- JOIN rental AS r
-- ON c.customer_id = r.customer_id
-- GROUP BY c.customer_id
-- ORDER BY rental_id_count DESC
-- ) AS B
-- 	ON cus.customer_id = B.customer_id
```



## ストアドプロシージャー
'https://qiita.com/jiyu58546526/items/5dd2cb70fd2f67d5acf9'  

### ストアドプロシージャとは
データベースに自分で定義して（データベースサーバーに貯めて(stored))、後で使うことができる手続き(Procedure, 戻り値のない関数)。何回も同じSQL分を打つ必要がなくなったり、クライアントとサーバーの間の通信量を減らしたりできる。

## 参考（NoSQL)
SQLを扱わないデータベース  
Not only SQLの略  
