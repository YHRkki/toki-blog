## undefined
    undefined 的字面意思就是：未定义的值 。这个值的语义是，希望表示一个变量最原始的状态，而非人为操作的结果 。

这种原始状态会在以下 4 种场景中出现

【1】声明了一个变量，但没有赋值
```js
var foo;
console.log(foo); // undefined
```
【2】访问对象上不存在的属性
```js
console.log(Object.foo); // undefined
var arr = [];
console.log(arr[0]); // undefined
```
【3】函数定义了形参，但没有传递实参
```js
//函数定义了形参 a
function fn(a) {
    console.log(a); // undefined
}
fn(); //未传递实参
```
【4】使用 void 对表达式求值
```js
// ECMAScript 明确规定 void 操作符 对任何表达式求值都返回 undefined
void 0 ; // undefined
void false; // undefined
void []; // undefined
void null; // undefined
void function fn(){} ; // undefined
```
## null
    null 的字面意思是：空值  。这个值的语义是，希望表示 一个对象被人为的重置为空对象，而非一个变量最原始的状态 。

    当一个对象被赋值了null 以后，原来的对象在内存中就处于游离状态，GC 会择机回收该对象并释放内存。因此，如果需要释放某个对象，就将变量设置为 null，即表示该对象已经被清空，目前无效状态。

```js
// null 有属于自己的类型 Null，而不属于Object类型，
// typeof 之所以会判定为 Object 类型，是因为JavaScript 数据类型在底层都是以二进制的形式表示的，
// 二进制的前三位为 0 会被 typeof 判断为对象类型，而 null 的二进制位恰好都是 0 ，因此，null 被误判断为 Object 类型。
typeof null == 'object'
```

## 总结
    用一句话总结两者的区别就是：undefined 表示一个变量自然的、最原始的状态值，而 null 则表示一个变量被人为的设置为空对象，而不是原始状态。所以，在实际使用过程中，为了保证变量所代表的语义，不要对一个变量显式的赋值 undefined，当需要释放一个对象时，直接赋值为 null 即可。