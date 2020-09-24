# reactive, toRefs, ref, 生命周期钩子函数

```html

<div id="app">
  x: {{ x }} <br/>
  y: {{ y }}
</div>
<script type="module">
  import { createApp, reactive, onMounted, onUnmounted, toRefs } from './node_modules/vue/dist/vue.esm-browser.js'

  function useMousePosition () {
    const position = reactive({
      x: 0,
      y: 0,
    })

    const update = e => {
      position.x = e.pageX
      position.y = e.pageY
    }

    onMounted(() => {
      window.addEventListener('mousemove', update)
    })
    onUnmounted(() => {
      window.removeEventListener('mousemove', update)
    })

    return toRefs(position)
  }

  const app = createApp({
    setup () {
      // 第一个参数 props
      // 第二个参数 context，attars，emit，slots
      // const position = useMousePosition()
      const { x, y } = useMousePosition()

      return {
        // position
        x, y
      }
    },
    mounted () {

    }
  })
  console.log(app);

  app.mount("#app")
</script>
```

```html

<div id="app">
  <button @click="increase">Add - {{ count }}</button>
</div>
<script type="module">
  import { createApp, ref } from './node_modules/vue/dist/vue.esm-browser.js'

  function useCount () {
    const count = ref(0)
    return {
      count,
      increase: () => {
        count.value++
      }
    }
  }

  createApp({
    setup () {
      return {
        ...useCount()
      }
    }
  }).mount('#app')
</script>
```

# Computed

- 第一种用法

    ```js
    watch(() => count.value + 1)
    ```

- 第二种用法

    ```js
    const count = ref(1)
    const plusOne = computed({
    	get: () => count.value + 1,
    	set: val => {
    		count.value = val - 1
    	}
    })
    ```

```html

<div id="app">
  <button @click="push">Button</button>
  未完成：{{ activeCount }}
</div>
<script type="module">
  import { createApp, reactive, computed } from './node_modules/vue/dist/vue.esm-browser.js'

  const data = [
    { text: 'reading', completed: false },
    { text: 'coding', completed: false },
    { text: 'dating', completed: true },
  ]

  createApp({
    setup () {
      const todos = reactive(data)

      const activeCount = computed(() => {
        return todos.filter(item => !item.completed).length
      })

      return {
        activeCount,
        push: () => {
          todos.push({
            text: '开会',
            completed: false
          })
        }
      }
    }
  }).mount('#app')
</script>
```

# Watch

- Watch 的三个参数
    - 第一个参数: 要监听的数据
    - 第二个参数: 监听到数据变化后执行的函数，这个函数有两个参数分别 是新值和旧值
    - 第三个参数: 选项对象，`deep` 和 `immediate`
- Watch 的返回值
    - 取消监听的函数

```html

<div id="app">
  <p>
    请问一个 yes/no 的问题：
    <input v-model="question" type="text">
  </p>
  <p>{{ answer }}</p>
</div>

<script type="module">
  // https://www.yesno.wtf/api
  import { createApp, ref, watch } from './static/vue.esm-browser.js'

  createApp({
    setup () {
      const question = ref('')
      const answer = ref('')

      watch(question, async (newValue, oldValue) => {
        const response = await fetch('https://www.yesno.wtf/api')
        const data = await response.json()
        answer.value = data.answer
      })

      return {
        question,
        answer,
      }
    },
  }).mount('#app')
</script>
```

# WatchEffect

- 是 `watch` 函数的简化版本，也用来监视数据的变化
- 接收一个函数作为参数，监听函数内响应式数据的变化

# ToDoList

## 功能列表

- 添加
- 删除
- 编辑
    - 双击待办项，展示编辑文本框
    - 按回车或者编辑文本框失去焦点，修改数据
    - 按esc取消编辑
    - 把编辑文本框清空按回车，删除这一项
    - 显示编辑文本框的时候获取焦点
- 切换
    - 点击 checkbox，改变所有待办项状态
    - All / Active / Completed
    - 其它
        - 显示未完成待办项个数
        - 移除所有完成的项目
        - 如果没有待办项，隐藏 main 和 footer
- 存储

## 自定义指令

- Vue 2.x

    ```js
    Vue.directive('editingFocus', {
      bind(el, binding, vnode, prevVnode) {},
      inserted() {},
      update() {}, // remove
      componentUpdated() {},
      unbind() {}
    })
    ```

- Vue 3.0

    ```js
    app.directive('editingFocus', {
      beforeMount(el, binding, vnode, prevVnode) {},
      mounted() {},
      beforeUpdate() {}, // new
      updated() {},
      beforeUnmount() {}, // new
      unmounted() {}
    })
    ```
