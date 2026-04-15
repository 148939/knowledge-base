# 检索索引：本文档收录【React】【核心基础】知识点，内容100%来自React官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

React 是一个用于构建用户界面的 JavaScript 库。它由 Facebook 开发和维护，被广泛用于构建单页面应用（SPA）。React 的核心思想是组件化，将 UI 拆分为独立可复用的组件，每个组件都有自己的状态和生命周期。

## 2. 核心知识点（结构化层级）

### 2.1 JSX 语法
1. JSX 简介
2. JSX 表达式
3. JSX 属性
4. JSX 嵌套
5. JSX 与 HTML 的区别

### 2.2 组件基础
1. 函数组件
2. 类组件
3. 组件渲染
4. 组合组件
5. 提取组件

### 2.3 Props
1. Props 传递
2. Props 只读
3. Props 验证
4. 默认 Props

### 2.4 State 与生命周期
1. State 概念
2. setState 使用
3. 生命周期方法（类组件）
4. 状态提升

### 2.5 事件处理
1. 事件绑定
2. 事件对象
3. 传递参数
4. 阻止默认行为

### 2.6 条件渲染
1. if/else
2. 三元运算符
3. 逻辑与运算符
4. 阻止组件渲染

### 2.7 列表渲染
1. 渲染多个组件
2. key 属性
3. 提取列表组件

### 2.8 表单
1. 受控组件
2. 非受控组件
3. textarea
4. select
5. 处理多个输入

## 3. 官方标准语法 / API

### 函数组件语法
```jsx
// 基础函数组件
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// 箭头函数组件
const Welcome = (props) => {
  return <h1>Hello, {props.name}</h1>;
};

// 简化箭头函数组件
const Welcome = ({ name }) => <h1>Hello, {name}</h1>;
```

### 类组件语法
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### State 使用语法
```jsx
// 类组件中的 State
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Increment
        </button>
      </div>
    );
  }
}

// 函数式 setState
this.setState((state, props) => ({
  count: state.count + props.increment
}));
```

### 事件处理语法
```jsx
// 事件绑定
<button onClick={handleClick}>Click me</button>

// 类组件中绑定
class Button extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick() {
    console.log('Clicked');
  }
  
  render() {
    return <button onClick={this.handleClick}>Click</button>;
  }
}

// 箭头函数绑定
class Button extends React.Component {
  handleClick = () => {
    console.log('Clicked');
  };
  
  render() {
    return <button onClick={this.handleClick}>Click</button>;
  }
}

// 传递参数
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
```

### 条件渲染语法
```jsx
// if/else
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

// 三元运算符
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? <UserGreeting /> : <GuestGreeting />}
    </div>
  );
}

// 逻辑与
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>You have {unreadMessages.length} unread messages.</h2>
      }
    </div>
  );
}

// 阻止渲染
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }
  return <div className="warning">Warning!</div>;
}
```

### 列表渲染语法
```jsx
// 基础列表渲染
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>{number}</li>
  );
  return <ul>{listItems}</ul>;
}

// 提取组件
function ListItem(props) {
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()} value={number} />
      )}
    </ul>
  );
}
```

### 表单语法
```jsx
// 受控组件
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = { value: '' };
  }

  handleChange = (event) => {
    this.setState({ value: event.target.value });
  }

  handleSubmit = (event) => {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}

// 处理多个输入
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };
  }

  handleInputChange = (event) => {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

## 4. 官方示例代码（可运行）

### 待办事项应用示例
```jsx
import React, { useState } from 'react';

function TodoApp() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Learn React', completed: false },
    { id: 2, text: 'Build a todo app', completed: false }
  ]);
  const [newTodoText, setNewTodoText] = useState('');

  function handleAddTodo(e) {
    e.preventDefault();
    if (newTodoText.trim()) {
      setTodos([
        ...todos,
        {
          id: Date.now(),
          text: newTodoText.trim(),
          completed: false
        }
      ]);
      setNewTodoText('');
    }
  }

  function toggleTodo(id) {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  }

  function deleteTodo(id) {
    setTodos(todos.filter(todo => todo.id !== id));
  }

  const completedCount = todos.filter(todo => todo.completed).length;
  const itemsLeft = todos.length - completedCount;

  return (
    <div style={{ maxWidth: '400px', margin: '2rem auto', padding: '0 1rem' }}>
      <h1>Todo App</h1>
      
      <form onSubmit={handleAddTodo}>
        <input
          type="text"
          value={newTodoText}
          onChange={(e) => setNewTodoText(e.target.value)}
          placeholder="What needs to be done?"
          style={{ width: '70%', padding: '0.5rem', marginRight: '0.5rem' }}
        />
        <button type="submit" style={{ padding: '0.5rem 1rem' }}>
          Add
        </button>
      </form>

      <ul style={{ listStyle: 'none', padding: 0 }}>
        {todos.map(todo => (
          <li key={todo.id} style={{ 
            padding: '0.5rem', 
            borderBottom: '1px solid #eee',
            display: 'flex',
            justifyContent: 'space-between',
            alignItems: 'center'
          }}>
            <div>
              <input
                type="checkbox"
                checked={todo.completed}
                onChange={() => toggleTodo(todo.id)}
                style={{ marginRight: '0.5rem' }}
              />
              <span style={{ 
                textDecoration: todo.completed ? 'line-through' : 'none',
                color: todo.completed ? '#999' : 'inherit'
              }}>
                {todo.text}
              </span>
            </div>
            <button 
              onClick={() => deleteTodo(todo.id)}
              style={{ 
                background: '#ff4444', 
                color: 'white', 
                border: 'none',
                padding: '0.25rem 0.5rem',
                borderRadius: '4px',
                cursor: 'pointer'
              }}
            >
              ×
            </button>
          </li>
        ))}
      </ul>

      <div style={{ marginTop: '1rem', color: '#666' }}>
        {itemsLeft} items left
        {completedCount > 0 && (
          <button 
            onClick={() => setTodos(todos.filter(todo => !todo.completed))}
            style={{ marginLeft: '1rem' }}
          >
            Clear completed
          </button>
        )}
      </div>
    </div>
  );
}

export default TodoApp;
```

### 温度转换器示例
```jsx
import React, { useState } from 'react';

const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}

function tryConvert(temperature, convert) {
  const input = parseFloat(temperature);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}

function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}

function TemperatureInput(props) {
  function handleChange(e) {
    props.onTemperatureChange(e.target.value);
  }

  return (
    <fieldset>
      <legend>Enter temperature in {scaleNames[props.scale]}:</legend>
      <input
        value={props.temperature}
        onChange={handleChange} />
    </fieldset>
  );
}

function Calculator() {
  const [temperature, setTemperature] = useState('');
  const [scale, setScale] = useState('c');

  function handleCelsiusChange(temperature) {
    setTemperature(temperature);
    setScale('c');
  }

  function handleFahrenheitChange(temperature) {
    setTemperature(temperature);
    setScale('f');
  }

  const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
  const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

  return (
    <div>
      <TemperatureInput
        scale="c"
        temperature={celsius}
        onTemperatureChange={handleCelsiusChange} />
      <TemperatureInput
        scale="f"
        temperature={fahrenheit}
        onTemperatureChange={handleFahrenheitChange} />
      <BoilingVerdict
        celsius={parseFloat(celsius)} />
    </div>
  );
}

export default Calculator;
```

## 5. 官方注意事项 / 最佳实践

### JSX 注意事项
1. **JSX 是表达式**：可以在 if、for 循环中使用，可以赋值给变量，可以作为参数传递，可以作为返回值
2. **JSX 属性命名**：使用驼峰命名法，如 className 而非 class，onClick 而非 onclick
3. **自闭合标签**：没有子元素的标签必须闭合，如 <img />
4. **JSX 防止注入攻击**：React DOM 在渲染前会转义所有嵌入的值，防止 XSS 攻击

### 组件最佳实践
1. **单一职责**：每个组件只负责一个功能
2. **可复用性**：设计组件时考虑复用性
3. **Props 向下传递**：通过 props 传递数据，不要依赖全局变量
4. **状态提升**：共享状态提升到最近的共同父组件
5. **组合优于继承**：使用组合而非继承来复用代码

### State 最佳实践
1. **State 是局部的**：State 只在组件内部有效
2. **不要直接修改 State**：使用 setState 修改
3. **State 更新可能是异步的**：如果新状态依赖旧状态，使用函数式 setState
4. **State 更新会合并**：setState 会合并对象，不会替换整个 state
5. **最小化 State**：只把真正需要响应式的数据放在 state 中

### 事件处理注意事项
1. **事件命名**：使用驼峰命名法
2. **this 绑定**：类组件中注意 this 的绑定
3. **传递事件对象**：需要时显式传递事件对象
4. **阻止默认行为**：使用 e.preventDefault() 而非 return false

### 列表渲染最佳实践
1. **始终使用 key**：为列表项提供唯一的 key
2. **key 应该稳定**：不要使用索引作为 key，除非列表是静态的
3. **key 在兄弟元素中唯一**：不需要全局唯一，只需要在兄弟元素中唯一
4. **提取列表组件**：将列表项提取为单独的组件

### 表单最佳实践
1. **优先使用受控组件**：大多数情况下使用受控组件
2. **非受控组件使用场景**：文件上传等特殊情况使用非受控组件
3. **处理多个输入**：使用 name 属性和计算属性名处理多个输入
4. **表单验证**：在客户端进行表单验证

## 6. 官方文档参考链接

- React 官方文档：https://react.dev/
- React 入门教程：https://react.dev/learn
- React API 参考：https://react.dev/reference/react
- React 旧版文档（中文）：https://zh-hans.reactjs.org/
