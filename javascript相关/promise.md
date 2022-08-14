## Promise 出现的原因
在 Promise 出现以前，我们处理一个异步网络请求，大概是这样：
```js
// 请求 代表 一个异步网络调用。
// 请求结果 代表网络请求的响应。
请求1(function(请求结果1){
    处理请求结果1
})

// 但是，需求变化了，我们需要根据第一个网络请求的结果，再去执行第二个网络请求，代码大概如下：
请求1(function(请求结果1){
    请求2(function(请求结果2){
        处理请求结果2
    })
})
// 但是。。需求是永无止境的，于是乎出现了如下的代码：
请求1(function(请求结果1){
    请求2(function(请求结果2){
        请求3(function(请求结果3){
            请求4(function(请求结果4){
                请求5(function(请求结果5){
                    请求6(function(请求结果3){
                        ...
                    })
                })
            })
        })
    })
})
```
这就是**回调地狱**

更糟糕的是，我们基本上还要对每次请求的结果进行一些处理，代码会更加臃肿，在一个团队中，代码 review 以及后续的维护将会是一个很痛苦的过程。

回调地狱带来的负面作用有以下几点：

- 代码臃肿。
- 可读性差。
- 耦合度过高，可维护性差。
- 代码复用性差。
- 容易滋生 bug。
- 只能在回调里处理异常。

出现了问题，自然就会有人去想办法。这时，就有人思考了，能不能用一种更加友好的代码组织方式，解决异步嵌套的问题。
```js
let 请求结果1 = 请求1();
let 请求结果2 = 请求2(请求结果1);
let 请求结果3 = 请求3(请求结果2);
let 请求结果4 = 请求2(请求结果3);
let 请求结果5 = 请求3(请求结果4);
```
类似上面这种同步的写法。于是 Promise 规范诞生了，

## 什么是 Promise
> Promise 是异步编程的一种解决方案，比传统的异步解决方案【回调函数】和【事件】更合理、更强大。
```js
// demo
new Promise(请求1)
    .then(请求2(请求结果1))
    .then(请求3(请求结果2))
    .then(请求4(请求结果3))
    .then(请求5(请求结果4))
    .catch(处理异常(异常信息))
```
比较一下这种写法和上面的回调式的写法。我们不难发现，Promise 的写法更为直观，并且能够在外层捕获异步函数的异常信息。
## API
> 一个 Promise 对象有三个状态，并且状态一旦改变，便不能再被更改为其他状态。
- pending，异步任务正在进行。
- resolved (也可以叫fulfilled)，异步任务执行成功。
- rejected，异步任务执行失败。
### Promise.resolve(value)
```js
//如果传入的 value 本身就是 Promise 对象，则该对象作为 Promise.resolve 方法的返回值返回。
function fn(resolve){
    setTimeout(function(){
        resolve(123);
    },3000);
}
let p0 = new Promise(fn);
let p1 = Promise.resolve(p0);
// 返回为true，返回的 Promise 即是 入参的 Promise 对象。
console.log(p0 === p1);

// 返回一个状态置为resolved的Promise对象
let p1 = Promise.resolve(123);
console.log(p1)
```
### Promise.reject
> 与 resolve 唯一的不同是，返回的 promise 对象的状态为 rejected。
### Promise.prototype.then
> 实例方法，为 Promise 注册回调函数，函数形式：fn(vlaue){}，value 是上一个任务的返回结果，then 中的函数一定要 return 一个结果或者一个新的 Promise 对象，才可以让之后的then 回调接收。
### Promise.prototype.catch
> 实例方法，捕获异常，函数形式：fn(err){}, err 是 catch 注册 之前的回调抛出的异常信息。
### Promise.race
> 类方法，多个 Promise 任务同时执行，返回最先执行结束的 Promise 任务的结果，不管这个 Promise 结果是成功还是失败
```js
//设置三个任务
const tasks = {
  task1() {
    return new Promise(...); //return 1
  },
  task2() {
    return new Promise(...); // return 2
  },
  task3() {
    return new Promise(...); // return 3
  }
};

//列表中的所有任务会并发执行，只要有一个promise对象出现结果，就会执行then方法
Promise.race([tasks.task1(), tasks.task2(), tasks.task3()]).then(result => console.log(result));
//假设任务1最开始返回结果，则控制台打印结果为`1`
```
### Promise.all
> 类方法，多个 Promise 任务同时执行。
如果全部成功执行，则以数组的方式返回所有 Promise 任务的执行结果。 如果有一个 Promise 任务 rejected，则只返回 rejected 任务的结果。
```js
//设置三个任务
const tasks = {
  task1() {
    return new Promise(...); //return 1
  },
  task2() {
    return new Promise(...); // return 2
  },
  task3() {
    return new Promise(...); // return 3
  }
};

//列表中的所有任务会并发执行，当所有任务执行状态都为fulfilled后，执行then方法
Promise.all([tasks.task1(), tasks.task2(), tasks.task3()]).then(result => console.log(result));
//最终结果为：[1,2,3]
```

## async/await
- async函数返回的是 Promise 对象,Promise可以作为await命令的参数
- await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）
- await必须写在async函数中
- async函数内部return语句返回的值，会成为then方法回调函数的参数。
```js
async function f() {
  return 'hello world';
}

f().then(v => console.log(v)) // "hello world"
```
- 任何一个await语句后面的 Promise 对象变为reject状态，那么整个async函数都会中断执行。
```js
async function f() {
  await Promise.reject('出错了');
  await Promise.resolve('hello world'); // 不会执行
}
```
- async/await使得异步代码看起来像同步代码，再也没有回调函数。
### Async/Await错误处理
> await 命令后面的 Promise 对象，运行结果可能是 rejected，所以最好把 await 命令放在 try...catch 代码块中。try..catch错误处理也比较符合我们平常编写同步代码时候处理的逻辑。
```js
async function myFunction() {
  try {
    await somethingThatReturnsAPromise();
  } catch (err) {
    console.log(err);
  }
}
```
## async的优势
- 代码简洁，我们不需要写.then，不需要写匿名函数处理 Promise 的 resolve 值，也不需要定义多余的 data 变量，还避免了嵌套代码。
- Promise的内部错误使用try catch捕获不到，只能只用then的第二个回调或catch来捕获，而async/await的错误可以用try catch捕获
## promise和async区别
- Promise的内部错误使用try catch捕获不到，只能只用then的第二个回调或catch来捕获，而async/await的错误可以用try catch捕获
- Promise一旦新建就会立即执行，不会阻塞后面的代码，而async函数中await后面是Promise对象会阻塞后面的代码。
- async函数会隐式地返回一个promise，该promise的reosolve值就是函数return的值。
- 使用async函数可以让代码更加简洁，不需要像Promise一样需要调用then方法来获取返回值，不需要写匿名函数处理Promise的resolve值，也不需要定义多余的data变量，还避免了嵌套代码。

```js
console.log('script start')
async function async1() {
    await async2()
    console.log('async1 end')
}
async function async2() {
    console.log('async2 end')
}
async1()

setTimeout(function() {
    console.log('setTimeout')
}, 0)

new Promise(resolve => {
    console.log('Promise')
    resolve()
})
.then(function() {
    console.log('promise1')
})
.then(function() {
    console.log('promise2')
})
console.log('script end')
// 打印顺序是： script start -> async2 end -> Promise -> script end -> async1 end -> promise1 -> promise2 -> setTimeout
```