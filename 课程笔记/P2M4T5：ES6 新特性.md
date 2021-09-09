## ECMAScript 2015 (ES6)
这是一个重要的大更新版本，有时也用 ES6 泛指 ECMAScript 2015 以后的所有版本。

- 解决原有语法上的一些问题或者缺陷
- 对原有语法进行增强
- 全新的对象、全新的方法、全新的功能
- 全新的数据类型和数据结构
## let 与块级作用域#5
新增关键字：let  
1. let 定义的变量在块级作用域内部能够被访问

遍历添加事件的问题也可以解决了（除了利用闭包效果自调用的方法以外）：
```js
for(let i=0; i<eles.length; i++){
    eles[i].onclick = function(){};
}
```
每一次循环中，循环体都是一个独立的块级作用域，事件添加完毕后，调用时也会回到各个独立的闭包内，因此不会出现问题。
>循环实际上有两层作用域，

2. let 不会进行声明提升
3. 同一作用域，使用 let 声明的变量不能再次被声明
4. 不能使用 let 在函数内部重新声明参数
## const#6
常量，在let的基础上增加只读要求

1. 在声明的同时，必须赋初值。
2. const 为对象时，对象内容是可以修改的，但是不能换对象。

【注意】实践中，尽量不用 var，无需修改就用 const，需要变动就用 let。

## 解构#7
### 数组的解构#7
无需使用数组下标，对应读取数组各项的内容。适用于批量的便捷操作。

e.g. 
有一个数组：`const arr = [1,2,3];`  
- 直接以数组对变量数组赋值 `const [a,b,c] = arr;`  
    那么 a=1,b=2,c=3
- 可以只取需要的变量 `const [, ,c] = arr;`  
    那么 c=3
- `const [a,...rest] = arr;`  
    那么 a=1, rest=[2,3]
- 变量可以设默认值 `const [a,b,c=0,d=0] = arr;`  
    如果有新赋值，即接收，如果没有，则不变。  
    那么 a=1, b=2, c=3, d=0  
    如果没有默认值，也没有新赋值，那么值为 undefined  

【常用】字符串 split 分割为数组，随即解构赋值。e.g. `const [,,a] = 'foo/bar/baz'.split('/');`

### 对象解构#8
数组的解构是依据顺序，对象的解构则需要依据属性名。  

e.g. 有一个对象： `const obj = {name: 'Tom', age: 18};`  
- `const {name} = obj;`  
    那么，得到 name='Tom'。name 是对象里的属性名，默认作为变量名。
- `const {name: newName = 'Jack'} = obj;`  
    那么，得到 newName='Tom'。newName 是自定义的变量名，后面跟的是它的默认值。

【常用】`const {log} = console;` 相当于 `const log = console.log;`，以后可以直接调用 log。
    
## 模板字符串
### 模板字符串字面量#9
反引号 \`\` 

1. 可以包含换行缩进的格式内容
2. 可以加入插值表达式作为字符串内容  
    e.g. var str = 

### 模板字符串标签函数#10

## 新扩展的字符串方法#11
- includes() 是否包含
- startsWith() 是否以它开始
- endsWith() 是否以它结束
以上三种方法均返回布尔值表示是否积极

## 参数默认值#12
函数的形参可以直接赋默认值，如果传入实参则接受，如果没有传入实参则以默认值执行。  
形如： `function (a='1'){}`
【注意】当有多个形参时，有默认值的形参殿后。

## 剩余操作符#13
剩余操作符 `...` 在【#7】的例子中已有使用，它后面跟一个变量，表示该变量以数组形式存储所有从此开始的参数。  
e.g. 
```js
function fun(...args){
    console.log(args);
}
fun(1,2,3,4);
```
此时调用 fun() 并传入四个实参，会得到由四个实参组成的数组。而如果函数内使用的是函数的自有属性 arguments，得到的是 Arguments 类数组对象。

## 展开操作符#14
`console.log(...arr);`

## 箭头函数#15
相当于匿名函数，并简化函数定义：  
参数 => 返回值
`(a,b) => a+b;`  
1. 多个参数或无参数，需要外加 ()
2. 多条语句时，不能省略 {}
3. 返回对象字面量时，需要外加 ()

e.g. 简写排序函数
`[10,2,5,1].sort( (x,y) => x-y );`
e.g. 简写过滤函数

### 箭头函数的 this#16
箭头函数内部没有 this，在其内部调用 this 总会到外层查找。因此完全修复了 this 的指向，this 总是指向词法作用域，也就是外层调用者 obj

## 对象字面量增强#17

- 属性名和值相同时，可以只写一个  
    `const obj = {name:'Tom',bar,age:12};`
- 方法简化：省略 `:function`  
    `const obj = {sayHi () {},name:'Jack'}`
- 计算属性名：直接在字面量内以变量作为属性名，变量以 [] 包裹  
    `const str = 'age'; const obj = {[str]:18};`

### Object.assign#18
将一个或多个源对象中的属性合并到一个目标对象中  
`Object.assign(target,source1,source2,...);`  
没有的属性 加入，已有的属性 后来的覆盖前面的。

e.g.1 合并进空对象实现复制  
    `const newObj = Object.assign({},obj);`
    
e.g.2 构造函数的 option 参数
```js
function Block(option) {
    Object.assign(this,option);
}
conts block = new Block({name:'Tom',age:12});
```
直接以对象作为实参传入，实现对新建实例的属性赋值。

### Object.is 方法#19
判断两个实参是否相同  
`Object.is(+0,-0);` false  
`Object.is(NaN,NaN);` true

而使用全等于判断时，结果不同  
`+0 === -0;` true  
`NaN === NaN;` false

## class类#20
使用 class 关键字来声明一个类型 
```js
 class Person {
     constructor (a,b){
         this.name = a;
         this.age = b;
     }
 }
```

### 添加静态方法 static
使用 static 关键字在构造函数或类中添加静态方法
```js
class Person{
    static create() {}
}
```
【注意】静态方法只能由构造函数调用，其 this 指向类自身，而非实例对象

### 类的继承#22
使用 extends 关键字继承类  
```js
class Student extends Person {
    constructor (a,b,c) {
        super(a,b);
        this.number = c;
    }
}
```
通过 super() 调用父类构造函数

## Set 数据结构#23
集合，成员不能重复
### set 常用属性和方法
- forEach()  
    参数是一个函数，通常用于遍历  
    另外，常用 for of 循环遍历 set
- size  
    存储 set 的长度
- has()  
    判断是否包含某成员，返回布尔值
- delete()
    删除某一成员，返回布尔值，表示是否删除成功
- clear()
    清空成员  
【常用】使用 set 来进行数组去重：`const newSet = new Set(arr);` 此时 newSet 是 set 类型，有以下两种方法再将其转回数组：
1、`const newArr = Array.from(newSet);` 调用 Array.from() ；2、`const newArr = [...newSet];` 先展开再放入空数组。

## Map 数据结构#24
可以以任意类型的数据作为键，而对象中只能以字符串或 Symbol 作为键（属性名）。解决了对象中以对象作键名的键值对不能一一对应的问题。
### Map 常用方法
- set(k,v)  
    设置键值。k：键；v：值
- get(k)  
    传入k，返回v
- has()
- delete()
- clear()
- forEach()

## Symbol 数据类型
每一个 Symbol 都是唯一的
Symbol() 括号内是描述文本，而非它的值

ES6 开始，Symbol 也可以作为对象的属性名，并且这些属性是对象的私有成员，在对象外不能访问到，只有通过对象的 gerOwnPropertySymbols() 方法才能获取。
### 方法
- for()  
- toStringTag()

## 遍历方法 for of#27
```js
for (const items of arr) {
    console.log(arr[items]);
}
```
可以用来遍历数组、类数组结构、Set、Map 等，但不建议用于对象的遍历。

## ES6 的其他新特性
- 可迭代接口
- 迭代器模式
- 生成器
- Proxy 代理对象
- Reflect 统一的对象操作 API
- Promise 异步解决方案
- ES Modules 语言层面的模块化标准

## ECAMScript 2016 的新特性前瞻
- includes() 方法可以替代了数组的 indexOf() 方法，用于判断是否包含某项成员。
- 指数运算符 `**`，可以替代 Math.pow()  
    e.g. `2 ** 3` 等价于 `Math.pow(2,3)`，表示2的3次方。