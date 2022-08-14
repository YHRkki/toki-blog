## 具体来说，对于 Redux，我们可以将这些步骤分解为更详细的内容：

#### 初始启动：
- 使用最顶层的 root reducer 函数创建 Redux store
- store 调用一次 root reducer，并将返回值保存为它的初始 state
- 当 UI 首次渲染时，UI 组件访问 Redux store 的当前 state，并使用该数据来决定要呈现的内容。同时监听 store 的更新，以便他们可以知道 state 是否已更改。
#### 更新环节：
- 应用程序中发生了某些事情，例如用户单击按钮
- dispatch 一个 action 到 Redux store，例如 dispatch({type: 'counter/increment'})
- store 用之前的 state 和当前的 action 再次运行 reducer 函数，并将返回值保存为新的 state
- store 通知所有订阅过的 UI，通知它们 store 发生更新
- 每个订阅过 store 数据的 UI 组件都会检查它们需要的 state 部分是否被更新。
- 发现数据被更新的每个组件都强制使用新数据重新渲染，紧接着更新网页

优点
- Redux轻量，生态丰富，可以结合流行的redux-thunk、redux-saga等进行使用
- Redux的写法比较固定，团队应用中风格比较稳定，提高协作中的可维护性
- 因为Redux中的reducer更新时，每次return的都是不可变对象，所以时间旅行操作相对容易

缺点
- 每一次的dispatch都会从根reducer到子reducer嵌套递归的执行，所以效率相对较低；
- Redux核心是不可变对象，在Reducer中的操作都要比较小心，注意不能修改到state的属性
- Redux中写法固定，模板代码较多