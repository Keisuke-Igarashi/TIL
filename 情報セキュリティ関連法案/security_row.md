# [情報セキュリティ関連法案]()

## 1. 私たちを取り巻く

* 私法:私人と私人との関係を規律する法律（例:民法）
* 公法:私人と国家・行政主体の関係を規律する法律（例:刑法、産業法）
* **ポイント**：判断はあくまで司法が実施するもののため、事前に白黒つけられないという点は意識しておく必要がある。

### 憲法・電気通信事業法

* 通信の秘密とは
  誰にも通信の内容や通信の存在、相手方といった事実を知られずに秘密のうちに通信を行うことができることは、個人の私生活の自由・自由なコミュニケーションの手段を保障する上でも大変重要なことです。このようなことから、憲法等において、通信の秘密が個人として生きていく上で必要不可欠な権利として保障されています。

* 根拠法令等

  * 憲法第21条2項、電気通信事業法第4条第1項、179条（郵便や電話時代に作られたものなので現代のインターネット時代に合っていない部分がある）
  * 各種ガイドライン（電気通信事業者におけるサイバー攻撃等への対処と通信の秘密に関するガイドラインなど（あくまでもガイドライン）

* 通信の秘密の範囲
  個別の通信に係る通信内容のほか、個別の通信に係る通信の日時、場所、通信当事者の氏名、住所・居所、電話番号等の当事者の識別符号等これらの事項を知られることによって通信の意味内容を推知されるような事項全てが含まれる。

  →ログもこれに該当する。

* 侵すの意味
  * 知得:積極的に通信の秘密を知ろうとする意思のもとで知得しようとする行為漏らさなくても知ることだけで侵害にあたる。機械的な収集も知得にあたる。
  * 窃用:発信者又は受信者の意思に反して利用すること
  * 漏えい:他人が知り得る状態に置くこと
    ※通信当事者の同意があれば侵すことにはならない。
    ※違法性阻却事由（違法性をないものにする理由）
    通信当事者の同意を得ることなく通信の秘密を侵した場合であっても、正当防衛（刑法第36条）、緊急避難（刑法第37条）に当たる場合や、正当行為（刑法第35条）に当たる場合等違法性阻却事由がある場合には、例外的に通信の秘密を侵
    すことが許容されること
  * (メモ) 知るだけで犯罪になる可能性。機械的にみることも違法になる可能性もある。例外的に許されるものがある。わかりやすいのは通信当事者の同意。

* まずは、あれ？これ通信の秘密大丈夫？と気づけるようになることが大事
* 電気通信事業者におけるサイバー攻撃等への対処と通信の秘密に関するガイドライン
  https://www.jaipa.or.jp/other/intuse/guideline_v5.pdf
* 電気通信事業における個人情報保護に関するガイドライン
  https://www.soumu.go.jp/main_content/000507466.pdf
* 電気通信事業における個人情報保護に関するガイドライン解説
  https://www.soumu.go.jp/main_content/000603940.pdf

### 刑法（不正指令電磁的記録に関する罪）

* 刑法上はマルウェアはウィルスとして記載される。
* マルウェアを作成しただけで処罰される可能性がある。
* 正当な理由
  * ウィルス対策ソフトの開発・試験などを行う場合
  * ウィルスを発見した人がウィルスの研究機関やウィルス対策ソフトの制作会社へウィルスを提供する場合
* ウイルス（＝「不正指令電磁的記録」）とは
  * 利用者の）意図に沿うべき動作をさせず、又はその意図に反する動作をさせるべ
    き不正な指令を与える電磁的記録
* バグはウィルス罪の対象外
* 正当な理由のもとに作成、提供、取得、保管したということを証拠として残しておく
* 社外に公開する場合は利用者にプログラムの動作を正確に説明する
* 参考資料
* サイバーセキュリティ関係法令Q&AハンドブックVer1.0
  内閣官房内閣サイバーセキュリティセンター（ＮＩＳＣ） (令和２年３月２日)
  Q65 不正プログラムと刑事罰
  <https://www.nisc.go.jp/security-site/files/law_handbook.pdf>
* いわゆるコンピュータ・ウイルスに関する罪について
  法務省
  http://www.moj.go.jp/content/001267498.pdf
* アラートループ事件の被疑者２名に対する起訴猶予処分を受けて
  横浜パーク法律事務所
  <http://www.yokohama-park-law.com/news/20190529.html>

### 不正アクセス禁止法

* 不正アクセス禁止法制定前（～1990年代後半）までは、不正アクセス自体を処罰する法令がなかった

* 本来アクセスする権限のないコンピューターを利用する行為

* 不正アクセスによる被害者において防御策が行われていなかった場
  合、被害者は処罰を求めることはできない点に注意!

* スタンドアロンの場合は、不正アクセス禁止法には該当しないが、その他の不正競争防止法や窃盗罪として処罰される可能性がある。

  

### 刑法（電磁的記録不正作出罪・電子計算機使用詐欺罪）

* 私電磁的記録不正作出罪(例：顧客データベース)
* 公電磁的記録不正作出罪(例：住民基本台帳/自動車登録ファイル)
* 電子計算機使用詐欺罪（窃取したクレジットカード情報を使用する、残額を改ざんしたプリペイドカードを使用する）
  * コンピュータなど人以外を欺く行為が詐欺罪の対象外となることから、この不都合を解消するために作られた詐欺罪の規定です。1987年に新たに設けられました。
