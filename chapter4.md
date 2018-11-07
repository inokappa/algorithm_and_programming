# 4 章. ループの応用
<!--ts-->
   * [4 章. ループの応用](#4-章-ループの応用)
      * [環境](#環境)
      * [1. モンテカルロ法 (Monte Carlo method)](#1-モンテカルロ法-monte-carlo-method)
      * [2. モンテカルロ法による円周率計算のプログラム](#2-モンテカルロ法による円周率計算のプログラム)
      * [3. 計算量](#3-計算量)
      * [演習問題](#演習問題)
         * [問 4.1](#問-41)
         * [問 4.2](#問-42)
         * [問 4.3](#問-43)
         * [問 4.3](#問-43-1)
         * [問 4.4](#問-44)
      * [以上](#以上)

<!-- Added by: user, at: 2018-11-07T22:58+00:00 -->

<!--te-->
## 環境

本メモで利用する環境は以下の通り.

```sh
root@0be431eebb77:/work# cat /etc/debian_version
9.5

root@0be431eebb77:/work# gcc -v
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/lib/gcc/x86_64-linux-gnu/6/lto-wrapper
Target: x86_64-linux-gnu
Configured with: ../src/configure -v --with-pkgversion='Debian 6.3.0-18+deb9u1' --with-bugurl=file:///usr/share/doc/gcc-6/README.Bugs --enable-languages=c,ada,c++,java,go,d,fortran,objc,obj-c++ --prefix=/usr --program-suffix=-6 --program-prefix=x86_64-linux-gnu- --enable-shared --enable-linker-build-id --libexecdir=/usr/lib --without-included-gettext --enable-threads=posix --libdir=/usr/lib --enable-nls --with-sysroot=/ --enable-clocale=gnu --enable-libstdcxx-debug --enable-libstdcxx-time=yes --with-default-libstdcxx-abi=new --enable-gnu-unique-object --disable-vtable-verify --enable-libmpx --enable-plugin --enable-default-pie --with-system-zlib --disable-browser-plugin --enable-java-awt=gtk --enable-gtk-cairo --with-java-home=/usr/lib/jvm/java-1.5.0-gcj-6-amd64/jre --enable-java-home --with-jvm-root-dir=/usr/lib/jvm/java-1.5.0-gcj-6-amd64 --with-jvm-jar-dir=/usr/lib/jvm-exports/java-1.5.0-gcj-6-amd64 --with-arch-directory=amd64 --with-ecj-jar=/usr/share/java/eclipse-ecj.jar --with-target-system-zlib --enable-objc-gc=auto --enable-multiarch --with-arch-32=i686 --with-abi=m64 --with-multilib-list=m32,m64,mx32 --enable-multilib --with-tune=generic --enable-checking=release --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=x86_64-linux-gnu
Thread model: posix
gcc version 6.3.0 20170516 (Debian 6.3.0-18+deb9u1)
```

## 1. モンテカルロ法 (Monte Carlo method)

* 乱数を利用した確率的な数値計算シュミレーションによって問題の近似解等を求める方法
* モンテカルロ法を用いた有名なプログラムの例として円周率 π を求めるものがある

半径 $r$ が 1 の円とそれを取り囲む変の長さが 2 の正方形がある. 4 分割された円の面積を $A_{circle}$ とし, 1 x 1 の正方形の面積を $A_{square}$ とすると, 点が斜面の部分に打たれる確率 $p$ は以下の式で表される.

$$ p = \frac{A_{circle}}{A_{square}} = \frac{\frac{1}{4}\pi r^2}{r^2} = \frac{\pi}{4} $$

この式を変更し $\pi$ を求めると以下のような式となる.

$$ \pi = 4p = 4\times\frac{A_{circle}}{A_{square}} $$

$p$ はランダムに打たれた点の数の比率 $(A_{circle}\div A_{square})$ を計算することによって求めることが出来る.

## 2. モンテカルロ法による円周率計算のプログラム

点の座標値 $(x, y)$ を乱数として発生させ, $A_{circle}$ の部分に打たれる点の個数と $A_{square}$ の部分に打たれる点の個数を求めれば円周率を求めることが出来る.

以下のコードは, モンテカルロ法により円周率 $\pi$ を求めることが出来る.

```c
/* code: ex4-1.c */
#include <stdio.h>
#include <stdlib.h>

#define POINTS 1000

int main()
{
  int i, count, points;
  double x, y, q;
  double pi;
  
  points = POINTS;
  count = 0;
  
  for (i = 0; i < points; i++) {
    x = (double) rand () / ((double) RAND_MAX + 1.0);
    y = (double) rand () / ((double) RAND_MAX + 1.0);
    q = (x * y) + (x * y);
    
    if (q <= 1.00)
      count++;
  }
  
  pi = (double) count / (double) points *(double) 4.00;
  printf ("circle: %d\t", count);
  printf ("square: %d\t", points);
  printf ("PI: %f\n", pi);
  
  return 0;
}
```

* 0 以上 RAND_MAX 以下の数値を発生させる rand 関数 (0 $\leq$ rand() $\leq$ RAND_MAX) を式に代入し, 0 以上 1 未満になるような乱数を発生させている
* 乱数は 1000 個の点の座標値 (x, y) として使われ, $A_{circle}$ の部分に打たれる点なのか, $A_{square}$ の部分に打たれる点なのか判定し, その合計数を求めている

以下の実行例では, $A_{circle}$ に打たれる点が 829 個, $A_{square}$ の部分に打たれる点が 1000 個となり, 円周率が 3.316000 と計算される.

```sh
root@0be431eebb77:/work# gcc ex4-1.c -o ex4-1
root@0be431eebb77:/work# ./ex4-1
circle: 829     square: 1000    PI: 3.316000
```

理論的には, シュミレーションに用いる点の個数が増加すれば, 増加するほど精度の高い円周率を得ることが出来る.

以下のコードは, シュミレーションに利用する点の個数を変化させながら円周率を求めるプログラムで for ループを利用して点の数を 10 個から 10 倍ずつ増加させて最終的に 10 億個になるシュミレーションである.

```c
/* code: ex4-2.c */
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

int main()
{
  int i, j, count, points;
  double x, y, q;
  double pi;

  for (j = 1; j < 10; j++) {
    points = 1;
    count = 0;
    points = points * pow (10, j);
    for (i = 0; i < points; i++) {
      x = (double) rand () / ((double) RAND_MAX + 1.0);
      y = (double) rand () / ((double) RAND_MAX + 1.0);
      q = (x * y) + (x * y);
      if (q <= 1.00)
        count++;
    }
    pi = (double) count / (double) points *(double) 4.00;
    printf ("circle: %d\t", count);
    printf ("square: %d\t", points);
    printf ("PI: %f (%+f)\n", pi, (pi - M_PI));
  }

  return 0;
}
```

これをコンパイルして実行すると以下のように出力される. `M_PI` は, math.h に定義されている円周率の値が定義されており, シュミレーションの値の結果との差分を出力している.

```sh
root@0be431eebb77:/work# gcc ex4-2.c -lm -o ex4-2 -g3
root@0be431eebb77:/work# ./ex4-2
circle: 8       square: 10      PI: 3.200000 (+0.058407)
circle: 84      square: 100     PI: 3.360000 (+0.218407)
circle: 835     square: 1000    PI: 3.340000 (+0.198407)
circle: 8533    square: 10000   PI: 3.413200 (+0.271607)
circle: 84543   square: 100000  PI: 3.381720 (+0.240127)
circle: 846860  square: 1000000 PI: 3.387440 (+0.245847)
circle: 8464359 square: 10000000        PI: 3.385744 (+0.244151)
circle: 84655640        square: 100000000       PI: 3.386226 (+0.244633)
circle: 846560173       square: 1000000000      PI: 3.386241 (+0.244648)
```

教科書の出力だと, 点の数が増えるに従ってより正確な円周率に近づく傾向が見られているが, 手元の環境では点の数が増えても正確な円周率に近づくことはなかった...のは何故だろう... (システムによって乱数生成アルゴリズムが異なるので, 計算結果が異なると記述されている)

おそらく, 上記の疑問に対する答えの一つだと思うが, 上記のコードは rand 関数を利用しているが, 乱数の質はコンパイラの実装に依存している為, 非常に古いコンパイラによっては乱数の質が良くない場合がある為, drand48 (線形合同アルゴリズムと 48 ビット整数演算に基づく) や random 関数 (非線形加法フィードバックに基づく), その他の高品質な疑似乱数生成器 (PRNGs) の利用が望ましい とのこと.

## 3. 計算量

* 計算量 (computational complexity) とはアルゴリズムの動作に必要な資源の量を評価するもの
* 時間計算量 (time complexity) と空間計算量 (space complexity) があり, 通常は単に計算量と言えば, 時間計算量のことを指す
* 時間計算量では, 計算に必要なステップ数を評価する
* 評価では複数のアルゴリズムがあり, その評価を行う場合, 計算時間はコンピュータの処理能力によって異なる為, 入力データの大きさに対する基本演算のステップ回数で比較する必要がある
* 空間計算量は, 領域計算量とも呼ばれることがあり, 計算に必要とされるメモリ量を評価する
* アルゴリズムの速度比較方法 = ビッグ・オー記法 (big O notation)
* ビッグ・オー記法によって, データの数と実行時間の関係を表現することが出来る

例えば, 配列の線形探索においては, $n$ 個のデータに対して, 計算量 $O(n)$ の時間が掛かる. また, 配列の二分探索の場合は計算量 $O(\log n)$ となる.

以下は様々な関数における増加率.

* 対数時間 (logarithmic time) ... $\log_{2}n$, $n\log_{2}n$
* 多項式時間 (polynomial time) ... $n^2$, $n^3$
* 指数関数時間 (exponential time) ... $2^n$, $n^n$, $n!$

## 演習問題

### 問 4.1

モンテカルロ法について簡単に説明しなさい.

* 乱数を利用した確率的な数値計算シュミレーションによって問題の近似解等を求める方法
* https://unit.aist.go.jp/diversity/ja/jst/teens/montecarlo.htm このサイトの解説が, 自分のような人間には解りやすかった

### 問 4.2

以下のビッグ・オー記法のを関数増加率の大きさで小さい順に並べなさい. $c$ 定数, $!$ は階乗を表し, $n$ は非常に大きな値とする.

$O(logn)$, $O(n)$, $O(nlogn)$, $O(c)$, $O(n^c)$, $O(n!)$

以下, 解答より引用.

$O(c)$ $<$ $O(logn)$ $<$ $O(n)$ $<$ $O(nlogn)$ $<$ $O(n^c)$ $<$ $O(n!)$

### 問 4.3

C 言語の標準ライブラリ stdlib.h に定義されている rand 関数と srand 関数について調べなさい.

以下, 解答より引用.


| **関数** | **詳細** |
|:---|:---|
| rand | 疑似乱数を発生させる |
| srand | rand 関数で返される疑似乱数の乱数種 (seed) を設定する, srand 関数にて同一の乱数種の値で呼び出した場合, rand 関数から同じ疑似乱数列が生成される |

以下, rand 関数と srand 関数を用いて疑似乱数を生成するサンプル. [C言語関数辞典 - rand](http://www.c-tipsref.com/reference/stdlib/rand.html) より引用させて頂いた. 有難うございます.

```c
/* code: rand-sample.c */
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

/* main */
int main(void) {
    int i, n;

    /* 乱数のシードを与える */
    srand((unsigned)time(NULL));

    for ( i = 0; i <= 9; i++ ) {
        /* 乱数を発生させる */
        n = rand();

        /* 表示 */
        printf("%2d回目 = %3d\n", i + 1, n);
    }
    return EXIT_SUCCESS;
}
```

これをコンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc rand-sample.c -o rand-sample -g3
root@0be431eebb77:/work# ./rand-sample
 1回目 = 1471756858
 2回目 = 1381332833
 3回目 = 1717719774
 4回目 = 1863423450
 5回目 = 1715428247
 6回目 = 799290199
 7回目 = 1453794275
 8回目 = 1058746752
 9回目 = 2001340863
10回目 = 692996476
```

### 問 4.3

円周率を求めるアルゴリズムにはどのようなものがあるか調査しなさい.

* AGM 法 (ガウス・ルジャンドルアルゴリズム, 算術幾何平均法) ... めっちゃ速かった
* ライプニッツの公式 ... 遅いらしい
* モンテカルロ法 ... AGM 法に比べるとめっちゃ遅い

これら以外に, 級数展開による方法, アレキメデスの方法 (正多角形による方法) 等がある. (解答より引用)

尚, [3月14日は円周率の日！ということで円周率を求めるプログラムを書こう！](https://qiita.com/sassy_watson/items/75e8bd85d02ab991ee1b) のコードを実際に試してみたが, 以下のように AGM 法の処理は爆速だった. 有難うございます.

```sh
# AGM 法
root@0be431eebb77:/work# time ./pi3
3.14057925052216857509
3.14159264621354283875
3.14159265358979400418
3.14159265358979400418
3.14159265358979400418

real    0m0.004s
user    0m0.000s
sys     0m0.000s

# モンテカルロ法
root@0be431eebb77:/work# time ./pi2
3.200000000
3.120000000
3.156000000
3.166800000
3.138880000
3.141920000
3.140898800
3.141709760
3.141604092

real    0m24.512s
user    0m24.460s
sys     0m0.000s
root@0be431eebb77:/work#
```

### 問 4.4

円周率計算を利用して CPU 演算速度を測るソフトウェアがある. 一般的なコンピュータで約 100 万桁の円周率を求めるのにかかる時間を調査しなさい.

EC2 (CentOS 7, t2.micro) で実施した場合, Super Pi では以下のように Segmentation fault で計測出来なった.

```sh
$ ./super_pi
./super_pi: 1 行:  1538 Segmentation fault      ./pi $1
```

[TachusPI](https://bellard.org/pi/pi2700e9/tpi.html) を利用して計測したところ, 以下のような結果となった.

```sh
$ ./tpi -o pi.txt 1000000
Using 790MiB of RAM
Computation to 1000000 digits, formula=Chudnovsky
Output file=pi.txt, format=txt, binary result size=415kB
Binary Splitting
Depth=17
  mem   max  disk   max operation                       compl lv
17.3M 17.3M     0     0 completed                      100.0%  0
  time = 0.352 s
Compute P, Q
16.1M 17.3M     0     0 completed
  time = 0.004 s
Division
17.9M 17.9M     0     0 completed
  time = 0.034 s
Sqrt
17.3M 17.9M     0     0 completed
  time = 0.023 s
Final multiplication
20.0M 20.0M     0     0 completed
  time = 0.017 s
Total time (binary result) = 0.431 s
Base conversion
17.3M 20.0M     0     0 completed
  time = 0.067 s
Total time (base 10 result) = 0.498 s
Writing result to 'pi.txt'
```

## 以上

* アルゴリズムの勉強っぽくなってきたな...ついていけるかな...でも, 面白い章だった
* Super Pi が EC2 でダウンロード出来なかったり, Segmentation fault したりするのは辛かった
