今天的这篇文章和大家聊聊Python当中的排序，和很多高级语言一样，Python封装了成熟的排序函数。我们只需要调用内部的sort函数，就可以完成排序。但是实际场景当中，排序的应用往往比较复杂，比如对象类型，当中有多个字段，我们希望按照指定字段排序，或者是希望按照多关键字排序，这个时候就不能简单的函数调用来解决了。

## 字典排序

我们先来看下最常见的字典排序的场景，假设我们有一个字典的数组，字典内有多个字段。我们希望能够根据字典当中的某一个字段来进行排序，我们用实际数据来举个例子：

```python
kids = [
    {'name': 'xiaoming', 'score': 99, 'age': 12},
    {'name': 'xiaohong', 'score': 75, 'age': 13},
    {'name': 'xiaowang', 'score': 88, 'age': 15}
]
```

这里的kids是一个dict类型的数组，dict当中拥有name， score和age三个字段。假设我们当下希望能够按照score来排序，应该怎么办呢？

对于这个问题，解决的方案有很多，首先，我们可以使用上一篇文章当中提到的匿名函数来指定排序的。这里的用法和上篇文章优先队列的用法是一样的，我们直接来看代码：

```python
sorted(kids, key=lambda x: x['score'])
```

在匿名函数当中我们接收的x是kids当中的元素，也就是一个dict，所以我们想要指定我们希望的字段，需要用dict访问元素的方法，也就是用中括号来查找对应字段的值。

假如我们希望按照多关键字排序呢？

首先介绍一下多关键字排序，还是用上面的数据打比方。在上面的例子当中，各个kid的score都不一样，所以排序的结果是确定的。但如果存在两个人的score相等，我希望年龄小的排在前面，那么应该怎么办呢？我们分析一下可以发现，原本是按照分数从小到大排序，但有可能会出现分数相等的情况。这个时候，我们希望能够按照在分数相等的情况下来比较年龄，也就是说我们希望根据两个关键字来排序，第一个关键字是分数，第二个关键字是年龄。

由于Python当中支持tuple和list类型的排序，也就是说我们可以直接比较[1, 3]和[1, 2]的大小关系，Python会自动一次比较两个数组当中的元素的大小。如果相等就自动往后比较，直到出现不等或者结束为止。

明白了这点，其实就很好办了。我们只要在匿名函数当中稍稍修改，让它返回的结果增加一个字段即可。

```python
sorted(kids, key=lambda x: (x['score'], x['age']))
```

## itemgetter

除了匿名函数，Python也有自带的库可以解决这个问题。用法和匿名函数非常接近，使用起来稍稍容易一些。

它就是operator库当中的itemgetter函数，我们直接来看代码：

```python
from operator import itemgetter

sorted(kids, key=itemgetter('score'))
```

如果是多关键字也可以，传入多个key即可：

```python
sorted(kids, key=itemgetter('score', 'age'))
```

## 对象排序

我们接下来看一下对象的自定义排序，我们首先把上面的dict写成对象：

```python
class Kid:
    def __init__(self, name, score, age):
        self.name = name
        self.score = score
        self.age = age

    def __repr__(self):
        return 'Kid, name: {}, score: {}, age:{}'.format(self.name, self.score, self.age)
```

为了方便观察打印结果，我们重载了__repr__方法，可以简单地将它当做是Java当中的toString方法，这样我们可以指定在print它的时候的输出结果。

同样，operator当中也提供了对象的排序因子函数，用法上和itemgetter一样，只是名字不同。

```python
from operator import attrgetter

kids = [Kid('xiaoming', 99, 12), Kid('xiaohong', 75, 13), Kid('xiaowang', 88, 15)]

sorted(kids, key=attrgetter('score'))
```

我们也可以使用匿名函数lambda来实现：

```python
sorted(kids, key=lambda x: x.score)
```

## 自定义排序

到这里还没有结束，因为仍然存在一些问题解决不了。虽然我们实现了多关键字排序，但是还有一个问题解决不了，就是排序的顺序问题。

我们可以在sorted函数的参数当中传入reverse=True来控制是正序还是倒叙，但是如果我使用多关键字，想要按照某个关键字升序，某个关键字降序怎么办？举个例子，比如说我们想要按照分数降序，年龄升序就没办法通过reverse来解决了，这就是当前解决不了的问题。

那应该怎么办呢？

这个时候就需要终极排序杀器上场了，也就是标题当中所说的自定义排序。也就是说我们自己实现一个定义元素大小的函数，然后让sorted来调用我们这个函数来完成排序。这也是C++和Java等语言的用法。

自定义的函数并不难写，我们随手就来：

```python
def cmp(kid1, kid2):
    return kid1.age < kid2.age if kid1.score == kid2.score else kid1.score > kid2.score
```

如果看不明白，也没关系，我写成完整版：

```python
def cmp(kid1, kid2):
    if kid1.score == kid2.score:
        return kid1.age < kid2.age
    else:
        return kid1.score > kid2.score
```

写完了之后，还没有结束，这个函数是不能直接投入使用的，他和我们之前提到的lambda匿名函数是不一样的。之前的匿名函数只是用来指定字段的，所以我们不能直接将这个函数传递给key，还需要在外面包一层加工处理才可以。不过这一层处理函数Python也已经有现成的工具了，我们可以直接调用，它在functools里，我们来看代码：

```python
from functools import cmp_to_key

sorted(kids, key=cmp_to_key(cmp))
```

我们来看一下cmp_to_key函数里的源码：

```python
def cmp_to_key(mycmp):
    """Convert a cmp= function into a key= function"""
    class K(object):
        __slots__ = ['obj']
        def __init__(self, obj):
            self.obj = obj
        def __lt__(self, other):
            return mycmp(self.obj, other.obj) < 0
        def __gt__(self, other):
            return mycmp(self.obj, other.obj) > 0
        def __eq__(self, other):
            return mycmp(self.obj, other.obj) == 0
        def __le__(self, other):
            return mycmp(self.obj, other.obj) <= 0
        def __ge__(self, other):
            return mycmp(self.obj, other.obj) >= 0
        __hash__ = None
    return K
```

我们可以看到，在函数内部，它其实定义了一个类，然后在类当中重载了比较函数，最后返回的是一个重载了比较函数的新的对象。这些__lt__, __gt__函数就是类当中重载的比较函数。比如__lt__是小于的判断函数，__eq__是相等的函数。那么问题来了，我们能不能直接在Kid类当中重载比较函数呢，这样就可以直接排序了。

答案是确定的，我们当然可以这么办，实际上这也是面向对象当中非常常用的做法。相比于自定义比较函数，我们往往更倾向于在类当中定义好优先级。Python当中实现的方法也很简单，就是我们手动实现一个__lt__函数，sorted默认会将小的元素排在前面，所以我们只用实现__lt__一个函数就够了。这个函数当中传入的参数是另一个对象，我们直接在函数里面写清楚比较逻辑就行了。返回True表示当前对象比other小，否则比other大。

我们附上完整代码：

```python
class Kid:
    def __init__(self, name, score, age):
        self.name = name
        self.score = score
        self.age = age

    def __repr__(self):
        return 'Kid, name: {}, score: {}, age:{}'.format(self.name, self.score, self.age)

    def __lt__(self, other):
        return self.score > other.score or (self.score == other.score and self.age < other.age)
```

实现了比较函数之后，我们直接调用sorted，不用任何其他传参就可以对它进行排序了。

今天的内容虽然难度不大，但是在我们日常编程当中非常常用，经常会出现需要对复杂的对象和内容进行排序的情况，所以希望大家都掌握，因为一定会派上用场的。

今天的文章就是这些，如果觉得有所收获，请顺手点个关注，你们的举手之劳对我来说很重要。

![IMAGE](resources/B7AE373ACC841036368D0E2FA480D876.jpg)