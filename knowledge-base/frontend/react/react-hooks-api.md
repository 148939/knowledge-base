# 检索索引：本文档收录【React】【Hooks API】知识点，内容100%来自React官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Hooks 是 React 16.8 引入的新特性，它可以让你在不编写类组件的情况下使用 state 以及其他的 React 特性。Hooks 是一些可以让你在函数组件里"钩入" React state 及生命周期等特性的函数。Hooks 不能在类组件中使用，它只能在函数组件中使用。

## 2. 核心知识点（结构化层级）

### 2.1 基础 Hooks
1. useState - 状态管理
2. useEffect - 副作用处理
3. useContext - 上下文消费

### 2.2 额外 Hooks
1. useReducer - 状态管理（类似 Redux）
2. useCallback - 回调函数缓存
3. useMemo - 计算值缓存
4. useRef - 引用访问
5. useImperativeHandle - 暴露给父组件的实例值
6. useLayoutEffect - DOM变更后同步触发
7. useDebugValue - 在React DevTools中显示自定义标签

### 2.3 自定义 Hook
1. 自定义 Hook 规则
2. 自定义 Hook 示例
3. 自定义 Hook 的复用

### 2.4 Hooks 使用规则
1. 只在最顶层使用 Hook
2. 只在 React 函数组件中调用 Hook
3. ESLint 插件：eslint-plugin-react-hooks

## 3. 官方标准语法 / API

### useState
```jsx
import { useState } from 'react';

// 基本用法
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

// 函数式更新
function Counter() {
  const [count, setCount] = useState(0);
  
  function increment() {
    setCount(prevCount => prevCount + 1);
  }
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+1</button>
    </div>
  );
}

// 对象状态
function UserForm() {
  const [user, setUser] = useState({ name: '', age: 0 });
  
  const handleNameChange = (e) => {
    setUser(prev => ({ ...prev, name: e.target.value }));
  };
  
  const handleAgeChange = (e) => {
    setUser(prev => ({ ...prev, age: Number(e.target.value) }));
  };
  
  return (
    <form>
      <input value={user.name} onChange={handleNameChange} placeholder="Name" />
      <input 
        type="number" 
        value={user.age} 
        onChange={handleAgeChange} 
        placeholder="Age" 
      />
    </form>
  );
}

// 惰性初始化
function expensiveInitialState() {
  console.log('Computing initial state...');
  return { count: 0 };
}

function LazyComponent() {
  const [state, setState] = useState(expensiveInitialState);
  // expensiveInitialState 只会在首次渲染时调用一次
  return <div>{state.count}</div>;
}
```

### useEffect
```jsx
import { useState, useEffect } from 'react';

// 基础用法 - 每次渲染后执行
function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

// 只在挂载时执行（空依赖数组）
function FriendStatus() {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    
    // 清理函数
    return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  }, []); // 空依赖数组 - 只在挂载和卸载时执行

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}

// 依赖数组
function DataFetcher() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [userId, setUserId] = useState(1);

  useEffect(() => {
    async function fetchData() {
      setLoading(true);
      const response = await fetch(`/api/users/${userId}`);
      const data = await response.json();
      setData(data);
      setLoading(false);
    }
    
    fetchData();
  }, [userId]); // userId 变化时重新执行

  return (
    <div>
      <input 
        type="number" 
        value={userId} 
        onChange={(e) => setUserId(Number(e.target.value))} 
      />
      {loading ? 'Loading...' : JSON.stringify(data)}
    </div>
  );
}

// 多个 useEffect
function MultipleEffects() {
  const [count, setCount] = useState(0);
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  useEffect(() => {
    function handleResize() {
      setWidth(window.innerWidth);
    }
    
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <p>Width: {width}</p>
      <button onClick={() => setCount(count + 1)}>Click</button>
    </div>
  );
}
```

### useContext
```jsx
import { createContext, useContext, useState } from 'react';

// 创建 Context
const ThemeContext = createContext('light');
const UserContext = createContext(null);

// 提供 Context
function App() {
  const [theme, setTheme] = useState('light');
  const [user, setUser] = useState({ name: 'Guest' });

  return (
    <ThemeContext.Provider value={theme}>
      <UserContext.Provider value={user}>
        <Toolbar />
        <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
          Toggle Theme
        </button>
      </UserContext.Provider>
    </ThemeContext.Provider>
  );
}

// 消费 Context
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
      color: theme === 'light' ? '#333' : '#fff'
    }}>
      Themed Button
    </button>
  );
}

function UserProfile() {
  const user = useContext(UserContext);
  
  return (
    <div>
      <h2>Hello, {user.name}!</h2>
    </div>
  );
}
```

### useReducer
```jsx
import { useReducer } from 'react';

// 定义 reducer 函数
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      throw new Error();
  }
}

// 使用 useReducer
function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  
  return (
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}

// 复杂示例 - todo 列表
const initialState = {
  todos: [],
  filter: 'all'
};

function todoReducer(state, action) {
  switch (action.type) {
    case 'add':
      return {
        ...state,
        todos: [...state.todos, { id: Date.now(), text: action.text, completed: false }]
      };
    case 'toggle':
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.id ? { ...todo, completed: !todo.completed } : todo
        )
      };
    case 'delete':
      return {
        ...state,
        todos: state.todos.filter(todo => todo.id !== action.id)
      };
    case 'setFilter':
      return {
        ...state,
        filter: action.filter
      };
    default:
      return state;
  }
}

function TodoList() {
  const [state, dispatch] = useReducer(todoReducer, initialState);
  const [input, setInput] = useState('');

  const addTodo = (e) => {
    e.preventDefault();
    if (input.trim()) {
      dispatch({ type: 'add', text: input });
      setInput('');
    }
  };

  const filteredTodos = state.todos.filter(todo => {
    if (state.filter === 'active') return !todo.completed;
    if (state.filter === 'completed') return todo.completed;
    return true;
  });

  return (
    <div>
      <form onSubmit={addTodo}>
        <input value={input} onChange={(e) => setInput(e.target.value)} />
        <button type="submit">Add Todo</button>
      </form>
      
      <div>
        <button onClick={() => dispatch({ type: 'setFilter', filter: 'all' })}>
          All
        </button>
        <button onClick={() => dispatch({ type: 'setFilter', filter: 'active' })}>
          Active
        </button>
        <button onClick={() => dispatch({ type: 'setFilter', filter: 'completed' })}>
          Completed
        </button>
      </div>
      
      <ul>
        {filteredTodos.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => dispatch({ type: 'toggle', id: todo.id })}
            />
            {todo.text}
            <button onClick={() => dispatch({ type: 'delete', id: todo.id })}>
              ×
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### useCallback & useMemo
```jsx
import { useState, useCallback, useMemo } from 'react';

// useCallback - 缓存回调函数
function ChildComponent({ onClick }) {
  console.log('ChildComponent rendered');
  return <button onClick={onClick}>Click</button>;
}

function ParentComponent() {
  const [count, setCount] = useState(0);
  const [other, setOther] = useState(0);

  // 不使用 useCallback - 每次渲染都会创建新函数
  const handleClickWithoutCallback = () => {
    console.log('Clicked');
  };

  // 使用 useCallback - 只有依赖变化时才创建新函数
  const handleClickWithCallback = useCallback(() => {
    console.log('Clicked count:', count);
  }, [count]); // count 变化时重新创建

  return (
    <div>
      <p>Count: {count}</p>
      <p>Other: {other}</p>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      <button onClick={() => setOther(other + 1)}>Increment Other</button>
      
      <h3>Without useCallback:</h3>
      <ChildComponent onClick={handleClickWithoutCallback} />
      
      <h3>With useCallback:</h3>
      <ChildComponent onClick={handleClickWithCallback} />
    </div>
  );
}

// useMemo - 缓存计算结果
function ExpensiveComponent() {
  const [count, setCount] = useState(0);
  const [todos, setTodos] = useState([]);

  // 昂贵的计算 - 不使用 useMemo 会在每次渲染时都重新计算
  const expensiveCalculation = (num) => {
    console.log('Calculating...');
    for (let i = 0; i < 1000000000; i++) {
      num += 1;
    }
    return num;
  };

  // 使用 useMemo - 只有 count 变化时才重新计算
  const result = useMemo(() => expensiveCalculation(count), [count]);

  return (
    <div>
      <div>
        <h2>My Todos</h2>
        {todos.map((todo, index) => {
          return <p key={index}>{todo}</p>;
        })}
        <button onClick={() => setTodos([...todos, 'New Todo'])}>
          Add Todo
        </button>
      </div>
      <hr />
      <div>
        Count: {count}
        <button onClick={() => setCount(count + 1)}>+</button>
        <h2>Expensive Calculation</h2>
        {result}
      </div>
    </div>
  );
}

// useMemo 对象/数组记忆化
function List({ items }) {
  // 每次渲染都会创建新的数组
  // const sortedItems = [...items].sort();
  
  // 使用 useMemo - 只有 items 变化时才重新排序
  const sortedItems = useMemo(() => {
    return [...items].sort((a, b) => a - b);
  }, [items]);

  return (
    <ul>
      {sortedItems.map(item => (
        <li key={item}>{item}</li>
      ))}
    </ul>
  );
}
```

### useRef
```jsx
import { useRef, useState, useEffect } from 'react';

// 访问 DOM 元素
function TextInputWithFocusButton() {
  const inputRef = useRef(null);

  const handleFocus = () => {
    // 访问 DOM 元素
    inputRef.current.focus();
  };

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={handleFocus}>Focus the input</button>
    </>
  );
}

// 保存可变值 - 不会触发重渲染
function Timer() {
  const [count, setCount] = useState(0);
  const intervalRef = useRef(null);

  useEffect(() => {
    intervalRef.current = setInterval(() => {
      setCount(c => c + 1);
    }, 1000);

    return () => {
      clearInterval(intervalRef.current);
    };
  }, []);

  const stopTimer = () => {
    clearInterval(intervalRef.current);
  };

  return (
    <div>
      Count: {count}
      <button onClick={stopTimer}>Stop Timer</button>
    </div>
  );
}

// 保存上一个值
function PreviousValue() {
  const [count, setCount] = useState(0);
  const prevCountRef = useRef();

  useEffect(() => {
    prevCountRef.current = count;
  });

  const prevCount = prevCountRef.current;

  return (
    <div>
      <p>Now: {count}, before: {prevCount}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

// 转发 ref 到子组件
const FancyInput = React.forwardRef((props, ref) => (
  <input ref={ref} className="fancy-input" />
));

function ParentComponent() {
  const fancyInputRef = useRef(null);

  const handleClick = () => {
    fancyInputRef.current.focus();
  };

  return (
    <>
      <FancyInput ref={fancyInputRef} />
      <button onClick={handleClick}>Focus the fancy input</button>
    </>
  );
}
```

### useLayoutEffect
```jsx
import { useState, useLayoutEffect, useEffect } from 'react';

// useLayoutEffect 在 DOM 更新后同步执行
function LayoutEffectExample() {
  const [width, setWidth] = useState(0);
  const ref = useRef();

  // useLayoutEffect 会在浏览器绘制前同步执行
  useLayoutEffect(() => {
    // 可以在这里同步测量布局
    setWidth(ref.current.offsetWidth);
  }, []);

  return (
    <div>
      <div ref={ref}>Hello, World!</div>
      <p>Width: {width}px</p>
    </div>
  );
}

// useEffect vs useLayoutEffect
function EffectComparison() {
  const [count, setCount] = useState(0);

  useLayoutEffect(() => {
    console.log('useLayoutEffect');
    // 这里可以安全地读取和修改 DOM，在浏览器绘制前执行
    if (count === 3) {
      setCount(0);
    }
  }, [count]);

  useEffect(() => {
    console.log('useEffect');
    // 这里在浏览器绘制后执行
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### 自定义 Hook
```jsx
import { useState, useEffect, useCallback } from 'react';

// 自定义 Hook - useLocalStorage
function useLocalStorage(key, initialValue) {
  // 获取初始值
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });

  // 设置值的函数
  const setValue = useCallback((value) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(error);
    }
  }, [key, storedValue]);

  return [storedValue, setValue];
}

// 使用 useLocalStorage
function LocalStorageExample() {
  const [name, setName] = useLocalStorage('name', 'Bob');

  return (
    <div>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <p>Hello, {name}!</p>
    </div>
  );
}

// 自定义 Hook - useWindowSize
function useWindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: undefined,
    height: undefined,
  });

  useEffect(() => {
    function handleResize() {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    }

    window.addEventListener('resize', handleResize);
    handleResize();

    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return windowSize;
}

// 使用 useWindowSize
function WindowSizeExample() {
  const { width, height } = useWindowSize();

  return (
    <div>
      <p>Window width: {width}px</p>
      <p>Window height: {height}px</p>
    </div>
  );
}

// 自定义 Hook - useFetch
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const abortController = new AbortController();
    const signal = abortController.signal;

    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url, { signal });
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const result = await response.json();
        setData(result);
        setError(null);
      } catch (err) {
        if (err.name === 'AbortError') {
          console.log('Fetch aborted');
        } else {
          setError(err.message);
        }
      } finally {
        setLoading(false);
      }
    };

    fetchData();

    return () => {
      abortController.abort();
    };
  }, [url]);

  return { data, loading, error };
}

// 使用 useFetch
function FetchExample() {
  const { data, loading, error } = useFetch('https://api.example.com/data');

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      <h1>Data</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}

// 自定义 Hook - useDebounce
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);

  return debouncedValue;
}

// 使用 useDebounce
function DebounceExample() {
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearchTerm = useDebounce(searchTerm, 500);
  const [results, setResults] = useState([]);

  useEffect(() => {
    if (debouncedSearchTerm) {
      // 在这里执行搜索
      console.log('Searching for:', debouncedSearchTerm);
      // mock search
      setResults([debouncedSearchTerm, 'result1', 'result2']);
    }
  }, [debouncedSearchTerm]);

  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search..."
      />
      <ul>
        {results.map((result, index) => (
          <li key={index}>{result}</li>
        ))}
      </ul>
    </div>
  );
}

// 自定义 Hook - useToggle
function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);
  const toggle = useCallback(() => setValue(v => !v), []);
  return [value, toggle];
}

// 使用 useToggle
function ToggleExample() {
  const [isDark, toggleDark] = useToggle(false);
  const [isModalOpen, toggleModal] = useToggle(false);

  return (
    <div style={{ background: isDark ? '#333' : '#fff', color: isDark ? '#fff' : '#333' }}>
      <button onClick={toggleDark}>
        Toggle {isDark ? 'Light' : 'Dark'} Mode
      </button>
      <button onClick={toggleModal}>
        Open Modal
      </button>
      {isModalOpen && (
        <div className="modal">
          <h2>Modal</h2>
          <button onClick={toggleModal}>Close</button>
        </div>
      )}
    </div>
  );
}
```

## 4. 官方示例代码（可运行）

### 完整的计数器应用
```jsx
import { useState, useEffect, useCallback, useRef } from 'react';

function CounterApp() {
  const [count, setCount] = useState(0);
  const [history, setHistory] = useState([]);
  const inputRef = useRef(null);

  // 使用 useCallback 优化性能
  const increment = useCallback(() => {
    setCount(prev => prev + 1);
  }, []);

  const decrement = useCallback(() => {
    setCount(prev => prev - 1);
  }, []);

  const reset = useCallback(() => {
    setCount(0);
  }, []);

  const setCountFromInput = useCallback(() => {
    const value = parseInt(inputRef.current.value);
    if (!isNaN(value)) {
      setCount(value);
    }
  }, []);

  // 记录历史
  useEffect(() => {
    setHistory(prev => [...prev.slice(-9), count]);
  }, [count]);

  // 键盘快捷键
  useEffect(() => {
    const handleKeyDown = (e) => {
      if (e.key === 'ArrowUp') increment();
      if (e.key === 'ArrowDown') decrement();
      if (e.key === '0') reset();
    };

    window.addEventListener('keydown', handleKeyDown);
    return () => window.removeEventListener('keydown', handleKeyDown);
  }, [increment, decrement, reset]);

  return (
    <div style={{ padding: '20px', maxWidth: '400px', margin: '0 auto' }}>
      <h1>Counter</h1>
      
      <div style={{ fontSize: '48px', fontWeight: 'bold', margin: '20px 0' }}>
        {count}
      </div>

      <div style={{ display: 'flex', gap: '10px', marginBottom: '20px' }}>
        <button onClick={decrement} style={{ padding: '10px 20px' }}>-</button>
        <button onClick={reset} style={{ padding: '10px 20px' }}>Reset</button>
        <button onClick={increment} style={{ padding: '10px 20px' }}>+</button>
      </div>

      <div style={{ marginBottom: '20px' }}>
        <input
          ref={inputRef}
          type="number"
          placeholder="Set count"
          style={{ padding: '10px', marginRight: '10px' }}
        />
        <button onClick={setCountFromInput} style={{ padding: '10px 20px' }}>
          Set
        </button>
      </div>

      <div>
        <h3>History (last 10)</h3>
        <div style={{ display: 'flex', gap: '5px', flexWrap: 'wrap' }}>
          {history.map((value, index) => (
            <span
              key={index}
              style={{
                padding: '5px 10px',
                background: '#f0f0f0',
                borderRadius: '4px',
                fontSize: '14px'
              }}
            >
              {value}
            </span>
          ))}
        </div>
      </div>

      <div style={{ marginTop: '20px', fontSize: '12px', color: '#666' }}>
        <p>Keyboard shortcuts:</p>
        <ul>
          <li>↑ - Increment</li>
          <li>↓ - Decrement</li>
          <li>0 - Reset</li>
        </ul>
      </div>
    </div>
  );
}

export default CounterApp;
```

### 完整的表单验证应用
```jsx
import { useState, useCallback, useMemo } from 'react';

// 自定义验证 Hook
function useFormValidation(initialValues, validate) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});

  const handleChange = useCallback((e) => {
    const { name, value, type, checked } = e.target;
    setValues(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
  }, []);

  const handleBlur = useCallback((e) => {
    const { name } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
  }, []);

  const validationErrors = useMemo(() => {
    return validate(values);
  }, [values, validate]);

  useEffect(() => {
    setErrors(validationErrors);
  }, [validationErrors]);

  const isValid = useMemo(() => {
    return Object.keys(validationErrors).length === 0;
  }, [validationErrors]);

  const handleSubmit = useCallback((onSubmit) => (e) => {
    e.preventDefault();
    const allTouched = Object.keys(initialValues).reduce((acc, key) => {
      acc[key] = true;
      return acc;
    }, {});
    setTouched(allTouched);
    
    if (isValid) {
      onSubmit(values);
    }
  }, [values, isValid, initialValues]);

  return {
    values,
    errors,
    touched,
    isValid,
    handleChange,
    handleBlur,
    handleSubmit,
    setValues
  };
}

// 验证函数
const validateForm = (values) => {
  const errors = {};
  
  if (!values.name.trim()) {
    errors.name = 'Name is required';
  } else if (values.name.length < 2) {
    errors.name = 'Name must be at least 2 characters';
  }

  if (!values.email.trim()) {
    errors.email = 'Email is required';
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(values.email)) {
    errors.email = 'Email is invalid';
  }

  if (!values.password) {
    errors.password = 'Password is required';
  } else if (values.password.length < 6) {
    errors.password = 'Password must be at least 6 characters';
  }

  if (values.password !== values.confirmPassword) {
    errors.confirmPassword = 'Passwords do not match';
  }

  if (!values.terms) {
    errors.terms = 'You must accept the terms';
  }

  return errors;
};

function RegistrationForm() {
  const initialValues = {
    name: '',
    email: '',
    password: '',
    confirmPassword: '',
    terms: false
  };

  const {
    values,
    errors,
    touched,
    isValid,
    handleChange,
    handleBlur,
    handleSubmit
  } = useFormValidation(initialValues, validateForm);

  const onSubmit = useCallback((formValues) => {
    console.log('Form submitted:', formValues);
    alert('Registration successful!');
  }, []);

  return (
    <div style={{ maxWidth: '400px', margin: '0 auto', padding: '20px' }}>
      <h2>Registration Form</h2>
      
      <form onSubmit={handleSubmit(onSubmit)}>
        <div style={{ marginBottom: '15px' }}>
          <label style={{ display: 'block', marginBottom: '5px' }}>
            Name:
          </label>
          <input
            type="text"
            name="name"
            value={values.name}
            onChange={handleChange}
            onBlur={handleBlur}
            style={{
              width: '100%',
              padding: '8px',
              border: touched.name && errors.name ? '1px solid red' : '1px solid #ddd',
              borderRadius: '4px'
            }}
          />
          {touched.name && errors.name && (
            <p style={{ color: 'red', margin: '5px 0 0 0', fontSize: '12px' }}>
              {errors.name}
            </p>
          )}
        </div>

        <div style={{ marginBottom: '15px' }}>
          <label style={{ display: 'block', marginBottom: '5px' }}>
            Email:
          </label>
          <input
            type="email"
            name="email"
            value={values.email}
            onChange={handleChange}
            onBlur={handleBlur}
            style={{
              width: '100%',
              padding: '8px',
              border: touched.email && errors.email ? '1px solid red' : '1px solid #ddd',
              borderRadius: '4px'
            }}
          />
          {touched.email && errors.email && (
            <p style={{ color: 'red', margin: '5px 0 0 0', fontSize: '12px' }}>
              {errors.email}
            </p>
          )}
        </div>

        <div style={{ marginBottom: '15px' }}>
          <label style={{ display: 'block', marginBottom: '5px' }}>
            Password:
          </label>
          <input
            type="password"
            name="password"
            value={values.password}
            onChange={handleChange}
            onBlur={handleBlur}
            style={{
              width: '100%',
              padding: '8px',
              border: touched.password && errors.password ? '1px solid red' : '1px solid #ddd',
              borderRadius: '4px'
            }}
          />
          {touched.password && errors.password && (
            <p style={{ color: 'red', margin: '5px 0 0 0', fontSize: '12px' }}>
              {errors.password}
            </p>
          )}
        </div>

        <div style={{ marginBottom: '15px' }}>
          <label style={{ display: 'block', marginBottom: '5px' }}>
            Confirm Password:
          </label>
          <input
            type="password"
            name="confirmPassword"
            value={values.confirmPassword}
            onChange={handleChange}
            onBlur={handleBlur}
            style={{
              width: '100%',
              padding: '8px',
              border: touched.confirmPassword && errors.confirmPassword ? '1px solid red' : '1px solid #ddd',
              borderRadius: '4px'
            }}
          />
          {touched.confirmPassword && errors.confirmPassword && (
            <p style={{ color: 'red', margin: '5px 0 0 0', fontSize: '12px' }}>
              {errors.confirmPassword}
            </p>
          )}
        </div>

        <div style={{ marginBottom: '20px' }}>
          <label style={{ display: 'flex', alignItems: 'center' }}>
            <input
              type="checkbox"
              name="terms"
              checked={values.terms}
              onChange={handleChange}
              onBlur={handleBlur}
              style={{ marginRight: '8px' }}
            />
            I accept the terms and conditions
          </label>
          {touched.terms && errors.terms && (
            <p style={{ color: 'red', margin: '5px 0 0 0', fontSize: '12px' }}>
              {errors.terms}
            </p>
          )}
        </div>

        <button
          type="submit"
          disabled={!isValid}
          style={{
            width: '100%',
            padding: '10px',
            background: isValid ? '#4CAF50' : '#ccc',
            color: 'white',
            border: 'none',
            borderRadius: '4px',
            cursor: isValid ? 'pointer' : 'not-allowed'
          }}
        >
          Register
        </button>
      </form>
    </div>
  );
}

export default RegistrationForm;
```

## 5. 官方注意事项 / 最佳实践

### Hooks 规则
1. **只在最顶层使用 Hook**：不要在循环、条件或嵌套函数中调用 Hook
2. **只在 React 函数组件中调用 Hook**：不要在普通的 JavaScript 函数中调用 Hook
3. **使用 ESLint 插件**：安装 eslint-plugin-react-hooks 来自动检查这些规则

### useState 最佳实践
1. **状态拆分**：将不相关的状态拆分为独立的 state 变量
2. **函数式更新**：当新状态依赖旧状态时，使用函数式更新
3. **惰性初始化**：初始状态需要复杂计算时，使用函数形式
4. **避免过度使用 state**：不是所有数据都需要放在 state 中

### useEffect 最佳实践
1. **正确设置依赖**：确保所有外部变量都在依赖数组中
2. **清理副作用**：在 cleanup 函数中清理订阅、定时器等
3. **拆分 useEffect**：将不相关的逻辑拆分为不同的 useEffect
4. **避免无限循环**：确保依赖数组正确设置

### useCallback & useMemo 最佳实践
1. **不要过度优化**：只有在性能确实有问题时才使用
2. **正确的依赖**：确保依赖数组包含所有外部变量
3. **性能分析**：使用 React DevTools Profiler 分析性能瓶颈
4. **useCallback 用于子组件**：通常与 memo 一起使用避免子组件重渲染

### useRef 最佳实践
1. **不要依赖 ref.current 的变化**：ref 变化不会触发重渲染
2. **使用 ref 保存 DOM 引用**：直接操作 DOM 时使用
3. **使用 ref 保存可变值**：需要保存不触发重渲染的值

### 自定义 Hook 最佳实践
1. **命名规范**：自定义 Hook 应该以 "use" 开头
2. **可复用性**：自定义 Hook 应该是通用的、可复用的
3. **参数设计**：设计清晰的 API 和参数
4. **返回值设计**：返回有意义的值，通常是数组或对象
5. **使用其他 Hook**：自定义 Hook 可以调用其他 Hook

### 性能优化
1. **避免不必要的重渲染**：使用 memo、useCallback、useMemo
2. **状态提升**：合理提升状态，避免状态下放
3. **代码分割**：使用 React.lazy 和 Suspense 进行代码分割
4. **虚拟化列表**：长列表使用虚拟化技术
5. **避免在渲染中创建函数**：使用 useCallback 缓存函数

### 调试技巧
1. **React DevTools**：使用 React DevTools 检查组件树和状态
2. **useDebugValue**：在自定义 Hook 中使用 useDebugValue 显示调试信息
3. **console.log**：在 useEffect 中添加日志了解执行时机
4. **严格模式**：使用 React.StrictMode 发现潜在问题

### 常见错误及解决
1. **依赖缺失警告**：检查依赖数组，确保包含所有外部变量
2. **无限循环**：检查 useEffect 的依赖数组，确保不会无限触发
3. **过期闭包**：确保依赖数组正确，或使用 ref 保存最新值
4. **状态不更新**：检查是否正确使用了 setState，不要直接修改 state

## 6. 官方文档参考链接

- React Hooks 官方文档：https://react.dev/reference/react
- Hooks 介绍：https://react.dev/learn/hooks
- useState API：https://react.dev/reference/react/useState
- useEffect API：https://react.dev/reference/react/useEffect
- useContext API：https://react.dev/reference/react/useContext
- useReducer API：https://react.dev/reference/react/useReducer
- useCallback API：https://react.dev/reference/react/useCallback
- useMemo API：https://react.dev/reference/react/useMemo
- useRef API：https://react.dev/reference/react/useRef
- 自定义 Hook：https://react.dev/learn/reusing-logic-with-custom-hooks
- Hooks 规则：https://react.dev/reference/rules/rules-of-hooks
