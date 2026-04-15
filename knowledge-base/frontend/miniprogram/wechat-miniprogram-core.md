# 检索索引：本文档收录【微信小程序】【核心基础】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

微信小程序是一种不需要下载安装即可使用的应用，它实现了应用"触手可及"的梦想，用户扫一扫或搜一下即可打开应用。小程序在微信客户端内部运行，具有跨平台、轻量级、即用即走等特点。

## 2. 核心知识点（结构化层级）

### 2.1 小程序目录结构

小程序的基本目录结构包含以下文件：
- `app.js`：小程序逻辑
- `app.json`：小程序公共配置
- `app.wxss`：小程序公共样式表
- `pages/`：页面目录
- `utils/`：工具函数目录

### 2.2 页面文件结构

每个页面由四个文件组成：
- `.js`：页面逻辑
- `.wxml`：页面结构
- `.wxss`：页面样式
- `.json`：页面配置

### 2.3 数据绑定与事件处理

使用双大括号 `{{ }}` 进行数据绑定，使用 `bindtap`、`catchtap` 等绑定事件。

### 2.4 生命周期

- 应用生命周期：`onLaunch`、`onShow`、`onHide`、`onError`
- 页面生命周期：`onLoad`、`onShow`、`onReady`、`onHide`、`onUnload`

### 2.5 组件系统

微信小程序提供了丰富的基础组件，包括视图容器、基础内容、表单组件、导航组件、媒体组件等。

## 3. 官方标准语法 / API

```javascript
// 注册小程序
App({
  onLaunch: function() {
    console.log('小程序启动');
  },
  globalData: {
    userInfo: null
  }
});

// 注册页面
Page({
  data: {
    message: 'Hello World',
    count: 0
  },
  onLoad: function(options) {
    console.log('页面加载', options);
  },
  handleTap: function() {
    this.setData({
      count: this.data.count + 1
    });
  }
});

// 数据绑定示例 (WXML)
<view>{{ message }}</view>
<button bindtap="handleTap">点击次数: {{ count }}</button>

// 条件渲染
<view wx:if="{{ condition }}">显示内容</view>
<view wx:else>其他内容</view>

// 列表渲染
<view wx:for="{{ items }}" wx:key="id">
  {{ index }}: {{ item.name }}
</view>
```

## 4. 官方示例代码（可运行）

```javascript
// app.js - 小程序入口
App({
  onLaunch() {
    console.log('小程序初始化完成');
    this.checkLoginStatus();
  },

  checkLoginStatus() {
    const token = wx.getStorageSync('token');
    if (token) {
      this.getUserInfo();
    }
  },

  getUserInfo() {
    wx.request({
      url: 'https://api.example.com/user/info',
      method: 'GET',
      header: {
        'Authorization': 'Bearer ' + wx.getStorageSync('token')
      },
      success: (res) => {
        this.globalData.userInfo = res.data;
      }
    });
  },

  globalData: {
    userInfo: null,
    baseUrl: 'https://api.example.com'
  }
});

// pages/index/index.js - 首页逻辑
Page({
  data: {
    motto: 'Hello World',
    userInfo: {},
    hasUserInfo: false,
    canIUse: wx.canIUse('button.open-type.getUserInfo')
  },

  onLoad() {
    if (app.globalData.userInfo) {
      this.setData({
        userInfo: app.globalData.userInfo,
        hasUserInfo: true
      });
    }
  },

  getUserInfo(e) {
    console.log(e);
    this.setData({
      userInfo: e.detail.userInfo,
      hasUserInfo: true
    });
  },

  navigateToDetail() {
    wx.navigateTo({
      url: '/pages/detail/detail?id=123'
    });
  }
});

// pages/index/index.wxml - 首页结构
<view class="container">
  <view class="userinfo">
    <button wx:if="{{!hasUserInfo && canIUse}}" open-type="getUserInfo" bindgetuserinfo="getUserInfo">获取头像昵称</button>
    <block wx:else>
      <image class="userinfo-avatar" src="{{userInfo.avatarUrl}}" mode="cover"></image>
      <text class="userinfo-nickname">{{userInfo.nickName}}</text>
    </block>
  </view>
  
  <view class="usermotto">
    <text class="user-motto">{{motto}}</text>
  </view>

  <button bindtap="navigateToDetail">查看详情</button>
</view>

// pages/index/index.wxss - 首页样式
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #aaa;
}

.userinfo-avatar {
  overflow: hidden;
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.usermotto {
  margin-top: 200px;
}

// app.json - 小程序配置
{
  "pages": [
    "pages/index/index",
    "pages/detail/detail",
    "pages/logs/logs"
  ],
  "window": {
    "backgroundTextStyle": "light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "微信小程序",
    "navigationBarTextStyle": "black"
  },
  "tabBar": {
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "首页"
      },
      {
        "pagePath": "pages/logs/logs",
        "text": "日志"
      }
    ]
  }
}
```

## 5. 官方注意事项 / 最佳实践

1. **性能优化**
   - 合理使用 `setData`，避免频繁更新数据
   - 图片资源使用合适的尺寸和格式
   - 分包加载，减少主包体积

2. **代码规范**
   - 使用 ES6+ 语法，提升代码可读性
   - 合理组织目录结构，模块化开发
   - 做好错误处理和用户提示

3. **安全注意事项**
   - 不要在前端存储敏感信息
   - 使用 HTTPS 协议进行网络请求
   - 对用户输入进行验证和过滤

4. **用户体验**
   - 合理使用加载状态和错误提示
   - 避免过多的弹窗和打扰
   - 提供流畅的页面跳转和交互

## 6. 官方文档参考链接

- 官方文档：https://developers.weixin.qq.com/miniprogram/dev/framework/
- 组件文档：https://developers.weixin.qq.com/miniprogram/dev/component/
- API 文档：https://developers.weixin.qq.com/miniprogram/dev/api/
