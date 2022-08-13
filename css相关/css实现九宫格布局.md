### 方法一：利用grid创建网络布局
[参考文档](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)
```css
/*指定一个三行三列的网格，列宽和行高都是100px*/
.wrapper {
  display: grid;
  grid-template-columns: 100px 100px 100px; /*定义每一列的列宽*/
  grid-template-rows: 100px 100px 100px;/*定义每一行的行高*/
}
.list {
  background: #eee;
}
.list:nth-child(odd) {
  background: #999;
}
```
#### html
```html
<div class="wrapper">
  <div class="list list1">1</div>
  <div class="list list2">2</div>
  <div class="list list3">3</div>
  <div class="list list4">4</div>
  <div class="list list5">5</div>
  <div class="list list6">6</div>
  <div class="list list7">7</div>
  <div class="list list8">8</div>
  <div class="list list9">9</div>
</div>
```
### 方法二：利用display：table
1. 三行li，每个li里三列div（模拟表格的结构）

2. 父元素ul使用display: table（此元素会作为块级表格来显示（类似 `<table>`），表格前后带有换行符。）

3. li元素使用display: table-row （此元素会作为一个表格行显示，（类似 `<tr>`））

4. li元素内部三个子元素使用display: table-cell（此元素会作为一个表格单元格显示（类似 `<td>`和 `<th>`））
```css
.table {
  display: table;
}

.table li {
  display: table-row;
  background: #beffee;
}

.table li:nth-child(odd) {
  background: #bec3ff;
}

.table li div {
  width: 200px;
  line-height: 200px;
  display: table-cell;
  text-align: center;
}

.table li:nth-child(odd) div:nth-child(even) {
  background: #beffee;
}

.table li:nth-child(even) div:nth-child(even) {
  background: #bec3ff;
}
```
#### html
```html
<ul class="table">
  <li>
    <div>1</div>
    <div>2</div>
    <div>3</div>
  </li>
  <li>
    <div>4</div>
    <div>5</div>
    <div>6</div>
  </li>
  <li>
    <div>7</div>
    <div>8</div>
    <div>9</div>
  </li>
</ul>
```
### 方法三：利用flex弹性布局
```css
.flexDiv {
  width: 100%;
  display: flex;
  /*换行*/
  flex-wrap: wrap;
}

.flex {
  width: calc(calc(100% / 3) - 10px);
  margin: 5px;
  height: 200px;
  box-sizing: border-box;
  border: 1px solid #000;
}

.flex:nth-of-type(odd) {
  background: red;
}
```
#### html
```html
<div class="flexDiv">
  <div class="flex"></div>
  <div class="flex"></div>
  <div class="flex"></div>
  <div class="flex"></div>
  <div class="flex"></div>
  <div class="flex"></div>
  <div class="flex"></div>
  <div class="flex"></div>
  <div class="flex"></div>
</div>
```
除此之外还可以使用浮动和定位实现，[具体参考地址](https://blog.csdn.net/qq_24698097/article/details/114230533)