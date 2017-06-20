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


## 文字列

String 型は Charcter 型がシーケンシャルに並んでおり、このシーケンシャルに並んでいる型が CharcterView 型である。
Charcter 型 -> CharcterView 型 -> String 型

### 比較

`==` を使用できる。  
比較は Unicode の正準等価に基づいて判断される。  
→見た目、機能面から判断

## 真偽値
  
Int と Bool は明確に違って、`i = 1` と `i = true` は同値ではない。
以下に例を示す。  

```swift:sample.swift
let i = 1
if i { 
	// コンパイルエラー
	// 条件式は i == 1 としなければいけない。
}
```

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


