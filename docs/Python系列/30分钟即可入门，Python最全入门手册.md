最近有许多小伙伴后台联系我，说目前想要学习Python，但是没有一份很好的资料入门。一方面的确现在市面上Python的资料过多，导致新手会不知如何选择，另一个问题很多资料内容也很杂，从1+1到深度学习都包括，**纯粹关注Python本身语法**的优质教材并不太多。

刚好我最近看到一份不错的英文Python入门资料，我将它做了一些**整理和翻译**写下了本文。这份资料非常纯粹，只有Python的基础语法，专门针对想要学习Python的小白。

## 注释

Python中用#表示单行注释，#之后的同行的内容都会被注释掉。

```python
# Python中单行注释用#表示，#之后同行字符全部认为被注释。
```

使用三个连续的双引号表示多行注释，两个多行注释标识之间内容会被视作是注释。

```python
""" 与之对应的是多行注释
    用三个双引号表示，这两段双引号当中的内容都会被视作是注释
"""
```

# 基础变量类型与操作符

Python当中的数字定义和其他语言一样：

```python
#获得一个整数
3
# 获得一个浮点数
10.0
```

我们分别使用+, -, *, /表示加减乘除四则运算符。

```python
1 + 1   # => 2
8 - 1   # => 7
10 * 2  # => 20
35 / 5  # => 7.0
```

这里要注意的是，在Python2当中，10/3这个操作会得到3，而不是3.33333。因为除数和被除数都是整数，所以Python会自动执行整数的计算，帮我们把得到的商取整。如果是10.0 / 3，就会得到3.33333。目前Python2已经不再维护了，可以不用关心其中的细节。

但问题是Python是一个**弱类型**的语言，如果我们在一个函数当中得到两个变量，是无法直接判断它们的类型的。这就导致了同样的计算符可能会得到不同的结果，这非常蛋疼。以至于程序员在运算除法的时候，往往都需要手工加上类型转化符，将被除数转成浮点数。

在Python3当中拨乱反正，修正了这个问题，即使是两个整数相除，并且可以整除的情况下，得到的结果也一定是浮点数。

如果我们想要得到整数，我们可以这么操作：

```python
5 // 3       # => 1
-5 // 3      # => -2
5.0 // 3.0   # => 1.0 # works on floats too
-5.0 // 3.0  # => -2.0
```

两个除号表示**取整除**，Python会为我们保留去除余数的结果。

除了取整除操作之外还有取余数操作，数学上称为取模，Python中用%表示。

```python
# Modulo operation
7 % 3  # => 1
```

Python中支持**乘方运算**，我们可以不用调用额外的函数，而使用\*\*符号来完成：

```python
# Exponentiation (x**y, x to the yth power)
2**3  # => 8
```

当运算比较复杂的时候，我们可以用括号来强制改变运算顺序。

```python
# Enforce precedence with parentheses
1 + 3 * 2  # => 7
(1 + 3) * 2  # => 8
```

## 逻辑运算

Python中用首字母大写的True和False表示真和假。

```python
True  # => True
False  # => False
```

用and表示与操作，or表示或操作，not表示非操作。而不是C++或者是Java当中的&&, || 和！。

```python
# negate with not
not True   # => False
not False  # => True

# Boolean Operators
# Note "and" and "or" are case-sensitive
True and False  # => False
False or True   # => True
```

在Python底层，**True和False其实是1和0**，所以如果我们执行以下操作，是不会报错的，但是在逻辑上毫无意义。

```python
# True and False are actually 1 and 0 but with different keywords
True + True # => 2
True * 8    # => 8
False - 5   # => -5
```

我们用==判断相等的操作，可以看出来True==1， False == 0.

```python
# Comparison operators look at the numerical value of True and False
0 == False  # => True
1 == True   # => True
2 == True   # => False
-5 != False # => True
```

我们要小心Python当中的bool()这个函数，它并不是转成bool类型的意思。如果我们执行这个函数，那么**只有0会被视作是False，其他所有数值都是True**：

```python
bool(0)     # => False
bool(4)     # => True
bool(-6)    # => True
0 and 2     # => 0
-5 or 0     # => -5
```

Python中用==判断相等，>表示大于，>=表示大于等于， <表示小于，<=表示小于等于，!=表示不等。

```python
# Equality is ==
1 == 1  # => True
2 == 1  # => False

# Inequality is !=
1 != 1  # => False
2 != 1  # => True

# More comparisons
1 < 10  # => True
1 > 10  # => False
2 <= 2  # => True
2 >= 2  # => True
```

我们可以用and和or拼装各个逻辑运算：

```python
# Seeing whether a value is in a range
1 < 2 and 2 < 3  # => True
2 < 3 and 3 < 2  # => False
# Chaining makes this look nicer
1 < 2 < 3  # => True
2 < 3 < 2  # => False
```

注意not，and，or之间的优先级，其中not > and > or。如果分不清楚的话，可以用括号强行改变运行顺序。

## list和字符串

关于list的判断，我们常用的判断有两种，一种是刚才介绍的==，还有一种是is。我们有时候也会简单实用is来判断，那么这两者有什么区别呢？我们来看下面的例子：

```python
a = [1, 2, 3, 4]  # Point a at a new list, [1, 2, 3, 4]
b = a             # Point b at what a is pointing to
b is a            # => True, a and b refer to the same object
b == a            # => True, a's and b's objects are equal
b = [1, 2, 3, 4]  # Point b at a new list, [1, 2, 3, 4]
b is a            # => False, a and b do not refer to the same object
b == a            # => True, a's and b's objects are equal
```

Python是全引用的语言，其中的对象都使用引用来表示。is判断的就是**两个引用是否指向同一个对象**，而==则是判断两个引用指向的具体内容是否相等。举个例子，如果我们把引用比喻成地址的话，is就是判断两个变量的是否指向同一个地址，比如说都是沿河东路XX号。而==则是判断这两个地址的收件人是否都叫张三。

显然，住在同一个地址的人一定都叫张三，但是住在不同地址的两个人也可以都叫张三，也可以叫不同的名字。所以如果a is b，那么a == b一定成立，反之则不然。

Python当中对字符串的限制比较松，**双引号和单引号都可以表示字符串**，看个人喜好使用单引号或者是双引号。我个人比较喜欢单引号，因为写起来方便。

字符串也支持+操作，表示两个字符串相连。除此之外，我们把两个字符串写在一起，即使没有+，Python也会为我们拼接：

```python
# Strings are created with " or '
"This is a string."
'This is also a string.'

# Strings can be added too! But try not to do this.
"Hello " + "world!"  # => "Hello world!"
# String literals (but not variables) can be concatenated without using '+'
"Hello " "world!"    # => "Hello world!"
```

我们可以使用[]来查找字符串当中某个位置的字符，用**len**来计算字符串的长度。

```python
# A string can be treated like a list of characters
"This is a string"[0]  # => 'T'

# You can find the length of a string
len("This is a string")  # => 16
```

我们可以在字符串前面**加上f表示格式操作**，并且在格式操作当中也支持运算，比如可以嵌套上len函数等。不过要注意，只有Python3.6以上的版本支持f操作。

```python
# You can also format using f-strings or formatted string literals (in Python 3.6+)
name = "Reiko"
f"She said her name is {name}." # => "She said her name is Reiko"
# You can basically put any Python statement inside the braces and it will be output in the string.
f"{name} is {len(name)} characters long." # => "Reiko is 5 characters long."
```

最后是None的判断，在Python当中None也是一个对象，**所有为None的变量都会指向这个对象**。根据我们前面所说的，既然所有的None都指向同一个地址，我们需要判断一个变量是否是None的时候，可以使用is来进行判断，当然用==也是可以的，不过我们通常使用is。

```python
# None is an object
None  # => None

# Don't use the equality "==" symbol to compare objects to None
# Use "is" instead. This checks for equality of object identity.
"etc" is None  # => False
None is None   # => True
```

理解了None之后，我们再回到之前介绍过的bool()函数，它的用途其实就是判断值是否是空。所有类型的**默认空值会被返回False**，否则都是True。比如0，""，[], {}, ()等。

```python
# None, 0, and empty strings/lists/dicts/tuples all evaluate to False.
# All other values are True
bool(None)# => False
bool(0)   # => False
bool("")  # => False
bool([])  # => False
bool({})  # => False
bool(())  # => False
```

除了上面这些值以外的所有值传入都会得到True。

# 变量与集合

## 输入输出

Python当中的标准输入输出是**input和print**。

print会输出一个字符串，如果传入的不是字符串会自动调用__str__方法转成字符串进行输出。**默认输出会自动换行**，如果想要以不同的字符结尾代替换行，可以传入end参数：

```python
# Python has a print function
print("I'm Python. Nice to meet you!")  # => I'm Python. Nice to meet you!

# By default the print function also prints out a newline at the end.
# Use the optional argument end to change the end string.
print("Hello, World", end="!")  # => Hello, World!
```

使用input时，Python会在命令行接收一行字符串作为输入。可以在input当中传入字符串，会被当成提示输出：

```python
# Simple way to get input data from console
input_string_var = input("Enter some data: ") # Returns the data as a string
# Note: In earlier versions of Python, input() method was named as raw_input()
```

## 变量

Python中声明对象**不需要带上类型**，直接赋值即可，Python会自动关联类型，如果我们使用之前没有声明过的变量则会出发NameError异常。

```python
# There are no declarations, only assignments.
# Convention is to use lower_case_with_underscores
some_var = 5
some_var  # => 5

# Accessing a previously unassigned variable is an exception.
# See Control Flow to learn more about exception handling.
some_unknown_var  # Raises a NameError
```

Python支持**三元表达式**，但是语法和C++不同，使用if else结构，写成：

```python
# if can be used as an expression
# Equivalent of C's '?:' ternary operator
"yahoo!" if 3 > 2 else 2  # => "yahoo!"
```

上段代码等价于：

```python
if 3 > 2:
    return 'yahoo'
else:
    return 2
```

## list

Python中用[]表示空的list，我们也可以直接在其中填充元素进行初始化：

```python
# Lists store sequences
li = []
# You can start with a prefilled list
other_li = [4, 5, 6]
```

使用append和pop可以在list的末尾插入或者删除元素：

```python
# Add stuff to the end of a list with append
li.append(1)    # li is now [1]
li.append(2)    # li is now [1, 2]
li.append(4)    # li is now [1, 2, 4]
li.append(3)    # li is now [1, 2, 4, 3]
# Remove from the end with pop
li.pop()        # => 3 and li is now [1, 2, 4]
# Let's put it back
li.append(3)    # li is now [1, 2, 4, 3] again.
```

list可以通过[]加上下标访问指定位置的元素，如果是负数，则表示**倒序访问**。-1表示最后一个元素，-2表示倒数第二个，以此类推。如果访问的元素超过数组长度，则会出发**IndexError**的错误。

```python
# Access a list like you would any array
li[0]   # => 1
# Look at the last element
li[-1]  # => 3

# Looking out of bounds is an IndexError
li[4]  # Raises an IndexError
```

list支持切片操作，所谓的切片则是从原list当中**拷贝**出指定的一段。我们用start: end的格式来获取切片，注意，这是一个**左闭右开区间**。如果留空表示全部获取，我们也可以额外再加入一个参数表示步长，比如[1:5:2]表示从1号位置开始，步长为2获取元素。得到的结果为[1, 3]。如果步长设置成-1则代表反向遍历。

```python
# You can look at ranges with slice syntax.
# The start index is included, the end index is not
# (It's a closed/open range for you mathy types.)
li[1:3]   # Return list from index 1 to 3 => [2, 4]
li[2:]    # Return list starting from index 2 => [4, 3]
li[:3]    # Return list from beginning until index 3  => [1, 2, 4]
li[::2]   # Return list selecting every second entry => [1, 4]
li[::-1]  # Return list in reverse order => [3, 4, 2, 1]
# Use any combination of these to make advanced slices
# li[start:end:step]
```
如果我们要指定一段区间倒序，则前面的start和end也需要反过来，例如我想要获取[3: 6]区间的倒序，应该写成[6:3:-1]。

只写一个:，表示全部拷贝，如果用is判断拷贝前后的list会得到False。可以使用del删除指定位置的元素，或者可以使用remove方法。

```python
# Make a one layer deep copy using slices
li2 = li[:]  # => li2 = [1, 2, 4, 3] but (li2 is li) will result in false.

# Remove arbitrary elements from a list with "del"
del li[2]  # li is now [1, 2, 3]

# Remove first occurrence of a value
li.remove(2)  # li is now [1, 3]
li.remove(2)  # Raises a ValueError as 2 is not in the list
```

insert方法可以**指定位置插入元素**，index方法可以查询某个元素第一次出现的下标。

```python
# Insert an element at a specific index
li.insert(1, 2)  # li is now [1, 2, 3] again

# Get the index of the first item found matching the argument
li.index(2)  # => 1
li.index(4)  # Raises a ValueError as 4 is not in the list
```

list可以进行加法运算，两个list相加表示list当中的元素合并。**等价于使用extend**方法：

```python
# You can add lists
# Note: values for li and for other_li are not modified.
li + other_li  # => [1, 2, 3, 4, 5, 6]

# Concatenate lists with "extend()"
li.extend(other_li)  # Now li is [1, 2, 3, 4, 5, 6]
```

我们想要判断元素是否在list中出现，可以使用**in关键字**，通过使用len计算list的长度：

```python
# Check for existence in a list with "in"
1 in li  # => True

# Examine the length with "len()"
len(li)  # => 6
```

## tuple

tuple和list非常接近，tuple通过()初始化。和list不同，**tuple是不可变对象**。也就是说tuple一旦生成不可以改变。如果我们修改tuple，会引发TypeError异常。

```python
# Tuples are like lists but are immutable.
tup = (1, 2, 3)
tup[0]      # => 1
tup[0] = 3  # Raises a TypeError
```

由于小括号是有改变优先级的含义，所以我们定义单个元素的tuple，**末尾必须加上逗号**，否则会被当成是单个元素：

```python
# Note that a tuple of length one has to have a comma after the last element but
# tuples of other lengths, even zero, do not.
type((1))   # => <class 'int'>
type((1,))  # => <class 'tuple'>
type(())    # => <class 'tuple'>
```

tuple支持list当中绝大部分操作：

```python
# You can do most of the list operations on tuples too
len(tup)         # => 3
tup + (4, 5, 6)  # => (1, 2, 3, 4, 5, 6)
tup[:2]          # => (1, 2)
2 in tup         # => True
```

我们可以用多个变量来解压一个tuple：

```python
# You can unpack tuples (or lists) into variables
a, b, c = (1, 2, 3)  # a is now 1, b is now 2 and c is now 3
# You can also do extended unpacking
a, *b, c = (1, 2, 3, 4)  # a is now 1, b is now [2, 3] and c is now 4
# Tuples are created by default if you leave out the parentheses
d, e, f = 4, 5, 6  # tuple 4, 5, 6 is unpacked into variables d, e and f
# respectively such that d = 4, e = 5 and f = 6
# Now look how easy it is to swap two values
e, d = d, e  # d is now 5 and e is now 4
```

解释一下这行代码：

```python
a, *b, c = (1, 2, 3, 4)  # a is now 1, b is now [2, 3] and c is now 4
```

我们在b的前面加上了星号，**表示这是一个list**。所以Python会在将其他变量对应上值的情况下，将剩下的元素都赋值给b。

补充一点，tuple本身虽然是不可变的，但是**tuple当中的可变元素是可以改变的**。比如我们有这样一个tuple：

```python
a = (3, [4])
```

我们虽然不能往a当中添加或者删除元素，但是a当中含有一个list，我们可以改变这个list类型的元素，这并不会触发tuple的异常：

```python
a[1].append(0) # 这是合法的
```

## dict

dict也是Python当中经常使用的容器，它等价于C++当中的map，即**存储key和value的键值对**。我们用{}表示一个dict，用:分隔key和value。

```python
# Dictionaries store mappings from keys to values
empty_dict = {}
# Here is a prefilled dictionary
filled_dict = {"one": 1, "two": 2, "three": 3}
```

dict的key必须为不可变对象，所以**list、set和dict不可以作为另一个dict的key**，否则会抛出异常：

```python
# Note keys for dictionaries have to be immutable types. This is to ensure that
# the key can be converted to a constant hash value for quick look-ups.
# Immutable types include ints, floats, strings, tuples.
invalid_dict = {[1,2,3]: "123"}  # => Raises a TypeError: unhashable type: 'list'
valid_dict = {(1,2,3):[1,2,3]}   # Values can be of any type, however.
```

我们同样用[]查找dict当中的元素，我们传入key，获得value，等价于get方法。

```python
# Look up values with []
filled_dict["one"]  # => 1
filled_dict.get('one') #=> 1
```

我们可以call dict当中的keys和values方法，获取dict当中的所有key和value的集合，会得到一个list。在Python3.7以下版本当中，返回的结果的顺序可能和插入顺序不同，在Python3.7及以上版本中，Python会保证返回的顺序和插入顺序一致：

```python
# Get all keys as an iterable with "keys()". We need to wrap the call in list()
# to turn it into a list. We'll talk about those later.  Note - for Python
# versions <3.7, dictionary key ordering is not guaranteed. Your results might
# not match the example below exactly. However, as of Python 3.7, dictionary
# items maintain the order at which they are inserted into the dictionary.
list(filled_dict.keys())  # => ["three", "two", "one"] in Python <3.7
list(filled_dict.keys())  # => ["one", "two", "three"] in Python 3.7+


# Get all values as an iterable with "values()". Once again we need to wrap it
# in list() to get it out of the iterable. Note - Same as above regarding key
# ordering.
list(filled_dict.values())  # => [3, 2, 1]  in Python <3.7
list(filled_dict.values())  # => [1, 2, 3] in Python 3.7+
```

我们也可以用in判断一个key是否在dict当中，注意只能判断key。

```python
# Check for existence of keys in a dictionary with "in"
"one" in filled_dict  # => True
1 in filled_dict      # => False
```

如果使用[]查找不存在的key，会引发KeyError的异常。如果使用**get方法则不会引起异常，只会得到一个None**：

```python
# Looking up a non-existing key is a KeyError
filled_dict["four"]  # KeyError

# Use "get()" method to avoid the KeyError
filled_dict.get("one")      # => 1
filled_dict.get("four")     # => None
# The get method supports a default argument when the value is missing
filled_dict.get("one", 4)   # => 1
filled_dict.get("four", 4)  # => 4
```

setdefault方法可以**为不存在的key**插入一个value，如果key已经存在，则不会覆盖它：

```python
# "setdefault()" inserts into a dictionary only if the given key isn't present
filled_dict.setdefault("five", 5)  # filled_dict["five"] is set to 5
filled_dict.setdefault("five", 6)  # filled_dict["five"] is still 5
```

我们可以使用update方法用另外一个dict来更新当前dict，比如a.update(b)。对于a和b交集的key会被b覆盖，a当中不存在的key会被插入进来：

```python
# Adding to a dictionary
filled_dict.update({"four":4})  # => {"one": 1, "two": 2, "three": 3, "four": 4}
filled_dict["four"] = 4         # another way to add to dict
```

我们一样可以使用del删除dict当中的元素，同样只能传入key。

Python3.5以上的版本支持使用\*\*来解压一个dict：

```python
{'a': 1, **{'b': 2}}  # => {'a': 1, 'b': 2}
{'a': 1, **{'a': 2}}  # => {'a': 2}
```

## set

set是用来存储**不重复元素**的容器，当中的元素都是不同的，相同的元素会被删除。我们可以通过set()，或者通过{}来进行初始化。注意当我们使用{}的时候，必须要传入数据，否则Python会将它和dict弄混。

```python
# Sets store ... well sets
empty_set = set()
# Initialize a set with a bunch of values. Yeah, it looks a bit like a dict. Sorry.
some_set = {1, 1, 2, 2, 3, 4}  # some_set is now {1, 2, 3, 4}
```

set当中的元素也必须是不可变对象，因此list不能传入set。

```python
# Similar to keys of a dictionary, elements of a set have to be immutable.
invalid_set = {[1], 1}  # => Raises a TypeError: unhashable type: 'list'
valid_set = {(1,), 1}
```

可以调用add方法为set插入元素：

```python
# Add one more item to the set
filled_set = some_set
filled_set.add(5)  # filled_set is now {1, 2, 3, 4, 5}
# Sets do not have duplicate elements
filled_set.add(5)  # it remains as before {1, 2, 3, 4, 5}
```

set还可以被认为是集合，所以它还支持一些集合交叉并补的操作。

```python
# Do set intersection with &
# 计算交集
other_set = {3, 4, 5, 6}
filled_set & other_set  # => {3, 4, 5}

# Do set union with |
# 计算并集
filled_set | other_set  # => {1, 2, 3, 4, 5, 6}

# Do set difference with -
# 计算差集
{1, 2, 3, 4} - {2, 3, 5}  # => {1, 4}

# Do set symmetric difference with ^
# 这个有点特殊，计算对称集，也就是去掉重复元素剩下的内容
{1, 2, 3, 4} ^ {2, 3, 5}  # => {1, 4, 5}
```

set还支持**超集和子集的判断**，我们可以用大于等于和小于等于号判断一个set是不是另一个的超集或子集：

```python
# Check if set on the left is a superset of set on the right
{1, 2} >= {1, 2, 3} # => False

# Check if set on the left is a subset of set on the right
{1, 2} <= {1, 2, 3} # => True
```

和dict一样，我们可以使用in判断元素在不在set当中。用copy可以拷贝一个set。

```python
# Check for existence in a set with in
2 in filled_set   # => True
10 in filled_set  # => False

# Make a one layer deep copy
filled_set = some_set.copy()  # filled_set is {1, 2, 3, 4, 5}
filled_set is some_set        # => False
```

# 控制流和迭代

## 判断语句

Python当中的判断语句非常简单，并且Python不支持switch，所以即使是多个条件，我们也只能**罗列if-else**。

```python
# Let's just make a variable
some_var = 5

# Here is an if statement. Indentation is significant in Python!
# Convention is to use four spaces, not tabs.
# This prints "some_var is smaller than 10"
if some_var > 10:
    print("some_var is totally bigger than 10.")
elif some_var < 10:    # This elif clause is optional.
    print("some_var is smaller than 10.")
else:                  # This is optional too.
    print("some_var is indeed 10.")
```

## 循环

我们可以用in来循环迭代一个list当中的内容，这也是Python当中基本的循环方式。

```python

"""
For loops iterate over lists
prints:
    dog is a mammal
    cat is a mammal
    mouse is a mammal
"""
for animal in ["dog", "cat", "mouse"]:
    # You can use format() to interpolate formatted strings
    print("{} is a mammal".format(animal))
```

如果我们要循环一个范围，可以使用range。range加上一个参数表示从0开始的序列，比如range(10)，表示[0, 10)区间内的所有整数：

```python
"""
"range(number)" returns an iterable of numbers
from zero to the given number
prints:
    0
    1
    2
    3
"""
for i in range(4):
    print(i)
```

如果我们传入两个参数，则**代表迭代区间的首尾**。

```python
"""
"range(lower, upper)" returns an iterable of numbers
from the lower number to the upper number
prints:
    4
    5
    6
    7
"""
for i in range(4, 8):
    print(i)
```

如果我们传入第三个元素，表示每次**循环变量自增的步长**。

```python
"""
"range(lower, upper, step)" returns an iterable of numbers
from the lower number to the upper number, while incrementing
by step. If step is not indicated, the default value is 1.
prints:
    4
    6
"""
for i in range(4, 8, 2):
    print(i)
```

如果使用enumerate函数，可以**同时迭代一个list的下标和元素**：

```python
"""
To loop over a list, and retrieve both the index and the value of each item in the list
prints:
    0 dog
    1 cat
    2 mouse
"""
animals = ["dog", "cat", "mouse"]
for i, value in enumerate(animals):
    print(i, value)
```


while循环和C++类似，当条件为True时执行，为false时退出。并且判断条件不需要加上括号：

```python
"""
While loops go until a condition is no longer met.
prints:
    0
    1
    2
    3
"""
x = 0
while x < 4:
    print(x)
    x += 1  # Shorthand for x = x + 1
```

## 捕获异常

Python当中使用**try和except捕获异常**，我们可以在except后面限制异常的类型。如果有多个类型可以写多个except，还可以使用else语句表示其他所有的类型。finally语句内的语法**无论是否会触发异常都必定执行**：

```python
# Handle exceptions with a try/except block
try:
    # Use "raise" to raise an error
    raise IndexError("This is an index error")
except IndexError as e:
    pass                 # Pass is just a no-op. Usually you would do recovery here.
except (TypeError, NameError):
    pass                 # Multiple exceptions can be handled together, if required.
else:                    # Optional clause to the try/except block. Must follow all except blocks
    print("All good!")   # Runs only if the code in try raises no exceptions
finally:                 #  Execute under all circumstances
    print("We can clean up resources here")
```

## with操作

在Python当中我们经常会使用资源，最常见的就是open打开一个文件。我们**打开了文件句柄就一定要关闭**，但是如果我们手动来编码，经常会忘记执行close操作。并且如果文件异常，还会触发异常。这个时候我们可以使用with语句来代替这部分处理，使用with会**自动在with块执行结束或者是触发异常时关闭打开的资源**。


以下是with的几种用法和功能：

```python
# Instead of try/finally to cleanup resources you can use a with statement
# 代替使用try/finally语句来关闭资源
with open("myfile.txt") as f:
    for line in f:
        print(line)

# Writing to a file
# 使用with写入文件
contents = {"aa": 12, "bb": 21}
with open("myfile1.txt", "w+") as file:
    file.write(str(contents))        # writes a string to a file

with open("myfile2.txt", "w+") as file:
    file.write(json.dumps(contents)) # writes an object to a file

# Reading from a file
# 使用with读取文件
with open('myfile1.txt', "r+") as file:
    contents = file.read()           # reads a string from a file
print(contents)
# print: {"aa": 12, "bb": 21}

with open('myfile2.txt', "r+") as file:
    contents = json.load(file)       # reads a json object from a file
print(contents)     
# print: {"aa": 12, "bb": 21}
```

## 可迭代对象

凡是可以使用in语句来迭代的对象都叫做**可迭代对象**，它和迭代器不是一个含义。这里只有可迭代对象的介绍，想要了解迭代器的具体内容，请移步传送门：

[Python——五分钟带你弄懂迭代器与生成器，夯实代码能力](https://mp.weixin.qq.com/s?__biz=MzUyMTM5OTM2NA==&mid=2247484777&idx=1&sn=236b215aff90bf4cccb56b8385eece18&chksm=f9daf842cead7154ad10951904789ff549611e86df4fefae71653c7c96be627bee447dfc7f89&scene=21#wechat_redirect)

当我们调用dict当中的keys方法的时候，返回的结果就是一个可迭代对象。

```python
# Python offers a fundamental abstraction called the Iterable.
# An iterable is an object that can be treated as a sequence.
# The object returned by the range function, is an iterable.

filled_dict = {"one": 1, "two": 2, "three": 3}
our_iterable = filled_dict.keys()
print(our_iterable)  # => dict_keys(['one', 'two', 'three']). This is an object that implements our Iterable interface.

# We can loop over it.
for i in our_iterable:
    print(i)  # Prints one, two, three
```

我们**不能使用下标来访问**可迭代对象，但我们可以用iter将它转化成迭代器，使用next关键字来获取下一个元素。也可以将它转化成list类型，变成一个list。

```python
# However we cannot address elements by index.
our_iterable[1]  # Raises a TypeError

# An iterable is an object that knows how to create an iterator.
our_iterator = iter(our_iterable)

# Our iterator is an object that can remember the state as we traverse through it.
# We get the next object with "next()".
next(our_iterator)  # => "one"

# It maintains state as we iterate.
next(our_iterator)  # => "two"
next(our_iterator)  # => "three"

# After the iterator has returned all of its data, it raises a StopIteration exception
next(our_iterator)  # Raises StopIteration

# We can also loop over it, in fact, "for" does this implicitly!
our_iterator = iter(our_iterable)
for i in our_iterator:
    print(i)  # Prints one, two, three

# You can grab all the elements of an iterable or iterator by calling list() on it.
list(our_iterable)  # => Returns ["one", "two", "three"]
list(our_iterator)  # => Returns [] because state is saved
```

## 函数

使用def关键字来定义函数，我们在传参的时候如果指定函数内的参数名，**可以不按照函数定义的顺序**传参：

```python
# Use "def" to create new functions
def add(x, y):
    print("x is {} and y is {}".format(x, y))
    return x + y  # Return values with a return statement

# Calling functions with parameters
add(5, 6)  # => prints out "x is 5 and y is 6" and returns 11

# Another way to call functions is with keyword arguments
add(y=6, x=5)  # Keyword arguments can arrive in any order.
```

可以在参数名之前加上\*表示任意长度的参数，参数会被转化成list：

```python
# You can define functions that take a variable number of
# positional arguments
def varargs(*args):
    return args

varargs(1, 2, 3)  # => (1, 2, 3)
```

也可以指定任意长度的关键字参数，在参数前加上\*\*表示接受一个dict：

```python
# You can define functions that take a variable number of
# keyword arguments, as well
def keyword_args(**kwargs):
    return kwargs

# Let's call it to see what happens
keyword_args(big="foot", loch="ness")  # => {"big": "foot", "loch": "ness"}
```

当然我们也可以两个都用上，这样可以接受任何参数：

```python
# You can do both at once, if you like
def all_the_args(*args, **kwargs):
    print(args)
    print(kwargs)
"""
all_the_args(1, 2, a=3, b=4) prints:
    (1, 2)
    {"a": 3, "b": 4}
"""
```

传入参数的时候我们也可以使用\*和\*\*来解压list或者是dict：

```python
# When calling functions, you can do the opposite of args/kwargs!
# Use * to expand tuples and use ** to expand kwargs.
args = (1, 2, 3, 4)
kwargs = {"a": 3, "b": 4}
all_the_args(*args)            # equivalent to all_the_args(1, 2, 3, 4)
all_the_args(**kwargs)         # equivalent to all_the_args(a=3, b=4)
all_the_args(*args, **kwargs)  # equivalent to all_the_args(1, 2, 3, 4, a=3, b=4)
```

Python中的参数**可以返回多个值**：

```python
# Returning multiple values (with tuple assignments)
def swap(x, y):
    return y, x  # Return multiple values as a tuple without the parenthesis.
                 # (Note: parenthesis have been excluded but can be included)

x = 1
y = 2
x, y = swap(x, y)     # => x = 2, y = 1
# (x, y) = swap(x,y)  # Again parenthesis have been excluded but can be included.
```

函数内部定义的变量即使和全局变量重名，也**不会覆盖全局变量的值**。想要在函数内部使用全局变量，需要加上**global**关键字，表示这是一个全局变量：

```python
# Function Scope
x = 5

def set_x(num):
    # Local var x not the same as global variable x
    x = num    # => 43
    print(x)   # => 43

def set_global_x(num):
    global x
    print(x)   # => 5
    x = num    # global var x is now set to 6
    print(x)   # => 6

set_x(43)
set_global_x(6)
```

Python支持**函数式编程**，我们可以在一个函数内部返回一个函数：

```python
# Python has first class functions
def create_adder(x):
    def adder(y):
        return x + y
    return adder

add_10 = create_adder(10)
add_10(3)   # => 13
```

Python中可以使用lambda表示**匿名函数**，使用:作为分隔，:前面表示匿名函数的参数，:后面的是函数的返回值：

```python
# There are also anonymous functions
(lambda x: x > 2)(3)                  # => True
(lambda x, y: x ** 2 + y ** 2)(2, 1)  # => 5
```

我们还可以将函数作为参数使用map和filter，实现元素的批量处理和过滤。关于Python中map、reduce和filter的使用，具体可以查看之前的文章：

[五分钟带你了解map、reduce和filter](https://mp.weixin.qq.com/s?__biz=MzUyMTM5OTM2NA==&mid=2247484825&idx=1&sn=a0122d252e0ac828dcffda4e7ea78f7c&chksm=f9daf8b2cead71a4ad5501b073302910c4111e0af5dc996a23b0ea5cf8e13424f4f1449446ea&scene=21#wechat_redirect)

```python
# There are built-in higher order functions
list(map(add_10, [1, 2, 3]))          # => [11, 12, 13]
list(map(max, [1, 2, 3], [4, 2, 1]))  # => [4, 2, 3]

list(filter(lambda x: x > 5, [3, 4, 5, 6, 7]))  # => [6, 7]
```

我们还可以结合循环和判断语来给list或者是dict进行初始化：

```python
# We can use list comprehensions for nice maps and filters
# List comprehension stores the output as a list which can itself be a nested list
[add_10(i) for i in [1, 2, 3]]         # => [11, 12, 13]
[x for x in [3, 4, 5, 6, 7] if x > 5]  # => [6, 7]

# You can construct set and dict comprehensions as well.
{x for x in 'abcddeef' if x not in 'abc'}  # => {'d', 'e', 'f'}
{x: x**2 for x in range(5)}  # => {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

# 模块

使用**import语句引入一个Python模块**，我们可以用.来访问模块中的函数或者是类。

```python
# You can import modules
import math
print(math.sqrt(16))  # => 4.0
```

我们也可以使用from import的语句，单独引入模块内的函数或者是类，而不再需要写出完整路径。使用from import \*可以引入模块内所有内容（不推荐这么干）

```python
# You can get specific functions from a module
from math import ceil, floor
print(ceil(3.7))   # => 4.0
print(floor(3.7))  # => 3.0

# You can import all functions from a module.
# Warning: this is not recommended
from math import *
```

可以使用as给模块内的方法或者类起别名：

```python
# You can shorten module names
import math as m
math.sqrt(16) == m.sqrt(16)  # => True
```

我们可以使用dir查看我们用的模块的路径：

```python
# You can find out which functions and attributes
# are defined in a module.
import math
dir(math)
```

这么做的原因是如果我们当前的路径下也有一个叫做math的Python文件，那么**会覆盖系统自带的math的模块**。这是尤其需要注意的，不小心会导致很多奇怪的bug。

# 类

我们来看一个完整的类，相关的介绍都在注释当中

```python
# We use the "class" statement to create a class
class Human:

    # A class attribute. It is shared by all instances of this class
    # 类属性，可以直接通过Human.species调用，而不需要通过实例
    species = "H. sapiens"

    # Basic initializer, this is called when this class is instantiated.
    # Note that the double leading and trailing underscores denote objects
    # or attributes that are used by Python but that live in user-controlled
    # namespaces. Methods(or objects or attributes) like: __init__, __str__,
    # __repr__ etc. are called special methods (or sometimes called dunder methods)
    # You should not invent such names on your own.
    # 最基础的构造函数
    # 加了下划线的函数和变量表示不应该被用户使用，其中双下划线的函数或者是变量将不会被子类覆盖
    # 前后都有双下划线的函数和属性是类当中的特殊属性
    def __init__(self, name):
        # Assign the argument to the instance's name attribute
        self.name = name

        # Initialize property
        self._age = 0

    # An instance method. All methods take "self" as the first argument
    # 类中的函数，所有实例可以调用，第一个参数必须是self
    # self表示实例的引用
    def say(self, msg):
        print("{name}: {message}".format(name=self.name, message=msg))

    # Another instance method
    def sing(self):
        return 'yo... yo... microphone check... one two... one two...'

    # A class method is shared among all instances
    # They are called with the calling class as the first argument
    @classmethod
    # 加上了注解，表示是类函数
    # 通过Human.get_species来调用，所有实例共享
    def get_species(cls):
        return cls.species

    # A static method is called without a class or instance reference
    @staticmethod
    # 静态函数，通过类名或者是实例都可以调用
    def grunt():
        return "*grunt*"

    # A property is just like a getter.
    # It turns the method age() into an read-only attribute of the same name.
    # There's no need to write trivial getters and setters in Python, though.
    @property
    # property注解，类似于get，set方法
    # 效率很低，除非必要，不要使用
    def age(self):
        return self._age

    # This allows the property to be set
    @age.setter
    def age(self, age):
        self._age = age

    # This allows the property to be deleted
    @age.deleter
    def age(self):
        del self._age
```

以上内容的详细介绍之前也有过相关文章，可以查看：

[Python——__slots__，property和对象命名规范](https://mp.weixin.qq.com/s?__biz=MzUyMTM5OTM2NA==&mid=2247485110&idx=1&sn=3006f006e33e325ced917ca1dc071497&chksm=f9dafb9dcead728b11e27204da77430098aa7a5aea34f7a9d43e1e8589a9f1255814a1573bda&scene=21#wechat_redirect)

下面我们来看看Python当中类的使用：

```python
# When a Python interpreter reads a source file it executes all its code.
# This __name__ check makes sure this code block is only executed when this
# module is the main program.
# 这个是main函数也是整个程序入口的惯用写法
if __name__ == '__main__':
    # Instantiate a class
    # 实例化一个类，获取类的对象
    i = Human(name="Ian")
    # 执行say方法
    i.say("hi")                     # "Ian: hi"
    j = Human("Joel")
    j.say("hello")                  # "Joel: hello"
    # i和j都是Human的实例，都称作是Human类的对象
    # i and j are instances of type Human, or in other words: they are Human objects

    # Call our class method
    # 类属性被所有实例共享，一旦修改全部生效
    i.say(i.get_species())          # "Ian: H. sapiens"
    # Change the shared attribute
    Human.species = "H. neanderthalensis"
    i.say(i.get_species())          # => "Ian: H. neanderthalensis"
    j.say(j.get_species())          # => "Joel: H. neanderthalensis"

    # 通过类名调用静态方法
    # Call the static method
    print(Human.grunt())            # => "*grunt*"

    # Cannot call static method with instance of object 
    # because i.grunt() will automatically put "self" (the object i) as an argument
    # 不能通过对象调用静态方法，因为对象会传入self实例，会导致不匹配
    print(i.grunt())                # => TypeError: grunt() takes 0 positional arguments but 1 was given

    # Update the property for this instance
    # 实例级别的属性是独立的，各个对象各自拥有，修改不会影响其他对象内的值
    i.age = 42
    # Get the property
    i.say(i.age)                    # => "Ian: 42"
    j.say(j.age)                    # => "Joel: 0"
    # Delete the property
    del i.age
    # i.age                         # => this would raise an AttributeError
```

这里解释一下，实例和对象可以理解成一个概念，实例的英文是instance，对象的英文是object。都是指类经过实例化之后得到的对象。

# 继承

继承可以让子类**继承父类的变量以及方法**，并且我们还可以在子类当中指定一些属于自己的特性，并且还可以重写父类的一些方法。一般我们会将不同的类放在不同的文件当中，使用import引入，一样可以实现继承。

```python
from human import Human


# Specify the parent class(es) as parameters to the class definition
class Superhero(Human):

    # If the child class should inherit all of the parent's definitions without
    # any modifications, you can just use the "pass" keyword (and nothing else)
    # but in this case it is commented out to allow for a unique child class:
    # pass
    # 如果要完全继承父类的所有的实现，我们可以使用关键字pass，表示跳过。这样不会修改父类当中的实现

    # Child classes can override their parents' attributes
    species = 'Superhuman'

    # Children automatically inherit their parent class's constructor including
    # its arguments, but can also define additional arguments or definitions
    # and override its methods such as the class constructor.
    # This constructor inherits the "name" argument from the "Human" class and
    # adds the "superpower" and "movie" arguments:
    # 子类会完全继承父类的构造方法，我们也可以进行改造，比如额外增加一些参数
    def __init__(self, name, movie=False,
                 superpowers=["super strength", "bulletproofing"]):

        # add additional class attributes:
        # 额外新增的参数
        self.fictional = True
        self.movie = movie
        # be aware of mutable default values, since defaults are shared
        self.superpowers = superpowers

        # The "super" function lets you access the parent class's methods
        # that are overridden by the child, in this case, the __init__ method.
        # This calls the parent class constructor:
        # 子类可以通过super关键字调用父类的方法
        super().__init__(name)

    # override the sing method
    # 重写父类的sing方法
    def sing(self):
        return 'Dun, dun, DUN!'

    # add an additional instance method
    # 新增方法，只属于子类
    def boast(self):
        for power in self.superpowers:
            print("I wield the power of {pow}!".format(pow=power))
```

```python
if __name__ == '__main__':
    sup = Superhero(name="Tick")

    # Instance type checks
    # 检查继承关系
    if isinstance(sup, Human):
        print('I am human')
    # 检查类型
    if type(sup) is Superhero:
        print('I am a superhero')

    # Get the Method Resolution search Order used by both getattr() and super()
    # This attribute is dynamic and can be updated
    # 查看方法查询的顺序
    # 先是自身，然后沿着继承顺序往上，最后到object
    print(Superhero.__mro__)    # => (<class '__main__.Superhero'>,
                                # => <class 'human.Human'>, <class 'object'>)

    # 相同的属性子类覆盖了父类
    # Calls parent method but uses its own class attribute
    print(sup.get_species())    # => Superhuman

    # Calls overridden method
    # 相同的方法也覆盖了父类
    print(sup.sing())           # => Dun, dun, DUN!

    # Calls method from Human
    # 继承了父类的方法
    sup.say('Spoon')            # => Tick: Spoon

    # Call method that exists only in Superhero
    # 子类特有的方法
    sup.boast()                 # => I wield the power of super strength!
                                # => I wield the power of bulletproofing!

    # Inherited class attribute
    sup.age = 31
    print(sup.age)              # => 31

    # Attribute that only exists within Superhero
    print('Am I Oscar eligible? ' + str(sup.movie))
```

## 多继承

我们创建一个蝙蝠类：

```python

# Another class definition
# bat.py
class Bat:

    species = 'Baty'

    def __init__(self, can_fly=True):
        self.fly = can_fly

    # This class also has a say method
    def say(self, msg):
        msg = '... ... ...'
        return msg

    # And its own method as well
    # 蝙蝠独有的声呐方法
    def sonar(self):
        return '))) ... ((('

if __name__ == '__main__':
    b = Bat()
    print(b.say('hello'))
    print(b.fly)
```

我们再创建一个蝙蝠侠的类，同时继承Superhero和Bat：

```python
# And yet another class definition that inherits from Superhero and Bat
# superhero.py
from superhero import Superhero
from bat import Bat

# Define Batman as a child that inherits from both Superhero and Bat
class Batman(Superhero, Bat):

    def __init__(self, *args, **kwargs):
        # Typically to inherit attributes you have to call super:
        # super(Batman, self).__init__(*args, **kwargs)      
        # However we are dealing with multiple inheritance here, and super()
        # only works with the next base class in the MRO list.
        # So instead we explicitly call __init__ for all ancestors.
        # The use of *args and **kwargs allows for a clean way to pass arguments,
        # with each parent "peeling a layer of the onion".
        # 通过类名调用两个父类各自的构造方法
        Superhero.__init__(self, 'anonymous', movie=True, 
                           superpowers=['Wealthy'], *args, **kwargs)
        Bat.__init__(self, *args, can_fly=False, **kwargs)
        # override the value for the name attribute
        self.name = 'Sad Affleck'

    # 重写父类的sing方法
    def sing(self):
        return 'nan nan nan nan nan batman!'
```

执行这个类：

```python
if __name__ == '__main__':
    sup = Batman()

    # Get the Method Resolution search Order used by both getattr() and super().
    # This attribute is dynamic and can be updated
    # 可以看到方法查询的顺序是先沿着superhero这条线到human，然后才是bat
    print(Batman.__mro__)       # => (<class '__main__.Batman'>, 
                                # => <class 'superhero.Superhero'>, 
                                # => <class 'human.Human'>, 
                                # => <class 'bat.Bat'>, <class 'object'>)

    # Calls parent method but uses its own class attribute
    # 只有superhero有get_species方法
    print(sup.get_species())    # => Superhuman

    # Calls overridden method
    print(sup.sing())           # => nan nan nan nan nan batman!

    # Calls method from Human, because inheritance order matters
    sup.say('I agree')          # => Sad Affleck: I agree

    # Call method that exists only in 2nd ancestor
    # 调用蝙蝠类的声呐方法
    print(sup.sonar())          # => ))) ... (((

    # Inherited class attribute
    sup.age = 100
    print(sup.age)              # => 100

    # Inherited attribute from 2nd ancestor whose default value was overridden.
    print('Can I fly? ' + str(sup.fly)) # => Can I fly? False
```

# 进阶

## 生成器

我们可以通过yield关键字创建一个生成器，每次我们调用的时候执行到yield关键字处则停止。下次再次调用则还是从yield处开始往下执行：

```python
# Generators help you make lazy code.
def double_numbers(iterable):
    for i in iterable:
        yield i + i

# Generators are memory-efficient because they only load the data needed to
# process the next value in the iterable. This allows them to perform
# operations on otherwise prohibitively large value ranges.
# NOTE: `range` replaces `xrange` in Python 3.
for i in double_numbers(range(1, 900000000)):  # `range` is a generator.
    print(i)
    if i >= 30:
        break
```

除了yield之外，我们还可以使用()小括号来生成一个生成器：

```python
# Just as you can create a list comprehension, you can create generator
# comprehensions as well.
values = (-x for x in [1,2,3,4,5])
for x in values:
    print(x)  # prints -1 -2 -3 -4 -5 to console/terminal

# You can also cast a generator comprehension directly to a list.
values = (-x for x in [1,2,3,4,5])
gen_to_list = list(values)
print(gen_to_list)  # => [-1, -2, -3, -4, -5]
```

关于生成器和迭代器更多的内容，可以查看下面这篇文章：

[五分钟带你弄懂迭代器与生成器，夯实代码能力](https://mp.weixin.qq.com/s?__biz=MzUyMTM5OTM2NA==&mid=2247484777&idx=1&sn=236b215aff90bf4cccb56b8385eece18&chksm=f9daf842cead7154ad10951904789ff549611e86df4fefae71653c7c96be627bee447dfc7f89&scene=21#wechat_redirect)

## 装饰器

我们引入functools当中的wraps之后，可以创建一个装饰器。装饰器可以在不修改函数内部代码的前提下，在外面包装一层其他的逻辑:

```python
# Decorators
# In this example `beg` wraps `say`. If say_please is True then it
# will change the returned message.
from functools import wraps


def beg(target_function):
    @wraps(target_function)
    # 如果please为True，额外输出一句Please! I am poor :(
    def wrapper(*args, **kwargs):
        msg, say_please = target_function(*args, **kwargs)
        if say_please:
            return "{} {}".format(msg, "Please! I am poor :(")
        return msg

    return wrapper


@beg
def say(say_please=False):
    msg = "Can you buy me a beer?"
    return msg, say_please


print(say())                 # Can you buy me a beer?
print(say(say_please=True))  # Can you buy me a beer? Please! I am poor :(
```

装饰器之前也有专门的文章详细介绍，可以移步下面的传送门：

[一文搞定Python装饰器，看完面试不再慌](https://mp.weixin.qq.com/s?__biz=MzUyMTM5OTM2NA==&mid=2247485152&idx=1&sn=1386f218b0d5674547599cf00befd93d&chksm=f9dafbcbcead72dd4329068c609c2becc9326be8d3f4b53f0193185747e778f6dbc805601fbe&scene=21#wechat_redirect)


## 结尾

不知道有多少小伙伴可以看到结束，原作者的确非常厉害，把Python的基本操作基本上都囊括在里面了。如果都能读懂并且理解的话，那么Python这门语言就算是入门了。

原作者写的是一个**Python文件**，所有的内容都在Python的注释当中。我在它的基础上做了修补和额外的描述。如果想要获得原文，可以点击查看原文，或者是在公众号内回复**learnpython**获取。

如果你之前就有其他语言的语言基础，我想本文读完应该不用30分钟。当然在30分钟内学会一门语言是不可能的，也不是我所提倡的。但至少通过本文我们可以做到熟悉Python的语法，知道大概有哪些操作，剩下的就要我们亲自去写代码的时候去体会和运用了。

根据我的经验，在学习一门新语言的前期，不停地查阅资料是免不了的。希望本文可以作为你在使用Python时候的查阅文档。

今天的文章就到这里，原创不易，需要你的**一个关注**，你的举手之劳对我来说很重要。

![IMAGE](resources/B7AE373ACC841036368D0E2FA480D876.jpg)