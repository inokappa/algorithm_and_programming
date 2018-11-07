# 6 章. 配列の仕組み
<!--ts-->
   * [6 章. 配列の仕組み](#6-章-配列の仕組み)
   * [1. 配列の仕組み](#1-配列の仕組み)
   * [2. 多次元配列](#2-多次元配列)
   * [3. 文字列](#3-文字列)
   * [演習問題](#演習問題)
      * [問 6.1](#問-61)
      * [問 6.2](#問-62)
      * [問 6.3](#問-63)
      * [問 6.4](#問-64)
      * [問 6.5](#問-65)
      * [問 6.6](#問-66)
   * [以上](#以上)

<!-- Added by: user, at: 2018-11-07T22:58+00:00 -->

<!--te-->
# 1. 配列の仕組み

* 同じ型の変数データを多数利用する場合, 変数の宣言や取扱が煩雑になる
* このような問題を解決するする方法の一つとして配列 (array) がある
* 配列の要素を指定する為の通し番号は添字 (index) と呼ばれる

以下は配列と添字の関係を示したもの.

| **添字** | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| **値** | 30 | 20 | 10 | 25 | 15 |  |  |  |  |  |

* 添字には整数値が使われ, その値は 0 や 1 からスタートするが, -1 や -2 のような負の値の添字を使うことが出来るプログラミング言語もある
* 連想配列と呼ばれる, 整数以外の文字列等の添字を使うことが出来るプログラミング言語もある
* 配列の要素には同じデータ型しか使えないが, 異なるデータ型を混ぜて使える配列も存在する

以下は C 言語での配列の宣言例.

```c
int a[10];
```

この例では 10 個の整数型の様子を持つ a という名前の配列を宣言している.

以下, 配列を使わずに 5 つの整数の平均値を求めるコード.

```c
/* code: ex6-1.c */
#include <stdio.h>

int main ()
{
  int a, b, c, d, e;
  int sum, avg;
  
  a = 30;
  b = 20;
  c = 10;
  d = 25;
  e = 15;
  sum = a + b + c + d + e;
  avg = sum / 5;
  printf ("%d\n", avg);
  
  return 0;
}
```

上記のコードについて, 配列を使うと以下のようになる.

```c
/* code: ex6-2.c */
#include <stdio.h>

int main ()
{
  int a[10];
  int i, sum, avg;
  
  a[0] = 30;
  a[1] = 20;
  a[2] = 10;
  a[3] = 25;
  a[4] = 15;
  sum = 0;
  for (i = 0; i < 5; i++) {
    sum += a[i];
  }
  avg = sum / 5;
  printf ("%d\n", avg);
  
  return 0;
}
```

このコードでは, for ループにより変数 i が増加する. この変数は配列の添字となっており, 配列の各要素が加算されて 5 つの数の合計が求められる.

```sh
root@0be431eebb77:/work# gcc ex6-2.c -o ex6-2 -g3
root@0be431eebb77:/work# ./ex6-2
20
```

以下は ex6-2.c と同じ動作をするコードである.

```c
/* code: ex6-3.c */
#include <stdio.h>

int main ()
{
  int a[10] = { 30, 20, 10, 25, 15 };
  int i, sum, avg;
  
  sum = 0;
  for (i = 0; i < 5; i++) {
    sum += a[i];
  }
  avg = sum / 5;
  printf ("%d\n", avg);
  
  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc ex6-3.c -o ex6-3 -g3
root@0be431eebb77:/work# ./ex6-3
20
```

このコードでは配列の初期化を配列の宣言文内で行っている. 配列の要素は波括弧 `{}` 内に列挙する. 要素数が宣言した配列の数より少ない場合は, 列挙した要素以外は初期化されない. 列挙した要素が多い場合は, 多くのコンパイラでは警告が出る. 例えば, 以下のように宣言した数よりも要素の数が多い場合.

```c
/* code: ex6-3a.c */
#include <stdio.h>

int main ()
{
  int a[3] = { 30, 20, 10, 25, 15 };
  int i, sum, avg;
  
  sum = 0;
  for (i = 0; i < 5; i++) {
    sum += a[i];
  }
  avg = sum / 5;
  printf ("%d\n", avg);
  
  return 0;
}
```

以下のようにコンパイラ時エラーが発生することを確認した.

```sh
# gcc ex6-3a.c -o ex6-3a -g3
ex6-3a.c: In function 'main':
ex6-3a.c:6:28: warning: excess elements in array initializer
   int a[3] = { 30, 20, 10, 25, 15 };
                            ^~
ex6-3a.c:6:28: note: (near initialization for 'a')
ex6-3a.c:6:32: warning: excess elements in array initializer
   int a[3] = { 30, 20, 10, 25, 15 };
                                ^~
ex6-3a.c:6:32: note: (near initialization for 'a')
```

以下のコードは, 要素数 10 の配列の中に 0 〜 99 まで範囲の乱数を入れていくコードである.

```c
/* code: ex6-4.c */
#include <stdio.h>
#include <stdlib.h>

#define ARRAY_SIZE 10

int main ()
{
  int a[ARRAY_SIZE];
  int i, sum, avg;

  for (i = 0; i < ARRAY_SIZE; i++) {
    a[i] = rand () % 100;
  }

  for (i = 0; i < ARRAY_SIZE; i++) {
    printf ("%03d ", a[i]);
  }
  printf ("\n");

  return 0;
}
```

これをコンパイルして実行する. 以下のように 0 パディングされた 100 以下のランダムな整数が 10 個出力される.

```sh
root@0be431eebb77:/work# gcc ex6-4.c -o ex6-4 -g3
root@0be431eebb77:/work# ./ex6-4
083 086 077 015 093 035 086 092 049 021
```

上記の例では `#define` を利用して, 配列の要素数 `ARRAY_SIZE` を定義している. 配列の要素数を変更したい場合には, `ARRAY_SIZE` を変更するだけで良い. rand 関数は, 0 以上, RAND_MAX で定義された値以下の疑似乱数整数を返す. `%` 記号はモジュロ演算で, 剰余の計算をする.

# 2. 多次元配列

多くのプログラミング言語では, 多次元配列を利用することが出来る. 以下は 2 次元配列を図示したものである.

| | **C0** | **C1** | **C2** | **C3** |
|:---|:---|:---|:---|:---|
| **R0** | a[0][0] | a[0][1] | a[0][2] | a[0][3] |
| **R1** | a[1][0] | a[1][1] | a[1][2] | a[1][3] |
| **R2** | a[2][0] | a[2][1] | a[2][2] | a[2][3] |

以下のコードは, 上記の 2 次元配列表が示すような整数型の 2 次元の配列を作成し, 値を代入した後に二重の for ループによって配列内の値を出力する.

```c
/* code: ex6-5.c */
#include <stdio.h>

int main ()
{
  int i, j;
  int a[3][4] = {
    {0, 10, 20, 30},
    {40, 50, 60, 70},
    {80, 90, 100, 110}
  };

  for (i = 0; i < 3; i++) {
    for (j = 0; j < 4; j++) {
      printf ("array[%d][%d] = %3d\n", i, j, a[i][j]);
    }
  }

  return 0;
}
```

これをコンパイラして実行してみる.

```sh
root@0be431eebb77:/work# gcc ex6-5.c -o ex6-5 -g3
root@0be431eebb77:/work# ./ex6-5
array[0][0] =   0
array[0][1] =  10
array[0][2] =  20
array[0][3] =  30
array[1][0] =  40
array[1][1] =  50
array[1][2] =  60
array[1][3] =  70
array[2][0] =  80
array[2][1] =  90
array[2][2] = 100
array[2][3] = 110
```

以下は, 3 次元配列のコード例である.

```c
/* code: ex6-6.c */
#include <stdio.h>

int main ()
{
  int i, j, k;
  int a[2][3][4] = {
    {{0, 1, 2, 3},
     {4, 5, 6, 7},
     {8, 9, 10, 11}},
    {{0, 10, 20, 30},
     {40, 50, 60, 70},
     {80, 90, 100, 110}}
  };

  for (i = 0; i < 2; i++) {
    for (j = 0; j < 3; j++) {
      for (k = 0; k < 4; k++) {
        printf ("array[%d][%d][%d] = %3d\n", i, j, k, a[i][j][k]);
      }
    }
  }

  return 0;
}
```

これをコンパイラして実行してみる.

```sh
root@0be431eebb77:/work# gcc ex6-6.c -o ex6-6 -g3
root@0be431eebb77:/work# ./ex6-6
array[0][0][0] =   0
array[0][0][1] =   1
array[0][0][2] =   2
array[0][0][3] =   3
array[0][1][0] =   4
array[0][1][1] =   5
array[0][1][2] =   6
array[0][1][3] =   7
array[0][2][0] =   8
array[0][2][1] =   9
array[0][2][2] =  10
array[0][2][3] =  11
array[1][0][0] =   0
array[1][0][1] =  10
array[1][0][2] =  20
array[1][0][3] =  30
array[1][1][0] =  40
array[1][1][1] =  50
array[1][1][2] =  60
array[1][1][3] =  70
array[1][2][0] =  80
array[1][2][1] =  90
array[1][2][2] = 100
array[1][2][3] = 110
```

C 言語の場合, 要素数を宣言する `[]` を増やすことによって, 配列の次元を増やすことが出来る. 多次元配列は 1 次元配列に変換することが可能である. 例えば, 3 次元配列 a の各次元が X, Y, Z であり, 0 以上の値となる添字を i, j, k とすると, 1 次元配列 b は以下の式で変換することが出来る.

```
b[X × Y × i + Y × j + k] = a[i][j][k]
```

一般的に多次元の配列は要素数が大きくなりやすい為, 配列に使用出来るメモリの上限に注意する必要がある. データを格納する為の配列のメモリは, スタック領域と呼ばれる場所に確保される. スタック領域で利用出来るメモリサイズは OS やコンパイラの制限を受ける.

# 3. 文字列

* 文字列 (string) は一連の文字 (character) から出来ている
* C 言語では標準で文字列型を持っておらず, 文字列は配列を利用して作ることが出来る

以下のコードは `OUJ` という文字列を出力するコードである.

```c
/* code: ex6-7.c */
#include <stdio.h>

int main ()
{
  char s[4];
  s[0] = 'O';
  s[1] = 'U';
  s[2] = 'J';
  s[3] = '\0';
  printf ("%s\n", s);
  
  return 0;
}
```

これをコンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc ex6-7.c -o ex6-7 -g3
root@0be431eebb77:/work# ./ex6-7
OUJ
```

このコードは文字型の要素数が 4 の配列 s を宣言し, その配列の先頭から順番に `O`, `U`, `J` の文字を配列に代入している. 最後には `\0` という文字列の終端を表す為の特別な文字 (ヌル文字) を配列に代入している. 長さ n の文字列を格納する為には, ヌル文字も必要になる為, n + 1 個の要素数を持つ配列を用意する必要がある.

C 言語では, 文字列のような配列に対しては代入演算子を利用することが出来ない為, ある配列から別の配列への要素を代入する場合, 以下のコードのように, ループ文等を用いて配列の要素を 1 つずつコピーしていく必要がある.

```c
/* code: ex6-8.c */
#include <stdio.h>

void string_copy (char *target, char *source) // void string_copy (char target[], char source[]) と記述出来る
{
  int i;
  i = 0;
  while (source[i] != '\0') {
    target[i] = source[i];
    i++;
  }
  target[i] = '\0';
}

int main ()
{
  char s[20] = "University";
  char t[20];
  
  string_copy (t, s);
  printf ("%s\n", t);
  
  return 0;
}
```

これをコンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc ex6-8.c -o ex6-8 -g3
root@0be431eebb77:/work# ./ex6-8
University
```

このコードの string_copy 関数は, for ループを用いて, 配列 source の各要素からヌル文字以外を配列 target の各要素にコピーしていく. 最後にヌル文字を追加して文字列をコピーする. この関数を呼び出す時には, コピー先に十分なメモリが確保されていなければならない.

以下の表のように, C 言語のライブラリには文字列を扱う関数が多数含まれている. これらの関数は string.h に含まれる為, 利用する場合には `#include <string.h>` を宣言する必要がある.

| **関数** | **英語での意味** | **関数の説明** |
|:---|:---|:---|
| strcpy | copy a string | 文字列をコピー |
| strcat | concatenate two strings | 2 つの文字列を連結 |
| strcmp | compare two strings | 2 つの文字列を比較 |
| strncmp | compare part of two strings | 2 つの文字列を文字数を指定して比較 |
| strchr | locate character in strings | 文字列の先頭から文字を探索 |
| strstr | locate a substrings | 文字列から文字列を探索 |
| strlen | calculate the length of string | 文字列の長さを求める |

以下のコードは, 文字列をコピーする関数 strcpy 関数を利用した例で ex6-8.c と同様の結果となる.

```c
/* code: ex6-9.c */
#include <stdio.h>
#include <string.h>

int main ()
{
  char s[20] = "University";
  char t[20];
  
  strcpy (t, s);
  printf ("%s\n", t);
  
  return 0;
}
```

これをコンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc ex6-9.c -o ex6-9 -g3
root@0be431eebb77:/work# ./ex6-9
University
```

関数を使うことで, ex6-8.c よりもだいぶんコードの記述量が減っている. 特に理由が無い限りは関数を使うべきなのかな.

以下のコードは strcmp 関数を利用したコードである.

```c
/* code: ex6-10.c */
#include <stdio.h>
#include <string.h>

int main ()
{
  char s0[] = "aaaaa";
  char s1[] = "bbbbb";
  char s2[] = "aaaaaaa";
  
  int i;
  printf ("strcmp(str1, str2)\n");
  i = strcmp (s0, s0);
  printf ("[%s] [%s] (%d)\n", s0, s0, i);

  i = strcmp (s0, s1);
  printf ("[%s] [%s] (%d)\n", s0, s1, i);

  i = strcmp (s1, s0);
  printf ("[%s] [%s] (%d)\n", s1, s0, i);

  i = strcmp (s0, s2);
  printf ("[%s] [%s] (%d)\n", s0, s2, i);
  
  return 0;
}
```

これをコンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc ex6-10.c -o ex6-10 -g3
root@0be431eebb77:/work# ./ex6-10
strcmp(str1, str2)
[aaaaa] [aaaaa] (0)
[aaaaa] [bbbbb] (-1)
[bbbbb] [aaaaa] (1)
[aaaaa] [aaaaaaa] (-97)
```

strcmp 関数は 2 つの文字列 s1 と s2 を比較する. strcmp 関数は以下の書式で定義されている. `const` は定数である為, 書き換え不可能な文字列であり, 関数は文字列を関数内では変更しないことを意味している.

```c
int strcmp(const char *s1, const char *s2)
```

この関数は s1 が s2 に比べて小さい場合, 等しい場合, 大きい場合にそれぞれ, 以下のような整数値を返す.

* 小さい場合... ゼロよりも小さい整数
* 等しい場合... ゼロと等しい整数
* 大きい場合... ゼロより大きい整数

# 演習問題

## 問 6.1

コード 6.2 では, 整数型の配列が宣言されている. これを浮動小数点型の配列に変更したプログラムを作成しなさい.

```c
/* code: q6-1.c */
#include <stdio.h>

int main ()
{
  float a[10];
  int i;
  float sum, avg;
  
  a[0] = 30;
  a[1] = 20;
  a[2] = 10;
  a[3] = 25;
  a[4] = 15;
  sum = 0.0;
  for (i = 0; i < 5; i++) {
    sum += a[i];
  }
  avg = sum / 5.00;
  printf ("%f\n", avg);
  
  return 0;
}
```

このコードをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc q6-1.c -o q6-1 -g3
root@0be431eebb77:/work# ./q6-1
20.000000
root@0be431eebb77:/work#
```

ポイントは変数の宣言に `float` を利用している点. また, 平均値を出力する printf 関数において, `%d\n` ではなく `%f\n` としている点.

## 問 6.2

コード 6.5 を参考にして, 九九表 (掛け算表) を表示するプログラムを作成しなさい. コードでは, 九九表の値を一度, 2 次元配列に代入してから, 配列内の値を出力しなさい.

```c
/* code: q6-2.c */
#include <stdio.h>
#define TABLE 9

int main ()
{
  int i, j;
  int a[TABLE][TABLE]; 

  for (i = 0; i < TABLE; i++) {
    for (j = 0; j < TABLE; j++) {
      a[i][j] = (i + 1) * (j + 1);
    }
  }

  for (i = 0; i < TABLE; i++) {
    for (j = 0; j < TABLE; j++) {
      printf ("%02d ", a[i][j]);
    }
    printf ("\n");
  }

  return 0;
}
```

このコードをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc q6-2.c -o q6-2 -g3
root@0be431eebb77:/work# ./q6-2
01 02 03 04 05 06 07 08 09
02 04 06 08 10 12 14 16 18
03 06 09 12 15 18 21 24 27
04 08 12 16 20 24 28 32 36
05 10 15 20 25 30 35 40 45
06 12 18 24 30 36 42 48 54
07 14 21 28 35 42 49 56 63
08 16 24 32 40 48 56 64 72
09 18 27 36 45 54 63 72 81
```

## 問 6.3

コード 6.6 を変更にして, 3 次元配列に 1 以上 100 以下の乱数を代入するプログラムを作成しなさい. コードでは, 乱数の値を一度, 3 次元配列に代入してから, 配列内の値を出力しなさい.

```c
/* code: q6-3.c */
#include <stdio.h>
#include <stdlib.h>

int main ()
{
  int i, j, k;
  int a[2][3][4]; 

  for (i = 0; i < 2; i++) {
    for (j = 0; j < 3; j++) {
      for (k = 0; k < 4; k++) {
        a[i][j][k] = (rand () % 100) + 1;
      }
    }
  }

  for (i = 0; i < 2; i++) {
    for (j = 0; j < 3; j++) {
      for (k = 0; k < 4; k++) {
        printf ("%03d ", a[i][j][k]);
      }
      printf ("\n");
    }
    printf ("\n");
  }
  return 0;
}
```

これをコンパイルして実行してみる.

```c
root@0be431eebb77:/work# gcc q6-3.c -o q6-3 -g3
root@0be431eebb77:/work# ./q6-3
084 087 078 016
094 036 087 093
050 022 063 028

091 060 064 027
041 027 073 037
012 069 068 030

```

## 問 6.4

コード 6.10 を変更にして, 文字列の最初の 3 文字のみを比較するようにしなさい. 文字列の比較には, srtncmp 関数を用いること.

```c
/* code: q6-4.c */
#include <stdio.h>
#include <string.h>

int main ()
{
  char s0[] = "aaaaa";
  char s1[] = "bbbbb";
  char s2[] = "aaaaaaa";
  
  int i;
  printf ("strncmp(str1, str2)\n");
  i = strncmp (s0, s0, 3);
  printf ("[%s] [%s] (%d)\n", s0, s0, i);

  i = strncmp (s0, s1, 3);
  printf ("[%s] [%s] (%d)\n", s0, s1, i);

  i = strncmp (s1, s0, 3);
  printf ("[%s] [%s] (%d)\n", s1, s0, i);

  i = strncmp (s0, s2, 3);
  printf ("[%s] [%s] (%d)\n", s0, s2, i);
  
  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc q6-4.c -o q6-4 -g3
root@0be431eebb77:/work# ./q6-4
strncmp(str1, str2)
[aaaaa] [aaaaa] (0)
[aaaaa] [bbbbb] (-1)
[bbbbb] [aaaaa] (1)
[aaaaa] [aaaaaaa] (0)
```

strncmp を man してみる.

```sh
$ man strncmp
...
     #include <string.h>

     int
     strcmp(const char *s1, const char *s2);

     int
     strncmp(const char *s1, const char *s2, size_t n);
...
```

strncmp は第三引数に比較する文字数を指定する.

## 問 6.5

strlen 関数を用いて文字列 "abcdefg" の長さを表示するプログラムを作成しなさい.

```c
/* code: q6-5.c */
#include <stdio.h>
#include <string.h>

int main ()
{
  char s[] = "abcdefg";
  
  int i;
  i = strlen (s);
  printf ("[%s] (%d)\n", s, i);
  
  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc q6-5.c -o q6-5 -g3
root@0be431eebb77:/work# ./q6-5
[abcdefg] (7)
```

入力した文字列の長さを返すように改変してみる.

```c
/* code: q6-5a.c */
#include <stdio.h>
#include <string.h>

int main ()
{
  char s[10];

  printf ("Enter Words: ");
  scanf ("%s", &s);

  int i;
  i = strlen (s);
  printf ("[%s] (%d)\n", s, i);

  return 0;
}
```

実行してみる.

```sh
root@0be431eebb77:/work# ./q6-5a
Enter Words: foobar
[foobar] (6)
```

## 問 6.6

配列はデータの集まりを処理する為に重要である. 配列と一緒に使うと便利なものとして構造体 (structure) がある. 構造体を用いると複数のデータ型を 1 つにまとめて扱うことが出来る. (1) 構造体の配列, (2) 構造体のポインタ配列を使ったプログラムを作成しなさい.

[chapter 06 (Algorithms and Programming 2016)](http://compsci.world.coocan.jp/OUJ/2016AL/al2016web/ch06.html) より引用.

```c
/* code: q6-6.c   (v1.16.00) */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
 
#define MAX 10
 
struct student
{
  int id;
  char grade;
  char name[128];
};
typedef struct student STUDENT_TYPE;
 
/* ------------------------------------------- */
int main ()
{
  STUDENT_TYPE db1[MAX];
  STUDENT_TYPE *db2[MAX];
  int i;
 
  printf ("database1\n");
  for (i = 0; i < MAX; i++) {
    db1[i].id = 100 + i;
    db1[i].grade = 'a' + rand () % 5;
    strcpy (db1[i].name, "John Doe");
    printf ("%d %c %s\n", db1[i].id, db1[i].grade, db1[i].name);
  }
 
  printf ("\n");
  printf ("database2\n");
  for (i = 0; i < MAX; i++) {
    db2[i] = malloc (sizeof (STUDENT_TYPE));
    db2[i]->id = 200 + i;
    db2[i]->grade = 'a' + rand () % 5;
    strcpy (db2[i]->name, "John Doe");
    printf ("%d %c %s\t\t", db2[i]->id, db2[i]->grade, db2[i]->name);
    printf ("%d %c %s\n", (*db2[i]).id, (*db2[i]).grade, (*db2[i]).name);
  }
  for (i = 0; i < MAX; i++) {
    free (db2[i]);
  }
 
 
  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc q6-6.c -o q6-6 -g3
root@0be431eebb77:/work# ./q6-6
database1
100 d John Doe
101 b John Doe
102 c John Doe
103 a John Doe
104 d John Doe
105 a John Doe
106 b John Doe
107 c John Doe
108 e John Doe
109 b John Doe

database2
200 c John Doe          200 c John Doe
201 c John Doe          201 c John Doe
202 a John Doe          202 a John Doe
203 e John Doe          203 e John Doe
204 d John Doe          204 d John Doe
205 b John Doe          205 b John Doe
206 a John Doe          206 a John Doe
207 b John Doe          207 b John Doe
208 c John Doe          208 c John Doe
209 b John Doe          209 b John Doe
```

ふむ. 現時点で, このコードを理解するのは難しい. ごめんなさい. 以下, 印刷教材より引用.

* 構造体のメンバ参照に使われているドット演算子とアロー演算子に注意すること
* `db2[i]->id` と `(*db2[i]).id` は同義である
* 構造体のポインタ配列では, malloc と free が利用されている

# 以上

* 配列の理解が深まった (これまではふわっとしか理解していなかった)
* 文字列の生成に配列を利用する必要があるというのは意外だった
* ポインタとか構造体とかがサラッと出てくるので焦る...
