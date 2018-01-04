# Suffix tree compress

```py
#!/bin/python
#-*- coding:utf-8 -*-

##########################################################################
#init:
#root (
#	None,
#	{
#		'a': (
#			'abcabe',
#			{},
#		)
#	}
#)
#
#step1
#root (
#	None,
#	{
#		'a': (
#			'abcabe',
#			{},
#		),
#		'b': (
#			'bcabe',
#			{},
#		),
#		'c': (
#			'cabe',
#			{},
#		)
#	}
#)
#
#
#step2
#
#abc abe$
#j = 3, k = 5
#mid = ab[0:2]
#mid.out[e] = 'e$'
#mid.out[c] = 'cabe$'[2:]
#这个时候ab和abcabe边一致
#root (
#	None,
#	{
#		'a': (
#			'ab',
#			{
#				'e':(
#					'e$',
#					{}
#				),
#				'c':(
#					'cabe$',
#					{}
#				)
#			},
#		),
#		'b': (
#			'bcabe',
#			{},
#		),
#		'c': (
#			'cabe',
#			{},
#		)
#	}
#)
##########################################################################
import sys
sys.path.append("..")

from graph.helper import *
from exceptions import Exception

class SuffixTree(object):

    class Node(object):
        def __init__(self, lab):
            self.lab = lab # label on path leading to this node
            self.out = {}  # outgoing edges; maps characters to nodes

    def __init__(self, s):
        """ Make suffix tree, without suffix links, from s in quadratic time
            and linear space """
        self.root = self.Node('root')
        self.root.out[s[0]] = self.Node(s) # trie for just longest suf
        # add the rest of the suffixes, from longest to shortest
        for i in xrange(1, len(s)):
            # start at root; we’ll walk down as far as we can go
            cur = self.root
            j = i
            while j < len(s):
                if s[j] in cur.out:
                    child = cur.out[s[j]]
                    lab = child.lab
                    # Walk along edge until we exhaust edge label or
                    # until we mismatch
                    k = j+1
                    while k-j < len(lab) and s[k] == lab[k-j]:
                        k += 1
                    if k-j == len(lab):
                        cur = child # we exhausted the edge
                        j = k
                    else:
                        # we fell off in middle of edge
                        cExist, cNew = lab[k-j], s[k]
                        # create “mid”: new node bisecting edge
                        mid = self.Node(lab[:k-j])
                        mid.out[cNew] = self.Node(s[k:])
                        # original child becomes mid’s child
                        mid.out[cExist] = child
                        # original child’s label is curtailed
                        child.lab = lab[k-j:]
                        # mid becomes new child of original parent
                        cur.out[s[j]] = mid
                else:
                    # Fell off tree at a node: make new edge hanging off it
                    cur.out[s[j]] = self.Node(s[j:])

    def followPath(self, s):
        """ Follow path given by s.  If we fall off tree, return None.  If we
            finish mid-edge, return (node, offset) where 'node' is child and
            'offset' is label offset.  If we finish on a node, return (node,
            None). """
        cur = self.root
        i = 0
        while i < len(s):
            c = s[i]
            if c not in cur.out:
                return (None, None) # fell off at a node
            child = cur.out[s[i]]
            lab = child.lab
            j = i+1
            while j-i < len(lab) and j < len(s) and s[j] == lab[j-i]:
                j += 1
            if j-i == len(lab):
                cur = child # exhausted edge
                i = j
            elif j == len(s):
                return (child, j-i) # exhausted query string in middle of edge
            else:
                return (None, None) # fell off in the middle of the edge
        return (cur, None) # exhausted query string at internal node

    def hasSubstring(self, s):
        """ Return true iff s appears as a substring """
        node, off = self.followPath(s)
        return node is not None

    def hasSuffix(self, s):
        """ Return true iff s is a suffix """
        node, off = self.followPath(s)
        if node is None:
            return False # fell off the tree
        if off is None:
            # finished on top of a node
            return '$' in node.out
        else:
            # finished at offset 'off' within an edge leading to 'node'
            return node.lab[off] == '$'

    def dfsTraversal(self, node, nodes, edges, index=0):
        if node == self.root:
            nodes.append((str(index), {'label':'root'}))

        parent = str(index)
        for k,v in node.out.items():
            index += 1
            nodes.append((str(index), {'label': v.lab}))
            edges.append((parent, str(index)))

            index = self.dfsTraversal(v, nodes, edges, index)

        return index
        

    def drawTree(self):
        nodes = []
        edges = []
        self.dfsTraversal(self.root, nodes, edges)

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
        s = SuffixTree(argv[1])
    else:
        s = SuffixTree('abcab$')

    s.drawTree()

if __name__ == "__main__":
    #s = SuffixTree("xxyxxyyy$")
    #s = SuffixTree("ueiouu$")     
    #s = SuffixTree("mississi$")
    main(sys.argv)

```



