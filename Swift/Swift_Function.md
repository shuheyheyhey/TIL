# Swift 関数とクロージャ

## 関数定義

以下のように `func` をつけて定義する。  
`func testFunc()`

## 引数  

### 外部引数名と内部引数名

外部引数名と内部引数名の 2 つを持つことができる。  
外部引数名はメソッド呼び出し側で指定する引数名を指し、  
内部引数名はメソッド内部で使用する引数名を指す。  

例えば以下の定義があったとして  
`func testFunction(user: String, to group: String)`  
外部からは以下のように呼び出し可能。  
`testFunction(user: "syuhei", to: "Cypher")`  
内部では `group` を引数名として扱うことができる。

外部引数名を省略したい場合は `_` を使用する。  
`func testFunction(_ user: String, _ group: String)`

### デフォルト値

関数の引数にデフォルト値を持たせることが可能。  
`func testFunction(user: String = "test")`

上記は引数指定がなくても呼び出すことが可能。  
`testFunction()`

複数の引数を受けるメソッドで常に全ての引数を指定する必要がない場合に使用するとよい。  

### inout 引数

要はポインター引数で引数を参照し、渡された値を変更することで外部にも影響を与えるもの。  
`func testFunction(user: inout String)`  
呼び出し元は `&` を引数につけて呼び出す。  
`testFunction(user: &testString)`

低レベルな API にアクセスするためには必要になってくるだろう。  
Swift は基本的にメモリの直接アクセスはできない設計になっている。  
必要に迫られない限り使用すべきでないかも。  
[参考](https://news.realm.io/jp/news/ray-fix-tryswift-tokyo-the-safety-of-unsafe-swift/)  

### 可変長引数

任意個の引数を受けるには引数定義末尾に `...`を追記する。  
一つの関数に対しては最大で一つの可変長引数を定義できる。  
`func testFunction(strings: String...)`

呼び出し側は `,` で区切って引数を指定する。
`testFunction(strings: "yukawa", "syuhei")`

関数内部では Array<E>型 として扱われる。  

## クロージャ

無名関数。  

### 定義方法

```swift:sample.swft
{(引数名: 型) ->　戻り値 in
  //処理
}
```

### 引数

外部引数名、デフォルト引数は使えない。  
外部引数名が使えないので、定義されたクロージャを呼び出すには直接引数を指定することとなる。

```swift:sample.swift
let double = {(x: Int) -> Int in
  x * 2
}

double(2)
```

上記で return を記載していないが、クロージャが一つの式で終わる場合は return が省略可能。  

#### 簡略引数名

内部引数名を省略できる。  
じゃどうやってアクセスするのかというと "$ + インデックス" を使用する。  

```swift:sample.swift
let isEqual: (Int, Int) -> Bool = {
    return $0 == $1
}

isEqual(1, 1)
```
※書き方が変わっているが、引数と戻り値が型アノテーションによって明確になっているため、  
クロージャでは型を省略している。

### 変数と定数のキャプチャ

クロージャはクロージャ自体が定義されるスコープ内の定数、変数をキャプチャし、使用することができる。
以下では `do` キーワードを使用して新しいスコープを作成する。  

```swift:sample.swift
let greeting: (String) -> String
do {
  let symbol = "!"
  greeting = { user in
    return "Hello, \(user)\(symbol)"
  }
  greeting("yukawa")
  symbol // 別スコープ定義されているためエラー
}
```

`symbol` へのアクセスはスコープ外からのアクセスとなるため実行できない。  
greeting は外部からアクセスされているにも関わらず symbol へのアクセスができている。  
これはクロージャ自身が定義されたスコープの変数や参照を保持することによって実現されている。  
→ ObjC と同様にキャプチャされた変数は strong となる。循環参照には注意が必要。  

 ### 引数としてのクロージャ

 #### 属性

 クロージャに指定できる属性として `@escaping` と `@autoclosure` がある。  

 ##### escaping

非同期的に実行されるクロージャ  
関数に引数として渡したクロージャが関数のスコープ外で保持される可能性があるものにつける属性。  
例えばキューにタスクを追加していく実装を考えたとき、  
引数としてクロージャを受け取る関数とは別の箇所で実行されるだろう。  

```swift:sample.swift
var queue = [() -> void]()
func enqueue(operation: @escaping () -> Void) {
  queue.append(operation)
}

enqueue{ print("execute1") }
enqueue{ print("execute2") }

queueu.forEach{ $0() }
```

##### autoclosure

クロージャを用いた遅延評価  
Swift は他の多くのプログラミング言語と同様に関数としての引数が、その関数に引き渡される前に実行される。  

```swift:sample.swift
func or(_ lhs: Bool, _ rhs: Bool) -> Bool {
  if lhs {
    print("true")
    return true
  } else {
    print("false")
    return rhs()
  }
}

func lhs() -> Bool {
  print("lhs excuted")
  return true
}

func rhs() -> Bool {
  print("rhs excuted")
  return false
}

or(lhs(), rhs())

```

出力結果は以下となる。  

```
lhs excuted
rhs excuted
true
```

このケースでは第二引数を実行する必要はない。  
以下のように第二引数をクロージャとすることで第二引数を必要な時に実行するようにできる。  
これを遅延評価という。  
```swift:sample.swift
func or(_ lhs: Bool, _ rhs: () -> Bool) -> Bool {
・
・
・
or(lhs(), {return rhs()})
```

出力結果は以下となる。  

```
lhs excuted
true
```

しかし呼び出し側が煩雑になり可読性が悪い。  
これに対して @autoclosure 属性を使用する。  
これは引数をクロージャで包むという暗黙的な処理する属性となる。  

```  
```swift:sample.swift
func or(_ lhs: Bool, _ rhs: @autoclosure () -> Bool) -> Bool {
・
・
・
or(lhs(), rhs())
```

## トレイリングクロージャ

関数の最後の引数がクロージャである場合に、クロージャを() の外に押し出す記法。  

```swift:sample.swift
func excute(pram: Int, handler: (String) -> Void) {
  handler("param is \(param")
}

//　トレイリングクロージャ不使用
excute(pram: 1, handler: {string in
  print(string)
})

// トレイリングクロージャ
excute(pram: 1) {string in
  print(string)
}
```

クロージャの処理が複数行にまたがる場合に見にくくなるので、可読性をあげるために使用する。  
→個人的には大差ないんじゃないかと思う。  
