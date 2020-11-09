## `构造函数方法`

**`Array.of()`**

```javascript
// 用于将参数依次转化为数组中的一项，然后返回这个新数组，而不管这个参数是数字还是其它。它基本上与Array构造器功能一致，唯一的区别就在单个数字参数的处理上
Array.of(8)  ==>  [8]
Array(8)  ==>  [undefined * 8]
```

**`Array.from()`**

```javascript
// Array.from(arrLike, mapFn, this)
// arrLike 伪数组对象
// mapFn 可选，新数组的每个元素都会执行该回调
// this 可选，执行回调 mapFn 的 this 对象
Array.from({length: 10}, (v, i) => i + 1)  ==>  [ 1, 2, 3, 4, 5, 6, 7, 8, 9， 10]
```

**`Array.isArray()`**

```javascript
// ES6 判断变量是否为数组类型
// ES5 有如下方法判断类型
var a = []
// 1.基于instanceof
a instanceof Array
// 2.基于constructor
a.constructor === Array
// 3.基于Object.prototype.isPrototypeOf
Array.prototype.isPrototypeOf(a)
// 4.基于getPrototypeOf
Object.getPrototypeOf(a) === Array.prototype
// 5.基于Object.prototype.toString
Object.prototype.toString.apply(a) === '[object Array]'
// TODO: 以上，除了Object.prototype.toString外，其它方法都不能正确判断变量的类型。
```



****



## `改变自身值的 API`

```javascript
let a = [1, 2, 3]
let b
```

**`Array.pop()`**

```javascript
// pop() 方法删除一个数组中的最后的一个元素，并且返回这个元素。
b = a.pop()  ==>  3
a  ==>  [1, 2]
```

**`Array.push()`**

```javascript
// push()方法添加一个或者多个元素到数组末尾，并且返回数组新的长度。
b = a.push(1999)  ==>  4
a  ==>  [1, 2, 3, 1999]
```

**`Array.reverse()`**

```javascript
// reverse() 方法颠倒数组中元素的位置，第一个会成为最后一个，最后一个会成为第一个，该方法返回对数组的引用。
b = a.reverse()  ==>  [3, 2, 1]
a  ==>  [3, 2, 1]
```

**`Array.shift()`**

```javascript
// shift() 方法删除数组的第一个元素，并返回这个元素
b = a.shift()  ==>  1
a  ==>  [2, 3]
```

**`Array.sort()`**

```javascript
// arr.sort([compareFunction])
// sort() 方法对数组元素进行排序，并返回这个数组
// comparefunction是可选的，如果省略，数组元素将按照各自转换为字符串的 Unicode(万国码) 位点顺序排序，例如 "Boy" 将排到 "apple" 之前。
b = a.sort((a, b) => a - b)  ==>  [1, 2, 3] 从小到大
b = a.sort((a, b) => b - a)  ==>  [3, 2, 1] 从大到小
```

**`Array.splice()`**

```javascript
// arr.splice(start, deleteCount)
// splice()方法用新元素替换旧元素的方式来修改数组
// start 指定从索引值处开始修改内容
// deleteCount 指定要删除元素的个数
b = a.splice(0, 2)  ==>  [1, 2]
a  ==>  [3]
```

**`Array.unshift()`**

```javascript
// 从数组头部插入元素，并返回新数组的长度
b = a.unshift(11, 22)  ==>  5
a  ==>  [11, 22, 1, 2, 3]
```



****



## `不会改变数组自身的方法`

**`Array.concat()`**

```javascript
// arr.concat(val1, val2, ...valn)
// concat() 方法将传入的数组或者元素与原数组合并，组成一个新的数组并返回。
// 用于连接两个或多个数组。该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本。
b = a.concat(4, [5, 6])  ==>  [1, 2, 3, 4, 5, 6]
a  ==>  [1, 2, 3]
```

**`Array.join()`**

```javascript
// arr.join([separator = ','])
// separator 可选，默认值为逗号，将数组拼接成一个字符串
b = a.join('-')  ==>  '1-2-3'
a  ==>  [1, 2, 3]
```

**`Array.slice()`**

```javascript
// arr.slice(start, end)
// slice() 方法将数组中一部分元素浅拷贝存入新的数组对象，并且返回这个数组对象。
// 参数 start 指定复制开始位置的索引，默认为 0，end 如果有值则表示复制结束位置的索引
b = a.slice()  ==>  [1, 2, 3]
let c = a.slice(1, 2)  ==>  [2]
```

**`Array.toString()`**

```javascript
// toString() 方法返回数组的字符串形式
// 该字符串由数组中的每个元素的 toString() 返回值经调用 join() 方法连接（由逗号隔开）组成。
b = a.toString()  ==>  '1,2,3'
```

**`Array.indexof()`**

```javascript
// arr.indexOf(ele, fromIndex = 0)
// indexOf() 方法用于查找元素在数组中第一次出现时的索引，如果没有，则返回 -1
// ele 为要查找的元素， fromIndex 为开始查找的位置，默认值为 0
b = a.indexof(1)  ==>  0
b = a.indexof('1')  ==>  -1 严格匹配，因此不会匹配到字符串 1
```

**`Array.lastIndexof()`**

```javascript
// arr.lastIndexof(ele, fromIndex = length - 1)
// arr.indexof() 的逆向查找
```

**`Array.includes()`**

```javascript
// arr.includes(ele, fromIndex = 0)
// 用来判断当前数组是否包含某个指定的值，如果是，则返回 true，否则返回 false。
console.log(a.incloudes(1))  ==>  true
console.log(a.incloudes(4))  ==>  false
```



****



## `遍历方法`

**`Array.forEach()`**

```javascript
// arr.forEach(fn(v, i, arr), thisArg)
// 返回值为 undefined 
// fn 接收3个参数 v 当前值 i 当前索引 arr 数组本身
// thisArg 可选，用来当做fn函数内的 this 对象。
```

**`Array.every()`**

```javascript
// arr.every(fn)
// 使用传入的函数测试所有元素，只要其中有一个函数返回值为 false，那么该方法的结果为 false；否则为 true
b = a.every(v => v > 0)  ==>  true
b = a.every(v => v > 1)  ==> false
```

**`Array.some()`**

```javascript
// arr.some(fn)
// 与 every 相反
b = a.some(v => v < 0)  ==>  false
b = a.some(v => v > 2)  ==>  truea
```

**`Array.filter()`**

```javascript
// arr.filter(fn, this)
// filter() 方法使用传入的函数测试所有元素，并返回所有通过测试的元素组成的新数组。它就好比一个过滤器，筛掉不符合条件的元素。
b = a.filter(v => v > 1)  ==>  [2, 3]
```

**`Array.map()`**

```javascript
// arr.map(fn, this)
// 遍历数组，返回新数组
b = a.map(v => v + 1)  ==>  [2, 3, 4]
```

**`Array.reduce()`**

```javascript
// arr.reduce(fn(prevVal, val, index, array), initialValue)
// reduce() 方法接收一个方法作为累加器，数组中的每个值(从左至右) 开始合并，最终为一个值。
// 当 fn 第一次执行的时候，若 initialValue 有值，则 prevVal = initialValue， val 为数组的第一个值
// 若 initialValue 没有值，prevVal 为数组的第一个值，val 为第二个值
b = a.reduce((pv, v) => pv + v)  ==>  6
b = a.reduce((pv, v) => return pv + v, 4)  ==>  10


// 计算数组中每个元素出现的次数
let names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

let nameNum = names.reduce((pre,cur)=>{
  if(cur in pre){
    pre[cur]++
  }else{
    pre[cur] = 1 
  }
  return pre
},{})
console.log(nameNum); //{Alice: 2, Bob: 1, Tiff: 1, Bruce: 1}


// 数组去重
let arr = [1,2,3,4,4,1]
let newArr = arr.reduce((pre,cur)=>{
    if(!pre.includes(cur)){
      return pre.concat(cur)
    }else{
      return pre
    }
},[])
console.log(newArr);// [1, 2, 3, 4]


// 将二维数组转化为一维
let arr = [[0, 1], [2, 3], [4, 5]]
let newArr = arr.reduce((pre,cur)=>{
    return pre.concat(cur)
},[])
console.log(newArr); // [0, 1, 2, 3, 4, 5]
```

**`Array.find()&findIndex()`**

```javascript
// find() 方法基于 ECMAScript 2015（ES6）规范，返回数组中第一个满足条件的元素（如果有的话）， 如果没有，则返回 undefined。
// findIndex() 方法则返回数组中第一个满足条件的元素的索引（如果有的话）否则返回-1。
a.find(v => v > 0)  ==>  1
a.find(v => v > 20)  ==>  undefined
a.findIndex(v => v % 2 == 0)  ==>  1
a.findIndex(v => v > 20)  ==>  -1
```

