# Rabin Karp 算法

```py
#--*--coding:utf-8--*--
##################################################################################################
#rabin karp
#对源串s 用游标i进行遍历，每次取s[i:i+len(p)] 做hash值 记为s..hash..i
#对模式串p 计算hash值，记为p..hash
#比较s..hash..i == p..hash ?
#相等则进行碰撞比较
#复杂度O((n-m+1)m) 原因计算hash值过程应该是O(m)复杂度
##################################################################################################

def ranbin_karp(s, p):
    for i in xrange(len(s) - len(p) + 1):
        if hash(s[i:i + len(p)]) == hash(p[:]):
            k = 0
            for j in range(i, i + len(p)):
                if s[j] != p[k]:
                    break;
                k += 1
            try:
                if j == i + len(p) - 1:
                    return i
            except UnboundLocalError:
                return 0
                
    return -1


print ranbin_karp("abaskdfklasjdflkasjdf", "jdfl")

```



