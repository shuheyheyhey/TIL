# Swift 制御構文

## if と guard

オプショナルバインディングにおいて、if-let と guard-let について  

* if-let 例  

```swift:sample.swift
if let a = oprionalA {
	// 値が存在する場合
} else {
	// 値が存在しない場合
}
```

* guard-let 例  
 
```swift:sample.swift
guard let a = optionalA else {
	// 値が存在しない場合
	// スコープから脱出しなければいけない→関数だったら return
}
```

if-let では制御構文のスコープ内でしか変数 a を利用できない。guard は制御構文のスコープを抜けた後は変数 a に値が存在することを保証するものとなる。  

## where キーワード

switch 文において、対象となる変数が指定された条件を満たしているかチェックできる。  

```swift:sample.swift
switch optionalA {
case .some(let a) where a < 2:
	// 変数 a が存在してかつ where キーワード以降の条件を指定できる。
default:
	break
```

→オプショナルバインディングをしてさらに値のチェックが必要になったとき、guard と if を組み合わせるように考えていた。状況によるかもしれないが、使える場所はありそう。

## 繰り返し構文

### for 文
値の連続を表現する Sequence プロトコルに準拠した型の要素にアクセスするための制御構文。 これに準拠した代表的なものに Array<Element>、 Dictionary<Key, Value> などコレクションを表す型がある。

#### for-in 文

```swift:sample.swift
// closed range
for var i in 0...5 {
print(i)
}

// half-open range
for var j 0..<5 {
print(j)
}
// 逆順で回したいなら
for var k in (0...5).reversed() {
print(k)
}
```

##### for-case 文
パターンにマッチする要素のみを取り出す。

```swift:sample.swift
for case 2..<3 in array {
	// マッチしたものがあった場合に入ってくる。
}
```

→値へのアクセスはどのように書けばいいのだろう？

### while 文
指定された条件を繰り返す。  

##### repeat-while 文
指定条件によっては一度も処理が実行されない。  
少なくても 1 回は while 内の処理を行なってほしい場合は repeat-while を使用する。  

```swift:sample.swift
repeat {
	print a
	a += 1
} while a < 1
```

## 制御を移す

break、continue は他の言語のとおり。
fallthrough と break や continue での動作を制御するラベルが独特(というか知らない)。  

### fallthrough 文
C での switch 文において、break しない限り次の case 処理を実行する。  
Swift では break による明示的な指定がなくても、一つの case の処理が終われば switch のスコープから抜けるようになっている。  
もし次の case 処理を引き続き実行させたい場合は fallthrough を指定する。

```swift:sample.swift
switch a {
case 1:
	print("case 1")
	fallthrough
case 2:
	print("case 2")
default:
	break
```
上記の以下のように出力される。   

```
case 1  
case 2
```

### ラベル

ネストされた繰り返し構文の中で break や continue が指定された場合、
それらが指定されたスコープ範囲で有効であり、他スコープでは有効ではない。

```swift:sample.swift
for i in array1 {
	for j in array2 {
		// ここで抜けるのは array2 の for であり、 array1 の for は抜けない。
		break
	} 
}
```

これに対してラベルを使用すると break や continue が効くスコープを指定できる。

```swift:sample.swift
testlabel: for i in array1 {
	for j in array2 {
		// ここで抜けるのは array1 の for から抜ける。
		break testlabel
	} 
}
``` 

## 遅延実行

リソースの解放など、実行フローに関わらず実行させたい処理の記述には defer 文 を使用する。

### defer

defer が記述されているスコープを抜ける際に実行される処理を記述する。

```swift:sample.swift
defer {
	// スコープから退出する際に実行される処理
}
```

## パターンマッチ

値や型の安全性を担保するため、パターンマッチでの確認を行う。
よく使うものとして以下がある。

* 式パターン
* バリューバインディングパターン
* 列挙型パターン
* 連想値パターン
* 型キャスティングパターン
