# HTMLタグ情報

## ul

'https://html-coding.co.jp/annex/dictionary/html/ul/'

## span

'https://html-coding.co.jp/annex/dictionary/html/span/'

単体としては特に意味を持たないがインライン要素としてグループ化する。
styleのまとめての適用が可能

## label
'https://html-coding.co.jp/annex/dictionary/html/label/'
フォームの中でフォームの項目名と構成部品を関連付けるためのタグ

## input
`https://html-coding.co.jp/annex/dictionary/html/input/`
formタグで作成したフォームの中でテキスト入力欄やボタンなどの部品を作成する要素

## thead
* 'https://html-coding.co.jp/annex/dictionary/html/thead/'

    *テーブルヘッダ。表のタイトルを作成するブロック

## from

* 入力ホーム以外の値をPOSTする

    ```html
    <input type="hidden" name="example" value="<連携したい値>">
    ```

    * nameでアクセス可能(flaskのrequestの例)

        ```python
        if request.method == 'POST':
        architecture_id = request.form['architecture_id']
        ```