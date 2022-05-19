# Subdomain Enumeration

正しいサブドメインを見つける方法。
範囲を正確に把握するために必要。

![サブドメインとは](https://ja.wix.com/blog/2020/12/what-is-a-subdomain/?utm_source=google&utm_medium=cpc&utm_campaign=12446647374^119260858238&experiment_id=^^501686756794^^_DSA&gclid=Cj0KCQjwspKUBhCvARIsAB2IYuuHbwy-bzHef2-x1s9A8pR-yyOzqsllbddfEHUa5JkSmkMqf0g2jRAaAnqCEALw_wcB)

ドメインの前につけるドメインのこと
store.tryhackme.com
のstoreなど。別のコンテンツを割り当てる。


以下3種類を紹介

- Brute Force
- OSINT
- Virtual Host

## Task2 OSINT -SSL/TLS Certificates

When an SSL/TLS (Secure Sockets Layer/Transport Layer Security) certificate is created for a domain by a CA (Certificate Authority), CA's take part in what's called "Certificate Transparency (CT) logs". 

ドメインネームから作られるこのログは誰でも見れる。
'http://crt.sh/'
'https://transparencyreport.google.com/https/certificates'

## Task3 OSINT - Search Engines

Google, such as the site: filter, can narrow the search results. For example, "-site:www.domain.com site:*.domain.com" would only contain results leading to the domain name domain.com but exclude any links to www.domain.com; therefore, it shows us only subdomain names belonging to domain.com.  

```
site:www.tryhackme.com site:*.tryhackme.com
```

## Task4 DNS Bruteforce

dnsreconを利用するのも一例  

```
dnsrecon -t brt -d acmeitsupport.thm
```

## Task 5 OSINT -sublist3r

- [sublist3rのGitHub](https://github.com/aboul3la/Sublist3r)

サンプルコード
```
./sublist3r.py -d acmitsupport.thm
```

## Task6 Virual Hosts

webserverはホストヘッダーを確認して、どのwebサイトコンテンツを返却するかを決める。  
その仕組みを利用して、wordlistからランダムにリクエストを送ることでサブドメインを特定  
する方法。（**DNSサーバーには登録前のhostsなどのサイト情報も取得できる可能性がある**）  

```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host:
FUZZ.acmeitsupport.thm" -u http://10.10.158.245 -fs {size}
```

-w : wordlistを指定
-H : headerを指定
-fs: 該当のサイズをフィルタリング

これでサイズが他のレスポンスと異なる結果をサブドメインとして特定する。


