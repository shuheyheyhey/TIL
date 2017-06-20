# Java 文字列

* imutable  
String クラス
* mutable  
StringBuilder クラス

## 初期化

以下は実は String オブジェクトの生成が 2 回行われている。

```java:sample.java
String s = new String("abc")
```

Java ではリテラル "abc" は String 型のオブジェクトに置換される。
つまり `new String` と `"abc"` で 2回のオブジェクト生成がされていることになる。　　

## 文字列結合

Java では文字列結合に + 演算子が使用できる。

```java:sample.java
String s = "abc"
s += "def"
```

この += 演算子は内部的には StringBuilder クラスに置換されて最終的に toString() メソッドで String 型に置き換えられている。
ループの中で += 演算子で結合させまくると・・・  
パフォーマンスに影響が出てくるのは容易に想像ができる。
この場合 StringBuilder クラスを事前に定義しておいて結合していく方がよいだろう。

## 比較

`==` で比較するのは同一性であって、同値性ではない。
同じ文字列を指すオブジェクトでも参照先が異なれば真にはならない。
ただし、リテラル文字の場合は同じ参照先を指すようになるので真となる点には注意が必要。
まぁとにかく String.equals() をつかえばよい。