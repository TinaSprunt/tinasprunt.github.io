# 作用域与作用域链

## 一、什么是作用域

作用域就是一个独立的代码区域，域内的变量不会暴露到外部，外部无法访问，也就是说具有隔离性。

```js
function outFun() {
  var inVariable = "内层变量2";
}
outFun();
//  inVariable 的作用域仅在函数outFun内，外部无法访问
console.log(inVariable); // 结果 Uncaught ReferenceError: inVariable is not defined
```

JavaScript 使用词法作用域，也就说定义时就已经决定了作用域范围

> JavaScript uses lexical scoping. This means that functions are executed using the variable scope that was in effect when they were defined, not the variable scope that is in effect when they are invoked.
>
> JavaScript 使用词法作用域。 这意味着函数是使用定义时有效的变量范围来执行的，而不是调用它们时有效的变量范围。

## 二、作用域类型及效果

js 具有以下 3 类作用域

- 全局作用域
- 函数作用域
- 块级作用域（ES6 之后才有的）

### 全局作用域

全局作用域就表示可以在代码任何地方访问到，以下几种情况均会产生全局作用域

- 最外层函数
- 最外层函数外面定义的变量
- 未定义直接赋值的变量
- 所有 windowns 对象的属性

**最外层函数 和 最外层函数外面定义的变量**

```js
//最外层变量
var outVariable = "out variable";

//最外层函数
function outFun() {
  //内层变量
  var inVariable = "inner variable";

  //内层函数
  function innerFun() {
    console.log(inVariable);
  }
  innerFun();
}

// 最外层函数外的变量，作用域是全局，可以直接访问
console.log(outVariable); // 结果 out variable

// 最外层函数作用域是全局，可以直接访问
outFun(); // 结果 inner variable

// 局部变量，作用域仅在 outFun 函数内部，外部不可以访问
console.log(inVariable); //inVariable is not defined

// 内部函数， 作用域仅在 outFun 函数内部，外部不可以访问
innerFun(); //innerFun is not defined
```

**未定义直接赋值的变量**

```js
function outFun() {
  undefVariable = "undefined variable";
}

outFun();
// 未定义直接赋值的变量，作用域是全局，可以直接访问
console.log(undefVariable); // 结果 undefined variable
```

**所有 windowns 对象的属性**

例如 window.name、window.location、window.top 等等，更多 windowns 属性见 [MDN Window](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)


## 参考文献

- https://github.com/mqyqingfeng/Blog/issues/3
- https://juejin.cn/post/6844903797135769614
- https://developer.mozilla.org/zh-CN/docs/Web/API/Window
