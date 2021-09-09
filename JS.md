# JS 梳理

任职要求：精通 JavaScript

- 核心：DOM、BOM、AJAX、JSON
- 思想：OOP 面向对象编程
- 库：jQuery、Bootstrap
- 框架：Vue、React

有了JS 的扎实基础才能理解库和框架的源码

## JS 数据类型

边界数据类型条件判断

### 数据类型的概念

![JS_data_types](images\JS_data_types.png)

Object 是**引用**类型，存储在**堆**内存，存储的是地址，多个引用指向同一个地址；

除 Object 以外的七种类型是**基础**类型，存储在**栈**内存，被引用或拷贝时，会创建一个完全相等的变量

### 类型检测方法

- typeof

  undefined、Boolean、String、Number、Symbol 等都能正确检测。

  - `typeof null` 结果是 object。要检测 null 使用 `===null` 即可。
  - 对象类型中，可以得到 function，其他则均为模糊结果 object，无法准确判断。

- instanceof

  准确判断复杂引用数据类型，即 Object

  - 不能判断基础数据类型

- Object.prototype.toString.call

  得到一个字符串结果，形如 `'[object Xxxx]'`，Xxxx 为数据类型（首字母大写），且区分对象类型。

  对象可以直接调用（加 call 也可），基础类型则须通过 call 调用，如 `Object.prototype.toString.call('1')`

通用的检测方法：

```js
function getType(obj){
  let type  = typeof obj;
  if (type !== "object") {    // 先进行typeof判断，如果是基础数据类型，直接返回
    return type;
  }
  // 对于typeof返回结果是object的，再进行如下的判断，正则返回结果
  return Object.prototype.toString.call(obj).replace(/^\[object (\S+)\]$/, '$1');  // 注意正则中间有个空格
}

/* 代码验证，需要注意大小写，哪些是typeof判断，哪些是toString判断？思考下 */
getType([])     // "Array" typeof []是object，因此toString返回
getType('123')  // "string" typeof 直接返回
getType(window) // "Window" toString返回
getType(null)   // "Null"首字母大写，typeof null是object，需toString来判断
getType(undefined)   // "undefined" typeof 直接返回
getType()            // "undefined" typeof 直接返回
getType(function(){}) // "function" typeof能判断，因此首字母小写
getType(/123/g)      //"RegExp" toString返回
```

### 类型转换方法

#### 强制类型转换

- Number()
- parseInt()
- parseFloat()
- toString()
- String()
- Boolean()

#### 隐式类型转换

- 逻辑运算符 (&&、 ||、 !)
- 运算符 (+、-、*、/)
  - 如果其中有一个是字符串，另外一个是 undefined、null 或布尔型，则调用 toString() 方法进行字符串拼接；如果是纯对象、数组、正则等，则默认调用对象的转换方法会存在优先级，然后再进行拼接。
  - 如果其中有一个是数字，另外一个是 undefined、null、布尔型或数字，则会将其转换成数字进行加法运算，对象的情况还是参考上一条规则。
  - 如果其中一个是字符串、一个是数字，则按照字符串规则进行拼接。
- 关系操作符 (>、 <、 <= 、>=)
- 相等运算符 (==)
  - 如果类型相同，无须进行类型转换；
  - 如果其中一个操作值是 null 或者 undefined，那么另一个操作符必须为 null 或者 undefined，才会返回 true，否则都返回 false；
  - 如果其中一个是 Symbol 类型，那么返回 false；
  - 两个操作值如果为 string 和 number 类型，那么就会将字符串转换为 number；
  - 如果一个操作值是 boolean，那么转换成 number；
  - 如果一个操作值为 object 且另一方为 string、number 或者 symbol，就会把 object 转为原始类型再进行判断（调用 object 的 valueOf/toString 方法进行转换）。
- if/while 条件

#### 特别说明：object 的隐式转换：

1. 如果部署了 Symbol.toPrimitive 方法，优先调用再返回；
2. 调用 valueOf()，如果转换为基础类型，则返回；
3. 调用 toString()，如果转换为基础类型，则返回；
4. 如果都没有返回基础类型，会报错。

## 深浅拷贝

### 浅拷贝

#### 什么是浅拷贝

创建一个新的对象，来接受你要重新复制或引用的对象值。  
如果对象属性是基本的数据类型，复制的就是基本类型的值给新对象；  
但如果属性是引用数据类型，复制的就是内存中的地址，如果其中一个对象改变了这个内存中的地址，肯定会影响到另一个对象。

#### 方法：

- `object.assign(target, ...sources)` (ES 6)
  - 可以拷贝 Symbol 类型的属性
  - 不会拷贝源对象的继承属性
  - 不会拷贝源对象的不可枚举属性

- 扩展运算符 `...`

  e.g. `let obj = {a:1, b:{c:1} }; let obj2 = {...obj}`

- `concat ` 拷贝数组

- `arr.slice(begin, end)` 拷贝数组

浅拷贝只能拷贝一层对象，对象内嵌套的对象不是复制，而依然是同一指向关系。

#### 手写函数实现浅拷贝：

```js
const shallowClone = (target) => {
  if (typeof target === 'object' && target !== null) {		// 判断是否是对象
    const cloneTarget = Array.isArray(target) ? [] : {}
    for (let prop in target) {				// for 循环遍历赋值
      if (target.hasOwnProperty(prop)) {
        cloneTarget[prop] = target[prop]		// 复制各项的值，值如果是对象则只是复制了其指向
      }
    }
    return cloneTarget
  } else {
    return target		// 不是对象则直接返回
  }
}
```

### 深拷贝

#### 理解深拷贝

将一个对象从内存中完整地拷贝出来一份给目标对象，并从堆内存中开辟一个全新的空间存放新对象，且新对象的修改并不会改变原对象，二者实现真正的分离。

#### 方法

1. ##### `JSON.parse( JSON.stringify( targetObj ) )` （简易的常用方法）

   对象转 JSON 字符串，再转对象

   不足：

   - 拷贝的对象的值中如果有函数、undefined、symbol 这几种类型，经过 JSON.stringify 序列化之后的字符串中这个键值对会消失；
   - 拷贝 Date 引用类型会变成字符串；
   - 无法拷贝不可枚举的属性；
   - 无法拷贝对象的原型链；
   - 拷贝 RegExp 引用类型会变成空对象；
   - 对象中含有 NaN、Infinity 以及 -Infinity，JSON 序列化的结果会变成 null；
   - 无法拷贝对象的循环引用，即对象成环 (obj[key] = obj)。

2. ##### 递归 （基础）

   ```js
   function deepClone(obj) {
     let cloneObj = {}
     for (let key in obj) {		// 遍历对象
       if (typeof obj[key] === 'object') {		// 判断对象成员是否为对象
         deepClone (obj[key])					// 是对象则再次调用此函数
       } else {
         cloneObj[key] = obj[key]			// 是基本数据类型则直接赋值
       }
     }
     return cloneObj
   }
   ```

   依然未能解决的缺陷：

   - 这个深拷贝函数并不能复制不可枚举的属性以及 Symbol 类型；

   - 这种方法只是针对普通的引用类型的值做递归复制，而对于 Array、Date、RegExp、Error、Function 这样的引用类型并不能正确地拷贝；

   - 对象的属性里面成环，即循环引用没有解决。

  3. ##### 最终改进版 （进阶）

     ```js
     const isComplexDataType = obj => (typeof obj === 'object' || typeof obj === 'function') && (obj !== null)		// 判断是否为对象的函数
     const deepClone = function (obj, hash = new WeakMap()) {
       if (obj.constructor === Data) return new Data(obj);			// 是 Data 就新生成 Data
       if (obj.constructor === RegExp) return new RegExp(obj);	// 是 RegExp 就新生成 RegExp
       if (hash.has(obj)) return hash.get(obj);								// 有循环引用，就使用 WeakMap 解决
       // 以上情况均不符合，进入下面的遍历
       let cloneObj = Object.create(
       	Object.getPrototypeOf(obj),						// 原型
         Object.getOwnPropertyDescriptors(obj)	// 所有自身属性的描述符
       );
       hash.set(obj, cloneObj)
       for (let key of Reflect.ownKey(obj)) {	// 遍历对象的键数组
         cloneObj[key] = (isComplexDataType(obj[key]) && typeof obj[key] !== 'function') ? deepClone(obj[key], hash) : obj[key];
       }
       return cloneObj
     }
     ```

     

## 应聘

1. #### 岗位

   瞄准用人需求有的放矢，突出匹配点。

2. #### 简历

   重中之重：项目经历

   - 真实参与的（前提）
   - 很熟悉的
     - 业务上：项目的业务背景是什么；使用到的技术是如何推动了业务运行
     - 技术实现上：整体的技术实现思路；用到哪些比较不错的技术点，解决了什么比较困难的问题
   - 有亮点的
     - 在这个项目里做了什么有价值的事；
     - 做了什么有技术含量的事；

3. #### 复习

   系统性的复习，而非零散的复习

   1. JavaScript 相关知识

      <img src="D:\warzdrun\OneDrive\前端课程\笔记\images\JS mindmap.png" alt="JS mindmap" style="zoom:200%;" />

   2. CSS 相关知识

      ![CSS mindmap](D:\warzdrun\OneDrive\前端课程\笔记\images\CSS mindmap.png)

   3. 前端框架相关知识

      ![framework mindmap](D:\warzdrun\OneDrive\前端课程\笔记\images\framework mindmap.png)

   4. 工程化相关知识

      ![engineering mindmap](D:\warzdrun\OneDrive\前端课程\笔记\images\engineering mindmap.png)

   5. 性能优化相关知识

      ![performance optimization mindmap](D:\warzdrun\OneDrive\前端课程\笔记\images\performance optimization mindmap.png)

   6. 计算机网络相关知识

      ![network mindmap](D:\warzdrun\OneDrive\前端课程\笔记\images\network mindmap.png)

   7. 前端安全相关知识

      ![secure mindmap](D:\warzdrun\OneDrive\前端课程\笔记\images\secure mindmap.png)

## 前端的算法重点

### 数据的存储方式

- #### 顺序存储

  - 栈 stack

    ```js
    class Stack {
      constructor() {
        this.items = []
      }
      push(element) {
        this.items.push(element)
      }
      pop() {
        return this.items.pop()			// 删除并返回数组的最后一个元素
      }
      get size() {
        return this.items.length
      }
      get isEmpty() {
        return !this.items.length
      }
      clear() {
        this.items = []
      }
      print() {
        console.log(this.items.toString())
      }
    }
    
    // 初始化一个栈
    var s = new Stack()
    s.push(1)
    s.push(2)
    s.push(3)
    s.push(4)
    console.log(s)
    console.log(s.isEmpty)
    console.log(s.size)
    
    ```

  - 队列 queue

    ```js
    class Queue {
      constructor() {
        this.items = []
      }
      enqueue(element) {
        this.items.push(element)
      }
      shift() {
        return this.items.shift()				// 删除数组的第一个元素，并返回第一个元素的值
      }
      get size() {
        return this.items.length
      }
      get isEmpty() {
        return !this.items.length
      }
      clear() {
        this.items = []
      }
      print() {
        console.log(this.items.toString())
      }
    }
    
    // 初始化一个队列
    var s = new Queue()
    s.enqueue(1)
    s.enqueue(2)
    s.enqueue(3)
    s.enqueue(4)
    console.log(s)
    console.log(s.isEmpty)
    console.log(s.size)
    ```

  栈和队列都是顺序存储结构，通过数组的方式模拟实现。二者区别在于，栈是先进后出，队列是先进先出。

- #### 链式存储

  - 链表

    

  - 二叉树
