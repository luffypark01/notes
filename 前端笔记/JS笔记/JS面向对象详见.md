文章教会你什么是对象、constructor、创建对象的5种模式、实现继承5种方式、深浅拷贝等用法。 本文章配有免费教学视频，欢迎大家访问该视频[《面向对象编程》](https://link.juejin.cn/?target=https%3A%2F%2Fwww.bilibili.com%2Fvideo%2FBV1q7411v7f9)。感谢大家动动小手关注-点赞-收藏，后续会不断更新优质内容。

JavaScript语句具有很强的面向对象编程能力

### 对象是什么

面向对象编程(Object oriented Programming,缩写为OOP)是目前主流的编程范式。它将真实世界各种复杂的关系，抽象为一个个对象，然后由对象之间的分工与合作，完成对真实世界的模拟。

每一个对象都是功能中心，具有明确分工，可以完成接受信息、处理数据、发出信息等任务。对象可以复用，通过继承机制还可以定制。因此，面向对象编程具有灵活、代码可复用、高度模块化等特点，容易维护和开发，比起由一系列函数或指令组成的传统的过程式编程（procedural programming），更适合多人合作的大型软件项目。

那么，“对象”（object）到底是什么？我们从两个层次来理解。

**【1】对象是单个实物的抽象**

一本书、一辆汽车、一个人都可以是对象，一个数据库、一张网页、一个与远程服务器的连接也可以是对象。当实物被抽象成对象，实物之间的关系就变成了对象之间的关系，从而就可以模拟现实情况，针对对象进行编程。

**【2】对象是一个容器，封装了属性(property)和方法(method)**

属性是对象的状态，方法是对象的行为（完成某种任务）。比如，我们可以把动物抽象为`animal`对象，使用“属性”记录具体是那一种动物，使用“方法”表示动物的某种行为（奔跑、捕猎、休息等等）。

### 构造函数

面向对象编程的第一步，就是要生成对象。前面说过，对象是是单个实物的抽象。通常需要一个模板，表示某一类实物的共同特征，然后对象根据这个模板生成。

典型的面向对象编程语言（比如 C++ 和 Java），都有“类”（class）这个概念。所谓“类”就是对象的模板，对象就是“类”的实例。但是，JavaScript 语言的对象体系，不是基于“类”的，而是基于构造函数（constructor）和原型链（prototype）。

JavaScript 语言使用构造函数（constructor）作为对象的模板。所谓”构造函数”，就是专门用来生成实例对象的函数。它就是对象的模板，描述实例对象的基本结构。一个构造函数，可以生成多个实例对象，这些实例对象都有相同的结构。

构造函数就是一个普通的函数，但是有自己的特征和用法。

```actionscript
var Dog = function(){
    this.name = '阿黄';
}
```

上面代码中， `Dog`就是构造函数。为了与普通函数区别，构造函数名字的第一个字母通常大写。

构造函数的特点有两个：

- 函数体内使用了`this`关键字，代表了所要生成的对象实例。
- 生成对象的时候，必须使用new命令

根据需要，构造函数可以接受参数

```arcade
function Dog (name){
    this.name = name;
}
var d1 = new Dog('阿黄');
console.log(d1.name);//阿黄
```

如果忘记使用new操作符，则this将代表全局对象window

```arcade
function Dog(){
    this.name = name;
}
var d1 = Dog();
//Uncaught TypeError: Cannot read property 'name' of undefined
console.log(d1.name);
```

上述代码，忘记使用`new`命令，其实是导致`d1`编程了`undefined`,而`name`属性变成了全局变量。因此，应该非常小心，避免不使用`new`命令、直接调用构造函数

为了保证构造函数必须与`new`命令一起使用，一个解决办法是，构造函数内部使用严格模式，即第一行加上`use strict`。这样的话，一旦忘了使用`new`命令，直接调用构造函数就会报错。

```actionscript
function Dog(name){
    'use strict';
    this.name = name;
}
var d1 = Dog('阿黄');
```

上面代码的`Dog`为构造函数，`use strict`命令保证了该函数在严格模式下运行。由于严格模式中，函数内部的`this`不能指向全局对象，默认等于`undefined`，导致不加`new`调用会报错（JavaScript不允许对`undefined`添加属性）。

**instanceof**

 该运算符运行时指出对象是否是特定类的一个实例

 另一个解决办法，构造函数内部判断是否使用了`new`命令，如果发现没有使用，则直接返回一个实例对象

**instanceof操作符可以用来鉴别对象的类型**

```pgsql
function Dog(name){
    if(!(this instanceof Dog)){
        return new Dog(name);
    }
    this.name = name;
}
var d1 = Dog('阿黄');
console.log(d1.name);//'阿黄'
console.log(Dog('阿黄').name);//'阿黄'
```

上述代码中的构造函数，不管加不加`new`命令，都会得到同样的结果

### new命令

大家也能看到，如果我们想创建一个对象，声明构造函数之后，必须使用`new`命令来实例化对象。那么我们来研究一下`new`命令的原理

使用`new`命令时，它后面的函数依次执行下面的步骤

1. 创建一个空对象，作为将要返回的对象实例
2. 将这个空对象的原型，指向了构造函数的`prototype`属性
3. 将这个空对象赋值给函数内部的`this`关键字
4. 开始执行构造函数内部的代码

也就是说，构造函数内部，`this`指的是一个新生成的空对象，所有针对`this`的操作，都会发生在这个空对象上。构造函数之所以叫“构造函数”，就是说这个函数的目的，就是操作一个空对象（即`this`对象），将其“构造”为需要的样子。

### constructor

 每个对象在创建时都自动拥有一个构造函数属性`contructor`,其中包含了一个指向其构造函数的引用。而这个`constructor`属性实际上继承自原型对象，而constructor也是原型对象唯一的自有属性

```abnf
function Dog(){
    
}
var d1 = new Dog();
console.log(d1.constructor === Person);//true
console.log(d1.__proto__.constructor === Person);//true
```

以下是`Dog`的内部属性，发现constructor是继承属性

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/62cc6ab3fcc6479c9a3442fbd2589fd5~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

虽然对象实例及其构造函数之间存在这样的关系，可以使用instanceof来检查对象类型。

**返回值**

 函数中的return语句用来返回函数调用后的返回值，而new构造函数的返回值有点特殊。

 如果构造函数使用return语句但没有指定返回值，或者返回值是一个原始值，那么这时将忽略返回值，同时使用这个新对象作为调用结果

```arcade
function Fn(){
    this.a = 2;
    return;
}
var test = new Fn();
console.log(test);//{a:2}
```

 如果构造函数显式地使用return语句返回一个对象，那么调用表达式的值就是这个对象

```arcade
var obj = {a:1};
function fn(){
    this.a = 2;
    return obj;
}
var test = new fn();
console.log(test);//{a:1}
```

 使用构造函数的好处在于所有用同一个构造函数创建的对象都具有同样的属性和方法

```javascript
function Person(name){
    this.name = name;
    this.sayName = function(){
        console.log(this.name);
    }
}
var p1 = new Person('Tom');
var p2 = new Person('Jack');
```

 构造函数允许给对象来配置同样的属性，但是构造函数并没有消除代码冗余。使用构造函数的主要问题是每个方法都要在每个实例上重新创建一遍。在上面的例子中，每一个对象都有自己的`sayName()`方法。这也意味着如果有100个对象实例，就有100个函数做相同的事情，只是使用的数据不同。

```javascript
function Person(name){
    this.name = name;
    this.sayName = function(){
        console.log(this.name);
    }
}
var p1 = new Person('Tom');
var p2 = new Person('Jack');
console.log(p1.sayName === p2.sayName);//false
```

 上面代码中，`p1`和`p2`是用一个构造函数的两个实例，他们具有`sayName`方法。由于`sayName`方法是生成在每个实例对象上面，所以两个实例就生成了两次。也就是说，每创建一个实例，就会新建一个`sayName`方法。这既没有必要，又浪费系统资源，因此所有`sayName`方法都是同样的行为，完全应该共享。

这个问题的解决方法。就是JavaScript的原型对象(prototype)

### 原型对象

 说起原型对象，就要说到**原型对象**、**实例对象**和**构造函数**的三角关系

 接下来以下面两行代码，来详细说明他们的关系

```actionscript
function Foo(){};
var f1 = new Foo();
```

**构造函数**

 用来初始化新创建的对象的函数是构造函数。在例子中，`Foo`函数是构造函数

**实例对象**

 通过构造函数的new操作创建的对象是实例对象，又通常被称为对象实例。可以用一个构造函数，构造多个实例对象。下面的`f1`和`f2`就是实例对象

```abnf
function Foo(){};
var f1 = new Foo();
var f2 = new Foo();
console.log(f1 === f2);//false
```

**原型对象和prototype**

 通过构造函数的new操作创建实例对象后，会自动为构造函数创建`prototype`属性，该属性指向实例对象的原型对象。通过同一个构造函数实例化的多个对象具有相同的原型对象。这个的例子中，Foo.prototype是原型对象

```arcade
function Foo(){};
Foo.prototype.a = 1;
var f1 = new Foo;
var f2 = new Foo;

console.log(Foo.prototype.a);//1
console. log(f1.a);//1
console.log(f2.a);//1
```

#### prototype属性的作用

 JavaScript继承机制的设计思想就是，原型对象的所有属性和方法，都能被实例对象共享。也就是说，如果属性和方法定义在原型上，那么所有实例对象就能共享，不仅节省了内存，还体现了实例对象之间的联系。

如何为对象指定原型。JavaScript规定，每个函数都有一个`prototype`属性，指向了一个对象

```arcade
function fn(){};
//函数fn默认具有prototype属性，指向了一个对象
console.log(typeof fn.prototype);//"Object"
```

对于普通函数来说，该属性基本没用。但是对于构造函数来说，生成实例的时候，该属性会自动成为实例对象的原型。

```arcade
function  Person(name){
    this.name = name;
}
Person.prototype.age = 18;
var p1 = new Person('大王');
var p2 = new Person('二王');
console.log(p1.age);//18
console.log(p2.age);//18
```

上面代码中，构造函数`Person`的`prototype`属性，就是实例对象`p1`和`p2`的原型对象。原型对象上添加一个`age`属性，结果，实例对象都共享了该属性。

原型对象的属性不是实例对象自身的属性。只要修改原型对象，变动就立刻会体现在所有实例对象上

```arcade
Person.prototype.age = 40;
console.log(p1.age);//40
console.log(p2.age);//40
```

上面代码中，原型对象的`age`属性的值变为`40`，两个实例对象的`age`属性立刻跟着变了。这是因为实例对象其实没有`age`属性，都是读取原型对象的`age`属性。也就是说，当实例对象本身没有某个属性或方法的时候，它会到原型对象去寻找该属性或方法。这就是原型对象的特殊之处。

如果实例对象就有某个属性或方法，它就不会再去原型对象寻找这个属性和方法

```arcade
p1.age = 35;
console.log(p1.age);//35
console.log(p2.age);//40
console.log(Person.prototype.age) //40
```

上面代码中，实例对象`p1`的`age`属性改为`35`，就使得它不再去原型对象读取`age`属性，后者的值依然为`40`。

**总结**

 原型对象的作用，就是定义所有实例对象共享的属性和方法。这也是它被称为原型对象的原因，而实例对象可以视作从原型对象衍生出来的子对象

```javascript
Person.prototype.sayAge = function(){
    console.log('My age is'+ this.age);
}
```

上面代码中，`Person.prototype`对象上面定义了一个`sayAge`方法，这个方法将可以在所有`Person`实例对象上面调用。

#### 原型链

 JavaScript规定，所有对象都有自己的原型对象(prototype)。一方面，任何一个对象，都可以充当其它对象的原型；另一方面，由于原型对象也是对象，所以它也有自己的原型。因此，就会形成一个`原型链`(prototype chain):对象的原型，再到原型的原型......

 如果一层层的往上寻找，所有对象的原型最终都可以寻找到`Object.prototype`,即`Object`构造函数的`prototype`属性。也就是说，所有对象都继承了Object.prototype的属性。这就是所有对象都有`valueof`和`toString`方法的原因，因为这是从`Object.prototype`继承的。

 那么，`Object.prototype`对象有没有它的原型呢？回答是`Object.prototype`的原型是`null`。`null`没有任何属性和方法，也没有自己的原型。因此，原型链的尽头就是`null`。

```javascript
Object.getPrototypeOf(Object.prototype);//null
```

 上面代码表示，`Object.prototype`对象的原型是`null`，由于`null`没有任何属性，所以原型链到此为止。`Object.getPrototypeOf`方法返回参数对象的原型，具体介绍在对象的方法这节课中。

 读取对象的某个属性时，JavaScript 引擎先寻找对象本身的属性，如果找不到，就到它的原型去找，如果还是找不到，就到原型的原型去找。如果直到最顶层的`Object.prototype`还是找不到，则返回`undefined`。如果对象自身和它的原型，都定义了一个同名属性，那么优先读取对象自身的属性，这叫做“覆盖”（overriding）

 注意，一级级向上，在整个原型链上寻找某个属性，对性能是有影响的。所寻找的属性在越上层的原型对象，对性能的影响越大。如果寻找某个不存在的属性，将会遍历整个原型链。

 举例来说，如果让构造函数`prototype`属性指向一个数组，就意味着实例对象可以调用数组方法

```javascript
var MyArray = function () {};

MyArray.prototype = Array.prototype;
MyArray.prototype.constructor = MyArray;

var mine = new MyArray();
mine.push(1, 2, 3);
mine.length // 3
mine instanceof Array // true
```

#### constructor

 原型对象默认只会取得`constructor`属性,指向该原型对象对应的构造函数。至于其他方法，则是从Object继承来的

```abnf
function Foo(){};
console.log(Foo.prototype.constructor === Foo);//true
```

 由于实例对象可以继承原型对象的属性，所以实例对象也拥有`constructor`属性，同样指向原型对象对应的构造函数

```arcade
function Foo(){};
var f1 = new Foo();
console.log(f1.constructor === Foo);//true
console.log(f1.constuctor === Foo.prototype.constructor);//true
f1.hasOwnProperty('constructor');//false
```

 上面代码中，`f1`是构造函数`Foo`的实例对象，但是`f1`自身没有`constructor`属性，该属性其实是读取原型链上面的`Foo.prototype.constructor`属性



![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dc5f019d1fbf475e96c5c272004b6213~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

constructor属性的作用是，可以得知某个实例对象，到底是哪一个构造函数产生的。

```abnf
function Foo(){};
var f1 = new Foo();
console.log(f1.constructor === Foo);//true
console.log(f1.constructor === Array);//false
```

`constructor`属性表示原型对象与构造函数之间的关联关系，如果修改了原型对象，一般会同时修改`constructor`属性，防止引用的时候出错

举个例子：

```javascript
function Person(name){
    this.name = name;
}
console.log(Person.prototype.constructor === Person);//true

//修改原型对象
Person.prototype = {
    fn:function(){
        
    }
};
console.log(Person.prototype.constructor === Person);//false
console.log(Person.prototype.constructor === Object);//true
```

 所以，修改原型对象时，一般要同时修改`constructor`属性的指向

```javascript
function Person(name){
    this.name = name;
}
console.log(Person.prototype.constructor === Person);//true

//修改原型对象
Person.prototype = {
    constructor:Person,
    fn:function(){
        console.log(this.name);
    }
};
var p1 = new Person('阿黄');
console.log(p1 instanceof Person);//true
console.log(Person.constructor == Person);//true
console.log(Person.constructor === Object);//false
```

**`__proto__`**

 实例对象内部包含一个`__proto__`属性，指向该实例对象对应的原型对象

```abnf
function Foo(){};
var f1 = new Foo;
console.log(f1.__proto__ === Foo.prototype);//true
```

#### 总结

 构造函数、原型对象和实例对象之间的关系是实例对象和构造函数之间没有直接联系

```actionscript
function Foo(){};
var f1 = new Foo();
```

 以上代码的原型对象是Foo.prototype,实例对象是f1,构造函数是Foo

 原型对象和实例对象的关系

```abnf
console.log(Foo.prototype === f1.__proto__);//true
```

 原型对象和构造对象的关系

```abnf
console.log(Foo.prototype.constructor === Foo);//true
```

 而实例对象和构造函数则没有直接关系，间接关系是实例对象可以继承原型对象的constructor属性

```abnf
console.log(f1.constructor === Foo);//true
```

 如果非要扯实例对象和构造函数的关系，那只能是下面这句代码，实例对象是构造函数的new操作的结果

```haxe
var f1 = new Foo;
```

这句代码执行以后，如果重置原型对象，则会打破它们三个的关系

```abnf
function Foo(){};
var f1 = new Foo;
console.log(Foo.prototype === f1.__proto__);//true
console.log(Foo.prototype.constructor === Foo);//true

Foo.prototype = {};
console.log(Foo.prototype === f1.__proto__);//false
console.log(Foo.prototype.constructor === Foo);//false
```

 　所以，代码顺序很重要

### 创建对象的5种模式

 如何创建对象，或者说如何更优雅的创建对象。接下来将从最简单的创建对象的方式入手，逐步介绍5中创建对象的模式

#### 对象字面量

 一般地，我们创建一个对象会使用对象字面量的形式

> 有三种方式来创建对象，包括new构造函数、对象直接量和Object.create()函数

【1】new构造函数

 使用new操作符后跟Object构造函数用以初始化一个新创建的对象

```dart
var person = new Object();
person.name = 'mjj';
person.age = 28;
```

【2】对象字面量

 JavaScript提供了字面量的快捷方式，用于创建大多数原生对象值。使用字面量只是隐藏了与new操作符相同的基本过程，于是也可以叫做语法糖

```actionscript
var person = {
    name:'mjj';
    age:28
}
```

 使用对象字面量的方法来定义对象，属性名会自动转成字符串

【3】Object.create()

 生成实例对象的常用方法是，使用`new`命令让构造函数返回一个实例。但是很多时候，只能拿到一个实例对象，它可能根本不是由构造函数生成的，那么能不能从一个实例对象，生成另一个实例对象呢？

 ES5定义了一个名为Object.create()的方法，用来满足这种需求。该方法接受一个对象作为参数，然后以它为原型，返回一个实例对象。该实例完全继承原型对象的属性。

```arcade
//原型对象
var A = {
    getX:function(){
        console.log('hello');
    }
};
//实例对象
var B = Object.create(A);
console.log(B.getX);//"hello"
```

上面代码中，`Object.create`方法以`A`对象为原型，生成了`B`对象。`B`继承了`A`的所有属性和方法。

```actionscript
var person1 = {
    name:'mjj',
    age:28,
    sayName: function(){
        alert(this.name);
    }
}
```

 如果我们要创建大量的对象，则如下所示

```actionscript
var person1 = {
    name:'mjj',
    age:28,
    sayName: function(){
        alert(this.name);
    }
}
var person2 = {
    name:'alex',
    age:38,
    sayName: function(){
        alert(this.name);
    }
}
/*
var person3 = {}
var person4 = {}
var person5 = {}
......
*/
```

 虽然对象字面量可以用来创建单个对象，但如果要创建多个对象，会产生大量的重复代码

#### 工厂模式

 为了解决上述问题，人们开始使用工厂模式。该模式抽象了创建具体对象的过程，用函数来封装以特地接口创建对象的细节

```javascript
function createPerson(name,age){
    var p = new Object();
    p.name = name;
    p.age = age;
    p.sayName = function(){
        alert(this.name);
    }
    return p;
}
var p1 = createPerson('mjj',28);
var p2 = createPerson('alex',28);
var p3 = createPerson('阿黄',8);
```

 工厂模式虽然解决了创建多个相似对象的问题，但没有解决对象识别的问题，因为使用该模式并没有给出对象的类型

#### 构造函数模式

 可以通过创建自定义的构造函数，来定义自定义对象类型的属性和方法。创建自定义的构造函数意味着可以将它的实例标识为一种特定的类型，而这正是构造函数模式胜过工厂模式的地方。该模式没有显式地创建对象，直接将属性和方法赋给了this对象，且没有return语句

```actionscript
function Person(name,age){
    this.name = name;
    this.age = age;
    this.sayName = function(){
        alert(this.name);
    };
}
var person1 = new Person("mjj",28);
var person2 = new Person("alex",25);
```

 使用构造函数的主要问题是每个方法都要在每个实例上重新创建一遍，创建多个完成相同任务的方法完全没有必要，浪费内存空间

```javascript
function Person(name,age){
    this.name = name;
    this.age = age;
    this.sayName = function(){
        alert(this.name);
    };
}
var p1 = new Person("mjj",28);
var p2 = new Person("alex",25);
//具有相同的sayName()方法在p1和p2这两个实例中缺占用了不同的内存空间
console.log(person1.sayName === person2.sayName);//false
```

##### 构造函数拓展模式

 在构造函数模式的基础上，把方法定义转移到构造函数外部，可以解决方法被重复创建的问题

```javascript
function Person(name,age){
    this.name = name;
    this.age = age;
    this.sayName = sayName;
}
function sayName(){
    alert(this.name);
}
var p1 = new Person("mjj",28);
var p2 = new Person("alex",25);
console.log(person1.sayName === person2.sayName);//true
```

 现在，新问题又来了。在全局作用域中定义的函数实际上只能被某个对象调用，这让全局作用域有点名不副实。而且，如果对象需要定义很多方法，就要定义很多全局函数，严重污染全局空间，这个自定义的引用类型没有封装性可言了

##### 寄生构造函数模式

 该模式的基本思想是创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象。该模式是工厂模式和构造函数模式的结合

 寄生构造函数模式与构造函数模式有相同的问题，每个方法都要在每个实例上重新创建一遍，创建多个完成相同任务的方法完全没有必要，浪费内存空间

```javascript
function Person(name,age){
    var p = new Object();
    p.name = name;
    p.age = age;
    p.sayName = function(){
        alert(this.name);
    }
    return p;
}
var p1 = new Person('mjj',28);
var p2 = new Person('alex',28);
//具有相同作用的sayName()方法在person1和person2这两个实例中却占用了不同的内存空间
console.log(p1.sayName === p2.sayName);//false
```

 还有一个问题是，使用该模式返回的对象与构造函数之间没有关系。因此，使用`instanceof`运算符和`prototype`属性都没有意义。所以，该模式要尽量避免使用

```javascript
function Person(name,age){
    var p = new Object();
    p.name = name;
    p.age = age;
    p.sayName = function(){
        alert(this.name);
    }
    return p;
}
var p1 = new Person('mjj',28);
console.log(p1 instanceof Person);//false
console.log(p1.__proto__ === Person.prototype);//false
```

##### 稳妥构造函数模式

 所谓稳妥对象指没有公共属性，而且方法也不引用this对象。稳妥对象最适合在一些安全环境中(这些环境会禁止使用this和new)或者在防止数据被其他应用程序改动时使用

 稳妥构造函数与寄生构造函数模式相似，但有两点不同，一是新创建对象的实例方法不引用this；二是不适用new操作符调用构造函数。

```arcade
function Person(name,age){
    //创建要返回的对象
    var p = new Object();
    //可以在这里定义私有变量和函数
    //添加方法
    p.sayName = function (){
        console.log(name);
    }
    //返回对象
    return p;
}
//在稳妥模式创建的对象中，除了使用sayName()方法之外，没有其他方法访问name的值
var p1 = Person('mjj',28);
p1.sayName();//"mjj"
```

 与寄生构造函数模式相似，使用稳妥构造函数模式创建的对象与构造函数之间也没有什么关系，因此instanceof操作符对这种对象也没有什么意义

#### 原型模式

 使用原型对象，可以让所有实例共享它的属性和方法。换句话说，不必在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到原型对象中

```abnf
function Person(){
    Person.prototype.name = "mjj";
    Person.prototype.age = 29;
    Person.prototype.sayName = function(){
        console.log(this.name);
    }
}
var p1 = new Person();
p1.sayName();//"mjj"
var p2 = new Person();
p2.sayName();//"mjj"
alert(p1.sayName === p2.sayName);//true
```

##### 更简单的原型模式

 为了减少不必要的输入，也为了从视觉上更好地封装原型的功能，用一个包含所有属性的方法的对象字面量来重写整个原型对象

 但是，经过对象字面量的改写后，`constructor`不再指向`Person`。因此此方法完全重写了默认的`prototype`对象，使得`Person.prototype`的自有属性`constructor`属性不存在，只有从原型链中找到`Object.prototype`中的`constructor`属性

```arcade
function Person(){};
Person.prototype = {
    name:'mjj',
    age:28,
    sayName:function(){
        console.log(this.name);
    }
}
var p1 = new Person();
p1.sayName();//"mjj"
console.log(p1.constructor === Person);//false
console.log(p1.constructor === Object);//true
```

 可以显示地设置原型对象的constructor属性

```arcade
function Person(){};
Person.prototype = {
    constructor:Person,
    name:'mjj',
    age:28,
    sayName:function(){
        console.log(this.name);
    }
}
var p1 = new Person();
p1.sayName();//"mjj"
console.log(p1.constructor === Person);//true
console.log(p1.constructor === Object);//false
```

 原型模式问题在于引用类型值属性会被所有的实例对象共享并修改，这也是很少有人单独使用原型模式的原因。

```arcade
function Person(){};
Person.prototype = {
    constructor:Person,
    name:'mjj',
    age:28,
    friends:['alex','阿黄'],
    sayName:function(){
        console.log(this.name);
    }
}
var p1 = new Person();
var p2 = new Person();
p1.friends.push('阿黑');
alert(p1.friends);//['alex','阿黄','阿黑']
alert(p2.friends);//['alex','阿黄','阿黑']
alert(p1.friends === p2.friends);//true
```

#### 组合模式

 组合使用构造函数模式和原型模式是创建自定义类型的最常见方式。构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性，这种组合模式还支持向构造函数传递参数。实例对象都有自己的一份实例属性的副本，同时又共享对方法的引用，最大限度地节省了内存。该模式是目前使用最广泛、认同度最高的一种创建自定义对象的模式

```javascript
function Person(name,age){
    this.name = name;
    this.age = age;
    this.friends = ['alex','阿黄'];
}
Person.prototype = {
    constructor:Person,
    sayName:function(){
        console.log(this.name);
    }
}
var p1 = new Person('mjj',28);
var p2 = new Person('jjm',30);
p1.friends.push('wusir');
alert(p1.friends);//['alex','阿黄','wusir']
alert(p2.friends);//['alex','阿黄']
alert(p1.friends === p2.friends);//false
alert(p1.sayName === p2.sayName);//true
```

##### 动态原型模式

 动态原型模式将组合模式中分开使用的构造函数和原型对象都封装到构造函数中，然后通过检查方法是否被创建，来决定是否初始化原型对象

 使用这种方法将分开的构造函数和原型对象合并到了一起，使得代码更加整齐，也减少了全局控件的污染

> 注意：如果原型对象中包含多个语句，只需要检查其中一个语句即可

```javascript
function Person(name,age){
    //属性
    this.name = name;
    this.age = age;
    //方法
    if(typeof this.sayName != "function"){
        Person.prototype.sayName = function(){
            console.log(this.name);
        }
    }
}
var p1 = new Person('小马哥',28);
p1.sayName();//"小马哥"
```

#### 总结

 从使用对象字面量形式创建一个对象开始说起，创建多个对象会造成代码冗余；使用工厂模式可以解决该问题，但存在对象识别的问题；接着介绍了构造函数模式，该模式解决了对象识别的问题，但存在关于方法的重复创建问题；接着介绍了原型模式，该模式的特点就在于共享，但引出了引用类型值属性会被所有的实例对象共享并修改的问题；最后，提出了构造函数和原型组合模式，构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性，这种组合模式还支持向构造函数传递参数，该模式是目前使用最广泛的一种模式

### 基于面向对象的选项卡案例

**效果**

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/727cfe02919d46f581c7c60a2c4fde52~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

#### 面向过程选项卡实现

html部分

```xml
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>01 普通方式选项卡实现</title>
        <style type="text/css">
            *{
                padding: 0;
                margin: 0;
            }
            a{
                text-decoration: none;
            }
            body{
                background-color: #BAA895;
            }
            #wrap{
                width: 302px;
                height: 400px;
                margin: 100px auto;
            }
            ul{
                list-style: none;
                overflow: hidden;
                border: 1px solid #3081BF;
                height: 45px;
                width: 300px;
            }
            ul li{
                float: left;
                width: 100px;
                height: 45px;
                line-height: 45px;
                text-align: center;
            }
            ul li a{
                display: inline-block;
                width: 100px;
                height: 100%;
                font-size: 18px;
                color: #262626;
            }
            ul li.active{
                background-color:  #3081BF;
                font-weight: bold;
            }
            .content{
                width: 300px;
                height: 300px;
                border: 1px solid  #3081BF;
                display: none;
            }
        </style>
    </head>
    <body>
        <div id="wrap">
            <ul>
                <li class="active">
                    <a href="javascript:void(0);">推荐</a>
                </li>
                <li>
                    <a href="javascript:void(0);">小说</a>
                </li>
                <li>
                    <a href="javascript:void(0);">导航</a>
                </li>
            </ul>
            <div class="content"  style="display:block;">推荐</div>
            <div class="content">小说</div>
            <div class="content">导航</div>
        </div>
        <script type="text/javascript">
            window.onload = function(){
                // 1.获取需要的标签
               var tabLis = document.getElementsByTagName('li');
               var contentDivs=document.getElementsByClassName('content');
                for(var i = 0; i < tabLis.length; i++){
                    // 保存每个i
                    tabLis[i].index = i;
                    tabLis[i].onclick = function(){
                        for(var j = 0; j < tabLis.length;j++){
                            tabLis[j].className = '';
                            contentDivs[j].style.display = 'none';
                        }
                        this.className  = 'active';
                        contentDivs[this.index].style.display = 'block';
                    }
                }

            }
        </script>

    </body>
</html>
```

慢慢改成面向对象的形式

**封装：将函数和方法分离**

```javascript
window.onload = function(){
    // 1.获取需要的标签
    var tabLis = document.getElementsByTagName('li');
    var contentDivs = document.getElementsByClassName('content');
    for(var i = 0; i < tabLis.length; i++){
        // 保存每个i
        tabLis[i].index = i;
        tabLis[i].onclick = clickFun;
    }
    function clickFun(){
        for(var j = 0; j < tabLis.length;j++){
            tabLis[j].className = '';
            contentDivs[j].style.display = 'none';
        }
        this.className  = 'active';
        contentDivs[this.index].style.display = 'block';		
    }

}
```

**基于面向对象来实现**

```gcode
思路：
1.创建一个TabSwitch的构造函数
2.给当前对象添加属性(状态：比如绑定的html元素)
3.给当前对象的原型对象上添加方法(点击方法)
window.onload = function(){
    // 1.创建构造函数
    function TabSwitch(obj){
        console.log(obj);
        // 2.绑定实例属性
        this.tabLis = obj.children[0].getElementsByTagName('li');
        this.contentDivs = obj.getElementsByTagName('div');
        for(var i = 0; i < this.tabLis.length; i++){
            // 保存每个i
            this.tabLis[i].index = i;
            this.tabLis[i].onclick = this.clickFun;
        }

    }
    TabSwitch.prototype.clickFun = function(){
        // 去掉所有
        for(var j = 0; j < this.tabLis.length;j++){
            this.tabLis[j].className = '';
            this.contentDivs[j].style.display = 'none';
        }
        this.className  = 'active';
        this.contentDivs[this.index].style.display = 'block';		

    }
    var wrap = document.getElementById('wrap');
    var tab = new TabSwitch(wrap);

}
```

当你感觉自己写的非常完美，在网页上一运行，发现报错了

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0c5d5904b8cb404f9f73c74fdcd0c9da~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

这是因为在`clickFun`此时是指向了当前点击的`li`标签，而我们希望此方法中的this指向了`tab`对象。

将`clickFun`的调用放在一个函数里，这样就不会改变`clickFun`的所属对象了。同时，还会存在另一个问题，此时的`clickFun`的this指向了tab对象，但是`this.className`,`this.index`,此处的this应该指向的是`tab`对象，那么但tab对象中没有这两个属性。所以以下改造才正确

```kotlin
// 1.创建TabSwitch构造函数
function TabSwitch(id){
    // 保存this
    var _this = this;
    var wrap = document.getElementById(id);
    this.tabLis = wrap.children[0].getElementsByTagName('li');
    this.contentDivs = wrap.getElementsByTagName('div');
    for(var i = 0; i< this.tabLis.length; i++){
        // 设置索引
        this.tabLis[i].index = i;
        // 给按钮添加事件
        this.tabLis[i].onclick = function(){
            _this.clickFun(this.index);
        }
    }
}
// 原型方法
TabSwitch.prototype.clickFun = function(index){
    // 去掉所有
    for(var j = 0; j < this.tabLis.length;j++){
        this.tabLis[j].className = '';
        console.log(this.contentDivs)
        this.contentDivs[j].style.display = 'none';
    }
    this.tabLis[index].className  = 'active';
    this.contentDivs[index].style.display = 'block';	
};
new TabSwitch('wrap');
```

**最终版**

将代码提取到一个单独的js文件中，在用的时候引入即可

### 实现继承的5种方式

 学习如何创建对象是理解面向对象编程的第一步，不知道大家是否学会了呢？上文中对创建对象有详细介绍，如果还没有理解透彻的同学，建议再回去温故而知新。那么第二部分是理解继承。开明宗义，继承是指在原型对象的所有属性和方法，都能被实例对象共享。也就是说，我们只要在原有对象的基础上，略作修改，得到一个新的的对象。

#### 原型链继承

 JavaScript使用原型链作为实现继承的主要方法，实现的本质是重写原型对象，代之以一个新类型的实例。下面代码中，原来存在于`SuperType`的实例对象的属性和方法，现在也存在于`SubType.prototype`中了

```javascript
function Super(){
    this.value = true;
}
Super.prototype.getValue = function(){
    return this.value
}
function Sub(){};
//Sub继承了Super
Sub.prototype = new Super();
Sub.prototype.constroctor = Sub;

var ins = new Sub();
console.log(ins.getValue());//true
```

 以上代码定了两个类型：`Super`和`Sub`。`Sub`继承了`Super`,而继承是通过创建`Super`实例，并将实例赋给`Sub.prototype`实现的。**实现的本质是重写对象，代之以一个新类型的属性。**换句话说，原来存在于`Super`的实例中的所有属性和方法，现在也存在与`Sub.prototype`中。如图所示。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6020b73d6610412d9e79e213b6d94489~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

 上图可以看出，我们没有使用`Sub`默认提供的原型，而是给它换了一个新原型；这个新原型就是`Super`的实例。于是，新原型不仅具有作为一个`Super`的实例所拥有的属性和方法，而且它还指向了`Super`的原型。最终结果就是这样的：

```abnf
ins=>Sub的原型=>Super的原型
```

`getValue()`方法仍然还在`Sub.prototype`中，但`value`属性则位于`Sub.prototype`中。这是因为`value`是一个实例属性，而`getValue()`则是一个原型方法。既然`Sub.prototype`现在是`Super`的实例，那么`value`位于该实例中。

此外，要注意`ins.constructor`现在指向的 是 `Super`,这是因为原来` Sub.prototype` 中的 constructor 被重写了的缘故。

 原型链**最主要的问题私有原型属性会被实例共享**，而这也正是为什么要在构造函数中，而不 是原型对象中定义属性的原因。在通过原型来实现继承时，**原型实例会变成另一个类型的实例**。于是，原先的实例属性也就顺理成章的变成了现在的原型属性了。

```javascript
function Super(){
    this.colors = ['red','green','blue'];
}
Super.prototype.getValue = function(){
    return this.colors
}
function Sub(){};
//Sub继承了Super
Sub.prototype = new Super();
var ins1 = new Super();
ins1.colors.push('black');
console.log(ins1.colors);//['red','green','blue','black'];
var ins2 = new Sub();
console.log(ins2.colors);//['red','green','blue','black'];
```

 原型链的**第二个问题，在创建子类型的实例时，不能向父类型的构造函数传递参数**。实际上，应该说是没有办法在不影响所有都想实例的情况下，给父类型的构造函数传递参数。再加上包含引用类型值的原型属性会被所有实例共享的问题，在实践中很少会单独使用原型链继承

**注意问题**

 使用原型链继承方法要谨慎地定义方法，子类型有时候需要重写父类的某个方法，或者需要添加父类中不存在的某个方法。但不管怎样，给原型添加方法的代码一定要放在替换原型的语句之后。

```javascript
function Super() {
    this.colors = ['red', 'green', 'blue'];
}
Super.prototype.getValue = function() {
    return this.colors
}

function Sub() {
    this.colors = ['black'];
};
//Sub继承了Super
Sub.prototype = new Super();

//添加父类已存在的方法，会重写父类的方法
Sub.prototype.getValue = function() {
    return this.colors;
}
//添加父类不存在的方法
Sub.prototype.getSubValue = function(){
    return false;
}
var ins = new Sub();
//重写父类的方法之后得到的结果
console.log(ins.getValue()); //['black']
//在子类中新定义的方法得到的结果
console.log(ins.getSubValue());//false
//父类调用getValue()方法还是原来的值
console.log(new Super().getValue());//['red', 'green', 'blue']
```

#### 借用构造函数继承

 借用构造函数的技术(有时候也叫做伪类继承或经典继承)。这种技术的基本思想相当简单，即**在子类构造函数的内部调用父类构造函数**。别忘了，函数只不过是在特定环境中执行代码的对象，因此通过使用`apply()`和`call()`方法也可以在新创建的对象上执行构造函数。

```javascript
function Super() {
    this.colors = ['red', 'green', 'blue'];
}
Super.prototype.getValue = function(){
    return this.colors;
}
function Sub(){
    //继承了Super
    Super.call(this);//相当于把构造函数Super中的this替换成了ins实例对象，这样在Super只有定义的私有属性会被继承下来，原型属性中定义的公共方法不会被继承下来
}
var ins = new Sub();
console.log(ins.colors);
```

**传递参数**

 相对于原型链来演，借用构造函数继承有一个很大的优势，即可以在子类构造函数中向父类构造函数传递参数

```actionscript
function B(name){
    this.name = name;
}
function A(){
    
    //继承了B，同时还传递了参数
    B.call(this,'MJJ');
    //实例属性
    this.age = 28;
}
var p = new A();
alert(p.name);//'MJJ'
alert(p.age);//28
```

**借用构造函数的问题**

 如果仅仅是借用构造函数，那么将无法避免构造函数模式存在的问题——方法都在构造函数中定义，因此函数复用就无从谈起。而且，在父类的原型中定义的方法，对子类而言是不可见的。所以这种方式使用较少

#### 组合继承（重要）

组合继承，指的是将原型链和借用构造函数技术组合到一起，从而发挥两者之长的一种继承模式。其背后的思想是使用原型链实现对原型上的公共属性和方法的继承，而通过借用构造函数继承来实现对父类私有属性的继承。这样，即通过在父类原型上定义方法实现了函数复用，又能够保证每个实例都有父类的私有属性。

```javascript
function Super(name){
    this.name = name;
    this.colors = ['red','blue','green'];
}
Super.prototype.sayName = function(){
    alert(this.name);
}
function Sub(name,age){
    Super.call(this,name);
    this.age = age;
}
// 继承方法
Sub.prototype = new Super();
Sub.prototype.constructor = Sub;
Sub.prototype.sayAge = function(){
    alert(this.age);
}
var ins = new Sub('mjj',28);
ins.colors.push('black');
console.log(ins.colors);// ["red", "blue", "green", "black"]
ins.sayName();//'mjj'
ins.sayAge();//28

var ins2 = new Sub('alex',38);
console.log(ins2.colors);//["red", "blue", "green"]
ins2.sayName();//'alex'
ins2.sayAge();//38
```

 在上个例子中，`Sub`构造函数定义了两个属性：`name`和`age`。`Super`的原型定义了一个`sayName()`方法。在`Sub`构造函数中调用`Super`构造函数时传入了name参数，紧接着又定义它自己的属性age。然后，将`Super`的实例赋值给`Sub`的原型，然后又在该新原型上定义了方法`sayAge()`。这样一来，就可以让不同的`Sub`实例分别拥有自己的属性——包括colors属性，又可以使用相同的方法

 组合继承避免了原型链和借用构造函数的缺陷，融合了他们的优点，称为JavaScript中最常用的继承模式。

**组合继承的问题**

无论在什么情况下，都会调用两次父类的构造函数：一次是在创建子类原型的时候，另一次是在子类构造函数内部。

#### 寄生组合式继承

 组合继承是JavaScript最常用的继承模式；不过，它也有自己的不足。组合继承最大的问题就是无论什么情况下，都会调用两次父类构造函数：一次是在创建子类原型的时候，另一次是在子类构造函数内部。没错，子类型最终会包含超类型对象的全部实例属性，但我们不得不在调用子 类型构造函数时重写这些属性。再来看一看下面组合继承的例子。

```javascript
function Super(name){
    this.name = name;
    this.colors = ['red','blue','green'];
}
Super.prototype.sayName = function(){
    alert(this.name);
}
function Sub(name,age){
    Super.call(this,name);
    this.age = age;
}
// 继承方法
Sub.prototype = new Super();
Sub.prototype.constructor = Sub;
Sub.prototype.sayAge = function(){
    alert(this.age);
}
var ins = new Sub('mjj',28);
ins.colors.push('black');
console.log(ins.colors);// ["red", "blue", "green", "black"]
ins.sayName();//'mjj'
ins.sayAge();//28

var ins2 = new Sub('alex',38);
console.log(ins2.colors);//["red", "blue", "green"]
ins2.sayName();//'alex'
ins2.sayAge();//38
```

 所谓寄生组合式继承，即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。其背 后的基本思路是:不必为了指定子类型的原型而调用超类型的构造函数，我们所需要的无非就是超类型 原型的一个副本而已。本质上，就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型 的原型。寄生组合式继承的基本模式如下所示。

```javascript
function Super(name){
    this.name = name;
    this.colors = ['red','blue','green'];
}
Super.prototype.sayName = function(){
    alert(this.name);
}
function Sub(name,age){
    //继承实例属性
    Super.call(this,name);
    this.age = age;
}
// 继承公有的方法
Sub.prototype = Object.create(Super.prototype);
Sub.prototype.constructor = Sub;
Sub.prototype.sayAge = function(){
    alert(this.age);
}
var ins = new Sub('mjj',28);
ins.colors.push('black');
console.log(ins.colors);// ["red", "blue", "green", "black"]
ins.sayName();//'mjj'
ins.sayAge();//28

var ins2 = new Sub('alex',38);
console.log(ins2.colors);//["red", "blue", "green"]
ins2.sayName();//'alex'
ins2.sayAge();//38
```

#### 多重继承

JavaScript中不存在多重继承，那也就意味着一个对象不能同时继承多个对象，但是我们可以通过变通方法来实现。

```xml
<!DOCTYPE html>
<html lang="zh">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title>28 多重继承</title>
</head>
<body>
	<script type="text/javascript">
	// 多重继承:一个对象同时继承多个对象
	// Person  Parent  Me
	function Person(){
		this.name = 'Person';
	}
	Person.prototype.sayName = function(){
		console.log(this.name);
	}
	// 定制Parent
	function Parent(){
		this.age = 30;
	}
	Parent.prototype.sayAge = function(){
		console.log(this.age);
	}
	
	function Me(){
		// 继承Person的属性
		Person.call(this);
		Parent.call(this);
	}
	// 继承Person的方法
	Me.prototype = Object.create(Person.prototype);
	// 不能重写原型对象来实现 另一个对象的继承
	// Me.prototype = Object.create(Parent.prototype);
	// Object.assign(targetObj,copyObj)
	Object.assign(Me.prototype,Parent.prototype);
	
	// 指定构造函数
	Me.prototype.constructor = Me;
	var me = new Me();
	
	</script>
</body>
</html>
```

### Object对象中的相关方法

`Object`对象的原生方法分成两类：`Object`本身的方法与`Object`的实例方法。

所谓本身的方法就是直接定义在当前构造函数对象`Object`对象上的

```awk
//例子
Object.xxx()
```

`Object`实例方法就是定义在`Object.prototype`的方法。它可以被`Object`实例直接共享

```javascript
Object.prototype.hello = function(){
    console.log('hello');
}
var obj = new Object();
obj.hello();//hello
```

#### Object的静态方法

所谓`静态方法`，是指定制在`Object`对象本身的方法

以下两个方法的作用都是**用来遍历对象的属性**

##### Object.keys()

`Object.keys`方法的参数是一个对象，返回一个数组。该数组的成员都是对象自身的所有属性名

```awk
var arr = ['a','b','c'];
Object.keys(arr);//['0','1','2'];
var obj = {
    0:'a',
    1：'b',
    2: 'c'
}
Object.keys(obj);//['0', '1', '2']
```

##### Object.getOwnPropertyNames()

`Object.getOwnPropertyNames`方法与`Object.keys`类似，也是接受一个对象作为参数，返回一个数组，包含了该对象自身的所有属性名。

```awk
var obj = {
    0:'a',
    1：'b',
    2: 'c'
}
Object.getOwnPropertyNames(obj);// ["0", "1", "2"]
```

对于一般的对象来说，`Object.keys()`和`Object.getOwnPropertyNames()`返回的结果是一样的。只有涉及不可枚举属性时，才会有不一样的结果。`Object.keys`方法只返回可枚举的属性，`Object.getOwnPropertyNames`方法还返回不可枚举的属性名。

```awk
var arr = ['a','b','c'];
Object.getOwnPropertyNames(arr);//['0','1','2','length'];
```

上面代码中，数组的`length`属性是不可枚举的属性，所以只出现在`Object.getOwnPropertyNames`方法的返回结果中。

由于 JavaScript 没有提供计算对象属性个数的方法，所以可以用这两个方法代替。

```stylus
var obj = {
    0:'a',
    1：'b',
    2: 'c'
}
Object.keys(obj).length //3
Object.getOwnPropertyNames(obj).length //3
```

一般情况下，使用`Object.keys`方法遍历对象的属性应用最多。

##### Object.getPrototypeOf()

`Object.getPrototypeOf`方法返回参数对象的原型。这是获取原型对象的标准方法

```abnf
var Fn = function(){};
var f1 = new Fn();
console.log(Object.getPrototypeOf(f1) === Fn.prototyoe);//true
```

上面代码中，实例对象`f`的原型是`F.prototype`。

下面是几种特殊对象的原型。

```javascript
// 空对象的原型是 Object.prototype
Object.getPrototypeOf({}) === Object.prototype // true

// Object.prototype 的原型是 null
Object.getPrototypeOf(Object.prototype) === null // true

// 函数的原型是 Function.prototype
function f() {}
Object.getPrototypeOf(f) === Function.prototype // true
```

##### Object.setPrototypeOf()

`Object.setPrototypeOf`方法接收两个参数，第一个是现有对象，第二个是原型对象

```reasonml
var a = {};
var b = {x : 1};
Object.setPrototypeOf(a,b);
console.log(Object.getPrototypeOf(a));//{x:1}
a.x //1
```

`new`命令可以使用`Object.setPrototypeOf`方法模拟。

```javascript
var F = function () {
  this.foo = 'bar';
};

//var f = new F();
// 等同于
var f = Object.setPrototypeOf({}, F.prototype);
F.call(f);
```

##### Object.create()

生成实例对象的常用方法是，使用`new`命令让构造函数返回一个实例。但是很多时候，只能拿到一个实例对象，它可能根本不是由构建函数生成的，那么能不能从一个实例对象，生成另一个实例对象呢？

JavaScript 提供了`Object.create`方法，用来满足这种需求。该方法接受一个对象作为参数，然后以它为原型，返回一个实例对象。该实例完全继承原型对象的属性。

```dart
// 原型对象
var A = {
  print: function () {
    console.log('hello');
  }
};

// 实例对象
var B = Object.create(A);

Object.getPrototypeOf(B) === A // true
B.print() // hello
B.print === A.print // true
```

上面代码中，`Object.create`方法以`A`对象为原型，生成了`B`对象。`B`继承了`A`的所有属性和方法。

#### 其它方法

除了上面提到的两个方法,`Object`还有不少的其它静态方法，后文咱们会一一详细讲解

**（1）对象属性模型的相关方法**

- `Object.getOwnPropertyDescriptor()`：获取某个属性的描述对象。
- `Object.defineProperty()`：通过描述对象，定义某个属性。
- `Object.defineProperties()`：通过描述对象，定义多个属性。

**（2）控制对象状态的方法**（了解部分，自行查阅MDN）

- `Object.preventExtensions()`：防止对象扩展。
- `Object.isExtensible()`：判断对象是否可扩展。
- `Object.seal()`：禁止对象配置。
- `Object.isSealed()`：判断一个对象是否可配置。
- `Object.freeze()`：冻结一个对象。
- `Object.isFrozen()`：判断一个对象是否被冻结。

#### Object的实例方法

方法定义在`Object.prototype`对象上。我们称之为实例方法，所有`Object`的实例对象都继承了这些方法

`Object`实例对象的方法，主要有以下六个。

- `Object.prototype.valueOf()`：返回当前对象对应的值。
- `Object.prototype.toString()`：返回当前对象对应的字符串形式。
- `Object.prototype.toLocaleString()`：返回当前对象对应的本地字符串形式。
- `Object.prototype.hasOwnProperty()`：判断某个属性是否为当前对象自身的属性，还是继承自原型对象的属性。
- `Object.prototype.isPrototypeOf()`：判断当前对象是否为另一个对象的原型。
- `Object.prototype.propertyIsEnumerable()`：判断某个属性是否可枚举。

##### Object.prototype.valueOf()

`valueOf`方法的作用是返回一个对象的`值`,默认情况下返回对象本身

```abnf
var obj = new Object();
obj.valueOf() === obj;//true
```

`valueof`方法的主要用途是，JavaScript自动类型转换时会默认调用这个方法

```arcade
var obj = new Object();
//JavaScript就会默认调用valueOf()方法，求出obj的值再与1相加
console.log(1+obj);//"1[object Object]"
```

所以，如果自定义`valueOf`方法，就可以得到想要的结果

```arcade
var obj = new Object();
obj.valueOf = function(){
    return 2;
}
console.log(1 + obj)；//3
```

**原理**：用自定义的`Object.valueOf`,覆盖`Object.prototype.valueOf`

##### Object.prototype.toString()

`toString`方法的作用是返回一个对象的字符串形式，默认返回类型字符串

```reasonml
var obj1 = new Object();
console.log(obj1.toString());//"[object Object]"
var obj2 = {a:1};
obj2.toString() // "[object Object]"
```

返回的这个结果说明了对象的类型。

字符串`[object Object]`本身没有太大的用处，但是通过自定义`toString`方法，可以让对象在自动类型转换时，得到想要的字符串形式。

```arcade
var obj = new Object();
obj.toString = function(){
    return 'hello';
}
console.log(obj + '' + 'world');//"hello world"
```

像数组、字符串、函数、Date对象都分别定义了自定义的`toString`方法，覆盖了`Object.prototype.toString()`方法

```reasonml
[1, 2, 3].toString() // "1,2,3"
'123'.toString() // "123"

(function () {
  return 123;
}).toString()
// "function () {
//   return 123;
// }"

(new Date()).toString()
// "Tue May 10 2016 09:11:31 GMT+0800 (CST)"
```

**Object.prototype.toLocaleString()**方法跟`toString`方法用法一致。

目前，主要有三个对象自定义了`toLocaleString`方法

- Array.prototype.toLocaleString()
- Number.prototype.toLocaleString()
- Date.prototype.toLocaleString()

举例来说，日期的实例对象的`toString`和`toLocaleString`返回值就不一样，而且`toLocaleString`的返回值跟用户设定的所在地域相关

```arcade
var date = new Date();
date.toString() // "Tue Jan 01 2018 12:01:33 GMT+0800 (CST)"
date.toLocaleString() // "1/01/2018, 12:01:33 PM"
```

##### Object.prototype.isPrototypeOf()

实例对象的`isPrototypeOf`方法，用来判断该对象是否为该对象的原型。

```gcode
var o1 = {};
var o2 = Object.create(o1);
var o3 = Object.create(o2);
o2.isPrototypeOf(o3);//true
o1.isPrototypeOf(o3);//true
```

上面代码中，`o1`和`o2`都是`o3`的原型。这表明只要实例对象处在参数对象的原型链上，`isPrototypeOf`方法都返回`true`。

```javascript
Object.prototype.isPrototypeOf({}) // true
Object.prototype.isPrototypeOf([]) // true
Object.prototype.isPrototypeOf(Object.create(null)) // false
```

上面代码中，由于`Object.prototype`处于原型链的最顶端，所以对各种实例都返回`true`，只有直接继承自`null`的对象除外。

##### `Object.prototype.__proto`__

实例对象的`__proto__`属性（前后各两个下划线），返回该对象的原型。该属性可读写。

```abnf
var obj = {};
var p = {};

obj.__proto__ = p;
Object.getPrototypeOf(obj) === p // true
```

上面代码通过`__proto__`属性，将`p`对象设为`obj`对象的原型。

根据语言标准，`__proto__`属性只有浏览器才需要部署，其他环境可以没有这个属性。它前后的两根下划线，表明它本质是一个内部属性，不应该对使用者暴露。因此，应该尽量少用这个属性，而是用`Object.getPrototypeOf()`和`Object.setPrototypeOf()`，进行原型对象的读写操作。

##### Object.prototype.hasOwnProperty

`Object.prototype.hasOwnProperty`方法接受一个字符串作为参数，返回一个布尔值，表示该实例对象自身是否具有该属性。

```awk
var obj = {
    a: 123
}
obj.hasOwnProperty('b');//false
obj.hasOwnProperty('a');//true
obj.hasOwnProperty('toString');//false
```

上面代码中，对象`obj`自身具有`a`属性，所以返回`true`。`toString`属性是继承的，所以返回`false`。

#### 属性描述对象

JavaScript提供了一个内部数据结构，用来描述对象的属性，控制它的行为，比如该属性是否可写、可遍历等等。这个内部数据结构称之为"属性描述对象"。每个属性都有自己对应的属性描述对象，保存该属性的一些元信息

下面是属性描述对象的一个例子

```yaml
{
  value: 123,
  writable: false,
  enumerable: true,
  configurable: false,
  get: undefined,
  set: undefined
}
```

属性描述对象提供了6个元属性

| 属性         | 含义                                                         |
| ------------ | ------------------------------------------------------------ |
| value        | `value`是该属性的属性值，默认为`undefined`。                 |
| writable     | `writable`是一个布尔值，表示属性值（value）是否可改变（即是否可写），默认为`true`。 |
| enumerable   | `enumerable`是一个布尔值，表示该属性是否可遍历，默认为`true`。如果设为`false`，会使得某些操作（比如`for...in`循环、`Object.keys()`）跳过该属性。 |
| configurable | `configurable`是一个布尔值，表示可配置性，默认为`true`。如果设为`false`，将阻止某些操作改写该属性，比如无法删除该属性，也不得改变该属性的属性描述对象（`value`属性除外）。也就是说，`configurable`属性控制了属性描述对象的可写性。 |
| get          | `get`是一个函数，表示该属性的取值函数（getter），默认为`undefined`。 |
| set          | `set`是一个函数，表示该属性的存值函数（setter），默认为`undefined`。 |

##### Object.getOwnPropertyDescriptor()

`Object.getOwnPropertyDescriptor()`方法可以获取属性描述对象。它的第一个参数是目标对象，第二个参数是一个字符串，对应目标对象的某个属性名。

```reasonml
var obj = {name:'MJJ'};
Object.getOwnPropertyDescriptor(obj,'name');
/*
{
configurable: true
enumerable: true
value: "MJJ"
writable: true
}
*/
//toString为继承来的属性，无法获取
Object.getOwnPropertyDescriptor(obj,'toString');//undefined
```

> 注意：Object.getOwnPropertyDescriptor()方法只能用于对象自身的属性，不能用于继承的属性

##### Object.defineProperty()

`Object.defineProperty`方法允许通过属性描述对象，定义或修改一个属性，然后返回修改后的对象。

语法如下:

```reasonml
Object.defineProperty(object, propertyName, attributesObject)
```

`Object.defineProperty`方法接受三个参数，依次如下。

- object：属性所在的对象
- propertyName：字符串，表示属性名
- attributesObject：属性描述对象

举例说明，定义`obj.name`可以写成下面这样

```sqf
var obj = Object.defineProperty({},'name',{
    value:'mjj',
    writable:false,
    enumerable:true,
    configurable:false
})
console.log(obj.name);//mjj
obj.name = 'alex';
console.log(obj.name);//mjj
```

上面代码中，`Object.defineProperty()`方法定义了`obj.p`属性。由于属性描述对象的`writable`属性为`false`，所以`obj.p`属性不可写。注意，这里的`Object.defineProperty`方法的第一个参数是`{}`（一个新建的空对象），`p`属性直接定义在这个空对象上面，然后返回这个对象，这是`Object.defineProperty()`的常见用法。

如果属性已经存在，`Object.defineProperty()`方法相当于更新该属性的属性描述对象。

##### Object.defineProperties()

如果一次性定义或修改多个属性，可以使用`Object.defineProperties()`方法

```arcade
var obj = Object.defineProperties({}, {
  p1: { value: 123, enumerable: true },
  p2: { value: 'abc', enumerable: true },
  p3: { 
    get: function () { 
        return this.p1 + this.p2 
    },
    enumerable:true,
    configurable:true
  }
});
console.log(obj.p1);//123
console.log(obj.p2);//"abc"
console.log(obj.p3);//"123abc"
```

上面代码中，`Object.defineProperties()`同时定义了`obj`对象的三个属性。其中，`p3`属性定义了取值函数`get`，即每次读取该属性，都会调用这个取值函数。

注意，一旦定义了取值函数`get`（或存值函数`set`），就不能将`writable`属性设为`true`，或者同时定义`value`属性，否则会报错

```actionscript
var obj = {};

Object.defineProperty(obj, 'p', {
  value: 123,
  get: function() { return 456; }
});
// TypeError: Invalid property.
// A property cannot both have accessors and be writable or have a value

Object.defineProperty(obj, 'p', {
  writable: true,
  get: function() { return 456; }
});
// TypeError: Invalid property descriptor.
// Cannot both specify accessors and a value or writable attribute
```

上面代码中，同时定义了`get`属性和`value`属性，以及将`writable`属性设为`true`，就会报错。

`Object.defineProperty()`和`Object.defineProperties()`参数里面的属性描述对象，`writable`、`configurable`、`enumerable`这三个属性的默认值都为`false`。

```reasonml
var obj = {};
Object.defineProperty(obj, 'foo', {});
Object.getOwnPropertyDescriptor(obj, 'foo')
/*
{
configurable: false,
enumerable: false,
value: undefined,
writable: false,
}
*/
```

上面代码中，定义`obj.foo`时用了一个空的属性描述对象，就可以看到各个元属性的默认值。

##### Object.prototype.propertyIsEnumerable()

实例对象的`propertyIsEnumerable()`方法返回一个布尔值，用来判断某个属性是否可遍历。注意，这个方法只能用于判断对象自身的属性，对于继承的属性一律返回`false`。

```awk
var obj = {};
obj.p = 123;
obj.propertyIsEnumerable('p') // true
obj.propertyIsEnumerable('toString') // false
```

上面代码中，`obj.p`是可遍历的，而`obj.toString`是继承的属性。

#### 元属性

##### value

`value`属性是目标属性的值

```reasonml
var obj = {};
obj.p = 123;

Object.getOwnPropertyDescriptor(obj, 'p').value //123
Object.defineProperty(obj, 'p', { value: 246 });
obj.p // 246
```

上面代码是通过`value`属性，读取或改写`obj.p`的例子。

##### writable

`writable`属性是一个布尔值，决定了目标属性的值（value）是否可以被改变。

```php
var obj = {};

Object.defineProperty(obj, 'a', {
  value: 37,
  writable: false
});
obj.a // 37
obj.a = 25;
obj.a // 37
```

上面代码中，`obj.a`的`writable`属性是`false`。然后，改变`obj.a`的值，不会有任何效果。

注意，正常模式下，对`writable`为`false`的属性赋值不会报错，只会默默失败。但是，严格模式下会报错，即使对`a`属性重新赋予一个同样的值。

```javascript
'use strict';
var obj = {};

Object.defineProperty(obj, 'a', {
  value: 37,
  writable: false
});

obj.a = 37;
// Uncaught TypeError: Cannot assign to read only property 'a' of object
```

上面代码是严格模式，对`obj.a`任何赋值行为都会报错。

如果原型对象的某个属性的`writable`为`false`，那么子对象将无法自定义这个属性。

```dart
var proto = Object.defineProperty({}, 'foo', {
  value: 'a',
  writable: false
});

var obj = Object.create(proto);
obj.foo = 'b';
obj.foo; // 'a'
```

上面代码中，`proto`是原型对象，它的`foo`属性不可写。`obj`对象继承`proto`，也不可以再自定义这个属性了。如果是严格模式，这样做还会抛出一个错误。

但是，有一个规避方法，就是通过覆盖属性描述对象，绕过这个限制。原因是这种情况下，原型链会被完全忽视。

```dart
var proto = Object.defineProperty({}, 'foo', {
  value: 'a',
  writable: false
});

var obj = Object.create(proto);
Object.defineProperty(obj, 'foo', {
  value: 'b'
});

obj.foo // "b"
```

##### enumerable

`enumerable`(可遍历性)返回一个布尔值，表示目标属性是否可遍历。

如果一个属性的`enumerable`为false，下面三个操作不会取到该属性

- `for...in`循环
- `Object.key`方法
- `JSON.stringify`方法

因此，`enumerable`可以用来设置“秘密”属性。

```javascript
var obj = {};

Object.defineProperty(obj, 'x', {
  value: 123,
  enumerable: false
});

obj.x // 123

for (var key in obj) {
  console.log(key);
}
// undefined
Object.keys(obj)  // []
JSON.stringify(obj) // "{}"
```

上面代码中，`obj.x`属性的`enumerable`为`false`，所以一般的遍历操作都无法获取该属性，使得它有点像“秘密”属性，但不是真正的私有属性，还是可以直接获取它的值。

注意，`for...in`循环包括继承的属性，`Object.keys`方法不包括继承的属性。如果需要获取对象自身的所有属性，不管是否可遍历，可以使用`Object.getOwnPropertyNames`方法。

另外，`JSON.stringify`方法会排除`enumerable`为`false`的属性，有时可以利用这一点。如果对象的 JSON 格式输出要排除某些属性，就可以把这些属性的`enumerable`设为`false`。

##### configurable

`configurable`(可配置性）返回一个布尔值，决定了是否可以修改属性描述对象。也就是说，`configurable`为`false`时，`value`、`writable`、`enumerable`和`configurable`都不能被修改了。

```php
var obj = Object.defineProperty({}, 'p', {
  value: 1,
  writable: false,
  enumerable: false,
  configurable: false
});

Object.defineProperty(obj, 'p', {value: 2})
// TypeError: Cannot redefine property: p

Object.defineProperty(obj, 'p', {writable: true})
// TypeError: Cannot redefine property: p

Object.defineProperty(obj, 'p', {enumerable: true})
// TypeError: Cannot redefine property: p

Object.defineProperty(obj, 'p', {configurable: true})
// TypeError: Cannot redefine property: p
```

上面代码中，`obj.p`的`configurable`为`false`。然后，改动`value`、`writable`、`enumerable`、`configurable`，结果都报错。

注意，`writable`只有在`false`改为`true`会报错，`true`改为`false`是允许的。

```php
var obj = Object.defineProperty({}, 'p', {
  writable: true,
  configurable: false
});

Object.defineProperty(obj, 'p', {writable: false})
// 修改成功
```

至于`value`，只要`writable`和`configurable`有一个为`true`，就允许改动。

```php
var o1 = Object.defineProperty({}, 'p', {
 value: 1,
 writable: true,
 configurable: false
});

Object.defineProperty(o1, 'p', {value: 2})
// 修改成功

var o2 = Object.defineProperty({}, 'p', {
 value: 1,
 writable: false,
 configurable: true
});

Object.defineProperty(o2, 'p', {value: 2})
// 修改成功
```

另外，`writable`为`false`时，直接目标属性赋值，不报错，但不会成功。

```php
var obj = Object.defineProperty({}, 'p', {
  value: 1,
  writable: false,
  configurable: false
});

obj.p = 2;
obj.p // 1
```

上面代码中，`obj.p`的`writable`为`false`，对`obj.p`直接赋值不会生效。如果是严格模式，还会报错。

可配置性决定了目标属性是否可以被删除（delete）。

```php
var obj = Object.defineProperties({}, {
  p1: { value: 1, configurable: true },
  p2: { value: 2, configurable: false }
});

delete obj.p1 // true
delete obj.p2 // false

obj.p1 // undefined
obj.p2 // 2
```

上面代码中，`obj.p1`的`configurable`是`true`，所以可以被删除，`obj.p2`就无法删除

#### 存取器

除了直接定义以外，属性还可以用存取器定义。其中，存值函数称为`setter`,使用属性描述对象的`set`属性；取值函数称为`getter`,使用属性描述对象的`get`属性

一旦对目标属性定义了存取器，那么存取的时候，都将执行对应的函数。利用这个功能，可以实现许多高级特性，比如某个属性禁止赋值

```actionscript
var obj = Object.defineProperty({},'p',{
    get:function(){
        return 'getter';
    },
    set:function(value){
        console.log('setter:'+value);
    }
})
obj.p //"getter"
obj.p = 123;//"setter:123"
```

上面代码中，`obj.p`定义了`get`和`set`属性。`obj.p`取值时，就会调用`get`；赋值时，就会调用`set`。

JavaScript还提供了存取器的另一种写法。

```csharp
var obj = {
    get p(){
        return 'getter';
    },
    set p(value){
        console.log('setter:'+ value);
    }
}
```

上面的写法与定义属性描述对象是等价的，而且使用更广泛。

> 注意，取值函数`get`不能接受参数，存值函数`set`只能接受一个参数（即属性的值）。

存取器往往用于，属性的值依赖对象内部数据的场合

```awk
var obj = {
    $n : 5,
    get next(){
        return this.$n++;
    },
    set next(value){
        if(value >= this.$n){
            this.$n = value;
        }else{
            throw new Error('新的值必须大于当前值');
        }
    }
};
obj.next //5
obj.next = 10;
obj.next //10
obj.next = 5;
// Uncaught Error: 新的值必须大于当前值
```

上面代码中，`next`属性的存值函数和取值函数，都必须依赖内部属性`$n`

#### 深浅拷贝

##### 基本类型的拷贝

先开看一段非常经典的代码

```abnf
var a = 1;
var b = a;
a = 200;
console.log(a);//200
console.log(b);//1
```

基本类型是`按值传递`，引用类型`按引用传递`，数值作为基本类型是保存在栈内存中，可以直接拿来用的，赋值时什么就是什么，不会受到传递元素的改变带来影响。

##### 引用类型的拷贝

简单说，引用类型是生成一个指针保存在堆内存中，当给引用类型赋值时，我们的写的是在栈内存中，也就是说我们拿到的其实是一个指针。这个指针是指向了栈内存中这个引用类型的代码。

提到拷贝涉及到的两种拷贝类型：**深拷贝**和**浅拷贝**

```undefined
操作拷贝之后的数据不会影响到原数据的值拷贝，就是深拷贝，反正，有影响则为浅拷贝 
```

###### 浅拷贝

```xquery
var a = {
    name:'mjj',
    age:20,
    hobby:'eat',
    friend:{
        name:'alex',
        age:38
    }
}
function shadowCopy(to,from){
    for(var key in from){
        to[key] = from[key]
    }
    return to;
}
var newObj = shadowCopy({},a);
newObj.age = 18;
newObj.friend.name = '阿黄';
/*
{
	age: 20,//没被改变
	friend: {
		name: "阿黄",//同时被改变，说明是同一个引用
		age: 38
	},
	hobby: "eat"
	name: "mjj
}

*/
```

我们发现，首先，**浅拷贝不是直接赋值**，浅拷贝新建了一个对象，然后将源对象的属性都一一复制过来,复制的是值，而不是引用。

我们知道，对象都是按地址引用进行访问的，浅拷贝的复制只复制了第一层的属性，并没有递归将所有的值复制过来，所以，操作拷贝数据，对原数据产生了影响，故而为浅拷贝。

###### 深拷贝

> 深拷贝就是对目标的完全拷贝，不像浅拷贝那样只是复制了一层引用，就连值也都复制了。
>
> 只要进行了深拷贝，它们老死不相往来，谁也不会影响谁。

使用深拷贝可以使新创建的对象和原来的完全脱离关系

```xquery
var a = {
    name:'mjj',
    age:20,
    hobby:'eat',
    friend:{
        name:'alex',
        age: 38,
        hobby:'鸡汤'
	}	
}
function deepCopy(to,from){
    for(var key in from){
        //不遍历原型链上的属性
        if(from.hasOwnProperty(key)){
            //如果值是对象并且有值，则递归一下
            if(from[key] && typeof from[key] === 'object'){
                //区分是一般对象还是数组对象
               to[key] = from[key].constructor === Array ? []: {};
               to[key] = deepCopy(to[key],from[key]);
            }else{
                //如果不是，就直接赋值
                to[key] = from[key];
            }
        }
        
    }
      return to;
  
}
var newObj = deepCopy(a);
newObj.age = 18;
newObj.friend.name = '阿黄';
```

`hasOwnProperty`属性用来过滤掉继承的属性