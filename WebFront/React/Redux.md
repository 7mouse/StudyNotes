# Redux基础

包含基础概念和API, 如何通过用户发出 Action，Reducer 函数算出新的 State，然后View 重新渲染。

## 什么是Redux

Redux是一个React环境中的数据管理工具, 应用于**多交互, 多数据源**

### 应用场景:

- 多个组件之间的交互复杂
  - 需要共享数据
  - 其状态必须在任何地方获取
  - 需要改变全局状态
  - 需要跨父子改变另一个组件状态
- 不同用户之间使用方式不同=>用户权限不同, 利用redux控制
- views需要从多个来源获取数据

### 基本概念:

Redux Flow包含四个部分: 

- ActionCreators 动作生成器

- Store 存储器

- Reducers 减压器

  释义: It's called a reducer because it's the type of function you would pass to [Array.prototype.reduce(reducer, ?initialValue)]

  它被叫做reducer 是因为 它与被传入Array.prototype.reduce的函数是属于同一类型 (纯函数)

  reduce接受回调函数 (prev, cur) => return newValue

  reducer模型 (previosState, action) => newState

### 工作流程:

- 首先, 用户发出Action

  ```javascript
  store.dispatch(action)
  ```

- Store会将action自动转交给Reducer处理 (prevState, Action) => newState

  ```javascript
  let nextState = todoApp(previousState, action)
  ```

- State一旦发生变化, Store就会触发监听函数

  ```javascript
  store.subscribe(listener)
  ```

- listener 可以通过 store.getState() 得到当前状态。可以在此时更新视图

  ```javascript
  function listener() {
      let newState = store.getState();
      component.setState(()=>newState);
  }
  ```

![img](F:\Notes\WebFront\React\Redux.assets\bg2016091802.jpg)

## Redux 基础概念

### Store

store 是保存数据的地方, 相当于一个容器, **整个引用只能有一个Store**

- 创建一个Store

  ```javascript
  import {createStore} from 'redux';
  const store = createStore(reducers);
  ```

  createStore 接受一个函数参数,  reducers将负责进行action的处理, 并将结果反馈于store

### State

store对象存储数据, 而State就是store的一个时间的快照, 可以通过 store.getState() 获取

```javascript
import { createStore } from 'redux';
const store = createStore(fn);

const state = store.getState();
```

Redux规定, 一个State对应一个View, 只要State相同, View就相同。

### Action

在MVC框架中, 用户只能通过View的交互操作来造成State的变化, 而我们不允许直接更新Store中的State

此时就需要向Store发出Action通知, 表示State要变化了

Action 是一个对象, 其中的type属性是必须的, 表示Action的名称。 其他属性将作为携带信息

```javascript
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};
```

Action表示的是当前的动作, 将要发生的事情, 然后将当前的情况告诉store请求变化

**store.dispatch(action) 是view向store发出action的唯一方法**

### ActionCreators

View与store交互频繁, 我们不可能一次次手动生成action, 这里就可以采用工厂模式

```javascript
const ADD_TODO = '添加 TODO'; 
// 传递拼写错误type字符串并不会报错, 也不会执行Reducers
// 所以转化为变量, 调用undefined变量会报错, 方便调试

function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}

const action = addTodo('Learn Redux');
```

其中也可以将类型封装, 生成函数封装

```javascript
// store/actionTyoes
export const ADD_TODO = 'add_todo';

// store/actionCreators
export const addTodo(){}
	// 或
const actionCreator = {
    addTodo(){}
}
Object.freeze(actionCreator); //防止修改内容函数, 导致store安全性
export actionCreators;
```

### Reducer

Store收到一个Action后需要进行处理, 来获取newState, 这个计算过程成为reducer

Reducer是一个函数, 接受当前State和Action作为参数, 并返回一个新的State

```javascript
const reducer = function(state, action) {
	// if (action.type === '...')...
	return state;
}
```

可以添加defaultState 作为一个默认初始State

```javascript
const defaultState = 0;
const reducer = (state = defaultState, action) => {
  switch (action.type) {
    case 'ADD':
      return state + action.payload;
    default: 
      return state;
  }
};

const state = reducer(1, {
  type: 'ADD',
  payload: 2
});
```

Reducers **必须是纯函数**, 不得改变传入的state

#### 纯函数

- Reducer 函数最重要的特征是，它是一个纯函数。也就是说，只要是同样的输入，必定得到同样的输出。

  不存在随机性(Date, Math.random())

-  并且无副作用, 不对参数进行修改, 不影响元数据

纯函数编写规范

- 不得改写参数
- 不能调用系统 I/O 的API
- 不能调用`Date.now()`或者`Math.random()`等不纯的方法，因为每次会得到不一样的结果

Reducer编写规范

```javascript
// State 是一个对象
function reducer(state, action) {
  return Object.assign({}, state, { thingToChange });
  // 或者
  return { ...state, ...newState };
}

// State 是一个数组
function reducer(state, action) {
  return [...state, newItem];
}
```

最好把 State 对象设成只读。你没法改变它，要得到新的 State，唯一办法就是生成一个新对象。这样的好处是，任何时候，与某个 View 对应的 State 总是一个不变的对象。



### Store监听函数

设置监听

``` javascript
import { createStore } from 'redux';
const store = createStore(reducer);

store.subscribe(listener);
```

解除监听

store.subscribe会返回一个解除监听的方法

```javascript
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);

unsubscribe();
```



## Store的实现

store提供了三个API:

- store.getState()
- store.dispatch()
- store.subscribe()

```javascript
import { createStore } from 'redux';
let { subscribe, dispatch, getState } = createStore(reducer);
```

createStore还接受第二个参数, 表示state初始状态

```javascript
let store = createStore(todoApp, window.STATE_FROM_SERVER)
```

window.STATE_FROM_SERVER表示State的最初状态, 一般由服务器给出, 它会覆盖Reducer的默认初始值

```javascript
// 简单实现
const createStore = (reducer) => {
  let state;
  let listeners = []; //订阅者模式

  const getState = () => state;

  const dispatch = (action) => {
    state = reducer(state, action);
    listeners.forEach(listener => listener());
  };

  const subscribe = (listener) => {
    listeners.push(listener);
    return () => {
      listeners = listeners.filter(l => l !== listener);
    }
  };

  dispatch({}); //获得初始state

  return { getState, dispatch, subscribe };
};
```

## Reducer拆分

常见的reducer文件 

```javascript
const chatReducer = (state = defaultState, action = {}) => {
  const { type, payload } = action;
  switch (type) {
    case ADD_CHAT:
      return Object.assign({}, state, { //理论上应该使用深拷贝
        chatLog: state.chatLog.concat(payload)
      });
    case CHANGE_STATUS:
      return Object.assign({}, state, { //理论上应该使用深拷贝
        statusMessage: payload
      });
    case CHANGE_USERNAME:
      return Object.assign({}, state, { //理论上应该使用深拷贝
        userName: payload
      });
    default: return state;
  }
};
```

包含三个Action 的处理, 包含对于三个属性的处理

针对不同type的action请求执行不同的方法, 并返回更改对应属性的newState

但是在大型项目中, 需要对Reducers进行拆分, 针对不同属性每个模块函数单独处理

可以划分为三个函数(处理函数接受(previousValue, action), **如果previous是对象, 不能直接操作previous**

```javascript
const chatReducer = (state = defaultState, action = {}) => {
  return {
    chatLog: chatLog(state.chatLog, action), //传入对应属性 previousValue, action
    statusMessage: statusMessage(state.statusMessage, action),
    userName: userName(state.userName, action)
  }
};
```

以上返回一个新对象, 新对象中每个属性都对应一个处理函数, 并且处理函数返回一个新值

Redux提供了一个combineReducers负责合并三个Reducer, 当函数名与属性名相同时

```javascript
import { combineReducers } from 'redux';

const chatReducer = combineReducers({
  chatLog,
  statusMessage,
  userName
})

export default todoApp;
```

函数名与属性名不同的时候

```javascript
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,
  c: c
})

// 等同于
function reducer(state = {}, action) {
  return {
    a: doSomethingWithA(state.a, action),
    b: processB(state.b, action),
    c: c(state.c, action)
  }
}
```

combineReducers的简单实现

```javascript
const combineReducers = reducers => {
  return (state = {}, action) => {
    return Object.keys(reducers).reduce(
      (nextState, key) => {
        nextState[key] = reducers[key](state[key], action);
        return nextState;
      },
      {} //设置newState初始值
    );
  };
};
```

```javascript
import { combineReducers } from 'redux'
import * as reducers from './reducers'

const reducer = combineReducers(reducers)
```

# Redux进阶

Redux如何进行异步操作? 

当Action 发出以后，Reducer 立即算出 State，这叫做同步；

当Action 发出以后，过一段时间再执行 Reducer，这就是异步。

怎么才能 Reducer 在异步操作结束后自动执行呢？这就要用到新的工具：中间件（middleware）。![img](F:\Notes\WebFront\React\Redux.assets\bg2016092002.jpg)

## 中间件 middleware 的概念

中间件就是在执行一部分操作之前, 先进行部分处理

而在Redux的三个部分:

（1）Reducer：纯函数，只承担计算 State 的功能，不合适承担其他功能，也承担不了，因为理论上，纯函数不能进行读写操作。

（2）View：与 State 一一对应，可以看作 State 的视觉层，也不合适承担其他功能。

（3）Action：存放数据的对象，即消息的载体，只能被别人操作，自己不能进行任何操作。

想来想去，只有发送 Action 的这个步骤，即`store.dispatch()`方法，可以添加功能。举例来说，要添加日志功能，把 Action 和 State 打印出来，可以对`store.dispatch`进行如下改造。

```javascript
let next = store.dispatch;
store.dispatch = function dispatchAndLog(action) {
  console.log('dispatching', action);
  next(action);
  console.log('next state', store.getState());
}
```

上面代码中，对`store.dispatch`进行了重定义，在发送 Action 前后添加了打印功能。这就是中间件的雏形。

中间件本质就是一个函数，对`store.dispatch`方法进行了改造，在发出 Action 和执行 Reducer 这两步之间，添加了其他功能。

## 中间件的用法

```javascript
import { applyMiddleware, createStore } from 'redux';
import createLogger from 'redux-logger';
const logger = createLogger();

const store = createStore(
  reducer,
  applyMiddleware(logger)
);
```

上述代码, redux-logger提供一个createLogger, 可以生产日志中间件logger

将他放在applyMiddleware中, 传入createStore即可, 就完成了对store.dispatch()方法的增强

### 注意事项:

- 如果仍需要加入初始状态值, 则参数为createStore(reducer, inital_state, applyMiddleWare(logger))

- 中间件添加的次序也有讲究

  ```javascript
  const store = createStore(
    reducer,
    applyMiddleware(thunk, promise, logger)
  );
  ```

  上面代码中，`applyMiddleware`方法的三个参数，就是三个中间件。有的中间件有次序要求，使用前要查一下文档。比如，`logger`就一定要放在最后，否则输出结果会不正确。

### applyMiddleware()

redux提供的原生方法, 它的原理是将所有中间件生成一个数组, 依次执行

```javascript
export default function applyMiddleware(...middlewares) {
  return (createStore) => (reducer, preloadedState, enhancer) => {
    var store = createStore(reducer, preloadedState, enhancer);
    var dispatch = store.dispatch;
    var chain = [];

    var middlewareAPI = {
      getState: store.getState,
      dispatch: (action) => dispatch(action)
    };
    chain = middlewares.map(middleware => middleware(middlewareAPI));
    dispatch = compose(...chain)(store.dispatch);

    return {...store, dispatch}
  }
}
```

以上是源码, 上面代码中，所有中间件被放进了一个数组`chain`，然后嵌套执行，最后执行`store.dispatch`。可以看到，中间件内部（`middlewareAPI`）可以拿到`getState`和`dispatch`这两个方法。

### 异步操作的基本思路

与同步发送action不同, 异步调用需要包含三个Action(发布, 成功, 失败)

- action种类不同

  ```javascript
// 写法一：名称相同，参数不同
    { type: 'FETCH_POSTS' }
    { type: 'FETCH_POSTS', status: 'error', error: 'Oops' }
    { type: 'FETCH_POSTS', status: 'success', response: { ... } }
    
    // 写法二：名称不同
{ type: 'FETCH_POSTS_REQUEST' }
    { type: 'FETCH_POSTS_FAILURE', error: 'Oops' }
    { type: 'FETCH_POSTS_SUCCESS', response: { ... } }
  ```
  
- 异步操作State也要变更

    ```javascript
    let state = {
      // ... 
      isFetching: true,
      didInvalidate: true,
      lastUpdated: 'xxxxxxx'
    };
    ```

    State 的属性:

    `isFetching`表示是否在抓取数据。

    `didInvalidate`表示数据是否过时，

    `lastUpdated`表示上一次更新时间。

    操作开始时，送出一个 Action，触发 State 更新为"正在操作"状态，View 重新渲染

    操作结束后，再送出一个 Action，触发 State 更新为"操作结束"状态，View 再一次重新渲染

## redux-thunk 中间件

异步操作至少要送出两个 Action：用户触发第一个 Action，这个跟同步操作一样，没有问题；如何才能在操作结束时，系统自动送出第二个 Action 呢？

首先我们必须改造Action

奥妙就在Action Creator之中

```javascript
class AsyncApp extends Component {
  componentDidMount() {
    const { dispatch, selectedPost } = this.props
    dispatch(fetchPosts(selectedPost))
  }

// ...
```

上面代码是一个异步组件的例子。加载成功后（`componentDidMount`方法），它送出了（`dispatch`方法）一个 Action，向服务器要求数据 `fetchPosts(selectedSubreddit)`。这里的`fetchPosts`就是 Action Creator。

下面就是`fetchPosts`的代码，关键之处就在里面。

![img](F:\Notes\WebFront\React\Redux.assets\bg2016092003.jpg)

```javascript
const fetchPosts = postTitle => (dispatch, getState) => {
  dispatch(requestPosts(postTitle));
  return fetch(`/some/API/${postTitle}.json`)
    .then(response => response.json())
    .then(json => dispatch(receivePosts(postTitle, json)));
  };
};

// 使用方法一
store.dispatch(fetchPosts('reactjs'));
// 使用方法二
store.dispatch(fetchPosts('reactjs')).then(() =>
  console.log(store.getState())
```

上面代码中，`fetchPosts`是一个Action Creator（动作生成器），返回一个函数。

这个函数执行后，先发出一个Action（`requestPosts(postTitle)`），然后进行异步操作。

拿到结果后，先将结果转成 JSON 格式，然后再发出一个 Action（ `receivePosts(postTitle, json)`）。

（1）**`fetchPosts`返回了一个函数，而普通的 Action Creator 默认返回一个对象。**

（2）返回的函数的参数是`dispatch`和`getState`这两个 Redux 方法，普通的 Action Creator 的参数是 Action 的内容。

（3）在返回的函数之中，先发出一个 Action（`requestPosts(postTitle)`），表示操作开始。

（4）异步操作结束之后，再发出一个 Action（`receivePosts(postTitle, json)`），表示操作结束。

这样的处理，就解决了自动发送第二个 Action 的问题。

但是，又带来了一个新的问题，Action 是由`store.dispatch`方法发送的。而`store.dispatch`方法正常情况下，参数只能是对象，不能是函数。这时，就要使用中间件[`redux-thunk`](https://github.com/gaearon/redux-thunk)。

```javascript
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import reducer from './reducers';

// Note: this API requires redux@>=3.1.0
const store = createStore(
  reducer,
  applyMiddleware(thunk)
);
```

上面代码使用`redux-thunk`中间件，**改造`store.dispatch`，使得后者可以接受函数作为参数。**

**因此，异步操作的第一种解决方案就是，写出一个返回函数的 Action Creator，然后使用`redux-thunk`中间件改造`store.dispatch`。**



## redux-promise 中间件

和thunk类似, 它将改造dispatch 使得store能够接受并处理promise对象

也就是说我们可以使用ActionCreator创建一个返回值为Promise的Action

> ```javascript
> import { createStore, applyMiddleware } from 'redux';
> import promiseMiddleware from 'redux-promise';
> import reducer from './reducers';
> 
> const store = createStore(
>   reducer,
>   applyMiddleware(promiseMiddleware)
> ); 
> ```

这个中间件使得`store.dispatch`方法可以接受 Promise 对象作为参数。这时，Action Creator 有两种写法。写法一，返回值是一个 Promise 对象。

> ```javascript
> const fetchPosts = 
>   (dispatch, postTitle) => new Promise(function (resolve, reject) {
>      dispatch(requestPosts(postTitle));
>      return fetch(`/some/API/${postTitle}.json`)
>        .then(response => {
>          type: 'FETCH_POSTS',
>          payload: response.json()
>        });
> });
> ```

写法二，Action 对象的`payload`属性是一个 Promise 对象。这需要从[`redux-actions`](https://github.com/acdlite/redux-actions)模块引入`createAction`方法，并且写法也要变成下面这样。

> ```javascript
> import { createAction } from 'redux-actions';
> 
> class AsyncApp extends Component {
>   componentDidMount() {
>     const { dispatch, selectedPost } = this.props
>     // 发出同步 Action
>     dispatch(requestPosts(selectedPost));
>     // 发出异步 Action
>     dispatch(createAction(
>       'FETCH_POSTS', 
>       fetch(`/some/API/${postTitle}.json`)
>         .then(response => response.json())
>     ));
>   }
> ```

上面代码中，第二个`dispatch`方法发出的是异步 Action，只有等到操作结束，这个 Action 才会实际发出。注意，`createAction`的第二个参数必须是一个 Promise 对象。

看一下`redux-promise`的[源码](https://github.com/acdlite/redux-promise/blob/master/src/index.js)，就会明白它内部是怎么操作的。

> ```javascript
> export default function promiseMiddleware({ dispatch }) {
>   return next => action => {
>     if (!isFSA(action)) {
>       return isPromise(action)
>         ? action.then(dispatch)
>         : next(action);
>     }
> 
>     return isPromise(action.payload)
>       ? action.payload.then(
>           result => dispatch({ ...action, payload: result }),
>           error => {
>             dispatch({ ...action, payload: error, error: true });
>             return Promise.reject(error);
>           }
>         )
>       : next(action);
>   };
> }
> ```

从上面代码可以看出，如果 Action 本身是一个 Promise，它 resolve 以后的值应该是一个 Action 对象，会被`dispatch`方法送出（`action.then(dispatch)`），但 reject 以后不会有任何动作；如果 Action 对象的`payload`属性是一个 Promise 对象，那么无论 resolve 和 reject，`dispatch`方法都会发出 Action。

中间件和异步操作，就介绍到这里。[下一篇文章](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)将是最后一部分，介绍如何使用`react-redux`这个库。