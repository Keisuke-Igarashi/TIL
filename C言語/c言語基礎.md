# C言語基礎

* 特徴：メモリを意識しながら記載する必要がある

  ![image-20220705092027181](C:\Users\nflabs-03\Documents\git\TIL\TIL\C言語\img\image-20220705092027181.png)

* Visual Studio

  * チームエクスプローラーからxxxx.slnを開くことでソリューションエクスプローラーでの読み込みが可能

    ![image-20220705092153807](C:\Users\nflabs-03\Documents\git\TIL\TIL\C言語\img\image-20220705092153807.png)

  * フォルダビュー：作業フォルダに含まれているファイルやフォルダを確認する

  * ソリューションビュー：ソリューション内のプロジェクトを確認することができる。ビルドはここから。

  * コードの実行

    ![image-20220705092341858](C:\Users\nflabs-03\Documents\git\TIL\TIL\C言語\img\image-20220705092341858.png)

  * プロジェクトの作成

    * プロジェクトとソリューションを新規作成

      ![image-20220705111139467](C:\Users\nflabs-03\Documents\git\TIL\TIL\C言語\img\image-20220705111139467.png)

    * プロジェクトのテンプレートを設定する

      ![image-20220705111254309](C:\Users\nflabs-03\Documents\git\TIL\TIL\C言語\img\image-20220705111254309.png)

  * プロジェクトの名前を設定する

    ![image-20220705111342788](C:\Users\nflabs-03\Documents\git\TIL\TIL\C言語\img\image-20220705111342788.png)

  * ソースファイルやヘッダーファイルを追加することでプログラムを実行可能
    * stdio.hなどのインポート用のファイル（ヘッダファイルはプロジェクトのどこに格納されているか）
  
      
  
  * .exeファイルはプロジェクト単位にできるという理解でよいか？確認。
  
    
  
  * exeファイルを実行すると文字化けする（後ほど確認して詳細記載）
  
    → powershellの文字コードが日本語に対応していなかった。powershell上で以下コマンドを実施することで文字化けが解消した。
  
    ```powershell
    chcp 932
    ```
  
    

## 2.変数

* 数値：int

* 文字：char

* 宣言だけでなく一緒に初期化を(初期化忘れのバグ防止につながる)

  ```c
  int day = 1;
  ```

* char型の代入にはシングルクオートを付けること（ダブルだとエラーになる）

  ```c
  char alpabet = 'A';
  ```

* 命名規則と制限

  ![image-20220707150503503](img/image-20220707150503503.png)

* 変数の型

  * 型ごとの最大最小（limits.h, float.h)

    ```c
    #define _CRT_SECURE_NO_WARNINGS
    #include <stdio.h>
    #include <stdlib.h>
    #include <limits.h>
    #include <float.h>
    
    int main(void) {
    	/* char */
    	printf("charの最大値 = %d\n", CHAR_MAX);
    	printf("charの最小値 = %d\n", CHAR_MIN);
    	printf("unsigned char の最大値 = %d\n\n", UCHAR_MAX);
    
    	/* short int */
    	printf("short int の最小値 = %d\n", SHRT_MIN);
    	printf("short int の最大値 = %d\n", SHRT_MAX);
    	printf("usinged short int の最大値 = %u\n\n", USHRT_MAX);
    
    	/* int */
    	printf("int の最小値 = %d\n", INT_MIN);
    	printf("int の最大値 = %d\n", INT_MAX);
    	printf("usigned int の最大値 = %u\n\n", UINT_MAX);
    
    	/* long */
    	printf("long の最小値 = %ld\n", LONG_MIN);
    	printf("long の最大値 = %ld\n", LONG_MAX);
    	printf("usigned long の最大値 = %lu\n", ULONG_MAX);
    
    	/* float型の最小値と最大値 */
    	printf("FLT_MIN = %e\n", -FLT_MAX);
    	printf("FLT_MAX = %e\n", FLT_MAX);
    
    	/* double型の最小値と最大値 */
    	printf("DBL_MIN = %e\n", -DBL_MAX);
    	printf("DBL_MAX = %e\n", DBL_MAX);
    
    #ifdef LDBL_MAX/* LDBL_MAXが定義されている場合のみ表示 */
    	/* long double型の最小値と最大値 */
    	printf("LDBL_MIN = %Le\n", -LDBL_MAX);
    	printf("LDBL_MAX = %Le\n", LDBL_MAX);
    #endif
    
    	return 0;
    }
    ```

  * 実行結果

    ```
    charの最大値 = 127
    charの最小値 = -128
    unsigned char の最大値 = 255
    
    short int の最小値 = -32768
    short int の最大値 = 32767
    usinged short int の最大値 = 65535
    
    int の最小値 = -2147483648
    int の最大値 = 2147483647
    usigned int の最大値 = 4294967295
    
    long の最小値 = -2147483648
    long の最大値 = 2147483647
    usigned long の最大値 = 4294967295
    FLT_MIN = -3.402823e+38
    FLT_MAX = 3.402823e+38
    DBL_MIN = -1.797693e+308
    DBL_MAX = 1.797693e+308
    LDBL_MIN = -1.797693e+308
    LDBL_MAX = 1.797693e+308
    ```

    

## 3.標準入出力

* [参考サイト](https://qiita.com/angel_p_57/items/03582181
  e9f7a69f8168[)

* 標準入力

  * キーボードからの入力
  * scanf関数などを利用する

* 標準出力

  * ディスプレイへの出力
  * printf関数などを利用する

* 変換指定子

  ```c
  printf("Hello! Cworld! 今日は研修%d日目です。", day);
  ```

  ![image-20220707100829682](img/image-20220707100829682.png)

  * 文字リテラルをprintfする場合

    ```c
    #define _CRT_SECURE_NO_WARNINGS
    #include <stdio.h>
    #include <stdlib.h>
    
    int main()
    {
    	char alphabet = 105;
    	printf("%c", alphabet);
    
    	return 0;
    }
    ```

    


* 標準入力scanf関数

  ```c
  scanf( "%d" , &day );
  // 複数同時入力
  scanf("%d %d %d",&i,&j,&k);
  ```

  

  * ありがちなミス

    1. scanf関数は出力関数ではないことに注意→文字を一緒に出力したりはできない。scanf( "%d\n", &day)みたいに書いても期待した動作とはならない。

    2. 変数に**&を付け忘れる**

       

* scanf、printf以外の標準入出力

  * getchar関数
    * 1文字ずつ入力
  * putchar関数
    * 1文字ずつ出力

## 4.演算子

* インクリメント、デクリメント

  * 前置インクリメントと後置インクリメント

    ![image-20220707221505689](img/image-20220707221505689.png)

* 比較演算子

  * 正しい場合（真）は１，（偽）は０を返す

* 論理演算子

  ```c
  #define _CRT_SECURE_NO_WARNINGS
  #include <stdio.h>
  #include <stdlib.h>
  
  /* ===========================================================
   * 演習
   *
   * ===========================================================*/
  int main()
  {
  	int compVal = 0;
  	int a = 10;
  	int b = 20;
  
  	// 1(真)
  	compVal = (10 == a++) && (21 == ++b);
  	printf("compVal = %d\n", compVal);
  	a = 10;
  	b = 20;
  
  	// 0(偽)
  	compVal = (11 == a++) && (21 == ++b);
  	printf("compVal = %d\n", compVal);
  	a = 10;
  	b = 20;
  
  	// 0(偽)
  	compVal = (10 == ++a) && (20 == b++);
  	printf("compVal = %d\n", compVal);
  	a = 10;
  	b = 20;
  
  	// 1(真)
  	compVal = (11 == ++a) && (20 == b++);
  	printf("compVal = %d\n", compVal);
  
  	return 0;
  }
  ```

  

* 演算子の優先順位

  * 見やすいコードにするため()を付ける

    ```c
    a + (b * c)
    ```

  * ()の優先度が一番高い

    ![image-20220707221820315](img/image-20220707221820315.png)

    ![image-20220707221852493](img/image-20220707221852493.png)

  * キャスト演算子

    * 暗黙の型変換

      サイズが小さい方から大きい型への代入は自動的にキャストが行われる

    * 明示的な型変換を行うことが大事

  * P72の記載についてメモ

    a/ 9,0はcastしなくてもfloatになる

  * 演習4.2についてできなかったので復習含めて記載

    ```c
    #define _CRT_SECURE_NO_WARNINGS
    #include <stdio.h>
    #include <stdlib.h>
    
    int main()
    {
    	int a = 0;
    	int b = 0;
    	
    	printf("二つの整数を入力してください\n");
    	printf("整数a : ");
    	scanf("%d", &a);
    	printf("整数b : ");
    	scanf("%d", &b);
        
    	double f = (double) a / b * 100;
    	printf("aの値はbの%lf%%です", f);
    	return 0;
    
    }
    ```

    


## 5.制御構造

* 順次
* 条件分岐
  * if文さえ知っていればswitchは知らなくてもどうにかなる
  * switch分は比較対象が1つであることがわかりやすい。
* 繰り返し処理

##　6.配列

* 配列

  ```c
  int array[3] = {10, 20, 30};
  ```

  * 宣言の場合は、要素数に変数は利用できないことに注意。宣言後は変数名を利用できる。
  * 暗黙の0の初期化。例えば要素数を4として、3つしか定義しなかった場合は暗黙で0がはいる。文字の場合は「\0」（NULL文字）

* 文字列

  * c言語での文字列はchar型の配列となる。

    ```c
    char str[] = {'a', 'b', 'c', 'd', '\0'};
    ```

    * \0：NULL文字
    * 文字列の宣言をする際に配列の最後にNULL文字が必ず必要となる。つまりN文字の文字列を格納するためにはすくなくともN+1の文字分の大きさが必要になるということ

  * 文字列の宣言の例

    ```c
    char str1[5] = "abcd";
    char str2[] = "abcd";
    char str3[5] = {'a', 'b', 'c', 'd'};
    char str4[5] = {'a', 'b', 'c', 'd', '\0'};
    char str5[] = {'a', 'b', 'c', 'd', '\0'};
    ```

  * 文字リテラルと文字列リテラル

    * 文字リテラル

      * ''でくくった文字（1文字）

      * 数値に置き換えられる

    * 文字列リテラル

      * ""でくくった文字の並び
      * 終わりには自動的にNULL文字が付加される
      * 途中にNULL文字が入ることもある（この場合文字列ではない)

    * 文字列

      * NULL文字以外の文字が並び、最後がNULL文字で終わる
      * char型の配列を使うことが多い

  * 文字列の長さ

    ```c
    char str[] = "C Language Programming";
    printf("%d¥n", sizeof(str));
    ```

    * あくまでも配列のメモリサイズであることに留意
    * NULL文字を含めた文字数が表示されることに注意

  * 文字リテラルの暗号化解読（シーザー暗号）

    ```c
    #define _CRT_SECURE_NO_WARNINGS
    #include <stdio.h>
    #include <stdlib.h>
    
    /* ===========================================================
     * 演習
     * 13文字分だけずれているアルファベットの文字列を、
     * 元の文字列に戻してください。
     * ===========================================================*/
    int main()
    {
    	char str[] = "P Cebtenzzvat Ynathntr!";
    	int x = 0;
    	int y = 0;
    
    	for (int i = 0; i < sizeof(str); i++) {
    	
    		
    		x = (int)str[i];
    
    		if (65 <= x && x <= 90) {
    			
    			x = (int)str[i] - 13;
    
    			if (x < 65) {
    				y = 65 - x;
    				x = 90 - y + 1;
    			}
    			
    			printf("%c", x);
    
    		}
    		else if (97 <= x && x <= 122) {
    
    			x = (int)str[i] - 13;
    
    			if (x < 97) {
    				y = 97 - x;
    				x = 122 - y + 1;
    			}
    
    			printf("%c", x);
    
    		}
    		else {
    
    			x = (int)str[i];
    			printf("%c", x);
    		}
    	}
    
        return 0;
    }
    ```

    

* 二次元配列

  ```c
  int main() {
  	char str[3][8] ={ "hoge", "fuga", "hoga" };
  	printf("%s¥n", str[0]); //hoge
  	printf("%s¥n", str[1]); //fuga
  	printf("%s¥n", str[2]); //hoga
  }
  ```

  * 九九表のサンプルコード

    ```c
    #define _CRT_SECURE_NO_WARNINGS
    #include <stdio.h>
    #include <stdlib.h>
    
    /* ===========================================================
     * 演習
     * 二次元配列を使って九九表を作成し、表示してください。
     * ===========================================================*/
    int main()
    {
    	int num[9][9];
    	int result = 0;
    	int temp = 0;
    
    	for (int i = 1; i < 10; i++) {
    		for (int j = 1; j < 10; j++) {
    			num[i-1][j-1] = i * j;
    			result = i * j;
    		}
    	}
    
    	for (int i = 0; i < 9; i++) {
    		for (int j = 0; j < 9; j++) {
    			printf("%3d", num[i][j]);			
    		}
    		printf("\n");
    	}
    	return 0;
    }
    ```

    

## 7.関数

* 基礎

  ```c
  /* ===========================================================
   * 演習
   * 高さと底辺を引数として平行四辺形の面積を返す関数を実装してください。
   *
   * ===========================================================*/
  
  
  int main() {
  	int x = 0;
  	int y = 0;
  	int result = 0;
  
  	printf("高さを入力してください:");
  	scanf("%d", &x);
  	printf("底辺を入力してください:");
  	scanf("%d", &y);
  
  	/*ここで関数を呼び出し、resultに値を代入してください*/
  	result = func(x, y);
  
  	printf("面積は%dです。\n", result);
  
  	return 0;
  }
  
  int func(int x, int y) {
  	int result = 0;
  	result = x * y;
  	return result;
  }
  ```

  * プロトタイプ宣言

    ```c
    int func (int, int );
    ```

    https://monozukuri-c.com/langc-funclist-prototype/

  * 関数の引数の渡し方は値渡しである。呼び出し元と呼び出し先は変数の共有が不可能

  

* 再帰関数

  * 階乗

    ```c
    #define _CRT_SECURE_NO_WARNINGS
    #include <stdio.h>
    #include <stdlib.h>
    
    /* ===========================================================
     * 演習
     * 正の整数の階乗を計算してください。
     * 再帰関数を使う場合と使わない場合の2種類の関数を用意してください。
     * ===========================================================*/
    
    //	再帰呼び出しをしない版
    int factorial(int num) {
    	int result = 1;
    	for (int i = 1; i <= num; i++) {
    		//printf("%d", i);
    		result = result * i;
    
    	}
    	return result;
    }
    
    //	再帰呼び出しをする版
    
    int factorial_recursive(int num) {
    
    	if ( num > 1) {
    		return num * factorial_recursive(num - 1);
    	}
    	else {
    		return 1;
    	}
    		
    }
    
    
    int main()
    {
    //	再帰呼び出しをしない版の関数呼び出し
    //	factorial()
    //	結果を表示
    
    	int num = 0;
    	int result1 = 0;
    	int result2 = 0;
    
    	while (1) {
    		scanf("%d", &num);
    
    		if (num == -1) {
    			break;
    		}
    
    		result1 = factorial(num);
    		printf("factorial(%d)=%d\n", num, result1);
    
    		//	再帰呼び出しをする版の関数呼び出し
    		//	factorial_recursive()
    		//	結果を表示
    
    		result2 = factorial_recursive(num);
    		printf("factorial_recursive(%d)=%d\n", num, result2);
    
    	}
    	return 0;
    }
    ```

    

  * Ex7.3を実施すること

  * フィボナッチ数列

    ```C
    * ===========================================================
     * 演習
     * N番目のフィボナッチ数を求めてください。
     * 再帰関数を使う場合と使わない場合の2種類の関数を用意してください。
     * ===========================================================*/
    // 再帰呼び出しをしない版
    int fib(int num)
    {
    
    	int fib[100];
    
    	for (int i = 0; i <= num; i++) {
    
    		if (i == 0 || i == 1) {
    			fib[i] = 1;
    		}
    		else {
    			fib[i] = fib[i-1] + fib[i-2];
    		}
    	}
    
    	return fib[num-1];
    
    }
    // 再帰呼び出しをする版
    int fib_recursive(int num)
    {
    
    	int fib[100];
    
    	if (num == 1 || num == 2) {
    		return 1;
    	}
    	else {
    
    		fib[num] = fib_recursive(num - 1) + fib_recursive(num - 2);
    		return fib[num];
    	}
    }
    
    int main()
    {
    	int n = 0;
    
    	printf("Input number > ");
    	scanf("%d", &n);
    
    	printf("%d\n", fib(n));
    	printf("%d\n", fib_recursive(n));
    
    	return 0;
    }
    
    ```

    

  * Ex7.4を実施すること

  

## 補足：デバッグ方法

* デバッグ：プログラムのバグの原因を特定し、修正すること
* デバッガー：デバッグを実行するツールであり、バグを特定するための
  様々な機能を備える
* デバッギー：デバッグ対象のプログラムのこと

* 変数ウォッチ
  * ローカル変数ウィンドウ
  * ウォッチウィンドウ
    * 変数の値変更が可能
* 各種ビューワー（ウィンドウがなければ「デバック→ウィンドウ」にそれぞれ有）
  * モジュール
    * DLL：C言語のライブラリのビルドファイル
  * メモリ
  * 逆アセンブリ
  * レジスタ

## DebugビルドとReleaseビルド

* 
* リンク方式
  * 静的リンク：オブジェクトファイルにライブラリを埋め込む。マルウェアはこっちが多い。
  * 動的リンク：実行時にライブラリコードを読み込む。.dllファイルから読み込むためライブラリファイルがない場合ソースが動かない。

## 8.メモリ

* メモリ

  * CPUは1byte(8bit)単位でメモリを認識する![image-20220708100950406](img/image-20220708100950406.png)
  * メモリの世界は16進数で表現される![image-20220708101135690](img/image-20220708101135690.png)

* sizeof

  * sizeof(型)で型が宣言時に確保するメモリのサイズを取得
  * sizeof(変数)/sizeof(型)で配列の数を算出できる
  * 後ほどここもっと充実させてノート書く

* 変数のサイズ

* メモリと番地

* 変数確保のイメージ

* エンディアン

* Visual Studio上でアドレスが動的に変化してしまう場合は以下設定を行う

  C/C++ → 最適化→ 最適化
  →無効
  C/C++ → コード生成→ セキュリティチェック
  →無効
  リンカー→ 詳細設定→ ランダム化されたベースアドレス
  →いいえ

* Ex9.1

  ```c
  #define _CRT_SECURE_NO_WARNINGS
  #include <stdio.h>
  #include <stdlib.h>
  
  /* ===========================================================
   * 演習
   * 変数のアドレスを直接指定することで値を書き換えられることを確認して下さい。
   * ===========================================================*/
  int main()
  {
  	int p = 100;
  
  	// 変数pのアドレスを表示
  	printf("p address = 0x%p\n", &p);
  
  	// 変更前の変数pの表示
  	printf("p = %d\n", p);
  
  	// 変数pのアドレスを直接指定して書き換え
  	*(int *)0x0019FEDC = 9999;
  
  	// 変更後の変数pの表示
  	printf("p = %d\n", p);
  
  	return 0;
  }
  ```

  

  * プログラム実行前

  ```
  p address = 0x0019FEDC
  p = 100
  p = 100
  ```

  * プログラム実行後

  ```
  p address = 0x0019FEDC
  p = 100
  p = 9999
  ```

  

## 9.ポインタ

* 変数とアドレス

  * %fでアドレスを表示

* ポインタ

  * ポインタ宣言時に型も併せて宣言する。ポインタが参照する変数もポインタの型に依存する

  * ポインタを使用するには以下の段階を踏む必要がある

    * 宣言
    * アドレス(値)の設定
    *  使用	

    ```c
    #define _CRT_SECURE_NO_WARNINGS
    #include <stdio.h>
    #include <stdlib.h>
    
    /* ===========================================================
     * 演習
     * 変数の値をポインタを用いて変更してください。
     * ===========================================================*/
    int main()
    {
    	int a = 100;
    	int *p;		//int型のポインタを宣言
    	p = &a;		//pに変数aのアドレスを代入
    	*p = 1000;	//間接参照演算子*を利用してaに1000を代入
    
    	printf("a=%d", a);
    
    	return 0;
    }
    ```

    

* ポインタと関数 アドレス渡し

* 配列とアドレス

* ダブルポインタ

## 10.変数のスコープ

## 11.動的なメモリ割り当て

* スタック領域とヒープ領域
* malloc関数とfree関数

## 12.構造体

* typedef
* 単方向リスト

## 13.プリプロセッサ

## 14.ファイル入出力

## 15.標準Cライブラリ

* string.h
* ctype.h
* math.h
* stdlib.h

