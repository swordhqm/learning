# 朴素匹配算法

记`len(s) = n, len(p) = m`, 其中`s`是源串，`p`是模式串，在`s`串中找模式串`p`出现的位置。

最坏情况，最后一次匹配到模式串，那么匹配次数`(n-m+1)m`, 最坏情况，如果`m=n/2`, 那么最后会是$$O(n^2)$$

比如：`s="aaaaaaaaaaab", p="aaaaab"`

```py
def match(s, p): 
    i, j = 0, 0
    for i in xrange(len(s) - len(p) + 1):
        for j in xrange(len(p)):
            if s[i + j] != p[j]:
               break

        if j == len(p) - 1 and s[i + j] == p[j]:
            return i
    return -1

match("asnbsdfb", "bsdf"）
```

![](/assets/naivealgorithm.svg)

