# JDBC编程

## 1、JDBC简介

JDBC是Java DataBase Connectivity的缩写，它是Java程序访问数据库的标准接口。

使用Java程序语言访问数据库时，Java代码并不是直接通过TCP连接去访问数据库，而是通过JDBC接口来访问，而JDBC接口则通过JDBC驱动来实现真正对数据库的访问。

从代码来看，Java标准库自带的JDBC接口其实就是定义了一组接口，而某个具体的JDBC驱动其实就是实现了这些接口的类。

![image-20200922151544970](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200922151544970.png)

实际上，一个MySQL的JDBC的驱动就是一个jar包，它本身也是纯java编写的。我们自己编写的代码只需要引用Java标准库提供的java.sql包下面的相关接口，由此再间接的通过MySQL驱动的jar包通过网络访问MySQL服务器，所有复杂的网络通讯都被封装到JDBC驱动中，因此，Java程序本身只需要引入一个MySQL驱动的jar包就可以正常访问MySQL服务器。

![image-20200922152428666](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200922152428666.png)

**使用JDBC的好处**

- 各数据厂商使用相同的接口，Java代码不需要针对不同数据库分别开发
- Java程序编译期仅依赖java.sql包，不依赖具体数据库的jar包
- 可随时替换底层数据库，访问数据库的Java代码基本不变

## 2、JDBC查询

Java程序要通过JDBC接口来查询数据库。JDBC是一套接口规范，就在Java的标准库`java.sql`里放着。接口并不能直接实例化，而是必须实例化对应的实现类，然后通过接口引用这个实例。那么JDBC实现类在哪？

因为JDBC接口并不知道我们要使用哪个数据库，所以，用哪个数据库，我们就去使用哪个数据库的”实现类“，我们把某个数据库实现了JDBC接口的jar包称为JDBC驱动

所谓JDBC驱动，就是一个第三方的jar包，我们直接添加一个Maven依赖就可以了

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
    <scope>runtime</scope>
</dependency>
```

注意这里添加的依赖`scope`是`runtime`，因为编译Java程序并不需要这个jar包，只有在运行时才需要。如果把`runtime`改成`compile`，虽然也能正常编译，但是在IDE里写程序的时候，会多出来一大堆类似`com.mysql.jdbc.Connection`这样的类，非常容易与Java标准库的JDBC接口混淆，所以坚决不要设置为`compile`。

### 2.1、JDBC连接

**Connection**：Connection代表一个JDBC连接，它相当于Java程序到数据库的连接（通常是TCP连接）。打开一个Connection时，需要准备URL、用户名和口令，才能成功连接到数据库。

假设数据库运行在本机`localhost`，端口使用标准的`3306`，数据库名称是`learnjdbc`，那么URL如下：

```
jdbc:mysql://localhost:3306/learnjdbc?useSSL=false&characterEncoding=utf8
```

>后面的两个参数表示不使用SSL加密，使用UTF-8作为字符编码（注意MySQL的UTF-8是`utf8`）

要获取数据库连接，使用如下代码：

```java
//jdbc连接的URL，不同的数据库有不同的格式
String JDBC_URL = "jdbc:mysql://localhost:3306/learnjdbc";
String JDBC_USER = "root";
String JDBC_PASSWORD = "password";
//获取连接
Connection con = DriverManager.getConnection("JDBC_URL,JDBC_USER,JDBC_PASSWORD");
// TODO：访问数据库。。。
//关闭连接
con.close();
```

核心代码是`DriverManager`提供的静态方法`getConnection()`。`DriverManager`会自动扫描classpath，找到所有的JDBC驱动，然后根据我们传入的URL自动选择一个合适的驱动。

因为JDBC连接是一种昂贵的资源，所以使用后要及时释放。使用`try(resource)`来自动释放JDBC连接资源

```java
try (Connection con = DriverManager.getConnection("JDBC_URL,JDBC_USER,JDBC_PASSWORD")){
	//代码
}
```

>注：Java TryWtihResource语句用来在该语句结束时自动关闭资源
>
>try() {} ，括号中填入我们需要使用的资源（数据库连接，网络连接等），try语句在该语句结束时自动关闭这些资源。如果try()里面有两个资源，用逗号分开，资源的close方法的调用顺序与它们的创建顺序相反。带有资源的try语句可以像一般的try语句一样具有catch和finally块。在try-with-resources语句中，任何catch或finally块都是在声明的资源被关闭后才会执行的。

### 2.2、JDBC查询

获取到JDBC连接后，就可以查询数据库了。查询数据库分以下几个步骤：

1、通过`Connection`提供的`createStatement()`方法创建一个`Statement`对象，用于执行一个查询；

2、执行`Statement`对象提供的`executeQuery(Select * FROM students)`并传入SQL语句，执行查询并获得返回结果集，使用`ResultSet`来引用这个结果集；

3、反复调用`ResultSet`的`next()`方法并读取每一行结果

代码如下：

```java
try(Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {
    try(Statement stmt = conn.createStatement()) {
        try(ResultSet rs =  stmt.executeQuery("SELECT id,grade,name,gender FROM students WHERE gender="1")) {
            while( rs.next()){
                long id = rs.getLong(1); //注意：索引是从1开始的
                long grade = rs.getLong(2);
                String name = rs.getString(3);
                int gender = rs.getInt(4);
            }
        }
    }
}
```

注意要点：

- `Connection`、`Statement`和`ResultSet`都是需要关闭的资源，因此嵌套使用`try(resource)`确保即使关闭
- `rs.next()`用于判断是否有下一行记录，如果有，将自动把当前行移动到下一行（一开始获得`ResultSet`时当前行不是第一行）
- `ResultSet`获取列时，索引从`1`开始而不是`0`
- 必须根据`SELECT`的列的对应位置来调用`getLong(1)`，`getString(2)`这些方法，否则对应位置的数据类型不对，将报错。

### 2.3、SQL注入

使用`Statement`拼字符非常引发SQL注入问题，这是因为SQL参数往往是从方法参数传入的。

**SQL注入带来的问题**

我们来看一个例子：

```java
User login(String name, String pass){
    //...
     stmt.executeQuery("SELECT * FROM user WHERE login='" + name + "' AND pass='" + pass + "'");
    //...
}
```

其中，参数`name`和`pass`通常都是Web页面输入后由程序接收到的。

如果用户的输入是程序期待的值，就可以拼出正确的SQL。例如：name = `"bob"`，pass = `"1234"`：

```java
SELECT * FROM user WHERE login='bob' AND pass='1234'
```

但是，如果用户的输入是一个精心构造的字符串，就可以拼出意想不到的SQL，这个SQL也是正确的，但它查询的条件不是程序设计的意图。例如：name = `"bob' OR pass="`, pass = `" OR pass='"`：

```java
SELECT * FROM user WHERE login='bob' OR pass=' AND pass=' OR pass=''
```

这个SQL语句执行的时候，根本不用判断口令是否正确，这样一来，登录就形同虚设。

**SQL注入解决方式**

要避免SQL注入攻击，一个办法是针对所有字符串参数进行转义，但是转义很麻烦，而且需要在任何使用SQL的地方增加转义代码。

还有一种方式使用`PreparedStatement`。使用`PreparedStatement`可以**完全避免SQL注入问题**，因为`PreparedStatement`始终使用`?`作为占位符，并且把数据库连同SQL本身传给数据库，这样可以保证每次传给数据库的SQL语句是相同的，只是占位符的数据不同，还能高效利用数据库本身对查询的缓存。上述登录SQL如果用`PreparedStatement`可以改写如下：

```java
User login(String name, String pass) {
    ...
    String sql = "SELECT * FROM user WHERE login=? AND pass=?";
    PreparedStatement ps = conn.prepareStatement(sql);
    ps.setObject(1, name);
    ps.setObject(2, pass);
    ...
}
```

所以，`PreparedStatement`比`Statement`更安全，而且更快。

我们把上面使用`Statement`的代码改为使用`PreparedStatement`：

```java
try(Connection conn = DriverManager.getConnection(JDBC_URL,JDBC_USER,JDBC_PASS)) {
    try(PreparedStatement ps = conn.PreparedStatement("SELECT id, grade, name, gender FROM students WHERE gender=? AND grade=?")) {
        ps.setObject(1,"M");
        ps.setObject(2, 3);
        try(ResultSet rs = ps.executeQuery()) {
            while(rs.next()) {
                long id = rs.getLong("id");
                long grade = rs.getLong("grade");
                String name = rs.getString("name");
                String gender = rs.getString("gender");
            }
        }
    }
}
```

使用`PreparedStatement`比`Statement`稍有不同，首先必须调用`setObject()`设置每个占位符`?`的值，最后获取的仍然是`ResultSet`对象。

另外注意到从结果集读取列时，使用`String`类型的列名比索引要易读，而且不容易出错。

注意到JDBC查询的返回值总是`ResultSet`，即使我们写这样的聚合查询`SELECT SUM(score) FROM ...`，也需要按结果集读取：

```java
ResultSet rs = ...
if (rs.next()) {
    double sum = rs.getDouble(1);
}
```

### 2.4、Statement和PreparedStatement区别

先来说说，什么是java中的Statement：Statement是java执行数据库操作的一个重要方法，用于在已经建立数据库连接的基础上，向数据库发送要执行的SQL语句。具体步骤：

​		1.首先导入java.sql.\*；这个包

​		2.然后加载驱动，创建连接，得到Connection接口的的实现对象，比如对象名叫做conn

​		3.然后再用conn对象去创建Statement的实例，方法是：Statement stmt = conn.creatStatement("SQL语句字符串")

　　Statement 对象用于将 SQL 语句发送到数据库中。实际上有三种 Statement 对象，它们都作为在给定连接上执行 SQL语句的包容器：Statement、PreparedStatement（它从 Statement 继承而来）和CallableStatement（它从 PreparedStatement 继承而来）。它们都专用于发送特定类型的 SQL 语句：Statement 对象用于执行不带参数的简单 SQL 语句；PreparedStatement 对象用于执行带或不带参数的预编译 SQL 语句；CallableStatement 对象用于执行对数据库已存储过程的调用。

​	**综上所述，总结如下**：Statement每次执行sql语句，数据库都要执行sql语句的编译，最好用于仅执行一次查询并返回结果的情形，效率高于PreparedStatement，但存在sql注入风险。PreparedStatement是预编译执行的。在执行可变参数的一条SQL时，PreparedStatement要比Statement的效率高，因为DBMS预编译一条SQL当然会比多次编译一条SQL的效率高。安全性更好，有效防止SQL注入的问题。对于多次重复执行的语句，使用PreparedStatement效率会更高一点。执行SQL语句是可以带参数的，并支持批量执行SQL。由于采用了Cache机制，则预编译的语句，就会放在Cache中，下次执行相同的SQL语句时，则可以直接从Cache中取出来。

## 3、JDBC更新

### 3.1、插入

插入操作是`INSERT`，即插入一条新记录。通过JDBC进行插入，本质上也是用`PreparedStatement`执行一条SQL语句，不过最后执行的不是`executeQuery`，而是执行`executeUpdate`

```java
try(Connection conn = DriverManager.getConnection(JDBC_URL,JDBC_USER,JDBC_PASS)) {
    try(PreparedStatement ps = conn.PreparedStatement("INSERT INTO students (id, grade, name, gender) VALUES (?,?,?,?)")) {
        ps.setObject(1,999); // 注意：索引从1开始
        ps.setObject(2, 1);  //grade
        ps.setObject(3, "Bob"); // name
        ps.setObject(4, "M"); // gender
        int n = ps.excuteUpdate(); // 1
    }
}
```

设置参数与查询是一样的，有几个`?`占位符就必须设置对应的参数。虽然`Statement`也可以执行插入操作，但我们仍要严格遵循**绝对不能收到拼SQL字符串**的原则，以避免安全漏洞。

当成功执行`executeUpdate()`后，返回值是int，表示插入的记录数量。此处总是`1`，因为只插入了一条记录

### 3.2、插入并获取主键

如果数据库的表设置了主键，那么在执行`INSERT`语句时，并不需要指定主键，数据库会自动分配主键。对于使用自增主键的程序，有个额外的步骤，就是如何获取插入后的自增主键的值

要获取自增主键，不能先插入，再查询。因为两条SQL执行期间可能有别的程序也插入了同一个表。获取自增主键最正确的写法是在创建`PreparedStatement`的时候，指定一个`RETURN_GENERATED_KEYS`标志位，表示JDBC必须返回插入的自增主键。实例代码如下：

```java
try(Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)){
    try(PreparedStatement ps = conn.PreparedStatement("INSERT INTO students (grade, name, gender) VALUES (?,?,?)", Statement.RETURN_GENERATE_KEYS)) {
        ps.setObject(1, 1);  //grade
        ps.setObject(2, "Bob"); // name
        ps.setObject(3, "M"); // gender
        int n = ps.excuteUpdate(); // 1
        try(ResultSet rs = ps.getGenerateKeys()) {
            if(rs.next()) {
                long id = re.getLong(1); // 索引从1开始
            }
        }
    }
}
```

注意事项：

1、一是调用`preparedStatement()`时，第二个参数必须传入常量`Statement.RETURN_GENERATED_KEYS`，否则JDBC驱动不会返回自增主键

2、执行`executeUpdated`方法后，必须调用`getGeneratedKeys`获取一个`ResultSet`对象，这个对象包含了数据库自动生成的主键的值，读取该对象每一行来获取自增主键的值。

### 3.3、更新

更新操作`Update`语句，它可以一次更新若干列的记录。更新操作和插入操作在JDBC代码的层面上实际上没有区别，除了SQL语句不同：

```java
try(Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASS)) {
	try(PreparedStatement ps = conn.prepareStatement("UPDATE students SET name = ? WHERE id = ?")) {
        ps.setObject(1,"Bob");
        ps.serObject(2, 999);
        int n = ps.excuteUpdate(); //返回更新的行数
    }
}
```

`executeUpdate()`返回数据库实际更新的行数。返回结果可能是正数，也可能是0（表示没有任何记录更新）。 

### 3.4、删除

删除操作是`DELETE`语句，它可以一次删除若干列，和更新一样，除了SQL语句不同外，JDBC代码都是相同的：

```java
try(Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASS)) {
	try(PreparedStatement ps = conn.prepareStatement("DELETE FROM students WHERE id = ?")) {
        ps.setObject(1,3);
        int n = ps.excuteUpdate(); //返回删除的行数
    }
}
```

## 4、JDBC事务

数据库事务（Transaction）是由若干个SQL语句构成的一个操作序列，有点类似于Java的`synchronized`同步。数据库系统保证在一个事务中要么所有SQL语句全部执行成功，要么全部都不执行，即数据库具有ACID特性：

- Atomicity：原子性，将所有SQL作为原子工作执行单元，要么全部执行，要么全部不执行；
- Consistency：一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户必定增加100；
- Isolation：隔离性，如果有多个事务并发执行，每个事务做出的修改必然与其他事务隔离；
- Durability：持久性，即事务完成后，对数据库数据的修改被持久化存储

对于单条SQL语句，数据库系统自动将其作为一个事务执行，这种事务被称为**隐式事务**

要手动把多条SQL语句作为一个事务执行，使用`BEGIN`开启一个事务，使用`COMMIT`提交一个事务，这种事务被称为**显示事务**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

很显然多条SQL语句要想作为一个事务执行，就必须使用显式事务。

`COMMIT`是指提交事务，即试图把事务内的所有SQL所做的修改永久保存。如果`COMMIT`语句执行失败了，整个事务也会失败。

有些时候，我们希望主动让事务失败，这时，可以用`ROLLBACK`回滚事务，整个事务会失败：

```
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
ROLLBACK;
```

数据库事务是由数据库系统保证的，我们只需要根据业务逻辑使用它就可以。

###  4.1、事务隔离级别

数据库事务可以并发执行，而数据库系统从效率考虑，对事务定义了不同的隔离级别。SQL标准定义了4种隔离级别，分别对应可能出现的数据不一致的情况：

| Isolation Level  | 脏读（Dirty Read） | 不可重复读（Non Repeatable Read） | 幻读（Phantom Read） |
| :--------------- | :----------------- | :-------------------------------- | :------------------- |
| Read Uncommitted | Yes                | Yes                               | Yes                  |
| Read Committed   | -                  | Yes                               | Yes                  |
| Repeatable Read  | -                  | -                                 | Yes                  |
| Serializable     | -                  | -                                 | -                    |

对应用程序来说，数据库事务非常重要，很多运行着关键任务的应用程序，都必须依赖数据库事务保证程序的结果正常。

### 4.2、Read Uncommitted

Read Uncommitted是隔离级别最低的一种事务级别。在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读取到的数据就是脏数据，这就是脏独（Dirty Read）

### 4.3、Read Committed

在Read Committed隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。

不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据可能就不一致

### 4.4、Repeatable Read

在Repeatable Read隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。

幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

### 4.5、Serializable

Serializable是最严格的隔离级别。在Serializable隔离级别下，所有事务按照次序依次执行，因此，脏读、不可重复读、幻读都不会出现。

虽然Serializable隔离级别下的事务具有最高的安全性，但是，由于事务是串行执行，所以效率会大大下降，应用程序的性能会急剧降低。如果没有特别重要的情景，一般都不会使用Serializable隔离级别。

###  4.6、默认隔离级别

如果每天指定隔离级别，数据库就会使用默认隔离级别。在MySQL中，如果使用InnoDB，默认的隔离级别时**可重复读**

### 4.7、JDBC事务

对应于程序来说，数据库事务非常重要，很多运行着关键任务的应用程序，都必须依赖数据库事务保证程序的结果正常。

举个例子：假设小明准备给小红支付100，两人在数据库中的记录主键分别是`123`和`456`，那么用两条SQL语句操作如下：

```sql
UPDATE accounts SET balance = balance - 100 WHERE id = 123 AND balance >= 100;
UPDATE accounts SET balance = balance + 100 WHERE id = 456;
```

这两条语句必须以事务的方式执行才能保证业务的正确性，因为一旦第一条执行成功而第二条执行失败，系统就会凭空减少100，而有了事务，要么执行成功，要么失败

JDBC事务代码：

```java
Connection conn = openConnection();
try{
    //关闭自动提交
    conn.setAutoCommit(false);
    //执行多条SQL语句
    insert(); update(); delete();
    //提交事务
    conn.commit;
} catch (SQLException e) {
    //回滚事务
    conn.rollback();
} finally {
    conn.setAutoCommit(true);
    conn.close();
}
```

其中，开启事务的关键代码是`conn.setAutoCommit(false)`，表示自动提交关闭。提交事物的代码在在执行指定若干SQL语句后，调用`conn.commit()`。要注意事务不总是能成功，如果事务提交失败，会抛出SQL异常（也可能在执行SQL语句的时候就抛出了），此时我们必须捕获并调用`conn.rollback()`回滚事务。最后，在`finally`中通过`setAutoCommit(true)`把`Connection`对象的状态恢复到初始值

实际上，默认情况下，我们获取到`Connection`连接后，总是处于“自动提交”模式，也就是每执行一条SQL都是作为事务自动执行的，这也是为什么前面几节我们的更新操作总能成功的原因：因为默认有这种“隐式事务”。只要关闭了`Connection`的`autoCommit`，那么就可以在一个事务中执行多条语句，事务以`commit()`方法结束。

设置事务的隔离级别，使用如下代码：

```java
// 设定隔离级别为READ COMMITTED:
conn.setTransactionIsolation(Connection.TRANSRACTION_READ_COMMIT);
```

如果没有调用上述方法，那么会使用数据库的默认隔离级别。MySQL的默认隔离级别是`REPEATABLE READ`。

## 5、JDBC Batch

使用JDBC数据库的时候，经常会执行一些批量操作

例如，一次性给会员增加可用优惠券若干，我们可以执行如下操作：

```sql
INSERT INTO coupons (user_id, type, expires) VALUES (123, "DISCOUNT", "2020-9-23");
INSERT INTO coupons (user_id, type, expires) VALUES (234, "DISCOUNT", "2020-9-23");
INSERT INTO coupons (user_id, type, expires) VALUES (345, "DISCOUNT", "2020-9-23");
INSERT INTO coupons (user_id, type, expires) VALUES (456, "DISCOUNT", "2020-9-23");
......
```

实际上执行JDBC的时候，因为只有占位符不同，所以SQL实际上是一样的：

```java
for(var params : paramsList) {
	PreparedStatement ps = conn.preparedStatement("INSERT INTO coupons (user_id, type, expires) VALUES (?,?,?)");
	ps.setLong(params.get(0));
	ps.setString(params.get(1));
	ps.setString(params.get(2));
	ps.executeUpdate();
}
```

通过一个循环来执行`PreparedStatement`虽然可行，但是性能很低。SQL数据库对SQL语句相同，但只有参数不同的若干语句可以作为batch执行，即批量执行，这种操作有特别优化，速度远远快于循环执行每个SQL

在JDBC代码中，我们可以利用SQL数据库的这一特性，把同一个SQL但参数不同的若干次操作合并为一个batch执行。我们以批量插入为例，示例代码如下：

```java
try(PreparedStatement ps = conn.prepareStatement("INSERT INTO students (name, gender, grade, score) VALUES (?, ?, ?, ?)")) {
    //对同一个PreparedStatement反复设置参数并调用addBatch();
    for(Student s : students) {
        ps.setString(1, s.name);
        ps.setString(2, s.gender);
        ps.setString(3, s.grade);
        ps.setString(4, s.score);
        ps.addBatch(); //添加到batch
    }
    
    //执行batch
    int[] ns = ps.executeBatch();
    for(int n : ns) {
        System.out.println(n + " inserted."); // batch中每个SQL执行的结果数量
    }
}
```

执行batch和执行一个SQL的不同点在于，需要对一个`PreparedStatement`反复设置参数并调用`addBatch()`，这样就相当于给一个SQL加上了多组参数，相当于变成了“多组”SQL.

第二个不同点是调用的不是`executeUpdate()`，而是`executeBatch()`，因为我们设置了多组参数，相应地，返回结果也是多个`int`值，因此返回类型是`int[]`，循环`int[]`数组即可获取每组参数执行后影响的结果数量。

## 6、JDBC连接池

我们学习多线程的时候知道，创建线程是一个昂贵的操作，如果有大量的小任务需要执行，并且频繁的创建个销毁线程，实际上会消耗大量系统资源，往往创建和销毁线程所需要的时间比任务执行时间还要长，所以，为了提高效率，可以使用线程池。

类似的，我们在使用JDBC的增删改查操作时，如果每一次操作基于打开一次连接，操作，关闭连接，那么创建和销毁`Connection`的代价就太大了。为了避免频繁的创建和销毁JDBC连接，我们可以通过连接池（Connection Pool）复用已经创建好的连接。

JDBC连接池有一个标准的接口`java.sql.DaraSource`，注意这个类仅仅是接口。要使用JDBC连接池，我们必须选择一个JDBC连接池接口的实现。常用JDBC连接池有：

- HikariCP
- C3P0
- BoneCP
- Druid

目前使用最广泛的是HikariCP。我们以HikariCP为例，要使用JDBC连接池，先添加HikariCP的依赖如下：

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>2.7.1</version>
</dependency>
```

紧接着，我们创建一个`DataSource`实例，这个实例就是连接池：

```java
HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:mysql://localhost:3306/test");
config.setUsername("root");
config.setPassword("password");
config.addDataSourceProperty("connectionTimeout", "1000"); //连接超时：1s
config.addDataSourceProperty("idleTimeout", "60000"); //空闲超时：60s
config.addDataSourceProperty("maximumPoolSize", "10"); // 最大连接数：10
DataSource ds = new HikariDataSource(config);
```

注意创建`DataSource`是一个非常昂贵的操作，所以`DataSource`实例总是作为一个全局变量存储，并贯穿整个应用程序声明周期。

有了连接池以后，我们如何使用它呢？和前面的代码类似，只是获取`Connection`时，把`DriverManage.getConnection()`改为`ds.getConnection()`：

```java
try (Connection conn = ds.getConnection()) { // 在此获取连接
	//
} //在此关闭连接
```

通过连接池获取连接，并不需要把JDBC的相关URL、用户名、口令等信息，因为这些信息已经存储在线程池内部了，（创建`HikariDataSource`时传入的`HikariConfig`持有这些信息）。一开始，线程池内部并没有连接，所以，第一次调用`ds.getConnection()`，会迫使线程池内部先创建一个`Connection`，返回给客户端使用。当我们调用`conn.close()`方法时（`在try(resource){...}`结束处），并不是真正的"关闭"连接，而是释放到线程池中，以便下次获取连接能直接返回。

因此，线程池内部维护若干个`Connection`实例，如果调用`ds,getConnection()`，就选择一个空闲的连接，并标记为正在使用然后返回，如果对`Connection`调用`close()`，就将连接标记为空闲并释放回线程池中。这样一来，我们就通过连接池维护少量连接，便可频繁执行大量SQL语句。

连接池通常提供大量参数可配置的。例如，维护的最小、最大活动连接数，指定一个连接在空闲一段时间后自动关闭等，需要根据应用程序的负载合理地配置这些参数。此外，大多数连接池都提供了详细的实时状态以便进行监控。





















