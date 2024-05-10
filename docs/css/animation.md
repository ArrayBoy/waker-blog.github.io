#### 1. 把一个div变成圆左边蓝色 右边绿色，鼠标放上去圆旋转

```css
.ex1{
    width: 300px;
    height: 300px;
    background:linear-gradient(to right,blue 50%, green 50%);  
    border-radius: 150px;
    transition: all 0.5s;
}
.ex1:hover {
    transform: rotate(360deg);
}
```

```html
<div class="ex1"></div>
```