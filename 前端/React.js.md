# React

***

## 1、React 简介

- React 是一个用于构建用户界面的 JAVASCRIPT 库。

- React 主要用于构建UI，很多人认为 React 是 MVC 中的 V（视图）。

- ***特点***

  > - **1.声明式设计** −React采用声明范式，可以轻松描述应用。
  > - **2.高效** −React通过对DOM的模拟，最大限度地减少与DOM的交互。
  > - **3.灵活** −React可以与已知的库或框架很好地配合。
  > - **4.JSX** − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。
  > - **5.组件** − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。
  > - **6.单向响应的数据流** − React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。

## 2、React 元素渲染

- 元素是构成React应用的最小单位，它用于描述屏幕上的输出

- **React 只会更新必要的部分**

  值得注意的是 React DOM 首先会比较元素内容先后的不同，而在渲染过程中只会更新改变了的部分。

## 3、React JSX

- React使用JSX来替代常规的JavaScript（不一定一定要使用，但建议）

- JSX有以下优点：

  > - JSX 执行更快，因为它在编译为 JavaScript 代码后进行了优化。
  > - 它是类型安全的，在编译过程中就能发现错误。
  > - 使用 JSX 编写模板更加简单快速。

- 注意:

  由于 JSX 就是 JavaScript，一些标识符像 `class` 和 `for` 不建议作为 XML 属性名。作为替代，React DOM 使用 `className` 和 `htmlFor` 来做对应的属性。

- 可以在JSX中使用JavaScript表达式，表达式写在花括号{}中。

- JSX中不能使用if else语句，但可以使三元运算符。{i == 1 ? 'True!' : 'False'}

- 总结：

  >```jsx
  >const element = <h1>Hello, world!</h1>;
  >```
  >
  >这种看起来科囊看起来有些奇怪的标签语法既不是字符串，也不是HTML
  >
  >它被称为JSX，一种JavaScript的语法扩展。我们推荐在React中使用JSX来描述用户界面。
  >
  >JSX是在JavaScript内部实现的。
  >
  >我们指导元素是构成React应用的最小单位，JSX就是用来声明React当中的元素。

## 4、React 组件

[如果我们需要向组件传递参数，可以使用this.props对象。]()

- 封装一个输出"Hello World!"的组件

  ```jsx
  function HelloMessage(props){
      return <h1>Hello World!</h1>;
  }
  const element = <HelloMessage />; //为用户自定义组件，自定义的组件必须以大写字母开头，否则会报错
  ReactDOM.render(){
      element;
      docunment.getElementById('example')
  }
  ```

- 创建组件的两种方式

  ```jsx
  //使用函数定义一个组件
  function HelloMessage(props){
  	return <h1>Hello World!</h1>
  }
  
  //使用ES6 class来定义一个组件
  class Welcome extends React.Component{
      render(){
          return <h1>Hello World!</h1>;
      }
  }
  ```

- 复合组件

  ```jsx
  function Name(props){
      return <h1>网站名称：{props.name}</h1>;
  }
  function Url(props){
      return <h1>网站地址：{props.url}</h1>;
  }
  function Age(props){
      return <h1>网站年龄：{props.age}</h1>;
  }
  function App(){
      return (
      	<div>
          	<Name Name="菜鸟教程"></Name>
              <Url url="http://www.runoob.com"></Url>
              <Age age=12></Age>
          </div>
      );
  }
  ReactDOM.render(){
      <App></App>,
      document.getElementById('example')
  }
  ```

## 5、React State(状态)

- **[React把组件看成是一个状态机，通过与用户的交互，实现不同状态，然后渲染UI，让用户界面和数据保持一致。]()**

- React里，只需更新组件的state，然后根据新的state重新渲染用户界面（不操作DOM）。

- 实例

  - ```jsx
    class Clock extends React.Componemt{
        constructot(props){
            super(props);
            this.state = {date : new Date()};
        }
        
        render(){
            return (
            	<div>
                	<h1>Hello World!</h1>
                    <h2>现在时间是 {this.state.date.toLocaleTimeString()}</h2>
                </div>
            );
        }
    }
    
    ReactDOM.render(
    	<Clock></Clock>,
        document.getElementById('example')
    );
    ```

  
***将生命周期方法添加到类中***
  
- 在具有许多组件的应用程序中，在销毁时释放组件所占用的资源非常重要
  
- 每当 Clock 组件第一次加载到 DOM 中的时候，我们都想生成定时器，这在 React 中被称为**挂载**。
  
  同样，每当 Clock 生成的这个 DOM 被移除的时候，我们也会想要清除定时器，这在 React 中被称为**卸载**。
  
  ```jsx
  class Clock extends React.Component {
      constructor(props){
          super(props);
          this.state = {date : new date()}
      }
      
      componentDidMount() {
          this.timerID = setInterval(
          	() => this.trck(),//箭头函数，箭头函数会默认帮我们绑定外层this的值，所以在箭头函数中this的值和外层的this是一样的
              1000
          );
      }
      
      componentWillUnmount() {
          clearInterval(this.timerID)
      }
      
      tick() {
          this.setState({
              data : new Date()
          });
      }
      
      render() {
          return (
          	<div>
              	<h1>Hello World!</h1>
                  <h2>现在时间是：{this.state.data.toLocaleTimeString()}</h2>
              </div>
          )
      }
  }
  
  ReactDOM.render(
  	<Clock></Clock>
      document.getElementById('example')
  )
```
  
- 上面代码中的**componentDidMount()** 与 **componentWillUnmount()** 方法被称作生命周期钩子。
  
- 在组件输出到 DOM 后会执行 **componentDidMount()** 钩子，我们就可以在这个钩子上设置一个定时器。
  
  this.timerID 为定时器的 ID，我们可以在 **componentWillUnmount()** 钩子中卸载定时器。
  
- ***代码执行顺序***
  
    - 当 `<Clock />`被传递给 `ReactDOM.render()` 时，React 调用 `Clock` 组件的构造函数。 由于 `Clock` 需要显示当前时间，所以使用包含当前时间的对象来初始化 `this.state` 。 我们稍后会更新此状态。
    - React 然后调用 `Clock` 组件的 `render()` 方法。这是 React 了解屏幕上应该显示什么内容，然后 React 更新 DOM 以匹配 `Clock` 的渲染输出。
    - 当 `Clock` 的输出插入到 DOM 中时，React 调用 `componentDidMount()` 生命周期钩子。 在其中，`Clock` 组件要求浏览器设置一个定时器，每秒钟调用一次 `tick()`。
    - 浏览器每秒钟调用 `tick()` 方法。 在其中，`Clock` 组件通过使用包含当前时间的对象调用 `setState()` 来调度UI更新。 通过调用 `setState()` ，React 知道状态已经改变，并再次调用 `render()` 方法来确定屏幕上应当显示什么。 这一次，`render()` 方法中的 `this.state.date` 将不同，所以渲染输出将包含更新的时间，并相应地更新 DOM。
  - 一旦 `Clock` 组件被从 DOM 中移除，React 会调用 `componentWillUnmount()` 这个钩子函数，定时器也就会被清除。
  
***数据自顶向下流动***
  
  - 父组件和子组件都不能知道某个组件是有状态还是无状态的，并且他们不关心某个组件是被定义成一个函数还是一个类。这就是为什么状态通常被称为局部或封装。 除了拥有并设置它的组件外，其它组件不可访问。

## 6、React Props

- state和props的主要区别在于props是不可变的，而state可以根据与用户的交互来改变。这就是为什么有些容器组件需要定义state来更新和修改数据。而子组件只能通过props来传递数据

- 以下实例演示如何组合使用state和props

  ```jsx
  class WebSite extends React.Component {
      constructor() {
          super();
          this.state = {
              name = "菜鸟教程",
              site = "https://www.runoob.com"
          }
      }
      
      render() {
          return(
          	<div>
                  <Name name={this.state.name}></Name> //设置name和site的值来获取父组件传递过来的值
                  <Site site={this.state.site}></Site>
              </div>
          );
      }
  }
  
  class Name extends React.Component {
      render() {
          return(
          	<h1>{this.props.name}</h1>//在父组件设置State,并通过在子组件上使用props将其传递到子组件上
          );
      }
  }
  
  class Site extends React.Component {
      render() {
          return(
          	<a>{this.props.site}</a>
          );
      }
  }
  
  ReactDOM.render(
  	<Websitw></Websitw>,
      document.getElementById('example')
  )
  ```

## 7、React事件处理

- React事件绑定属性的命名采用驼峰式写法，而不是小写

  ```jsx
  <button onClick={activateLasers}>激活按钮</button>
  activateLasers(){
      //
  }
  ```

- 向事件处理程序传递参数。例如，若是 id 是你要删除那一行的 id，以下两种方式都可以向事件处理程序传递参数：

  ```jsx
  <button onClick={(e) => this.deleteRow(id,e)}>Delete Row</button>
  <button onClick={this.deleteRow.bind(this,id)}>Delete Row</button>
  //参数 e 作为 React 事件对象将会被作为第二个参数进行传递。通过箭头函数的方式，事件对象必须显式的进行传递，但是通过 bind 的方式，事件对象以及更多的参数将会被隐式的进行传递。
  ```

- **理解 React 中 es6 创建组件 this 的方法**

  在JavaScript中，this对象是运行时基于函数的执行环境（也就是上下文）绑定的。

  ### 从 react 中的 demo 说起

  Facebook最近一次更新react时，将es6中的class加入了组件的创建方式当中。Facebook也推荐组件创建使用通过定义一个继承自 React.Component 的class来定义一个组件类。官方的demo：

  ```jsx
  class LikeButton extends React.Component {
    constructor() {
      super();
      this.state = {
        liked: false
      };
      this.handleClick = this.handleClick.bind(this);
    }
    handleClick() {
      this.setState({liked: !this.state.liked});
    }
    render() {
      const text = this.state.liked ? 'liked' : 'haven\'t liked';
      return (
        <div onClick={this.handleClick}>
          You {text} this. Click to toggle.
        </div>
      );
    }
  }
  ReactDOM.render(
    <LikeButton />,
    document.getElementById('example')
  );
  ```

  上面的demo中有大量this的使用，在 class LikeButton extends React.Component 中我们是声明该class，因为this具体是由其上下文决定的，因此在类定义中我们无法得知this用法。 相当于是new了上面定义的类，首先调用 constructor() 函数， this.state 的this上下文就是该实例对象；同理，render() 函数中 this.state.liked 的this上下文也是该对象。问题在于 onClick={this.handleClick} ，获取该函数引用是没有问题，这里的this上下文就是该对象。

  这时问题来了，在原来 React.createClass 中， handleClick() 在onClick事件触发的时候，会自动绑定到LikeButton实例上，这时候该函数的this的上下文就是该实例。不过在ES6的class的写法中，Facebook取消了自动绑定，实例化LikeButton后，handleClick()的上下文是div的支撑实例（ backing instance ），而 handleClick() 原本要绑定的上下文是LikeButton的实例。对于该问题，我们有多种解决方案。

  ### 使用 bind() 函数改变 this 的上下文

  可以在class声明中的constructor()函数中，使用：

  ```jsx
  this.handleClick = this.handleClick.bind(this);
  ```

  该方法是一个bind()绑定，多次使用。在该方法中，我们在声明该实例后，可以在该实例任何地方使用 handleClick() 函数，并且该 handleClick() 函数的this的上下文都是LikeButton实例对象。

  除此我们也可以在具体使用该函数的地方绑定this的上下文到LikeButton实例对象，代码如下：

  ```jsx
  <div onClick={this.handleClick.bind(this)}>
    You {text} this. Click to toggle.
  </div>
  ```

  这种方法需要我们每次使用bind()函数绑定到组件对象上。

  ### es6的箭头函数

  es6中新加入了箭头函数=>，箭头函数除了方便之外还有而一个特征就是将函数的this绑定到其定义时所在的上下文。这个特征也可以帮助我们解决这个问题。使用如下代码：

  ```jsx
  <div onClick={() => this.handleClick()}>
    You {text} this. Click to toggle.
  </div>
  ```

  这样该 this.handleClick() 的上下文就会被绑定到 LikeButton 的实例对象上。个人认为，使用箭头函数使得 JavaScript 更加接近面向对象的编程风格。

  ### this 的总结

  this 的本质就是：this跟作用域无关的，只跟执行上下文有关。

## 8、React条件渲染

- 在React中，你可以创建不同的组件来封装各种你需要的行为，然后还可以根据不同的状态变化只渲染其中的一部分

- 在React中的条件渲染和JavaScript中的一致，使用JavaScript操作符if或者条件运算符来创建表示当前状态的元素，然后让React根据他们的状态来更新UI

  ```jsx
  function UserGreeting(props) {
  	return <h1>欢迎回来！</h1>;
  }
  function GuestGreeting(props) {
      return <h1>请先注册！</h1>;
  }
  function Greeting(props) {
      const isLoggedIn = props.isLoggedIn;
      if (isLoggedIn) {
          return <UserGreeting></UserGreeting>;
      }
      return <GuestGreeting></GuestGreeting>;
  }
  ReactDOM.render(
  	<Greeting isLoggedIn={false}></Greeting>,
      document.getElementById('example')
  )
  ```

- ***元素变量***

  你可以使用变量来存储元素。它可以帮助你有条件的渲染组件的一部分，而输出的其他部分不会改变

  ```jsx
  class LoginControl extends React.Component {
      constructor(props) {
          super(props);
          this.handleLoginClick = this.handleLoginClick.bind(this);
          this.handleLogoutCliock = this.handleLogoutClick.bind(this);
          this.state = {isLoggedIn : false};
      }
      
      handleLoginClick() {
          this.setState = {isLoggedIn : true};
      }
      
      handleLoginClick() {
          this.setState = {isLoggedIn : false};
      }
      
      render() {
          const isLoggedIn = this.state.isLoggedIn;
          let button = null;
          if(isLoggedIn) {
              button = <LogoutButton onClick={this.handleLogoutClick}></LogoutButton>;
          } else {
              button = <LoginButton onClick={this.handleLoginClick}></LoginButton>;
          }
          return (
              <div>
                  <Greeting isLoggedIn={isLoggedIn}></Greeting>//引用之前的方法
                  {button}
              </div>
          );
      }
  }
  
  ReactDOM.render(
  	<LoginControl></LoginControl>,
      document.getElementById('example')
  )
  ```

- ***与运算符&&***

  可以通过用花括号包裹代码在 JSX 中嵌入任何表达式 ，也包括 JavaScript 的逻辑与 &&，它可以方便地条件渲染一个元素。

- ***三目运算符 ***condition ? true : false。

- ***阻止组件渲染***

  在少数情况下，你可能希望隐藏组件。让render返回null而不是它的渲染结果即可实现

  ```jsx
  function WarningBanner(props) {
      if(!props.warn) {
          return null;
      }
      return (
      	<div className="warning">
          	警告！
          </div>
      )
  }
   
  class Page extends React.Component {
      construnctor(props){
          super(props);
          this.state = {showWarning : true};
          this.handleToggleClick = this.handleToggleClick.bind(this);
      }
      handleToggleClick() {
          this.setState(prevState => ({
              showWarnning: !prevState.showWarning
          }));
      }
      render(){
          retrun(
          	<div>
              	<WarningBanner warn={this.state.showWarning}></WarningBanner>
                  <button onClick={this.handleToggleClick}>
                     	{this.state.showWarning ? '隐藏':'显示'}
                  </button>
              </div>
          );
      }
  }
  
  ReactDOM.render(
  	<Page></Page>,
      document.getElementById('example')
  )
  ```

  组件的render方法返回null并不会影响该组件生命周期方法的回调。

## 9、React 列表 & Keys

- 我们可以使用JS中的map()方法来创建列表

  ```jsx
  function NumberList(props) {
      const numbers = props.numbers;
      const listItems = numbers.map((number) => 
      	<li key={number.toString()}>{number}</li>
      );
      return(
        <ul>{listItem}</ul>                           
      );
  }
  
  const numbers = [1,2,3,4,5];
  
  ReactDOM.render(
  	<NumberList></NumberList>,
      document.getElementById('example')
  )
  ```

***Keys***

- Keys可以在DOM中的某些元素被增加或删除的时候帮助React识别哪些元素发生了变化。

## 10、React 组件 API

### 10.1、设置状态：setState

**1、不要直接更新状态**

例如，此代码不会重新渲染组件：

```jsx
// Wrong
this.state.comment = 'Hello';
```

应当使用 setState():

```jsx
// Correct
this.setState({comment: 'Hello'});
```

构造函数是唯一能够初始化 this.state 的地方。

**2、状态更新可能是异步的**

React 可以将多个 setState() 调用合并成一个调用来提高性能。

因为 this.props 和 this.state 可能是异步更新的，你不应该依靠它们的值来计算下一个状态。

例如，此代码可能无法更新计数器：

```jsx
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

要修复它，请使用第二种形式的 setState() 来接受一个函数而不是一个对象。 该函数将接收先前的状态作为第一个参数，将此次更新被应用时的props做为第二个参数：

```jsx
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

上方代码使用了箭头函数，但它也适用于常规函数：

```jsx
// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

**3、状态更新合并**

当你调用 setState() 时，React 将你提供的对象合并到当前状态。

例如，你的状态可能包含一些独立的变量：

```jsx
constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

你可以调用 setState() 独立地更新它们：

```jsx
componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

这里的合并是浅合并，也就是说 this.setState({comments}) 完整保留了 this.state.posts，但完全替换了 this.state.comments。

### 10.2、替换状态：replaceState

```
replaceState(object nextState[, function callback])
```

- **nextState**，将要设置的新状态，该状态会替换当前的**state**。
- **callback**，可选参数，回调函数。该函数会在**replaceState**设置成功，且组件重新渲染后调用。

**replaceState()**方法与**setState()**类似，但是方法只会保留**nextState**中状态，原**state**不在**nextState**中的状态都会被删除。

### 10.3、设置属性：setProps

```
setProps(object nextProps[, function callback])
```

- **nextProps**，将要设置的新属性，该状态会和当前的**props**合并
- **callback**，可选参数，回调函数。该函数会在**setProps**设置成功，且组件重新渲染后调用。

设置组件属性，并重新渲染组件。

**props**相当于组件的数据流，它总是会从父组件向下传递至所有的子组件中。当和一个外部的JavaScript应用集成时，我们可能会需要向组件传递数据或通知**React.render()**组件需要重新渲染，可以使用**setProps()**。

更新组件，我可以在节点上再次调用**React.render()**，也可以通过**setProps()**方法改变组件属性，触发组件重新渲染。

### 10.4、替换属性：replaceProps

```
replaceProps(object nextProps[, function callback])
```

- **nextProps**，将要设置的新属性，该属性会替换当前的**props**。
- **callback**，可选参数，回调函数。该函数会在**replaceProps**设置成功，且组件重新渲染后调用。

**replaceProps()**方法与**setProps**类似，但它会删除原有 props。

### 10.5、强制更新：forceUpdate

```
forceUpdate([function callback])
```

- **callback**，可选参数，回调函数。该函数会在组件**render()**方法调用后调用。

forceUpdate()方法会使组件调用自身的render()方法重新渲染组件，组件的子组件也会调用自己的render()。但是，组件重新渲染时，依然会读取this.props和this.state，如果状态没有改变，那么React只会更新DOM。

forceUpdate()方法适用于this.props和this.state之外的组件重绘（如：修改了this.state后），通过该方法通知React需要调用render()

一般来说，应该尽量避免使用forceUpdate()，而仅从this.props和this.state中读取状态并由React触发render()调用。

### 10.6、获取DOM节点：findDOMNode

```
DOMElement findDOMNode()
```

- 返回值：DOM元素DOMElement

如果组件已经挂载到DOM中，该方法返回对应的本地浏览器 DOM 元素。当**render**返回**null** 或 **false**时，**this.findDOMNode()**也会返回**null**。从DOM 中读取值的时候，该方法很有用，如：获取表单字段的值和做一些 DOM 操作。

### 10.7、判断组件挂载状态：isMounted

```
bool isMounted()
```

- 返回值：**true**或**false**，表示组件是否已挂载到DOM中

**isMounted()**方法用于判断组件是否已挂载到DOM中。可以使用该方法保证了**setState()**和**forceUpdate()**在异步场景下的调用不会出错。

## 11、React 组件生命周期

组件的生命周期可分成三个状态：

- Mounting：已插入真实DOM
- Updating：正在被重新渲染
- Unmounting：已移出真实DOM

生命周期方法有：

- **componentWillMount**在渲染前调用，在客户端也在服务端

- **componentDidMount**在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问

- **componentWillReceiveProps** 在组件接收一个新的props(更新后)时被调用。这个方法在初始化render时不会被调用

- **shouldComponentUpdate** 返回一个布尔值。在组建接收到新的props和state时被调用。在初始化时或者使用forceUpdate时不被调用。可以在你确认不需要更新组件时使用

- **componentWillUpdate** 在组件接收新的props或者state但还没有被render时被调用。在初始化时不被调用

- **componentDidUpdate** 在组件完成更新后立即调用。在初始化时不会被调用

- **componentWillUnmount** 在组件从DOM中移除之前立即被调用

  ```jsx
  class Button extends React.Component {
      constructor(props) {
          super(props);
          this.state = {data : 0};
          this.setNewNumber = this.setNewNumber.bind(this);
      }
      setNewNumber() {
          this.setState({data: this.state.fata + 1})
      }
      render() {
          return(
          	<div>
              	<button onClick={this.setNewNumber}>INCREMENT</button>
                  <Content myNumber = {this.state.data}></Content>
              </div>
          );
      }
  }
  
  class Content extends React.Component {
      componentWillMount() {
          consloe.log('在渲染前调用');
      }
      componentDidMount() {
          console.log('在第一次渲染之后调用');
      }
      componentWillReceiveProps(newProps) {
          console.log('在组件被接收一个新的props(更新后)时被调用');
      }
      shouldComponentUpdate(newProps,newState) {
          return true; //在组建接收到新的props和state时被调用
      }
      componentWillUpdate(nextProps,nextState) {
          console.log('在组件接收新的props或者state但还没有被render时被调用');
      }
      componentDidUpdate(prevProps,prevState) {
          console.log('在组件完成更新后立即调用');
      }
      componentWillUnmount() {
          console.log('在组件从DOM中移除之前立即被调用');
      }
     	render() {
          return(
              <div>
                  <h3>{this.props.myNumber}</h3>
              </div>
          );
      }
  }
  
  ReactDOM.render(
  	<Button></Button>,
      document.getElementById('example')
  )
  ```

  

## 12、React AJAX

- React组件的数据可以通过componentDidMount方法中的Ajax来获取，当从服务端获取的数据时可以将数据存储在state中，再用this.setState方法重写渲染UI

- 当使用异步加载数据时，在组件卸载前使用componentWillUnmount来取消未完成的操作

  ```jsx
  class UserGist extends React.Component {
      constructor(props) {
          super(props);
          this.state = {username:'',lastGistUrl:''};
      }
      
      componentDidMount() {
          this.serverRequest = $.get(this.props.source,function(request){
              var lastGist = result[0];
              this.setState({
                  username:lastGist.owner.login,
                  lastGistUrl:lastGist.html_url
              });
          }.bind(this));
      }
      
      //卸载React组件时，把多余请求关闭，以免影响其他框架或组件的操作
      componentWillUnmount() {
          this.serverRequset.abort();
      }
      
      render() {
          return(
          	<div>
              	{this.state.username} 用户最新的Gist共享地址
                  <a href={this.state.lastGistUrl}>{this.state.lastGistUrl}</a>
              </div>
          )
      }
  }
  
  ReactDOM.render(
  	<UserGist source="https://api.github.com/users/octocat/gists"></UserGist>,
      document.getElementById('example')
  );
  ```

  

## 13、React 表单与事件

- 在 HTML 当中，像 <input>, <textarea>, 和 <select> 这类表单元素会维持自身状态，并根据用户输入进行更新。但在React中，可变的状态通常保存在组件的状态属性中，并且只能用 setState() 方法进行更新。

  ```jsx
  class HelloMessage extends React.Component {
  	constructor(props){
          super(props);
          this.state = {value: 'Hello'};
          this.handleChange = this.handleChange.bind(this);
      }
      
      handleChange(event){
          this.setState({
              value: event.target.value
          });
      }
      
      render() {
          var value = this.state.value;
          return(
          	<div>
              	<input type="text" value={value} onChange={this.handleChange}></input>
                  <p>{value}</p>
              </div>
          );
      }
  }
  
  ReactDOM.render(
  	<HelloMessage></HelloMessage>,
      document.getElementById('example')
  );
  ```

- 如何在子组件上使用表单

  ```jsx
  class Content extends React.Component {
      render() {
          <div>
          	<input type="text" value={this.props.myDataProp} onChange={this.props.updateStateProp}></input>
              <p>{this.props.myDataProp}</p>
          </div>
      }
  }
  
  class HelloMessage extends React.Component {
      constructor(props) {
        super(props);
        this.state = {value: 'Hello Runoob!'};
        this.handleChange = this.handleChange.bind(this);
      }
      
      handleChange(event){
          this.state = ({value: event.target.value});
      }
      
      render() {
  		var value = this.state.value;
          return(
           <div>
           	<Content myDataProp={value} updateStateProp = {this.handleChange}></Content>
           </div>
          );
      }
  }
  
  ReactDOM.render(
  	<HelloMessage></HelloMessage>,
      document.getElementById('example')
  )
  ```

  **Select下拉框**

  - 在React中，不使用selected属性，而在根select标签上用value属性来表示选中项

    ```jsx
    class FlavorForm extends React.Component { 
        constructor(props) {
            super(props);
            this.state = {value: 'coconut'};
            this.handleChange = this.handleChange.bind(this);
            this.handleSubmit = this.handleSubmit.bind(this);
      	}
        handleChange(event) {
           this.setState({value: event.target.value});
        }
        
        handleSubmit() {
        	alert('Your favorite flavor is: ' + this.state.value);
        	event.preventDefault();
        }
        
    	render() {
            return (
                <form onSubmit={this.handleSubmit}>
                	<label>
                    	选择您最喜欢的网站
                        <select value={this.state.value} onChange={this.handleChange}>
                        	<option value="gg">Google</option>
                            <option value="rn">Runoob</option>
                            <option value="tb">Taobao</option>
                            <option value="fb">Facebook</option>
                        </select>
                    </label>
                    <input type="submit" value="提交" />
                </form>
            )
        }
    }
    
    ReactDOM.render(
    	<FlavorForm></FlavorForm>,
        document.getElementById('example')
    );
    ```

  **React事件**

  - 以下实例演示通过 onClick 事件来修改数据：

    ```jsx
    class HelloWorld extends React.Component {
        constructor(props) {
            super(props);
            this.state = {value : "张雨晨"}；
            this.handleChange = this.handleChange.bind(this);
        }
        
        handleChange(event) {
            this.setState({
               value : "Rain" 
            });
        }
        
        render() {
            var value = this.state.value;
            return(
                <div>
                	<button onChange={this.handleChange}>点我</button>
                	<h2>{value}</h2>
                </div>
            );
        }
    }
    
    ReactDOM.render(
    	<HelloWorld></HelloWorld>,
        document.getElementById('example')
    )
    ```

## 14、React Refs

- React支持一种非常特殊的属性Ref，你可以用来绑定render()输出到任何组件上

- 这个特殊的属性允许你引用 render() 返回的相应的支撑实例（ backing instance ）。这样就可以确保在任何时间总是拿到正确的实例。

  ```jsx
  class MyComponent extends React.Component {
    handleClick() {
      // 使用原生的 DOM API 获取焦点
      this.refs.myInput.focus();
    }
    render() {
      //  当组件插入到 DOM 后，ref 属性添加一个组件的引用于到 this.refs
      return (
        <div>
          <input type="text" ref="myInput" />
          <input
            type="button"
            value="点我输入框获取焦点"
            onClick={this.handleClick.bind(this)}
          />
        </div>
      );
    }
  }
   
  ReactDOM.render(
    <MyComponent />,
    document.getElementById('example')
  );
  ```

  

​	