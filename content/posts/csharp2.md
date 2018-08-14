---
title: "C#[2]"
date: 2018-07-31T23:38:50+09:00
draft: false
---

# C#メモ

## String

C#のString型に対する==は値の比較となる。
Javaの様に参照の比較ではない。

```C#
//内容は等しいが、異なるオブジェクトのStringを2つ作る
string a = new string(new char[] { 'h', 'e', 'l', 'l', 'o' });
string b = new string(new char[] { 'h', 'e', 'l', 'l', 'o' });

Console.WriteLine(a == b); //true
Console.WriteLine(a.Equals(b)); //true
```

## ビット演算

&(論理積)は主にどのビットが立っているのか調べる場合に使用される。逆に|(論理和)はビットを立てたい場合に使用される。

```C#
using System;

class BitsInByte {
    int t;
    byte val;

    val = 123;
    for(t=128; t > 0; t/2) {
        if((val&t) != 0){
            Console.Write("1 ");
        } else {
            Console.Write("0 ");
        }
    }

    for(t=128; t > 0; 1>>t) {
        if((val&t) != 0){
            Console.Write("1 ");
        } else {
            Console.Write("0 ");
        }
    }
    //10000000
    //01000000
    //...
    //00000001
}
```

シフトした結果、溢れたビットの情報は消えてしまう。
循環はしない。

符号付値がビット演算を行っても符号が変わらない様に符号付ビット(左端)は変更されない。