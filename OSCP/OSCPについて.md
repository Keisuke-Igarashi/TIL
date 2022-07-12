# OSCP受験について

## 公式ホームページ
https://www.offensive-security.com/pwk-oscp/

## 試験概要
- 所要時間:48H
- 初めの24時間で5つのマシンへのペネトレーションテスト（ハッキング）をして、後半の24時間でそれについてのレポートをまとめて提出する。
- 5つのマシンは難易度が違って、それぞれにポイントが振り分られてる。1番簡単なマシンは10ポイント。次に20ポイントのマシンが2つ。そして25ポイントのマシンが2つで合計100ポイント。100ポイントのうち70ポイントを取ることが出来たら合格。
- HackTheBoxをイメージすれば試験のイメージがしやすい。
- レポートは英語で記載。
- OSCP試験を受ける前に、PWK(Penetration Testing with kali linux)というコースを受講する必要あり(90日、60日、30日から選べる)
- 費用は15万程度（円高でもっと上がっているかも）

## 学習の順序
- TryHackMeから始めるのがベータ
- 次にHackTheBox(1日1マシン?) Writeupを見ながら勉強していく
- 合格体験記(https://qiita.com/elliot_tk/items/a41654ad30619e4d37f3)では150程度のCTF(Capture The Flag(https://cybersecurity-jp.com/column/33780))をやったとのこと

## レポートについて
- Offensive Security社が出しているレポート例と同じように記述していく。(https://www.offensive-security.com/pwk-online/PWK-Example-Report-v1.pdf)
- スクリーンショットは忘れずに

## その他
- 25ポイントの１つのマシーンはwindowsの"Buffer Overflow"と決まっており、事前に準備することができる唯一のマシン
- １つ１つCheetSheetを作っていくとよいかも
- VHL(Virtual Hacking Labs) ペネトレの基礎が書かれた資料が配布され、それをもとにラボネットワーク内のマシンを攻略していき、そのレポートを提出するとコースの終了証がもらえる（月1万円程度）


## 本ページの記載にあたり参考にしたサイト
https://qiita.com/elliot_tk/items/a41654ad30619e4d37f3  
https://white-lily.hateblo.jp/entry/oscp-review

[マルウェア解析ブログ](https://bankingmalware.hatenablog.com/entry/2020/09/27/154602)
→cheesheetや参考にしたサイトなどわかりやすく書かれている

[Hacktheboxのwirteup(w/o metasploit)](https://rana-khalil.gitbook.io/hack-the-box-oscp-preparation/linux-boxes/lame-writeup-w-o-metasploit)

## その他tips

* webで問題閲覧できるのでgoogle翻訳使える。
* レポートはDeeplearning + gramallyで行ける
* typo使ってwriteup書く