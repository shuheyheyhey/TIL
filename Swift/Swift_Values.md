# Swift Values

## Swift での値の宣言

### 変数
`var`  
変更可能な値を定義する。
### 定数  
`let`  

不変な値を定義する。
定数は必ずしも宣言時に定義されている必要はない。
最初の代入以降変更はできないだけ。

```swift:sample.swift
let a
a = "test"
```

## 比較

`==` を使って同値性を確認できる。
同値性の確認は型が一致している必要がある。
例えば Int 型と Float 型を比較した場合はコンパイルエラーになる。
Int8 型と Int16 型での比較も同様である。
Swift では暗黙的な型変換を行わず、
明示的にキャストして比較を行わせて桁落ちなどによる意図しない結果を出力しないよう考えられている。

## 整数

### Int
Int8,Int16,Int32,Int64 がある。  
`var i = 0` の型推論はデフォルトで Int32 となる。

### Double
浮動小数点を扱う型は Float 型と Double 型がある。  
リテラルで宣言した場合の型推論はデフォルトで Double となる。  

ちょっとメモ：  
Double の宣言では exponetioal を使うこともできる  
`let exponentDouble = 1.2e1`

### 演算

`++` や `--` はない。  
演算する際には型が一致していないものではできないので注意。


## 文字列

`String` 型  
文字列は文字がシーケンシャルに並んでいるものであることは言うまでもない。  
文字を表す Charcter 型を、CharcterView 型が配列として持っている。  
`Charcter` 型 -> `CharcterView` 型 -> `String` 型

### 比較

`==` を使用できる。  
比較は Unicode の正準等価に基づいて判断される。  
→見た目、機能面から判断

## 真偽値
  
`Bool` 型  
Int と Bool は明確に違って、`i = 1` と `i = true` は同値ではない。
以下に例を示す。  

```swift:sample.swift
let i = 1
if i { 
	// コンパイルエラー
	// 条件式は i == 1 としなければいけない。
}
```

## 配列

`Array<Element>` 型  
シンタックスシュガーとして `[Element]` が用意されている。
空配列を用意する場合 `[]` を使用する(コンパイラが型推論できないので、型アノテーションは必須となる)。  
配列へのアクセスはサブスクリプトを使用し、要素のインデックスにアクセスする。

## 辞書

`Dictionary<Key, Value>` 型  
シンタックスシュガーとして `[Key: Value]` が用意されている。
空辞書を用意する場合は `[:]` を使用する(コンパイラが型推論できないので、型アノテーションは必須となる)。  
Value に入れる型は制限はないが、 Key に入れる型は `Hashable` プロトコルに準拠しているものである必要がある。  
ハッシュ値によって Key の一意性を担保したり、検索の高速化を実現している。

* 要素の削除  
nil を Value に代入することで削除可能。

```
var dic = ["key": 1]
dic["key"] = nil // dic = [:]
```

## 範囲型

```
Range<Bound> 型
CountableRange<Bound> 型
ClosedRange<Bound> 型
CountableClosedRange<Bound> 型
```

半開区間、閉区間、カウント可能かの組み合わせで上記の 4 つがある。
<Bound> はプレースホルダ型で `Array<Element>` の <Element> と同じ。   
Double 型はカウントの単位がわからないので、カウント可能なものの範囲型を使用できない。

* 半開区間
`..<`  
始点 or 終点の一方を含まない範囲。  
* 閉区間
`...`
始点 or 終点の両方を含む範囲。

## Type Aliases

既存の型に対して別名をつけて定義することができる。  
例えば  

```swift:sample.swift
typealias AudioSample = UInt16   
var maxAmplitudeFound = AudioSample.min
```

これは関数に対しても可能

```swift:sample.swift
typealias TestClosure = (Int) -> Void}
```


