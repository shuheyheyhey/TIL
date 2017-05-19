 コマンドラインでの Swift のコンパイル  
  
Xcode が入っていれば *swiftc* を使ってコンパイルできる。  
使い方は clang などとほぼ同じ。  
詳しくは  
    swiftc --help
  
アセンブリ出力したい場合  
    swiftc -S test.swift
