# 检索索引：本文档收录【Vue3】【组件API】知识点，内容100%来自Vue官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

组件是 Vue 应用的基本构建块。Vue 3 提供了强大的组件系统，支持组合式 API（Composition API）和选项式 API（Options API）两种编写风格。组件可以封装可重用的代码和模板，具有自己的状态、方法和生命周期。

## 2. 核心知识点（结构化层级）

### 2.1 组件定义
1. SFC（单文件组件）结构
2. 组合式 API（&lt;script setup&gt;）
3. 选项式 API
4. 函数式组件
5. 动态组件

### 2.2 Props
1. Props 声明
2. Props 验证
3. 单向数据流
4. Props 透传
5. defineProps() 宏

### 2.3 Events
1. 事件触发与监听
2. 事件验证
3. 自定义事件
4. v-model 实现
5. defineEmits() 宏

### 2.4 插槽（Slots）
1. 默认插槽
2. 具名插槽
3. 作用域插槽
4. 动态插槽名
5. defineSlots() 宏

### 2.5 生命周期钩子
1. 创建阶段：setup()
2. 挂载阶段：onBeforeMount, onMounted
3. 更新阶段：onBeforeUpdate, onUpdated
4. 卸载阶段：onBeforeUnmount, onUnmounted
5. 其他钩子：onActivated, onDeactivated, onErrorCaptured

### 2.6 组件通信
1. Props/Events
2. 依赖注入（Provide/Inject）
3. 事件总线
4. 全局状态管理
5. 组件引用（Refs）

### 2.7 组件进阶
1. 递归组件
2. 异步组件
3. 组件缓存（keep-alive）
4. 组件 v-model
5. 函数组件
6. Teleport 传送门
7. Suspense  suspense

## 3. 官方标准语法 / API

### 组件定义语法
```vue
<script setup>
// 组合式 API - script setup 语法
import { ref, computed } from 'vue'

// 响应式状态
const count = ref(0)

// 计算属性
const doubled = computed(() => count.value * 2)

// 方法
function increment() {
  count.value++
}
</script>

<template>
  <div>
    <p>Count: {{ count }}</p>
    <p>Doubled: {{ doubled }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<style scoped>
/* 组件样式 */
</style>
```

### Props 语法
```vue
<script setup>
// 声明 props
const props = defineProps({
  // 基础类型
  title: String,
  // 多个可能的类型
  likes: [Number, String],
  // 必填的
  author: {
    type: String,
    required: true
  },
  // 带有默认值
  publishedDate: {
    type: Date,
    default: () => new Date()
  },
  // 自定义验证
  rating: {
    type: Number,
    validator(value) {
      return value >= 0 && value <= 5
    }
  }
})

// 使用 TypeScript
interface Props {
  title: string
  likes?: number
  author: string
}

const props = defineProps<Props>()

// 带默认值的 TypeScript Props
withDefaults(defineProps<Props>(), {
  likes: 0
})
</script>
```

### Events 语法
```vue
<script setup>
// 声明 emits
const emit = defineEmits(['update:title', 'submit', 'delete'])

// 带验证的 emits
const emit = defineEmits({
  'update:title': (newTitle: string) => {
    return newTitle.length > 0
  },
  submit: null,
  delete: (id: number) => {
    return id > 0
  }
})

// TypeScript 语法
const emit = defineEmits<{
  (e: 'update:title', value: string): void
  (e: 'submit', data: object): void
  (e: 'delete', id: number): void
}>()

// 触发事件
function updateTitle(newTitle) {
  emit('update:title', newTitle)
}

function handleSubmit() {
  emit('submit', { data: 'value' })
}
</script>
```

### 插槽语法
```vue
<!-- 父组件 -->
<template>
  <MyComponent>
    <!-- 默认插槽 -->
    <p>这是默认插槽内容</p>
    
    <!-- 具名插槽 -->
    <template #header>
      <h1>这是头部插槽</h1>
    </template>
    
    <!-- 作用域插槽 -->
    <template #default="{ item, index }">
      <p>{{ index }}: {{ item.name }}</p>
    </template>
    
    <!-- 动态插槽名 -->
    <template #[dynamicSlotName]>
      动态插槽内容
    </template>
  </MyComponent>
</template>

<!-- 子组件 -->
<script setup>
// TypeScript 插槽声明
defineSlots<{
  default?: (props: { item: any; index: number }) => any
  header?: () => any
  footer?: (props: { total: number }) => any
}>()
</script>

<template>
  <div>
    <!-- 默认插槽 -->
    <slot></slot>
    
    <!-- 具名插槽 -->
    <slot name="header"></slot>
    
    <!-- 带默认内容的插槽 -->
    <slot name="footer">
      <p>这是默认的页脚</p>
    </slot>
    
    <!-- 作用域插槽 -->
    <div v-for="(item, index) in items" :key="index">
      <slot :item="item" :index="index"></slot>
    </div>
  </div>
</template>
```

### 生命周期钩子语法
```vue
<script setup>
import { 
  onBeforeMount, 
  onMounted, 
  onBeforeUpdate, 
  onUpdated, 
  onBeforeUnmount, 
  onUnmounted,
  onActivated,
  onDeactivated,
  onErrorCaptured
} from 'vue'

// 挂载前
onBeforeMount(() => {
  console.log('组件即将挂载')
})

// 挂载后
onMounted(() => {
  console.log('组件已挂载')
  // 可以访问 DOM
})

// 更新前
onBeforeUpdate(() => {
  console.log('组件即将更新')
})

// 更新后
onUpdated(() => {
  console.log('组件已更新')
  // DOM 已更新
})

// 卸载前
onBeforeUnmount(() => {
  console.log('组件即将卸载')
  // 清理工作
})

// 卸载后
onUnmounted(() => {
  console.log('组件已卸载')
})

// keep-alive 激活
onActivated(() => {
  console.log('组件被激活')
})

// keep-alive 停用
onDeactivated(() => {
  console.log('组件被停用')
})

// 错误捕获
onErrorCaptured((error, instance, info) => {
  console.error('捕获到错误:', error)
  return false // 阻止错误继续传播
})
</script>
```

### Provide/Inject 语法
```vue
<!-- 父组件 -->
<script setup>
import { provide, ref, reactive } from 'vue'

// 提供响应式数据
const theme = ref('light')
const user = reactive({ name: 'John', age: 30 })

// 提供静态数据
provide('appName', 'My App')

// 提供响应式数据
provide('theme', theme)
provide('user', user)

// 提供方法
provide('updateTheme', (newTheme) => {
  theme.value = newTheme
})
</script>

<!-- 后代组件 -->
<script setup>
import { inject } from 'vue'

// 注入数据
const appName = inject('appName')
const theme = inject('theme')
const user = inject('user')
const updateTheme = inject('updateTheme')

// 带默认值的注入
const fallbackValue = inject('key', 'default value')

// 使用工厂函数创建默认值
const expensiveDefault = inject('key', () => {
  return computeExpensiveValue()
})
</script>
```

## 4. 官方示例代码（可运行）

### 基础组件示例
```vue
<!-- Button.vue -->
<script setup>
defineProps({
  variant: {
    type: String,
    default: 'primary',
    validator: (value) => ['primary', 'secondary', 'danger'].includes(value)
  },
  size: {
    type: String,
    default: 'medium',
    validator: (value) => ['small', 'medium', 'large'].includes(value)
  },
  disabled: {
    type: Boolean,
    default: false
  }
})

defineEmits(['click'])
</script>

<template>
  <button 
    :class="['btn', `btn-${variant}`, `btn-${size}`]"
    :disabled="disabled"
    @click="$emit('click', $event)"
  >
    <slot></slot>
  </button>
</template>

<style scoped>
.btn {
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s;
}

.btn-primary {
  background-color: #4CAF50;
  color: white;
}

.btn-secondary {
  background-color: #9E9E9E;
  color: white;
}

.btn-danger {
  background-color: #f44336;
  color: white;
}

.btn-small {
  padding: 4px 8px;
  font-size: 12px;
}

.btn-medium {
  padding: 8px 16px;
  font-size: 14px;
}

.btn-large {
  padding: 12px 24px;
  font-size: 16px;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>

<!-- 使用示例 -->
<script setup>
import { ref } from 'vue'
import Button from './Button.vue'

const count = ref(0)

function handleClick() {
  count.value++
}
</script>

<template>
  <div>
    <h2>按钮组件示例</h2>
    <p>计数: {{ count }}</p>
    
    <div style="margin: 20px 0; display: flex; gap: 10px; flex-wrap: wrap;">
      <Button @click="handleClick">主要按钮</Button>
      <Button variant="secondary">次要按钮</Button>
      <Button variant="danger">危险按钮</Button>
      <Button size="small">小按钮</Button>
      <Button size="large">大按钮</Button>
      <Button disabled>禁用按钮</Button>
    </div>
  </div>
</template>
```

### 表单组件示例
```vue
<!-- FormInput.vue -->
<script setup>
const props = defineProps({
  modelValue: {
    type: [String, Number],
    default: ''
  },
  label: {
    type: String,
    required: true
  },
  type: {
    type: String,
    default: 'text'
  },
  placeholder: {
    type: String,
    default: ''
  },
  error: {
    type: String,
    default: ''
  },
  required: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['update:modelValue'])

function handleInput(event) {
  emit('update:modelValue', event.target.value)
}
</script>

<template>
  <div class="form-input">
    <label :for="label">
      {{ label }}
      <span v-if="required" class="required">*</span>
    </label>
    <input
      :type="type"
      :id="label"
      :value="modelValue"
      :placeholder="placeholder"
      :class="{ 'input-error': error }"
      @input="handleInput"
    />
    <p v-if="error" class="error-message">{{ error }}</p>
  </div>
</template>

<style scoped>
.form-input {
  margin-bottom: 16px;
}

label {
  display: block;
  margin-bottom: 4px;
  font-weight: 500;
}

.required {
  color: #f44336;
  margin-left: 2px;
}

input {
  width: 100%;
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  box-sizing: border-box;
}

.input-error {
  border-color: #f44336;
}

.error-message {
  color: #f44336;
  font-size: 12px;
  margin: 4px 0 0 0;
}
</style>

<!-- 使用示例 -->
<script setup>
import { ref, reactive } from 'vue'
import FormInput from './FormInput.vue'

const form = reactive({
  name: '',
  email: '',
  password: ''
})

const errors = reactive({
  name: '',
  email: '',
  password: ''
})

function validateForm() {
  let isValid = true
  
  if (!form.name.trim()) {
    errors.name = '请输入姓名'
    isValid = false
  } else {
    errors.name = ''
  }
  
  if (!form.email.trim()) {
    errors.email = '请输入邮箱'
    isValid = false
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) {
    errors.email = '请输入有效的邮箱地址'
    isValid = false
  } else {
    errors.email = ''
  }
  
  if (!form.password) {
    errors.password = '请输入密码'
    isValid = false
  } else if (form.password.length < 6) {
    errors.password = '密码至少6位'
    isValid = false
  } else {
    errors.password = ''
  }
  
  return isValid
}

function handleSubmit() {
  if (validateForm()) {
    alert('表单提交成功！\n' + JSON.stringify(form, null, 2))
  }
}
</script>

<template>
  <div style="max-width: 400px; margin: 0 auto; padding: 20px;">
    <h2>表单组件示例</h2>
    
    <FormInput
      v-model="form.name"
      label="姓名"
      placeholder="请输入姓名"
      :error="errors.name"
      :required="true"
    />
    
    <FormInput
      v-model="form.email"
      label="邮箱"
      type="email"
      placeholder="请输入邮箱"
      :error="errors.email"
      :required="true"
    />
    
    <FormInput
      v-model="form.password"
      label="密码"
      type="password"
      placeholder="请输入密码"
      :error="errors.password"
      :required="true"
    />
    
    <button 
      @click="handleSubmit"
      style="width: 100%; padding: 12px; background: #4CAF50; color: white; border: none; border-radius: 4px; cursor: pointer; font-size: 16px;"
    >
      提交
    </button>
  </div>
</template>
```

### 组件通信示例
```vue
<!-- 主题上下文 -->
<!-- ThemeProvider.vue -->
<script setup>
import { provide, ref, computed } from 'vue'

const isDark = ref(false)

const theme = computed(() => isDark.value ? 'dark' : 'light')

function toggleTheme() {
  isDark.value = !isDark.value
}

provide('theme', theme)
provide('isDark', isDark)
provide('toggleTheme', toggleTheme)
</script>

<template>
  <div :class="['theme-provider', theme]">
    <slot></slot>
  </div>
</template>

<style>
.theme-provider {
  min-height: 100vh;
  padding: 20px;
  transition: all 0.3s;
}

.theme-provider.light {
  background-color: #ffffff;
  color: #333333;
}

.theme-provider.dark {
  background-color: #333333;
  color: #ffffff;
}
</style>

<!-- 主题切换按钮 -->
<!-- ThemeToggle.vue -->
<script setup>
import { inject } from 'vue'

const isDark = inject('isDark')
const toggleTheme = inject('toggleTheme')
</script>

<template>
  <button @click="toggleTheme" class="theme-toggle">
    {{ isDark ? '切换到浅色' : '切换到深色' }}
  </button>
</template>

<style scoped>
.theme-toggle {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  background-color: #4CAF50;
  color: white;
}
</style>

<!-- 卡片组件 -->
<!-- Card.vue -->
<script setup>
const props = defineProps({
  title: String
})
</script>

<template>
  <div class="card">
    <h3 v-if="title">{{ title }}</h3>
    <slot></slot>
  </div>
</template>

<style scoped>
.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
}
</style>

<!-- 主应用 -->
<script setup>
import ThemeProvider from './ThemeProvider.vue'
import ThemeToggle from './ThemeToggle.vue'
import Card from './Card.vue'
</script>

<template>
  <ThemeProvider>
    <h1>组件通信示例 (Provide/Inject)</h1>
    <ThemeToggle />
    
    <Card title="卡片 1">
      <p>这是卡片 1 的内容</p>
    </Card>
    
    <Card title="卡片 2">
      <p>这是卡片 2 的内容</p>
    </Card>
  </ThemeProvider>
</template>
```

## 5. 官方注意事项 / 最佳实践

### 组件设计原则
1. **单一职责**：每个组件应该只负责一个功能
2. **可重用性**：设计组件时考虑未来的复用场景
3. **可维护性**：组件应该易于理解和修改
4. **Props 向下传递，Events 向上传递**：遵循单向数据流

### Props 和 Events 最佳实践
1. **Props 验证**：始终为 props 提供类型和验证
2. **避免修改 Props**：遵循单向数据流，不要直接修改 props
3. **事件命名**：使用 kebab-case 命名事件
4. **v-model**：为需要双向绑定的组件实现 v-model

### 插槽最佳实践
1. **提供默认内容**：为插槽提供合理的默认内容
2. **作用域插槽**：使用作用域插槽提高组件灵活性
3. **具名插槽**：对于复杂布局使用具名插槽

### 性能优化
1. **合理使用 v-if 和 v-show**：v-if 适用于不频繁切换，v-show 适用于频繁切换
2. **避免在 v-for 中使用 v-if**：可以使用计算属性先过滤数组
3. **使用 key**：为 v-for 提供唯一的 key
4. **组件懒加载**：使用 defineAsyncComponent 异步加载组件
5. **合理使用 keep-alive**：缓存需要保留状态的组件

### 最佳实践总结
1. 使用 &lt;script setup&gt; 语法糖
2. 充分利用 TypeScript 类型系统
3. 组件命名遵循 PascalCase
4. 文件命名遵循 PascalCase 或 kebab-case
5. 合理拆分组件，保持组件简洁
6. 编写可测试的组件
7. 遵循 Vue 官方风格指南

## 6. 官方文档参考链接

- Vue 3 组件基础：https://cn.vuejs.org/guide/essentials/component-basics.html
- 组件深入：https://cn.vuejs.org/guide/essentials/component-basics.html
- Props：https://cn.vuejs.org/guide/components/props.html
- Events：https://cn.vuejs.org/guide/components/events.html
- 插槽：https://cn.vuejs.org/guide/components/slots.html
- 生命周期钩子：https://cn.vuejs.org/guide/essentials/lifecycle.html
- 依赖注入：https://cn.vuejs.org/guide/components/provide-inject.html
