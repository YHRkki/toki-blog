### React 性能优化

#### 使用 memo 来缓存组件

父组件在每次状态更新时，都会导致子组件重新渲染，即使传入子组件的状态没有发生变更。

为了减少重复渲染，我们可以使用 `React.memo` 来缓存组件，这样只有当传入组件的状态只发生变化时才会重新渲染，如果传入的只和上一次没有发生变化，则返回缓存的组件：

```javascript
import React from "react";

export default React.memo((props) => <div>{props.value}</div>);
```

#### 使用  useMemo  缓存大量计算

又是渲染是不可避免的，但如果你的组件是一个功能组件，重新渲染会导致每次都调用大型计算函数，这是非常消耗性能的，我们可以使用 `useMemo `来缓存计算结果，这样只有当输入的内容发生变化时，才会重新调用函数计算结果。

通过这种方式，我们达到空间换时间的策略，减少在一帧的工作时间内，js 线程执行时间不影响 GUI 线程，从而提高性能。

```javascript
import { useMemo } from "react";

// 避免这样做
function Component(props) {
  const someProp = heavyCalculation(props.item);
  return <AnotherComponent someProp={someProp} />;
}

// 只有 `props.item` 改变时someProp的值才会被重新计算
function Component(props) {
  const someProp = useMemo(() => heavyCalculation(props.item), [props.item]);
  return <AnotherComponent someProp={someProp} />;
}
```

#### 使用 PureComponent、shouldComponentUpdate

memo 仅针对函数组件，对于 class 组件，我们可以使用 PureComponent 或者是自己书写 shouldComponentUpdate 来优化 diff 是否需要重新渲染当前组件。

```javascript
import React, { Component } from "react";

// 第一种优化
class AnotherComponent extends React.PureComponent {
  render() {
    return <div>{this.props.someOtherProp}</div>;
  }
}

//第二种优化
class AnotherComponent extends Component {
  shouldComponentUpdate(nextProps) {
    return this.props !== nextProps;
  }
  render() {
    return <div>{this.props.someOtherProp}</div>;
  }
}
```

`PureComponent `会进行浅比较判断组件是否需要重新渲染，对于引用类型，只会判断是否是同一份引用。

需要注意的是，浅比较也是需要耗费性能的，所以开发者需要在此进行权衡。

> 同时，React 官方并不提倡开发者自己书写 `shouldComponentUpdate`，官方更加提倡调用方式用 React 官方的 `PureComponent`。

#### 调整 CSS 而不是强制组件加载和卸载

渲染成本很高，尤其是在需要更改 DOM 时。例如想要一次只能看到一个项目时，你可能想要卸载不可见的组件，并在它变得可见时将其重新加载。如果加载/卸载的组件“很重”，则此操作可能非常消耗性能并可能导致延迟。在这些情况下，最好通过 CSS 隐藏它，同时将内容保存到 DOM。

有时在保持组件加载的同时通过 CSS 隐藏可能是有益的，而不是通过卸载来隐藏。对于具有显著的加载/卸载时序的重型组件而言，这是有效的性能优化手段。

```javascript
import { useState } from "react";

// 避免对大型的组件频繁对加载和卸载
function Component(props) {
  const [view, setView] = useState("view1");
  return view === "view1" ? <SomeComponent /> : <AnotherComponent />;
}

// 使用该方式提升性能和速度
const visibleStyles = { opacity: 1 };
const hiddenStyles = { opacity: 0 };

function DemoComponent() {
  const [visible, setVisible] = useState(false);

  return (
    <>
      <Component1 style={visible ? visibleStyles : hiddenStyles} />
    </>
  );
}
```

