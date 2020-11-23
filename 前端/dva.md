# 一图胜千言, 何况是四图? 图解DVA

### 示例背景

最常见的 Web 类示例之一: TodoList = Todo list + Add todo button



### 图解一: React 表示法

![img](https://cdn.yuque.com/yuque/0/2018/png/103904/1528436560812-2586a0b5-7a6a-4a07-895c-f822fa85d5de.png)

按照 React 官方指导意见, 如果多个 Component 之间要发生交互, 那么状态(即: 数据)就维护在这些 Component 的最小公约父节点上, 也即是 ``



```
 ` 以及`` 本身不维持任何 state, 完全由父节点`` 传入 props 以决定其展现, 是一个纯函数的存在形式, 即: `Pure Component
```



### 图解二: Redux 表示法

React 只负责页面渲染, 而不负责页面逻辑, 页面逻辑可以从中单独抽取出来, 变成 store

![img](https://cdn.yuque.com/yuque/0/2018/png/103904/1528436134375-4c15f63d-72f1-4c73-94a6-55b220d2547c.png)

与图一相比, 几个明显的改进点:

1. 状态及页面逻辑从 ``里面抽取出来, 成为独立的 store, 页面逻辑就是 reducer

1. `` 及``都是 Pure Component, 通过 connect 方法可以很方便地给它俩加一层 wrapper 从而建立起与 store 的联系: 可以通过 dispatch 向 store 注入 action, 促使 store 的状态进行变化, 同时又订阅了 store 的状态变化, 一旦状态有变, 被 connect 的组件也随之刷新

1. 使用 dispatch 往 store 发送 action 的这个过程是可以被拦截的, 自然而然地就可以在这里增加各种 Middleware, 实现各种自定义功能, eg: logging



这样一来, 各个部分各司其职, 耦合度更低, 复用度更高, 扩展性更好



### 图解三: 加入 Saga

![img](https://cdn.yuque.com/yuque/0/2018/png/103904/1528436167824-7fa834ea-aa6c-4f9f-bab5-b8c5312bcf7e.png)

上面说了, 可以使用 Middleware 拦截 action, 这样一来异步的网络操作也就很方便了, 做成一个 Middleware 就行了, 这里使用 redux-saga 这个类库, 举个栗子:



1. 点击创建 Todo 的按钮, 发起一个 type == addTodo 的 action

1. saga 拦截这个 action, 发起 http 请求, 如果请求成功, 则继续向 reducer 发一个 type == addTodoSucc 的 action, 提示创建成功, 反之则发送 type == addTodoFail 的 action 即可



### 图解四: Dva 表示法

![img](https://cdn.yuque.com/yuque/0/2018/png/103904/1528436195004-cd3800f2-f13d-40ba-bb1f-4efba99cfe0d.png)

有了前面的三步铺垫, Dva 的出现也就水到渠成了, 正如 Dva 官网所言, Dva 是基于 React + Redux + Saga 的最佳实践沉淀, 做了 3 件很重要的事情, 大大提升了编码体验:

1. 把 store 及 saga 统一为一个 `model` 的概念, 写在一个 js 文件里面

1. 增加了一个 Subscriptions, 用于收集其他来源的 action, eg: 键盘操作

1. model 写法很简约, 类似于 DSL 或者 RoR, coding 快得飞起✈️



`约定优于配置, 总是好的`😆



```js
app.model({
  namespace: 'count',
  state: {
    record: 0,
    current: 0,
  },
  reducers: {
    add(state) {
      const newCurrent = state.current + 1;
      return { ...state,
        record: newCurrent > state.record ? newCurrent : state.record,
        current: newCurrent,
      };
    },
    minus(state) {
      return { ...state, current: state.current - 1};
    },
  },
  effects: {
    *add(action, { call, put }) {
      yield call(delay, 1000);
      yield put({ type: 'minus' });
    },
  },
  subscriptions: {
    keyboardWatcher({ dispatch }) {
      key('⌘+up, ctrl+up', () => { dispatch({type:'add'}) });
    },
  },
});
```

