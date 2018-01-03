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
#所以这个看起来和rabin karp算法思想一致，但是hash 部分应该有更高效的方式
#下面提供的hash计算方法 在针对字串连续计算上，复杂度O（1）
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

```py

class CalHash:
    '''
        #优化hash部分核心思想
        # (a + b + c) % d = (a %d + (b+c) %d) %d
        # 若 (a + b + c) %d = t
        # (b + c + e) % d = ((t - a) + e) %d
    '''
    
    #Source('abcad', 3)
    class Source:
        def __init__(self, s, wsize):
            self.s = s
            self.lastHash = None
            self.lastIndex = None
            self.wsize = wsize

            assert(self.wsize <= len(self.s))
              
    '''
        q: factor
        d: 进制
    '''
    def __init__(self, source, q):
        '''
        cal hash by every char
        '''
        self.source = source
        self.d = 256
        self.q = q #hash length

        self.h = 1         
        for i in xrange(self.source.wsize - 1):
            self.h = (self.h * self.d) % self.q

    def calNextHash(self):
        if self.source.wsize <= 0:
            raise Exception('wsize_error')

        if not self.source.lastHash:
            #first time init
            t = 0
            for ch in self.source.s[0:self.source.wsize]:
                t = (t * self.d + ord(ch)) % self.q

            self.source.lastHash = t
            self.source.lastIndex = 0
            
            return t
        else:
            index = self.source.lastIndex + 1

            if len(self.source.s) - index >= self.source.wsize:
                f_ch = self.source.s[self.source.lastIndex]
                l_ch = self.source.s[index + self.source.wsize - 1]
                
                t = self.source.lastHash

		t = (self.d * (t - ord(f_ch)*self.h) + ord(l_ch)) % self.q
		# We might get negative values of t, converting it to
		# positive
		if t < 0:
		    t = t + self.q

                self.source.lastIndex = index
                self.source.lastHash = t

                return t

            else:
                raise Exception('reach_end', 2)

def ranbin_karp1(s, p):
    q = 101
    try:
        p_hash = CalHash(CalHash.Source(p, len(p)), q).calNextHash()
    except Exception, e:
        if e.message == 'wsize_error':
            return 0

    s_calHash = CalHash(CalHash.Source(s, len(p)), q)
    for i in xrange(len(s) - len(p) + 1):
        if s_calHash.calNextHash() == p_hash:
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


#print ranbin_karp("abaskdfklasjdflkasjdf", "jdfl")
s = "asldkjfskaldf"
#s_source = CalHash.Source(s, 3)
#s_calHash = CalHash(s_source, 101)
#print s_calHash.calNextHash()
print ranbin_karp1("ffzzz", "fla")

```



