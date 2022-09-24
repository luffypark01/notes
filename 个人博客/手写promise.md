# 手写Promise

promise是js进行异步编程的一种新的解决方案，它本质是一个构造函数，通过生成promise对象来进行相关的异步操作，promise在js中具有重要的地位，通过自己动手实现promise能够使我们更加清晰的认识和理解promise。

## 1.搭出基本框架

首先我们来看看promise对象的基本机构

```html
<script src="./02.js"></script>
<script>
    let p = new Promise((resolve, reject) => {
        resolve('OK')
    });
    console.log(p)
    p.then(value => {
        console.log(value)
    },reason => {
        console.warn(reason)
    })
</script>
```

这里可以看到Promise对象里需要传出一个函数,因此我们给Promise函数先定义一个形参，然后它的then方法也是需要传入两个函数，因此then方法也要设置对应的两个形参，代码如下

```javascript
function Promise(executor){

}

Promise.prototype.then = function (onResolved,onRejected){
    
}
```

ok,那么这样基本框架就搭好了

## 2.resolve和reject结构搭建

当我们给Promise对象传入函数时,传入的函数会同步调用，而且传入函数的参数中的resolve和reject本身也是函数，因此需要在内部先定义这两个函数，基本框架如下

```javascript
function Promise(executor){
    //resolve函数
    function resolve(data){
        //data是要传入的参数
    }

    //reject函数
    function reject(data){

    }
    //同步调用执行器函数
    executor(resolve,reject);
}
```

在Promise对象中有两个重要属性promiseState和promiseResult分别表示对象状态(初始值为'pending')和对象结果值，当我们调用resolve函数或reject函数时会改变这两个属性，我们需要先在Promise对象中设置这两个值，再在resolve和reject函数中修改这两个值

```javascript
function Promise(executor){
    this.PromiseState = 'pending';
    this.PromiseResult = null  //设置初始值
    const self = this //保存实例对象的this的值
    //resolve函数
    function resolve(data){   //data是要传入的参数
        //修改对象状态
        self.PromiseState = 'fullfilled' //将状态设置为成功.fullfilled和resolved是一个意思
        //2.设置对象结果值
        self.PromiseResult = data;
    }

    //reject函数
    function reject(data){

        self.PromiseState = 'rejected' //将状态设置为成功.fullfilled和resolved是一个意思

        self.PromiseResult = data;
    }
    //同步调用执行器函数
    executor(resolve,reject);
}
```

还需要注意一点的是Promise对象的状态只能够改变一次，l例如当我们调用resolve函数改变Promise对象的状态，之后再调用reject函数也不会改变Promise的状态和结果值，因此在这个函数时我们需要做一个判断

```javascript
function resolve(data){   //data是要传入的参数
        if(self.PromiseState !== 'pending') return; //当状态已经发生改变时就不往下执行
        //修改对象状态
        self.PromiseState = 'fullfilled' //将状态设置为成功.fullfilled和resolved是一个意思
        //2.设置对象结果值
        self.PromiseResult = data;
    }

    //reject函数
    function reject(data){
        if(self.PromiseState !== 'pending') return;
        self.PromiseState = 'reject' //将状态设置为成功.fullfilled和resolved是一个意思

        self.PromiseResult = data;
    }
```



## 3.实现抛出异常改变promise的状态

```html
<script src="./02.js"></script>
<script>
    let p = new Promise((resolve, reject) => {
        // resolve('OK')
        throw 'error'
    });
    console.log(p)
    p.then(value => {
        console.log(value)
    },reason => {
        console.warn(reason)
    })
</script>
```

改变promise对象状态有三种方式，分别是调用resolve函数、调用reject函数和抛出异常。前两个我们已经实现，现在来实现通过抛出错误来改变Promise对象状态。要进行异常处理我们就要使用try...catch语句，抛出异常是在我们出入的函数里面执行的，所以要对执行器函数进行异常捕获，将捕获的异常交给reject函数来处理即可

```javascript
function Promise(executor){
    this.PromiseState = 'pending';
    this.PromiseResult = null  //设置初始值
    const self = this //保存实例对象的this的值
    //resolve函数
    function resolve(data){   //data是要传入的参数
        if(self.PromiseState !== 'pending') return;
        //修改对象状态
        self.PromiseState = 'fullfilled' //将状态设置为成功.fullfilled和resolved是一个意思
        //2.设置对象结果值
        self.PromiseResult = data;
    }

    //reject函数
    function reject(data){
		if(self.PromiseState !== 'pending') return;
        self.PromiseState = 'rejected' //将状态设置为成功.fullfilled和resolved是一个意思

        self.PromiseResult = data;
    }
    //同步调用执行器函数
    try{
        executor(resolve,reject);
    }catch (e){
        reject(e);
    }

}
```

## 4.实现then方法执行回调

```javascript
p.then(value => {
    console.log(value)
},reason => {
    console.warn(reason)
})
```

调用then方法并向里面传入两个函数，根据promise对象的状态来决定执行哪一个函数，因此在then方法内部需要做一个判断，因为是p对象即Promise对象调用的then方法，因此在then方法内部也可以通过this.PromiseState和this.PromiseResult来取得Promise对象都状态和结果值，this.PromiseResult作为value或reason传入

```javascript
Promise.prototype.then = function (onResolved,onRejected){
    if(this.PromiseState === 'fullfilled'){
        onResolved(this.PromiseResult);
    }
    if(this.PromiseState == 'rejected'){
        onRejected(this.PromiseResult)
    }
}
```

## 5.实现异步任务的回调执行

上面我们的代码都是只能同步执行的，而Promise是用来处理异步问题，因此我们接下来要实现异步任务的then方法回调

```
<script src="./02.js"></script>
<script>
    let p = new Promise((resolve, reject) => {
        setTimeout(()=>{
            resolve('OK')
        },1000)
    });
    console.log(p)
    p.then(value => {
        console.log(value)
    },reason => {
        console.warn(reason)
    })
</script>
```

用setTimeout来模拟异步代码，代码从上到下执行，由于then方法里面我们只值判了fullfilled和rejected，所以不修改代码的话，下面的then方法什么都不会做，因此要在then方法里面判断Promise对象状态为pending的情况，当状态为penging时，保存这两个回调函数，等异步代码执行完成确定状态后再执行回调函数

先在Promise对象里面添加callback属性来存回调函数

```javascript
this.PromiseState = 'pending';
this.PromiseResult = null  //设置初始值
this.callback = {} //保存回调函数
```

再在then方法里面保存回调函数

```javascript
Promise.prototype.then = function (onResolved,onRejected){
    if(this.PromiseState === 'fullfilled'){
        onResolved(this.PromiseResult);
    }
    if(this.PromiseState == 'rejected'){
        onRejected(this.PromiseResult)
    }
    if(this.PromiseState === 'pending'){
        this.callback = {
            onResolved, //这里是简写
            onRejected
        }
    }
}
```

然后在resolve和reject函数里面添加回调函数

```javascript
//resolve函数
function resolve(data){   //data是要传入的参数
    if(self.PromiseState !== 'pending') return;
    //修改对象状态
    self.PromiseState = 'fullfilled' //将状态设置为成功.fullfilled和resolved是一个意思
    //2.设置对象结果值
    self.PromiseResult = data;
    if(self.callback.onResolved){
        self.callback.onResolved(data);
    }
}

//reject函数
function reject(data){
    if(self.PromiseState !== 'pending') return;
    self.PromiseState = 'rejected' //将状态设置为成功.fullfilled和resolved是一个意思

    self.PromiseResult = data;
    if(self.callback.onRejected){
        self.callback.onRejected(data);
    }
}
```

## 6.多个回调执行的实现

```
<script src="./02.js"></script>
<script>
    let p = new Promise((resolve, reject) => {
        setTimeout(()=>{
            resolve('OK')
        },1000)
    });
    p.then(value => {
        console.log(value)
    },reason => {
        console.warn(reason)
    })
    p.then(value => {
        alert(value)
    },reason => {
        alert(reason)
    })
</script>
```

给真正的Promise对象绑定多个回调函数能够依次执行绑定的回调函数，要实现这一点，我们上面的代码肯定是不行的，因为后面的then方法会覆盖之前的对调函数，因此需要将this.callback = {}改为this.callbacks = [],用一个列表来存储每一组绑定的回调函数，通过遍历来依次执行。

在then方法里面，给callbacks里面添加每组回调函数

```javascript
if(this.PromiseState === 'pending'){
    this.callbacks.push({
        onResolved, //这里是简写
        onRejected
    })
}
```

然后再在resolve函数和reject函数里面遍历callbacks依次执行回调函数

```javascript
//resolve函数
function resolve(data){   //data是要传入的参数
    if(self.PromiseState !== 'pending') return;
    //修改对象状态
    self.PromiseState = 'fullfilled' //将状态设置为成功.fullfilled和resolved是一个意思
    //2.设置对象结果值
    self.PromiseResult = data;
    self.callbacks.forEach(item=>{
        item.onResolved(data);
    });
}

//reject函数
function reject(data){
    if(self.PromiseState !== 'pending') return;
    self.PromiseState = 'rejected' //将状态设置为成功.fullfilled和resolved是一个意思

    self.PromiseResult = data;
    self.callbacks.forEach(item=>{
        item.onRejected(data);
    });
}
```

## 7.同步修改状态then方法结果返回

Promise的then方法本身返回的也是一个Promise对象，如果回调函数返回的是一个非Promise对象，那么then方法返回的Promise对象的状态为成功，而且值为回调函数返回的结果值(如果回调函数什么都不返回则结果值为undefined)。

如果回调函数返回的是一个Promise对象，则then返回的对象的状态和结果值与回调函数中返回的Promise对象的状态和结果值一样。

因此我们需要在then函数里面修改代码，将一个Promise对象作为返回值

```javascript
Promise.prototype.then = function (onResolved,onRejected){
    return new Promise((resolve,reject)=>{
        if(this.PromiseState === 'fullfilled'){
            let result = onResolved(this.PromiseResult); //获取回调函数的返回值
            if(result instanceof Promise){  //判断是否为Promise对象
                result.then(value => {
                    resolve(value);
                },reason => {
                    reject(reason);
                })
            }else {
                //结果状态为成功
                resolve(result)
            }
        }
        if(this.PromiseState == 'rejected'){
            let result = onRejected(this.PromiseResult); //获取回调函数的返回值
            if(result instanceof Promise){
                result.then(value => {
                    resolve(value);
                },reason => {
                    reject(reason);
                })
            }else {
                //结果状态为成功
                resolve(result)
            }
        }
        if(this.PromiseState === 'pending'){
            this.callbacks.push({
                onResolved, //这里是简写
                onRejected
            })
        }
    })
}
```

## 8.异步修改状态then方法结果返回

要实现异步修改then的结果返回就必须要对this.PromiseState === 'pending'这种情况进行处理，我们需要修改给callbacks里面添加的回调函数，与上面的步骤差不多，首先要得到回调函数的返回值，再根据这个返回值的类型进行判断(过程同上)，要注意的是这里我们要用一个变量self来记录this指向,而且还要使用try...catch语句来处理抛出异常的情况

```javascript
self = this
if(this.PromiseState === 'pending'){
    this.callbacks.push({
        onResolved:function(){
            try{
                let result = onResolved(self.PromiseResult);
                if(result instanceof Promise){
                    result.then(value => {
                        resolve(value)
                    },reason => {
                        reject(reason)
                    })
                }else {
                    resolve(result)
                }
            }catch (e) {
                reject(e);
            }
        },
        onRejected:function() {
            try {
                let result = onRejected(self.PromiseResult);
                if(result instanceof Promise){
                    result.then(value => {
                        resolve(value)
                    },reason => {
                        reject(reason)
                    })
                }else {
                    resolve(result)
                }
            }catch (e) {
                reject(e);
            }
        }
    })
}
```

## 9.then方法的完善与优化

上面的代码有大量的重复部分，为了使代码更加简洁，我们需要对相同的部分实现代码的封装

```javascript
Promise.prototype.then = function (onResolved,onRejected){
    const self = this // 记录this的值
    return new Promise((resolve,reject)=>{
        function callback(type) {
            try{
                let result = type(self.PromiseResult);
                if(result instanceof Promise){
                    result.then(value => {
                        resolve(value)
                    },reason => {
                        reject(reason)
                    })
                }else {
                    resolve(result)
                }
            }catch (e) {
                reject(e);
            }
        }
        if(this.PromiseState === 'fullfilled'){
            callback(onResolved);
        }
        if(this.PromiseState == 'rejected'){
            callback(onRejected);
        }
        if(this.PromiseState === 'pending'){
            this.callbacks.push({
                onResolved:function(){
                    callback(onResolved);
                },
                onRejected:function() {
                    callback(onRejected);
                }
            })
        }
    })
}
```

这样，我们的代码就简洁明了了。

## 10.实现catch方法-异常穿透与值传递

Promise对象的catch方法是用来指定状态失败时的回调函数，它具有异常穿透的特性

因为then方法的大部分功能都已实现，所以我们只需要在catch方法里面调用then方法就行

```javascript
Promise.prototype.catch = function (onRejected){
    return this.then(undefined,onRejected);
}
```

这样catch方法返回的依然是一个Promise对象

真正的Promise，then可以只传一个函数(状态成功时的回调函数)，而且就算前面多次调用then最后也可以用catch来对异常或失败状态的处理即异常穿透。

要做到允许在then方法中不传第二个回调函数，就必须在then方法中给第二个回调函数设置一个默认值

```javascript
Promise.prototype.then = function (onResolved,onRejected){
    const self = this // 记录this的值
    if(typeof onRejected !== 'function'){
        onRejected = reason =>{
            throw reason
        }
    }
    return new Promise((resolve,reject)=>{...})
}    
```

这样就可以让异常不断往后抛，最终交给catch来处理

Promise除了具有异常穿透的特性之外还具有值传递的特性，即如果前面的then方法里面什么都不传，结果和状态依然可以向后传递

```html
<script src="./02.js"></script>
<script>
    let p = new Promise((resolve, reject) => {
        setTimeout(()=>{
            // resolve('OKKK');
            reject('error')
        },1000)
    });
    p.then()
    .then(value => {
        console.log("ssss")
    })
    .then(value => {
        console.log("xxxx")
    }).catch(reason => {
        console.warn(reason)
    })
</script>
```

要实现这一点我们需要给第一个回调函数设置一个默认值

```javascript
Promise.prototype.then = function (onResolved,onRejected){
    const self = this // 记录this的值
    if(typeof onRejected !== 'function'){
        onRejected = reason =>{
            throw reason
        }
    }
    if(typeof onResolved !== 'function'){
        onResolved = value=>value;
    }
    return new Promise((resolve,reject)=>{...})
}    
```

这样我们就实现了值传递

## 11.实现resolve和reject API

Promise.resolve用于返回一个Promise，要注意这个resolve方法是属性Promise函数对象的，而不是属于实例对象的

与上面部分差不多，直接上代码：

```javascript
Promise.resolve = function (value){
    return new Promise((resolve,reject)=>{
        if(value instanceof Promise){
            value.then(v=>{
                resolve(v)
            },r=>{
                reject(r);
            })
        }else {
            resolve(value)
        }
    })
}
```

而Promise.reject返回的是一个失败对象的Promise,无论传什么返回的都是一个失败状态的Promise,因此更为简单

```javascript
Promise.reject = function (reason){
    return new Promise((resolve,reject)=>{
        reject(reason);
    })
}
```

## 12.实现all API

Promise的all方法，作用是传入一个含有多个Promise对象的数组，如果数组中的所有对象状态为成功则返回一个成功的Promise对象（结果值为包含所有传入对象结果值的数组），如果在这一组传入的Promise对象中有状态为失败的对象则返回一个失败的Promise对象

要实现all方法我们需要一个计数器，来判断所有的状态都已成功的情况

```javascript
Promise.all = function (Promises){
    let count = 0
    let results = [] //存放结果值
    return new Promise((resolve,reject)=>{
        for(let i=0;i<Promises.length;i++){
            Promises[i].then(v=>{
                results[i] = v;
                count++;
                if(count == Promises.length){
                    resolve(results);
                }
            },r=>{
                reject(r);
            })
        }
    })
}
```

## 13.实现race API

race方法与all类似，也是接收一组Promise对象，最后也是返回一个Promise对象，其返回对象的结果值和状态由传入的对象中第一个改变状态的Promise对象决定。实现起来就更简单了，如下：

```javascript
Promise.race = function(promises){
    return new Promise((resolve,reject)=>{
        for(let i=0;i<promises.length;i++){
            promises[i].then(v=>{
                resolve(v);
            },r=>{
                reject(r);
            })
        }
    })
}
```

## 14.实现then方法回调的异步执行

在真正的Promise中，then方法是异步执行的，也就是说then方法里面的回调函数必须等同步代码都执行完毕后再执行

举个例子：

```html
<script src="./02.js"></script>
<script>
    let p = new Promise((resolve, reject) => {
    	resolve('OKKK');
    	console.log(111)
    });
    p.then(v=>{
    	console.log(222)
    })
    console.log(333)
</script>
```

上面代码的打印结果为111、333、222，回调函数最后执行因为它是异步执行的

要实现异步执行也很简单，我们需要在then方法中的同步代码放到定时器里

```javascript
if(this.PromiseState === 'fullfilled'){
	setTimeout(()=>{
		callback(onResolved);
	})
}
if(this.PromiseState == 'rejected'){
	setTimeout(()=>{
		callback(onRejected);
	})
}
```

除了这里还要在Promise内部定义的resolve函数和reject函数中将相关的代码放在定时器里

```javascript
function resolve(data){   //data是要传入的参数
    if(self.PromiseState !== 'pending') return;
    //修改对象状态
    self.PromiseState = 'fullfilled' //将状态设置为成功.fullfilled和resolved是一个意思
    //2.设置对象结果值
    self.PromiseResult = data;
    setTimeout(()=>{
        self.callbacks.forEach(item=>{
            item.onResolved(data);
        });
    })
}

//reject函数
function reject(data){
    if(self.PromiseState !== 'pending') return;
    self.PromiseState = 'rejected' //将状态设置为成功.fullfilled和resolved是一个意思

    self.PromiseResult = data;
    setTimeout(()=>{
        self.callbacks.forEach(item=>{
            item.onRejected(data);
        });
    })
}
```

## 15.将Promise函数封装成一个类

到了这一步，我们已经实现大部分Promise的功能，接下来就是要将Promise函数封装成一个对象，下面是完整代码

```javascript
class Promise{
    //构造方法
    constructor(executor) {
        this.PromiseState = 'pending';
        this.PromiseResult = null  //设置初始值
        this.callbacks = [] //保存回调函数
        const self = this //保存实例对象的this的值
        //resolve函数
        function resolve(data){   //data是要传入的参数
            if(self.PromiseState !== 'pending') return;
            //修改对象状态
            self.PromiseState = 'fullfilled' //将状态设置为成功.fullfilled和resolved是一个意思
            //2.设置对象结果值
            self.PromiseResult = data;
            setTimeout(()=>{
                self.callbacks.forEach(item=>{
                    item.onResolved(data);
                });
            })
        }

        //reject函数
        function reject(data){
            if(self.PromiseState !== 'pending') return;
            self.PromiseState = 'rejected' //将状态设置为成功.fullfilled和resolved是一个意思

            self.PromiseResult = data;
            setTimeout(()=>{
                self.callbacks.forEach(item=>{
                    item.onRejected(data);
                });
            })
        }
        //同步调用执行器函数
        try{
            executor(resolve,reject);
        }catch (e){
            reject(e);
        }
    }
    then(onResolved,onRejected){
        const self = this // 记录this的值
        if(typeof onRejected !== 'function'){
            onRejected = reason =>{
                throw reason
            }
        }
        if(typeof onResolved !== 'function'){
            onResolved = value=>value;
        }
        return new Promise((resolve,reject)=>{
            function callback(type) {
                try{
                    let result = type(self.PromiseResult);
                    if(result instanceof Promise){
                        result.then(value => {
                            resolve(value)
                        },reason => {
                            reject(reason)
                        })
                    }else {
                        resolve(result)
                    }
                }catch (e) {
                    reject(e);
                }
            }
            if(this.PromiseState === 'fullfilled'){
                setTimeout(()=>{
                    callback(onResolved);
                })
            }
            if(this.PromiseState == 'rejected'){
                setTimeout(()=>{
                    callback(onRejected);
                })
            }
            if(this.PromiseState === 'pending'){
                this.callbacks.push({
                    onResolved:function(){
                        callback(onResolved);
                    },
                    onRejected:function() {
                        callback(onRejected);
                    }
                })
            }
        })
    }
    catch(onRejected){
        return this.then(undefined,onRejected);
    }
    static resolve(value){
        return new Promise((resolve,reject)=>{
            if(value instanceof Promise){
                value.then(v=>{
                    resolve(v)
                },r=>{
                    reject(r);
                })
            }else {
                resolve(value)
            }
        })
    }
    static reject(reason){
        return new Promise((resolve,reject)=>{
            reject(reason);
        })
    }
    static all(Promises){
        let count = 0
        let results = [] //存放结果值
        return new Promise((resolve,reject)=>{
            for(let i=0;i<Promises.length;i++){
                Promises[i].then(v=>{
                    results[i] = v;
                    count++;
                    if(count == Promises.length){
                        resolve(results);
                    }
                },r=>{
                    reject(r);
                })
            }
        })
    }
    static race(promises){
        return new Promise((resolve,reject)=>{
            for(let i=0;i<promises.length;i++){
                promises[i].then(v=>{
                    resolve(v);
                },r=>{
                    reject(r);
                })
            }
        })
    }
}
```

好了，到此为止，我们的手写Promise就结束了。
