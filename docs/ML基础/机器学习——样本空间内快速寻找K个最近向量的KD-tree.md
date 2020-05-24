https://www.joinquant.com/view/community/detail/c2c41c79657cebf8cd871b44ce4f5d97
https://blog.csdn.net/v_july_v/article/details/8203674

今天是机器学习的第15篇文章，之前的文章当中讲了Kmeans的相关优化，还讲了大名鼎鼎的EM算法。有些小伙伴表示喜欢看这些硬核的，于是今天上点硬菜，我们来看一个机器学习领域经常用到的数据结构——KD-Tree。

## 从线段树到KD树

在讲KD树之前，我们先来了解一下线段树的概念。线段树在机器学习领域当中不太常见，作为高性能维护的数据结构，经常出现在各种算法比赛当中。线段树的本质是一棵维护一段区间的平衡二叉树。

比如下图就是一个经典的线段树：

![IMAGE](resources/C0C0469CEE0A47E506F07B730A4DCB42.jpg =706x535)

从下图当中我们不难看出来，这棵线段树维护的是一个区间内的最大值。比如树根是8，维护的是整个区间的最大值，每一个中间节点的值都是以它为树根的子树中所有元素的最大值。

通过线段树，我们可以在$O(logN)$的时间内计算出某一个连续区间的最大值。比如我们来看下图：

![IMAGE](resources/714225684370FAFA6BDD1673C066698F.jpg =961x547)

当我们要求被框起来的元素中的最大值，我们只需要找到能够覆盖这个区间的中间节点就行。我们可以发现被红框框起来的两个节点的子树刚好覆盖这个区间，于是整个区间的最大值，就是这两个元素的最大值。这样，我们就把一个需要$O(n)$查找的问题降低成了$log(N)$，不但如此，我们也可以做到$O(logN)$复杂度内的更新，也就是说我们不但可以快速查询，还可以更新线段当中的元素。

当然线段树的应用非常广泛，也有许多种变体，这里我们不过多深入，感兴趣的同学可以期待一下周三的算法与数据结构专题，在之后的文章当中会为大家分享线段树的相关内容。在这里，我们只需要有一个大概的印象，线段树究竟完成的是什么样的事情即可。

线段树维护的是一个线段，也就是区间内的元素，也就是说维护的是一个一维的序列。如果我们将数据的维度扩充一下，扩充到多维呢？

是的，你没有猜错，从某种程度上来说，我们可以把KD-Tree看成是线段树拓展到多维空间当中的情况。

## KD-Tree定义

我们来看一下KD-Tree的具体定义，这里的K指的是K维空间，D自然就是dimension，也就是维度，也就是说KD-Tree就是K维度树的意思。

在我们构建线段树的时候，其实是一个递归的建树过程，我们每次把当前的线段一分为二，然后用分成两半的数据分别构建左右子树。我们可以简单写一下伪代码，来更直观地感受一下：

```python
class Node:
    def __init__(self, value, lchild, rchild):
        self.value = value
        self.lchild = lchild
        self.rchild = rchild   
        
def build(arr):
    n = len(arr):
    left, right = arr[: n//2], arr[n//2:]
    lchild, rchild = self.build(left), self.build(right)
    return Node(max(lchild.value, rchild.value), lchild, rchild)
```

我们来看一个二维的例子，在一个二维的平面当中分布着若干个点。

![IMAGE](resources/4A18CF4CAEC48DF6D89BAA0E0BE907CF.jpg =1249x833)

我们首先选择一个维度将这些数据一分为二，比如我们选择x轴。我们对所有数据按照x轴的值排序，选出其中的中点进行一分为二。

![IMAGE](resources/0A77CBA94FE18CA35388B335B11A333C.jpg =1123x800)

在这根线左右两侧的点被分成了两棵子树，对于这两个部分的数据来说，我们更换一个维度，也就是选择y轴进行划分。一样，我们先排序，然后找到中间的点，再次一分为二。我们可以得到：

![IMAGE](resources/7AAF0A620C4841AC4F23F8D7D987C79D.jpg =1092x765)

我们重复上述过程，一直将点分到不能分为止，为了能更好地看清楚，我们对所有数据标上坐标（并不精确）。

![IMAGE](resources/AF9C33E8342DFA468CE79651311317DF.jpg =1119x779)

如果我们把空间看成是广义的区间，那么它和线段树的原理是一样的。最后得到的也是一棵完美二叉树，因为我们每次都选择了数据集的中点进行划分，可以保证从树根到叶子节点的长度不会超过$logN$。

我们代入上面的坐标之后，我们最终得到的KD-Tree大概是下面这个样子：

![IMAGE](resources/51AE478DA9D85155097A12BCB23BFCFF.jpg =861x459)

## KD-Tree 建树

在建树的过程当中，我们的树深每往下延伸一层，我们就会换一个维度作为衡量标准。原因也很简单，因为我们希望这棵树对于这K维空间都有很好的表达能力，方便我们根据不同的维度快速查询。

在一些实现当中，我们会计算每一个维度的方差，然后选择方差较大的维度进行切分。这样做自然是因为方差较大的维度说明数据相对分散，切分之后可以把数据区分得更加明显。但我个人觉得这样做意义不是很大，毕竟计算方差也是一笔开销。所以这里我们选择了最朴素的方法——轮流选择。

也就是说我们从树根开始，选择第0维作为排序和切分数据的依据，然后到了树深为1的这一层，我们选择第一维，树深为2的这一层，我们选择第二维，以此类推。当树深超过了K的时候，我们就对树深取模。

明确了这一点之后，我们就可以来写KD-Tree的建树代码了，和上面二叉树的代码非常相似，只不过多了维度的处理而已。

```python
    # 外部暴露接口
    def build_model(self, dataset):
        self.root = self._build_model(dataset)
        # 先忽略，容后再讲
        self.set_father(self.root, None)

    # 内部实现的接口
    def _build_model(self, dataset, depth=0):
        if len(dataset) == 0:
            return None

        # 通过树深对K取模来获得当前对哪一维切分
        axis = depth % self.K
        m = len(dataset) // 2
        # 根据axis这一维排序
        dataset = sorted(dataset, key=lambda x: x[axis])
        # 将数据一分为二
        left, mid, right = dataset[:m], dataset[m], dataset[m+1:]

        # 递归建树
        return KDTree.Node(
            mid[axis],
            mid,
            axis,
            depth,
            len(dataset),
            self._build_model(left, depth+1),
            self._build_model(right, depth+1)
        )
```

这样我们就建好了树，但是在后序的查询当中我们需要访问节点的父节点，所以我们需要为每一个节点都赋值指向父亲节点的指针。这个值我们可以写在建树的代码里，但是会稍稍复杂一些，所以我把它单独拆分了出来，作为一个独立的函数来给每一个节点赋值。对于根节点来说，由于它没有父亲节点，所以赋值为None。

我们来看下set_father当中的内容，其实很简单，就是一个树的递归遍历：

```python
def set_father(self, node, father):
    if node is None:
        return
    # 赋值
    node.father = father
    # 递归左右
    self.set_father(node.lchild, node)
    self.set_father(node.rchild, node)
```

## 快速批量查询

KD-Tree建树建好了肯定是要来用的，它最大的用处是可以在单次查询中获得距离样本最近的若干个样本。在稳定的数据集当中，我们可以在$log(n)$的时间内完成查询，但是对于特殊情况可能会长一些，但是也比我们通过朴素的方法查询要快得多。

所以我们很容易想明白，KD-Tree一个广泛的使用场景是用来优化KNN算法。我们在之前介绍KNN算法的文章当中曾经提到过，KNN算法在预测的时候需要遍历整个数据集，然后计算数据集中每一个样本与当前样本的距离，选出最近的K个来，这需要大量的开销。而使用KD-Tree，我们可以在一次查询当中直接查找到K个最近的样本，因此大大提升KNN算法的效率。

那么，这个查询操作又是怎么实现的呢？

这个查询基于递归实现，因此对于递归不熟悉的小伙伴，可能初看会比较困难，可以先阅读一下之前关于递归的文章。

首先我们先通过递归查找到KD-Tree上的叶子节点，这个查找应该非常容易，本质上来说我们就是将当前样本不停地与分割线进行比较，看看是在分割线的左侧还是右侧。和二叉搜索树的元素查找是一样的：

```python
    def iter_down(self, node, data):
        # 如果是叶子节点，则返回
        if node.lchild is None and node.rchild is None:
            return node
        # 如果左节点为空，则递归右节点
        if node.lchild is None:
            return self.iter_down(node.rchild, data)
        # 同理，递归左节点
        if node.rchild is None:
            return self.iter_down(node.rchild, data)
        # 都不为空则和分割线判断是左还是右
        axis = node.axis
        next_node = node.lchild if data[axis] <= node.boundray else node.rchild
        return self.iter_down(next_node, data)
```

我们找到了叶子节点，其实代表样本空间当中的一小块空间。

我们来实际走一下整个流程，假设我们要查找3个点。首先，我们会创建一个候选集，用来存储答案。当我们找到叶子节点之后，这个区域当中只有一个点，我们把它加入候选集。

![IMAGE](resources/EB594663CA7227F25A2A73FF443D4F30.jpg =1263x882)

在上图当中紫色的x代表我们查找的样本，我们查找到的叶子节点之后，在两种情况下我们会把当前点加入候选集。第一种情况是候选集还有空余，也就是还没有满K个，这里的K是我们查询的数量，也就是3。第二种情况是当前点到样本的距离小于候选集中的一个，那么我们需要更新候选集。

这个点被我们访问过之后，我们会打上标记，表示这个点已经访问过了。这个时候我们需要判断，整棵树当中的搜索是否已经结束，如果当前节点已经是根节点了，说明我们的遍历结束了，那么返回候选集，否则说明还没有，我们需要继续搜索。上图当中我们用绿色表示在候选集当中，黄色表示已经访问过。

由于我们的搜索还没有结束，所以需要继续搜索。继续搜索需要判断样本和当前分割线的距离来判断和分割线的另一侧有没有可能存在答案。由于叶子节点没有另一侧，所以作罢，我们往上移动一个，跳转到它的父亲节点。

![IMAGE](resources/064C8A39ADC69FE7DDAF50ABAFD7AB7C.jpg =1241x901)

我们计算距离并且查看候选集，此时候选集未满，我们加入候选集，标记为已经访问过。它虽然存在分割线，但是也没有另一侧的节点，所以跳过。

我们再往上，遍历到它的父亲节点，我们执行同样的判断，发现此时候选集还有空余，于是将它继续加入答案：

![IMAGE](resources/87C1BB9297636683E8A95319272CEA05.jpg =1261x886)

但是当我们判断到分割线距离的时候，我们发现这一次样本到分割线的举例要比之前候选集当中的最大距离要大，所以分割线的另一侧很有可能存在答案：

![IMAGE](resources/D72A48C2101A5DEB421946FF3647B3C7.jpg =1324x922)

这里的d1是样本到分割线的距离，d2是样本到候选集当中最远点的距离。由于到分割线更近，所以分割线的另一侧很有可能也存在答案，这个时候我们需要搜索分割线另一侧的子树，一直搜索到叶子节点。

![IMAGE](resources/051177E047893019012F75DDE5EB43D8.jpg =1306x913)

我们找到了叶子节点，计算距离，发现此时候选集已经满了，并且它的距离大于候选集当中任何一个答案，所以不能构成新的答案。于是我们只是标记它已经访问过，并不会加入候选集。同样，我们继续往上遍历，到它的父节点：

![IMAGE](resources/82C4A0EC87D21ABE66DE28F7BBADC39F.jpg =1264x912)

比较之后发现，我们到它的举例小于候选集当中的一个，于是我们更新候选集，去掉距离大于它的答案。然后我们重复上述的过程，直到根节点为止。

由于后面没有更近的点，所以候选集一直没有更新，最后上图当中的三个打了绿标的点就是答案。

我们把上面的流程整理一下，就得到了递归函数当中的逻辑，我们用Python写出来其实已经和代码差不多了：


```python
def query(node, data, answers, K):
    # 判断node是否已经访问过
    if node.visited:
        # 标记访问
        node.visited = True
        # 计算data到node中点的距离
        dis = cal_dis(data, node.point)
        # 如果小于答案中最大值则更新答案
        if dis < max(answers):
            answers.update(node.point)
        # 计算data到分割线的距离
        dis = cal_dis(data, node.split)
        # 如果小于最长距离，说明另一侧还可能有答案
        if dis < max(answers):
            # 获取当前节点的兄弟节点
            brother = self.get_brother(node)
            if brother is not None:
                # 往下搜索到叶子节点，从叶子节点开始寻找
                leaf = self.iter_down(brother, data)
                if leaf is not None:
                    return self.query(leaf, data, answers, K)
        # 如果已经到了根节点了，退出
        if node is root:
            return answers
        # 递归父亲节点
        return self.query(node.father, data, answers, K)
    else:
        if node is root:
            return answers
        return self.query(node.father, data, answers, K)

```

最终写成的代码和上面这段并没有太多的差别，在得到距离之后和答案当中的最大距离进行比较的地方，我们使用了优先队列。其他地方几乎都是一样的，我也贴上来给大家感受一下：

```python
def _query_nearest_k(self, node, path, data, topK, K):
    # 我们用set记录访问过的路径，而不是直接在节点上标记
    if node not in path:
        path.add(node)
        # 计算欧氏距离
        dis = KDTree.distance(node.value, data)
        if (len(topK) < K or dis < topK[-1]['distance']):
            topK.append({'node': node, 'distance': dis})
            # 使用优先队列获取topK
            topK = heapq.nsmallest(K, topK, key=lambda x: x['distance'])
        axis = node.axis
        # 分割线都是直线，直接计算坐标差
        dis = abs(data[axis] - node.boundray)
        if len(topK) < K or dis <= topK[-1]['distance']:
            brother = self.get_brother(node, path)
            if brother is not None:
                next_node = self.iter_down(brother, data)
                if next_node is not None:
                    return self._query_nearest_k(next_node, path, data, topK, K)
        if node == self.root:
            return topK
        return self._query_nearest_k(node.father, path, data, topK, K)
    else:
        if node == self.root:
            return topK
        return self._query_nearest_k(node.father, path, data, topK, K)
```

这段逻辑大家应该都能看明白，但是有一个疑问是，我们为什么不在node里面加一个visited的字段，而是通过传入一个set来维护访问过的节点呢？这个逻辑只看代码是很难想清楚的，必须要亲手实验才会理解。如果在node当中加入一个字段当然也是可以的，如果这样做的话，在我们执行查找之后必须得手动再执行一次递归，将树上所有节点的node全部置为false，否则会影响下一次查询。而这会带来开销，所以才转换思路使用set来进行访问判断。

这里的iter_down函数和我们上面贴的查找叶子节点的函数是一样的，就是查找当前子树的叶子节点。如果我没记错的话，这也是我们文章当中第一次出现在递归当中调用另一个递归。对于初学者而言，这在理解上可能会相对困难一些。我个人建议可以亲自动手试一试在纸上画一个kd-tree进行手动模拟试一试，自然就能知道其中的运行逻辑了。这也是一个思考和学习非常好用的方法。

## 优化

当我们理解了整个kd-tree的建树和查找的逻辑之后，我们来考虑一下优化。

这段代码看下来初步可以找到两个可以优化的地方，第一个地方是我们建树的时候。我们每次递归的时候由于要将数据一分为二，我们是使用了排序的方法来实现的，而每次排序都是$nlogn$的复杂度，这其实是不低的。其实仔细想想，我们没有必要排序，我们只需要选出根据某个轴排序前n/2个数。也就是说这是一个选择问题，并不是排序问题，所以可以想到我们可以利用之前讲过的快速选择的方法来优化。使用快速选择，我们可以在$O(n)$的时间内完成数据的拆分。

另一个地方是我们在查询K个邻近点的时候，我们使用了优先队列维护的候选集当中的答案，方便我们对答案进行更新。同样，优先队列获取topK也是$nlogn$的复杂度。这里也是可以优化的，比较好的思路是使用大顶堆来代替。可以做到$logN$的插入和弹出，相比于heapq的nsmallest方法要效率更高。

## 总结

到这里，我们关于KD-tree的原理部分已经差不多讲完了，我们有了建树和查询功能之后就可以用在KNN算法上进行优化了。但是我们现在的KD-tree只支持建树以及查询，如果我们想要插入或者删除集合当中的数据应该怎么办？难道每次修改都重新建树吗？这显然不行，但是插入和删除节点都会引起树结构的变化很有可能导致树不再平衡，这个时候我们应该怎么办呢？

我们先卖个关子，相关的内容将会放到下一篇文章当中，感兴趣的同学不要错过哦。

最后，我把KD-tree完整的代码放在了ubuntu.paste上，想要查看完整源码的同学请在公众号内回复kd-tree进行查看。

今天的文章就是这些，如果觉得有所收获，请顺手点个在看或者转发吧，你们的举手之劳对我来说很重要。

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf3ak9b69aj3076076dgg.jpg)