## 新样式
    border-radius：创建圆角边框
    box-shadow：为元素添加阴影
    border-image：使用图片来绘制边框
    background-size：属性常用来调整背景图片的大小
    word-wrap：文字换行
    text-overflow：文本裁剪
## transition 过渡
transition属性可以被指定为一个或多个CSS属性的过渡效果，多个属性之间用逗号进行分隔，必须规定两项内容：

- 过度效果
- 持续时间

语法如下：

    transition： CSS属性，花费时间，效果曲线(默认ease)，延迟时间(默认0)
    transition：(width, 1s, linear, 2s)

## transform 转换
transform属性允许你旋转，缩放，倾斜或平移给定元素

transform-origin：转换元素的位置（围绕那个点进行转换），默认值为(x,y,z):(50%,50%,0)

使用方式：

    transform: translate(120px, 50%)：位移
    transform: scale(2, 0.5)：缩放
    transform: rotate(0.5turn)：旋转
    transform: skew(30deg, 20deg)：倾斜
## animation 动画
动画这个平常用的也很多，主要是做一个预设的动画。和一些页面交互的动画效果，结果和过渡一样，让页面不会那么生硬

    animation-name：动画名称
    animation-duration：动画持续时间
    animation-timing-function：动画时间函数
    animation-delay：动画延迟时间
    animation-iteration-count：动画执行次数，可以设置为一个整数，也可以设置为infinite，意思是无限循环
    animation-direction：动画执行方向
    animation-paly-state：动画播放状态
    animation-fill-mode：动画填充模式