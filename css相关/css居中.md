### CSS 不定宽高的垂直水平居中

```html
<div class="wrapper flex-center">
    <p>horizontal and vertical</p>
</div>
```
1. flex 布局
```css
.wrapper {
    width: 300px;
    height: 300px;
    border: 1px solid #ccc;
}

.flex-center {
    display: flex;
    justify-content: center;
    align-items: center;
}
```
2. 父元素 flex 布局 子元素 margin: auto
```css
.wrapper {
    width: 300px;
    height: 300px;
    border: 1px solid #ccc;
    display: flex;
}

.wrapper > p {
    margin: auto;
}
```
3. absolute + transform
```css
.wrapper {
    width: 300px;
    height: 300px;
    border: 1px solid #ccc;
    position: relative;
}

.wrapper > p {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}
```
4. table-cell
```css
.wrapper {
    width: 300px;
    height: 300px;
    border: 1px solid #ccc;

    display: table-cell;
    text-align: center;
    vertical-align: middle;
}
```
5. absolute + margin: auto
```css
.wrapper {
    width: 300px;
    height: 300px;
    border: 1px solid #ccc;
    position: relative;
}

.wrapper > p {
    width: 170px;
    height: 20px;
    margin: auto;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
}
```
6. grid 布局
```css
.wrapper {
    display: grid;
}
.wrapper > p {
    justify-self: center;
    align-self: center;
}
```
