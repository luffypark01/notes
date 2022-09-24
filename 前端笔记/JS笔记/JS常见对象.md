# JavaScript常用对象介绍

#### 对象(object)

到目前为止，我们看到的大多数的像Array、String、Number等这些类型的，他们统统属于对象，而且，object也是ECMAScript中使用最多的一个类型。并且每个对象都有属性和方法。好比是可以构造出一个人，这个人他有年龄和姓名等，这些统统属于这个人的**属性**，这个人他有爱泡妹子，泡妹子是这个人的动作，我们称为这个对象的**方法**。

**对象的属性**：它是属于这个对象的某个变量。比如字符串的长度、数组的长度和索引、图像的宽高等

**对象的方法**：只有某个特定属性才能调用的函数。表单的提交、时间的获取等。

> 由此可见，对象是由一些彼此相关的属性和方法集合在一起而构成的一个数据实体。

##### 对象的创建方式

为了使用Person对象来描述一个特定的人,我们需要创建一个Person对象的实例(instance)。实例是对象的具体表示；对象是统称，实例是个体。例如，你和我都是人，都可以用Person对象来描述；但你和我是两个不同的个体，很可能有着不同的属性（例如，你和我的年龄可能不一样）。因此，你和我对应着两个不同的Person对象——他们虽然都是Person对象,但它们是两个不同的实例

1. 创建Object实例的方式有两种。第一种是使用new操作符后跟Object构造函数，如下所示：

```arcade
//创建对象
var person = new Object();
//给对象添加name和age属性
person.name = 'jack';
person.age = 28;
//给对象添加fav的方法
person.fav = function(){
    console.log('泡妹子');
}
```

1. 使用**对象字面量**表示法。对象字面量是对象定义的一种简写形式，目的在于简化创建包含大量属性的对象的过程。

```arcade
var person = {
    name : 'jack';
    age : 28,
    fav : function(){
   	 console.log('泡妹子');
	}
}
```

在上个例子中，从`{`开始，表示对象字面量的开始，一直到最后的`}`结束。我们定义了name属性，之后是一个冒号，再后面是这个属性的值。在对象字面量中，使用逗号来分离不同的属性。

在使用对象字面量语法时，属性名也可以使用字符串，如下：

```arcade
var person = {
    "name" : 'jack';
    "age" : 28,
    "fav" : function(){
   	 console.log('泡妹子');
	}
}
```

除此之外

```abnf
var person = {};//与new Object()相同
```

##### 点语法

上述的例子，我们如何来访问person对象中的age属性和fav方法呢？**点语法**是你最好的朋友。例如

```awk
person.name; //jack
person.fav();//泡妹子
```

##### 括号表示法

另外一种访问属性的方式使用括号表示法，代替这样的代码

```awk
person.name; //jack
```

使用如下所示的代码

```css
person['name'];
```

> 总结：我们推荐使用**点语法**来访问对象的属性。

#### 内置对象

在电视上的烹饪节目中,只要镜头一转,厨师就可以端出一盘美味的菜肴并向大家介绍说:"这是我刚做好的"。Javascript与这种节目里的主持人颇有几分相似：它提供了一些列预先定义好的对象，而我们可以把这些对象直接用在自己的脚本里。我们称这种对象叫**内置对象（navtive object）。**

其实我们已经见过一些Javascript内置对象了。数组就是一种Javascript内置对象中的一种。本节我们主要介绍数组常用的**属性**和**方法**。

##### Array

###### 数组创建方式

1. 跟object创建对象一样，使用Array构造函数方式创建，如下代码

   ```arcade
   var colors = new Array();
   ```

2. 使用字面量方式创建数组

   ```abnf
   var colors = [];
   ```

###### 检测数组

确定某个值到底是否是数组

```arcade
var colors = ['red','green','blue'];
if(Array.isArray(colors)){
    //对数组执行某些操作
}
```

###### 转换方法

调用数组的toString()方法会返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串。

```actionscript
var colors = ['red','green','blue'];
alert(colors.toString());//red,green,blue
```

另外，`toLocalString()`方法经常也会返回与`toString()`方法相同的值，但也不总是如此。例如：

```actionscript
var person1 = {
    toLocaleString : function () {
        return '马大帅';
    },
    toString : function () {
        return '马小帅';
    }
}
var person2 = {
    toLocaleString : function () {
        return '隔壁老王';
    },
    toString : function () {
        return '隔壁老李';
    }
}
var people = [person1,person2];
alert(people);//马小帅,隔壁老李
alert(people.toString());//马小帅,隔壁老李
alert(people.toLocaleString()); //马大帅,隔壁老王
```

定义了两个对象： person1和person2。分别为每个对象定义了一个`toString()`方法和`toLocalString()`方法，各个方法返回不同的值。然后，创建一个包含前面定义的两个对象的数组。将数组传递给`alert()`时，输出结果为`马小帅,隔壁老李`。因为调用数组每一项的`toString()`方法(同样，这与下一行显示调用toString())方法得到的结果相同。而当调用数组的每一项的`toLocalString()`方法.

###### 分割字符串

`toLocalString()`方法和`toString()`方法，在默认情况下都会以逗号分割的字符串的形式返回。而如果使用`join()`方法，`join()`方法只接收一个参数。

```axapta
var colors = ['red','blue','green'];
colors.join('||'); //red||blue||green
```

###### 栈方法

数组也可以想栈一样，既可以插入和删除项的数据结构。栈是一种LIFO(Last-In-First-Out,后进先出)的数据结构，也就是最新添加的那项元素最早被删除。而栈中项的插入（叫做**推入**）和移除(叫做**弹出**)，只发生在一个位置——栈的顶部。数组专门提供了`push()`和`pop()`方法，以便实现类似栈的行为。

**push()方法**

可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。

```arcade
var colors = [];
var count = colors.push('red','blue','green');
alert(count); //3
```

**pop()方法**

从数组末尾移除最后一项，减少数组的 length 值，**然后返回移除的项 。**

```arcade
var item = colors.pop(); //取最后一项
alert(item); //green
alert(colors.length); //2
```

###### 队列方法

栈数据结构的访问规则是 `LIFO(后进先出)`，而队列数据结构的访问规则是 `FIFO(First-In-First-Out， 先进先出)`。队列在列表的末端添加项，从列表的前端移除项。由于` push()`是向数组末端添加项的方法， 因此要模拟队列只需一个从数组前端取得项的方法。实现这一操作的数组方法就是` shift()`，它能够移 除数组中的第一个项并返回该项，同时将数组长度减 1。结合使用 `shift()`和` push()`方法，可以像使 用队列一样使用数组。

**shift()方法**

```awk
var colors = ['red','blue','green'];
var item = colors.shift();//取得第一项
alert(item); //"red"
alert(colors.length); //2
```

**unshift()方法**

ECMAScript 还为数组提供了一个` unshift()`方法。顾名思义，`unshift()`与` shift()`的用途相反: 它能在数组前端添加任意个项并返回新数组的长度。因此，同时使用 `unshift()`和 `pop()`方法，可以 12 从相反的方向来模拟队列，即在数组的前端添加项，从数组末端移除项 。

```arcade
var colors = [];
var count  = colors.unshift('red','green'); //推入两项
alert(count); //2
console.log(colors); // ["red", "green"]
```

###### 重排序方法

数组中已经存在两个可以直接用来重排序的方法：`reverse()`和`sort()`。

**reverse()方法**

reverse翻转数组项的顺序

```maxima
var values = [1,2,3,4,5];
values.reverse();
alert(values); // 5,4,3,2,1
```

**sort()方法**

 默认情况下，`sort()`方法按升序排列——即最小的值位于最前面，最大的值排在最后面。 为了实现排序，`sort()`方法会调用每个数组项的` toString()`转型方法，然后比较得到的字符串，以确定如何排序 。即使数组中的每一项都是数值，sort()方法比较的也是字符串。如下所示。

```maxima
var values = [0,1,5,10,15];
varlus.sort();
alert(values); //0,1,10,15,5
```

 可见，即使例子中值的顺序没有问题，但 sort()方法也会根据测试字符串的结果改变原来的顺序。 因为数值 5 虽然小于 10，但在进行字符串比较时，"10"则位于"5"的前面，于是数组的顺序就被修改了。 不用说，这种排序方式在很多情况下都不是最佳方案。

 因此 sort()方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值的前面。 以完成数组中数值的**升序**和**降序**功能

 比较函数接收两个参数，如果第一个参数位于第二个参数之前则返回一个负数，如果两个参数相等则返回0，如果第一个参数位于第二个参数之后则返回正数。举例：

```smali
function compare(v1,v2){
    if(v1 < v2){
        return -1;
    }else if (v1 > v2){
        return 1;
    }else{
        return 0;
    }
}
```

 这个比较函数可以适用于大多数数据类型，只要将其作为参数传递给sort()方法即可，如下面例子所示。

```maxima
var values = [0,1,5,10,15];
values.sort(compare);
alert(values); //0,1,5,10,15
```

 在将比较函数传递到`sort()`方法之后，数值仍然保持了正确的升序。当然，也可以通过比较函数产生降序排序的结果，只要交换比较函数返回的值即可。

```maxima
function compare(v1,v2){
    if(v1 < v2){
        return 1;
    }else if (v1 > v2){
        return -1;
    }else{
        return 0;
    }
}
var values = [0, 1, 5, 10, 15];
values.sort(compare);
alert(values);    // 15,10,5,1,0
```

 比较函数在第一个值应该位于第二个之后的情况下返回 1，而在第一个值 应该在第二个之前的情况下返回-1。交换返回值的意思是让更大的值排位更靠前，也就是对数组按照降 序排序。当然，如果只想反转数组原来的顺序，使用 reverse()方法要更快一些。

###### 操作方法

ECMAScript为操作已经包含在数据中项提供了许多方法。其中包含了像`concat()`、`slice()`、`splice()`来对数组的项进行操作。

**concat()方法**

数组合并方法，一个数组调用concat()方法去合并另一个数组，返回一个新的数组。concat()接收的参数是可以是任意的。

- 参数为一个或多个数组，则该方法会将这些数组中每一项都添加到结果数组中。
- 参数不是数组，这些值就会被简单地添加到结果数组的末尾

```awk
var colors = ['red','blue','green'];
colors.concat('yello');//["red", "blue", "green", "yello"]
colors.concat({'name':'张三'});//["red", "blue", "green", {…}]
colors.concat({'name':'李四'},['black','brown']);// ["red", "blue", "green", {…}, "black", "brown"]
```

**slice()方法**

`slice()`方法，它能够基于当前数组中一个或多个项创建一个新数组。`slice()`方法可以接受一或两个参数，既要返回项的起始和结束位置。

- 一个参数的情况下，slice()方法会返回从该参数指定位置开始到当前数组默认的所有项
- 两个参数的情况下，该方法返回起始和结束位置之间的项——但不包括结束位置的项。

> 注意： slice()方法不会影响原始数组

```awk
var colors = ['red','blue','green','yellow','purple'];
colors.slice(1);//["blue", "green", "yellow", "purple"]
colors.slice(1,4);// ["blue", "green", "yellow"]
```

>  如果`slice()`方法的参数中有一个负数，则用数组长度加上该数来确定响应的位置。例如，在一个包含5项的数组上调用`slice(-2,-1)`与调用`slice(3,4)`得到的结果相同。如果技术位置小于起始位置，则返回空数组

```arcade
var colors = ['red','blue','green','yellow','purple'];
colors.slice(-2,-1);//["yellow"] 
colors.slice(-1,-2);//[]
```

**splice()方法**

`splice()`方法这个恐怕要算是最强大的数组的方法了，它有很多种用法。

`splice()`的主要用途是向数组的中路插入想。使用这种方法的方式则有3中。

1. **删除**：可以删除任意数量的项，只需指定2个参数：要删除的第一项的位置和要删除的个数。例如`splice(0,2)`会删除数组中的前两项
2. **插入**：可以向指定位置插入任意数量的项，只需提供3个参数：**起始位置**、**0（要删除的个数）**和**要插入的项**。如果要插入多个项，可以再传入第四、第五、以至任意多个项。例如，`splice(2,0,'red','green')`会从当前数组的位置2开始插入字符串`'red'`和`'green'`。
3. **替换**：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数:**起始位置**、**要删除的项数**和**要插入的任意数量的项**。插入的项数不必与删除的项数相等。例如，` splice (2,1,"red","green")`会删除当前数组位置 2 的项，然后再从位置 2 开始插入字符串` "red"`和`"green"`。

splice()方法始终都会返回一个数组，该数组中包含从原始数组中删除的项(如果没有删除任何 项，则返回一个空数组)。

```awk
var colors = ["red", "green", "blue"];
var removed = colors.splice(0,1); 
alert(colors); // green,blue 
alert(removed); // red，返回的数组中只包含一项
removed = colors.splice(1, 0, "yellow", "orange"); 
alert(colors); // green,yellow,orange,blue alert(removed); // 返回的是一个空数组
removed = colors.splice(1, 1, "red", "purple"); 
alert(colors); // green,red,purple,orange,blue alert(removed); // yellow，返回的数组中只包含一项
```

###### 位置方法

ECMAScript 5 为数组实例添加了两个位置方法:`indexOf()`和 `lastIndexOf()`。这两个方法都接收两个参数:要查找的项和(可选的)表示查找起点位置的索引。其中，**indexOf()方法从数组的开头(位置 0)开始向后查找，lastIndexOf()方法则从数组的末尾开始向前查找**。

这两个方法都返回要查找的那项在数组中的位置，或者在没找到的情况下返回-1。在比较第一个参数与数组中的每一项时，会使用全等操作符；也就是说，要求查找的项必须严格相等(就像使用===一样)。举个栗子：

```arcade
var numbers = [1,2,3,4,5,4,3,2,1];
alert(numbers.indexOf(4)); //3
alert(numbers.lastIndexOf(4));// 5
alert(numbers.indexOf(4,4));// 5
alert(numbers.lastIndexOf(4,4));//3

var person = {name:"mjj"};
var people = [{name:'mjj'}];
var morePeople = [person];
alert(people.indexOf(person)); //-1
alert(morePeople.indexOf(person));// 0
```

###### 迭代方法

ECMAScript 5 为数组定义了 5 个迭代方法。

- every()
- some()
- filter()
- map()
- forEach()

在这里常用的是后三个，我们只介绍后三个。

**filter()方法**

`filter()`函数，它利用指定的函数确定是否在返回的数组中包含某一项。例如要返回一个所有数值都大于2的数组，可以使用如下代码。

```arcade
var numbers = [1,2,3,4,5,4,3,2,1];
var filterResult = numbers.filter(function(item, index, array){
    return (item > 2);
});
alert(filterResult); //[3,4,5,4,3]
```

**map()方法**

`map()`方法也返回一个数组，而这个数组的每一项都是在原始数组中的对应项上运行输入函数的结果。例如，可以给数组中的每一项乘以2，然后返回这些乘积组成的数组，如下所示

```arcade
var numbers = [1,2,3,4,5,4,3,2,1];
var filterResult = numbers.map(function(item, index, array){
    return item * 2;
});
alert(filterResult); //[2,4,6,8,10,8,6,4,2]
```

**forEach()方法**

它只是对数组中的每一项运行传入的函数。这个方法没有返回值， 本质上与使用 for 循环迭代数组一样。来看一个例子 。

```delphi
//执行某些操作 10
var numbers = [1,2,3,4,5,4,3,2,1];
numbers.forEach(function(item, index, array){
});
```

这些数组方法通过执行不同的操作，可以大大方便处理数组的任务

##### String

String 类型的每个实例都有一个 length 属性，表示字符串中包含多个字符

```abnf
var stringValue = "hello world"; 
alert(stringValue.length);     //"11"
```

这个例子输出了字符串"hello world"中的字符数量，即"11"

###### 1.字符方法

两个用于访问字符串中特定字符的方法是:`charAt()`和 `charCodeAt()`。这两个方法都接收一个 参数，即基于 0 的字符位置。其中，`charAt()`方法以单字符字符串的形式返回给定位置的那个字符 (ECMAScript 中没有字符类型)。

例如:

```abnf
var stringValue = "hello world"; 
alert(stringValue.charAt(1));   //"e"	
```

字符串`"hello world"`位置 1 处的字符是"e"，因此调用 `charAt(1)`就返回了"e"。如果你想得到 的不是字符而是字符编码，那么就要像下面这样使用 charCodeAt()了。

```abnf
var stringValue = "hello world";
alert(stringValue.charCodeAt(1)); //输出"101" 
```

这个例子输出的是"101"，也就是小写字母"e"的字符编码。

ECMAScript 5 还定义了另一个访问个别字符的方法。在支持此方法的浏览器中，可以使用方括号加数 字索引来访问字符串中的特定字符，如下面的例子所示。

```abnf
var stringValue = "hello world";
alert(stringValue[1]);   //"e"
```

###### 2.字符串操作方法

下面介绍与操作字符串有关的几个方法。

**concat()**

用于将一或多个字符串拼接起来， 7 返回拼接得到的新字符串。先来看一个例子。

```awk
var stringValue = "hello ";
var result = stringValue.concat("world"); alert(result); //"hello world" alert(stringValue); //"hello" 
```

在这个例子中，通过 stringValue 调用 `concat()`方法返回的结果是"hello world"——但 stringValue 的值则保持不变。实际上，concat()方法可以接受任意多个参数，也就是说可以通过它来拼接任意多个字符串。再看一个例子:

```awk
var stringValue = "hello ";
var result = stringValue.concat("world", "!");
alert(result); //"hello world!" 
alert(stringValue); //"hello" 
```

这个例子将"world"和"!"拼接到了"hello"的末尾。虽然 `concat()`是专门用来拼接字符串的方法，但实践中使用更多的还是加号操作符(+)。而且，**使用加号操作符在大多数情况下都比使用 concat() 方法要简便易行(特别是在拼接多个字符串的情况下)。**

ECMAScript 还提供了三个基于子字符串创建新字符串的方法:`slice()`、`substr()`和 `substring()`。 这三个方法都会返回被操作字符串的一个子字符串，而且也都接受一或两个参数。第一个参数指定字符串的开始位置，第二个参数(在指定的情况下)表示字符串到哪里结束。

具体来说，`slice()`和 `substring()`的第二个参数指定的是字符串最后一个字符后面的位置。

而 `substr()`的第二个参数指定的则是返回的字符个数。如果没有给这些方法传递第二个参数，则将字符串的长度作为结束位置。

与 concat()方法一样，slice()、substr()和 substring()也不会修改字符串本身的值——它们只是 返回一个基本类型的字符串值，对原始字符串没有任何影响。请看下面的例子。

```awk
ar stringValue = "hello world";
alert(stringValue.slice(3));//"lo world"
alert(stringValue.substring(3));//"lo world"
alert(stringValue.substr(3));//"lo world"
alert(stringValue.slice(3, 7));//"lo w"
alert(stringValue.substring(3,7));//"lo w"
alert(stringValue.substr(3, 7));//"lo worl"
```

这个例子比较了以相同方式调用 `slice()`、`substr()`和 `substring()`得到的结果，而且多数情 况下的结果是相同的。在只指定一个参数3的情况下，这三个方法都返回`"lo world"`，因为`"hello" `中的第二个`"l"`处于位置 3。而在指定两个参数 3 和 7 的情况下，`slice()`和 `substring()`返回`"lo w"` ("world"中的"o"处于位置 7，因此结果中不包含"o")，但 `substr()`返回`"lo worl"`，因为它的第二 个参数指定的是要返回的字符个数。

在传递给这些方法的参数是负值的情况下，它们的行为就不尽相同了。其中，slice()方法会将传 入的负值与字符串的长度相加，substr()方法将负的第一个参数加上字符串的长度，而将负的第二个 参数转换为 0。最后，substring()方法会把所有负值参数都转换为 0。下面来看例子

```awk
var stringValue = "hello world";
alert(stringValue.slice(-3));//"rld" 
alert(stringValue.substring(-3));//"hello world"
alert(stringValue.substr(-3)); //"rld"
alert(stringValue.slice(3, -4));//"lo w" 
alert(stringValue.substring(3, -4));//"hel"
alert(stringValue.substr(3, -4)); //""(空字符串)
```

这个例子清晰地展示了上述三个方法之间的不同行为。在给 slice()和 substr()传递一个负值 参数时，它们的行为相同。这是因为-3 会被转换为 8(字符串长度加参数 11+(3)=8)，实际上相当 于调用了 slice(8)和 substr(8)。但 substring()方法则返回了全部字符串，因为它将-3 转换 成了 0。

当第二个参数是负值时，这三个方法的行为各不相同。slice()方法会把第二个参数转换为 7，这 就相当于调用了 slice(3,7)，因此返回"lo w"。substring()方法会把第二个参数转换为 0，使调 用变成了 substring(3,0)，而由于这个方法会将较小的数作为开始位置，将较大的数作为结束位置， 因此最终相当于调用了 substring(0,3)。substr()也会将第二个参数转换为 0，这也就意味着返回 包含零个字符的字符串，也就是一个空字符串。

###### 4.字符串位置方法

有两个可以从字符串中查找子字符串的方法:`indexOf()`和 `lastIndexOf()`。这两个方法都是从 一个字符串中搜索给定的子字符串，然后返回子字符串的位置(如果没有找到该子字符串，则返回-1)。 这两个方法的区别在于:`indexOf()`方法从字符串的开头向后搜索子字符串，而 `lastIndexOf()`方法 是从字符串的末尾向前搜索子字符串

**indexOf()和lastIndexOf()**

```awk
var stringValue = "hello world";
alert(stringValue.indexOf("o"));             //4
alert(stringValue.lastIndexOf("o"));         //7
alert(stringValue.indexOf("o", 6));         //7
alert(stringValue.lastIndexOf("o", 6));     //4
```

###### 4.trim()方法

ECMAScript 5 为所有字符串定义了 trim()方法,删除字符串的前后空格.

```abnf
var stringValue = "   hello world   ";
var trimmedStringValue = stringValue.trim();
alert(stringValue);            //"   hello world   "
alert(trimmedStringValue);     //"hello world"
```

###### 5.字符串大小写转换方法

ECMAScript 中涉及字符串大小写转换的方法有四个.`toLowerCase()`、`toLocaleLowerCase()`、`toUpperCase()`和 `toLocaleUpperCase()`。 其中，`toLowerCase()`和 `toUpperCase()`是两个经典的方法 ,而 toLocaleLowerCase()和 toLocaleUpperCase()方法则是针对特定地区的实现。对有些地 区来说，针对地区的方法与其通用方法得到的结果相同，但少数语言(如土耳其语)会为 Unicode 大小 写转换应用特殊的规则，这时候就必须使用针对地区的方法来保证实现正确的转换.

```awk
var stringValue = "hello world";
alert(stringValue.toLocaleUpperCase());  //"HELLO WORLD"
alert(stringValue.toUpperCase());        //"HELLO WORLD"
alert(stringValue.toLocaleLowerCase());  //"hello world"
alert(stringValue.toLowerCase());        //"hello world"
```

##### Date日期

ECMAScript 中的 Date 类型是在早期 Java 中的 java.util.Date 类基础上构建的。为此，Date 类型使用自 UTC(Coordinated Universal Time，国际协调时间)1970 年 1 月 1 日午夜(零时)开始经过 的毫秒数来保存日期。在使用这种数据存储格式的条件下，Date 类型保存的日期能够精确到 1970 年 1 月 1 日之前或之后的 285 616 年。

要创建一个日期对象，使用 new 操作符和 Date 构造函数即可，如下所示

```arcade
var now = new Date([parameters]);
```

前边的语法中的参数（parameters）可以是一下任何一种：

- 无参数 : 创建今天的日期和时间，例如： `var today = new Date();`.
- 一个符合以下格式的表示日期的字符串: "月 日, 年 时:分:秒." 例如： `var Xmas95 = new Date("December 25, 1995 13:30:00")。`如果你省略时、分、秒，那么他们的值将被设置为0。
- 一个年，月，日的整型值的集合，例如： `var Xmas95 = new Date(1995, 11, 25)。`
- 一个年，月，日，时，分，秒的集合，例如： `var Xmas95 = new Date(1995, 11, 25, 9, 30, 0);`.

###### Date对象的方法

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014092757012-437264570.png)

###### 日期格式化方法

Date 类型还有一些专门用于将日期格式化为字符串的方法，这些方法如下。

- toDateString()——以特定于实现的格式显示星期几、月、日和年;
- toTimeString()——以特定于实现的格式显示时、分、秒和时区;
- toLocaleDateString()——以特定于地区的格式显示星期几、月、日和年;
- toLocaleTimeString()——以特定于实现的格式显示时、分、秒;
- toUTCString()——以特定于实现的格式完整的 UTC 日期。

```arcade
var myDate = new Date();
```

**toDateString()**

```awk
myDate.toDateString();//"Mon Apr 15 2019"
```

**toTimeString()**

```awk
myDate.toTimeString();//"10:11:53 GMT+0800 (中国标准时间)"
```

**toLocaleDateString()**

```awk
myDate.toLocaleDateString();//"2019/4/15"
```

**toLocaleTimeString()**

```awk
myDate.toLocaleTimeString();//"上午10:11:53"
```

**toUTCString()**

```awk
myDate.toUTCString();//"Mon, 15 Apr 2019 02:11:53 GMT"
```

**栗子：返回了用数字时钟格式的时间**

```arcade
var time = new Date();
var hour = time.getHours();
var minute = time.getMinutes();
var second = time.getSeconds();
var temp = "" + ((hour > 12) ? hour - 12 : hour);
if (hour == 0)
    temp = "12";
temp += ((minute < 10) ? ":0" : ":") + minute;
temp += ((second < 10) ? ":0" : ":") + second;
temp += (hour >= 12) ? " P.M." : " A.M.";
alert(temp);
```

##### 字符串和数值之间转换

###### 字符串转数值

```arcade
var str = '123.0000111';
console.log(parseInt(str));
console.log(typeof parseInt(str));
console.log(parseFloat(str));
console.log(typeof parseFloat(str));
console.log(Number(str));
```

###### 数值转字符串

```arcade
var num  = 1233.006;
// 强制类型转换
console.log(String(num));
console.log(num.toString());

// 隐式转换
console.log(''.concat(num));

// toFixed()方法会按照指定的小数位返回数值的字符串 四舍五入
console.log(num.toFixed(2));
```

##### Globle对象

Global(全局)对象可以说是 ECMAScript 中最特别的一个对象了，因为不管你从什么角度上看， 这个对象都是不存在的。ECMAScript 中的 Global 对象在某种意义上是作为一个终极的“兜底儿对象” 来定义的。换句话说，不属于任何其他对象的属性和方法，最终都是它的属性和方法。事实上，没有全 局变量或全局函数; 所有在全局作用域中定义的属性和函数，都是 Global 对象的属性。本书前面介绍 过的那些函数，诸如 isNaN()、isFinite()、parseInt()以及 parseFloat()，实际上全都是 Global 对象的方法。除此之外，Global 对象还包含其他一些方法。

###### URI 编码方法

Global 对象的 `encodeURI()`和 `encodeURIComponent()`方法可以对 URI(Uniform Resource Identifiers，通用资源标识符)进行编码，以便发送给浏览器。有效的 URI 中不能包含某些字符，例如 5 空格。而这两个 URI 编码方法就可以对 URI 进行编码，它们用特殊的 UTF-8 编码替换所有无效的字符， 从而让浏览器能够接受和理解。

其中，`encodeURI()`主要用于整个 URI(例如，[http://www.apeland/web index.html](http://www.apeland/web index.html))，而 `encodeURIComponent()`主要用于对 URI 中的某一段(例如前面 URI 中的 web index.html)进行编码。 它们的主要区别在于，`encodeURI()`不会对本身属于 URI 的特殊字符进行编码，例如冒号、正斜杠、 问号和井字号;而 `encodeURIComponent()`则会对它发现的任何非标准字符进行编码 ,看个例子

```qml
var url = `http://www.apeland/web index.html`;
console.log(encodeURI(url));//http://www.apeland/web%20index.html
console.log(encodeURIComponent(url));//http%3A%2F%2Fwww.apeland%2Fweb%20index.html
```

使用 encodeURI()编码后的结果是除了空格之外的其他字符都原封不动，只有空格被替换成了 %20。而 encodeURIComponent()方法则会使用对应的编码替换所有非字母数字字符

> 一般来说，我们使用 encodeURIComponent()方法的时候要比使用 encodeURI()更多，因为在实践中更常见的是对查询字符串参数而不是对基础 URI 进行编码。

与 `encodeURI()`和 `encodeURIComponent()`方法对应的两个方法分别是 `decodeURI()`和 `decodeURIComponent()`。其中，`decodeURI()`只能对使用 `encodeURI()`替换的字符进行解码。例如， 它可将%20 替换成一个空格，但不会对%23 作任何处理，因为%23 表示井字号(#)，而井字号不是使用 `encodeURI()`替换的。同样地，`decodeURIComponent()`能够解码使用 `encodeURIComponent()`编码

##### window对象

ECMAScript 虽然没有指出如何直接访问 Global 对象，但 Web 浏览器都是将这个全局对象作为 window 对象的一部分加以实现的。因此，在全局作用域中声明的所有变量和函数，就都成为了 window 对象的属性。来看下面的例子。

```qml
var color = "red";
function sayColor(){
    alert(window.color);
}
window.sayColor();  //"red"
```

##### Math对象

ECMAScript 还为保存数学公式和信息提供了一个公共位置，即 Math 对象.与我们在 JavaScript 直 接编写的计算功能相比，Math 对象提供的计算功能执行起来要快得多。Math 对象中还提供了辅助完成 这些计算的属性和方法。

###### 1.Math 对象的属性

Math 对象包含的属性大都是数学计算中可能会用到的一些特殊值。下表列出了这些属性

| 属性         | 说明                           |
| ------------ | ------------------------------ |
| Math.E       | 自然对数的底数，即常量e的值    |
| Math.LN10    | 10的自然对数 ln(10)            |
| Math.LN2     | 2的自然对数                    |
| Math.LOG2E   | 以2为底e的对数                 |
| Math.LOG10E  | 以10为底e的对数                |
| Math.PI      | π的值                          |
| Math.SQRT1_2 | 1/2的平方根(即2的平方根的倒数) |
| Math.SQRT2   | 2的平方根                      |

###### 2.min()和 max()方法

Math 对象还包含许多方法，用于辅助完成简单和复杂的数学计算。

其中，min()和 max()方法用于确定一组数值中的最小值和最大值。这两个方法都可以接收任意多 个数值参数，如下面的例子所示

```apache
var max = Math.max(3, 54, 32, 16);
alert(max);    //54
var min = Math.min(3, 54, 32, 16);
alert(min);    //3
```

这两个方法经常用于避免多 余的循环和在 if 语句中确定一组数的最大值。

**例子:**

要找到数组中最大或最小值,可以像下面这样用`apply()`方法

```maxima
var values = [1,2,36,23,43,3,41];
var max = Math.max.apply(null, values);
```

这个技巧的关键是把 Math 对象作为 apply()的第一个参数，从而正确地设置 this 值。然后，可以将任何数组作为第二个参数。

###### 3.舍入方法

将小数值舍入为整数的几个方法:`Math.ceil()`、`Math.floor()`和` Math.round()`。 这三个方法分别遵循下列舍入规则:

- `Math.ceil()`执行向上舍入，即它总是将数值向上舍入为最接近的整数;
- `Math.floor()`执行向下舍入，即它总是将数值向下舍入为最接近的整数;
- `Math.round()`执行标准舍入，即它总是将数值四舍五入为最接近的整数(这也是我们在数学课上学到的舍入规则)。

```arcade
var num = 25.7;
var num2 = 25.2;
alert(Math.ceil(num));//26
alert(Math.floor(num));//25
alert(Math.round(num));//26
alert(Math.round(num2));//25
```

###### 4.random()方法

`Math.random()`方法返回大于等于 0 小于 1 的一个随机数

例子1:**获取min到max的范围的整数**

```arcade
function random(lower, upper) {
	return Math.floor(Math.random() * (upper - lower)) + lower;
}
```

例子2: **获取随机颜色**

```arcade
/**
 * 产生一个随机的rgb颜色
 * @return {String}  返回颜色rgb值字符串内容，如：rgb(201, 57, 96)
 */
function randomColor() {
	// 随机生成 rgb 值，每个颜色值在 0 - 255 之间
	var r = random(0, 256),
		g = random(0, 256),
		b = random(0, 256);
	// 连接字符串的结果
	var result = "rgb("+ r +","+ g +","+ b +")";
	// 返回结果
	return result;
}
```

例子3: **获取随机验证码**

```arcade
function createCode(){
    //首先默认code为空字符串
    var code = '';
    //设置长度，这里看需求，我这里设置了4
    var codeLength = 4;
    //设置随机字符
    var random = new Array(0,1,2,3,4,5,6,7,8,9,'A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R', 'S','T','U','V','W','X','Y','Z');
    //循环codeLength 我设置的4就是循环4次
    for(var i = 0; i < codeLength; i++){
        //设置随机数范围,这设置为0 ~ 36
        var index = Math.floor(Math.random()*36);
        //字符串拼接 将每次随机的字符 进行拼接
        code += random[index]; 
    }
    //将拼接好的字符串赋值给展示的Value
    return code
}
```