# ES6 新特性

<!-- toc -->

## 何谓 ES6 ？

- ECMAScript（ES） 是规范、 JavaScript 是 ES 的实现  
- ES6 的第一个版本 在 2015 年 6 月发布，正式名称是《ECMAScript 2015 标准》（简称 ES2015）  
- ES6 指是 5.1 版以后的 JavaScript 的下一代标准，涵盖了 ES2015、ES2016、ES2017 等等  
  

## 1.1 let  VS var

使用 let 可以避免变量提升、变量污染、变量重复声明和临时死区等问题，推荐在 ES6 标准中使用 let 声明变量。

 和 let 相比，var 存在诸多问题，比如：  

### 1.1.1 越域  

变量作用域：使用 let 声明的变量具有块级作用域，在 {} 内部声明的变量对外不可见，而使用 var 声明的变量具有函数级作用域，会被提升到函数的顶部。这样可以减少变量的污染和包围性问题。

```js
{
  var a = 1;
  let b = 1;
}
console.log (a) // 输出 1
console.log (b) // 报错 Uncaught ReferenceError: b is not defined
```

### 1.1.2 重复声明  

变量重复声明：使用 let 声明的变量不允许重复声明，而使用 var 声明的变量可以在同一作用域内重复声明，会覆盖前面的声明。
  
```javascript
// 使用 let 声明变量
let x = 10;
let x = 20; // SyntaxError: Identifier 'x' has already been declared

// 使用 var 声明变量
var x = 10;
var x = 20; // 没有错误，x 被重新赋值为 20
```
### 1.1.3 变量提升  

临时死区：使用 let 声明的变量存在临时死区（temporal dead zone），在声明之前使用变量会报错，而使用 var 则不存在这种情况。

```javascript
// 使用 let 声明变量
console.log (x); // ReferenceError: Cannot access 'x' before initialization
let x = 10;

// 使用 var 声明变量
console.log (x); // 输出 undefined
var x = 10;
```


## 1.2 const  

```js
// 1. 声明之后不允许改变
// 2. 一但声明必须初始化，否则会报错
const a = 1;
a = 3; //Uncaught TypeError: Assignment to constant variable.
```

## 1.3 解构  

### 1.3.1 数组解构  

```js
let arr = [1, 2, 3];
// 以前我们想获取其中的值，只能通过角标。ES6 可以这样：
const [x, y, z] = arr;//x，y，z 将与 arr 中的每个位置对应来取值
// 然后打印
console.log (x, y, z);
```

### 1.3.2 对象解构  

```js
const person = {
    name: "jack",
    age: 21,
    language: ['java', 'js', 'css']
}
// 解构表达式获取值，将 person 里面每一个属性和左边对应赋值
const {name, age, language} = person;
// 等价于下面
//const name = person.name;
//const age = person.age;
//const language = person.language;
// 可以分别打印
console.log (name);
console.log (age);
console.log (language);
```

// 扩展：如果想要将 name 的值赋值给其他变量，可以如下，nn 是新的变量名

```js
const person = {
    name: "jack",
    age: 21,
    language: ['java', 'js', 'css']
}
const {name: nn, age, language} = person;
console.log (nn);
console.log (age);
console.log (language);
```

## 1.4 链判断  

如果读取对象内部的某个属性，往往需要判断一下，属性的上层对象是否存在。  
比如，读取 `message.body.user.firstName` 这个属性，安全的写法是写成下面这样。  

```js
let  message = null;
// 错误的写法
const  firstName = message.body.user.firstName || 'default';

// 正确的写法
const firstName = (message
                   && message.body
                   && message.body.user
                   && message.body.user.firstName) || 'default';
console.log (firstName)
```
  
这样的层层判断非常麻烦，因此 [ES2020](https://github.com/tc39/proposal-optional-chaining) 引入了 “链判断运算符”（optional chaining operator）?.，简化上面的写法。  

```js
let message = null;
const firstName = message?.body?.user?.firstName || 'default';
console.log (firstName);
```

## 1.5 参数默认值  

```js
// 在 ES6 以前，我们无法给一个函数参数设置默认值，只能采用变通写法：
function add (a, b) {
  // 判断 b 是否为空，为空就给默认值 1
  b = b || 1;
  return a + b;
}
// 传一个参数
console.log (add (10));

// 现在可以这么写：直接给参数写上默认值，没传就会自动使用默认值
function add2 (a, b = 1) {
  return a + b;
}

// 传一个参数
console.log (add2 (10));
```  

## 1.6 箭头函数  

以前声明一个方法：

```js
var print = function (obj) {
  console.log (obj);
}
```

现在可以简写为：

```js

let print = obj => console.log (obj);
// 测试调用
print (100);
```

两个参数的情况：

```js
let sum = function (a, b) {
    return a + b;
}
// 简写为：
// 当只有一行语句，并且需要返回结果时，可以省略 {} , 结果会自动返回。
let sum2 = (a, b) => a + b;
// 测试调用
console.log (sum2 (10, 10));//20
// 代码不止一行，可以用 `{}` 括起来
let sum3 = (a, b) => {
    c = a + b;
    return c;
};
// 测试调用
console.log (sum3 (10, 20));//30
```

## 1.7 模板字符串  

以前的写法：

```js
let name = "张三";
    let person = {
      age: 25,
      email: "123@qq.com"
    }
    const { age, email } = person;
    let info = "你好，我的名字是：【" + name + "】，年龄是：【" + age + "】，邮箱是：【" + email + "】"
    console.log (info);
```

模板字符串的写法：

```js
let name = "张三";
    let person = {
      age: 25,
      email: "123@qq.com"
    }
    let info = `你好，我的名字是：${name}，年龄是：${person.age}，邮箱是：${person.email}`
    console.log (info);
```
## 1.8 Promise  

代表 异步对象，类似 `Java` 中的 `CompletableFuture` 

Promise 是现代 JavaScript 中异步编程的基础，是一个由异步函数返回的可以向我们指示当前操作所处的状态的对象。在 Promise 返回给调用者的时候，操作往往还没有完成，但 Promise 对象可以让我们操作最终完成时对其进行处理（无论成功还是失败）  。

### 1.8.1 fetch api  

fetch 是浏览器支持从远程获取数据的一个函数，这个函数返回的就是 Promise 对象  。使用示例：

```js
const fetchPromise = fetch (
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json"
);

console.log (fetchPromise);

fetchPromise.then ((response) => {
  console.log (`已收到响应：${response.status}`);
});

console.log ("已发送请求……");
```


通过 fetch () API 得到一个 Response 对象： 

- response.status： 读取响应状态码  
- response.json ()：读取响应体 json 数据（这也是个异步对象）；

使用示例：

```js
const fetchPromise = fetch (
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

fetchPromise.then ((response) => {
  const jsonPromise = response.json ();
  jsonPromise.then ((json) => {
	console.log (json [0].name);
  });
});
```

### 1.8.2 Promise 状态  

首先，Promise 有三种状态：  

- 待定（pending）：初始状态，既没有被兑现，也没有被拒绝。这是调用 fetch () 返回 Promise 时的状态，此时请求还在进行中。  
- 已兑现（fulfilled）：意味着操作成功完成。当 Promise 完成时，它的 then () 处理函数被调用。  
- 已拒绝（rejected）：意味着操作失败。当一个 Promise 失败时，它的 catch () 处理函数被调用。  

### 1.8.3Promise 对象  

代码模板：

```js
const promise = new Promise ((resolve, reject) => {
// 执行异步操作
if (/* 异步操作成功 */) {
      resolve (value);// 调用 resolve，代表 Promise 将返回成功的结果
    } else {
      reject (error);// 调用 reject，代表 Promise 会返回失败结果
    }
});
```

ES6 之前写法示例：  

```js
let get = function (url, data) {
	return new Promise ((resolve, reject) => {
		$.ajax ({
			url: url,
			type: "GET",
			data: data,
			success (result) {
				resolve (result);
			},
			error (error) {
				reject (error);
			}
		});
	})
}
```

## 1.9Async 函数  

async function 声明创建一个绑定到给定名称的新异步函数。函数体内允许使用 await 关键字，这使得我们可以更简洁地编写基于 promise 的异步代码，并且避免了显式地配置 promise 链的需要。  

async 函数是使用 async 关键字声明的函数。async 函数是 [AsyncFunction](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/AsyncFunction) 构造函数的实例，并且其中允许使用 await 关键字。  

async 和 await 关键字让我们可以用一种更简洁的方式写出基于 [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 的异步行为，而无需刻意地链式调用 promise。  

async 函数 返回的还是 Promise 对象 。

代码模板如下所示：

```js
async function myFunction () {
  // 这是一个异步函数

}
```

在异步函数中，你可以在调用一个返回 Promise 的函数之前使用 await 关键字。这使得代码在该点上等待，直到 Promise 被完成，这时 Promise 的响应被当作返回值，或者被拒绝的响应被作为错误抛出。  

使用示例：

```js
async function fetchProducts () {
  try {
    // 在这一行之后，我们的函数将等待 `fetch ()` 调用完成
    // 调用 `fetch ()` 将返回一个 “响应” 或抛出一个错误
    const response = await fetch (
      "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
    );
    if (!response.ok) {
      throw new Error (`HTTP 请求错误：${response.status}`);
    }
    // 在这一行之后，我们的函数将等待 `response.json ()` 的调用完成
    // `response.json ()` 调用将返回 JSON 对象或抛出一个错误
    const json = await response.json ();
    console.log (json [0].name);
  } catch (error) {
    console.error (`无法获取产品列表：${error}`);
  }
}

fetchProducts ();
```


## 1.10 模块化 

将 JavaScript 程序拆分为可按需导入的单独模块的机制。Node.js 已经提供这个能力很长时间了，还有很多的 JavaScript 库和框架已经开始了模块的使用（例如，[CommonJS](https://en.wikipedia.org/wiki/CommonJS) 和基于 [AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md) 的其他模块系统 如 [RequireJS](https://requirejs.org/)，以及最新的 [Webpack](https://webpack.github.io/) 和 [Babel](https://babeljs.io/)）。  

好消息是，最新的浏览器开始原生支持模块功能了。  

`index.html`  内容如下所示：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="main.js" type="module"/>
</head>
<body>
<h1 > 模块化测试 </h1>
</body>
</html>
```

指定脚本类型为 `module`即可
