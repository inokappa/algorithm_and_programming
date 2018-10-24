# 1 章. プログラミング
<!--ts-->
   * [1 章. プログラミング](#1-章-プログラミング)
      * [1. アルゴリズムとプログラミング言語](#1-アルゴリズムとプログラミング言語)
         * [抜粋](#抜粋)
         * [メモ](#メモ)
      * [2. コンパイラ](#2-コンパイラ)
         * [抜粋](#抜粋-1)
         * [メモ](#メモ-1)
      * [3. 入出力と演算](#3-入出力と演算)
         * [3.1 出力と入力](#31-出力と入力)
         * [3.2 変数](#32-変数)
         * [3.3 算術演算 (arithmetic operator)](#33-算術演算-arithmetic-operator)
         * [3.4 変数と式](#34-変数と式)
         * [3.5 プログラム中のコメント](#35-プログラム中のコメント)
      * [演習問題](#演習問題)
         * [問 1.1](#問-11)
         * [問 1.2](#問-12)
         * [問 1.3](#問-13)
      * [以上](#以上)

<!-- Added by: user, at: 2018-10-24T00:38+00:00 -->

<!--te-->
## 1. アルゴリズムとプログラミング言語

### 抜粋

* プログラム (program) とは, コンピュータに対する処理を記述したもの
* プログラムによって問題を解く為の処理手順はアルゴリズム (algorithm) と呼ばれる
* コンパイラ (compiler) は, プログラムをコンピュータが実行可能な機械語に変換するソフトウェア
* インタプリタ (interpreter) は, ソースコードを逐次変換しながら実行しているくソフトウェア

### メモ

* プログラミング言語の数が数千あると書かれていて驚いた
* 人気のあるプログラミング言語ランキングで一位が Java, 二位が C, 三位が C++, 四位が Python と続いている

## 2. コンパイラ

### 抜粋

* コンパイラ (compiler) は, プログラムをコンピュータが実行可能な機械語に変換するソフトウェア
* ソースプログラムは最終的に実行形式であるターゲットプログラムに変換される

以下はターゲットプログラムへの変換過程.

```
ソースプログラム (source program)
↓
文法解析器 (syntax analyzer)
↓
意味解析器 (semantic analyzer)
↓
中間コード生成器 (intermediate code generator)
↓
最適化コード生成器 (code optimizer)
↓
コード生成器 (code generator)
↓
ターゲットプログラム (target program)
```

* 教材のコードは gcc や clang 等コンパイルが可能

### メモ

* コンパイルの過程で実は色んな仕事をしているんだな...
* gcc = GNU Compiler Collection
 
MacOS X に導入されていいる gcc は以下のようなもの.

```sh
$ sw_vers
ProductName:    Mac OS X
ProductVersion: 10.13.6
BuildVersion:   17G65
$ gcc -v
Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
Apple LLVM version 9.1.0 (clang-902.0.39.2)
Target: x86_64-apple-darwin17.7.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bin
```

尚, 以後は上記の gcc を利用してコードをコンパイルしていく.

## 3. 入出力と演算

### 3.1 出力と入力

* ヘッダファイル stdio.h には printf 関数をはじめ, 様々な入出力に関連したライブラリ関数が含まれている
* C 言語では必ず 1 つの main 関数を持つ

以下のコードは標準出力に文字データを出力するプログラム.

```c
/* code: ex1-1.c */
#include <stdio.h>

int main()
{
  printf ("The Open University of Japan\n");
  
  return 0;
}
```

コンパイルして実行してみる.

```sh
$ gcc ex1-1.c -o ex1-1
$ ./ex1-1; echo $?
The Open University of Japan
0
```

main 関数は整数値を返し, プログラムを呼び出したプロセスに値 0 を返す.

以下のコードは scanf 関数を利用した入力の例, 整数型のデータ入力を要求し, 入力されたデータを表示する.

```c
/* code: ex1-2.c */
#include <stdio.h>

int main()
{
  int a;
  printf ("Enter an integer: ");
  scanf ("%d", &a);
  printf ("The integer you entered was %d.\n", a);
  
  return 0;
}
```

コンパイルして実行してみる.

```sh
$ gcc ex1-2.c -o ex1-2
$ ./ex1-2
Enter an integer: 1000
The integer you entered was 1000.
```

scanf 関数はキーボード等の標準入力からのデータ入力を行うが, リダイレクションによるデータ入力も可能である.

```sh
$ echo 100 | ./ex1-2
Enter an integer: The integer you entered was 100.

$ echo 200 > ex1-2.txt
$ ./ex1-2 < ex1-2.txt
Enter an integer: The integer you entered was 200.
```

### 3.2 変数

* C 言語では変数を使用する前に, 変数の宣言を行う必要がある
* 変数の宣言では, 使用する変数の型を指定する
* データ型によって値を保存する為に使われるメモリサイズは異なる, また, OS によってもメモリサイズは異なる

以下, データ型の一覧.

| **データ型** | **キーワード** | **キーワードの英語** |
|:---|:---|:---|
| 文字列 | char | character |
| 整数 | int | integer |
| 浮動小数点数 | float | floting-point |
| 倍精度浮動小数点数 | double | double precision floating-point |
| 値無し | void | void |

以下のコードは sizeof 演算子を使ってデータ型のメモリサイズを表示するプログラムである.

```c
/* code: ex1-3.c */
#include <stdio.h>

int main()
{
  char a;
  short b;
  int c;
  long d;
  float e;
  double f;
  
  printf ("char:   %zd byte (s)\n", sizeof (a));
  printf ("short:  %zd byte (s)\n", sizeof (b));
  printf ("int:    %zd byte (s)\n", sizeof (c));
  printf ("long:   %zd byte (s)\n", sizeof (d));
  printf ("float:  %zd byte (s)\n", sizeof (e));
  printf ("double: %zd byte (s)\n", sizeof (f));
  
  return 0;
}
```

コンパイルして実行してみる.

```sh
$ gcc ex1-3.c -o ex1-3
$ ./ex1-3
char:   1 byte (s)
short:  2 byte (s)
int:    4 byte (s)
long:   8 byte (s)
float:  4 byte (s)
double: 8 byte (s)
```

### 3.3 算術演算 (arithmetic operator)

* 演算子には優先順位があり, 乗算, 除算, 剰余は加算, 減算の演算子よりも優先度が高い

| **演算** | **演算子** |
|:---|:---|
| 加算 | `+` |
| 減算 | `-` |
| 乗算 | `*` |
| 除算 | `/` |
| 剰余 | `%` |

以下のコードは整数型の変数に対して, 算術演算を行うプログラムである.

```c
/* code: ex1-4.c */
#include <stdio.h>

int main()
{
  int a, b, c;
  a = 10;
  b = 3;
  c = 0;
  printf ("a=%d\n", a);
  printf ("b=%d\n\n", b);
  c = a + b;
  printf ("a + b = %d\n", c);
  c = a - b;
  printf ("a - b = %d\n", c);
  c = a * b;
  printf ("a * b = %d\n", c);
  c = a / b;
  printf ("a / b = %d\n", c);
  c = a % b;
  printf ("a %% b = %d\n", c);
  
  return 0;
}
```

コンパイルして実行する.

```sh
$ gcc ex1-4.c -o ex1-4
$ ./ex1-4
a=10
b=3

a + b = 13
a - b = 7
a * b = 30
a / b = 3
a % b = 1
```

* 算術関数は math.h をインクルードして利用する
* gcc や clang 等のコンパイラでコンパイルする場合には `-lm` オプションを付与する必要がある

以下は math.h 定義されている関数の一例.

| 計算 | double 型 | float 型 | long double 型 |
|:---|:---|:---|
| sin の計算 | sin | sinf | sinl |
| cos の計算 | cos | cosf | cosl |
| tan の計算 | tan | tanf | tanl |
| べき乗の計算 | pow | powf | powl |
| 平方根の計算 | sqrt | sqrtf | sqrtl |
| 天井関数の計算 | sqrt | sqrtf | sqrtl |

以下, べき乗を求める pow 関数を利用したプログラムの例.

```c
/* code: ex1-5.c */
#include <stdio.h>
#include <math.h>

int main()
{
  double x, y, z;
  x = 30.0;
  y = 3.0;
  z = 0.0;
  printf ("x=%f\n", x);
  printf ("y=%f\n\n", y);
  z = pow (x, y);
  printf ("pow (x, y) = %f\n", z);
  
  return 0;
}
```

コンパイルして実行してみる.

```sh
$ gcc ex1-5.c -o ex1-5
$ ./ex1-5
x=30.000000
y=3.000000

pow (x, y) = 27000.000000
```

### 3.4 変数と式

以下, 摂氏から華氏への換算を行うプログラムの例. 変数を利用することで複雑な計算式を解くことが出来る.

```c
/* code: ex1-6.c */
#include <stdio.h>
#include <math.h>

int main()
{
  float celsius, fahrenheit;
  
  celsius = 36.5;
  fahrenheit = (9.0 / 5.0) * celcius + 32.0;
  printf ("%f (Celsius) = %f (Fahrenheit)\n", celsius, fahrenheit);
  
  return 0;
}
```

コンパイルして実行してみる.

```sh
$ gcc ex1-6.c -o ex1-6
$ ./ex1-6
36.500000 (Celsius) = 97.699997 (Fahrenheit)
```

### 3.5 プログラム中のコメント

* `/* */`
* `//` の 1 行コメントも利用可能

以下, サンプルコード.

```c
/* code: ex1-7.c */
#include <stdio.h>

int main()
{
  printf ("The Open University of Japan\n");
  /* Web address
     https://www.ouj.ac.jp/ */
  
  // C++ Style comments
  // C99 allows single-line comments
}
```

一応, コンパイルして実行してみる.

```sh
$ gcc ex1-7.c -o ex1-7
$ ./ex1-7
The Open University of Japan
```

## 演習問題

### 問 1.1

天井関数 (ceil) と床関数 (floor) を利用したプログラムを作成しなさい.

```c
/* code: q1-1.c */
#include <stdio.h>
#include <math.h>

int main ()
{
  double x, y;
  
  x = 3.14159;
  y = 0.0;
  printf ("x = %f\n\n", x);
  y = ceil (x);
  printf ("ceil (x) = %f\n", y);
  y = floor (x);
  printf ("floor (x) = %f\n", y);
  
  return 0;
}
```

そもそも, 天井, 床関数とは.

> 床関数（ゆかかんすう、英: floor function）と天井関数（てんじょうかんすう、英: ceiling function）は、実数に対しそれぞれそれ以下の最大あるいはそれ以上の最小の整数を対応付ける関数である。

[wikpedia](https://ja.wikipedia.org/wiki/%E5%BA%8A%E9%96%A2%E6%95%B0%E3%81%A8%E5%A4%A9%E4%BA%95%E9%96%A2%E6%95%B0) より.

コードをコンパイルして実行する.

```sh
$ gcc q1-1.c -o q1-1
$ ./q1-1
x = 3.141590

ceil (x) = 4.000000
floor (x) = 3.000000
```

### 問 1.2

平方根を計算するプログラムを作成しなさい. sqrt, sqrtf, sqrtl の異なるデータ型の平方根を求める関数を利用すること.

```c
/* code: q1-2.c */
#include <stdio.h>
#include <math.h>

int main ()
{
  float fx, fz;
  double dx, dz;
  long double lx, lz;
  
  fx = 100.00F;
  fz = sqrtf (fx);
  printf ("fx = %f\n", fx);
  printf ("sqrtf (fx) = %f\n\n", fz);

  dx = 100.00;
  dz = sqrt (dx);
  printf ("dx = %f\n", dx);
  printf ("sqrt (dx) = %f\n\n", dz);

  lx = 100.00L;
  lz = sqrt (lx);
  printf ("lx = %Lf\n", lx);
  printf ("sqrt (lx) = %Lf\n\n", lz);
  
  return 0;
}
```

* printf 関数では float 型や double 型を出力する場合, `%f` を用いる
* scanf 関数で double 型変数への入力を行う場合には `%lf` が使われる (処理系によって異なる場合がある)

コードをコンパイルして実行する.

```sh
$ gcc q1-2.c -o q1-2
$ ./q1-2
fx = 100.000000
sqrtf (fx) = 10.000000

dx = 100.000000
sqrt (dx) = 10.000000

lx = 100.000000
sqrt (lx) = 10.000000
```

### 問 1.3

華氏から摂氏への換算を行うプログラムを作成しなさい.

```c
/* code: q1-3.c */
#include <stdio.h>
#include <math.h>

int main()
{
  float celsius, fahrenheit;
  
  fahrenheit = 36.5;
  celsius = (5.0 / 9.0) * fahrenheit - 32.0;
  printf ("%f (Fahrenheit) = %f (Celsius)\n", fahrenheit, celsius);
  
  return 0;
}
```

コードをコンパイルして実行する.

```sh
$ gcc q1-3.c -o q1-3
$ ./q1-3
36.500000 (Fahrenheit) = -11.722222 (Celsius)
```

これだけだとつまらないので, ちょっと進化させてみる.

```c
/* code: q1-4.c */
#include <stdio.h>
#include <math.h>

int main()
{
  float celsius, fahrenheit;

  printf ("Enter fahrenheit: ");
  scanf ("%f", &fahrenheit);

  celsius = (5.0 / 9.0) * fahrenheit - 32.0;
  printf ("%f (Fahrenheit) = %f (Celsius)\n", fahrenheit, celsius);
  
  return 0;
}
```

scanf 関数を利用して, 華氏温度にキーボードから入力した値を代入してみる.

```sh
$ gcc q1-4.c -o q1-4
$ ./q1-4
Enter fahrenheit: 100.0
100.000000 (Fahrenheit) = 23.555555 (Celsius)
```

こんな感じ.

## 以上

感じたことを少々.

* C 言語は Go 言語っぽい雰囲気を感じた
* printf するにも, 変数を利用するにも常に型を意識する必要があるのは新鮮だった
* コード行末の `;` の入力を忘れることが度々...
* C 言語のテストの書き方ってどうするんだろう
