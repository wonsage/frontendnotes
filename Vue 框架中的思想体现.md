# Vue 思想吸收

状态管理

1. 多个组件共用某些数据

   如果直接将组件的 data 指向同一对象作为来源，每一个组件都可以对源数据访问和修改，这样为调试带来困难，错误做法。

2. store 模式

   store 对象内有数据（states）和对数据操作的相关方法（actions）。

   多个组件可将 store 作为公用状态，但使用 store 要遵从约定：组件不允许直接变更 store 的 state，而必须执行 action 来 dispatch 事件实现变更。这样，可以记录所有 store 中发生的 state 变更。

3. Vuex

   store 模式更进一步，Vuex 则实现了整个工程下所有组件可访问的状态。

   actions commit mutations，mutations mutate states.

