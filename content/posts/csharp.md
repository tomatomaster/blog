---
title: "C#"
date: 2018-07-29T21:46:49+09:00
draft: false
---

# C#メモ

## リテラル

C#の整数リテラルはint uint long ulongの順に当てはめられる。

## ショートサーキット評価

```p | q // (OR)```
pが```true```ならばqは評価せずとも、```true```
そこで登場するのが短絡評価（サーキット評価)。  
サーキット評価は ```p||q```と記述し、pで式の評価が決まるならば、qの評価を行わない。

## 暗黙的型変換

```C#

//コンパイル可能
long longValue = 123131314;
double doubleValue = longValue;

//コンパイル失敗
long longValue = 123131314.1231;
double doubleValue = longValue;

//コンパイル失敗
decimal dValue = 12313;
float  f = dValue;
double d = dValue;

```

### decimal

```C#

decimal dValue = 0.01;
int iValue = 100
double doubleValue = 200.2;
float fValue 300.3;

// iValueはdecimalとして評価される。
var sum = dValue + iValue;
// エラー
var sum = dValue + doubleValue;
// エラー
var sum = dValue + fValue;

```

### double

decimalがオペランドに含まれていない場合、もう一方のオペランドはdoubleに変換される。

### float

decimal, floatがオペランドに含まれていない場合、もう一方のオペランドはfloatに変換される。

### ulong

上記の場合以外、もう一方のオペランドはulongに変換される。
ただし、符号付型(sbyte, short, int long)の場合はエラーが発生する。

### long

上記以外の場合、もう一方のオペランドはlongに変換される。

### uint

上記以外の場合、一方のオペランドがsbyte, short, intの場合両オペランドがlong型に変換される。

### uint

上記以外の場合、もう一方のオペランドもuintに変換される。

### int

上記以外の場合、もう一方のオペランドはintに変換される。

### 注意点

```C#

byte b = 10;
int i =  b + b;
// intに変換されるためキャストが必要
byte b2 = (byte)(b+ b);
char c = (char)(b+ b);

```

単項演算子でも型変換は発生する。  

```C#

byte b = 10;
int i = -b;

uint ui = 10;
//uintはlongに変換される。
long l = -10;

```

## Switch

C#のSwitchは言語仕様で、あるcaseで処理が行われたら必ずbreakしなければならないと決められている。defaultでもこれは必須となっている。

## C#のガベージコレクション
そのインスタンスがどこからも参照されなくなったらガベージコレクトされる。

## デストラクタ

```C#

~ クラス名() {

}

```
ガベージコレクトされる際に呼ばれる。Javaと同様必ず呼び出される保証はないので、基本使用しない。

## 多次元配列

```C#

int[,] table = new int[10, 20];
int[,,] tripleTable = new int[10, 20, 30];

```

### ジャグ配列

```C#

int[3][] jagged = new int[3][];
jagged[0] = new int[2];
jagged[1] = new int[3];
jagged[2] = new int[4];

```
jagged  
[][]  
[][][]  
[][][][]  

