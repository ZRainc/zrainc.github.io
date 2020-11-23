# XML 进阶

## 1、XML 命名空间

XML 命名空间提供避免元素命名冲突的方法

**命名冲突**

在 XML 中，元素名称是由开发者定义的，当两个不同的文档使用相同的元素名时，就会发生命名冲突。

这个 XML 携带 HTML 表格的信息：

```xml
<table>
<tr>
<td>Apples</td>
<td>Bananas</td>
</tr>
</table>
```

这个 XML 文档携带有关桌子的信息（一件家具）：

```xml
<table>
<name>African Coffee Table</name>
<width>80</width>
<length>120</length>
</table>
```

假如这两个 XML 文档被一起使用，由于两个文档都包含带有不同内容和定义的 <table> 元素，就会发生命名冲突。

XML 解析器无法确定如何处理这类冲突。

**使用前缀来避免命名冲突**

- 在XML中的命名冲突可以通过使用名称前缀从而容易地避免

  ```xml
  <h:table>
  <h:tr>
  <h:td>Apples</h:td>
  <h:td>Bananas</h:td>
  </h:tr>
  </h:table>
  
  <f:table>
  <f:name>African Coffee Table</f:name>
  <f:width>80</f:width>
  <f:length>120</f:length>
  </f:table>
  ```

**XML 命名空间 - xmlns 属性**

- 当在 XML 中使用前缀时，一个所谓的用于前缀的命名空间被必须定义

  ```xml
  <root>
  
  <h:table xmlns:h="http://www.w3.org/TR/html4/">
  <h:tr>
  <h:td>Apples</h:td>
  <h:td>Bananas</h:td>
  </h:tr>
  </h:table>
  
  <f:table xmlns:f="http://www.w3cschool.cc/furniture">
  <f:name>African Coffee Table</f:name>
  <f:width>80</f:width>
  <f:length>120</f:length>
  </f:table>
  
  </root>
  ```

  当命名空间被定义在元素的开始标签中时，所有带有相同前缀的子元素都会与同一个命名空间相关联。

**默认的命名空间**

- 为元素定义默认的命名空间可以让我们省去在所有的子元素中使用前缀的工作。它的语法如下：xmlns="*namespaceURI*"

  ```xml
  <table xmlns="http://www.w3.org/TR/html4/">
  <tr>
  <td>Apples</td>
  <td>Bananas</td>
  </tr>
  </table>
  ```

## 2、XML CDATA

**PCDATA - 被解析的字符数据**

- XML 文档中所有文本均会被解析器解析

**CDATA - (未解析)字符数据**

- 术语CDATA是不应该由 XML 解析器解析的文本数据
- "<"和"&"字符在XML元素中都是非法的
- CDATA 部分由"<![CDATA["开始，由"]]>" 结束：

## 3、服务器上的XML

XML 文件是类似 HTML 文件的纯文本文件，XML 能够通过标准的Web服务器轻松的存储和生成

## 4、XML 总结

XML 可用于交换、共享和存储数据。

XML 文档形成 [树状结构](https://www.runoob.com/xml/xml-tree.html)，在"根"和"叶子"的分支机构开始的。

XML 有非常简单的 [语法规则](https://www.runoob.com/xml/xml-syntax.html)。带有正确语法的 XML 是"形式良好"的。有效的 XML 是针对 [DTD](https://www.runoob.com/xml/xml-dtd.html) 进行验证的。

[XSLT](https://www.runoob.com/xml/xml-xsl.html) 用于把 XML 转换为其他格式，比如 HTML。

所有现代的浏览器有一个内建的 [XML 解析器](https://www.runoob.com/xml/xml-parser.html)，可读取和操作 XML。

[DOM](https://www.runoob.com/xml/xml-dom.html)（Document Object Model）定义了一个访问 XML 的标准方式。

[XMLHttpRequest](https://www.runoob.com/xml/xml-http.html) 对象提供了一个网页加载后与服务器进行通信的方式。

[XML 命名空间](https://www.runoob.com/xml/xml-namespaces.html)提供了一种避免元素命名冲突的方法。

[CDATA](https://www.runoob.com/xml/xml-cdata.html) 区域内的文本会被解析器忽略。

我们的 [XML 实例](https://www.runoob.com/xml/xml-examples.html)也代表了这个 XML 教程总结。