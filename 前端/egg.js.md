# Egg.js

## 1、新手指南

### 1.1、egg简介

- Egg.js 为企业级框架和应用而生，专注于提供Web开发核心功能和一套灵活可扩展的插件机制。
- Egg的插件机制有很高的可扩展性，**一个插件只做一件事**，Egg通过框架聚合这些插件，并根据自己的业务场景定制配置，这样应用的开发成本就变得很低。
- Egg 奉行 **约定优于配置** ，按照一套统一的约定进行应用的开发

**与社区框架的差异**

- **Express** 框架简单并且扩展性强，非常适合做个人项目，但框架本身缺少约定，标准的 MVC 模型会有各种千奇百怪的写法。Egg 按照约定进行开发，奉行『约定优于配置』，团队协作成本低。
- **sails** 和Egg 一样奉行约定由于配置，扩展性很好。但相比Egg，Sails支持Blueprint REST API、[WaterLine](https://github.com/balderdashy/waterline) 这样可扩展的 ORM、前端集成等，但这些功能都由Sails提供的。而 Egg 不直接提供这些功能，只是集成各种功能插件，比如实现 egg-blueprint，egg-waterline 等这样的插件，再使用 sails-egg 框架整合这些插件就可以替代 [Sails](http://sailsjs.com/) 了。

### 1.2、Egg.js 和 Koa

**异步编程模型**

- Node.js是一个异步的世界，官方的API支持的都是callback形式的异步编程模型，这会带来很多问题，比如：

  >callback hell：最臭名昭著的callback嵌套问题
  >
  >release zalgo：异步函数可能同步调用 callback 返回数据，带来不一致性

  社区提供了很多方法，最终胜出的事Promise，还有async function

- async function：可以通过 await 关键字来等待一个Promise 被resolve (或者 reject，此时会抛出异常）

```js
const fn = function() {
    const user = await getUser();
    const posts = await fetchPosts(user.id);
    return { user, posts};
};
fn().then( res => console.log(res)).catch(err => console.error(err.stack));
```

**Koa**

- Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石.
- Middleware：Koa 的中间件和 Express 不同，Koa 选择了洋葱圈模型。所有的请求经过一个中间件的时候都会执行两次，对比 Express 形式的中间件，Koa 的模型可以非常方便的实现后置处理逻辑。
- Context：和 Express 只有 Request 和 Response 两个对象不同，Koa 增加了一个 Context 的对象，作为这次请求的上下文对象。

### 1.3、渐进式开发

- Egg 可以一步步渐进的进行框架演进，从最开始的状态到插件的雏形，再到抽成独立的插件，最后沉淀到框架

- 一般来说，当应用中有可能会复用到的代码时，直接放到 `lib/plugin` 目录去，如例子中的 `egg-ua`。
- 当该插件功能稳定后，即可独立出来作为一个 `node module` 。
- 如此以往，应用中相对复用性较强的代码都会逐渐独立为单独的插件。
- 当你的应用逐渐进化到针对某类业务场景的解决方案时，将其抽象为独立的 framework 进行发布。
- 当在新项目中抽象出的插件，下沉集成到框架后，其他项目只需要简单的重新 `npm install` 下就可以使用上，对整个团队的效率有极大的提升。

## 2、基础功能

### 2.1、目录结构

```
egg-project
├── package.json
├── app.js (可选) //用于自定义启动时的初始化工作
├── agent.js (可选)//用于自定义启动时的初始化工作
├── app
|   ├── router.js //用于配置 URL 路由规则
│   ├── controller //用于解析用户的输入，处理后返回相应的结果
│   |   └── home.js
│   ├── service (可选) //用于编写业务逻辑层，建议使用
│   |   └── user.js
│   ├── middleware (可选) //用于编写中间件
│   |   └── response_time.js
│   ├── schedule (可选)  //用于定时任务
│   |   └── my_task.js
│   ├── public (可选) //用于放置静态资源
│   |   └── reset.css
│   ├── view (可选)
│   |   └── home.tpl
│   └── extend (可选)  //用于框架的扩展
│       ├── helper.js (可选)
│       ├── request.js (可选)
│       ├── response.js (可选)
│       ├── context.js (可选)
│       ├── application.js (可选)
│       └── agent.js (可选)
├── config   //用于编写配置文件
|   ├── plugin.js // 用于配置需要加载的插件
|   ├── config.default.js
│   ├── config.prod.js
|   ├── config.test.js (可选)
|   ├── config.local.js (可选)
|   └── config.unittest.js (可选)
└── test   // 单元测试
    ├── middleware
    |   └── response_time.test.js
    └── controller
        └── home.test.js
```

- `app/view/**` 用于放置模板文件，可选，由模板插件约定，具体参见[模板渲染](https://eggjs.org/zh-cn/core/view.html)。
- `app/model/**` 用于放置领域模型，可选，由领域类相关插件约定，如 [egg-sequelize](https://github.com/eggjs/egg-sequelize)

### 2.2、框架内置基础对象

### 2.3、运行环境

### 2.5、配置

### 2.6、中间件

### 2.7、路由（Router）

- Router 主要用来描述请求 URL 和具体承担执行动作的 Controller 的对应关系，框架约定了`app/router.js`文件用于统一所有路由规则



