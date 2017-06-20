# Command Line Tips

## sort(1)

sort(1)を使って文字順序の整列を行おうとした際に以下のエラーが発生。

```
$ zipinfo /Users/PC-109/Desktop/FileSize/CGPDF/CGPDF1.2.2_Xcode/CypherGuard\ PDF.ipa | sort -k 9
sort: string comparison failed: Illegal byte sequence
sort: Set LC_ALL='C' to work around the problem.
sort: The strings compared were ` Payload/CypherGuard PDF.app/X@2x.png' and ` Payload/CypherGuard PDF.app/\343??\343?\270\343??\343?\246\343?\256\343??\343?\251\343?\263\343??\345??\343??込\343?\277\343?\252\343??.cpd'.
```

文字列の比較箇所で問題が発生するよう。
LC_ALL='C' を指定するとバイトコードでの比較オーダーになり、解決される。

```
$ zipinfo /Users/PC-109/Desktop/FileSize/CGPDF/CGPDF1.2.2_Xcode/CypherGuard\ PDF.ipa | LC_ALL='C' sort -k 9
```

sort(1) のデフォルトのオーダーは何か？  
→環境による。[参考](https://unix.stackexchange.com/questions/43465/whats-the-default-order-of-linux-sort)

