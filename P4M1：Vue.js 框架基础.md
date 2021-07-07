# Vue.js 框架基础

## T1：初识 Vue.js

### 选择 Vue.js

传统开发的弊端：

- DOM 操作频繁
- DOM 操作与逻辑代码混合，可维护性差
- 不同功能区域的代码写在一起，可维护性差
- 模块间的依赖关系复杂

#### Vue.js 核心特性

##### 数据驱动视图

- 单项数据绑定：数据变化会自动更新到对应元素中，无需手动操作 DOM。
- 双向数据绑定：仅适用于可输入元素。数据自动渲染到页面，元素内容变化也能更新到数据。

优点：基于 MVVM 模型实现的数据驱动视图解放了DOM操作；View 与 Model 处理分离，降低代码耦合

缺点：双向绑定时的 Bug 调试难度增大；大型项目的 View 与 Model 过多，维护成本高

##### 组件化开发

- 允许将网页功能封装为自定义 HTML 标签，复用时书写自定义标签名即可，提高了代码可复用性。

- 组件不仅可以封装结构，还可以封装样式与逻辑代码，大大提高了开发效率与可维护性。

### Vue.js 的初步使用

#### 安装 Vue #3

- 本地引入
- CDN 引入
- npm 安装

以下基于 vue@2.6.12

#### Vue 实例

Vue 实例是通过 Vue() 创建的对象，是使用 Vue 功能的基础。Vue() 的参数是配置对象，下面将详细介绍。

#### el 选项 #5

用于选取**一个** DOM 元素（不能是 html 或 body）作为 Vue 实例的挂载目标。可以是 DOM 元素变量，也可以是 CSS 选择器字符串。

只有挂载元素内部才会被 Vue 进行处理，外部为普通 HTML 元素。它代表 MVVM 中的 View 层。

挂载后调用：`vuemodel.$el`

定义实例后再挂载：`vuemodel.$mount()`

#### 插值表达式 {{}} #6

插值表达式只能写在元素内容，且内部只能写 JavaScript 表达式（可调用函数）。

它的值是动态生成，便于维护。

#### data 选项 #7

data 对象存储 vue 实例需要使用的数据。

各项数据可以被实例直接调用，如 `vuemodel.$data.data1` = `vuemodel.data1` 

也可以在视图直接通过插值表达式访问，如 `<p>{{data1}}</p>` ，且它们是双向绑定。

【注意】data 中的数组类型值，索引操作（如，修改某项的值）或 length 操作（如，length=0，清空数组）不能自动更新视图。其他数组方法正常。借助 `Vue.set(data.arr, index, newValue)` 可以实现绑定渲染。

#### methods 选项 #8

methods 对象存储需要在 vue 实例中使用的函数。

e.g. 

```js
methods: {
    fn (value) {
        return value * 2
    }
}
```

方法可在插值表达式中使用，如 `{{ fn(dataA) }}`

也可以被实例调用，如 `vuemodel.fn()`，方法的 this 均指向实例，便于访问实例下的数据和其他方法。

## T2：Vue.js 基础指令

指令写在开始标签内，其本质就是 HTML 自定义属性。Vue.js 的指令以 v- 开头。

### 内容处理指令

#### v-once 指令 #2

使元素内部的插值表达式只生效一次，即第一次获取数据并渲染到视图后，就不再随数据变化。

#### v-text 指令 #3

将元素内容整体替换为指定纯文本数据，如 `<p v-text="newContent"></p>`，newContent 是 data 中的数据，能够驱动视图。

赋值以纯文本显示，不会解析 HTML 结构。

#### v-html 指令 #4

值中的 HTML 会被解析。

### 属性绑定指令 v-bind

动态绑定 HTML 属性，e.g.1 `<p v-bind:title="titleOfPara"></p>` ，v-bind 可省略。

绑定的属性值可以是变量、表达式。

一次绑定多个属性和值，e.g.2 `<p v-bind="attrObj"></p>` ，attrObj 是一个对象，它的各个键值对即为属性和值。此时，v-bind 作为自定义属性存在。

#### class 属性的绑定 #7

使用 `:class` 绑定的属性不影响原生 class 属性，可以共存。

绑定多个类名时的控制：`:class="{a: true, b: false, 'class-c': true}"` 。对象键值对形式，K 为类名，V 为布尔值，控制类名是否生效。

绑定多个需要控制和不需要控制的类名：`:class="[a, {b: true, c: false}, d]"` 。数组，成员又可以是对象。

#### style 属性的绑定 #8

`:style` 绑定的值是一个对象。类似于 CSS，但值要引号包裹，带 `-` 的属性改为驼峰命名或用引号包裹。

同名的样式，`:style` 的会覆盖掉原生 `style` 的。

绑定多个样式对象：`:style="[baseStyle, styleObj]"` ，使用对象数组。成员对象会自动融合，通常用于基础样式和特殊样式，便于复用和维护。

### 渲染指令

#### v-for 指令 #10

用于遍历数据渲染结构，数组和对象均可遍历。

数组：`<li v-for="(item, index) in arr">{{ item }}</li>`

对象：`<li v-for="(value, key, index) in obj">{{ value }}</li>`

使用 v-for 时，应始终指定唯一的 key 属性，可以提高渲染性能并避免问题（数据变化，而渲染惰性，导致错乱）。key 值可来自于数组每一项的 id 属性。

还可以利用 v-for 生成多个元素：`<li v-for="(item, index) in 5">{{ item }}</li>`

需要多个元素的重复生成时，可以对虚拟元素 &lt;template> 使用 v-for，它无需 `:key` 。

#### v-show 指令 #11

用于控制元素显示与隐藏，适用于显示隐藏频繁切换时使用。

v-show 作为 vue 提供的属性，值为 true 时元素显示，值为 `false` 时元素隐藏。

（不适用于虚拟元素，如 template）

#### v-if 指令 #12

用于根据条件控制元素的创建与移除。

```js
<p v-if="false">条件为假，元素不会被创建。</p>
<p v-else-if="true">条件为真，元素被创建。</p>
<p v-else>没有进入 else，元素不被创建。</p>
```

【注意】

- 使用 v-if 的同类型元素也需要绑定不同的 key
- 应避免将 v-if 与 v-for 应用于同一标签，因为可能会导致性能浪费。通常将 v-if 用在父元素上，先于遍历元素生成。

### T3：事件与表单处理

#### 事件绑定指令 v-on

#### 表单输入绑定指令 v-model #2~6

用于给 &lt;input>、 &lt;textarea> 及 &lt;select> 等输入类表单元素设置双向数据绑定。

单选框



复选框

- 单个选项（要求能勾选能取消的场景，比如同意协议）

  绑定的数据初始化为空字符串。勾选后会得到 true，取消勾选后会得到 false

- 多个选项

  绑定数据初始化为空数组，勾选选项后，其 value 值会被 push 进数组。

选择框 &lt;select>

- 单选题

  【常用】添加一个 value 为空的选项，在没有选择时默认选它，反馈信息。

- 多选题



### vue 指令的修饰符

可用在 v-on 指令后，以 `.` 连接

#### 事件修饰符

##### .prevent

阻止默认事件行为，相当于 event.preventDefault()

##### .stop

阻止事件传播（冒泡），相当于event.stopPropagation()

用在子元素上，阻止向上冒泡。

##### .once

用于设置事件只会触发一次

修饰符可以连写

#### 按键修饰符（只对键盘事件生效）

按键码

已被废弃的 KeyCode 仍可以直接作按键码，还是更推荐使用 vue 提供的别名。

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

按键修饰符连续使用时，任一按键都能触发事件，即**或**关系。

#### 系统修饰符

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

系统修饰符连续使用时，意为快捷键，即需共同使用。

##### .exact

用在恰恰只需要精确的系统修饰符触发的事件。

#### 鼠标修饰符

- `.left`
- `.right`
- `.middle`

#### v-model 指令的修饰符

v-model 用于表单元素的双向数据绑定，见【[#2~6]()】

##### .trim

自动过滤用户输入内容首尾两端的空格。

##### .lazy

将 v-model 的触发方式由 input 事件触发更改为 change 事件触发

##### .number

自动将用户输入的值使用 parseFloat() 转换为数值类型，如无法转换，则返回原始内容

## T4：进阶语法

### 自定义指令

补充内置指令，满足定制化需求。

#### 自定义全局指令 #2

```js
Vue.directive('focus', {
    inserted: function (el, binding) {
        el.focus()
    }
})
```

在生成实例前使用。上面例子定义的指令 v-focus 可以实现获取页面焦点，例如搜索框。

`inserted` 是一个钩子函数，表示在元素插入父节点时调用。根据需求可换成其他钩子函数。

钩子函数的参数有：

- el：所选元素

- binding：指令的详细设置
  - modifiers：修饰符，对象
  - expression：vue 属性值的表达式
  - value：属性值，或者说 expression 的计算结果

#### 自定义局部指令 #3

只能在当前 Vue 实例或组件内使用的指令。

在 Vue 构造函数的实参内配置：

```js
directives: {
    focus: {
        inserted (el) {
            el.focus();
        }
    }
}
```

### 过滤器

过滤器可以在插值表达式和 v-bind 绑定属性的值中使用

内容和过滤器以管道 `|` 连接

全局过滤器 #4~5

在生成 Vue 实例前，设置过滤器。

```js
Vue.filter('filterA', function () {
    
})
```

过滤器函数的参数：

- value：接收到的要处理的值，始终是第一个参数。

过滤器函数的返回值替换掉原值

多个过滤器可以连续使用。

局部过滤器 #6

只能在当前实例中使用。在 Vue 构造函数的实参内配置：

```js
filters: {
    
}
```

局部过滤器与全局过滤器同名时，局部过滤器生效。

【常用】全局定义通用的过滤器，在有特殊需求的地方重定义同名的局部过滤器。

计算属性 computed #7

视图部分不建议出现复杂的功能代码

计算属性使用时为属性形式，访问时会自动执行对应函数

```js
computed： {
    
}
```

methods 与 computed 区别：

- computed 具有缓存型，methods 没有。

- computed 通过属性名访问，methods 需要调用。

- computed 仅适用于计算操作，重复计算同一个值。

setter

计算属性默认只用 getter，getResult 作为一个方法；当需要 setter 时，getResult 是一个对象，包含了 get() 和 set() 两个方法。

### 侦听器 #10~11

```js
data: {
    value: []
}
watch: {
    value (val, oldVal) {}
}
```

侦听器写在 watch 对象内，为 data 中要侦听数据成员的同名函数。

##### 监听对象

```js
data: {
    obj: {
        content1: 'c1', content2: 'c2'
    }
},
watch: {
    obj: {
        deep: true,	// 打卡 deep 开关，才可以监听到对象成员的变化，数组不需要。
        handler (val) {}	// 监听对象、数组时，只能获取到新值 val
    }
}
```

##### 监听数组

监听数组，不要打开 deep。

无法监听到数组的长度变化和使用索引的修改，只有数组的方法或 Vue.set() 产生的变化能监听到。

#### Vue DevTools #12

网页使用了 Vue.js 时使用

#### Vue 实例的生命周期 #13

生命周期钩子函数：

1. 创建阶段（每个实例只能执行一次）
   - beforeCreate：实例初始化之前调用。
   - **created**： 实例创建后调用。
   - beforeMount：实例挂载之前调用。
   - **mounted**： 实例挂载完毕

2. 运行阶段（按需调用）
   - beforeUpdate：数据更新后，视图更新前调用。
   - updated： 视图更新后调用。

3. 销毁阶段（每个实例只能执行一次）
   - beforeDestroy：实例销毁之前调用。
   - destroyed： 实例销毁后调用。

## T5：Todomvc 项目综合案例

#### 准备工作



