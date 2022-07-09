# curlコマンド

[参考サイト](https://atmarkit.itmedia.co.jp/ait/articles/1608/09/news031.html)

curlはURL構文を用いてデータを転送するためのオープンソースなコマンドラインツール/ライブラリ。プロジェクト名は「cURL」(Client for URLs)であり、コマンドラインツール「curl」、ライブラリの「libcurl」が成果物。

コマンドの書式

```bash
curl http://www.arcarkit.co.jp/ait/subtop/dotnet -O
```

* -Oオプション：転送元と同じ名前でカレントディレクトリに保存する保存する
* -Aオプション(--user-agent "Webブラウザ")：実行時のWebブラウザ名を指定する