今天是**Python专题的第11篇**文章，我们来聊聊面向对象的一些进阶使用。

## \_\_slots__

如果你看过github当中一些大牛的代码，你会发现很多大牛经常在类的顶部加上__slots__关键字。如果你足够好奇，你可能会试着把这个关键字去掉再运行试试，你会发现去掉了之后什么也没有发生，一切依然运行得很好。

那么这个__slots__关键字究竟是做什么的呢？

它主要有两个功能，我们先来说第一个功能，就是**限制用户的使用**。

我们都知道Python是一门非常灵活的**动态语言**，很多在其他语言看起来完全不能容忍的事情在Python当中是可行的，这也是Python的设计理念，为了灵活和代码方便牺牲了效率。比如我们来看一个很简单的例子，由于Python是动态语言，所以类的成员甚至可以在类创建好了之后动态创建。这在静态语言当中是绝对不行的，我们只能调用类当中已有的属性，是**不能或者很难**添加新属性的。

比如这段代码：

```python
class Exp:
    def __init__(self):
        self.a = None
        self.b = None


if __name__ == "__main__":
    exp = Exp()
    exp.c = 3
    print(exp.c)
```

我们定义了一个类叫做Exp，我们为它创建了a和b两个成员。但是我们在使用的时候，**对c成员进行了赋值**。要知道Exp类当中是没有成员c的，但是程序并不会报错，我们这么运行了之后它会将c添加进这个实例当中。

从一方面来看，这当然非常灵活，但是另一方面，这也留下了**隐患**。如果用户随意添加属性，可能会导致未知的问题，尤其在复杂的系统当中。所以有些时候为了严谨，我们会不希望用户做这种动态的修改。__slots__正是用来做这个的。

我们把这个关键字加上，再来运行结果就不一样了：

```python
class Exp:

    __slots__ = ['a', 'b']
    def __init__(self):
        self.a = None
        self.b = None


if __name__ == "__main__":
    exp = Exp()
    exp.c = 3
    print(exp.c)
```


如果你运行这段代码的话，你会得到一个报错，**提示你Exp这个对象当中并没有c这个成员**，也就是说我们只能运用__slots__这个关键字当中定义的成员，对于没有定义的成员不能随意创建，这样就限制了用户的使用。

虽然现在大部分人使用这个关键字都是报着这个目的，但是很遗憾的是，Python创建者的初衷其实并不是这个。这就谈到了__slots__关键字的第二个作用，就是**节省内存**。

如果了解过Python底层的实现原理，你会发现在Python当中**为每一个实例都创建了一个字典**，就是大名鼎鼎的__dict__字典。正是因为背后有一个字典，所以我们才可以创造出原本不存在的成员，也才支持这样动态的效果。我们可以人工地调用这个字典输出其中的内容，我们在加上__slots__关键字之前，输出的结果是这样的：

```python
{'a': None, 'b': None}
```

但是加上了这个关键字之后，会**得到一个报错**，会告诉你Exp这个对象当中没有__dict__这个成员。原因很简单，因为使用dict来维护实例，会消耗大量的内存，额外存储了许多数据，而使用__slots__之后，Python内部将不再为实例创建一个字典来维护，而是会使用一个固定大小的数组，这样就节省了大量的空间。这个节省可不是一点半点，一般可以**节省一半以上**。也就是说牺牲了一定的灵活性，保证了性能。这一点也是__slots__这个关键字设计的初衷，但是现在很多人都用错了地方。

## property

这个关键字在的文章当中曾经提到过，不过很不好意思的是，由于之前写文章的时候对它的了解还很有限，导致一些阐述存在一些谬误，所以这里再提一下这个关键字的运用作为弥补。

property可以帮我们**绑定类当中一些属性的赋值和获取**，也就是get和set。我们来看个例子：

```python
class Exp:
    def __init__(self, param):
        self.param = param

    @property
    def param(self):
        return self._param

    @param.setter
    def param(self, value):
        self._param = value
```

这里的property注解会在我们调用.param的时候被执行，而param.setter会在我们为param这个属性赋值的时候被执行。所以你可能会奇怪，为什么我们在__init__方法当中初始化的时候用的是self.param = param而不是self._param = param，这是因为我们在执行前者的时候，Python**一样会调用@param.setter**这个注解，所以我们没有必要写成后者的形式。当然你也可以这么写，不过两者是完全等价的。

作为一个前Java程序员**为类当中所有变量加上get和set方法**几乎成了政治正确，所以我特别喜欢为类当中所有的属性加上property。但是**这是不对的**，加上property是非常耗时的，所以如非必要不要这么做，我们直接调用来进行赋值就好了，如果有必要，我们可以手动写上get和set方法。那么问题来了，既然不是为了规范，那么我们又为什么要用到property呢？

答案很简单，为了**校验变量类型**。

由于Python是动态语言，并且是隐式类型的，所以我们拿到变量的时候并不知道它究竟是什么类型，也不知道用户为给它赋值成什么类型。所以在一些情况下我们可能会希望做好限制，告诉用户只能将这个变量赋值成这个类型，否则就会报错。通过使用property，我们可以很方便地做到这点。

```python
class Exp:

    def __init__(self, param):
        self.param = param

    @property
    def param(self):
        return self._param

    @param.setter
    def param(self, value):
        if not isinstance(value, str):
            raise TypeError('Want a string')
        self._param = value
```

除此之外，property还有一个用法是**代替函数**。举个例子：

```python
class Exp:

    def __init__(self, param):
        self.param = param

    @property
    def param(self):
        return self._param

    @param.setter
    def param(self, value):
        if not isinstance(value, str):
            raise TypeError('Want a string')
        self._param = value

    @property
    def hello(self):
        return 'hello ' + self.param
```

这样我们就可以通过.hello来代替调用一个函数，这样做其实是一种**动态计算**。hello的结果并没有被存储起来，之后当我们调用的时候才会执行，在一些场景下这样做会非常方便。

## 命名规范

最后我们来看下Python对象当中的命名规范，在之前的文章当中我们曾经说过，在Python当中没有对public和private的字段做区分，**所有的字段都是public的**，也就是说用户可以拿到类当中所有的字段和方法。为了规范，程序员们约定俗成，决定所有**加了下划线的方法和变量都看成是private**的，即使我们能调用，但是一般情况下我们也不这么干。

所以我们通常会写两个方法，一个是公开的接口，一个是内部的实现。我们调用的时候只调用公开的接口，公开的接口再去调用内部的实现。这在Python当中已经成了惯例，因为我们在调用内部方法的时候，往往还会传入一些内部的参数。

我们来看个简单的例子：

```python
class ExpA:

    def __init__(self):
        pass

    def public_func(self):
       self._private_func()

    def _private_func(self):
        print('private ExpA')


if __name__ == "__main__":
    exp = ExpA()
    exp.public_func()
```

除了_之外我们经常还会看到一些**两个下划线的变量和方法**，那么它们之间又有什么区别呢？

为了回答这个问题，我们来看下面这个例子：

```python
class ExpA:

    def __init__(self):
        pass

    def public_func(self):
       self.__private_func()

    def __private_func(self):
        print('private ExpA')

class ExpB(ExpA):

    def __init__(self):
        pass

    def public_func(self):
       self.__private_func()

    def __private_func(self):
        print('private ExpB')

if __name__ == "__main__":
    exp = ExpB()
    exp.public_func()
    exp._ExpB__private_func()
    exp._ExpA__private_func()
```

请问最后会输出什么？

我们试一下就知道，第一行输出的是private ExpB，这个没有问题。但是后面两个是什么？

后面两个就是\_\_private\_func，只不过**系统自动将它重新命名**了。重新命名的原因也很简单，因为Python**禁止加了两个下划线的方法被子类覆盖**。所以这两者的区别就在这里，它们都被认为是private的方法和属性，但是一个下划线允许子类覆盖，而两个下划线不行。所以如果我们在开发的时候希望我们某一个方法不会被子类覆盖，那么我们就需要加上两个下划线。

最后，我们来看一个小问题。在C++当中当我们的变量名和系统的关键字冲突的时候，我们往往会在变量前面加上一个\_来作为区分。但是由于Python当中下划线被赋予了含义，所以我们不能这么干，那么当变量冲突的时候应该怎么办呢？答案也很简单，我们可以**把下划线加在后面**，比如lambda\_。

## 总结

回顾一下今天的内容，主要是__slots__, property和下划线在类当中的使用。这三者都是Python面向对象当中**经常用到**的知识，了解它们不但可以让我们写出更规范的代码，也有助于帮助我们理解其他大牛的源码，因此是非常必要的。

今天的文章就是这些，如果觉得有所收获，请顺手点个**关注**吧，你们的举手之劳对我来说很重要。

![IMAGE](resources/B7AE373ACC841036368D0E2FA480D876.jpg)