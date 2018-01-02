# KMP 算法

记`len(s) = n, len(p) = m`, 其中`s`是源串，`p`是模式串，在`s`串中找模式串`p`出现的位置。

[http://blog.csdn.net/gesanghuazgy/article/details/52214718](http://blog.csdn.net/gesanghuazgy/article/details/52214718)

摘录：

根据NaiveAlgorithm 其实Next数组就是解决 位置 j 未匹配上时，需要跳步的数目。

KMP 快的原因，只需要跳步模式串，主串不需要回溯

![](/assets/kmp01.jpg)





