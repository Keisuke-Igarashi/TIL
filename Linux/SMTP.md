# SMTP

電子メールを**送信**するためのプロトコル  
simple mail transfer protocol

## 特徴

- ポート番号：25(465, 587)

1. 配送(transfer)
    - 25番ポートでサーバー間のメール転送を行う
2. 提出
    - 一般的に、クライアントからの受付には587番
    　ポートが用いられる。
    - 25番ポートに接続しようとしても、ほとんどの
    インターネットサービスプロバイダがブロック（OP25B)

## 使い方

SMTPはあくまでもメール送信用のプロトコル
メール受信機能はない

'https://www.tohoho-web.com/ex/draft/smtp.htm'

'http://www.puni.net/~mimori/smtp/ref.html'

```
EHLO
MAIL FROM:xxx.xxx.xxx
250 ok
RCPT TO:xxx.xxx.xxx
250 ok
data

sebject:

本文本文本文
本文本文本文
本文本文本文

.

quit
```
みたいな流れ（対話型）
