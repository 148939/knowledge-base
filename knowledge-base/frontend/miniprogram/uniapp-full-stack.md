# 检索索引：本文档收录【UniApp】【全栈开发】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

UniApp 是一个使用 Vue.js 开发所有前端应用的框架，开发者编写一套代码，可发布到 iOS、Android、H5、以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉/淘宝）、快应用等多个平台。UniApp 采用 Vue.js 语法，结合微信小程序的组件 API，实现了真正的"一次编写，多端运行"。

## 2. 核心知识点（结构化层级）

### 2.1 项目结构

- `pages.json`：页面路由配置
- `manifest.json`：应用配置文件
- `App.vue`：应用入口文件
- `main.js`：主入口文件
- `pages/`：页面目录
- `static/`：静态资源目录
- `components/`：组件目录
- `uni_modules/`：uni-app 插件目录

### 2.2 页面与路由

- 页面生命周期：onLoad、onShow、onReady、onHide、onUnload
- 应用生命周期：onLaunch、onShow、onHide
- 页面跳转：uni.navigateTo、uni.redirectTo、uni.switchTab、uni.reLaunch
- 路由参数传递与接收

### 2.3 组件系统

- 基础组件：view、text、image、button、input
- 表单组件：form、picker、switch、checkbox、radio
- 导航组件：navigator
- 媒体组件：video、audio、camera
- 地图组件：map
- 自定义组件：组件通信、插槽、生命周期

### 2.4 数据绑定与事件处理

- 数据绑定：{{ }} 插值、v-bind 属性绑定
- 事件处理：v-on 事件绑定、事件修饰符
- 列表渲染：v-for、:key
- 条件渲染：v-if、v-else-if、v-else、v-show

### 2.5 API 与平台兼容性

- 网络请求：uni.request
- 数据存储：uni.setStorage、uni.getStorage
- 界面交互：uni.showToast、uni.showModal、uni.showLoading
- 设备相关：uni.getSystemInfo、uni.chooseImage
- 条件编译：#ifdef、#ifndef、#endif

## 3. 官方标准语法 / API

```vue
<!-- 页面结构示例 -->
<template>
  <view class="container">
    <!-- 数据绑定 -->
    <text>{{ message }}</text>
    
    <!-- 属性绑定 -->
    <image :src="imageUrl" mode="aspectFill"></image>
    
    <!-- 事件绑定 -->
    <button @click="handleClick">点击我</button>
    
    <!-- 列表渲染 -->
    <view v-for="(item, index) in list" :key="index">
      {{ index }}: {{ item.name }}
    </view>
    
    <!-- 条件渲染 -->
    <view v-if="isVisible">显示内容</view>
    <view v-else>隐藏内容</view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello UniApp',
      imageUrl: '/static/logo.png',
      list: [
        { name: '张三' },
        { name: '李四' },
        { name: '王五' }
      ],
      isVisible: true
    }
  },
  onLoad(options) {
    console.log('页面加载', options)
  },
  onShow() {
    console.log('页面显示')
  },
  methods: {
    handleClick() {
      uni.showToast({
        title: '点击成功',
        icon: 'success'
      })
    }
  }
}
</script>

<style>
.container {
  padding: 20rpx;
}
</style>
```

```javascript
// 网络请求示例
uni.request({
  url: 'https://api.example.com/data',
  method: 'GET',
  data: {
    id: 1
  },
  success: (res) => {
    console.log('请求成功', res.data)
  },
  fail: (err) => {
    console.error('请求失败', err)
  }
})

// 数据存储示例
uni.setStorage({
  key: 'userInfo',
  data: {
    name: '张三',
    age: 25
  },
  success: () => {
    console.log('存储成功')
  }
})

uni.getStorage({
  key: 'userInfo',
  success: (res) => {
    console.log('获取成功', res.data)
  }
})

// 页面跳转示例
uni.navigateTo({
  url: '/pages/detail/detail?id=123'
})

// 条件编译示例
// #ifdef MP-WEIXIN
console.log('仅在微信小程序中执行')
// #endif

// #ifdef H5
console.log('仅在H5中执行')
// #endif
```

## 4. 官方示例代码（可运行）

```vue
<!-- pages/index/index.vue - 首页示例 -->
<template>
  <view class="container">
    <view class="header">
      <image class="logo" src="/static/logo.png"></image>
      <text class="title">{{ title }}</text>
    </view>
    
    <view class="content">
      <view class="section">
        <text class="section-title">计数器</text>
        <view class="counter">
          <button @click="decrement">-</button>
          <text class="count">{{ count }}</text>
          <button @click="increment">+</button>
        </view>
      </view>
      
      <view class="section">
        <text class="section-title">待办事项</text>
        <view class="todo-input">
          <input v-model="newTodo" placeholder="请输入待办事项" />
          <button @click="addTodo">添加</button>
        </view>
        <view class="todo-list">
          <view 
            v-for="(todo, index) in todos" 
            :key="index" 
            class="todo-item"
            @click="toggleTodo(index)"
          >
            <text :class="{ completed: todo.completed }">{{ todo.text }}</text>
            <button @click.stop="deleteTodo(index)">删除</button>
          </view>
        </view>
      </view>
      
      <view class="section">
        <text class="section-title">网络请求</text>
        <button @click="fetchData">获取数据</button>
        <view v-if="loading" class="loading">加载中...</view>
        <view v-else-if="data" class="data">
          <text>{{ data }}</text>
        </view>
      </view>
      
      <view class="section">
        <text class="section-title">页面跳转</text>
        <button @click="goToDetail">跳转到详情页</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      title: 'UniApp 示例',
      count: 0,
      newTodo: '',
      todos: [],
      loading: false,
      data: null
    }
  },
  onLoad() {
    this.loadTodos()
  },
  methods: {
    increment() {
      this.count++
    },
    decrement() {
      this.count--
    },
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({
          text: this.newTodo,
          completed: false
        })
        this.newTodo = ''
        this.saveTodos()
      }
    },
    toggleTodo(index) {
      this.todos[index].completed = !this.todos[index].completed
      this.saveTodos()
    },
    deleteTodo(index) {
      this.todos.splice(index, 1)
      this.saveTodos()
    },
    saveTodos() {
      uni.setStorageSync('todos', this.todos)
    },
    loadTodos() {
      const saved = uni.getStorageSync('todos')
      if (saved) {
        this.todos = saved
      }
    },
    fetchData() {
      this.loading = true
      uni.request({
        url: 'https://jsonplaceholder.typicode.com/posts/1',
        method: 'GET',
        success: (res) => {
          this.data = res.data.title
        },
        fail: (err) => {
          uni.showToast({
            title: '请求失败',
            icon: 'none'
          })
        },
        complete: () => {
          this.loading = false
        }
      })
    },
    goToDetail() {
      uni.navigateTo({
        url: '/pages/detail/detail?id=' + this.count
      })
    }
  }
}
</script>

<style>
.container {
  padding: 20rpx;
}

.header {
  text-align: center;
  margin-bottom: 40rpx;
}

.logo {
  width: 200rpx;
  height: 200rpx;
  margin-bottom: 20rpx;
}

.title {
  font-size: 36rpx;
  font-weight: bold;
}

.content {
  display: flex;
  flex-direction: column;
  gap: 40rpx;
}

.section {
  background-color: #f5f5f5;
  padding: 30rpx;
  border-radius: 10rpx;
}

.section-title {
  font-size: 32rpx;
  font-weight: bold;
  margin-bottom: 20rpx;
  display: block;
}

.counter {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 30rpx;
}

.count {
  font-size: 48rpx;
  font-weight: bold;
  min-width: 100rpx;
  text-align: center;
}

.todo-input {
  display: flex;
  gap: 20rpx;
  margin-bottom: 20rpx;
}

.todo-input input {
  flex: 1;
  border: 1px solid #ddd;
  padding: 10rpx 20rpx;
  border-radius: 5rpx;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20rpx 0;
  border-bottom: 1px solid #ddd;
}

.todo-item:last-child {
  border-bottom: none;
}

.completed {
  text-decoration: line-through;
  color: #999;
}

.loading, .data {
  margin-top: 20rpx;
  text-align: center;
}
</style>
```

```json
// pages.json - 页面配置
{
  "pages": [
    {
      "path": "pages/index/index",
      "style": {
        "navigationBarTitleText": "首页"
      }
    },
    {
      "path": "pages/detail/detail",
      "style": {
        "navigationBarTitleText": "详情页"
      }
    }
  ],
  "globalStyle": {
    "navigationBarTextStyle": "black",
    "navigationBarTitleText": "UniApp",
    "navigationBarBackgroundColor": "#F8F8F8",
    "backgroundColor": "#F8F8F8"
  },
  "tabBar": {
    "color": "#7A7E83",
    "selectedColor": "#3cc51f",
    "borderStyle": "black",
    "backgroundColor": "#ffffff",
    "list": [
      {
        "pagePath": "pages/index/index",
        "iconPath": "static/tab-home.png",
        "selectedIconPath": "static/tab-home-active.png",
        "text": "首页"
      }
    ]
  }
}
```

## 5. 官方注意事项 / 最佳实践

1. **性能优化**
   - 合理使用数据更新，避免频繁 setData
   - 图片资源使用合适的尺寸和格式
   - 使用分包加载减少主包体积
   - 合理使用 computed 和 watch

2. **跨平台兼容**
   - 使用条件编译处理平台差异
   - 避免使用平台特定 API
   - 测试各平台的表现

3. **代码规范**
   - 遵循 Vue.js 风格指南
   - 组件化开发，提高复用性
   - 合理使用 uni_modules 插件

4. **用户体验**
   - 合理使用加载状态和错误提示
   - 优化页面切换动画
   - 提供良好的错误处理

## 6. 官方文档参考链接

- 官方文档：https://uniapp.dcloud.net.cn/
- 组件文档：https://uniapp.dcloud.net.cn/component/
- API 文档：https://uniapp.dcloud.net.cn/api/
- 插件市场：https://ext.dcloud.net.cn/
