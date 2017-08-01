# UIテスト自動化

## 参考文献

* Xctest or appium
   http://bradleygolden.me/2016/03/03/appium-vs-xcodes-xctest/

* XCTest
    https://github.com/apple/swift-corelibs-xctest
    http://qiita.com/taji-taji/items/c00e5b94376c37f17443

* Appium
    http://www.atmarkit.co.jp/ait/articles/1504/27/news025.html
    http://www.atmarkit.co.jp/ait/articles/1506/02/news017.html

## XCTest について

### 特徴

Xcode プロジェクトにUIテストプロジェクトを作成して実行する。  
プロジェクトコードを利用でき、使い慣れたアプリケーションコードを使用できる。  

### 実際に触ってのメモ

* SecureTextField へのアクセス
UITextField をSecure にした場合は "secureTextFields" でアクセスする。
* 特定のUIテストクラスを実行させたい場合
コマンドで特定のクラスを実行させたい場合は -only-testing: を使用する
[参考](https://stackoverflow.com/questions/19335504/is-it-possible-to-run-individual-test-cases-test-classes-on-the-command-line-wit)

## Appium について

### 特徴  

Appium はクライアント／サーバー型の構成となっている。  
クライアントは API を通じてテストコード(スクリプト)からの操作を Mobile JSON Wire Protocol 変換して、  
Appium サーバに送る。Appium サーバーは Mobile JSON Wire Protocol を解釈し、テストを行い、
処理結果をクライアントに返す。  
Android や iOS で共通のテストコードを利用できる。
XCTest に対して環境構築が必要な分、導入コストがかかる。

### 導入

http://kissybnts.hatenablog.com/entry/2016/09/04/153012
http://qiita.com/bittersweet01/items/bb6d9eabaf7700c6eb0c

## UI テストのためのデザインパターン

###  PageObject

http://softwaretest.jp/labo/tech/labo-292/

