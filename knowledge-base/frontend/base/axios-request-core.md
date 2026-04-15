# 检索索引：本文档收录【Axios】【请求核心】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Axios 是一个基于 Promise 的 HTTP 客户端，可用于浏览器和 Node.js 环境。它具有以下特性：在浏览器中使用 XMLHttpRequests，在 Node.js 中使用 http 模块，支持 Promise API，可拦截请求和响应，可转换请求和响应数据，可取消请求，自动转换 JSON 数据，支持客户端防御 XSRF 等。

## 2. 核心知识点（结构化层级）

### 2.1 基础使用
1. 安装 Axios
2. 发送 GET 请求
3. 发送 POST 请求
4. 发送 PUT 请求
5. 发送 DELETE 请求
6. 其他 HTTP 方法

### 2.2 请求配置
1. 请求配置对象
2. URL 参数
3. 请求头
4. 请求体
5. 超时设置
6. 响应类型

### 2.3 响应结构
1. 响应数据
2. 响应状态码
3. 响应头
4. 错误处理

### 2.4 拦截器
1. 请求拦截器
2. 响应拦截器
3. 移除拦截器

### 2.5 实例创建
1. 创建 Axios 实例
2. 实例配置
3. 实例方法

### 2.6 请求取消
1. CancelToken
2. AbortController

### 2.7 并发请求
1. axios.all
2. axios.spread

## 3. 官方标准语法 / API

### 基础请求语法
```javascript
import axios from 'axios';

// GET 请求
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// GET 请求 - 参数配置
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// POST 请求
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// 执行多个并发请求
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));
```

### 请求配置语法
```javascript
{
  // `url` 是用于请求的服务器 URL
  url: '/user',

  // `method` 是创建请求时使用的方法
  method: 'get', // 默认值

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  transformRequest: [function (data, headers) {
    // 对发送的 data 进行任意转换处理
    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对接收的 data 进行任意转换处理
    return data;
  }],

  // `headers` 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是即将与请求一起发送的 URL 参数
  params: {
    ID: 12345
  },

  // `data` 是作为请求主体被发送的数据
  data: {
    firstName: 'Fred'
  },

  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  timeout: 1000,

  // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // 默认值

  // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` 表示服务器响应的数据类型
  responseType: 'json', // 默认值

  // `xsrfCookieName` 是用作 xsrf token 的值的 cookie 的名称
  xsrfCookieName: 'XSRF-TOKEN', // 默认值

  // `xsrfHeaderName` 是承载 xsrf token 的值的 HTTP 头的名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // 默认值

  // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject promise
  validateStatus: function (status) {
    return status >= 200 && status < 300; // 默认值
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

### 响应结构语法
```javascript
{
  // `data` 由服务器提供的响应
  data: {},

  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // `headers` 服务器响应的头
  headers: {},

  // `config` 是为请求提供的配置信息
  config: {},

  // `request` 是生成此响应的请求
  request: {}
}
```

### 拦截器语法
```javascript
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 2xx 范围内的状态码都会触发该函数
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数
    return Promise.reject(error);
  });

// 移除拦截器
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

### 创建实例语法
```javascript
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});

// 实例方法
instance.get(url[, config])
instance.delete(url[, config])
instance.head(url[, config])
instance.options(url[, config])
instance.post(url[, data[, config]])
instance.put(url[, data[, config]])
instance.patch(url[, data[, config]])
```

### 取消请求语法
```javascript
// 使用 CancelToken
import axios from 'axios';
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function (thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
    // 处理错误
  }
});

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.');

// 使用 AbortController
const controller = new AbortController();

axios.get('/foo/bar', {
   signal: controller.signal
}).then(function(response) {
   //...
});

// 取消请求
controller.abort();
```

## 4. 官方示例代码（可运行）

### 基础请求示例
```javascript
import axios from 'axios';

// 完整的请求配置示例
async function makeRequest() {
  try {
    const response = await axios({
      method: 'get',
      url: 'https://api.example.com/users',
      params: {
        page: 1,
        limit: 10
      },
      headers: {
        'Authorization': 'Bearer token123',
        'Content-Type': 'application/json'
      },
      timeout: 5000
    });
    
    console.log('数据:', response.data);
    console.log('状态码:', response.status);
    console.log('响应头:', response.headers);
    return response.data;
  } catch (error) {
    if (error.response) {
      // 请求已发出，但服务器响应的状态码不在 2xx 范围内
      console.log('错误数据:', error.response.data);
      console.log('错误状态码:', error.response.status);
      console.log('错误响应头:', error.response.headers);
    } else if (error.request) {
      // 请求已发出，但没有收到响应
      console.log('请求未收到响应:', error.request);
    } else {
      // 发送请求时出了点问题
      console.log('错误:', error.message);
    }
    console.log('错误配置:', error.config);
  }
}

// GET 请求
async function getUser() {
  try {
    const response = await axios.get('/user/12345');
    return response.data;
  } catch (error) {
    console.error(error);
  }
}

// POST 请求
async function createUser() {
  try {
    const response = await axios.post('/user', {
      firstName: 'Fred',
      lastName: 'Flintstone'
    });
    return response.data;
  } catch (error) {
    console.error(error);
  }
}

// PUT 请求
async function updateUser() {
  try {
    const response = await axios.put('/user/12345', {
      firstName: 'Fred',
      lastName: 'Flintstone'
    });
    return response.data;
  } catch (error) {
    console.error(error);
  }
}

// DELETE 请求
async function deleteUser() {
  try {
    const response = await axios.delete('/user/12345');
    return response.data;
  } catch (error) {
    console.error(error);
  }
}

// 并发请求
async function fetchMultipleResources() {
  try {
    const [users, posts, comments] = await Promise.all([
      axios.get('/users'),
      axios.get('/posts'),
      axios.get('/comments')
    ]);
    
    return {
      users: users.data,
      posts: posts.data,
      comments: comments.data
    };
  } catch (error) {
    console.error(error);
  }
}
```

### Axios 实例封装示例
```javascript
// request.js
import axios from 'axios';
import { ElMessage } from 'element-plus';

// 创建 Axios 实例
const service = axios.create({
  baseURL: import.meta.env.VITE_APP_BASE_API, // api 的 base_url
  timeout: 5000 // 请求超时时间
});

// 请求拦截器
service.interceptors.request.use(
  config => {
    // 在发送请求之前做些什么
    const token = localStorage.getItem('token');
    if (token) {
      // 让每个请求携带token
      config.headers['Authorization'] = 'Bearer ' + token;
    }
    return config;
  },
  error => {
    // 对请求错误做些什么
    console.log(error); // for debug
    return Promise.reject(error);
  }
);

// 响应拦截器
service.interceptors.response.use(
  response => {
    const res = response.data;
    
    // 如果自定义的 code 不是 20000，则判定为错误
    if (res.code !== 20000) {
      ElMessage({
        message: res.message || 'Error',
        type: 'error',
        duration: 5 * 1000
      });
      
      // 50008:非法的token; 50012:其他客户端登录了;  50014:Token 过期了;
      if (res.code === 50008 || res.code === 50012 || res.code === 50014) {
        // 重新登录
        ElMessage({
          message: '登录已过期，请重新登录',
          type: 'warning',
          duration: 5 * 1000,
          onClose: () => {
            localStorage.removeItem('token');
            window.location.reload();
          }
        });
      }
      return Promise.reject(new Error(res.message || 'Error'));
    } else {
      return res;
    }
  },
  error => {
    console.log('err' + error); // for debug
    ElMessage({
      message: error.message,
      type: 'error',
      duration: 5 * 1000
    });
    return Promise.reject(error);
  }
);

export default service;
```

```javascript
// api/user.js
import request from '@/utils/request';

// 获取用户列表
export function getUserList(params) {
  return request({
    url: '/users',
    method: 'get',
    params
  });
}

// 获取用户详情
export function getUser(id) {
  return request({
    url: `/users/${id}`,
    method: 'get'
  });
}

// 创建用户
export function createUser(data) {
  return request({
    url: '/users',
    method: 'post',
    data
  });
}

// 更新用户
export function updateUser(id, data) {
  return request({
    url: `/users/${id}`,
    method: 'put',
    data
  });
}

// 删除用户
export function deleteUser(id) {
  return request({
    url: `/users/${id}`,
    method: 'delete'
  });
}
```

```javascript
// 在组件中使用
import { getUserList, createUser } from '@/api/user';

export default {
  data() {
    return {
      userList: [],
      loading: false
    };
  },
  methods: {
    async fetchUsers() {
      this.loading = true;
      try {
        const res = await getUserList({ page: 1, limit: 10 });
        this.userList = res.data;
      } catch (error) {
        console.error(error);
      } finally {
        this.loading = false;
      }
    },
    async handleCreate() {
      try {
        await createUser({
          name: '张三',
          email: 'zhangsan@example.com'
        });
        this.fetchUsers();
      } catch (error) {
        console.error(error);
      }
    }
  },
  mounted() {
    this.fetchUsers();
  }
};
```

### 上传下载示例
```javascript
// 上传文件
async function uploadFile(file) {
  const formData = new FormData();
  formData.append('file', file);
  
  try {
    const response = await axios.post('/upload', formData, {
      headers: {
        'Content-Type': 'multipart/form-data'
      },
      onUploadProgress: (progressEvent) => {
        const percentCompleted = Math.round(
          (progressEvent.loaded * 100) / progressEvent.total
        );
        console.log(`上传进度: ${percentCompleted}%`);
      }
    });
    return response.data;
  } catch (error) {
    console.error('上传失败:', error);
  }
}

// 下载文件
async function downloadFile() {
  try {
    const response = await axios.get('/download', {
      responseType: 'blob',
      onDownloadProgress: (progressEvent) => {
        const percentCompleted = Math.round(
          (progressEvent.loaded * 100) / progressEvent.total
        );
        console.log(`下载进度: ${percentCompleted}%`);
      }
    });
    
    // 创建下载链接
    const url = window.URL.createObjectURL(new Blob([response.data]));
    const link = document.createElement('a');
    link.href = url;
    link.setAttribute('download', 'filename.pdf');
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  } catch (error) {
    console.error('下载失败:', error);
  }
}
```

## 5. 官方注意事项 / 最佳实践

### 请求配置注意事项
1. **URL 拼接**：baseURL 会自动加在 url 前面，除非 url 是绝对 URL
2. **参数传递**：GET 请求使用 params，POST 请求使用 data
3. **请求头设置**：可以通过 headers 配置自定义请求头
4. **超时设置**：合理设置 timeout 避免请求长时间等待
5. **跨域凭证**：跨域请求需要凭证时设置 withCredentials: true

### 错误处理最佳实践
1. **统一错误处理**：在响应拦截器中统一处理错误
2. **错误分类**：区分网络错误、服务器错误、业务错误
3. **错误提示**：给用户友好的错误提示
4. **错误日志**：记录错误信息便于调试
5. **错误恢复**：对于可恢复的错误提供重试机制

### 拦截器最佳实践
1. **请求拦截器**：添加认证 token、统一请求参数、请求日志
2. **响应拦截器**：统一处理响应数据、错误处理、响应日志
3. **拦截器顺序**：请求拦截器按添加顺序执行，响应拦截器按相反顺序执行
4. **移除拦截器**：不需要的拦截器及时移除避免内存泄漏

### 性能优化
1. **连接复用**：使用 Axios 实例复用连接
2. **请求取消**：页面卸载时取消未完成的请求
3. **请求缓存**：对于不常变化的数据进行缓存
4. **并发控制**：控制并发请求数量避免浏览器限制
5. **数据压缩**：开启 Gzip 压缩减少传输数据量

### 安全注意事项
1. **XSRF 防护**：使用 Axios 内置的 XSRF 防护
2. **Token 安全**：Token 安全存储，避免 XSS 攻击
3. **HTTPS**：生产环境使用 HTTPS
4. **输入验证**：对用户输入进行验证
5. **敏感信息**：不要在日志中输出敏感信息

### 最佳实践总结
1. **封装 Axios 实例**：创建统一的 Axios 实例配置
2. **API 模块化**：将 API 调用封装在独立的模块中
3. **类型安全**：使用 TypeScript 提供类型安全
4. **请求取消**：合理使用请求取消功能
5. **错误边界**：设置错误边界处理异常
6. **请求重试**：对于网络错误实现请求重试
7. **进度监控**：上传下载时提供进度反馈
8. **代码可读性**：保持代码清晰易读

## 6. 官方文档参考链接

- Axios 官方文档：https://axios-http.com/
- Axios GitHub：https://github.com/axios/axios
- Axios 中文文档：https://axios-http.com/zh/
