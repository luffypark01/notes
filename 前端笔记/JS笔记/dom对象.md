# JavaScript的BOM对象

## BOM

ECMAScript 是 JavaScript 的核心，但如果要在 Web 中使用 JavaScript，那么 BOM(浏览器对象模型)则无疑才是真正的核心。BOM 提供了很多对象，用于访问浏览器的功能，这些功能与任意网页内容无关。多年来，缺少事实上的规范导致 BOM 既有意思又有问题，因为浏览器提供商会按照各自的想法随意去扩展它。于是，浏览器之间共有的对象就成为了事实上的标准。这些对象在浏览器中得以存在，很大程度上是由于它们提供了与浏览器的互操作性。W3C 为了把浏览器中 JavaScript 最基本的部分标准化，已经将 BOM 的主要方面纳入了 HTML5 的规范中。

### window对象

BOM 的核心对象是 window，它表示浏览器的一个实例。在浏览器中，window 对象有双重角色， 它既是通过 JavaScript 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象。这意味着 在网页中定义的任何一个对象、变量和函数，都以 window 作为其 Global 对象，因此有权访问 `parseInt()`等方法。

#### 系统对话框方法

**警告框**

```dart
window.alert('mjj');
```

效果显示

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014093742549-1002386572.png)

**确认框**

```arcade
var a = window.confirm('你确定要离开网站?');
console.log(a);
```

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014093757704-1158731376.png)

如果点击确定,`a`的值返回`true`,点击取消,`a`的值返回`false`

**弹出框**

```pgsql
var name = window.prompt('请输入你早晨吃了什么?','mjj');
console.log(name);
```

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014093809943-325241047.png)

```
prompt()`方法接收两个参数,第一个参数是显示的文本,第二个参数是默认的文本,如果点击了确定,则`name`的结果为`mjj
```

###### 定时任务方法

在ECMAScript语法中为window对象提供了两个非常有用的定时任务的方法:`setTimeout()`和`setInterval()`方法

**setTimeout()**

setTimeout()方法表示一次性定时任务做某件事情,它接收两个参数,第一个参数为执行的函数,第二个参数为时间(毫秒计时:1000毫秒==1秒)

```arcade
window.setTimeout(function(){
    console.log('mjj');
},0)
console.log('xiongdada');
```

![img](https://img2020.cnblogs.com/blog/1364810/202010/1364810-20201014093704184-1708350562.png)

会发现,先打印了`xiongdada`,再打印了`mjj`,因为第二个参数是一个表示等待多长时间的毫秒数，但经过该时间后指定的代码不一定会执行。 JavaScript 是一个单线程序的解释器，因此一定时间内只能执行一段代码。为了控制要执行的代码，就 有一个 JavaScript 任务队列。这些任务会按照将它们添加到队列的顺序执行。setTimeout()的第二个 参数告诉 JavaScript 再过多长时间把当前任务添加到队列中。如果队列是空的，那么添加的代码会立即 执行;如果队列不是空的，那么它就要等前面的代码执行完了以后再执行。

**setInterval()**

`setInterval()`方法表示周期性循环的定时任务.它接收的参数跟`setTimeout()`方法一样.

例子: **每隔1秒让num数值+1,当num数值大于5时,停止更新**

```maxima
var num = 0;
var timer = null;
timer = setInterval(function(){
    num++;
    if (num > 5) {
        clearInterval(timer);
        return;
    }
    console.log('num:'+ num);
},1000);
```

我们可以使用`clearInterval()`来清除当前的定时任务

#### location对象

location是最有用的BOM对象之一,它提供了与当前窗口中加载的文档有关的信息，还提供了一 些导航功能。事实上，location 对象是很特别的一个对象，因为它既是 window 对象的属性，也是document 对象的属性;

换句话说，window.location 和document.location 引用的是同一个对象。 location 对象的用处不只表现在它保存着当前文档的信息，还表现在它将 URL 解析为独立的片段，让 开发人员可以通过不同的属性访问这些片段。

| 属性名   | 例子                         | 说明                                                         |
| -------- | ---------------------------- | ------------------------------------------------------------ |
| hash     | #content"                    | 返回URL中的hash(#),如果没有,则返回空字符串                   |
| host     | "www.apeland.cn:80"          | 返回服务器名称和端口号                                       |
| hostname | "www.apeland.cn"             | 返回不带端口号的服务器名称                                   |
| href     | "https://www.apeland.cn/web" | 返回当前加载页面的完成URL                                    |
| pathname | ''/web/"                     | 返回url中的目录和文件名                                      |
| port     | '80'                         | 返回url中指定的端口号,如果url中不包含端口号,则这个属性返回空字符串 |
| seach    | "?name=zhangsan"             | 返回url的查询字符串,这个字符串以问号开头                     |
| protocol | "https:"                     | 返回页面使用的协议.通常是http:或https:                       |

#### 查询字符串参数

 虽然通过上面的属性可以访问到 location 对象的大多数信息，但其中访问 URL 包含的查询字符 串的属性并不方便。尽管 location.search 返回从问号到 URL 末尾的所有内容，但却没有办法逐个 访问其中的每个查询字符串参数。为此，可以像下面这样创建一个函数，用以解析查询字符串，然后返回包含所有参数的一个对象:

```arcade
function getQueryStringArgs() {
    // ?username=mjj&pwd=18
    // 1.取得查询字符串并去掉开头的问号
    var qs = location.search.length > 0 ? location.search.substring(1) : "";
    // 保存数据的对象
    var args = {};
    // 2.取得每一项
    var items = qs.length ? qs.split('&'):  [];
    var item = null,name =  null,value = null;
    // 3.逐个将每一项添加到args对象中
    for(var i = 0; i < items.length; i++){
        item = items[i].split('=');
        name = decodeURIComponent(item[0]);
        value = decodeURIComponent(item[1]);
        if (name.length) {
            args[name]  = value;
        }
    }
    return args;

}
//假设地址查询参数为?name=zhangsan&age=18
//另外还要考虑?,=,&编码的情况
var args = getQueryStringArgs();
console.log(args);//{name: "zhangsan", age: "18"}
```

#### 位置操作

**href属性**

 使用location对象可以通过很多方式来改变浏览器的位置.首先,也是最常用的方式通过href属性将其传递一个url

**栗子:2秒之后跳转到小猿圈的web页面**

```javascript
setTimeout(function(){
    location.href = 'https://www.apeland.cn/web';
},2000)
```

 当通过上述任何方式修改 URL 之后，浏览器的历史记录中就会生成一条新记录，因此用户通 过单击“后退”按钮都会导航到前一个页面。要禁用这种行为，可以使用 replace()方法。这个方法 只接受一个参数，即要导航到的 URL;结果虽然会导致浏览器位置改变，但不会在历史记录中生成新记 录。在调用 replace()方法之后，用户不能回到前一个页面,

**栗子:2秒之后跳转网页但不会产生历史记录**

```lisp
setTimeout(function(){
    location.replace('https://www.apeland.cn/web');
},2000)
```

**reload()方法**

 它的作用是重新加载当前显示的页面,如果调用 reload() 时不传递任何参数，页面就会以最有效的方式重新加载。也就是说，如果页面自上次请求以来并没有改 变过，页面就会从浏览器缓存中重新加载。如果要强制从服务器重新加载，则需要像下面这样为该方法 传递参数 true。

```glsl
location.reload();//重新加载(有可能从缓存中加载)
location.reload(true);//重新加载(强制从服务器加载)
```

#### navigator对象

最早由 Netscape Navigator 2.0 引入的 navigator 对象，现在已经成为识别客户端浏览器的事实标准 ,每个浏览器中的 navigator 对象也都有一套自己的属性

```reasonml
//此方法只适应非IE浏览器
function hasPlugin(name){
    console.log(navigator.plugins);
    var   name = name.toLowerCase();
    for (var i=0; i < navigator.plugins.length; i++){
        if (navigator.plugins[i].name.toLowerCase().indexOf(name) > -1){ return true;
                                                                       } }
    return false;
}
//检测 Flash 
alert(hasPlugin("Flash"));
alert(hasPlugin("Native Client"));
```

#### history对象

history 对象保存着用户上网的历史记录,使用 `go()`方法可以在用户的历史记录中任意跳转，可以向后也可以向前。这个方法接受一个参数， 表示向后或向前跳转的页面数的一个整数值。负数表示向后跳转(类似于单击浏览器的“后退”按钮),正数表示向前跳转(类似于单击浏览器的“前进”按钮)。 看例子:

```awk
//后退一页 history.go(-1);
//前进一页 history.go(1);
//前进两页 history.go(2);
```