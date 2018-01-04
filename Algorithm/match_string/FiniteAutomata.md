# 有限状态自动机模式匹配

Pk 是 Pqa 的后缀

![](/assets/match_string/FiniteAutomata1.bmp)

![](/assets/match_string/FiniteAutomata2.bmp)

![](/assets/match_string/FiniteAutomata3.jpg)



```py
#-*-coding:utf-8-*-

'''
receive a pattern string p, generate a FiniteAutomata
'''
class FiniteAutomata:
    '''
                'a',    'b',    'c'
    0           [ {'a': 1, 'b': 0, 'c': 0},
    1               ...
    2               ...
    .               ...
    .               ...
    .               ...
    m               ...
                )]

    '''
    def __init__(self, p):
        self.p = p
        
        self.table = [] 
        for i in xrange(0, len(self.p) + 1):
            self.table.append({})
        
        #alphas, this could be done within O(len(p))
        self.alphas = set(p)
        
        self.compute_transition()


    def compute_transition(self):
        m = len(self.p)
        
        for i in xrange(0, m + 1):
            for ch in self.alphas:
                k = min(m + 1, i + 2)

                while k > 0:
                    k -= 1
                    if not k:
                        self.table[i].update({ch: 0})

                    #test if Pk is suffix Pq(ch)
                    Pk = self.p[:k]
                    Pq_ch = self.p[:i] + ch
                    if self.isSuffix(Pk, Pq_ch):
                       self.table[i].update({ch: k})
                       break;
    
    def isSuffix(self, suffix, source):
        assert(len(source) >= len(suffix))
        j = len(source)
        for i in xrange(len(suffix) - 1, -1, -1):
            j -= 1 
            if suffix[i] != source[j]:
                return False

        return True


    def searchIn(self, s):
        state, tran = 0, self.table[0]
        for i in xrange(len(s)):
            ch = s[i]
            state = tran.get(ch, 0)
            tran = self.table[state]

            if state == len(self.table) - 1:
                return i - len(self.p)  + 1

        return -1

f = FiniteAutomata("ababaca");
print f.searchIn("aababacbababaca")

```



