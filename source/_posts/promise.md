---
title: I promise!
date: 2017-10-24
tags:
- Promise
- 1024 程序猿节
---

# 关于 Promise

1024 程序猿节快乐～～

参考文献： [JavaScript Promise 迷你书（中文版）](http://liubin.org/promises-book)

## 什么是 Promise

在 JS 中，我们处理异步方法一般使用回调函数，但是回调函数其实是有点反人类的，因为在函数执行到回调执行之间是有一定的时间间隔的，但是我们固定思维是瀑布流式的，所以相对于异步，同步是更适合我们的方式；同时，回调还可能导致 **回调地狱**。

Promise 是用来解决回调问题的一个机制，在写法上可以让代码可读性更高，写起来看起来都比较舒服。Promise 通过 resolve 解决了回调函数无法给调用函数返回值的问题，同时 Promise 下的几个方法可以解决需要对多个异步请求做处理的问题。

其实 Promise 的底层还是回调，只是做了一层封装，看起来像同步，让回调的使用体验更好一点。

<iframe src='http://caniuse.com/promises/embed/agents=desktop,ios_saf,android' style="width:100%;height: 300px;border:none"></iframe>


## Promise 基本用法

语法如下：

```
var promise = new Promise((resolve, reject) => {
    // 异步处理
    // 处理结束后、调用resolve 或 reject
});
promise.then(onFulfilled, onRejected)
promise.catch(onRejected)
```

下面是一个最简单的例子：

``` javascript
var promise = new Promise((resolve, reject) => {
	if (true) {
		resolve(42);
	} else {
		reject(80);
	}
});

// value 与 error 就是上面的 resolve 和 reject 方法里的参数
promise.then((value) => {
    console.log(value); // 42
}).catch((error) => {
    console.error(error); // 80
});
```

声明一个 Promise 实例，然后在判断条件符合的时候 `resolve` 了，不符合的时候 `reject`, 同时把相应的值作为参数参入；然后通过实例 promise 的 `then` 和 `catch` 方法捕获两种情况，分别做相应处理。

Promise 使用比较都的情况是异步请求，下面以 jQuery 的 ajax 封装一层 Promise get 请求。

``` javascript
  // getAsyncPromise 封装了 promise	
  function getAsyncPromise(url) {
    var p = new Promise((resolve, reject) => {
      $.ajax({
        url: url,
        method: 'get',
      })
      .done((data) => {
        resolve(data)
      })
      .fail((error) => {
        reject(error)
      })
    })
    // 返回一个 promise 实例
    return p
  }
```

``` javascript
var p = getAsyncPromise('http://omhr7p9e5.bkt.clouddn.com/hexo-blog/promise/1.json')
p.then((data)=>{
	return getAsyncPromise('http://omhr7p9e5.bkt.clouddn.com/hexo-blog/promise/2.json')
})
.then((data)=>{
	console.log(data)
})
.catch(()=>{

})
```

通过 getAsyncPromise 请求获得一个 Promise 实例对象，每个 `then` 又返回一个 promise，所以可以通过多个 `then` 来取代多层回调嵌套。功能同回调是一直的，但是看起来直观多了，同时代码也易于维护。其优势显而易见。

## Promise 还能怎么用？

Promise 还有多个静态方法，其中两个静态方法：

- Promise.resolve()
- Promise.reject()

``` javascript
Promise.resolve(10).then((val)=> {
	console.log(val) // 10
})
```

链式写法 `promise chain` ，`then` 方法每次执行完都会返回一个 **新的** Promise 实例对象：

``` javascript
function taskA() {
    console.log("Task A");
}
function taskB() {
    console.log("Task B");
}
function onRejected(error) {
    console.log("Catch Error: A or B", error);
}
function finalTask() {
    console.log("Final Task");
}

var promise = Promise.resolve();
promise
    .then(taskA)
    .then(taskB)
    .catch(onRejected)
    .then(finalTask);
```

promise chain 中的参数传递，在 taskA 执行结束后将参数传给 taskB: 

``` javascript
function doubleUp(value) {
	// value === 2 from increment return
	return value * 2;
}
function increment(value) {
	// value === 1 from Promise.resolve(1)
	return value + 1;
}
function output(value) {
	// value === 4 from doubleUp return 
	console.log(value);// => (1 + 1) * 2
}

var promise = Promise.resolve(1);
promise
    .then(increment)
    .then(doubleUp)
    .then(output)
    .catch(function(error){
        // promise chain中出现异常的时候会被调用
        console.error(error);
    });
```

## Promise 更好用的方法

- Promise.all()
- Promise.race()

### Promise.all()

``` javascripot
var arr = [promiseA ,promiseB, promiseC]
var all = Promise.all(arr)
all.then((results) => {
	// results === [resultA, resultB, resultC]
	console.log(results);
})
```

Promise.all() 接受的参数为一个 `promise` 数组 `arr`，当数组里的所有 `promise` 都 'resolve' 或者 'reject' 了之后，`then` 里的方法才会被触发，参数 `results` 为 `arr` 里各个 promise 所对应的返回值。

还是利用前面的 `getAsyncPromise` 方法：

``` javascript
var promises = []
for (var i=1; i<=3; i++) {
	promises.push(getAsyncPromise('http://omhr7p9e5.bkt.clouddn.com/hexo-blog/promise/'+i+'.json'))
}

Promise.all(promises)
	.then((results) => {
		console.log(results)
	})
	.catch((err)=>{
		console.log(err)
	})
```

结果如下：

![Promise.all()](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/promise/promise-all.png)

### Promise.race()

`Promise.race()` 从字面上来说就是比赛(race)，哪个 `promise` 先到终点了就直接执行 `then` 方法了，`then` 的返回值是最先完成的 `promise` 的返回值，其他用法与 `Promise.all()` 完全一样。

### 利用 Promise.race() 来设置 promise 超时

原理：`Promise.race(arr)` 的参数 `arr` 中可以有多个 `Promise`，设置一个为 `超时 Promise`，则 `超时 Promise` 或者任何其他一个 `Promise` 改变状态，Race is over。

``` javascript
function delayPromise(ms) {
    return new Promise(function (resolve) {
        setTimeout(resolve, ms);
    });
}

function timeoutPromise(promise, ms) {
    var timeout = delayPromise(ms).then(function () {
            throw new Error('Operation timed out after ' + ms + ' ms');
        });
    return Promise.race([promise, timeout]);
}

// 运行示例
var taskPromise = new Promise(function(resolve){
    // 随便一些什么处理
    var delay = Math.random() * 2000;
    setTimeout(function(){
        resolve(delay + "ms");
    }, delay);
});

timeoutPromise(taskPromise, 1000).then(function(value){
    console.log("taskPromise在规定时间内结束 : " + value);
}).catch(function(error){
    console.log("发生超时", error);
});
```

## 弊端

但是，Promise 肯定也是存在问题的，一个是对老旧浏览器的兼容需要用到 polyfill，二是格式必须遵守其规则，当然，不管什么新语言，新方法，新技术都有其必须遵守的规则。