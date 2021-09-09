# P4M1Live：Promise

## 同步与异步

JavaScript 最初设计是单线程的，所有任务线性执行。随着需求发展，今天的 JavaScript 是有多线程参与的，但是由主线程主导的。

同步任务是即时、顺序执行的，异步任务与书写顺序无关，它们会被加入任务列队。

> setTimeout()、setInterval() 本身是同步的，但它的执行回调函数的机制是异步的。

任务队列的异步任务会在正确的时机重新插到主线程末尾执行。（这一机制也造成了 setTimeout() 存在不够精准的问题。）

如果要对异步操作的结果进行处理，那就必须要使用回调函数。如果多个异步操作都需要依赖前一个的结果，那就需要嵌套回调，称之为“回调地狱”。

## 原生 Promise（ES6）

中文为“期约”，来自 ES6。Promise 可以解决回调地狱的问题。

Promise 是一个类，使用需要创建一个实例，该 Promise 实例指代我们要进行的异步操作。

状态：创建后的 Promise 实例是待定状态，它只能变成 fulfilled 和 rejected 两种状态，状态确定后不可再更改。

### 使用 Promise

1. 创建 promise 实例  
   使用 new 新建，参数函数内有 resolve 和 reject 两个函数。
2. 指定确定状态后的回调函数  
   Promise 实例调用一下两个函数：
   - then()：成功时的处理函数
   - catch()：失败时的处理函数

例子：封装 ajax 函数

```js
function ajax (url) {
  new Promise (function (resolve, reject) {
    const xhr = new XMLHttpRequest()
    xhr.open()
    xhr.onload = function () {
      if(this.status === 200) {
        resolve(this.response)
      } esle {
        reject(new Error(this.statusText))
      }
    }
    xhr.send()
  })
}
```

Promise.then() 返回的是 Promise 对象，因此它可以链式调用实现先后依赖的异步操作，而不需要嵌套。e.g. `Promise().then().then()`

## 语法糖 Async 函数（ES7）

Promise 解决了回调地狱的问题，但又造成了新的问题。

（generator 是过渡）

任何函数都可以设置为 async 模式，但它内部必须要与 await 配合使用。*await 设置了 resolve 的部分，而 reject 的错误情况需要通过 try...catch 处理。*

## try...catch

`try {} catch {}`

先尝试运行 try 块，只有当其中出现错误时，错误不会被直接抛出，而是执行 catch 块。

结合 async/await 使用时，就是在 async 函数内用 try 包裹 await 部分。