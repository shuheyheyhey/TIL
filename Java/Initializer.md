# スタティックイニシャライザとインスタンスイニシャライザ
http://promamo.com/?p=4139

* スタティックイニシャライザ
クラス変数(static な変数) を初期化する際に使用する。　　
クラス変数はインスタンス化されていなくても使用可能で、
コンストラクタを呼び出す必要はない。
なのでクラス変数を初期化するためにスタティックイニシャライザを使って初期化する。
```java:sample

```
* インスタンスイニシャライザ  
インスタンス変数のイニシャライズで使用する。  
コンストラクタを使えばいいのではと思うが、
例えばコンストラクタをオーバーロードするような実装において、
オーバロード側で処理を記述せずに共通処理をインスタンスイニシャライザに追い出すことができる
(継承元で書いとけばとも思うけど)。
```java:sample
public class TestClass {
	public int testValue;
	
	{
		
	} 
}
```
* どっちが先に呼び出される？
クラスの初期化時点でスタティックイニシャライザは呼び出される。
なのでインスタンスイニシャライザよりも早い。
```
public class TestClass {
	public static int classValue;
	public int instanceValue;
	
	static {
		classValue = 0;
		System.out.println("called static initializer");
	}
	
	{
		instanceValue = 0;
		
	}
}

