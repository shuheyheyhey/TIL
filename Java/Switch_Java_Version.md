# Java バージョンの切り替え方

あるシステムのテストの際に不具合が発生。  
不具合が発生する端末の Java のバージョンが古く、  
原因切り分けのため、自身の環境の Java バージョンを落として試す機会があった。  
環境は macOS 10.12

参考  
http://www.task-notes.com/entry/20150406/1428289200


## Java のバージョン確認  

`$ java -version`

## リンク先の確認  

```
$ ls -l  /usr/bin/java  
lrwxr-xr-x  1 root  wheel  74 10  3  2016 /usr/bin/java -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/java
```

## インストールされている Java のバージョン確認

`$ /usr/libexec/java_home -V`

## 現在使用しているバージョン  

`$ /usr/libexec/java_home`

## 切り替え  

[java_home についての説明](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/java_home.1.html) を見る限り、
java_home のリンク先の設定によって実行環境を切り替えているよう。
以下を実行すれば切り替えが可能になる。

```
$ export JAVA_HOME=$(/usr/libexec/java_home -v ${指定したいバージョン})
```
