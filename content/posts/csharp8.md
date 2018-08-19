---
title: "C#[8]"
date: 2018-08-17T08:31:35+09:00
draft: false
---

# C#メモ

## ジェネリクス

### コンストラクター制約

```C#
class Test<T> where T : new() {

}
```

型パラメーターTに当てはめるクラスは引数なしコンストラクタを持つ必要がある。**引数なしコンストラクタはコンストラクタを記述しない場合、自動的に生成されるが、引数ありコンストラクタを記述した場合は自動的には生成されない。**

複数の制約を記述する際は、コンストラクター制約は一番最後に記述する必要がある。

### 参照型制約と値型制約

```C#
// Tは参照型に限定される
class Test<T> where T : class {

}

// Tは値型に限定される
class Test<T> where T : struct {
}
```

値型には構造体や列挙型も含まれる。

## 複数の制約を指定する

1. 参照型制約/値型制約 or 基本クラス制約
2. インターフェース制約
3. コンストラクター(new())制約

の順番で記載する必要がある。

2つの型パラメーターに対する制約

```C#
class TwoWheres<T, V> where T : class
                      where V : struct {

                      }
```

### 型パラメーターのデフォルト値

型パラメーターが参照型であればデフォルト値は```null```で、値型であれば```0```である。これらを動的に決定する方法として```default(T)```構文が用意されている。T型に応じて```null```か```0```に変化する。

### ジェネリック構造体

```C#
using System;

struct KeyValue<TKey, TValue> {
    public TKey key;
    public TValue value;

    public KeyValue(TKey a, TValue b) {
        key = a;
        val = b;
    }
}
```

### ジェネリックメソッド

```C#

メソッドだけをジェネリック化できる。クラスメソッド、インスタンスメソッドの両方ともジェネリック化可能。  
引数から型推論ができるため、型指定をする必要がない。


//ジェネリッククラスではない
class ArrayUtils {

  public static bool CopyInsert<T>(T e, int idx, T[] src, T[] target) {
 //...
 }
}

class Demo {
  static void Main() {
      //Tは型推論される。
      ArrayUtils.CopyInsert(99, 2, nums, nums2);
      //
      ArrayUtils.CopyInsert("in C#", 1, strs, strs2);

  }
}

```

ただし、型推論ができない場合には明示的に型指定を行うこともできる。

```C#
ArrayUtils.CopyInsert<string>("in C#", 1, strs, strs2);
```

### ジェネリックメソッドの制約を使う

通常の制約と同じ

```C#
public static void CopyInsert<T>(T e, int  idx, T[]src, T[] target) where T : class {

}
```

### ジェネリックデリゲート

```C#
delegate T delegateName<T>(T v) {

}
```

サンプル
```C#
using System;

delegate T Invert<T>(T v);

class GenDelegateDemo {
    //デリゲート対象
    static double Recip(double v) {
        return 1 / v;
    }
    //デリゲート対象
    static string ReverseStr(string str) {
        string result = "";
        foreach(char ch in str)
            result = ch + result;

        return result;
    }

    static void Main() {
        Invert<double> invDel = Recip;
        Invert<double> invDel2 = ReverseStr;

        InvDel(4.0);
        string str = "ABCDEFG";
        str = invDel2(str);
    }
}
```

