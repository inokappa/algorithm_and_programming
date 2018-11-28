# 2 章. 条件分岐
<!--ts-->
   * [2 章. 条件分岐](#2-章-条件分岐)
      * [1. if 文による条件分岐](#1-if-文による条件分岐)
         * [1.1 if 文](#11-if-文)
         * [1.2 if-else 文](#12-if-else-文)
      * [2. switch 文による条件分岐](#2-switch-文による条件分岐)
      * [3. 条件演算子](#3-条件演算子)
      * [4. goto 文](#4-goto-文)
      * [演習問題](#演習問題)
         * [問 2.1](#問-21)
         * [問 2.2](#問-22)
         * [問 2.3](#問-23)
      * [以上](#以上)

<!-- Added by: user, at: 2018-11-28T05:47+00:00 -->

<!--te-->
## 1. if 文による条件分岐

### 1.1 if 文

* if 文を利用することで条件分岐が可能となる

以下の例だと, 条件式が真 (`true`) である場合, 処理が実行されるが, 偽 (false) である場合には実行されない.

```c
if ( 条件式 )
  条件式が真 true だった場合の処理;
```

以下, C 言語の比較演算子の例.

| **比較演算子** | **比較演算子の意味** | **比較演算子の意味 (英語)** |
|:---|:---|:---|
| `==` | 等しい | equal to |
| `!=` | 等しくない | not equal to |
| `>` | 大きい | greater than |
| `<` | 小さい | less than |
| `>=` | 大きいか等しい | greater than or equal to |
| `<=` | 小さいか等しい | less than or equal to |

複数の命令を実行したい場合には, 以下のように波括弧 (カーリーブランケット: curly brankey) の記号 `{ }` を利用したブロック文を用いる.

```c
if ( 条件式 ) {
  処理 1;
  処理 2;
  ...
  処理 n;
}
```

### 1.2 if-else 文

以下の例では, 条件式が真となる場合には, 処理 T が実行され, 偽の場合には処理 F が実行される.

```c
if ( 条件式 )
  処理 T;
else
  処理 F;
```

複数の命令を実行したい場合には, 以下のようにカーリーブランケットを利用したブロック文を利用する.

```c
if ( 条件式 ) {
  処理 T0;
  処理 T1;
} else {
  処理 F0;
  処理 F1;
}
```

## 2. switch 文による条件分岐

* 式 (expression) の値に応じて多分岐を行う
* `default` はどの `case` にも一致しなかった場合に実行される, `default` は省略可能, `case` と `default` の順序は任意
* `breke` はそれ以降の処理を行わずに switch 文を抜ける
* if-else 文に置き換えることが出来る

```c
switch ( 式 ) {
  case 定数式 0:
    処理;
    ...
    break;
  case 定数式 1:
    処理;
    ...
    break;
  case 定数式 2:
    処理;
    ...
    break;
  default:
    処理;
    ...
    break;
}
```

## 3. 条件演算子

* 三項演算子 (ternary operator)
* if-else 文と類似したコードを書くことが可能
* 条件式が真であれば, 式 T の値を, 偽であれば式 F の値を返すという演算を行う
* 式 T と式 F は同じ型の値を返す必要がある

```c
条件式 ? 式 T : 式 F 
```

以下, if-else 文との比較.

```c
// 条件演算子バージョン
result = a > b ? x : y

// if-else バージョン
if (a > b) {
  result = x;
} else {
  result = y;
}
```

## 4. goto 文

* goto 文はコードの位置を移動させることが出来る
* goto 文はスパゲティコードを作り出してしまう原因となる為, 多用することは推奨されていない
* 特殊なエラー処理や多重にネストされたループから抜け出す為に使われることが稀にある
* ループ処理や関数等の制御構造と比較して処理の流れが解りにくくなり, コードの可読性が悪くなる

以下, for 文と goto 文の比較.

```c
# for 文
for ( i = 0; i < 1024; i++ ) {
  処理;
}

# goto 文
i = 0;
LABEL_A:
  if ( i < 1024 )
    goto LABEL_B;
  処理;
  i ++;
  goto LABEL_A
LABEL_B:
```

た, 確かに goto 文は可読性悪い...

## 演習問題

### 問 2.1

以下のコードを変更し, scanf 関数を用いて変数 x と変数 y の値を, キーボード等の標準入力から入力出来るようにしなさい.

```c
/* code: ex2-2.c */
#include <stdio.h>

int main()
{
  int x, y;

  x = 500;
  y = 700;
  printf ("X = %d\n", x);
  printf ("Y = %d\n", y);

  if (x > y)
    printf ("X is greater than Y.\n");
  else
    printf ("X is less than or equal to Y.\n");

  return 0;
}
```

このコードをコンパイルして実行してみる.

```sh
$ gcc ex2-2.c -o ex2-2
$ ./ex2-2
X = 500
Y = 700
X is less than or equal to Y.
```

これを以下のように改修する.

```c
/* code: q2-1.c */
#include <stdio.h>

int main()
{
  int x, y;

  printf ("Enter X Value: ");
  scanf ("%d", &x);
  printf ("Enter Y Value: ");
  scanf ("%d", &y);  
  
  printf ("X = %d\n", x);
  printf ("Y = %d\n", y);

  if (x > y)
    printf ("X is greater than Y.\n");
  else
    printf ("X is less than or equal to Y.\n");

  return 0;
}
```

このコードをコンパイルして実行してみる.

```sh
$ gcc q2-1.c -o q2-1
$ ./q2-1
Enter X Value: 3
Enter Y Value: 5
X = 3
Y = 5
X is less than or equal to Y.

$ ./q2-1
Enter X Value: 1000
Enter Y Value: 10
X = 1000
Y = 10
X is greater than Y.
```

### 問 2.2

以下のコードを変更し, 変数 grade が小文字のデータに対しても多分岐出来るようにしなさい.

```c
/* code: ex2-3.c */
#include <stdio.h>

int main ()
{
  char grade;

  grade = 'B';

  switch (grade) {
  case 'A':
    printf ("excellent\n");
    break;
  case 'B':
    printf ("good\n");
    break;
  case 'C':
    printf ("fair\n");
    break;
  case 'D':
    printf ("barely passing\n");
    break;
  case 'F':
    printf ("not passing\n");
    break;
  default:
    printf ("ERROR: invalid character\n");
    break;
  }
  printf ("Your grade is %c\n", grade);
  return 0;
}
```

このコードをコンパイルして実行する.

```sh
$ gcc ex2-3.c -o ex2-3
$ ./ex2-3
good
Your grade is B
```

これを以下のように改修する.

```c
/* code: ex2-3.c */
#include <stdio.h>

int main ()
{
  char grade;

  printf ("Enter grade: ");
  scanf ("%c", &grade);

  switch (grade) {
  case 'A':
  case 'a':
    printf ("excellent\n");
    break;
  case 'B':
  case 'b':
    printf ("good\n");
    break;
  case 'C':
  case 'c':
    printf ("fair\n");
    break;
  case 'D':
  case 'd':
    printf ("barely passing\n");
    break;
  case 'F':
  case 'f':
    printf ("not passing\n");
    break;
  default:
    printf ("ERROR: invalid character\n");
    break;
  }
  printf ("Your grade is %c\n", grade);
  return 0;
}
```

このコードをコンパイルして実行してみる.

```sh
$ gcc q2-2.c -o q2-2
$ ./q2-2
Enter grade: a
excellent
Your grade is a

$ ./q2-2
Enter grade: b
good
Your grade is b

$ ./q2-2
Enter grade: F
not passing
Your grade is F

$ ./q2-2
Enter grade: e
ERROR: invalid character
Your grade is e
```

### 問 2.3

以下のコードでは switch 文が使われている. switch 文を使わずに if-else 文で書き換えなさい. 論理演算子 (論理積 `&&`, 論理和 `||`, 否定 `!`) 等を利用すること.

```c
/* code: q2-3a.c */
#include <stdio.h>

int main ()
{
  int a;
  a = 3;
  switch (a) {
    case 0:
    case 1:
    case 2:
      printf ("A\n");
      break;
    case 3:
    case 4:
      printf ("B\n");
      break;
    default:
      printf ("ERROR: invalid number\n");
      break;
  }

  return 0;
}
```

このコードをコンパイルして実行してみる.

```sh
$ gcc q2-3a.c -o q2-3a
$ ./q2-3a
B
```

これを以下のように, まずはファイルの分割を行った.

```c
/* code: q23b.c */
#include <stdio.h>
#include "q23b.h"

int main ()
{
  int num;
  num = 1;

  char res;
  res = *foobar(num);
  printf ("%c\n", res);

  return 0;
}

/* code: q23b.h */
#ifndef Q23B_H
#define Q23B_H

char *foobar(int);

#endif

/* code: foobar.c */
# include "q23b.h"

char *foobar(int num){
  switch (num) {
    case 0:
    case 1:
    case 2:
      return "A";
      break;
    case 3:
    case 4:
      return "B";
      break;
    default:
      return "ERROR: invalid number";
      break;
  }
}
```

その上で, foobar.c をリファクタリングしていく. リファクタリングするにあたって, [Unity](http://www.throwtheswitch.org/unity) を使って以下のようなテストコードを書いた. C 言語のユニットテストには Unity がシンプルで良い感じだったので, 別の機会に詳細に掘り下げる予定.

```c
/* code: tests/test_q23b.c */
#include <stddef.h>
#include "vendor/unity.h"
#include "../q23b.h"

void setUp(void)
{
}

void tearDown(void)
{
}

void test_foobar_0(void)
{
   TEST_ASSERT_EQUAL_STRING("A", foobar(0));
}

void test_foobar_1(void)
{
   TEST_ASSERT_EQUAL_STRING("A", foobar(1));
}

void test_foobar_2(void)
{
   TEST_ASSERT_EQUAL_STRING("A", foobar(2));
}

void test_foobar_3(void)
{
   TEST_ASSERT_EQUAL_STRING("B", foobar(3));
}

void test_foobar_4(void)
{
   TEST_ASSERT_EQUAL_STRING("B", foobar(4));
}

void test_foobar_5(void)
{
   TEST_ASSERT_EQUAL_STRING("ERROR: invalid number", foobar(5));
}

int main(void)
{
   UnityBegin("tests/test_q23b.c");

   RUN_TEST(test_foobar_0);
   RUN_TEST(test_foobar_1);
   RUN_TEST(test_foobar_2);
   RUN_TEST(test_foobar_3);
   RUN_TEST(test_foobar_4);
   RUN_TEST(test_foobar_5);

   UnityEnd();

   return 0;
}
```

テストは make で実行出来るように, 以下のような Makefile も用意した.

```sh
CFLAGS  = -std=c99
CFLAGS += -g
CFLAGS += -Wall
CFLAGS += -Wextra
CFLAGS += -pedantic
CFLAGS += -Werror

VFLAGS  = --quiet
VFLAGS += --tool=memcheck
VFLAGS += --leak-check=full
VFLAGS += --error-exitcode=1

test: tests.out
        @./tests.out

memcheck: tests.out
        @valgrind $(VFLAGS) ./tests.out
        @echo "Memory check passed"

clean:
        rm -rf *.o *.out *.out.dSYM *.dSYM

tests.out: q23b.c foobar.c tests/vendor/unity.c tests/test_q23b.c
        @echo Compiling $@
        @$(CC) $(CFLAGS) foobar.c tests/vendor/unity.c tests/test_q23b.c -o tests.out

build:
        @$(CC) $(CFLAGS) q23b.c foobar.c -o q23b
```

試しにテストを走らせてみる.

```sh
$ make test
Compiling tests.out
tests/test_q23b.c:47:test_foobar_0:PASS
tests/test_q23b.c:48:test_foobar_1:PASS
tests/test_q23b.c:49:test_foobar_2:PASS
tests/test_q23b.c:50:test_foobar_3:PASS
tests/test_q23b.c:51:test_foobar_4:PASS
tests/test_q23b.c:52:test_foobar_5:PASS

-----------------------
6 Tests 0 Failures 0 Ignored
OK
```

いい感じ. 以下のようにリファクタリングを行った.

```c
/* code: foobar.c */
# include "q23b.h"

char *foobar(int num){
  if (num == 0 || num == 1 || num == 2) {
    return "A";
  } else if (num == 3 || num == 4) { 
    return "B";
  } else {
    return "ERROR: invalid number";
  }
} 
```

試しにテストを走らせる.

```sh
$ make test
tests/test_q23b.c:47:test_foobar_0:PASS
tests/test_q23b.c:48:test_foobar_1:PASS
tests/test_q23b.c:49:test_foobar_2:PASS
tests/test_q23b.c:50:test_foobar_3:PASS
tests/test_q23b.c:51:test_foobar_4:PASS
tests/test_q23b.c:52:test_foobar_5:PASS

-----------------------
6 Tests 0 Failures 0 Ignored
OK
```

いい感じ.

## 以上

感じたことなど.

* goto 文は出来る限り「使うな」
* 三項演算子が出てきた (Ruby も C も同じ書き方なんだな)
* switch 文にて, 条件が複数になる場合 case 文は縦に並べて書く必要があるのは辛い
* 教材には掲載されていないが, コード分割の雰囲気を掴むことが出来た
* C のテストはとりあえず Unity で書いていこうと思う (Unity はググらビリティ悪いよな)
