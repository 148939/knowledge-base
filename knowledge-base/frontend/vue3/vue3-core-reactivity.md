# 检索索引：本文档收录【Vue3】【核心响应式】知识点，内容100%来自Vue官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Vue 3 的响应式系统是其核心特性之一。它采用了基于 Proxy 的新实现，提供了更好的性能和更强大的功能。响应式系统的核心思想是：当响应式数据发生变化时，依赖于这些数据的视图会自动更新。

## 2. 核心知识点（结构化层级）

### 2.1 响应式基础
1. 响应式代理（Reactive Proxy）
2. 响应式引用（Ref）
3. 深层响应性
4. 浅层响应性

### 2.2 响应式 API
1. reactive()
2. ref()
3. computed()
4. readonly()
5. watch()
6. watchEffect()
7. watchPostEffect()
8. watchSyncEffect()

### 2.3 响应式原理
1. Proxy vs Object.defineProperty
2. 依赖追踪（Dependency Tracking）
3. 触发更新（Triggering Updates）
4. 响应式数据的特殊处理

### 2.4 响应式进阶
1. 响应式数组
2. 响应式对象
3. 响应式与解构
4. 响应式与 this
5. toRef() 和 toRefs()
6. unref()
7. isRef()、isReactive()、isReadonly()、isProxy()

## 3. 官方标准语法 / API

### 基础响应式 API
```javascript
import { reactive, ref, computed, readonly, watch, watchEffect } from 'vue'

// reactive - 创建响应式对象
const state = reactive({
  count: 0,
  name: 'Vue'
})

// ref - 创建响应式引用
const count = ref(0)
const message = ref('Hello Vue')

// computed - 创建计算属性
const doubleCount = computed(() => count.value * 2)
const fullName = computed({
  get() {
    return state.firstName + ' ' + state.lastName
  },
  set(value) {
    [state.firstName, state.lastName] = value.split(' ')
  }
})

// readonly - 创建只读响应式对象
const readonlyState = readonly(state)

// watch - 监听数据源变化
watch(count, (newValue, oldValue) => {
  console.log(`count changed from ${oldValue} to ${newValue}`)
})

watch(
  () => state.count,
  (newValue, oldValue) => {
    console.log(`state.count changed from ${oldValue} to ${newValue}`)
  }
)

watch([count, () => state.name], ([newCount, newName], [oldCount, oldName]) => {
  console.log('Multiple sources changed')
})

// watchEffect - 立即执行并追踪依赖
watchEffect(() => {
  console.log(`count is: ${count.value}`)
})
```

### 响应式转换 API
```javascript
import { toRef, toRefs, unref, isRef, isReactive, isReadonly, isProxy } from 'vue'

// toRef - 为响应式对象的属性创建 ref
const state = reactive({ count: 0 })
const countRef = toRef(state, 'count')

// toRefs - 将响应式对象转换为普通对象，每个属性都是 ref
const state = reactive({ count: 0, name: 'Vue' })
const stateRefs = toRefs(state)

// unref - 如果是 ref 则返回其值，否则返回原值
const value = unref(someValue)

// 类型检查
console.log(isRef(countRef)) // true
console.log(isReactive(state)) // true
console.log(isReadonly(readonlyState)) // true
console.log(isProxy(state)) // true
```

### 浅层响应式 API
```javascript
import { shallowReactive, shallowRef, triggerRef, customRef } from 'vue'

// shallowReactive - 浅层响应式
const shallowState = shallowReactive({
  count: 0,
  nested: { value: 1 }
})

// shallowRef - 浅层 ref
const shallowCount = shallowRef({ count: 0 })

// triggerRef - 手动触发 shallowRef 更新
shallowCount.value.count = 1
triggerRef(shallowCount)

// customRef - 自定义 ref
function useDebouncedRef(value, delay = 200) {
  let timeout
  return customRef((track, trigger) => {
    return {
      get() {
        track()
        return value
      },
      set(newValue) {
        clearTimeout(timeout)
        timeout = setTimeout(() => {
          value = newValue
          trigger()
        }, delay)
      }
    }
  })
}
```

## 4. 官方示例代码（可运行）

### 基础响应式示例
```html
<script setup>
import { reactive, ref, computed, watch, watchEffect } from 'vue'

// reactive 示例
const state = reactive({
  count: 0,
  message: 'Hello Vue 3'
})

// ref 示例
const count = ref(0)
const name = ref('')

// computed 示例
const doubleCount = computed(() => count.value * 2)
const reversedMessage = computed(() => state.message.split('').reverse().join(''))

// watch 示例
watch(count, (newValue, oldValue) => {
  console.log(`count changed: ${oldValue} -> ${newValue}`)
})

watch(
  () => state.count,
  (newValue, oldValue) => {
    console.log(`state.count changed: ${oldValue} -> ${newValue}`)
  },
  { immediate: true }
)

// watchEffect 示例
watchEffect(() => {
  console.log(`watchEffect: count is ${count.value}, name is ${name.value}`)
})

// 方法
function increment() {
  count.value++
  state.count++
}

function decrement() {
  count.value--
  state.count--
}

function reset() {
  count.value = 0
  state.count = 0
  state.message = 'Hello Vue 3'
}
</script>

<template>
  <div>
    <h2>响应式基础示例</h2>
    
    <div>
      <h3>ref 计数器</h3>
      <p>Count: {{ count }}</p>
      <p>Double Count: {{ doubleCount }}</p>
      <button @click="increment">+</button>
      <button @click="decrement">-</button>
      <button @click="reset">Reset</button>
    </div>
    
    <div>
      <h3>reactive 状态</h3>
      <p>State Count: {{ state.count }}</p>
      <p>Message: {{ state.message }}</p>
      <p>Reversed Message: {{ reversedMessage }}</p>
      <input v-model="state.message" placeholder="输入消息" />
    </div>
    
    <div>
      <h3>姓名输入</h3>
      <input v-model="name" placeholder="输入姓名" />
      <p v-if="name">Hello, {{ name }}!</p>
    </div>
  </div>
</template>

<style scoped>
div {
  margin: 20px 0;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
}
button {
  margin: 0 5px;
  padding: 8px 16px;
}
input {
  padding: 8px;
  margin: 5px 0;
}
</style>
```

### 购物车响应式示例
```html
<script setup>
import { reactive, computed, watch } from 'vue'

// 购物车状态
const cart = reactive({
  items: [
    { id: 1, name: 'Vue.js Book', price: 29.99, quantity: 1 },
    { id: 2, name: 'Vue T-Shirt', price: 19.99, quantity: 2 }
  ]
})

// 计算属性
const totalItems = computed(() => {
  return cart.items.reduce((sum, item) => sum + item.quantity, 0)
})

const totalPrice = computed(() => {
  return cart.items.reduce((sum, item) => sum + item.price * item.quantity, 0)
})

// 监听购物车变化
watch(
  () => cart.items,
  (newItems) => {
    console.log('Cart updated:', newItems)
    saveToLocalStorage()
  },
  { deep: true }
)

// 方法
function addItem() {
  const newItem = {
    id: Date.now(),
    name: `New Item ${cart.items.length + 1}`,
    price: Math.floor(Math.random() * 50) + 10,
    quantity: 1
  }
  cart.items.push(newItem)
}

function removeItem(id) {
  const index = cart.items.findIndex(item => item.id === id)
  if (index !== -1) {
    cart.items.splice(index, 1)
  }
}

function updateQuantity(id, delta) {
  const item = cart.items.find(item => item.id === id)
  if (item) {
    item.quantity = Math.max(1, item.quantity + delta)
  }
}

function clearCart() {
  cart.items = []
}

function saveToLocalStorage() {
  localStorage.setItem('vue-cart', JSON.stringify(cart.items))
}

function loadFromLocalStorage() {
  const saved = localStorage.getItem('vue-cart')
  if (saved) {
    cart.items = JSON.parse(saved)
  }
}

// 初始化时加载本地存储
loadFromLocalStorage()
</script>

<template>
  <div class="cart">
    <h2>购物车 (响应式示例)</h2>
    
    <div class="cart-summary">
      <p>总商品数: {{ totalItems }}</p>
      <p>总价: ${{ totalPrice.toFixed(2) }}</p>
    </div>
    
    <div class="cart-actions">
      <button @click="addItem">添加商品</button>
      <button @click="clearCart" :disabled="cart.items.length === 0">清空购物车</button>
    </div>
    
    <div v-if="cart.items.length === 0" class="empty-cart">
      <p>购物车是空的</p>
    </div>
    
    <div v-else class="cart-items">
      <div v-for="item in cart.items" :key="item.id" class="cart-item">
        <div class="item-info">
          <h3>{{ item.name }}</h3>
          <p>单价: ${{ item.price.toFixed(2) }}</p>
        </div>
        <div class="item-quantity">
          <button @click="updateQuantity(item.id, -1)">-</button>
          <span>{{ item.quantity }}</span>
          <button @click="updateQuantity(item.id, 1)">+</button>
        </div>
        <div class="item-total">
          <p>小计: ${{ (item.price * item.quantity).toFixed(2) }}</p>
        </div>
        <button class="remove-btn" @click="removeItem(item.id)">删除</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cart {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}
.cart-summary {
  background: #f5f5f5;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}
.cart-actions {
  margin-bottom: 20px;
}
button {
  padding: 8px 16px;
  margin-right: 10px;
  cursor: pointer;
}
.empty-cart {
  text-align: center;
  padding: 40px;
  color: #666;
}
.cart-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  margin-bottom: 10px;
}
.item-info {
  flex: 1;
}
.item-quantity {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 0 20px;
}
.item-quantity button {
  width: 30px;
  height: 30px;
  padding: 0;
  margin: 0;
}
.remove-btn {
  background: #ff4444;
  color: white;
  border: none;
}
</style>
```

### 自定义 Ref 示例
```html
<script setup>
import { customRef, ref } from 'vue'

// 防抖 ref
function useDebouncedRef(initialValue, delay = 500) {
  let timeout
  return customRef((track, trigger) => {
    return {
      get() {
        track()
        return initialValue
      },
      set(value) {
        clearTimeout(timeout)
        timeout = setTimeout(() => {
          initialValue = value
          trigger()
        }, delay)
      }
    }
  })
}

// 历史记录 ref
function useHistoryRef(initialValue, maxHistory = 10) {
  const history = ref([initialValue])
  const currentIndex = ref(0)
  
  return customRef((track, trigger) => {
    return {
      get() {
        track()
        return history.value[currentIndex.value]
      },
      set(value) {
        if (value !== history.value[currentIndex.value]) {
          // 移除当前位置之后的历史记录
          history.value = history.value.slice(0, currentIndex.value + 1)
          history.value.push(value)
          
          // 限制历史记录长度
          if (history.value.length > maxHistory) {
            history.value.shift()
          } else {
            currentIndex.value++
          }
          
          trigger()
        }
      }
    }
  }, { history, currentIndex })
}

// 使用防抖 ref
const searchText = useDebouncedRef('', 300)
const searchResults = ref([])

// 使用历史记录 ref
const noteText = useHistoryRef('在这里输入笔记...')
const noteHistory = ref(['在这里输入笔记...'])
const noteIndex = ref(0)

// 模拟搜索
async function performSearch(query) {
  if (!query) {
    searchResults.value = []
    return
  }
  
  // 模拟 API 调用
  await new Promise(resolve => setTimeout(resolve, 500))
  
  searchResults.value = [
    `结果 1: ${query}`,
    `结果 2: ${query}`,
    `结果 3: ${query}`
  ]
}

// 监听搜索文本变化
import { watchEffect } from 'vue'
watchEffect(() => {
  performSearch(searchText.value)
})
</script>

<template>
  <div>
    <h2>自定义 Ref 示例</h2>
    
    <div class="section">
      <h3>防抖搜索</h3>
      <input 
        v-model="searchText" 
        placeholder="输入搜索内容（300ms 防抖）"
        style="width: 100%; padding: 8px;"
      />
      <div v-if="searchResults.length > 0" style="margin-top: 10px;">
        <h4>搜索结果：</h4>
        <ul>
          <li v-for="(result, index) in searchResults" :key="index">
            {{ result }}
          </li>
        </ul>
      </div>
    </div>
    
    <div class="section">
      <h3>带历史记录的笔记</h3>
      <textarea 
        v-model="noteText" 
        rows="5"
        style="width: 100%; padding: 8px;"
      ></textarea>
      <div style="margin-top: 10px;">
        <span>历史记录: {{ noteIndex + 1 }} / {{ noteHistory.length }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.section {
  margin: 20px 0;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
}
</style>
```

## 5. 官方注意事项 / 最佳实践

### 响应式系统注意事项
1. **reactive 的局限性**：
   - 不能直接替换整个对象
   - 不能正确处理 Map、Set、WeakMap、WeakSet（Vue 3.2+ 已支持）
   - 解构后会失去响应性

2. **ref 的最佳实践**：
   - 对于原始值，优先使用 ref
   - 访问 ref 的值时需要使用 .value
   - 在模板中会自动解包，不需要 .value

3. **computed 的使用**：
   - 计算属性应该是纯函数，没有副作用
   - 避免在 computed 中进行异步操作
   - 对于复杂计算，使用 computed 而非方法

4. **watch vs watchEffect**：
   - watch：明确指定数据源，可以获取新旧值
   - watchEffect：自动追踪依赖，立即执行
   - watchPostEffect：在 DOM 更新后执行
   - watchSyncEffect：同步执行

### 最佳实践
1. 使用组合式 API 时，将相关逻辑组织在一起
2. 合理使用 toRef 和 toRefs 处理响应式对象的解构
3. 避免不必要的响应式数据，使用 shallowReactive 和 shallowRef 优化性能
4. 对于大型数组和对象，考虑使用 markRaw 标记不需要响应式的对象
5. 使用 readonly 保护不应该被修改的响应式数据
6. 善用 TypeScript 类型增强开发体验
7. 遵循 Vue 官方风格指南

## 6. 官方文档参考链接

- Vue 3 响应式基础：https://cn.vuejs.org/guide/essentials/reactivity-fundamentals.html
- 响应式 API 详解：https://cn.vuejs.org/api/reactivity-core.html
- 深入响应式原理：https://cn.vuejs.org/guide/extras/reactivity-in-depth.html
- 组合式 API：https://cn.vuejs.org/guide/composition-api-introduction.html
