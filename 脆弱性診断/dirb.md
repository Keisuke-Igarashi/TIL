# dirb

辞書攻撃ツール

[参考サイト](https://ethicalhacking.hatenablog.com/entry/brute-force-attack-using-dirb-on-kali-linux-2021-1/)

* kaliに標準インストールされている

* バージョン確認

  ```bash
  dirb
  
  -----------------
  DIRB v2.22    
  By The Dark Raver
  -----------------
  ```

* パスワード辞書（リスト）

  ```
  ┌──(kali㉿kali)-[/usr/share/dirb/wordlists]
  └─$ ls                                                                                         
  big.txt     common.txt   extensions_common.txt  mutations_common.txt  small.txt    stress
  catala.txt  euskera.txt  indexes.txt            others                spanish.txt  vulns
  ```

  

* その他同様なツールとして、gobusterやdirbusterなどがある