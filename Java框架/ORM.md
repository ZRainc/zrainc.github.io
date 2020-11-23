# ORM

面向对象编程和关系型数据库，都是目前最流行的数据，但是它们的模型是不一样的。、

面向对象编程把所有实体看成对象（object），关系型数据库则是采用实体之间的关系（relation）连接数据。很早就有人提出，关系也可以用对象表达，这样的话，就能使用面向对象的编程语言，来操作关系型数据库。

![img](https://www.wangbase.com/blogimg/asset/201902/bg2019021802.png)

**简单来说，ORM就是通过实例对象的语法，完成关系型数据库的操作的技术，是“对象—关系映射”（Object/Relational Mapping）的缩写**

ORM把数据库映射成对象：

```
- 数据库的表（table） --> 类（class）
- 记录（record，行对象） --> 对象（object）
- 字段（field） --> 对象的属性（attribute）
```

举例来说，下面是一行 SQL 语句。

> ```javascript
> SELECT id, first_name, last_name, phone, birth_date, sex
>  FROM persons 
>  WHERE id = 10
> ```

程序直接运行 SQL，操作数据库的写法如下。

> ```javascript
> res = db.execSql(sql);
> name = res[0]["FIRST_NAME"];
> ```

改成 ORM 的写法如下。

> ```javascript
> p = Person.get(10);
> name = p.first_name;
> ```

一比较就可以发现，ORM 使用对象，封装了数据库操作，因此可以不碰 SQL 语言。开发者只使用面向对象编程，与数据对象直接交互，不用关心底层数据库。

**优点**

>- 数据模型都在一个地方定义，更容易更新和维护，也利于代码重用
>- ORM 有现成的工具，很多功能可以自动完成，比如数据消毒、预处理、事务等等
>
>- 它迫使你使用MVC架构，ORM就是天然的 Model，最终使代码更清晰
>- 基于 ORM 的业务代码比较简单，代码量少，语义性好，容易理解
>- 你不必编写性能不佳的 SQL

**缺点**

>- ORM 库不是轻量级工具，需要花很多精力学习和设置
>- 对于复杂的查询，ORM要么是无法表达，要么是性能不如原生的SQL
>- ORM抽象掉了数据库底层，开发者无法了解底层的数据库操作，也无法定制一些特殊的SQL































