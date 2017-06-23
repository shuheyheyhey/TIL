# Swift Optional\<Wrapped>型

Swift では nil 代入は基本的に禁止である。  
ただ、状況によっては nil の代入が必要な場合があり、その時には `Optional<Wrapped>`型を使用する。プレースホルダ型(ジェネリクス)となっている。  
→`Optional<Int>`, `Optional<String>` のように Wrapped に具体的な型を指定する。  
シンタックスシュガーとして `?` が用意されている。
→`Int?`, `String?` のように  

## Opetional\<Wrapped>型のケース

値の存在を示す .some と不在を示す .none とがある。  
enum で定義されている。

```swift:sample.swift
enum Optional<Wrapped> {
	case none
	case some(Wrapped)
}
```

.none は nil リテラルに対応づけられており、`Optional<Wrapped>.none == nil` が成り立つ。

```swift:sample.swift
// var o1: Int? = nil と等価
var o1 = Optional<Int>.none

// var o2: Int? = 1 と等価(型アノテーションがなければ Optional ではない)
var o2 = Optional<Int>.some(1) 
```

## 型推論

`Optional<Wrapped>`型の型推論は .some に持たせる値からコンパイル時に推論される。    
.none の場合は型推論が不可なため、型アノテーションの記述が必須となる。  

## アンラップ

`Optional<Wrapped>`型の変数をそのまま使用することはできない。  
値を使用する場合にアンラップして型を取り出す操作が必要となる。
以下の 3 つの方法がある。

* オプショナルバインディング  
`if-let` 構文を使用する。

```
if let a = OptionalA {
	// OptionalA が nil(.none)ではない場合に実行される。
}
```

* ?? 演算子  
中値演算子の ?? を使用する。  

```
// OptionalA に値があればその値が代入され、なければ 1 が代入される
let a = OptionalA ?? 1
```

* 強制アンラップ  
! 演算子を使って、強制的に型を取り出す。  

```
let a = OptionalA!
```

もし OptionalA に値がなければ実行時エラーとなる。  
できるだけ使用は避ける。  
→値が保証されている場合やプログラムを終了させたい場合に使用する。

## オプショナルチェイン

アンラップせずに `Optional<Wrapped>`型のプロパティにアクセスする方法。  

```swift:sample.swift
let optionalDouble: Double? = 1.0
let infinite = optionalDouble?.isInfinite
```
もしアクセスしようとする `Optional<Wrapped>`型 が nil だった場合は nil をリターンする。

疑問メモ：  
代入される変数は Optional である必要がある？  

## ImplicitlyUnwrappedOptional\<Wrapped>型

値にアクセスする際に自動で強制アンラップを行う型。  
シンタックスシュガーとして `Int!`, `String!` という書き方が用意されている。  

```swift:sample.swift
var a: Int! = 1
let b = a + 1 // Optional<Wrapped> 型と違ってアンラップの処理は必要ない

var c: Int! = nil
let d = c + 1 // 実行時エラー
```

これも強制アンラップと同様に使用は避けるべき。  
→値が保証されている場合やプログラムを終了させたい場合に使用する。  
