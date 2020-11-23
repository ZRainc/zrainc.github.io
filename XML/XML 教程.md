# XML 教程

XML 指可扩展标记语言（e**X**tensible **M**arkup **L**anguage）

XML 被设计用来传输和存储数据

## 1、XML 简介

XML 被设计用来传输和存储数据

HTML 被设计用来显示数据

**什么是XML**

- XML 可扩展标记语言
- 像HTML的标记语言
- XML 的设计宗旨是传输数据，不是显示数据
- XML 标签没有被预定，需要自行标签
- XML 被设计为具有自我描述性

**XML 不会做任何事**

- XML 不会做任何事情，XML被设计用来结构化、存储以及传输信息

  ```xml
  <note>
  	<to>Tove</to>
      <from>Jani</from>
      <heading>Reminder</heading>
      <body>Do not forget me this weekend!</body>
  </note>
  ```

  以上的这些信息具有自我描述性，包含了发送者和接收者的信息，同时拥有标题以及消息主体

  通过 XML 可以发明自己的标签

## 2、XML 用途

- XML 把数据从 HTML分离
- XML 简化数据共享、传输和平台变更等

## 3、XML 树结构

- XML 文档形成了一种树结构，它从“根部”开始，然后扩展到“枝叶”

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<note>
	<to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Do not forget me this weekend!</body>
</note>
```

第一行是 XML 声明。它定义 XML 的版本（1.0）和所使用的编码（UTF-8 : 万国码, 可显示各种语言）

根元素：<note>

## 4、XML 语法结构

- XML 文档必须有根元素

- XML声明，可选，有的话要放在第一行

- 所有XML 元素都必须有一个关闭标签

- XML 标签对大小写敏感

- XML 必须正确嵌套

- XML 的属性值必须加引号

  ```xml
  <note date=12/11/2007> <!--错误 -->
  <to>Tove</to>
  <from>Jani</from>
  </note>
  
  <note date="12/11/2007"> <!--正确 -->
  <to>Tove</to>
  <from>Jani</from>
  </note>
  ```

- **实体引用**

  在xml 中，一些字符具有特殊的意义

  如果您把字符 "<" 放在 XML 元素中，会发生错误，这是因为解析器会把它当作新元素的开始。

  这样会产生 XML 错误：

  ```xml
  <message>if salary < 1000 then</message>
  ```

  为了避免这个错误，请用**实体引用**来代替 "<" 字符：

  ```
  <message>if salary &lt; 1000 then</message>
  ```

  在 XML 中，有 5 个预定义的实体引用：

  | &lt;   | <    | less than      |
  | ------ | ---- | :------------- |
  | &gt;   | >    | greater than   |
  | &amp;  | &    | ampersand      |
  | &apos; | '    | apostrophe     |
  | &quot; | "    | quotation mark |

  **注释：**在 XML 中，只有字符 "<" 和 "&" 确实是非法的。大于号是合法的，但是用实体引用来代替它是一个好习惯。

- XML 注释

  ```xml
  <!-- 这是一个注释 -->
  ```

- XML 中，空格会被保留

  HTML 会把多个连续的空格字符裁减（合并）为一个，

  XML 文档的空格不会被删减

- XML 以 LF 存储换行

## 5、XML 元素

XML 文档包含 XML 元素

XML 元素指的是从（且包括）开始标签直到（且包括）结束标签的部分，一个标签可以包括：其他元素、文本、属性或混合以上所有

**XML 命名规则**

- 名称可以包含字母、数字以及其他字符
- 不能以数字或者标点符号开始
- 名称不能以字母xml（或者 XML、Xml 等等）开始
- 名称不能包含空格
- 可使用任何名称，没有保留的字词

**最佳命名习惯**

- 是名称具有描述性，下划线很不错，<first_name>、<last_name>

- 避免适应"-"、"."、":"

**XML元素是可扩展的**

- xml的优势之一，就是可以在不中断应用程序的情况下进行扩展

## 6、XML 属性

- XML 元素具有属性，类似 HTML，属性提供有关元素的额外信息

- XML 属性必须加引号

- XML属性 VS 元素

  <person sex="female">
  <firstname>Anna</firstname>
  <lastname>Smith</lastname>
  </person>

  

  <person>
  <sex>female</sex>
  <firstname>Anna</firstname>
  <lastname>Smith</lastname>
  </person>

  在第一个实例中，sex 是一个属性。在第二个实例中，sex 是一个元素。这两个实例都提供相同的信息。

- 尽量使用元素来描述数据，而仅仅使用属性来提供与数据无关的信息

## 7、XML 验证

拥有正确语法的XML被称为形式良好的XML，通过DTD验证的XML 是合法的XML

**XML DTD**

- DTD的目的是定义XML文档的结构，它使用多个合法的结构来定义文档元素

  ```dtd
  <!DOCTYPE note
  [
  <!ELEMENT note (to,from,heading,body)>
  <!ELEMENT to (#PCDATA)>
  <!ELEMENT from (#PCDATA)>
  <!ELEMENT heading (#PCDATA)>
  <!ELEMENT body (#PCDATA)>
  ]>
  ```

**XML模式**

- W3C支持一种基于XML的DTD替代者，他称为XML Schema:

  ```xml
  <xs:element name="note">
  
  <xs:complexType>
  <xs:sequence>
  <xs:element name="to" type="xs:string"/>
  <xs:element name="from" type="xs:string"/>
  <xs:element name="heading" type="xs:string"/>
  <xs:element name="body" type="xs:string"/>
  </xs:sequence>
  </xs:complexType>
  
  </xs:element>
  ```

## 8、XML CSS

通过使用CSS（层叠样式表并置样式表），您可以添加显示信息到XML文档中。将XML根据CSS定义的样式展示

## 9、XML XSLT

通过使用XSTL，你可以把 XML 文档转换成 HTML 格式