
![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/7/4/16bbcb7e8f8afd21?w=724&h=758&f=png&s=87597)
**使用场景**
当两个组件存在相同的方法，例如需要进行初始化，例如分页操作，进入页面时，需要对页面初始化页面。这个时候你可以选择传统的写组件来进行分离，但是使用mixins可以不通过状态传递，直接进行强大的混合，方便了许多。 

mixins允许你封装一块在应用的其他组件中都可以使用的函数。如果使用姿势得当，他们不会改变函数作用域外部的任何东西，因此哪怕执行多次，只要是同样的输入你总是能得到一样的值，

对于有冲突的代码，这里可以分为两个情况，如果是vue生命周期里的钩子函数，那将会进行合并，以此执行mixins以及组件的函数；如果是methods等方法，（不是钩子函数）那组件中的方法将会覆盖mixins中的方法。

> Mixins 能够灵活地实现为 Vue 组件分发可复用功能。

**在 Vue 中 mixins 有两种类型：**

1. 局部 Mixins： 这就是我们在这篇文章中所处理的类型。它的范围仅局限于导入和注册的组件。局部 mixin 的影响范围由引入它的组件所决定。
2. 全局 Mixins： 这是一种不同的 mixin，无论在任何 Vue 项目中，它是定义在 Main.js 文件中的。它会影响一个应用中的所有组件，所以 Vue 的开发团队建议要谨慎使用。一个全局 mixin 的定义看起来就像这样：
```javascript
Vue.mixin({
  mounted() {
    console.log("hello world!");
  }
});
```

**使用**

创建mixins文件夹
![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/7/4/16bbcb7e92fd270b?w=284&h=79&f=png&s=4210)
通过import引入并在mixins里声明一次
```javascript
import DeviceCurtain from '../Mixins/deviceCurtain'

export default {
	mixins: [DeviceCurtain],
}
```
