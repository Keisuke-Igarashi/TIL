# ファイル入出力
* 1文字目
    - r:読み出し
    - w:ファイルが既に存在する場合は上書き
    - x:書き込み指定したファイルが存在しない場合のみ書き込み
    - a:ファイルがすでに存在する場合は末尾に追記
* 2文字目
    - t:テキスト(省略した場合はこちら)
    - b:バイナリ

```python
# 読み込みモードでオープン
file = open("test.txt", "r")

# readですべて読み込む
string = file.read()

print(string)
# 一度開いたファイルオブジェクトは忘れずにクローズする
file.close()
```

```python
# 書き込みモードでオープン
file = open("practice.txt", "w", encoding="UTF-8")

# writeで書き込む
file.write(tasizan(a,b))
file.write(kakezan(a,b))

# 一度開いたファイルオブジェクトは忘れずにクローズする
file.close()
```

* バイナリファイルへの書き込み
'https://naruport.com/blog/2019/9/20/python-tutorial-write-to-binary-file/#%E3%83%90%E3%82%A4%E3%83%8A%E3%83%AA%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%92%E6%9B%B8%E3%81%8D%E8%BE%BC%E3%81%BF%E3%83%A2%E3%83%BC%E3%83%89%E3%81%A7%E9%96%8B%E3%81%8F'

```python
with open('alphas.dat', mode='wb') as f:
    pass
```
