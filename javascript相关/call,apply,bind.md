**`call, apply, bind`**

- apply 、 call 、bind 三者都是用来改变函数的this对象的指向的；

- apply 、 call 、bind 三者第一个参数都是this要指向的对象，也就是想指定的上下文；

- apply 、 call 、bind 三者都可以利用后续参数传参；

- bind是返回对应函数，便于稍后调用；apply、call则是立即调用 。

- call 需要把参数按顺序传递进去，而 apply 则是把参数放在数组里。

- `某个函数的参数是明确知道数量时用 call ; 而不确定的时候用 apply，然后把参数 push进数组传递进去`。当参数数量不确定时，函数内部也可以通过 arguments 这个数组来遍历所有的参数

- `func.call(this, arg1, arg2);`
- `func.apply(this, [arg1, arg2]);`

#### 数组追加

```javascript
var array1 = [12 , "foo" , {name "Joe"} , -2458];
var array2 = ["Doe" , 555 , 100];
Array.prototype.push.apply(array1, array2);
/* array1 值为 [12 , "foo" , {name "Joe"} , -2458 , "Doe" , 555 , 100] */
```

#### 获取数组中的最大值和最小值

```javascript
var numbers = [5, 458 , 120 , -215 ];
var maxInNumbers = Math.max.apply(Math, numbers), //458
maxInNumbers = Math.max.call(Math,5, 458 , 120 , -215); //458
```

#### 代理log

```javascript
function log(){
    console.log.apply(console, arguments);
};
log(1); //1
log(1,2); //1 2
```

### 手写实现

- 不传入第一个参数，那么上下文默认为 `window`
- 改变了 `this` 指向，让新的对象可以执行该函数，并能接受参数

#### call

```javascript
Function.prototype.myCall = function(context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  context = context || window
  context.fn = this
  const args = [...arguments].slice(1)
  const result = context.fn(...args)
  delete context.fn
  return result
}
```

- 首先 `context` 为可选参数，如果不传的话默认上下文为 `window`
- 接下来给 `context` 创建一个 `fn` 属性，并将值设置为需要调用的函数
- 因为 `call` 可以传入多个参数作为调用函数的参数，所以需要将参数剥离出来
- 然后调用函数并将对象上的函数删除

#### apply

```javascript
Function.prototype.myApply = function(context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  context = context || window
  context.fn = this
  let result
  // 处理参数和 call 有区别
  if (arguments[1]) {
    result = context.fn(...arguments[1])
  } else {
    result = context.fn()
  }
  delete context.fn
  return result
}
```

#### bind

```javascript
Function.prototype.myBind = function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  const _this = this
  const args = [...arguments].slice(1)
  // 返回一个函数
  return function F() {
    // 因为返回了一个函数，我们可以 new F()，所以需要判断
    if (this instanceof F) {
      return new _this(...args, ...arguments)
    }
    return _this.apply(context, args.concat(...arguments))
  }
}
```

