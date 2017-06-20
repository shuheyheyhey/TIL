# 末尾再帰最適化

[参考](http://www.geocities.jp/m_hiroi/func/abcscm58.html)

関数の末尾で呼び出される処理をアセンブリ言語レベルで置き換えられる最適化のこと。  
関数呼び出しには通常関数のプロローグ、エピローグがある。  
caller 及び callie は書き換えられると困るレジスタを退避させる処理が必要となる。  
この処理ステップを省くよう最適化することを末尾再帰最適化という。  

```C:sample
/* 末尾再帰 */
int fact(int n, int a)
{
  if(n == 0){
    return a;
  } else {
    return fact(n - 1, a * n);
  }
}

/* 繰り返し */
int facti(int n, int a)
{
loop:
  if(n == 0) return a;
  a *= n;
  n--;
  goto loop;
}
```
