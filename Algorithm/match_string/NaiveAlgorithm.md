# 朴素匹配算法

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

![](/assets/parent.gv.svg)

