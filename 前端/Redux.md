# Redux

典型的Web应用程序通常由共享数据的多个UI组件组成。通常，多个组件的任务是负责展示同一对象的不同属性。这个对象表示可随时更改的状态。在多个组件中保持状态一致性会是场噩梦，特别是有多个通道用于更新同一个对象。

## 1、什么是Redux

Redux是一个流行的JavaScript框架，为程序提供一个可预测的状态容器。在标准的MVC框架中，数据可以在UI组件和存储之间双向流动，而Redux严格限制数据只能单向流动。

![image-20200729102333800](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200729102333800.png)

Redux设计思想：

- Redux将整个应用状态(state)存储到一个地方(通常我们称其为store)
- 当我们需要修改状态时，必须派发(dispatch)一个action(action是一个带有type字段的对象)
- 专门的状态处理函数reducer接收旧的state和action，返回一个新的state
- 通过 subscribe 设置订阅，每次派发动作时，通知所有的订阅者