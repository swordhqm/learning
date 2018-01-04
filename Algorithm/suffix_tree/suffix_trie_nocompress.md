#suffix trie no compress

```
#!/bin/python
#-*- coding:utf-8 -*-

##########################################################################
# construct a non compressed suffix trie
# draw it with graphviz
##########################################################################
import sys
sys.path.append("..")

from graph.helper import *
from exceptions import Exception

class SuffixTrie(object):

    def __init__(self, t):
        """ Make suffix trie from t """
        self.root = {}
        for i in xrange(len(t)): # for each suffix
            cur = self.root
            for c in t[i:]: # for each character in i'th suffix
                if c not in cur:
                    cur[c] = {} # add outgoing edge if necessary
                cur = cur[c]

    def followPath(self, s):
        """ Follow path given by characters of s.  Return node at
            end of path, or None if we fall off. """
        cur = self.root
        for c in s:
            if c not in cur:
                return None
            cur = cur[c]
        return cur

    def hasSubstring(self, s):
        """ Return true iff s appears as a substring of t """
        return self.followPath(s) is not None

    def hasSuffix(self, s):
        """ Return true iff s is a suffix of t """
        node = self.followPath(s)
        return node is not None and '$' in node
    
    def __dfsTraversal(self, node, nodes, edges, index = 0):
        """ get nodes & edges"""
        if node == self.root:
            nodes.append((str(index), {'label': 'root'}))
    
        parent = str(index)
        for k,v in node.items():
            index += 1
            print index
            nodes.append((str(index), {'label': k}))
            edges.append((parent, str(index)))
            index = self.__dfsTraversal(v, nodes, edges, index)

        return index
    
    def drawTree(self):
        nodes = []
        edges = []
        self.__dfsTraversal(self.root, nodes, edges, 0)
        print nodes
        print "==================================================="
        print edges
        g1 = digraph()
        styles = { 
            'edges': {
                'dir': 'forward'
            }   
        }
        g1 = apply_styles(g1, styles)
        add_nodes(g1, nodes)
        add_edges(g1, edges)
        render(g1, __file__.replace('.py', '/') + __file__)

def main(argv):
    import os
    os.system('./clean.sh ' + __file__)
    if len(argv) > 1:
        t = SuffixTrie(argv[1])
    else:
        t = SuffixTrie("$")
    t.drawTree()
    pass
if __name__ == "__main__":
    #s = SuffixTree("xxyxxyyy$")
    #s = SuffixTree("ueiouu$")     
    #s = SuffixTree("mississi$")
    main(sys.argv)

```



