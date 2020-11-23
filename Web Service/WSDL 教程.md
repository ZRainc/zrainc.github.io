# WSDL 教程

WSDL（网络服务描述语言，Web Services Description Language）是一门基于 XML 的语言，用于描述 Web Services 以及如何对他们进行访问

## 1、WSDL 简介

- WSDL 是基于 XML 的用于描述 Web Services 以及如何访问 Web Services 的语言

**什么是 WSDL?**

- WSDL 指网络服务描述语言
- WSDL 使用 XML 编写
- WSDL 是一种 XML 文档
- WSDL 用于描述网络服务
- WSDL 也可用于定位网络服务
- WSDL 还不是 W3C 标准

**WSDL** 是一种 XML 编写的文档，这种文档可描述某个 Web service。它可规定服务器的位置，以及此服务提供的操作（或方法）

## 2、WSDL 文档

- WSDL 文档仅仅是一个简单的 XML 文档。它包含一系列描述某个 Web Services 的定义。

**WSDL 文档结构**

| 元素       | 定义                       |
| :--------- | :------------------------- |
| <portType> | web service 执行的操作     |
| <message>  | web service 使用的消息     |
| <types>    | web service 使用的数据类型 |
| <binding>  | web service 使用的通信协议 |

WSDL 文档可包含其他元素，比如 extension 元素，以及一个 service 元素，此元素可把若干个 web services 的定义组合在一个单一的 WSDL 文档中

**WSDL 端口**

<protType> 元素是最重要的 WSDL元素

它可描述一个 web service、可被执行的操作，以及相关的信息

可以把<portType> 元素比作传统编程语言的一个函数库（或一个模块、或一个类）

**WSDL 消息**

<message> 元素定义一个操作的数据元素

每个消息均由一个或多个部件组成。可以把这些部件比作传统编程语言中一个函数调用的参数

**WSDL types**

<types> 元素定义 web service 使用的数据类型

为了最大程度的平台中立性，WSDL 使用 XML Schema 语法来定义数据类型

**WSDL Bindings**

<binding> 元素为每个端口定义消息格式和协议细节

```xml
<message name="getTermRequest">
	<part name="term" type="xs:string"/>
</message>

<message name="getTermResponse">
	<part name="value" type="xs:string"/>
</message>

<porttype name="glossaryTerms">
	<operation name="getTerm">
    	<input message="getTermRequest"/>
        <output message="getTermResponse"/>
    </operation>
</porttype>
```

在这个例子中，<portType>元素把 "glossaryTerms" 定义为某个*端口*的名称，把 "getTerm" 定义为某个*操作*的名称。

操作 "getTerm" 拥有一个名为 "getTermRequest" 的*输入消息*，以及一个名为 "getTermResponse" 的*输出消息*。

<message>元素可定义每个消息的*部件*，以及相关联的数据类型。

对比传统的编程，glossaryTerms 是一个函数库，而 "getTerm" 是带有输入参数 "getTermRequest" 和返回参数 getTermResponse 的一个函数。

## 3、WSDL 端口

<portType>元素是最重要的 WSDL 元素。

它可描述一个 web service、可被执行的操作，以及相关的消息。

可以把 <portType> 元素比作传统编程语言中的一个函数库（或一个模块、或一个类）。

**操作类型**

请求-操作时最普通的操作类型，不过 WSDL 定义了四种类型：

| 类型             | 定义                                     |
| :--------------- | :--------------------------------------- |
| One-way          | 此操作可接受消息，但不会返回响应。       |
| Request-response | 此操作可接受一个请求并会返回一个响应     |
| Solicit-response | 此操作可发送一个请求，并会等待一个响应。 |
| Notification     | 此操作可发送一条消息，但不会等待响应。   |

**One-way 操作**

```xml
<message name="newTermValues">
	<part name="term" type="xs:string"/>
    <part name="value" type="xs:string"/>
</message>

<portType name="glossaryTerms">
	<operation name="setTerm">
    	<input name="newTerm" message="newTermValues"/>
    </operation>
</portType>
```

在这个例子中，端口"glossaryTerms"定义了一个名为"setTerms"的 one-way 操作

这个"setTerm"操作可接受新术语表项目消息的输入，这些消息使用一条名为"newTermValues"的消息，此消息带有输入参数 "term" 和 "value"。不过，没有为这个操作定义任何输出。

**Resquest-Response**

```xml
<message name="getTermRequest">
	<part name="term" type="xs:string"/>
</message>

<message name="getTermResponse">
	<port name="value" type="xs:string"/>
</message>

<portType name="glossaryTerms">
	<operation name="getTerm">
    	<input message="getTermRequest"/>
        <output messagge="getTermResponse"/>
    </operation>
</portType>
```

在这个例子中，端口 "glossaryTerms" 定义了一个名为 "getTerm" 的 request-response 操作。

"getTerm" 操作会请求一个名为 "getTermRequest" 的输入消息，此消息带有一个名为 "term" 的参数，并将返回一个名为 "getTermResponse" 的输出消息，此消息带有一个名为 "value" 的参数。

## 4、WSDL绑定

WSDL绑定可为web服务定义消息格式和协议细节。

**绑定到SOAP**

```xml
<message name="getTermRequest">
	<part name="term" type="xs:string"/>
</message>

<message name="getTermResponse">
	<part name="value" type="xs:string"/>
</message>

<portType name="glossaryTerms">
	<operation name="getTerm">
    	<input message="getTermRequset"/>
        <output message="getTermResponse"/>
    </operation>
</portType>

<binding type="glossaryTerms" name="b1">
	<soap:binding style="document"
    	transport="http://schemas.xmlsoap.org/soap/http">
    	<operation>
        	<soap:operation 				soapAction="http://example.com/getTerm"/>
            <input><soap:body use="literal"/></input>
            <output><soap:body use="literal"/></output>
        </operation>
    </soap:binding>
</binding>
```

**binding**元素有两个属性name属性和type属性

名称属性定义binding的名称，而type属性指向用于binding的端口，在这个示例中是"glossaryTerms"入口

**soap:binding** 元素有两个属性 style属性和transport属性



