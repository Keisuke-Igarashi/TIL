# SSL/TLS

* SSL: Secure Sockets Layer
* SSL/TLSによって確立されるセキュアな通信経路上でHTTP通信を行う仕組みが「HTTP over SSL」「HTTP over TLS」（両者をまとめて「HTTP over SSL/TLS」と表記する）であり、URIスキーム「https」であらわされる。

* アプリケーション層とトランスポート層の間で暗号化が行われる
* ディジタル証明書（公開鍵証明書）を用いてサーバー、クライアントの正当性を相互に認証する
* SMTP、FTP、Telnet、POP3、IMAP4、LDAPなど多くのTCP/IPアプリケーションに対応している