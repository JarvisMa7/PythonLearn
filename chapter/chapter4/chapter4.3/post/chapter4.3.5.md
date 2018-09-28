# 4.3.5 标准库中的生成器函数

`itertools` 模块提供了很多生成器函数，这些函数处理可迭代的对象，并且返回生成器（节省内存，可迭代）。想要了解生成器函数，唯一可能的知识来源就是这篇[官方文档](https://docs.python.org/3/library/itertools.html#module-itertools)，本节会做简单的翻译和解释。它把生成器函数分为三大类。

第一类函数返回的是无限生成器：
    
    
| 函数名 | 参数 | 返回结果 | 示例 |
| --- | --- | --- | --- |
| count() | start, [step] | start, start+step, start+2*step, … | count(10) --> 10 11 12 13 14 ... |
| copy() | p（数组） | p0, p1, ...， pn, p0, p1, ... | cycle('ABCD') --> A B C D A B C D ... |
| repeat() | elem [,n] | elem, elem, elem, … 无尽队列或最多 n 次 |repeat(10, 3) --> 10 10 10|

这三个函数的注释都说明得清楚了，配合示例应该非常容易理解。

第二类函数返回的是有限生成器，长度和传入的可迭代对象有关，我选择几个比较常用的列出来 ：


| 函数名 | 参数 | 返回结果 | 示例 |
| --- | --- | --- | --- |
| accumulate() | p [,func] | p0, func(p0, p1), func(p1, p2), func(p2, p3) ... | accumulate([1,2,3,4,5]) --> 1 3 6 10 15 |
| chain() | p, q, … | p0, p1, … plast, q0, q1, … | chain('ABC', 'DEF') --> A B C D E F |
| compress() | data, selectors | (d[0] if s[0]), (d[1] if s[1]), … | compress('ABCDEF', [1,0,1,0,1,1]) --> A C E F |
| dropwhile() | pred, seq | 假设 pred 在第 n 个元素开始不成立：seq[n], seq[n+1] | dropwhile(lambda x: x<5, [1,4,6,4,1]) --> 6 4 1 |
| tee() | it, n | 产出 n 各元素的数组，每个元素可以看做 it 的备份，相当于把 it 复制了 n 份 | tee('ABC', 2) --> g1, g2(迭代 g1 和 g2 都会得到 A、B、C) |

最后一类是可以实现排列组合操作的生成器函数：


| 函数名 | 参数 | 返回结果 |
| --- | --- | --- |
| product() | p, q, … [repeat=1] | 生成各个可迭代对象的笛卡尔积，n 表示每个可迭代对象对象重复几次 |
| permutations() | p[, r] | 序列 p 所有长度为 r 的无重复元素排列 |
| combinations_with_replacement() | p, [r] |  序列 p 所有长度为 r 的有重复元素有序排列|
| combinations() | p, [r] | 序列 p 所有长度为 r 的有重复元素有序排列 |

举例说明：

```python
from itertools import *

# ABC 和 12 的笛卡尔积，所以共有 6 个元素
# 得到：A1、A2、B1、B2、C1、C2
l1 = list(product('ABC', '12'))

# 相当于 product('AB', 'AB')
# 得到：AA、AB、BA、BB
l2 = list(product('AB', repeat=2))

# ABCD 所有所有长度为 2 的无重复、无序子排列
# 每个元素可以和除了自己的另外三个元素组合，因此有 4 * 3 个
# 得到：AB、AC、AD、BA、BC、BD、CA、CB、CD、DA、DB、DC
l3 = list(permutations('ABCD', 2))

# ABCD 所有所有长度为 2 的无重复、有序子排列
# AB 和 BA 会被认为是相同的，所以只有 12 / 2 = 6 个
# AB、AC、AD、BC、BD、CD
l4 = list(combinations('ABCD', 2))

# ABCD 所有所有长度为 2 的有重复、有序子排列
# AA、BB 这样的也合法，所以有 6 + 4 = 10 个
# 得到：AA、AB、AC、AD、BB、BC、BD、CC、CD、DD
l5 = list(combinations_with_replacement('ABCD', 2))
```

本节仅列出了一部分常用的生成器函数，他们是系统库提供的轮子。因此在自己实现关于序列的操作以前，应该思考下这是否是常见操作，系统是否已经提供了轮子。在[官方文档的最后一节](https://docs.python.org/3/library/itertools.html#itertools-recipes) 还有一些基于上述生成器函数的拓展，通过简单的封装了 itertools 模块中的生成器函数，提供了更多常见的函数，比如 take、tail、consume、nth、flatten 等等，**强烈建议阅读一遍并且形成基本印象!**
