## 单链表的定义

链表是线性表的一种，线性表链式储存结构的特点是：用一组任意的存储单元存储线性表的数据元素(这组存储的单元可以是连续的，也可以是不连续的)。

<p>

**为了清楚地表示单链表，我们还要引入节点的概念**

<p>

### 节点的定义

为了表示链表中每一个数据元素a(i)与其直接后继数据元素a(i+1)之间的逻辑关系，对于数据a(i)来说，除了存储其本身的信息之外还需要存储一个指向其直接后继的信息。这两部分组成数据元素a(i)的存储映像，称为节点。节点有两个域，一个是存储信息的数据域，一个是存储下一个节点位置的指针域。指针域中存储的信息称作指针或链。

<p>

有了节点这个概念，我们就可以用python中的类来实现，节点的两个域我们可以实列的属性表示，通过实例化这个类就可以生成节点。

<p>

节点的实现：

```python
class Node():
    def __init__(self, dataval=None):
        self.dataval = dataval  # 数据域，生成节点需要具体内容
        self.nextval = None		# 指针域
```

<strong style="color:red">n个节点链结成一个链表。如果每个节点只包含一个指针域，那么这个链表就是线性链表又称单链表</strong>

<br>

我们一般将链表画成用箭头相链接的节点序列

![](img\06.png)

为了处理方便，我们一般在单链表的第一个节点(首元节点)之前再加一个节点，称为**头节点**，头节点的数据域一般不存储内容，或者存与整个链表相关的信息，比如链表的长度之类的，头节点的指针域指向首元节点。

同时还要引入一个概念**:头指针**，头指针指向列表开始的位置，如果有头节点就指向头节点，如果没有头节点就指向首元节点。

再加一个头节点是为了链表处理起来更加方便，这样就不必对元首节点做特俗处理，而且即便链表为空，头指针的指向也不为空。

<br>

在python中我们一般用类和实例来实现链表，下面是python代码：

```python
# 节点
class Node():
    def __init__(self, dataval=None):
        self.dataval = dataval  # 数据域
        self.nextval = None		# 指针域
# 链表
class Slinkedlist():
    def __init__(self):
        self.headval = None  # 初始化头指针
        
list1 = Slinkedlist()  # 生成链表
list1.headval = Node()  # 设置一个头节点，头指针指向头节点
e1 = Node("Mon")  #生成节点
e2 = Node("Tus")
e3 = Node("Wed")
# 再将各个节点连起来
list1.headval.nextval = e1  # 将头节点的指针域指向首元节点
e1.nextval = e2  # 下面一次按顺序连起来
e2.nextval = e3
```

实现了链表，接下来就是实现链表的方法和属性

1.打印链表，思路就是从首节点开始，通过节点指针域不断跳转到下一个，直到节点域为空为止

```python
class Slinkedlist():
    def __init__(self):
        self.headval = None  # 初始化头指针
    def printlist(self):  # 遍历链表
        printval = self.headval.nextval  # 这里是从元首节点开始
        while printval:
            print(printval.dataval)  # 打印内容
            printval = printval.nextval
```

2.求链表长度，思路差不多，加一个计数器就行了

```python
class Slinkedlist():
    def __init__(self):
        self.headval = None  # 初始化头指针
    def __len__(self):  # 求链表的长度
        lenval = self.headval.nextval
        num = 0
        while lenval:
            num += 1
            lenval = lenval.nextval
        self.headval.dataval = num  # 头节点储存链表的长度
        return num
```

3.通过下标取值，这里依赖前面的求长度的方法来判断输入下标的合法性

```python
class Slinkedlist():
    def __init__(self):
        self.headval = None  # 初始化头指针
    def get(self, index, k=0):  # 取值操作
        nm = -1  # 从-1开始表示从头节点开始，但是一般来说输入的下标是从0开始的
        lenth = self.__len__()  # 先求出长度判断是否越界
        if index > lenth - 1 or index < -1:
            raise "链表下标越界"
        node = self.headval  # 定义第一个值，从头节点开始找
        while nm < index:
            node = node.nextval
            nm += 1
        if k == 0:
            return node.dataval
        else:
            return node
```

4.通过值求下标，我们规定，头节点的下标为-1，首元节点的下标为0，后面依次递增

```python
class Slinkedlist():
    def __init__(self):
        self.headval = None  # 初始化头指针
        
    def index(self, data):  # 通过值拿到下标
        nm = 0
        node = self.headval.nextval
        while node:
            if node.dataval == data:
                return nm
            else:
                nm += 1
                node = node.nextval
        else:
            return None
```

5.插入操作，链表的插入操作和顺序表不同，链表的插入不需要移位，只需要将要插入的位置的前一个节点的指针域改为指向新的节点，同时新加入的节点的指针域要指向后面的节点。因为有头节点，所以我们不需要对首元节点特殊处理。

```python
class Slinkedlist():
    def __init__(self):
        self.headval = None  # 初始化头指针
 def insert(self, index, value):  # 插值操作
        newnode = value  # 获取新的节点
        ahead = self.get(index - 1, k=1)  # 获取要插入位置前面的节点
        newnode.nextval = ahead.nextval
        ahead.nextval = newnode
```

6.删除操作，同插入操作类似，这里我们将通过下标删除和通过值删除合在一起

```python
class Slinkedlist():
    def __init__(self):
        self.headval = None  # 初始化头指针
        
    def remove(self, obj):
        if type(obj) == int:
            if obj < -1:
                raise "输入的值不合法"
            data = self.get(obj,k=1)
            ahead = self.get(obj - 1,k=1)
            ahead.nextval = data.nextval
        else:
            index = self.index(obj)
            data = self.get(index,k=1)
            ahead = self.get(index -1,k=1)
            ahead.nextval = data.nextval
```

好了，python单链表基本就这些了。
