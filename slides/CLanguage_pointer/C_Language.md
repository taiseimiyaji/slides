---
marp: true
paginate: true
header: C言語のポインターの仕組みについて
footer: taisei miyaji
---

# C言語とは
- 1972年にデニス・リッチーが開発
- 当時は高水準言語だったが今では低水準言語に分類される。

---
# バイブル

https://www.amazon.co.jp/%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E8%A8%80%E8%AA%9EC-%E7%AC%AC2%E7%89%88-ANSI%E8%A6%8F%E6%A0%BC%E6%BA%96%E6%8B%A0-B-W-%E3%82%AB%E3%83%BC%E3%83%8B%E3%83%8F%E3%83%B3/dp/4320026926

---

# 他の言語との歴史の比較
- C 1973年
- COBOL 1959年
- Java 1994年
- PHP 1959年
- Python 1991年
- VB 1991年
- Rust 2009年

参考:
https://ja.wikipedia.org/wiki/%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E8%A8%80%E8%AA%9E%E3%81%AE%E6%AF%94%E8%BC%83

---
## 本題 ポインタの仕組み
参考: https://ja.wikibooks.org/wiki/C%E8%A8%80%E8%AA%9E/%E3%83%9D%E3%82%A4%E3%83%B3%E3%82%BF

メモリ(主記憶)にはアドレスと言う番号が振られている。
それぞれのアドレスの領域は1バイトです。
ポインタはこのアドレスを格納している変数のことです。

---

## データ型とサイズ
char: 1バイト
short: 2バイト
int: 4バイト
float: 4バイト
double: 8バイト

---
## ポインタの参照
```C
#include <stdio.h>//おまじないとよく言われるやつ。標準入出力のためのライブラリ

int main(void) {
  int = 55;
  int *pi = &i; //piにはiのアドレスが格納されます。
  *pi = 1234;
  printf("%d\n", i)
}
```

実行結果
```
1234
```
---
## アドレスの演算
```C
#include <stdio.h>
#include <stddef.h> // for ptrdiff_t

int main(void) {
  const int ary[] = {123, 55, 818, 1192, 777};

  const int *a = &ary[0]; // 要素0のアドレスを格納
  printf("&ary[0] = %p\n", a);

  const int *b = &ary[1]; // 要素1のアドレスを格納
  printf("&ary[1] = %p\n", b);

  ptrdiff_t delta = b - a; // 配列の要素間の距離（間にある要素の数）
  printf("&ary[1] - &ary[0] = delta = %td\n", delta);

  const int *v = ary + delta;
  printf("*vは%d\n", *v);
}

```
---

実行結果
```
&ary[0] = 0x7ffe0052c910
&ary[1] = 0x7ffe0052c914
&ary[1] - &ary[0] = delta = 1
*vは55
```

---
## 関数ポインタ

---
## ポインタのポインタ
ポインタを指すポインタを宣言できる
```C
#include <stdio.h>

int main(void) {
  int i,
     *pi = &i,
    **ppi = &pi; // 理論上何重にもネストできる

    **ppi = 1234;
  printf("%d", i); // 1234が出力される
}
```

## セグフォ(セグメンテーションフォールト)
以下のような場合に発生してプログラムが終了します。(デバッグしにくい)
- アクセス禁止のメモリ領域にアクセスしたり、読み込み専用のメモリに書き込もうとしたとき
- 再帰が深すぎるとき
- 配列サイズが大きすぎるとき
