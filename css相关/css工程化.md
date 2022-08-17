## css的问题
- 类名冲突的问题

        过深的层级不利于编写、阅读、压缩、复用
        过浅的层级容易导致类名冲突
- 重复样式

        重复的样式值：例如常用颜色、常用尺寸
        重复的代码段：例如绝对定位居中、清除浮动
        重复的嵌套书写
- css文件细分问题

## 如何解决
### 解决类名冲突
命名约定

    就是提供一种命名的标准，来解决冲突，常见的标准有：
    BEM （Block Element Modifier）例如 banner_dot_selected
    OOCSS
    AMCSS
    SMACSS
    其他

css in js

    css in js 的核心思想是：用一个JS对象来描述样式，而不是css样式表
    const styles = {
      backgroundColor: "#f40",
      color: "#fff",
      width: "400px",
      height: "500px",
      margin: "0 auto"
    }

    特点
    绝无冲突的可能：由于它根本不存在类名，所以绝不可能出现类名冲突
    更加灵活：可以充分利用JS语言灵活的特点，用各种招式来处理样式
    应用面更广：只要支持js语言，就可以支持css in js，因此，在一些用JS语言开发移动端应用的时候非常好用，因为移动端应用很有可能并不支持css
    书写不便：书写样式，特别是公共样式的时候，处理起来不是很方便
    在页面中增加了大量冗余内容：在页面中处理css in js时，往往是将样式加入到元素的style属性中，会大量增加元素的内联样式，并且可能会有大量重复，不易阅读最终的页面代码

css module

    通过命名规范来限制类名太过死板，而css in js虽然足够灵活，但是书写不便。

    实现原理：在webpack中，作为处理css的css-loader，它实现了css module的思想，要启用css module，需要将css-loader的配置modules设置为true。

    原理极其简单，开启了css module后，css-loader会将样式中的类名进行转换，转换为一个唯一的hash值。

    由于hash值是根据模块路径和类名生成的，因此，不同的css模块，哪怕具有相同的类名，转换后的hash值也不一样。

    如何应用样式:
    css module带来了一个新的问题：源代码的类名和最终生成的类名是不一样的，而开发者只知道自己写的源代码中的类名，并不知道最终的类名是什么，那如何应用类名到元素上呢？

    为了解决这个问题，css-loader会导出原类名和最终类名的对应关系，该关系是通过一个对象描述的
    .c1 { color: red } => .1g2ufgaujdfgayjugfujsa { color: red }
    同时导出一份对应关系 { c1： '1g2ufgaujdfgayjugfujsa' }

    注意事项

    css module往往配合构建工具使用
    css module仅处理顶级类名，尽量不要书写嵌套的类名，也没有这个必要
    css module仅处理类名，不处理其他选择器
    css module还会处理id选择器，不过任何时候都没有使用id选择器的理由
    使用了css module后，只要能做到让类名望文知意即可，不需要遵守其他任何的命名规范

### 解决重复样式问题
css in js

css预编译器

    有些第三方搞出一套css语言的进化版（less，sass）来解决这个问题，它支持变量、函数等高级语法，然后经过编译器将其编译成为正常的css

    预编译器的原理很简单，即使用一种更加优雅的方式来书写样式代码，通过一个编译器，将其转换为可被浏览器识别的传统css代码。

    // less代码
    @red: #f40;

    .redcolor { color: @red } => .redcolor { color: #f40 }

### 解决css文件细分问题

> 利用webpack拆分css

css-loader

    通过css-loader将css代码转换为js代码，
    处理原理简单到令人发指：就是将css代码作为字符串导出。
    .red { color:"#f40" } => module.exports = `.red { color:"#f40" }`

    css-loader干了什么：
        1. 将css文件的内容作为字符串导出
        2.将css中的其他依赖作为require导入，以便webpack分析依赖

style-loader

    由于css-loader仅提供了将css转换为字符串导出的能力，剩余的事情要交给其他loader或plugin来处理。

    style-loader可以将css-loader转换后的代码进一步处理，将css-loader导出的字符串加入到页面的style元素中

mini-css-extract-plugin 抽离css文件

    目前，css代码被css-loader转换后，交给的是style-loader进行处理。style-loader使用的方式是用一段js代码，将样式加入到style元素中。而实际的开发中，我们往往希望依赖的样式最终形成一个css文件,此时，就需要用到一个库：mini-css-extract-plugin

    该库提供了1个plugin和1个loader
        plugin：负责生成css文件
        loader：负责记录要生成的css文件的内容，同时导出开启css-module后的样式对象