---
theme: juejin
---
# 下属于Part2 Module3：Web APIs 网页应用编程

## Step1：DOM 概念


## Step1：DOM 获取页面元素
### 根据 id 获取元素#3

方法：document.getElementById()

参数：字符串类型的 id 的属性值

返回值：对应 id 名的元素对象

`var para = `

> 补充：在控制台打印具体的对象 `console.dir();`

#### 注意：

- 注意代码的执行顺序，js 放在 HTML 后面。如果 js 在 html 结构之前，会导致结构未加载，不能获取对应 id 的元素。
- js 中的 id 唯一性。如果有多个同 id 元素，js 只会选中第一个找到的元素。
- 部分浏览器支持直接使用 id 名访问元素，但不是标准方式，不推荐使用

### 根据 元素名 获取元素#4

方法：document.getElement**s**ByTagName()

参数：字符串类型的标签名

返回值：同名的元素对象组成的（伪）数组，HTMLcollection

#### 注意：

- 操作方法与操作数组的方法一致

- 此方法内部获取的元素是动态增加的。可以放在 HTML 结构前。

- 获取元素的顺序是依照 HTML 中元素的开始标签的先后顺序。

### 元素对象内部获取元素#5

连续调用方法，进一步选中元素对象。

类似于 CSS 的后代选择器：

```css
#box p {
}
```
选中 id 为 box 元素下的 p 元素。

在js中：

```js
var bs = document.getElementById('box').getElementsByTagName('p');
```

通常来说，将连续调用拆分开写，分别定义变量赋值，更有利于后续使用：

```js
var box = document.getElementById('box');
var ps = box.getElementsByTagName('p');
```

### 根据 name 属性获取元素#6

方法：document.getElementsByName()

参数：字符串类型的 name 属性值。

返回值：name 属性值相同的元素对象组成的数组

兼容性：IE 9 和 Opera 下有兼容性问题

#### 注意：
- 动态增加。和 getElementsByTagName() 相同

- 获取到的是 NodeList，而非 HTMLcollection

### 根据 类名 获取元素#7

方法：document.getElementsByClassName()

参数：字符串类型的 class 属性值

返回值：class 属性值相同的元素对象组成的数组，HTMLcollection

兼容性：不支持 IE 8 及以下

#### 注意：
- 动态增加

### 根据 选择器 获取元素#8
方法：document.querySelector()

参数：字符串类型的 css 中的选择器

兼容：不支持 IE 8 及以下  

注意：
- 需要在 HTML 后面使用
- 只能获取第一个符合选中条件的元素。另有 document.querySelector**All**() 可以获取到**所有**符合条件的元素，返回 NodeList

## Step2：DOM 事件基本应用

### 事件#9

执行机制：触发 → 响应

绑定事件（注册事件）三要素：  
    事件源（绑定对象）  
    事件类型（如何触发）  
    事件函数（如何响应）

事件监听

绑定方法：
- HTML 元素属性：

    写在元素的开始标签内，键值对形式，事件类型为属性，事件函数为属性值。例如：`<input onclick="alert('Wow!')">`
    
- DOM 对象属性：
  
    ```js
    var btn = document.getElementById('btn');
    btn.onclick = function (){
        alert('Wow!')
    }
    ```

常用的鼠标事件类型：
- onclick 鼠标左键单击触发
- ondbclick 鼠标左键双击触发
- onmousedown 鼠标按键按下触发
- onmouseup 鼠标按键放开时触发
- onmousemove 鼠标在元素上移动触发
- onmouseover 鼠标移动到元素上触发
- onmouseout 鼠标移出元素边界触发

### DOM 元素属性操作

#### 非表单元素的属性的操作#10

元素对象中已经封装了与元素属性同名的方法，方便直接调用，例如 href、title、id、src 等。

部分属性名由于与 js 关键字和保留字冲突，需要更换写法：

- class → className
- for → htmlFor
- rowspan → rowSpan

更改属性值：
以字符串类型，通过等号赋值

（注意：id 属性只读）

##### 【案例】
- 点击按钮切换图片#11

- 点击按钮显示隐藏元素#12
    - 通常以修改类名的方式。结合 CSS，两个类选择器分别实现隐藏和显示。
    > 补充说明：事件函数内部的 this #13  
    >> 普通函数 → window 对象  
    >> 构造函数 → 生成的实例对象  
    >> 对象的方法 → 对象本身  
    >> 事件函数 → 事件源
- 相册切换

#### innerHTML 和 innerText  

- innerHTML：适用于设置有内部子标签结构  
    读取时，包含内部的元素标签、空白换行；  
    写入时，按照 HTML 语法加载内容，字符实体会转义为符号显示。 
- innerText：适用于设置纯字符串  
    读取时，过滤掉元素标签、换行缩进等，只保留文字内容；  
    写入时，加载为普通字符，所写即所示。
    
#### 表单元素属性的操作#17

表单元素多为单标签元素，无法使用 innerHTML 或 innerText（双标签元素&lt;option&gt;除外），通常 value 可以用于大部分表单元素的内容获取。

type 属性可以获取 &lt;input&gt; 元素的类型。

另外，一些特殊的属性，比如：disabled、checked、selected 等，它们的属性值只有一个且与属性名相同。在 js 中读取或赋值时，使用布尔值，true 表示打开。

##### 【案例】
- 检测用户名和密码案例#18
    - onfocus 事件
- 随机设置下拉菜单选中项#19

- 搜索文本框#20

- 全选反选#21-23

#### 自定义属性的操作#24

支持自定义属性，但是没有特别对应的方法可供调用，而需要使用通用方法调用。以下几种方法适用于任何属性的操作。

- 获取 getAttribute(name)
- 设置 setAttribute(name，value)
- 移除 removeAttribute(name)

作为参数的属性名和属性值均为字符串格式。


#### 【单列】style 属性的操作#25

`element.style` 获取到的值是，由所有**行内样式**组成的样式对象，CSSStyleDeclaration.

`element.style.attribute` 可以进一步调用到该元素行内样式的某一具体 CSS 属性的数据。  
注意：类似 background-color 的包含连字符的单一属性需要改写为驼峰命名法，即 `body.style.backgroundColor`。

##### 【小结】js 中有关样式的操作#26
截至目前，在 js 中需要修改样式主要由两种方法：

- 为元素修改 className 类名，即会被应用对应的一组 CSS 样式（见【[#12](url)】【[#28](url '显示隐藏二维码#28')】）
- 获取元素的行内样式 style 属性值，并直接修改

修改类名适用于批量的、通用的样式，方便。同时需要注意层叠性。  
修改 style 属性值适用于细节变化的修改，精确。

#### 【综合案例】

- 开关灯#27
    - 有可利用的值做判断条件时，可省略一个判断变量

- 显示隐藏二维码#28
    - onmouseover、onmouseout 事件
    - 目标元素有多个类名时，使用字符串的 replace 方法进行替换  
      `string.replace('a','b')` 将 string 中的 a 替换为 b，并返回

- 当前输入的文本框高亮显示#29
    - 排他思想：排除其他；保留自己  
      做法：先排除所有（需要遍历），再单独做自己
- 点击按钮改变 div 的大小和位置#30

- 表格隔行变色、高亮显示#31

- tab 选项卡切换#32-33
    - 对应控制：两组数据中存储同样数量的元素对象，一组变换，引起另一组也变化。
    - 为了实现一一对应，即两个数组同下标位置的元素之间的同步。在为每一个子项元素绑定事件的遍历中，前置添加一个自定义属性存储它在数组中的下标，而后在事件函数中即可调用该自定义函数获取下标。

## Step3：DOM 节点操作

### 节点的属性#35

- nodeType 节点类型  
    只读
    - 值：
        - 1：元素节点
        - 2：属性节点
        - 3：文本节点
        另有其他值，见手册。
- nodeName 节点名称  
    只读
- nodeValue 节点值
    - 值：
        - 元素节点的节点值：null
        - 属性节点的节点值：该 HTML 属性的属性值
        - 文本节点的节点值：其文本
### 节点的层级关系

类似于 HTML 元素间的族谱关系，节点的层级关系主要可分为父子关系和兄弟关系。
#### 父子节点常用属性#36
- childNodes 该节点下所有**子节点**的集合  
    返回值 NodeList；只读；动态
    
- children 该节点下所有**子元素节点**的集合  
    返回值 HTMLCollection；只读；动态
- firstChild 第一个子节点  
    lastChild 最后一个子节点  
    只读；如果没有子节点，返回 null
- firstElementChild 第一个子元素节点  
    lastElementChild 最后一个子元素节点    
- parentNode 当前节点的**父节点**
    如果该节点位于 DOM 树顶端（如 document），或尚未插入一棵树中（如 新生成的节点），那么它返回 null
- parentElement 当前节点的**父元素节点**  
    如果该元素没有父节点，或父节点不是一个 DOM 元素节点，则返回 null。
    
##### 【案例】表格隔行变色重写#37
使用节点的方法获取到元素节点，而【#31】中的则是获取到元素。
二者效果类似，但分属不同体系。

#### 兄弟节点常用属性#38
- nextSibling  
    只读属性，返回与该节点同级的下一个节点，如果没有返回null。  
  previousSibling  
    只读属性，返回与该节点同级的上一个节点，如果没有返回null。
  
- nextElementSibling  
    只读属性，返回与该节点同级的下一个**元素**节点，如果没有返回null。  
  previousElementSibling  
    只读属性，返回与该节点同级的上一个**元素**节点，如果没有返回null。
    - 注意：nextElementSibling 和 previousElementSibling 有兼容性问题，IE9 以后才支持
  
### 节点的操作
#### 创建新节点的方法
- document.createElement("div") 创建元素节点，参数为元素名
- document.createAttribute("id") 创建属性节点，参数为属性名
- document.createTextNode("hello") 创建文本节点，参数为文本
通常来说，要创建变量存储创建出来的节点

#### 节点的添加、替换、插入、删除、克隆#40-42
- parentNode.appendChild(child)  
    将一个节点添加到指定父节点的子节点列表末尾。
    参数 child 可以是新建的节点，也可以是已存在的节点，那么即实现“剪切”效果。

- parentNode.replaceChild(newChild, oldChild)  
    用指定的节点替换当前节点的一个子节点，并返回被替换掉的节点。
- parentNode.insertBefore(newNode, referenceNode)  
    在参考节点**之前**插入一个拥有指定父节点的子节点。
    referenceNode 必须设置，如果为 null 则 newNode 将被插入到子节点的末尾，相当于 appendChild()
- parentNode.removeChild(child)  
    移除当前节点的一个子节点。  
    这个子节点必须存在于当前节点中
- Node.cloneNode()  
    克隆一个节点  
    - 参数：Boolean 布尔值  
         - true：深度克隆,该节点的所有后代节点都会被克隆
         - false：浅度克隆，只克隆该节点本身
         不建议省略参数
    - 注意：标签上的属性和属性值都会被复制，其中包括作为元素写在标签行内的绑定事件，而通过 JavaScript 动态绑定的事件不会被复制
#### 节点的判断方法#43
- Node.hasChildNodes()  
    没有参数，返回一个 Boolean 布尔值，来表示该元素是否包含有子节点（不区分节点类型）
    
- Node.contains(child)  
    返回一个 Boolean 布尔值，来表示传入的节点是否为该节点的后代节点（包含关系，而不局限在父子关系）
    

判断一个节点有无子节点的方法：
- `node.firstChild !== null`
- `node.childNodes.length > 0`
- `node.hasChildNodes()`
以上三个表达式的值均为布尔，true 表示有，false 表示无。

#### 【案例】
- 动态创建列表#44
- 动态创建表格#45-46
- 选择水果#47-48

## Step4：DOM 事件详解

使用键值形式的事件绑定不能多次绑定。

### 注册/绑定事件的其他方法

#### element.addEventListener()#49
- 参数：
    - 第一个参数：事件类型的字符串（不加'on'，只写'click'）
    - 第二个参数：事件函数
- 说明：此方法可以绑定多个事件，响应时会按代码先后顺序执行。
- 兼容性：不支持 IE 8 及以下

#### element.attachEvent()#50
- 参数：
    - 第一个参数：事件类型的字符串（一般写法的事件类型）
    - 第二个参数：事件函数
- 兼容性：IE 11、Chrome 不支持，仅支持 IE 10 及以下，且 IE 8 及以下处理事件列队时会出现顺序错乱

#### 绑定兼容写法#51

自行封装一个函数，内部做浏览器能力判断


```js
function addEvent(ele,type,fn){
    //有监听器能力的 IE 9 及以上使用
    if (ele.addEventListener) {
        ele.addEventListener(type,fn);
    } 
    //只有附加事件能力的 IE 8 及以下使用
    else if (ele.attachEvent) {
        ele.attachEvent('on'+type,fn);
    }
}
```
函数的三个参数分别是事件源、事件类型和事件函数


### 事件移除的方法

DOM 0级：

给事件赋 null 值

DOM 2级：

element.removeEventListener()

element.detachEvent()

分别对应前述的两种绑定方法

不能移除匿名函数，所以需要解绑的事件，在绑定时必须是被定义了的函数。

#### 解绑兼容写法#53

```js
function removeEvent() {
    if (ele.addEventListener) {
        ele.addEventListener(type,fn);
    } else if (ele.attachEvent) {
        ele.attachEvent('on'+type,fn);
    }
}
```
> 【tips】诸如上述的两个兼容浏览器的绑定/解绑函数，这些常用的自定义函数可以写入一个js文件作为公共库引入

### DOM 事件流#54

事件冒泡过程默认顺序为：从内层到外层，底层向上冒泡
事件捕获过程：从外层向内层，顶层先捕获向下传导

addEventListener() 有第三个参数，布尔值，决定事件流的方向。 true 事件捕获过程；false 事件冒泡过程（默认值）

先执行捕获，后执行冒泡

#### DOM 事件流的三个阶段#55

- 第一个阶段：事件捕获
- 第二个阶段：事件执行过程
- 第三个阶段：事件冒泡

onclick 事件类型，只能进行事件冒泡过程，没有捕获阶段  
attachEvent() 方法：只能进行事件冒泡过程，没有捕获阶段

#### 事件冒泡的应用：事件委托#56
将子级的事件委托给父级加载，子级触发事件时会向上冒泡到父级，而父级绑定的事件中可以找到那个触发事件的子级。  
适用于多个子级元素的公共类型事件。

```js
father.onclick = function (e) {
    e.target.atrribute = 'attr';
}
```
> 这里利用了事件函数的内置参数e，它存储着事件的真正事件源。  
> 此例中，e 代表了事件对象；target 是事件的触发元素。  
> 这样的事件委托有效节省代码，无需遍历多个同级元素添加类似事件。

### 事件对象#57-58

展开讲一讲上例中提到的事件对象：

- e.eventPhase 查看事件触发时所处的阶段
- e.target 用于获取触发事件的元素  
    e.srcElement 用于获取触发事件的元素，低版本浏览器使用  
    兼容写法：`var target = e.target || e.srcElement;`

- e.currentTarget 用于获取绑定事件的事件源元素  
    事件函数中，this 指向等同为 e.currentTarget
- e.type 获取事件类型  
    值：不带on  
    使用场景：需要对一个元素对象添加不同的事件类型时，以一个共同的函数作为事件函数，函数内以 e.type 作判断条件。避免了分别定义多个函数，以节省内存优化性能。
    
    ```js
    function fn(e) {
        switch (e.type) {
            case 'mouseover':
                break;
            case 'mouseout':
                break;
        }
    }
    ```
- e.clientX/e.clientY 事件触发时，鼠标距离浏览器窗口左上角的距离（即鼠标在浏览器窗口中的坐标）
- e.pageX/e.pageY 事件触发时，鼠标距离整个HTML页面左上顶点的距离  
    兼容性：IE8 及以下不支持

注意：e 在低版本浏览器中有兼容问题，低版本浏览器等效使用的是 window.event  
因此，兼容写法为 `e = e || window.event`

#### 【案例】图片跟随鼠标移动#59
### 取消默认行为和阻止冒泡#60
【小结】阻止默认行为:
- 在事件函数结尾 `return false;`
- `e.preventDefault();` 取消默认行为
- `e.returnValue = false;` 取消默认行为，低版本浏览器使用

阻止冒泡:

- `e.stopPropagation();` 阻止冒泡，标准方式
- `e.cancelBubble = true;` 阻止冒泡，IE 低版本，标准中已废弃

## Step5：DOM 特效

### offset 系列属性#61
- offsetParent  
    偏移参考父级，距离自己最近的有定位的父级，如果都没有定位参考body(html)，类似于 CSS 中绝对定位的参考元素
- offsetLeft、offsetTop
- offsetWidth、offsetHeight  
    边框及以内（border、padding、content）的宽高，类似于 CSS 中 border-box 模式下的width和height
### client 系列属性#62
- clientLeft、clientTop  
    左边框和上边框的尺寸，不常用
- clientWidth、clientHeight  
    边框以内（padding、content）的宽高
    

通常，在有边框时选用 offsetWidth & offsetHeight，没有边框时选用 clientWidth & clientHeight

### scroll 系列属性

- scrollLeft、scrollTop
- scrollWidth、scrollHeight

### 【案例】
- 拖拽#64
- 弹出层#65