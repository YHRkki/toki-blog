### 单行文本

```css
text-overflow: ellipsis;
overflow: hidden;
white-space: nowrap;
```

### 多行文本 有浏览器兼容问题

```css
overflow: hidden;
-webkit-line-clamp: 3; 
-webkit-box-orient: vertical;
display: -webkit-box;
```

### RN 文字省略

```
numberOfLines属性
```

