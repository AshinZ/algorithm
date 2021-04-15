# MySQL必知必会



### 概念

数据库：保存有组织的数据的容器

DBMS（数据库管理软件）：数据库是通过DBMS创建和操作的容器。

表：某种特定类型数据的结构化清单

模式：关于数据库和表的布局及特性的信息

列（column）： 表中的一个字段。所有表都是由一个或多个列组成的。

数据类型（datatype）： 所容许的数据的类型。每个表列都有相应的数据类型，它限制（或容许）该列中存储的数据。

行（row）： 表中的一个记录。

主键（primary key）：一列（或一组列），其值能够唯一区分表中每个行。（如果有主键那么不允许重复，且每行必须有主键值）



### 操作

#### 数据库相关

##### 安装数据库

```bash
mysqld --install
```



##### 初始化

```bash
mysqld --initialize --console
```



##### 开启服务

```bash
net start mysql
```



##### 关闭服务

```bash
net stop mysql
```



##### 登陆mysql

```bash
mysql -u root -p
```



##### 修改用户密码

```bash
alter user 'root'@'localhost' identified by 'root';
```



##### 查看数据库

```mysql
show databases;
```



##### 使用某个数据库

```mysql
use TABLENAME
```



##### 查看数据库表情况

```mysql
show tables;
```



##### 创建数据库

```mysql
create database NAME;
```



##### 导入sql文件

```mysql
source FILEPATH;
```



##### 查看表的结构

```mysql
show columns from TABLENAME;
//
desc TABLENAME
```



#### 检索操作

一般来说，检索操作的格式为：

```mysql
select (列名) from 表名 where 条件;
```



##### 去重

可以使用`DISTINC`关键词来进行去重。

如

```mysql
select DISTINCT (列名) from 表名 where 条件;
```

值得注意的是，如果只加在一个前面，而检索了多个列，该关键词只会作用于一个列。



##### 限制

如果返回的数据太多，我们可以在后面加上`limit n`来限制输出的行数。

```mysql
select DISTINCT (列名) from 表名 where 条件 limit n;
```

如果`limit`后面跟两个数字x和y，那么可以表示从第x条开始的y条数据。

如果数据不够则能输出多少输出多少。



##### 完全限定列名

可以通过`tablename.column`的形式来完全限定列名。



#### 排序检索数据

我们可以通过调用`ORDER BY`语句来对我们的查询结果进行排序。

如果不排序，数据会按照在底层中的顺序进行返回。

调用`ORDER BY`的语句：

```mysql
select DISTINCT (列名) from 表名 order by 列名;
```

如果是字符则按字符顺序排，数字则按照数字排。



##### 按照多个列排序

如果期望按照多个列排序，我们可以在`oder by`后面加上列名，按照顺序为排序顺序。

```mysql
select DISTINCT (列名) from 表名 order by 列名,列名;
```



##### 指定排序方向

可以通过`DESC`关键词让数据倒序排序，如果多个列需要倒序，则都需要加`DESC`关键词。

```mysql
select DISTINCT (列名) from 表名 order by 列名 DESC,列名;
```

当然，我们也可以用`ASC`关键词指定升序。



#### 过滤数据

通过添加`where`的表达式，可以过滤掉一部分的数据。

```mysql
select DISTINCT (列名) from 表名 where 条件;
```
> 如果`order by`和`where`一起用，`where`应该放在前面。



支持的子句操作符有如下几类：

![image-20210415220738688](https://i.loli.net/2021/04/15/FZ4GVQzg6LXdEpk.png)

>单引号用来限定字符串。如果将值与串类型的列进行比较，则需要限定引号。

##### between使用

```mysql
select * from table where 列名 between a and b;
```



##### 空值检查

可以通过`is NULL`来判断某个值是否为空

```mysql
select * from table where 列名 is NULL
```



#### 数据过滤

我们可以通过`and`或者`or`操作符来实现更强的数据过滤。



`and`语句

```mysql
select * from table where 列名=a and 列名=b;
```



`or`语句

```mysql
select * from table where 列名=a or 列名=b;
```



##### 计算次序

`and`的处理优先级高于`or`。但是我们可以使用`()`来提高优先级。

```mysql
select * from table where (列名=a or 列名=b) and 列名=c;
```



##### in操作符

我们可以使用`in`操作符来实现where的搜索集合确定。

```mysql
select * from table where (列名=a or 列名=b) and 列名 in (某个sql语句或者数据范围);
```

> `in`操作符比`or`运算的更快，所以有些时候我们可以用`in`来替代。



##### not操作符

用来否定之后的式子。

```mysql
select * from table where (列名=a or 列名=b) and 列名 not in (某个sql语句或者数据范围);
```



#### 用通配符进行过滤

如果要使用通配符，就需要使用`LIKE`关键词。

##### %通配符

`%`表示任何字符出现任何次数，类似正则中的*。

```mysql
select * from table where 列名 like "a%";
```

> 使用`like`往往需要用字符串。



##### _通配符

只匹配单个字符，不匹配多个字符。

```mysql
select * from table where 列名 like "_a%";
```

>通配符搜索的处理一般要比前面讨论的其他搜索所花时间更长。



