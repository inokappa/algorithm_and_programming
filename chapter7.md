# 7 章. 配列の操作
<!--ts-->
   * [7 章. 配列の操作](#7-章-配列の操作)
   * [1. 配列のデータ挿入](#1-配列のデータ挿入)
   * [2. 配列からのデータ削除](#2-配列からのデータ削除)
   * [3. 探索](#3-探索)
      * [線形探索, 二分探索](#線形探索-二分探索)
      * [3.1 線形探索](#31-線形探索)
      * [3.2 二分探索](#32-二分探索)
   * [演習問題](#演習問題)
      * [問 7.1](#問-71)
      * [問 7.2](#問-72)
      * [問 7.3](#問-73)
      * [問 7.4](#問-74)
      * [問 7.5](#問-75)
      * [問 7.6](#問-76)
   * [以上](#以上)

<!-- Added by: user, at: 2018-11-14T23:03+00:00 -->

<!--te-->
# 1. 配列のデータ挿入

既にデータが挿入されている配列に要素を追加する場合を考える.

| 添字 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| **データ** | 100 | 300 | 500 | 400 | 700 | 800 | 600 | | |

添字 6 にデータを追加する場合, データを追加するだけなので特に難しいことは無い. ところが, 以下のように添字 **0** にデータを挿入する場合, 各添字のデータを右に 1 つずつ移動して添字 **0** にデータを挿入する為の空きを作る必要がある.

| 添字 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| **データ** | 600 | 100 | 300 | 500 | 400 | 700 | 800 | | |

このデータ移動について, 配列内のデータの数を $n$ とすると, 運が良ければデータ移動は不要であるが, 最悪の場合には $n$ 個のデータを移動させて配列の空きを確保する必要がある. この時に必要な平均の計算量は $O(n)$ となる.

# 2. 配列からのデータ削除

挿入と同様に, 既にデータが挿入されている配列から添字を指定してデータを削除する場合を考える. また, この削除ではデータを削除した後にデータが配列の先頭から連続的に並ぶような操作を行う.

| 添字 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| **データ** | 100 | 300 | 500 | 400 | 700 | 800 | ~~600~~ | | |

データ 600 を削除する. この削除は簡単でデータを削除した後でも既にデータが配列の先頭から連続的に並んでいる為, 特別な操作は不要である. 

| 添字 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| **データ** | ~~600~~ | 100 | 300 | 500 | 400 | 700 | 800 | | |

ところが, 上記のように添字 **0** のデータ 600 を削除した場合, 削除した後は添字 **0** に空きが出来てしまう為, データを 1 個ずつ順番に移動して配列先頭からデータが連続的に並ぶようにする必要がある. この場合, 挿入と同様に配列内のデータの数を $n$ とすると, 最良の場合はデータの移動は不要であるが, 最悪の場合では $n$ 個のデータを移動させる必要がある為, 平均の計算量は $O(n)$ となる.

# 3. 探索

## 線形探索, 二分探索

配列のデータを探索する手法として, 線形探索 (linear search) と二分探索 (binary search) について検討する.

## 3.1 線形探索

添字の値を順番に増加させながら, 配列の要素となるデータと, 探索キー (search key) となるデータを順番に比較していくことで探索出来る. 以下のコードは線形探索を行う実装例となる. ([chapter 07 (Algorithms and Programming 2016)](http://compsci.world.coocan.jp/OUJ/2016AL/al2016web/ch07.html) より引用.)

```c
/* code: ex7-1.c   (v1.16.00) */
#include <stdio.h>
#include <stdlib.h>
#define ARRAY_SIZE 13
 
/* ------------------------------------------- */
int linear_search (int array[], int n, int key)
{
  int i;
  for (i = 0; i < n; i++) {
    if (array[i] == key) {
      return i;
    }
  }
  return -1;
}
 
/* ------------------------------------------- */
void print_array (int array[], int n)
{
  int i;
  for (i = 0; i < n; i++) {
    printf ("%d ", array[i]);
  }
  printf ("\n");
}
 
/* ------------------------------------------- */
int main ()
{
  int index, key;
  int array[ARRAY_SIZE] = {
    900, 990, 210, 50, 80, 150, 330,
    470, 510, 530, 800, 250, 280
  };
  key = 800;
  print_array (array, ARRAY_SIZE);
  index = linear_search (array, ARRAY_SIZE, key);
  if (index != -1) {
    printf ("Found: %d (Index:%d)\n", key, index);
  }
  else {
    printf ("Not found: %d\n", key);
  }
  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc ex7-1.c -g3 -o ex7-1
root@0be431eebb77:/work# ./ex7-1
900 990 210 50 80 150 330 470 510 530 800 250 280
Found: 800 (Index:10)
```

このコードの linear_search 関数は, 配列内から線形探索によって探索キーと一致するデータを探し, データが見つかれば, 見つかったデータの配列の添字の値を返す.

これを少し改修. 検索対象のデータ (`key`) を scanf 関数で入力出来るようにして, 探索対象となる配列のデータは rand 関数を利用して乱数で生成するようにした.

```c
/* code: ex7-1a.c   (v1.16.00) */
#include <stdio.h>
#include <stdlib.h>
#define ARRAY_SIZE 1000000

/* ------------------------------------------- */
int linear_search (int array[], int n, int key)
{
  int i;
  for (i = 0; i < n; i++) {
    if (array[i] == key) {
      return i;
    }
  }
  return -1;
}

/* ------------------------------------------- */
void print_array (int array[], int n)
{
  int i;
  for (i = 0; i < n; i++) {
    printf ("%d ", array[i]);
  }
  printf ("\n");
}

/* ------------------------------------------- */
int main ()
{

  int key;
  printf ("Enter an integer: ");
  scanf ("%d", &key);

  int index;
  int array[ARRAY_SIZE];
  int i;
  for (i = 0; i < ARRAY_SIZE; i ++) {
    array[i] = rand () % ARRAY_SIZE;
  }
  // print_array (array, ARRAY_SIZE);
  index = linear_search (array, ARRAY_SIZE, key);
  if (index != -1) {
    printf ("Found: %d (Index:%d)\n", key, index);
  }
  else {
    printf ("Not found: %d\n", key);
  }
  return 0;
}
```

これをコンパイルして実行する.

```sh
root@0be431eebb77:/work# gcc ex7-1a.c -g3 -o ex7-1a
root@0be431eebb77:/work# ./ex7-1a
Enter an integer: 456
Found: 456 (Index:757414)
```

## 3.2 二分探索

二分探索 (binary search) は, **整列済みの配列**に対して探索を行う. 整列 (sorting) とは, データを値の大小関係に従って並べる操作で, 整列された配列は順序配列 (ordered array) と呼ぶ. 二分探索では, 配列の中央の値と比較し, 検索するキーの値との大小関係を基に探索を進める.

検索したい値が中央の値より大きいか, 小さいか調べ, 二分割した配列の片方には, 目的のキーの値が存在しないことを確かめながら検索を行う. 二分探索は整列済みの配列でなければならないという制約があるものの, 平均の計算量は線形探索よりも高速である.

以下, 11 個の要素を持った順序配列に対して二分探索が行われる様子を図示したもの.

まずは, 配列の要素はソートされて 100 から順番に格納されている.

| 添字 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| **データ** | 100 | 200 | 300 | 400 | 500 | 600 | 700 | 800 | 900 | 990 | 999 |

この配列からデータ 800 を二分探索していくと, まずは添字 **5** のデータ 600 と探索対象の 800 を比較し, 600 以下のデータは探索の対象外となる. 

| 添字 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| **データ** | ~~100~~ | ~~200~~ | ~~300~~ | ~~400~~ | ~~500~~ | ~~600~~ | 700 | 800 | 900 | 990 | 999 |

次に残った配列の要素の中央値である添字 **8** の 900 と探索対象の 800 を比較し, 800 よりも大きい為, 添字 **8** 以上のデータは探索の対象外となる.

| 添字 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| **データ** | ~~100~~ | ~~200~~ | ~~300~~ | ~~400~~ | ~~500~~ | ~~600~~ | 700 | 800 | ~~900~~ | ~~990~~ | ~~999~~ |

という流れで二分探索によって 800 が探索されるが, 処理の過程で配列の探索範囲が半分になっていく為, データが多くなると, 二分探索は線形探索と比べると圧倒的に高速に処理されることが解る.

以下のコードは二分探索を行うコード例である. ([chapter 07 (Algorithms and Programming 2016)](http://compsci.world.coocan.jp/OUJ/2016AL/al2016web/ch07.html) より引用. 若干, 上の図に合わせて改変している.)

```c
/* code: ex7-2.c   (v1.16.00) */
#include <stdio.h>
#include <stdlib.h>
#define ARRAY_SIZE 11

/* ------------------------------------------- */
int binary_search (int array[], int num, int key)
{
  int middle, low, high;
  low = 0;
  high = num - 1;
  while (low <= high) {
    middle = (low + high) / 2;
    if (key == array[middle]) {
      return middle;
    }
    else if (key < array[middle]) {
      high = middle - 1;
    }
    else {
      low = middle + 1;
    }
  }
  return -1;
}

/* ------------------------------------------- */
void print_array (int array[], int n)
{
  int i;
  for (i = 0; i < n; i++) {
    printf ("%d ", array[i]);
  }
  printf ("\n");
}

/* ------------------------------------------- */
int main ()
{
  int index, key;
  int array[ARRAY_SIZE] = {
    50, 80, 150, 210, 250, 280, 330,
    470, 510, 530, 800
  };

  key = 210;
  print_array (array, ARRAY_SIZE);
  index = binary_search (array, ARRAY_SIZE, key);
  if (index != -1) {
    printf ("Found: %d (Index:%d)\n", key, index);
  }
  else {
    printf ("Not found: %d\n", key);
  }
  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc ex7-2.c -g3 -o ex7-2
root@0be431eebb77:/work# ./ex7-2
50 80 150 210 250 280 330 470 510 530 800
Found: 210 (Index:3)
```

このコードは, 前の線形探索と同様に配列の要素をランダムに生成するようにして, 且つ, 探索の対象は画面から入力出来るように改造する余地は十分にある.

尚, 線形探索 (ex7-1) と二分探索 (ex7-2) で実行速度を比較してみると, 配列の要素数が 13 と小さいものの以下のような若干の差異が見られた.

```sh
root@0be431eebb77:/work# time ./ex7-1
900 990 210 50 80 150 330 470 510 530 800 250 280
Found: 800 (Index:10)

real    0m0.004s
user    0m0.000s
sys     0m0.000s
root@0be431eebb77:/work# time ./ex7-2
50 80 150 210 250 280 330 470 510 530 800 900 990
Found: 210 (Index:3)

real    0m0.003s
user    0m0.000s
sys     0m0.000s
root@0be431eebb77:/work# time ./ex7-1
900 990 210 50 80 150 330 470 510 530 800 250 280
Found: 800 (Index:10)

real    0m0.005s
user    0m0.000s
sys     0m0.000s
root@0be431eebb77:/work# time ./ex7-2
50 80 150 210 250 280 330 470 510 530 800 900 990
Found: 210 (Index:3)

real    0m0.004s
user    0m0.000s
sys     0m0.000s
```

# 演習問題

## 問 7.1

順序配列について説明しなさい.

* データが整列された配列を順序配列 (ordered array) と呼ばれている

## 問 7.2

線形探索と二分探索で探索に必要な平均の計算量について答えなさい.

| 探索手法 | 平均の計算量 | メモ |
|:---|:---|:---|
| 線形探索 | $O(n)$ | データ量に応じて計算量が増える | 
| 二分探索 | $O(log\ n)$ | 処理の度に処理対象を減らすようなアルゴリズム |

## 問 7.3

二分探索の特徴を簡単に述べなさい.

* 順序配列に対して探索を行う
* 配列の中央値との値を比較し, 検索キーの値との大小関係を基に探索を進める
* 検索したい値が中央の値よりも大きいか, 小さいかを調べ, 二分割した配列の片方には, 目的のキーの値が存在しないことを確かめながら検索を行う
* 平均の計算量は $O(log\ n)$ で高速である

## 問 7.4

C 言語のライブラリに含まれる lsearch 関数を利用したプログラムを作成しなさい.

[こちら](http://compsci.world.coocan.jp/OUJ/2016AL/al2016web/ch07.html) より引用.

```c
/* code: q7-1.c   (v1.16.00) */
#include <stdio.h>
#include <stdlib.h>
#include <search.h>
 
#define ARRAY_SIZE 10
 
/* ------------------------------------------- */
int compare (int *x, int *y)
{
  return (*x - *y);
}
 
/* ------------------------------------------- */
void print_array (int array[], int n)
{
  int i;
  for (i = 0; i < n; i++) {
    printf ("%d ", array[i]);
  }
  printf ("\n");
}
 
/* ------------------------------------------- */
int main ()
{
  int key;
  int *r;
  size_t elements;
  int array[ARRAY_SIZE] = {
    12, 19, 70, 44, 16, 38, 10, 30, 28, 98
  };
  key = 16;
  elements = ARRAY_SIZE;
  print_array (array, ARRAY_SIZE);
 
  r = (int *) lsearch (&key, &array, &elements, sizeof (int),
               (int (*)(const void *, const void *)) compare);
  if (r != NULL) {
    printf ("Found: %d\n", key);
  }
  else {
    printf ("Not found: %d\n", key);
  }
  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc q7-1.c -g3 -o q7-1
root@0be431eebb77:/work# ./q7-1
12 19 70 44 16 38 10 30 28 98
Found: 16
```

## 問 7.5

C 言語のライブラリに含まれる bsearch 関数を利用したプログラムを作成しなさい.

[こちら](http://compsci.world.coocan.jp/OUJ/2016AL/al2016web/ch07.html) より引用.

```c
/* code: q7-2.c   (v1.16.00) */
#include <stdio.h>
#include <stdlib.h>
#include <search.h>
 
#define ARRAY_SIZE 10
 
/* ------------------------------------------- */
int compare (int *x, int *y)
{
  return (*x - *y);
}
 
/* ------------------------------------------- */
void print_array (int array[], int n)
{
  int i;
  for (i = 0; i < n; i++) {
    printf ("%d ", array[i]);
  }
  printf ("\n");
}
 
/* ------------------------------------------- */
int main ()
{
  int key;
  int *r;
  int array[ARRAY_SIZE] = {
    10, 12, 16, 19, 28, 30, 38, 44, 70, 98
  };                /* ordered array! */
 
  key = 16;
  print_array (array, ARRAY_SIZE);
 
  r = (int *) bsearch (&key, &array, ARRAY_SIZE, sizeof (int),
               (int (*)(const void *, const void *)) compare);
  if (r != NULL) {
    printf ("Found: %d\n", key);
  }
  else {
    printf ("Not found: %d\n", key);
  }
  return 0;
}
```

これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc q7-2.c -g3 -o q7-2
root@0be431eebb77:/work# ./q7-2
10 12 16 19 28 30 38 44 70 98
Found: 16
```

## 問 7.6

コード (q7-3.c) は, 1 節と 2 節で述べた配列操作に関連するコードである. このコードは, データ挿入, データ削除, データ挿入可能な場所の探索を行う関数からなっている. gprof 等のプロファイラで関数を分析しなさい.

[こちら](http://compsci.world.coocan.jp/OUJ/2016AL/al2016web/ch07.html) より引用.

```c
/* code: q7-3.c   (v1.16.00) */
#include <stdio.h>
#include <stdlib.h>
#define MAX 1000000
 
/* ------------------------------------------- */
void array_print (int a[], int max)
{
  int i;
  for (i = 0; i < max; i++) {
    printf ("%02d ", a[i]);
  }
  printf ("\n");
}
 
/* ------------------------------------------- */
int array_find_empty (int a[], int max)
{
  int i;
  for (i = 0; i < max; i++) {
    if (a[i] == -1) {
      return i;
    }
  }
  return -1;
}
 
/* ------------------------------------------- */
void array_insert (int a[], int max, int index, int empty, int data)
{
  int i;
  if (empty > index) {
    for (i = empty; i > index; i--) {
      a[i] = a[i - 1];
    }
  }
  else {
    for (i = empty; i < index; i++) {
      a[i] = a[i + 1];
    }
  }
  a[index] = data;
}
 
/* ------------------------------------------- */
int array_delete (int a[], int index)
{
  int data;
  data = a[index];
  a[index] = -1;
  return data;
}
 
/* ------------------------------------------- */
int main ()
{
  int i, j, index_ins, index_del, empty, data;
  int a[MAX];
 
  for (j = 0; j < MAX; j++) {
    a[j] = rand () % 100;
  }
  data = a[MAX / 2];
  a[MAX / 2] = -1;
 
  for (i = 0; i < 1000; i++) {
    index_ins = rand () % MAX;
    index_del = rand () % MAX;
    /* printf("ins:%d  del:%d\n", index_ins, index_del );  */
 
    empty = array_find_empty (a, MAX);
    /* printf("empty:%d\n", empty ); */
 
    array_insert (a, MAX, index_ins, empty, data);
 
    data = array_delete (a, index_del);
    /* array_print( a, MAX ); */
  }
  return 0;
}
```

とりあえず, これをコンパイルして実行してみる.

```sh
root@0be431eebb77:/work# gcc q7-3.c -g3 -o q7-3
root@0be431eebb77:/work# ./q7-3
root@0be431eebb77:/work#
```

何も出力されない. gprof で解析してみたが...解答のような結果が出力されないので要調査.

# 以上

* 今まで何気なく配列にデータの出し入れを実装していたけど, その裏でこんなことが行われていたのかー, 面白い
* 線形探索, 二分探索の違いについて理解出来た
* 実際にコードを書く際には lsearch 関数, bsearch 関数を利用すれば良い
* gprof の解析がうまく動かない (解答のような結果が出力されない) ので調査しよう
