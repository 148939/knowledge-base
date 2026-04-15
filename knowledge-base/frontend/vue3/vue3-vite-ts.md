# 检索索引：本文档收录【Vue3】【Vite与TypeScript】知识点，内容100%来自Vue官方文档、Vite官方文档和TypeScript官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

### Vite
Vite 是一种新型前端构建工具，能够显著提升前端开发体验。它主要由两部分组成：
1. 一个开发服务器，它基于原生 ES 模块提供了丰富的内建功能，如速度快到惊人的模块热更新（HMR）。
2. 一套构建指令，它使用 Rollup 打包你的代码，并且它是预配置的，可输出用于生产环境的高度优化过的静态资源。

### Vue 3 + TypeScript
Vue 3 从底层就是用 TypeScript 编写的，这意味着你无需任何额外的工具就能获得完整的 TypeScript 支持。TypeScript 为 Vue 应用提供了更好的类型安全、更好的开发体验和更好的可维护性。

## 2. 核心知识点（结构化层级）

### 2.1 Vite 项目创建
1. 使用 create-vite 创建项目
2. 项目结构
3. 配置文件 vite.config.ts
4. 环境变量
5. 开发服务器
6. 生产构建

### 2.2 Vite 核心功能
1. 模块热更新（HMR）
2. 依赖预构建
3. TypeScript 支持
4. CSS 预处理器
5. 静态资源处理
6. 插件系统

### 2.3 Vue 3 与 TypeScript
1. script setup 与 TypeScript
2. 类型声明 defineProps
3. 类型声明 defineEmits
4. 类型声明 defineSlots
5. 组件类型定义
6. 响应式 API 类型
7. provide/inject 类型
8. ref 类型注解

### 2.4 项目配置
1. tsconfig.json 配置
2. vite.config.ts 配置
3. 路径别名
4. 类型声明文件
5. ESLint 配置
6. Prettier 配置

### 2.5 高级特性
1. 插件开发
2. SSR 支持
3. 库模式
4. 性能优化
5. 类型安全的状态管理

## 3. 官方标准语法 / API

### Vite 项目创建
```bash
# 使用 npm
npm create vite@latest my-vue-app -- --template vue-ts

# 使用 yarn
yarn create vite my-vue-app --template vue-ts

# 使用 pnpm
pnpm create vite my-vue-app --template vue-ts
```

### Vite 配置文件
```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { fileURLToPath, URL } from 'node:url'

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  server: {
    port: 3000,
    open: true,
    proxy: {
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, '')
      }
    }
  },
  build: {
    outDir: 'dist',
    sourcemap: true,
    rollupOptions: {
      output: {
        manualChunks: {
          'vue-vendor': ['vue', 'vue-router', 'pinia'],
          'ui-vendor': ['element-plus']
        }
      }
    }
  }
})
```

### TypeScript 配置
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "skipLibCheck": true,
    
    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve",
    
    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    
    /* Path mapping */
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

### Vue 3 与 TypeScript 基础
```vue
<script setup lang="ts">
import { ref, computed, reactive, onMounted } from 'vue'

// ref 类型注解
const count = ref<number>(0)
const message = ref<string>('Hello Vue 3 + TypeScript')
const user = ref<{ name: string; age: number } | null>(null)

// 自动类型推断
const isActive = ref(false)

// 计算属性类型
const doubleCount = computed(() => count.value * 2)

// 响应式对象类型
interface User {
  name: string
  age: number
  email: string
}

const state = reactive<User>({
  name: 'Alice',
  age: 30,
  email: 'alice@example.com'
})

// 函数类型
function increment(): void {
  count.value++
}

function greet(name: string): string {
  return `Hello, ${name}!`
}

// 生命周期钩子
onMounted(() => {
  console.log('Component mounted')
})
</script>
```

### defineProps 类型声明
```vue
<script setup lang="ts">
// 方式一：使用接口
interface Props {
  title: string
  count?: number
  disabled?: boolean
}

const props = defineProps<Props>()

// 方式二：使用 withDefaults 设置默认值
interface PropsWithDefaults {
  title: string
  count: number
  disabled: boolean
}

const propsWithDefaults = withDefaults(defineProps<PropsWithDefaults>(), {
  count: 0,
  disabled: false
})

// 方式三：运行时声明（不推荐，但兼容性好）
const propsRuntime = defineProps({
  title: {
    type: String,
    required: true
  },
  count: {
    type: Number,
    default: 0
  },
  disabled: Boolean
})
</script>
```

### defineEmits 类型声明
```vue
<script setup lang="ts">
// 方式一：使用类型字面量
const emit = defineEmits<{
  (e: 'change', value: number): void
  (e: 'update:title', title: string): void
  (e: 'submit', data: { name: string; email: string }): void
}>()

// 方式二：使用接口
interface Emits {
  (e: 'change', value: number): void
  (e: 'update:title', title: string): void
  (e: 'submit', data: { name: string; email: string }): void
}

const emit = defineEmits<Emits>()

// 方式三：运行时声明
const emitRuntime = defineEmits(['change', 'update:title', 'submit'])

// 触发事件
function handleClick() {
  emit('change', 42)
  emit('update:title', 'New Title')
  emit('submit', { name: 'Alice', email: 'alice@example.com' })
}
</script>
```

### defineSlots 类型声明
```vue
<script setup lang="ts">
// 定义插槽类型
defineSlots<{
  default?: (props: { item: User; index: number }) => any
  header?: (props: { title: string }) => any
  footer?: (props: { total: number }) => any
}>()

interface User {
  id: number
  name: string
  email: string
}
</script>

<template>
  <div>
    <slot name="header" :title="title" />
    <div v-for="(item, index) in items" :key="item.id">
      <slot :item="item" :index="index" />
    </div>
    <slot name="footer" :total="items.length" />
  </div>
</template>
```

### provide/inject 类型安全
```typescript
// types/theme.ts
import type { InjectionKey } from 'vue'

export interface Theme {
  primary: string
  secondary: string
  isDark: boolean
}

export const themeKey: InjectionKey<Theme> = Symbol('theme')
export const toggleThemeKey: InjectionKey<() => void> = Symbol('toggleTheme')
```

```vue
<!-- App.vue -->
<script setup lang="ts">
import { provide, ref, computed } from 'vue'
import { themeKey, toggleThemeKey, type Theme } from '@/types/theme'

const isDark = ref(false)

const theme = computed<Theme>(() => ({
  primary: isDark.value ? '#42b983' : '#35495e',
  secondary: isDark.value ? '#35495e' : '#42b983',
  isDark: isDark.value
}))

function toggleTheme() {
  isDark.value = !isDark.value
}

provide(themeKey, theme)
provide(toggleThemeKey, toggleTheme)
</script>
```

```vue
<!-- ChildComponent.vue -->
<script setup lang="ts">
import { inject } from 'vue'
import { themeKey, toggleThemeKey } from '@/types/theme'

// 使用默认值防止 undefined
const theme = inject(themeKey)!
const toggleTheme = inject(toggleThemeKey)!
</script>
```

### 组件类型定义
```typescript
// components/MyButton.vue
<script setup lang="ts">
interface Props {
  text: string
  type?: 'primary' | 'secondary' | 'danger'
  disabled?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  type: 'primary',
  disabled: false
})

defineEmits<{
  (e: 'click', event: MouseEvent): void
}>()
</script>

<template>
  <button :class="['btn', `btn-${type}`]" :disabled="disabled" @click="$emit('click', $event)">
    {{ text }}
  </button>
</template>

<style scoped>
.btn { /* ... */ }
.btn-primary { /* ... */ }
.btn-secondary { /* ... */ }
.btn-danger { /* ... */ }
</style>
```

```typescript
// 使用组件时的类型
import type MyButton from '@/components/MyButton.vue'

// 获取组件的 props 类型
type MyButtonProps = InstanceType<typeof MyButton>['$props']

// 获取组件的实例类型
type MyButtonInstance = InstanceType<typeof MyButton>
```

## 4. 官方示例代码（可运行）

### 完整项目示例 - Todo 应用
```typescript
// src/types/todo.ts
export interface Todo {
  id: number
  text: string
  completed: boolean
  createdAt: Date
}

export type TodoFilter = 'all' | 'active' | 'completed'
```

```typescript
// src/stores/todo.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
import type { Todo, TodoFilter } from '@/types/todo'

export const useTodoStore = defineStore('todo', () => {
  const todos = ref<Todo[]>([
    { id: 1, text: 'Learn Vue 3', completed: false, createdAt: new Date() },
    { id: 2, text: 'Learn TypeScript', completed: true, createdAt: new Date() },
    { id: 3, text: 'Learn Vite', completed: false, createdAt: new Date() }
  ])
  
  const filter = ref<TodoFilter>('all')
  
  const filteredTodos = computed(() => {
    switch (filter.value) {
      case 'active':
        return todos.value.filter(todo => !todo.completed)
      case 'completed':
        return todos.value.filter(todo => todo.completed)
      default:
        return todos.value
    }
  })
  
  const activeCount = computed(() => 
    todos.value.filter(todo => !todo.completed).length
  )
  
  const completedCount = computed(() => 
    todos.value.filter(todo => todo.completed).length
  )
  
  function addTodo(text: string) {
    if (text.trim()) {
      todos.value.push({
        id: Date.now(),
        text: text.trim(),
        completed: false,
        createdAt: new Date()
      })
    }
  }
  
  function toggleTodo(id: number) {
    const todo = todos.value.find(t => t.id === id)
    if (todo) {
      todo.completed = !todo.completed
    }
  }
  
  function removeTodo(id: number) {
    const index = todos.value.findIndex(t => t.id === id)
    if (index !== -1) {
      todos.value.splice(index, 1)
    }
  }
  
  function setFilter(newFilter: TodoFilter) {
    filter.value = newFilter
  }
  
  function clearCompleted() {
    todos.value = todos.value.filter(t => !t.completed)
  }
  
  return {
    todos,
    filter,
    filteredTodos,
    activeCount,
    completedCount,
    addTodo,
    toggleTodo,
    removeTodo,
    setFilter,
    clearCompleted
  }
})
```

```vue
<!-- src/components/TodoInput.vue -->
<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits<{
  (e: 'add', text: string): void
}>()

const newTodoText = ref('')

function handleAdd() {
  if (newTodoText.value.trim()) {
    emit('add', newTodoText.value)
    newTodoText.value = ''
  }
}
</script>

<template>
  <div class="todo-input">
    <input
      v-model="newTodoText"
      type="text"
      placeholder="添加新的待办事项..."
      @keyup.enter="handleAdd"
    />
    <button @click="handleAdd">添加</button>
  </div>
</template>

<style scoped>
.todo-input {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.todo-input input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.todo-input button {
  padding: 10px 20px;
  background: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}
</style>
```

```vue
<!-- src/components/TodoList.vue -->
<script setup lang="ts">
import type { Todo, TodoFilter } from '@/types/todo'

interface Props {
  todos: Todo[]
  filter: TodoFilter
  activeCount: number
  completedCount: number
}

const props = defineProps<Props>()

const emit = defineEmits<{
  (e: 'toggle', id: number): void
  (e: 'remove', id: number): void
  (e: 'set-filter', filter: TodoFilter): void
  (e: 'clear-completed'): void
}>()

const filters: { value: TodoFilter; label: string }[] = [
  { value: 'all', label: '全部' },
  { value: 'active', label: '未完成' },
  { value: 'completed', label: '已完成' }
]
</script>

<template>
  <div class="todo-list">
    <div class="filters">
      <button
        v-for="f in filters"
        :key="f.value"
        :class="{ active: filter === f.value }"
        @click="emit('set-filter', f.value)"
      >
        {{ f.label }}
      </button>
    </div>
    
    <ul class="items">
      <li v-for="todo in todos" :key="todo.id" :class="{ completed: todo.completed }">
        <input
          type="checkbox"
          :checked="todo.completed"
          @change="emit('toggle', todo.id)"
        />
        <span>{{ todo.text }}</span>
        <button class="remove" @click="emit('remove', todo.id)">×</button>
      </li>
    </ul>
    
    <div class="footer">
      <span>{{ activeCount }} 项未完成</span>
      <button v-if="completedCount > 0" @click="emit('clear-completed')">
        清除已完成
      </button>
    </div>
  </div>
</template>

<style scoped>
.todo-list {
  border: 1px solid #ddd;
  border-radius: 4px;
}

.filters {
  display: flex;
  gap: 10px;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.filters button {
  padding: 5px 10px;
  border: 1px solid #ddd;
  background: white;
  border-radius: 4px;
  cursor: pointer;
}

.filters button.active {
  background: #42b983;
  color: white;
  border-color: #42b983;
}

.items {
  list-style: none;
  padding: 0;
  margin: 0;
}

.items li {
  display: flex;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.items li.completed span {
  text-decoration: line-through;
  color: #888;
}

.items li input[type="checkbox"] {
  margin-right: 10px;
}

.items li span {
  flex: 1;
}

.items li .remove {
  background: #ff4444;
  color: white;
  border: none;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  padding: 0;
  cursor: pointer;
}

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background: #f5f5f5;
}

.footer button {
  background: none;
  border: none;
  color: #888;
  cursor: pointer;
  text-decoration: underline;
}
</style>
```

```vue
<!-- src/App.vue -->
<script setup lang="ts">
import { storeToRefs } from 'pinia'
import { useTodoStore } from './stores/todo'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'

const todoStore = useTodoStore()
const { todos, filter, filteredTodos, activeCount, completedCount } = storeToRefs(todoStore)
</script>

<template>
  <div class="app">
    <h1>Todo 应用 (Vue 3 + Vite + TypeScript)</h1>
    
    <TodoInput @add="todoStore.addTodo" />
    
    <TodoList
      :todos="filteredTodos"
      :filter="filter"
      :active-count="activeCount"
      :completed-count="completedCount"
      @toggle="todoStore.toggleTodo"
      @remove="todoStore.removeTodo"
      @set-filter="todoStore.setFilter"
      @clear-completed="todoStore.clearCompleted"
    />
  </div>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.app h1 {
  text-align: center;
  color: #42b983;
}
</style>
```

```typescript
// src/main.ts
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const app = createApp(App)
const pinia = createPinia()

app.use(pinia)
app.mount('#app')
```

```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { fileURLToPath, URL } from 'node:url'

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```

```json
{
  "name": "vue-vite-ts-todo",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vue-tsc && vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.4.0",
    "pinia": "^2.1.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.0.0",
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "vue-tsc": "^1.8.0"
  }
}
```

## 5. 官方注意事项 / 最佳实践

### Vite 最佳实践
1. **使用官方模板**：使用 create-vite 创建项目，获得最佳配置
2. **合理使用插件**：只安装必要的插件，避免过度配置
3. **环境变量**：使用 .env 文件管理环境变量
4. **路径别名**：配置 @ 别名简化导入
5. **生产构建优化**：配置 manualChunks 进行代码分割
6. **开发服务器**：合理配置 proxy 解决跨域问题

### TypeScript 最佳实践
1. **启用严格模式**：在 tsconfig.json 中启用 strict: true
2. **类型注解**：为复杂类型添加类型注解，简单类型让 TypeScript 推断
3. **避免 any**：尽量避免使用 any，使用 unknown 代替
4. **类型守卫**：使用类型守卫缩小类型范围
5. **接口定义**：为数据对象定义接口
6. **组件类型**：为组件 props、emits、slots 定义类型

### Vue 3 + TypeScript 最佳实践
1. **使用 script setup**：使用 <script setup lang="ts"> 获得最佳体验
2. **类型安全的 props**：使用 defineProps 的类型声明语法
3. **类型安全的 emits**：使用 defineEmits 的类型声明语法
4. **类型安全的 slots**：使用 defineSlots 定义插槽类型
5. **类型安全的 provide/inject**：使用 InjectionKey 确保类型安全
6. **ref 类型**：为 ref 添加类型注解
7. **reactive 类型**：为 reactive 定义接口
8. **组件实例类型**：使用 InstanceType 获取组件实例类型

### 性能优化
1. **依赖预构建**：利用 Vite 的依赖预构建提升开发体验
2. **代码分割**：使用动态导入进行代码分割
3. **Tree Shaking**：确保代码可以被 Tree Shaking
4. **按需加载**：使用组件库的按需加载功能
5. **类型检查**：将类型检查与构建分离，使用 vue-tsc

## 6. 官方文档参考链接

- Vite 官方文档：https://cn.vitejs.dev/
- Vue 3 TypeScript 文档：https://cn.vuejs.org/guide/typescript/overview.html
- TypeScript 官方文档：https://www.typescriptlang.org/zh/docs/
- create-vite：https://cn.vitejs.dev/guide/#scaffolding-your-first-vite-project
- Vue 3 与 TypeScript：https://cn.vuejs.org/guide/typescript/composition-api.html
