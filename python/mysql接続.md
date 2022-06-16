# **pythonでのmysql接続**

## ライブラリの種類について

    [参考サイト](https://algorithm.joho.info/programming/python/mysql-module-library/)


## mysql-connector-python 

* 初回実行時Authentication Errorになる場合がある。my.cnf(=my.ini)を書き換えて対応すること
    
    * [Authentication Error](https://kataware.hatenablog.jp/entry/2018/05/30/215153)

## PyMySQL

* 接続方法

    ```python
    import pymysql.cursors

    def get_db():
    connect = pymysql.connect(
                            host="mysqldb",
                            # port=3306,
                            user='root',
                            password='password',
                            db='majdb',
                            cursorclass=pymysql.cursors.DictCursor
    )

    return connect
    ```

* SQLの記載方法

    ```python
     connect = get_db()

            with connect.cursor() as cursor:
          

                # 建築家名から建築家IDを取得する
                search_word = "%" + archiname + "%"
                sql = "SELECT * FROM architect WHERE architect_name LIKE %s;"
                cursor.execute(sql,(search_word,))
                result = cursor.fetchall()
                result_dict = result[0]
                archiname_id = result_dict['architect_id']

                # 建築家IDから建築物を取得する
                search_word = archiname_id
                sql = "SELECT * FROM architecture WHERE architect_id = %s;"
                cursor.execute(sql,(search_word,))
                result2 = cursor.fetchall()

            
            print(type(result))
            print(result_dict)

            connect.close()
    ```

        * LIKE検索の方法は少し特殊なので注意すること
        * ほんと初歩的だがWHEREの綴りミスで2Hくらい無駄にしたので注意すること

* INSERTについて

    * [参考サイト](https://github.com/PyMySQL/PyMySQL#example)

    ```python
    import pymysql.cursors

    # Connect to the database
    connection = pymysql.connect(host='localhost',
                                user='user',
                                password='passwd',
                                database='db',
                                cursorclass=pymysql.cursors.DictCursor)

    with connection:
        with connection.cursor() as cursor:
            # Create a new record
            sql = "INSERT INTO `users` (`email`, `password`) VALUES (%s, %s)"
            cursor.execute(sql, ('webmaster@python.org', 'very-secret'))

        # connection is not autocommit by default. So you must commit to save
        # your changes.
        connection.commit()

        with connection.cursor() as cursor:
            # Read a single record
            sql = "SELECT `id`, `password` FROM `users` WHERE `email`=%s"
            cursor.execute(sql, ('webmaster@python.org',))
            result = cursor.fetchone()
            print(result)
        ```