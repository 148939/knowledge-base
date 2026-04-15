# React 状态管理

## 1. 核心概述（官方定义）

### 状态管理概述
状态管理是管理应用程序状态的方式，包括如何存储状态、如何更新状态、以及状态如何在组件间共享。

### React 内置状态管理
React 提供了 useState、useReducer、useContext 等内置 Hook 来管理状态。

### 第三方状态管理库
- Redux：最流行的状态管理库
- MobX：响应式状态管理
- Zustand：轻量级状态管理
- Jotai：原子化状态管理
- Recoil：Facebook 官方状态管理库

---

## 2. 核心知识点（结构化层级）

### 2.1 useState

#### 基本使用
- 声明状态
- 更新状态
- 函数式更新
- 惰性初始化

#### 注意事项
- 状态更新是异步的
- 状态更新会合并
- 不要直接修改状态

### 2.2 useReducer

#### 基本使用
- reducer 函数
- dispatch 动作
- 初始化状态

#### 适用场景
- 复杂状态逻辑
- 多个子值的状态
- 下一个状态依赖前一个状态

### 2.3 Context API

#### 创建 Context
- createContext
- Provider
- Consumer

#### useContext Hook
- 读取 Context
- 避免不必要的重渲染
- Context 组合

### 2.4 Redux

#### 核心概念
- Store：存储状态
- Action：描述发生了什么
- Reducer：描述状态如何变化
- Dispatch：发送 Action

#### Redux Toolkit
- configureStore
- createSlice
- createAsyncThunk
- createEntityAdapter

### 2.5 Zustand

#### 基本使用
- create store
- 选择状态
- 更新状态

#### 特点
- 轻量级
- 不需要 Provider
- 支持 TypeScript
- 支持中间件

### 2.6 Jotai

#### 基本概念
- Atom：原子状态
- 派生 Atom
- 组合 Atom

#### 特点
- 原子化
- 细粒度更新
- TypeScript 友好

### 2.7 状态提升

#### 概念
- 将状态提升到共同父组件
- 通过 props 向下传递
- 通过回调向上传递

#### 适用场景
- 兄弟组件共享状态
- 跨组件通信

---

## 3. 官方标准语法 / API

### 3.1 useState 语法

```jsx
import { useState } from 'react';

// 基本使用
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}

// 函数式更新
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(prev => prev + 1)}>
        Increment
      </button>
    </div>
  );
}

// 惰性初始化
function Counter() {
  const [count, setCount] = useState(() => {
    const initialCount = someExpensiveComputation();
    return initialCount;
  });
  
  return <div>Count: {count}</div>;
}

// 对象状态
function UserForm() {
  const [user, setUser] = useState({
    name: '',
    age: 0
  });
  
  function handleNameChange(e) {
    setUser(prev => ({
      ...prev,
      name: e.target.value
    }));
  }
  
  return (
    <div>
      <input value={user.name} onChange={handleNameChange} />
      <p>Name: {user.name}</p>
    </div>
  );
}
```

### 3.2 useReducer 语法

```jsx
import { useReducer } from 'react';

// 定义 reducer
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: action.payload };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>
        +
      </button>
      <button onClick={() => dispatch({ type: 'decrement' })}>
        -
      </button>
      <button onClick={() => dispatch({ type: 'reset', payload: 0 })}>
        Reset
      </button>
    </div>
  );
}

// 复杂状态示例
const initialState = {
  todos: [],
  filter: 'all'
};

function todoReducer(state, action) {
  switch (action.type) {
    case 'add':
      return {
        ...state,
        todos: [...state.todos, action.payload]
      };
    case 'toggle':
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      };
    case 'setFilter':
      return {
        ...state,
        filter: action.payload
      };
    default:
      return state;
  }
}
```

### 3.3 Context API 语法

```jsx
import { createContext, useContext, useState } from 'react';

// 创建 Context
const ThemeContext = createContext('light');
const UserContext = createContext(null);

// Provider 组件
function App() {
  const [theme, setTheme] = useState('light');
  const [user, setUser] = useState({ name: 'Guest' });
  
  return (
    <ThemeContext.Provider value={theme}>
      <UserContext.Provider value={{ user, setUser }}>
        <Toolbar />
        <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
          Toggle Theme
        </button>
      </UserContext.Provider>
    </ThemeContext.Provider>
  );
}

// 使用 useContext
function Toolbar() {
  return (
    <div>
      <ThemedButton />
      <UserProfile />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{
      background: theme === 'light' ? '#fff' : '#333',
      color: theme === 'light' ? '#000' : '#fff'
    }}>
      Themed Button
    </button>
  );
}

function UserProfile() {
  const { user, setUser } = useContext(UserContext);
  
  return (
    <div>
      <p>User: {user.name}</p>
      <button onClick={() => setUser({ name: 'Alice' })}>
        Change User
      </button>
    </div>
  );
}

// 默认值
const CountContext = createContext(0);

function CountDisplay() {
  const count = useContext(CountContext);
  return <p>Count: {count}</p>;
}

function App() {
  return (
    <div>
      {/* 使用默认值 0 */}
      <CountDisplay />
      
      {/* 提供值 42 */}
      <CountContext.Provider value={42}>
        <CountDisplay />
      </CountContext.Provider>
    </div>
  );
}
```

### 3.4 Redux Toolkit 语法

```jsx
// store.js
import { configureStore, createSlice } from '@reduxjs/toolkit';

// 创建 slice
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => {
      state.value += 1;
    },
    decrement: state => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    }
  }
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;

const store = configureStore({
  reducer: {
    counter: counterSlice.reducer
  }
});

export default store;

// 组件中使用
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement, incrementByAmount } from './store';

function Counter() {
  const count = useSelector(state => state.counter.value);
  const dispatch = useDispatch();
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
      <button onClick={() => dispatch(incrementByAmount(5))}>
        +5
      </button>
    </div>
  );
}
```

### 3.5 Zustand 语法

```jsx
import { create } from 'zustand';

// 创建 store
const useStore = create((set) => ({
  count: 0,
  increment: () => set(state => ({ count: state.count + 1 })),
  decrement: () => set(state => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 })
}));

// 使用 store
function Counter() {
  const { count, increment, decrement, reset } = useStore();
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}

// 选择器
function CountDisplay() {
  const count = useStore(state => state.count);
  return <p>Count: {count}</p>;
}

// 异步操作
const useStore = create((set) => ({
  users: [],
  fetchUsers: async () => {
    const response = await fetch('/api/users');
    const users = await response.json();
    set({ users });
  }
}));
```

---

## 4. 官方示例代码（可运行）

### 4.1 待办应用（useReducer + Context）

```jsx
import { createContext, useContext, useReducer, useState } from 'react';

// Context
const TodoContext = createContext();
const TodoDispatchContext = createContext();

// Reducer
function todoReducer(state, action) {
  switch (action.type) {
    case 'add':
      return [...state, {
        id: Date.now(),
        text: action.text,
        completed: false
      }];
    case 'toggle':
      return state.map(todo =>
        todo.id === action.id
          ? { ...todo, completed: !todo.completed }
          : todo
      );
    case 'delete':
      return state.filter(todo => todo.id !== action.id);
    default:
      throw new Error('Unknown action');
  }
}

// Provider
function TodoProvider({ children }) {
  const [todos, dispatch] = useReducer(todoReducer, []);
  
  return (
    <TodoContext.Provider value={todos}>
      <TodoDispatchContext.Provider value={dispatch}>
        {children}
      </TodoDispatchContext.Provider>
    </TodoContext.Provider>
  );
}

// Hooks
function useTodos() {
  return useContext(TodoContext);
}

function useTodoDispatch() {
  return useContext(TodoDispatchContext);
}

// 组件
function AddTodo() {
  const [text, setText] = useState('');
  const dispatch = useTodoDispatch();
  
  function handleSubmit(e) {
    e.preventDefault();
    if (text.trim()) {
      dispatch({ type: 'add', text });
      setText('');
    }
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Add todo..."
      />
      <button type="submit">Add</button>
    </form>
  );
}

function TodoList() {
  const todos = useTodos();
  const dispatch = useTodoDispatch();
  
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <input
            type="checkbox"
            checked={todo.completed}
            onChange={() => dispatch({ type: 'toggle', id: todo.id })}
          />
          <span style={{
            textDecoration: todo.completed ? 'line-through' : 'none'
          }}>
            {todo.text}
          </span>
          <button onClick={() => dispatch({ type: 'delete', id: todo.id })}>
            ×
          </button>
        </li>
      ))}
    </ul>
  );
}

// 应用
function App() {
  return (
    <TodoProvider>
      <h1>Todo App</h1>
      <AddTodo />
      <TodoList />
    </TodoProvider>
  );
}

export default App;
```

### 4.2 购物车应用（Zustand）

```jsx
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

// Store
const useCartStore = create(
  persist(
    (set, get) => ({
      items: [],
      
      addItem: (product) => {
        const items = get().items;
        const existing = items.find(item => item.id === product.id);
        
        if (existing) {
          set({
            items: items.map(item =>
              item.id === product.id
                ? { ...item, quantity: item.quantity + 1 }
                : item
            )
          });
        } else {
          set({
            items: [...items, { ...product, quantity: 1 }]
          });
        }
      },
      
      removeItem: (id) => {
        set({
          items: get().items.filter(item => item.id !== id)
        });
      },
      
      updateQuantity: (id, quantity) => {
        if (quantity <= 0) {
          get().removeItem(id);
        } else {
          set({
            items: get().items.map(item =>
              item.id === id
                ? { ...item, quantity }
                : item
            )
          });
        }
      },
      
      clearCart: () => set({ items: [] }),
      
      getTotal: () => {
        return get().items.reduce(
          (total, item) => total + item.price * item.quantity,
          0
        );
      },
      
      getItemCount: () => {
        return get().items.reduce(
          (count, item) => count + item.quantity,
          0
        );
      }
    }),
    {
      name: 'cart-storage'
    }
  )
);

// 组件
function ProductList({ products }) {
  const addItem = useCartStore(state => state.addItem);
  
  return (
    <div>
      <h2>Products</h2>
      <div style={{ display: 'grid', gap: '1rem' }}>
        {products.map(product => (
          <div key={product.id} style={{ border: '1px solid #ccc', padding: '1rem' }}>
            <h3>{product.name}</h3>
            <p>${product.price}</p>
            <button onClick={() => addItem(product)}>
              Add to Cart
            </button>
          </div>
        ))}
      </div>
    </div>
  );
}

function ShoppingCart() {
  const items = useCartStore(state => state.items);
  const removeItem = useCartStore(state => state.removeItem);
  const updateQuantity = useCartStore(state => state.updateQuantity);
  const clearCart = useCartStore(state => state.clearCart);
  const getTotal = useCartStore(state => state.getTotal);
  const getItemCount = useCartStore(state => state.getItemCount);
  
  return (
    <div>
      <h2>Shopping Cart ({getItemCount()} items)</h2>
      {items.length === 0 ? (
        <p>Cart is empty</p>
      ) : (
        <>
          <ul>
            {items.map(item => (
              <li key={item.id}>
                <span>{item.name}</span>
                <span>${item.price}</span>
                <input
                  type="number"
                  value={item.quantity}
                  onChange={(e) => updateQuantity(item.id, parseInt(e.target.value))}
                  min="0"
                />
                <span>${item.price * item.quantity}</span>
                <button onClick={() => removeItem(item.id)}>Remove</button>
              </li>
            ))}
          </ul>
          <div>
            <strong>Total: ${getTotal()}</strong>
          </div>
          <button onClick={clearCart}>Clear Cart</button>
        </>
      )}
    </div>
  );
}

// 应用
const products = [
  { id: 1, name: 'Product A', price: 10 },
  { id: 2, name: 'Product B', price: 20 },
  { id: 3, name: 'Product C', price: 30 }
];

function App() {
  return (
    <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '2rem', padding: '2rem' }}>
      <ProductList products={products} />
      <ShoppingCart />
    </div>
  );
}

export default App;
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 状态管理选择

1. **何时使用本地状态**
   - 只在组件内部使用的状态
   - 不需要跨组件共享的状态
   - 简单的 UI 状态（展开/折叠、表单输入等）

2. **何时使用 Context**
   - 需要跨多层组件共享的状态
   - 主题、用户信息、语言设置等全局状态
   - 避免 prop drilling

3. **何时使用第三方库**
   - 复杂的状态逻辑
   - 需要时间旅行调试
   - 需要持久化状态
   - 大型应用，状态管理复杂

### 5.2 useState 最佳实践

1. **最小化状态**
   - 只存储真正需要的状态
   - 可以通过 props 或计算得到的不要存储

2. **状态拆分**
   - 将不相关的状态拆分到多个 useState
   - 避免超大的状态对象

3. **函数式更新**
   - 新状态依赖旧状态时使用
   - 确保状态更新的正确性

### 5.3 Context 最佳实践

1. **避免不必要的重渲染**
   - 拆分 Context
   - 使用 useMemo 优化 Provider
   - 使用选择器选择需要的状态

2. **Context 组合**
   - 将相关的 Context 放在一起
   - 避免过深的 Provider 嵌套

3. **默认值**
   - 提供合理的默认值
   - 便于测试和开发

### 5.4 Redux 最佳实践

1. **使用 Redux Toolkit**
   - 简化 Redux 使用
   - 内置最佳实践
   - 减少样板代码

2. **正常化状态形状**
   - 使用对象存储列表
   - 通过 ID 引用
   - 避免重复数据

3. **单一数据源**
   - 所有状态在一个 store
   - 便于调试和追踪

### 5.5 Zustand 最佳实践

1. **拆分 store**
   - 按功能拆分 store
   - 避免超大 store

2. **使用选择器**
   - 只选择需要的状态
   - 避免不必要的重渲染

3. **中间件**
   - 使用 persist 持久化
   - 使用 devtools 调试

---

## 6. 官方文档参考链接

- [React 官方文档 - useState](https://react.dev/reference/react/useState)
- [React 官方文档 - useReducer](https://react.dev/reference/react/useReducer)
- [React 官方文档 - useContext](https://react.dev/reference/react/useContext)
- [Redux 官方文档](https://redux.js.org/)
- [Redux Toolkit 官方文档](https://redux-toolkit.js.org/)
- [Zustand 官方文档](https://zustand-demo.pmnd.rs/)
- [Jotai 官方文档](https://jotai.org/)
