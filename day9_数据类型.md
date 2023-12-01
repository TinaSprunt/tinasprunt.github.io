# 数据类型

## 一、js 数据类型

- Null
- Undefined
- Boolean
- Number
- BigInt
- String
- Symbol
- Object

### Null

Null 类型 只有一个值 null

### Undefined

Undefined 类型 只有一个值 undefined ，undefined 表示值缺失， null 表示对象缺失

### Boolean

Boolean 类型有 2 个值，true 和 false

### Number

1. 创建方式

- 使用 `let xxx = 1` 新建 number (创出来的是 number 原始值)
- 使用 `let xxx = new Number(1)` 函数创建（创出来的是 number 对象，MMDN 官方建议尽量不要这样用）

```js
let a = 1
let b = new Number(1)

console.log(typeof a, a); // number 1 
console.log(typeof b, b); // object Number {1}
```

2. Number 类型可以安全存储以下范围内的数字

- `2的-1074次方`（Number.MIN_VALUE）和 `2的1024次方`（Number.MAX_VALUE）之间的正浮点数
- `-2的-1074次方`（Number.MIN_VALUE）和 `-2的1024次方`（Number.MAX_VALUE）之间的负浮点数
- `-(2的53次方 − 1)`（Number.MIN_SAFE_INTEGER）到 `2的53次方 − 1`（Number.MAX_SAFE_INTEGER）范围内的整数

3. 检查数字是否在安全范围方法 `Number.isSafeInteger(x)`

4. `NaN`是运算结果不表示数字时的结果，也是 js 中唯一不等于自身的值

### BigInt

1. 如何使用 BigInt

- 将 `n` 附加在整数末尾
- 调用 `BigInt()`

```js
const x = 9999999999999n;

const y = BigInt(9999999999999);

console.log(x == y); // 结果为 true
```

2. BigInt 可以用来存储任意大的整数，但是不能存储浮点数

```js
const x = 99999999999.99n
console.log(x) // 结果为报错 Uncaught SyntaxError: Invalid or unexpected token
```

3. BigInt 运算过程中产生的浮点数会被取整

```js
const x = 4n;
const y = 3n;
console.log(x / y); // 结果是 1n 而不是 1.3333333333333
```

4. BigInt 与 Number 不严格相等，但是宽松相等

```js
const x = 1n; // BigInt 类型的数值 1

const y = 1; // Number 类型的数值 1

console.log(x === y); // 结果为 false
console.log(x == y); // 结果为 true
```

5. BigInt 和 Number 类型的数值可以进行比较, 也可以混合在一个数组中进行排序

```js
const mixed = [4n, 6, -12n, 10, 4, 0, 0n];
// ↪  [4n, 6, -12n, 10, 4, 0, 0n]

mixed.sort();
// ↪ [-12n, 0, 0n, 10, 4n, 4, 6]
```

6. 被 Object 包装的 BigInt 使用 Object 的比较规则

```js
const x = Object(0n);
const y = Object(0n);
console.log(x === y); // false
```

7. BigInt 类型的数值不可以直接 JSON.stringify() 转换为 JSON ，因为无法序列化

```js
const x = BigInt(1);
console.log(JSON.stringify(x)); // Uncaught TypeError: Do not know how to serialize a BigInt

// 先定义 toJSON 方法
BigInt.prototype.toJSON = function () {
  return this.toString();
};

// 使用定义的 toJSON 方法进行序列化
const y = BigInt(1);
console.log(y.toJSON()); // 1
```

8. BigInt 与 Number 转换会损失精度，仅建议在数据大于 `2的1024次方` 时使用

### String

1. 创建方式

- 使用 `''` 或 `""` 新建字符串(创出来的是字符串原始值)
- 使用 `new String()` 函数创建（创出来的是字符串对象，MMDN 官方建议尽量不要这样用）

```js
const s1 = "2 + 2";
const s2 = new String("2 + 2");
const s3 = s2.valueOf(); // String 对象始终可以使用 valueOf() 方法将其转换为对应的原始值

console.log(s1); // 结果 2 + 2
console.log(s2); // 结果 String {'2 + 2'}
console.log(s3); // 结果 2 + 2
```

2. 字符串比较（所有比较都区分大小写）

```js
const a = "a";
const b = "b";
console.log(a < b); // 结果 true 即 "a" < "b"

const a = "a";
const A = "A";
console.log(a < A); // 结果 false 即同字母，小写 > 大写
```

3. 字符串可以按照下标进行访问

```js
const s1 = "hello world!";
console.log(s1[6]); // 结果为 w ，空格也占据下标
```

### Symbol

1. 如何创建 Symbol 类型的值

```js
const symbol1 = Symbol();
console.log(typeof symbol1); // 结果 symbol

const symbol2 = Symbol(42);
console.log(symbol2); // 结果 Symbol(42)
```

2. Symbol 是**唯一并且不可变的**原始值

```js
const symbol3 = Symbol(42);
console.log(symbol3 === 42); // 结果 false，不可修改

const symbol4 = Symbol("foo");
console.log(
  typeof symbol4.toString(),
  symbol4.toString(),
  typeof symbol4,
  symbol4
); // 结果 string Symbol(foo) symbol Symbol(foo)
```

3. 每次都创建新的 Symbol

```js
const symbol5 = Symbol("foo");
const symbol6 = Symbol("foo");
console.log(symbol5 === symbol6); // 结果 false
```

### Object

## 二、类型判断

1. typeof：能判断所有值类型，函数。不可对 null、对象、数组进行精确判断，因为都返回 object

```js
console.log(typeof undefined); // undefined
console.log(typeof 2); // number
console.log(typeof true); // boolean
console.log(typeof "str"); // string
console.log(typeof Symbol("foo")); // symbol
console.log(typeof 2172141653n); // bigint
console.log(typeof function () {}); // function
// 不能判别
console.log(typeof []); // object
console.log(typeof {}); // object
console.log(typeof null); // object
```

2. instanceof: 能判断对象类型（包括继承关系都能找到），但不能判断基础数据类型

instanceof 运行机制是 判断在其原型链中能否找到该类型的原型

```js
class People {}
class Student extends People {}

const vortesnail = new Student();

console.log(vortesnail instanceof People); // true
console.log(vortesnail instanceof Student); // true
```

3. Object.prototype.toString.call()：所有原始数据类型都是能判断的，还有 **Error 对象，Date 对象**等

```js
Object.prototype.toString.call(2); // "[object Number]"
Object.prototype.toString.call(""); // "[object String]"
Object.prototype.toString.call(true); // "[object Boolean]"
Object.prototype.toString.call(undefined); // "[object Undefined]"
Object.prototype.toString.call(null); // "[object Null]"
Object.prototype.toString.call(Math); // "[object Math]"
Object.prototype.toString.call({}); // "[object Object]"
Object.prototype.toString.call([]); // "[object Array]"
Object.prototype.toString.call(function () {}); // "[object Function]"
```

## 三、类型判断实践

如何判断一个变量为数组

```js
const arr = ["111", "sss1", "**", 12, { name: "tom" }, [12, "a1"]];

// 1. Array.isArray() 函数
Array.isArray(arr); // true

// 2. __ proto__属性 判断
arr.__proto__ === Array.prototype; // true

// 3. instanceof 查找继承
arr instanceof Array; // true

// 4.  Object.prototype.toString.call() 判断
Object.prototype.toString.call(arr); // "[object Array]"
```

## 四、参考资料

- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures#string_%E7%B1%BB%E5%9E%8B
- https://blog.csdn.net/aaahuahua/article/details/123882822
