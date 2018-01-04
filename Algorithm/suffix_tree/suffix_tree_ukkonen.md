# Suffix tree unkonen

```py
#!/bin/python
#-*- coding:utf-8 -*-

##########################################################################
# giving a string.  generate its suffix tree.
# Ukkonen
##########################################################################
import sys
sys.path.append("..")

from graph.helper import *
from exceptions import Exception

class Node:
    '''
    int start: start pos
    end: 
    int index: node index. as for internal node this change to be negative, leaves to be positive
    '''
    def __init__(self, start, end, index = -1, children = None, suffixLink = None):
        self.start = start
        self.end = end
        self.children = {}
        self.suffixLink = suffixLink
        self.index = index

    def __str__(self):
        return str({
            'start': self.start,
            'end': self.end.end,
            #'children': self.children,
            'suffixLink': self.suffixLink
        })

class EndOfPathException(Exception):
    pass

class SuffixTree:
    class End:
        def __init__(self, end):
            self.end = end

    def __init__(self, p_in):
        self.negtiveIndex = -1
        self.positiveIndex = 0
        self.root = Node(0, SuffixTree.End(0), self.negtiveIndex)
        self.negtiveIndex -= 1
        self.p_in = p_in
        ''' 
            [0] activeNode
            [1] activeEdge
            [2] activeLength
        '''
        self.active = [self.root, -1, 0]
        self.end = SuffixTree.End(0)
        self.remainingSuffixCount = 0
        self.__createSuffixTree()
    
    def __selectActiveChild(self):
        activeNode = self.active[0]
        activeEdge = self.active[1]
        activeLength = self.active[2]
        node = activeNode.children.get(self.p_in[activeEdge])
        return node
    
    def __nextChar(self, i):
        node = self.__selectActiveChild()

        activeNode = self.active[0]
        activeEdge = self.active[1]
        activeLength = self.active[2]

        if self.__diff(node) > activeLength:
            j = node.start + activeLength
            return self.p_in[j]

        if self.__diff(node) == activeLength:
            if node.children.get(self.p_in[i]):
                return self.p_in[i]
        else:
            self.active[0] = node
            #self.active[1] = node.start
            #self.active[2] -= self.__diff(node)
            self.active[2] = activeLength - self.__diff(node)
            self.active[1] = activeEdge + self.__diff(node)
            return self.__nextChar(i)

        raise EndOfPathException('End of Path')


    def __diff(self, node):
        return node.end.end - node.start

    def __walkDown(self, i):
        node = self.__selectActiveChild()
        #active length is greater than path edge length.
        #walk past current node so change active point.
        #This is as per rules of walk down for rule 3 extension.
        if self.__diff(node) <= self.active[2]:
            self.active[0] = node
            self.active[2] = self.active[2] - self.__diff(node) + 1
            self.active[1] = node.children[self.p_in[i]].start
        else:
            self.active[2] += 1
 
    def __startPhase(self, i):
        lastCreatedInternalNode = None
        self.end.end += 1
        self.remainingSuffixCount += 1
        while self.remainingSuffixCount > 0:
            #if active length is 0 then look for current character from root.
            if self.active[2] == 0:
                assert(self.active[0] == self.root)
                node = self.root.children.get(self.p_in[i])
                if node:
                    self.active[1] = node.start
                    self.active[2] += 1
                    #print 'active node[root] no create:', self.active[0]
                    break
                else:
                    self.root.children[self.p_in[i]] = Node(i, self.end, self.positiveIndex)
                    self.positiveIndex += 1
                    self.remainingSuffixCount -= 1
                    #print 'active node[root] create:', self.active[0]
                    break
            else:
                try:
                    ch = self.__nextChar(i)
                    if ch == self.p_in[i]:
                        if lastCreatedInternalNode:
                            lastCreatedInternalNode.suffixLink = self.__selectActiveChild()
                        self.__walkDown(i)
                        break
                    else:
                        #next character is not same as current character so create a new internal node as per 
                        #rule 2 extension.
                        node = self.__selectActiveChild()
                        oldStart = node.start
                        assert(self.active[2])
                        node.start = node.start + self.active[2]
                        #create new internal node
                        newInternalNode = Node(oldStart, SuffixTree.End(node.start), self.negtiveIndex)
                        self.negtiveIndex -= 1

                        #create new leaf node
                        newLeafNode = Node(i, self.end, self.positiveIndex)
                        self.positiveIndex += 1

                        #set internal nodes child as old node and new leaf node.
                        newInternalNode.children[self.p_in[newInternalNode.start + self.active[2]]] = node
                        newInternalNode.children[self.p_in[i]] = newLeafNode
                        self.active[0].children[self.p_in[newInternalNode.start]] = newInternalNode

                        #if another internal node was created in last extension of this phase then suffix link of that
                        #node will be this node.
                        if lastCreatedInternalNode:
                            lastCreatedInternalNode.suffixLink = newInternalNode

                        #set this guy as lastCreatedInternalNode and if new internalNode is created in next extension of this phase
                        #then point suffix of this node to that node. Meanwhile set suffix of this node to root.
                        lastCreatedInternalNode = newInternalNode
                        newInternalNode.suffixLink = self.root

                        #if active node is not root then follow suffix link
                        if self.active[0] != self.root:
                            self.active[0] = self.active[0].suffixLink
                        #if active node is root then increase active index by one and decrease active length by 1
                        else:
                            self.active[1] += 1
                            self.active[2] -= 1

                        self.remainingSuffixCount -= 1
                    
                except EndOfPathException, e:
                    #this happens when we are looking for new character from end of current path edge. Here we already have internal node so
                    #we don't have to create new internal node. Just create a leaf node from here and move to suffix new link.
                    #TODO what about the detail of 
                    node = self.__selectActiveChild()
                    node.children[self.p_in[i]] = Node(i, self.end, self.positiveIndex)
                    self.positiveIndex += 1
                    if lastCreatedInternalNode:
                        lastCreatedInternalNode.suffixLink = node
                    
                    lastCreatedInternalNode = node
                    #if active node is not root then follow suffix link
                    if self.active[0] != self.root:
                        self.active[0] = self.active[0].suffixLink
                    
                    #if active node is root then increase active index by one and decrease active length by 1
                    else:
                        self.active[1] += 1
                        self.active[2] -= 1

                    self.remainingSuffixCount -= 1
                    
    def __setIndexUsingDfs(self, node, val, length):
        if not node:
            return

        val += node.end.end - node.start
        if not node.children:
            node.index = length - val
            assert(node.index >= 0)
            print 'index: ', node.index, 'node: [', node.start, '-', node.end.end, ']'
            return

        for k,v in node.children.items():
            self.__setIndexUsingDfs(v, val, length)

    def __drawTree(self, i=None):
        nodes = []
        edges = []
        self.__dfsTraversal(self.root, nodes, edges)
        #print nodes
        g1 = digraph()
        styles = {
            'edges': {
                'dir': 'forward'
            }
        }
        g1 = apply_styles(g1, styles)
        add_nodes(g1, nodes)
        add_edges(g1, edges)

        if i:
            render(g1, __file__.replace('.py', '/') + __file__ + str(i))
            print __file__.replace('.py', '/') + __file__ + str(i)
        else:
            render(g1, __file__.replace('.py', '/') + __file__)


    def __dfsTraversal(self, node, nodes=[], edges=[]):
        if not node:
            return
        
        def n(v):
            return self.p_in[v.start:v.end.end] + '|' + str(v.start) + '-' + str(v.end.end) + '|' + str(v.index)

        if node == self.root:
            parent = 'root'
            nodes.append(parent)
        else:
            parent = self.p_in[node.start:node.end.end] + '|' + str(node.start) + '-' + str(node.end.end) + '|' + str(node.index)

        for k,v in node.children.items():
            #print v
            child = self.p_in[v.start:v.end.end] + '|' + str(v.start) + '-' + str(v.end.end) + '|' + str(v.index)
            if child not in nodes:
                nodes.append(child)
                #if v.index >= 0:
                #    print "leaves : ", child, "index: ", v.index
            if (parent, child) not in edges: 
                edges.append((parent, child))

            if v.suffixLink:
                edge = ((   
                    child,
                    n(v.suffixLink) if v.suffixLink != self.root else 'root'
                ),{
                    'color': 'red'
                })
                
                if edge not in edges:
                    edges.append(edge)

            self.__dfsTraversal(v, nodes, edges)


    def __createSuffixTree(self):
        root = self.root
        
        for i in range(0, len(self.p_in)):
            self.__startPhase(i)
            print self.p_in[i], self.active[1], self.active[2], self.remainingSuffixCount, self.active[0]
            self.__drawTree(i)
        
        #self.__setIndexUsingDfs(root, 0, len(self.p_in))
        #self.__drawTree()
        print "done"

def main(argv):
    import os
    os.system('./clean.sh ' + __file__)
    print argv
    if len(argv) > 1:
        s = SuffixTree(argv[1])
    else:
        s = SuffixTree("mississi$")

if __name__ == "__main__":
    #s = SuffixTree("xxyxxyyy$")
    #s = SuffixTree("ueiouu$")     
    #s = SuffixTree("mississi$")
    main(sys.argv)

```



