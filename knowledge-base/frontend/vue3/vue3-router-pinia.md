# 检索索引：本文档收录【Vue3】【Router与Pinia】知识点，内容100%来自Vue官方文档和Pinia官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

### Vue Router
Vue Router 是 Vue.js 的官方路由管理器。它与 Vue.js 核心深度集成，让构建单页面应用变得易如反掌。Vue Router 支持嵌套路由、动态路由匹配、模块化的路由配置、路由守卫等功能。

### Pinia
Pinia 是 Vue.js 的官方状态管理库，是 Vuex 的替代方案。它提供了更简洁的 API、更好的 TypeScript 支持、更轻量的体积，并且去除了 mutations，直接使用 actions 修改状态。

## 2. 核心知识点（结构化层级）

### 2.1 Vue Router 核心
1. 路由基础
   - 安装与配置
   - 路由声明
   - 路由出口
   - 导航链接

2. 动态路由匹配
   - 动态参数
   - 响应路由参数变化
   - 捕获所有路由
   - 匹配优先级

3. 嵌套路由
   - 嵌套的出口
   - 嵌套的配置

4. 编程式导航
   - 导航到不同位置
   - 替换当前位置
   - 历史记录导航

5. 命名路由与命名视图
   - 命名路由
   - 命名视图

6. 路由守卫
   - 全局前置守卫
   - 全局解析守卫
   - 全局后置钩子
   - 路由独享守卫
   - 组件内守卫

7. 路由元信息
8. 数据获取
9. 滚动行为
10. 路由懒加载

### 2.2 Pinia 核心
1. Store 基础
   - 定义 Store
   - 使用 Store
   - 响应式解构

2. State
   - 访问 State
   - 修改 State
   - 重置 State
   - 订阅 State

3. Getters
   - 定义 Getters
   - 访问 Getters
   - 向 Getter 传参

4. Actions
   - 定义 Actions
   - 调用 Actions
   - 订阅 Actions

5. 插件
   - 插件简介
   - 编写插件
   - 使用插件

6. 持久化

## 3. 官方标准语法 / API

### Vue Router 基础语法
```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'
import About from '../views/About.vue'

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: About
  },
  {
    path: '/users/:id',
    name: 'User',
    component: () => import('../views/User.vue')
  }
]

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes
})

export default router
```

### 路由导航语法
```vue
<script setup>
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

// 编程式导航
function goToHome() {
  router.push('/')
}

function goToUser(id) {
  router.push({ name: 'User', params: { id } })
}

function goWithQuery() {
  router.push({ path: '/search', query: { q: 'vue' } })
}

function replaceCurrent() {
  router.replace('/some-path')
}

function goBack() {
  router.back()
}

function goForward() {
  router.forward()
}
</script>

<template>
  <div>
    <!-- 声明式导航 -->
    <router-link to="/">首页</router-link>
    <router-link :to="{ name: 'About' }">关于</router-link>
    <router-link :to="{ name: 'User', params: { id: 1 } }">用户1</router-link>
    
    <!-- 编程式导航 -->
    <button @click="goToHome">回到首页</button>
    <button @click="goToUser(2)">用户2</button>
    
    <!-- 路由出口 -->
    <router-view />
  </div>
</template>
```

### 路由守卫语法
```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({ /* ... */ })

// 全局前置守卫
router.beforeEach((to, from, next) => {
  console.log('从', from.path, '到', to.path)
  next()
})

// 全局前置守卫（Promise 语法）
router.beforeEach(async (to, from) => {
  const isAuthenticated = await checkAuth()
  if (!isAuthenticated && to.meta.requiresAuth) {
    return { name: 'Login' }
  }
})

// 全局解析守卫
router.beforeResolve(async (to) => {
  // 在导航被确认之前、所有组件内守卫和异步路由组件被解析之后调用
})

// 全局后置钩子
router.afterEach((to, from, failure) => {
  if (!failure) {
    sendToAnalytics(to.fullPath)
  }
})

// 路由独享守卫
const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: (to, from) => {
      // 只在进入路由时触发
    }
  }
]

// 组件内守卫
<script setup>
import { onBeforeRouteEnter, onBeforeRouteUpdate, onBeforeRouteLeave } from 'vue-router'

onBeforeRouteEnter((to, from, next) => {
  // 在渲染该组件的对应路由被验证前调用
  // 不能获取组件实例 `this`
  // 可以通过传一个回调给 next 来访问组件实例
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
})

onBeforeRouteUpdate((to, from) => {
  // 在当前路由改变，但是该组件被复用时调用
  // 可以访问组件实例 `this`
})

onBeforeRouteLeave((to, from) => {
  // 导航离开该组件的对应路由时调用
  // 可以访问组件实例 `this`
})
</script>
```

### Pinia Store 定义语法
```javascript
// stores/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0,
    name: 'Eduardo',
    isAdmin: true
  }),
  getters: {
    doubleCount: (state) => state.count * 2,
    doubleCountPlusOne(): number {
      return this.doubleCount + 1
    },
    getUserById: (state) => {
      return (userId) => state.users.find((user) => user.id === userId)
    }
  },
  actions: {
    increment() {
      this.count++
    },
    decrement() {
      this.count--
    },
    reset() {
      this.count = 0
    },
    async registerUser(login, password) {
      try {
        this.userData = await api.post({ login, password })
        showTooltip(`Welcome back ${this.userData.name}!`)
      } catch (error) {
        showTooltip(error)
        return error
      }
    }
  }
})

// 使用 Setup Store 语法
export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const name = ref('Eduardo')
  const doubleCount = computed(() => count.value * 2)
  
  function increment() {
    count.value++
  }
  
  function decrement() {
    count.value--
  }
  
  return { count, name, doubleCount, increment, decrement }
})
```

### Pinia 使用语法
```vue
<script setup>
import { storeToRefs } from 'pinia'
import { useCounterStore } from '@/stores/counter'

const counterStore = useCounterStore()

// 直接访问
console.log(counterStore.count)
console.log(counterStore.doubleCount)

// 响应式解构（保持响应性）
const { count, name, doubleCount } = storeToRefs(counterStore)

// 调用 actions
counterStore.increment()
counterStore.decrement()
counterStore.reset()

// 直接修改 state
counterStore.count++
counterStore.$patch({ count: counterStore.count + 1 })
counterStore.$patch((state) => {
  state.count++
  state.name = 'Sam'
})

// 重置 state
counterStore.$reset()

// 订阅 state
counterStore.$subscribe((mutation, state) => {
  console.log(mutation.type, mutation.payload)
})

// 订阅 actions
counterStore.$onAction(({ name, store, args, after, onError }) => {
  console.log(`Action "${name}" was called with args:`, args)
  
  after((result) => {
    console.log(`Action "${name}" finished with result:`, result)
  })
  
  onError((error) => {
    console.error(`Action "${name}" threw an error:`, error)
  })
})
</script>

<template>
  <div>
    <p>Count: {{ counterStore.count }}</p>
    <p>Double Count: {{ counterStore.doubleCount }}</p>
    <p>Name: {{ counterStore.name }}</p>
    
    <button @click="counterStore.increment">Increment</button>
    <button @click="counterStore.decrement">Decrement</button>
    <button @click="counterStore.reset">Reset</button>
  </div>
</template>
```

## 4. 官方示例代码（可运行）

### Vue Router 完整示例
```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import('../views/Home.vue')
  },
  {
    path: '/about',
    name: 'About',
    component: () => import('../views/About.vue')
  },
  {
    path: '/users',
    name: 'Users',
    component: () => import('../views/Users.vue'),
    children: [
      {
        path: ':id',
        name: 'UserDetail',
        component: () => import('../views/UserDetail.vue')
      }
    ]
  },
  {
    path: '/login',
    name: 'Login',
    component: () => import('../views/Login.vue'),
    meta: { requiresGuest: true }
  },
  {
    path: '/dashboard',
    name: 'Dashboard',
    component: () => import('../views/Dashboard.vue'),
    meta: { requiresAuth: true }
  },
  {
    path: '/:pathMatch(.*)*',
    name: 'NotFound',
    component: () => import('../views/NotFound.vue')
  }
]

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes,
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    } else {
      return { top: 0 }
    }
  }
})

// 全局前置守卫
router.beforeEach((to, from) => {
  const isAuthenticated = localStorage.getItem('isAuthenticated') === 'true'
  
  if (to.meta.requiresAuth && !isAuthenticated) {
    return { name: 'Login', query: { redirect: to.fullPath } }
  }
  
  if (to.meta.requiresGuest && isAuthenticated) {
    return { name: 'Dashboard' }
  }
})

export default router
```

```vue
<!-- App.vue -->
<script setup>
import { ref } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()
const isAuthenticated = ref(localStorage.getItem('isAuthenticated') === 'true')

function logout() {
  localStorage.removeItem('isAuthenticated')
  isAuthenticated.value = false
  router.push({ name: 'Home' })
}
</script>

<template>
  <div id="app">
    <nav>
      <router-link :to="{ name: 'Home' }">首页</router-link>
      <router-link :to="{ name: 'About' }">关于</router-link>
      <template v-if="isAuthenticated">
        <router-link :to="{ name: 'Users' }">用户</router-link>
        <router-link :to="{ name: 'Dashboard' }">仪表盘</router-link>
        <button @click="logout">退出</button>
      </template>
      <template v-else>
        <router-link :to="{ name: 'Login' }">登录</router-link>
      </template>
    </nav>
    
    <main>
      <router-view v-slot="{ Component }">
        <transition name="fade" mode="out-in">
          <component :is="Component" />
        </transition>
      </router-view>
    </main>
  </div>
</template>

<style>
#app {
  font-family: Arial, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

nav {
  background: #f5f5f5;
  padding: 10px;
  margin-bottom: 20px;
  border-radius: 4px;
}

nav a, nav button {
  margin-right: 15px;
  text-decoration: none;
  color: #333;
}

nav a.router-link-active {
  color: #42b983;
  font-weight: bold;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
```

```vue
<!-- views/Login.vue -->
<script setup>
import { ref } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()
const username = ref('')
const password = ref('')
const error = ref('')

async function handleLogin() {
  error.value = ''
  
  if (!username.value || !password.value) {
    error.value = '请输入用户名和密码'
    return
  }
  
  // 模拟登录
  await new Promise(resolve => setTimeout(resolve, 1000))
  
  localStorage.setItem('isAuthenticated', 'true')
  localStorage.setItem('username', username.value)
  
  const redirect = route.query.redirect || '/'
  router.push(redirect)
}
</script>

<template>
  <div class="login">
    <h1>登录</h1>
    
    <div v-if="error" class="error">{{ error }}</div>
    
    <form @submit.prevent="handleLogin">
      <div>
        <label for="username">用户名：</label>
        <input id="username" v-model="username" type="text" />
      </div>
      
      <div>
        <label for="password">密码：</label>
        <input id="password" v-model="password" type="password" />
      </div>
      
      <button type="submit">登录</button>
    </form>
  </div>
</template>

<style scoped>
.login {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
}

.error {
  color: red;
  margin-bottom: 10px;
}

div {
  margin-bottom: 10px;
}

label {
  display: block;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
}

button {
  width: 100%;
  padding: 10px;
  background: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

### Pinia 完整示例
```javascript
// stores/user.js
import { defineStore } from 'pinia'

export const useUserStore = defineStore('user', {
  state: () => ({
    users: [
      { id: 1, name: 'Alice', email: 'alice@example.com', isActive: true },
      { id: 2, name: 'Bob', email: 'bob@example.com', isActive: false },
      { id: 3, name: 'Charlie', email: 'charlie@example.com', isActive: true }
    ],
    loading: false,
    error: null,
    currentUserId: null
  }),
  
  getters: {
    activeUsers: (state) => state.users.filter(user => user.isActive),
    activeUserCount: (state) => state.users.filter(user => user.isActive).length,
    getUserById: (state) => (id) => state.users.find(user => user.id === id),
    currentUser: (state) => state.users.find(user => user.id === state.currentUserId)
  },
  
  actions: {
    setCurrentUserId(id) {
      this.currentUserId = id
    },
    
    async fetchUsers() {
      this.loading = true
      this.error = null
      
      try {
        // 模拟 API 调用
        await new Promise(resolve => setTimeout(resolve, 1000))
        // 数据保持原样
      } catch (error) {
        this.error = '获取用户列表失败'
        console.error(error)
      } finally {
        this.loading = false
      }
    },
    
    addUser(user) {
      this.users.push({
        id: Date.now(),
        ...user,
        isActive: true
      })
    },
    
    updateUser(id, updates) {
      const index = this.users.findIndex(user => user.id === id)
      if (index !== -1) {
        this.users[index] = { ...this.users[index], ...updates }
      }
    },
    
    deleteUser(id) {
      const index = this.users.findIndex(user => user.id === id)
      if (index !== -1) {
        this.users.splice(index, 1)
      }
    },
    
    toggleUserActive(id) {
      const user = this.users.find(user => user.id === id)
      if (user) {
        user.isActive = !user.isActive
      }
    }
  }
})
```

```vue
<!-- UserList.vue -->
<script setup>
import { ref, onMounted } from 'vue'
import { storeToRefs } from 'pinia'
import { useUserStore } from '@/stores/user'

const userStore = useUserStore()
const { users, activeUsers, loading, error } = storeToRefs(userStore)

const newUserName = ref('')
const newUserEmail = ref('')

onMounted(() => {
  userStore.fetchUsers()
})

function handleAddUser() {
  if (newUserName.value && newUserEmail.value) {
    userStore.addUser({
      name: newUserName.value,
      email: newUserEmail.value
    })
    newUserName.value = ''
    newUserEmail.value = ''
  }
}
</script>

<template>
  <div>
    <h2>用户管理</h2>
    
    <div v-if="loading" class="loading">加载中...</div>
    <div v-else-if="error" class="error">{{ error }}</div>
    
    <div v-else>
      <div class="stats">
        <p>总用户数: {{ users.length }}</p>
        <p>活跃用户数: {{ activeUsers.length }}</p>
      </div>
      
      <div class="add-user">
        <h3>添加用户</h3>
        <input v-model="newUserName" placeholder="姓名" />
        <input v-model="newUserEmail" placeholder="邮箱" />
        <button @click="handleAddUser">添加</button>
      </div>
      
      <table class="user-table">
        <thead>
          <tr>
            <th>ID</th>
            <th>姓名</th>
            <th>邮箱</th>
            <th>状态</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" :key="user.id">
            <td>{{ user.id }}</td>
            <td>{{ user.name }}</td>
            <td>{{ user.email }}</td>
            <td>
              <span :class="{ active: user.isActive, inactive: !user.isActive }">
                {{ user.isActive ? '活跃' : '不活跃' }}
              </span>
            </td>
            <td>
              <button @click="userStore.toggleUserActive(user.id)">
                切换状态
              </button>
              <button @click="userStore.deleteUser(user.id)" class="delete">
                删除
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.loading {
  text-align: center;
  padding: 20px;
}

.error {
  color: red;
  padding: 10px;
  background: #ffebee;
  border-radius: 4px;
}

.stats {
  background: #f5f5f5;
  padding: 10px;
  margin-bottom: 20px;
  border-radius: 4px;
}

.add-user {
  margin-bottom: 20px;
}

.add-user input {
  margin-right: 10px;
  padding: 8px;
}

button {
  padding: 8px 16px;
  margin: 0 5px;
  cursor: pointer;
}

.delete {
  background: #ff4444;
  color: white;
  border: none;
}

.user-table {
  width: 100%;
  border-collapse: collapse;
}

.user-table th,
.user-table td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.user-table th {
  background: #f5f5f5;
}

.active {
  color: green;
  font-weight: bold;
}

.inactive {
  color: red;
}
</style>
```

```javascript
// main.js
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'
import router from './router'

const app = createApp(App)
const pinia = createPinia()

// 添加持久化插件示例
function createPersistedStatePlugin() {
  return ({ store }) => {
    const savedState = localStorage.getItem(`pinia-${store.$id}`)
    if (savedState) {
      store.$patch(JSON.parse(savedState))
    }
    
    store.$subscribe((mutation, state) => {
      localStorage.setItem(`pinia-${store.$id}`, JSON.stringify(state))
    })
  }
}

pinia.use(createPersistedStatePlugin())

app.use(pinia)
app.use(router)
app.mount('#app')
```

## 5. 官方注意事项 / 最佳实践

### Vue Router 最佳实践
1. **路由懒加载**：使用动态导入实现路由懒加载，减少初始加载体积
2. **命名路由**：优先使用命名路由，避免硬编码路径
3. **路由元信息**：使用 meta 字段存储路由元信息，如权限、标题等
4. **路由守卫**：合理使用路由守卫处理权限和数据加载
5. **滚动行为**：配置合适的滚动行为
6. **404 路由**：配置捕获所有路由处理 404 页面

### Pinia 最佳实践
1. **Store 命名**：使用有意义的 Store 名称
2. **State 设计**：保持 State 扁平化，避免过度嵌套
3. **Getters 使用**：使用 Getters 计算派生状态
4. **Actions 使用**：使用 Actions 处理异步逻辑和复杂操作
5. **响应式解构**：使用 storeToRefs 保持响应性
6. **插件使用**：合理使用插件扩展功能，如持久化
7. **TypeScript**：充分利用 TypeScript 类型支持

### 性能优化
1. **路由懒加载**：减少初始包大小
2. **组件懒加载**：按需加载组件
3. **合理使用 Store**：只在必要时使用 Store，避免过度使用
4. **缓存**：合理使用 keep-alive 缓存组件
5. **数据预加载**：在路由守卫中预加载数据

## 6. 官方文档参考链接

- Vue Router 官方文档：https://router.vuejs.org/zh/
- Pinia 官方文档：https://pinia.vuejs.org/zh/
- Vue Router 入门：https://router.vuejs.org/zh/introduction.html
- Pinia 入门：https://pinia.vuejs.org/zh/getting-started.html
