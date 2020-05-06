今天是**Python专题的第8篇**文章。

今天我们依然介绍的是迭代器，不过介绍的是几个比较常用的**高级用法**，在实际场景当中非常实用，可以帮助我们大大简化代码的复杂度。

## 跳过开头

首先是跳过开始部分，这个在我们读取文本的时候最常用。在实际的应用当中，比如记录的日志或者是代码等等，一般来说**头部都会附上一段说明**，或者用注释标注或者是用特殊的符号标记。这些信息是给用到数据的程序员看的，当我们通过代码获取数据的时候，显然是希望可以过滤掉这些信息的。

比如我们有一段数据，它的开头用#做了一些注释：

```python
# This is a data for student
# Rows 100

xiaoming, 17, 99;
xiaoli, 18, 98;
...
```

常规操作当中，我们会创建一个打开文件的迭代器，我们通过遍历这个迭代器去获取文件当中的数据：

```python
with open('xxxx.txt') as f:
    for line in f:
        print(line)
```

如果只是用来输出还好，如果我们需要加工文件当中的数据，那么头部的注释信息就会干扰我们代码的运行。我们当然可以手动加入一些判断，但是这会比较麻烦，代码也不够美观。针对这个问题，一个比较好的解决方案是**dropwhile**。

dropwhile是itemtools当中的一个函数，它可以**接收一个我们自定义的过滤函数和迭代器重新生成一个新的迭代器**，这个新的迭代器当中会过滤掉之前迭代器头部不符合我们要求的数据：

在刚才的例子当中我们想要过滤掉头部加了#注释的部分，我们可以这么操作：

```python
from itertools import dropwhile
with open('xxxx.txt') as f:
    for line in dropwhile(lambda line: line.startswith('#'), f):
        print(line)
```

这样出来的结果就没有头部我们不需要的内容了。

当我们知道头部不符合情况的数据的格式的时候，可以使用dropwhile来规定过滤的格式。如果我们**知道需要过滤的条数**，则可以使用另外一个工具，叫做islice，它的本质是一个切片函数，就像是Python当中数组的切片功能一样，可以切出迭代器当中指定片段的数据。

举个例子：

```python
from itertools import dropwhile
with open('xxxx.txt') as f:
    for line in islice(f, 3, None):
        print(line)
```

这样我们就会从第三行开始获取，之前的数据会被过滤掉。它其实就代表着数组当中[3: ]的切片操作。

## 迭代排列组合

我们都知道在C++当中有一个叫做next_permutation的函数，可以传入一个数组，返回下一个字典序的排列。在Python当中也有同样的功能，但是是以迭代器的形式使用的。

举个简单的例子，比如我们有a, b, c三个元素，我们希望求出它的所有排列：

```python
items = ['a', 'b', 'c']
from itertools import permutations

for p in permutations(items):
    print(p)
```

permutations还支持多传一个参数，比如上述的排列当中我们希望只保留前两个元素，除了切片之外，我们只需要多传一个参数就好了，like this：

```python
for p in permutations(items, 2):
    print(p)
```

除了排列之外，itertools当中还支持组合，用法还是一样，只是把函数名称换成是combinations而已：

```python
from itertools import combindations
for c in combinations(items):
    print(c)
```

在一般的组合当中，一个元素一旦被选中那么它接下来就会从候选集当中移除，再也不会被选中。如果我们希望获得有放回的组合，我们可以再换一个函数，这个函数名称有点长，但是名字倒也直观叫做combinations_with_replacement。但既然是有放回的抽样，我们**需要设定元素的数量**，否则抽样可以无限进行下去。

```python
for c in combinations_with_replacement(items, 3):
    print(c)
```

## 迭代合并后的序列

上一篇文章当中我们介绍了zip可以同时迭代多个迭代器，除此之外还有一种情况是我们需要**把多个迭代器串起来迭代**。比如系统的日志打在了多个文件当中，我们希望找出其中有error的日志来分析。这个时候，我们希望的不是同时读取多个迭代器，而是希望能够有办法将多个迭代器的内容串联起来。这个功能就是itertools当中的**chain**方法，它接受多个迭代器，当我们遍历的时候，会自动将多个迭代器的内容串联起来，我们可以无缝迭代。

举个例子：

```python
from itertools import chain
nums = [1, 2, 3]
chars = ['a', 'b', 'c']

for i in chain(nums, chars):
    print(i)
```

这样我们会把nums和chars当中的内容一起输出出来，**就好像从头到尾只执行了一个迭代器一样**。

你可能会说我们不用chain也可以实现啊，我们可以这样：

```python
for i in nums + chars:
    print(i)
```

的确，从结果上来看这样也是行得通的。但是如果我们分析一下内部执行的时候的中间变量，会发现当我们执行nums+chars的时候，实际上是**先创建了一个新的临时list**。然后在这个list当中存储nums和chars的数据，也就是说我们迭代的其实是这个新的list。这带来的结果是我们**额外开辟了一段内存**，并且花费了一些时间。如果我们使用chain，它并不会有这样的中间变量，完全是通过迭代器来执行的迭代，非常节省内存，这也是chain的优点。

## 归并迭代的内容

对于归并操作我们应该都不陌生，在之前的归并排序以及一些题解的文章当中我们见过很多次。同样，我们在使用工具合并多个迭代器内容的时候，如果迭代器当中的内容有序，我们也可以**对多个迭代器当中的元素进行归并**，而不再需要我们自己手动操作。

使用我们之前介绍的heapq的库可以非常轻松地做到这一点，我们一起来看一个例子：

```python
a = [1, 3, 5]
b = [2, 4, 6]

import heapq

for c in heapq.merge(a, b):
    print(c)
```

执行之后，我们会得到[1, 2, 3, 4, 5, 6]的结果。也就是说通过heapq.merge操作，我们把多个有序的迭代器合并到了一起。当然我们也可以自己合并，但如果我们只是需要利用当中的数据的话，使用merge操作可以节省内存空间。

到这里内容就结束了，本文和之前的文章基本上列举完了常用的迭代器用法。当然，除了上述讲到的内容之外，Python当中的迭代器还有一些其他的用法，不过相对不太常用，感兴趣的同学可以私下了解。

今天的文章就是这些，如果觉得有所收获，请顺手点个关注吧，你们的举手之劳对我来说很重要。

![IMAGE](resources/B7AE373ACC841036368D0E2FA480D876.jpg)