# JavaScript

<hr>

## 1、JavaScript基础

### 1.1、JavaScript简介

- JavaScript是***脚本语言***，一种轻量级的编程语言，
- 可插入HTML页面
- 由现代浏览器执行

### 1.2、JavaScript输出

- 使用window.alert()
- 使用document.write()方法将内容写到HTML中
- 使用innerHTML写入到HTML元素
- 使用console.log()输出到浏览器控制台

### 1.3、JavaScript语法、语句、注释

- JavaScript数据类型：数字、字符串、数组、对象
- JavaScript对大小写敏感
- JavaScript单行注释//，单行注释/* ... */

### 1.4、JavaScript变量

- 变量必须以字母开头，也可以以$和_符号开头（不推荐）、变量大小写敏感

- const、let、var声明变量的区别

  > - const声明常量，后面不可修改
  > - let声明变量，块作用域，后面不能覆盖之前声明的值
  > - var声明变量，函数作用域，后面能覆盖之前声明的变量

### 1.5、JavaScript数据类型

- ***值类型(基本类型)***：字符串(String)、数字(Number)、布尔(Boolean)、对空(Null)、未定义(undefined)、Symbol

- ***引用数据类型***：对象(Object)、数组(Array)、函数(Function)

- JavaScript拥有动态数据类型，这意味着相同变量可用作不同的类型

  ```javascript
  var x;					//x为undefined
  var x = 5;				//现在x为数字
  var x = "zhang";		//现在x为字符串
  ```

- JavaScript各类型变量声明

```javascript
var x = "zhangyuchen";   //可使用单引号或双引号
var x = 34.00;			//Java只有一种数字类型Number
var x = true;			//true或者false
var x = new Array();	// var x = new Array("1","2","3")
var x = {name:"zhang",age:23}
```

- Undefinde：这个值表示变量不含值
- Null：将变量设置为Null来清空变量

### 1.6、JavaScript对象

- 跟Java中的对象类似

- 对象定义

  ```javascript
  var person = {
      firstName:"John", 
      lastName:"Doe", 
      age:50, 
      eyeColor:"blue"
  };
  ```

- 对象属性访问

  ```js
  person.lastName;
  person["lastName"];
  ```

### 1.7、JavaScript函数

- 语法

  ```js
  function functionname(){
      // 执行代码
  }
  
  //带参数的函数
  function myFunction(var1,var2){
      //执行代码
  }
  
  //带返回值的函数
  function myTest(){
      var x = 5 ;
      return x ; //
  }
  ```

### 1.8、JavaScript作用域

- 局部变量：函数内部，函数执行完销毁
- 全局变量：没有在函数内部声明的，页面关闭后销毁

### 1.9、Javascript事件

- 可以是浏览器行为，也可以是用户行为
- 常见的HTML事件

```js
onchang()		//HTML元素改变
onclick()		//用户点击HTML元素
onmouseover()    //用户在一个HTML元素上移动鼠标
onmouseout()     //用户从一个HTML元素上移开鼠标
onkeydown()		//用户按下键盘按键
onload()		//浏览器已完成加载页面

```

### 1.10、JavaScript字符串

- 使用转义字符来使用引号\

- 字符串也可以是对象

  ```js
  var name = new String("zhang");
  ```

- 字符串属性

  | 属性            | 描述                       |
  | --------------- | -------------------------- |
  | constructor     | 返回创建字符串属性的函数   |
  | length          | 返回字符串的长度           |
  | ***prototype*** | 允许您向对象添加属性和方法 |

- 字符串方法

  CharAt()、concat()、indexOf()、subString()等

### 1.11、JavaScript运算符

- 算术运算符：+、-、*、/、%、++、--
- 赋值运算符：=、+=、-=、*=、/=、%=
- ***注意***：数字与字符串相加，返回字符串 y="5"+5; //55

### 1.12、JavaScript比较和逻辑运算符

- 比较运算符：==、===（值和类型均相等）、!=、!===、>、<、>=、<=

- 逻辑运算符：&&(与)、||(或)、!(非)

- 条件运算符

  ```js
  //语法  variablename=(condition)?value1:value2 
  //如果变量 age 中的值小于 18，则向变量 voteable 赋值 "年龄太小"，否则赋值 "年龄已达到"。
  var age;
  var voteable = (age<18)?"年龄太小":"年龄已达到";
  ```

### 1.13、JavaScript条件语句

- **if 语句** - 只有当指定条件为 true 时，使用该语句来执行代码
- **if...else 语句** - 当条件为 true 时执行代码，当条件为 false 时执行其他代码
- **if...else if....else 语句**- 使用该语句来选择多个代码块之一来执行
- **switch 语句** - 使用该语句来选择多个代码块之一来执行

### 1.14、JavaScript循环

- **for** - 循环代码块一定的次数
- **for/in** - 循环遍历对象的属性
- **while** - 当指定的条件为 true 时循环指定的代码块
- **do/while** - 同样当指定的条件为 true 时循环指定的代码块，该循环至少会执行一次

### 1.15、JavaScript break和continue语句

- break用于跳出当前循环
- continue用于跳出当前循环的一个迭代

### 1.16、JavaScript typeof，null和undefined

- typeof：检测变量的数据类型   注：数组也是对象

- null：一个只有一个值的特殊类型，表示一个空对象的引用。typeof(null)   // 返回object

- undefined：一个没有设置值的变量。 typeof一个没有值得变量会返回undefined

- undefined和null的区别

  值相等，类型不相等

  ```js
  typeof undefined;			// undefined
  typeof null;				// null
  undefined == null;			//true
  undefined === null;			//false
  ```

### 1.17、JavaScript类型转换

- 6种不同数据类型：string、number、boolean、object、function、symbol

- 3种对象类型：Object、Array、Date

- 2种不包含任何值得数据类型：undefined、null

  ```js
  typeof(NaN);			//number
  typeof(Array);			//object
  typeof(null);			//object
  typeod(Date);			//object
  ```

- constructor属性：返回所有JavaScript变量得构造函数

- JavaScript类型转换

  > 数字转字符串   String(123)   或   (123).toString ，布尔和日期同理
  >
  > 字符串转数字	parseFloat()，解析字符串，返回浮点数      parseInt()：解析字符串，返回整数
  >
  > 布尔转数字   Number(false) // 0     Number(true)  // 1

### 1.18、JavaScript正则表达式

- 语法

  ```js
  var pattern = /runoob/i					//    /正则表达式主题/修饰符（可选）
  //修饰符i:执行对大小写不敏感的匹配
  //修饰符g:执行全局匹配（不是在匹配到一个之后就停止）
  //修饰符m:执行多行匹配
  ```

- 方法

  search()：返回子串的起始位置。

  replace()：替换

  test()：检测一个字符串是否匹配某个模式，返回true/false

  exec() 方法用于检索字符串中的正则表达式的匹配。该函数返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。

  ```js
  //test
  var patt = /e/;
  patt.test("The best things in life are free!"); //true
  
  //exce()
  /e/.exec("The best things in life are free!");// e
  ```

### 1.19、JavaScript错误

- **try** 语句测试代码块的错误。
- **catch** 语句处理错误。
- **throw** 语句创建自定义错误。
- **finally** 语句在 try 和 catch 语句之后，无论是否有触发异常，该语句都会执行。

### 1.20、JavaScript调试

- **debugger** 关键字用于停止执行 JavaScript，并调用调试函数。这个关键字与在调试工具中设置断点的效果是一样的

### 1.21、JavaScript变量提升

- JavaScript 中，函数及变量的声明都将被提升到函数的最顶部。
- JavaScript 中，变量可以在使用后声明，也就是变量可以先使用再声明。
- JavaScript 只有声明的变量会提升，初始化的不会。

### 1.22、JavaScript严格模式

- 使用"use strict"指令
- 不允许使用为声明的变量等

### 1.23、JavaScript使用误区

- 赋值运算符应用错误

  ```js
  var x = 0 ;
  if(x == 10);   //返回false
  
  var x = 0;
  if(x = 10);    //x被赋值为10了,而10为true,所以返回true
  ```

- 比较运算符常见错误：===和==的区别

- 浮点型数据类型使用注意事项

  JavaScript 中的所有数据都是以 64 位**浮点型数据(float)** 来存储。

  ```js
  var x = 0.1;
  var y = o.2;
  var z = x + y;			//z 的结果为 0.30000000000000004
  if(z == 0.3);			//false
  
  //使用整数的除法可解决
  var z = (x * 10 + y * 10) / 10;       // z 的结果为 0.3
  ```

### 1.24、JavaScript this关键字

面向对象语言中 this 表示当前对象的一个引用。但在 JavaScript 中 this 不是固定不变的，它会随着执行环境的改变而改变。

- 在方法中，this 表示该方法所属的对象。
- 如果单独使用，this 表示全局对象。
- 在函数中，this 表示全局对象。
- 在函数中，在严格模式下，this 是未定义的(undefined)。
- 在事件中，this 表示接收事件的元素。
- 类似 call() 和 apply() 方法可以将 this 引用到任何对象。

### 1.25、JavaScript let和const

- let 声明的变量只在 let 命令所在的代码块内有效。

- const 声明一个只读的常量，一旦声明，常量的值就不能改变。

- JavaScript块级作用域

  > ES6 可以使用 let 关键字来实现块级作用域。
  >
  > let 声明的变量只在 let 命令所在的代码块 **{}** 内有效，在 **{}** 之外不能访问。

### 1.26、JavaScript JSON

- JSON语法规则：大括号对象，中括号数组

| 函数                                                         | 描述                                           |
| :----------------------------------------------------------- | :--------------------------------------------- |
| [JSON.parse()](https://www.runoob.com/js/javascript-json-parse.html) | 用于将一个 JSON 字符串转换为 JavaScript 对象。 |
| [JSON.stringify()](https://www.runoob.com/js/javascript-json-stringify.html) | 用于将 JavaScript 值转换为 JSON 字符串。       |

### 1.27、JavaScript Void

- javascript:void(0) 中最关键的是 void 关键字， void 是 JavaScript 中非常重要的关键字，该操作符指定要计算一个表达式但是不返回值。 

- void()仅仅是代表不返回任何值，但是括号内的表达式还是要运行，如

  ```js
  void(alert("Warnning!"))
  ```

## 2.JS 函数

### 2.1、JavaScript函数定义

- 函数提升（Hoisting)

  提升（Hoisting）是 JavaScript 默认将当前作用域提升到前面去的的行为。

- 自调用函数

  ```js
  (function () {
      var x = "Hello!!";      // 我将调用自己
  })();
  ```

- ***箭头函数***（ES6）

  > - 箭头函数的表达式比普通函数表达式更简洁
  >
  > - 当我们使用箭头函数的时候，箭头函数会默认帮我们绑定外层this的值，所以在箭头函数中this的值和外层的this是一样的
  > - 使用 **const** 比使用 **var** 更安全，因为函数表达式始终是一个常量
  
  ```js
  //语法
  //(参数1,参数2) => {函数声明}
  
  // ES5
  var x = function(x, y) {
       return x * y;
}
   
  // ES6
  const x = (x, y) => x * y;
  ```
  

### 2.2、JavaScriptn函数参数

**函数显示参数(Parameters)与隐式参数(Arguments)**

- 函数显示参数在函数定义时给出
- 函数隐式参数在函数调用时传递给函数真正的值

**arguments对象**

- arguments对象包含了函数调用的参数数组

  ```js
  x = findMax(1, 123, 500, 115, 44, 88)
  function findMax() {
      var i, max = arguments[0];
      if(arguments.length < 2) return max;
      for(i = 0; i < arguments.length; i++){
          if(arguments[i] > max){
              max = arguments[i]
          }
      }
      return max;
  }
  ```

**通过值传递参数**

- 在函数中调用的参数是函数的隐式参数。JavaScript 隐式参数通过值来传递：函数仅仅只是获取值。如果函数修改参数的值，不会修改显式参数的初始值（在函数外定义）。隐式参数的改变在函数外是不可见的。

**通过对象传递参数**

- 在JavaScript中，可以引用对象的值。因此我们在函数内部修改对象的属性就会修改其初始的值。修改对象属性可作用于函数外部（全局变量）。修改对象属性在函数外是可见的。

### 2.3、JavaScript函数调用



















## 3、JS问题总结

### 3.1、对象

#### 3.1.1、对象使用和属性

- JavaScript 中所有变量都可以当作对象使用，除了两个例外 [`null`](http://bonsaiden.github.io/JavaScript-Garden/zh/#core.undefined) 和 [`undefined`](http://bonsaiden.github.io/JavaScript-Garden/zh/#core.undefined)。

```js
false.toString(); // 'false'
[1, 2, 3].toString(); // '1,2,3'

function Foo(){}
Foo.bar = 1;
Foo.bar; // 1
```

- 一个常见的误解是数字的字面值（literal）不能当作对象使用。这是因为 JavaScript 解析器的一个错误， 它试图将*点操作符*解析为浮点数字面值的一部分。

```js
2.toString(); // 出错：SyntaxError
```

- 有很多变通方法可以让数字的字面值看起来像对象。

```js
2..toString(); // 第二个点号可以正常解析
2 .toString(); // 注意点号前面的空格
(2).toString(); // 2先被计算
```

***对象作为数据类型***

- JavaScript 的对象可以作为[*哈希表*](http://en.wikipedia.org/wiki/Hashmap)使用，主要用来保存命名的键与值的对应关系。

- 使用对象的字面语法 - `{}` - 可以创建一个简单对象。这个新创建的对象从 `Object.prototype` [继承](http://bonsaiden.github.io/JavaScript-Garden/zh/#object.prototype)下面，没有任何[自定义属性](http://bonsaiden.github.io/JavaScript-Garden/zh/#object.hasownproperty)。

```js
var foo = {}; // 一个空对象

// 一个新对象，拥有一个值为12的自定义属性'test'
var bar = {test: 12}; 
```

***访问属性***

- 有两种方式来访问对象的属性，点操作符或者中括号操作符

```js
var foo = {name:'zhang'};
foo.name; // zhang
foo['name']; //zhang

var get = 'name';
foo[get]; //zhang

foo.1234; //SyntaxError 对象代表尝试解决语法上不合法的代码的错误，Javascript引擎发现了不符合语法规范的tokens或token顺序时抛出SyntaxError.
foo[1234]; //undefined
```

- 两种语法是等价的，但是中括号操作符在下面两种情况依然有效
  - 动态属性设置
  - 属性名不是一个有效的变量名（比如属性名中包含空格，或者属性名是JS关键字）

***删除属性***

- 删除属性的唯一方法是使用 `delete` 操作符；设置属性为 `undefined` 或者 `null` 并不能真正的删除属性， 而**仅仅**是移除了属性和值的关联。

```js
var obj = {
    bar : 1,
    foo : 2,
    baz : 3
};
obj.bar = undefined;
obj.foo = null;
delete obj.baz;
for(var i in obj){
    if(obj.hasOwnProperty(i)){
        console.log(i, ''+obj[i]);
    }
}
```

- 上面的输出结果有 `bar undefined` 和 `foo null` - 只有 `baz` 被真正的删除了，所以从输出结果中消失。

***属性名的语法***

```js
var test = {
    'case': 'I am a keyword so I must be notated as a string',
    delete: 'I am a keyword too so me' // 出错：SyntaxError
};
```

- 对象的属性名可以使用字符串或者普通字符声明。但是由于 JavaScript 解析器的另一个错误设计， 上面的第二种声明方式在 ECMAScript 5 之前会抛出 `SyntaxError` 的错误。

- 这个错误的原因是 `delete` 是 JavaScript 语言的一个*关键词*；因此为了在更低版本的 JavaScript 引擎下也能正常运行， 必须使用*字符串字面值*声明方式。

#### 3.1.2、原型

