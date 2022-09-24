### this绑定规则

#### 默认绑定

- 全局环境下，this默认绑定到window

```abnf
console.log(this === window); //true
```

- 函数独立调用时，this默认绑定到window

```javascript
function foo(){
    console.log(this === window);
}
foo();//true
```

- 被嵌套的函数独立调用时，this默认绑定到window

```javascript
var a = 0;
var obj = {
    a : 2,
    foo : function (){
        function test(){
            console.log(this);
        }
        test();
    }
}
obj.foo();
```

上面代码虽然`test()`函数被嵌套在`obj.foo()`函数中，但`test()`函数是独立调用，而不是方法调用。所以this默认绑定到window

**IIFE**

IIFE立即执行函数实际是函数声明后立即调用执行，内部的this指向了window

```arcade
var a = 0;
function foo(){
    (function test(){
        console.log(this.a);
    })()
};
var obj = {
    a : 2,
    foo:foo
}
obj.foo();//0
```

等价于上例

```arcade
var a = 0;
var obj = {
    a : 2,
    foo : function(){
        function test(){
            console.log(this.a);
        }
        test();
    }
}
obj.foo();//0
```

**闭包**

 类似地，`test()`函数是独立调用，而不是方法调用，所以this默认绑定到window

> 注意：函数共有4中调用方法

```arcade
var a = 0;
function foo() {
    function test() {
        console.log(this.a);
    }
    test();
}
var obj = {
    a: 2;
    foo: foo
}
obj.foo();//0
```

 由于闭包的this默认绑定到window对象，但又常常需要访问嵌套函数的this，所以常常在嵌套函数中使用`var that = this`,然后在闭包中使用that替代this，使用作用域查找的方法来找到嵌套函数的this值

```arcade
var a = 0;
function foo(){
    var that = this;
    function test(){
        console.log(that.a);
    }
    return test;
};
var obj = {
    a : 2,
    foo:foo
}
obj.foo()();//2
```

#### 隐式绑定

 一般地,被直接对象所包含的函数调用，也被称为方法地调用，this隐式绑定到该直接对象

```arcade
function foo(){
    console.log(this.a);
}
var obj1 = {
    a : 1;
    foo: foo,
    obj2 : {
        a:2,
        foo:foo
    }
}
//foo()函数的直接对象是obj1,this隐式绑定到obj1
obj1.foo();//1
//foo()函数的直接对象是obj2,this隐式绑定到obj2
obj1.obj2.foo();//2
```

#### 隐式丢失

 隐式丢失是指被隐式绑定的函数丢失绑定对象，从而默认绑定到window。**这种情况容易出错却又常见**

【**函数别名**】

```arcade
var a = 0;
function foo(){
    console.log(this.a);
}
var obj = {
    a: 1,
    foo:foo
}
//把obj.foo赋予别名bar，造成了隐式丢失，因为只是把foo()函数赋给了bar，而bar与obj对象则毫无关系
var bar = obj.foo;
bar();//0
//等价于
var a = 0;
var bar = function foo(){
    console.log(this.a);
}
bar();//0
```

**【参数传递】**

```arcade
var a = 0;

function foo() {
    console.log(this.a);
}

function bar(fn) {
    fn();
}
var obj = {
    a: 2,
    foo: foo
}
//把obj.foo当做参数传递给bar函数时,有隐式的函数赋值 fn = obj.foo,只是把foo函数赋给了fn,而fn与obj对象毫无关系
bar(obj.foo);//0
//等价于
var a = 0;
function bar(fn){
    fn();
}
bar(function foo(){
    console.log(this.a);
})
```

**【内置函数】**

内置函数与上例类似，也会造成隐式丢失

```javascript
var a = 0;
function foo(){
    console.log(this.a);
}
var obj = {
    a : 2,
    foo:foo
}
setTimeout(obj.foo,100);//0
```

**【间接调用】**

 函数的“间接引用”一般都在无意间创建，最容易在赋值时发生，会造成隐式丢失

```arcade
function foo(){
    console.log(this.a);
}
var a = 2;
var o = {a: 3,foo: foo};
var p = {a: 4};
o.foo();//3;
//将o.foo函数赋值给p.foo函数，然后立即执行。相当于仅仅是foo()函数的立即调用
(p.foo = o.foo)();//2
//另一种情况
function foo() {
    console.log( this.a );
}
var a = 2;
var o = { a: 3, foo: foo };
var p = { a: 4 };
o.foo(); // 3
//将o.foo函数赋值给p.foo函数，之后p.foo函数再执行，是属于p对象的foo函数的执行
p.foo = o.foo;
p.foo();//4
```

**【其他情况】**

 在javascript引擎内部，obj和obj.foo储存在两个内存地址，简称为M1和M2。只有obj.foo()这样调用时，是从M1调用M2，因此this指向obj。但是，下面三种情况，都是直接取出M2进行运算，然后就在全局环境执行运算结果（还是M2），因此this指向全局环境

```arcade
var a = 0;
var obj = {
    a:2,
    foo:foo
}
function foo(){
    console.log(this.a);
}
(obj.foo = obj.foo)();//0
(false || obj.foo)();//0
(1,obj.foo)();//0
```

#### 显示绑定

 通过call()、apply()、bind()方法把对象绑定到this上，叫做显示绑定。对于被调用的函数来说，叫做间接调用

```arcade
var a = 0;
function foo(){
    console.log(this.a);
}
var obj = {
    a : 2
};
foo();//0
foo.call(obj);//2
```

 普通的显示绑定无法解决隐式丢失问题

```arcade
var a = 0;
function foo(){
    console.log(this.a);
}
var obj1 = {
    a : 1
};
var obj2 = {
    a : 2
}
foo.call(obj1);//1
foo.call(obj2);//2
```

**【硬绑定】**

 硬绑定是显式绑定的一个变种，使this不能再被修改

```javascript
var a = 0;
function foo(){
    console.log(this.a);
}
var obj = {
    a:2
};
var bar = function (){
    foo.call(obj);
}
//在bar函数内部手动调用foo.call(obj)。因此，无论之后如何调用函数bar，它总会手动会在obj上调用foo
bar();//2
setTimeout(bar,2000);//2
bar.call(window);//2
```

**【API】**

 javascript中新增了许多内置函数，具有显式绑定的功能，如数组的5个迭代方法：`map()`、`forEach()`、`filter()`、`some()`、`every()`

```awk
var id = 'window';
function foo(el){
    console.log(el,this.id);
}
var obj = {
    id: 'fn'
};
[1,2,3].forEach(foo);//1 "window" 2 "window" 3 "window"
[1,2,3].forEach(foo,obj);//1 "fn" 2 "fn" 3 "fn"
```

#### new绑定

 如果函数或者方法调用之前带有关键字new，它就构成构造函数调用。对于this绑定来说，称为new绑定

【1】构造函数通常不适用return关键字，他们通常初始化新对象，当构造函数的函数体执行完毕时，它会显式返回。在这种情况下，构造函数调用表达式的计算结果就是这个新对象的值

```arcade
function fn(){
    this.a = 2;
}
var test = new fn();
console.log(test);//{a:2}
```

【2】如果构造函数使用return语句但没有指定返回值，或者返回一个原始值，那么这时将忽略返回值，同时使用这个新对象作为调用结果

```arcade
function fn(){
    this.a = 2;
    return;
}
var test = new fn();
console.log(test);//{a:2}
```

【3】使用构造函数显示地使用return语句返回一个对象，那么调用表达式的值就是这个对象

```arcade
var obj = {a:1};
function fn(){
    this.a = 2;
    return obj;
}
var test = new fn();
console.log(test);//{a:1}
```

> 注意：尽管有时候构造函数看起来像一个方法调用，它依然会使用这个新对象作为this。也就是说，在表达式new o.m()中，this并不是o

```arcade
var o = {
    m:function(){
        return this;
    }
}
var obj = new o.m();
console.log(obj,obj === o);//{} false
console.log(obj.contructor === o.m);//true
```

#### 严格模式

【1】严格模式下，独立调用的函数的this指向undefined

```arcade
function fn(){
    'use strict';
    console.log(this);//undefined
}
fn();

function fn(){
    console.log(this);//window
}
fn();
```

【2】在非严格模式下，使用函数的call()或apply()方法时，null或undefined值会被转换成全局对象。而在严格模式下，函数的this值始终是指定的值

```qml
var color = 'red';
function displayColor(){
    console.log(this.color);
}
displayColor.call(null);//red

var color = 'red';
function displayColor(){
    'use strict';
    console.log(this.color);
}
displayColor.call(null);//TypeError: Cannot read property 'color' of null
```

#### 总结

- this的四种绑定规则：默认绑定、隐式绑定、显式绑定和new绑定，分别对应函数的四种调用方式：独立调用、方法调用、间接调用和构造函数调用。
- 分清这四种绑定规则不算难，但是比较麻烦的是需要练就火眼金睛，识别出隐式丢失的情况。
- 说到底，JavaScript如此复杂的原因是因为函数过于强大。因为，函数是对象，所以原型链比较复杂；因为函数可以作为值被传递，所以执行环境栈比较复杂；同样地，因为函数具有多种调用方式，所以this的绑定规则也比较复杂
- 只有理解了函数，才算理解javascript

 