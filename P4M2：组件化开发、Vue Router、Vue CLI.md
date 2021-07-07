## T1：Vue.js 组件

### 组件注册

#### 全局注册

全局注册的组件在注册后可以用于任意实例或组件中，需要在根 Vue 实例创建前注册。

``Vue.component('exampleName', { /*option*/ });``

#### 注册组件时的配置

组件的本质是可复用的 Vue 实例，其配置对象与 new Vue() 相似。

组件的命名规则有两种：kebab-case、PascalCase，而在 HTML 使用时只支持 kebab-case

##### template

设置组件的结构。字符串形式的 HTML 结构。

template 中只能有一个根元素

##### data

用来存储组件的数据。一个将数据对象作为返回值的函数。

data 以函数形式的原因：保证每个组件实例调用的数据在函数的闭包内部，互不影响。

#### 局部注册

在实例配置对象下的 components 对象

局部注册时，组件名作为 components 对象的键名，使用 kebab-case 命名则必须加引号。

【常用】将组件对象提前声明，在 components 对象内赋值即可。还可使用 ES6 语法，结合 PascalCase 命名法，如 `components: { MyComponentA, MyComponentB }` 。

### 组件通信

#### 父组件向子组件传值

子组件：设置 props 选项，默认数组，其中的各项成员即可在 template 中以插值表达式取用。⚠️ props 不要与 data 存在同名属性。

props 一般使用 camelCase，在 js 中无需引号。父组件绑定时仍使用 kebab-case。

父组件：在组件元素属性内设置对应属性

- 静态数据：`dataName="theStringData"`
- 动态数据：`:dataName="attrOfFatherData"`



##### 单向数据流

父子组件间的 prop 都是单向下行绑定的，父组件传过来的数据变化会影响子组件，子组件接收到的数据修改不会影响父组件，也就无法保存修改。

【特别情况】prop 数据为对象或数组时，接收到的是指针，因此子组件的 props 修改会影响到父组件的数据。

因此，如果需要在子组件中处理接收到的 props 数据，应转存进 data 或参与计算成为 computed，再进行处理。

##### props 类型检测

需要设置检查类型以对接收到的值筛选时，将 props 写为对象，接收的值为键名，对应的类型为值。任意类型则值为 `null` / `undefined`，对应多个类型则值为数组。

当接收到的值类型与期望不一致时，值仍可以成功被接收，但会伴有 warning 提示。

```json
props: {
  parStr: String,
  parNum: Number,
  parArr: Array,
  parObj: Object,
  parAny: null,
  parData: [Array, String, Boolean]
}
```

##### props 验证

对单个 prop 做更详细对验证设置，需将该 prop 成员的值写为对象：

```json
parData: {

}
```

- type：值的类型，跟简写时一样

- required：是否必需，布尔值。为 true 时，为必填项，如果没收到值会报错。

- default：用来给此 prop 设置默认值，在未接收到父组件传值时生效。此项不应与 required 同时设置。

  如果默认值为数组或对象时，必须为工厂函数返回的形式。e.g.

  ```json
  default: function () {
    retrun [1,2,3,4]
  }
  ```

- validator：校验函数，return 值为 false 时会弹 warning

  e.g. 

  ```js
  validator (value) {
    return value.startsWith('abc')
  }
  ```

【注意】验证函数内无法通过 this 获取到实例，这里的 this 指向 window

##### 非 props 的属性值

父组件给子组件设置了属性，但此属性在 props 中不存在，那么它们会自动绑定到子组件的根元素上。

如果子组件根元素已有对应属性，则父组件的传值会覆盖原值。不希望被覆盖，需要在子组件配置项添加 `inheritAttrs: false` 。style 和 class 例外，这两个属性值只会合并。

#### 子组件向父组件传值

子向父传值需要通过自定义事件实现。父组件通过监听子组件的数据变化

$emit() 自定义事件，将它放在子组件数据变化函数内，这样父组件监听 $emit 事件获取到子组件状态，可以对数据做相应的修改。

【注意】$emit 命名时也使用 kebab-case 命名。

自定义事件传值 #15

在自定义事件函数 $emit() 第二个参数传入数据，父组件在事件触发时的执行函数就能获取到该值。

##### 组件与 v-model



#### 非父子关系组件间传值

##### 兄弟组件 #18

通过父组件进行数据中转

##### EventBus 事件总线 #19

EventBus 适用于关系不够明晰的两组件之间

##### 完全无关的组件

#### 其他通信方式

通常用于没有直接关系的组件间。

$root #20

$refs #21

### 组件插槽

组件插槽可以便捷的设置组件内容

#### 单个插槽 #23

&lt;slot> 标签写在模板内，这时，组件标签的内容会被插入到 &lt;slot> 标签的位置。

注意⚠】组件内获取数据与其渲染位置相关，例如组件模板内可以获取到组件内数据，实例中的组件内容只能获取到实例的数据。

&lt;slot> 的内容是默认值（后备内容），在组件没有内容时生效。

#### 具名插槽 #24

组件内有多个位置需要使用插槽，这时需要为每个插槽设置 name 属性，未命名的 slot 是默认插槽，名为 `default`。

在组件内容中的 `&lt;template v-slot:slot-name>` 内写入插槽内容，或简写为 `&lt;template #slot-name>` 。

#### 作用域插槽 #25

用于让插槽可以使用子组件的数据的情况

插槽 prop：将需要被插槽使用的数据通过 v-bind 绑定给 &lt;slot>，如 `<slot :value="tags" :index="id">默认文本</slot>`，这里的 tags 和 id 来自组件的 data。

- 多个插槽

  ```html
  <com-a>
  	<template v-slot:slot-a="dataObj">{{ dataObj.value }}</template>
      <template v-slot:slot-b="dataObj">{{ dataObj.value }}</template>
  </com-a>
  ```

- 单个默认插槽

  ```html
  <com-a v-slot="dataObj">
  	{{ dataObj.value }}
      {{ dataObj.index }}
  </com-a>
  ```
  
  这里省略了 &lt;template> 元素，v-slot 直接用于组件元素；v-slot 也省略了插槽默认名。

【常用】将 dataObj 解构赋值出来：

```html
<template #slot-a="{ value, index }">
	{{ value }} {{ index }}
</template>
```

这里相当于 { value, index } = dataObj

### 内置组件

#### 动态组件 #26

实现多个组件的快速切换，例如选项卡。

给 &lt;component> 元素绑定 is 属性，属性值为对应的组件名：`<component :is="MyComA"></component>`

动态组件的切换时伴随着组件实例的卸载和新建，因此组件数据不会被保留

#### keep-alive 组件 #27~28

针对上述的组件切换后数据丢失的情况，keep-alive 组件就是用于保留组件状态或避免组件重新渲染的。

以 &lt;keep-alive> 组件元素包裹 &lt;component> 即可。

要具体指定哪些组件会被缓存，要为 &lt;keep-alive> 设置 include 属性，属性值为需要缓存的组件名。e.g. `"ComA,ComB,ComC"` / `""['ComA','ComB','ComC']"` / `"/Com[ABC]/"` 。exclude 属性则是设置不会被缓存的组件。

max 属性规定了缓存的最大组件个数

#### 过渡组件

##### transition 组件 #30

用于给元素和组件添加进入/离开过渡动画：

- 条件渲染 (使用 v-if )
- 条件展示 (使用 v-show )
- 动态组件
- 组件根节点

以 &lt;transition> 元素包裹需要过渡显示的元素或组件。过渡效果则结合类名选择，写在 CSS 中。

transition 组件自带了6个 class 来设置过渡的具体效果：

- 入场：v-enter，v-enter-to，v-enter-active
- 退场：v-leave，v-leave-to，v-leave-active

v-enter-to 和 v-leave 不常用，它们以默认状态即可。

多个 transition 时，为各个设置不同的 name，其类名也修改为 *name*- 开头的形式。

添加 appear 属性可以令元素在初始渲染时实现入场过渡效果。

###### 自定义过渡类名 #32

transition 组件的原生类名以外，还可以通过以下 HTML 属性来自定义类名控制过渡效果：

- 设置入场类名的属性：enter-class，enter-to-class，enter-active-class
- 设置退场类名的属性：leave-class，leave-to-class，leave-active-class
- 设置初始渲染类名的属性：appear-class，appear-to-class，appear-active-class

通常结合第三方 CSS 动画库使用，例如 Animate.css



##### transition-group 组件 #33

.v-move 类名用来设置由列表元素变更导致元素位移时的效果。退场元素在退场过程中会脱标，不影响后续元素。

## T2：Vue Router

Vue Router 是 Vue.js 的官方插件，用于快速实现单页应用。

#### 单页应用 #2

SPA (Single Page Application) 网站的主体功能都集中在单个页面中，常见于后台管理系统、移动端、小程序

优点：前后端分离开发，提高了开发效率；业务场景切换时，局部更新结构；用户体验好，更加接近本地应用

缺点：不利于 SEO；初次首屏加载速度较慢；页面复杂度比较高

### 原生的前端路由

URL 与内容间的映射关系。前端路由的实现方式有两种：

#### Hash #4~5

Hash 是 URL 中的

判断不同的 hash 值以请求不同的模块页面

封装

```js
var router = {
    routes: {},		//路由存储位置：保存 url 和内容处理函数的对应关系
    route： function (path, callback) {
        this.routes[path] = callback;
    },
    init: function () {
        var that = this;
        window.onhashchange = function () {
            var hash = location.hash.replace('#', '');
            that.routes[hash] && that.routes[hash](); 
        }
    }
};
```

hash 方式的特点：

兼容性好；地址中包含 #，不够美观；前进后退繁琐。

#### History #6~7

History 是 HTML5 提供的

通过 popstate 事件监听前进后退操作，并检测 state

History 的特点：实现简易，地址美观；完整的前进后退；可存储数据量更大；不能兼容旧浏览器；刷新时会请求该路径，需要后端配合。

### Vue Router 的基础使用

可以使用 CDN、npm 包、js 文件引入

1. View 端（HTML）配置 Vue 路由组件 &lt;router-link> 和 &lt;router-view>
2. 定义路由中使用的各个组件
3. 设置路由规则 routes 匹配地址和对应组件
4. 创建 Vue Router 实例，在 routes 属性下配置路由，值为前面的 routes
5. 在 Vue 实例中的 router 属性下将路由注入 

这时，Vue 实例下会有新成员：$router 和 $route。$router 是 Vue 路由的实例，它可以调用路由的相关方法；$route 是路由规则。

#### 多个视图时的命名

需求：路由导航后，希望在同级展示多个组件（视图），也即一个 URL 对应多个视图

处理：命名视图

1. 添加 &lt;router-view> 并设 name 属性（未设置的默认名为 default）

2. 路由规则对象 routes 的 components 对象，如：

   ```json
   // 注意：component 和 components 是两个项
   components: {
       viewNameA: componentNameA,
       default: componentNameB
   }
   ```

### 动态路由

需求：同一类的多个 URL 对应一个组件，而组件内容因 URL 的某些参数而有所不同

处理：

1. 规则 routes 的 path 中设置**路径参数**，e.g. `path: 'user/:id'` ，id 是路径参数，以 `:` 开头，它存储在 `vm.$route.params` 中
2. 在此类 URL 的组件模板中使用路径参数，以产生不同的组件内容

动态路由中，组件会得到复用，只是组件内容被修改，而没有组件销毁和再生成的过程，因此组件的钩子用不了。

#### 侦听路由参数 #12

需求：响应路由的参数变化

处理：通过组件的 watch 项监听 $route

e.g.

```json
watch: {
    $route (route) {
        
    }
}
```

#### 路由传参 #13~14

需求：动态路由的组件需要在多处使用时，不应在组件中直接调用 `vm.$route.params`，避免耦合

处理：通过路由传参

1. 路由规则中添加 `props: true` 。这样路径参数须由组件的 props 接收，而非 `vm.$route.params`
2. 组件配置 props，例如：`props: [id]`
3. 组件模板直接插值路径参数

##### 多个命名视图的情况

多个命名视图（见【】）时，上述步骤1中的路由规则下 props 项应改写为对象，各视图分别开关 props。如：`props: { default: true, viewNameA: true }`

##### 传静态参数的情况

将路由规则的 props 项中视图对应的值写为对象，包含要传的参数和值，如：`props: {default: true, sidebar: {a:1, b:2}}`

### 嵌套路由 #15

路由项下嵌套子路由

1. 在组件模板内添加 &lt;router-link> 和 &lt;router-view>，形成视图的嵌套。
2. 路由内设置子路由 children 项，值等同与 routes 的结构，根据视图嵌套结构形成路由嵌套。

### 编程式导航

通过函数设置导航

调用路由实例的方法可以实现导航：`vm.$router.push()`

参数可以是 URL 字符串，也可以是对象 `{path: '/user'}`

&lt;router-link> 的 to 属性使用 v-bind 绑定方式时也可以对象为属性值：`<router-link :to="{path: '/user'}"></router-link>`

命名路由

对较长的 URL，为路由规则对象添加 name 属性，可以简便配置导航，省去写 URL，路径参数也更清晰。

e.g.

`routes = [{ path: '/user/:id/info/school', name: 'school', component: School }]`

`<router-link :to="{ name: 'school', params: {id: 1} }"></router-link>`

别名 #19

alias 属性，值为一个 URL 字符串，通常以关键路径变量组成

这样，可以在地址栏或 &lt;router-link> 中以别名地址访问

与通过 name 访问的区别：

name 访问时地址栏仍为原 URL，alias 访问时地址栏即为别名路径。

重定向 #18

对常见的错误路径重定向至已有页面

e.g. `redirect: '/'` 重定向至首页

导航守卫 #20

对每次路由监控并作一些自定义处理操作



```JS
router.beforeEach(function (to, from, next) {
// to：目的地址；from：来源地址；next：下一步操作
    // next() 无实参，即放行
    // next(false) 阻止此路由
    // next('/user') 实参为 URL，阻止原路由，导航至新地址
    // next 应有且只有一次，可以结合条件做判断
});
```

### VueRouter 的 History 模式 #21

上述路由都是基于 VueRouter 的默认模式：Hash 模式

如果需要使用 History 模式，在 router 实例中配置 `mode: 'history'`

## T3：Vue CLI

Vue CLI 是 Vue 官方推出的脚手架工具，进行快速开发。

统一项目风格；初始化配置项目依赖；提供单文件组件。

##### 安装

`npm i -g @vue/cli`

#### 项目搭建

1. `vue create ProjectName`
2. 选择配置 Presets
3. 选择包管理器

#### 目录与文件

- public：预览文件目录
- src：资源文件目录
  - assets：静态资源目录
  - components：项目组件目录
  - App.vue：根组件文件
  - main.js：入口文件

##### .vue 单文件组件

单文件组件集合了结构 &lt;template>、逻辑&lt;script>、样式 &lt;style>。它们的使用方式与模块一致。

```vue
<template>
<div>组件框架</div>
</template>

<script>
export default {
  name: 'MyComA'
}
</script>

<style ></style>
```





#### 打包与部署

vue 文件 → 传统三件套

`npm run build`

编译打包后，根目录新增 dist 目录

部署到服务器上

可以使用 serve 模块部署静态文件，`serve dist`

## LIVE：

开发人员需要具备的能力：

- 编程能力（硬实力） 
- 沟通能力（软实力）

技术类问题的交流方式，想办法提高沟通效率：

- 附上全面的信息，关键代码显示出来。描述清晰，资源齐全。
- 问题要准确、聚焦
- 过于简单的问题不要问

知识点类问题：

- 先提供自己的思路，引导对方思考
- 场景复现
- 善用文档

遇到问题：

1. 先看控制台报错
2. x
3. x
4. 截取错误信息
5. 上代码
6. 提出个人思考（升华）

### Web Components

W3C 组件规范

框架服务于大型项目，小型项目使用插件即可。

技术是为业务服务的，技术满足需求即可。

#### 自定义元素

在 js 代码中自定义元素，并引入 HTML

1. 声明一个自定义元素的类，可以从 HTMLElement 或具体元素的类上继承。
2. 使用 window.customElements.define() 

#### shadow DOM

将 shadow tree 挂载到主 DOM 树上，有利于各 HTML 部分的封闭性。

在 shadow tree 中，同样可以调用 document 的方法。

#### slot 插槽

