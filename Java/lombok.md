# lombok

Java のボイラーレートコードを廃止するためのツール  
ボイラーレートコードとは言語仕様上省くことができない定型的なコードを指す。  
例えば getter/setter やコンストラクタ、リソースのクローズなどなど。  

## 使用

lombok の定義する annotatiion をつけることによってボイラーレートコードを排除する。  
例として以下に getter/setter を lombok を使って排除したものを示す。
annotation は他にもある(https://projectlombok.org/features/all) 
* lombok 適用前
```java:
public class Person {
	private int age;
	private String name = "Syuhei";
	
	// Getter
	int getAge() {
		return this.age;	
	}
	
	// Setter
	void setAge(int age) {
		this.age = age;
	}
	
	// Getter
	String getName() {
		return this.name;
	}
}
```
* lombok 適用後
```public class Person {
	@GETTER
	@SETTER
	private int age;
	
	@GETTER
	private String name = "Syuhei";	
}
```
* 利用コード
```java:
public class PersonReader
	public static void main(String[] args) {
		Person person = new Person();
		person.setAge(31);
		
		System.out.println("age = " + person.getAge());
		System.out.println("name = " + person.getName());
	}
}
```

# アノテーション

Java SE 5 から登場したらしい。
クラスやメソッド、パッケージのメタデータとして注釈を記入する。

## コメントと同じでは？

コンパイルされたプログラムの中に情報として意味づけされたオブジェクトを残す。
コメントと大きく異なる部分は、プログラム的に残される点。
プログラムからリフレクションを使って注釈情報などにアクセスできる。

## アノテーションの実装

アノテーションの定義は以下のように行う。
```java:
修飾子 @interface アノテーション型名 { アノテーション本体 }  
```
アノテーション型の実体は java.lang.annotation.Annotation インタフェースを継承する。  
このため extends で他の型を継承することはできない。 

## アノテーションの種類

アノテーションはインターフェースの一種であり以下の種類がある。
* マーカー・アノテーション  
データが無く名前だけを持つアノテーション  
@Override や @Deprecated など  
自前で定義すると
```java
public @interface MarkerAnnotation { }
```
* 単一値アノテーション  
データを一つだけ持つアノテーション。見かけはメソッド呼び出しに似ている。  
例えば @SuppressWarnings のようなもの。
@SuppressWarning はコンパイラに警告を抑止させるため、引数に抑止したい警告文字列をセットして使う。
自前で定義すると以下のような
```java
public @interface SingleAnnotation { 
	String value();
}
```
* フル・アノテーション
複数のデータを持つアノテーション
自前で定義すると以下のような  
```java
public @interface FullAnnotation {
	String value();
	int id();
}
```

