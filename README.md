# fed-e-task-03-05

【Part 3. Vue.js 框架源码与进阶】【Mod 5. Vue.js 3.0 Composition APIs 及 3.0 原理剖析】

## 作业要求

1、Vue 3.0 性能提升主要是通过哪几方面体现的？

答：

- 响应式系统升级

  Vue.js 3.0 中使用 `Proxy` 对象重写响应式系统

  - 可以监听动态新增的属性
  - 可以监听删除的属性
  - 可以监听数组的索引和 `length` 属性

- 编译优化

  Vue.js 3.0 中标记和提升所有的静态根节点，diff 的时候只需要对比动态节点内容

  - `Fragments` （升级 vetur 插件）
  - 静态提升
  - Patch flag
  - 缓存事件处理函数

- 源码体积的优化
  - Vue.js 3.0 中移除了一些不常用的 API
    - 例如: `inline-template`、 `filter` 等
  - Tree-shaking

2、Vue 3.0 所采用的 Composition Api 与 Vue 2.x 使用的 Options Api 有什么区别？

答：

1. 在组件比较复杂的情况下，可以将逻辑代码合到一起去，而不会被 option 强行分隔。这提高了代码质量的上限，同时也拉低了代码质量的下限。
2. 更好的进行复用。
   在 vue2 中，想要复用部分逻辑的代码，都是通过 `mixin` 进去。但 `mixin` 进去的内容实际上很不直观，而且相同命名会被覆盖。而通过 composition API，因为所有的方法都是引入的，可以将单独某个逻辑进行封装。
3. 更好的 typescript 支持。不会再往 vue 原型上添加很多内容，而是通过引入的方式，类型定义会更清晰

> [Vue 3 - Composition API](https://v3.vuejs.org/guide/composition-api-introduction.html#why-composition-api)

`3、Proxy` 相对于 `Object.defineProperty` 有哪些优点？

答：

1. `Proxy` 可以劫持整个对象，这样以来操作便利程度远远优于 Object.defineProperty。
2. `Proxy` 可以直接监听数组的变化，无需进行数组方法重写。
3. `Proxy` 支持 13 种拦截操作，是 `Object.defineProperty` 不具备的。

4、Vue 3.0 在编译方面有哪些优化？

答：

- Fragment

  Vue3 中不在要求模版的跟节点必须是只能有一个节点。跟节点和和 render 函数返回的可以是纯文字、数组、单个节点，如果是数组，会自动转化为 Fragments。

- PatchFlag

  在 Vue3.0 中，在这个模版编译时，编译器会在动态标签末尾加上类似 /_ Text _/ PatchFlag。只能带 patchFlag 的 Node 才被认为是动态的元素，会被追踪属性的修改。并且 PatchFlag 会标识动态的属性类型有哪些，比如这里 的 TEXT 表示只有节点中的文字是动态的。

  每一个 Block 中的节点，就算很深，也是直接跟 Block 一层绑定的，可以直接跳转到动态节点而不需要逐个逐层遍历。

- hoistStatic 静态节点提升

  当使用 hoistStatic 时，所有静态的节点都被提升到 render 方法之外。这意味着，他们只会在应用启动的时候被创建一次，而后随着每次的渲染被不停的复用。

- cacheHandler 事件监听缓存

  编译器会为你动态创建一个内联函数，内联函数里面再去引用当前组件上最新的 handler。之后编译器会将内联函数缓存。每次重新渲染时如果事件处理器没有变，就会使用缓存中的事件处理而不会重新获取事件处理器。这个节点就可以被看作是一个静态的节点。这种优化更大的作用在于当其作用于组件时，之前每次重新渲染都会导致组件的重新渲染，在通过 handler 缓存之后，不会导致组件的重新渲染了。

- Tree Shaking

  利用 ES6 模块的静态引用特性，在编译时正确的判断到底加载了哪些代码。对代码全局做一个分析，找到那些没用被用到的模块、函数、变量，并把这些去掉。

5、Vue.js 3.0 响应式系统的实现原理？

答案：

简单来说，响应式原理就是利用 `Proxy` 第二个参数 `handler` 也就是陷阱操作符中，拦截各种取值、赋值操作，依托 `track` 和 `trigger` 两个函数进行依赖收集和派发更新。

`track` 用来在读取时收集依赖。

`trigger` 用来在更新时触发依赖。
