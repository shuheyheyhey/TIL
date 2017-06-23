# Atom + plantuml-viewer

Atom と パッケージの plantuml-viewer を使ってクラス図を書くことができる。  
かっちりしたものには向いてないが、メモ的な使い方には向いていそう。  

## 準備

* Atom 本体のインストール  
[Atom 公式サイト](https://atom.io/)
* graphviz のインストール  
今回は Homebrew でインストールした。  
`brew install graphviz`
* JRE or JDK
plantuml-viewer は Java ランタイムで動作するので必要

## 導入手順

1. Atom を起動
2. メニューバーの Prefarences.. を開くと Settigs という画面が立ち上がる
3. Install を選択し、以下の 2 つのパッケージをインストールする
	* language-plantuml
	* plantuml-viewer
4. Settigs -> Packages より 「plantuml-viewer」の Settigs を選択し、以下の設定を行う
	* Charset  
	UTF-8
	* Graphviz Dot Executable
	準備した graphviz の dot 実行ファイルを指定する。今回は以下だった。  
	`/usr/local/Cellar/graphviz/2.38.0/bin/dot`
5. メニューバーの File -> New File を選択して新規ファイルを作成
6. Atom のクラス図を表現する記法に従って記述して .pu というファイル形式で保存する
7. 「ctrl + alt + p」でクラス図を確認できる

## クラス図の書き方

http://plantuml.com/class-diagram