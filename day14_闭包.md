# 闭包

### 闭包的简单理解

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

### 实践上的闭包

由上述案例就是理论上的闭包（广义闭包），但是我们实践中经常讲的和使用的是狭义闭包（满足广义闭包）：

- **虽然执行上下文被销毁，但作用域链里仍然保存着相关信息**（比如，内部函数从父函数中返回）
- **在代码中引用了自由变量**

以下是理解这 2 句话的几个经典案例

**案例一**：

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

上述案例输出结果为 local scope , 执行函数 f 时, f 函数内部没有 scope 变量，根据作用链，向上一级查找， 找到了赋值为 local scope 的局部变量 scope，已经找到就不会再往上找了

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

可以看到，里面有 checkscope函数的上下文 checkscopeContext、以及更上一层的 globalContext

**即使 checkscopeContext 本身被销毁， js 依旧会让 checkscopeContext.AO 活在内存中**，这就是为什么可以取到 local scope 的原因


