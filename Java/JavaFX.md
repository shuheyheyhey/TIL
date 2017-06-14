# JavaFX
================================

[公式ページ](http://www.oracle.com/technetwork/jp/java/javafx/overview/index.html)  
JVM 上で動作するアプリケーションの GUI ライブラリ  
今回使用してわからなかった点をまとめる。  

## 役割について

FXML、CSS と Java コードが登場したが、それぞれの役割が曖昧だった。自分的に分かりやすい iOS や Mac での言い方に置き換えてみる。  

* FXML  
Interface Builder。Viewコンポーネントの配置を書き出すことができる。  
* CSS  
各 View コンポーネントのデザイン。Auto Layout 的な位置づけに近い？  
* Java コード  
ロジック。ViewController にもなるし、Model にだってなる。  

## FXML

[チュートリアル](https://docs.oracle.com/javase/jp/8/javafx/fxml-tutorial/)

FXML を使用することでアプリケーションロジックから切り離してユーザインターフェースを作成することができる。

### Contorller への関連づけ

FXML ルートオブジェクトに対して、Contoller が何であるかの定義を追加する。ちなみにルートオブジェクトは Container である必要はない。
```
	fx:controller="jp.example.TabPaneRootController"
```

ちょっと疑問メモ：  
FXML 内に各コンポーネントを書き込んでそれぞれ別々のコントローラを割り振ることは可能なのか？  
→できない。コントローラーはルートコンポーネントのみに関連づけれる。

### Controller からのアクセス

* FXML
操作したいコンポーネントに id をつける。  
`fx:id="value"`
* Controller  
アノテーション `@FXML` を使用する。  
`@FXML private String value`

ちょっと疑問メモ：  
調べてみるとアクセスレベル `private` がよく使われているが、その必要はあるのか？
→Contller からのアクセスしかありえないわけだしアクセスレベル最小が妥当。

### Initialize

Controller に View の持つ各コンポーネントの初期化を行いたい場合は `javafx.fxml.Initializable.initialize(URL url, ResourceBundle resourcebundle)` を使用する。
このメソッドは FXML をロードしたあとに呼び出される。

ちょっと疑問メモ：
Controller は所詮 Java クラスなのだからコンストラクタで代用できるか？  
→無理。initialize メソッドよりもコンストラクタの方が早い段階で呼び出される。FXML のロードが完了しない限り、定義されたコンポーネントの所得はできない。View の初期化としてコンストラクタは使えない。ただコンストラクタで値だけ準備しておいて、initializa で値をバインドするのはいいかも。

## Develop Tips

### TabPane の各タブの表示内容を別 FXML ファイルから参照したい場合

今回は FXML ファイル内から別の FXML を読み込むように処理を書いた。この方法では `fx:include` タグを使って、ルートコンポーネントにサブコンポーネントをネストさせる。  
以下、実装例  

	<TabPane id="TabPane" prefHeight="310.0" prefWidth="600.0" tabClosingPolicy="UNAVAILABLE">
        <tabs>
          <Tab fx:id="PrefarencesPane" text="Prefarences">
            <content>
              <fx:include fx:id="PrefarencesContent" source="Prefarences.fxml" />
            </content>
          </Tab>
          <Tab fx:id="SubscriptionPane" text="Subscription">
            <content>
              <fx:include fx:id="SubscriptionContent" source="Subscription.fxml" />
            </content>
          </Tab>
          <Tab fx:id="AppsPane" text="Apps">
            <content>
              <fx:include fx:id="AppsContent" source="Apps.fxml" />
            </content>
          </Tab>
          <Tab fx:id="ProjectsPane" text="Projects">
            <content>
              <fx:include fx:id="ProjectsContent" source="Prefarences.fxml" />
            </content>
          </Tab>
        </tabs>
      </TabPane>

### メニューバー表示をシステムに合わせる

MenuBar コンポーネントの useSystemMenuBar を true に設定する。  
```
<MenuBar id="Menu" useSystemMenuBar="true">
```

### Localized の方法

追記予定
      
### 親コンポーネントに合わせてサイズを調整する

追記予定