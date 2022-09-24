# ES6~ES11笔记



# ES6

#### 1.let变量声明以及声明特性

1.1 变量不能重复声明

```javascript
let star = '罗志祥'

let star = '小猪' //报错
复制代码
```

1.2 块级作用域 全局，函数，eval

```javascript
//if else while for 
{
	let girl = '范爷'
}
console.log(girl) //报错
复制代码
```

1.3不存在变量提升

```javascript
console.log(song) //报错
let song = '恋爱达人'
复制代码
```

1.4不影响作用域链

```javascript
{
	let school = '北大'
	function fn(){
		console.log(school)
	}
	fn()
}
复制代码
```

let经典案例：遍历绑定事件

```javascript
for(let i=0;i<items.length;i++){
	items[i].onclick = function(){
		//this.style.background = 'pink'
		items[i].style.background = 'pink'
	}
}
复制代码
```

#### 2.常量的声明以及声明特性

2.1声明常量

```javascript
const school = '北大'
复制代码
```

2.2一定要赋初始值

```javascript
const A    //报错
复制代码
```

2.3一般常量使用大写(潜规则)

```javascript
const a = 100
复制代码
```

2.4常量的值不能修改

```javascript
school = 'beida' //报错
复制代码
```

2.5块级作用域

```javascript
{
	const PLAYER = 'UZI'
}
console.log(PLAYER) //报错
复制代码
```

2.6对于数组和对象的元素修改，不算做常量的修改，不会报错

```javascript
const TEAM = ['UZI','MXLG','Ming','Letme']
TEAM.push('Meiko')
复制代码
```

#### 3.变量的解构赋值

解构赋值：ES6允许按照一定模式从数组和对象中提取值，对变量进行赋值

3.1数组的解构

```javascript
const F4 = ['小沈阳','刘能','赵四','宋小宝']
let [xiao, liu, zhao, song] = F4
console.log(xiao)
console.log(liu)
console.log(zhao)
console.log(song)
复制代码
```

3.2对象的解构

```javascript
const zhao = {
	name: '赵本山',
	age: '不详',
	xiaopin: function(){
		console.log('我可以演小品')
	}
}
//对象用花括号进行解构
let {name, age, xiaopin} = zhao
console.log(name)
console.log(age)
console.log(xiaopin)
xiaopin()
复制代码
```

3.3如果解构不成功，变量的值就等于`undefined`

```javascript
let [foo] = []
let [bar, foo] = [1]
//以上两种情况都属于解构不成功，foo的值都会等于undefined
复制代码
```

#### 4.模板字符串

ES6引入新的声明字符串的方式 ``, ' ', ""

4.1声明

```javascript
let str = `我也是一个字符串哦!`
console.log(str,typeof str)
复制代码
```

4.2内容中可以直接出现换行符

```javascript
let str = `<ul>
			<li>沈腾</li>
			<li>玛丽</li>
			<li>艾伦</li>
			</ul>`			
复制代码
```

4.3变量拼接（常用）

```javascript
let lovest = '魏翔'
let out = `${lovest}是最搞笑的演员!!`
console.log(out)
复制代码
```

#### 5.简化对象写法

ES6 允许在大括号里面直接写入变量和函数，作为对象的属性和方法

```javascript
let name = '北大'
let change = function(){
	console.log('最高学府!')
}

const school = {
	name,
	change,
	//方法的简化写法
	impove(){
		console.log('提高你的技能')
	}
}
复制代码
```

#### 6.箭头函数

ES6 允许使用 [箭头] （=>）定义函数

```javascript
//声明一个函数
//let fn =function(){
//
//}
let fn = (a,b) => {
	return a+b
}
//调用函数
let result= fn(1,2)
console.log(result)  //3
复制代码
```

6.1this是静态的，this始终指向函数声明时所在作用域下的this的值

```javascript
function getName(){
    console.log(this.name)
}
let getName2 = () => {
    console.log(this.name)
}

//设置 window 对象的 name 属性值
window.name = '北大'
const school = {
    name: '清华'
}

//直接调用
getName()  //北大
getName2() //北大

//call 方法调用
getName.call(school)  //清华
getName2.call(school) //北大
复制代码
```

6.2不能作为构造函数实例化对象

```javascript
let Person = (name, age) => {
    this.name = name
    this.age = age
}
let me = new Person('xiao',17)
console.log(me) //报错
复制代码
```

6.3不能使用 arguments 变量

```javascript
let fn = () =>{
    console.log(arguments)
}
fn(1,2,3) //报错
复制代码
```

6.4箭头函数的简写

```javascript
//1)省略小括号, 当形参有且只有一个的时候
let add = n => {
    return n + n
}
console.log(add(9)) //18
//2)省略花括号, 当代码体只有一条语句的时候, 此时 return 必须省略
//  而且语句的执行结果就是函数的返回值
let pow = n => n * n
console.log(pow(8)) //64
复制代码
```

6.5箭头函数实践

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>箭头函数实践</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background: #58a;
        }
    </style>
</head>
<body>
    <div id="ad"></div>
    <script>
        //需求-1 点击 div 2s 后颜色变成粉色
        let ad = document.getElementById('ad')
        ad.addEventListener('click',function(){
            //保存 this 的值
            // let _this = this
            //定时器
            setTimeout(() => {
                //修改背景颜色 this
                // console.log(this)
                // _this.style.background = 'pink'
                this.style.background = 'pink' //this指向ad
            }, 2000)
        })

        //需求-2 从数组中返回偶数的元素
        const arr = [1,6,9,10,100,25]
        // const result = arr.filter(function(item){
        //     if(item%2 === 0){
        //         return true
        //     }else{
        //         return false
        //     }
        // }) 

        const result = arr.filter(item => item%2 === 0)

        console.log(result)

        //箭头函数适合与 this 无关的回调. 定时器, 数组的方法回调
        //箭头函数不适合与 this 有关的回调. 事件回调, 对象的方法
    </script>
</body>
</html>
复制代码
```

#### 7.函数参数默认值

ES6允许给函数参数赋值初始值

7.1形参初始值 具有默认值的参数, 一般位置要靠后(潜规则)

```javascript
function add(a,b,c=10){
    return a+b+c
}
let result = add(1,2)
console.log(result) //13
复制代码
```

7.2与解构赋值结合

```javascript
function connect({host='127.0.0.1', username, password, port}){
    console.log(host)
    console.log(username)
    console.log(password)
    console.log(port)
}
connect({
    host: 'localhost',
    username: 'root',
    password: 'root',
    port: '3306'
})
复制代码
```

#### 8.rest参数

ES6 引入 rest 参数，用于获取函数的实参，用来代替 arguments

ES5 获取实参的方式

```javascript
function date(){
    console.log(arguments)
}
date('柏芝','阿娇','思慧')
复制代码
```

，必须放到最后，否则会报错

```javascript
function date(...args){
    console.log(args)  //数组形式 可使用filter some every map 等方法
}
date('柏芝','阿娇','思慧')
复制代码
```

#### 9.扩展运算符

[...] 扩展运算符能将 [数组] 转换为逗号分隔的 [参数序列]

```javascript
const tfboys = ['易烊千玺','王源','王俊凯']
function chunwan(){
    console.log(arguments)
}
chunwan(...tfboys) //chuanwan('易烊千玺','王源','王俊凯')
复制代码
```

扩展运算符应用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>扩展运算符应用</title>
</head>
<body>
    <div></div>
    <div></div>
    <div></div>
    <script>
        //1.数组的合并
        const kuaizi = ['王太利','肖央']
        const fenghuang = ['曾毅','玲花']
        // const zuixuanxiaopingguo = kuaizi.concat(fenghuang)
        const zuixuanxiaopingguo = [...kuaizi,...fenghuang]
        console.log(zuixuanxiaopingguo)

        //2. 数组的克隆 浅拷贝
        const sanzhihua = ['E','G','M']
        const sanyecao = [...sanzhihua]
        console.log(sanyecao)

        //3. 将伪数组转为真正的数组
        const divs = document.querySelectorAll('div')
        const divArr = [...divs]
        console.log(divArr)
    </script>
</body>
</html>
复制代码
```

#### 10.Symbol

10.1 ES6引入了一种新的原始数据类型Symbol， 表示独一无二的值。

它是JavaScript语言的第七种数据类型，是一种类似于字符串的数据类型。 Symbol特点 1)Symbol的值是唯一的，用来解决命名冲突的问题 2)Symbol值不能与其他数据进行运算 3) Symbol 定义的对象属性不能使用for..in 循环遍历，但是可以使用 Reflect.ownKeys来获取对象的所有键名

```javascript
//创建Symbol
let s = Symbol()
//console.log(s,typeof s)
let s2 = Symbol('北大')
let s3 = Symbol('北大')
console.log(s2 === s3) //false
//Symbol.for 创建
let s4 = Symbol.for('北大')
let s5 = Symbol.for('北大')
console.log(s4 === s5) //true

//不能与其他数据进行运算
//let result = s + 100
//let result = s > 100
//let result = s + s

//js 7种原始数据类型
//u undefined
//s string symbol
//o object
//n number null
//b boolean
复制代码
```

10.2 Symbol 创建对象属性

```javascript
//向对象中添加方法 up down
let gane = {
    name: '俄罗斯方块',
    up: function(){},
    down: function(){}
}

//声明一个对象
//let methods = {
//    up: Symbol(),
//    down: Symbol()
//}

//game[method.up] = function(){
//    console.log('改变形状')
//}
//game[method.down] = function(){
//    console.log('快速下降')
//}

let youxi = {
    name: '狼人杀',
    [Symbol('say')]: function(){
        console.log('发言')
    },
    [Symbol('zibao')]: function(){
        console.log('自爆')
    }
}

console.log(youxi)

复制代码
```

10.3 Symbol内置属性

```javascript
class Person{
    static [Symbol.hasInstance](param){
        console.log(param)
        console.log('我被用来检测类型了')
        return false
    }
}
let o = {}

console.log(o instanceof Person) //


const arr = [1,2,3]
const arr2 = [4,5,6]
arr2[Symbol.isConcatSpreadable] = false //不展开
console.log(arr.concat(arr2)) //[1,2,3,Array(3)]
```

#### 11迭代器 Iterator

11.1迭代器(Iterator)是一种接口,为各种不同的数据结构提供统一的访问机制。 任何数据结构只要部署Iterator 接口，就可以完成遍历操作。

1)ES6 创造了一-种新的遍历命令for...of循环，Iterator 接口主要供for...of消费

2)原生具备iterator接口的数据(可用for of遍历) a) Array b) Arguments c) Set d) Map e) String f ) TypedArray g) NodeList

3)工作原理 a)创建一个指针对象， 指向当前数据结构的起始位置 b)第一次调用对象的next方法，指针自动指向数据结构的第一个成员 c) 接下来不断调用next方法，指针一直往后移动，直到指向最后一个成员 d)每调用next方法返回一个包含value和done属性的对象 **注:需要自定义遍历数据的时候，要想到迭代器。**

```javascript
//声明一个数组
const xiyou = ['唐僧','孙悟空','猪八戒','沙僧']

//使用 for...of 遍历数组 键值/for...in 遍历 键名
for(let v of xiyou){
    console.log(v) 
}

let iterator = xiyou[Symbol.iterator]()

//调用对象的next方法
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())
复制代码
```

11.2 自定义遍历数据

```javascript
//声明一个对象
const banji = {
    name: '终极一班',
    stus: [
        'xiaoming',
        'xiaotian',
        'xiaodong',
        'xiaonan'
    ],
    [Symbol.iterator](){
        //索引变量
        let index = 0
        //保存this
        let _this = this
        return {
            next: function(){
                if(index < _this.stus.length){
                    const result = { value: _this.stus[index],done: false}
                    //下标自增
                    index++
                    return result
                }else{
                    return {value: undefined,done: true}
                }
            }
        }
    }
}
//遍历这个对象
for(let v for banji){
    console.log(v)
}
复制代码
```

#### 12.生成器

12.1生成器函数是ES6提供的一种异步编程解决方案，语法行为和传统函数完全不同

```javascript
//生成器函数其实就是一个特殊的函数
//异步编程 纯回调函数
//yield 函数代码的分隔符
function * gen(){
    //console.log('111')
    yield '一只没有耳朵'
    //console.log('222')
    yield '一只没有尾巴'
    //console.log('333')
    yield '真奇怪'
    //console.log('444')
}

let iterator = gen()
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())

//遍历
//for(let v of gen()){
//    console.log(v)
//}
复制代码
```

12.2生成器函数参数传递

```javascript
function * gen(arg){
    console.log(arg)
    let one = yield 111
    console.log(one)  //BBB
    let two = yield 222
    console.log(two)  //CCC
    let three = yield 333
    console.log(three)  //DDD
}

let iterator = gen('AAA')
console.log(iterator.next())
//next方法可以传入实参
console.log(iterator.next('BBB'))  //传入的参数作为第一个yeild 的返回结果
console.log(iterator.next('CCC'))  //传入的参数作为第二个yeild 的返回结果
console.log(iterator.next('DDD'))  //传入的参数作为第三个yeild 的返回结果
复制代码
```

12.3生成器函数实例

```javascript
//异步编程 文件操作 网络操作(ajax,request) 数据库操作
//1s 后控制台输出111 2s后输出222 3s后输出333
//回调地狱
setTimeout(()=>{
    console.log(111)
    setTimeout(()=>{
    	console.log(222)
    	setTimeout(()=>{
    		console.log(333)
		},3000)
	},2000)
},1000)

function one(){
    setTimeout(()=>{
    	console.log(111)
        iterator.next()
	},1000)
}
function two(){
    setTimeout(()=>{
    	console.log(222)
        iterator.next()
	},2000)
}
function three(){
    setTimeout(()=>{
    	console.log(333)
        iterator.next()
	},3000)
}

function * gen(){
    yield one()
    yield two()
    yield three()
}

let iterator = gen()
iterator.next()
复制代码n'b'v
//模拟获取 用户数据 订单数据 商品数据
function getUsers() {
    setTimeout(()=>{
        let data = '用户数据'
        //调用next方法,并且将数据传入
        iterator.next(data)
    },1000)
}
function getOrders() {
    setTimeout(()=>{
        let data = '订单数据'
        //调用next方法,并且将数据传入
        iterator.next(data)
    },1000)
}
function getGoods() {
    setTimeout(()=>{
        let data = '商品数据'
        //调用next方法,并且将数据传入
        iterator.next(data)
    },1000)
}

function * gen(){
    let users = yield getUsers()
    let orders = yield getOrders()
    let goods = yield getGoods()
}

let iterator = gen()
iterator.next()
复制代码
```

#### 13.Promise

13.1 Promise是ES6引入的异步编程的新解决方案。语法上Promise是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果。

1. Promise 构造函数: Promise (excutor) {}
2. Promise.prototype.then 方法 链式调用
3. Promise.prototype.catch 方法

```javascript
//实例化 Promise 对象
const p = new Promise(function(resolve,reject){
    setTimeout(()=>{
        let data = '数据库中的用户数据'
        //指定成功的回调
        resolve(data)
        
        let err = '数据读取失败'
        //指定失败的回调
        reject(err)
    },1000)
})

//调用promise对象的then方法
p.then(function(value){//执行成功的回调
    console.log(value)
},function(reason){//执行失败的回调
    console.error(reason)
})
复制代码
```

13.2 Promise对象的catch方法

```javascript
const p = new Promise((resolve,reject)=>{
	setTimeout(()=>{
		//设置p对象的状态为失败,并设置失败的值
		reject('出错了！')
	},1000)
}) 

//p.then(function(value){},function(reason){
//	console.error(reason)
//})

p.catch(function(reason){
    console.warn(reason)
})
复制代码
```

13.3 Promise读取文件 需要node环境

```javascript
//1.引入fs模块
const fs = require('fs')

//2.调用方法读取文件
//fs.readFile('./resources/为学.md',(err,data)=>{
    //如果失败,则抛出错误
    //if(err) throw err
    //如果没有错误，则输出内容
    //console.log(data.toString())
//})

//3.使用Promise封装
const p = new Promise((resolve,reject)=>{
    fs.readFile('./resources/为学.md',(err,data)=>{
    	//如果失败
    	if(err)  reject(err)
    	//如果成功
    	resolve(data)
	})
})

p.then(funstion(value){
    console.log(value.toString())   
},function(reason){
    console.log('读取失败！')
})
复制代码
```

13.4 Promise-then方法

```javascript
//创建promise对象
const p = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('用户数据')
        //reject('出错了！')
    },1000)
})

//掉用then方法 then方法的返回结果是 Promise 对象，对象的状态由回调函数的执行结果决定
//如果回调函数中返回的结果是 非 promise 类型的属性，状态为成功，返回值为对象的成功的值

const result = p.then(value=>{
    console.log(value)
    //1. 非 promise 类型的属性
    //return '123'
    //2. 是 promise 对象
    return new Promise((resolve,reject)=>{
        //resolve('ok')
        reject('error')
    })
    //3.抛出错误
    //throw new Error('出错了！')
    //throw '出错了！'
},reason=>{
    console.warn(reason)
})

console.log(reasult)
复制代码
```

13.5 Promise实践-读取多个文件

```javascript
//引入fs模块
const fs = require('fs')

fs.readFile('./resources/为学.md',(err,data1)=>{
    fs.readFile('./resources/插秧诗.md',(err,data2)=>{
    	fs.readFile('./resources/观书有感.md',(err,data3)=>{
    		let result = data1 + '\r\n' + data2 + '\r\n' + data3
            console.log(result)
		})
	})
})


//使用 promise 实现
const p = new Promise((resolve,reject)=>{
    fs.readFile('./resources/为学.md',(err,data)=>{
    	resolve(data)
    })
})

p.then(value=>{
    return new Promise((resolve,reject)=>{
         fs.readFile('./resources/插秧诗.md',(err,data)=>{
    		resolve([value,data]) //向下传递
    	})
    })
}).then(value=>{
    return new Promise((resolve,reject)=>{
         fs.readFile('./resources/观书有感.md',(err,data)=>{
    		 //压入
             value.push(data)
             resolve(value)
    	})
    })
}).then(value=>{
    console.log(value.join('\r\n'))
})
复制代码
```

#### 14.Set集合

14.1 ES6提供了新的数据结构Set (集合)。它类似于数组，但成员的值都是唯一的，集合实现了iterator接口，所以可以使用「扩展运算符」和 for...of进行遍历， 集合的属性和方法:

1. size 返回集合的元素个数
2. add 增加一个新元素，返回当前集合
3. delete 删除元素，返回boolean值
4. has 检测集合中是否包含某个元素，返回boolean值

```javascript
//声明一个 set
let s = new Set()
let s2 = new Set(['大事儿','小事儿','好事儿','坏事儿','小事儿'])

//元素个数
console.log(s2.size)
//添加新元素
s2.add('喜事儿')
//删除元素
s2.delete('坏事儿')
//检测
console.log(s2.has('糟心事'))
//清空
s2.clear()

for(let v of s2){
    console.log(v)
}
复制代码
```

14.2 Set 实践

```javascript
let arr = [1,2,3,4,5,4,3,2,1]
//1.数组去重
let result = [...new Set(arr)]
//2.交集
let arr2 = [4,5,6,5,6]
let result = [...new Set(arr)].filter(item => new Set(arr2).has(item))
//3.并集
let union = [...new Set([...arr,...arr2])]
console.log(union)
//4.差集
let diff = [...new Set(arr)].filter(item => !(new Set(arr2).has(item)))
复制代码
```

#### 15.Map

 ES6提供了Map数据结构。它类似于对象，也是键值对的集合。但是“键”的范围不限于字符串，各种类型的值(包括对象)都可以当作键。Map也实现了iterator 接口，所以可以使用「扩展运算符」和 for... of 进行遍历。Map的属性和方法:

1. size 返回Map的元素个数|
2. set 增加一个新元素，返回当前Map
3. get 返回键名对象的键值
4. has 检测Map中是否包含某个元素，返回boolean值
5. clear 清空集合，返回undefined

```javascript
//声明 Map
let m = new Map()

//添加元素
m.set('name','北大')
m.set('change',function(){
    console.log('改变人生')
})
let key = {
    school: 'beida'
}
m.set(key,['北京','上海','深圳'])

//size
console.log(m.size)

//删除
m.delete('name')

//获取
console.log(m.get('change'))

//清空
m.clear()

//遍历
for(let v of m){
    console.log(v)
}
复制代码
```

#### 16.class类

16.1 ES6提供了更接近传统语言的写法，引入了Class (类)这个概念，作为对象的模板。通过class关键字，可以定义类。基本上，ES6 的class可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。 知识点:

1. class 声明类
2. constructor 定义构造函数初始化
3. extends 继承父类
4. super 调用父级构造方法
5. static定义静态方法和属性
6. 父类方法可以重写

```javascript
//ES5 通过构造函数创建对象
function Phone(brand,price){
    this.brand = brand
    this.price = price
}

//添加方法
Phone.prototype.call = function(){
    console.log('我可以打电话')
}

//实例化对象
let Huawei = new Phone('华为',5999)
Huawei.call()

//ES6 class
class Shouji{
    //构造方法
    constructor(brand,price){
        this.brand = brand
    	this.price = price
    }
    //方法必须使用该语法，不能使用ES5的对象完整式
    call(){
        console.log('我可以打电话')
    }
}

let onePlus = new Shouji('1+',1999)
consloe.log(onePlus)
复制代码
```

16.2 类的静态成员

```javascript
//function Phone(){
//    
//}
//Phone.name = '手机'
//Phone.change = function(){
//   console.log('改变世界')
//}
//Phone.prototype.size = '5.5inch' 
//let nokia = new Phone()

//console.log(nokia.name) //undefined
//console.log(nokia.size) //5.5inch

class Phone{
    //静态属性
    static name = '手机'
	static change(){
        console.log('改变世界')
    }
}
let nokia = new Phone()
console.log(nokia.name) //undefined
console.log(Phone.name) //手机
复制代码
```

16.3对象继承

```javascript
//ES5对象继承
//手机
function Phone(brand,price){
    this,brand = brand
    this.price = price
}

Phone.prototype.call = function(){
    console.log('打电话')
}

//智能手机
function SmartPhone(brand,price,color,size){
    Phone.call(this,brand,price) //调用call方法，继承Phone的属性
    this.color = color
    this.size = size
}

//设置子级构造函数的原型
SmartPhone.prototype = new Phone

//声明子类的方法
SmartPhone.prototype.photo = function(){
    conosle.log('拍照')
}

const chuizi = new SmartPhone('锤子',2499,'黑色','5.5inch')

console.log(chuizi)
复制代码
class Phone(){
    //构造方法
    constructor(brand,price){
          this,brand = brand
    	  this.price = price
    }
    //父类的成员属性
    call(){
         console.log('打电话')
    }
}

//类的继承和方法重写
class SmartPhone extends Phone {
    //构造方法
    constructor(brand,price,color,size){
        super(brand,price) //Phone.call(this,brand,price) 
        this.color = color
    	this.size = size
    }
    
    photo(){
        console.log('拍照')
    }
    
    //重写父类的方法
    call(){
        console.log('视频通话')
    }
}

const xiaomi = new SmartPhone('小米',799,'黑色','4.7inch')
console.log(xiaomi)
xiaomi.call()  //视频通话
xiaomi.photo() //拍照
复制代码
```

16.4 get 和 set

```javascript
class Phone{
    get price(){
        console.log('价格属性被读取了')
        return 'iloveyou'
    }
    set price(newVal){
        console.log('价格属性被修改了')
    }
}

//实例化对象
let s = new Phone()

//console.log(s.price) //触发get方法
s.price = 'free'  //触发set方法
复制代码
```

#### 17.数值扩展

```javascript
//0. Number.EPSILON 是 JavaScript 表示的最小精度
//EPSILON 属性的值接近于 2.220446049250313e-16
function equal(a,b){
    if(Math.abs(a-b) < Number.EPSILON){
        return true
    }else{
        return false
    }
}
console.log(equal(01+0.2,0.3)) //true

//1.二进制和八进制 十进制和十六进制
let b = 0b1010
let o = 0o777
let d = 100
let x = oxff

//2.Number.isFinite 检测一个数是否为有限数
console.log(Number.isFinite(100))    	//true
console.log(Number.isFinite(100/0))		//false
console.log(Number.isFinite(Infinity))	//fasle

//3.Number.isNaN 检测一个数值是否为NaN
console.log(Number.isNaN(123))

//4.Number.parseInt Number.parseFloat 字符串转整数、浮点数
console.log(Number.parseInt('5201314love'))  //5201314
console.log(Number.parseFloat('3.1415926神奇')) //3.1415926

//5.Number.isInteger 判断一个数是否为整数
consoloe.log(Number.isInteger(5))   //true
consoloe.log(Number.isInteger(2.5)) //false

//6.Math.trunc 将数字的小数部分抹掉
consoloe.log(Math.trunc(3.5)) //3

//7.Math.sign 判断一个数到底是正数 负数 还是零
console.log(Math.sign(100))  // 1
console.log(Math.sign(0))    // 0
console.log(Math.sign(-1000))// -1
复制代码
```

#### 18.对象方法扩展

```javascript
//1.Object.is 判断两个值是否完全相等
console.log(Object.is(120,120)) //true
console.log(Object.is(NaN,NaN)) //true
console.log(NaN === NaN) 		//false

//2.Object.assign 对象的合并
const config1 = {
    host: 'localhost',
    port: 3306,
    name: 'root',
    pass: 'root',
    text: 'test'
}
const config2 = {
    host: 'http://baidu.com',
    port: 33060,
    name: 'baidu.com',
    pass: 'iloveyou',
    text2: 'test2'
}
console.log(Object.assign(config1,config2))  //config2覆盖config1相同的属性，不同的属性会保留下来

//3.Object.setPrototypeOf 设置原型对象 Object.getPrototypeOf 获取原型对象
const school = {
    name: '北大'
}
const cities = {
    xiaoqu: ['北京','上海','深圳']
}

Object.setPrototypeOf(school,cities) //不建议这么做
console.log(Object.getPrototypeOf(school))
console.log(school)
复制代码
```

#### 19.模块化

19.1 模块化是指将一个大的程序文件,拆分成许多小的文件,然后将小文件组合起来。 19.2 模块化的好处 模块化的优势有以下几点:

1. 防止命名冲突
2. 代码复用
3. 高维护性

19.3 ES6模块化语法

模块功能主要由两个命令构成: export 和import。 ●export命令用于规定模块的对外接口 ●import命令用于输入其他模块提供的功能

```javascript
//m1.js
//分别暴露
export let school = '北大'

export function teach(){
    console.log('教你技能')
}
复制代码
//m2.js
//统一暴露
let school = '北大'
function findJob(){
    console.log('找工作')
}

export {school,findJob}
复制代码
//m3.js
//默认暴露
export default{
    school: '北大',
    change: function(){
        console.log('改变你')
    }
}
复制代码
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ES6 模块化</title>
</head>
<body>
    <script type="module">
        //1.通用的导入方式
        //引入 m1.js 模块内容
        import * as m1 from "./src/js/m1.js"
        //引入 m2.js 模块内容
        import * as m2 from "./src/js/m2.js"
        //引入 m3.js 模块内容
        import * as m3 from "./src/js/m3.js"
       
        console.log(m3)
        m3.default.change()
        
        //2.解构赋值形式
        import {school, teach} from "./src/js/m1.js"
        import {school as beida, findJob} from "./src/js/m2.js"
        import {dafault as m3} from "./src/js/m3.js"
        
        //3.简编形式  针对默认暴露 (常用)
        import ms from "./src/js/m3.js"
        console.log(m3)
        m3.change()
    </script>
</body>
</html>
复制代码
```

# ES7

#### 1.includes 和 ** 幂运算

```javascript
// indexOf 返回的是数字
const mingzhu = ['西游记','红楼梦','三国演义','水浒传']

//判断
console.log(mingzhu.includes('西游记'))  //true
console.log(mingzhu.includes('金瓶梅'))  //false

// **
console.log(2 ** 10) //1024
console.log(Math.pow(2,10))  //1024
复制代码
```

# ES8

#### 1.async 和 await

async 和 await 两种语法的结合可以让异步代码像同步代码一样。

#### 2.async 函数

1）async 函数的返回值为promise对象

2）promise 对象的结果由 async 函数执行的返回值决定

```javascript
//async 函数
async function fn(){
	//返回一个字符串
	// return '尚硅谷'
	// 返回的结果不是一个 Promise 类型的对象，返回的结果就是成功Promise 对象
	// return
	//抛出错误，返回的结果是一个失败的Promise
	// throw new Error(' 出错啦!')
	//返回的结果如果是一个Promise 对象
	return new.Promise((resolve,reject)=>{
	// resolve( '成功的数据' )
 	reject("失败的错误" )
	})
}
const result = fn()
//调用then 方法
result.then(value => {
	console.log(value)
}, reason => {
	console.warn(reason)
})

复制代码
```

#### 3.await 表达式

1）await必 须写在async函数中

2）await 右侧的表达式- -般为promise对象

3）await 返回的是promise成功的值

4）await 的promise失败了，就会抛出异常，需要通过try..catch捕获处理

```javascript
//创建 promise 对象
const p = new Promise((resolve,reject)=>{
    //resolve('用户数据')
    reject('出错啦！')
})

// await 要放在 async 函数中
async function main(){
    try{
        let result = await p
        console.log(result)
    }catch(e){
        console.log(e)
    }
}

//调用函数
main()
复制代码
```

#### 4.async 和 await 结合读取文件

```javascript
//1.引入fs模块
const fs = require('fs')

//读取为学
function readWeiXue(){
    return new Promise((resolve,reject)=>{
        fs.readFile("./resources/为学.md",(err,data)=>{
            //如果成功
            if(!err) resolve(data)
            //如果失败
            reject(err)
        })
    })
}

function readChaYangShi(){
    return new Promise((resolve,reject)=>{
        fs.readFile("./resources/插秧诗.md",(err,data)=>{
            //如果成功
            if(!err) resolve(data)
            //如果失败
            reject(err)
        })
    })
}

function readGuanShu(){
    return new Promise((resolve,reject)=>{
        fs.readFile("./resources/观书有感.md",(err,data)=>{
            //如果成功
            if(!err) resolve(data)
            //如果失败
            reject(err)
        })
    })
}

//声明一个 async 函数
async function main(){
    //获取为学内容
    let weixue = await readWeiXue()
    //获取插秧诗内容
    let chayang = await readChaYangShi()
    //获取观书有感内容
    let guanshu = await readGuanShu()
    
    console.log(weixue.toString())
    console.log(chayang.toString())
    console.log(guanshu.toString())
}

main()
复制代码
```

#### 5.async 和 await 结合封装AJAX请求

```javascript
//发送 AJAX 请求,返回的结果是 Promise 对象
function sendAJAX(url){
    return new Promise((resolve,reject)=>{
        //1.创建对象
    	const x = new XMLHttpRequest()
    	//2.初始化
    	x.open('GET', url)
    	//3.发送
    	x.send()
    	//4.事件绑定
    	x.onreadystatechange = function(){
        	if(x.readyState === 4){
            	if(x.status >=200 && x.status <300){
                	//成功
                	resolve(x.response)
            	}else{
                	//失败
                	reject(x.status)
            	}
        	}
    	}
    })
}

//Promise.then 方法测试
sendAJAX('https://api.apiopen.top/getJoke').then(value=>{
    console.log(value)
},reason=>{})

//async 与 await 测试
async function main(){
    let reasult = await sendAJAX('https://api.apiopen.top/getJoke')
    //再次测试
    let tianqi = await sendAJAX('http://www.tianqiapi.com/index/doc?version=day')
    console.log(reasult)
}

main()
复制代码
```

#### 6.ES8对象方法扩展

```javascript
//声明对象
const school = {
    name: '北大',
    cities: ['北京','上海','深圳'],
    xueke: ['前端','Java','大数据','运维']
} 

//获取对象所有的键
console.log(Object.keys(school))
//获取对象所有的值
console.log(Object.values(school))
//entries
console.log(Object.entries(school))
//创建Map
const m = new Map(Object.entries(school))
console.log(m)

//对象属性的描述对象
conosle.log(Object.getOwnPropertyDescriptors(school))

const obj = Object.create(null,{
    name:{
        //设置值
        value: '北大',
        //属性特性
        writable: true,
        configurable: true,
        enumerable: true
    }
})
复制代码
```

# ES9

#### 1.对象的 rest 参数和扩展运算符

Rest参数与spread 扩展运算符在ES6中已经引入，不过ES6 中只针对于数组. 在ES9中为对象提供了像数组样的rest参数和扩展运算符

```javascript
//rest参数
function connect({host, port, ...user}){
    console.log(host)
    console.log(port)
    console.log(user)
}

connect({
    host: '127.0.0.1',
    port: 3306,
    username: 'root',
    password: 'root',
    type: 'master'
})

//扩展运算符 对象合并
const ski1lOne = {
	q: '天音波'
}
const skillTwo = {
	W: '金钟罩'
}
const skillThree = {
	e: '天雷破'
}
const skillFour = {
	r: '猛龙摆尾'
}
const mangseng = {...skillOne, ...skillTwo, ... skillThree, ...skil1Four}
console.log(mangseng)
复制代码
```

#### 2.正则扩展-命名捕获分组

```javascript
//声明一个字符串
let str = '<a href="http://www.baidu.com">百度</a>'
// 提取 url 与标签文本
const reg = /<a = href="(.*)">(.*)<\/a>/
//执行
const result = reg.exec(str)
console.log(result[1])
console.log(result[2])

//命名捕获分组 比数字下标更容易维护
const reg = /<a = href="(?<url>.*)">(?<text>.*)<\/a>/
const result = reg.exec(str)

console.log(result.groups.url)
console.log(result.groups.text)
复制代码
```

#### 3.正则扩展-反向断言

```javascript
//声明字符串
let str = 'JS5211314你知道么555啦啦啦'
//正向断言
// const reg = /\d+(?=啦)/
// const result = reg.exec(str)
//反向断言
const reg = /(?<=么)\d+/
const result = reg.exec(str)
console.log(result)
复制代码
```

#### 4.正则扩展-dotAll模式

```javascript
//dot . 元字符 除换行符以外的任意单个字符
let str =`
<ul>
	<li>
		<a>肖生克的救赎</a>
		<p>上映日期: 1994-09-10</p>
	</li>
	<li>
		<a>阿甘正传</a>
		<p>上映日期: 1994-07-06</p>
	</li>
</ul> `

//声明正则
const reg = /<li>\s+<a>(.*?)<\/a>\s+<p>(.*?)<\/p>/
const reg = /<li>.*?<a>(.*?)<\/a>.*?<p>(.*?)<\/p>/gs
//执行匹配
//const result = reg.exec(str)
let result
let data = []
while(result = reg.exec(str)){
    data.push({title: result[1], time: result[2]})
}
console.log(data)
复制代码
```

# ES10

#### 1.对象扩展方法Object.fromEntries 将二维数组转为对象

```javascript
//二位数组
const result = Object.fromEntries([
    ['name','北大'],
    ['xueke','Java,大数据,前端,云计算']
])

//Map
const m = new Map()
m.set('name','beida')
const result = Object.fromEntries(m)

//Object.entires ES8 将对象转为二维数组
const arr = Object.entires({
    name: '北大'
})
console.log(arr)
复制代码
```

#### 2.字符串扩展方法trimStart 与 trimEnd

```javascript
//trim 清除字符串左右空白
let str = '   iloveyou    '

console.log(str)				
console.log(str.trimStart())	//清除字符串左空白
console.log(str.trimEnd())		//清除字符串右空白
复制代码
```

#### 3.数组扩展方法 flat 和 flatMap

```javascript
//flat 平
//将多维数组转化为低维数组
// const arr = [1,2,3,4,[5,6]]
// const arr = [1,2,3,4,[5,6,[7,8,9]]]
//参数为深度 是一个数字
// console .1og(arr.flat(2))

//flatMap 如果返回值是多维数组,可以对其降维
const arr = [1,2,3,4]
const result = arr.flatMap(item => [item * 10])
console.log(result);
复制代码
```

#### 4.Symbol.prototype.description

```javascript
//创建 Symbol
let s = Symbol('北大')

console.log(s.description) //北大
复制代码
```

# ES11

#### 1.类的私有属性

```javascript
class Person{
    //公有属性
    name;
    //私有属性
    #age;
    #weight;
    //构造方法
    constructor(name, age, weight){
        this.name = name
        this.#age = age
        this.#weight = weight
    }
	//内部访问
	intro(){
        console.log(this.name)
        console.log(this.#age)
        console.log(this.#weight)
    }
}

//实例化
const girl = new Person('小红',18,'45kg')

console.log(girl.name)
console.log(girl.#age)    //无法通过实例化对象进行访问
console.log(girl.#weight)
复制代码
```

#### 2.Promise.allSettled

```javascript
//声明两个promise对象
const p1 = new Promise((resolve, reject)=>{
	setTimeout(()=>{
		resolve( '商品数据- 1');
	}, 1000)
})
const p2 = new Promise((resolve, reject)=>{
	setTimeout(()=>{
		resolve('商品数据- 2');
		// reject('出错啦!');
}, 1000 )
})
//调用allsettled 方法 始终返回成功的promise
// const result = Promise.allSettled([p1, p2]);

// all方法 都成功才返回成功的promise
// const res = Promise.all([p1, p2]);

console.log(res)
复制代码
```

#### 3.String.prototype.matchAll

```javascript
let str =`
<ul>
	<li>
		<a>肖生克的救赎</a>
		<p>上映日期: 1994-09-10</p>
	</li>
	<li>
		<a>阿甘正传</a>
		<p>上映日期: 1994-07-06</p>
	</li>
</ul> `

//声明正则
const reg = /<li>.*?<a>(.*?)<\/a>.*?<p>(.*?)<\/p>/gs

//调用方法
const result = str.matchAll(reg)

const arr = [...result]

console.log(arr)
复制代码
```

#### 4.可选链操作符

```javascript
// ?.
function main(config){
	// const dbHost = config && config.db && config.db.host
const dbHost = config?.db?.host
console.log(dbHost)
}
main({
	db:{
		host: '192.168.1.100',
		username: 'root'
    },
	cache: {
		host: '192.168.1.200',
		username: 'admin'
	}
})
复制代码
```

#### 5.动态import

```javascript
//hello.js
export function hello(){
    alert('Hello')
}
复制代码
//app.js
const btn = document.getElementById('btn')
btn.onclick = function(){
    //调用import函数进行动态导入模块
    import('./hello.js').then(module=>{
        module.hello()
    })
}
复制代码
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>动态import加载</title>
</head>
<body>
    <button id="btn"></button>
    <script src="./js/app.js" type="module"></script>
</body>
</html>
复制代码
```

#### 6.BigInt

```javascript
// 大整形
//let n = 521n
// console.1og(n, typeof(n))
//函数
//let n = 123
// console.log(BigInt(n))
// console.log(BigInt(1.2)) //报错
//大数值运算
let max = Number.MAX_SAFE INTEGER //最大安全整数
console.log(max)
console.1og(max + 1)  
console.1og (max + 2) //运算异常

console.log(BigInt(max))
console.1og(BigInt(max) + BigInt(1))
console.log(BigInt(max) + BigInt(2))
复制代码
```

#### 7.绝对全局对象globalThis

无论在什么环境下，始终指向window