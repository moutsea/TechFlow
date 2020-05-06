今天是Python专题的第7篇文章，我们继续介绍迭代相关。

## enumerate

首先介绍的是enumerate函数。

在我们日常编程的过程当中，经常会遇到一个问题。

在C语言以及一些古老的语言当中是没有迭代器这个概念的，所以我们要遍历数组或者是容器的时候，往往只能通过下标。有了迭代器之后，我们遍历的过程方便了很多，我们可以直接用一个变量去迭代一个容器当中的值。最简单的例子就是数组的遍历，比如我们要遍历items这个数组。我们可以直接：

```python
for item in items:
```

通过迭代器的方式我们可以很轻松地遍历数组，而不再需要下标，也不需要计算数组的长度了。但是如果我们在循环体当中需要知道元素的下标该怎么办？比较笨的办法就是我们在循环外创建一个变量来记录位置，比如：

```python
idx = 0
for item in items:
    operation()
    idx += 1
```

这样可以解决问题，但是很麻烦，一点也不简洁，用专业的话来说一点也不pythonic（符合Python标准的代码）。为了追求pythonic，于是有了enumerate函数，来解决了我们又想直接迭代又需要知道元素下标的情形。

它的用法也很简单，我们把需要迭代的对象或者迭代器传入enumerate函数当中，它会为我们创建一个新的迭代器，同时返回下标以及迭代的内容。我们来看一个例子：

```python
for i, item in enumerate(items):
```

除此之外，enumerate还支持传入参数。比如在某些场景当中，我们希望下标从1开始，而不再是0开始，我们可以额外多传入一个参数实现这点：

```python
for i, item in enumerate(items, 1):
```

循环是我们编程的时候必不可少的操作，也正因此，enumerate函数使用非常广泛。但是有一点需要注意，如果我们迭代的是一个多参数的数组，我们需要注意要将index和value区分开。举个例子：

```python
data = [(1, 3), (2, 1), (3, 3)]
```

在不用enumerate的时候，我们有两种迭代方式：

```python
for x, y in data:

for (x, y) in data:
```

但是如果我们使用enumerate的话，由于引入了一个index，我们必须要做区分，否则会报错，所以我们只有一种迭代方式：

```python
for i, (x, y) in enumerate(data):
```

## zip

接下来要介绍的另一个函数同样是方便我们迭代的，不过它针对的是另一个场景——多对象迭代。

它的应用场景非常简单，就是我们同时想迭代多份数据，比如用户的名字和用户的职业数据是分开的，我们希望同时遍历一个用户的职业和名字。如果不使用zip，我们可能只能放弃迭代器回到传统的下标遍历的模式了。这样当然是可以的，不过有两个小问题，第一个小问题当然是代码的可读性变差了，不够pythonic，第二个问题是我们需要维护两个容器长度不一样的情况，会增加额外的代码。而使用zip，可以同时解决以上两个问题。

我们来看一个例子：

```python
names = ['xiaoming', 'xiaohua', 'xiaohei', 'xiaoli']
jobs = ['coach', 'student', 'student', 'student', 'professor']

for name, job in zip(names, jobs):
  print(name, job)
```

最后输出的结果是人名和职业的tuple：

```python
xiaoming coach
xiaohua student
xiaohei student
xiaoli student
```

上面举的例子当中，names和jobs的长度其实是不一致的，在使用了zip的情况下，会自动替我们按照其中较短的那个进行截断。如果我们不希望截断，我们也可以使用itertools下的zip_longest来代替zip：

```python
from itertools import zip_longest
for name, job in zip_longest(names, jobs):
```

这样的话长度不够的元素会以None来填充，zip_longest提供了一个参数fillvalue，可以填充成我们指定的值。

无论是zip还是zip_longest，都可以支持多迭代器的遍历。比如：

```python
names = ['xiaoming', 'xiaohua', 'xiaohei', 'xiaoli']
jobs = ['coach', 'student', 'student', 'student', 'professor']
hobbies = ['footbal', 'tennis', 'badminton', 'basketbal']

for name, job, hobby in zip(names, jobs, hobbies):
  print(name, job, hobby)
```

zip除了方便我们迭代遍历之外，另一个很大的用处是可以很方便地生成dict。比如刚才的例子当中，我们想生成一个名称和职业的dict，一般的办法当然是先定义一个dict，然后遍历所有的key和value，来生成dict。然而使用zip，我们可以将这个操作简化到一行代码：

```python
jobDict = dict(zip(names, jobs))
```

需要注意的是，我们调用zip返回的结果其实是一个迭代器，我们在转化成dict的时候自动遍历了迭代器当中的内容。比如我们如果直接打印出zip调用结果的话，就会发现屏幕上输出的是一个迭代器的地址：

```python
print(zip(names, jobs))
>>> <zip object at 0x10ec93b40>
```

我们想要获得它的内容，需要将它手动转成list：

```python
print(list(zip(names, jobs)))
>>> [('xiaoming', 'coach'), ('xiaohua', 'student'), ('xiaohei', 'student'), ('xiaoli', 'student')]
```
无论是enumerate还是zip其实底层都是基于迭代器实现的，从原理上来说并没有什么太深奥的内容，而且我们不使用它们也不影响我们写代码。但是Python之所以是Python，之所以很多人称道它简洁的语言和逻辑，离不开我们广泛地使用这些简化代码逻辑的工具和方法。因此我们加以了解是非常有必要的，希望大家都能写出pythonic的代码，不仅写代码能力强，而且代码本身也漂亮。

今天的文章就是这些，如果觉得有所收获，请顺手点个关注吧，你们的举手之劳对我来说很重要。

![IMAGE](resources/B7AE373ACC841036368D0E2FA480D876.jpg)