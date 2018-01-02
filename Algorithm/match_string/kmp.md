# KMP 算法

记`len(s) = n, len(p) = m`, 其中`s`是源串，`p`是模式串，在`s`串中找模式串`p`出现的位置。

[http://blog.csdn.net/gesanghuazgy/article/details/52214718](http://blog.csdn.net/gesanghuazgy/article/details/52214718)

摘录：

根据NaiveAlgorithm 其实Next数组就是解决 位置 j 未匹配上时，需要跳步的数目。

KMP 快的原因，只需要跳步模式串，主串不需要回溯

![](/assets/match_string/kmp01.jpg)

![](/assets/match_string/kmp02.jpg)

核心问题，如何求解Next数组

```
首先next[0]=-1,next[1]=0；
之后每一位j的next求解: 
比较j-1字符与next[j-1]是否相等， 
如果相等则next[j]=next[j-1]+1, 
如果不相等，则比较j-1字符与next[next[j-1]]是否相等， 
1) 如果相等则next[next[j-1]]+1, 
2)如果不相等则继续以此下去，直到next[…]=-1,则next[j]=0.
```

```
==================================前后缀
如果在x位未匹配成功，其实是不存在前后缀 相同，那么计算的Next[7] = 0

s  a b a b a b b x
j= 0 1 2 3 4 5 6
a              b
ab             bb
aba            abb
abab           babb
ababa          ababb
ababab         bababb
abababb        abababb

=================================按上面提供的思路逐步获取Next数组

next[0] = -1
next[1] = 0

j = 2
j-1 字符 s[1] 和 Next[j-1] s[0]
不等
j-1 字符 和 Next[Next[j-1]] 字符
s[1] 和 Next[0] = -1 处字符 由于不存在
Next[2] = 0

j = 3
j-1 字符 s[2] 和 Next[j-1] s[0] 相等
Next[3] = 1

j = 4
j-1 字符 s[3] 和 Next[j-1] s[1] 相等
Next[4] = 2

j = 5
j-1 字符 s[4] 和 Next[j-1] s[2] 相等
Next[5] = 3

j = 6
j-1 字符 s[5] 和 Next[j-1] s[3] 相等
Next[6] = 4

这里要注意前面实际是ababab, Next[6] = 4, 意味着当j=6 未匹配上时候，模式串从位置4 开始比较 实际上相当于模式串向前移动2位。如果还没匹配上 这个时候会变成Next[4] = 2, 也就是模式串会再挪2位。

=================================匹配过程
abab babababbbab
abab abb                j=4, j->Next[4] = 2
  ab ababb                j=2, j->Next[2] = 0
     a bababb                j=0, j->Next[0] = -1
      abababb                i++, 逐个匹配
```



