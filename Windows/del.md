# **del**コマンド

* delコマンド

    ```powershell
    del /S /Q *.doc c:¥¥users¥¥%username%¥¥ > nul
    ```

    * [参考サイト](https://docs.microsoft.com/ja-jp/windows-server/administration/windows-commands/del)
    * 1つ以上のファイルを削除する。eraseコマンドと同じ操作を実行する。
    * /s オプション：現在のディレクトリとすべてのサブディレクトリから、指定されたファイルを削除。削除中のファイルの名前を表示する。
    * /q オプション：クワイエットモードを指定する。削除の確認を求めるメッセージは表示されない。
    * nul：Linux系における「/dev/null」におおむね該当。標準出力の出力先をnulにリダイレクトすることで証跡を残さないようにしているのかも。
        * [参考サイト](https://www.pg-fl.jp/program/dos/doscmd/dev_nul.htm)
