---
title: "C#[5]"
date: 2018-08-13T08:25:24+09:00
draft: false
---

# C#メモ

## 演算子のオーバーロード

### 書式

```C#

//単行演算子
public static MyClass oeprator - (MyClass a1){
 //hogehoge
}

//2項演算子
public static MyClass oeprator + (MyClass a1, MyClass a2){
 //hogehoge
}
```

単行演算子のオペランドはその定義クラス。二項演算子のオペランドはそのどちらかが、その定義クラス。演算子の引数にはref/out修飾子を使用することはできない。

```C#
①
public static MyClass operator + (MyClass a1, int a2)

②
public static MyClass operator + (int a1, MyClass a2)
```

これらは別物。したがって、①を定義しただけでは下記計算を行うことはできない。

```C#

MyClass a1 = new MyClass();
var a3 = 1 + a1;

```

```==``` をオーバーロードする場合は```!=```も同時にオーバーロードする必要がある。これは```<```と```>```。また、```<=```と```>=```も同様である。

##インデクサー

```C#

要素型 this [int インデックス] {

    get {
        // a = Myclass[i]の時に呼び出される。
    }

    set {
        // MyClass[i] = aの時に呼び出される。
    }
}

インデクサーはgetだけを定義する。setだけを定義することもできる。


多次元のインデクサーを定義することもできる。

```C#

要素型 this [int インデックス, int インデックス] {

    get {
        // a = Myclass[i]の時に呼び出される。
    }

    set {
        // MyClass[i] = aの時に呼び出される。
    }
}

```

インデクサーは配列の様に記憶領域を持たない。
インデクサーは[インデックス]形式で呼ばれるメソッドに近い。
そのため、ref/outのパラメーターとして、メソッドに渡すことはできない。

## プロパティ

``` C#

class MyClass {
    private int prop;

    int PropertyName {
        get {
            return prop;
        }

        set {
            if(value > 0)
                prop = value;
        }
    }
}

public static Main() {
    var my = new MyClass();
    my.PropertyName = 10; // PropertyNameのsetが呼び出される。
    var result = my.PropertyName; // PropertyNameのgetが呼び出される。
}

```

## 自動実装するプロパティ

C# 3.0から登場した機能  

```C#

型 名前 {get; set;}

```

と宣言することで、プロパティとして扱うことができる。  
getとsetは一緒に宣言する必要がある。  
get/setの制御を行うことはできない。
プロパティにしておくことで、後々制御したい場合に拡張できる。
他にもメリットがあるらしいが、とりあえずここではここまで。

アクセサーに修飾子をつけることができる。 これは自動実装するプロパティでも同様。

```C#

class MyClass {
    int maximum;

    public int Max {
        get {
            return maximum;
        }
        private set {
            if(value < 0) maximum = -value;
            else maximum = value;
        }
    }
}

public int Max { get; private set; }

```


