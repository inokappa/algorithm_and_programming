# 5 章. 関数
<!--ts-->
   * [5 章. 関数](#5-章-関数)
   * [1. 関数](#1-関数)
      * [序説](#序説)
      * [1.1 プログラムコードの関数](#11-プログラムコードの関数)
      * [1.2 関数とスコープ](#12-関数とスコープ)
   * [2. 引数, 戻り値, 値渡し, 参照渡し](#2-引数-戻り値-値渡し-参照渡し)
      * [2.1 引数と戻り値](#21-引数と戻り値)
      * [2.2 "値渡し"と"参照渡し"](#22-値渡しと参照渡し)
   * [3. 再帰関数](#3-再帰関数)
   * [いきなり, ポインタとは...](#いきなり-ポインタとは)
   * [演習問題](#演習問題)
      * [問 5.1](#問-51)
      * [問 5.2](#問-52)
      * [問 5.3](#問-53)
      * [問 5.4](#問-54)
      * [問 5.5](#問-55)
   * [以上](#以上)

<!-- Added by: user, at: 2018-11-01T03:29+00:00 -->

<!--te-->
# 1. 関数

## 序説

* 頻繁に使うコードは関数 (function) として 1 つのコードにまとめると再利用が可能
* 構造化言語において関数は構造化を実現する上で重要な役割を果たす
* プログラミング言語によっては, 関数はサブルーチン, プロシージャと呼ばれることもある

## 1.1 プログラムコードの関数

以下, 冗長なコードの例.

```c
/* code: ex5-1.c */
#include <stdio.h>

int main()
{
  int i;

  for (i = 0; i < 10; i++)
    printf ("%d", i);
  printf ("\n");
  
  for (i = 0; i < 10; i++)
    printf ("%d", i);
  printf ("\n");

  for (i = 0; i < 10; i++)
    printf ("%d", i);
  printf ("\n");

  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc ex5-1.c -o ex5-1 -g3
root@0be431eebb77:/work# ./ex5-1
0123456789
0123456789
0123456789
```

以下は, このコードから 0 〜 9 の数値を表示する print_numbers を関数に切り出した場合のコード.

```c
/* code: ex5-2.c */
#include <stdio.h>

void print_numbers (void)
{
  int i;

  for (i = 0; i < 10; i++)
    printf ("%d", i);
  printf ("\n");    
}

int main ()
{
  print_numbers ();  
  print_numbers ();  
  print_numbers ();  
  
  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc ex5-2.c -o ex5-2 -g3
root@0be431eebb77:/work# ./ex5-2
0123456789
0123456789
0123456789
```

* 結果はどちらのコードも同じになるがプログラムコードの見通しは関数化したプログラムコードの方が俄然良い
* `main` も関数の 1 つであり, これはコードの初めに呼ばれる特殊な関数である
* main 関数よりも後に関数を記述する場合, 関数プロトタイプ (function prototype = 関数の宣言) が必要になる
* 関数の宣言には, **関数数の戻り値の型**, **関数の引数の型**, **関数の引数の数**等の情報を記述する

以下は, main 関数より前に print_numbers 関数の関数プロトタイプが記述されている.

```c
/* code: ex5-3.c */
#include <stdio.h>

void print_numbers (void);

int main ()
{
  print_numbers ();  
  print_numbers ();  
  print_numbers ();  
  
  return 0;
}

void print_numbers (void)
{
  int i;

  for (i = 0; i < 10; i++)
    printf ("%d", i);
  printf ("\n");    
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc ex5-3.c -o ex5-3 -g3
root@0be431eebb77:/work# ./ex5-3
0123456789
0123456789
0123456789
```

* main 関数よりも前に関数を記述する場合, 関数プロトタイプの宣言は省略可能...だが, バグ防止等の観点から関数プロトタイプの世間を強く推奨している書籍が多い
* ライブラリに含まれる様々な関数にも関数プロトタイプに関する情報があり, 通常, これらはヘッダファイルに記述されている

例えば, sqrt 関数 (平方根を求める関数) の関数プロトタイプ (`double sqrt (double);`) であり, math.h というヘッダファイルに関数プロトタイプの記述があるので, sqrt 関数を利用する場合には, 以下のように math.h をインクルードして利用する.

```c
...
#include <stdio.h>
#include <math.h>
...
```

## 1.2 関数とスコープ

* 変数にはスコープ (scope) と呼ばれる属性がある
* 変数のスコープとは, 変数が有効であるコードの範囲のことである
* スコープの範囲の違いによって, グローバル変数 (global variable) とローカル変数 (local variable) がある
* グローバル変数では, コードの全ての範囲で変数が有効である
* ローカル変数では, ローカル変数が宣言されたブロック内の範囲だけで変数が有効である

以下のコードでは, 関数 f と関数 g の中で同じ変数名 `i` が使われているが, それぞれの関数でスコープが異なる.

```c
/* code: ex5-4.c */
#include <stdio.h>

void g (void)
{
  int i;
  for (i = 0; i < 3; i++) {
    printf ("a");
  }
}

void f (void)
{
  int i;
  for (i = 0; i < 5; i++) {
    g ();
  }
}

int main ()
{
  f ();
  return 0;
}
```

これをコンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc ex5-4.c -o ex5-4 -g3
root@0be431eebb77:/work# ./ex5-4
aaaaaaaaaaaaaaa
```

同じ変数名のグローバル変数とローカル変数を宣言することも可能であるが, ローカル変数が宣言されているブロック内では, グローバル変数よりもローカル変数の方が優先される.

# 2. 引数, 戻り値, 値渡し, 参照渡し

## 2.1 引数と戻り値

* 引数 (parameter) は, 実引数 (argument, actual argument) と仮引数 (parameter, formal parameter) の 2 つがある
* 実引数とは関数にわたす値のこと
* 関数の中で引数の値を受け取る変数は仮引数という
* 戻り値 (return value) とは関数が返す値

以下のコードは三角形の面積 ($area$) を求める関数の例.

```c
/* code: ex5-5.c */
#include <stdio.h>

float triangle (float base, float height)
{
  float c;
  c = (base * height) / 2.000F;
  return c;
}

int main ()
{
  float t;
  t = triangle (3.00, 4.00);
  printf ("triangle = %f\n", t);
  t = triangle (5.00, 6.00);
  printf ("triangle = %f\n", t);
  
  return 0;
}
```

尚, 三角形の面積は以下の公式で求められる.

$$ area = \frac{1}{2} \times base \times height $$

上記のコードにおいて, 底辺 ($base$) と高さ ($height$) の 2 つの float 型の引数を持ち, この引数に値が渡されると関数 (triangle) が三角形の面積を計算する. この関数の戻り値として float 型の面積の値が関数から返される. 3.00, 4.00 及び 5.00 と 6.00 が実引数であり, 関数で宣言されている base と height が仮引数となる.

一応, このコードをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc ex5-5.c -o ex5-5 -g3
root@0be431eebb77:/work# ./ex5-5
triangle = 6.000000
triangle = 15.000000
```

## 2.2 "値渡し"と"参照渡し"

* 引数の渡し型には, `値渡し` (pass by value) と `参照渡し` (pass by reference) と呼ばれるものがある
* `値渡し` は変数のコピーが作成されて値が渡される方法で, 関数内で変数の値が変更されても元の変数の値は変わらない
* `参照渡し` は, 変数そのものを渡す為, 関数内で変数の値の変更は, 呼び出した側の変数にも影響して値が変わる

以下のコードは `値渡し` と `参照渡し` の場合では, 変数 a の出力する値が異なる例.

```c
/* code: ex5-6.c */
#include <stdio.h>

void add_pass_by_value (int i)
{
  i = i + 1;
}

void add_pass_by_reference (int *i)
{
  *i = *i + 1;
}

int main()
{
  int a;
  a = 10;
  add_pass_by_value (a);
  printf ("%d\n", a);

  a = 10;
  add_pass_by_reference (&a);
  printf ("%d\n", a);
  
  return 0;
}
```

このコードをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc ex5-6.c -o ex5-6 -g3
root@0be431eebb77:/work# ./ex5-6
10
11
```

C 言語では変数ポインタを取得出来る為, 変数へのポインタを渡すことで `参照渡し` と同じ効果がある `アドレス渡し` が可能であるとのこと...ポインタの話がサラッと出てきているので辛い...ポインタについては別で勉強する必要があるな.

# 3. 再帰関数

再帰呼び出し (recursion call) とは, 関数や手続き等が, 自分自身を呼び出して実行することで, アルゴリズムの中には, 再帰的にプログラムを記述することによって効果的な処理を出来るものがある. 以下, 再帰プログラミングの例として用いられる階乗 (factorial) の計算式で, 1 から $n$ までの自然数の総乗である. 階乗は $n!$ で表現され, 以下のような式で定義される. (尚, $0!=1$ と定義されている.)

$$ n!=\prod_{k=1}^n k=n\times\left(n-1\right)\times\left(n-2\right)\cdots\times3\times2\times1 $$

実際の階乗の計算例は以下の通り. $n$ の値が大きくなるに従って急激に数が増加する.

```
1! = 1
2! = 2 x 1 = 2
3! = 3 x 2 x 1 = 6
4! = 4 x 3 x 2 x 1 = 24
..
..
10! = 10 x 9 x 8 x 7 x 6 x 5 x 4 x 3 x 2 x 1 = 3,628,800
```

この階乗を計算をコードに落とし込むと以下のような感じで factorial 関数を再帰で呼び出して計算結果を返す. factorial 関数は, factorial 関数内で $n - 1$ という引数で呼び出すようになっており, $n$ が $0$ になった時点で 1 を返し, その時点で factorial 関数の呼び出しを止める.

```c
/* code: ex5-7.c */
#include <stdio.h>

int factorial (int n)
{
  if (n == 0) {
    return 1;
  } else {
    return n * factorial (n - 1);
  }
}

int main()
{
  int i;
  i = 5;
  printf ("%d! = %d\n", i, factorial (i));
  
  return 0;
}
```

これをコンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc ex5-7.c -o ex5-7 -g3
root@0be431eebb77:/work# ./ex5-7
5! = 120
```

これを gdb でステップ実行してみると以下のような感じになる.

```sh
(gdb) run
The program being debugged has been started already.
Start it from the beginning? (y or n) y
Starting program: /work/ex5-7

Breakpoint 1, main () at ex5-7.c:16
16        i = 5;
(gdb) s
17        printf ("%d! = %d\n", i, factorial (i));
(gdb) s
factorial (n=5) at ex5-7.c:6
6         if (n == 0) {
(gdb) s
9           return n * factorial (n -1);
(gdb) s
factorial (n=4) at ex5-7.c:6
6         if (n == 0) {
(gdb) s
9           return n * factorial (n -1);
(gdb) s
factorial (n=3) at ex5-7.c:6
6         if (n == 0) {
(gdb) s
9           return n * factorial (n -1);
(gdb) s
factorial (n=2) at ex5-7.c:6
6         if (n == 0) {
(gdb) s
9           return n * factorial (n -1);
(gdb) s
factorial (n=1) at ex5-7.c:6
6         if (n == 0) {
(gdb) s
9           return n * factorial (n -1);
(gdb) s
factorial (n=0) at ex5-7.c:6
6         if (n == 0) {
(gdb) s
7           return 1;
```

`return n * factorial (n -1);` が 5 回繰り返された後に `return 1;` が返っていることが解る.

# いきなり, ポインタとは...

本教材ではポインタがサラッとしか触れられていないのが辛い. とりあえず, 本教材に書かれている内容を抜粋する.

* ポインタは変数等のオブジェクトを指すもの
* `*` 間接演算子 (indirection operator) は, ポンタを介して値に間接的に参照し, ポインタが指し示すアドレスに格納されている値を参照する演算子である
* `&` アドレス演算子 (address-of operator) は, オペランドのアドレスを与える, 変数のアドレスを取得する演算子である

これだけではピンと来ないので...自分で調べるしかない.

# 演習問題

## 問 5.1

以下のコードは, どのような値を出力するか答えなさい.

```c
/* code: q5-1.c */
#include <stdio.h>

float trapezoid (float a, float b, float h)
{
  float c;
  c = ((a + b) * h) / 2.000F;
  return c;
}

int main ()
{
  float t;
  t = trapezoid (3.00, 4.00, 5.00);
  printf ("trapezoid = %f\n", t);
  t = trapezoid (5.00, 6.00, 7.00);
  printf ("trapezoid = %f\n", t);
 
  return 0;
}
```

これをコンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc q5-1.c -o q5-1 -g3
root@0be431eebb77:/work# ./q5-1
trapezoid = 17.500000
trapezoid = 38.500000
```

これは台形の面積を求めるコードだった. 尚, C 言語では, 関数に設定出来る引数の数の上限はコンパイラに依存している. C90 規格では 31 個, C99 規格では 127 個まで利用出来る.


## 問 5.2

以下のコードは, どのような値を出力するか答えなさい.

```c
/* code: q5-2.c */
#include <stdio.h>

struct student
{
  int id;
  char grade;
  float average;
};
typedef struct student STUDENT_TYPE;

STUDENT_TYPE initialize_student_record (STUDENT_TYPE s)
{
  s. id++;
  s. grade = 'x';
  s. average = 0.0;
  return s;
}

int main ()
{
  STUDENT_TYPE student;

  student. id = 20;
  student. grade = 'a';
  student. average = 300.000;
  printf ("%d %c %f\n", student. id, student. grade, student. average);
  student = initialize_student_record (student);
  printf ("%d %c %f\n", student. id, student. grade, student. average);

  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc q5-2.c -o q5-2 -g3
root@0be431eebb77:/work# ./q5-2
20 a 300.000000
21 x 0.000000
```

## 問 5.3

以下の再帰関数のコードは, どのような値を出力するか答えなさい.

```c
/* code: q5-3.c */
#include <stdio.h>

int fibonacci (int n)
{
  if (n == 0) {
    return 0;
  } else if (n == 1) {
    return 1;
  } else {
    return (fibonacci (n - 1) + fibonacci (n - 2));
  }
}

int main ()
{
  int i;
  i = 10;
  printf ("fibonacci (%d) = %d\n", i, fibonacci (i));
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc q5-3.c -o q5-3 -g3
root@0be431eebb77:/work# ./q5-3
fibonacci (10) = 55
```

関数名から判るとおり, これはフィボナッチ数を計算するプログラムである. 以下はフィボナッチ数の数式.

$$  a_n = \frac{1}{\sqrt{5}} \left\{ \left({\frac{1+\sqrt{5}}{2}}\right)^{n} - \left(\frac{1-\sqrt{5}}{2}\right)^{n} \right\}  $$

このプログラムをちょっと弄って 0 〜 11 まで数のフィボナッチ数列を計算させてみる.

```c
/* code: q5-3a.c */
#include <stdio.h>

int fibonacci (int n)
{
  if (n == 0) {
    return 0;
  } else if (n == 1) {
    return 1;
  } else {
    return (fibonacci (n - 1) + fibonacci (n - 2));
  }
}

int main ()
{
  int i;
  for (i = 0; i < 11; i++) {
    printf ("fibonacci (%d) = %d\n", i, fibonacci (i));
  }
}
```

コンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc q5-3a.c -o q5-3a -g3
root@0be431eebb77:/work# ./q5-3a
fibonacci (0) = 0
fibonacci (1) = 1
fibonacci (2) = 1
fibonacci (3) = 2
fibonacci (4) = 3
fibonacci (5) = 5
fibonacci (6) = 8
fibonacci (7) = 13
fibonacci (8) = 21
fibonacci (9) = 34
fibonacci (10) = 55
```

## 問 5.4

以下の再帰関数のコードは, どのような値を出力するか答えなさい.

```c
/* code: q5-4.c */
#include <stdio.h>

void foo (int n)
{
  if (n < 15) {
    foo (n + 1);
    printf ("%d ", n);
  }
}

int main ()
{
  foo (0);
  return 0;
}
```

コンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc q5-4.c -o q5-4 -g3
root@0be431eebb77:/work# ./q5-4
14 13 12 11 10 9 8 7 6 5 4 3 2 1 0
```

上記のように 14 から 0 の数値が出力される.

## 問 5.5

コード ex5-7.c の階乗を計算する再帰関数のコードで $i = 5$ を変更し, $i = -1$ として負の整数を引数にして関数を呼び出した場合, どのような問題があるか考えなさい.

以下, ex5-7c のコード.

```c
/* code: ex5-7.c */
#include <stdio.h>

int factorial (int n)
{
  if (n == 0) {
    return 1;
  } else {
    return n * factorial (n - 1);
  }
}

int main()
{
  int i;
  i = 5;
  printf ("%d! = %d\n", i, factorial (i));
  
  return 0;
}
```

とりあえず, ex5-7.c の実行結果.

```sh
root@0be431eebb77:/work# ./ex5-7
5! = 120
```

上記のコードを以下のように書き換えてみる.

```c
/* code: q5-5.c */
#include <stdio.h>

int factorial (int n)
{
  if (n == 0) {
    return 1;
  } else {
    return n * factorial (n - 1);
  }
}

int main()
{
  int i;
  i = -1;
  printf ("%d! = %d\n", i, factorial (i));
  
  return 0;
}
```

これを実行する.

```sh
root@0be431eebb77:/work# ./q5-5
Segmentation fault
```

あちゃー. 一応, gdb で確認.

```sh
(gdb) run
The program being debugged has been started already.
Start it from the beginning? (y or n) y
Starting program: /work/q5-5

Breakpoint 1, main () at q5-5.c:16
16        i = -1;
(gdb) s
17        printf ("%d! = %d\n", i, factorial (i));
(gdb) s
factorial (n=-1) at q5-5.c:6
6         if (n == 0) {
(gdb) s
9           return n * factorial (n - 1);
(gdb) s
factorial (n=-2) at q5-5.c:6
6         if (n == 0) {
(gdb) s
9           return n * factorial (n - 1);
(gdb) s
factorial (n=-3) at q5-5.c:6
6         if (n == 0) {
(gdb) s
9           return n * factorial (n - 1);
(gdb) s
factorial (n=-4) at q5-5.c:6
6         if (n == 0) {
(gdb)
```

上記のように `factorial (n=-1)`, `factorial (n=-2)`, `factorial (n=-3)` となる為, `n == 0` の条件に合致しなくなり, 無限ループが発生して最終的には `Segmentation fault` となる.

# 以上

* C 言語の関数の雰囲気が判ってきた
* いきなりポインタとか構造体とか出てきてビビった...ちゃんと勉強する必要があるな