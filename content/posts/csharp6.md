---
title: "C#[6]"
date: 2018-08-14T08:17:31+09:00
draft: false
---

# C#メモ

## Vritual/abstract

- virtual
  - virtual修飾子がつけられたメソッドは継承先で、override修飾子をつけることにより、オーバーラーイド可能
  - オーバーライドを行わなくても良い。

- abstract
  - 必ずオーバーライドする必要がある。
  - abstractメソッドを持つクラスはabstractクラスと呼ばれ、直接インスタンス化することはできない。abstractクラスは、abstractメソッドと通常のメソッドが同居できる。interfaceは同居できない。

## sealed

クラス宣言のに使用する修飾子。継承を禁止する。

```C#
sealed class A {

}
```

## GetHashCode

ハッシュマップなどのハッシュ値を計算するために使用される。Equalメソッドをオーバーライドした場合は同時にオーバーライドして実装する必要がある。

## ボックス化/解除

値型もobject型を基底に持つ。
値型をobjectに代入することができる。(ボックス化)

ボックス化された値を値型の変数に代入することをボックス解除される。

## インタフェース

### 定義できる

インタフェースでは

- メソッドのシグニチャ定義
- プロパティ
- インデクサー（配列みたいに扱えるやつ）
- イベント

を定義することができる。

### 定義できない

- データメンバー（クラスのメンバ変数）
- コンストラクター
- デストラクター
- 演算子メソッド
- 静的メンバー

## インタフェースのプロパティ

インタフェースのプロパティは自動実装されない。実装側で実する必要がある。
インターフェースのプロパティにはアクセス修飾子をつけることはできない。

```C#

public interface ISeries {
  
  int Next {
    get;
    set;
  }
}

class ByTwos : ISeries {
  int val;

  public int Next {
    get {
      val += 2;
      return val;
    }

    set {
      val = value;
    }
  }
}

```

## インタフェースのインデクサー

```C#

public interface ISeries {
  int this[int index] {
    get;
  }
}

class ByTwos : ISeries {
  public int this[int index] {
    get {
      val = 0;
      for(int i=0; i<index; i++)
       val += 2;
      return val;
    }
  }
}

```

## インタフェースの明示的実装

```C#
interface IMyIF {
  int MyMeth(int x);
}

interface IYourIF {
  int MyMeth(int x);
}

class MyClass : IMyIF {
  int IMyIF.MyMeth(int x) {
    reutnr x/3;
  }

  int IYourIF.MyMeth(int x) {
    reutnr x * 10;
  }
}
```

別I/Fで同一I/Fが使用されていた場合に、明示的な実装を行うことで、同一区クラスにそれらを実装することができる。

明示的な実装が行われたI/Fはアクセス修飾子をつけることができない。この場合、特殊なアクセス制御が行われる。クラス型の参照変数ではアクセスできないが、I/F型の参照変数からはアクセスできる。

```C#
MyClass my = new MyClass(); //アクセスできない。(private的な扱い)
IFClass if = new MyClass(); //アクセスできる(public的な扱い))
```

## 構造体

クラスは参照型、クラスにアクセスする参照変数そのものには意味がない。あくまでオブジェクトの参照先をしめしているだけ。ただ、場合によっては変数自身が意味をもっていると嬉しい場合がある。また、参照を介してオブジェクトにアクセスするのは参照を介している分オーバーヘッドが生じる。  
これを解決するために構造体型が存在する。

構文はクラスとほぼ同等だが、デフォルトコンストラクター（引数のないコンストラクター）とデストラクターを定義することはできない。また、参照ではないため、new演算子を使用せずに呼び出すことができる。（newを使用した呼び出しもできる。）

## 列挙型

```C#
//基本
//順に0, 1, 2, ...と数字が割り振られる。
enum Coin {Penny, Nickel, Dime, Quarter};

//Quarter以降は101, 102
enum Coin {Penny, Nickel, Dime, Quarter=100, HalfDollar, DOllar};

//基本はintだが、byteをベースにすることもできる。
enum Coin : byte {Penny, Nickel, Dime, Quarter=100, HalfDollar, DOllar};

```

列挙型を数値として表示したい場合はint型にキャストする必要がある。