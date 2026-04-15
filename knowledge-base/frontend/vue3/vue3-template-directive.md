# 检索索引：本文档收录【Vue3】【模板与指令】知识点，内容100%来自Vue官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Vue 使用基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定到底层组件实例的数据。所有 Vue 模板都是语法层面合法的 HTML，可以被符合规范的浏览器和 HTML 解析器解析。Vue 会将模板编译成虚拟 DOM 渲染函数，从而高效地更新 DOM。

## 2. 核心知识点（结构化层级）

### 2.1 模板语法
1. 文本插值
2. 原始 HTML
3. Attribute 绑定
4. JavaScript 表达式
5. 缩写语法

### 2.2 指令系统
1. v-bind
2. v-on
3. v-model
4. v-if / v-else-if / v-else
5. v-show
6. v-for
7. v-once
8. v-memo
9. v-cloak
10. v-pre
11. v-html
12. v-text

### 2.3 事件处理
1. 监听事件
2. 事件处理方法
3. 内联事件处理
4. 事件修饰符
5. 按键修饰符
6. 系统修饰符
7. 鼠标按键修饰符

### 2.4 表单输入绑定
1. 文本输入
2. 多行文本
3. 复选框
4. 单选按钮
5. 选择框
6. 值绑定
7. 修饰符

### 2.5 列表渲染
1. v-for 遍历数组
2. v-for 遍历对象
3. v-for 遍历范围
4. 维护状态（key）
5. 数组变化侦测
6. 显示过滤/排序后的结果

### 2.6 条件渲染
1. v-if
2. v-else
3. v-else-if
4. template 上的 v-if
5. v-show
6. v-if vs v-show

## 3. 官方标准语法 / API

### 文本插值与表达式
```vue
<template>
  <!-- 文本插值 -->
  <p>Message: {{ msg }}</p>
  
  <!-- 原生 HTML -->
  <p>Using v-html directive: <span v-html="rawHtml"></span></p>
  
  <!-- Attribute 绑定 -->
  <div v-bind:id="dynamicId"></div>
  <button v-bind:disabled="isButtonDisabled">Button</button>
  
  <!-- 缩写 -->
  <div :id="dynamicId"></div>
  <button :disabled="isButtonDisabled">Button</button>
  
  <!-- JavaScript 表达式 -->
  <p>{{ number + 1 }}</p>
  <p>{{ ok ? 'YES' : 'NO' }}</p>
  <p>{{ message.split('').reverse().join('') }}</p>
  <div :id="'list-' + id"></div>
</template>
```

### 指令语法
```vue
<template>
  <!-- v-bind -->
  <img v-bind:src="imageSrc" alt="image" />
  <img :src="imageSrc" alt="image" />
  <div :class="{ active: isActive, 'text-danger': hasError }"></div>
  <div :class="[isActive ? 'active' : '', errorClass]"></div>
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
  
  <!-- v-on -->
  <button v-on:click="doSomething">Click</button>
  <button @click="doSomething">Click</button>
  <button @click="doSomething($event)">Click</button>
  <form @submit.prevent="onSubmit">Submit</form>
  
  <!-- v-model -->
  <input v-model="message" />
  <textarea v-model="message"></textarea>
  <input type="checkbox" v-model="checked" />
  <input type="radio" v-model="picked" value="one" />
  <select v-model="selected">
    <option value="">请选择</option>
    <option value="a">选项A</option>
  </select>
  
  <!-- v-if / v-else -->
  <p v-if="seen">现在你看到我了</p>
  <p v-else>现在你看不到我</p>
  
  <!-- v-show -->
  <p v-show="seen">现在你看到我了</p>
  
  <!-- v-for -->
  <li v-for="item in items" :key="item.id">{{ item.text }}</li>
  <li v-for="(item, index) in items" :key="item.id">{{ index }} - {{ item.text }}</li>
  <li v-for="(value, key) in object" :key="key">{{ key }}: {{ value }}</li>
  <span v-for="n in 10" :key="n">{{ n }}</span>
  
  <!-- v-once -->
  <span v-once>这个将不会改变: {{ msg }}</span>
  
  <!-- v-memo -->
  <div v-memo="[valueA, valueB]">...</div>
  
  <!-- v-pre -->
  <span v-pre>{{ 这不会被编译 }}</span>
  
  <!-- v-cloak -->
  <div v-cloak>{{ message }}</div>
</template>
```

### 事件修饰符
```vue
<template>
  <!-- 事件修饰符 -->
  <!-- 阻止单击事件继续传播 -->
  <a @click.stop="doThis"></a>
  
  <!-- 提交事件不再重载页面 -->
  <form @submit.prevent="onSubmit"></form>
  
  <!-- 修饰符可以串联 -->
  <a @click.stop.prevent="doThat"></a>
  
  <!-- 只有修饰符 -->
  <form @submit.prevent></form>
  
  <!-- 添加事件监听器时使用事件捕获模式 -->
  <div @click.capture="doThis">...</div>
  
  <!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
  <div @click.self="doThat">...</div>
  
  <!-- 点击事件将只会触发一次 -->
  <a @click.once="doThis"></a>
  
  <!-- 滚动事件的默认行为（即滚动行为）将会立即触发 -->
  <div @scroll.passive="onScroll">...</div>
</template>
```

### 按键修饰符
```vue
<template>
  <!-- 按键修饰符 -->
  <!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
  <input @keyup.enter="submit" />
  
  <!-- 只有在 `key` 是 `PageDown` 时调用 -->
  <input @keyup.page-down="onPageDown" />
  
  <!-- 系统修饰键 -->
  <!-- Alt + Enter -->
  <input @keyup.alt.enter="clear" />
  
  <!-- Ctrl + Click -->
  <div @click.ctrl="doSomething">Do something</div>
  
  <!-- 精确匹配修饰符 -->
  <!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
  <button @click.ctrl="onClick">A</button>
  
  <!-- 有且只有 Ctrl 被按下的时候才触发 -->
  <button @click.ctrl.exact="onCtrlClick">A</button>
  
  <!-- 没有任何系统修饰符被按下的时候才触发 -->
  <button @click.exact="onClick">A</button>
  
  <!-- 鼠标按钮修饰符 -->
  <button @click.left="onClick">Left Click</button>
  <button @click.right="onRightClick">Right Click</button>
  <button @click.middle="onMiddleClick">Middle Click</button>
</template>
```

### v-model 修饰符
```vue
<template>
  <!-- .lazy - 在 "change" 事件后同步 -->
  <input v-model.lazy="msg" />
  
  <!-- .number - 自动将用户的输入值转为数值类型 -->
  <input v-model.number="age" type="number" />
  
  <!-- .trim - 自动过滤用户输入的首尾空白字符 -->
  <input v-model.trim="msg" />
</template>
```

## 4. 官方示例代码（可运行）

### 模板语法与插值示例
```vue
<script setup>
import { ref, computed } from 'vue'

const msg = ref('Hello Vue 3!')
const rawHtml = ref('<span style="color: red">This should be red.</span>')
const dynamicId = ref('my-id')
const isButtonDisabled = ref(false)
const number = ref(10)
const ok = ref(true)
const activeColor = ref('blue')
const fontSize = ref(20)
const isActive = ref(true)
const hasError = ref(false)
const errorClass = ref('text-danger')
</script>

<template>
  <div>
    <h2>模板语法示例</h2>
    
    <div class="section">
      <h3>文本插值</h3>
      <p>Message: {{ msg }}</p>
      <input v-model="msg" placeholder="修改消息" />
    </div>
    
    <div class="section">
      <h3>原始 HTML</h3>
      <p>Using v-text: <span v-text="rawHtml"></span></p>
      <p>Using v-html: <span v-html="rawHtml"></span></p>
    </div>
    
    <div class="section">
      <h3>Attribute 绑定</h3>
      <div :id="dynamicId" :style="{ padding: '10px', border: '1px solid #ccc' }">
        这是一个带有动态 ID 的 div
      </div>
      <p>动态 ID: {{ dynamicId }}</p>
      
      <button :disabled="isButtonDisabled" @click="isButtonDisabled = !isButtonDisabled">
        {{ isButtonDisabled ? '已禁用' : '点击禁用' }}
      </button>
    </div>
    
    <div class="section">
      <h3>JavaScript 表达式</h3>
      <p>Number + 1: {{ number + 1 }}</p>
      <p>Ok ? 'YES' : 'NO': {{ ok ? 'YES' : 'NO' }}</p>
      <p>Reversed message: {{ msg.split('').reverse().join('') }}</p>
      <div :id="'list-' + dynamicId">动态 ID: list-{{ dynamicId }}</div>
    </div>
    
    <div class="section">
      <h3>Class 与 Style 绑定</h3>
      <div 
        :class="{ active: isActive, 'text-danger': hasError }"
        :style="{ color: activeColor, fontSize: fontSize + 'px' }"
      >
        这是一个带有动态类和样式的 div
      </div>
      
      <div :class="[isActive ? 'active' : '', errorClass]">
        数组语法绑定类
      </div>
      
      <button @click="isActive = !isActive">切换 isActive</button>
      <button @click="hasError = !hasError">切换 hasError</button>
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
.active {
  font-weight: bold;
}
.text-danger {
  color: red;
}
button {
  margin: 5px;
  padding: 8px 16px;
}
input {
  padding: 8px;
  width: 100%;
  max-width: 300px;
}
</style>
```

### 事件处理示例
```vue
<script setup>
import { ref } from 'vue'

const counter = ref(0)
const name = ref('')
const message = ref('')
const checkedNames = ref([])
const picked = ref('')
const selected = ref('')

function increment() {
  counter.value++
}

function decrement() {
  counter.value--
}

function greet() {
  alert('Hello!')
}

function warn(message, event) {
  alert(message)
  if (event) {
    console.log(event.target.tagName)
  }
}

function onSubmit() {
  alert('表单已提交')
}
</script>

<template>
  <div>
    <h2>事件处理示例</h2>
    
    <div class="section">
      <h3>基础事件</h3>
      <p>计数器: {{ counter }}</p>
      <button @click="increment">+</button>
      <button @click="decrement">-</button>
      <button @click="greet">问候</button>
    </div>
    
    <div class="section">
      <h3>内联事件处理器</h3>
      <button @click="counter += 1">增加 1</button>
      <p>{{ counter }}</p>
      <button @click="warn('还不能提交', $event)">提交</button>
    </div>
    
    <div class="section">
      <h3>事件修饰符</h3>
      <div @click="alert('外层 div 被点击')">
        <button @click.stop="alert('按钮被点击（事件已停止传播）')">
          点击我（事件停止传播）
        </button>
      </div>
      
      <form @submit.prevent="onSubmit" style="margin-top: 10px;">
        <input type="text" placeholder="输入内容" />
        <button type="submit">提交（preventDefault）</button>
      </form>
      
      <div @click.self="alert('只有点击 div 本身才会触发')" style="padding: 20px; background: #eee; margin-top: 10px;">
        <p>点击这里不会触发</p>
        <button @click="alert('按钮被点击')">点击按钮</button>
      </div>
    </div>
    
    <div class="section">
      <h3>按键修饰符</h3>
      <input 
        v-model="name" 
        placeholder="输入后按 Enter"
        @keyup.enter="alert('你输入了: ' + name)"
      />
      <p>输入: {{ name }}</p>
    </div>
    
    <div class="section">
      <h3>表单输入绑定</h3>
      
      <div style="margin: 10px 0;">
        <input v-model="message" placeholder="编辑我" />
        <p>消息是: {{ message }}</p>
      </div>
      
      <div style="margin: 10px 0;">
        <p>多行文本:</p>
        <textarea v-model="message" placeholder="添加多行文本"></textarea>
      </div>
      
      <div style="margin: 10px 0;">
        <p>复选框:</p>
        <input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />
        <label for="jack">Jack</label>
        <input type="checkbox" id="john" value="John" v-model="checkedNames" />
        <label for="john">John</label>
        <input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
        <label for="mike">Mike</label>
        <p>选中的名字: {{ checkedNames }}</p>
      </div>
      
      <div style="margin: 10px 0;">
        <p>单选按钮:</p>
        <input type="radio" id="one" value="One" v-model="picked" />
        <label for="one">One</label>
        <input type="radio" id="two" value="Two" v-model="picked" />
        <label for="two">Two</label>
        <p>选中: {{ picked }}</p>
      </div>
      
      <div style="margin: 10px 0;">
        <p>选择框:</p>
        <select v-model="selected">
          <option disabled value="">请选择</option>
          <option>A</option>
          <option>B</option>
          <option>C</option>
        </select>
        <p>选中: {{ selected }}</p>
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
button {
  margin: 5px;
  padding: 8px 16px;
}
input, textarea, select {
  padding: 8px;
  margin: 5px 0;
}
</style>
```

### 列表渲染示例
```vue
<script setup>
import { ref, computed } from 'vue'

const items = ref([
  { id: 1, message: 'Foo', completed: false },
  { id: 2, message: 'Bar', completed: true },
  { id: 3, message: 'Baz', completed: false },
  { id: 4, message: 'Qux', completed: true }
])

const object = ref({
  title: 'How to do lists in Vue',
  author: 'Jane Doe',
  publishedAt: '2020-03-22'
})

const newItemMessage = ref('')
const showCompleted = ref(false)

const filteredItems = computed(() => {
  return showCompleted.value 
    ? items.value.filter(item => item.completed)
    : items.value
})

function addItem() {
  if (newItemMessage.value.trim()) {
    items.value.push({
      id: Date.now(),
      message: newItemMessage.value,
      completed: false
    })
    newItemMessage.value = ''
  }
}

function removeItem(id) {
  const index = items.value.findIndex(item => item.id === id)
  if (index !== -1) {
    items.value.splice(index, 1)
  }
}

function toggleComplete(item) {
  item.completed = !item.completed
}
</script>

<template>
  <div>
    <h2>列表渲染示例</h2>
    
    <div class="section">
      <h3>v-for 遍历数组</h3>
      <ul>
        <li v-for="item in items" :key="item.id">
          {{ item.message }}
        </li>
      </ul>
    </div>
    
    <div class="section">
      <h3>v-for 带索引</h3>
      <ul>
        <li v-for="(item, index) in items" :key="item.id">
          {{ index }} - {{ item.message }}
        </li>
      </ul>
    </div>
    
    <div class="section">
      <h3>v-for 遍历对象</h3>
      <ul>
        <li v-for="(value, key) in object" :key="key">
          {{ key }}: {{ value }}
        </li>
      </ul>
    </div>
    
    <div class="section">
      <h3>v-for 遍历范围</h3>
      <span v-for="n in 10" :key="n" style="margin: 0 5px;">{{ n }}</span>
    </div>
    
    <div class="section">
      <h3>待办事项列表</h3>
      
      <div style="margin-bottom: 10px;">
        <input 
          v-model="newItemMessage" 
          placeholder="添加新事项"
          @keyup.enter="addItem"
        />
        <button @click="addItem">添加</button>
      </div>
      
      <div style="margin-bottom: 10px;">
        <label>
          <input type="checkbox" v-model="showCompleted" />
          只显示已完成
        </label>
      </div>
      
      <ul class="todo-list">
        <li v-for="item in filteredItems" :key="item.id" class="todo-item">
          <input 
            type="checkbox" 
            :checked="item.completed" 
            @change="toggleComplete(item)"
          />
          <span :class="{ completed: item.completed }">{{ item.message }}</span>
          <button @click="removeItem(item.id)" class="remove-btn">×</button>
        </li>
      </ul>
      
      <p>总计: {{ items.length }} 项</p>
    </div>
    
    <div class="section">
      <h3>template 上的 v-for</h3>
      <ul>
        <template v-for="item in items" :key="item.id">
          <li>{{ item.message }}</li>
          <li class="divider" v-if="!item.completed">---</li>
        </template>
      </ul>
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
button {
  margin: 5px;
  padding: 8px 16px;
}
input {
  padding: 8px;
  margin: 5px 0;
}
.todo-list {
  list-style: none;
  padding: 0;
}
.todo-item {
  display: flex;
  align-items: center;
  padding: 10px;
  border: 1px solid #ddd;
  margin: 5px 0;
  border-radius: 4px;
}
.todo-item span {
  flex: 1;
  margin-left: 10px;
}
.completed {
  text-decoration: line-through;
  color: #888;
}
.remove-btn {
  background: #ff4444;
  color: white;
  border: none;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
.divider {
  border: none;
  color: #ccc;
}
</style>
```

## 5. 官方注意事项 / 最佳实践

### 模板语法注意事项
1. **表达式限制**：模板中只能包含单个 JavaScript 表达式，不能包含语句或流控制
2. **原始 HTML**：使用 v-html 时要小心，避免 XSS 攻击
3. **简洁性**：保持模板简洁，复杂逻辑应移到计算属性或方法中

### 指令使用注意事项
1. **v-for 必须有 key**：始终为 v-for 提供唯一的 key 属性
2. **避免 v-if 和 v-for 一起使用**：如果确实需要，应该先使用计算属性过滤
3. **v-if vs v-show**：v-if 有更高的切换开销，v-show 有更高的初始渲染开销
4. **事件修饰符顺序**：使用修饰符时，顺序很重要，相应的代码会以同样的顺序产生

### 性能优化
1. **合理使用 v-once**：对于不需要更新的内容使用 v-once
2. **v-memo 优化**：对于大型列表使用 v-memo 优化性能
3. **避免在模板中使用复杂表达式**：使用计算属性替代
4. **合理使用 key**：确保 key 稳定且唯一，不要使用 index 作为 key

### 最佳实践总结
1. 保持模板简洁和可读
2. 复杂逻辑使用计算属性
3. 始终为 v-for 提供 key
4. 避免在 v-for 中使用 v-if
5. 合理使用事件修饰符
6. 使用 v-model 简化表单处理
7. 遵循 Vue 官方风格指南

## 6. 官方文档参考链接

- Vue 3 模板语法：https://cn.vuejs.org/guide/essentials/template-syntax.html
- 事件处理：https://cn.vuejs.org/guide/essentials/event-handling.html
- 表单输入绑定：https://cn.vuejs.org/guide/essentials/forms.html
- 列表渲染：https://cn.vuejs.org/guide/essentials/list.html
- 条件渲染：https://cn.vuejs.org/guide/essentials/conditional.html
- 内置指令：https://cn.vuejs.org/api/built-in-directives.html
