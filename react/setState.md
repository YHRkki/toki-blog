
## 对React中的setState的理解？
- setState这个方法在调用的时候是同步的，但是引起React的状态更新是异步的 【React状态更新是异步的】
- setState第一个参数可以是一个对象，或者是一个函数，而第二个参数是一个回调函数
## setState第二个参数的作用？
    因为setState是一个异步的过程，所以说执行完setState之后不能立刻更改state里面的值。如果需要对state数据更改监听，setState提供第二个参数，就是用来监听state里面数据的更改，当数据更改完成，调用回调函数，用于可以实时的获取到更新之后的数据

## 为什么setState设计为异步的？
    setState设计为异步，可以显著的提升性能：如果每次调用setState都进行一次更新，那么意味着render函数会被频繁调用，界面重新渲染，这样效率是很低的；最好的办法应该是获取到多个更新，之后进行批量更新；

    如果同步更新了state，但是还没有执行render函数，而且props依赖于state中的数据，那么state和props不能保持同步；state和props不能保持一致性，会在开发中产生很多的问题；

## 当你调用setState的时候，发生了什么事？
1. 将传递给 setState 的对象合并到组件的当前状态，触发所谓的调和过程（Reconciliation）
2. 然后生成新的DOM树并和旧的DOM树使用Diff算法对比
3. 根据对比差异对界面进行最小化重渲染

## 批量更新
```js
import React, { Component } from 'react'

class App extends Component {
  state = { count: 1 }

  handleClick = () => {
    this.setState({ count: this.state.count + 1 })
    console.log(this.state.count) // 1

    this.setState({ count: this.state.count + 1 })
    console.log(this.state.count) // 1

    this.setState({ count: this.state.count + 1 })
    console.log(this.state.count) // 1
  }
  render() {
    return (
      <>
        <button onClick={this.handleClick}>加1</button>
        <div>{this.state.count}</div>
      </>
    )
  }
}

export default App
```
点击按钮触发事件，打印的都是 1，页面显示 count 的值为 2

在异步中：

- 对同一个值进行多次 setState， setState 的批量更新策略会对其进行覆盖，取最后一次的执行结果
- 如果是同时 setState 多个不同的值，在更新时会对其进行合并批量更新。

上述的例子，实际等价于如下：

`Object.assign( previousState, {index: state.count+ 1}, {index: state.count+ 1}, ... )`

由于后面的数据会覆盖前面的更改，所以最终只加了一次

如果是下一个state依赖前一个state的话，推荐给setState一个参数传入一个function：
```js
onClick = () => {
  this.setState((prevState, props) => {
    return {count: prevState.count + 1};
  });
  this.setState((prevState, props) => {
    return {count: prevState.count + 1};
  });
}
```

## setTimeout
```js
import React, { Component } from 'react'

class App extends Component {
  state = { count: 1 }

  handleClick = () => {
    this.setState({ count: this.state.count + 1 })
    console.log(this.state.count) // 1

    this.setState({ count: this.state.count + 1 })
    console.log(this.state.count) // 1

    setTimeout(() => {
      this.setState({ count: this.state.count + 1 })
      console.log(this.state.count) // 3
    })
  }
  render() {
    return (
      <>
        <button onClick={this.handleClick}>加1</button>
        <div>{this.state.count}</div>
      </>
    )
  }
}

export default App
```
点击按钮触发事件，发现 setTimeout 里面的 count 值打印值为 3，页面显示 count 的值为 3。setTimeout 里面 setState 之后能马上能到最新值。

在 setTimeout 里面，setState 是同步的；经过前面两次的 setState 批量更新，count 值已经更新为 2。在 setTimeout 里面的首先拿到新的 count 值 2，再一次 setState，然后能实时拿到 count 的值为 3。
## DOM 原生事件
```js
import React, { Component } from 'react'

class App extends Component {
  state = { count: 1 }

  componentDidMount() {
    document.getElementById('btn').addEventListener('click', this.handleClick)
  }

  handleClick = () => {
    this.setState({ count: this.state.count + 1 })
    console.log(this.state.count) // 2

    this.setState({ count: this.state.count + 1 })
    console.log(this.state.count) // 3

    this.setState({ count: this.state.count + 1 })
    console.log(this.state.count) // 4
  }

  render() {
    return (
      <>
        <button id='btn'>触发原生事件</button>
        <div>{this.state.count}</div>
      </>
    )
  }
}

export default App
```
点击按钮，会发现每次 setState 打印出来的值都是实时拿到的，不会进行批量更新。

在 DOM 原生事件里面，setState 也是同步的。
> 在setTimeout或者原生dom事件中，由于是同步的操作，所以并不会进行覆盖现象

## 为什么建议传递给 setState 的参数是一个 callback 而不是一个对象？
- setState它是一个异步函数，他会合并多次修改，降低diff算法的比对频率。这样也会提升性能。
- 但更新是异步的，不能依赖它们的值去计算下一个 state。而setState 中函数写法的 preState 参数，总是能拿到即时更新（同步）的值。

## 使用原则是什么？

- 如果新状态不依赖于原状态【使用对象方式】
- 如果新状态依赖于原状态 【使用函数方式】
- 如果需要在setState()执行后，获取最新的状态数据，可以在第二个callback函数中读取到异步更新的最新值
- 在组件生命周期或React合成事件中，setState是异步；
- this.state是否异步，关键是看是否命中 batchUpdata 机制，命中就异步，未命中就同步。
- setState 中的 preState 参数，总是能拿到即时更新（同步）的值。
- 在setTimeout或者原生dom事件中，setState是同步；
- 不要直接修改state中的引用数据

## 执行过程
1. 将setState传入的partialState参数存储在当前组件实例的state暂存队列中。
2. 判断当前React是否处于批量更新状态，如果是，将当前组件加入待更新的组件队列中。
3. 如果未处于批量更新状态，将批量更新状态标识设置为true，用事务再次调用前一步方法，保证当前组件加入到了待更新组件队列中。
4. 调用事务的waper方法，遍历待更新组件队列依次执行更新。
5. 执行生命周期componentWillReceiveProps。
6. 将组件的state暂存队列中的state进行合并，获得最终要更新的state对象，并将队列置为空。
7. 执行生命周期componentShouldUpdate，根据返回值判断是否要继续更新。
8. 执行生命周期componentWillUpdate。
9. 执行真正的更新，render。
10. 执行生命周期componentDidUpdate。