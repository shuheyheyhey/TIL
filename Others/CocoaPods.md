# CocoaPods

## 参考サイト

[公式](https://cocoapods.org/)  
[使用方法](https://guides.cocoapods.org/using/getting-started.html#getting-started)

Cocoa依存のライブラリ管理ツール

## 導入

ruby の パッケージ配布サービス "gem" を使用してインストールする。  
Mac では標準で Ruby 環境が用意されている。
以下のコマンドを実行  
`sudo gem install cocoapods`  

permission denied で弾かれる。
gem でのインストール先は /usr/bin/pod 以下となる。
この場所は Elcapitan? からはシステム側の管理領域箇所とユーザの管理領域箇所がわかれており、sudo でもインストールできない。

### 対策

#### rbenv を使う

rbenv を使ってユーザごとに Ruby 環境を構築する方法

#### Sudo-less installation

以下を参考に。  
https://guides.cocoapods.org/using/getting-started.html

#### インストール先を /usr/local/bin にする

今回はこれで対応。  
以下のコマンドを実行。  
`sudo gem install -n /usr/local/bin cocoapods`  

## setup

`pod setup`  
ローカルの ~/.cocoapods に　github上で管理されている podライブラリの管理情報が展開される。  
[Specリポジトリについて](http://qiita.com/makoto_kw/items/701b1060a6809fd1bb60)

## init

`pod init`
プロジェクトのディレクトリ内で上記コマンドを実行し、cocoapods を有効化する。  
 Podfile が作成され、これにインストールしたいライブラリの情報を記述する。

### Podfile

書き方は以下を参考に。  
https://guides.cocoapods.org/syntax/podfile.html

## install

`pod install`
Podfile に記述されたライブラリがインストールされ、プロジェクトディレクトリ内に該当ライブラリを適用した .xcworkspace が作成される。
.xcproj は元ファイルのままとなっている。

## update  

`pod update LIBRARY_NAME`  
指定したライブラリをアップデートしてくれる。  
まとめてしたい場合は LIBRARY_NAME を省略する。  
→ライブラリ管理情報も pull してくれる？
