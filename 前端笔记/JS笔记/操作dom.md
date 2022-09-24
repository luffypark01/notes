# JavaScript的DOM操作

### DOM介绍

#### 1.文档：DOM中的"D"

 DOM是"**Document Object Model**"(文档对象模型)的首字母缩写。如果没有document，DOM也就无从谈起。当创建一个网页并把它加载到web浏览器中时，DOM就在幕后悄然而生。它将根据你编写的网页文档创建一个文档对象。

 在我们自己的语言中，”对象“这个词的含义往往不那么明确和具体，它几乎可以称呼任何一种客观存在的事物。但在程序设计语言中，”对象“这个词的含义非常明确和具体。

#### 2.对象：DOM中的”O“

对象是一种独立的数据集合。与某个特定对象相关联的变量被称为这个对象的属性；可以通过某个特定对象去调用的函数被称为这个对象的方法。

Javascript语言中的对象可以分为三种类型：

1. 用户定义对象（user-defined object）：由程序员自行创建的对象。
2. 内建对象（native object）:内建的Javascript语言中的对象，如Array、Math和Date等。
3. 宿主对象（host object）:由浏览器提供的对象，比如window对象

#### 3.模型：DOM中的“M”

 DOM中的“M”代表着“Model”(模型)，但说它代表着“Map(地图)”也是可以的。模型也好，地图也罢，它们的含义都是某种事物的表现形式。就像一个模型火车代表着一列真正的火车、一张城市街道图代表着一个世纪存在的城市那样，DOM代表着被加载到浏览器窗口里的当前网页：浏览器由我们提供了当前的地图（或者说模型），而我们可以通过JavaScript去读取这张地图。

 既然是地图，就必须有诸如方向、等高线和比例尺之类的记号。要想看懂和使用地图，就必须知道这个记号的含义和用途——这个道理同样适用于DOM。要想从DOM获取信息，我们必须先把各种用来表示和描述一份文档的记号弄明白。

 DOM把一份文档表示为一颗树（这里所说的“树“是数学意义上的概念），这是我们理解和运用这一模型的关键。更具体的说，DOM把文档表示一颗家谱树。

 家谱数本身又是一种模型。家谱树的典型用法是表示一个人类家族的谱系关系并使用parent(父)、child(子)、sibling(兄弟)等记号来表明家族成员之间的关系。家谱树可以把一些相当复杂的关系简明表示出来：一位特定的成员既是某些成员的父辈，又是另一位成员的子辈，同时还是另一位成员的兄弟。

 类似于人类家族谱系的情况，家谱树模型也非常适用用来表示一份用HTML语言编写出来的文档。

```xml
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>class list</title>
</head>
<body>
	<h2>你要买什么课程？</h2>
	<p title='请您选择购买的课程'>本课程是web全栈课程，期待你的购买！</p>
	<ul id='classList'>
		<li class='item'>JavaScript</li>
		<li class='item'>css</li>
		<li>DOM</li>
	</ul>
</body>
</html>
```

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094408137-601890453.png)

 有此图可以看出来该家谱数以html为根元素的。毫无疑问，html元素完全可以代表整个文档。

 我们会发现`<head>`和`<body>`两个分枝。他们存在于同一个层次且互不包含，所以他们是兄弟关系。他们有着共同的父元素`<html>`,但又各有各的子元素，所以它们本身又是其它一些元素的父元素。

`<head>`元素由两个子元素：`<meta>`和`<title>`（这两个元素是兄弟关系）。`<body>`元素由三个子元素：`<h1>`、`<p>`、`<ul>`(这三个元素是兄弟关系)。如果继续深入下去，我们将发现`<ul>`也是一个父元素。他有三个子元素，他们都是`<li>`元素。

 利用这种简单的家谱关系记号，我们可以把各元素之间的关系简明清晰地表达出来。

 如果把各种文档元素都想象成一颗家谱树的各个节点的话，我们就可以用同样的记号来描述DOM。不过，与使用”家谱树“这个术语相比，把一份文档称为一颗“节点树”更准确。

#### 节点

 节点（node）这个名词来自网络理论，它代表着网络中的一个连接点。网络是有节点构成的集合。

 在现实世界里，一切事物都是有原子构成。原子是现实世界的节点。但原子本身还可以进一步分解为更细小的亚原子微粒。这些亚原子微粒同样是节点。

 DOM也是同样的情况。文档也是有节点构成的集合，只不过此时的节点是文档树上的树枝和树叶而已。

 在DOM里存在着许多不同类型的节点。就像原子包含着亚原子微粒那样，有些DOM节点还包含着其他类型的节点。

##### 1.元素节点

在刚才上述的“课程列表”的文档时，像`<body>`、`<p>`、`<ul>`之类的元素这些我们都称为叫元素节点（element node）。如果把web上的文档比作一座楼，元素就是建造这座楼的砖块，这些元素在文档中的布局形成了文档的结构。

 各种标签提供了元素的名字。文本段落元素的名字是"p"，无序列表元素的名字是“ul”,列表项元素的名字是“li”

##### 2.文本节点

元素只是不同节点类型中的一种。如果一份文档完全由一些空白元素构成，它将有一个结构，但这份文档本身将不会包含什么内容。在网上，内容决定着一切，**没有内容的文档是没有任何价值的，而绝大数内容都是有文本提供**。

<p title='请您选择购买的课程'>本课程是web全栈课程，期待你的购买！</p>。由此可见，<p>元素包含着文本"本课程是web全栈课程，期待你的购买！"。它是一个文本节点(text node)

##### 3.属性节点

还存在着其他的一些节点类型。例如，注释就是另外一种节点类型。但这里我们介绍的是属性节点。

 元素都或多或少的有一些属性，属性的作用是对元素做出更具体的描述。例如，几乎所有的元素都有一个title属性，而我们可以利用这个尚需经对包含在元素里的东西做出准确的描述：

```xml
<p title='请您选择购买的课程'>本课程是web全栈课程，期待你的购买！</p>
```

在DOM中，`title='请您选择购买的课程'`是一个属性节点（attribute node）。如图所示。因为属性总是被放在起始标签里，所以属性节点总是被包含在元素节点当中。并非所属地额元素都包含着属性，但所有的属性都会被包含在元素里。

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094424319-658938069.png)

#### 获取元素节点的方式

##### 1.getElementById()方法

DOM提供了一个名为`getElemntById()`的方法，这个方法将返回一个与那个给定id属性值的元素节点相对应的对象。这个方法是与document对象相关联的函数。``getElemntById()`方法只有一个参数：你想获得的那个元素的id属性值，这个id值必须是字符串类型。

语法：

```reasonml
document.getElementById(id)
```

例子：

```dart
var oUl = document.getElementById('classList');
```

这个调用之后返回一个对象，这个对象对应着和docuemnt对象里的一个独一无二的元素，那个元素的HTML id属性值是 `classList`。

```arcade
console.log(typeof oUl);//object
```

**文档中的每一个元素都对应着一个对象。**利用DOM提供的方法，我们可以把与这些元素怒相对应的任何一个对象筛选出来。

##### 2.getElementsByTagName()方法

这个方法将返回一个**元素对象集合**。

语法：

```reasonml
document.getElementsByTagName(localName: DOMString)
```

例子：

```dart
var listItems = document.getElementsByTagName('li')
```

结果：

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094438133-907231937.png)

这个元素集合中的每个元素都是一个对象。我们可以通过`listItems.length`来获取当前集合的长度，通过`for`循环来遍历出每一项的元素对象。

> 请注意：即使在整个文档里只有一个元素有给定的标签名，getElementsByTagName()方法也将返回一个元素集合。此时，它的长度为1。

##### 3.getElementsByClassName()方法

 我们在构建网页的时候会存在相同或更多的li标签，如果用`getElementsByTagName()`方法获取到再筛选无疑是增加工作效率。在之前我们用到了class将对应的某些元素进行归类，同样情况下,DOM中也提供了通过类名来获取某些DOM元素对象的方法`getElementBysClassName()`.

语法：

```reasonml
document.getElementsByClassName(classNames: DOMString)
```

例子：

```dart
var classItems = document.getElementsByClassName('item');
```

结果：

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094457721-1733495335.png)

由此可见，它获取出来的也是元素对象集合。只不过通过类名设置之后，只获取出来一部分的元素对象。

#### 加油干，你可以的

我们可以通过上述讲解的三种方式来找到对应的元素，找到元素后，我们就可以利用`getAttribute()`方法和`setAttribute()`方法对元素的属性值进行操作

##### 1.getAttrbute()方法

getAttribute()方法只接收一个参数——你打算查询的属性的名字。

语法：

```reasonml
object.getAttribute(attributeName: DOMString)
```

比如上述案例，**如何将p标签中的title属性打印出来**？

```arcade
var oP = document.getElementsByTagName('p')[0];
var title = oP.getAttribute('title');
console.log(title);
```

结果：

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094510897-985929035.png)

如果查询不到结果，则该调用则返回null，null是JavaScript语言中的空值，其含义是’你说的这个东西不存在‘

##### 2.setAttribute()方法

`setAttrbute()`方法它允许我们对属性节点的值做出修改。此方法传递两个参数。第一个参数为**属性名**，第二个参数为**属性值**

语法：

```reasonml
object.setAttribute(attribute,value)
```

例如:

```dart
var classList = document.getElementById('classList');
classList.setAttribute('title','这是我们最新的课程');
```

效果：

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094544329-1567388853.png)

同样的情况，我们也可以通过setAttribute()方法对上述的p标签的title属性进行修改。

 这里有一个非常值得关注的细节：通过setAttribute()方法对文档做出的修改，如果我们查看源代码发现看到的依旧是原来的属性值——也就是说，setAttribute()方法做出的修改不会反映为文档本身的源码里。这种“表里不一”的现象源自于DOM的工作模式：**先加载文档的静态内容、再以动态方式对他们进行刷新，动态刷新不影响文档的静态内容。**这正是DOM的真正威力和诱人之处：对页面内容的刷新不需要最终用户在他们的浏览器里执行页面刷新操作就可以实现。

#### 节点属性

在文档对象模型(DOM)中，每一个节点都是一个对象。DOM节点有三个重要的属性：

1. nodeName: 节点的名称
2. nodeValue：节点的值
3. nodeType: 节点的类型

##### **1.nodeName属性：节点的名称，是只读**

1. 元素节点的nodeName与标签名相同
2. 属性节点的nodeName与属性的名称相同
3. 文本节点的nodeName永远是#text
4. 文档节点的nodeName永远是#document

##### **2.nodeValue属性：节点的值**

1. 元素节点的 nodeValue 是 undefined 或 null
2. 文本节点的 nodeValue 是文本自身
3. 属性节点的 nodeValue 是属性的值

##### **3.nodeType 属性:** **节点的类型，是只读的。**

**以下常用的几种结点类型:**

| 元素类型 | 节点类型 |
| -------- | -------- |
| 元素     | 1        |
| 属性     | 2        |
| 文本     | 3        |
| 注释     | 8        |
| 文档     | 9        |

**验证**

```xml
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>节点属性</title>
</head>
<body>
	
	<div id="box">我是一个文本节点<!-- 我是注释 --></div>
	<script type="text/javascript">
		// 1.获取元素节点
		var divNode = document.getElementById('box');
		console.log(divNode.nodeName + '|' + divNode.nodeValue + "/" + divNode.nodeType);
		// 2.获取属性节点
		var attrNode = divNode.attributes[0];
		console.log(attrNode.nodeName + '|' + attrNode.nodeValue + "/" + attrNode.nodeType);
		// 3.获取文本节点
		var textNode = divNode.childNodes[0];
		console.log(textNode.nodeName + '|' + textNode.nodeValue + "/" + textNode.nodeType);
		// 4.获取注释节点
		var commentNode = divNode.childNodes[1];
		console.log(commentNode.nodeName + '|' + commentNode.nodeValue + "/" + commentNode.nodeType);
		// 5.文档节点
		console.log(document.nodeName + '|' + document.nodeValue + "/" + document.nodeType);
		
	</script>
	
</body>
</html>
```

- `attributes`属性是获取到该节点对象上的所有属性的集合
- `childNodes`属性是获取到该节点对象的所有子节点的集合

效果显示：

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094558953-1902471326.png)

##### childNodes属性

访问选定元素节点下的所有子节点的列表，返回的值可以看作是一个数组，他具有length属性。

语法：

```stylus
elementNode.childNodes
```

注意：

如果选定节点没有子节点，则该属性返回不包含节点的NodeList

##### firstChild属性

`firstChild`属性返回`childNodes`数组中的第一个子节点。如果选定的节点没有子节点，则该属性返回NULL。

语法：

```stylus
node.firstChild
```

> 与elementNode.childNodes[0]是同样的效果

##### lastChild属性

`lastChild`属性返回`childNodes`数组中的最后一个子节点。如果选定的节点没有子节点，则该属性返回NULL。

语法：

```stylus
node.lastChild
```

> 与elementNode.childNodes[elementNode.childNodes.length-1]是同样的效果

##### parentNode属性

**1.获取指定节点的父节点**

语法：

```stylus
elementNode.parentNode
```

注意：父节点只能有一个

**2.访问祖先节点**

```stylus
elementNode.parentNode.parentNode
```

**3.访问兄弟节点**

nextSiblings属性可返回某个**节点之后**紧跟的节点,如果无此节点，则该属性返回null

语法：

```stylus
nodeObject.nextSibling
```

previousSibling()属性可返回**节点之前**紧跟的节点，如果无此节点，则该属性返回null

语法：

```stylus
nodeObject.previousSibling
```

以上两个方法，在谷歌浏览器上获取出来的是空白文本节点（例如，换行符号）。

**解决问题方法**

判断节点的nodeType是否为1，如果是为元素节点，跳过

```arcade
function get_nextSibling(n){
    var x = n.nextSibling;
    while (x && x.nodeType!=1){
        x = x.nextSibling;
    }
    return x;
}
var paraNode = document.getElementById('p1');
console.log(get_nextSibling(paraNode).nodeName);//UL
```

#### 节点方法

##### 1.创建节点createElement()

`createElement()`方法可创建元素节点。此方法可返回一个Element对象

语法：

```haxe
var newNode = document.createElement(tagName);
```

参数：

tagName:字符传值，这个字符串用来指明创建元素的类型

例子：

```haxe
var newNode = document.createElement('p');
console.log(newNode.nodeName);//P
```

仅仅是创建了一个新的p元素节点，如何来设置该标签的文本内容呢？

注意：此文本内容，p标签既可以是纯文本，也可以在p标签中包含一些其它标签的文本。

**innerText属性**

仅仅对元素来获取或者设置该文本

```haxe
newNode.innerText = 'mjj';
console.log(newNode);//<p>mjj</p>
```

如果像在p标签插入一段`<a>mjj</a>`，会发现`<a>mjj</a>`它也会充当文本的来进行设置。

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094611888-1442517083.png)

**innerHTML属性**

既可以设置文本又可以设置标签

```haxe
newNode.innerHTML = '<a>mjj</a>'
```

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094622722-1164152709.png)

> 注意：如果想获取节点对象的文本内容，可以直接调用innerText或者innerHTML属性来获取

如何将创建的新节点对象插入到我们指定的节点位置呢？下节知晓

##### 2.插入节点appendChild()

在指定的节点的最后一个子节点之后添加一个新的子节点

语法：

```haxe
appendChild(newNode);
```

参数：

newNode：指定追加的节点

##### 3.插入节点insertBefore()

`insertBefore()`方法可在已有的子节点前插入一个新的子节点

语法：

```reasonml
insertBefore(newNode,node);
```

参数：

newNode:要插入的新节点

node:指定此节点前插入节点

```xml
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>节点的方法</title>
</head>
<body>
	<div id="box">
		<p>html</p>
	</div>
	<script type="text/javascript">
		var oBox = document.getElementById('box');
		var newNode  = document.createElement('p');
		newNode.innerHTML= '<a>JavaScript</a>';
		oBox.insertBefore(newNode2,newNode);
        //等价于oBox.insertBefore(newNode2,oBox.firstChild);
	</script>
	
</body>
</html>
```

##### 4.删除节点removeChild()

removeChild()方法从子节点列表中删除某个节点。如果删除成功，此方法可返回被删除的节点，如失败，则返回NULL

语法：

```crmsh
nodeObject.removeChild(node);
```

参数：

node：必需的，指定要删除的节点

例子：

```xml
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>节点的方法</title>
</head>
<body>
	<div id="box"><p>html</p></div>
	<script type="text/javascript">
		var oBox = document.getElementById('box');
		var x = oBox.removeChild(oBox.childNodes[0]);
		console.log(x);
	</script>
	
</body>
</html>
```

注意：把删除的子节点赋值给 x，这个子节点不在DOM树中，但是还存在内存中，可通过 x =null来删除对象。

##### 5.替换元素节点replaceChild()

replaceChild实现子节点(对象)的替换。返回被替换对象的引用

语法：

```haxe
node.replaceChild(newnode,oldnew);
```

参数：

newnode：必需，用于替换的oldnew的对象

oldnew：必需，被newnode替换的对象

例子：

```haxe
var oldnew = document.getElementById('text');
var newNode  = document.createElement('p');
newNode.innerHTML = 'Vue';
oldnew.parentNode.replaceChild(newNode,oldnew);
```

##### 6.创建文本节点createTextNode

createTextNode()方法创建新的文本节点，返回新创建的Text节点

语法：

```haskell
document.createTextNode(data);
```

参数：

data: 字符串值，可规定此节点的文本

例子：创建一个div并向其中添加一条消息

```haxe
var newNode = document.createElement('div');
var textNode  = document.createTextNode('我是一条文本信息');
newNode.appendChild(textNode);
document.body.appendChild(newNode);
```

#### 样式设置

通过JavaScript以不同的方式来操作CSS样式，我们可以通过该节点对象的style属性来实现。

##### 1.动态设置样式

直接在想要动态设置样式的元素内部添加内联样式。这是用HTMLElement.style属性来实现。这个属性包含了文档中每个元素的内联样式信息。你可以设置这个对象的属性直接修改元素样式

例子：

```xml
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>操作样式</title>
</head>
<body>
	<p id = 'box'>MJJ</p>
	<script type="text/javascript">
		var para = document.getElementById('box');
		para.style.color = 'white';
		para.style.backgroundColor = 'black';
		para.style.padding = '10px';
		para.style.width = '250px';
		para.style.textAlign = 'center';
	</script>
	
</body>
</html>
```

效果显示：

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094647069-945805054.png)

> CSS样式的JavaSript属性版本以小驼峰式命名法书写，而CSS版本带连接符号（`backgroundColor` 对 `background-color`）

##### 2.操作属性的类来控制样式

- 首先在HTML的`<head>`中添加下列代码

  ```xml
  <style type="text/css">
      .highlight {
          color: white;
          background-color: black;
          padding: 10px;
          width: 250px;
          text-align: center;
      }
  </style>
  ```

- 现在我们改为使用HTML操作的常用方法 — [`Element.setAttribute()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/setAttribute) — 这里有两个参数，你想在元素上设置的属性，你要为它设置的值。在这种情况下，我们在段落中设置类名为highlight：

  ```dart
  var para = document.getElementById('box');
  para.setAttribute('class','highlight');
  ```

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014094657767-1145559115.png)

两种方式各有优缺点，选择哪种取决于你自己。第一种方式无需安装，适合简单应用，第二种方式更加正统（没有CSS和JavaScript的混合，没有内联样式，而这些被认为是不好的体验）。当你开始构建更大更具吸引力的应用时，你可能会更多地使用第二种方法，但这完全取决于你自己。

#### 事件

事件是您在编程时系统内发生的动作或者发生的事情，系统通过它来告诉您在您愿意的情况下您可以以某种方式对它做出回应。例如：如果您在网页上单击一个按钮，您可能想通过显示一个信息框来响应这个动作。

##### 一系列的事件

就像上面提到的, **事件**是您在编程时系统内发生的动作或者发生的事情— 系统会在事件出现的时候触发某种信号并且会提供一个自动加载某种动作（列如：运行一些代码）的机制，比如在一个机场，当一架将起飞的飞机的跑道清理完成后，飞行员会收到一个信号，结果是他们开始起飞。

在Web中, 事件在浏览器窗口中被触发并且通常被绑定到窗口内部的特定部分 — 可能是一个元素、一系列元素、被加载到这个窗口的 HTML 代码或者是整个浏览器窗口。举几个可能发生的不同事件：

- 用户在某个元素上点击鼠标或悬停光标。
- 用户在键盘中按下某个按键。
- 用户调整浏览器的大小或者关闭浏览器窗口。
- 一个网页停止加载。
- 提交表单。
- 播放、暂停、关闭视频。
- 发生错误

主要事件有：

| 事件        | 说明                 |
| ----------- | -------------------- |
| onclick     | 鼠标单击事件         |
| onmouseover | 鼠标经过事件         |
| onmouseout  | 鼠标移开事件         |
| onchange    | 文本框内容改变事件   |
| onselect    | 文本框内容被选中事件 |
| onfocus     | 光标聚焦事件         |
| onblur      | 光标失焦事件         |
| onload      | 网页加载事件         |

##### 鼠标单击事件(onclick)

onclick是鼠标单击事件，当您在网页上单击鼠标时，就会发生该事件。同时onclick事件调用的程序块也会被执行，通常与按钮一起使用。

比如，我们单击按钮时，触发onclick事件，并计算两个数的和。

```xml
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title></title>
        <script type="text/javascript">
            function add() {
                var a, b, sum;
                a = 5;
                b = 6;
                sum = a + b;
                alert('两个数的和为:'+ sum);
            }
        </script>
    </head>
    <body>
        <input type="button" value="提交" onclick="add();">
    </body>
</html>
```

##### 鼠标经过事件(onmouseover)&&鼠标移开事件(onmouseout)

鼠标经过事件，当鼠标移到一个元素上时，该对象就出发了onmouseover事件，并执行onmouseover事件调用的程序。

当鼠标移到一个元素上时，该对象就出发了onmouseout事件，并执行onmouseout事件调用的程序。

```xml
<!doctype html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css">
            .box{
                width: 200px;
                height: 200px;
                background-color: red;
            }
        </style>
        <script type="text/javascript">
            function over(){
                console.log('切换成绿色');
            }
            function out() {
                console.log('切换成红色');
            }
        </script>
    </head>
    <body>
        <div class="box" onmouseover="over();" onmouseout="out()"></div>
    </body>
</html>
```

##### 光标聚焦事件(onfocus)&&光标失焦事件(onblur)

当网页中的。元素获得焦点时，执行onfocus事件，并调用程序让程序执行。尤其是表单控件。onblur事件和onfocus是相对事件，当光标离开当前聚焦元素的时候，触发onblur事件，同时执行被调用的程序。

```xml
<!doctype html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <script type="text/javascript">
            function message() {
                console.log('请输入姓名');
            }
            function message2() {
                console.log('请正确输入您的用户名');
            }
        </script>
    </head>
    <body>
        <form action="">
            <p class="name">
                <label for="username">用户名:</label>
                <input type="text" id="username" onfocus="message();" onblur="message2();">
            </p>
            <p class="pwd">
                <label for="pwd">密码:</label>
                <input type="password" id="pwd">
            </p>
            <input type="submit" value="注册">
        </form>
    </body>
</html>
```

##### 内容选中事件(onselect)

当文本框或者文本域中的文字被选中时，触发onselect事件，同时带调用的程序就会被执行。

```xml
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script type="text/javascript">
            function select2() {
                console.log('内容被选中了');
            }
        </script>g
    </head>
    <body>
        <textarea name="" id="" cols="30" rows="10" onselect="select2();">请写入个人简介,字数不少于200字！</textarea>

    </body>
</html>
```

##### 文本框内容改变事件(onchange)

通过改变文本框的内容来触发onchange事件，同时执行被调用的程序。

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        function changetext() {
            console.log('内容改变了');
        }
    </script>
</head>
<body>
<input type="text" value="mjj" onchange="changetext();">
</body>
</html>
```

当改变用户文本框内的文字后，触发onchange事件。

##### 加载事件(onload)

事件会在页面加载完成后，立即发生，同时执行被调用的程序。

注意：加载页面时，触发onlaod事件，事件写在body标签内。

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        function message() {
            console.log('加载中，请稍后......');
        }
    </script>
</head>
<body onload="message();">

</body>
</html>
```