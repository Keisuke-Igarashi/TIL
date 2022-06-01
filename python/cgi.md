# CGI
* Common Gateway Interface

* ディレクトリ構成

cgi ※ここでhttp.server立てる前提
 |- cgi-bin
 |- index.html



```python
 python -m http.server --cgi 8080
```

* http://localhost:8080/cgi-bin/cgi_test.pyでアクセス

cgi_test.py
```python
import sys
import io
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')

html = """Content-Type: text/html

<html>
    <head>
        <meta charset='UTF-8'>
    </head>
<body>
    <h1>Hello CGI!</h1>
</body>
</html>
"""
print(html)
```

* POSTリクエストのパラメータを受け取るには、cgiモジュールのFiledStorageクラスを利用する。

```python
import cgi
import sys
import io
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')

html_body = """Content-Type: text/html

<!DOCTYPE html>
<html>
    <head>
        <meta charset='UTF-8'>
    </head>
<body>
    {}さんこんにちは！
</body>
</html>
"""

form = cgi.FieldStorage()
text = form.getvalue('data')
print(html_body.format(text))
```

* デバッグ→cgitb
-[cgitbモジュールのオプション等](https://docs.python.org/ja/3/library/cgitb.html)

* 日付の表示プログラム

```python
from datetime import date
import sys
import io
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')
today = date.today()
print(today)

html = """Content-Type: text/html

<html>
    <head>
        <meta charset='UTF-8'>
    </head>
<body>
    <h1>今日の日付は{}です</h1>
</body>
</html>
"""
print(html.format(today))
```

* postのサンプルプログラム

-(pythonで現在時刻を取得)[https://note.nkmk.me/python-datetime-now-today/]

```python
import cgi
import sys
import io
from datetime import date
import cgitb

cgitb.enable()
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')

html_body = """Content-Type: text/html

<!DOCTYPE html>
<html>
    <head>
        <meta charset='UTF-8'>
    </head>
<body>
    <h1>ユーザ情報</h1>
        <h3>名前：{}</h3>
        <h3>誕生日:{}</h3>
        <h3>{}</h3>
</body>
</html>
"""

form = cgi.FieldStorage()
name = form.getvalue('fullname')
birthday = form.getvalue('birthday')
msg = ''

if birthday[4:] == str(date.today())[4:]:
    msg = '誕生日おめでとうございます'

print(html_body.format(name, birthday, msg))
```

-(htmlからファイルを受け取る方法)[https://docs.python.org/ja/3/library/cgi.html]




## web上でのファイルのアップロード

* ファイルのアップロードプログラム
* MIMETYPE "multipart/form-data"について
'https://www.yoheim.net/blog.php?q=20171201'
* サーバー配下のファイルはすべてcgiとして認識されてしまうため、
 imageファイルはサーバー外のディレクトリに配置すること！！**めっちゃはまったので注意***


```html
<!DOCTYPE html>

<html lang="ja" xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="utf-8" />
    <title></title>
</head>
<body>
    <form action="/cgi-bin/fileupload.py" method="POST" enctype="multipart/form-data">
        <p><input type="file" name="image" accept="image/*"></p>
        <p><input type="submit"></p>
    </form>

    <a href="./cgi-bin/filelist.py">アップロードしたリスト一覧はこちら</a>
</body>
</html>
```

```python
import cgi
import sys
import io
import cgitb
import base64
import numpy
import cv2
from PIL import Image
import html

cgitb.enable()
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')

form = cgi.FieldStorage()

html_body = """Content-Type: text/html

<!DOCTYPE html>
<html>
    <head>
        <meta charset='UTF-8'>
    </head>
<body>
    {}
</body>
</html>
"""

if form['image'].filename:
    # ファイルのインスタンス
    fileitem = form['image']

    # ファイルの名前(filename属性)
    fileitem_name = form['image'].filename

    # ファイルのバイト値(value属性)
    fileitem_byte = form['image'].value

    filepath = './cgi-bin/image/' + fileitem_name

    # ファイルの保存(解答より)
    with open(filepath,'wb') as f:
        f.write(fileitem.file.read())
        output = 'ファイルが保存されました'
        
    # ファイルの保存（numpyを使ったやり方。日本語のファイル名が文字化け)
    # ネットで調べたところ日本語の文字列に対応していない（https://teratail.com/questions/26881
    # arr = numpy.asanyarray(bytearray(fileitem_byte), dtype=numpy.uint8)
    # d_img = cv2.imdecode(arr, -1)
    # cv2.imwrite(html.unescape(filepath), d_img)

    print(html_body.format(output))

else:
    output = 'ファイルを選択してください'
    print(html_body.format(output))
```
