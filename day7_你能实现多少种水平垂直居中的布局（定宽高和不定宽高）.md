# 你能实现多少种水平垂直居中的布局（定宽高和不定宽高）

## 一、定宽高

### 绝对定位 + 负 margin 值

```vue
<template>
  <div id="app">
    <div class="box">
      <div class="children-box"></div>
    </div>
  </div>
</template>
<style type="text/css">
.box {
  width: 200px;
  height: 200px;
  border: 1px solid red;
  position: relative; /*父元素相对定位*/
}
.children-box {
  position: absolute; /*子元素绝对定位*/
  width: 100px;
  height: 100px;
  background: yellow;
  left: 50%; /*定位移动至右下角*/
  top: 50%;
  margin-left: -50px; /*根据父元素大小计算需要 负margin 移动的距离*/
  margin-top: -50px;
}
</style>
```

### 绝对定位 + transform

> transform 只能转换由盒模型定位的元素

语法：

- transform: translateX(x); /_沿 X 轴方向平移_/
- transform: translateY(y); /_沿 Y 轴方向平移_/
- transform: translate(x, y); /_沿 X 轴和 Y 轴同时平移_/

参考：http://www.lvyestudy.com/css3/transform-translate

```vue
<template>
  <div id="app">
    <div class="box">
      <div class="children-box"></div>
    </div>
  </div>
</template>

<style scoped>
.box {
  width: 200px;
  height: 200px;
  border: 1px solid red;
  position: relative;
}
.children-box {
  position: absolute;
  width: 100px;
  height: 100px;
  background: yellow;
  left: 50%;
  top: 50%;
  transform: translate(
    -50%,
    -50%
  ); /*与上一个相同，仅仅是 负margin 移动换成 transform 移动*/
}
</style>
```

### 绝对定位 + left/right/bottom/top + margin

```vue
<template>
  <div id="app">
    <div class="box">
      <div class="children-box"></div>
    </div>
  </div>
</template>
<style type="text/css">
.box {
  width: 200px;
  height: 200px;
  border: 1px solid red;
  position: relative; /*父元素相对定位*/
}
.children-box {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0; /** 子元素绝对定位，且left/right/bottom/top 全部定位为0*/
  background: yellow;
  margin: auto; /*margin auto自适应*/
  height: 100px;
  width: 100px;
}
</style>
```

### flex 布局

> display 属性设置元素是否被视为块或者内联元素以及用于子元素的布局，例如流式布局（默认的布局方式）、网格布局（ grid ）或弹性布局（ flex ）。
> 参考： https://www.runoob.com/cssref/pr-class-display.html

```vue
<template>
  <div id="app">
    <div class="box">
      <div class="children-box"></div>
    </div>
  </div>
</template>
<style type="text/css">
.box {
  width: 200px;
  height: 200px;
  border: 1px solid red;
  display: flex; /**父元素 flex 弹性布局*/
  justify-content: center; /**内部元素 主轴居中（横轴） */
  align-items: center; /**内部元素 交叉轴居中 （纵轴）*/
}
.children-box {
  background: yellow;
  height: 100px;
  width: 100px;
}
</style>
```

### grid 布局

```vue
<template>
  <div id="app">
    <div class="box">
      <div class="children-box"></div>
    </div>
  </div>
</template>
<style type="text/css">
.box {
  width: 200px;
  height: 200px;
  border: 1px solid red;
  display: grid; /**父元素 grid 网格布局*/
}
.children-box {
  width: 100px;
  height: 100px;
  background: yellow;
  margin: auto; /**子元素 margin auto 自适应*/
}
</style>
```

### table-cell + vertical-align + inline-block/margin: auto

```vue
<template>
  <div id="app">
    <div class="box">
      <div class="children-box"></div>
    </div>
  </div>
</template>
<style type="text/css">
.box {
  width: 200px;
  height: 200px;
  border: 1px solid red;
  display: table-cell; /* 作为一个表格单元格显示（类似 `<td>` 和 `<th>`）*/
  text-align: center; /* 内部元素横向居中 */
  vertical-align: middle; /* 内部元素纵向居中*/
}
.children-box {
  width: 100px;
  height: 100px;
  background: yellow;
  display: inline-block; /* 布局为 行内块 元素*/
  /* display: inline-block; 也可以换成 margin: auto; */
}
</style>
```

## 不定宽高

### 绝对定位 + transform

```vue
<template>
  <div id="app">
    <div class="box">
      <div class="children-box">111111</div>
    </div>
  </div>
</template>
<style type="text/css">
.box {
  width: 200px;
  height: 200px;
  border: 1px solid red;
  position: relative;
}
.children-box {
  position: absolute;
  background: yellow;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%); 
}
</style>
```

### table-cell























## display 特性补充

- none 此元素不会被显示。
- block 此元素将显示为块级元素，此元素前后会带有换行符。
- inline 默认。此元素会被显示为内联元素，元素前后没有换行符。
- inline-block 行内块元素。（CSS2.1 新增的值）
- list-item 此元素会作为列表显示。
- run-in 此元素会根据上下文作为块级元素或内联元素显示。
- table 此元素会作为块级表格来显示（类似 `<table>）`，表格前后带有换行符。
- inline-table 此元素会作为内联表格来显示（类似 `<table>`），表格前后没有换行符。
- table-row-group 此元素会作为一个或多个行的分组来显示（类似 `<tbody>`）。
- table-header-group 此元素会作为一个或多个行的分组来显示（类似 `<thead>`）。
- table-footer-group 此元素会作为一个或多个行的分组来显示（类似 `<tfoot>`）。
- flow-root 生成一个块级元素盒，其会建立一个新的块级格式化上下文，定义格式化上下文的根元素。
- table-row 此元素会作为一个表格行显示（类似 `<tr>`）。
- table-column-group 此元素会作为一个或多个列的分组来显示（类似 `<colgroup>`）。
- table-column 此元素会作为一个单元格列显示（类似 `<col>`）
- table-cell 此元素会作为一个表格单元格显示（类似 `<td>` 和 `<th>`）
- table-caption 此元素会作为一个表格标题显示（类似 `<caption>`）
- inherit 规定应该从父元素继承 display 属性的值。

### 参考文档

- https://juejin.cn/post/6844903982960214029
- https://juejin.cn/post/7061588533214969892
- http://www.lvyestudy.com/css3/transform-translate
- https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html
- https://www.runoob.com/cssref/pr-class-display.html
