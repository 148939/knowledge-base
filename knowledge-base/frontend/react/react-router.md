# 检索索引：本文档收录【React】【React Router】知识点，内容100%来自React Router官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

React Router 是 React 的官方路由库，它使你能够在单页应用（SPA）中实现客户端路由，通过 URL 同步 UI，提供声明式、可组合的 API 来管理应用程序的导航和路由。React Router v6 是当前最新版本，提供了更简洁、更强大的 API。

## 2. 核心知识点（结构化层级）

### 2.1 核心组件
1. BrowserRouter - 浏览器路由容器
2. HashRouter - Hash 路由容器
3. MemoryRouter - 内存路由容器
4. Routes/Route - 路由匹配和渲染
5. Link - 导航链接组件
6. NavLink - 激活状态的导航链接
7. Outlet - 嵌套路由的占位符
8. Navigate - 编程式导航组件

### 2.2 核心钩子（Hooks）
1. useNavigate - 编程式导航
2. useLocation - 获取当前位置信息
3. useParams - 获取路由参数
4. useSearchParams - 获取/设置 URL 查询参数
5. useRoutes - 函数式路由配置
6. useMatch - 匹配路由路径
7. useResolvedPath - 解析路径
8. useInRouterContext - 检查是否在路由上下文中

### 2.3 路由配置
1. 基础路由配置
2. 嵌套路由
3. 动态路由参数
4. 路由匹配策略
5. 索引路由（Index Route）
6. 通配符路由
7. 相对路径

### 2.4 高级特性
1. 路由守卫（Protected Routes）
2. 懒加载路由
3. 路由过渡动画
4. 错误处理路由
5. 查询参数处理
6. 路由状态传递
7. 路由加载器和动作（v6.4+）

## 3. 官方标准语法 / API

### 基础路由设置
```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import About from './pages/About';
import Contact from './pages/Contact';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

### Link 和 NavLink
```jsx
import { Link, NavLink } from 'react-router-dom';

function Navigation() {
  return (
    <nav>
      {/* 基础 Link */}
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
      <Link to="/contact">Contact</Link>
      
      {/* NavLink - 支持激活状态 */}
      <NavLink
        to="/"
        style={({ isActive }) => ({
          color: isActive ? 'red' : 'blue',
          fontWeight: isActive ? 'bold' : 'normal'
        })}
      >
        Home
      </NavLink>
      
      <NavLink
        to="/about"
        className={({ isActive }) => isActive ? 'active-link' : 'link'}
      >
        About
      </NavLink>
      
      {/* 相对路径导航 */}
      <Link to=".." relative="path">Go Back</Link>
      <Link to="." relative="path">Refresh</Link>
      
      {/* 带查询参数 */}
      <Link
        to="/search"
        state={{ from: 'navigation' }}
        search="?q=react&sort=date"
      >
        Search
      </Link>
    </nav>
  );
}
```

### 动态路由参数
```jsx
import { Routes, Route, useParams } from 'react-router-dom';

function UserProfile() {
  // 获取路由参数
  const { userId } = useParams();
  return <h1>User Profile: {userId}</h1>;
}

function ProductDetail() {
  const { category, productId } = useParams();
  return (
    <div>
      <h2>Category: {category}</h2>
      <p>Product ID: {productId}</p>
    </div>
  );
}

function App() {
  return (
    <Routes>
      {/* 单个参数 */}
      <Route path="/users/:userId" element={<UserProfile />} />
      
      {/* 多个参数 */}
      <Route path="/products/:category/:productId" element={<ProductDetail />} />
      
      {/* 可选参数（使用 ?） */}
      <Route path="/posts/:postId?" element={<Posts />} />
      
      {/* 通配符 */}
      <Route path="/files/*" element={<Files />} />
    </Routes>
  );
}
```

### useNavigate - 编程式导航
```jsx
import { useNavigate } from 'react-router-dom';

function LoginForm() {
  const navigate = useNavigate();

  const handleSubmit = async (e) => {
    e.preventDefault();
    
    // 模拟登录
    await loginUser();
    
    // 导航到首页
    navigate('/');
    
    // 导航并替换历史记录
    navigate('/dashboard', { replace: true });
    
    // 导航并传递状态
    navigate('/profile', { 
      state: { from: 'login', userId: 123 } 
    });
    
    // 返回上一页
    navigate(-1);
    
    // 前进一页
    navigate(1);
    
    // 返回到指定页
    navigate(-2);
  };

  return <form onSubmit={handleSubmit}>{/* 表单内容 */}</form>;
}
```

### useLocation - 获取位置信息
```jsx
import { useLocation } from 'react-router-dom';

function CurrentPageInfo() {
  const location = useLocation();
  
  console.log('Full location object:', location);
  // {
  //   pathname: '/users/123',
  //   search: '?tab=profile',
  //   hash: '#section',
  //   state: { from: 'home' },
  //   key: 'abc123'
  // }

  return (
    <div>
      <p>Path: {location.pathname}</p>
      <p>Search: {location.search}</p>
      <p>Hash: {location.hash}</p>
      <p>State: {JSON.stringify(location.state)}</p>
    </div>
  );
}

// 监听路由变化
import { useEffect } from 'react';

function RouteWatcher() {
  const location = useLocation();

  useEffect(() => {
    console.log('Route changed to:', location.pathname);
    // 可以在这里发送页面浏览事件等
  }, [location]);

  return null;
}
```

### useSearchParams - 查询参数
```jsx
import { useSearchParams } from 'react-router-dom';

function SearchPage() {
  const [searchParams, setSearchParams] = useSearchParams();

  // 获取查询参数
  const query = searchParams.get('q');
  const page = searchParams.get('page') || '1';
  const sort = searchParams.get('sort') || 'date';
  const hasFilter = searchParams.has('filter');

  // 获取所有参数
  const allParams = Object.fromEntries(searchParams);

  // 设置查询参数
  const handleSearch = (newQuery) => {
    setSearchParams({ q: newQuery, page: '1' });
  };

  const handlePageChange = (newPage) => {
    // 保留现有参数，只更新 page
    setSearchParams(prev => {
      prev.set('page', newPage);
      return prev;
    });
  };

  const handleSortChange = (newSort) => {
    // 使用函数形式更新
    setSearchParams(prev => {
      const newParams = new URLSearchParams(prev);
      newParams.set('sort', newSort);
      newParams.delete('page'); // 重置页码
      return newParams;
    });
  };

  return (
    <div>
      <h1>Search Results for: {query}</h1>
      <p>Page: {page}</p>
      <p>Sort: {sort}</p>
      
      <button onClick={() => handleSortChange('price')}>
        Sort by Price
      </button>
    </div>
  );
}
```

### 嵌套路由和 Outlet
```jsx
import { Routes, Route, Outlet, Link } from 'react-router-dom';

function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      
      {/* 子导航 */}
      <nav>
        <Link to="profile">Profile</Link>
        <Link to="settings">Settings</Link>
        <Link to="messages">Messages</Link>
      </nav>
      
      {/* 子路由的占位符 */}
      <Outlet />
    </div>
  );
}

function Profile() {
  return <h2>Profile Settings</h2>;
}

function Settings() {
  return <h2>Account Settings</h2>;
}

function Messages() {
  return <h2>Your Messages</h2>;
}

function DashboardHome() {
  return <h2>Welcome to Dashboard!</h2>;
}

function App() {
  return (
    <Routes>
      <Route path="/dashboard" element={<Dashboard />}>
        {/* 索引路由 - 当访问 /dashboard 时显示 */}
        <Route index element={<DashboardHome />} />
        
        {/* 嵌套路由 */}
        <Route path="profile" element={<Profile />} />
        <Route path="settings" element={<Settings />} />
        <Route path="messages" element={<Messages />} />
      </Route>
    </Routes>
  );
}
```

### useRoutes - 函数式路由配置
```jsx
import { useRoutes } from 'react-router-dom';
import Home from './Home';
import About from './About';
import Users from './Users';
import UserProfile from './UserProfile';

function App() {
  const element = useRoutes([
    {
      path: '/',
      element: <Home />
    },
    {
      path: '/about',
      element: <About />
    },
    {
      path: '/users',
      element: <Users />,
      children: [
        { index: true, element: <UsersList /> },
        { path: ':id', element: <UserProfile /> }
      ]
    },
    {
      path: '*',
      element: <NotFound />
    }
  ]);

  return element;
}
```

### 路由守卫（Protected Routes）
```jsx
import { Navigate, Outlet, useLocation } from 'react-router-dom';
import { useAuth } from '../hooks/useAuth';

function ProtectedRoute() {
  const { isAuthenticated } = useAuth();
  const location = useLocation();

  if (!isAuthenticated) {
    // 重定向到登录页，并保存当前位置以便登录后返回
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  // 已认证，渲染子路由
  return <Outlet />;
}

function AdminRoute() {
  const { isAuthenticated, user } = useAuth();

  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }

  if (!user.isAdmin) {
    return <Navigate to="/unauthorized" replace />;
  }

  return <Outlet />;
}

// 使用示例
function App() {
  return (
    <Routes>
      {/* 公开路由 */}
      <Route path="/login" element={<Login />} />
      <Route path="/" element={<Home />} />
      
      {/* 需要认证的路由 */}
      <Route element={<ProtectedRoute />}>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/profile" element={<Profile />} />
      </Route>
      
      {/* 需要管理员权限的路由 */}
      <Route element={<AdminRoute />}>
        <Route path="/admin" element={<AdminPanel />} />
        <Route path="/admin/users" element={<UserManagement />} />
      </Route>
      
      {/* 404 页面 */}
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}

// 登录后重定向
function Login() {
  const navigate = useNavigate();
  const location = useLocation();
  const { login } = useAuth();

  const from = location.state?.from?.pathname || '/';

  const handleLogin = async () => {
    await login();
    navigate(from, { replace: true });
  };

  return (
    <div>
      <h2>Login</h2>
      <button onClick={handleLogin}>Login</button>
    </div>
  );
}
```

### 懒加载路由
```jsx
import { Suspense, lazy } from 'react';
import { Routes, Route } from 'react-router-dom';

// 懒加载组件
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Profile = lazy(() => import('./pages/Profile'));

function LoadingFallback() {
  return <div>Loading...</div>;
}

function App() {
  return (
    <Suspense fallback={<LoadingFallback />}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/profile" element={<Profile />} />
      </Routes>
    </Suspense>
  );
}

// 按路由分组懒加载
const AdminRoutes = lazy(() => import('./routes/AdminRoutes'));

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route 
        path="/admin/*" 
        element={
          <Suspense fallback={<Loading />}>
            <AdminRoutes />
          </Suspense>
        } 
      />
    </Routes>
  );
}
```

### 错误处理路由
```jsx
import { Routes, Route, useRouteError, isRouteErrorResponse } from 'react-router-dom';

function ErrorBoundary() {
  const error = useRouteError();

  if (isRouteErrorResponse(error)) {
    // 路由错误响应
    return (
      <div>
        <h1>{error.status} {error.statusText}</h1>
        <p>{error.data?.message || 'Something went wrong'}</p>
      </div>
    );
  } else if (error instanceof Error) {
    // 其他错误
    return (
      <div>
        <h1>Unexpected Error</h1>
        <p>{error.message}</p>
      </div>
    );
  } else {
    return <h1>Unknown Error</h1>;
  }
}

function NotFound() {
  return (
    <div>
      <h1>404 - Page Not Found</h1>
      <p>The page you're looking for doesn't exist.</p>
      <Link to="/">Go Home</Link>
    </div>
  );
}

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      
      {/* 路由级错误处理 */}
      <Route 
        path="/users/:id" 
        element={<UserDetail />}
        errorElement={<ErrorBoundary />}
      />
      
      {/* 嵌套路由错误处理 */}
      <Route path="/dashboard" element={<Dashboard />}>
        <Route path="profile" element={<Profile />} />
        <Route 
          path="settings" 
          element={<Settings />}
          errorElement={<SettingsError />}
        />
      </Route>
      
      {/* 404 路由 */}
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}
```

### v6.4+ 数据加载器（Loaders）和动作（Actions）
```jsx
import { 
  createBrowserRouter, 
  RouterProvider,
  useLoaderData,
  useActionData,
  useNavigation,
  Form,
  redirect
} from 'react-router-dom';

// Loader - 数据加载函数
async function postLoader({ params, request }) {
  const { postId } = params;
  const post = await fetchPost(postId);
  
  if (!post) {
    throw new Response('Not Found', { status: 404 });
  }
  
  return post;
}

// Action - 数据提交函数
async function postAction({ request, params }) {
  const formData = await request.formData();
  const updates = Object.fromEntries(formData);
  
  await updatePost(params.postId, updates);
  
  return redirect(`/posts/${params.postId}`);
}

// 使用 loader 数据
function PostDetail() {
  const post = useLoaderData();
  const actionData = useActionData();
  const navigation = useNavigation();
  
  const isSubmitting = navigation.state === 'submitting';

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
      
      <Form method="post">
        <input 
          type="text" 
          name="title" 
          defaultValue={post.title} 
        />
        <textarea 
          name="content" 
          defaultValue={post.content} 
        />
        <button type="submit" disabled={isSubmitting}>
          {isSubmitting ? 'Saving...' : 'Save'}
        </button>
      </Form>
      
      {actionData?.error && (
        <p className="error">{actionData.error}</p>
      )}
    </div>
  );
}

// 配置路由
const router = createBrowserRouter([
  {
    path: '/',
    element: <Home />,
    loader: homeLoader
  },
  {
    path: '/posts/:postId',
    element: <PostDetail />,
    loader: postLoader,
    action: postAction,
    errorElement: <ErrorPage />
  }
]);

function App() {
  return <RouterProvider router={router} />;
}
```

## 4. 官方示例代码（可运行）

### 完整的博客应用
```jsx
import { 
  BrowserRouter, 
  Routes, 
  Route, 
  Link, 
  NavLink,
  Outlet,
  useParams,
  useNavigate,
  useLocation,
  useSearchParams,
  Navigate
} from 'react-router-dom';
import { useState, useEffect } from 'react';

// 模拟数据
const mockPosts = [
  { id: 1, title: 'Getting Started with React', category: 'react', content: 'Learn React basics...' },
  { id: 2, title: 'React Router v6 Guide', category: 'react', content: 'Master React Router...' },
  { id: 3, title: 'TypeScript Tips', category: 'typescript', content: 'TypeScript best practices...' },
];

// 布局组件
function Layout() {
  return (
    <div>
      <Header />
      <main style={{ padding: '20px' }}>
        <Outlet />
      </main>
      <Footer />
    </div>
  );
}

function Header() {
  return (
    <header style={{ 
      background: '#333', 
      color: 'white', 
      padding: '1rem',
      display: 'flex',
      justifyContent: 'space-between',
      alignItems: 'center'
    }}>
      <Link to="/" style={{ color: 'white', textDecoration: 'none' }}>
        <h1 style={{ margin: 0 }}>My Blog</h1>
      </Link>
      <nav>
        <NavLink 
          to="/" 
          style={({ isActive }) => ({
            color: 'white',
            margin: '0 10px',
            textDecoration: isActive ? 'underline' : 'none'
          })}
        >
          Home
        </NavLink>
        <NavLink 
          to="/posts" 
          style={({ isActive }) => ({
            color: 'white',
            margin: '0 10px',
            textDecoration: isActive ? 'underline' : 'none'
          })}
        >
          Posts
        </NavLink>
        <NavLink 
          to="/about" 
          style={({ isActive }) => ({
            color: 'white',
            margin: '0 10px',
            textDecoration: isActive ? 'underline' : 'none'
          })}
        >
          About
        </NavLink>
      </nav>
    </header>
  );
}

function Footer() {
  return (
    <footer style={{ 
      background: '#f5f5f5', 
      padding: '1rem', 
      marginTop: '2rem',
      textAlign: 'center'
    }}>
      <p>© 2024 My Blog</p>
    </footer>
  );
}

// 首页
function Home() {
  const recentPosts = mockPosts.slice(0, 3);
  
  return (
    <div>
      <h2>Welcome to My Blog</h2>
      <h3>Recent Posts</h3>
      <ul>
        {recentPosts.map(post => (
          <li key={post.id}>
            <Link to={`/posts/${post.id}`}>{post.title}</Link>
          </li>
        ))}
      </ul>
      <Link to="/posts">View all posts →</Link>
    </div>
  );
}

// 文章列表
function Posts() {
  const [searchParams, setSearchParams] = useSearchParams();
  const category = searchParams.get('category');
  
  const filteredPosts = category 
    ? mockPosts.filter(post => post.category === category)
    : mockPosts;

  const handleCategoryChange = (newCategory) => {
    if (newCategory) {
      setSearchParams({ category: newCategory });
    } else {
      setSearchParams({});
    }
  };

  return (
    <div>
      <h2>All Posts</h2>
      
      <div style={{ marginBottom: '20px' }}>
        <button onClick={() => handleCategoryChange(null)}>All</button>
        <button onClick={() => handleCategoryChange('react')}>React</button>
        <button onClick={() => handleCategoryChange('typescript')}>TypeScript</button>
      </div>

      <div style={{ display: 'grid', gap: '20px' }}>
        {filteredPosts.map(post => (
          <article key={post.id} style={{ border: '1px solid #ddd', padding: '15px' }}>
            <h3>
              <Link to={`/posts/${post.id}`}>{post.title}</Link>
            </h3>
            <p>Category: {post.category}</p>
            <p>{post.content}</p>
          </article>
        ))}
      </div>
    </div>
  );
}

// 文章详情
function PostDetail() {
  const { postId } = useParams();
  const navigate = useNavigate();
  const location = useLocation();
  
  const post = mockPosts.find(p => p.id === parseInt(postId));
  
  if (!post) {
    return <Navigate to="/404" replace />;
  }

  return (
    <div>
      <button onClick={() => navigate(-1)}>← Back</button>
      <article>
        <h2>{post.title}</h2>
        <p>Category: {post.category}</p>
        <div style={{ marginTop: '20px' }}>
          <p>{post.content}</p>
        </div>
      </article>
      
      <div style={{ marginTop: '20px', padding: '10px', background: '#f5f5f5' }}>
        <h4>Debug Info:</h4>
        <p>Location: {location.pathname}</p>
        <p>Post ID: {postId}</p>
      </div>
    </div>
  );
}

// 关于页面
function About() {
  return (
    <div>
      <h2>About</h2>
      <p>This is a demo blog built with React Router v6.</p>
    </div>
  );
}

// 404 页面
function NotFound() {
  return (
    <div style={{ textAlign: 'center', padding: '50px' }}>
      <h2>404 - Page Not Found</h2>
      <p>The page you're looking for doesn't exist.</p>
      <Link to="/">Go back home</Link>
    </div>
  );
}

// 主应用
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route index element={<Home />} />
          <Route path="posts" element={<Posts />} />
          <Route path="posts/:postId" element={<PostDetail />} />
          <Route path="about" element={<About />} />
          <Route path="404" element={<NotFound />} />
          <Route path="*" element={<NotFound />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

## 5. 官方注意事项 / 最佳实践

### 路由设计原则
1. **RESTful URL**：URL 应该有意义且符合 RESTful 风格
2. **层次清晰**：嵌套路由反映应用的信息架构
3. **可收藏的 URL**：每个重要页面都应该有一个唯一的 URL
4. **用户友好**：URL 应该简短、易读、易记

### v6 升级注意事项
1. **Switch → Routes**：使用 Routes 替代 Switch
2. **exact 移除**：路由匹配现在默认是精确匹配
3. **component/render → element**：使用 element prop 传递组件
4. **嵌套路由简化**：在父路由的 element 中使用 Outlet
5. **useHistory → useNavigate**：使用 useNavigate 替代 useHistory

### 性能优化
1. **懒加载路由**：使用 React.lazy 和 Suspense 懒加载路由组件
2. **代码分割**：按路由进行代码分割，减少初始加载体积
3. **避免过度渲染**：合理使用 memo、useMemo、useCallback
4. **预加载**：对可能访问的路由进行预加载

### 安全性
1. **路由守卫**：使用 Protected Routes 保护敏感页面
2. **验证参数**：对路由参数进行验证和清理
3. **避免 URL 注入**：不要直接将用户输入拼接到 URL 中
4. **状态安全**：通过 location.state 传递的敏感数据要注意安全

### 测试建议
1. **路由导航测试**：测试所有路由的导航是否正常
2. **参数测试**：测试动态路由参数的各种情况
3. **错误处理测试**：测试 404、错误边界等情况
4. **守卫测试**：测试路由守卫的逻辑

### 常见问题
1. **刷新页面 404**：确保服务器配置正确，所有路由都指向 index.html
2. **路由不匹配**：检查路径顺序，更具体的路径应该放在前面
3. **useNavigate 不工作**：确保组件在 BrowserRouter 内部
4. **参数未更新**：使用 useEffect 监听 useParams 的变化

## 6. 官方文档参考链接

- React Router 官方文档：https://reactrouter.com/
- React Router v6 教程：https://reactrouter.com/en/main/start/tutorial
- React Router API 参考：https://reactrouter.com/en/main/start/overview
- 升级到 v6 指南：https://reactrouter.com/en/main/upgrading/v5
- GitHub 仓库：https://github.com/remix-run/react-router
