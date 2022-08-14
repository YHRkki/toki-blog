## 为什么会出现 `hooks`
> React hooks是react16.8 以后，react新增的钩子API，目的是增加代码的可复用性，逻辑性，弥补无状态组件没有生命周期，没有数据管理状态state的缺陷。

- 让函数组件也能做类组件的事，有自己的状态，可以处理一些副作用，能获取 ref ，也能做数据缓存。
- 解决逻辑复用难的问题。
- 放弃面向对象编程，拥抱函数式编程。

hooks功能分类 | hooks | 具体功能
---|:--:|---
数据驱动更新 | useState | 数据驱动更新，适用于简单数据类型
数据驱动更新 | useReducer | 订阅状态，创建reducer，更新视图，适用于复杂类型(对象或者数组)
执行副作用 | useEffect | 异步状态下，视图更新后，执行副作用
执行副作用 | useLayoutEffect | 同步状态下，视图更新前，执行副作用
状态获取与传递 | useContext | 订阅并获取 react context 上下文，用于组件跨层级获取状态
状态获取与传递 | useRef | 获取组件或元素实例
状态派生与保存 | useMemo | 派生并缓存新的状态，常用于性能优化
状态获取与传递 | useCallback | 缓存状态，常用于缓存提供给子代组件的 callback 回调函数
## useState
> useState 可以使函数组件像类组件一样拥有 state，函数组件通过 useState 可以让组件重新渲染，更新视图。
```js
// state 目的提供给 UI ，作为渲染视图的数据源。
// dispatchAction 改变 state 的函数，可以理解为推动函数组件渲染的渲染函数。
// initData 有两种情况，第一种情况是非函数，将作为 state 初始化的值。 第二种情况是函数，函数的返回值作为 useState 初始化的值。
const [ state , dispatch ] = useState(initData)
```

## useReducer
> useReducer 是 react-hooks 提供的能够在无状态组件中运行的类似redux的功能 api 。
```js
// state 更新之后的 state 值。
// dispatchAction 派发更新的 dispatchAction 函数, 本质上和 useState 的 dispatchAction 是一样的。
// reducer 一个函数 reducer ，我们可以认为它就是一个 redux 中的 reducer , reducer的参数就是常规reducer里面的state和action, 返回改变后的state

import React, { useReducer } from 'react';

const counterReducer = (state, action) => {
  switch (action.type) {
    case 'INCREASE':
      return { ...state, count: state.count + 1 };
    case 'DECREASE':
      return { ...state, count: state.count - 1 };
    default:
      throw new Error();
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });

  const handleIncrease = () => {
    dispatch({ type: 'INCREASE' });
  };

  const handleDecrease = () => {
    dispatch({ type: 'DECREASE' });
  };

  return (
    <div>
      <h1>Counter with useReducer</h1>
      <p>Count: {state.count}</p>

      <div>
        <button type="button" onClick={handleIncrease}>+</button>
        <button type="button" onClick={handleDecrease}>-</button>
      </div>
    </div>
  );
};

export default Counter;
```
> 这里有一个需要注意的点就是：**如果返回的 state 和之前的 state ，内存指向相同，那么组件将不会更新。**

## 什么时候用 useReducer，什么时候用 useState呢
下面的情况可能更适合useReducer

- 不同的属性捆绑在一起，应该在一个状态对象中管理 （比如分页[pageIndex，pageSize，total]，数据请求[error, res, isLoading]）
- 复杂的业务逻辑更适合useReducer

下面的情况可能更适合useState

- 不同的属性不会以任何相关的方式变化，并且可以由多个useState钩子管理
- 当你管理基本类型的时候使用useState
- 当你处理复杂类型(对象或者数组)的时候使用useReducer

[参考文章](https://blog.csdn.net/weixin_43213137/article/details/120745665)

## useEffect
> React hooks也提供了 api ，用于弥补函数组件没有生命周期的缺陷。其本质主要是运用了 hooks 里面的 useEffect ， useLayoutEffect
```js
/* 模拟数据交互 */
function getUserInfo(a){
    return new Promise((resolve)=>{
        setTimeout(()=>{
           resolve({
               name:a,
               age:16,
           })
        },500)
    })
}

const Demo = ({ a }) => {
    const [ userMessage , setUserMessage ] :any= useState({})
    const div= useRef()
    const [number, setNumber] = useState(0)
    /* 模拟事件监听处理函数 */
    const handleResize =()=>{}
    /* useEffect使用 ，这里如果不加限制 ，会是函数重复执行，陷入死循环*/
    useEffect(()=>{
       /* 请求数据 */
       getUserInfo(a).then(res=>{
           setUserMessage(res)
       })
       /* 定时器 延时器等 */
       const timer = setInterval(()=>console.log(666),1000)
       /* 操作dom  */
       console.log(div.current) /* div */
       /* 事件监听等 */
       window.addEventListener('resize', handleResize)
         /* 此函数用于清除副作用, 组件卸载时执行 */
       return function(){
           clearInterval(timer)
           window.removeEventListener('resize', handleResize)
       }
    /* 只有当props->a和state->number改变的时候 ,useEffect副作用函数重新执行 ，如果此时数组为空[]，证明函数只有在初始化的时候执行一次相当于componentDidMount */
    },[ a ,number ])
    return (<div ref={div} >
        <span>{ userMessage.name }</span>
        <span>{ userMessage.age }</span>
        <div onClick={ ()=> setNumber(1) } >{ number }</div>
    </div>)
}
```
> 对于 useEffect 执行， React 处理逻辑是采用异步调用 ，对于每一个 effect 的 callback， React 会向 setTimeout回调函数一样，放入任务队列，等到主线程任务完成，DOM 更新，js 执行完成，视图绘制完毕，才执行。所以 effect 回调函数不会阻塞浏览器绘制视图。

如上在 useEffect 中做的功能如下：

1. 请求数据。
2. 设置定时器,延时器等。
3. 操作 dom , 在 React Native 中可以通过 ref 获取元素位置信息等内容。
4. 注册事件监听器, 事件绑定，在 React Native 中可以注册 NativeEventEmitter 。
5. 还可以清除定时器，延时器，解绑事件监听器等。

## useLayoutEffect

useLayoutEffect 和 useEffect 不同的地方是采用了同步执行，那么和useEffect有什么区别呢？

1. 首先 useLayoutEffect 是在 DOM 更新之后，浏览器绘制之前，这样可以方便修改 DOM，获取 DOM 信息，这样浏览器只会绘制一次，如果修改 DOM 布局放在 useEffect ，那 useEffect 执行是在浏览器绘制视图之后，接下来又改 DOM ，就可能会导致浏览器再次回流和重绘。而且由于两次绘制，视图上可能会造成闪现突兀的效果。
2. useLayoutEffect callback 中代码执行会阻塞浏览器绘制。

## useContext

可以使用 useContext ，来获取父级组件传递过来的 context 值，这个当前值就是最近的父级组件 Provider 设置的 value 值，useContext 参数一般是由 createContext 方式创建的 ,也可以父级上下文 context 传递的 ( 参数为 context )。useContext 可以代替 context.Consumer 来获取 Provider 中保存的 value 值。

```js
/* 用useContext方式 */
const DemoContext = ()=> {
    const value:any = useContext(Context)
    /* my name is alien */
return <div> my name is { value.name }</div>
}

/* 用Context.Consumer 方式 */
const DemoContext1 = ()=>{
    return <Context.Consumer>
         {/*  my name is alien  */}
        { (value)=> <div> my name is { value.name }</div> }
    </Context.Consumer>
}

export default ()=>{
    return <div>
        <Context.Provider value={{ name:'alien' , age:18 }} >
            <DemoContext />
            <DemoContext1 />
        </Context.Provider>
    </div>
}
```

## useRef
获取 DOM 元素
```js
const DemoUseRef = ()=>{
    const dom= useRef(null)
    const handerSubmit = ()=>{
        /*  <div >表单组件</div>  dom 节点 */
        console.log(dom.current)
    }
    return <div>
        {/* ref 标记当前dom节点 */}
        <div ref={dom} >表单组件</div>
        <button onClick={()=>handerSubmit()} >提交</button>
    </div>
}
```
保存状态: 可以利用 useRef 返回的 ref 对象来保存状态，只要当前组件不被销毁，那么状态就会一直存在。
> 需要注意的是 useRef 保存的状态变更不会引起组件重新渲染，可以利用这个特性计算组件渲染次数等功能
```js
const status = useRef(false)
/* 改变状态 */
const handleChangeStatus = () => {
  status.current = true
}
```

## useMemo

useMemo 可以在函数组件 render 上下文中同步执行一个函数逻辑，这个函数的返回值可以作为一个新的状态缓存起来。那么这个 hooks 的作用就显而易见了：

场景一：在一些场景下，需要在函数组件中进行大量的逻辑计算，那么我们不期望每一次函数组件渲染都执行这些复杂的计算逻辑，所以就需要在 useMemo 的回调函数中执行这些逻辑，然后把得到的产物（计算结果）缓存起来就可以了。

场景二：React 在整个更新流程中，diff 起到了决定性的作用，比如 Context 中的 provider 通过 diff value 来判断是否更新

```js
// create：第一个参数为一个函数，函数的返回值作为缓存值，如上 demo 中把 Children 对应的 element 对象，缓存起来。
// deps： 第二个参数为一个数组，存放当前 useMemo 的依赖项，在函数组件下一次执行的时候，会对比 deps 依赖项里面的状态，是否有改变，如果有改变重新执行 create ，得到新的缓存值。
// cacheSomething：返回值，执行 create 的返回值。如果 deps 中有依赖项改变，返回的重新执行 create 产生的值，否则取上一次缓存值。
const cacheSomething = useMemo(create,deps)

// 缓存计算结果
function Scope(){
    const style = useMemo(()=>{
      let computedStyle = {}
      // 经过大量的计算
      return computedStyle
    },[])
    return <div style={style} ></div>
}

// 缓存组件,减少子组件 rerender 次数
function Scope ({ children }){
   const renderChild = useMemo(()=>{ children()  },[ children ])
   return <div>{ renderChild } </div>
}
```

## useCallback
useMemo 和 useCallback 接收的参数都是一样，都是在其依赖项发生变化后才执行，都是返回缓存的值，区别在于 useMemo 返回的是函数运行的结果，useCallback 返回的是函数，这个回调函数是经过处理后的也就是说父组件传递一个函数给子组件的时候，由于是无状态组件每一次都会重新生成新的 props 函数，这样就使得每一次传递给子组件的函数都发生了变化，这样不仅有多余的计算，还触发了子组件的重新渲染。此时我们就可以通过 usecallback 来处理此函数，然后作为 props 传递给子组件。

```js
/* 用react.memo */
const DemoChildren = React.memo((props)=>{
   /* 只有初始化的时候打印了 子组件更新 */
    console.log('子组件更新')
   useEffect(()=>{
       props.getInfo('子组件')
   },[])
   return <div>子组件</div>
})

const DemoUseCallback=({ id })=>{
    const [number, setNumber] = useState(1)
    /* 此时usecallback的第一参数 (sonName)=>{ console.log(sonName) }
     经过处理赋值给 getInfo */
    const getInfo  = useCallback((sonName)=>{
          console.log(sonName)
    },[id])
    return <div>
        {/* 点击按钮触发父组件更新 ，但是子组件没有更新 */}
        <button onClick={ ()=>setNumber(number+1) } >增加</button>
        <DemoChildren getInfo={getInfo} />
    </div>
}
```