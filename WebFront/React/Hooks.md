# React Hooks

## 为什么使用Hooks

### 类组件的缺点

这是一个简单的Button类组件

```javascript
import React, { Component } from "react";

export default class Button extends Component {
  constructor() {
    super();
    this.state = { buttonText: "Click me, please" };
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    this.setState(() => {
      return { buttonText: "Thanks, been clicked!" };
    });
  }
  render() {
    const { buttonText } = this.state;
    return <button onClick={this.handleClick}>{buttonText}</button>;
  }
}
```

可以组件类过于复杂

- 大型组件很难拆分和重构，也很难测试。
- 业务逻辑分散在组件的各个方法之中，导致重复逻辑或关联逻辑。
- 组件类引入了复杂的编程模式，比如 render props 和高阶组件。

这时候我们可以选择使用函数组件替代类组件

### 函数组件的优势

函数组件代码简单, 其次没有生命周期, 也保证了一定的执行效率

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

但是函数组件要求使用纯函数, 不能包含状态, 也不支持生命周期, 无法取代类组件。

**React Hooks 说白了就是用来加强函数组件，在不使用类的情况下, 也能够完成状态生命周期等**

Hook使用场景：
 Hook的出现基本可以替代class组件（除了个别场景）；
 若项目比较旧，并不需要直接将所有代码重构为Hooks，因为它完全向下兼容，可以渐进式地来使用；
 **Hook只能在函数组件中使用，不能在类组件，或者函数组件之外的地方使用；**

## 什么是Hooks

Hook 指的是钩子, 而Hooks 指的就是需要用来增强函数组件的一系列钩子。

需要使用什么功能, 就可以使用对应的钩子, 所有的钩子都已use开头命名, 所有钩子都是为函数提供额外功能。

四个最常用的生命周期钩子

- useState()
- useContext()
- useReducer()
- useEffect()



### useState() 状态钩子

纯函数不能拥有状态, 我们使用useState()为函数添加状态

useState接受一个默认值(可以是对象), 返回数组, 包含两个参数, 指向状态的变量和设置状态的函数

类似于state和setState, 这也是约定的命名格式。

设置状态函数 接受用以替换的对象, 或一个函数(类比this.setState)

我们可以在一个函数组件中我们可以创建多个状态钩子**

```javascript
import React, { useState } from "react";

export default function  Button()  {
  const  [buttonText, setButtonText] =  useState("Click me,   please");

  function handleClick()  {
    return setButtonText("Thanks, been clicked!");
  }

  return  <button  onClick={handleClick}>{buttonText}</button>;
}
```

你也可以将多个state合并到一个对象中,  然而, 不像 class 中的 `this.setState`，**更新 state 变量总是*替换*它而不是合并它。**



### useEffect() 副作用钩子

你可以把 `useEffect` Hook 看做 `componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount` 这三个函数的组合。

它接受一个函数作为参数, 在每次render后都会自动执行

在React中有两种常见的副作用操作: 需要清除的和不需要清除的

#### 无需清除的effect

一般指的是我们在DOM更新后顺便执行的额外代码, 比如发送网络请求, 手动变更DOM, 记录日志, 这些都是常见的无需清楚操作。

类组件:

```javascript
componentDidMount() {    document.title = `You clicked ${this.state.count} times`;  }  componentDidUpdate() {    document.title = `You clicked ${this.state.count} times`;  }
```

函数组件:

```javascript
useEffect(() => {    document.title = `You clicked ${count} times`;  });
```

- useEffect接受一个函数作为effect, 在执行DOM更新之后调用它。**所以effect每次都会获取最新值**

- Hook 使用了 JavaScript 的闭包机制，而不用在 JavaScript 已经提供了解决方案的情况下，还引入特定的 React API。

-  默认情况下，effect在第一次渲染之后和每次**DOM更新完成**之后都会执行。

- 每次渲染都会生成新的effect, 替代之前的, 某种意义上每个effect属于每次特定的渲染

  

- 与 `componentDidMount` 或 `componentDidUpdate` 不同，使用 `useEffect` 调度的 effect 不会阻塞浏览器更新屏幕，这让你的应用看起来响应更快。大多数情况下，effect 不需要同步地执行。在个别情况下（例如测量布局），有单独的 [`useLayoutEffect`](https://zh-hans.reactjs.org/docs/hooks-reference.html#uselayouteffect) Hook 供你使用，其 API 与 `useEffect` 相同。

#### 需要清除的effect

有些effect是需要清除的, 包括订阅外部数据源, 清除可以有效防止内存泄漏

类组件:

我们通常在 `componentDidMount` 中设置订阅，并在 `componentWillUnmount` 中清除它。

假设我们有一个 `ChatAPI` 模块，它允许我们订阅好友的在线状态。以下是我们如何使用 class 订阅和显示该状态：

```react
class FriendStatus extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isOnline: null };
    this.handleStatusChange = this.handleStatusChange.bind(this);
  }

  componentDidMount() {    
      ChatAPI.subscribeToFriendStatus(
          this.props.friend.id,
          this.handleStatusChange    
      );  
  }  
  componentWillUnmount() {
      ChatAPI.unsubscribeFromFriendStatus(      
          this.props.friend.id,      
          this.handleStatusChange    
      );  
  }  
  handleStatusChange(status) {    
        this.setState({      
            isOnline: status.isOnline    
        });  
  }
  render() {
    if (this.state.isOnline === null) {
      return 'Loading...';
    }
    return this.state.isOnline ? 'Online' : 'Offline';
  }
}
```

你会注意到 `componentDidMount` 和 `componentWillUnmount` 之间相互对应。

使用生命周期函数迫使我们拆分这些逻辑代码，即使这两部分代码都作用于相同的副作用。

函数组件

```react
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {    
  	function handleStatusChange(status) {      
  		setIsOnline(status.isOnline);    
  	}    
  	ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);    
  	// Specify how to clean up after this effect:    
  	return function cleanup() {      
  		ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);    
  	};  
  });
  
  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

出于对添加和删除订阅的代码的紧密性的考虑， `useEffect` 设计清除effect是在同一个地方执行。

**可选的, 如果你的 effect 返回一个函数，React 将会在执行清除操作时调用它, 每次运行effect都会执行清除!**

effect 在每次渲染的时候都会执行。这就是为什么 React *会*在执行当前 effect 之前对上一个 effect 进行清除。

**不得不说的,  函数组件中也可以多次调用useEffect, 我们可以按照用途来分离他们。**



#### 性能优化

首先必须了解, 为什么effect每次运行都要执行清除effect

这是因为effect如果仅在componentWillUnmount时清除, 中间过程如果发生props变化, 极有可能导致bug

就上述根据好友id订阅例子, 如果不在componentWillUpdate中针对好友id变更做处理, 则会导致清除时内存指向错误

而effect 在每次运行都生成新的effect, 清除旧的effect, 就可以保证内容一直是最新最正确的



那么重复的渲染也可能导致一系列性能问题,  `componentDidUpdate` 通过添加对 `prevProps` 或 `prevState` 的比较逻辑解决

```react
componentDidUpdate(prevProps, prevState) {
  if (prevState.count !== this.state.count) {
    document.title = `You clicked ${this.state.count} times`;
  }
}
```

而 useEffect 接受第二个参数, 参数为**数组**, 表示监听变化的State变量, **仅当第一次执行和State数组发生变化时执行**

**请确保数组包含所有外部作用域中会随时间变化并且在 effect 中使用的变量**, 否则将引用初次使用的变量值

如果数组为 [], 则仅在初次渲染时运行

未来版本可能会在构建的时候自动添加第二个参数



#### 如果我的 effect 的依赖频繁变化，我该怎么办？

我们通常会设置 数组为空 来避免频繁更新

```react
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1); // 这个 effect 依赖于 `count` state    }, 1000);
    return () => clearInterval(id);
  }, []); // 🔴 Bug: `count` 没有被指定为依赖
  return <h1>{count}</h1>;
}
```

但是由于effect 处于一个闭包中, 所以effect中的state变量是一个死值, 只有当重新生成effect的时候才会变化

而上述代码count不被监听, 所以执行结果始终为 setCount(0+1)

指定 `[count]` 作为依赖列表就能修复这个 Bug，但会导致每次改变发生时定时器都被重置。并且事实上，每个 `setInterval` 在被清除前（类似于 `setTimeout`）都会调用一次。但这并不是我们想要的。

这时候我们可以使用setState的函数式更新形式, 它允许我们指定state如何改变但是不引用当前state

setState(prevState => newState)

```react
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(c => c + 1); // ✅ 在这不依赖于外部的 `count` 变量    }, 1000);
    return () => clearInterval(id);
  }, []); // ✅ 我们的 effect 不适用组件作用域中的任何变量
  return <h1>{count}</h1>;
}
```



### useContext() 共享状态钩子

首先需要创建Context, 然后使用Context.Provide包裹子组件, 在Context.Provide中传递属性

同一个组件树下的子组件可以通过 let property = useContext(Context) 获取属性property

```react
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);  
  return (    
      <button style={{ background: theme.background, color: theme.foreground }}>
      	I am styled by theme context!    
      </button>  
	);
}
```

### 自定义hooks

首先我们必须保证 hooks被使用于顶级React函数中, 不能包含在判断, 循环语句中

因为hooks总是按照声明顺序执行, 如果期间顺序变化会导致后续所有hooks 失效, 出现bug



以下是一个自定义hooks

```javascript
import { useState, useEffect } from 'react';

function useFriendStatus(friendID) {  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

核心逻辑

```javascript
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  // ...

  return isOnline;
}
```

使用自定义hooks

```javascript
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);
  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

```javascript
function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);
  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```

