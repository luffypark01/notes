# day13 内置函数和推导式

![image-20201230125816095](assets/image-20201230125816095.png) 

今日概要：

- 匿名函数
- 生成器
- 内置函数
- <span style='color:red;'>**附加**</span>：推导式，属于数据类型的知识，内部的高级的用法会涉及到【生成器】和【函数】的知识。



## 1. 匿名函数

传统的函数的定义包括了：函数名 + 函数体。

```python
def send_email():
    pass

# 1. 执行
send_email()
# 2. 当做列表元素
data_list = [send_email, send_email, send_email ]
# 3. 当做参数传递
other_function(send_email)
```



匿名函数，则是基于lambda表达式实现定义一个可以没有名字的函数，例如：

```python
data_list = [ lambda x:x+100,  lambda x:x+110, lambda x:x+120 ]

print( data_list[0] )
```

```python
f1 = lambda x:x+100

res = f1(100)
print(res)
```



基于Lambda定义的函数格式为：`lambda 参数:函数体`

- 参数，支持任意参数。

  ```python
  lambda x: 函数体
  lambda x1,x2: 函数体
  lambda *args, **kwargs: 函数体
  ```

- 函数体，只能支持单行的代码。

  ```python
  def xxx(x):
      return x + 100
      
  lambda x: x + 100
  ```

- 返回值，默认将函数体单行代码执行的结果返回给函数的执行这。

  ```python
  func = lambda x: x + 100
  
  v1 = func(10)
  print(v1) # 110
  ```



```python
def func(a1,a2):
    return a1 + a2 + 100

foo = lambda a1,a2: a1 + a2 + 100
```

匿名函数适用于简单的业务处理，可以快速并简单的创建函数。

### 练习题

根据函数写写出其匿名函数的表达方式

```python
def func(a1,a2):
    return a1 + a2

func = lambda a1,a2: a1+a2
```

```python
def func(data):
    return data.replace("苍老师","***")

func= lambda data: data.replace("苍老师","***")
```

```python
def func(data):
    name_list = data.replace(".")
    return name_list[-1]

func = lambda data: data.replace(".")[-1]
```

在编写匿名函数时，由于受限 函数体只能写一行，所以匿名函数只能处理非常简单的功能。



### 扩展：三元运算

简单的函数，可以基于lambda表达式实现。

简单的条件语句，可以基于三元运算实现，例如：

```python
num = input("请写入内容")

if "苍老师" in num:
    data = "臭不要脸"
else:
    data = "正经人"
    
print(data)
```

```python
num = input("请写入内容")
data = "臭不要脸" if "苍老师" in num else "正经人"
print(data)

# 结果 =  条件成立时    if   条件   else   不成立
```

lambda表达式和三元运算没有任何关系，属于两个独立的知识点。



掌握三元运算之后，以后再编写匿名函数时，就可以处理再稍微复杂点的情况了，例如：

```python
func = lambda x: "大了" if x > 66 else "小了"

v1 = func(1)
print(v1) # "小了"

v2 = func(100)
print(v2) # "大了"
```





## 2. 生成器

生成器是由函数+yield关键字创造出来的写法，在特定情况下，用他可以帮助我们节省内存。

- 生成器函数，但函数中有yield存在时，这个函数就是生产生成器函数。

  ```python
  def func():
      print(111)
      yield 1
  ```

  ```python
  def func():
      print(111)
      yield 1
  
      print(222)
      yield 2
  
      print(333)
      yield 3
  
      print(444)
  ```

- 生成器对象，执行生成器函数时，会返回一个生成器对象。

  ```python
  def func():
      print(111)
      yield 1
  
      print(222)
      yield 2
  
      print(333)
      yield 3
  
      print(444)
      
  data = func()
  
  # 执行生成器函数func，返回的生成器对象。
  # 注意：执行生成器函数时，函数内部代码不会执行。
  ```

  ```python
  def func():
      print(111)
      yield 1
  
      print(222)
      yield 2
  
      print(333)
      yield 3
  
      print(444)
      
  data = func()
  
  v1 = next(data)
  print(v1)
  
  v2 = next(data)
  print(v2)
  
  v3 = next(data)
  print(v3)
  
  v4 = next(data)
  print(v4)  # 结束或中途遇到return，程序爆：StopIteration 错误
  ```

  ```python
  data = func()
  
  for item in data:
      print(item)
  ```


生成器的特点是，记录在函数中的执行位置，下次执行next时，会从上一次的位置基础上再继续向下执行。



### 应用场景

- 假设要让你生成 300w个随机的4位数，并打印出来。

  - 在内存中一次性创建300w个
  - 动态创建，用一个创建一个。

  ```python
  import random
  
  val = random.randint(1000, 9999)
  print(val)
  ```

  ```python
  import random
  
  data_list = []
  for i in range(300000000):
      val = random.randint(1000, 9999)
  	data_list.append(val)
      
  # 再使用时，去 data_list 中获取即可。
  # ...
  ```

  ```python
  import random
  
  
  def gen_random_num(max_count):
      counter = 0
      while counter < max_count:
          yield random.randint(1000, 9999)
          counter += 1
  
  
  data_list = gen_random_num(3000000)
  # 再使用时，去 data_list 中获取即可。
  ```

- 假设让你从某个数据源中获取300w条数据（后期学习操作MySQL 或 Redis等数据源再操作，了解思想即可）。

<img src="assets/image-20201230174253215.png" alt="image-20201230174253215" style="zoom: 33%;" />

所以，当以后需要我们在内存中创建很多数据时，可以想着用基于生成器来实现一点一点生成（用一点生产一点），以节省内存的开销。



### 扩展

```python
def func():
    print(111)
    v1 = yield 1
    print(v1)

    print(222)
    v2 = yield 2
    print(v2)

    print(333)
    v3 = yield 3
    print(v3)

    print(444)


data = func()

n1 = data.send(None)
print(n1)

n2 = data.send(666)
print(n2)

n3 = data.send(777)
print(n3)

n4 = data.send(888)
print(n4)
```



## 3.内置函数

<img src="assets/image-20201230201618164.png" alt="image-20201230201618164" style="zoom:50%;" />

Python内部为我们提供了很多方便的内置函数，在此整理出来36个给大家来讲解。

- 第1组（5个）

  - abs，绝对值

    ```python
    v = abs(-10)
    ```

  - pow，指数

    ```python
    v1 = pow(2,5) # 2的5次方  2**5
    print(v1)
    ```

  - sum，求和

    ```python
    v1 = sum([-11, 22, 33, 44, 55]) # 可以被迭代-for循环
    print(v1)
    ```

  - divmod，求商和余数

    ```
    v1, v2 = divmod(9, 2)
    print(v1, v2)
    ```

  - round，小数点后n位（四舍五入）

    ```python
    v1 = round(4.11786, 2)
    print(v1) # 4.12
    ```

- 第2组：（4个）

  - min，最小值

    ```python
    v1 = min(11, 2, 3, 4, 5, 56)
    print(v1) # 2
    ```

    ```
    v2 = min([11, 22, 33, 44, 55]) # 迭代的类型（for循环）
    print(v2)
    ```

    ```python
    v3 = min([-11, 2, 33, 44, 55], key=lambda x: abs(x))
    print(v3) # 2
    ```

  - max，最大值

    ```python
    v1 = max(11, 2, 3, 4, 5, 56)
    print(v1)
    
    v2 = max([11, 22, 33, 44, 55])
    print(v2)
    ```

    ```python
    v3 = max([-11, 22, 33, 44, 55], key=lambda x: x * 10)
    print(v3) # 55
    ```

  - all，是否全部为True

    ```python
    v1 = all(   [11,22,44,""]   ) # False
    ```

  - any，是否存在True

    ```python
    v2 = any([11,22,44,""]) # True
    ```

    

- 第3组（3个）

  - bin，十进制转二进制
  - oct，十进制转八进制
  - hex，十进制转十六进制

- 第4组（2个）

  - ord，获取字符对应的unicode码点（十进制）

    ```
    v1 = ord("武")
    print(v1, hex(v1))
    ```

  - chr，根据码点（十进制）获取对应字符

    ```python
    v1 = chr(27494)
    print(v1)
    ```

- 第5组（9个）

  - int

  - foat

  - str，unicode编码

  - bytes，utf-8、gbk编码

    ```python
    v1 = "武沛齐"  # str类型
    
    v2 = v1.encode('utf-8')  # bytes类型
    
    v3 = bytes(v1,encoding="utf-8") # bytes类型
    ```

  - bool

  - list

  - dict

  - tuple

  - set

- 第6组（13个）

  - len

  - print

  - input

  - open

  - type，获取数据类型

    ```python
    v1 = "123"
    
    if type(v1) == str:
        pass
    else:
        pass
    ```

  - range 

    ```python
    range(10)
    ```

  - enumerate

    ```python
    v1 = ["武沛齐", "alex", 'root']
    
    for num, value in enumerate(v1, 1):
        print(num, value)
    ```

  - id

  - hash

    ```python
    v1 = hash("武沛齐")
    ```

  - help，帮助信息

    - pycharm，不用
    - 终端，使用

  - zip

    ```python
    v1 = [11, 22, 33, 44, 55, 66]
    v2 = [55, 66, 77, 88]
    v3 = [10, 20, 30, 40, 50]
        
    result = zip(v1, v2, v3)
    for item in result:
        print(item)
    ```

  - callable，是否可执行，后面是否可以加括号。

    ```
    v1 = "武沛齐"
    v2 = lambda x: x
    def v3():
        pass
    
    
    print( callable(v1) ) # False
    print(callable(v2))
    print(callable(v3))
    ```

  - sorted，排序

    ```python
    v1 = sorted([11,22,33,44,55])
    ```

    

    ```python
    info = {
        "wupeiqi": {
            'id': 10,
            'age': 119
        },
        "root": {
            'id': 20,
            'age': 29
        },
        "seven": {
            'id': 9,
            'age': 9
        },
        "admin": {
            'id': 11,
            'age': 139
        },
    }
    
    result = sorted(info.items(), key=lambda x: x[1]['id'])
    print(result)
    ```

    ```python
    data_list = [
        '1-5 编译器和解释器.mp4',
        '1-17 今日作业.mp4',
        '1-9 Python解释器种类.mp4',
        '1-16 今日总结.mp4',
        '1-2 课堂笔记的创建.mp4',
        '1-15 Pycharm使用和破解（win系统）.mp4',
        '1-12 python解释器的安装（mac系统）.mp4',
        '1-13 python解释器的安装（win系统）.mp4',
        '1-8 Python介绍.mp4', '1-7 编程语言的分类.mp4',
        '1-3 常见计算机基本概念.mp4',
        '1-14 Pycharm使用和破解（mac系统）.mp4',
        '1-10 CPython解释器版本.mp4',
        '1-1 今日概要.mp4',
        '1-6 学习编程本质上的三件事.mp4',
        '1-18 作业答案和讲解.mp4',
        '1-4 编程语言.mp4',
        '1-11 环境搭建说明.mp4'
    ]
    result = sorted(data_list, key=lambda x: int(x.split(' ')[0].split("-")[-1]) )
    print(result)
    ```





## 4.推导式

推导式是Python中提供了一个非常方便的功能，可以让我们通过一行代码实现创建list、dict、tuple、set 的同时初始化一些值。

请创建一个列表，并在列表中初始化：0、1、2、3、4、5、6、7、8、9...299 整数元素。

```python
data = []
for i in range(300):
    data.append(i)
```

- 列表

  ```python
  num_list = [ i for i in range(10)]
  
  num_list = [ [i,i] for i in range(10)]
  
  num_list = [ [i,i] for i in range(10) if i > 6 ]
  ```

- 集合

  ```python
  num_set = { i for i in range(10)}
  
  num_set = { (i,i,i) for i in range(10)}
  
  num_set = { (i,i,i) for i in range(10) if i>3}
  ```

- 字典

  ```python
  num_dict = { i:i for i in range(10)}
  
  num_dict = { i:(i,11) for i in range(10)}
  
  num_dict = { i:(i,11) for i in range(10) if i>7}
  ```

- 元组，<span style="color:red">不同于其他类型。</span>

  ```python
  # 不会立即执行内部循环去生成数据，而是得到一个生成器。
  data = (i for i in range(10))
  print(data)
  for item in data:
      print(item)
  ```



### 练习题

1. 去除列表中每个元素的 `.mp4`后缀。

   ```python
   data_list = [
       '1-5 编译器和解释器.mp4',
       '1-17 今日作业.mp4',
       '1-9 Python解释器种类.mp4',
       '1-16 今日总结.mp4',
       '1-2 课堂笔记的创建.mp4',
       '1-15 Pycharm使用和破解（win系统）.mp4',
       '1-12 python解释器的安装（mac系统）.mp4',
       '1-13 python解释器的安装（win系统）.mp4',
       '1-8 Python介绍.mp4', '1-7 编程语言的分类.mp4',
       '1-3 常见计算机基本概念.mp4',
       '1-14 Pycharm使用和破解（mac系统）.mp4',
       '1-10 CPython解释器版本.mp4',
       '1-1 今日概要.mp4',
       '1-6 学习编程本质上的三件事.mp4',
       '1-18 作业答案和讲解.mp4',
       '1-4 编程语言.mp4',
       '1-11 环境搭建说明.mp4'
   ]
   
   result = []
   for item in data_list:
       result.append(item.rsplit('.',1)[0])
       
   result = [ item.rsplit('.',1)[0] for item in data_list]
   ```

2. 将字典中的元素按照 `键-值`格式化，并最终使用 `;`连接起来。

   ```python
   info = {
       "name":"武沛齐",
       "email":"xxx@live.com",
       "gender":"男",
   }
   
   data_list [] 
   for k,v in info.items():
       temp = "{}-{}".format(k,v)
       temp.append(data_list)
   resutl = ";".join(data)
   
   result = ";".join( [ "{}-{}".format(k,v) for k,v in info.items()] )
   ```

3. 将字典按照键从小到大排序，然后在按照如下格式拼接起来。（微信支付API内部处理需求）

   ```python
   info = {
       'sign_type': "MD5",
       'out_refund_no': "12323",
       'appid': 'wx55cca0b94f723dc7',
       'mch_id': '1526049051',
       'out_trade_no': "ffff",
       'nonce_str': "sdfdffd",
       'total_fee': 9901,
       'refund_fee': 10000
   }
   
   data = "&".join(["{}={}".format(key, value) for key, value in sorted(info.items(), key=lambda x: x[0])])
   print(data)
   ```

4. 看代码写结果

   ```python
   def func():
       print(123)
   
   
   data_list = [func for i in range(10)]
   
   print(data_list)
   ```

5. 看代码写结果

   ```python
   def func(num):
       return num + 100
   
   
   data_list = [func(i) for i in range(10)]
   
   print(data_list)
   ```

6. 看代码写结果（执行出错，通过他可以让你更好的理解执行过程）

   ```python
   def func(x):
       return x + i
   
   data_list = [func for i in range(10)]
   
   val = data_list[0](100)
   print(val)
   ```

7. 看代码写结果（新浪微博面试题）

   ```python
   data_list = [lambda x: x + i for i in range(10)]  # [函数,函数,函数]   i=9
   
   v1 = data_list[0](100)
   v2 = data_list[3](100)
   print(v1, v2)  # 109 109
   ```

   

### 小高级

1. 推导式支持嵌套

   ```python
   data = [ i for i in range(10)]
   
   data = []
   for i in range(10):
       data.append(i)
   ```

   ```python
   data = [ [i,j] for j in range(5) for i in range(10) ]
   
   data = []
   for j in range(5):
       for i in range(10):
           data.append([i,j])
   ```

   ```python
   # 一副扑克牌
   poker_list = [ [color, num] for num in range(1, 14) for color in ["红桃", "黑桃", "方片", "梅花"]]
   print(poker_list)	
   
   
   poker_list = [ (color,num) for num in range(1,14) for color in ["红桃", "黑桃", "方片", "梅花"] ]
   ```

2. 烧脑面试题

   ```python
   def num():
          return [lambda x: i * x for i in range(4)]
      
      
      # 1. num()并获取返回值  [函数,函数,函数,函数] i=3
      # 2. for循环返回值
      # 3. 返回值的每个元素(2)
      result = [m(2) for m in num()]  # [6,6,6,6]
      print(result)
   ```

   ```python
   def num():
          return (lambda x: i * x for i in range(4))
      
      
      # 1. num()并获取返回值  生成器对象
      # 2. for循环返回值
      # 3. 返回值的每个元素(2)
      result = [m(2) for m in num()]  # [0,2,4,6 ]
      print(result)
   
   ```
   

### 内置函数

    abs		# 求绝对值
    all 	#Return True if bool(x) is True for all values x in the iterable.If the iterable is empty, return True.
    any    #Return True if bool(x) is True for any x in the iterable.If the iterable is empty, return False.
    ascii  #Return an ASCII-only representation of an object,ascii("中国") 返回"'\\u4e2d\\u56fd'"
    bin    #返回整数的2进制格式
    bool   # 判断一个数据结构是True or False, bool({}) 返回就是False, 因为是空dict
    bytearray # 把byte变成 bytearray, 可修改的数组
    bytes    # bytes("中国","gbk")
    callable # 判断一个对象是否可调用
    chr		 # 返回一个数字对应的ascii字符 ， 比如chr(90)返回ascii里的'Z'
    
    classmethod #面向对象时用，现在忽略
    compile		#py解释器自己用的东西，忽略
    complex		#求复数，一般人用不到
    copyright	#没用
    credits	   	#没用
    delattr     #面向对象时用，现在忽略
    dict		#生成一个空dict
    dir			#返回对象的可调用属性
    divmod		#返回除法的商和余数 ，比如divmod(4,2)，结果(2, 0)
    enumerate   #返回列表的索引和元素，比如 d = ["alex","jack"]，enumerate(d)后，得到(0, 'alex')  (1, 'jack')
    eval		#可以把字符串形式的list,dict,set,tuple,再转换成其原有的数据类型。
    exec        #把字符串格式的代码，进行解义并执行，比如exec("print('hellworld')")，会解义里面的字符串并执行
    exit		#退出程序
    filter		#对list、dict、set、tuple等可迭代对象进行过滤， filter(lambda x:x>10,[0,1,23,3,4,4,5,6,67,7])过滤出所有大于10的值
    float		#转成浮点
    format		#没用
    frozenset   #把一个集合变成不可修改的
    getattr		#面向对象时用，现在忽略
    globals		#打印全局作用域里的值
    hasattr		#面向对象时用，现在忽略
    hash		#hash函数
    help
    hex			#返回一个10进制的16进制表示形式,hex(10) 返回'0xa'
    id			#查看对象内存地址
    input
    int
    isinstance  #判断一个数据结构的类型，比如判断a是不是fronzenset, isinstance(a,frozenset) 返回 True or False
    issubclass	#面向对象时用，现在忽略
    iter		#把一个数据结构变成迭代器，讲了迭代器就明白了
    len
    list
    locals
    map			# map(lambda x:x**2,[1,2,3,43,45,5,6,]) 输出 [1, 4, 9, 1849, 2025, 25, 36]
    max			# 求最大值
    memoryview  # 一般人不用，忽略
    min			# 求最小值
    next		# 生成器会用到，现在忽略
    object		#面向对象时用，现在忽略
    oct         # 返回10进制数的8进制表示
    open
    ord			# 返回ascii的字符对应的10进制数 ord('a') 返回97，
    print
    property    #面向对象时用，现在忽略
    quit
    range
    repr       #没什么用
    reversed   # 可以把一个列表反转
    round		#可以把小数4舍5入成整数 ，round(10.15,1)  得10.2
    set
    setattr     #面向对象时用，现在忽略
    slice      # 没用
    sorted
    staticmethod #面向对象时用，现在忽略
    str
    sum        #求和,a=[1, 4, 9, 1849, 2025, 25, 36],sum(a) 得3949
    super		#面向对象时用，现在忽略
    tuple
    type
    vars       #返回一个对象的属性，面向对象时就明白了
    zip		   #可以把2个或多个列表拼成一个， a=[1, 4, 9, 1849, 2025, 25, 36]，b = ["a","b","c","d"]，
                list(zip(a,b)) 得结果
                [(1, 'a'), (4, 'b'), (9, 'c'), (1849, 'd')]



## 总结

1. 匿名函数，基于lambda表达式实现一行创建一个函数。一般用于编写简单的函数。
2. 三元运算，用一行代码实现处理简单的条件判断和赋值。
3. 生成器，函数中如果yield关键字
   - 生成器函数
   - 生成器对象
   - 执行生成器函数中的代码
     - next
     - for（常用）
     - send
4. 内置函数（36个）
5. 推导式
   - 常规操作
   - 小高级操作











