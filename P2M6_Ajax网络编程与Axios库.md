## P2M6T1：Ajax 基础

Ajax 技术体系由四种部分组成：JavaScript、CSS、DOM、XMLHttpRequest 对象

### 原生 Ajax

发送 Ajax 请求步骤：

1. 创建 XMLHttpRequest 类型的对象

   XHR 对象

2. 准备发送，打开与一个网址之间的连接

   open()

3. 执行发送动作

   send()

4. 指定 xhr 状态变化事件处理函数

#### XHR 对象 #4

Ajax API 提供了一个 XMLHttpRequest 对象类型 `let xhr = new XMLHttpRequest();`

IE 6 中没有 XMLHttpRequest 类型，需要使用兼容写法：

```js
var xhr;
if (window.XMLHttpRequest) {
    xhr = new XMLHttpRequest();
} else {
    xhr = new ActiveXObject('Microsoft.XMLHTTP');
}
```

#### open() 方法 #5

open() 是 XMLHttpRequest 的方法，它的实例对象可以调用。 `xhr.open(method,url);`

参数：

- method：要使用的 HTTP 方法，字符串。
  - GET：
- url：URL 地址，字符串。
  - method 是 'GET' 时，可以在 URL 地址末尾后缀 `?K=V`，筛选获取需要的内容。
- ：是否异步加载，布尔值。默认值：true

#### send() 方法和请求头 #6

setRequestHeader() 用于设置请求头

【注意】GET 时，不需要设置；POST 时，必须设置请求头。在 open() 后、send() 前调用。

`xhr.setRequestHeader(header, value);`

参数：

- header：，字符串
  - Content-Type：【常用】表明传输的数据类型

- value：，字符串

  - 在 header 为 'Content-Type' 时，

    【常用】'application/x-www-form-urlencoded' 和 'application/json'



send() 用于发送 HTTP 请求

`xhr.send(body);`

参数：

- body：在XHR请求中要发送的数据体，根据请求头中的类型进行传参。字符串。

  【注意】GET 时，不需要设置数据体（请求头），send() 传 null 或者不传参。

#### 响应状态分析 #7

readyState 属性返回一个值代表 XMLHttpRequest 代理当前所处的状态

值：

- 0：UNSENT

  代理 XHR 被创建，但尚未调用 open()

- 1：OPENED

  open() 方法已经被调用，建立了连接

- 2：HEADER_RECEIVED

  send() 方法已经被调用，并且已经可以获取状态行和响应头

- 3：LOADING

  响应体下载中， responseText 属性可能已经包含部分数据

- 4：DONE【常用】

  响应体下载完成，可以直接使用 responseText



XMLHttpRequest 对象有 onreadystatechange 事件，每当 readyState 改变时，就会触发。



status 属性返回一个值代表

- 200：OK，就绪
- 304：
- 404：NOT FOUND，未找到页面
- 500：服务器错误

#### 同步和异步

open() 的第三个参数决定同步或异步执行，见【[#6]()】

使用同步加载时，程序会卡死在 send() 部分，因为后续事件尚未声明。

【常用】为了让这个事件可以更加可靠（一定触发），在发送请求 send() 之前，一定是先注册 readyStateChange

### 服务端的数据格式

不同的数据格式的处理方式不同，根据不同的数据类型设置请求头的 Content-Type

#### XML 数据格式

早期格式，已淘汰

- 大量元数据冗余

#### JSON 数据格式 #10

JavaScript 对象表示法，当前的流行格式

类似于 JavaScript 对象字面量，但有区别：

- 属性名必须加引号

JavaScript 中有 JSON 对象类型，它有相关的（静态）方法处理对象类型的数据。

- JSON.parse(str)

  将传入的字符串转为对象，并返回

- JSON.stringify(obj)

  将传入的对象转为字符串，并返回

### 原生 Ajax 应用

#### JSON Server #11

使用 JSON Server 搭建一个本地的临时服务器

文件所在文件夹下，`json-server --watch file.json`

#### JSON 文件的书写方法 #12

#### GET 方法获取数据 #13

GET 请求可以从服务器获取数据

#### POST 方法提交数据 #14

POST 请求可以为服务器添加数据

send() 内传入要添加的数据，实参是字符串类型，但数据的形式因 setRequestHeader() 参数中的 Contetn-Type 而异。

- Content-Type 值为 application/x-www-form-urlencoded 时，

  send() 传数据形如 `k1=v1&k2=v2`

- Content-Type 值为 application/json 时，以 json 格式写法，形如：

  ```json
  xhr.send(`{
      "k1":v1,
      "k2":v2
  }`);
  ```

  通常使用 `` 包裹，保证 json 内的换行正常显示

  或者，将 json 对象转为字符串传入，如：

  ```js
  xhr.send(JSON.stringify({
      "k1":v1,
      "k2":v2
  }));
  ```

### 处理响应数据渲染 #15

在通过 GET 获取数据后，请求变量下的 responseText 属性便有了值，它是字符串类型的。接下来我们需要把获取到的数据渲染到页面中去。

1. 把 responseText 转为对象

   `let data = JSON.parse(xhr.responseText);`

   得到的 data 是一个数组，数组的每一项是一个对象

2. 定义一个空字符串

3. 遍历数组 data

   每次循环中将一个对象的值拼接 HTML 结构，并以模板字符串的形式加进空字符串，或使用模板引擎

4. 将字符串加进目标父元素的 innerHTML，即渲染上页面

### 封装 Ajax 库 

【注意】工作中使用第三方提供的 Ajax 库即可，这里仅供了解

#### 提取参数 #16

观察 Ajax 的整个过程中需要用到的参数，然后分配它们。

参数1：{string} method 请求方法

参数2：{string} url 请求地址

参数3：{object} params 请求参数（对象内细分各项数据对应不同情境）

参数4：{function} done 请求完成后执行的回调函数（自定义后续的数据操作）

#### 统筹逻辑

## P2M6T2：Ajax 常用库

### jQuery 中的 Ajax

#### $.ajax() #1

参数：一个包含所有需要参数的**对象** option，对象中可包含以下属性：

- url

- type：请求方法，默认GET

- dataType：服务端响应数据类型

  - json
  - encoded
  - jsonp 见【[T3#3]()】

- contentType：请求体内容类型，默认值 application/x-www-form-urlencoded

- data：需要传递到服务器的数据

  智能匹配请求方法，GET 则通过 url 传递，POST 则通过请求体传递

- timeout：请求超时事件

- beforeSend：请求发起前触发的回调函数
- success：请求成功（status：200）后触发的回调【常用】
- error：请求失败后触发的回调
- complete：请求完成后（不论请求是否成功）触发的回调



【注意】调用回调函数时获取到的 data 是对象数组类型。

#### $.get() #2

对 ajax() 常用的 GET 请求的快捷化

参数：

- url*
- data
- success
- dataType

#### $.post() #3

参数：

- url*
- data
- success
- dataType

#### 其他类型请求 #4

通过 ajax() 实现

- PUT：更改某一条的具体属性

  URL 地址结尾要加 id 的属性值，以选中该条

- DELETE：删除某一条

#### jQuery 中的其他 Ajax 方法 #5

- ajaxSetup() 设置 ajax() 的参数默认值，方便近似需求的多次调用，简化代码
  - 参数：

### Axios

Axios 是一个使用广泛的 Ajax 第三方包，支持 node 和浏览器端，支持 Promise

CDN 引入：`<script src="https://unpkg.com/axios/dist/axios.min.js"></script>`

#### Axios API #7

Axios 引入了一个全局方法：axios()

参数：一个对象，存放所有的配置项；或两个参数，url 和配置项对象

- url*：用于请求的服务器 URL【常用】
- method：创建请求时使用的方法【常用】
- baseURL：传递相对 URL 前缀，将自动加在 url 前面
- headers：即将被发送的自定义请求头，对象。键值对形式配置 Content-Type
- params：即将与请求一起发送的 URL 参数，对象【常用】
- data：作为请求主体被发送的数据【常用】
- timeout：指定请求超时的毫秒数(0 表示无超时时间)
- responseType：表示服务器响应的数据类型，默认 'json'

#### Axios 的全局配置默认值 #8

修改 axios.defaults 下的各项属性

e.g.1 设置 baseURL 的默认值：`axios.defaults.baseURL = 'http://api.example.com';`

e.g.2 设置 headers 请求头的默认配置：`axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';`

#### Axios 拦截器 #10

在请求前拦截：axios.interceptors.request

在响应被 then/catch 处理前拦截：axios.interceptors.response

拦截到一般是要执行一些操作，需要继续调用 .use(function () {})

e.g.1 在请求前拦截到，并修改 config

```js
axios.interceptors.request.use(function (config) {
	config.params = {
        'id':1
    };
    return config;
})
```

e.g.2 在响应前拦截到，并修改响应内容，只输出响应的数据

```js
axios.interceptors.response.use(function (response) {
    return response.data;
})
```

拦截器针对所有 axios 请求都有效，相当于修改全局默认配置

#### 快速请求方法 #11

- axios.get(url*, config)

- axios.post(url*, data)

- axios.delete(url*, config])

- axios.put(url*, data)

### XMLHttpRequest 2.0 #12~13

HTML 5 中对 XMLHttpRequest 进行了升级

#### 新增事件 #12

- xhr.onprogress：请求进行中（rs3）触发。

  事件对象 e

- xhr.onload：请求完成时（rs4）触发。无需再在 onreadystatechange 事件内加条件判断请求完成。

#### 新增属性 #13

- response：xhr 的响应，其类型取决于 responseType 的值。
- responseType：
  - text：文本【默认】。此时，response 相当于 responseText
  - json：response 返回一个 JavaScript 数组对象，免去了对 responseText 转对象的操作
  - document
  - blob
  - arraybuffer

## P2M6T3：跨域和模板引擎应用

#### 同源策略 #1

在同源策略下，只有同源的地址才可以相互通过 AJAX 的方式请求。同源说明的是两个地址的关系，它们的域名、协议、端口完全相同。

不同源地址之间的请求称之为跨域请求，这在现代化的 Web 应用中非常常见。

跨域解决方案

### 跨域

#### JSONP #2

全称：JSON with Padding

JSONP 只能发送 GET 请求

JSONP 用的是 script 标签，与 AJAX 提供的 XMLHttpRequest 没有任何关系

##### jQuery 中使用 JSONP #3

jQuery 中使用 JSONP 是封装进 $.ajax() 方法的，将 dataType 配置为 jsonp 即可。

参数中的 jsonp 属性可以配置回调函数的参数名称，它需要与后台接口的回调函数参数名保持一致。默认值 'callback'

jsonpCallback 属性配置 jQuery 回调函数名，默认分配的是 'callback'

#### CORS

全称：Cross Origin Resource Share

在服务端配置

允许的地址可以直接在前端访问，比如使用 Ajax

#### 【案例】JSONP 请求百度搜索关键词预测 #5~6

### 模板引擎

模板引擎方便获取到数据后渲染进页面。各大公司有自研引擎。下面以腾讯的 artTemplate 为例。

#### 使用模板步骤

- 引入模板文件
- 创建模板
- 将数据跟模板进行绑定
- 在模板里面编写代码解析数据
- 绑定数据和模板之后得到内容
- 将数据内容写到页面上面

#### template() 方法

参数：

- id：需要调用的模板 script 元素的 id，字符串。
- 传入变量：一个对象，对象内的属性可在模板元素内用 `<%= %>` 包裹引用

#### 【案例】百度搜索预测套用模板 #9

### 【综合案例】留言板 #10~12