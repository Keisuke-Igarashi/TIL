# bash

[参考資料](https://owasp.org/www-pdf-archive/Shellshock_-_Tudor_Enache.pdf)

* bashについて

  * .shで利用することができる
  * "one-liners"で定義することができる

  ```bash
  ┌──(kali㉿kali)-[~]
  └─$ welcome() { echo "Hi $USER, here's the date:"; date; }
  
  ┌──(kali㉿kali)-[~]
  └─$ welcome
  Hi kali, here's the date:
  2022年  7月  9日 土曜日 16:15:27 JST
  ```

