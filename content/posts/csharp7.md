---
title: "C#[7]"
date: 2018-08-16T10:04:45+09:00
draft: false
---

# C#メモ

## デリゲート

メソッド専用のインタフェースみたいなもの  
戻り値と引数がデリゲート宣言と一致していれば、デリゲート変数に代入することができる。

```C#

delegate string StrMod(string str);

public string ReplaceSpaces(string a) {
    retunr a.Replace(' ', '-');
}

static void Main() {
    StrMod strOp = ReplaceSpaces;
    strOp("test");
}

```

### デリゲートのマルチキャスト

デリゲートは連鎖することができる。（デリゲートのマルチキャスト）

```C#

using System;

delegate void StrMod(ref string str);

class StringOps
{
    static void ReplacesSpaces(ref string a)
    {
        a = a.Replace(' ', '-');
    }

    static void Reverse(ref string a)
    {
        string temp = "";
        int i, j;

        for (j = 0, i = a.Length - 1; i >= 0; i--, j++)
        {
            temp += a[i];
        }

        a = temp;
    }

    static void Main()
    {
        StrMod strOp;
        strOp = ReplacesSpaces;
        strOp += Reverse;
        var str = "This is a test";
        Console.WriteLine(str);
        strOp(ref str);
        Console.WriteLine(str);
    }
}

```

### デリゲートを使用する理由

デリゲート変数に代入されるメソッドはコンパイル時ではなく、実行時に決定される。そのため、プラグインなどの開発に利用できる。

## 匿名メソッド

```C#

delegate void CountIt();

CountIt count = delegate {
    //処理
}

delegate void CountIt(int i);

CountIt = delegate (int end) {
    //処理
}

//　匿名メソッドは値を返すこともできる。
delegate int CountIt(int i);

CountIt = delegate (int end) {
    int result;
    //処理
    return result;
}


```

### キャプチャ

匿名メソッドを囲んでいるスコープの変数にアクセスすることができる。この変数は匿名メソッドにキャプチャされていると言う。匿名メソッドを参照しているデリゲートインスタンスがGC
されるまで、キャプチャされたインスタンスはGCされない。

```C#

int hogehoge;
delegate int CountIt(int i);

CountIt = delegate (int end) {
    int result;
    result += hogehoge; //hogehogeにアクセスすることができる。hogehogeはキャプチャされている。
    //処理
    return result;
}

```

## イベント

リスナーの登録にデリゲートを利用することができる。  
ただし、複数のリスナーをマルチキャスト登録されることは問題ないが、イベントハンドラーの参照を意図せず変更されることは避けたい。
そんため、デリゲートをマルチキャストの使用に制限するeventが存在する。

[イベント実装のガイドライン](https://docs.microsoft.com/ja-jp/dotnet/standard/design-guidelines/event#feedback)

```C#

イベント送信側のクラス（イベントハンドラー）

delegate void MyEventHandler();

class MyEventHandler {
public event MyEventHandler SomeEvent;　// リスナーはこの変数にイベント発生時の処理を登録する。

}

```

## 名前空間

名前空間が指定されていないとグローバル名前空間に変数やメソッドが登録される。


```C#

namespace 名前空間名 {
    //以下の記述は全て名前空間に含まれる。

}

```

既存の名前空間や型に別名を付けることもできる。

```C#

using エイリアス = 名前;

```

同一の名前空間ブロックが複数ファイルに存在しても構わない。これらは一つの名前空間として扱われる。ただし、これらは同時にコンパイルする必要がある。

名前空間はネストすることができる。この場合ネストされた名前空間には```.``` で繋いでアクセスすることができる。

### 名前空間エイリアス修飾子

```C#

using alpha = Alpha.Utility;
alpha.MyClass 
//alphaという名前空間が存在し、その中にMyClassが存在すると競合する。
//alphaは名前空間ではなくエイリアスであることを明記したい。
alpha::MyClass//::を利用することで、alphaがエイリアスであることが明記される。
 

```



