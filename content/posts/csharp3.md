---
title: "C#[3]"
date: 2018-08-01T22:56:21+09:00
draft: false
---

# C#メモ

## アクセス修飾子

C#はアクセス修飾子をつけないと```private```指定になる。

## メソッドの引数（オブジェクトの参照）

C#はオブジェクトを参照している変数をメソッドの引数として渡して、メソッド内で変数を操作すると、呼び出し元変数が参照しているオブジェクトも操作された状態になっている。
変数はコピーだが、そのコピーの参照先は呼び出し元と同一であるため、このような挙動になる。ただし、コピーなのでメソッド内で別のオブジェクトを参照しても、呼び出し元の参照先は変わらない。

```C#

void Test(Test t) {
    t.a = 100;
    return
}

void Test2(Test t) {
    t = new Fuga();
    return
}

var hoge = new Test()
hoge.a = 0;
Test(hoge);
//hoge.aは100になる。

Test2(hoge)
//hogeはTestオブジェクトを参照している。Fugaオブジェクトは参照していない。

```

確認用

```C#

using System;

class Hoge
{
    static void Main()
    {
        var test = new Hoge.Test(3);
        Change(test);
        Console.Write(test.a);
    }

    static void Change(Test t)
    {
        t.a = 100;
        t = new Test(3);
    }

    class Test
    {
        public int a;
        public Test(int a)
        {
            this.a = a;
        }
    }
}

```