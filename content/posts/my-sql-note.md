---
title: "My Sql Note"
date: 2023-10-09T14:55:11+08:00
type: "posts"
---


## **数据库**

### 什么是数据库

按照一定结构组织,管理和存储数据的仓库

### 数据库的分类

- 层级数据库
- 网状型数据库(父节点不唯一)
- 关系型数据库(当前流行(大多数)

​			特点:由二维表格组成(有行有列)

### mysql数据库中一些组成关系 

​	MySQL,是一款关系型数据库的产品,安装mysql只是安装了mysql(数据库)的服务

​	MySQL数据库管理系统-->若干数据库-->若干库中的表-->若干表中的数据(尽量不要超过1亿条)

### SQL(结构化查询语言)

#### 简介

​	Structured Query Language(结构化查询语言),它是用于操作关系开数据库的一套语法,绝大多数关系 开数据库都支持.

​	方言:各个数据库厂商针对结构化查询语言的扩展,每个数据库厂商方言,只能自己数据库用

#### 组成部分

​	DDL:数据定义语言 Data Definition Language 建库建表,删除,修改数据库和表结构

​	DML:(重要)数据操作语言Data Manipulation Language 操作数据:插入,更新,删除,它是数据的操作

​	DQL:(最重要)数据查询语言Data Query Language 查询数据 (最难)

​	DCL:数据控制语言Data Control Language 和数据库用户,以及用户角色相关(运维和DBA数据库管理员)

​	CCL: 指针控制语言指针控制语言Cursor Control Language 在数据库中编写数据库代码时使用的,实际开发涉及很少(方言太多)

​	TPL:事务处理语言Transaction Processing Language  和事务处理相关的(TCL Transaction Control Language事务控制语言)

#### DDL

##### 作用

用于创建,修改,删除数据库

用于创建数据表,修改表结构,删除数据表

##### 分析

实际开发常用:创建数据库,创建数据表

##### 语法

- **连接数据库**:`mysql -u用户名 -p密码  ` 

- **创建库**:`create database[if not exists]  数据库名称`;   名称不能是中文,数字可以是名称,但不能作为第一个字符

  ​	创建一个名为gorilla的数据库:`create database gorilla`;

  ​	Character set:用于指定创建数据库的字符集.如果不设定,使用mysql安装时的字符集,UTF-8

  ​	Collate:用于指定创建数据库时的校对规则.每个字符集都有默认的校对规则

- **查看数据库**:`show databases`;

- 删除数据库(不常用)drop database [if  exists] 数据库名称; drop database gorilla;  (删除gorilla的数据库)

- 使用数据库:`use 数据库名称`

- 快速创建一个相同的表(创建的是表结构,内容不会复制)

  ```sql
  CREATE TABLE 表名 like 要复制的表;
  
  INSERT INTO 表名 SELECT * FROM 要复制的表;/*蠕虫复制,会复制数据*/
  ```

- **创建表**:

  	create table 表名(列名  数据类型 (有的数据类型要指定长度),列名  数据类型,列名  数据类型,列名  数据类型);

  列名之间用","格开  最后一个列名不用写","   

  decimal用在钱上

  varchar 可变长度字符

  char 固定长度字符

  bit 位类型(只存0和1)

​		**需要指定长度**:

​			char(10)  ['a','b','','','','','','','','']

​			varchar(10)  ['a','b']

![image-20210527101815491](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210527101815491.png)

```sql
	例子:
/*
varchar(长度 255) : 就是字符串 ,  varchar(10) 最大存10个字符,但是变长字符串根据实际存储内容改变, 例如: 添加的数据为 abc  长度就为3 
char(长度 255) :字符串  char(10) : 固定长度为10  例如 添加的数据为  abc 剩下7位空格补齐
decimal（m,d）:  m表示长度(整数+小数的长度) d表示小数位     decimal(11,2) : 9位整数  2位小数
*/
create table stu(
		id int,
		`name` varchar(20), 
    /*
    重音符 `关键字`  name是sql的关键字,所以要用``
    取消关键字
    */
		gender varchar(10),
		age int,
		birthday date,
		telephone varchar(20)
)
```

- 删除表

​	`drop table stu;`

##### 数据库文档

​	mysql.chm(13章)

​		创建表关键字:create

​		删除表关键字:drop

​		修改表关键字:alter

#### DML数据操作语言

##### 作用:

​		往表中插入数据 

​		修改表中的数据 

​		删除表中的数据

##### 插入:

###### 		语法:

`insert into 表名称(列名称,列名称...)values(列值,列值...) ;`    要求列名称与列值数量完全一致,一一对应

insert into 表名 values(值1,值2,值3);

> 缩写语法不能省略任何字段
>
> 值和字段的顺序类型,必须匹配
>
> 列类型是varchar或者char,values里面要用'包裹起来'或者"包裹起来";

###### 		批量插入:

```sql
insert into 表名称(列名称,列名称...这里可以省略部分字段)values(列值,列值...) ,
	values(列值,列值...) ,
	values(列值,列值...),
	values(列值,列值...) ;
```



##### 删除

###### 		语法:

​	delete form 表名称[where 条件];  不指定where条件,表中数据全部删除,不能删除列,只能删除行

###### 		常用条件:

拼接条件关键字where,在更新和查询操作时,涉及条件也是用这个关键字

**条件**有:

​					=           >             <             >=                  <=            != (<>)

| 语句                   | 含义                      |
| :--------------------- | :------------------------ |
| between  值1  and  值2 | 在值1和值2之间, 包头包尾  |
| in(值1,值2..值n)       | 在值1,值2...值 n 之中都可 |
| is null                | 为null                    |
| is not null            | 不为null                  |

**条件拼接**:

| 条件 | 含义                                     |
| ---- | ---------------------------------------- |
| and  | 条件1 与 条件2	(同时满足条件1与条件2) |
| or   | 条件1 或 条件2                           |
| not  | 非条件 1                                 |

```sql
delete  form student where age in (20,15,50);
/*删除age为20, 15 ,20 的用户*/
```

##### 摧毁表

`truncate table 表名称`

摧毁表**不能添加条件**

​			作用: 清除表中数据,并且重新构建表结构

###### truncate和delete的区别(面试题)

1. 条件删除

   > delete可以带where;
   >
   > truncate 不带,删除整个表

2. 事务回滚

   > delete是DML(数据操作语言),操作时原数据被放到rollback segment中,可以被回滚
   >
   > truncate是DDL(数据定义语言),不能被回滚
   >
   > > rollback segment 数据库中的一些存储空间,用来临时保存当数据库发生改变时候的先前值,两个目的:
   > >
   > > 1.用户想rollback到修改前的值,就要从rollback segment里面获取,当然只对transaction过程有效!一旦commit,rollback segment就会失.
   > >
   > > 2.并发session访问一个数据值改变但还没有提交的事务的表,select语句开始读取一个表的同时,一个事务也在修改,修改前的值就会存到rollback segment里,select语句也是从rollback segment里面读取表的值.

3. 清理速度

   > delete每删除一行,要保存事务日志,在数据量大的情况下,速度较慢
   >
   > truncate不能回滚,数据量大时相较delete速度快

4. 高水位重置

   > 不断进行表记录的DML操作,不断提高表的高水位线(HWM),delete操作后,虽然数据删除,但并没有降低表的高水位!DML操作数据库容量只会上升,不会下降!在用了delete后select的速度和delete前一样
   >
   > 而truncate后会重置高水位线,数据库容量也会被重置,之后再进行DML操作速度也会有所提升

truncate table 语法算DDL,删除表,重建表结构

delete from 算DML,只删除表中数据

##### 更新表	

`update 表名 set 列名 = 列值,列名 = 列值,列名 = 列值[where条件];`

没加条件,则更新所有数据		

#### DQL数据库查询语言

##### 作用:

​	用于查询数据

##### 语法:

​	基础    `select 列名1,列名2...from 表名 where 条件;`

​	查询全部列    `select * from 表名 where 条件;`

##### 去重操作:

​	关键字:distinct

​	用法:`select distinct 列名 from 表名 where 条件; `   (通常列名为1个)

##### 别名:

​	作用:给查询结果的列名起别名,原始列名不变,只是显示变了

​	用法:

- 标准 select 列名1  as 别名  ,列名2 as 别名,...from 表名 where 条件;

- 简写 select 列名1  别名 ,列名2 from 表名 where 条件;



##### like(模糊查询)

​	作用:模糊查询

​	特殊符号:%表示任意内容

​	写法:	like'固定内容%'   以固定内容开头

​				like'%固定内容'    以固定内容结尾

​				like'%固定内容%'  包含固定内容    

​    ex:  like'马%' (第一个字是马)     like'%马%'(有马字就可以)

​    特殊符号:_表示占位符,有一个就代表要有一个字符

​	ex:like'马__'(第一个字是马,且为三个字)

##### 排序查询

###### 	排序关键字:

​		order by 列名  asc(升序(默认))|desc(降序)

###### 	ex:

- selcect * from stu order by age desc;(查询年龄,以降序排序)

- selcect * from stu order  by age desc ,math asc(查询年龄与成绩,年龄降序排序,成绩升序排序)

##### 聚合查询

###### 		特点: 

把表中指定列内容聚合到一起

###### 		聚合函数:

| 函数        | 作用                                            |
| ----------- | ----------------------------------------------- |
| sum(列名)   | 求和                                            |
| max(列名)   | 最大                                            |
| min(列名)   | 最小                                            |
| avg(列名)   | 平均                                            |
| count(列名) | 记录总数	(为null的不参与计数,一般为id 或者*) |

count(里面可以不写列名) 可以是

```mysql
#输出表中Person表中email出现的次数
select count(0),email from Person
```

###### 		ex:

`select sum(math),count(math),sum(math)/count(*)from stu;`(查询数学成绩的总和,成绩的个数,平均数)

##### 分组查询:

###### 含义:

​	把具有相同特征的数据分成一组

###### 语法:

​	group by 列名 

​	分组后加条件:**having条件**(having可以使用衍生列,用于分组后的筛选,出现在groupby关键字后;where只能使用原始表中存在的列)

​	组合用法 :group by 列名 having 条件

###### 注意:

​	使用分组查询之后 ,查询的列名就不能再是*(全部列)了,只能是被分组的列以及聚合到函数.

###### ex:

按男女分组,查询男女各多少人

​	`select sex,count(*) from student group by sex;`

查询年龄大于25岁的人,按性别分组

​	`select sex,count(*) from student where age>25 group by sex;`

统计每组人数,查询年龄 大于25岁的人

​	`select age,count(*) from student where age>25 group by age;` 先过滤条件后分组

​	`select age,count(*) from student group by age having age>25;`先分组再过滤条件

##### 分页查询

###### 分页:

​	把数据分批次查询出来

###### 分页的思想:

​	换(时间换空间,空间换时间)

###### 分页的意义:

​	经过多次查询,节省内存空间

###### 实现思路:

​	固定值:每页显示的条数

​	总记录数

​	当前页

###### SQL语句分页:

​	关键字:limit   (mysql方言)

​	两个参数:

​		1.查询数据的开始记录索引,从0开始

​		2.每次查询多少条数据出来

​	语法格式:

​		limit开始记录数索引,每次查询的条数

​	ex:

​		查询学生表的数据,5条分页

​		select * from student limit 0,5;/*第一页*/

​		select * from student limit 5,5;/*第二页*/

​		select * from student limit 10,5;/*第三页*/

​		计算开始记录索引的公式:  **(当前页-1)*每页显示的条数**

​		计算总页数:  总条数 % 每页显示的条数 == 0 ? (总条数/每页显示的条数):(总条数/每页显示的条数+1)

​								如果总条数15,每页5条,分3页

​								如果总条数16,每页6条,分3页



### 数据库备份:

dump... (备份)   上面的是恢复

### 数据完整性

#### 作用:

​	保证数据的完整和准确,防止用户误输入,引起数据错误

​	实现的方式就是添加约束 

#### 约束的种类:四种

##### 	1.实体完整性

​			保证 表中第每条数据的唯一性

​			方式:	添加 主键

​			主键特点:唯一且非空

###### 			添加方式:

​				1.navicate右键Design Table ,id后点一下设置主键		

​				2.语法:

​						创建表时:

​						`create table 表名 (列名 数据类型 primary key,列名 数据类型,...);`   //只能给某一个列设为主键

​						`create table 表名 (列名1 数据类型,列名2 数据类型,列名 数据类型 primary key(列名1,列名2));`//可以在括号内写多个列名,用逗号分隔

​						修改表时:

​					`alter table 表名 add primary key (列名)`

​		主键自增长

​			自动增长要求:**必须整型**

​			使用方式:

​				`create table  表名(列名1 数据类型 primary key auto_increment,列名2 数据类型, 列名3 数据类型)`

​			**在设置了主键自增长之后,在插入数据时就无须关注主键列的值**

##### 	2.域完整性

​		保证每一列数据的准确性

​		a.数据类型的限定 int只能输数值,  date 只能输入日期

​		b.非空限定 在类型后加not null 不是空

​		c.唯一限定 在类型后加 unique 唯一 不一定非空

##### 	3.参照完整性 

​		多表的内容

##### 	4.自定义完整性(实际项目需求中添加)

----------------------------------

### 数据库的多表

#### 多表

​	用数据库中的表描述现实生活中实体间的关系

#### 表关系分类

​	一对多(多对一):一个学生只能属于一个班级,一个班级可以有多个学生

​	一对一:一个人一个身份证号;一个身份证号只能属于一个人

​	多对多: 一个老师可以教多个学生,一个学生可以被多个老师教

#### 实现多表的方式

##### 	一对多和多对一:

​			一为主表,多为从表

​			在**从表**中加入**外键**字段

​			外键:用于描述两个表之间的关系的**字段**,从表中有一列,该列的取值只能来源于主表的主键或者是null,外键一般叫主表名称_id

##### 	一对一:(不常见)

​			1.通过外键实现:从表中添加 外键字段,给外键字段添加 非空和唯一约束

​			2.通过主键实现:把主键字段同时设置为外键

> 如果字段不多,可以把两个实体放到一张表中

##### 	多对多:

​			通过第三张表

​			第三张表中包含两个外键字段,同时他们还是**联合主键**

#### 确定两张表关系的方式

##### 找外键

有外键的一定是从表,外键字段引用的一定是主表的主键.两张表的关系一定是:一对多或一对一的一种,如果外键字段上有非空和唯一两个约束,那就是一对一.否则是一对多.

多对多 一定是三张表.

#### Java代码描述多表

##### 一对多描述方式:

​	主表实体类(一的一方):包含一个从表实体的  集合  引用 private List<从表实体>名;并生成getset方法

​	从表实体类(多的一方):包含一个主表实体的  对象  引用private 主表实体 名;并生成getset方法

##### 一对一描述方式:

​	各自包含对方一个  对象  引用

##### 多对多描述方式:

​	各自包含对方一个  集合  引用

#### 数据库添加外键的语句(固定)

1. ##### 创建表时:

`constraint 约束名 foreign key(字段名) references 表名(字段名)`

```sq
create table user(
	id int primary key auto_increment,/*主表的id*/
	name varchar(100),......
) ;
```

```sql
create table orders(
	id int primary key auto_increment,/*从表自己的id*/
	order_num varchar(100),//下划线分割
	......
	user_id  int,/*外键id*/
	constraint FK_ORDERS_USERID foreign key(user_id) references user(id)
    //或者!!!下面这种方式
    foreign key (user_id) references user(id)
);
```

| constraint | FK_ORDERS_USERID   | foreign key  | (user_id)    | references   | user     | (id)     |
| ---------- | ------------------ | ------------ | ------------ | ------------ | -------- | -------- |
| 关键字     | 外键名称(自己命名) | 外键(关键字) | 外键字段名称 | 关联(关键字) | 主表名称 | 主表主键 |

外键名称库中唯一,不能重复,按照上方规范命名

2. ##### 修改表时:

参考mysql.chm

```sql
alter table 从表名字 add foreign key(外键字段名) references 一表名字(主键名字);
```

#### 多表操作(重要)

##### 增删改操作分析

###### 增:

​		主表:单表操作

​		从表:可以提供主表信息的选择

###### 改:

​		主表:单表操作

​		从表:提供主表信息的选择

###### 删

​		主表:

​				~有从表引用时:

​									从表数据有必要保留:	先把从表中引用的数据解除引用,然后再删除主表

​									从表数据没有必要保留:删除主表时,同时删除从表数据.先删从表,才能册主表,否则报错

![image-20210528145318362](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210528145318362.png)

​				~无从表引用时:单表操作

​		从表:单表操作

##### 多表查询(重点)

通过SQL语句实现,查询多张表的数据

###### 交叉连接:

​	关键字: cross join

​	用法:`select * from 表1 cross join 表2;` (显式) 

​			`select*from表1,表2;`(隐式)

​	返回结果:两张表的笛卡尔积 

###### 内连接

​	关键字:inner join

​	用法: `select * from 表1 inner join 表2 on 条件; `(显式)

​			 `select * from表1,表2 where 条件;`(隐式)

​	作用:在交叉连接基础之上,进行条件过滤,使结果正确

###### 左外连接

​	关键字:left outer join  (outer可以省略)

​	用法 : select * from 表1 left outer join 表2(显式) on 条件;       (没有隐式,如果是隐式就和交叉连接无法区分)

​	作用:返回两张表符合条件的数据之外 ,再返回左表的**所有**数据

​			左表:写在left outer join 关键字左边的表

###### 右外连接

​	关键字:right outer join  (outer可以省略)

​	用法 : select * from 表1 right outer join 表2(显式) on 条件;       (没有隐式,如果是隐式就和交叉连接无法区分)

​	作用:返回两张表符合条件的数据之外 ,再返回右表的**所有**数据

​			右表:写在right outer join 关键字右边的表

###### 子查询

​	特点 :在一条查询语句中,又包含了另一条查询语句

​	标识:外层查询语句(内层查询语句)    先执行小括号的,再执行外面的

![image-20210528160825555](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210528160825555.png)



![image-20210528161915615](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210528161915615.png)

自我一对多:

​	在一张表中体现出一对多的效果

​	ex:

​		员工和上级都会放到员工表中;子部门和父部分 都会放到部门表中;子菜单和父菜单都是菜单

​	在一张表中体现数据的父子关系(希望其是子菜单看外键(多个),希望其是父菜单看主键)

> 左外连接和右外连接的交集是内连接

### 数据库事务(重要,不能卡壳)

#### 什么事务

指逻辑上的一组操作,要么全成功,要么全失败

#### 事务的特性

Atomicity原子性

​	组成事务的各个单元为最小单位,不可再拆分

Consistency一致性

​	执行事务之前和之后的数据保持一致的状态(例:ab账户2000,转前2000,转账后总额也为2000)

Isolcation隔离性

​	多个事务并发访问时,互相不能被打扰

Durability持久性

​	当事务执行成功之后,是永久性的保存,不会受其他因素影响

#### 事务的问题(不考虑隔离性的多线程访问问题)   全是错误的!

##### 脏读

一个事务读到了另一个事务**未提交**的数据

##### 	不可重复读

一个事务读到了另一个事务**已经提交**的update数据(因为被打扰,不满足隔离性)

##### 	幻(虚)读

一个事务读到了另一个事务**已经提交**的insert或delete数据

#### 事务的隔离级别

##### 		读未提交read uncommitted;(一般用大写)

​		以上三和错误情况都防不住.100%会发生以上三种错误情况

##### 		读已提交read committed;(一般用大写)

​		可以防住脏读,100%会发生不可重复读和幻读

##### 		可重复读repeatable read;(一般用大写) (mysql默认隔离级别)

​		可以防住脏读和不可重复读,幻读有几率会发生

##### 		串行化(序列化)serializable;(一般用大写)

​		以上三种错误情况都能防住

##### 		设置隔离级别的方式:

​		set session transaction isolation level 隔离级别

​			(会话级:当前黑窗口有效,关闭后再打开就失效)

​	mysql的默认隔离级别是 repeatable read

​	*隔离级别越高,执行效率越低*

#### 事务的操作

​	在Mysql数据库中,默认每条语句独立一个事务,执行完语句后,事务自动提交

​	手动控制开启事务,从而达到多条语句处于一个事务中

##### 	mysql设置手动开启事务

start transaction;

手动提交:commit;

手动回滚:rollback;

##### 	结束标识

提交或回滚(提交或回滚后需要再次开启事务)

##### 回滚点

* savepoint 回滚点名字;
* rollback to 回滚点名字;

### 索引

index 添加在数据表中某列或者某些列的检索规则

#### 使用索引:

​	优势: 详情戳([索引优化](#索引优化))

1. **提高查询效率**,类似大学图书馆的书目索引

2. 通过索引列队数据进行排序,降低数据排序的成本,**加快排序速度**

   >最快的排序就是不用排序，也就是**利用索引排序**。把对顺序的维护**分散到**每一次增删改的过程中，而不是查询时再匆忙从零开始排序。
   >
   >> 班级按照身高排后,按成绩排,出班级时候也是身高排,成绩排,已经把排序在维护索引时做了

3. **加快分组速度**

​	弊端:

		1. 索引也是一张表,保存了主键与索引字段,并指向实体表的记录,所以索引列也要占用空间;
		2. 由于增删改同时也需要维护索引,所以会造成增删改的效率降低
		
		字典新增几百个字，需要额外编排目录，要多占几页纸

​	结论:在适当的位置添加索引

#### 添加索引的原则:

1. 经常用于**查询**条件的字段,建议添加索引 
2. 经常用于**排序**字段,建议添加索引
3. 经常需要**连接**的列上建索引,加快连接速度(外键上加索引等)
4. 辨识度大于70%以上的才建议使用索引,建立在区分度高的字段上

#### 索引的分类:

##### 	主键索引:

加速查询 + 列值唯一（不可以有null）+ 表中只有一个 , 主键约束  **primary key**

添加主键,自带主键索引,一张表只能有一个主键索引

##### 	唯一索引:

加速查询 + 列值唯一（可以有null） 唯一约束  **unique**

添加唯一约束,自带唯一索引,添加了唯一索引,也就说明该字段具备唯一约束.

##### 	普通索引:

添加**index**来实现让字段有索引,加速查询

##### 	组合索引:

多列值组成一个索引 index

组成索引的字段是组合的

#### 索引的创建方式:(语法)

##### 直接创建（普通、唯一）

```sql
-- 创建普通索引
create index 索引名 on 表名(列名);
-- 创建唯一索引
create unique index 索引名 on 表名(列名);
-- 创建普通组合索引
create index 索引名 on 表名(列名1,列名2....);
-- 创建唯一组合索引
create unique index 索引名 on 表名(列名1,列名2...);
```

##### 修改表时指定

```java
-- 添加一个主键，这意味着索引值必须是唯一的，且不能为NULL
alter table 表名 add primary key(id);
-- 添加唯一索引（除了NULL外，NULL可能会出现多次）
alter table 表名 add unique(列名); -- 索引名就是列名
-- 添加普通索引，索引值可以出现多次。
alter table 表名 add index(列名)；-- 索引名就是列名
```

##### 创建表时指定【掌握】

```sql
create table student(
 id int,
 username varchar(32),
 age int,
 primary key(id), -- 主键
 unique(username), -- 唯一
 index(age) -- 普通
);
```

##### 	索引失效:

​		1.模糊匹配时,如果条件没有一定辨识度,那么索引就会失效(%写在模糊匹配字符的前面也会失效)

​		2.当多条件查询时,如果使用or关键字,只要有一个条件没有索引就会失效,必须每个条件都有索引

​		3.当查询条件字段有计算时,索引会失效

​		4.当条件有<>,is null , is not null ,!= 这些条件时,索引失效

​		5.字符串不加引号,会导致mysql隐式转换,导致索引失效

#### 索引的原理:

原理请[戳](#数据结构与数据库结合相关知识)

**索引是一种数据结构，**用于高效搜索目标数据，在MySQL中具体实现为B+树（InnoDB引擎,mysql5.6后默认引擎）

​	**索引 = 排序后的数据结构**

​	mysql的索引使用的是B+Tree

![image-20210530120608324](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210530120608324.png)

### 函数

​	函数就是方法

​	之前写的sql语句都是一条,当多条sql语句完成同一个功能时,我们称为数据库函数,或者数据库存储过程.

​	根据功能的不同,区分是函数还是存储过程

#### 	函数特点:

​		通用,不同业务领域都可以使用

#### 	存储过程的特点:

​		不通用,用于特定业务领域.代码根据不同需求场景改变.

##### 细节:

​	函数和存储过程都采用了大量的数据库方言,移植性差,不通用(互联网领域用的少)

##### 说明:

​	数据库中没有内置的存储过程,但有内置函数,在实际开发中可能会用到内置的函数

#### Mysql数据库函数分类:

​	字符串函数,日期函数,数学函数,高级函数(if() ifnull()  case  when  then  and)'

##### 字符串函数

```
1.  函数：CONCAT(s1,s2...sn)
	描述：字符串 s1,s2 等多个字符串合并为一个字符串
	实例：SELECT CONCAT("a", "b", "c", "d");
2.  函数：CHAR_LENGTH(str)
	描述：返回字符串 str 的字符数
	实例：SELECT CHAR_LENGTH("abcd");
3.  函数：LENGTH(str)  返回字符串 s 的字节数
	描述：中文在utf8占3个字节,在gbk下占2个字节
	实例：SELECT LENGTH("abcd") ;
4.  函数：UCASE(s) | UPPER(s)
	描述：将字符串转换为大写
	实例：SELECT UCASE("itcast");
5.  函数：LCASE(s) | LOWER(s)
	描述：将字符串转换为小写
	实例：SELECT LCASE("ITCAST");
6.  函数：LOCATE(s1,s)
	描述：从字符串 s 中获取 s1 的开始位置(从1开始)
	实例：SELECT LOCATE('he','abcd');
7.  函数：TRIM(str) | LTRIM(str) | RTRIM(str)
	描述：字符串去空格
	实例：SELECT TRIM("  abcd");
8.  函数：REPLACE(s,s1,s2)
	描述：将字符串 s2 替代字符串 s 中的字符串 s1
	实例：SELECT REPLACE('abc','a','x');
9.  函数：SUBSTR(s, start, length)
	描述：从字符串 s 的 start 位置截取长度为 length 的子字符串
	实例：SELECT SUBSTR("itcast", 2, 3);
10. 函数：STRCMP(str1,str2)
	描述：比较字符串大小,左大于右时返回1，	
	左等于右时返回0，左小于于右时返回-1，
	实例：SELECT STRCMP("a","b");
	
	
	-- 字符串转为 日期
	select cast('1999-1-1' as date);
	-- 字符串转为 整型
	select cast('19' as SIGNED);

```

##### 日期函数

```
1.  函数：NOW() | CURDATE() | CURTIME()
	描述：获取系统当前日期时间、日期、时间
	实例：SELECT NOW();
2.  函数：YEAR(DATE) | MONTH(DATE) | DAY(DATE)
	描述：从日期中选择出年、月、日
	实例：SELECT YEAR(NOW()); 
3.  函数：LAST_DAY(DATE)
	描述：返回月份的最后一天
	实例：SELECT LAST_DAY(NOW());
4.  函数：ADDDATE(DATE,n) | SUBDATE(DATE,n)
	描述：计算起始日期 DATE 加（减） n 天的日期
	实例：SELECT ADDDATE(NOW(),10);
5.  函数：QUARTER(DATE)
	描述：返回日期 DATE 是第几季节，返回 1 到 4
	实例：SELECT QUARTER(NOW());
6.  函数：DATEDIFF(d1,d2)
	描述：计算日期 d1->d2 之间相隔的天数
	实例：SELECT DATEDIFF('2019-08-01','2019-07-01');
7.  函数：DATE_FORMAT(d,f)
	描述：按表达式 f的要求显示日期 d
	实例：SELECT DATE_FORMAT(NOW(),'%Y-%m-%d');

```

##### 数学函数

```
1.  函数：ABS(x)
	描述：返回 x 的绝对值　　
	实例：SELECT ABS(-1);
2.  函数：CEIL(x) | FLOOR(x)
	描述：向上（下）取整
	实例：SELECT CEIL(1.5);
3.  函数：MOD(x,y)
	描述：返回x mod y的结果，取余
	实例：SELECT MOD(5,4);
4.  函数：RAND()
	描述：返回 0 到 1 的随机数
	实例：SELECT RAND();
5.  函数：ROUND(x)  round(x,y)  保留x的y位小数且四舍五入
	描述：四舍五入
	实例：SELECT ROUND(1.23456);
6.  函数：TRUNCATE(x,y) 舍尾法
	描述：返回数值 x 保留到小数点后 y 位的值
	实例：SELECT TRUNCATE(1.23456,3);
```

##### Mysql高级函数

```
在查询代码的过程中，可能我们需要对查询的结果进行判断。就需要使用到case when then
-- 语法 
SELECT 
    CASE [字段,值] 
           WHEN   判断条件1    THEN    希望的到的值1
           WHEN   判断条件2    THEN    希望的到的值2
           ELSE 前面条件都没有满足情况下得到的值 
      END
FROM  table_name;
```

### 数据结构与数据库结合相关知识

#### 二叉查找树

![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1589358394072-a5d5f642-25e5-4d01-ba52-ba7251609d4a.png)

> **在一颗树中找到目标数据所需的比较次数 = 目标数据所在的层级**
>
> 但是按顺序输入时,它会变成一个线性结构,因为右边的节点总是比根节点大,如果是12345,根节点是1,他的右叶子结点是2,2的右叶子结点是3 ....

#### 二叉平衡树

可以自动调整节点，好让“树的层级”不至于太深

从小到大一直依次放入数据,会不停"左旋",保证层级

[动画演示](https://www.cs.usfca.edu/~galles/visualization/AVLtree.html)

保证查找速度的前提是**数据必须全部在内存中**,显然对于大量数据是不明智的,要涉及磁盘-内存的io操作

> 本身数据是存在磁盘中,需要使用时从磁盘载入内存,其中就设计磁盘内存的io操作
>
> >  磁盘是原料仓库,内存是生产车间,cpu是生产工人

可以这样组织数据库:

- 把数据存在磁盘中
- 数据按树结构组织

- 查询时分块读取数据并比较，持续进行磁盘IO读取节点，直到找到目标数据(但是频繁进行磁盘IO(磁盘IO耗时),效率低下)

> 总结影响磁盘IO的关键就是**树的层级(深度)**,数据在树的第二层,只需比较到数据直接返回,不用磁盘io,但如果数据在32层,就要进行32次磁盘io

![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1589361951577-71cfd4b1-bfd8-44cc-a968-5de2d821fe2b.png)

#### B树

![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1589362752238-4daa5676-b2c3-419f-9016-a3cf84bb057f.png)

B树有个“阶”的概念，比如“三叉平衡”的B树其实叫“3阶B树”

“N阶B树”**每个节点可以存N-1个数据**（二叉平衡树每个节点只存1个数据），且每个节点至多可以连接N个子节点。

**每次加载一个节点时都可以从磁盘带出更多条数据，从而减少磁盘IO的次数**,

典型**空间换时间**

##### 问题

如何通过B树组织数据库的表数据

![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1589370021227-2ce244b2-e5de-4aeb-8a57-5cfb81109d80.png)

#### 

MySQL有"**页**"的概念,大小是16k,可以理解MySQL中的"页"就是B树上的一个个节点.

假设表中的一行数据大概占1kb(很大了,一般15-20字节),因为节点最大size是16K,所以每个节点最多只能存16行数据.

[二叉树](#二叉平衡树)-->[B树](#B树) //因为B树在一个节点能存更多数据,但显然比二叉树多15行数据也没有多大的改变,目的是要让一个节点**尽可能塞入更多数据以减少树的深度**

#### B+树(mysql索引实现方式)

把每一行数据的主键存进去

所谓B+树**把原先B树中分散在各个节点的数据都“赶到”最底层的叶子节点，非叶子节点只存储主键-addr形式的数据**

单节点存16KB,一个主键8字节(bigint按最大算)+6字节(指针addr)=14字节

16*1024/14=1170个(主键和指针)

**单节点存的数据数量增多,对应树的深度减少,磁盘和内存的io次数也就减少,大大提高了效率**

##### ![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1589448311743-b494fa66-02d0-43b8-ab9f-f920caec8af2.png)

##### B+树特征

* 非叶子节点不存数据
* 叶子节点数据用链表相连(叶子节点是有序链表,非常便于做范围查询)

##### 问题

B+树如何获取数据?

之前B数的节点存了完整的数据,找到第二次某节点可以直接返回,但是B+树只存了主键与addr,就算匹配到了,还得继续深入找到叶子节点读取完整的数据.

![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1589374780467-c8dbebf7-eb2e-418b-8536-cc90aea27b37.png)

B树和B+树的区别

- B树的节点都会存储整行数据，占用空间大存储addr少，而B+树的非叶子节点只存储主键，能容纳更多addr

- 由于非叶子节点能容纳更多addr，那么同一个节点能指向更多下级节点，所以相同数据量时，B+树通常更加“矮”，IO更少

- B树的查询效率是不稳定的，最好情况是根节点，最差情况是叶子节点，而B+树是稳定的，每次都要查询到叶子节点

  > 相同数据量的情况下，B+树远远矮于B树。比如B树的查询IO次数是1~100，而B+树恒定为3,优选B+树!

- B+树的叶子节点是有序列表，非常便于范围查询



### SQL优化

金句:SQL优化本质是减少磁盘io次数

#### 数据三范式

##### 第一范式  原子性 

所有列不可再分

> 不能把"电话号,邮箱,QQ"存到一个contact列里,应该拆分成phone,email,qqnumber三个列

##### 第二范式 唯一性

必须存在业务主键,非主键字段应依赖于**全部**业务主键(可能存在复合主键)

每张表最好都设定主键,并且最好和业务无关,比如自增id

在一个数据库表中，**一个表中只能说一件事**，不可以把多种数据保存在同一张数据库表中(不能把订单编号和商品编号作为联合主键来设计订单信息表,应该分开来订单对应订单编号,商品各种信息对应商品编号,新创建一个订单项目表来存订单编号和商品编号)

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/2012040114063976.png)

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/2012040114082156.png)

##### 第三范式 直接性

确保每列都和主键列直接相关,而不能间接相关

订单数据表,把客户id作为外键和订单表建立相应关系,而不可以把客户的其他信息(姓名,联系方式等)

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/2012040114105477.png)

##### 总结

一般前两个范式都会遵守,第三个范式会被打破,**越遵从范式化设计，表的拆分越细致，查询时需要关联的表就越多**.

#### 优化sql1:反三范式

例如

1. 数量*单价 可以得到金额 ,但是单独添加金额这个冗余的字段,可以减少计算提高查询统计的速度

> 降低范式就是增加字段，允许冗余，达到以空间换时间的目的

2. 订单表里存商品价格,而价格应该放在商品表中,这样做的目的是当商品打折或者降价时,订单表的商品价格不应该随着商品表的价格改变而改变,应该维持订单价格,看似违反第三范式,其实这样做就不用存储某一时间段商品的价格了.

#### 优化sql2:数据类型选择

##### 1.业务允许,尽量使用unsigned

| **数据类型** | **占据空间** | **范围（有符号）** | **范围（无符号）**       | **描述**   |
| ------------ | ------------ | ------------------ | ------------------------ | ---------- |
| tinyint      | 1 个字节     | -2^7 ~ 2^7-1       | 0 - 255                  | 小整数值   |
| smallint     | 2 个字节     | -2^15 ~ 2^15-1     | 0 - 65535                | 大整数值   |
| mediumint    | 3 个字节     | -2^23 ~ 2^23-1     | 0 - 16777215             | 大整数值   |
| int          | 4 个字节     | -2^31 ~ 2^31-1     | 0 - 4294967295           | 大整数值   |
| bigint       | 8 个字节     | -2^63 ~ 2^63-1     | 0 - 18446744073709551615 | 极大整数值 |

所谓unsigned就是无符号,tinyint正常取值是-128-127,但是业务很少用到负数,就可以用unsigned让正向范围翻倍 0-255;

##### 2.varchar和char的比较

* char长度固定,不考虑边界,**读写效率高于varchar**,适合存储长度固定,频繁读写的数据,如身份证号,手机号

* varchar 长度不固定,可以通过varchar(10)指定上限,适合存储长度波动,更新不频繁的数据,如自我介绍等

  > char的存储长度不够灵活,但是varchar需要用1-2个字节来存储当前值的实际长度,更新会导致重新计算
  >
  > 金句: 没有最完美的类型,只有最适合的类型

##### 3.小数类型

| **数据类型** | **占据空间**                         | **是够精确** |
| ------------ | ------------------------------------ | ------------ |
| float        | 4个字节                              | 非精确       |
| double       | 8 个字节                             | 非精确       |
| decimal      | 每4个字节存9个数字，小数点占一个字节 | 精确         |

decimal效率不如float和double

##### 4.时间类型

| **数据类型** | **占据空间** | **取值范围**                              |
| ------------ | ------------ | ----------------------------------------- |
| date         | 3个字节      | 1000-01-01 ~ 9999-12-31                   |
| time         | 3~6个字节    | -838:59:59 ~ 838:59:59                    |
| datetime     | 5~8个字节    | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59 |
| timestamp    | 4~7个字节    | 1970-01-01 00:00:01 ~ 2038-01-19 03:14:07 |

具体跟着公司走,可能还会用Long存秒数

##### 5.尽量避免null

使用NOT NULL,除非业务要求可能是null

> 1、null长度不为0，占用空间
>
> 2、null参与计算，且运算时返回值为null，null与任何值比较返回值为null
>
> 3、null对索引有影响
>
> 4、null值参与排序（asc null排在最后，desc null排在最前）

NOT NULL：一定要传递值，且不能为NULL，否则报错

DEFAULT 'xx'：传不传都可以，不传就使用	值xx，**可以传NULL**

NOT NULL DEFAULT 'xx'：传不传都可以，不传就使用默认值xx，**不能传NULL**

#### 语句书写顺序

SELECT ... FROM table WHERE ... GROUP BY ... HAVING ... ORDER BY ... LIMIT ...

> 除了SELECT，后面几个顺序可以记忆为：温哥华OL，意思是温哥华白领。

#### 索引优化

> 聚簇索引:主键索引,**叶子节点是表的数据,不需要回表**,也是,存的是索引+数据
>
> ![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1589586538060-89e3781c-22ed-4b16-a734-d5466af2d6aa.png)
>
> 非聚簇索引:辅助索引(唯一索引,普通索引),叶子节点是主键,必要时要根据主键回表查询,**回表会增加磁盘io次数**,除了主键索引其他都是辅助索引
>
> ![图片.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1608956710604-75cdbc20-a59e-473f-9e98-b963ac3cd49d.png)

----



> 需要根据username搜索订单,且要得到用户的年龄,如果给username添加一个普通索引后,叶子节点存储id,非叶子节点是username,想要得到user_age,还需要在回表去找对应的user_age.
>
> ![图片.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1610162572296-97295d9c-ab7c-444b-a99a-6c8ea8f106eb.png)

在业务中用**索引覆盖**,避免回表

> 给user_name和user_age加一个组合索引,**当辅助索引上的字段完全满足本次查询的列时，就是所谓的索引覆盖**,也正是避免用select* from 会提高效率__(不用*提高索引覆盖的几率)

理解:在非主键索引上(加一个联合索引)查询就可以得到所需要的所有字段，不需要回表(减少了磁盘io操作)，即为**索引覆盖**,在查询时不用select * 提高索引覆盖的几率,自然效率会上升

> 使用**Explain**分析是否 走索引,加在select前

##### 最左匹配原则

###### where

为什么对于组合索引index(name, age)，条件WHERE age=25无法利用索引

第一个要比较是name不是age,不满足最左匹配原则

> 教室是按身高和成绩排,非要用成绩去找,在教室里面是查不到的,因为前提是身高!
>
> 数据库直接语法匹配,不满足最左匹配,直接放弃索引走全盘扫描

###### order by

在排序时字段顺序不一致,`ORDER BY age, name（字段顺序不一致`

字段排序方式不同步，DESC和ASC混着来`ORDER BY name DESC, age ASC`

都会导致不走索引,重排序

##### 联合索引使用场景

* **多条件查询**，提高利用率 `where a,b,c  就可以 index(a,b,c)` where不管顺序,但不能缺少字段

* **避免回表**,但实际回表还是会多一些

* **经常要排序**,添加联合索引,索引在维护时候已经是排序过的,根据索引去排序会快

* **count统计**

  `SELECT COUNT(*) FROM t_user_follow WHERE uid=123 and platform=6 and follow_status=1;`

  此时最好建立联合索引index(uid, platform, follow_status)，速度会稍微快一些。

* 

### JDBC

#### JDBC基本概述

​	JDBC：Java DataBase Connectivty
​	它是sun公司推出的一套java程序操作数据库的规范。是JavaEE规范的其中之一。

#### JDBC快速入门（练）

```java
步骤：
	1.注册驱动
	2.获取连接
	3.获取操作对象
	4.执行SQL语句，获取结果
	5.处理结果（增删改得到的是，影响数据库的行数。查询的得到的是，结果集）
	6.释放资源（释放顺序是：后获取的先关闭）
	
	public class JdbcInsert {
    public static void main(String[] args) throws SQLException {
        String sql = "insert into tbl_user(username,password,money) values('jack','1234',1000);";//DML
        //1.注册驱动
        DriverManager.registerDriver(new Driver());
        //2.获得连接
        String url = "jdbc:mysql://localhost:3306/day15";
        ///////////// 协议  子协议  主机      端口   数据
        String username = "root";
        String password = "root";
        Connection connection = DriverManager.getConnection(url, username, password);
        //3.获得语句执行者对象(帮我们执行sql语句的)
        Statement statement = connection.createStatement();
        //4.执行sql
        int count = statement.executeUpdate(sql);
        //5.处理返回结果
        System.out.println("影响记录行数为:"+count);
        //6.释放资源
        statement.close();
        connection.close();
    }
}

------------------------------------------------------
    public class JdbcSelect {
    public static void main(String[] args) throws SQLException {
        String sql = "select * from tbl_user;";//DQL
        //1.注册驱动
        DriverManager.registerDriver(new Driver());
        //2.获得连接
        String url = "jdbc:mysql://localhost:3306/day15";
        ///////////// 协议  子协议  主机      端口   数据
        String username = "root";
        String password = "root";
        Connection connection = DriverManager.getConnection(url, username, password);
        //3.获得语句执行者对象(帮我们执行sql语句的)
        Statement statement = connection.createStatement();
        //4.执行sql,获取结果集
        ResultSet resultSet = statement.executeQuery(sql);
        //5.输出结果
        while(resultSet.next()){
            Integer id = resultSet.getInt(1);
            String un = resultSet.getString(2);
            String pw = resultSet.getString(3);
            Integer mn = resultSet.getInt(4);
            Integer id2 = resultSet.getInt("id");
            String un2 = resultSet.getString("username");
            String pw2 = resultSet.getString("password");
            Integer mn2 = resultSet.getInt("money");
            System.out.println(id+","+un+","+pw+","+mn);
            System.out.println(id2+","+un2+","+pw2+","+mn2);
        }

        //6.释放资源
        resultSet.close();
        statement.close();
        connection.close();
    }
}
```

#### 常用对象说明

他们都是JDK API中的类和接口。在java.sql包下

##### DriverManager：

	DriverManager：
		它是jdbc的管理对象，也是工具类。它里面的方法都是静态的。
		常用方法：
			注册驱动：registerDriver(java.sql.Driver接口的实现类);
			获取连接：
				getConnection(String url);
				getConnection(String url,Properties properties);
				getConnection(String url,String username,String password);
	
				通常情况下，我们会采用第三种方式，因为看的清楚。
				url：连接数据库的url
				     格式是固定的
					    jdbc:mysql://主机:端口/数据库名称?参数名=参数值&参数名=参数值....
				     通常的写法：
					    jdbc:mysql://localhost:3306/db_name
				
				如果我们使用了参数，建议大家使用第二种方式。参数的内容可以不拼接在url后面，而是写在properties里面
				
				username：指的就是数据库用户名
				passwrod：指的就是数据库密码
				他们两个用properties的方式时，也是写在properties里的
	
				连接的参数有：
					在mysql.chm的26章节中，有一个Connector/J

##### Connection：

```
Connection：
		它是jdbc操作的核心对象。它可以获取操作数据库的对象。操作事务也由它来完成的。
		常用方法：
			获取操作对象：Statement st = createStatement();
			获取预处理对象：PreparedStatement pstm =  prepareStatement(String sql);
			设置事务的手动控制：setAutoCommit(boolean isAuto);
			提交事务：commit();
			回滚事务：rollback();
			释放资源：close()；
```

##### Statement:

```
Statement：
		它是jdbc的操作对象。它可以执行DML和DQL，以及没有任何结果返回的DDL语句。
		常用方法：
			用于执行增删改方法的：int res = executeUpdate(String sql);
			用于执行查询方法的：ResultSet rs = executeQuery(String sql);
			它可以执行DML和DQL：boolean bl = execute(String sql);
					   返回值含义：
						是否有结果集。
						true：有结果集，执行的是DQL语句
						false：没有结果集，执行的是DML语句
			释放资源：close()；
```

##### ResultSet：

```
ResultSet：
		它是jdbc的结果集对象。就是把返回结果封装成了一个内存对象，对象的形式就是一个二维表格。
		我们要想获取数据，就需要遍历这个结果集对象。
		常用方法：
			让结果集游标向下移动一个：next();
			让结果集游标向上移动一个：previous();
			获取指定列的数据内容：getXXX(int columnIndex | String columnLabel);
					     XXX的含义就是数据类型。例如：getString(),getInt(),getObject()等等。
					     columnIndex：它是从1开始的。
			释放资源：close()；
```

### SQL注入的解决

PreparedStatement 预编译对象能解决PreparedStatement

### 数据库连接池

DBCP  C3P0 淘汰, 历史产品

HikariCP:日本产品(快)  作为了流行技术springboot默认数据源(DataSource)

Druid (德鲁伊):阿里产品, 提供其他组件多 稳定  速度有一定的保证

### Mybatis框架

具有通用性的繁琐代码封装起来,让使用者不用关心封装起来的细节,提高开发效率,降低开发难度,统一开发标准,这个被封装起来的代码,就是框架,半成品软件,可以根据这个软件继续进行开发,完成个性化需求

#### 主要思想

将程序中的大量 SQL 语句剥离出来，使用 XML 文件或注解的方式实现 SQL 的灵活配置，将 SQL 语句与程序代码分离，在不修改程序代码的情况下，直接在配置文件中修改 SQL 语句。

#### ORM映射()

![image-20210502084141324](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210502084141324.png)

#### 前期准备

1. 导入jar包 mybatis 的jar包和数据库的驱动包

2. 导入数据库

3. 编写实体类

4. 编写持久层接口(接口可以用来动态代理实现CRUD)

   > 动态代理是所有框架的底层
   >
   > ```java
   > sqlSession.getMapper(UserMapper.class); //或者userdao 给接口生成映射的代理对象-实现类
   > ```

#### 编码步骤

1. 编写核心配置文件SqlMapConfig.xml

   > 核心配置文件主要用于配置数据库连接和 MyBatis 运行时所需的各种特性,文件中配置了数据库环境和映射文件的位置

2. 映射文件UserDao.xml / WebsiteMapper.xml(在这里面配置sql标签之类的)

   > ![image-20211008155235682](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211008155235682.png)

##### 	复杂查询等

![image-20211008165044158](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211008165044158.png)

##### 	模糊查询

![image-20211008165253428](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211008165253428.png)



#### 动态SQL![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1001990-20180420091927414-873899959.png)

1.  `<if>`

```xml
<select id="findByCondition" parameterType="com.gorilla.domain.QueryPojo" resultType="com.gorilla.domain.User">
        select * from tbl_user where 1=1
        <if test="username!=null and username!=''">
            and username=#{username}
        </if>
        <if test="id!=null and id !=0">
            and id >#{id}
        </if>
</select>
```

2. `<where>`

一般where和if 一起使用,动态拼接查询条件

3. `<set>`

匹配条件后才会对这个字段进行更新

```xml
 <update id="updateUser" parameterType="com.gorilla.domain.User">
        update  tbl_user
            <set>
                <if test="username!=null and username!=''">
                    username=#{username},
                </if>
            </set>
     where id=#{id}
</update>
```

4.`<foreach>`

集合对象进行遍历,然后把每一项的内容 作为参数传到 sql 语句中

5.`<include>`

抽取重复公共代码

#### 延迟加载																																						策略

按需加载,需要用到数据的时候才进行加载,不需要用到就不加载(懒加载)

#### 替换为注解开发

```java
/*
dao层写    
property 实体类   cloumn 数据库的列 
one 一对一 many 一对多
*/
//查询所有的账户
    @Select("select * from tbl_account ")
    @Results(value={
            @Result(property = "id" , column = "id" , id = true),
            @Result(property = "username" , column = "username" ),
            @Result(property = "password" , column = "password" ),
            @Result(property = "gender" , column = "gender" ),
            @Result(property = "email" , column = "email" ),
            @Result(property = "telephone" , column = "telephone" ),
            @Result(property = "user" ,javaType = User.class ,
            one = @One(select = "com.gorilla.dao.UserDao.findUserByUid") ,column = "uid"
    )

    })
    List<Account> findAll();
```



### 

### NoSQL

NoSQL = Not Only SQL)，意即“不仅仅是SQL”，是一项全新的数据库理念，泛指非关系型的数据库。

#### 发展

互联网2.0兴起,非关系型数据库成了一个非常热门的新领域; 传统关系型数据库在超大规模高并发的SNS类型的web2.0纯动态网站已经力不从心.

##### 难题

> 1.**High performance对数据库高并发读写的需求**,web2.0,网站要根据用户个性化信息来实时生成动态页面和提供动态信息,往往要达到每秒上万次读写请求,上万次SQL**写**数据硬盘IO就无法承受,如:点击次数,投票计数
>
> 2.**Huge Storage 对海量数据的高效率存储和访问的需求**,类似Facebook,twitter这样的SNS(Social Network Service，社交网络服务)网站,每天海量数据动态,要求高效率存储访问
>
> 3.**High Scalability && High Availability- 对数据库的高可扩展性和高可用性的需求** 
>
> 当用户量和访问量与日俱增,数据库没办法像web server 和 app server 一样通过添加硬件和服务节点来扩展性能,并且要不间断24小时工作,要扩展往往要停机维护和数据迁移. 

####  键值(Key-Value)存储数据库

> 相关产品： Tokyo Cabinet/Tyrant、Redis、Voldemort、Berkeley DB
>
> 典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
>
> 数据模型： 一系列键值对
>
> 优势： 快速查询
>
> 劣势： 存储的数据缺少结构化

#### 列存储数据库

> 相关产品：Cassandra, HBase, Riak
>
> 典型应用：分布式的文件系统
>
> 数据模型：以列簇式存储，将同一列数据存在一起
>
> 优势：查找速度快，可扩展性强，更容易进行分布式扩展
>
> 劣势：功能相对局限

#### 文档型数据库

> 相关产品：CouchDB、MongoDB
>
> 典型应用：Web应用（与Key-Value类似，Value是结构化的）
>
> 数据模型： 一系列键值对
>
> 优势：数据结构要求不严格
>
> 劣势： 查询性能不高，而且缺乏统一的查询语法

#### 图形(Graph)数据库

> 相关数据库：Neo4J、InfoGrid、Infinite Graph
>
> 典型应用：社交网络
>
> 数据模型：图结构
>
> 优势：利用图结构相关算法。
>
> 劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。

#### NoSQL特点

1.易扩展

> 去掉关系特性,数据间无关系,在架构层面带来可扩展的能力

2.大数据量下高性能

> 无关系性,数据库的结构简单

3.数据模型灵活

> 无需事先为要存储的数据建立字段,存储自定义的数据格式

4.高可用

> NoSQL在不太影响性能的情况，就可以方便的实现高可用的架构。比如Cassandra，HBase模型，通过复制模型也能实现高可用。



### Redis

Redis是用C语言开发的一个开源的高性能键值对（key-value）数据库。

#### 支持类型:

> 1. string 字符串类型
> 2. hash 散列类型
> 3. list 列表类型
> 4. set 集合类型
> 5. sorted set / zset 有序集合类型

#### 应用场景

> 1. 缓存（数据查询、短连接、新闻内容、商品内容等等）。（最多使用）
>
> 2. 聊天室的在线好友列表。
>
> 3. 任务队列。（秒杀、抢购、12306等等）
>
> 4. 应用排行榜。
>
> 5. 网站访问统计。
>
> 6. 数据过期处理（可以精确到毫秒）
>
> 7. 分布式集群架构中的session分离。

#### 常用命令

redis默认**十六个**数据库

1. 切换数据库 select 索引
2. dbsize 查看key的个数
3. flushdb 清空当前数据
4. flushall 清空所有数据库 16个库全删(删的是数据)

##### 通用语法

| 语法          | 含义                                                         |
| ------------- | ------------------------------------------------------------ |
| del key       | 根据key删除                                                  |
| keys *        | 查看所有key                                                  |
| type key      | 查询key的类型,根据类型 就可以使用不同的api                   |
| ttl key       | 查看key的有效时间    1 永不超时.  -2 没有这个数据或者这个数据已经过时了.  正数: 还有多少秒过期 |
| expire key  s | 设置数据的过期时间 单位秒  例如：expire name 20  表示key为name的缓存20秒后过期  男 |

##### string类型

| 语法    | 含义 |
| ------- | ---- |
| set key | 存入 |
| get key | 取出 |
| del key | 移除 |

> 特点: key = value

##### hash类型

hash是一个string类型的field和value的映射表。添加和删除操作都是O（1）（平均）的复杂度。hash类型特别适合用于存储对象。

| 语法                                  | 含义               |
| ------------------------------------- | ------------------ |
| **hset** key filed key                | 存入一个字段一个值 |
| **hmset** key field value field value | 存入多个字段多个值 |
| hget key filed                        | 取出               |
| hdel key filed                        | 删除               |
| hgetall key                           | 查看所有key语法    |

> 特点:
>
> key:{fieldname:fieldvalue,fieldname:fieldvalue}
>
> user: {username:zhangsan,gender:male,age:18}

##### list类型

| 语法                            | 含义                                               |
| ------------------------------- | -------------------------------------------------- |
| **lpush** key value value value | 左添加                                             |
| **rpush** key value value value | 右添加                                             |
| lrange key startIndex  endIndex | 0 表示从左开始的第一个值 -1 表示从右开始的第一个值 |
| lpop  key                       | 左弹                                               |
| rpop  key                       | 右弹                                               |

> 特点：
>
> ​	 key:[91，93，95]

##### set类型 天然幂等性

| 语法                           | 含义 |
| ------------------------------ | ---- |
| sadd key value value value ... | 存入 |
| smembers  key                  | 获取 |
| srem key  value value          | 移除 |

##### sorted set / zset类型

|                                                 |                                           |
| ----------------------------------------------- | ----------------------------------------- |
| zadd key  socre value  socre value  socre value | 存入                                      |
| zscore key member                               | 取出 获得指定成员的分数                   |
| zrange key start(索引) end(索引)                | 升序查询数据 [withscores]  []表示可有可无 |
| reverse                                         | 反转                                      |
| zrevrange  key start  end  [withscores]         | 降序查询数据                              |
| zrem key member                                 | 删除指定的成员                            |

#### Java连接Redis

1.导入jedis

2.

```java
        //1.创建jedis对象
        Jedis jedis = new Jedis("localhost",6379);
        //2.调用api，存入redis
        jedis.set("username","张三");
        //3.调用api从redis获取
        String username = jedis.get("username");
		//4.释放资源
		jedis.close();
```

##### JedisPool

```java
		//1.创建jedis配置对象
        JedisPoolConfig config = new JedisPoolConfig();
        //2.设置参数
        config.setMinIdle(3);//最小空闲数
        config.setMaxWaitMillis(3000); //最大等待时间
        config.setMaxTotal(30);//最大连接数
        //3.创建jedis连接池
        JedisPool jedisPool = new JedisPool(config,"localhost",6379);
        //4.获取连接
        Jedis jedis = jedisPool.getResource();
        //5.执行操作
        System.out.println(jedis.keys("*"));
        //6.释放
        jedis.close();
        jedisPool.close();
```

#### Redis持久化

1. RDB持久化（默认支持，无需配置）

> 特定时间间隔把内种数据集快照写入磁盘
>
> > 优势:可以选择每个小时归档一次最近24小时的数据，同时还要每天归档一次最近30天的数据,一旦出现故障,可以非常容易恢复,相较AOF,RDB启动效率更高,只需要fork出子进程,让子进程完成持久化工作
>
> > 劣势: 如果最大限度避免数据丢失(高可用),在定时持久化之前系统宕机,没来得及写入的都会丢失
>
> > 配置文件 `save 900 1` #每900秒(15分钟)至少有1个key发生变化，则dump内存快照。

2. AOF持久化

> 以日志的形式记录服务器所处理的每一个写操作,Redis服务器启东初读取日志重新构建数据库,以保证数据库数据完整
>
> > redis有(每秒,每修改,不)三种同步策略,**每秒同步**是异步持久化效率高,**每修改同步**是同步持久化效率低
>
> >对于相同数量的数据集而言，AOF文件通常要大于RDB文件
> >
> >AOF运行效率低于RDB,但每秒同步效率较高
>
> > 配置文件 `appendonly yes` no改为yes
> >
> > `appendfsync always` 修改同步 `appendfsync everysec`每秒同步  `appendfsync no`不同步
>
> <font color='red'>注意</font>:redis如果希望启动 日志机制 需要以配置文件方式启动redis 而不是双击打开exe程序。如果直接双击打开服务器 不会读取配置文件

3. 无持久化

> 可以通过配置的方式禁用Redis服务器的持久化功能，这样我们就可以将Redis视为一个功能加强版的memcached了。

> redis同时使用RDB和AOF

### RBAC

RBAC  是基于角色的访问控制（Role-Based Access Control ）在 RBAC  中，权限与角色相关联，用户通过成为适当角色的成员而得到这些角色的权限。这就极大地简化了权限的管理。这样管理都是层级相互依赖的，权限赋予给角色，而把角色又赋予用户，这样的权限设计很清楚，管理起来很方便。

经典权限五表

用户表user--(多对多)----角色表 role---(多对多)--权限表 permission  

 用户角色中间表(用户和角色的主键作为外键)    角色权限中间表(角色和权限的主键作为外键)





mysql 检查运行时间 先运行sql再运行此语句

```mysql 
show profiles
```





