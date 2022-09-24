# 深入理解JavaScript的闭包

## 闭包

### 前言

现在去面试前端开发的岗位，如果你面对的面试官也是个前端，并且不是太水的话，你有很大的概率被问到JavaScript的闭包。

### 什么是闭包

什么是闭包，百度、Google之后，你可能会搜索很多答案...

《JavaScript高级程序设计》这样描述

> 闭包是指有权访问另一个函数作用域中的变量的函数

《JavaScript权威指南》这样描述

> 从技术的角度讲，所有的JavaScript函数都是闭包：它们都是对象，它们都关联到作用域链

《你不知道的JavaScript》这样描述

> **当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是当前词法作用域之外执行**

最认可的当属《你不知道的JavaScript》，前面两种说话都没有错。

但闭包应该是基于词法作用域书写代码时产生的自然结果，是一种现象！你也不用为了利用闭包而特意的创建，因为闭包的在你的代码中随处可见，只是你还不知道当时你写的那一段代码其实就产生了闭包

#### 讲解闭包

上面已经说到，**当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行**。

看段代码

```arcade
function fn1(){
    var name='小马哥'
    function fn2(){
        console.log(name);
    }
    fn2();
}
fn1();
```

如果是根据《JavaScript高级程序设计》和《JavaScript权威指南》来说，上面的代码已经产生闭包了。fn2访问到了fn1的变量，满足了条件“有权访问另一个函数作用域中的变量的函数”，fn2本身是个函数，所以满足了条件“所有的JavaScript函数都是闭包”。

这的确是闭包，但是这种方式定义的闭包不太好观察。

再看一段代码：

```arcade
function fn1(){
    var name='小马哥'
    function fn2(){
        console.log(name);
    }
    return fn2;
}
var fn3 = fn1();
fn3();
```

这样就清晰地展示了闭包：

- fn2的词法作用域能访问fn1的作用域
- 将fn2当做一个值返回
- fn1执行后，将fn2的引用赋值给fn3
- 执行fn3，输出了变量name

我们知道通过引用关系，fn3就是fn2函数本身。执行fn3能正常输出name,这不就是**fn2能记住并访问它所在的词法作用域，并且fn2函数的运行还是在当前词法作用域之外**

正常来说，当fn1函数执行完毕之后，其作用域是会被销毁的，然后垃圾回收器会释放那段内存空间。而闭包却很神奇的将fn1的作用域存活了下来，**fn2依然持有该作用域的引用，这个引用就是闭包**。

> 总结：**某个函数在定义时的词法作用域之外的地方被调用，闭包可以使该函数极限访问定义时的词法作用域**。

注意：对函数值的传递可以通过其他的方式，并不一定只有返回该函数这一条路，比如可以用回调函数：

```arcade
function fn1() {
	var name = '小马哥';
	function fn2() {
		console.log(name);
	}
	fn3(fn2);
}
function fn3(fn) {
	fn();
}
fn1();
```

本例中，将内部函数fn2传递给fn3，当它在fn3中被运行时，它是可以访问到name变量的。

所以无论通过哪种方式将内部的函数传递到所在的词法作用域以外，它都会持有对原始作用域的引用，无论在何处执行这个函数都会使用闭包。

### 再次解释闭包

以上的例子会让人觉得有点学院派了，但是闭包绝不仅仅是一个无用的概念，你写过的代码当中肯定有闭包的身影，比如类似如下的代码：

```reasonml
function waitSomeTime(msg,time){
    setTimeout(function(){
        console.log(msg);
    },time);
}
waitSomeTime('hello',1000);
```

定时器中有一个匿名函数，该匿名函数就有涵盖waitSomeTime函数作用域的闭包，因此当1秒之后，该匿名函数能输出msg。

另一个很经典的例子就是for循环中使用定时器延迟打印的问题：

```arcade
for (var i = 1; i <= 10; i++) {
	setTimeout(function () {
		console.log(i);
	}, 1000);
}
```

我们预期的结果为1~10，但却输出10此11。这是因为setTimeout中的匿名函数执行的时候，for循环都已经结束了，for循环结束的条件是i大于10，所以输出10此11。

> 原因：i是声明在全局作用域中的，定时器中的匿名函数也是执行在全局作用域中，那当时是每次都输出11

原因知道了，解决起来就简单了，我们可以让i在每次迭代的时候，都产生一个私有的作用域，在这个私有的作用域中保存当前i的值

```arcade
for (var i = 1; i <= 10; i++) {
	(function () {
		var j = i;
		setTimeout(function () {
			console.log(j);
		}, 1000);
	})();
}
```

这样就达到我们的预期了呀，让我们用一种比较优雅的写法改造一些，将每次迭代的i作为实参传递给自执行函数，自执行函数中用变量去接收：

```arcade
for (var i = 1; i <= 10; i++) {
	(function (j) {
		setTimeout(function () {
			console.log(j);
		}, 1000);
	})(i);
}
```

### 闭包的应用

- setTimeout

```javascript
//原生的setTimeout传递的第一个函数不能带参数
setTimeout(function(param){
    alert(param)
},1000)


//通过闭包可以实现传参效果
function func(param){
    return function(){
        alert(param)
    }
}
var f1 = func(1);
setTimeout(f1,1000);
```

- 闭包的应用比较典型的是定义模块和封装变量，我们将操作函数暴露给外部，而细节隐藏在模块内容

```pgsql
function module(){
    var arr = []; //私有变量
    function add(val){
        if(typeof val ==='number'){
            arr.push(val);
        }
    }
    function get(index){
        if(index < arr.length){
            return arr[index];
        }else {
            return null
        }
    }
    return {
        add:add,
        get:get
    }
}

var mod1 = module();
mod1.add(1);
mod1.add(2);
mod1.add('xxx');
console.log(mod1.get(2));
```

- 缓存

```arcade
function getNewValue(key) {
    var obj = {
      name:'张三'
    }
    return obj[key]
  }
  var CacheCount = (function () {
    var cache = {};
    return {
      getCache: function (key) {
        if (key in cache) { // 如果结果在缓存中
          console.log(cache);
          
          return cache[key]; // 直接返回缓存中的对象
        }
        var newValue = getNewValue(key); // 外部方法，获取缓存
        cache[key] = newValue; // 更新缓存
        return newValue;
      }

    };

  })();

  console.log(CacheCount.getCache("name"));
  console.log(CacheCount.getCache("name"));
```

## JavaSript的高级函数

#### 回调函数

```reasonml
function createDiv(cb){
    let oDiv = document.createElement('div');
    document.body.appendChild(oDiv);
    if(typeof cb === 'function'){
        cb(oDiv);
	}
}
createDiv(function(oDiv){
   oDiv.style.color = 'red'; 
});
```

这个例子中，有一个createDiv这个函数，这个函数负责创建一个div并添加到页面中，但是之后要再怎么操作这个div，createDiv这个函数就不知道，所以把权限交给调用createDiv函数的人，让调用者决定接下来的操作，就通过回调的方式将div给调用者。

这是体现出了抽象,既然不知道div接下来的操作，那么就直接给调用者，让调用者去实现

**抽象就是隐藏更具体的实现细节，从更高的层次看待我们要解决的问题**。

### 数组中遍历

在编程的时候，并不是所有功能都是现成的，比如上面例子中，可以创建好几个div，对每个div的处理都可能不一样，需要对未知的操作做抽象，预留操作的入口，作为一名程序员，我们需要具备这种在恰当的时候将代码抽象的思想

接下来看一下ES5中提供的几个数组操作方法，可以更深入的理解抽象的思想，ES5之前遍历数组的方式是：

```arcade
var arr = [1, 2, 3, 4, 5];
for (var i = 0; i < arr.length; i++) {
  var item = arr[i];
  console.log(item);
}
```

仔细看一下，这段代码中用for，然后按顺序取值，有没有觉得如此操作有些不够优雅，为出现错误留下了隐患，比如把length写错了，一不小心复用了i。既然这样，能不能抽取一个函数出来呢？最重要的一点，我们要的只是数组中的每一个值，然后操作这个值，那么就可以把遍历的过程隐藏起来：

```arcade
function forEach(arr, callback) {
  for (var i = 0; i < arr.length; i++) {
    var item = arr[i];
    callback(item);
  }
}
forEach(arr, function (item) {
  console.log(item);
});
```

以上的forEach方法就将遍历的细节隐藏起来的了，把用户想要操作的item返回出来，在callback还可以将i、arr本身返回：`callback(item, i, arr)`。

JS原生提供的forEach方法就是这样的：

```arcade
arr.forEach(function (item) {
  console.log(item);
});
```

跟forEach同族的方法还有map、some、every等。思想都是一样的，通过这种抽象的方式可以让使用者更方便，同事又让代码变得更加清晰。

抽象是一种很重要的思想，让可以让代码变得更加优雅，并且操作起来更方便。**在高阶函数中也是使用了抽象的思想，所以学习高阶函数得先了解抽象的思想**。

### 高阶函数

#### 什么是高阶函数

至少满足以下条件中的一个，就是高阶函数

- 将其他函数作为参数传递
- 将函数作为返回值

简单来说，就是**一个函数可以操作其他函数**，将其他函数作为参数或将函数作为返回值。我相信，写过JS代码的同学对这个概念都是很容易理解的，因为在JS中函数就是一个普通的值，可以被传递，可以被返回。

**参数可以被传递，可以被返回**

#### 函数作为参数传递

函数作为参数传递就是我们上面提到的回调函数，回调函数在异步请求中用的非常多，使用者想要在请求成功后利用请求回来的数据做一些操作，但是又不知道请求什么时候结束。

用jQuery来发一个Ajax请求

```awk
function getDetailData(sub_category_id, callback) {
$.ajax(`https://www.luffycity.com/api/v1/courses/?sub_category=${sub_category_id}&ordering=`, function (res) {
    if (typeof callback === 'function') {
      callback(res);
    }
  });
}
getDetailData('1', function (res) {
  // do some thing
});
```

类似Ajax这种操作非常适合用回调去做，当一个函数里不适合执行一些具体的操作，或者说不知道要怎么操作时，可以将相应的数据传递给另一个函数，让另一个函数来执行，而这个函数就是传递进来的回调函数。

另一个典型的例子就是数组排序

#### 函数作为值返回

在判断数据类型的时候最常用的是typeof,但是typeof有一定的局限性,比如：

```arcade
console.log(typeof []);//object
console.log(typeof {});//object
```

判断数组和对象都是输出object,如果想要更细致的判断应该要使用Object.prototype.toString

```pgsql
console.log(Object.prototype.toString.call([])); // 输出[object Array]
console.log(Object.prototype.toString.call({})); // 输出[object Object]
```

于是，我们可以写出判断对象、数组、数字的方法

```javascript
function isObject(obj){
    return Object.prototype.toString.call(obj) === '[object Object]';
}
function isArray(arr) {
  return Object.prototype.toString.call(arr) === '[object Array]';
}
function isNumber(number) {
  return Object.prototype.toString.call(number) === '[object Number]';
}
```

我们发现这三个方法太像了，可以做一些抽取:

```javascript
function isType(type) {
  return function (obj) {
    return Object.prototype.toString.call(obj) === '[object ' + type + ']';
  }
}
var isArray = isType('Array');
console.log(isArray([1,2]));
```

这个isType方法就是高阶函数，该函数返回了一个函数，并且利用闭包，将代码变得优雅。