### CSS 不定宽高的垂直水平居中

```html
<div class="wrapper flex-center">
    <p>horizontal and vertical</p>
</div>
```

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
```css
.wrapper {
    display: grid;
}
.wrapper > p {
    justify-self: center;
    align-self: center;
}
```
