# Python Tips

## ファイル操作  
[Refarence](https://docs.python.jp/3/library/os.path.html)  
	* ファイルの存在を確認する
	os.path を使用する。  
	以下は.sample というディレクトリ or ファイルが存在するか確認する。
	os.path.isfile(path) や os.path.isdir(path) で確認も可能。  
		
		```python:sample.py
		import os
		path = "./sample"
		os.path.exists(path)
		```
	
	* ファイルの拡張子をチェックする
	
		```python:sample.py
		import os.path
		# root にファイル名、ext に拡張子が入る(例:.png)
		root, ext = os.path.splitext(path)
		```
		
ちょっと疑問で要検証メモ:  
・.区切りがあるファイルでもきちんと拡張子とれるか？  
・本当に確認したければ URI での確認が必要なのでは？

## コマンドライン

* 補完
readline を使ってパス補完する場合

	```python:sample
	class CommandForPath:
   		def setUp(self):
        # パス入力補完を行うためのセットアップ
        readline.set_completer_delims(' \t\n;')                                               
        readline.parse_and_bind("tab: complete")
        readline.parse_and_bind("bind '\t' rl_complete")                                      
        readline.set_completer(self.complete)
	```

## 演算
	
* 論理演算
&& と || は　"and" と "or"
	
	Mask の取り方  
	[参考](http://qiita.com/7shi/items/41d262ca11ea16d85abc)
	
	```python:sample.py
	flag1 = 120
	flag2 = 121
	bin(flag1 & flag2)
	```

* 2進数から int を得る方法
キャスト時に元となる進数を指定する

	```python:sample.py
	flag = 0b111111
	int(flag, 2)
	```

## 参考となるURL

* コマンドライン  
http://qiita.com/koara-local/items/cf4ca8be9eaef92bc238
http://qiita.com/tdrk/items/9b23ad6a58ac4032bb3b
	* 引数  
	http://www.python-izm.com/contents/basis/command_line_arguments.shtml
	* Cmd  
	https://stackoverflow.com/questions/8772303/python-readline-tab-	completion-in-cmd-cmd-when-sys-stdout-has-been-replaced

* プロセス
	* 終了
	http://www.python-izm.com/contents/basis/exit.shtml

* 文字列  
http://qiita.com/tomotaka_ito/items/594ee1396cf982ba9887

* Python で keychain にアクセス  
https://pypi.python.org/pypi/keyring

* クラス  
http://www.tohoho-web.com/python/class.html


    def complete(self, text, state):
        return (glob.glob(text+'*')+[None])[state]
    
    def input(self, prompt):
        line = raw_input(prompt)
        return line
```
