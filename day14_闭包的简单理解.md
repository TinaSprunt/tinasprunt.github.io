# 闭包的简单理解

## 一个最简的闭包

MDN 定义：在 js 中，每创建一个函数，闭包就会在函数创建的同时被创建出来。可以在一个内层函数中访问到其外层函数的作用域。

通俗来说，闭包是指那些能够访问自由变量的函数。 **闭包 = 函数 + 函数能够访问的自由变量。**

**自由变量：是指在函数中使用的，但既不是函数参数也不是函数的局部变量的变量。**

```js
var a = 1;

function func1() {
  console.log(a);
}

func1();
```

如上案例，func1 函数可以访问变量 a ，变量 a 既不是 func1 函数的参数、也不是 func1 函数的局部变量，所以 a 是一个自由变量

此时， func1 函数和自由变量 a 就构成了一个闭包

## 如何理解、判断、使用闭包

由上述案例就是理论上的闭包（广义闭包），但是我们实践中经常讲的和使用的是狭义闭包（满足广义闭包）：

- **虽然执行上下文被销毁，但作用域链里仍然保存着相关信息**（比如，内部函数从父函数中返回）
- **在代码中引用了自由变量**

以下是理解这 2 句话的经典案例

```js
var scope = "global scope";
function checkscope() {
  var scope = "local scope";
  function f() {
    return scope;
  }
  return f; // 从父函数返回的内部函数 f
}

var foo = checkscope();
foo(); // 结果：local scope
```

上述案例输出结果为 local scope , 执行函数 f 时, f 函数内部没有 scope 变量，根据作用域链，向上一级查找， 找到了赋值为 local scope 的局部变量 scope，已经找到就不会再往上找了

如果不明白为什么输出是 local scope ，可以参考 [作用域与作用域链](https://github.com/TinaSprunt/tinasprunt.github.io/blob/master/day13_%E4%BD%9C%E7%94%A8%E5%9F%9F%E4%B8%8E%E4%BD%9C%E7%94%A8%E5%9F%9F%E9%93%BE.md)

1. 什么是从父函数返回的内部函数？

- f 就是从父函数返回的内部函数

2. 执行过程中的上下文变化

- 1. 进入全局代码，创建全局执行上下文 globalContext ，globalContext 压入执行上下文栈

- 2. 执行 checkscope 函数， 创建 checkscope 函数执行上下文 checkscopeContext ，checkscopeContext 压入执行上下文栈，，checkscopeContext 初始化，创建变量对象、作用域链、this 等

- 3. checkscope 函数执行完毕，checkscopeContext 从执行上下文栈中弹出

- 4. 执行 f 函数，创建 f 函数执行上下文 fContext ，fContext 被压入执行上下文栈，，fContext 初始化，创建变量对象、作用域链、this 等

- 5. f 函数执行完毕，fContext 从执行上下文栈中弹出

由上述执行过程中上下文的变化可以得知，**f 函数执行的时候，checkscope 函数的上下文 checkscopeContext 已经销毁**，那么是如何读取到属于 checkscope 函数的 local scope 的呢？

因为 f 的执行上下文 fContext 维护了一个作用域链：

```js
fContext = {
  Scope: [AO, checkscopeContext.AO, globalContext.VO],
};
```

可以看到，里面有 checkscope 函数的上下文 checkscopeContext、以及更上一层的 globalContext

**即使 checkscopeContext 本身被销毁， js 依旧会让 checkscopeContext.AO 活在内存中**，这就是为什么可以取到 local scope 的原因

所以再看一遍狭义闭包的定义，就非常清晰了

- 虽然执行上下文被销毁，但作用域链里仍然保存着相关信息
- 在代码中引用了自由变量

## 实践

**案例 1**

```js
// 非闭包的写法
var data = [];

for (var i = 0; i < 3; i++) {
  data[i] = function () {
    console.log(i);
  };
}

data[0](); // 结果 3
data[1](); // 结果 3
data[2](); // 结果 3
```

为什么输出结果都是 3 呢？

当执行到 `data[0]();` 之前, 全局上下文 globalContext 的 VO 为:

```js
globalContext = {
    VO: {
        data: [...],
        i: 3
    }
}
```

执行到 `data[0]();` 函数的时候，`data[0]();` 函数的上下文 `data[0]Context` 中维护的作用域链为：

```js
data[0]Context = {
    Scope: [AO, globalContext.VO]
}
```

`data[0]();` 的 AO 并没有值，于是顺着作用链找，找到 globalContext 的 VO ，而 globalContext 的 VO 中找到的 i 的值是 3 ，所以打印结果是 3

`data[1]` 和 `data[2]` 同理，**因为都不是自己的上下文中找到的 i 的值，都是取的作用域链中 globalContext 的 VO 的 i 值**，于是都打印 3

**案例 2**

```js
// 闭包的写法
var data = [];

for (var i = 0; i < 3; i++) {
  data[i] = (function (i) {
    return function () {
      console.log(i);
    }; // 从父函数返回的内部函数
  })(i);
}

data[0](); // 结果 0
data[1](); // 结果 1
data[2](); // 结果 2
```

当执行到 `data[0]();` 之前, 全局上下文 globalContext 的 VO 依旧为:

```js
globalContext = {
    VO: {
        data: [...],
        i: 3
    }
}
```

但是当执行到函数 `data[0]();` 的时候， `data[0]();` 的上下文 `data[0]Context` 中维护的作用域链就变成了

```js
data[0]Context = {
    Scope: [AO, 匿名函数Context.AO globalContext.VO]
}
```

多出来了 匿名函数的上下文 `匿名函数Context`, `匿名函数Context` 的 AO 为

```js
匿名函数Context = {
  AO: {
    arguments: {
      0: 0,
      length: 1,
    },
    i: 0,
  },
};
```

于是 `data[0]();` 自己的上下文 `data[0]Context` 中找不到 i， 顺着作用链往上找，就找到的是 `匿名函数Context` 的 AO 中的 i , 输出 0

`data[1]()` 和 `data[2]()` 同理，**因为都不是自己的上下文中找到的 i 的值，都是取的各自的上下文中的作用域链中 `匿名函数Context` 的 VO 的 i 值**, 所以 `data[1]()` 输出 1 , `data[2]()` 输出 2

## 参考文献

- https://github.com/mqyqingfeng/Blog/issues/9
- https://javascript.ruanyifeng.com/oop/this.html
- https://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html
