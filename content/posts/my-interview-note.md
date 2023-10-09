---
title: "Jeff的Java面试笔记"
date: 2023-10-09T14:45:10+08:00
type: "posts"
---

学习网站

![image-20210823220417820](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210823220417820.png)



![image-20210823220438211](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210823220438211.png)

![d3b827765474461878dc0b49dd1cb851](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/d3b827765474461878dc0b49dd1cb851.jpg)



### 计算机原理相关

分为输入信息,存储信息,处理信息,输出信息四个阶段

计算机底层都是二进制来传输数据和信息,一个指令转化为电信号010101101...进来,内存存储二进制,CPU从内存读取二进制文件,处理并执行,发送执行结果电信号010101...给输出设备,输出设备解析~

### JVM

![image-20211116000053678](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211116000053678.png)

如果回收完还是没有足够空间存放,就会触发OOM(OutOfMemoryError)

![image-20211116000123034](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211116000123034.png)

对象引用的分类

1、强引用,程序中普遍存在的的对象引用;
2、软引用, SoftReference实现,内存溢出前回收;
3、弱引用, WeakReference实现,下一次垃圾回收
4、虚引用, Phantom Reference实现,形同虚设



面试问题

技巧

回答面试官问题前,先说一下知道的.

例如何解决缓存雪崩? 什么是缓存雪崩,如何解决,用什么解决,这么做的好处



#### springboot自动配置原理?

meta-inf     

#### springboot有几种部署方式

三种

jar java-jar     war  tomcat



描述2:
熟悉Java语言。对并发编程有一定了解,能设计并开发多线程程序;熟悉JVM的基本
概念,能进行JVM调优;熟悉集合、JUC等源码;熟悉反射、动态代理等Java语言特性。
熟悉 Spring、 Mybatis等常用框架,理解 Spring中常见的设计模式、对 Spring IOC、AOP
等特性的原理有一定了解。



描述2:
熟悉Java语言。对并发编程有一定了解,能设计并开发多线程程序;熟悉JVM的基本
概念,能进行JVM调优;熟悉集合、JUC等源码;熟悉反射、动态代理等Java语言特性。

![image-20210917092557331](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210917092557331.png)

![image-20210917093123207](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210917093123207.png)









语雀 tabd 禅道

![image-20210917112224522](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210917112224522.png)

#### 项目经验

7、项目经验着重写1到2个就可以了,其他的可以略写
8、项目经验要重点体现解决了什么问题,以及自己发挥的价值
项目背景(遇到了什么问题)
项目目标(要解决什么问题,以及解决到什么程度)
项目亮点(描述技术选型、遇到并解决了什么问题)
项目成果(取得了什么成果,最好量化)



#### SQL调优

设置慢查询时间,对慢查询语句进行记录

##### 索引

主键索引   不为空且不能重复 

唯一索引	可以为空,但不能重复

一般索引	



存emoji表情 编码utf-8 mb4



### 1.简单介绍下项目

  数据库表的设计

### 2.事务的特性(transaction)

#### 概念

如果一个业务包含多个步骤的操作,那么这些操作要么同时成功,要么同时失败

#### 原理

所有的操作先放在临时的事务日志中,当commit的时候,将日志中的信息写入数据库中;若rollback,会把事务日志中的信息相当于清空操作,相当于什么都没干

#### 特性 ACID

* A atomicity 原子性:一组逻辑单元,不能再分割,最小的单位
* C consistency 一致性:事务的前后,数据保持一致,要么同时成功,要么同时失败
* I  isolation隔离性: 多个事务之间互相隔离,互不影响
* D durability 持久性: 事务不可逆,一旦提交,不能再回滚()

> mysql事务默认自动提交,**每条sql语句默认使用一个事务,**
>
> 有些时候,我们应该手动开启事务,然后做操作,若成功了把这些操作都提交(持久化到数据库中),否则就回滚(刚才的操作都不算数)

开启事务-->执行sql语句-->提交事务

##### 不考虑隔离性会产生的问题]

* 脏读:一个事务读到另一个事务没有提交的数据

  > 例:假设两个线程同时进来,A事务进行更新(这个时候遇到某些错误导致A还没来得及提交到数据库,也就是没有成功执行修改 ),B事务就进行读取,B读到了A更新前的数据,产生了脏读
  >
  > 财务给张三发了1000元的工资，然后张三查询自己的账户，果然多了1000元，变成了2000元，结果财务操作过程有误，事务回滚。当张三再查账户时，却发现账户只有1000元

* 不可重复读: 在一个事务中,两次读取某些记录的结果不一致.是因为某些事务更新了这些数据导致的

  > 例: B读了两次数据,A在B读第二次数据的过程中修改了数据,导致B读两次数据不一致,这时发生了不可重复读
  >
  > > 不可重复读有一种特殊情况，`当两个或多个事务查询相同的记录，然后各自基于查询的结果更新记录时会造成第二类丢失更新问题。每个事务不知道其它事务的存在，最后一个事务对记录所做的更改将覆盖其它事务之前对该记录所做的更改`。这种情况就是大名鼎鼎的**第二类丢失更新**。主流的数据库已经默认屏蔽了第一类丢失更新问题（`如果事务A被提交，B事务被撤销，那么会连同事务A所做的更新也被撤销。`），但我们编程的时候仍需要特别注意第二类丢失更新。

* 幻读(虚读):在一个事务中,两次统计的结果不一致,是因为某些事务新增或者删除了这些数据导致的.

  > 例:事务1查询id<10的记录时，返回了2条记录，接着事务2插入了一条id为3的记录，并提交。接着事务1查询id<10的记录时，返回了3条记录，说好的可重复读呢？结果却多了一条数据。发生了幻读
  >
  > > 幻读强调的集合的增减，而不是单独一条数据的修改

> 不可重复读重点在于update和delete，而幻读的重点在于insert

#### 事务的隔离级别

```markdown
多个事务之间应该相互隔离的,相互独立的.但是如果多个事务操作同一批数据,则会引发一些问题,设置不同的隔离级别就可以解决这些问题.
```

| 级别 | 名字     | 隔离级别         | 脏读 | 不可重复读 | 幻读 | 数据库默认隔离级别        |
| ---- | -------- | ---------------- | ---- | ---------- | ---- | ------------------------- |
| 1    | 读未提交 | read uncommitted | 有   | 有         | 有   |                           |
| 2    | 读已提交 | read committed   | 无   | 有         | 有   | Oracle和SQL Server        |
| 3    | 可重复读 | repeatable read  | 无   | 无         | 有   | MySQL,mysql默认处理了幻读 |
| 4    | 串行化   | serializable     | 无   | 无         | 无   | 单事务                    |

> 理解记忆:
>
> * 读了还没提交:一个事务进来还没有提交,你就来读,着什么急?会有很多问题,都防不住,只能解决第一类更新丢失
> * 读了已经提交:这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。这种隔离级别可以防止**脏读**问题，但会出现不可重复读及幻读问题。
> * 可重复读: 这是MySQL的默认事务隔离级别，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。这种隔离级别可以防止**除幻读**外的其他问题.(就是每次读取的结果集都相同，而不管其他事务有没有提交)
> * 可串行化:这是最高的隔离级别，它通过强制事务排序，使之不可能相互冲突，从而解决幻读、第二类更新丢失问题。在这个级别，可以解决上面提到的所有并发问题，但可能导致大量的超时现象和锁竞争，通常数据库不会用这个隔离级别，我们需要其他的机制来解决这些问题:乐观锁和悲观锁。
>
> > 乐观锁比较适用于读多写少的情况(多读场景)，悲观锁比较适用于写多读少的情况(多写场景)。

###  Spring常用注解

#### 1.1 `@Component`

**作用：**

```mel
调用无参构造创建一个bean对象，并把对象存入spring的IOC容器，交由spring容器进行管理。相当于在xml中配置一个bean。
```

**属性：**

```applescript
value：指定bean的id。如果不指定value属性，默认bean的id是当前类的类名。首字母小写。
```

#### 1.2 `@Controller`

**作用：**

```aspectj
作用上与@Component。一般用于表现层的注解。
```

**属性：**

```applescript
value：指定bean的id。如果不指定value属性，默认bean的id是当前类的类名。首字母小写。
```

#### 1.3 `@Service`

**作用：**

```aspectj
作用上与@Component。一般用于业务层的注解。
```

**属性：**

```applescript
value：指定bean的id。如果不指定value属性，默认bean的id是当前类的类名。首字母小写。
```

#### 1.4 `@Repository`

**作用：**

```aspectj
作用上与@Component。一般用于持久层的注解。
```

**属性：**

```applescript
value：指定bean的id。如果不指定value属性，默认bean的id是当前类的类名。首字母小写。
```

#### 1.5 `@Bean`

**作用：**

```java
用于把当前方法的返回值作为bean对象存入spring的ioc容器中
```

**属性：**

```java
name：用于指定bean的id。当不写时，默认值是当前方法的名称。注意：当我们使用注解配置方法时，如果方法有参数，spring框架会去容器中查找有没有可用的bean对象，查找的方式和Autowired注解的作用是一样的。
```

**案例：**

```java
/**
 * 获取DataSource对象
 * @return
 */
@Bean(value = "dataSource")
public DataSource getDataSource() {
    try {
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass(this.driver);
        dataSource.setJdbcUrl(this.url);
        dataSource.setUser(this.username);
        dataSource.setPassword(this.password);
        return dataSource;
    }catch (Exception exception) {
        throw new RuntimeException(exception);
    }
}
```

### 2）用于依赖注入的注解

#### 2.1 `@Autowired`

**作用：**

```java
@Autowire和@Resource都是Spring支持的注解形式动态装配bean的方式。Autowire默认按照类型(byType)装配，如果想要按照名称(byName)装配，需结合@Qualifier注解使用。
```

**属性：**

```java
required：@Autowire注解默认情况下要求依赖对象必须存在。如果不存在，则在注入的时候会抛出异常。如果允许依赖对象为null，需设置required属性为false。
```

**案例：**

```java
@Autowire 
@Qualifier("userService") 
private UserService userService;
```

#### 2.2 `@Qualifier`

**作用：**

```java
在自动按照类型注入的基础之上，再按照 Bean 的 id 注入。它在给字段注入时不能独立使用，必须和  @Autowire一起使用；但是给方法参数注入时，可以独立使用。
```

**属性：**

```java
value：用于指定要注入的bean的id，其中，该属性可以省略不写。
```

**案例：**

```java
@Autowire
@Qualifier(value="userService") 
//@Qualifier("userService")     //value属性可以省略不写
private UserService userService;
```

#### 2.3 `@Resource`

**作用：**

```java
@Autowire和@Resource都是Spring支持的注解形式动态装配bean的方式。@Resource默认按照名称(byName)装配，名称可以通过name属性指定。如果没有指定name，则注解在字段上时，默认取（name=字段名称）装配。如果注解在setter方法上时，默认取（name=属性名称）装配。
```

**属性：**

```java
name：用于指定要注入的bean的id
type：用于指定要注入的bean的type
```

**装配顺序**

```java
1.如果同时指定name和type属性，则找到唯一匹配的bean装配，未找到则抛异常；
2.如果指定name属性，则按照名称(byName)装配，未找到则抛异常；
3.如果指定type属性，则按照类型(byType)装配，未找到或者找到多个则抛异常；
4.既未指定name属性，又未指定type属性，则按照名称(byName)装配；如果未找到，则按照类型(byType)装配。
```

**案例：**

```java
@Resource(name="userService")
//@Resource(type="userService")
//@Resource(name="userService", type="UserService")
private UserService userService;
```

#### 2.4 `@Value`

**作用：**

```java
通过@Value可以将外部的值动态注入到Bean中，可以为基本类型数据和String类型数据的变量注入数据
```

**案例：**

```java
// 1.基本类型数据和String类型数据的变量注入数据
@Value("tom") 
private String name;
@Value("18") 
private Integer age;


// 2.从properties配置文件中获取数据并设置到成员变量中
// 2.1jdbcConfig.properties配置文件定义如下
jdbc.driver \= com.mysql.jdbc.Driver  
jdbc.url \= jdbc:mysql://localhost:3306/eesy  
jdbc.username \= root  
jdbc.password \= root

// 2.2获取数据如下
@Value("${jdbc.driver}")  
private String driver;

@Value("${jdbc.url}")  
private String url;  
  
@Value("${jdbc.username}")  
private String username;  
  
@Value("${jdbc.password}")  
private String password;
```

### 3）用于改变bean作用范围的注解

#### 3.1 `@Scope`

**作用：**

```java
指定bean的作用范围。
```

**属性：**

```java
value：
    1）singleton：单例
    2）prototype：多例
    3）request： 
    4）session： 
    5）globalsession：
```

**案例：**

```java
@Autowire
@Scope(value="prototype")
private UserService userService;
```

### 4）生命周期相关的注解

#### 4.1 `@PostConstruct`

**作用：**

```java
指定初始化方法
```

**案例：**

```java
@PostConstruct  
public void init() {  
    System.out.println("初始化方法执行");  
}
```

#### 4.2 `@PreDestroy`

**作用：**

```java
指定销毁方法
```

**案例： [集合.xmind](H:\IDM\java\集合.xmind) **

```java
@PreDestroy  
public void destroy() {  
    System.out.println("销毁方法执行");  
}
```



### Java的集合类

### 多线程

#### 线程池

#### JVM内存与调优

### 简单介绍下项目

#### 项目遇到的难点

用到了哪些组件?为什么要用?

##### 跨域问题

统计线上人数?



### 集合排序?(考察集合与API)

### SQL查询效率?(考察SQL优化)

不要使用*   不使用null查询  不是使用or  不使用函数计算

模糊匹配时候 只用后面一个%

查询时候如果知道只查询一条,使用limit,就不会继续向下扫描

### RPC框架,和MQ 

### Linux

#### 测一下两个端口间是否是通的

### HashMap底层

### Cookie和Session区别

cookie禁用了 ,进行 URL重写，就是把session id直接附加在URL路径的后面

### 缓存与数据库双写一致性 

同步:写数据库时候同时写缓存

异步:

> 1.数据库有日志,修改过后的有日志,主数据库把日志发给从数据库
>
> 阿里的kindle?可以把自己伪装成从服务器,
>
> 根据消息中间件去把数据库的日志去写给redis
>
> 2.新建一个任务,按照一个频率去把数据库修改的东西与redis进行更新,但是弊端在于这个任务的执行频率不能确定

用双删的策略,执行写操作前删除缓存? (不确定)

线程去解决没有结果,消息中间件可以解决异步



jpa可以用来生成表(面向对象,创建好对象运行就可以生成数据库表)

hashmap里的key存value该如何处理? 

双检索里用volatile修饰为什么?



RDB和AOF

什么是Restful风格

接口幂等性

重复提交的请求,接口必须一致,网络抖动不能导致重复扣款,代码逻辑可以通过订单的id标记他的唯一性,判断如果已经付过款,就不能二次付款/或者生成唯一token放在redis中,页面跳转时获取token放在前端,下次请求时,先提交token,然后校验,生成新的token在redis里更新,后台验证,等form表单再次提交到时这样等新的token和旧的token不一致的话,不让提交

linux动态查看日志

spring创建对象的作用域有哪些

spring创建的单例线程安全是如何解决(controller层和service层创建全局变量就会导致线程安全问题)

aop的底层?用的是jdk还是cglib动态代理

euraka做集群? 双节点互相注册

分布式事务如何处理的?CAP只能满足其中两个  BASE 理论   强一致性,最终一致性事后补偿最大努力通知等 熔断机制

 如何保证redis里面的数据都是热点数据

如何解决缓存雪崩 搭建缓存,避免单机故障 / 通过加锁或队列控制读数据库写缓存的数量/缓存的reload机制

如何解决缓存穿透? 布隆过滤

什么时候用到了消息中间件?用在哪里  用在了注册服务,发短信,发邮件  消息丢失?消息重复消费?

用到了全文检索?

输入一个关键词-->提交到后台进行分词-->es的倒排索引,用全局的映射文档建立好索引,存储在es不同的分片上,es里有主节点..节点-->通过分词后的结果去查找到想要的结果-->记录总条数等?

主节点挂了,选举机制是?

es集群,必须只能有一个master节点,多个master会有脑裂现象,避免脑裂,选举出master节点被多数节点认可,设置最小节点数量minimum_master_nodes:2

项目中用到了那些设计模式?

利用框架自带的去完成

接口里都是抽象方法,和抽象类的区别

冒泡排序

快速排序

Hashmap为什么线程不安全

链表的数据结构

JVM

![1617678673749](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1617678673749.png)

### String与StringBuilder区别

StringBuilder是可变的,String不是可变的

String常用方法

* substring()//截取字符串,得到新的并返回

* replace()//替换

* split()//切割

  正则表达式相关方法

* matches("正则表达式")//验证正则表达式是否符合规则

* `split("\\s+")`//根据空格切割字符串

* `replaceAll("\\s+", "*")`;//使用*替换字符串空格

StringBuilder常用方法

* append()//添加
* reverse()//翻转
* toString()//转为String

### 集合和数组的区别

**数组:**

1.是引用数据类型的一种,可以存储多个元素

2.数组的长度是固定的 int[] arr1 = new int[10];  int[] arr2 = {1,2,3};  

3.数组即可以存储基本数据类型的数据,又可以存储引用数据类型的数据 int[] double[] String[] Student[]

```
sort排序  toString 转换成字符串
```



**集合:**  

1.是引用数据类型的一种,可以存储多个元素

2.集合的长度是可以变化的(添加元素,删除集合中的元素)

3.集合只能存储引用数据类型的数据 

> ArraysList 集合内部采用数组保存数据

 `ArrayList<int> 错误  ArrayList<Integer>  ArrayList<Student> ArrayList<String>   ArrayList<Integer>正确`  

```
1.增：
      1).public boolean add(E e) 将指定的元素追加到此集合的末尾.【排队】
      E是什么？当定义集合时，“泛型”是什么类型，这个E就是什么类型：
      ArrayList<String> list = new ArrayList<>();//add()方法的参数就是String类型      
      2).public void add(int index,E element) 在此集合中的指定位置插入指定的元素【插队】
2.删：
      3).public boolean remove(Object o) 删除指定的元素，返回删除是否成功。返回值：是否删除成功！
      4).public E remove(int index) 删除指定索引处的元素，返回被删除的元素。返回值：被删除的对象！
3.改：
      5).public E set(int index,E element) 修改指定索引处的元素，返回被修改的元素
4.查：
      6).public E get(int index) 返回指定索引处的元素
      7).public int size() 返回集合中的元素的个数
```

### final关键字

![image-20210417173829525](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210417173829525.png)



只能赋值一次

### 权限修饰符

![image-20210417123403728](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210417123403728.png)

final关键字的赋值有以下几种方式：

- 显式赋值：private final Integer code = 1

- 静态代码块/代码块赋值

- 构造器赋值

  > 被 static final 修饰后不能在构造器赋值了

> 金句:当类被加载进内存的时候，这个属性并没有给其分配内存空间，而只是定义了一个变量，只有当类被实例化的时候这个属性才被分配内存空间，而实例化的时候同时执行了构造函数，所以属性被初始化了，也就符合了当它被分配内存空间的时候就需要初始化，以后不再改变的条件。



#### Static关键字

##### 静态变量

静态变量被多个对象所共享，它的静态变量只有一份（在静态区),一个对象改变了值，其它对象使用的就是改变后的

```java
//其他代码略
    public static String country = "中国";
    System.out.println(stu1.country)//中国
    System.out.println(stu2.country)//中国
    stu1.country = "中华人民共和国";
    //静态变量被多个对象所共享，一个对象改变了值，其它对象使用的就是改变后的
    System.out.println("改变后的2"+stu2.country);//中华人民共和国
    System.out.println("改变后的1"+stu1.country);//中华人民共和国
```

##### 静态方法

static 关键字用来声明独立于对象的静态方法。静态方法不能使用类的非静态变量。

### 接口

一个抽象类型，是抽象方法的集合

特性

- 接口中每一个方法也是隐式抽象的,接口中的方法会被隐式的指定为 **public abstract**（只能是 public abstract，其他修饰符都会报错）。**接口中的每一个方法都是抽象方法!!!**
- 接口中可以含有变量，但是接口中的变量会被隐式的指定为 **public static final** 变量（并且只能是 public，用 private 修饰会报编译错误）。
- 接口不能被实例化  `不能new`
- 实现类必须实现接口中所有的**抽象**方法
- 一个类可以实现多个接口
- 一个接口可以继承多个接口

### 多态

同一个引用类型，使用不同的实例而执行不同的操作

`你和你妹和你爸仨人都会唱歌(但你们仨人都有着不同的音乐风格,你是摇滚,你妹是朋克,你爸风格没有界限,平时没事就唱一唱();其实是你和你妹继承了你爸的优良的唱歌细胞【多态必须有父子类继承关系】,完后俩人自己再摸索摸索,你和你妹平时也会唱一唱()【子类重写父类方法】,这时你妈她想听听摇滚,就让你来唱一唱(传个你进来)【使用父类的引用指向子类的对象】,你妈想听朋克就让你妹来唱一唱(传你妹进来)【使用父类的引用指向子类的对象】~`

重点在于**重写**,子类重写了父类的方法,在调用时,看似调用的是父类的方法,其实调用的是子类重写过的方法

多态的使用规则：

> 1、必须有继承关系的父子类
>
> 2、子类要重写父类的方法
>
> 3、使用父类的引用指向子类的对象（向上转型）`// Worker w  = new Sales();   w.work();` 

```java
public abstract class Worker {
    //抽象方法
    public abstract void work();
}
```

```java
public class Sales extends Worker {
    @Override
    public void work() {
        System.out.println("售货");
    }
}
```

```java
//经理
public class Manager {
    //方法的重载

   //找售货员谈话
    public void talk(Sales sales){
        sales.work();
    }
	
   //找员工谈话
    public void talk(Worker worker){
        worker.work();
    }
}
```

```java
public class Test {

    public static void main(String[] args) {
        //接收子类的实例
        Worker w = new Sales();
		//实质是父类引用调用子类
        Manager m = new Manager();
        m.talk(w); //调用谈话方法
    }
}
```



`Animal a = new Cat();`

`由于Cat是继承自它的父类Animal，所以Animal类型的引用是可以指向Cat类型的对象的。这就是“向上转型”` 

### 包装类

比基本类型强大

##### 拆装箱

    在JDK5的时候，多了一个新的特性，叫做自动拆装箱。
        拆箱： 包装类类型转成对应的基本类型。
        装箱： 基本类型转成对应的包装类类型。
    
    有了自动拆装箱后，基本类型以及包装类类型可以自动转换。

##### 一些方法

```markdown
如果要将字符串转成int类型，那么就是Integer中的静态方法parseInt

字符串包装类String的方法
concat 拼接
contains包含
startsWith&endsWith 以??开头/结尾
indexOf&lastIndexOf 字符串内容第一次/最后一次出现的索引
toCharArray 将字符串转成一个字符数组并返回
toLowerCase&toUpperCase 将字符串转成小写/大写返回
trim 去除前后空格
split 切割

```

### Collection

![image-20211215162522997](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211215162522997.png)

![1593089223830](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1593089223830.png)


​    

```markdown
Collection是所有单列集合的根接口
常见方法：
    (常用)public boolean add(E e) ： 把给定的对象添加到当前集合中 。
    public void clear() :清空集合中所有的元素。
    public boolean remove(E e) : 把给定的对象在当前集合中删除。
    public boolean contains(Object obj) : 判断当前集合中是否包含给定的对象。
    public boolean isEmpty() : 判断当前集合是否为空。
    (常用)public int size() : 返回集合中元素的个数。
    public Object[] toArray() : 把集合中的元素，存储到数组中

Collection是一个接口，如果要用，需要使用实现类，最常用的实现类是ArrayList

Collection中**没有索引有关**的方法，因为不是所有的集合都有索引
```

 ```markdown
for循环不适用与没有索引的集合,用Collection里的Iterator<E>迭代器:

	Iterator是一个接口，表示迭代器，里面的方法：
	boolean hasNext()：判断是否还有元素可以获取。
	E next()：获取光标位置的元素，将光标向后移动。
 ```

#### 并发修改异常

```java
/*
    如果迭代器遍历的过程中使用集合的方法对集合进行增删操作，再次进行遍历会引发并发修改异常。
 */
public class Demo01Test {
    public static void main(String[] args) {
        //创建集合
        ArrayList<String> list = new ArrayList<>();
        //添加元素
        list.add("hello");
        list.add("world");
        list.add("java");
        list.add("php");
        //遍历删除
        Iterator<String> iterator = list.iterator();
        //循环遍历
        while (iterator.hasNext()) {
            String str = iterator.next();
            if (str.equals("hello")) {
                list.remove(str);
                break;
            }
        }
    }
}
```

#### List接口

##### 特点

1. 有序（按照什么顺序存，就按照什么顺序取）
2. 有索引（可以根据索引获取元素）
3. 可以重复（可以存放重复元素）

```markdown
List是Collection下面的一个子接口，有三个特点：


List是一个接口，如果要用，需要使用实现类，最常用的实现类是ArrayList。
```

##### 常见方法

```markdown
public void add(int index, E element) : 将指定的元素，添加到该集合中的指定位置上。
public E get(int index) :返回集合中指定位置的元素。
public E remove(int index) : 移除列表中指定位置的元素, 返回的是被移除的元素。
public E set(int index, E element) :用指定元素替换集合中指定位置的元素,返回值的更新前的元素。
```

##### ArrayList实现类和特点 

```markdown
/*
    ArrayList是List接口下面最常用的实现类。
    ArrayList内部是在使用数组保存数据。
    ArrayList的(数组的)特点是查询快，增删慢。
    
    ArrayList又称为**可变数组**,其实就是数组,知识自动扩容
    
    ArrayList = 数组 + 自动扩容
 */
```

##### LinkedList实现类和特点(了解)

```markdown
    LinkedList也是List接口下面的实现类
    LinkedList内部是在使用**双向链表**保存数据。
    LinkedList(链表的)特点是查询慢增删快。

    LinkedList特有方法(了解)：
        public void addFirst(E e) :将指定元素插入此列表的开头。
        public void addLast(E e) :将指定元素添加到此列表的结尾。
        public E getFirst() :返回此列表的第一个元素。
        public E getLast() :返回此列表的最后一个元素。
        public E removeFirst() :移除并返回此列表的第一个元素。
        public E removeLast() :移除并返回此列表的最后一个元素。
        public E pop() :从此列表所表示的堆栈处弹出一个元素。
        public void push(E e) :将元素推入此列表所表示的堆栈。
```

#### Set接口

##### 特点

Set接口有以下特点：
       1. 无索引（不能根据索引获取元素的）
              2. 不可重复（不能保存重复元素）
              3. 无序的（大部分Set集合满足的特点）（按照什么顺序存，不一定按照什么顺序取）

```java
/*
    Set是Collection下面的一个子接口。
    Set是一个接口，如果要用需要使用实现类，Set接口下最常用的实现类是HashSet
    HashSet满足Set接口的所有的特点（无序，无索引，不可重复）
 */
//创建Set集合
        Set<String> set = new HashSet<>();
```

### 哈希值

```java
/*
    哈希值其实就是一个int数字，我们可以将哈希值看成对象的一个标识（特征码）
    在Object中有一个方法叫做hashCode，可以获取对象的哈希值。取值[-21亿-21亿]
        int hashCode()：获取对象的哈希值。

    Object中的hashCode方法，`哈希值的计算方式是根据对象的地址值计算的`。
    对象的哈希值根据地址值计算一般来说意义不大，我们更多的是希望哈希值是根据属性计算的，如果两个对象的属性完全相同，
    哈希值也应该相同。
    如果想要自己定义哈希值的计算规则，需要重写hashCode方法。
    哈希值是对象的一个标识，但并不是唯一的标识，对象的哈希值允许重复。
 */
Person p =  new Person("张三",15);
sout(p.hashCode());//输出p的哈希值
/*
也可以重写hashCode方法,自定义哈希值的计算规则
*/
```

#### 哈希表  (复习 归并时按照字母归并)

![1607312321427](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1607312321427.png)



#### HashSet保证唯一性

```markdown
     HashSet判断唯一性的过程
        1.先比较对象的哈希值。
            如果哈希值不同，肯定是不同的对象。
            如果哈希值相同，不一定是同一个对象。
        2. 如果哈希值相同，还会调用equals进行比较。
            如果equals的结果是true，表示对象相同。
            如果equals的结果是false，表示对象不同
            
如果使用HashSet存储自定义对象并保证唯一性（对象的属性相同就看成是同一个对象），需要同时重写hashCode和equals，缺一不可。
```



![1593181249975](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1593181249975-1632306460491.png)



### Map

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1169114-20190710091441495-1652040981.png)

Map接口不是Collection的子接口,但仍然被看做Collection框架的一部分

![image-20200629213921984](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20200629213921984.png)

![image-20210922183439643](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210922183439643.png)



#### 常见方法

```java
/*
    与collection单列集合不同,Map是双列集合，里面的每一个元素都是键值对，一个键对应一个值。
    Map<K,V>有两个泛型，K表示键的类型，V表示值的类型

    Map中的常见方法：*/
        (常用)V put(K key, V value)：向Map集合中添加键值对元素。key表示键，value表示值。如果在添加时key重复，会使用新的值覆盖掉原来的值。
        (常用)V get(Object key)：根据键获取对应的值。
        boolean containsKey(Object key)：判断map集合中是否包含指定的键。
        V remove(Object key)：根据键删除整个的键值对，返回值是被删除掉的值。

    Map是一个接口，如果要用需要使用实现类，最常用的实现类是HashMap();
 
```

#### Map集合的遍历

##### keySet()遍历

```java
/*
    Map集合不能通过迭代器或者增强for直接进行遍历。
    如果要遍历Map集合，我们可以获取到Map集合中所有的键，将所有的键放入到Set集合中， 然后遍历Set集合，拿到集合中的每一个键，
    然后根据键去Map集合中获取对应的值。

    Map中有一个方法叫做keySet，可以获取到所有的键
        Set<K> keySet()：获取Map集合中所有的键，并放入到Set集合中返回。

    keySet遍历步骤：
        1. 调用Map集合的keySet方法获取到所有的键，然后放入到Set集合中。
        2. 遍历Set集合，拿到里面的每一个键。
        3. 调用Map集合的get方法，根据键获取对应的值。
 */
public class Demo02KeySet {
    public static void main(String[] args) {
        //创建集合
        Map<String, String> map = new HashMap<>();
        //添加元素
        map.put("it001", "张三");
        map.put("it002", "李四");
        map.put("it003", "王叔叔");
        //开始遍历
        //1. 调用Map集合的keySet方法获取到所有的键，然后放入到Set集合中。
        Set<String> keys = map.keySet();
        //2. 遍历Set集合，拿到里面的每一个键。
        for (String key : keys) {
            //3. 调用Map集合的get方法，根据键获取对应的值。
            String value = map.get(key);
            System.out.println(key + "-----" + value);
        }
    }
}
```

##### entrySet遍历

```java
/*
    Map集合还有一种遍历方式是通过Entry对象的方式进行遍历。
    Map集合中每一个元素其实都是一个键值对，每一个键值对都是一个Entry对象。

    如果要通过Entry对象的方式遍历Map集合，我们需要先获取到Map集合中所有的Entry对象，将这些Entry对象放入到一个Set集合，然后
    遍历Set集合，拿到Set集合中每一个Entry对象，根据该Entry对象获取键以及对应的值。

    在Map集合中有一个方法叫做entrySet，可以获取到所有的Entry对象
        Set<Map.Entry<K,V>> entrySet()：获取到所有的Entry对象并放入到Set集合中返回。

    Entry中获取键和值的方法
\K getKey()：获取到Entry对象中的键。,\
        V getValue()：获取Entry对象中的值。


    entrySet遍历步骤：
        1. 调用Map集合的entrySet方法获取到所有的Entry对象并放入到Set集合中返回
        2. 遍历Set集合，拿到里面的每一个Entry对象
        3. 调用Entry对象的getKey和getValue获取到键和值


    Map集合有两种遍历方式
        keySet方式(键找值)【推荐】
        entrySet方式(键值对)
 */
public class Demo03EntrySet {
    public static void main(String[] args) {
        //创建集合
        Map<String, String> map = new HashMap<>();
        //添加元素
        map.put("it001", "张三");
        map.put("it002", "李四");
        map.put("it003", "王叔叔");
        //开始遍历
        //1. 调用Map集合的entrySet方法获取到所有的Entry对象并放入到Set集合中返回
        Set<Map.Entry<String, String>> entries = map.entrySet();
        //2. 遍历Set集合，拿到里面的每一个Entry对象
        for (Map.Entry<String, String> entry :entries) {
            //3. 调用Entry对象的getKey和getValue获取到键和值
            String key = entry.getKey();
            String value = entry.getValue();
            System.out.println(key + "----" + value);
        }
    }
}
```

##### HashMap判断唯一性

```java
/*	keyvalue相同就看做同一个对象,key如是自定义类,就必须重写hashcode和equals方法
    练习：每位学生（姓名，年龄）都有自己的家庭住址。那么，既然有对应关系，则将学生对象和家庭住
         址存储到map集合中。学生作为键, 家庭住址作为值。
    注意，学生姓名相同并且年龄相同视为同一名学生。


    HashMap保证唯一性(键)的方式和HashSet是一模一样。 因为HashSet内部就是在使用HashMap保存数据。 HashSet的存储方式是把HashMap中的Key作为Set的对应存储项
    判断唯一性的方式：
        1. 先比较两个对象的哈希值。
            如果对象的哈希值不同，肯定是不同的对象。
            如果对象的哈希值相同，不一定是同一个对象。
        2. 然后比较两个对象的equals方法
            如果equals方法结果是true，表示两个对象相同。
            如果equals方法结果是false，表示两个对象不同。

    如果HashMap要保证键的唯一性（属性相同就看成是同一个对象），需要同时重写hashCode和equals方法
 */
```

##### LinkedHashMap

```java
/*
    了解

    LinkedHashMap是Map接口下一个不常用的实现类
    内部除了有一个哈希表之外还有一个链表，链表可以保证有序
 */
```

#### HashMap底层

x = logn就是2^x =n

我们知道HashMap底层是由数组+链表/红黑树构成的，
当我们通过put(key, value)向hashmap中添加元素时，需要通过散列函数确定元素究竟应该放置在数组中的哪个位置，当不同的元素被放置在了数据的同一个位置时，后放入的元素会以链表的形式，插在前一个元素的尾部，这个时候我们称发生了hash冲突

put hash算法算出hash值,获取数组对应下标,存

get 获取数组对应下标,找到对应链表,链表里存的是key和value,根据equals方法判断,找到则返回



### Collections

```markdown
 Collections是一个操作集合的工具类。
 static void shuffle(List<?> list)：对集合中的内容打乱顺序。
 static void sort(List list)：对集合中的元素进行排序（自然排序）
 static void sort(List list, Comparator c)：对集合中的内容进行排序，参数c表示比较器（比较器排序）
 ////Comparator是个接口,要使用它需要创建一个实现类,并在其中定义
 使用//// Collections.sort(list, new Rule());
 static boolean addAll(Collection c, T... elements)：批量添加元素。
            参数c：表示向哪个集合添加元素。
            参数elements：表示要添加那些元素。 该参数是可变参数，可以向该参数位置传递任意个数据。
 使用//// Collections.addAll(list, "hello", "world", "java", "php");           
```

```java
public class Rule implements Comparator<Student>{
    /*
        当调用Collections的sort方法进行比较器排序时，那么系统内部会自动调用compare方法比较两个对象的大小。
        如果该方法返回值是正数，说明第一个参数大于第二个参数。
        如果该方法的返回值是0，说明两个对象相等。
        如果该方法的返回值是负数，说明第一个参数小于第二个参数。
        排序公式：
            升序就是一减二
     */
    @Override
    public int compare(Student o1, Student o2) {
        //根据年龄升序排序
        return o1.getAge() - o2.getAge();
    }
```

### 可变参数

```java
/*
    JDK5的时候，多了一个新的特性叫做可变参数。
    如果一个方法的参数是可变参数，那么可以在该参数位置传递任意个数据。

    可变参数格式：
        修饰符 返回值类型 方法名(参数类型... 参数名) {
            方法体;
            return 返回值;
        }

        1. 在调用可变参数的方法时，可以向可变参数位置传递任意个数据
        2. 可变参数的本质就是数组，所以可以将可变参数当成数组去使用
        3. 以为可变参数的本质是数组，所以调用可变参数的方法时，也可以向可变参数位置传递数组。

    注意：
        1. 一个方法中最多只能有一个可变参数。
        2. 方法中可变参数必须在最后一个位置。
 */
//其他代码忽略
public static int getSum(int... nums) { //这里的int...nums 就是可变参数,参数是int类型的数组 
}
```





### 泛型(未完待续)

**泛型是一种技术,对泛型类和泛型接口的统称**

<T>T就是形式类型参数

<String> String就是实际类型参数

ArrayList<T> 就是一种模板(模板方法模式),ArrayList<String>/ArrayList<Integer>... 变量类型定不了,就抽成类型参数

`泛型类就是一种模板类`

`翻译翻译 ArrayList<String> 程序员告诉编译器:我要传String类型的值进来,帮我盯着点!`不过在运行时,还是用Object[] 去接收的

定义了ArrayList<String> 编译器层面就会被赋值成

```java
public class StringArrayList{
    private String[] array;//ArrayList是由数组存储,但数组被定义了类型,就不能存其他类型了
}
//--------------------------------------
//JDK1.5之前没有泛型时
public class ArrayList{
    private Object[] array;
    //...
}
//使用时还需要向下转型
//如果装错了转型后还会报错
ArrayList list = new ArrayList();
list.add(new Integer(123));
// ERROR: ClassCastException:
String second = (String) list.get(1);

//所以泛型的好处是
1.省略了使用时向下转型
2.将运行时异常提前到编译期(指定了泛型,编译时会检查,如果传入的不是当前泛型指定类编译都会报错)
```

`金句:变量是对数据的抽取,而泛型是对变量类型的抽取,抽取成类型的参数,抽象`

`金句:代码模板的本质:用Object接收一切对象，用泛型+编译器限定特定对象，用多态支持类型强转`

> 代码模板的定义:类框架都搭建好,不确定这些方法和规则是用来操作什么类型对象的,但必须做到:**无论传什么类型,这套规则都能接收**

![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1599582925187-7c90e872-a107-487a-94e3-2bbbf5b7fa79.png)

例子

```java
public User getUser(String name){
    ...
}
public User getUser(Date birthday){
    ...
}

//使用变量类型对泛型进行抽取
public User getUser(T t){
    ...
}
```

#### 注意

**泛型是对引用类型的抽取，基本类型是无法抽取的**

```markdown
泛型也可以省略，如果省略泛型相当于泛型是Object,底层还是用Object去存
泛型擦除:
	Java中的泛型都是伪泛型，泛型只在源代码阶段有效，一旦编译，泛型就会消失。
源代码 --> 经过编译器 -->.class文件(字节码文件) -->虚拟机

也就是说泛型在编译过后只剩下了一个确定的类型

```

#### 理解

* 泛型是JDK为**编译器**创造的语法糖,只在编译期有编译器负责解析,虚拟机不知情
* 存入的时候,普通类**继承**泛型类并给变量类型T赋值以后,就能强制让**编译器**进行类型校验
* 取出,代码编译时,**编译器**底层会根据实际类型参数(传入的是String?User?)自动进行类型转换,不需要再手动强转
* 编译后.Class 虚拟机看到的还是Object
* 可以利用**反射**绕过编译器对泛型的检查



```java
public class GenericClassDemo {

    public static void main(String[] args) throws Exception {

        List<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        // 编译器会阻止
        // list.add(333);

        // 但泛型约束只存在于编译期，底层仍是Object，所以运行期可以往List存入任何类型的元素
        Method addMethod = list.getClass().getDeclaredMethod("add", Object.class);
        addMethod.invoke(list, 333);

        // 打印输出观察是否成功存入Integer（注意用Object接收）
        for (Object obj : list) {
            System.out.println(obj);
        }
    }
}
```



```java
//泛型类:
public class Factory<T> {};//使用时泛型才能确定

//泛型方法:
public class Factory<T> {

    /*
        定义方法，接收什么类型的参数，就得到什么类型的结果
     */
    public <E> E getSame(E e) {
        return e;
    }
}

//泛型接口
public interface MyInterface<T> {
    //定义抽象方法，使用泛型类型T当做参数类型和返回值类型
    T method(T t);
}
```

#### 泛型通配符

```java
/*    如果想要接受任何类型类型的泛型，可以使用泛型通配符
    ?表示泛型通配符，可以匹配任何类型的泛型。

    泛型通配符要放在方法参数位置被动匹配， 不要主动使用。
*/

public class Demo01Generic {
    public static void main(String[] args) {
        //定义集合
        ArrayList<String> strList = new ArrayList<>();
        //添加元素
        strList.add("hello");
        strList.add("world");
        strList.add("java");
        //调用printArrayList遍历
        printArrayList(strList);

        //定义集合
        ArrayList<Integer> intList = new ArrayList<>();
        intList.add(100);
        intList.add(200);
        intList.add(300);
        printArrayList(intList);

        //定义集合
        ArrayList<?> list = new ArrayList<>();
        //list.add("aa");
    }

    /*
        定义方法，可以遍历保存任何数据的集合。
     */
    public static void printArrayList(ArrayList<?> list) {
        for (Object obj : list) {
            System.out.println(obj);
        }
    }
```

#### 泛型限定

```java
/*
    泛型限定
        如果限制泛型通配符的取值范围，那么可以使用泛型限定。

    格式：
        <? extends A>：泛型要么是A类，要么是A类的子类。 上限。
        <? super A>： 泛型要么是A类，要么是A类的父类。 下限。

    泛型主要用于代码重构，代码设计。
    
 */
● 频繁往外读取内容的，适合用<? extends T>：extends返回值稍微精确些
● 经常往里插入的，适合用<? super T>：super允许存入子类型元素
```

![image-20210925104800936](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210925104800936.png)

### 数据结构

#### 栈

先进后出

#### 队列

先进先出

#### 数组

查询快,增删慢 

查询快:在内存中存储连续,根据数组首元素计算出其他元素地址值

增删慢:数组定长.要增删需要创建新的数组

#### 链表

链表一个节点至少包含

* 1.真正存储的数据
* 2.指向下个节点的地址

查询慢,增删快(增删快也是相对,数据量大时也要查的快些才能增删快)

查询慢:离散存储,要找某节点,必须先找到他上一个节点

增删快:如果要对链表增删,修改地址的值

### 异常

```java
/*
    异常指的是不正常，指的是程序出现了某些问题。

    在Java中所有的问题都可以使用一个类来表示，这个类叫做Throwable。
    Throwable是所有异常和错误的父类。

    Throwable
        |--Exception：异常。是程序中轻微的，可以挽回的问题。 相当于人得了感冒。
        |--Error： 错误。 是程序中严重的，不可挽回的问题。 相当于人得了绝症。
 */
```

![image-20200630213805313](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20200630213805313.png)

![](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20200630213844656.png)

#### throw关键字

```java
/*
    throw关键字

    作用：用来手动向外抛出异常。

    格式：
        throw new 异常类名();

    注意：
        使用throw向外抛出的一定要是一个异常对象。

    在创建异常对象时可以在构造方法中传递一个字符串，该字符串表示异常信息。


注意：
        1. 如果在方法内抛出了编译时异常，那么必须要使用throws进行声明
        2. 在调用使用throws声明异常的方法时，调用者方法如果不解决这个异常(try...catch)，必须也叫使用throws进行声明
        3. 如果在方法中抛出的是运行时异常，可以不使用throws进行使用。
        4. 如果方法中有可能抛出多个异常，那么我们需要使用throws进行多个异常的声明.
        5. 如果在方法中有可能抛出多个异常，那么我们也可以直接使用throws声明这些异常的父类异常.


*/
```

#### try catch

```java
/*
    如果想要解决掉异常，而不是将异常向外抛，可以使用try...catch语句(捕获处理)

    格式：
        try {
            可能出现异常的代码
        } catch(异常类名 变量名) {
            出现异常后执行的代码
        }

    如果try中引发的异常和catch小括号中写的异常类名相同，那么catch可以捕获到该异常。


    执行流程：
        第一种情况：try中的代码没有任何异常，那么代码会跳过catch继续向下执行。
        第二种情况：try中的代码有异常并且catch捕获到了这个异常，那么代码会从try直接跳到catch中。
        第三种情况：try中的代码有异常但是catch没有捕获到这个异常，这个异常会依旧向外抛。

    两种处理异常的方式
        向外抛（甩锅）：throw throws
        解决异常： try...catch

 */


//可以在try catch后追加finally代码块

    格式：
        try {
            【A】可能会出现异常的代码
        } catch(要捕获的异常类名 变量名) {
            【B】出现异常后执行的代码
        } finally {
            【C】一定会执行的代码
        }
    finally代码块的使用场景：
        一定要执行的代码放在finally代码块中，比如后期释放资源的操作(IO流中的关闭流，JDBC关闭连接)都是在finally代码块中执行的
 */

        
        
        多异常可以追加多个catch  注:父类异常不能放在子类异常的前面，因为父类异常的catch会拦截掉所有能捕获到的异常
        
        
 /*
        * 在继承关系中,方法重写时候,父类方法没有抛异常,子类方法也不能抛,遇到异常只能try catch 
        * 在方法重写时，如果父类方法抛出了异常，那么子类方法可以抛，也可以不抛。如果子类方法向外抛，要么抛出和父类方法异常一样的异常，要么抛出父类方法异常的子类异常(父类抛IOException,子类可以不抛,也可以抛IOException,或者抛IOException的子类异常)。
   		以上两点值适用于编译时异常
   */
```

#### 自定义异常

```java
/*
    自定义异常

    如何自定义异常：认贼作父

    如果我们定义的类继承的是编译时异常，那么这个类就是编译时异常。
    如果我们定义类继承的是运行时异常，那么这个类运行时异常。

    一般来说，定义编译时异常都会继承Exception，定义运行时异常都会继承RuntimeException
 */
```

### 线程

#### 进程:

正在内存中运行的应用程序就是进程!(任务管理器里就会显示当前进程)

#### 线程:

程序中的执行单元,每个线程都能执行一个任务(迅雷在下载东西,下载电影是一个线程,下载音乐是一个线程)..

如果一个程序中只有一个线程,那么这个程序就是**单线程程序**.

> 同时只能做一个事情

如果一个程序中有多个线程,那么这个程序就**多线程程序**.

> 同时可以做多个事情
>
> > 这个同时并不是真正意义同时,知识同一个时间点"一起"执行,由CPU调度,一个CPU只能调度一个线程,因为CPU会在多个线程之间非常快速切换,所以可以看成同时.
>
> 从根本上,多线程好处是提高CPU的资源利用率

​	**一个线程只能同时执行一个任务**

​	一个程序中**至少有一个**线程

​	

#### 并发和并行  

并发:同一个时间,多个线程一起执行(单个CPU)

并行:同一个时间,多个线程同时执行(多个CPU)



#### 多线程实现

```java
在Java中有一个类叫做Thread，这个类表示线程类，我们可以使用这个类完成多线程程序。

多线程的第一种实现方式：
    1. 定义类继承Thread
    2. 重写Thread中的run方法，并在run方法中定义线程要执行的任务。
    3. 创建Thread子类对象。
    4. 通过Thread子类对象调用start方法，启动线程，线程会执行自己的run方法。

Thread中的run方法：
    void start()：让线程执行，线程会执行自己的run方法。
```

##### 多线程第一种实现方式: 继承Thread类

```java
/*
    Thread中的常见方法

    构造方法：
        Thread()：空参数的构造方法。
        Thread(String name)：一个参数是字符串的构造方法，参数表示线程名字。

    其他方法：
        String getName()：获取线程名字。
        void setName(String name)：设置线程名字。
        static Thread currentThread()：获取当前正在执行的线程对象。
        static void sleep(long millis)：线程休眠，参数是休眠的毫秒值。


    Thread中的run方法：
        void start()：让线程执行，线程会执行自己的run方法。

 */
public class Demo02ThreadTest {
    public static void main(String[] args) {
        System.out.println("main...start");
        //创建Thread子类对象
        //表示创建了一个线程，只不过该线程并没有执行。
        MyThread m = new MyThread();
        MyThread m2 = new MyThread();
        MyThread m3 = new MyThread();
        //通过Thread子类对象调用start方法，启动线程，线程会执行自己的run方法。
        m.start();
        m2.start();
        m3.start();
        //输出100次HelloJava
        for (int i = 1; i <= 100; i++) {
            System.out.println("main线程在输出HelloJava：" + i);
        }
    }
}
```

```java
/*
    Thread是线程类，当MyThread继承Thread后，MyThread也就变成了线程类。
 */
public class MyThread extends Thread{
    //重写run方法，在run方法中定义线程要执行的任务（输出100次HelloWorld）
    @Override
    public void run() {
        for (int i = 1; i <= 100; i++) {
            System.out.println("新线程在输出HelloWorld：" + i);
        }
    }
}
```



##### 多线程第二种实现方式:实现Runnable接口

推荐使用第二种:

1. 解决了Java中类与类单继承的局限性
2. 降低了耦合性(关联性)
3. Runnable中只有一个run方法，没有getName，sleep，setName...，功能更加纯粹，我们只需要在里面关注线程要执行的任务。

 **实现步骤：**

1. 定义类，然后实现Runnable接口。
2. 重写Runnable接口中的run方法，在run方法中定义线程要执行的。
3. 创建Runnable实现类对象。
4. 创建Thread线程对象，并将Runnable实现类对象作为参数传递。
5. 调用线程对象的start方法，启动线程，线程会执行对应的run方法。

```java
/*
    在java中Thread类才表示线程类。
    Task这个类和Thread没有关系，所以Task并不是线程类。
    我们可以将Task看成线程任务类，因为该类只有一个run方法，在该类中只需要关注线程要执行的任务。
    
     *  1.start():启动当前线程,调用当前线程的run()
 *      2.run():通常需要重写Thread类中的此方法,将创建的线程要执行的操作声明在此方法中
 *      3.currentThread():静态方法,返回执行当前代码的线程
 *      4.getName():获取当前线程的名字
 *      5.setName():设置当前线程的名字
 *      6.yield():释放当前CPU执行权,this.yield()
 *      7.join():在线程a中调用线程b的join()方法,此时线程a处于阻塞状态,一直到线程b执行结束,线程a才解除阻塞
 *      8.stop():过时的方法,强制停止此线程
 *      9.sleep(long millitime):让当前线程"睡眠"指定的millitime毫秒,在指定的millitime毫秒时间内,当前线程是阻塞状态
 *      10.isAlive():判断当前线程是否存活(run方法执行完后就死亡)
 */
public class Demo01RunnableTest {
    public static void main(String[] args) {
        //创建Runnable实现类对象【task对象表示线程要执行的任务】
        Task task = new Task();
        //创建Thread线程对象，并将Runnable实现类对象作为参数传递。
        Thread t = new Thread(task);//表示将线程和task任务进行绑定，该线程要执行的任务是task中的run方法。
        //调用线程对象的start方法，启动线程，线程会执行对应的run方法。
        t.start();
        //输出100次HelloJava
        for (int i = 1; i <= 100; i++) {
            System.out.println("main线程在输出HelloJava：" + i);
        }

    }
}
```

```java
/**
	Task这个类和Thread没有关系，所以Task并不是线程类。
    我们可以将Task看成线程任务类，因为该类只有一个run方法，在该类中只需要关注线程要执行的任务。
*/
public class Task implements Runnable{
    //定义线程要执行的任务【输出100次HelloWorld】
    @Override
    public void run() {
        for(int i = 1; i <= 100; i++) {
            System.out.println(Thread.currentThread().getName() + "在输出HelloWorld:" + i);
        }
    }
}
```

##### 改为匿名内部类 

```java
public class Demo01InnerThread {
    public static void main(String[] args) {
        //创建Runnable的实现类对象【匿名内部类】
        Runnable r = new Runnable() {
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行了");
            }
        };
        //创建线程对象并启动
        new Thread(r).start();

        
       //----------实现Runnable的对象,只是匿名的  -------------------------
        //最终版
        //创建线程对象并启动
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行了");
            }
        }).start();
    }
}
```

##### 多线程第三种实现方式:实现Callable接口(jdk5.0之后) 更强大

 与使用 Runnable相比, Callable功能更强大些

>相比run()方法,重写call()方法可以有返回值
>方法可以抛出异常
>支持泛型的返回值
>需要借助Future接口里的FutureTask类,比如获取返回结果

```java
//假设CallableTest类实现了Callable接口,并重写了call方法
CallableTest ct = new CallableTest();
//需要借助FutureTask类,也继承了Runnable接口
FutureTask futureTask = new FutureTask(ct);
//5.将FutureTask对象作为参数传递到Thread类的构造器中,创建Thread对象,并调用start()
new Thread(futureTask).start();//因为futureTask共同实现了Runnable和Future接口

//这时就可以用futureTask去get到重写的call方法的返回值
futureTask.get();
```

##### 多线程第四种实现方式:使用线程池(用的最多)

```java
/*
* 创建线程的方式四:线程池(开发中更常用)

  提前创建好很多线程,放入线程池,使用时直接获取,用完放回线程池,可以避免频繁创建销毁
* 好处:
*      1.提高响应速度(减少创建新线程的时间)
*      2.降低资源消耗(重复利用线程池中线程,不需要每次都创建)
*      3.便于线程管理
*      corePoosize:核心池的大小
*      maximumPoolSize:最大线程数
*      keepAliveTime:线程没有任务时最多保持多长时间后会终止
*/
ExecutorService 线程池接口,常见子类ThreadPoolExecutor

Executors:工具类,线程池的工厂类,创建并返回不同类型的线程池
```

```java
		//提供指定线程数量的线程池
		ExecutorService executorService = Executors.newFixedThreadPool(10);


        /*
        //设置线程池的属性
        //
        //对executorService进行强转 ThreadPoolExecutor间接实现了ExecutorService接口
        */
		System.out.println(executorService.getClass());//看这个对象是哪个类造的
        ThreadPoolExecutor executorService1 = (ThreadPoolExecutor) executorService;
        executorService1.setCorePoolSize(15);//设置核心池的大小
        executorService1.setMaximumPoolSize(10);//设置最大线程数
         

		//2.执行指定的线程操作,需要提供实现Runnable接口或Callable接口的对象
		//只是造了个池子,具体干什么要写在实现类里,可用用Runnable或者Callable 
		//这里的NumThread1和NumThread2都实现了Runnable接口并重写了run方法
		executorService.execute(new NumThread1());//适用于Runnable  
		executorService.execute(new NumThread2());
		//executorService.submit();//适用于Callable
		executorService.shutdown();//不用了可以关闭线程池
```





#### 线程安全问题的解决

##### JDK5之前:

1. 同步代码块

```java
synchronized(同步监视器){

	//需要被同步的代码
}
/*
1.操作共享数据的代码,就是需要被同步的代码
2.共享数据:多个线程共同操作的变量,比如:厕所的坑位 , 卖的票
3.同步监视器:俗称锁  任何一个类的对象都能充当锁  要求多个线程必须共用一把锁(拿到锁,释放锁给另个线程争抢)
*/
**实现runnable接口的时候,同步监视器可以为this,因为只new了一个对象,this调用的就是当前类的对象,用当前对象充当监视器,因为监视器需要"唯一"
    注意观察两种创建线程的方式
    假设Task实现了Runnable接口
    Task task = new Task(); //自始至终值new了一个对象
    Thread t = new Thread(task);
	Thread t2 = new Thread(task);//表示将线程和task任务进行绑定，该线程要执行的任务是task中的run方法。
	t.start();
	t2.start();
------------------------------------
**继承Thread类时候的同步监视器不能为this,因为监视器需要"唯一"
    假设MyThead类继承了Thread类
    MyThread m = new MyThread();
    MyThread m2 = new MyThread();//new 了两个对象
	m.start();
	m2.start();
```

> 好处:解决了线程安全问题
>
> 坏处:在同步代码块中只有一个线程参与,其他线程等待,效率低

	2. 同步方法

> 操作共享数据的代码完整的声明在一个方法中,就可以**把这个方法声明成同步**的
>
> 同步方法仍然涉及到**同步监视器**，只是不需要显示声明
>
> * 使用实现Runnable方式，抽取需要被同步的代码为一个方法，给方法返回值类型前加 synchronized 关键字 ，这时的同步监视器是this
> * 使用继承Thread方式，抽取需要被同步的代码为一个方法，给方法返回值类型前加 synchronized 关键字 （这时同步监视器为new的三个MyThread ），错误！ 需要给这个方法前在加上static；这时的**同步监视器是当前的类**（当前类是唯一，所以可以被当成同步监视器）

3. Lock锁 jdk5.0新增  

> Lock是一个接口,要用他的实现类ReentrantLock

```java
private ReetrantLock lock = new ReetrantLock();//括号里可以写true,(fire:true),表示公平,一个线程拿到锁,就轮到另一个了

//使用
//给需要同步的代码上方法调用,也可以(理解给这个代码加了synchronized,但不是)
lock.lock();
//把这段需要同步的代码用try finally 包裹起来
在finally中调用解锁
    lock.unlock(); 
```

##### synchronized和Lock对比

* 相同:都可以解决线程安全问题
* 不同
  * Lock需要手动锁定和手动解锁，synchronized隐式锁，除了作用域自动释放同步监视器
  * Lock只有代码块锁，synchronized有代码块锁和方法锁
  * 使用Lock锁，JVM花费较少时间调度线程

##### synchronized和volatile区别?

##### 单例模式之饿汉式(线程安全,一开始就初始化)   

单例模式:**一个类的实例在多线程环境下只会被创建一次**;

> 注意:
>
> 1. 单例类只能有一个实例
> 2. 单例类必须自己创建自己的唯一实例
> 3. 单例类必须给所有其他对象提供这一实例

```java
public class EagerSingleton{
   
    private static EagerSingleton instance = new EagerSingleton();//一开始就初始化,因为是私有,所有这段代码只执行一次,只创建一个EagerSingleton对象  
    private EagerSingleton(){}//构造方法私有,防止外界使用new关键字创建对象;
    public static EagerSingleton getInstance(){//提供静态属性,并获取对象
        return instance;
    }
}
```

##### 单例模式之懒汉式(线程不安全,但可以改成线程安全) 线程安全后是双检索

```java
public class LazySingleton{
    private static LazySingleton instance;//声明一个静态实例
    private LazySingleton(){}//空参私有构造
    public static LazySingleton getInstance(){
    	if(instance == null){
            //判断实例是空才初始化
            instance = new LazySingleton();
        }
        return instance;
    }
    
}


//-----改为线程安全------单例模式之双检索----------------

public class LazySingletonSafe{
    private volatile static LazySingletonSafe instance;//声明一个静态实例
    private LazySingletonSafe(){}//空参构造
    public static LazySingletonSafe getInstance(){
        //这里直接设置判断,后面的线程进来如果instance不为空,就不会进入同步代码快,提高了效率
        if(instance == null){
            //同步代码块,加锁使线程安全
            synchronized(LazySingletonSafe.class){//类作为同步监视器
                //判断实例是否是空
                if(instance == null){
                    instance = new LazySingletonSafe();
                }
            }
        }
        return instance;
    }
} 
```



#### 线程死锁

> 不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，就形成线程死锁
>
> 死锁状态，没有提示和异常，知识所有线程都在阻塞状态，无法继续

##### 线程生命周期

![image-20210923114015583](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210923114015583.png)

##### 解决/避免线程死锁

* 专门的算法
* 尽量减少同步资源的定义
* 尽量避免**嵌套**同步

#### 线程之间的通信

利用wait()和notify();来让这个线程完成后另一个线程开始,

* wait(),会让当前线程进入阻塞状态，同时会释放同步监视器(锁) 
* notify(),唤醒被wait的**一个**线程,如果多个,唤醒优先级高的
* notifyAll(),唤醒所有被wait的线程

> 假设两个线程，1进入同步代码块拿到当前锁，执行wait()，并释放锁；这时线程2拿到被1释放的锁进入同步代码块，执行notify()方法，唤醒被阻塞的1线程，2又执行wait()，释放锁，被1拿到，1执行notify()，唤醒线程被阻塞的线程2.......这时两个线程就可以交替执行，形成“通信

##### 注意

* 以上三种**必须使用在同步代码块/同步方法**中
* 三个方法的调用者必须是同步代码块/同步方法中的监视器 synchronized(this),也就是这个this,或者自己定义同步监视器
* 以上三个方法定义在obj类中,既然任何对象都可以作为同步监视器，并且都要用同步监视器去调用这三个方法，所以这三个方法被定义在Object类中，以满足**任何对象的调用**

##### sleep方法和wait方法的对比

* 相同：一旦执行这两个方法，都会让线程进入阻塞状态
* 不同：
  1. 声明位置不同：sleep在Thread类中声明，wait在Object类中声明
  2. 调用要求不同：sleep可在任何需要时候使用，wait必须在同步代码块中/同步方法使用
  3. 若两者都出现在同步方法/同步代码块中,sleep不会释放锁，wait会释放

### Lambda表达式(jdk1.8新)

Lambda表达式使用的是函数式编程思想

>   面向对象思想：怎么做。
>   函数式编程思想：做什么

```java
/*

例如上方匿名内部类创建线程时很多地方冗余

 在整个匿名内部类中最重要的是方法的前中后三点。
        前：方法参数
        中：方法体
        后：返回值。
*/
public class Demo01Inner {
    public static void main(String[] args) {
        //使用匿名内部类的方式完成多线程。
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行了");
            }
        }).start();

        //Lambda表达式初体验 简写省略{;}
        new Thread(() -> System.out.println(Thread.currentThread().getName()  + "执行了")).start();
        
        //标准格式
        //使用Lambda表达式完成多线程
        new Thread(() -> {
            System.out.println(Thread.currentThread().getName() + "执行了");
        }).start();
    }
}
```

#### 优点

使用Lambda表达式，可以只关注最核心的内容，可以解决匿名内部类的冗余 

#### 格式

    Lambda标准格式：
        (参数类型 参数名) -> {
            方法体;
            return 返回值;
        } 

#### 使用前提

1. 必须要有接口（不能是抽象类），接口中有且仅有一个需要被重写的抽象方法。（比如Runnable或Comparator）

2. 必须支持上下文推导。 要能够推导出来Lambda表达式表示的是哪个接口中的哪个方法。最常用的上下文推导的方式是使用函数式接口当做方法参数，然后传递Lambda表达式。

   > 如果一个接口中有且仅有一个需要被重写的抽象方法，那么该接口也叫做函数式接口。



### File

```java
/*
    构造方法：
        File(String pathname)：根据文件或文件夹的路径创建一个File对象
        File(String parent, String child)：根据父路径和子路径创建File对象。
 */
 //在Java中\是特殊的字符，叫做转义字符，表示将后面的字符改变含义。
//如果要表示一个普通的反斜线，需要写两个\\
File file = new File("d:\\iotest\\aa.txt");
File file2 = new File("d:\\iotest", "aa.txt");
//-----------------------------------------------------

相对路径:简单路径,不从盘符开始,指当前项目
绝对路径:详细路径,从盘符开始
//根据相对路径创建File对象，该File对象表示当前项目下的aa.txt
File file = new File("aa.txt");

//------------------------------------------------------
/*
		String getAbsolutePath()：获取File对象所表示的文件或文件夹的绝对路径。
        String getPath()：获取路径。
        String getName()：获取文件或文件夹的名字
        long length()：获取文件的字节数大小。只能对文件使用，如果File对象表示的是文件夹，调用length方法，结果是不确定的。
        //------------------------
        
         boolean exists()：判断File对象表示的文件或文件夹是否存在。
        boolean isFile()：判断File对象表示的是否是文件。
        boolean isDirectory()：判断File对象表示的是否是文件夹。
        //------------------------
        
        boolean createNewFile()：创建文件，如果文件已经存在，那么创建失败。
        boolean mkdir()：创建文件夹，如果文件夹已经存在，那么创建失败。上级不存在创建失败,但不报错
        boolean mkdirs()：创建文件夹，如果文件夹已经存在，那么创建失败。上级不存在创建上级
        
        boolean delete()：删除File对象表示的文件或文件夹。必须是空文件夹,如果有内容先删除文件再删文件夹
      	【使用代码删除的文件夹不走回收站】
*/
//----------------------------------------------------
/*
    	File中用于遍历的方法
       （了解）String[] list()：获取指定目录下所有文件和文件夹的名字并放入到字符串数组中返回。
        File[] listFiles()：获取指定目录下所有的文件和文件夹并放入到File数组中返回。
        
      	调用前判断File对象表示的文件夹是否存在
*/

```

### 递归

#### 概念

递归指的是方法调用方法自己

里面包含了隐式的循环,重复某段代码,只是无需循环控制

#### 注意

* 方法必须有出口(有结束条件,否则会无限循环)
* 递归次数不能太多

#### 用处

用于未知层级的场景,比如遍历文件夹,求一算法等等

```java
public class RecursiveTest {
    public static void main(String[] args) {
        //f(0) = 1  f(1)= 4   f(n+2) = 2*f(n+1)+f(n),求f(10)
        int n = 10;
        RecursiveTest recursiveTest = new RecursiveTest();
        System.out.println(recursiveTest.f(10));

        File file = new File("J:\\书\\docs");
        recursiveTest.printFileName(file);
    }
    //思路递归调用
    public int f(int n){
        if(n==0){
            return  1;
        }else if (n == 1){
            return 4;
        }else{
            //要保证最后的1和4能用上,所以用递归一直减下去判断
            return 2*f(n-1)+f(n-2);
        }
    }

    //输出这个文件夹内所有文件名,递归调用
    public void printFileName(File dir){
        int count = 0;
        File[] files = dir.listFiles();
        //遍历这个文件夹
        for (File file : files) {
            //如果是个文件夹
            if (file.isDirectory()) {
                //那么就继续遍历,直到是个文件为止
                printFileName(file);
            }else{
                //直到是个文件为输出
                System.out.println(file);
                count++;
            }
        }
        System.out.println("一共"+count+"个记录数");

    }

}
```



### 流

`流是什么？当不同的介质之间有数据交互的时候，JAVA就使用流来实现`

#### 流有哪些分类？

可以从不同的角度对流进行分类：

1. 处理的数据单位不同，可分为：字符流，字节流

2. 数据流方向不同，可分为：输入流，输出流

3. 功能不同，可分为：节点流，处理流

​    1和 2 都比较好理解，对于根据功能分类的，可以这么理解：

##### **节点流**：

节点流从一个特定的数据源读写数据。即节点流是直接操作文件，网络等的流，例如FileInputStream和FileOutputStream，他们直接从文件中读取或往文件中写入字节流。

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/2012121818051872.png)

##### **处理流**：

“连接”在已存在的流（节点流或处理流）之上通过对数据的处理为程序提供更为强大的读写功能。过滤流是使用一个已经存在的输入流或输出流连接创建的，过滤流就是对节点流进行一系列的包装。例如BufferedInputStream和BufferedOutputStream，使用已经存在的节点流来构造，提供带缓冲的读写，提高了读写的效率，以及DataInputStream和DataOutputStream，使用已经存在的节点流来构造，提供了读写Java中的基本数据类型的功能。他们都属于过滤流。

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/2012121818440350.png)

## **流结构介绍**：

Java所有的流类位于java.io包中，都分别继承字以下四种抽象流类型。

|        | 字节流       | 字符流 |
| ------ | ------------ | ------ |
| 输入流 | InputStream  | Reader |
| 输出流 | OutputStream | Writer |

1.继承自InputStream/OutputStream的流都是用于向程序中输入/输出数据，且数据的单位都是字节(byte=8bit)，如图，深色的为节点流，浅色的为处理流。

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/2012121818562293.png) ![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/2012121819001442.png)

2.继承自Reader/Writer的流都是用于向程序中输入/输出数据，且数据的单位都是字符(2byte=16bit)，如图，深色的为节点流，浅色的为处理流。

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/2012121819033620.png)

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/2012121819042121.png)

#### 常见流类介绍：

##### **节点流类型常见的有：**

对文件操作的字符流有FileReader/FileWriter，字节流有FileInputStream/FileOutputStream。

##### **处理流类型常见的有：**

**缓冲流**：缓冲流要“套接”在相应的节点流之上，对读写的数据提供了缓冲的功能，提高了读写效率，同时增加了一些新的方法。

　　字节缓冲流有BufferedInputStream/BufferedOutputStream，字符缓冲流有BufferedReader/BufferedWriter，字符缓冲流分别提供了读取和写入一行的方法ReadLine和NewLine方法。

　　对于输出地缓冲流，写出的数据，会先写入到内存中，再使用flush方法将内存中的数据刷到硬盘。所以，在使用字符缓冲流的时候，一定要先flush，然后再close，避免数据丢失。

**转换流**：用于字节数据到字符数据之间的转换。

　　仅有字符流InputStreamReader/OutputStreamWriter。其中，InputStreamReader需要与InputStream“套接”，OutputStreamWriter需要与OutputStream“套接”。

数据流：提供了读写Java中的基本数据类型的功能。

　　DataInputStream和DataOutputStream分别继承自InputStream和OutputStream，需要“套接”在InputStream和OutputStream类型的节点流之上。

对象流：用于直接将对象写入写出。

　　流类有ObjectInputStream和ObjectOutputStream，本身这两个方法没什么，但是其要写出的对象有要求，该对象必须实现Serializable接口，来声明其是可以序列化的。否则，不能用对象流读写。

　　还有一个关键字比较重要，transient，由于修饰实现了Serializable接口的类内的属性，被该修饰符修饰的属性，在以对象流的方式输出的时候，该字段会被忽略。



### Stream

Stream流不是一种数据结构，不保存数据，它只是在原数据集上定义了一组操作。

这些操作是惰性的，即每当访问到流中的一个元素，才会在此元素上执行这一系列操作。

Stream<T>是一个接口，该接口表示流。

#### 获取流的两种方式

1. 通过Collection集合（单列集合）调用stream()方法获取。【根据单列集合获取】

```java
public class Demo02CollectionGetStream {
    public static void main(String[] args) {
        //创建集合
        List<String> list = new ArrayList<>();
        //添加元素
        list.add("hello");
        list.add("world");
        list.add("java");
        //获取集合对应的流
        Stream<String> stream = list.stream();

        //通过流调用toArray将流转成数组，然后通过Arrays工具类将数组中的内容转成字符串，然后通过外面的输出语句输出。
        System.out.println(Arrays.toString(stream.toArray()));
    }
}
```

2. 通过Stream中的静态方法根据数组获取流。【根据数组获取】

```java
public class Demo03ArrayGetStream {
    public static void main(String[] args) {
        //创建数组
        String[] strArr = {"hello", "world", "java"};
        //通过Stream调用of方法，根据数组获取流
        //Stream<String> stream = Stream.of(strArr);
        //of方法不仅可以根据数组获取流，也可以根据多个元素获取流
        Stream<String> stream = Stream.of("你好", "我好", "大家好");
        //输出流中的内容
        System.out.println(Arrays.toString(stream.toArray()));
    }
}
```

#### Stream里的方法

##### forEach 

可以对流中的元素进行逐一处理，逐一操作

##### filter

可以对流中的元素进行过滤筛选

##### count

可以获取流中元素的个数

##### limit

可以获取流中的前几个元素

##### concat

合并两个流

#### 注意

1. 流调用非终结方法返回值都是Stream本身类型，但是返回的并不是自身的对象，返回的结果是一个新的流
2. Stream流不保存数据，故只能一次性使用，不能多次使用。

### IO流  

#### 注意

* 用完要关闭流,否则转为流的文件一直处于被占用状态

* 缓冲流本身并不具备读或者写的功能，缓冲流的主要作用是对其他流进行加速，因为缓冲流内部有一个数组作为了缓冲区，所以可以加速

![1596543893803](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1596543893803.png)

### Properties

#### 基本使用

properties是双列集合,叫做属性集

#### 特点

1.  Properties实现**Map**接口，拥有Map接口中的所有的方法。
2.  Properties没有泛型，里面的键和值都是字符串。
3.  Properties支持和流一起操作，可以从流中加载键值对【从文件中读取键值对】

#### 方法

```java
    Object setProperty(String key, String value)：添加键值对元素。
    (记住)String getProperty(String key)：根据键获取值。双列集合遍历方式之一用键来获取值
    Set<String> stringPropertyNames()：获取Properties中所有的键并放入到Set集合中返回。
```

##### 从文件读取键值对

load方法可以将文件中的键值对读取【加载】到Properties中

```java
void load(InputStream inStream)：参数要传递字节输入流
void load(Reader reader)：参数要传递字符输入流
```

properties文件中的键值对按照 `键=值` 格式保存必须

### 转换流

#### 编码和解码

编码：字符 -> 字节

解码；字节 -> 字符

#### 编码表

GBK： 		   1/2字节          支持两万多汉字或者字符。 每个汉字占2个字节

万国码：UTF-8  每一个字符都使用1/2/3/4个字节保存  每个汉字占3个字节

存emoji表情 编码utf-8 mb4

**可以在流指定编码读/写的方式**

### 序列化

#### 序列化与反序列化

序列化指把Java程序的对象写到文件中，需要使用序列化流ObjectOutputStream操作

反序列化是吧文件的对象读取到Java程序中，需要使用反序列化流ObjectinputStream操作



![1596635907036](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1596635907036.png)



##### 方法

ObjectOutputStream是序列化流，可以将Java程序中的对象写到文件中。 

```java
//序列化 
void writeObject(Object obj)//将对象写到文件。ObjectOutputStream写对象的特有方法 
//反序列化     
 Object readObject()//从文件中读取对象。ObjectInputStream读取对象的方法【特有方法】：
```

**向文件中写的对象必须要实现Serializable接口**

>   1. 被**static修饰的属性不能被序列化**。被static修饰的属性属于类，不属于对象，而序列化操作写的是对象
>   2. 如果我们不希望某个属性被序列化，同时不希望使用static关键字，那么可以使用**transient**。
>      transient表示瞬态，被transient修饰的属性无法被序列化。

#### 序列号

```java
/*
从文件中读取对象，当从文件中读取对象时，会对比文件中保存的版本号和class文件中的版本号是否一致，如果不一致，就会报错。如果要解决版本号冲突问题，可以给类固定一个版本号，不管该类怎么修改，版本号都不变。这样就解决了问题。

    我们可以在类中提供一个常量，该常量表示类的版本号。
  */
    //给类固定一个版本号，不管这个类怎么修改，版本号(序列号)永远是1
    private static final long serialVersionUID = 1L;
```

####  commons-io

是由第三方（Apache）提供的IO流操作的工具包

### JUnit单元测试

#### 概念

junit叫做单元测试，单元测试用来代替程序中的main方法，程序可以直接从单元测试方法开始运行。

如果在一个方法上加上**@Test**注解，那么这个方法就是一个单元测试方法了，程序可以直接从该方法开始运行。

#### 要求

1. 方法一般以test开头（软性规定）
2. public修饰，没有参数，没有返回值（硬性规定）

#### 单元测试其他注解

```java
/*
	单元测试中的其他注解（JUnit4）还有JUnit5没写
        Before：使用该注解修饰的方法，会在每次单元测试方法执行之前执行。
        After：使用该注解修饰的方法，会在每次单元测试方法执行之后执行。
        BeforeClass：使用该注解修饰的方法，会在所有方法执行之前执行，只执行一次。
        AfterClass：使用该注解修饰的方法，会在所有方法执行之后执行，只执行一次。
        注意：使用BeforeClass和AfterClass修饰的方法必须加静态。
*/
```

#### 断言

```java
/*
    断言：可以判断结果是否是预期值，如果是，程序正常，如果不是，程序会崩溃
        Assert.assertEquals(期望结果 , 实际结果);
 */
public class Demo03JUnit {
    @Test
    public void testMethod() {
        //求两个变量的和
        int a = 10;
        int b = 20;
        int sum = a + b;
        //使用断言判断这个结果是否是30
        Assert.assertEquals(50, sum);
    }
}
```

### 网络

#### IP地址

IP地址是网络中对一个设备的**唯一**标识(编号)

>IPv4
>在IPv4中,每个IP地址都是使用4个字节表示的。
>00000000 00000000 00000000 00000000
>XXX. XXX. XXX. XXX
>xxx的取值范围是0到255

#### 端口号

在计算机中对应用程序的唯一标识，要定位到某个计算机的某个应用程序需要IP地址和端口号

> 每一个程序都有端口号，范围0-65535，其中1024 （0-1023）大多已经被系统使用了，不建议使用
>
> 常用端口号
>
> tomcat：8080
>
> mysql： 3306
>
> http： 80
>
> https： 443

#### 客户端和服务器

![1596722687356](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1596722687356-1632391099839.png)

### InetAddress

```java
/*
    InetAddress表示IP地址

    方法：
        static InetAddress getByName(String host)：获取目标机器的IP地址对象。参数是IP地址或者主机名
        String getHostName()：获取此IP地址的主机名
        String getHostAddress()：返回文本显示中的IP地址字符串

 */
public class Demo05Test {
    public static void main(String[] args) throws UnknownHostException {

        InetAddress inetAddress = InetAddress.getByName("192.168.128.139");
        System.out.println(inetAddress.getHostName());
        System.out.println(inetAddress.getHostAddress());
    }
}

```



#### TCP中的IO

TCP使用IO流完成数据的发送和接收

##### 发送:

OutputStream

##### 接收:

InputStream

#### TCP通信

通过Socket指定的服务ip和端口得到socket对象，调用getOutputStream获取输出流，给服务器发送数据

#### Socket 套接字? 插座!!!

`在现实世界里把电器连接到插座上实现电流的传输，在计算机网络里，把客户端连接到服务器的socket就实现了数据流的传输！！！`

##### TCP通信的客户端  | Socket

在Java中有一个类叫做Socket，这个类就表示TCP的客户端程序。

```java
Socket(String host, int port)//参数host表示目标服务器的ip地址。 参数port表示目标服务器程序的端口号。
    
//通过Socket获取到的流不需要我们手动关闭，我们只需要关闭Socket即可
void close()//释放资源
    
        TCP客户端的实现步骤：
        1. 创建客户端Socket对象，指定要连接的服务器。
        2. 通过Socket调用getOutputStream获取输出流。
        3. 使用获取到的输出流给服务器发送数据。
        4. 通过Socket调用getInputStream获取输入流。
        5. 使用获取的输入流接收服务器发送过来的数据
        6. 释放资源
    

public class Demo01Client {
    public static void main(String[] args) throws IOException {
        //1. 创建客户端Socket对象，指定要连接的服务器。
        //创建客户端Socket对象时，会主动连接服务器，如果连接不成功，就会报错（三次握手）
        Socket socket = new Socket("127.0.0.1", 9527);
        //2. 通过Socket调用getOutputStream获取输出流。
        OutputStream out = socket.getOutputStream();
        //3. 使用获取到的输出流给服务器发送(写)数据。
        out.write("你好".getBytes());
        //4. 通过Socket调用getInputStream获取输入流。
        InputStream in = socket.getInputStream();
        //5. 使用获取的输入流接收(读取)服务器发送过来的数据
        byte[] bArr = new byte[1024];
        int len = in.read(bArr);
        System.out.println(new String(bArr, 0, len));
        //6. 释放资源
        socket.close();

    }
}
```

##### TCP案例服务端 | ServerSocket

在Java中有一个类叫做ServerSocket，这个类表示TCP的服务器端。

```java
/*
    TCP案例的服务器端

    在Java中有一个类叫做ServerSocket，这个类表示TCP的服务器端。

    ServerSocket构造方法：
        ServerSocket(int port)：参数要传递端口号，该端口号表示本服务器的端口号。

    ServerSocket其他方法：
        Socket accept()：监听并获取客户端Socket(请求)

    TCP服务器的实现步骤：
        1. 创建服务器ServerSocket对象，指定自己的端口号。
        2. 通过ServerSocket调用accept （同意）方法，监听并获取客户端Socket
        3. 通过Socket调用getInputStream，获取输入流（该输入流的数据源是客户端）
        4. 通过输入流接收客户端发送过来的数据。
        5. 通过Socket调用getOutputStream，获取输出流（该输出流的目的地是服务器）
        6. 通过输出流给客户端发送数据。
        7. 释放资源


 */
public class Demo02Server {
    public static void main(String[] args) throws IOException {
        //1. 创建服务器ServerSocket对象，指定自己的端口号。
        ServerSocket serverSocket = new ServerSocket(9527);
        //2. 通过ServerSocket调用accept方法，监听并获取客户端Socket
        Socket socket = serverSocket.accept();
        //3. 通过Socket调用getInputStream，获取输入流（该输入流的数据源是客户端）
        InputStream in = socket.getInputStream();
        //4. 通过输入流接收(读取)客户端发送过来的数据。
        byte[] bArr = new byte[1024];
        int len = in.read(bArr);
        System.out.println(new String(bArr, 0, len));
        //5. 通过Socket调用getOutputStream，获取输出流（该输出流的目的地是服务器）
        OutputStream out = socket.getOutputStream();
        //6. 通过输出流给客户端发送数据。
        out.write("收到".getBytes());
        //7. 释放资源
        socket.close();
        serverSocket.close();
    }
}
```

#### 上传图片图示

![1596722849155](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1596722849155.png)

在Java中有一个类叫做UUID，里面的静态方法randomUUID可以获取一个随机的，不重复的字符序列

UUID.randomUUID().toString() 可以用来服务器给自己电脑写数据时命名操作

#### TCP上传案例（多线程服务器）

```java
/*
    我们之前的上传服务器还有一些缺陷，如果有一个客户端上传了非常大的文件，服务器会一直给这个客户端执行上传任务，那么就无法
    监听其他客户端的请求了。

    原因：在程序中只有一个main线程，当程序启动时，main线程会执行，main线程会监听客户端的请求，如果有客户端来连接了，那么main线程
          还会给该客户端执行上传任务，如果上传文件非常大，那么main线程会一直执行该上传任务，那么就无法继续监听后面的客户端请求了。

    解决方式：使用多线程，让main线程监听客户端的请求，如果有客户端来连接了，那么就新建一个线程执行上传任务。那么新线程执行上传
             任务的同时不会影响到main线程的，main线程可以继续向下执行，监听其他客户端的请求。
 */
@SuppressWarnings("all")
public class Demo04ThreadServer {
    public static void main(String[] args) throws IOException {
        //1. 创建服务器ServerSocket对象
        ServerSocket serverSocket = new ServerSocket(9527);
        //要在死循环中监听客户端请求以及完成上传任务
        while(true) {
            //2. 调用accept方法，监听客户端的请求。
            Socket socket = serverSocket.accept();
            //新建一个线程，执行上传任务
            new Thread(() -> {
                try {
                    //3. 通过Socket对象获取输入流，用来从客户端读取数据。
                    InputStream in = socket.getInputStream();
                    //4. 创建字节输出流，用来向服务器自己电脑写数据。
                    OutputStream os = new FileOutputStream("d:\\server\\" + UUID.randomUUID().toString() + ".jpg");
                    //5. 开始读写，每读取一次，就将读取到的数据写到服务器自己的电脑。
                    byte[] bArr = new byte[1024];
                    int len;
                    while ((len = in.read(bArr)) != -1) {
                        //如果条件成立表示读取到了数据，就将读取到的数据写到服务器自己电脑。
                        os.write(bArr, 0, len);
                    }
                    //6. 释放资源
                    os.close();
                    //7. 通过Socket对象获取输出流，用来给客户端发送数据。
                    OutputStream out = socket.getOutputStream();
                    //8. 通过输出流调用write方法，发送数据。
                    out.write("上传成功".getBytes());
                    //9. 释放资源。
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }).start();
        }

    }
}

```

### TCP/IP协议

#### 三次握手

1. 客户端给服务端发送syn(synchronized sequence numbers同步序列编号)

   > 客户端的发送能力，服务端的接收能力正常。

2. 服务端收到后返回syn+ack

   > 服务端的接收和发送能力，客户端的接收和发送能力正常。但 是此时服务端不能确认客户端的接收能力是否正常

3. 客户端收到syn+ack,回应服务端ack

   > 客户端的接收和发送能力，自己的接收发送能力都正常。 通过三次握手，双方都确认对方的接收以及发送能力正常

![d8f9d72a6059252d20d93b0a6645fb3e59b5b9d2](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/d8f9d72a6059252d20d93b0a6645fb3e59b5b9d2.png)



三次握手后,socket连接了



#### 四次挥手

1. 客户端给服务端发送fin请求
2. 服务端收到请求给客户端回了fin+ack
3. 服务端给客户端发送fin表分手
4. 客户端给服务端再说了一句,表示收到你的分手请求,可以分手

![48540923dd54564e5260495ce0006487d0584fb6](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/48540923dd54564e5260495ce0006487d0584fb6.png)

### BIO NIO AIO

![NIOBIOAIO](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/NIOBIOAIO.png)

BIO底层是IO流

> 同步且阻塞,服务器实现模式为一个连接一个线程,即客户端有连接请求时服务端就需要启动一个线程进行处理,如果这个连接不做任何事情就会造成不必要的线程开销

NIO支持面向缓冲区的基于通道的IO操作,以更加高效的方式进行文件的读写操作

> 同步非阻塞,服务器实现模式为一个请求一个线程,即客户端发送的连接请求注册到多路复用器上,多路复用器轮询到连接有I/O请求时候才启动一个线程进行处理.用户进程需要时不时询问IO操作是否就绪

AIO

> 异步非阻塞,在此种模式下,用户进程只需要发起—个IO操作然后立即返回,等IO操作真正的完成以后,应用程序会得到IO操作完成的通知,此时用户进程只需要对数据进行处理就好了,不需要进行实际的IO读写操作,因为真正的IO读取或者写入操作已经由内核完成了
>
> 基于时间回调机制

Buffer 缓冲器     Channel 通道   Selector 选择器

#### 同步和异步（线程通信的机制）

​        同步：线程在完成某个功能时，得到结果之后才能做后面的事情【立即得到结果】
​        异步：线程在完成功能的时候，不用得到结果也可以做后面的事情【不会立即得到结果】

#### 阻塞和非阻塞（线程的状态）

​        阻塞：线程在执行任务时，会挂起。
​        非阻塞：线程在执行任务时，不会挂起，可以继续执行其他任务。

### 反射

#### 概念

Java中的反射是一种机制，利用这种机制可以获取到类中的成员变量，成员方法，构造方法，并且进行使用

>  反射就是换了一种形式去创建对象以及调用方法。

去看[类加载器](#类加载器ClassLoader(抽象类) )

#### 工作中的目的

* 创建实例
* 反射调用方法

#### 获取Class的三种方式

```java
//其他代码省略
//1. 通过对象调用getClass方法获取
Person p = new Person();
Class clazz = p.getClass();//获取Person类的class对象
//2. 通过类的class属性获取(最简单)
Class clazz2 = Person.class;//获取Person类的class对象
//3.通过Class类的静态方法forName获取(最常用,最灵活)
Class clazz3 = Class.forName("com.gorilla.demo.Person");//里面使用类加载器去加载.class文件
```

#### Class中的常见方法

```java
//获取类的全限定类名(包含包的类名)
String name = clazz.getName();//输出com.gorilla.demo.Person
//获取类的简单类名(不包含包的类名)
String simpleName = clazz.getSimpleName();//输出Person
//-----------------------------------------
//获取到Person类的所有构造方法
Constructor[] c = clazz.getConstructors();//获取到类中所有的构造方法
//获取到Person类的"空参"构造方法
Constructor c  = clazz.getConstructor();//获取类中的无参构造方法
Object obj = c.newInstance();//使用构造方法创建对象 相当于 new Person();
//----------------------------------
//获取到Person类的指定"有参"构造方法
Constructor c2 = clazz.getConstructor(Class<?>... parameterTypes);//这里参数是String.class , int.class 参数类型的类
Object obj = c2.newInstance(Object ... initargs)//这里传的值是"获取到的有参构造方法里面需要的参数"  //相当于 new Person("张三",18);
    
 //-----------------------------------
 //反射创建对象的简便形式 class类中的newInstance(),使用空参构造方法对象
  Object obj = clazz.newInstance();


//----------------------------
//Method getMethod(String methodname,Class...parameterTypes)取到类中指定的成员方法。参数name表示要获取的方法名，参数parameterTypes表示要获取的成员方法的参数列表。 
Method method = clazz.getMethod("eat",String.class); //调用名为eat的成员方法
同样的Method[] .getMethods()//获取所有的public方法
.getFiled(String name);//同理只能获取public修饰的方法
    
//Method类中有invoke(Object obj,Object... args)方法
//invoke(第一个参数全参或者无惨构造new Person/new Person("XX",15)也就是通过反射得到的new instance(),第二个参数代表调用这个方法时传递的参数)表示让该方法执行

    // 以下代码相当于 new Person("**",15).eat("吃")
method.invoke(obj,"吃");//名为Person类中名为eat的方法,被obj对象执行,并且给这个方法传递了一个"吃"的String类型的参数  如果调用的method没有返回值,可以不写第二个参数

getGenericSuperclass();//会返回泛型父类的Class对象  -->得到 ParameterizedTypeImpl -->向下强转(ParameterizedType).getActualTypeArguments() -->得到泛型的实际参数

getAnnotation(Class<T> annotationClass)//获取指定注释类型的注释
```

>  //反射创建对象的简便形式 class类中的newInstance(),使用空参构造方法对象
>  Object obj = clazz.newInstance();
>
>  这里的**newInstance()底层调用的是Constructor(构造类)里的Constructor.newInstance(),**要用Class对象创建实例,是通过构造器对象的,也就是说被反射的类对象在创建时候必须要有**空参构造**



> 1.为什么getMethod("方法名",方法参数类型.class)使用时要传入那两个参数?
>
> 因为在Class类的底层![image-20210924164054684](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210924164054684.png)是通过Method[] 来存的,需要传入方法名来明确本次调用的方法;其次是方法参数类型:因为JVM判断重载是依据**参数列表**,包括参数的**类型**和参数的**个数**,不包括变量名,变量名不同不是重载;
>
> 2.为什么getMethod("方法名",方法参数类型.class)第二个参数为什么要传.class?
>
> //getMethod("eat",String.class)
>
> 因为基本类型(如int),引用类型(String),类型不能用来进行传递!!!!要么传递**值**,要么传递**对象**(引用),String.class就是Class对象  , 实际上，调用Class对象的getMethod()方法时，内部会循环遍历所有Method，然后根据方法名methodName和参数类型parameterType匹配唯一的Method返回：
>
> ![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1602313745257-44279b16-ad14-4615-85bb-c7b2aa46186f.png)

> 为什么method.invoke(obj,args)要传入一个目标对象?
>
> 金句: **对象的本质就是用来存储数据的，**而方法作为一种行为描述，是所有对象共有的，不属于某个对象独有
>
> 方法共有要保证每个对象调用时,就要让方法"知道"是谁调用了我,所以JVM有一种隐性机制,当对象调用方法是,会**隐性传递当前调用该方法的对象参数(或者标记)**,方法可以根据这个标记知道当前调用"我"的是哪个对象
>
> 所以反射里也要告诉是**哪个对象**调用了方法
>
> invoke()在调用静态方法时不用传入具体对象,因为**静态方法是属于类**的
>
> > 静态方法是属于类的，动态方法属于实例对象，在类加载的时候就会分配内存，可以 通过类名直接去访问，非静态成员(变量和方法)属于类的对象，所以只有该对象初始化之后才存在，然后通过类的对象去访问
>
> ![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1602314351467-b98fe6a3-2bab-44f7-81c4-8119258e1082.png)
>
> 

#### 暴力反射(不建议使用)

暴力反射（不推荐，会破坏封装性）,可以获取到类中任何权限(包括私有)的成员进行使用

```java
getConstructor()只会返回public修饰的构造

getDeclaredConstructor()方法可以获取所有类型的构造~  但是返回的private构造不能用,可以通过AccessibleObject里面的void setAccessible(boolean flag)来修改如果参数是true表示取消权限检查,这样就可以使用了
```

-----反射原理------

#### 类加载器ClassLoader(抽象类) 

> ClassLoader 类加载器负责加载类,核心方法是loadClass(),传入需要加载的类会帮你加载
>
> 抽象类无法new创建对象,所以也无法用引用来调用方法 ,所以最核心的findClass()方法是抛了异常,具体的findClass()方法给子类实现(模板方法模式,**我能做的都给你做了，少量易变动的东西我留出来，你自己DIY去**)
>
> ```java
> Person p = new Person;//新创建一个对象 这个里面有一有个Person类里有eat方法
> p.eat();//p就是引用
> ```

加载.class文件大致可以分为3个步骤：

1. 检查是否已经加载，有就直接返回，避免重复加载

2. 当前缓存中确实没有该类，那么遵循父优先加载机制（双亲委派机制），加载.class文件

   > 双亲委派模型的工作流程是：如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把请求委托给父加载器去完成，依次向上，因此，所有的类加载请求最终都应该被传递到顶层的启动类加载器中，只有当父加载器在它的搜索范围中没有找到所需的类时，即无法完成该加载，子加载器才会尝试自己去加载该类。

3. 上面两步都失败了，调用findClass()方法加载

```java
@Override
public Class<?> findClass(String name) throws ClassNotFoundException {
  try {
    /*自己另外写一个getClassData()
                  通过IO流从指定位置读取xxx.class文件得到字节数组*/
    byte[] datas = getClassData(name);
    if(datas == null) {
      throw new ClassNotFoundException("类没有找到：" + name);
    }
    //调用类加载器本身的defineClass()方法，由字节码得到Class对象
    return defineClass(name, datas, 0, datas.length);
  } catch (IOException e) {
    e.printStackTrace();
    throw new ClassNotFoundException("类找不到：" + name);
  }
}
```

> 1. IO流读取.class文件(在类加载器里，判断如果加载了就不在加载直接返回,如果尚未加载，则遵循父优先的等级加载机制（所谓双亲委派机制），如果还没有加载成功则调用让子类重写的findClass(name)方法)
> 2. 在findClass中定义一个方法,通过IO流从指定位置读取xxx.class文件得到字节数组
> 3. 调用父类的defineClass()方法 传入2中得到的字节数组与相关参数,创建Class对象并返回给父类~
> 4. 这时内存中就有了Class对象
>
> ![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1602308435623-e39c997e-b23f-47ab-93ef-9447ce47aafc.png)



### Class类

class对反射的支持,写了一个内部类  设置很多字段来描述.class文件的结构

![image-20210924134413930](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210924134413930.png)

注解

![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1602311041841-68e66182-fd09-483d-a8a3-98a1941cd32d.png)

泛型![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1602311057031-2ef46b61-15c3-4d0b-aa57-a734b2206d74.png)

>字段、方法、构造器，因为这三个部分信息量太大了，JDK单独写了三个类：Field、Method、Constructor。这三个类有一个父类AccessibleObject，里面的`void setAccessible(boolean flag)：如果参数是true表示取消权限检查,就可以用暴力反射(可以调用private类型的方法)`
>
>Class类持有这三个对象数组的**引用**即可（因为一个类可能有多个方法，所以Class要用Method[] methods保存）

最终一个类中所有的信息,都被"解构"后保存在Class类中,(其中字段,方法,构造器用他们的对象单独表示)

Class类构造器是私有的,无法手动new一个Class对象,要得到Class对象去看[获取Class的三种方式](#获取Class的三种方式)只能由JVM创建(**JVM构造class对象时会传入一个类加载器**)

> 说:反射就是换了另一种方式去创建对象以及调用方法,class对反射的支持写了一个内部类,设置了很多用来描述.class文件的属性,其中字段,方法,构造器三个部分信息量太大,jdk单独写了三个类Field,Method,Constructor,这三个类有一个父类AccessibleObject,里面的有一个setAccessible方法表示取消权限校验检查,取消了后就可以使用暴力反射(也就是可以使用private类类型的方法).
>
> 反射是原理其实是传一个需要加载的类,类加载器生成并返回class对象(其中包括三个步骤,一个是判断这个类是否被加载,如果已经被加载直接返回,避免重复加载.一个是采用了父优先加载原则(双亲委派机制),如果前两步都加载失败,则采用第三种,这个采用了设计模式中的模板方法模式,父类的方法交给子类去实现,子类根据IO流读取字节码,根据字节码文件构建字节数组,通过调用父类的defineClass根据字节数组去构建class对象)
>
> 有三种方法可以得到class对象
>
> 1. new一个对象,根据 对象的引用.getClass
> 2. 类.class
> 3. Class.forName("全限定类名")
>
> 根据类对象clazz.getConstructor得到空参构造/或者.getConstructor(参数)得到全参/有参构造方法,根据构造去newInstance创建实例
>
> 调用方法时候,根据类对象clazz.getMethods("方法名",方法参数类型),执行根据得到的method对象.invoke(传入实例对象,方法的参数)

#### 注解

##### 概念

注解就是注释。只不过它不是给程序员看的，而是给虚拟机看的。它是程序的一部分。

##### 定义注解：

```java
public @interface 注解名称{

	String  getValue() default "no description" ; //还可以给注解赋初始值

	}
```



##### 元注解

加在注解上的注解，常用的有

* @Target ：加在注解上，限定该注解的使用位置。不写的话，好像默认各个位置都是可以的
* @Documented：用于制作文档，不是很重要，忽略便是

- @Retention（注解的保留策略）  

  > 设置注解的运行阶段  
  >
  > ```java
  > @Retention(RetentionPolicy.RUNTIME)
  > public @interface MyAnnotation {
  >  String getValue() default "no description";
  > }
  > ```

##### 注解的保留策略

* 源码阶段 SOURCE	//编译后就失效
* 字节码阶段 CLASS    //保存在类文件中,但在运行虚拟机时不会保留
* 运行时阶段 RUNTIME    //在运行虚拟机VM时保留,可以作为反射用时的保留策略

##### 注解三部曲

* **定义**注解

* **使用**注解

* **读取**注解

  

  > 读取注解可以通过反射的方式读取(前提是在定义注解时应该将其保留策略改为RUNTIME,要反射时调用,就要将注解保留在虚拟机将.class文件加载到内存为止)![image-20210921231959308](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210921231959308.png)
  >
  > 但读取不止反射一种途径,@Override,由编译器读取,它的保留策略为源码阶段,编译成.class就消失了

##### 注解属性的数据类型

* 八中基本数据类型(byte  short  int long    float double   boolean char )
* String
* 枚举(CityEnum)
* Class
* 注解类型
* 以上类型的一维数组

##### value属性

如果**注解的属性只有一个，且叫value**，那么使用该注解时，可以不用指定属性名，因为默认就是给value赋值：

![image-20210922000303049](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210922000303049.png)

![image-20210922000228148](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210922000228148.png)

![image-20210922000417523](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210922000417523.png)

![image-20210922000520182](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210922000520182.png)

##### 用常量做注解属性赋值

![image-20210922001628789](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210922001628789.png)

一些省略: 如果数组元素只有一个,那么可以省略花括号{}

> 大多数情况下,三个关系中(定义,使用,读取注解),框架会把注解类和读取注解的程序隐藏起来,除非阅读源码,否则看不到,只顾着使用,就忘了它是如何起作用的了~  （T_T）夺可怜鸭

### 动态代理

##### 代理

> 代理是一种模式，提供了对目标对象的间接访问方式，即通过代理访问目标对象。如此便于在目标实现的基础上增加额外的功能操作，前拦截，后拦截等，以满足自身的业务需求。![图片.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1618020998546-1a6cc70a-2934-4396-8541-424addba44b1.png)

#### 静态代理

##### 静态代理实现

编写一个实现类,实现与目标对象相同的接口,并在实现类中维护一个目标对象的引用.通过构造器传入目标对象,在代理对象中调用目标对象的同名方法,并且添加前置拦截后置拦截等所需业务功能.

> 代理类中维护一个目标代理对象的引用,用这个引用去完成其本身的方法,再在这个代理类上加上要加强的方法

###### 弊端

- 对代码进行增强时候(比如添加日志),静态代理虽然解决了直接更改代码,解决了日志,目标代码两者的耦合,却又造成了增强代码和代理类的耦合,代理类无法复用;
- 而且还是需要对目标代码一个
- 一个进行添加日志,代码重复度过高(每个方法都时添加日志的操作)依然没有解决

![图片.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1618025482003-d58f50a5-ce94-4989-a425-a6fc818a190d.png)

#### 动态代理

拦截对真实对象方法的直接访问，增强真实对象方法的功能

##### 先了解类Class 

用Person类描述人,Animal类描述动物,Class类用来描述Person类,Animal类,详情参考[反射](#反射),通过**泛型**来区分类Class<Person>,Class<Animal>

需要进行代理,重点在于**增强代码+目标对象**,而且只需要代理对象与目标对象的方法相同就OK. 所以要进行动态代理,要获得一个类的方法信息

##### 获得方法信息的两种途径:

* 目标类本身 (GCLib动态代理机制)
* 目标类实现的接口(JDK动态代理机制)

> 两种不同的思路造就了两种不同的代理机制
>
> 两者选择
>
> * 如果目标对象有接口的话，推荐使用jdk，在1.8版本之后性能远超过cglib(实际中面向接口编程,有service接口,serviceImpl实现servic接口)
>
> * 如果目标对象没有接口的话，只能使用cglib
>
> ![1573227895080](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1573227895080.png)

##### JDK动态代理

###### 思路

将需要代理的类抽取成一个接口,实现类实现这个接口并重写所有方法~

通过反射得到这个接口除了没有构造方法和实现类继承Object的方法外,其余都有,所以可以通过这个接口来得到代理对象



接口的**Class对象没有构造方法**,所以不能new,但是可以拷贝一个接口的Class对象,并且给其加上构造方法.

>  金句:这也正是JDK动态代理的本质:**用Class造Class，即用接口Class造出一个代理类Class**

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1618657069339-c705be10-c445-441a-ae68-c12b5661e48a.png)

用Proxy.getProxClass(类加载器随意指定一个,接口的Class对象),就可以通过这个接口Class对象copy一个代理Class对象,并且在这个对象中添加构造器等

代理对象的内部有个成员变量invocationHandler，而且代理对象的每个方法内部都会调用handler.invoke()  动态代理为了实现代理对象和增强代码的解耦，把增强代码也抽取出去了，让InvocationHandler作为它与目标对象的桥梁。

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1618661371916-03ff6bc1-2631-4509-bb8a-73b1979c4c69.png)

这个构造器中需要传入一个InvocationHandler.class,得到构造器(`代理对象的内部确实有个成员变量invocationHandler`),通过构造引用去newInstance()实例化时需要重写其中的invoke()方法(这其实是抽取出的增强代码)

Proxy源码

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1618661970523-60a68afb-595f-4665-a736-0e709c7e6329.png)

未优化的代码

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1618659925568-fdf46c89-4e91-4880-8441-b5c921073cce.png)

![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1618658859792-5a953bbe-48f8-45a2-98b3-bc474f980bb6.png)

JDK根据接口生成的其实是Proxy的Class对象，然后根据ProxyClass得到proxy代理对象。proxy代理对象实现了接口，同时也是Proxy类型的。proxy对象的原理是：内部维护一个InvocationHandler，而InvocationHandler是对增强代码的抽象。通过抽取InvocationHandler，将代理对象和增强代码解耦。并把目标对象target传给InvacationHandler调用目标方法

##### 方法

```java
● 先获得proxyClazz
● 再根据proxyClazz.getConstructor()获取构造器
● 最后constructor.newInstance()生成代理对象
    更好的API
JDK提供了Proxy.newProxyInstance()
```

原本代理对象直接调用目标对象，现在是代理对象调InvocationHandler，InvocationHandler再调目标对象。Proxy代理对象内部有InvocationHandler对象，而InvocationHandler对象内部有我们塞进去的目标对象，所以最终通过代理对象可以调用到目标对象，并且得到了增强。所以，代理模式就是俄罗斯套娃...

> 说: 对代码进行增强,需要对其增强时,可以使用代理模式,代理分为静态代理和动态代理
>
> 当对一个代码进行增强,比如对添加日志操作,如果对每个方法重新修改,工作量太大,而且不利于维护
>
> 这时如果用静态代理,会很大程度减少工作量,并且降低耦合~
>
> 方法全部抽取到接口中,代理类和目标类实现同一个接口,并在代理类中加入构造函数,将目标类传进去,维护一个目标类的引用,前置后置等增强方法可以写在其中,同时用目标类的引用去执行原有方法,完成代码的增强
>
> 但同时这样又会造成增强代码和代理类的耦合,这时能否自动生成代理对象,我们免去写代理对象?并且将增强代码和代理对象解耦,达到代码复用?
>
> 这时就可以用动态代理,动态代理要得到类的方法信息,有两种方式:
>
> 一种是目标类本身,一种是目标类实现的接口
>
> 两种方式造就两种不同代理方式,前者cglib动态代理,后者JDK动态代理
>
> 这里谈JDK动态代理,抽取要增强的方法到接口,问题有了,如何根据一个接口得到代理对象?
>
> 这个接口本身也是一个Class对象,这个对象里有目标类的方法,但是接口这个Class对象没有构造函数,我如何给接口安装一个构造器呢(不合理)?或者不改变接口本身,直接拷贝接口的信息到另一个Class,然后给Class装上构造器,所以动态代理本质就是**用接口Class造一个代理类Class**,java api中有一个getProxyClass方法(传入类加载器,和接口的class对象)就可以通过反射返回代理类的Class对象,并提供类加载器,和接口数组.生成的这个代理类里有构造方法,但要传入一个叫InvocationHandler的参数类型,然后根据构造方法生成的constructor对象去创建实例,要给构造器传入一个参数类型,那么这个实例必然要有成员变量要接收,代理对象的内部确实有个成员变量invocationHandler，而且代理对象的每个方法内部都会调用handler.invoke()！(创建实例时候要重写invoke方法),
>
> invoke 方法有三个参数 一个是 proxy,代理对象本身,一个是method,方法执行器(传入目标对象就执行目标方法),args(方法参数)动态代理为了实现增强方法和代理类的解耦,将增强方法抽取了出来
>
> ![image-20211024160012065](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211024160012065.png)
>
> 不过有更好的newProxyInstance()

##### cglib动态代理

```java
//不破坏业务代码对业务层功能增强
@Component  
public class CGLIBProxyFactory {

    @Autowired
    private TransactionManager transactionManager; //事务控制的类

    public Object createCGLIBProxy(Object target) {
        Object cglibProxy = null;

        
        // 使用第三方cglib技术（spring框架内置cglib的工具类）
        /*
            第一个参数：目标对象的字节码对象
            第二个参数：代理增强实现逻辑（接口）
         */
        //cglib库的Enhancer在Spring AOP中作为一种生成代理的方式被广泛使用。Enhancer是cglib中使用频率很高的一个类，它是一个字节码增强器，可以用来为无接口的类创建代理。它的功能与java自带的Proxy类挺相似的。它会根据某个给定的类创建子类，并且所有非final的方法都带有回调钩子。
        cglibProxy = Enhancer.create(target.getClass(), new MethodInterceptor() {

            /*
                   proxy 工具类生产的代理对象
                   method 调用的目标方法
                   args 该方法传递的实际参数
                   methodProxy  等同method
             */
            @Override
            public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {

                try {
                    transactionManager.begin();
                    // 调用目标对象原有功能
                    Object result = method.invoke(target, args);
                    transactionManager.commit();
                    return result;
                } catch (Exception e) {
                    e.printStackTrace();
                    transactionManager.rollback();
                } finally {
                    transactionManager.release();
                }
                return null;
            }
        });

        return cglibProxy;
    }
}
```

### 枚举

#### 多例模式

```java
public class Person{
    //类中将构造方法私有,外界无法直接创建对象
    private Person(){}
    
    //创建集合,用来保存程序中所有Person对象
    private static List<Person> list = new ArrayList<>();
    
    //定义一个静态代码块,在静态代码块中创建Person对象,并添加到集合中
   	static{
        for(i=0;i<3;i++){
            list.add(new Person());
        }
    }
    
   //定义方法,用来随机获取程序中的Person对象
    public static Person getPerson(){
    	//生成随机的索引
        Random r = new Random();
        int radomIndex = r.nexInt(list.size());
        //根据这个随机索引从集合中获取元素然后返回
        return list.get(randomIndex);
    }
}

```

#### 概念

枚举本质就是类，枚举是多例的一种体现。
在枚举中我们可以给出一些属性，枚举中的每个属性都是自身的对象。

#### 格式

```java
   public enum 枚举名 {
        属性,属性,属性
    }

//例子:
public enum Gender {
    //实例化多个对象(多例模式) 
    BOY("男"), //枚举中的属性,就是自身的对象
    
    GIRL("女");

    private String title;

    Gender(String title) {
        this.title = title;
    }

    @Override
    public String toString() {
        return title;
    }
}
/*
枚举中的属性在命名时建议全部大写,单词下划线隔开
枚举中的属性默认使用了static final进行修饰，因为使用static修饰了，所以我们可以通过枚举名直接使用里面的属性。

*/
```

#### 枚举和常量类的区别

枚举本质是一个**对象**,可以设置对象该有的一切

常量类往往只有**字段**,对象和字段没有可比性

两者作为常量使用时,区别不大

##### 非要谈优点

1. 限制重复定义常量

   > 枚举作为常量类时候,编译器会自动限制取值不能相同,而常量类做不到

2. 包含更多维度信息,枚举本身是对象

   > 对象可以包含更多信息
   >
   > ![image-20210927160646609](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210927160646609.png)

3. 枚举使用时可以用Switch判断,代码比if/else简洁



新增一个类型时候 常量类+ifelse所在的类 修改要改两处

枚举只需要在基础上新增一个类型就可以

有很多会员模式,时间长了同事将你的if(黄金会员)elseif(白银会员)写了很多处用来判断,如果第二版新增黑钻会员,就需要改每一处的代码, 但采用枚举,把容易变化的逻辑抽取出来共同维护就会很方便

`金句: 设计模式中说:越抽象越稳定,越具体越不稳定. 设计模式的目的不是消除变化而是隔离变化(一个房间里不能让兔子安静(不让代码变更),就把它放笼子里达到房间的安静)`

#### 枚举优化

1. for 改为 增强for 或者stream(jdk1.8新特性)

![图片.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1601354038742-8fe7afcd-b741-41f1-87fe-d2e73c82bd09.png)

![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1585298906489-c95b7e9d-10a9-4436-99d0-16c6fc35f455.png)

![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1601062444135-0f81d952-6d8b-4b64-ae5d-f96abe34d95d.png)

2. 用Map代替For,缓存枚举值,利用key提高检索速度(Mybatis枚举转化器就是这种写法)

![image.png](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1585470136549-a9afb26b-f446-48b3-ad2c-f95a5b6aa89a.png)

3. 结合Lombok简化代码

```java
@Getter
@AllArgsConstructor
public enum PlatformEnum  {
    TAOBAO(1, "淘宝", "taobao", "http://xxx"),
    PDD(2, "拼多多", "pdd", "http://xxx");

    private final Integer code;
    private final String sourceKey;
    private final String iconUrl;

    public static PlatformEnum getByCode(Integer code) {
        for (PlatformEnum platformEnum : PlatformEnum.values()) {
            if (platformEnum.getCode().equals(code)) {
                return platformEnum;
            }
        }
        return null;
    }
}
```

### XML相关

html是用来显示数据的；xml是用来描述数据、存放数据的 

<?xml version="1.0" encoding="UTF-8" ?>

两种约束:dtd(1.内部放到xml文件2.外部,本地3.公共,在网上),schema

#### xml解析方式

DOM解析(解析到内存 优:保留xml结构  缺:可能内存溢出)

SAX解析(逐行解析,解析一行释放一行, 优:效率高,不会内溢 缺:不保留xml文件结构)

PULL解析(安卓内置,类似sax)

##### 解析工具包

DOM4J(第三方提供)

将xml读内存,生成dom树,**并创建Document对象**,xml中的元素属性文本都是树中的**节点**

![1597156347362](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1597156347362.png)

相关API

```java
/*
SAXReader核心类
	read方法可以读取xml文件，生成dom树并创建Document对象
	Document表示整个的xml文档
     getRootElement获取根元素
Element表示元素
	List elements(String ele) 获得指定名称的所有子元素 获得当前元素的元素名
	
	先创建SAXReader对象-->调用read读取xml文件得到Document对象-->调用getRootElement获取根元素-->调用elements获取子元素-->遍历集合-->获取每一个元素-->通过元素得到属性或者元素的子元素
*/

/*
xpath根据表达式获取指定原色
获取到Document对象后,
 List selectNodes("xpath表达式")：获取多个满足要求的元素
        Node selectSingleNode("xpath表达式")：获取一个满足要求的元素
        
   表达式的规则：
        1. 绝对路径表达式方式 例如: /元素/子元素/子子元素...
        2. 相对路径表达式方式 例如: 子元素/子子元素.. 或者 ./子元素/子子元素..
        3. 全文搜索路径表达式方式 例如: //子元素//子子元素
        4. 谓语（条件筛选）方式 例如: //元素[@属性名=属性值]
*/
```

### Lombok

导入jar包,下载lombok插件

帮助我们生成get/set/等方法 只需要添加相关注解  (需要在idea安装并配置)

```java
@Data //包括了@Getter/@Setter;@ToString;@EqualsAndHashCode;@RequiredArgsConstructor

@EqualsAndHashCode(exclude = "name")//排除某个字段 //当两个对象 equals 时，他们的 hashcode 一定要相同，反之，当 hashcode 相同时，对象不一定 equals。所以 equals 和 hashcode 要一起实现
@AllArgsConstructor //生成全参构造
@NoArgsConstructor //生成无参构造
```

[Lombok常用注解 - 草木物语 - 博客园 (cnblogs.com)](https://www.cnblogs.com/ooo0/p/12448096.html)



## Web

### JavaScript

JavaScript(网景公司看到java很火,就把LiveScript改为JavaScript)简称js(这个简称也可以代表JScript微软的)

js不需要编译,由浏览器解释运行

HTML引入JavaScript的两种方式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
        <!第一种 --内嵌式-->
        <script>
            //往浏览器中写入内容 document表示整个文档对象
            document.write("1");
            /*
            真实项目中,提示框一般使用己封装插件和组件来实现
            */
            //---------三种弹框----------------
            //1.使用alert弹框提示信息，最后都会被转化为字符串输出
            alert(1+1);
            //2.confirm在alert基础上增加了让用户选择性的操作（提供两个按钮：确定和取消）
            var  delin = confirm(‘你确定要删除吗？’);
            //3.在confirm的基础上增加让用户输入的效果
            var flag = prompt(‘请输入你想反馈的意见：’);
            //-----------函数-------------
            function 函数名称(变量 , 变量 ,变量){
            	内容代码
             	return 返回值; 如果需要返回值直接返回即可 不需要声明返回值类型
            }
            //函数调用,demo1是函数名
            let returnData = demo1("aaaa" , "bbbb");
            //定义匿名式函数-留后 - 必须和js的事件匹配使用
            let fn = function(){
                console.log("匿名式函数");
            };
            
            document.getElementById("btn").onclick=function(){
        alert("123");
            </script>
        <!--第二种 外联式,这里面不能写js代码-->
        <script src="js/demo.js"></script>
    </head>
    <body>
        <!--由单击事件触发某个js函数 标签上有 onXxx() 都是事件-->
        <input type="button" value="点点我" onclick="myOnClick()" />
        <input id="btn" type="button" value="点我执行匿名函数"/>
    </body>
</html>
```

### window对象

Window 对象表示浏览器中打开的窗口。

| **方法名**                  | **说明**                                         |
| --------------------------- | ------------------------------------------------ |
| alert( )                    | 显示带有一个提示信息和一个确定按钮的警示框       |
| confirm( )                  | 显示一个带有提示信息、确定和取消按钮的对话框     |
| prompt( )                   | 显示可提示用户输入的对话框                       |
| setInterval(“方法”，毫秒值) | 按照指定的周期（以毫秒计）来调用函数或计算表达式 |
| setTimeout(“方法”，毫秒值)  | 在指定的毫秒数后调用函数或计算表达式。           |
| clearInterval()             | 取消由 setInterval() 设置的 timeout。            |
| clearTimeout()              | 取消由 setTimeout() 方法设置的 timeout。         |

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <style></style>
</head>
    <body>
        <script>
    	//页面加载函数: 整个页面执行完毕后加载此函数
        window.onload = function(){
            //1.有 setInterval的定时器
            setInterval("changeImg()",500);
        }
        </script>
    </body>
</html>
```

####  DOM(Document Object Model) 

JS把页面抽象成为一个对象,允许我们使用js来操作页面

![1572578467097](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1572578467097.png)

##### DOM获取元素

| 原生js                        | es6                             |
| ----------------------------- | ------------------------------- |
| getElementById(ID)            | querySelector(选择器)           |
| getElementsByClassName(class) | querySelectorAll(.class)        |
| getElementsByTagName(标签)    | querySelectorAll(标签)          |
| getElementsByName('name值')   | querySelectorAll([name=name值]) |

##### DOM操作内容

| 操作内容                                                     |      |
| ------------------------------------------------------------ | ---- |
| element.innerText; <br/>获取或者修改元素的纯文本内容,覆盖div的原内容,按照文本方式处理数据 |      |
| element.innerHTML;<br/>获取或者修改元素的html内容,覆盖div的原内容,按照html方式处理数据 |      |
| element.outerHTML;<br/>获取或者修改包含自身的html内容        |      |

##### DOM操作属性

| 操作属性                                                     |      |
| ------------------------------------------------------------ | ---- |
| setAttribute<br/> document.getElementById("username").setAttribute("type","button")<br/>把名为username的元素的类型改为button |      |
| getAttribute                                                 |      |
| removeAttribute                                              |      |

### Tomcat

![image-20211006170157914](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211006170157914.png)

### 架构

B/S : brower/server  浏览器 服务器模型    不要下载,更新快,效果欠佳

C/S  : client /server  客户端 服务器模型 	要下载,运行效率高,更新麻烦

两者都是基于请求/响应的模型,请求响应成对出现,先有请求再有响应

> 请求(request):浏览器给服务器的
>
> 响应(response):服务器给浏览器的内容
>
> > 服务器:服务器也是一台安装了特殊软件的电脑,能存放静态资源(不同用户访问看到内容一致);也能存动态资源(不同用户看到内容不一致)
> >
> > > 软件:
> > >
> > > * tomcat(小型服务器): 免费开源.自己维护.只支持两套规范 servlet/jsp
> > > * weblogic(大型服务器): oracle 公司(甲骨文)  经典的产品 oracle  数据库 . jdk目前是oracle公司维护 . 收费. 支持javaEE十三套规范(标准,接口)
> > > * websphere(中大型服务器): IBM  简称 was . 收费 支持javaEE十三套规范(标准,接口)

### URL

![image-20211003145338672](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211003145338672.png)

#### Request(请求)

##### 请求行

POST http://localhost:8080/day01/demo.html HTTP/1.1

GET http://localhost:8080/day01/demo.html?username=jack&nickname=rose HTTP/1.1

固定格式:  请求方式  请求路径  协议版本

##### 请求头

格式: 浏览器自带`key : value`

##### 请求体

只有post有请求体

格式:`key=value&key=value&key=value`

##### GET/POST区别

###### GET没有请求体

1.  参数以键值对的方式拼接到地址栏 第一个键值对使用?后续的键值对使用&符号拼接
2.  长度有限 地址栏可见 不安全

###### POST把数据放在请求体中

1. 地址栏不可见数据
2. 长度不限

###### GET传送数据量小

一般数据大小不超过2k-4k(根据浏览器不同限制不同)

###### POST传送数据量大

一般被默认为不受限制

Get 限制 Form 表单的数据集的值必须为 ASCII 字符；而 Post 支持整个 ISO10646 字符集。

Get执行效率比Post方法好,Get是form提交的默认方式

#### Response(响应)

##### 响应行

​	HTTP/1.1 200

​	格式: 协议的版本 状态码 状态码对应的提示信息

##### 响应头

​	格式:	`key:value`

##### 响应体

就是服务器 往浏览器写入一些字符串(html页面源码)  浏览器对html代码进行解析 最后展示效果 至此看到页面

###### 响应体的两种流-字符流

| **返回值**  | **方法名**                                | **说明**                           |
| ----------- | ----------------------------------------- | ---------------------------------- |
| PrintWriter | getWriter()                               | 获取响应字符输出流对象             |
| void        | setContentType(“text/html;charset=UTF-8”) | 设置响应内容类型，解决中文乱码问题 |

###### 响应体的两种流-字节流

| **返回值**          | **方法名**                                | **说明**                           |
| ------------------- | ----------------------------------------- | ---------------------------------- |
| ServletOutputStream | getOutputStream()                         | 获取响应字节输出流对象             |
| void                | setContentType(“text/html;charset=UTF-8”) | 设置响应内容类型，解决中文乱码问题 |

```java
//工具类快速封装对象 要求: 实体类的字段和表单提交字段必须一致 没有顺序区分(自动的转换类型)
//参数1: 需要封装的对象 参数2: 数据的来源

BeanUtils.copyProperties(student , map);
```



### Servlet

web三大核心组件之一

#### 概念

servlet : server applet   服务器的小程序 , 能够被运行的程序

- servlet运行在服务器端的java程序。
- Servlet是一个接口，一个类要想通过浏览器被访问到,那么这个类就必须直接或间接的实现Servlet接口,实现后重写里面的service方法

#### 作用

接收请求，处理请求，生成响应

#### 使用步骤

1. 创建web项目
2. 创建一个类实现servlet接口,并重写service方法(servlet访问的入口)
3. web.xml配置servlet
4. 部署启动项目浏览器测试

```xml
找到类,给类设置一个访问路径
<servlet>
    <servlet-name>别名:整个xml中必须是唯一的</servlet-name>
    <servlet-class>类的全限定类名,唯一锁定类.底层tomcat需要借助全限定类名反射实例化(new)对象</servlet-class>
	<!--这里是初始化的配置,通过servletConfig调用-->
    <init-param>
        <param-name>ds</param-name>
        <param-value>黎明</param-value>
    </init-param>
    <!--设置全局参数-->
    <context-param>
      <param-name>charset</param-name>
      <param-value>utf-8</param-value>  
    </context-param>

</servlet>
<servlet-mapping> servlet的映射配置
    <servlet-name>别名:使用已经定义好的别名</servlet-name>
    <url-pattern>访问的链接地址 /名称</url-pattern>
    <url-pattern>访问的链接地址2 /名称 可以有多个</url-pattern>
</servlet-mapping>
```

#### 执行过程

![image-20210416084533771](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210416084533771.png)

#### 生命周期

```java
public void init(ServletConfig servletConfig);初始化
public void service(ServletRequest servletRequest, ServletResponse servletResponse);服务
public void destroy();正常销毁
```

servlet其实是一个单例对象.  单例: 对象只能被初始化一次  单实例多线程

```
servlet是单实例的线程不安全的对象.(不要给它声明成员变量)
默认第一次请求来的时候,web服务器(servlet容器)会创建servlet的对象,执行servlet的init方法,实现初始化操作,然后执行servlet的service方法来处理当次请求.
再有请求来的时候,tomcat会获取一个线程,在当前线程中,会调用servlet的service方法来处理请求
当servlet被移除的时候或者web服务器关闭的时候,执行servlet的destory方法来显示销毁操作
```

##### 修改servlet创建时机

第一次访问时创建(提高服务器启动效率,但一些在应用加载时的初始化操作就无法完成)/服务器加载时创建(提高首次执行效率,但影响服务器启动效率)

```xml
<!--在servlet的配置文件xml中添加 启动时就加载servlet,1-6按照由小到大顺序加载->
<load-on-startup>2</load-on-startup>
```

#### 创建servlet三种方式

1.  实现 Servlet 接口，实现所有的抽象方法。该方式支持最大程度的自定义。
2.  继承 GenericServlet 抽象类，必须重写 service 方法，其他方法可选择重写。
    该方式让我们开发 Servlet 变得简单。但是这种方式和 HTTP 协议无关。
3.  (常用)继承 **HttpServlet** 抽象类, 不需要重写任何方法 不报错，需要重写 **doGet 和 doPost** 方法。该方式表示请求和响应都需要和 HTTP 协议相关。

#### ServletConfig

ServletConfig 是 Servlet 的配置参数对象，在 Servlet 的规范中，允许为每一个 Servlet 都提供一些初始化的配置。所以，每个 Servlet 都有一个自己的 ServletConfig。
作用：在 Servlet 的初始化时，把一些配置信息传递给 Servlet。生命周期和servlet相同

```xml
<servlet>
	<init-param>
        <param-name>parname</param-name>
        <param-value>黎明</param-value>
    </init-param>
</servlet>
```

##### 常用方法

| **返回值**          | **方法名**                    | **说明**                 |
| ------------------- | ----------------------------- | ------------------------ |
| String              | getInitParameter(String name) | 根据参数名称获取参数的值 |
| Enumeration<String> | getInitParameterNames()       | 获取所有参数名称的枚举   |
| String              | getServletName()              | 获取Servlet的名称        |
| ServletContext      | getServletContext()           | 获取ServletContext对象   |

#### ServletContext

ServletContext 是应用上下文对象(应用域对象)。每一个应用中只有一个 ServletContext 对象。
主要作用：可以获得应用的**全局初始化参数和达到 Servlet 之间的数据共享**,获取当前应用下所有路径下的所有类型的资源
生命周期：应用一加载则创建，应用被停止则销毁。

##### **获取ServletContext对象**

```markdown
继承HttpServlet后，可以直接调用 当前模块下获得的servletContext对象都是同一个 整个项目启动的过程中有且只有一个对象,对象销毁的时机是服务器关闭
	ServletContext sc = getServletContext();
	
    //获得ServletContext方式
    ServletContext sc1 = getServletConfig().getServletContext();
    System.out.println(sc1);
    //缩写
    ServletContext sc2 = getServletContext();//this.getServletConfig().getServletContext();
    System.out.println(sc2);
```

#### Servlet四大域对象(用于存储数据)

1. page（jsp有效），page域指的pageContext

   > 作用:
   >
   > 1. 当前页面共享数据`xxxAttribute()`
   >
   > 2. 获取其它八个[JSP内置对象 ](#JSP九大内置对象) `getRequest()`
   > 3. 可以操作四个域对象的数据 `xxxAttribute(...,int那个域)`
   >
   > findAttribute():依次从最小域对象到最大域对象中去查找指定的属性,若找到马上返回且终止查找;若没有找到返回null

2. request（一次请求），request域指的HttpServletContext

   > 浏览器每发起一次请求 都是产生一个新的request对象 , 每次响应之后request即销毁
   >
   > 创建: 请求开始创建
   >
   > 销毁:响应结束数据
   >
   > 经过http协议将请求的数据发送给服务器, 服务器tomcat会为此次请求创建request和response对象, 并且将http协议数据放入其中
   >
   > 共享数据: 一次请求过程中 所有的资源共享数据 , 共享自己本次请求的数据

3. session（一次会话），session域指HttpSession

   > 创建: 默认情况下 第一次调用request.getSession()创建
   >
   > 销毁: 三种销毁方式 超时30分钟 手动销毁 服务器非正常关闭
   >
   > 共享数据: 一次会话中所有的请求 , 共享自己本次会话的数据

4. application(当前web应用有效)，指的ServletContext

   > 创建: tomcat启动就创建servletContext对象
   >
   > 销毁: 服务器关闭就销毁
   >
   > 共享数据: 一个项目中 所有的会话 所有的请求 共享同一份数据

> 都内置了map集合,都有setAttribute和getAttribute和removeAttribute()方法,域对象指的是对象有作用域。也就是有作用范围。域对象可以实现数据的共享。不同作用范围的域对象，共享数据的能力也不一样。
>
> servletContext > HttpSession > HttpServletRequest>page数据共享的范围

api

```markdown
存储数据
	void setAttribute(String name,Object value)
获取数据
	Object getAttribute(String name)
删除数据
	void removeAttribute(String name)
```

ServletContext 可以获取当前应用下任何路径下的任何类型的资源

| **返回值** | **方法名**                       | **说明**                              |
| ---------- | -------------------------------- | ------------------------------------- |
| String     | getRealPath(String path)         | 获取当前应用的访问虚拟目录,真实的路径 |
| String     | getResourceAsStream(String path) | 直接获取文件流                        |

#### 注解

servlet 2.5 :  xml开发

servlet 3.0 以后 : 注解开发，注解是用来替代xml的, 简化xml的配置操作

### Request 和 Response 

- 用户通过浏览器访问服务器时，Tomcat将HTTP请求中所有的信息都封装在Request对象中,开发人员可以通过request对象方法，来获取浏览器发送的所有信息.
- response对象表示web服务器给浏览器返回的响应信息,开发人员可以使用response对象的方法，设置要返回给浏览器的响应信息

| **返回值**           | **方法名**                      | **说明**             |
| -------------------- | ------------------------------- | -------------------- |
| String               | getParameter(String name)       | 根据名称获取数据     |
| String[]             | getParameterValues(String name) | 根据名称获取所有数据 |
| Map<String,String[]> | getParameterMap()               | 获取所有参数的键值对 |

#### Request

##### 请求转发

request这个[域对象](#Servlet四大域对象)，必须匹配请求转发使用，一种服务器内部的资源跳转方式

```java
request.getRequestDispatcher("/内部路径").forward(request,response);
/**request.getRequestDispatcher("内部路径"); 获得请求调度器(中央控制)
*			.forward(request,response);  forward转向需要携带参数
*/
```

###### 特点

1.  一次请求多个servlet
2.  可以借助request域对象进行,数据共享
3.  地址栏不变(显示请求转发之前的地址)
4.  请求转发是服务器**内部**的事情 不能跳转站外(网站以外, 例如百度)资源

request.getRequestDispathcer(path)
.forward(request,response)

##### 其他功能

| **返回值**         | **方法名**       | **说明**       |
| ------------------ | ---------------- | -------------- |
| BufferedReader     | getReader()      | 获取字符输入流 |
| ServletInputStream | getInputStream() | 获取字节输入流 |

#### Response

| api                            | **说明**   |
| ------------------------------ | ---------- |
| response.setStatus(int 状态码) | 设置状态码 |
| response.sendError(int 状态码) | 发送错误   |

##### 常见状态码

| **状态码** | **说明**                     |
| ---------- | ---------------------------- |
| 200        | 成功                         |
| 302        | 重定向                       |
| 304        | 请求资源未改变，使用缓存     |
| 400        | 请求错误，常见于请求参数错误 |
| 404        | 请求资源未找到               |
| 405        | 请求方式不支持               |
| 500        | 服务器错误                   |

##### 重定向

客户端的一次请求到达后，发现需要借助其他 Servlet 来实现功能。

##### 重定向特点

1. 多次请求
2. 可以跳转站内和站外资源
3. 不能使用request共享数据(请求域对象中不能共享数据，可以重定向到其他服务器)
4. 地址栏发生了改变 显示的是重定向以后的地址

response.sendRedirect( "/day09/fourServlet");

location:设置重定向的地址

##### 重定向两种方式

方式1: 需要设置状态码 和 location 放到响应头中

```java
response.setStatus(302);
response.setHeader("location" , "/day06/two");
```

方式2:==sendRedirect("重定向的地址")==

```java
response.sendRedirect("/day06/two");
```

##### 请求转发和重定向对比

1. 重定向2次请求,请求转发1次请求
2. 重定向地址栏会改变,请求转发地址栏不变
3. 重定向是浏览器跳转,请求转发是服务器跳转
4. 重定向可以跳转到任意网址,请求转发只能跳转到当前项目
5. 重定向数据会丢失,请求转发不会丢失请求数据

### Cookie 和 Session

会话技术:
	http协议是一个无状态的协议(健忘症).
	在访问过程中,难免会产生一些数据,这些数据需要保存起来,就使用使用会话技术来保存.

数据可以保存在浏览器或者服务器上
	cookie:浏览器端会话技术(数据保存在浏览器上)
	session:服务器端会话技术(数据保存在服务器上)

常用的场景:
	用户登陆之后,用户的信息
	购物车(用户没有登录状态下的京东购物车)
	保存用户名
	...

#### 会话技术

日常生活中：从拨通电话到挂断电话之间的一连串你问我答的过程就是一个会话。

B/S架构中 : 从浏览器第一次给服务器发送请求时，建立会话；直到有一方断开，会话结束。

一次会话：包含多次请求响应。

> 从打开一个浏览器访问某个站点，到关闭这个浏览器的整个过程，称为一次会话
>
> HTTP是无状态的,服务器无法辨认访问者
> 会话机制帮助服务器记住客户端状态(我是xpp我要往我的购物车加东西了,记住我哈)(标识用户,跟踪状态),用会话跟踪技术就是对对HTTP无状态协议的扩展
>
> 会话是唯一标识一个用户并记录其状态,一个会话可以帮助服务器断定一个客户端,反推,当服务器无法断定客户端时,一次会话就结束了.(两种情况:1.服务器不行(session失效),客户端不行(cookie失效))
>
> 那么会话的基本原则是cookie和session共存

**客户端会话技术：**cookie

**服务器端会话技术：**session

#### Cookie

cookie可以理解为饭店票机给的小纸条(x先生,预计10:00点)，你拿着纸条进来用餐我就知道你排过队了
cookie有会话cookie(保存在浏览器内存中,浏览器关闭内存释放,cookie消失)和持久cookie(只要设置时间>0就可以实现持久化cookie,设置时间=0,响应到客户端后,会删除客户端cookie)

只要在服务器创建了session,即使不写addCookie("JSESSIONID",session.getId()),
JSESSIONID仍然会被服务器默认作为cookie返回,如果不设置cookie的时间,默认返回会话级cookie(关闭浏览器,cookie消失)。 

将产生的数据保存在浏览器端

##### 注意

* 响应的cookie是通过set-cookie的响应头带回给浏览器
* 请求的cookie是通过cookie请求头发送给服务器的
* 响应时必须
* response.addCookie(cookieName)

##### 细节

> * 每个网站最多只能有 20 个 Cookie，且大小不能超过 4KB。所有网站的 Cookie 总数不能超过 300 个。浏览器支持cookie 423原则
> * 存活时间限制 setMaxAge() 方法接收数字
>   负整数：当前会话有效，浏览器关闭则清除。
>   0：立即清除。
>   正整数：以秒为单位设置存活时间。
> * 访问路径限制
>   默认路径：取自第一次访问的资源路径前缀。只要以这个路径开头就能访问到。
>   设置路径：setPath() 方法设置指定路径。



#### Session

推测由于cookie传输敏感信息不安全/传输信息多会影响效率等原因有了session
session存在服务端的**内存**中(被**钝化**时会存到服务器的磁盘中),大的map,以键值对存储;只用给客户端传一个JsessionID(其实也是cookie),真正的数据存在服务端的session中,下次访问客户端带带上jsessionid,到服务端找到对应session

##### 概述

> 在WEB开发中，服务器可以为每个用户浏览器创建一个会话对象（session对象），注意：一个浏览器独占一个session对象(默认情况下)。因此，在需要保存用户数据时，服务器程序可以把用户数据写到用户浏览器独占的session中，当用户使用浏览器访问其它程序时，其它程序可以从用户的session中取出该用户的数据，为用户服务。
> Session和Cookie的主要区别在于：
> 	 Cookie是把用户的数据写给用户的浏览器。
>      Session技术把用户的数据写到用户独占的session中。
> ==Session对象由服务器创建，开发人员可以调用request对象的getSession方法得到session对象==

session需要基于cookie实现 (session也可以不基于cookie实现，如浏览器端禁用了cookie后，需要把id写到请求头中)

使用cookie存储session的id

![image-20210419105600194](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210419105600194.png)

服务器端会话技术：在一次会话的多次请求之间共享数据，将数据保存到服务器端

##### 工作流程:

```
当浏览器访问服务器的时候,服务器会看浏览器是否携带了id(session 依赖名为JSESSIONID的cookie)
*	若没有带，也就是第一次请求,服务器会在服务器上创建一块空间,然后操作数据,服务器会将这个空间的id(JSESSIONID)通过响应返回给浏览器.
*	若带了也就是第二次或后几次的请求,服务器获取这个id,拿着这个id去自己的内存中查找有无这块空间
	*	若找到了,直接把数据存入这块空间.
	*	若没有找到,服务器会在服务器上创建一块空间,操作数据,服务器会将这个空间的id(JSESSIONID)通过响应返回给浏览器.
```

==session底层是依赖于cookie.cookie中存放的就是JSESSIONID的值==

##### 生命周期

从创建到销毁的过程称为生命周期

创建时机:第一次调用request.getSession()创建session对象

销毁时机:

​	1. tomcat默认超时30分钟会自动销毁session

	2. 手动调用session.invalidate()销毁session对象
	3. 服务器非正常关闭销毁对象(死机等)



-session最大不活动时间[30min]

当服务器关机时,服务器会将当前内存中的session序列化到磁盘中生成sessions.ser,等重启完毕,又重新读回内存

当一个session长时间不活动,为了减少内存,也有可能被序列化到磁盘中.当session再次被访问时,才会被反序列化 也是session 的[**钝化**]和[**活化**] (在tomcat的conf的**context.xml中配置钝化时间**),钝化将每个session单独一个文件,不是sessions.ser

当session被序列化到磁盘时,存在session中的对象session.setAttribute("user", new User("xpp", 24)) 也会从内存中消失,不能一起和session序列化到磁盘,如果希望session中的对象也序列化到磁盘,那么这个对象必须实现[**序列化接口**!] 
public class User implements Serializable{}

##### 持久化session

就是自己new cookie，并给cookie设置时间

> Q:浏览器重启前后分别取获取Session数据，获取到的数据是否相同？
>
> A:会话级别cookie 不一样, 因为cookie丢了 session 找不到.  持久化级别cookie还可以同样的数据

> Q:服务器重启前后分别取获取Session数据，获取到的数据是否相同？
>
> A:条件的前提: 1.正常关闭 2.如果存入的是对象必须实现序列化接口 (因为只有对象实现序列化接口才会被session钝化时一起序列化到磁盘中)，获得数据是相同的

使用cookie的不好处 : 不够安全 , session会造成服务器压力的问题  , session不能无限的存储数据(使用缓存服务器redis 专门用于处理缓存的问题)

### URL重写

当用户禁用浏览器的cookie之后,浏览器就不能存放cookie了,但是session底层依赖于cookie,若还想使用session的话,就需要对**所有的url进行url重写**(在访问地址后面自己拼接一个jsessionid)

HttpServletResponse接口中提供了重写的方法,这时候JSESSIONID就编写了url的一部分

* encodeUrl(url):其他的连接使用此方法
* endcodeRedirectUrl(url):重定向的连接就使用此方法

### JSP

#### 概述

动态网页中，绝大部分内容都是固定不变的，只有局部内容需要动态产生和改变。Servlet输出静态网页是一件十分痛苦的事情。为了弥补Servlet的这种缺陷,SUN公司在Servlet的基础上推出了jsp（**Java Server Pages**）技术。将Java代码和HTML语句混合在同一个文件中编写

> jsp页面是由HTML语句 (输出静态部分)  和 Java代码(输出动态部分) 组成的文件，其后缀名为.jsp，**jsp的本质是一个Servlet**

![image-20210420091630593](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210420091630593.png)

##### JSP九大内置对象

![image-20210419204353565](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210419204353565.png)

#### EL表达式

jsp可以嵌套java 代码,使用脚本元素,不过太麻烦已经淘汰

使用EL(Expression Language从域中获得数据)+jstl(Jsp Standard Tag Library获得数据后遍历,判断)

> jstl(标准标签库)其实本质就是标签, 含一定逻辑的标签,主要在页面做判断和循环操作,有五个子库,core(常用),国际化(几乎不用),SQL(过时),XML(过时),Functions(几乎不用)

##### el语法

```markdown
语法 ${表达式} //只能从域对象中获取数据

* ${pageScope.键名} //从page域中获取指定键名对应的值

* ${requestScope.键名} //从request域中获取指定键名对应的值

* ${sessionScope.键名} //从session域中获取指定键名对应的值

* ${applicationScope.键名} //从application域中获取指定键名对应的值

* 简化写法:

 `${键名}`:从最小域到最大域依次查找,若找到,立即返回且终止查找;若没有找到,返回""
```

```markdown
简单类型：${键名}

对象类型：${键名.属性名 去掉get首字母小写}

单列集合类型：${键名[索引]}

双列集合类型：${键名["key"]}

往域中放入的数据的时候,尽量不用在属性名使用"."和数字
```

##### jstl语法

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--页面的前几行 必须引入jstl类库--%>

<html>
<head>
    <title>Title</title>
</head>
<body>
    <%--
        使用
        if(条件表达式){
            条件成立的情况
        }
        <c:if test="条件表达式"> 条件表达式支持el语法
            条件成立的情况
        </c:if>
    --%>
    <c:if test="${2>1}">
        大于
    </c:if>
    <%--------------------------------------%>
    <%--
    	<c:forEach items="遍历谁" var="变量名称">
       	 	items="遍历谁 支持el表达式"
        	var="变量名称 随意书写 将var的临时变量存入当前的page域中"
        </c:forEach>
    --%>
        <h1>数组</h1>
    <c:forEach items="${arr}" var="obj">
        ${obj}<br/>
    </c:forEach>
    <%--------------------------------------%>
        <h1>map集合</h1>
    <%--
        map 遍历有两种方式  去掉get首字母小写
        entry.getKey() entry.getValue()
    --%>
    <c:forEach items="${map}" var="entry">
        ${entry.key} @@  ${entry.value}<br/>
    </c:forEach>
</body>
</html>
```

#### JSP和Servlet区别?

##### 相同点

JSP名为Java Server Page,运行在服务器端JVM之上的Servlet容器里,Tomcat调用servlet的service()方法把JSP编译成Servlet,然后执行Servlet,执行的结果是HTML并响应给浏览器,jsp的本质就是Servelet。

##### 不同点

JSP侧重视图view，Servlet用于控制逻辑

Servlet没有内置对象，JSP有九大内置对象，必须通过HTTPServletRequest对象，HttpServletResponse对象以及HttpServlet对象得到。



### MVC模式

开发中有一种程序的设计思想 MVC : Model(模型) View(视图) Controller(控制器)

#### jsp发展

- 早期只有servlet，只能使用response输出html标签，非常麻烦。

  > out.println("<HTML>")
  > ....
  > out.println("</HTML>") 调用Service拿到数据,将美工搭建的静态HTML逐句复制到Servlet中
  > 将后端数据与HTML片段拼接在一起

- 后来有了jsp，简化了servlet开发；但如果过度使用jsp，在jsp页面中写了大量的java代码和html标签，造成难于维护，难于分工协作的场景。把Java代码(用来获取后台数据)内嵌到JSP页面,方便把动态数据渲染成静态页面。实质上JSP编译后的Java类还是在out.println()输出,本质还是一个servlet

- 再后来java的web开发，借鉴了MVC开发模式，使得程序设计的更加合理。

  - servlet:接受请求,处理请求,将数据转发给jsp
  - jsp:处理数据,然后展示给用户

#### mvc介绍

- 是软件工程中的一种软件架构模式(设计思想)，它是一种分离业务逻辑与显示界面的开发思想。

- M: Model(模型),除了servlet之外其他的实体类用来处理业务逻辑或者封装数据

- V: View(视图),目前jsp就是视图

- C: Controller(控制器),目前servlet就是控制器

- 优缺点
  优点

  - 降低耦合性(代码的关联度降低)，方便维护和拓展，利于分工协作
  - 重用性高

  缺点

  - 使得项目架构变得复杂，对开发人员要求高(开发难度提升)


![image-20210420143924371](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210420143924371.png)

##### 三层架构

三层架构将mvc模式的细化,把model进一步分为**service,dao**层

> 表示层web(表现层) : web层,与浏览器进行数据交互 
>
> 业务逻辑层service(业务层) : service层,专用于处理业务数据
>
> 数据访问层(持久层) : dao(Data Access Object)层,与数据库进行数据交换的

![image-20210420144906831](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210420144906831.png)

### Filter

#### 概念与作用

web三大核心组件之一,Filter过滤器

当用户访问服务器资源时，过滤器将请求拦截下来，完成一些特殊的功能.还可以对响应信息进行拦截过来(一般是用来做数据压缩的)

#### 应用场景

一般用于完成通用的操作:如登录验证、统一编码处理、敏感字符过滤

> tomcat8.5处理了get请求的中文乱码,但是post没有,可以用过滤器拦截下来若是post请求,执行request.setCharacterEncoding("utf-8"),其他方式不处理,最后放行

#### 执行时机

filter执行时机: 执行servlet之前执行filter

![image-20210422084705671](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210422084705671.png)

注意:始终请保持整个项目中有且只有一个Filter

> void init(FilterConfig config):初始化方法,在项目启动的时候就会执行,只执行一次
>
> void doFilter(request,response,FilterChain chain):执行过滤方法,当请求匹配上的时候就会执行
>
> void destroy():销毁,在项目被移除的时候或者服务器关闭的时候就会执行,只执行一次

#### 对比servlet

sevlet默认**第一次访问**才初始化(也可以修改load-on-startup修改初始化时机)每次执行sevlet的service方法;

filter是在服务器**启动时**会创建filter对象并调用其init方法,实现初始化操作,请求到来时,filter会匹配到路径,服务器从线程池获取一个线程,在线程中调用filter的doFilter() 方法,(**默认在请求到达服务器时就开始拦截**)当方法执行并放行后才会到达下一个地方,服务器正常关闭,会调用filter的destroy 方法,实现销毁

```java
package com.gorilla.b_filter;

import javax.servlet.*;
import java.io.IOException;

/**
 * 1.创建一个类
 * 2.实现一个接口  javax.servlet.Filter
 * 3.重写方法
 */
public class HelloFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("Filter初始化");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("执行过滤的方法 AAAA");

        //放行代码
        filterChain.doFilter(servletRequest , servletResponse  ); //404

        System.out.println("执行过滤的方法 BBBB");

    }

    @Override
    public void destroy() {
        System.out.println("Filter的销毁");
    }
}

```

#### 拦截路径配置

一个路径只能有一个servlet执行,但是一个路径可以有多个filter执行.且filter中不存在路径优先级问题

* 精确路径匹配   例如:/demo1   /1.html   /a/b/demo23
* 模糊路径匹配  例如:/a/b/*   /a/*   /*   注意:(/*在servlet中最好不用使用)
* 后缀名匹配  例如: *.jsp   *.do  *.action

在filter中一般使用的**模糊路径匹配**和**后缀名匹配**

#### 过滤器链

若匹配到多个filter,他们都会执行,就像一个链子把过滤器都串联起来,他只有一个放行方法filterChain.doFilter(req,resp),执行过后移交给下一个过滤器,最后一个会执行sevlet或者jsp.

> xml配置按照顺序排列,注解配置按照**filter类名**字典顺序排列
>
> 注解要加上@WebFilter("/demo"),表示拦截的路径

#### 拦截方式

```xml
<filter-mapping>
        <url-pattern>/two</url-pattern>
        <!--默认值 , 请求开始到达服务器进行拦截-->
        <!--<dispatcher>REQUEST</dispatcher>-->
        <!--请求转发的时候进行拦截,拦截后转发到另一个请求-->
        <dispatcher>FORWARD</dispatcher>
</filter-mapping>
```

### Listener

web三大核心组件之一,Listener监听器

web程序中，监视器监视某些对象(域对象ServletRequest,HttpSession,ServletContext)，一旦其发生相应的变化，就采取相应的操作(监听器的执行时机,不是访问时,是对象进行特殊操作时)

可以采用xml配置(<listener>配置要监听的类,类实现以下三个接口,并重写一些方法,在这个方法里需要先得到事件对象)或注解配置(@WebListener)两种方式

> ServletContextListener(spring框架底层)服务启动创建,对象创建时候,可以加载数据(初始化数据,加载配置文件),服务器停止即销毁
>
> HttpSessionListener,第一次调用getSession()创建,销毁方式与session销毁相同
>
> ServletRequestListener,请求开始创建,响应结束销毁 



#### 应用场景

统计历史访问次数、统计在线人数、系统启动时初始化配置信息

```markdown
监听web三大域对象：ServletRequest,HttpSession,ServletContext
监听器一共八个(只学一个 ServletContextListener)
1. 监听域对象创建和销毁(三个 印证原来的生命周期)  :  监听对象的创建和销毁
   ServletRequestListener,HttpSessionListener,ServletContextListener
2. 监听域对象对的属性操作(三个)  :  监听对象的数据的变化
   ServletRequestAttributeListener,HttpSessionAttributeListener,ServletContextAttributeListener
3. 监听特殊javaBean在session中的特殊处理(2个)
   HttpSessionBingdingListener(监听自己被绑定、解绑到HttpSession中)
   HttpSessionActivationListener(监听自己被钝化或激活了)
主要监听这三个对象的创建和销毁及其属性变化等
```

### 文件上传

将客户的数据通过网络复制到服务器上

#### 技术

使用apache提供的工具commos-fileupload.jar

使用springmvc框架,底层封装了commos-fileupload

使用servlet3.0提供的文件上传方式

#### 前端三要素:

1. form表达提交方式必须为post
2. enctype属性要修改为multipart/form-data(多部分表单数据)

3. 在表单中提交文件选择框,也就是必须要有一个type属性为file的input标签,且需要提供name属性,如果有多个文件添加multipart/form-data属性

使用@MultipartConfig//servlet3.0提供的注解,目的解析request对象

### Ajax和axios

Ajax Asynchronous JavaScript And XmL(异步JavaScript和XML),创建交互式快速动态网页应用的网页开发技术,无需重新加载整个网页情况下,更新部分网页的技术。

他本身不是一种新技术,而是多个技术的综合,用于创建动态网页的技术,不同于一般网页在更新内容时需要重新加载整个页面,【同步，期间不能别的操作】AJAX通过于服务器的进行少量数据交换,就能使网页实现【异步，期间可以进行其它操作】实时更新,在不重新加载整个页面的情况下,对网页的部分内容进行局部更新(百度搜索栏输入某字段,返回搜索推荐)

#### 原生Ajax

太传统，代码复杂性高。

##### 流程

1. 使用ajax发送请求，编写接收请求的服务端代码
2. 使用ajax接收响应，通过dom编程完成HTML的动态展示

##### 执行过程

打开服务器的连接open(请求的方式,请求的地址)

真正发送请求send()

处理服务器的响应onchangestate()

##### 底层

ajax底层就是XMLHTTPRequest(xhr),xhr向服务器发送请求和解析服务器响应提供了接口,能够以异步方式从服务器获取新数据

#### Ajax的优势:

1. 通过异步模式,提升用户体验

2. 优化了浏览器和服务器之间的传输,减少不必要的数据往返,减少了带宽占用

3. Ajax引擎在客户端运行,承担了一部分本来由服务器承担的工作,减少了大用户量下的服务器负载.

------------

axios是一个开源的可以用在浏览器端的NodeJS(JavaScript 运行环境)的异步通信框架,作用是实现了AJAX异步通信,一个专一的异步请求框架,用于封装底层的XMLHTTPRequest

#### axios常用方法

```java
axios.get(url,[config])发送get请求
axios.post(url,[data,[config]])发送post请求
then()结束服务器响应,并处理响应结果
axios.delete(url,[config])
axios.put(url,[data,[config]])
 
 //   ----------vue对axios进行了深度封装------------------
// 执行get请求参数比较多的时候
axios.get("/url" ,{  params:{key:value,key:value}  })
 
axios.get('/url',{
    params:{
		name: 'admin',
		password: '123'
    }
}).then(resp=>{ //箭头函数
	console.log(resp.data);
})
    
//执行post请求
axios.post('/url', {
    name: 'admin',
    password: '123'
}).then(resp=>{
    console.log(resp.data);
})
```

### Nginx

nginx是高性能http和反向代理服务器

##### nginx的Master - Worker模式

每一个Worker进程都维护一个线程（避免线程切换），处理连接和请求；注意Worker进程的个数由配置文件决定，一般和CPU个数相关（有利于进程切换），配置几个就有几个Worker进程。

![image-20210723182738258](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210723182738258.png)

目前流行架构

![image-20210723182436356](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210723182436356.png)

##### 反向代理

反向代理服务器位于用户与目标服务器之间，但是对于用户而言，反向代理服务器就相当于目标服务器，即**用户直接访问反向代理服务器就可以获得目标服务器的资源**。同时，用户不需要知道目标服务器的地址，也无须在用户端作任何设定。反向代理服务器通常可用来作为Web加速，即使用反向代理作为Web服务器的前置机来降低网络和服务器的负载，提高访问效率。

![image-20210723181815172](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210723181815172.png)

##### 正向代理

![image-20210723182015478](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210723182015478.png)

##### 负载均衡

![image-20210723180853083](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210723180853083.png)





## Maven

maven的作用: 管理jar 管理项目( 引入jar包的时候 可以进行批量导入 A需要 B  B需要 C  C需要D   引入A 自动的引入BCD的jar)

maven的核心功能: 项目构建(maven管理项目从出生到完结的一个周期状态-生命周期)

![image-20210507084936106](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210507084936106.png)

project Object Model: 项目对象模型 , 实际上就是maven管理项目的配置 , 需要描述的内容 都写在此文件当中

### 生命周期和插件

![image-20210402103733760](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210402103733760.png)

* clean : 清除 字节码文件(删除缓存)  没用
* compile: 编译 没用
* test: 批量执行携带@Test注解的测试程序 没用
* package:打包, 根据打包方式 <packaging>jar/war</packaging>生成不同的文件, jar包用来引入 , war用来发布
* install:安装,将这些工程放入本地仓库,以后可以通过坐标访问 , war工程没意义
* site-了解:站点 , maven会网站发布出去 并且生成一系列的文档信息
* deploy-了解:远程发布 传输 , 私服使用 以后将我们的jar包 发布到私服服务器上(maven高级说明)

### Maven高级

maven中依赖可以传递 ,依赖传递可以减少坐标的编写,但可能会造成版本冲突

![image-20211015111245756](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211015111245756.png)



#### 解决版本冲突

##### 依赖调节原则

1. 谁先声明就先用谁
2. 路径近的优先

##### 排除依赖

通过`<exclusions>`标签排不需要的依赖jar包

![image-20211015111719032](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211015111719032.png)

##### 版本锁定(重要)



```xml
<!--常量配置-->
<properties>
    <spring.version>5.1.5.RELEASE</spring.version>
    <junit.version>4.12</junit.version>
</properties>

<!--架构师：版本锁定-->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>
    </dependencies>
</dependencyManagement>    

<!--我们导入时不需要导入版本-->
<!--程序猿：引入坐标-->
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
    </dependency>
</dependencies>
```

#### Maven工程继承

子工程继承父工程就可以用父工程的依赖,目的**消除重复代码**

父工程打包方式`<packaging>pom</packaging>`

#### Maven工程聚合

pom.xml文件可以使用<modules><module>.../maven-son</module></modules>把其他maven聚合到一起,目的是为了进行**统一操作**,通常情况**父工程会聚合管理子模块(工程)**

![image-20211015113113966](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211015113113966.png)

## Spring

### 介绍

![v2-9692a67203ff7afdb82daeae233ab273_r](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/v2-9692a67203ff7afdb82daeae233ab273_r.jpg)

Spring 是java应用 全栈轻量级开源框架

核心是**IOC**(Inverse Of Control:控制反转)和**AOP**(Aspect Oriented Programming:面向切面编程)

Spring是一个全栈应用框架,提供了表现层SpringMVC和持久层SpringJDBC和业务层事务管理等技术,还整合了众多第三方框架和类库

### IOC

IOC(Inverse Of Control:控制反转),一种设计思想,知道我们设计出更加松耦合(耦合:依赖性强)的程序

> 控制:由谁来负责对象的创建和装配
>
> 反转:之前是程序员手动在类中控制对象实例的创建,现在反转到IOC容器去创建实例和装配

IOC 让对象的创建**不用去 new** 了，可以由 spring 根据我们提供的配置文 件自动生产，我们需要对象的时候，直接从 Spring 容器中获取即可.

### 二个接口

#### BeanFactory

IOC容器的顶级接口,定义了IOC的基础功能,但功能单一,**面向Spring自身使用**

第一次**使用到某个Bean时候**(调用getBean()),才对Bean进行加载实例化(用的时候再创建)

ApplicationContext

BeanFactory的衍生接口,拓展了BeanFactory,**面向程序员使用**

容器**启动时**,一次性创建并加载所有Bean(初始化时候全部创建好)

> 两种创建对象方式都是单例,只是创建对象的时机不同

### 三个实现类

`ClassPathXmlApplicationContext`

//读取类路径（classpath）下的xml配置文件

`FileSystemXmlApplicationContext`【一般不用】

//读取本地磁盘下的xml配置文件

`AnnotationConfigApplicationContext`

//读取java配置类加载配置

### 一个方法

`getBean(String name)`//通过指定id获取对象实例,需要强转

`getBean(Class<T> requiredType)`//通过指定类型获取对象实例,不要强转

`getBean(String name, Class<T> requiredType)`//通过指定id和类型获取对象实例

```java
@Test
public void test02()throws Exception{
    // 1.让spring解析xml配置文件，初始化IoC容器
    ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
    // 2.从ioc容器获取dao实现

    // 通过指定id获取对象的实例，需要手动强转
    // UserDao userDao = (UserDao) app.getBean("userDao");

    // 通过指定类型获取对象的实例，不需要强转，如果该接口下在ioc容器中出现多个实现，那么就报错了
    // UserDao userDao = app.getBean(UserDao.class);

    // 通过指定id和类型获取对象的实例
    UserDao userDao = app.getBean("userDao", UserDao.class);

    // 3.测试
    userDao.save();
}
```

```xml
<bean id="userDao2" class="com.gorilla.dao.impl.UserDaoImpl2"></bean>
<bean id="userDao" class="com.gorilla.dao.impl.UserDaoImpl"></bean>
```

![image-20210330100143990](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210330100143990.png)

### 配置文件

#### Bean标签基本配置

```xml
<!--
    基本配置
        id：此对象实例在ioc容器的唯一标识
        class：此对象的全限定类名]
	init-method 指定对象初始化时触发的方法
 	destroy-method  对象销毁前触发的方法
------------------------------------------
        bean标签创建对象，默认需要该对象提供无参构造
        底层代码：clazz.newInstance();
		通过反射创建对象
-->
<bean id="userDao" class="com.gorilla.dao.impl.UserDaoImpl" scope="prototype" init-method="init" destroy-method="destroy"></bean>  
```

#####  scope属性的取值

| scope取值        | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| **singleton**    | 默认值，单例对象  在ioc容器初始化时创建,在ioc容器关闭时候销毁 |
| **prototype**    | 多例对象   调用时ioc创建,对象失去引用时JVM进行CG回收         |
| request          | WEB项目中，Spring创建一个Bean的对象，将对象存入到request域中 |
| session          | WEB项目中，Spring创建一个Bean的对象，将对象存入到session域中 |
| global   session | WEB项目中，应用在Portlet环境，如果没有Portlet环境那么globalSession   相当于   session |

#### Bean依赖注入

依赖注入(Dependency Injection **DI**),是Spring IOC的具体实现

**就是给对象中赋值的过程**

##### 注入方式一:构造方法

> 传统方式 `UserService userService = new UserServiceImpl(userDao);`

> 使用构造方法注入
>
> ```xml
> <bean id="userDao" class="com.gorilla.dao.impl.UserDaoImpl">
> <!--
> 		name:构造方法属性名
> 		value：赋予的简单数据类型（不要new关键字直接定义的数据类型）
>      ref：赋予的引用数据类型（需要在ioc容器中）
> -->
>  <constructor-arg name="id" value="18"></constructor-arg>
> </bean> 
> ```

##### 注入方式二:set方法

> 传统方式
>
> ```java
> UserService userService = new UserServiceImpl();
> userService.setUserDao(userDao);
> ```

> set方法注入,需要把对象交个ioc容器,再完成依赖注入
>
> ![image-20210330105135048](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210330105135048.png)
>
> property标签
>
> ```xml
> <property>标签：属性注入
>  name属性：属性名称
>  value属性：注入的普通属性值
>  ref属性：注入的对象引用值
> 	<list>
>  <set>
>  <array>
>  <map>
>  <props>
> ```

##### 注入方式三:P命名空间注入

> 此方式底层还是set方法

#### 配置文件模块化

当xml配置非常多时,把部分配置拆解到其他配置文件中,就是配置文件模块化

```xml
<!--将这些标签导入到主配置文件applicationContext中,加载时只用加载application.xml即可-->
<beans>
    
<import resource="applicationContext-domian.xml"></import>
<import resource="applicationContext-dao.xml"></import>
    
</beans>
同配置文件中id不能重复,如果重复,后加载会覆盖先加载的
....
```

### DbUtils

DbUtils是`Apache`的一款用于简化Dao代码的工具类，它底层封装了JDBC技术。

#### 核心对象

```java
QueryRunner queryRunner = new QueryRunner(DataSource dataSource);
```

------------

### 注解

spring轻代码重配置,配置繁琐,用注解代替xml简化配置

```xml
<!--要使得spring注解生效,就要开启注解组件扫描,在xml配置,开启注解组件扫描-->
<context:component-scan base-package="com.gorilla"/>
```

#### IOC控制反转

```java
- @Component：相当于<bean id="" class=""></bean>
    
 // 如果不指定value默认的名称就是 类名首字母小写
- @Repository：在持久层使用注解 

- @Service：在业务层使用注解

- @Controller：在表现层使用注解
```

#### DI依赖注入

```xml
- @Autowired ：相当于 app.getBean(class类型)  代替此标签 <property name="accountDao" ref="accountDaoImpl"/>

- @Qualifie  ：需要跟@Autowired组合使用，来指定同类型下某个具体的id

- @Resource  ：由jdk自带，相当于 @Autowired + @Qualifie
```

```xml
- @Scope：相当于<bean scope=""></bean>

- @PostConstruct：相当于<bean init-method=""></bean>

- @PreDestroy：相当于<bean destroy-method=""></bean>

- @Value ：相当于  <property name="driverClassName" value="${jdbc.driver}">
```

#### Spring新注解

```xml
- @Configuration：相当于applicationContext.xml

- @ComponentScan：相当于<context:component-scan base-package=""/>  

- @PropertySource：相当于<context:property-placeholder location=""/> 

- @Bean：将第三方类（对象）交给ioc容器

- @Import：相当于<import resource=""/>  配置类模块化
```

> **控制事务两种方式:**
>
> 1.由service控制事务并把连接对象传递到dao中
>
> 2.有service控制事务并把连接对象绑定到当前线程上(用ThreadLocal实现)

> AOP(面向切面编程)其实就是**动态代理在Spring框架里的应用**
>
> 在不破坏源代码的情况下，实现对业务层功能增强（事务控制）ex:[cglib动态代理](#cglib动态代理(spring里的用法))

> JdbcTemplate是Spring的一款用于简化Dao代码的模板工具，它底层封装了JDBC技术,直接与spring内置事务管理器无缝对接，简化开发提高效率

#### AOP(面向切面编程)

AOP(面向切面编程)基于OOP(面向对象编程)之上的一种全新思想，指导我们在不破坏源代码的情况下，实现对目标对象的增强，技术用的就是[动态代理](#动态代理)(在程序运行期间，**将某段代码**<span style="color:red">动态切入</span>到**指定方法**的**指定位置**进行运行的编程方式。) 

##### 好处

1. 程序运行期间,不修改源码情况下对方法进行功能增强
2. 逻辑清晰,开发核心业务时,不必关心增强业务的代码
3. 减少重复代码,提高开发效率,便于维护

SpringAOP为了简化动态切入这部分操作,只需要少量`声明式配置`,不需要再去编写动态代理代码,就可实现AOP编程

##### Spring做了什么事?

Spring读取配置文件中的切面信息,根据切面中的描述,将增强功能增加在目标对象的切点方法上,动态创建代理对象,最后将经过代理之后对象放入ioc 容器中.

##### 常用注解

```java
//使用@Aspect注解，标注切面类
    @Component
	@Aspect // 标注为切面类 <aop:aspect ref="transactionManager">
	public class TransactionManager {

//使用@Before、@AfaterReturnning等注解，标注通知方法
    @Before("myPointcut()") // <aop:before method="before" pointcut-ref="myPointcut"/>
    public void before() {
        System.out.println("前置通知【开启事务】");
    }
    //执行顺序 @Before-->@After-->@AfterRunning(异常@AfterThrowing)
    可以使用环绕通知@Around

//使用@Pointcut注解，抽取切点表达式
    @Pointcut("execution(* com.gorilla.service..*.*(..))")
    public void myPointcut() {
    }

* 配置aop自动代理 <aop:aspectj-autoproxy/> 或 @EnableAspectJAutoProxy
```

```java
//使用四大通知写错顺序可能会有错误,使用环绕灵活解决
// 环绕通知 Proceeding + JoinPoint
@Around("myPointcut()")
public void around(ProceedingJoinPoint pjp) {
 try {
     this.before();
     // 调用切点（目标对象的方法）原有的功能
     pjp.proceed();
     this.afterReturning();
 } catch (Throwable throwable) {
     throwable.printStackTrace();
     this.afterThrowing();
 } finally {
     this.after();
 }
}
```



--------------

以下代码便于理解

MyAccountService

```java
@Service
public class MyAccountService {
    @Autowired
    private AccountDao accountDao;



    public void transfer(String outUser, String inUser, Double money) {

        // 业务操作 start
        accountDao.outAccount(outUser, money); // 转出

        int i = 1 / 0; // 制造bug

        accountDao.inAccount(inUser, money); // 转入
        // 业务操作 end
    }
}
```

CGLIBProxyFactory

```java
@Component
public class CGLIBProxyFactory {

    @Autowired
    private TransactionManager transactionManager;

    public Object createCGLIBProxy(Object target) {
        Object cglibProxy = null;

        // 使用第三方cglib技术（spring框架内置cglib的工具类）
        /*
            第一个参数：目标对象的字节码对象
            第二个参数：代理增强实现逻辑（接口）
         */
        cglibProxy = Enhancer.create(target.getClass(), new MethodInterceptor() {

            /*
                   proxy 工具类生产的代理对象
                   method 调用的目标方法
                   args 该方法传递的实际参数
                   methodProxy  等同method
             */
            @Override
            public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {

                try {
                    transactionManager.begin();
                    // 调用目标对象原有功能
                    Object result = method.invoke(target, args);
                    transactionManager.commit();
                    return result;
                } catch (Exception e) {
                    e.printStackTrace();
                    transactionManager.rollback();
                } finally {
                    transactionManager.release();
                }
                return null;
            }
        });

        return cglibProxy;
    }
}
```

测试类

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(locations = {"classpath:applicationContext.xml"})
public class AccountTest {

    @Autowired
    private MyAccountService myAccountService;

    @Autowired
    private CGLIBProxyFactory cglibProxyFactory;

    @Test
    public void test02() throws Exception {
        MyAccountService cglibProxy = (MyAccountService) cglibProxyFactory.createCGLIBProxy(myAccountService);
        cglibProxy.transfer("tom", "子涵妹妹", 300d);
    }
}
```

----------------

### ThreadLocal

ThreadLocal叫做**线程局部变量**,这个变量对其他线程是隔离的,在对象跨层传递时,使用ThreadLocal可以避免多次传递,打破层次间的约束.

应用场景:**进行事务操作,用于存储线程事务信息**

 通过ThreadLocal完成线程绑定connection对象,实现线程内对象共享,ConnectionHolder工具类

> ThreadLocal是除了加锁这种同步方式之外的一种保证一种规避多线程访问出现线程不安全的方法，当我们在创建一个变量后，如果每个线程对其进行访问的时候访问的都是线程自己的变量这样就不会存在线程不安全问题。

| 方法名            | 作用                     |
| ----------------- | ------------------------ |
| void set(T value) | 把value绑定到当前线程上  |
| T get()           | 获取当前线程上绑定的内容 |
| void remove()     | 移除当前线程上绑定到内容 |

### Spring事务

#### 编程式事务

将业务代码和事务代码放在一起,耦合性高,开发不使用

#### 声明式事务

将事务代码和业务代码隔离开发,通过一段配置让他们组装运行,最后达到事务控制的目的(通过AOP实现)

#### 事务相关API

##### PlatformTransactionManager

spring事务管理器的顶级接口，里面提供了我们常用的操作事务的方法,其子类负责具体工作

```markdown
- TransactionStatus getTransaction(TransactionDefinition definition);
	功能：获取事务的状态信息

- void commit(TransactionStatus status)；
	功能：提交事务

- void rollback(TransactionStatus status);
	功能：回滚事务
------------------------------------------------------	
TransactionDefinition  定义了事务的一些相关参数
spring事务定义参数的接口，比如定义：事务隔离级别、事务传播行为、是否只读、是否超时
**

| 隔离级别         | 含义                                                      
| DEFAULT          | 使用数据库默认的隔离级别                                     
| READ_UNCOMMITTED | 可能导致脏读、幻读或不可重复读。                             
| READ_COMMITTED   | (Oracle 默认级别)可防止脏读，但幻读和不可重复读仍可能会发生。 
| REPEATABLE_READ  | (MySQL默认级别)可防止脏读和不可重复读，但幻读仍可能发生。    
| SERIALIZABLE     | 确保不发生脏读、不可重复读和幻影读。             ---------------------------------------------------          
TransactionStatus 代表事务运行的一个实时状态
我们定义参数，会返回事务实时状态，进行提交/回滚等操作


三者关系
```

> **事务管理器**通过读取**事务定义参数**进行事务管理，然后会产生一系列的**事务状态**。

#### 使用事务

这里使用注解方式@Transactional

![image-20210402194106151](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210402194106151.png)

### spring集成web

servlet中的方法如何获取spring的容器对象？
spring提供了ContextLoaderListener监听器，加载spring环境，设置到ServletContext域,再使用WebApplicationContextUtils方法获得

```java
@WebServlet("/UserServlet")
public class UserServlet extends HttpServlet {

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        //每次使用都要调用doXX方法,里面创建了spring容器,使用完就销毁,浪费性能,
        //可以让spring容器在项目中初始化一次
        //利用ServerletContextListener监听器,tomcat启动创建ServletContext对象触发监听方法,在这个监听方法中创建spring容器,并把容器存储到servletContext域中,servlet就可以直接获取spring容器对象了
        //将获取域对象并强转为ApplicationContext,将这个方法封装到工具类中
        //以上步骤Spring提供的工具类 WebApplicationContextUtils已经帮我们做了
        
        UserService userService = app.getBean(UserService.class);
        userService.save();

        response.getWriter().write("ok");
    }

}
```

### SpringMVC

一个工作再web层的框架,底层封装了servlet,解决了Servlet代码书写繁琐的问题

基于mvc模式的web轻量级框架,本质是一个servlet,封装了共有的行为,开发人员只需关注特有行为三大组件:HandlerMapping(处理映射器),HandlerAdapter(处理器适配器),ViewResolver(视图解析器)

#### 工作原理

![image-20210403091617266](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210403091617266.png)

SpringMVC把Servlet一些通用功能进行抽取和封装,使用了之后web部分分成两部分

1. 前端控制器,有SpringMVC提供,主要负责接收参数和返回页面数据
2. 处理器:程序员编写,主要负责参数的处理和业务层调用

#### 工作流程

![image-20210403100415131](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210403100415131.png)

![image-20210403101426602](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210403101426602.png)

##### 三大组件

处理器映射器:HandlerMapping,根据URL寻找对应的处理器方法(通过XML,注解等去查找)

处理器适配器:HandlerAdapter,负责真正的去调用某个处理器的方法

视图解析器:ViewResolver,负责把逻辑视图转换成物理视图

#### 常用注解

`@Controller`,把Controller交个Spring容器管理

`@RequestMapping`用于建立请求 URL 和处理请求方法之间的对应关系

> 类上用:一级路径/user
>
> 方法上用:二级路径/findAll
>
> 完整路径:/user/findAll
>
> 常用属性:
>
> * value / path 指定当前方法的url地址,一个方法可以绑定多个地址
> * params:限定前端请求,必须携带指定参数
> * method:限定请求方式:get、post、put、delete等
>
> ```java
> @RequestMapping(path = {"/hello","/haha"},params = {"username","password"},method = {RequestMethod.POST,RequestMethod.GET})
> public String hello() {
>     System.out.println("hello方法执行了...");
>     return "success";
> }
> ```

##### 请求相关注解

`@RequestParam`用于**接收前端请求参数**,通常使用在方法的形参上

> required：默认值true，要求前端必须提供参数，否则报错
> defaultValue：给参数提供默认值
> name：接收指定前端的参数名

`@RequestHeader`相当于request.getHeader("请求头的名称")

> 一个网络请求路由到目标机器后，目标机器总要知道它要请求什么，以什么编码方式传输，数据长度等信息，这是表头要表达的内容

请求参数类型

​	简单类型,对象类型,数组类型,集合类型

#### SpringMVC文件上传

![image-20210403150029954](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210403150029954.png)

##### 文件上传前端要素:

1. form表单提交方式必须为post

2. enctype属性要修改为:pultipart/form-data

3. 必须有一个type属性为file的input标签,期需要有一个name属性,如果要上传多个文件需要添加multiple属性

   >index.jsp
   >
   >```html
   ><form action="/fileUpload" method="post" enctype="multipart/form-data">
   >姓名：<input type="text" name="username"> <br>
   >头像：<input type="file" name="picFile"> <br>
   ><input type="submit" value="提交">
   ></form>
   >```

##### 文件上传步骤:

1. 导入文件上传需要的commons-fileupload包

   > ```xml
   > <!--文件上传工具包-->
   > <dependency>
   >  <groupId>commons-fileupload</groupId>
   >  <artifactId>commons-fileupload</artifactId>
   >  <version>1.4</version>
   > </dependency>
   > ```

2. 配置文件上传解析器,springmvc的配置文件的文件上传解析器的id属性必须为multipartResolver

   > spring-mvc.xml
   >
   > ```xml
   > <!--
   >  文件上传解析器
   >      id="multipartResolver" 这个值是固定的
   > -->
   > <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
   >  <!--
   >      限制文件大小，单位B
   >          1KB = 1024B
   >          1MB = 1024KB
   >  -->
   >  <property name="maxUploadSize" value="1000000"></property>
   > </bean>
   > ```

3. 后端对应的接收文件方法参数类型必须为MultipartFile,参数名称必须与前端的name属性保持一致

   > UserController
   >
   > ```java
   > @RequestMapping("/fileUpload")
   > public String fileUpload(String username, MultipartFile picFile) throws IOException {
   >  System.out.println(username);
   > 
   >  System.out.println(picFile.getOriginalFilename()); // 原始文件名
   >  picFile.transferTo(new File("E:\\"+picFile.getOriginalFilename())); // 文件io复制，报错文件
   >  return "success";
   > }
   > ```

#### SpringMVC响应

![image-20210405084821850](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210405084821850.png)

##### 页面跳转-请求转发

1. 自定义视图解析器

2. servlet原生API

   > `request.getRequestDispatcher(path:"").forward(request, response);`

3. forward关键字(底层依然是servlet原生API)

   ![image-20210405085623150](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210405085623150.png)

###### 转发时携带数据(request域)

1.servlet原生API

> <body><h3>${message}</h3></body>
>
> `request.setAtribute("message","数据")` 

2.modelandview

> <body><h3>${message}</h3></body>
>
> 参数为ModelAndView,`modelAndView.addObject("message","数据")`

3.Model

> <body><h3>${message}</h3></body>
>
> 参数为Model `model.addAttribute("message","数据")`

##### 页面跳转-重定向

1.servlet原生API

> `response.sendRedirect(request.getContextPath()+"/index.jsp")`

2.redirect关键字

> 相当于：`response.sendRedirect(request.getContextPath() + "/index.jsp");`
>
> redirect关键字不需要我们再去拼接`request.getContextPath()`，直接写重定向地址即可

![image-20210405091155329](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210405091155329.png)

#### Ajax异步交互

springmvc中主要使用两个注解`@RequestBody`和`@ResponseBody`两个注解实现的

##### @RequestBody

接收前端传递的请求体中的json数据,可以自动转换封装进指定的对象中

##### @ResponseBody

把controller方法返回的对象通过转换器转换为指定的格式(通常为json),写入到reseponse对象的响应体中

> ```java
> @RequestMapping("/ajaxApi")
> @ResponseBody // 将user转为json，设置到响应体返回到浏览器
> public User ajaxApi(@RequestBody User user) { // 将前端请求体的json转为java对象【底层使用了jackson】
>  System.out.println(user);
>  user.setUsername("lucy");
>  return user; 
> }
> ```

### RESTful

Restful是一种软件架构**风格**、设计风格，而不是标准，只是提供了一组设计原则和约束条件。

主要用于**前后端分离的架构**，基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存机制等。

![image-20210405102647699](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210405102647699.png)

**Restful规范了HTTP请求动作,使用四个词语分别表示对资源的CRUD操作:** 

- GET(获取)、POST(新建)、PUT(更新)、DELETE(删除) 

```java
@RestController //  @Controller + @ResponseBody
@RequestMapping("/student")
public class StudentController {

    // @RequestMapping(value = "/student", method = RequestMethod.GET)
    @GetMapping
    // @ResponseBody
    public String findAll() {
        return "findAll";
    }

    // @RequestMapping(value = "/student/{id}", method = RequestMethod.GET)
    @GetMapping("/{id}")
    // @ResponseBody
    public String findById(@PathVariable Integer id) {
        return "findById:" + id;
    }

    // @RequestMapping(value = "/student", method = RequestMethod.POST)
    @PostMapping
    // @ResponseBody
    public String save() {
        return "save";
    }

    // @RequestMapping(value = "/student", method = RequestMethod.PUT)
    @PutMapping
    // @ResponseBody
    public String update() {
        return "update";
    }

    // @RequestMapping(value = "/student/{id}", method = RequestMethod.DELETE)
    @DeleteMapping("/student/{id}")
    // @ResponseBody
    public String delete(@PathVariable Integer id) {
        return "delete:" + id;
    }
}
```

#### 异常处理

​	使用Spring框架后,我们的代码最终是由框架来调用的。也就是说,异常最终会抛到框架中, 然后由框架指定异常处理器来统一处理异常。

![image-20210405105841457](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210405105841457.png)

##### 方式一:自定义异常处理器

> 创建一个类,实现HandlerExceptionResolver接口

##### 方式二:@ControllerAdvice

> @ControllerAdvice和@ExceptionHandler配合使用,当异常抛到controller层时,可以对异常进行统一处理
>
> @ControllerAdvice写在类上表明该类是一个全局异常处理类
>
> @ExceptionHandler写在方法上,ExceptionHandler中有一个value属性,可以指定处理的异常类型`@ExceptionHandler(NullPointerException.class) // 专门捕获空指针异常`

```java
@ControllerAdvice
public class MyControllerAdvice {


    @ExceptionHandler(Exception.class)
    public String ExceptionHandler(Exception ex,Model model){
        ex.printStackTrace();
        model.addAttribute("error", "服务器繁忙...");
        return "forward:/WEB-INF/error.jsp";
    }



    @ExceptionHandler(NullPointerException.class) // 专门捕获空指针异常
    public String NullExHandler(NullPointerException npe, Model model) {
        npe.printStackTrace();
        model.addAttribute("error", "这是一个空指针异常");
        return "forward:/WEB-INF/error.jsp";
    }

    @ExceptionHandler(ArithmeticException.class) // 专门捕获数学异常
    public String ArithmeticExceptionHandler(ArithmeticException ae,Model model){
        ae.printStackTrace();
        model.addAttribute("error", "这是一个数学异常");
        return "forward:/WEB-INF/error.jsp";
    }
}
```

#### 拦截器

SpringMVC拦截器类似于Servlet中的过滤器Filter,用于对处理器进行**预处理**和**后处理**

![image-20210405143821268](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210405143821268.png)

![image-20210405145832839](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210405145832839.png)

定义拦截器类:SpringMVC为我们提供了拦截器规范接口,创建一个类实现HandlerInterceptor,并重写其中的抽象方法

* preHandle方法:**调用处理器前**调用这个方法,如果这个方法返回true则请求继续向下执行,否则不会继续向下进行,处理器也不会调用

- postHandle方法:调**用完处理器后**调用这个方法,可以获取处理器方法返回结果ModelAndView
- afterCompletion方法,在前端控制器**渲染页面完成后**调用这个方法

```java
public class MyIntercepter1 implements HandlerInterceptor {


    /*
        预处理方法：拦截请求
        参数：
            request
            response
            handler：用户访问的目标方法
        返回结果：
            true：放行
            false：拦截
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        System.out.println("preHandle1");
        return true;
    }

    /*
        后处理：可以获取处理器方法返回结果【ModelAndView】
        参数：
            request
            response
            handler：目标处理器方法
            modelAndView：该目标方法执行后返回结果

     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) {
        System.out.println("postHandle1");
    }

    /*
        视图渲染完处理的：资源释放
            参数：
                request
                response
                handler
                ex：异常
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        System.out.println("afterCompletion1");
    }
}
```

##### 拦截器链

拦截器可以单独使用,也可以同时使用多个拦截器形成一条拦截器链,开发步骤相同,只是注册时候注册多个,按照配置的注册顺序就代表拦截器的执行顺序,执行顺序为`先进后出`

## SSM

![image-20210406084144011](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210406084144011.png)

#### 分页插件

PageHelper国内非常优秀的一款开源的 mybatis 分页插件，它支持基本主流与常用的数据库

官网：<https://pagehelper.github.io/>



## git

![image-20210109090637555](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210109090637555.png)

### git常用命令

```markdown
---------新建代码库--------------------------
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]
---------增加删除文件------------------------
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .
---------代码提交---------------------------
# 提交暂存区到仓库区
$ git commit -m [message]
----------分支-------------------------------
# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 合并指定分支到当前分支
$ git merge [branch]
-----------撤销----------------------------
# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard
```

更多详情查看 [git常用命令](https://www.cnblogs.com/lwh-note/p/9153149.html)

### idea使用git

#### 创建与推送

1. 安装git服务器后配置到这里

启动页面全局配置,在配置之前已经创建好项目,需要在settings里重新配置

![image-20201125230851592](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20201125230851592.png)

2. 工程交给git管理

![](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211015165544557.png)

3. 配置忽略文件.ignore

   1. 下载ignore插件并安装,重启idea
   2. 选择父工程,创建.ignore忽略文件

   ![image-20210517103458886](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210517103458886.png)

4. 右键git-->add 提交到本地仓库

5. 远程仓库名要和项目名一样

6. 推送到远程仓库 VSC-->Git-->Push

#### 拉取

![image-20210109110817952](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210109110817952.png)

#### 分支

##### 创建分支

![image-20210517144823589](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210517144823589.png)



![image-20210109113948898](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210109113948898.png)

##### 合并分支

1. 回到master

![image-20210109114328740](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210109114328740.png)

2. 合并分支代码

![image-20210109114526351](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210109114526351.png)

分支合并到主干

![image-20210517145616555](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210517145616555.png)

## 简单api

```java
RandomUtil.randomNumbers(6);//生成六位随机数
```

### JWT

![image-20210410100904357](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210410100904357.png)

![image-20201216160302413](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20201216160302413.png)

JWT，全称为JSON Web token，是用于对应用程序上的用户进行身份验证的标记。也就是说，使用JWT的应用程序不再需要保存有关其用户的cookie或session数据

#### 工具类JWTUtil 

创建token和解析token



## SpringBoot

Spring虽然组件代码轻量,但整合配置却非常繁琐

> 添加框架或技术,要添加响应maven坐标,还可能导致依赖冲突,并且需要大量的配置

SpringBoot基于<span style="color:red">约定优于配置</span>,按照其规定方式去开发代码,无需编写复杂配置

> **版本锁定**
>
> * 集合常用maven依赖版本,解决依赖容易冲突
>
> **起步依赖**
>
> * 集合了常用jar包
>
> **自动配置**
>
> * 集合了约定的默认配置
>
> **内置Tomcat**
>
> * 不需要外置Tomcat就可直接运行javaEE

@SpringBootApplication注解里主要包含了以下四类注解

>  jdk原生注解4个(指明适用范围,生命周期,等)
>
>  * @SpringBootConfigration
>
>   > 其中本质是@Configration,定义该类是个配置类(所以启动类也能当配置类)功能等同于applicationConfigration.xml文件
>   >
>   > > @Configration和@Bean可以创建一个简单spring配置类,可以理解为创建一个ioc容器
>
>  * @EnableAutoConfigration
>
>   > 本质是@Import({AutoConfigrationImportSelector.clas})
>   >
>   > > **自动装配AutoConfigrationImportSelector这个类内部提供了一个方法(selectImport),这个方法会扫描导入的所有jar包下的spring.factories文件,解析文件中自动配置类key=value,将列表中的类创建并放到ioc容器中**  
>   >
>   > >  EnableAutoConfigration和@Import搭配,将所有符合自动配置条件的bean定义加载到ioc容器
>   >
>   > @EnableAutoConfigration会对类路径中的jar包依赖进行自动配置,如maven中添加了spring-boot-starter-web,就自动添加Tomcat和SpringMVC的依赖,SpringBoot会对Tomcat和SpringMVC进行**自动配置**
>
>  * @ComponentScan()
>
>   > 自动扫描,并加载符合条件的组件(比如@Component)或者@Bean定义,最终将Bean定义加载到IOC容器中
>   >
>   > > 通过basePackages来控制@ComponentScan()扫描范围,不指定,默认扫描**@ComponentScan所在包和子包进行扫描**,也是为什么会将启动类放在根目录下的原因
>
>   启动类run()方法将启动类的字节码传入@SpringBootApplication注解,通过当前启动类的核心信息,创建ioc容器

### 使用

#### 新建maven项目

#### 导入坐标

```xml
<!--在各种starter中，定义了完成该功能需要的坐标合集，其中大部分版本信息来自于父工程-->

<!--设置父工程: 里面进行版本的锁定-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.2.RELEASE</version>
</parent>

<dependencies>
    <!--web环境起步依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--lombok-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
</dependencies>
```

#### 自定义配置文件

SpringBoot基于约定的,要替换默认配置,可以在 resources创建: `application.yml/yaml`或者`application.properties`

##### yaml

```yaml
# 简单格式，坑 key命名不能使用（username）
#区分大小写,冒号后必须有空格
age: 18

# 对象格式
user:
  username: jack
  age: ${age}
  gender: female


# 集合格式
addressList:
  - '北京'
  - '上海'
  - '广州'
```

#### 一些API

```java
@SpringBootApplication //用在启动类上,告诉springboot我是一个启动类
```

```java
@Value //spring提供的注解,读取配置文件中的属性值并逐个注入到Bean对象的对应属性中

Environment //表示整个应用运行时环境,读取配置文件中的属性值并逐一注入到bean
```

![image-20211017170438809](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211017170438809.png)

```java
@ConfigurationProperties //SpringBoot 提供,把配置文件的自定义属性批量注入到某个Bean对象的多个对应属性中
//导入一个spring-boot-configuration-processor坐标解决此注解爆红
```

![image-20210704100000264](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210704100000264.png)

SpringBoot启动流程

![image-20210704102947841](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210704102947841.png)

SpringBoot中推荐基于Java Config的方式来代替传统的XML方式去引入Bean

在原生SpringFramework中，装配组件有下面三种方式

1. 使用@Component注解，Spring2.5+
2. 使用配置类@Configuration与@Bean，Spring3.0+
3. 使用模块装配@EnableXXX与@Import，Spring3.1+

### SpringBoot整合其他框架

#### 整合Junit

spring提供了junit起步依赖,在pom.xml导入

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
</dependency>
```

```java
@RunWith(SpringRunner.class) // 切换运行器
@SpringBootTest// 读取启动类，要求测试基础包跟启动类基础包一致
public class QuickTest {

    @Autowired
    private UserProperties userProperties;

    @Test
    public void test01() throws Exception {
        System.out.println(userProperties);
    }
}
```

#### 整合SpringMVC

##### 拦截器配置

> 自定义一个springMVC拦截器类，实现 `HandlerInterceptor` 接口

```java
@Component
public class MyHandlerInterceptor implements HandlerInterceptor {

    // 预处理拦截
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle拦截了");
        return false;
    }
}
```

> 自定义一个springMVC的配置类，实现`WebMvcConfigurer`接口

```java
@Configuration // 配置类
public class MyMVCConfiguration implements WebMvcConfigurer {

    @Autowired
    private MyHandlerInterceptor myHandlerInterceptor;

    // 注册拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myHandlerInterceptor)
                .addPathPatterns("/img/**") // 拦截路径
                .excludePathPatterns("/img/1syn.jpg"); // 放行路径
    }
}
```

#### 整合Mybatis

```xml
<!--调整mysql的版本-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.6</version>
</dependency>
<!--加入mybatis的起步依赖,这是mybatis公司提供的-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>
```

##### mapper映射

```java
public interface UserMapper {

    List<User> findAll();
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gorilla.mapper.UserMapper">

    <select id="findAll" resultType="User">
        SELECT * FROM tb_user
    </select>

</mapper>
```

```yml
# 连接池指定数据库连接四项
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springboot_db
    username: root
    password: root


# mybatis配置
mybatis:
  mapper-locations: classpath:mapper/*.xml  # 指定映射文件目录
  type-aliases-package: com.gorilla.domain # 指定实体别名
  configuration:  # nickName  自动映射 nick_name
    map-underscore-to-camel-case: true
```

###### 把mapper代理对象到ioc容器

方式1

```java
@Mapper //写在mapper接口上,把mapper接口加载到ioc容器并且创建代理对象
```

 方式2(常用)

```java
@MapperScan("com.gorilla.mapper")//写在启动类上,把mapper包下接口交个ioc容器并创建代理对象
```

#### 整合Druid连接池

```xml
<!--druid依赖-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.15</version>
</dependency>
```

修改application.yml

![image-20211017172543095](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211017172543095.png)

#### 整合Mybatis Plus

Mybatis增强工具,简化开发提高效率,已经封装好CRUD方法,不需要写mapper.xml,直接调用方法

##### 使用

1. 导入依赖

```xml
<dependency>
	<groupId>com.baomidou</groupId>
	<artifactId>mybatis-plus-boot-starter</artifactId>
	<version>3.4.0</version>
</dependency>
```

2. mapper接口继承BaseMapper<T>

```java
public interface UserMapper extends BaseMapper<User> { //这里的泛型是实体类
}
```

3. 数据源配置

```yaml
# 数据源配置
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/springboot_db?useSSL=false
    username: root
    password: root

# 日志
logging:
  level:
    com.gorilla: debug
```

4. service里注入mapper,直接使用
5. controller里注入service
6. 启动类上加`@MapperScan("com.gorilla.mapper")`//把mapper包下接口交给ioc容器,并创建代理对象

##### 条件查询

| 查询方法  | 说明              |
| --------- | ----------------- |
| eq        | 等于=             |
| ne        | 不等与<>          |
| gt        | 大于>             |
| ge        | 大于等于>=        |
| lt        | 小于<             |
| le        | 小于等于<=        |
| like      | 模糊查询 LIKE     |
| notLike   | 模糊查询 NOT LIKE |
| in        | IN 查询           |
| notIn     | NOT IN 查询       |
| isNull    | NULL 值查询       |
| isNotNull | IS NOT NULL       |

```java
    // 创建条件对象
    QueryWrapper<User> qw = new QueryWrapper<>();  // where
    qw.like("nick_name", "王"); // nick_name like '%王%'
    qw.gt("age", 23); // age > 23
    qw.orderByDesc("age"); // ORDER BY age DESC
```

```java
public interface UserMapper extends BaseMapper<User> {

    // 自定义分组查询sql语句
    @Select("SELECT age ,COUNT(age) FROM tb_user GROUP BY age")
    public List<Map> findAgeCount();
}
```

##### 分页查询

```java
//Spring boot方式
@Configuration
public class MybatisPlusConfig {
    // 最新版分页拦截器
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));
        return interceptor;
    }
}
```

```java
// 分页查询
@Test
public void test08() throws Exception {
    // 1.开启分页
    Page<User> page = new Page<>(1, 5);
    // 2.查询分页
    page = userMapper.selectPage(page, null);

    // 3.获得结果
    System.out.println(page.getTotal());// 总记录数
    System.out.println(page.getPages()); // 总页数
    System.out.println(page.getCurrent()); // 当前页
    System.out.println(page.getSize()); // 每页格式
    System.out.println(page.getRecords()); // 分页list集合
}
```

#### 整合Redis

1. 启动redis服务

2. 导入依赖

```xml
<!--redis 起步依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

3. 配置文件

![image-20211017210714494](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211017210714494.png)

4. 实体类实现序列化接口 implements Serializable

5. 注入RedisTemplate,使用这个模板去操作

   > API
   >
   > ```java
   > //操作string类型
   > redisTemplate.opsForValue();
   > 
   > //操作hash类型
   > redisTemplate.opsForHash();
   > 
   > //操作list类型
   > redisTemplate.opsForList();
   > 
   > //操作set类型
   > redisTemplate.opsForSet();
   > 
   > //操作有序set类型
   > redisTemplate.opsForZSet();
   > ```

##### 模板对象

| 模板对象            | 序列化方式                      | 序列化效果                |
| ------------------- | ------------------------------- | ------------------------- |
| RedisTemplate       | JdkSerializationRedisSerializer | \xac\xed\x00\x05t\x04haha |
| StringRedisTemplate | StringRedisSerializer           | haha                      |

### SpringBoot工作原理

#### 自动装配

参考源码`@EnableAutoConfiguration` 注解内部使用 `@Import(AutoConfigurationImportSelector.class)`

读取`META-INF/spring.factories`，该配置文件中定义了大量的配置类，当 SpringBoot 应用启动时，会自动加载这些配置类，初始化Bean

> @Import(AutoConfigurationImportSelector.class)是的spring.factories中的类被扫描

![image-20210705103843568](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210705103843568.png)

并不是读取`META-INF/spring.factories`所有的Bean都会被初始化，在配置类中使用`@Condition`来加载满足条件的Bean

* ConditionalOnClass：判断环境中是否有对应字节码文件才初始化Bean
* ConditionalOnProperty：判断配置文件中是否有对应属性和值才初始化Bean
* ConditionalOnMissingBean：判断环境中没有对应Bean才初始化Bean

![image-20210705110250793](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210705110250793.png)

#### 例自定义云存储起步依赖 oss-spring-boot-starter

![image-20210705111633846](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210705111633846.png)

1. 导入oss依赖

```xml
	<!--阿里云oss-->
    <dependency>
        <groupId>com.aliyun.oss</groupId>
        <artifactId>aliyun-sdk-oss</artifactId>
        <version>3.10.2</version>
    </dependency>
```

2. 编写OssProperties

```java
@Data
@ConfigurationProperties("oss")
public class OssProperties {

    //区域
    private String endpoint;
    //访问id
    private String accessKeyId;
    //访问秘钥
    private String accessKeySecret;
    //桶名称
    private String bucketName;
    //访问URL
    private String url;
}
```

3. 编写OssTemplate

```java
public class OssTemplate {

    //区域
    private String endpoint;
    //访问id
    private String accessKeyId;
    //访问秘钥
    private String accessKeySecret;
    //桶名称
    private String bucketName;
    //访问URL
    private String url;

    public OssTemplate(OssProperties ossProperties) {
        this.endpoint = ossProperties.getEndpoint();
        this.accessKeyId = ossProperties.getAccessKeyId();
        this.accessKeySecret = ossProperties.getAccessKeySecret();
        this.bucketName = ossProperties.getBucketName();
        this.url = ossProperties.getUrl();
    }

    //文件上传
    public String upload(String fileName, InputStream inputStream) {

        // 创建OSSClient实例。
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        // 上传文件流。
        // <yourObjectName>表示上传文件到OSS时需要指定包含文件后缀在内的完整路径，例如 images/2020/11/11/asdf.jpg。
        String objectName = "images/" + new SimpleDateFormat("yyyy/MM/dd").format(new Date())
                + "/" + System.currentTimeMillis() + fileName.substring(fileName.lastIndexOf("."));

        // meta设置请求头,解决访问图片地址直接下载
        ObjectMetadata meta = new ObjectMetadata();
        meta.setContentType(getContentType(fileName.substring(fileName.lastIndexOf("."))));
        ossClient.putObject(bucketName, objectName, inputStream, meta);

        // 关闭OSSClient。
        ossClient.shutdown();

        return url + "/" + objectName;
    }

    public String getContentType(String FilenameExtension) {
        if (FilenameExtension.equalsIgnoreCase(".bmp")) {
            return "image/bmp";
        }
        if (FilenameExtension.equalsIgnoreCase(".gif")) {
            return "image/gif";
        }
        if (FilenameExtension.equalsIgnoreCase(".jpeg") ||
                FilenameExtension.equalsIgnoreCase(".jpg") ||
                FilenameExtension.equalsIgnoreCase(".png")) {
            return "image/jpg";
        }
        return "image/jpg";
    }
}
```

4. 编写OssAutoConfiguration

```java
@Configuration // 配置类
@EnableConfigurationProperties(OssProperties.class) // 读取配置文件属性对象
@ConditionalOnClass(OSS.class) // 满足项目导入阿里云坐标才能触发自动装配
public class OssAutoConfiguration {

    @Bean
    public OssTemplate ossTemplate(OssProperties ossProperties) {
        return new OssTemplate(ossProperties);
    }
}
```

#### 编写自动装配文件

> /META-INF/spring.factories

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  oss.autoconfiguration.OssAutoConfiguration
```

#### 测试

##### 导入oss起步依赖

```xml
<!-- oss 起步依赖 -->
<dependency>
    <groupId>com.gorilla</groupId>
    <artifactId>oss-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

##### 编写配置文件

```yaml
oss:
  access-key-id: LTAI4G3uHmEsKn5okn1wWYk6
  access-key-secret: ZhTbkMEuFhPmRTQvPpQJSRfiY41yCg
  bucket-name: tanhua-gxm
  url: https://tanhua-gxm.oss-cn-beijing.aliyuncs.com
  endpoint: oss-cn-beijing.aliyuncs.com
```

##### 单元测试

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class OSSTest {

    @Autowired
    private OssTemplate ossTemplate;

    @Test
    public void test01()throws Exception{
        FileInputStream in = new FileInputStream(new File("E:\\temp\\upload\\1546241957400.png"));
        String url = ossTemplate.upload("1546241957400.png", in);
        System.out.println(url);
    }
}
```

### SpringBoot项目部署

#### jar包发布

1. 配置打包插件

```xml
<build>
    <finalName>springboot-mp</finalName>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

2. 执行java命令

> 通过maven命令package完成项目打包；找到jar包位置，以命令行窗口执行
>
> ```shell
> java -jar springboot-mp.jar
> ```

#### war包发布

1. 找到项目的pom.xml文件修改默认打包方式为war

```xml
<packaging>war</packaging>
```

2. 配置启动类

```java
// war包用的 写在启动类里
@Override
protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
    return builder.sources(MPApplication.class);
}
```

3. 执行maven的package命令,把打好的war包放到Tomcat中

### SpringBoot项目监控

SpringBoot自带监控功能Actuator，可以帮助实现对程序内部运行情况监控，比如监控状况、Bean加载情况、环境变量、日志信息、线程信息等

### 日志框架

通常情况下，日志是由一个抽象（接口）层+实现层的组合来搭建的。

| 日志-接口层                                                  | 日志-实现层                                       |
| ------------------------------------------------------------ | ------------------------------------------------- |
| JCL（Jakarta Commons Logging）、SLF4J（Simple Logging Facade for Java） | jul（java.util.logging）、 log4j、logback、log4j2 |

> Spring Boot在2.0版本后默认选择了 SLF4J 结合 LogBack
>
> 在开发的时候不应该直接使用日志实现类，应该使用日志的接口层。 

```java
public class HelloWorld {
  public static void main(String[] args) {
    private static final Logger  logger = LoggerFactory.getLogger(HelloWorld.class);
    System.out.println("xxxx") // 仅能输出到到控制台
    logger.info("Hello World"); // 即可输出到控制台还能输出到日志文件
    
    //也可以直接在类上加@Slf4j注解,不用写 private static  ....
    //直接用
      log.debug("***");
      log.info("***");
      log.warn("***");
      log.error("***");
    // 常见日志输出级别从小到大排序：debug < info < warn < error
  }
}
```

springboot引入了日志坐标,所以可以直接使用 `SLF4J + Logback` 进行日志输出。  **SpringBoot默认日志级别为INFO**

```yaml
# 日志配置
logging:
  level: # 指定项目日志级别
    com.gorilla: debug
  file: # 指定日志输出路径文件
    name: D:\springboot-quick.log
```

### SpringBoot热更新

对业务更改后需要重启服务,SpringBoot提供了进行热部署的依赖启动器,用于热部署

#### 使用

1. 导入坐标

> ```xml
> <!-- 引入热部署起步依赖 -->
> <dependency>
> <groupId>org.springframework.boot</groupId>
> <artifactId>spring-boot-devtools</artifactId>
> </dependency>
> ```

2. 修改

![批注 2021-10-17 183009](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/%E6%89%B9%E6%B3%A8%202021-10-17%20183009.png)

3. “Ctrl+Shift+Alt+/”打开Maintenance选项框-->打开Registry
4. 找到“compiler.automake.allow.when.app.running”，将该选项后的Value值勾选

![image-20201021153641871](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20201021153641871.png)

5. 测试没有立即生效,按`ctrl+F9`





## 软件架构演进

### 单体应用架构 (集中式架构)

一个系统的多个模块做在一个web项目中,部署到一台tomcat服务器上

> 优点:架构简单,小型项目开发成本低,维护方便
>
> 缺点:模块间耦合高,单点容错率低,一个服务挂其他不可用

![image-20211017220129125](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211017220129125.png)

![1545912768292](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1545912768292.png)

### 垂直应用架构

系统拆分成多个模块,每个模块部署在同一台tomcat服务器上,系统访问量增大,单一应用无法满足需求,为了应对更高并发和业务,根据业务功能对系统进行拆分

> 优点:
>
> * 系统拆分实现了流量分担，解决了并发问题
>
> * 对不同模块进行优化和水平扩展
> * 一个系统问题不会影响其他系统,提高单点容错率
>
> 缺点:系统间独立,无法进行相互调用,会有重复开发任务

![image-20211017220446675](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211017220446675.png)

![1545912974097](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1545912974097.png)

### 分布式架构

把服务层service单独启动,部署在tomcat上对外提供服务,在controller 中使用远程调用访问服务层中的方法

将功能模块拆分部署。当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。

> 优点:抽取公共功能为服务层,系统间互相调用,提高代码复用性
>
> 缺点:系统耦合度变高,调用关系错综复杂,难以维护

![image-20211017220709871](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211017220709871.png)

![1545913905548](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1545913905548.png)



### 服务治理架构 SOA架构

**Service Oriented Architecture**面向服务架构,将原先的散乱的网状结构规整成星型结构

dubbo+zookeeper

服务越来越多,在分布式架构基础上,增加调度中心对系统进行实时管理

> 优点:
>
> * 服务自动注册和发现,不用人为记录服务地址
> * 服务自动订阅,服务列表自动推送,服务调用透明化,无需关心依赖关系
> * 动态监控服务状态监控报告,人为控制服务状态
>
> 缺点:
>
> * 服务间会有依赖关系,一旦某环节出错影响较大
> * 服务关系复杂,运维测试部署困难,不符合DevOps思想
>
> 

![image-20211017220821698](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211017220821698.png)

### 集群和分布式

```markdown
* 集群:
	多台服务器处理同一个业务模块，例如：电商中的门户网站  从性能角度考虑
	多台服务器重复完成同一个任务,即同一个任务部署在多台服务器上(饭店多个服务员都会上菜)

* 分布式:
	将项目功能拆分，每个拆分的模块独立部署，这种项目架构模式叫做分布式架构。从业务功能角度考虑
	多台服务器协同完成同一个任务,即同一个任务拆分为多个子任务,多个子任务部署在多台服务器上协同完成同一个任务(厨师,配菜工,洗菜工,完成一道菜)
```

集群和分布式从项目高并发和高可用处理能力考虑,集群是**一组计算机**高度紧密工作,完成计算工作,其中每个计算机称一个**节点**,根据这些计算机协作方式不同,将可将集群分三类

* 高可用集群 主从集群架构 _High availability Cluster **HAC**_

  > 为了避免出现单点故障问题,在故障时可快速恢复
  >
  > > 假设有两台计算机node1和node2,两个计算机共享资源,处理业务基本一致,互为**主从**,node1工作node2待命,若有故障,则切换node2,保证高可用,但对闲置节点饿浪费

* 负载均衡集群 nginx   _Load Balancing负载均衡_ 

  > 集群中每台计算机完成相同业务,不分主次,请求到达时,让请求均衡分发集群中的每个节点,出现故障时把请求发到其他节点即可(成本大)

* 科学计算集群(分布式集群)

  > **硬件设备限制**,将复杂任务拆分成一个个小的子任务,分配到集群中的不同节点完成,最后再把计算结果汇总,(廉价PC*n=超级计算机),集群中每个节点完成**不同任务**

> 这几种集群方式不是必须独立使用,系统架构经常组合使用
>
> 如下 **高可用分布式集群** 为分布式系统每个节点都部署负载均衡节点,**每个业务系统都有一个负载均衡的小集群**

![1543289444226](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1543289444226.png)



大牛诊所的1.0阶段就相当于软件开发的单体结构，一个程序员打天下，从头编到尾，很难做大做强。大牛诊所的2.0阶段就相当于软件开发的垂直结构，各科室按照业务划分，很容易横向扩展。博爱医院的1.0阶段就相当于软件开发的SOA结构，除了药房（数据库）外各个服务独立提供（科室主任负责），但需要大牛院长（ESB总线）来协调。博爱医院的2.0阶段就相当于软件开发的微服务结构，公共服务院内共享，科室主任管理功能弱化（只管医生业务），优点是扩容方便，哪个部门缺人直接加，不用看上下游，资源利用率高，人员和设备效率高。

### 微服务

![1628212245389](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1628212245389.png)

#### 微服务特点

业务需要彻底的组件化和服务化

* 单一职责:每个服务都对应唯一业务能力,做到单一职责
* 微: 微服务拆分粒度很小,一个用户管理就可以作为一个服务,每个服务虽小,但"五脏俱全"
* 面向服务:每个服务都要对外暴露Rest风格服务接口API,并不关心服务的技术实现,做到**平台和语言无关**,只要提供Rest的即可 RestFul  GET POST DELETE PUT
* 自治:服务间互相独立,互不干扰
  * 团队独立:每个服务都是独立开发团队,人数不能过多
  * 技术独立:面向服务,只用提供Rest接口,使用什么技术没人干涉
  * 前后端分离:采用前后端分离开发,提供统一Rest接口,后端不用再用PC,移动端开发不同接口
  * 数据库分离:每个服务都有自己的数据源
  * 部署独立,服务间虽有调用,但要做到服务重启不影响其他服务,有利于持续集成和持续交付,每个服务都是独立组件,可复用,可替换,降低耦合,容易维护

dubbo和springCloud不同，dubbo使用的是原生的RPC技术来进行服务调用，而springCloud则应用了rest Api，一个是底层socket，一个是应用层http，所以相比之下，dubbo性能较高，sb则牺牲性能换来了架构的灵活，并且springCloud是微服务的一栈式解决方案，生态十分强大，组件之间的集成也很方便；而dubbo则始终是一款RPC框架，dubbo对于service接口的依赖是很严重的，很多代码都要install之后才能继续后面的开发工作


## Dubbo

阿里开发,捐献给apache基金会,[SOA架构](#服务治理架构 SOA架构)服务间的远程调用

dubbo 协议，单一长连接，进行的是 NIO 异步通信，基于 hessian 作为序列化协议。使用的场景是：传输数据量小（每次请求在 100kb 以内），但是并发量很高。

为了要支持高并发场景，一般是服务提供者就几台机器，但是服务消费者有上百台，可能每天调用量达到上亿次！此时用长连接是最合适的，就是跟每个服务消费者维持一个长连接就可以，可能总共就 100 个连接。然后后面直接基于长连接 NIO 异步通信，可以支撑高并发请求。长连接，通俗点说，就是建立连接过后可以持续发送请求，无须再建立连接。

序列化，就是把数据结构或者是一些对象，转换为二进制串的过程，而反序列化是将在序列化过程中所生成的二进制串转换成数据结构或者对象的过程。

如果公司全采用Java,就用Dubbo可以作为微服务架构

### RPC

- RPC全称为remote procedure call，**即远程过程调用**,基于原生TCP通信,速度快,效率快,早期webservice,现在dubbo,都是RPC的典型代表,HSF(好舒服企业里用的多?) 还有谷歌的gRPC
- 像调用本地方法一样调用远程方法
- RPC不是一个具体技术,指的是整个网络远程调用过程
- RPC框架很多,RMI,Hessian,Dubbo等

> A机器通过网络调用B机器的某个方法，这个过程就称为RPC

![image-20201024171010906](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20201024171010906.png)

![image-20201024170913490](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20201024170913490.png)

> 0. 服务启动,加载,运行服务提供者 
>
> 1. 服务**提供者**在启动时,向注册中心注册自己提供的服务
> 2. 服务**消费者**在启动时,向注册中心订阅自己所需服务
> 3. **注册中心**返回服务提供者地址列表给消费者,如果有变更,注册中心将基于长连 接推送变更数据给消费者
> 4. 服务**消费者**,从提供者地址列表中,基于软负载均衡算法,选一台提供者进行调用,如果调用失败,再选另一台调用
> 5. 消费者和提供者,在内存中累计调用次数和调用时间,定时每分钟发送一次统计数据到**监控中心**

Dubbo推荐zookeeper作为注册中心,zk负责服务地址的**注册**和**查找**,相当于服务目录,服务提供者和消费者**只在启动时与注册中心交互**,注册中心不转发请求,压力较小

### 使用

> dubbo底层是需要通过网络传输数据的，因此被传输的对象必须实现序列化接口 
>
> ![image-20210414145625886](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210414145625886.png)

#### 启动时检查

配置在服务消费方,启动时主动检查注册中心或者服务提供者是否准备好提供服务

```yaml
dubbo:
	consumer:
		check: false
	registry:
		check: false #false 代表不检查, true代表检查,服务提供者一旦未准备好,就直接抛异常
```

#### 服务超时

> 服务消费者在调用服务提供者的时候会发生阻塞,等待的情形,这时服务消费者会一直等下去,造成线程堆积,服务宕机
>
> ![image-20210408122735621](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210408122735621.png)

##### 全局配置

```yaml
# dubbo允许提供者端设置一个服务的超时时间(默认为1s)，如果超过这个时间，服务无法作出反应，直接终止线程。
dubbo:
	provider:
		timeout: 3000
		retries: 0
		
# 这个时间可以设置在服务调用者一端，也可以设置在服务提供者一端(如果同时设置了，消费者提供者优先级高)
dubbo:
	consumer:
		timeout: 5000
		retries: 0 #提供了重试机制
```

```java
//服务提供者
@DubboService(timeout = 3000, retries = 0)  //在类上定义超时时间,优先级高于配置  也提供了重试机制
//服务消费者
@DubboReference(timeout = 5000,retries = 0)

```

#### 服务降级

```java
//消费者端编写接口实现类,提供该方法默认返回值
public class UserServiceMock implements UserService {
    @Override
    public String sayHello(String name) {
        return "mock " + name;
    }
}

```

```java
@RestController
public class UserController {

    // 连接zookeeper获取rpc远程地址完成调用
    @DubboReference(timeout = 2000,retries = 0,mock = "com.gorilla.service.UserServiceMock")
    private UserService userService;

    @GetMapping("/user/hello/{name}")
    public String sayHello(@PathVariable String name) {
        return userService.sayHello(name);
    }
}
```

#### 负载均衡

> 在高并发情况下，一个服务往往需要以集群的形式对外工作。
>
> 负载均衡,服务器一个消费者的请求应该由哪一个服务提供者去处理
>
> ![image-20210408161553059](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210408161553059.png)

dubbo支持四种负载均衡策略

- random（默认）：按权重随机选择，这是默认值。
- roundrobin：按权重轮询选择。
- leastactive：最少活跃调用数，相同活跃数的随机选择。
- consistenthash：一致性Hash，相同参数的请求总是发到同一提供者。

#### 多版本

>很多时候，当项目出现新功能时，会让一部分用户先使用新功能，用户反馈没问题时，再将所有用户迁移到新功能
>
>dubbo使用**version**属性来设置和调用同一个接口的不同版本

![image-20210408124848851](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210408124848851.png)

------

#### 服务提供者

1.导入依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.2.RELEASE</version>
</parent>

<dependencies>
    <!--基础环境-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <!--dubbo的起步依赖-->
    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo-spring-boot-starter</artifactId>
        <version>2.7.8</version>
    </dependency>
    <!-- zookeeper的api管理依赖 -->
    <dependency>
        <groupId>org.apache.curator</groupId>
        <artifactId>curator-recipes</artifactId>
        <version>4.2.0</version>
    </dependency>
    <!-- zookeeper依赖 -->
    <dependency>
        <groupId>org.apache.zookeeper</groupId>
        <artifactId>zookeeper</artifactId>
        <version>3.4.12</version>
    </dependency>
</dependencies>
```

2. service接口与实现,实现类上添加`@DubboService`

   > @DubboService 
   >
   > 1. 把对象交给ioc容器  2. 把对象注册到zookeeper中

3. 配置application.yml

   ```yaml
   # dubbo.application.name       服务名称，一般跟模块名称一致即可
   # dubbo.registry.address       注册中心的连接地址
   # dubbo.protocol.name          当前服务的访问协议，支持dubbo、rmi、hessian、http、webservice、rest、redis等
   # dubbo.protocol.port          当前服务的访问端口
   # dubbo.scan.base-packages     包扫描
   dubbo:
     application:
       name: day03-dubbo-demo-provider
     registry:
       address: zookeeper://127.0.0.1:2181
     protocol:
       name: dubbo
       port: 20880
     scan:
       base-packages: com.gorilla.service
   ```

4. 启动类

   ```java
   @SpringBootApplication
   public class ProviderApp {
       public static void main(String[] args) {
           SpringApplication.run(ProviderApp.class, args);
       }
   }
   ```

负载均衡



#### 服务消费者

1. 导入依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.2.RELEASE</version>
</parent>

<dependencies>
    <!--web环境-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--dubbo的起步依赖-->
    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo-spring-boot-starter</artifactId>
        <version>2.7.8</version>
    </dependency>
    <!-- zookeeper的api管理依赖 -->
    <dependency>
        <groupId>org.apache.curator</groupId>
        <artifactId>curator-recipes</artifactId>
        <version>4.2.0</version>
    </dependency>
    <!-- zookeeper依赖 -->
    <dependency>
        <groupId>org.apache.zookeeper</groupId>
        <artifactId>zookeeper</artifactId>
        <version>3.4.12</version>
    </dependency>
</dependencies>
```

2. 复制service接口,编写controller

```java
@RestController
public class UserController {

    @DubboReference // 连接zookeeper获取rpc远程地址完成调用
    private UserService userService;


    @GetMapping("/user/hello/{name}")
    public String sayHello(@PathVariable String name){
        return userService.sayHello(name);
    }
}
```

3. 配置yml

```yaml
# dubbo.application.name       服务名称，一般跟模块名称一致即可
# dubbo.registry.address       注册中心的连接地址
# dubbo.scan.base-packages     包扫描
dubbo:
  application:
    name: day03-dubbo-demo-consumer
  registry:
    address: zookeeper://127.0.0.1:2181
  scan:
    base-packages: com.gorilla.controller
```

4. 启动类

```java
@SpringBootApplication
public class ConsumerApp {
    
    public static void main(String[] args) {
        SpringApplication.run(ConsumerApp.class, args);
    }
}
```

### RPC执行流程

通过ip地址和端口号

![image-20210706101047927](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210706101047927.png)

### maven依赖关系

![image-20210706102204500](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210706102204500.png)

### Dubbo管理控制台

**进行服务管理 、 路由规则、动态配置、服务降级、访问控制、权重调整、负载均衡**等管理功能

> 看到zk都注册了哪些服务,有哪些消费者来消费服务,管理中心就是一个web应用,

```markdown
1. 从git上下载项目 https://github.com/apache/dubbo-admin/tree/master

2. 修改项目下的application.properties文件
		dubbo.registry.address=zookeeper://zk所在机器ip:zk端口
		dubbo.admin.root.password=root
		dubbo.admin.guest.password=guest
		
3. 切换到项目所在的路径 使用mvn 打包
		mvn clean package -Dmaven.test.skip=true
```

#### 使用

1. java -jar  ######.jar
2. 127.0.0.1:7001   root   root



## Zookeeper

![image-20210708083737111](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210708083737111.png)

> Zookeeper是一个开放源码的`分布式服务协调组件`，是Google Chubby的开源实现,高性能的分布式数据一致性解决方.将复杂的、容易出错的分布式一致性服务封装起来，构成一个高效可靠的原语(指令)集，并提供一系列简单易用的接口给用户使用

在大数据生态系统里，很多组件的命名都是某种动物，比如hadoop就是🐘，hive就是🐝。Zookeeper的作用是用来对这些组件进行管理，即动物园管理者:cop:。

### 使用

#### 配置

> 编辑`zoo.cfg`文件，修改`dataDir`的值为`../data`这是zookeeper存储数据的位置

#### 启动

> 进入安装路径的bin目录，双击`zkServer.cmd`即可启动zookeeper服务 
>
> 进入安装路径的bin目录，双击`zkCli.cmd`即可启动zookeeper命令行客户端
>
> 在客户端中输入`create /data ceshi`，没有报错，代表客户端可以成功操作服务器 

#### UI客户端

ZooInspector

> 在安装软件的build目录下，执行`java -jar zookeeper-dev-ZooInspector.jar`命令，即可启动软件的界面 

点击连接按钮，输入zk的服务地址，点击OK连接 

### Zookeeper数据结构

Zookeeper的数据节点可以看做：`树形（目录）结构`，树中的各节点被称为znode（即zookeeper node），一个znode可以有多个子节点。

> znode兼具文件和目录两种特点：既像文件一样维护着数据、元信息、ACL、时间戳等数据结构，又可以像目录一样挂载子目录。

使用绝对路径path来定位某个znode，比如`/gorilla/xa/java`，此处gorilla、xa、java分别是1级节点、2级节点、3级节点

![无标题](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/%E6%97%A0%E6%A0%87%E9%A2%98.png)

一个znode大体上分为3各部分：

1. data：当前节点的数据
2. children：当前节点的子节点
3. stat：当前节点的状态，用来描述当前节点的创建、修改记录等

```powershell
# 在zookeeper shell中使用get命令查看指定路径节点的data、stat信息：
[zk: localhost:2181(CONNECTED) 3] get /gorilla/xa/java
# cZxid：数据节点创建时的事务 ID
# ctime：数据节点创建时的时间
# mZxid：数据节点最后一次更新时的事务 ID
# mtime：数据节点最后一次更新时的时间
# pZxid：数据节点的子节点最后一次被修改时的事务 ID
# cversion：子节点的更改次数
# dataVersion：节点数据的更改次数
# aclVersion: 权限的更改次数
# ephemeralOwner: 持久节点还是临时节点
# dataLength：数据内容的长度
# numChildren：数据节点当前的子节点个数
```

#### 节点类型

**有4种类型的znode：**

1、PERSISTENT--`持久化目录节点`

- 客户端与zookeeper断开连接后，该节点依旧存在

2、PERSISTENT_SEQUENTIAL-`持久化顺序编号目录节点`

- 客户端与zookeeper断开连接后，该节点依旧存在，只是Zookeeper给该节点名称进行顺序编号

3、EPHEMERAL-`临时目录节点`

- 客户端与zookeeper断开连接后，该节点被删除

4、EPHEMERAL_SEQUENTIAL-`临时顺序编号目录节点`

- 客户端与zookeeper断开连接后，该节点被删除，只是Zookeeper给该节点名称进行顺序编号
- 另外，ZooKeeper的临时节点不允许拥有子节点

#### zookeeper常见命令

##### 创建节点

`create [-s] [-e] path data [acl]`

```shell
path：节点名称
data：节点数据
acl： 权限
-s：有序节点
-e：临时节点
```

##### 查看节点

> get path : 获取节点的状态信息和数据信息`get /node/node1`
>
> stat  path : 获取节点的状态信息

##### 查看节点列表

> ls   path:  查看指定节点的子节点列表 
>
> ls2 path:  查看指定节点的状态信息ls 以及子节点列表

##### 修改节点

> set path data[version] `set /gorilla/xa/java java`
>
> version基于版本号对指定节点内容进行修改,每次数据被修改，版本自动加1,如果版本号错误,zk拒绝本次修改

##### 删除节点

> delete path [version]
>
> rmr path

#### Zookeeper监听机制

它允许对zookeeper注册监听，当监听的对象（节点）发生指定的事件的时候，zookeeper就会返回一个通知。

##### 监听的事件类型

- None:客户端和服务端连接状态发生变化
- NodeCreated:创建节点
- NodeDeleted:删除节点
- NodeDataChanged:节点数据发生变化（dataVersion）
- NodeChildrenChanged:子节点发生变化(创建\删除\数据变更)

##### 命令

```shell
get path watch：    注册的监听器能够在监听节点的`内容发生改变`的时候，向客户端发出通知
get /node watch //对node节点进行监听

stat path watch：   注册的监听器能够在监听节点的`状态发生改变`的时候，向客户端发出通知

ls/ls2 path watch： 注册的监听器能够在监听节点的`子节点新增和删除`的时候，向客户端发出通知
```

### Java操作Zookeeper

不推荐Zookeeper 原生API,推荐**Apache Curator**

> Apache 是 Apache ZooKeeper的Java客户端库，简化ZooKeeper客户端的使用

#### 节点操作

详情操作去查

#### 节点监听

> 一旦服务端感知主题变了，那么只会发送一个事件类型和节点信息给关注的客户端，而不会包括具体的变更内容，所以事件本身是轻量级的，这就是所谓的"推"部分。然后，收到变更通知的客户端需要自己去拉变更的数据，这就是"拉"部分。

>Curator在这方面做了优化，它引入了Cache的概念用来实现对ZooKeeper服务器端进行事件监听。
>Cache是Curator对事件监听的包装，其对事件的监听可以近似看做是一个本地缓存视图和远程ZooKeeper视图的对比过程。
>而且Curator会`自动的再次监听`，我们就不需要自己手动的重复监听了。
>
>```markdown
>Curator中的cache共有三种:
>	NodeCache:  用来监听节点的创建和数据变化
>	PathChildrenCache: 用来监听指定节点的子节点增删和数据变化
>	TreeCache: 像上面两种Cache的结合体，能够监听自身节点的变化、也能够监听子节点的变化
>```

### Zookeeper应用场景

#### 分布式唯一ID

> 通过在ZooKeepr里创建顺序节点，很容易创建一个全局唯一的名字，即生成全局唯一的ID。
>
> ![image-20210708105117355](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210708105117355.png)

#### 配置中心

> 配置中心使用的是Zookeeper的watch机制
>
> ![image-20210708105623962](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210708105623962.png)

##### 痛点

在分布式应用系统中，会有很多的类似的配置文件，他们会散落在各个服务中，比如数据库的用户名和密码

这些配置如果后期要修改的话，就需要修改非常多的配置文件,于是就有了配置中心的概念

##### 方案

​	可以把所有的配置都放在一个配置中心，**然后各个服务分别去监听配置中心**，一旦发现里面的内容发生变化，立即获取变化的内容，然后更新本地配置即可

##### 实现

所有的服务需要监听Zookeeper的配置节点，当配置信息发生变化之后，Zookeeper会将变化信息推送给服务，当然服务也可以主动去拉取节点数据

##### 说明

push可以保证能够第一时间拿到更新配置，基本可以做到准实时的更新，但push存在问题，即如果有网络抖动，某一次push没有推送成功，将丢失这次配置的更新
pull可以保证一定可以拉取得到数据，pull一般采用定时拉取的方式，即使某一次出现网络问题，没有拉取得到数据，那在下一次定时器也将可以拉取得到数据

#### 注册发现

>注册发现使用的是Zookeeper的watch机制

~~~markdown
* 不同的客户端如机器节点，发生了变化，那么所有订阅的客户端都能够接收到相应的Watcher通知，并做出相应的处理。
~~~

![image-20210708110628658](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210708110628658.png)

#### 分布式锁

> 分布式锁使用的是Zookeeper的临时有序节点

![image-20210708111630654](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210708111630654.png)

##### 痛点

分布式系统中,会出现多台主机操作同一个资源的情况,如两台主机同时往一个文件中追加写文本,如果不做任何控制,可能出现一个写入被另一个写入操作覆盖住的情况

##### 方案

用一把锁,那个主机获取到锁,就执行写入,另一台主机等待,直到写入操作执行完成,另一台主机再去获取锁,执行写入,这就是分布式锁

分布式锁是**控制分布式系统之间同步访问共享资源的一种方式**

##### 实现

使用zookeeper的临时有序节点可以实现

1. 所有需要执行操作的主机都去zookeeper中创建一个临时有序节点
2. 然后获取到zookeeper上创建的这些节点进行从小到大排序
3. 判断自己创建的节点是否为最小,如果是就获取到了锁,如果不是,就对最小的节点注册一个监听
4. 获取到了锁,就执行响应操作,执行完毕,(利用临时有序节点特性)连接断开,节点消失,锁就被释放
5. 没获取到锁,就等待,一直监听节点消失,锁释放后,在重新执行抢夺锁的操作

### Zookeeper集群

保证高可用,就要部署zookeeper集群

Zookeeper 有三种运行模式：单机模式、集群模式和`伪集群模式`。

- 单机模式:使用一台主机一个zookeeper来对外提供服务,会有单点故障问题,仅适合开发环境
- 集群模式:使用多台服务器,每台服务器都部署一个zookeeper一起对外提供服务,用于生产环境
- 伪集群模式:服务器不够多的情况下,在一台服务器上部署多个zookeeper来对外提供服务

#### 伪集群

1. 复制三个zookeeper

2. 修改配置,编辑zk的配置文件,修改**端口号**和**服务地址**

   > ```markdown
   > dataDir： 数据存储的地址，修改为当前zk下的data目录
   > clientPort：当前zk对外的端口号，因为是在同一个服务器上部署多台，此端口号不能重复
   > server.id: 集群节点信息
   > 	格式为: server.A=B:C:D    
   >  	A：是一个数字，表示这个是服务器的编号
   >  	B：是这个服务器的ip地址  
   >  	C：Zookeeper服务器之间的通信端口 
   >  	D：Leader选举的端口
   >  例如：
   >       server.1=127.0.0.1:3181:3281
   >     	server.2=127.0.0.1:3182:3282
   >       server.3=127.0.0.1:3183:3283
   > ```

3. 创建节点id,子dataDir指定的目录下,创建一个文件，名字为：`myid`,文件内如为上一步service.id中的id 

4. 启动集群,双击每个目录的zkServer.cmd,启动三个服务器

5. 连接测试

   > ```powershell
   > # 进入到其中一个zk的bin目录执行连接命令
   > # zkCli.cmd -server 连接节点的ip：port
   > zkCli.cmd -server 127.0.0.1:2182
   > ```

#### 集群角色

- Leader（领导者）：负责投票的发起和决议，更新系统状态，是事务请求的唯一处理者，**一个ZooKeeper同一时刻只会有一个Leader**
- Follower（跟随者）：处理客户端请求，参与选主投票
- Observer（观察者）：处理客户端请求，不参与选主投票

Leader提供读写服务,`Follower或Observer只提供读服务`,但是Observer机器不参与Leader选举，也不参与写操作的『过半写成功』策略

 

![image-20210416161513750](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210416161513750.png)

#### 集群选举

> Zookeeper服务器有四个状态：
>
> - looking:**寻找leader状态**,当服务器处于该状态时,它会认为当前集群中没有leader,需要进入leader选举状态
> - leading:** 者状态**,表明当前服务器角色是leader
> - following：跟随者状态。表明当前服务器角色是follower。
> - observing：观察者状态。表明当前服务器角色是observer。

leader选举，要求 **可用节点数量 > 总节点数量/2**

#### 选举过程

1. 每个server发出一个投票。

   ```markdown
   	由于是初始情况，server1和server2都会将自己作为leader服务器来进行投票，每次投票会包含所推举的服务器的myid和zxid，
   	使用(myid, zxid)来表示，此时server1的投票为(1, 0)，server2的投票为(2, 0)，然后各自将这个投票发给集群中其他机器。
   ```

2. 集群中的每台服务器接收来自集群中各个服务器的投票。

3. 处理投票。针对每一个投票，服务器都需要将别人的投票和自己的投票进行pk，pk规则如下:

   ```markdown
   	优先检查zxid。zxid比较大的服务器优先作为leader。
   	如果zxid相同，那么就比较myid。myid较大的服务器作为leader服务器。
   	对于Server1而言，它的投票是(1, 0)，接收Server2的投票为(2, 0)，首先会比较两者的zxid，均为0，再比较myid，此时server2的myid最大，
   	于是更新自己的投票为(2, 0)，然后重新投票，对于server2而言，其无须更新自己的投票，只是再次向集群中所有机器发出上一次投票信息即可。
   ```

4. 统计投票。每次投票后，服务器都会统计投票信息，判断是否已经有过半机器接受到相同的投票信息，对于server1、server2而言，都统计出集群中已经有两台机器接受了(2, 0)的投票信息，此时便认为已经选出了leader

5. 改变服务器状态。一旦确定了leader，每个服务器就会更新自己的状态，如果是follower，那么就变更为following，如果是leader，就变更为leading。

>目前有5台服务器，每台服务器均没有数据，它们的编号分别是1,2,3,4,5,按编号依次启动，它们的选择举过程如下：
>
>- 服务器1启动，给自己投票，然后发投票信息，由于其它机器还没有启动所以它收不到反馈信息，服务器1的状态一直属于Looking(选举状态)。
>- 服务器2启动，给自己投票，同时与之前启动的服务器1交换结果，由于服务器2的编号大所以服务器2胜出，但此时投票数没有**大于半数**，所以两个服务器的状态依然是LOOKING。
>- 服务器3启动，给自己投票，同时与之前启动的服务器1,2交换信息，由于服务器3的编号最大所以服务器3胜出，此时投票数正好大于半数，所以服务器3成为领导者，服务器1,2成为小弟。
>- 服务器4启动，给自己投票，同时与之前启动的服务器1,2,3交换信息，尽管服务器4的编号大，但之前服务器3已经胜出，所以服务器4只能成为小弟。
>- 服务器5启动，后面的逻辑同服务器4成为小弟。

> Serverid：服务器ID
>
> 比如有三台服务器，编号分别是1,2,3。
>
> > 编号越大在选择算法中的权重越大。
>
> Zxid：数据ID
>
> 服务器中存放的最大数据ID.
>
> > 值越大说明数据越新，在选举算法中数据越新权重越大。

#### 运行时选举

假设正在运行的有Server1、Server2、Server3三台服务器，当前Leader是Server2，若某一时刻Leader挂了，此时便开始Leader选举。选举过程如下：

> 1. 变更状态。leader挂后,余下的服务器会将自己的服务器状态变更为looking,然后开始进入leader选举过程。
> 2. 每个server会发出一个投票。在运行期间,每个服务器上的zxid可能不同,此时假定server1的zxid为122,server3的zxid为122,在第一轮投票中,server1和server3都会投自己,产生投票(1,122),(3,122),然后各自将自己的投票发送给集群中的所有机器。
> 3. 接收来自各个服务器的投票.与启动过程相同。
> 4. 处理投票。与启动过程相同。此时,server3将会成为leader.
> 5. 统计投票。与启动时过程相同。
> 6. 改变服务器的状态。与启动时过程相同。

##### Observer角色

* Observer角色不参与集群的leader选举

* zk适合读多写少的场景

* zk集群里面，因为只有一个leader，所以写是没法扩展的，最多只能抗上万QPS

* zk集群中，follow节点有2个到4个节点，读起码每秒几万QPS，如果需要抗住更高的读请求，就可以引入Observer节点，因为Observer节点只是同步数据，可以无限扩展机器。

###### 配置

为了使用observer角色，在任何想变成observer角色的配置文件中加入如下配置：`peerType=observer`

并在所有server的配置文件中，配置成observer模式的server的那行配置追加`server.A=B:C:D:observer  `



## RocketMQ

消息中间件

分布式系统中的重要组件 解决**应用解耦**,**流量削峰**,**异步消息**等问题

![image-20210725091919729](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210725091919729.png)

#### 使用场景

##### 应用解耦

###### 场景1用户注册,需要发送注册邮件和注册短信

注册某用户时,点击注册会发短信,会发邮件, 将注册消息发给mq,短信模块和邮件模块来消费消息,对(本互相关联的)应用进行解耦~

![1571300317204-1620188030625](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1571300317204-1620188030625.png)

###### 场景2 出书-->各国翻译-->出版

作者出书-发布到编辑社- 翻译成中文,法文,英文  作者出了书告诉编辑社,翻译与作者无关,由编辑社去翻译,进行解耦)

![image-20210725170740375](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210725170740375.png)

##### 异步消息

###### 场景1  提交订单向mq发送消息-->库存,支付,物流等系统消费消息

MQ性能优越,异步订单数据把数据给MQ,其他系统直接从MQ拿数据,不用等其他系统回馈

数据的产生方不需要关心谁来使用数据，也不需要使用方等待处理的结果，只需要将数据发送到消息队列，数据使用方直接在消息队列中直接获取数据即可  

![image-20210725170916399](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210725170916399.png)

##### 削峰平谷

长江 -->大坝 -->放水

**削峰**:请求瞬间增多(每秒5000次请求)时,A系统承载能力不足(每秒最大1000个请求)时,用户将请求发到MQ,A系统可以每秒从MQ中拉取1000个请求,高峰被削掉.**平谷**:在高峰过后  的一段时间里,消费消息的速度依然维持1000/s,直到消费完积压的消息.

###### 场景

秒杀活动,流量暴增,需要在应用前端加入消息队列,将大量请求缓存起来,分散到很长一段时间处理,大大提高系统稳定性

![image-20210725171358137](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210725171358137.png)

#### 劣势

##### 系统可用性降低

引入外部依赖越多,系统稳定性越差,一旦MQ宕机,就会对业务造成影响

##### 系统复杂度提高

MQ的加入大大增加系统的复杂度,以前系统间是同步的远程调用,现在是MQ异步调用,消息也有可能丢失



### 常见消息中间件

- ActiveMQ：是Apache出品基于JMS（Java Message Service）规范的一种消息中间件的实现。传统公司可能还在用
- Kafka：Apache下的一个子项目 。特点：可以达到10W/s的吞吐速率；适合处理海量数据。高吞吐,会丢数据大数据领域用的比较多
- RabbitMQ：是基于AMQP(Advanced Message Queuing Protocol)协议上完成的，基Erlang语言开发的。专为高并发和分布式系统的一种语言,电信领域用的多),并发优秀,性能牺牲一些,安全性高(几乎100%消息不丢失),吞吐量低,金融领域用的多,用于小规模场景.在里面有交换机和队列,交换机把消息发到某一队列里
- RocketMQ ：阿里巴巴中间件团队推出产品。高吞吐,高性能,客户端只支持java,开源不如云上,全场景可用



架构主要分文4个角色 NameServer,Broker,Producer,Consumer

NameServer可以理解为同城快送品台

Broker可以理解为快递小哥,broker 给nameserver注册路由,快递小哥告诉快送平台我负责派送

producer理解为寄件人,向nameserver获得快递小哥信息,把货物给到快递小哥

consumer理解为收件人向nameserver联系到快递小哥,

topic理解为车,里面装有很多message queue(包裹),一个包裹里可以有很多货物(message)





![image-20210725095102532](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210725095102532.png)

![1576034886312](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1576034886312.png)

* Broker:RocketMQ核心,负责消息的接收,存储,投递等功能 

* NameServer:**消息队列的协调**者,Broker向他**注册路由信息**,同时Producer和Consumer向**其获取路由信息**

* Producer:消息的生产者,需要从NameServer获取Broker信息,然后与Broker建立连接,**向Broker发送消息**

* Consumer:消息的消费者,需要从NameServer获取Broker信息,然后与Broker建立连接,**从Broker获取消息**

* Topic:用来区分不同类型的消息,发送和接收消息前都需要先创建Topic,针对Topic来发送和接收消息

* Message Queue(消息队列):一个Topic 可以设置一个或多个Message Queue,这样生产者就可以**并行**往各个MessageQueue发送消息,消费者也可以**并行**的从多个Message Queue读取消息

* Message是消息的载体

* Producer Group:生产者组,多个发送同一类消息的生产者称为一个生产者组

* Gonsumer Group:消费者组,消费同一类消息的多个consumer实例组成一个消费者组,这类consumer消费的是**同一个topic 类型**的消息,消费者组使得在**消费消息方面**实现**负载均衡**(将一个Topic 中的不同Queue平均分配个同一个Consumer Group的不同consumer,并不是**将消息负载均衡**)和**容错**(一个consumer挂了,该consumer Group中的其他Consumer可以接着消费原Consumer消费的Queue)的目标变得非常容易

* 一个Topic的Queue中的消息只能被一个消费者组中的一个消费者消费(一个学校的一个学生对应一个包裹).一个Queue中的消息不允许同一个消费者组中的多个消费者同时消费(一个学校的很多人不能同时拿走一个包裹)(如果Consumer Group中**只有一个Consumer**,这个Consumer**可以消费多个Queue**(如果这个学校只有一个人,那么这些包裹都可能是他的,那就拿多个),但**一个Queue不能被两个Consumer同时消费**(但一个包裹不能给两个学生)) (多个不同的consumer group中的Consumer可以消费同一个queue(这个举例困难,抽象化记忆),但不允许同一个Consumer Group中的多个Consumer同时消费Queue)

![image-20201230182922302](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20201230182922302.png)

1. 启动NameServer，NameServer起来后监听端口，等待Broker、Producer、Consumer连 上来，相当于一个路由控制中心。
2. Broker启动，跟所有的NameServer保持长连接，定时发送心跳包，心跳包中包含当前Broker信息以及存储所有Topic信息注册成功后，NameServer集群中就有Topic跟Broker的映射关系。

3. 收发消息前，先创建Topic，创建Topic时需要指定该Topic要存储在哪些Broker上，也可以在发送消息时自动创建Topic
4. Producer发送消息，Broker节点启动时先跟NameServer集群中的其中一台建立长连接，并从NameServer中获取当前发送的Topic存在哪些Broker上轮询
   从队列列表中选择一个队列，然后与队列所在的Broker建立长连接从而向Broker发消息,NameServer集群各个节点间无差异,各节点间相互不进行信息通讯. 
5. Consumer跟Producer类似，跟其中一台NameServer建立长连接，获取当前订阅Topic存在哪些Broker上，然后直接跟Broker建立连接通，开始消费消息

### 使用

管理后台:http://192.168.136.160:8180

rocketMQ放在在docker中-->导坐标-->配置-->使用RocketMQTemplate.convertAndSend发送消息-->消费者接收消息 `@Component`和`@RocketMQMessageListener`继承RocketMQlistener监听topic队列中的消息

配置

```yaml
# 服务端口
server:
  port: 9001

# 配置mq服务地址和生产者组名
rocketmq:
  name-server: 192.168.136.160:9876
  producer:
    group: producer
    send-message-timeout: 30000
```

```yaml
#消费者
#服务器端口
server:
  port: 9002

#指定mq地址
rocketmq:
  name-server: 192.168.136.160:9876
```

```java
@Component
@RocketMQMessageListener(consumerGroup ="base-group",topic = "***")
public class BaseMessageListener implements RocketMQListener<String> {

    @Override
    public void onMessage(String s) {
        System.out.println("消费者接收：" + s);
    }
}
```

#### 延迟消息

rocketMQTemplate.syncSend("***", stringGenericMessage,3000 , 3);

##### 场景

电商系统提交了订单,可以发送一个延迟消息,1后检查这个订单状态,如果还未付款就取消订单释放库存

#### 顺序消息

![image-20210725105213099](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210725105213099.png)

> 消息有序指的是一类消息消费时，能按照发送的顺序来消费。
>
> 场景1:一个订单产生了三条消息分别是订单创建、订单付款、订单完成。那么消费时也要按照这个顺序消费才能有意义。
>
> 场景2:进行两个数据库的同步日志的时候,顺序是插入-->更新-->删除,如果顺序错误搞成删除-->更新-->插入,原本要删除的数据没有删除,就会造成两个数据库同步不一致的问题,所以要保证顺序消费

RocketMQ是通过将**相同OrderedID的消息发送到同一个队列**，**而一个队列的消息只由一个消费者处理**来实现顺序消息。

##### producer端保证消息顺序

RocketMQ提供了MessageQueueSelector(消息队列选择器)接口,通过重写里面的select方法,实现自己的算法,通过id和队列数进行取模行取模,假设有两个queue,i%2=0,放在queque1中,否则就放到queue2中

##### consumer端保证消息顺序

只用在@RocketMQMessageListener注解里里指定消费模式consumeMode ConsumeMode.ORDERLY ：顺序消费模式 

> - ConsumeMode.ORDERLY ：顺序消费模式
> - ConsumeMode.CONCURRENTLY：并发消费模式

#### 广播消息

集群（高可用）模式，只有一个消费者可以收到消息。广播模式是所有的消费者都可以收到消息。

需要把【消息模式】设置为 ：广播模式，同时开启2个以上的消费者，再次运行生产者，只需要在消费方设置一下消费模式即可。

![image-20210725110132382](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210725110132382.png)



> messageModel 
>
> - MessageModel.BROADCASTING：广播模式\
>
>   > 一个group中的每一个consumer都会消费
>
> - MessageModel.CLUSTERING：集群模式
>
>   > 一条消息只会被同一group中的一个consumer消费

场景,短信监听者和邮件监听者两者采用广播消费模式

#### 消息重复消费

影响消息正常发送和消费的重要原因是网络的不确定性

##### 问题

正常情况下在consumer消费完消息后应该发送一个ack(确认字符),通知broker这个消息已经正常消费过了,可从消息队列queue中剔除了,因为网络/其他原因ack没有及时发送到broker,会认为词条信息没有被消费,此后会**开启消息重投机制**把消息再次投递到consumer,在集群模式下,虽然消息在broker中保证一个group组中的consumer消费一次,但不同的group的consumer会推送多次

##### 解决

1. 消费端处理消息的业务逻辑保持幂等性

   > 幂等性:一个数据，或者一个请求，给你重复来多次，你得确保对应的数据是不会改变的，不能出错。

2. 保证生产者发送消息都有**唯一id**(可以放在**redis**中,消费过了就不处理,没消费过就处理)且保证消息处理成功与去重表的**日志**同时出现

   > 利用一张日志表来记录已经处理成功的消息ID,如果新到的消息ID已经在日志表中，那么就不再处理这条消息。

3. Redis,使用Redis分布式锁

4. Map,单机时使用map做限制,消费时查询消息id是否已经存在

5. 如果要写库,根据主键查一下,如果数据都有了,就不要插入了,update一下

6. 如果写redis,每次都是set,天然幂等性

RocketMQ保证消息不重复，如果业务需要保证严格的不重复消息，需要在业务端去重。

#### 保证消息不丢失

producer broker consumer 三者都会有消息丢失的情况  

##### 场景 发送短信验证码,要保证消息不丢失

##### Producer端保证消息不丢失

以下下三种手段同时使用最大程度保证消息在producer端不丢失

1. 采用`send(msg)`同步发消息,发送结果是同步感知的

2. 发送失败后可重试,设置重试次数,默认3次,设置发送失败重试时间`producer.setRetryTimesWhenSendFiled(10)`,
3. 集群里面发送失败可能是broker宕机,这时发送到其他broker上.

> 直接写send(msg,new SendCallback(){在里面定义发送成功/失败的回调函数})是异步发送,
>
> 同步发送:Producer 向 broker 发送消息，阻塞当前线程等待 broker 响应 发送结果。
>
> 异步发送:Producer 首先构建一个向 broker 发送消息的任务，把该任务提交给线程池，等执行完该任务时，回调用户自定义的回调函数，执行处理结果。

##### Broker端保证消息不丢失

最大限度保证broker消息不丢失,可以配置主从broker后,将两者的刷盘策略都改为同步刷盘(消息写入两台master后给producer返回成功)

1. 修改刷盘策略为同步刷盘,默认情况下是异步刷盘

> 同步刷盘：消息写入后等待刷盘成功返回
>
> 异步刷盘：消息写入内存后直接返回，由后台线程去定时刷盘
>
> 当消息投递到broker之后，会先存到page cache，然后根据broker设置的刷盘策略是否立即刷盘，也就是如果刷盘策略为异步，broker并不会等待消息落盘就会返回producer成功，也就是说当broker所在的服务器突然宕机，则会丢失部分页的消息。

2. 集群部署,配置主从master和slave的broker

##### Consumer保证消息不丢失

完成消费后进行手动ack确认

> At least Once机制  Consumer先pull 消息到本地，消费完成后，才向服务器返回ack
>
> 消息消费的ack机制一般分为:
>
> 1. 先提交后消费(解决消息重复消费,但会丢失信息)
> 2. 先消费,消费成功后再提交
>
> consumer端要保证消费消息的可靠性，主要通过At least Once+消费重试机制保证
>
> > 消费重试机制保证:消息消费失败,如果不提供重试消费的能力,也算不上完全可靠的消费

#### RocketMQ实现负载均衡

通过Topic在多Broker中分布式存储实现

##### producer端

发送端指定message queue 发送到相应的broker,来达到写入时候的负载均衡

> 可以提升写入吞吐量,因为当多个producer同时向一个broker写数据时,性能会下降
>
> 消息分布在多个broker中,为负载消息做准备

默认策略是随机选择 producer维护一个index,每次取的时候节点会自增,index 向所有broker个数取余,以达到`指定message queue 发送到相应的broker`,从而达到负载均衡的目的

> producer每次在使用broker的时候会记录这次使用的是哪个broker；下次在选取broker的时候主动排除这个broker；

##### consumer端

采用的是平均分配算法来进行负载均衡

###### 其他负载均衡算法

环形分配策略,手动配置分配策略,机房分配策略,一致性哈希分配策略,靠近机房分配策略德

##### 负载均衡consumer和queue不对等时候发生什么?

consumer和queue会优先平均分配

* 如果consumer少于queue的个数,则会存在部分consumer消费多个queue的情况 ;

* 如果consumer等于queue的个数,一个consumer消费一个queue
* 如果consumer大于queue的个数,会有部分consumer空余出来,白白浪费

> - queue个数大于Consumer个数，且queue个数能整除Consumer个数的话， 那么Consumer会平均分配queue。
> - queue个数大于Consumer个数，且queue个数不能整除Consumer个数的话， 那么会有一个Consumer多消费1个queue，其余Consumer平均分配。
> - queue个数小于Consumer个数，那么会有Consumer闲置，就是浪费掉了，其余Consumer平均分配到queue上。

#### RocketMQ消息堆积如何处理

下游消费系统如果宕机了，导致几百万条消息在消息中间件里积压，此时怎么处理?

首先要找到是什么原因导致的消息堆积，是Producer太多了，Consumer太少了导致的还是其他情况,然后看下消息消费速度是否正常，正常的话，可以通过上线更多consumer临时解决消息堆积问题。

Consumer和Queue不对等,上线多台在短时间内无法消费完堆积的消息怎么办?

* 准备一个临时的topic

* queue的数量是堆积的几倍

* queue分布到多broker中

* 上线一天consumer做消息的搬用工,把原来的Topic中的消息挪到新的topic里,不做业务逻辑功能,只是挪过去

* 上线N台consumer同时消费临时Topic中的数据

  >  未被消费的消息不会存在超时删除这情况,消息在消费失败后会进入重试队列

#### RocketMQ实现分布式事务?

在系统变的复杂后，分布式、微服务等架构技术，就要考虑到应用在系统中了。尤其数据量大了后，就需要对**数据库进行拆分**,一旦对数据库进行拆分,就有很多头疼问题,就是事务问题

> 这种问题在**微服务架构会更多**，因为各个服务都是**独立的模块，都是远程调用**，都没法在同一个事务中，都会遇到事务问题
>
> 解决分布式事务解决方式 有 **两阶段提交**，**TCC**等，还有常用就是**最终一致性方案**

**利用消息中间件,将业务异步化**

>如两人转钱,A用户扣款,在扣款成功时发送消息到消息中间件,加钱业务订阅扣款成功消息,执行加钱操作

##### 问题

>  先扣款后发消息,万一消息失败了,A扣了钱,B就没加钱
>
>  或者扣款消息发送成功,但A扣款失败,则B加了前,A没减钱

##### 解决

RocketMq消息中间件把消息分为**两个阶段**：**Prepared阶段**和**确认阶段**

###### prepared阶段（预备阶段）

发送一个消息到rq,这个消息只存储在commitlog中,但是consumeQueue中不可见,消费端无法消费消息

###### commit/rollback阶段(确认阶段)

> 这里的确认消息既可以commit也可以rollback(删除预备消息)

将prepared阶段的消息保存到consumeQueue中,消费端可以消费这个消息

![9033085-dd384efdd1a1a232 (1)](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/9033085-dd384efdd1a1a232%20(1).png)

###### 流程

1、在扣款之前，**先发送预备消息**
2、发送预备消息成功后，**执行本地扣款事务**
3、扣款成功后，**再发送确认消息**
4、消息端（加钱业务）可以看到确认消息，**消费此消息，进行加钱**

##### 新问题

当步骤5发送确认消息失败后,就又会出现新问题,A减了钱,B没收到加钱的消息

##### 解决

RocketMQ**状态回查**,它会定时遍历commitlog中的预备消息

> 预备消息最终肯定要么commit要么rollback,所以遍历预备消息去回查本地业务的执行状态
>
> 如果本地业务没有执行则rollback
>
> 如果本地业务执行成功(已经执行了),就补发commit消息
>
> 如果超过回查次数,默认回滚
>
> 在第5步犹豫了一下,再回去确认下,4是否做好了,没做好就确认回滚,做好了就提交

![9033085-cbc9d76a33d7c01c](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/9033085-cbc9d76a33d7c01c.png)

> 如果本地事务执行了很多张表,不需要将那些表都进行判断执行成功,而是设计一个Transaction表将**业务表和Transaction绑定在同一个本地事务中**,如果扣款本地事务成功时，Transaction中应当已经记录该TransactionId的状态为[已完成],当RocketMq回查时，只需要检查对应的**TransactionId的状态是否是「已完成」就好**

### Redis 分布式锁

多线程程序时,避免同时操作一个共享变量产生数据问题,通常需要一把锁来"互斥",以保证共享变量的正确性,其范围是在**同一个进程**中!

如果是**多个进程**,需要同时操作一个共享资源,就要用分布式锁来解决!

> 微服务架构,意味着一个应用会部署多个进程,那这多个进程如果需要修改MySQL中同一行记录时,为了避免操作乱序导致数据错误,就需要引入**分布式锁**,想要实现分布式锁，必须借助一个**外部系统**，所有进程都去这个系统上申请「加锁」,两个请求同时进来,只给一个进程返回成功,另一个返回失败
>
> > 这个外部系统,可以是MySQL,也可是Redis或者Zookeeper,为了性能通常考虑用后两者
>
> ![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/v2-241ea419cdba1e2b4b6f9685727fc11a_1440w.jpg)









## SpringCloud

公司技术栈多样化,用SpringCloud搭建微服务

### HTTP协议

http是一种网络传输协议,基于TCP,规定了数据传输的格式,现在客户端浏览器与服务端通信基本都采用HTTP协议,也可用来进行**远程服务调用**,缺点是**消息封装臃肿**,优势是对**服务的提供和调用方法没有任何技术限定**,更符合微服务理念,Rest风格就可以通过http协议来实现

> RPC也是一种**远程服务调用**的方式详情戳[RPC](#RPC)

### Spring的RestTemplate

spring提供了一个RestTemplate模板工具类,对基于Http的客户端进行了封装,并实现了对象与json的序列化和反序列化,RestTemplate并没有限定Http的客户端类型，而是进行了抽象，目前常用的3种都有支持(HttpClient,OkHttp,JDK原生URLConnection(默认))

#### 使用

在启动类可以注册一个RestTemplate对象

```java
	@Bean
	public RestTemplate restTemplate() {
		return new RestTemplate();
	}
```

在类中直接@Autowired注入

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = HttpDemoApplication.class)
public class HttpDemoApplicationTests {

	@Autowired
	private RestTemplate restTemplate;

	@Test
	public void httpGet() {
		User user = this.restTemplate.getForObject("http://localhost/user/8", User.class);
		System.out.println(user);
	}
}
```

- 通过RestTemplate的getForObject()方法，传递url地址及实体类的字节码，RestTemplate会自动发起请求，接收响应，并且帮我们对响应结果进行反序列化。

### SpringCloud

Spring最擅长的就是集成，把世界上最好的框架拿过来，集成到自己的项目中。

SpringCloud也是一样，它将现在非常流行的一些技术整合到一起，实现了诸如：配置管理，服务发现，智能路由，负载均衡，熔断器，控制总线，集群状态等等功能。其主要涉及的组件包括：

Netflix：  

- Eureka：注册中心   nacos 注册中心 作用：服务注册、服务发现
- gateway：服务网关   作用：智能路由、服务鉴权
- Ribbon：负载均衡  
- Feign：服务调用 
- Hystix：熔断器
- springcloudconfig 配置中心

版本命名为伦敦地铁站的名字

导入SpringCloud坐标,mapper扫描坐标,mysql驱动坐标,

![image-20211020093315447](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211020093315447.png)

### Eureka

提供服务注册和发现,是注册中心,分为Eureka服务端和Eureka客户端,服务的注册和发现,看做网约车平台,负责管理记录服务提供者的信息,服务调用者无需自己寻找服务，而是把自己的需求告诉Eureka，然后Eureka会把符合你需求的服务告诉你.

服务提供方通过"心跳"机制进行监控,出现问题后会把它从服务列表中剔除

![1548578752909](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1548578752909.png)

> * Eureka-Server:服务注册中心（可以是一个集群），对外暴露自己的地址。
>
> - 提供者：启动后向Eureka注册自己信息（地址，服务名称等），并且定期进行服务续约
> - 消费者：服务调用方，会定期去Eureka拉取服务列表，然后使用负载均衡算法选出一个服务进行调用。
> - 心跳(续约)：提供者定期通过http方式向Eureka刷新自己的状态

#### 服务注册和发现

注册服务和服务发现都是客户端

##### 注册服务

1. 在服务上添加Eureka客户端依赖,把服务注册到EurekaServer中

```xml
<!-- Eureka客户端 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2. 在服务提供者启动类上添加`@EnableDiscoveryClient `// 开启Eureka客户端发现功能

3. 配置yaml

   ```yaml
   server:
     port: 8081
   spring:
     datasource:
       url: jdbc:mysql://localhost:3306/mydb01
       username: root
       password: 123
     application:
       name: user-service # 应用名称  将来会作为服务的id使用。
   eureka:
     client:
       service-url: # EurekaServer地址
         defaultZone: http://127.0.0.1:10086/eureka
   #--------------注册时默认使用主机名,如果想用ip注册改用以下配置--------    
     instance:
       ip-address: 127.0.0.1 # ip地址
       prefer-ip-address: true # 更倾向于使用ip，而不是host名
       instance-id: ${eureka.instance.ip-address}:${server.port} # 自定义实例的id
   ```

##### 服务发现

1. 服务消费者端添加Eureka依赖

2. 启动类添加`@EnableDiscoveryClient`开启Eureka客户端 

3. 修改配置

   ```yaml
   server:
     port: 8080
   spring:
     application:
       name: consumer # 应用名称
   eureka:
     client:
       service-url: # EurekaServer地址
         defaultZone: http://127.0.0.1:10086/eureka
   ```

4. 注入@DiscoveryClient,并用DiscoveryClient的getInstances("服务提供者端配置的应用名称")取出服务实例,instance.get(0);//获取服务列表第一个实例

```java
@RestController
@RequestMapping("consumer")
public class ConsumerController {

    @Autowired
    private RestTemplate restTemplate;

    @Autowired
    private DiscoveryClient discoveryClient;

    @GetMapping("{id}")
    public User queryById(@PathVariable("id") Long id){
        // 根据服务id(spring.application.name)，获取服务实例列表
        List<ServiceInstance> instances = discoveryClient.getInstances("user-service");
        // 取出一个服务实例
        ServiceInstance instance = instances.get(0);
        // 查询
        User user = restTemplate.getForObject(instance.getUri()+"/user/" + id, User.class); //没用注册中心调用服务时候把url硬编码到代码中,地址出现变更,得不到通知,不方便维护
        return user;
    }
}
```

#### Eureka客户端

##### 服务注册

服务提供者在启动时，会检测配置属性中的：`eureka.client.register-with-erueka=true`参数是否正确，事实上默认就是true。如果值确实为true，则会向EurekaServer发起一个Rest请求，并携带自己的元数据信息，Eureka Server会把这些信息保存到一个双层Map结构中。

- 第一层Map的Key就是服务id，一般是配置中的`spring.application.name`属性
- 第二层Map的key是服务的实例id。一般host+ serviceId + port，例如：`locahost:user-service:8081`
- 值则是服务的实例对象，也就是说一个服务，可以同时启动多个不同实例，形成集群。

##### 服务续约

注册完成后,服务提供者维持一个**心跳**(定时向EurekaServer发起Rest请求),告诉EurekaServer"我还活着",称之为"服务续约",默认续约间隔30s,失效时间90s

> 默认情况下每个30秒服务会向注册中心发送一次心跳，证明自己还活着。如果超过90秒没有发送心跳，EurekaServer就会认为该服务宕机，会从服务列表中移除，这两个值在生产环境不要修改，默认即可。

##### 获取服务列表

当**服务消费者**启动时，会检测`eureka.client.fetch-registry=true`参数的值，如果为true，则会从Eureka Server服务的列表只读备份，然后缓存在本地。并且`每隔30秒`会重新获取并更新数据。通过以下方式更改

```yaml
eureka:
  client:
    registry-fetch-interval-seconds: 30
```

#### 服务下线,失效剔除,自我保护

##### 服务下线

当服务进行正常关闭操作时，它会触发一个服务下线的REST请求给Eureka Server，告诉服务注册中心：“我要下线了”。服务中心接受到请求之后，将该服务置为下线状态。

##### 失效剔除

服务可能由于内存溢出或网络故障等原因使得服务不能正常的工作，而服务注册中心并未收到“服务下线”的请求。相对于服务提供者的“服务续约”操作，服务注册中心在启动时会创建一个定时任务，默认每隔一段时间（默认为60秒）将当前清单中超时（默认为90秒）没有续约的服务剔除，这个操作被称为失效剔除。**每60s剔除一次超时时间超过90s的宕机服务**

##### 自我保护

网络延迟等原因，心跳失败实例的比例很有可能超标，但是此时就把服务剔除列表并不妥当，因为服务可能没有宕机,Eureka会统计服务实例最近15分钟心跳续约的比例是否低于了85%,开启自我保护,不再剔除服务

#### Eureka和Zookeeper对比

CAP原则分布式系统三个指标,在分布式系统中不能同时满足,P必须满足,组合只有CP或者AP

> C(Consistency)数据一致性
>
> > 在一个一致性的系统中，客户端向任何服务器发起一个写请求，将一个值写入服务器并得到响应，那么之后向任何服务器发起读请求，都必须读取到这个值(或者更加新的值)
>
> A(Availability)服务可用性
>
> > 在一个可用的分布式系统中，客户端向其中一个服务器发起一个请求且该服务器未崩溃，那么这个服务器最终必须响应客户端的请求
>
> P(Partition tolerance)服务对网络分区故障的容错性
>
> > 要满足P,分布式系统在任意的网络分区情况下都必须正常的工作,无论如何分区,你节点间互相通信断了,你都得保证正常运行
>
> 一个分布式系统里面，节点组成的网络本来应该是连通的。然而可能因为一些故障，使得有些节点之间不连通了，整个网络就分成了几块区域。数据就散布在了这些不连通的区域中。这就叫分区。大多数分布式系统都分布在多个子网络。每个子网络就叫做一个区（partition）。分区容错的意思是，区间通信可能失败。
>
> 当你一个数据项只在一个节点中保存，那么分区出现后，和这个节点不连通的部分就访问不到这个数据了。这时分区就是无法容忍的。
>
> 提高分区容忍性的办法就是一个数据项复制到多个节点上，那么出现分区之后，这一数据项就可能分布到各个区里。容忍性就提高了。
>
> **然而，要把数据复制到多个节点，就会带来一致性的问题，就是多个节点上面的数据可能是不一致的。要保证一致，每次写操作就都要等待全部节点写成功，而这等待又会带来可用性的问题。**
>
> 总的来说就是，数据存在的节点越多，分区容忍性越高，但要复制更新的数据就越多，一致性就越难保证。为了保证一致性，更新所有节点数据所需要的时间就越长，可用性就会降低。

一个保证了CP而一个舍弃了A的分布式系统，一旦发生网络故障或者消息丢失等情况，就要牺牲用户的体验，等待所有数据全部一致了之后再让用户访问系统,例如很多分布式数据库

一旦网络问题发生，节点之间可能会失去联系。为了保证高可用，需要在用户访问时可以马上得到返回，则每个节点只能用本地数据提供服务，而这样会导致全局数据的不一致性,如12306买票

Eureka满足AP(服务可用性和容错性),Zookeeper满足CP(数据一致性和容错性)

> zookeeper数据在各个服务间同步完成后才返回用户结果,但监听不到心跳会进行剔除,服务不可用
>
> eureka当网络故障时,eureka自我保护机制不会立刻剔除服务,虽然服务不一定可用,但至少能获得服务列表(服务降级等),满足高可用需求

>zookeeper集群有主从之分,zk集群有服务宕机,进行选举机制,集群可能因为选举导致阻塞,服务不可用
>
>eureka 集群中有服务宕机,因各服务器平等,所以其他服务不受影响

> zk是监听机制(监听节点变化,客户端立即得到通知)
>
> eureka服务发现会主动拉取服务,获取服务列表后会缓存起来,每隔30s重新拉取服务列表

#### BASE理论

全称：Basically Available(基本可用)，Soft state（软状态）,和 Eventually consistent（最终一致性）,即使无法做到强一致性（Strong consistency），但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性（Eventual consistency）。

##### Basically Available(基本可用)

什么是基本可用呢？假设系统，出现了不可预知的故障，但还是能用，相比较正常的系统而言：

1. 响应时间上的损失：正常情况下的搜索引擎 0.5 秒即返回给用户结果，而**基本可用**的搜索引擎可以在 1 秒作用返回结果。
2. 功能上的损失：在一个电商网站上，正常情况下，用户可以顺利完成每一笔订单，但是到了大促期间，为了保护购物系统的稳定性，部分消费者可能会被引导到一个降级页面。

##### Soft state（软状态）

什么是软状态呢？相对于原子性而言，要求多个节点的数据副本都是一致的，这是一种 “硬状态”。

软状态指的是：允许系统中的数据存在中间状态，并认为该状态不影响系统的整体可用性，即允许系统在多个不同节点的数据副本存在数据延时。

##### Eventually consistent（最终一致性）

系统能够保证在没有其他新的更新操作的情况下，数据最终一定能够达到一致的状态，因此所有客户端对系统的数据访问最终都能够获取到最新的值。





### Ribbon负载均衡

在消费者RestTamplate上加上@LoadBalanced

默认为**轮循**,可以在消费者yml中指定策略

```yaml
user-service:
  ribbon: 
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule  # 修改默认轮询策略为随机负载均衡测试
```

### Hystrix(豪猪,浑身刺,不好惹)

#### 概念

Hystix是Netflix开源的一个延迟和容错库，用于隔离访问远程服务、第三方库，防止出现级联失败。(解决服务雪崩)

雪崩(一个请求,需要调用多个微服务接口才能实现,一个服务A需要请求多个微服务B群,其中有一个B挂掉后,没有给A响应,这个服务A一直处于等待状态,当服务C要来请求服务A时,就会有多个A服务线程在等待,服务器资源耗尽,导致服务雪崩)比作一个生产线,需要某零件,整个生产线的产品必须用这个零件才能继续工作,此时零件不能供应,导致整个生产线瘫痪

#### 解决雪崩

##### 线程隔离、服务降级(解决雪崩一个方式)

Hystrix为每个依赖服务调用分配一个小**线程池**，如果线程池已满调用将被拒绝调用，**加速失败判定时间**,用户的请求将不再直接访问服务，而是通过线程池中的空闲线程来访问服务，如果**线程池已满**，或者**请求超时**，则会进行**服务降级**

> 服务降级:优先保证核心服务，而非核心服务不可用或弱可用

请求故障时,不会被阻塞,不会无休止等待,至少可以看到一个执行结果(友好提示信息)

服务降级虽然会导致请求失败，但是不会导致阻塞，而且最多会影响这个依赖服务对应的线程池中的资源，对其它服务没有响应。

触发Hystix服务降级的情况：

- 线程池已满
- 请求超时

###### 使用

1.导入依赖

2.添加注解(@EnableCircuitBreaker) 可以直接`@SpringCloudApplication`

> SpringCloudApplication继承了
>
> * @SpringBootApplication 
>
> * @EnableDiscoveryClient //开启Eureka客户端功能  
> * @EnableCircuitBreaker//开启熔断 

3.`@HystrixCommand(fallbackMethod="fallbackMethod")`//设置服务降级操作

自定义服务降级逻辑（要提前写好失败时降级的处理逻辑，方法要和服务的参数和返回值类型一致）

上面的方法的复用性低，**去掉**@HystrixCommand()里的**指定**,可以在类上指明统一的失败降级方法`@DefaultPropertis(fallbackMethod="defaultFallBack")`**设置统一降级逻辑**

类里定义defaultFallBack方法(参数不用写,因为是默认的,这里可能是id可能是字符串等)方法加上@HystrixCommand(),

超时设置(Hystix的默认超时时长为1,通过如下配置修改超时设置)

请求时间超过此值时会触发**服务降级**(按照业务逻辑的复杂度来定,假设业务A需要10s才能完成,则超时时间就要大于10s,如果业务B用0.1S就可以完成,则超时时间就可以不用1s,满足熔断器一定条件后进入熔断器(当不满足熔断的条件熔断器一直关闭,当满足熔断状态时进入熔断器(其中还会进入半开状态)判断后还不能正常处理请求进入熔断器打开状态,则服务降级))

```yaml
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 2000
```

##### 服务熔断(解决雪崩另一个方式)

###### 熔断器

也叫断路器，其英文单词为：Circuit Breaker   可以相较于电阻丝(打开时电路断路灯泡灭, 关闭时电路开路灯泡开) 

###### 状态机的3种状态:

- Closed：关闭状态（断路器关闭），所有请求都正常访问。
- Open：打开状态（断路器打开），所有请求都会执行服务降级。Hystix会对请求情况计数，当一定时间内失败请求百分比达到阈值，则触发熔断，断路器会完全关闭。默认失败比例的阈值是**50%**，请求次数最少**不低于20次**。
- Half Open：半开状态，open状态不是永久的，打开后会进入休眠时间（默认是5S）。随后断路器会自动进入半开状态。此时会释放1次请求通过，若这个请求是健康的，则会关闭断路器，否则继续保持打开，再次进行5秒休眠计时。

![1628302744694](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1628302744694.png)

### Feign(伪装)

Feign可以把Rest的请求进行隐藏，伪装成类似SpringMVC的Controller一样。不用再自己拼接url，拼接参数等等操作，一切都交给Feign去做。去注册中心找服务的远程调用,提高代码复用性.

ribbon负载均衡功能,以下拼接url调用的方式不优雅~

```java
String baseUrl = "http://user-service/user/";
User user = this.restTemplate.getForObject(baseUrl + id, User.class)
```

#### 使用

1.导入依赖

2.定义feign远程调用的接口添加` @FeignClient("这里写yml的name")`

> 1. 去注册中心找服务找user-service(去注册中心基于服务名称查询服务信息 )
> 2. 去服务里找具体请求资源
>
> ```java
> @FeignClient("user-service") //去注册中心基于服务名称查询服务信息  http://localhost:8081/user/5
> public interface UserClient {
> 
>     @GetMapping("/user/{id}")
>     public User findById(@PathVariable("id") Long id);
> }
> ```
>
> Feign会通过动态代理，帮我们生成实现类。这点跟mybatis的mapper很像

3.更改controller`@Autowired`注入步骤2定义的接口

4.在启动类上写开启feign`@EnableFeignClients`

#### Feign内置了负载均衡

已经集成了Ribbon依赖和自动配置,不需要额外引入依赖,也不需要在注册RestTemplate对象

```yaml
Ribbon:
  ReadTimeout: 2000 # 数据通信超时时长
  ConnectTimeout: 500 # 连接超时时长  
  MaxAutoRetries: 0 # 当前服务器的重试次数
  MaxAutoRetriesNextServer: 1 # 重试多少次服务
  OkToRetryOnAllOperations: false # 是否对所有的请求方式都重试 只对get方法重试
#----feign对hystrix也有集成,需要配置以下参数开启,但不像ribbon中那么简单----
feign:
  hystrix:
    enabled: true # 开启Feign的熔断功能
```

这里设置的时间和**熔断器的时间要配合**来设置,熔断器的时间要大于这里配置的时间

这里配置的最少的时间计算:(2000+500)+(2000+500)*1 

(2000+500)*(0+1)*(1+1)

如果还设置了MaxAutoRetries:1 则时间为

一次请求时间     当前服务器重试时间                 重试服务时间

(2000+500)   +        (2000+500)乘1        +     ((2000+500)+(2000+500)乘1)乘1     

这里的时间会与上面设置的熔断器有冲突,所以熔断器的延迟时间>这里配置的最少时间

### 网关Gateway(重要)

- 安全 ，只有网关系统对外进行暴露，微服务可以隐藏在内网，通过防火墙保护。
- 易于监控。可以在网关收集监控数据并将其推送到外部系统进行分析。
- 易于认证。可以在网关上进行认证，然后再将请求转发到后端的微服务，而无须在每个微服务中进行认证。
- 减少了客户端与各个微服务之间的交互次数
- 易于统一授权。

重要：路由设置 拦截器

#### 概述

网关为微服务架构提供一种简单有效统一的API路由管理

在微服务架构中，不同的微服务可以有不同的网络地址，各个微服务之间通过互相调用完成用户请求，客户端可能通过调用N个微服务的接口完成一个用户请求

网关是系统入口,封装了应用程序的内部结构,为客户端提供统一服务,与业务本身功能无关的公共逻辑可以在这里实现,认证、**鉴权**、监控、缓存、负载均衡、流量管控、**路由转发**等。

在目前的网关解决方案里，有Nginx+ Lua、Netflix Zuul 、Spring Cloud Gateway等等

存在的问题：1.客户端多次请求不同的微服务，增加客户端的复杂性  2.认证复杂，每个服务都要进行认证  3.http请求不同服务次数增加，性能不高

#### 使用

##### 静态和动态路由

1.创建网关服务模块，导入依赖

2.启动类

3.配置文件

4. application.yml 

   ```yml
   server:
     port: 10010
   spring:
     application:
       name: api-gateway
     cloud:
       # 网关配置
       gateway:
         # 路由配置：转发规则
         routes: #集合。
           # id: 唯一标识。默认是一个UUID
           # uri: 转发路径
           # predicates: 条件,用于请求网关路径的匹配规则
           - id: user-service #代表集合
             #uri: http://localhost:8081/ #写死,静态路由的配置方式
             uri: lb://user-service   # 网关去注册中心查询服务信息，而不是写死服务信息，叫做动态路由 lb loadBlance
             predicates:
               - Path=/user/** #用localhost:8081/user/访问,会将/user在请求路径保留
               filters:
          	    - StripPrefix=1 # 不给真实路径后拼上predicates里设置的/user
          	    #翻译翻译: 如果没有写filters,请求网关时 localhost:10010/user 能定位到  localhost:8081/user(会在此处拼一个predictes:-Path设置的/user) #所以 localhost:10010/user/2 --> localhost:8081/user/2
                   	# 如果写了filters,请求网关时 localhost:10010/user  只能定位到localhost:8081#所以 localhost:10010/user/user/2 --> localhost:8081/user/2
          	    - AddRequestParameter=name,admin #设置请求参数 测试内置局部过滤器其中一个(给请求参数赋默认值,这个其实没用,设置默认值可以在方法上写@RequestParam(defaultValue=" "))
          	    
   
   #添加注册中心的地址  配置动态路由时,要把网关GateWay作为Eureka的客户端
   #eureka:
   #  client:
   #    service-url:
   #      defaultZone: http://127.0.0.1:10086/eureka
   #  instance:
   #    prefer-ip-address: true
   #    hostname: 127.0.0.1
   #    instance-id: ${eureka.instance.hostname}:${spring.application.name}:${server.port}
   
   ```

#### 两种类型过滤器

##### 局部过滤器

- GatewayFilter 局部过滤器，是针对单个路由的过滤器。
- 在Spring Cloud Gateway 组件中提供了**大量内置的局部过滤器**，对请求和响应做过滤操作。
- 遵循约定大于配置的思想，只需要在配置文件配置局部过滤器名称，并为其指定对应的值，就可以让其生效。

局部过滤器(GatewayFilter)只需要在上面的yml的filters中配置就可以,对请求和响应做过滤操作。

##### 全局过滤器

用的最多的是全局过滤器

* GlobalFilter 全局过滤器，**不需要在配置文件中配置**，**系统初始化时加载**，**并作用在每个路由上**

* Spring Cloud Gateway 核心的功能也是通过内置的全局过滤器来完成。

* 自定义全局过滤器步骤：

  1. 网关服务模块下定义类实现 GlobalFilter 和 Ordered接口
  2. 复写方法(ctrl+i)
  3. 完成逻辑处理

  ![1587546455139](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1587546455139.png)

###### 用途

校验请求参数是否带有token,有就放行,否则提示未授权



#### 两种过滤器方式

* **pre 过滤器**，在转发之前执行，可以做参数校验、权限校验、流量监控、日志输出、协议转换等。

* **post 过滤器**，在后端微服务响应之后并且给前端响应之前执行，可以做响应内容、响应头的修改，日志的输出，流量监控等。

  ![1587546321584](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1587546321584.png)



### SpringCloudConfig集中配置组件

分布式系统里,服务数量多并且代码运行环境也不同,(开发环境,测试环境,预生产环境,正式生产环境即线上环境,每个微服务都要独立维护配置文件,非常麻烦),为了统一管理,需要**分布式配置中心组件**。spring cloud config支持配置服务放在配置服务的**内存**中，也支持放在**远程Git仓库**中。组件分两个角色，一是**config server**，二是**config client**。

Config Server是一个可横向扩展、集中式的配置服务器，它用于集中管理应用程序各个环境下的配置，默认使用Git存储配置文件内容，也可以使用SVN存储，或者是本地文件存储。

Config Client是Config Server的客户端，用于操作存储在Config Server中的配置内容。微服务在启动时会请求Config Server获取配置文件的内容，请求到后再启动容器。

#### 使用

##### Config Server 端

1. 将让配置中心管理的模块的yml文件提交到gitee远程仓库，命名为{application应用名称}-{profile开发环境}.yml 或.properties,复制git地址

2. 创建配置中心工程模块导入config的服务和spingboot提供的项目监控的jar包

   > ```xml
   > <dependencies>
   >  <dependency>
   >      <groupId>org.springframework.cloud</groupId>
   >      <artifactId>spring-cloud-config-server</artifactId>
   >  </dependency>
   >  <dependency>
   >      <groupId>org.springframework.boot</groupId>
   >      <artifactId>spring-boot-starter-actuator</artifactId> #springboot提供的项目监控jar包
   >  </dependency>
   > </dependencies>
   > ```

3. 创建启动类ConfigServerApplication

   ```java
   @SpringBootApplication
   @EnableConfigServer //开启配置中心功能
   public class ConfigServerApplication {
       public static void main(String[] args) {
           SpringApplication.run(ConfigServerApplication.class,args);
       }
   }
   ```

4. 编写配置文件yml

   ```yaml
   server:
     port: 12000
   spring:
     application:
       name: config-server
     cloud:
       config:
         server:
           git:
             uri: https://gitee.com/gorilla/config-test.git  # 管理配置文件git仓库地址
   ```

##### Config client端

1. `把配置文件交给配置中心管理的模块`中导入配置中心起步和配置中心客户端依赖（例:user-service的yml文件给配置中心管理，user-service就是配置中心客户端,把哪个服务交给配置中心管理,这个服务就是配置中心的客户端）

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

同样这个模块添加bootstrap.yml（在其中配置应用名称，开发环境，git分支以及配置中心的服务端口），**bootstrap.yml是比application.yml加载顺序更早的yml**,因为已经把user-service的yml上传到git了(交给配置中心管理了)，在user-service服务启动时,要先加载bootstrap.yml,其实就是先从配置中心(配置中心把配置放在git里)拿到user-service的配置。

```yaml
spring:
  cloud:
    config:
      name: user
      profile: dev
      label: master
      uri: http://127.0.0.1:12000
```



### Nacos

> 问题:windows环境下的demo
>
> 1. nacos配置单机模式启动(默认集群启动cluster)
> 2. nacos要运行sql，修改配置文件 

nacos和Eureka相似，也是一个注册中心，但功能更为强大，由阿里推出

#### 使用：

//spring-cloud 2.4后,不用写@Enable...

1. 安装nacos

2. 在父工程里添加启用nacos注册中心的依赖

```xml
<properties>
  <com.alibaba.cloud>2.1.2.RELEASE</com.alibaba.cloud>
</properties>
<dependencyManagement>
<!--spring cloud alibaba-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-dependencies</artifactId>
    <version>${com.alibaba.cloud}</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
</dependencyManagement>    
```

3. 在服务里导入nacos 的依赖

   ```xml
   <dependency>
       <groupId>com.alibaba.cloud</groupId>
       <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
   </dependency>
   ```

4. 在每个模块（服务提供者，消费者，网关等）里添加nacos注册发现依赖，并添加nacos的配置，默认端口8848

   ```yaml
   spring:
     cloud:
       nacos:
         discovery:
           server-addr: localhost:8848
   ```

5. nacos有自己的配置中心，也需要添加配置中心的客户端依赖（谁想让配置中心管理自己的yaml谁就是配置中心的客户端），把application.yaml手动添加到nacos配置中心中，nacos的配置文件默认**yaml**，不能识别yml

#### 选型

采用Eureka方案的考虑

- 想用Spring Cloud原生全家桶
- 想用本地文件和Git作为配置管理的,将配置与服务分开管理
- 考虑短期的稳定性

采用Nacos方案的考虑

- 想在线对服务进行上下线和流量管理
- 不想采用MQ实现配置中心动态刷新
- 不想新增配置中心生产集群
- 考虑引入Spring Cloud Alibaba生态



#### Eureka高可用

EurekaServer可以是一个集群,eureka**服务端**互相注册互相拉取注册信息,多个Eureka Server之间也会互相注册为服务，当服务提供者注册到Eureka Server集群中的某个节点时，该节点会把服务的信息同步给集群中的每个节点，从而实现高可用集群。

**高可用注册中心把EurekaServer自己也作为一个服务，注册到其它EurekaServer上**

配置时: 把service-url的值改成了另外一台EurekaServer的地址，而不是自己

复制另一个Eureka服务器,通过JVM参数覆盖配置文件

```
-Dserver.port=10087 -Deureka.cl+ient.serviceUrl.defaultZone=http://127.0.0.1:10086/eureka
```

![image-20211019120738248](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211019120738248.png)

#### 











## Docker

Docker 是dotCloud 公司出品的一款开源的应用容器引擎,可以让开发者打包他们的**应用以及依赖**打包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上。

大型项目组件多,运行环境复杂,会遇到问题:

* 依赖关系复杂,容易出现兼容问题
* 开发,测试,生产环境有差异

### docker如何解决

#### 解决依赖兼容问题:

- 将应用的Libs（函数库）、Deps（依赖）、配置与应用一起打包
- 将每个应用放到一个隔离`容器`去运行，避免互相干扰

#### 解决不同系统环境问题:

* Docker将用户程序与需要调用的系统(比如Ubuntu)函数库一起打包
* Docker运行到不同操作系统时,直接基于打包的库函数,借助于操作系统的Linux内核运行

**镜像（Image）：**

- Docker将应用程序及其所需的依赖、函数库、环境、配置等文件打包在一起，称为镜像。
- 镜像可以用来创建docker容器。

**容器（Container）：**

- **镜像中的应用程序运行后形成的进程**就是容器，只是Docker会给容器做隔离，对外不可见。
- 镜像和容器的关系，就像是面向对象程序设计中的类和对象一样。

**仓库（Registry）：**

- DockerHub是一个Docker镜像的托管平台。这样的平台称为Docker Registry。
- 国内也有类似于DockerHub 的公开服务，比如 网易云镜像服务、阿里云镜像、中科大库等。

### Docker架构

**Docker是一个CS架构的程序，由两部分组成：**

- 服务端(server)：Docker守护进程，负责处理Docker指令，管理镜像、容器等
- 客户端(client)：通过命令或RestAPI向Docker服务端发送指令。可以在本地或远程向服务端发送指令

![image-20210709085843821](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210709085843821.png)

通过MobaXterm远程连接

### 使用

#### Docker安装

```shell
# 1、yum 包更新到最新 
yum update

# 2、安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的 
yum install -y yum-utils device-mapper-persistent-data lvm2

# 3、设置yum源
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 4、安装docker，出现输入的界面都按 y 
yum install -y docker-ce

# 5、查看docker版本，验证是否验证成功
docker -v

# 6、启动docker环境
systemctl start docker

# 7、设置开机自启动
systemctl enable docker
```

#### 配置镜像加速

从docker hub（https://hub.docker.com/）上下载docker镜像，太慢

> 创建或修改 `/etc/docker/daemon.json `文件，修改为如下形式：

```shell
# 中国科技大学镜像地址
{
 "registry-mirrors":["https://docker.mirrors.ustc.edu.cn"]
}

# 网易云镜像地址
{
 "registry-mirrors":["http://hub-mirror.c.163.com"]
}

# 阿里云镜像,需要自己手动生成的加速地址:https://cr.console.aliyun.com/
```

```shell
# 重启docker
systemctl restart docker

# 查看是否成功
docker info
```

### Docker 命令

#### 基本命令

```shell
# 查看本地镜像 
docker images

# 搜索镜像仓库，推荐：https://hub.docker.com/
docker search 镜像名称

# 下载（拉取）镜像，镜像名称格式为 名称:版本号
docker pull 镜像名称

# 删除镜像
docker rmi 镜像名称

删除版本为none的docker镜像
docker images|grep none|awk '{print $3}'|xargs docker rmi
```

#### 容器相关命令

```shell
# 查看本地容器
docker ps 	  # 能查看正在运行
docker ps -a  # 能查看所有的容器（运行的和停止的）

# 创建一个新的容器并运行(-d  后台运行容器，并返回容器ID  -p 主机端口:容器端口  指定映射关系  如果没有-p，容器也能运行，但无法被外界访问)
docker run -d -p 主机端口:容器端口 --name=容器名 镜像名称

# 进入容器内部
docker exec -it 容器名称 /bin/bash
（容器就是一个小型操作系统)
cd 进入  ls 看  exit退出容器
# 查看容器信息
docker inspect 容器名称

# 启动容器
docker start 容器名称

# 停止容器 
docker stop 容器名称

# 删除容器 （先关闭、后删除）
docker rm 容器名称

#查看容器详情
docker inspect 容器名称
```

### Docker应用部署

#### 部署nginx

![image-20210709095604485](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210709095604485.png) 

```shell
# 搜索nginx镜像
docker search nginx

# 拉取nginx镜像
docker pull nginx

# 创建并启动容器
docker run -d  --name=nginx -p 80:80  nginx


# 操作容器中的nginx
docker exec -it nginx /bin/bash
```

#### 部署MySQL

![image-20210709100010477](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210709100010477.png) 

```shell
# 搜索mysql镜像
docker search mysql

# 拉取mysql镜像
docker pull mysql:5.7

# 创建并启动容器
docker run -d -p 3306:3306 --name=mysql5.7 -e MYSQL_ROOT_PASSWORD=root mysql:5.7 \
--character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

# 操作容器中的mysql
docker exec -it mysql5.7 /bin/bash

```

<span style="color:red">docker 容器中的 mysql 配置文件（mysqld.cnf）中的 bind-address 参数设置为 0.0.0.0 或者 *，这样才能允许远程连接。</span>

#### 部署Tomcat

![image-20210709101203373](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210709101203373.png)  

```shell
# 搜索tomcat镜像
docker search tomcat

# 拉取tomcat镜像
docker pull tomcat:8.5

# 创建并启动容器
docker run -d --name=tomcat8.5 -p 8080:8080 tomcat:8.5

# 在本地模拟一个项目(创建一个hello目录,然后向目录中输出一个文件index.html,内容为hello world)
mkdir /root/hello
echo "hello world" > /root/hello/index.html

# 从宿主机复制文件到容器(docker cp 宿主机路径 容器名称:容器路径)
docker cp /root/hello tomcat8.5:/usr/local/tomcat/webapps/

# 操作容器中的tomcat
docker exec -it tomcat8.5 /bin/bash
```



#### 部署Redis

![image-20210709101614149](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210709101614149.png) 

```shell
# 搜索redis镜像
docker search redis

# 拉取redis镜像
docker pull redis:5.0

# 创建并启动容器
docker run -d --name=redis5.0 -p 6379:6379 redis:5.0

# 操作容器中的redis
docker exec -it redis5.0 /bin/bash
```

### Docker数据卷

- 数据卷是**宿主机中的一个目录或文件**，它可以被挂载到容器中，在容器中对它的操作可以直接反应到宿主机上
- 一个容器可以被挂载多个数据卷，一个数据卷也可以被多个容器同时挂载

![image-20201206120246273](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20201206120246273.png)

#### 配置数据卷

![image-20210709102322786](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20210709102322786.png)

```shell
# 创建启动容器时，使用 –v 参数 设置数据卷
docker run ... –v 宿主机目录(文件):容器内目录(文件) ...
```

> 目录必须是绝对路径,目录不存在会自动创建,可挂载多个数据卷

```shell
# 在/root目录下创建tomcat目录用于存储tomcat数据信息
mkdir -p /root/tomcat/webapps

# 创建容器，设置端口映射、目录映射
# docker run ... –v 宿主机目录(文件):容器内目录(文件) ...
docker run -d --name=tomcat1 -p 8081:8080 -v /root/tomcat/webapps:/usr/local/tomcat/webapps tomcat:8.5
docker run -d --name=tomcat2 -p 8082:8080 -v /root/tomcat/webapps:/usr/local/tomcat/webapps tomcat:8.5
```

### Compose

Compose是Docker的服务编排工具，主要用来构建基于Docker的复杂应用

> 一个环境多个程序,这些程序单独作为容器启动,需要同时启动同时停止,一个个操作麻烦,需要一个批量操作容器的工具

Compose通过一个配置文件来管理多个Docker容器，非常适合组合使用多个容器进行开发的场景

#### 使用步骤

- 利用 Dockerfile 定义运行环境镜像

- 使用 docker-compose.yml 定义组成应用的各服务

- 运行 docker-compose up  启动应用

  > docker-compose up 本质是docker-compose logs -f，它会收集所有容器的日志输出直到退出docker-compose up命令，或者容器都停止运行，比如创建容器时失败，会退出当前命令行

  > docker-compose up -d  以后台的方式运行容器,不会在终端上打印运行日志

- docker-compose stop   命令将停止运行的容器，但不会删除它们

- docker-compose down 

  > 命令将停止运行的容器，并且会删除已停止的容器以及已创建的所有网络。  我们可以down进一步迈出第一步，并添加`-v`标记以删除所有卷。这对于通过运行在环境中进行完全重置非常有用`docker-compose down -v`。

![image-20201206134809464](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20201206134809464.png)

### Docker和VM对比

#### 相同

- 容器和虚拟机具有相似的资源隔离和分配优势

#### 不同

- 容器虚拟化的是操作系统，虚拟机虚拟化的是硬件。
- 传统虚拟机可以运行不同的操作系统，容器只能运行同一类型操作系统



## Elasticsearch相关

### 数据分类:

* 结构化数据

  > 具有固定格式或有限长度的数据，如数据库，元数据等

* 非结构化数据

  > 指不定长或无固定格式的数据，如邮件，word 文档等磁盘上的文件

### 非结构化数据查询

1. 顺序扫描法

   一个个文档看,从头看到尾

2. 全文检索

   非结构化数据一部分信息提取,重新组织使其变得有结构,如字典的索引

   > 创建索引耗时,但一旦创建,可以使用多次

   全文检索底层依赖**倒排索引**数据结构

   > 一个数据库中,一般是以文档id作为索引,以文档内容作为记录
   >
   > 而Inverted index 是将单词或者记录作为索引,将文档id作为记录,通过单词/记录找文档，这种索引的结构叫 倒排索引结构

#### 创建索引

获得原始文档-->创建lucene能识别的文档对象-->分析文档-->创建索引

1. 首先把所有的原始数据进行编号，形成文档列表

   ![img](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/v2-d50ffd4b3bc38e25e281fea9e07e14e6_1440w.jpg)

2. 把文档数据进行分词，得到很多的词条，以词条为索引。保存包含这些词条的文档的编号信息。

中文要用IKAnalyzer(IK分词器)

​	![img](https://pic2.zhimg.com/80/v2-0e77a230cbce4cd3c8b8e121bb211518_1440w.jpg?source=1940ef5c)

#### 搜索

用户输入词条时,对用户输入的数据进行分词,得到用户要搜索的所有词条,拿着词条去倒排索引列表中进行匹配,找到这些词条就能找到包含这些词条的所有文档的编号

### ElasticSearch 弹性搜索?

大型网站索引数据庞大,使用lucene原生代码方式不合适,用到成熟的ElasticSearch(常见的有solr和ElasticSearch)

> lucene是 apache的全文检索引擎工具包

Elasticsearch、Logstash、Kibana是Elastic的完整产品线

#### es特点

* 分布式,无需人工搭建集群
* restful风格,一切API遵循Rest原则,Rest请[戳](#RESTful)
* 近实时搜索,数据更新在es中几乎完全同步

#### Kibana

Kibana是一个基于Node.js的Elasticsearch索引库数据统计工具,Elasticsearch索引数据的控制台，并且提供了一定的API提示,可以生成各种图表



Elasticsearch也是**基于Lucene的全文检索库**，本质也是**存储数据**，很多概念与MySQL类似的。

对比关系：

```
索引库（indices）---------------------------------Database 数据库

 文档（Document）---------------------------------Row 行

   域字段（Field）--------------------------------Columns 列 
     
映射配置（mappings）-------------------------------每个列的约束（类型、长度）   
```

Elasticsearch采用Rest风格API，因此其API就是一次http请求，你可以用任何工具发起http请求

#### 使用kibana对索引库操作

创建索引的请求格式：

- 请求方式：PUT

- 请求路径：/索引库名

- 请求参数：json格式：

  ```json
  {
      "settings": {
          "属性名": "属性值"
        }
  }
  ```

  settings：就是索引库设置，其中可以定义索引库的各种属性，目前我们可以不设置，都走默认。

##### 创建索引库

`put /gorilla`    相当于数据库中的建库

##### 查看索引库

`get /gorilla`      `head /gorilla`//看索引库是否存在

##### 删除索引库

`delete /gorilla`

#### 使用kebana对类型和映射操作

```json
PUT /索引库名/_mapping
{
  "properties": {
    "字段名": {
      "type": "类型,可以是text,keyword,short,date,integer,object等. text可以分词,keyword不分词,两者都是string类型",
      "index": true， //是否索引
      "store": true， //是否存储
      "analyzer": "分词器" //分词器
    }
  }
}
```

##### 查看映射关系

`get /gorilla/_mapping/可以在这里再加上某个类型映射`

#### 映射属性

##### type

- String类型，又分两种：

  - text：可分词，不可参与聚合
  - keyword：不可分词，数据会作为完整字段进行匹配，可以参与聚合

- Numerical：数值类型，分两类

  - 基本数据类型：long、interger、short、byte、double、float、half_float
  - 浮点数的高精度类型：scaled_float  
    - 需要指定一个精度因子，比如10或100。elasticsearch会把真实值乘以这个因子后存储，取出时再还原。

- Date：日期类型

  elasticsearch可以对日期格式化为字符串存储，但是建议我们存储为毫秒值，存储为long，节省空间。

- Array：数组类型

  - 进行匹配时，任意一个元素满足，都认为满足
  - 排序时，如果升序则用数组中的最小值来排序，如果降序则用数组中的最大值来排序

- Object：对象类型

##### index

- true：字段会被索引，则可以用来进行搜索过滤。默认值就是true
- false：字段不会被索引，不能用来搜索,比如图片信息不想被索引就设置为false

##### store

​	默认是false,lucene里这个值为false,文档列表就不会有这个值,但es里就算是false也能搜索到结果,因为es在创建文档索引库时,会把文档中的原始数据备份,保存到一个`_source`的属性中,可以通过过滤`_source`来选择显示哪些数据,为true时,`_source`以外额外存储一份数据,会多余

##### boost

权重,新增数据时,可指定该数据的权重,分值越高排名越前

#### 一次创建索引库和类型

```json
PUT gorilla //创建索引库和指定类型同时操作
{
  "settings":{
      "索引库属性名":"索引库属性值 这里可以不设置走默认"
   },
  "mappings": {
    "properties" : {
        "images" : {
          "type" : "keyword",
          "index" : false
        },
        "price" : {
          "type" : "float"
        }
      }
  }
}
```

#### 使用kibana对文档操作

##### 新增文档并随机生成id

通过post请求,可以向一个以及存在id索引库中添加文档数据

```
POST /gorilla/_doc/(在这里写可以指定id值,不写则自动生成id)
{
  "title":"华为手机",
  "price":4999,
  "images":"http://www.gorilla.com"
}
```

##### 查看文档

```
GET /gorilla/_doc/_aAxOXsB0FyBKJmFNyv9(这里这个是文档id)
```

##### 修改文档

```
PUT /gorilla/_doc/这里必须要指定id,id不存在则是新增,id存在是修改
{
  "title":"华为手机",
  "price":4999,
  "images":"http://www.gorillacom"
}
```

##### 删除文档

```
DELETE /索引库名/_doc/id值
```

#### 智能判断

添加字段之前没有定义过类型,es会根据输入的数据来判断配型,动态添加数据映射

它的底层原理是**动态模板映射**,要更改,就只修改动态模板

> 动态模板映射  将未映射的string类型自动映射成keyword类型
>
> ![1547005993592](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1547005993592.png)
>
> ```
> PUT gorilla
> {
> "mappings": {
>    "properties": {
>      "title": {
>        "type": "text",
>        "analyzer": "ik_max_word"
>      }
>    },
>    "dynamic_templates": [
>      {
>        "strings": {
>          "match_mapping_type": "string",
>          "mapping": {
>            "type": "keyword"
>          }
>        }
>      }
>    ]
> }
> }
> ```
>
> - title字段：统一映射为text类型，并制定分词器
> - 其它字段：只要是string类型，统一都处理为keyword类型

#### 查询

> 基本语法

```json
GET /索引库名/_search
{
    "query":{
        "查询类型":{
            "查询条件":"查询条件值"
        }
    }
}
```

##### 查询类型

###### match_all 查询所有

###### match 类型查询

``` 
GET /gorilla/_search
{
	"query":{
		"match":{
			"titile":"小米手机" //这样查询出来不仅 小米 小米电视 华为手机都能查到
			"operator":"and" //不写关系的话默认是or,要指定"and"的关系,包含"小米"和"手机"的词条才会被搜索到
		}
	}
}
```

###### term 词条匹配

搜索前不会对**搜索词**进行分词, 精确值匹配,这些精确值可能是数字,时间,布尔或者那些**没有分词**的字符串,term匹配是最小的单位不会处理匹配词

> text类型字段需要加上.keyword 因为text类型字段会被倒排索引进行存储,倒排索引会利用分词器将文本分词 lonely wolf 被分成 `lonely` 和`wolf`两个词,所以 `lonely wolf` 作为一个进行查询自然是无法查询到结果的,文档存的是小写,用term查询`Lonely`就是`Lonely`,他并不会将匹配词转为`lonely`
>
> ```
> POST index_001/_search
> {
> "query": {
>  "term": {
>    "name.keyword": {
>      "value": "lonely wolf"
>    }
>  }
> }
> }
> ```

###### range范围查询

range查询找出那些落在指定区间内的数字或者时间

```json
GET /gorilla/_search
{
    "query":{
        "range": {
            "price": {
                "gte":  1000.0, //gte大于等于
                "lt":   2800.00 //小于
            }
    	}
    }
}
```

###### bool多条件查询

`bool`把各种其它查询通过`must`（与）、`must_not`（非）、`should`（或）的方式进行组合

```json
GET /gorilla/_search
{
    "query":{
        "bool":{
        	"must":     { "match": { "title": "大米" }},
        	"must_not": { "match": { "title":  "电视" }},
        	"should":   { "match": { "title": "手机" }}
        }
    }
}
```

###### fuzzy模糊查询

```json
GET /gorilla/_search
{
  "query": {
    "fuzzy": {
      "title": "appla",//允许有偏差,但默认偏差不能超过2
      "fuzziness":1 //更改偏差
    }
  },
    "_source": {
        "includes":["title","price"], //进行结果过滤 只要这两个字段
    	"excludes":["images"]//不要哪个字段
    }
}
```

###### 过滤filter

所有的查询都会影响到文档的评分及排名。如果我们需要在查询结果中进行过滤，并且不希望过滤条件影响评分，那么就不要把过滤条件作为查询条件来用。而是使用`filter`方式：

```json
GET /gorilla/_search
{
    "query":{
        "bool":{
        	"must":{ "match": { "title": "小米手机" }},
        	"filter":{
                "range":{"price":{"gt":2000.00,"lt":3800.00}}
        	}
        }
    }
}
```

没有查询条件,不希望评分,可以使用`constant_score`取代filter语句的bool查询

###### 分页

- from：开始位置  默认是0
- size：每页大小

```json
GET /gorilla/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "price": {
        "order": "asc"
      }
    }
  ],
  "from": 0,
  "size": 3
}
```

###### 高亮

​	服务端搜索数据得到结果-->结果中加上约定好的标签-->前端页面提前写好标签的css样式,实现高亮

```json
GET /gorilla/_search
{
  "query": {
    "match": {
      "title": "手机"
    }
  },
  "highlight": {
    "pre_tags": "<font color='red'>",
    "post_tags": "</font>",
    "fields": {
      "title": {}
    }
  }
}
```

#### 聚合aggregations

聚合可以完成数据的统计和分析

​	什么品牌手机最受欢迎?这些手机的最高价格最低价格?实现这些统计功能的比数据库的sql要方便的多，而且查询速度非常快，可以实现近实时搜索效果。

`桶`和`度量`两种,桶用来聚合(根据国籍,年龄等划分),度量用来聚合运算(球平均值,最大最小值总数等)

### Elasticsearch集群

#### 单点的问题

- 单台机器存储容量有限
- 单服务器容易出现单点故障，无法实现高可用
- 单服务的并发处理能力有限

基于以上不足来搭建集群

#### 单点存储量的问题

将数据拆成多份,每一份存储到不同机器节点(node),减少每个节点数据量,这就是**数据的分布式存储**,也叫**数据分片**(shard)

![1553071443011](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1553071443011.png)

#### 单点故障

当出现单点故障,分片的数据不再完整,这时可以对分片的数据进行备份,存储到其他节点,这就是**数据备份**,也叫**数据副本**(relica)

但每个分片备份一份成本过高,解决: 对数据进行分片,存储到不同节点,然后对每个分片进行备份,将自己的数据放到对方的节点上,完成互相备份

![1553072275126](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/1553072275126.png)

#### 搭建集群

用一台机器模拟,复制三份es软件-->分别修改其配置文件下的elasticsearch.yml-->复制ik分词器到每个节点的插件目录中 以此为例

```yaml
#允许跨域名访问
http.cors.enabled: true
http.cors.allow-origin: "*"
network.host: 127.0.0.1
# 集群的名称 ,默认的集群名字 elasticsearch
cluster.name: heima-elastic
#当前节点名称 每个节点不一样
node.name: node-01
#数据的存放路径 每个节点不一样
path.data: D:\worksoft\gorilla\es\elasticsearch-9201\data
#日志的存放路径 每个节点不一样
path.logs: D:\worksoft\gorilla\es\elasticsearch-9201\logs
# http协议的对外端口 每个节点不一样
http.port: 9201
# TCP协议对外端口 每个节点不一样
transport.tcp.port: 9301
#三个节点相互发现
discovery.seed_hosts: ["127.0.0.1:9301","127.0.0.1:9302","127.0.0.1:9303"]
cluster.initial_master_nodes: ["node-01", "node-02", "node-03"]
# 是否为主节点
node.master: true
```

包含三部分内容：协议、域名、端口号

http://www.baidu.com:80

当浏览器发ajax请求，协议、域名、端口号三者任意一个不同，浏览器出于安全角度考虑，不允许直接访问。这个现象就是**跨域问题**

实际开发允许跨域访问,可以通过java代码解决跨域问题。CORS：跨域资源共享

```json
PUT /gorilla
{
  "settings": {
    "number_of_shards": 3  //分片数量，这里设置为3
    "number_of_replicas": 1 //副本数量，这里设置为1，每个分片一个备份，一个原始数据，共2份
  }
}
```

#### elasticsearch客户端

JavaRestClient 用java操作es

创建索引库时,也会创建type和映射关系,这些操作java的API繁琐,不如直接使用kibana去创建索引库

导入High Level REST Client的 maven依赖-->用@Before 编写client的初始化和@After关闭连接,释放资源

**新增文档**IndexRequest-->new 一个Item去放数据 -->转化json,并绑定对象-->执行新增文档操作client.index()

**查看文档**GetRequest-->通过client.get()-->解析响应

**修改文档**indexRequest-->设置数据,json格式,将对象转为json-->数据绑定到请求对象-->client.index执行

**删除文档**DeleteRequest --> client.delte()

**批量请求**BulkRequest

**搜索数据**new SearchRequest -->设置查询方式-->绑定查询对象-->执行查询-->遍历查询结果集

> `sourceBuilder.query(QueryBuilders.matchAllQuery())`来添加的。这个`query()`方法接受的参数是：`QueryBuilder`接口类型  接口提供了很多实现类，分别对应我们在之前中学习的不同类型的查询，例如：term查询、match查询、range查询、boolean查询等

分页start = (page - 1) * size

**聚合查询**

>  聚合条件通过`sourceBuilder.aggregation()`方法来设置，而参数是一个接口：`AggregationBuilder`，这个接口也有大量的实现类

**高亮**

- `new HighlightBuilder()`：创建高亮构建器
- `.field("title")`：指定高亮字段
- `.preTags("")`和`.postTags("")`：指定高亮的前置和后置标签





## 项目相关

### Swagger

Swagger 是一个规范完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务

#### 作用

* 使得前后端分离开发更加方便，有利于团队协作

* 接口的文档在线自动生成，降低后端开发人员编写接口文档的负担

Spring将Swagger纳入,现在叫Springfox

#### 使用

* 导入依赖
* 开启Swagger功能

微服务架构中写在common包里,用的时候直接加上注解`@configuration`和`@ComponentScan("工具类的全限定类名")`

#### 常用注解

**@Api**：修饰整个类，描述Controller的作用  

**@ApiOperation**：描述一个类的一个方法，或者说一个接口  value 指定名称   notes 添加备注

@**ApiModelProperty**:对象接收参数时,描述对象的一个字段

**@ApiImplicitParam**：一个请求参数  参数如下

| 属性         | 取值   | 作用                                          |
| ------------ | ------ | --------------------------------------------- |
| paramType    |        | 查询参数类型                                  |
|              | path   | 以地址的形式提交数据                          |
|              | query  | 直接跟参数完成自动映射赋值                    |
|              | body   | 以流的形式提交 仅支持POST                     |
|              | header | 参数在request headers 里边提交                |
|              | form   | 以form表单的形式提交 仅支持POST               |
| dataType     |        | 参数的数据类型 只作为标志说明，并没有实际验证 |
|              | Long   |                                               |
|              | String |                                               |
| name         |        | 接收参数名                                    |
| value        |        | 接收参数的意义描述                            |
| required     |        | 参数是否必填                                  |
|              | true   | 必填                                          |
|              | false  | 非必填                                        |
| defaultValue |        | 默认值                                        |

#### knife4j

knife4j是为Java MVC框架集成Swagger生成Api文档的增强解决方案,前身是swagger-bootstrap-ui,取名knife4j是希望它能像一把匕首一样小巧,轻量,并且功能强悍!

两大核心功能 **文档说明**和**在线调试**

| 注解              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| `@EnableSwagger2` | 该注解是Springfox-swagger框架提供的使用Swagger注解，该注解必须加 |
| `@EnableKnife4j`  | 该注解是`knife4j`提供的增强注解,Ui提供了例如动态参数、参数过滤、接口排序等增强功能,如果你想使用这些增强功能就必须加该注解，否则可以不用加 |

- config  配置信息

- controller  控制层

- entity 实体层

- feign   需要远程调用的feign接口

- mapper  持久层

- service  业务层接口

  - impl 业务层接口实现类

> mapper持久层继承了basemapper<实体类>
>
> service继承了Iservice<实体类>
>
> serviceImpl继承了ServiceImpl<mapper类,实体类>
>
> 就可以不用自己写sql代码,完成基础的数据库操作(以上集成的类均是mybatis-plus提供)
>
> >  要自己定义sql在yaml配置扫描mapper的xml,在xml里写sql

业务中返回值用的自己定义的结果类,ResponseResult 里面正确时候okResult错误时候errorresult

#### mybatis-plus

`@TableName("表名字")`//实体类上加,映射表

`@TableId(value="id",type=IdType.AUTO)`//在主键上加

`@TableField("表字段")`//指定表字段''

### 分布式事务解决

#### XA协议两阶段提交

> XA规范由应用程序,资源管理器,事务管理器
>
> > 应用程序AP:定义事务边界（定义事务开始和结束
> >
> > 资源管理器RM:管理计算机共享的资源，许多软件都可以去访问这些资源,数据库,文件系统
> >
> > 事务管理器TM:负责管理全局事务，分配事务唯一标识，监控事务的执行进度，并负责事务的提交、回滚、失败恢复等

第一阶段 准备阶段 TM询问RM是否有能力保证成功的提交事务分支

第二阶段 确认阶段 TM根据RM第一阶段的准备结果,决定提交还是回滚

评价:保证强一致性同时,实现复杂,牺牲可用性,不适合高并发场景

#### TCC补偿

针对每个操作，都要注册一个与其对应的确认和补偿（撤销）操作。它分为三个阶段

Try 阶段主要是对业务系统做检测及资源预留

Confirm 阶段主要是对业务系统做确认提交

Cancel 阶段主要是在业务执行错误，需要回滚的状态下执行的业务取消

评价:可用性较强,但补偿机制需要写补偿代码

#### 消息最终一致性

核心思想是将分布式事务拆分成本地事务进行处理

消息生产方，需要额外建一个消息表，并记录消息发送状态。消息表和业务数据要在一个事务里提交，也就是说他们要在一个数据库里面。然后消息会经过MQ发送到消息的消费方。如果消息发送失败，会进行重试发送。消息消费方，需要处理这个消息，并完成自己的业务逻辑。此时如果本地事务处理成功，表明已经处理成功了，如果处理失败，那么就会重试执行。如果是业务上面的失败，可以给生产方发送一个业务补偿消息，通知生产方进行回滚等操作。生产方和消费方定时扫描本地消息表，把还没处理完成的消息或者失败的消息再发送一遍。

评价:一种非常经典的实现，避免了分布式事务，实现了最终一致性消息表会耦合到业务系统中

**项目中使用Seata  的AT模式  来解决分布式事务的 ** seata在容器中有

>AT模式前提
>
>- 基于支持本地 ACID 事务的关系型数据库。
>
>- Java 应用，通过 JDBC 访问数据库。
>
>AT模式基于两阶段提交协议的演变。
>
>第一阶段  业务数据和回滚日志记录在同一个本地事务中提交，释放本地锁和连接资源。
>
>第二阶段  提交异步化，非常快速地完成。回滚通过一阶段的回滚日志进行反向补偿。

在每一个库中都有一个日志表-->在每个微服务中导入依赖-->在微服务添加yml配置-->在resource目录下添加了file.conf 改了一下微服务名字 --> 在微服务调用方法上加上@GlobalTransactional,-->测试 1/0 -->如果有错误数据会回滚

### 分布式锁

#### redis 分布式锁

##### 　　redis 最普通的分布式锁

　　第一个最普通的实现方式，就是在 redis 里创建一个 key，这样就算加锁。

```
SET my:lock 随机值 NX PX 30000
```

　　执行这个命令就 ok。

- `NX`：表示只有 `key` 不存在的时候才会设置成功。（如果此时 redis 中存在这个 key，那么设置失败，返回 `nil`）
- `PX 30000`：意思是 30s 后锁自动释放。别人创建的时候如果发现已经有了就不能加锁了。

　　释放锁就是删除 key ，但是一般可以用 `lua` 脚本删除，判断 value 一样才删除：

```
-- 删除锁的时候，找到 key 对应的 value，跟自己传过去的 value 做比较，如果是一样的才删除。
if redis.call("get",KEYS[1]) == ARGV[1] then
    return redis.call("del",KEYS[1])
else
    return 0
end
```

　　为啥要用随机值呢？因为如果某个客户端获取到了锁，但是阻塞了很长时间才执行完，比如说超过了 30s，此时可能已经自动释放锁了，此时可能别的客户端已经获取到了这个锁，要是你这个时候直接删除 key 的话会有问题，所以得用随机值加上面的 `lua` 脚本来释放锁。

但是这样是肯定不行的。因为如果是普通的 redis 单实例，那就是单点故障。或者是 redis 普通主从，那 redis 主从异步复制，如果主节点挂了（key 就没有了），key 还没同步到从节点，此时从节点切换为主节点，别人就可以 set key，从而拿到锁。

##### RedLock 算法

这个场景是假设有一个 redis cluster，有 5 个 redis master 实例。然后执行如下步骤获取一把锁：

1. 获取当前时间戳，单位是毫秒；
2. 跟上面类似，轮流尝试在每个 master 节点上创建锁，过期时间较短，一般就几十毫秒；
3. 尝试在大多数节点上建立一个锁，比如 5 个节点就要求是 3 个节点 `n / 2 + 1`；
4. 客户端计算建立好锁的时间，如果建立锁的时间小于超时时间，就算建立成功了；
5. 要是锁建立失败了，那么就依次之前建立过的锁删除；
6. 只要别人建立了一把分布式锁，你就得不断轮询去尝试获取锁。

####  zk 分布式锁

zk 分布式锁，其实可以做的比较简单，就是某个节点尝试创建临时 znode，此时创建成功了就获取了这个锁；这个时候别的客户端来创建锁会失败，只能注册个监听器监听这个锁。释放锁就是删除这个 znode，一旦释放掉就会通知客户端，然后有一个等待着的客户端就可以再次重新加锁。



- PO：持久对象(persistent object)，就是在Object/Relation Mapping框架中的Entity，PO的每个属性基本上都对应数据库表里面的某个字段。完全是一个符合JavaBean规范的纯Java对象
- VO：值对象(Value Object)，通常用于业务层之间的数据传递，是抽象出的业务对象，可以和表对应，也可以不对应，这根据业务时的需要。表现层对象(View Object)，主要对应展示界面显示的数据对象，用一个VO对象来**封装整个界面展示所需**的对象数据。
- DTO：数据传输对象(Data Transfer Object)，从数据库中检索PO，但是**不需要把整个PO对象全部字段传输到客户端**，可以用**DTO重新封装**，传递到客户端。
- POJO：POJO(Plain Ordinary Java Object)简单的Java对象，实际就是普通JavaBean
- DAO：数据访问对象（Data Access Object）,是第一个面向对象的数据库接口，是一个数据访问接口(Data Access Object)。它可以把POJO持久化为PO，用PO组装出来VO、DTO。







### 其他知识

#### 堡垒机

堡垒机是通过切断终端对计算机网络和服务器资源的直接访问，采用协议代理的方式接管终端计算机对网络和服务器的访问

#### 跳板机

跳板机就是一台服务器，运维人员在维护过程中首先要统一登录到这台服务器，然后再登录到目标设备进行维护和操作；在腾讯，跳板机是开发者登录到服务器的唯一途径，开发者必须先登录跳板机，再通过跳板机登录到应用服务器。

sdkman 用来管理jdk

@EqualsAndHashCode(callSuper = true) //不仅判断子类的字段,还判断父类的字段







双亲委派模型?

某一个类加载器在接到加载类的请求时，首先将加载任务委托给父类加载器，依次递归，如果父类加载器可以完成类加载任务，则成功返回；如果父类加载器无法完成加载任务，将抛出ClassNotFoundException异常后，再调用自己的findClass()方法进行加载，依次类推。

好处是防止内存中出现多份同样的字节码,







## Gof 23  23中设计模式

### 创建型模式(用来new对象的)

#### 简单工厂模式

> 工厂模式 实例化对象不使用new，用工厂方法代替。将调用者跟实现类解耦
>
> 应用场景:Spring的IOC容器创建管理bean对象；反射中Class对象的newInstance方法

> ```java
> //有个Car类,每个车继承了Car类
> //需要车的时候,需要new 
> Car wuling = new WuLing();
> ```

 ```java
//这样不满足面向对象的开闭原则,如果需要再来一辆车,就需要改代码,这时用工厂方法模式
public class CarFactory{
    public  Car getCar(String car){//直接调用工厂的getCar方法,去创建对象
        if (car.equals("五菱宏光")){
            return new WuLing();
        }else if(car.equals("特斯拉")){
            return new Tesla();
        }else{
            return null;
        }
    }
 ```

#### 工厂方法模式

 **工厂方法模式 ** 和简单工厂相比，不用修改工厂方法，需要什么车直接创建该车的工厂实现车工厂接口。 满足面向对象**开闭原则**

 > 弊端:代码量太多了

 ```java
//将车工厂提取出来,变为一个接口,每个车都有自己的工厂,想要什么车直接添加该车工厂
public interface CarFactory{
    Car getCar();
}
 ```

 ```java
public WulingFactory implements CarFactory{
    //重写getCar方法
    @Override
    public Car getCar(){
        return new WuLing();
    }
}
 ```

 

#### 抽象工厂模式

> //不符合开闭原则(对扩展开放,对修改关闭),如果改动一处后稳定使用,就使用抽象工厂模式,如果需要经常改动就不利了;因为这时不仅要改大工厂接口,重写的方法等等都需要改
>
> 将抽象的接口再抽取为一个大工厂接口,让不同的工厂类去实现该大的工厂,并重写里面的方法
>
> ```java
> public interface BigFactory(){
>  PhoneProduct makePhone();//生产手机的接口
>  TvProduct makeTv();//生产电视的接口
> }
> ```
>
> ```java
> //例: 让苹果的工厂去实现大的抽象工厂,重写里面的制造手机和制造电视的方法
> public class AppleFactory implements BigFactory {
>  @Override
>  public PhoneProduct makePhone() {
>      return new ApplePhone();
>  }
> 
>  @Override
>  public TvProduct makeTV() {
>      return new AppleTv();
>  }
> }
> ```
>
> ![image-20211225235636817](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211225235636817.png)

#### 建造者模式

提供了一种创建对象的最佳方式,只要给指定复杂对象的类型和内容,建造者模式负责按顺序创建复杂对象

> 地基,钢筋,电线,粉刷
>
> 按照这个过程不仅可以造平房,也能造高楼

```java
//抽象的建造者,要有建造者就要有工人具体实施
public abstract class Build {

    //建造者需要完成以下几个步骤

    abstract Build buildA(String msg);//汉堡
    abstract Build buildB(String msg);//薯条
    abstract Build buildC(String msg);//可乐
    abstract Build buildD(String msg);//炸鸡

    //建造者还要抽象一个具体产品
    abstract Product getProduct();
}
```

```java
public class Worker extends Build {

    //具体的人去建造产品
    private Product product;

    public Worker() {
        product = new Product();
    }
    
    @Override
    Build buildA(String msg) {
        product.setBuildA(msg);
        return this;
    }
    //...
    @Override
    Product getProduct() {
        return product;
    }
    
}
```

#### 原型模式

需要一个和已有对象相似的对象，可以不用new，直接对该类实现Cloneable接口，重写clone方法，使用时直接调用clone 方法就可以

实现一个Cloneable接口,并重写其中的clone方法(这时进行的是**浅克隆**(如果对象两个属性,改变对象1某属性,克隆对象2的某属性也会跟着改变))；要进行**深克隆**,对对象的各个属性也进行改变

在Spring制造Bean时，用到工厂模式+原型模式（生产可乐时,有一个工厂,用初的第一瓶可乐配方"克隆"无数瓶可乐）

#### 单例模式

保证一个类仅有一个实例，并提供一个访问它的全局访问点  

保证内存中只有一个实例,减少内存开销,避免对资源的多重占用

但违背开闭原则

> 场景:某个类只要求生成一个对象的时候,如每个人饿身份证号等
>
> 某些类实例化占用资源较多,实例化耗时较长,且经常使用
>
> 多线程的线程池,网络连接池等

```java
//饿汉式,一上来就初始化,线程安全
public class Singleton{
    //private static唯一实例，保证全局唯一性
    private static final Singleton instance = new Singleton();
    // private构造方法保证外部无法实例化:
    private Singleton(){}
    public static Singleton getInstance(){
        return instance;
    }
}
```

```java
//懒汉式,延迟初始化,非线程安全
public class Singleton{
    private static final Singleton instance;
    private Singleton(){}
    public static Singleton getInstance(){
        if(instance == null){
            instance = new instance;
        }
        return instance;
    }
}
```

```java
//双检索,解决一开始创建对象的问题
public class Singleton{
    private volatile static Singleton instacne;
    private Singleton(){}
    public static Singleton getInstance(){
        if(instance == null){
            synchronized(Singleton.class){
                instance = new instance;
            }
        }
        return instance;
    }
}
```

那我们什么时候应该用Singleton呢？很多程序，尤其是Web程序，大部分服务类都应该被视作Singleton，如果全部按Singleton的写法写，会非常麻烦，所以，通常是通过约定让框架（例如Spring）来实例化这些类，保证只有一个实例，调用方自觉通过框架获取实例而不是`new`操作符：

```java
@Component // 表示一个单例组件
public class MyService {
    ...
}
```

### 结构型模式(从程序结构上实现松耦合,扩大整体的类结构,解决更大的问题)

#### 适配器模式

> 场景:字节流转字符流
>
> 

想象成电脑的网线接口适配器,将一个类的接口转换成希望的另外一个接口

//网线类里有上网方法.net()  接口Adapter是一个适配器,重写他的适配方法,这个适配器可用来上网,在用**组合方式**将网线接入进来,调用网线的net方法,这时适配器插入网线就有网络了

这里也叫**对象适配器**

 ```java
public class AdapterImpl implements Adapter {

    private EthernetCable ethernetCable; //这里的网线是接入进来的

    public AdapterImpl(EthernetCable ethernetCable) {
        this.ethernetCable = ethernetCable;
    }

    @Override
    public void adapter() {
        ethernetCable.net();
    }
    
}

//第一种,继承方式,单继承  类适配器
/*public class AdapterImpl extends EthernetCable implements Adapter {

    @Override
    public void adapter() {
        super.net();
    }
}*/
 ```

```java
public class Computer {

    //连接上网络的方法(传入适配器接口)
    public void net(Adapter adapter){
        adapter.adapter();
    }

    public static void main(String[] args) {
        Computer computer = new Computer();//电脑
        EthernetCable ethernetCable = new EthernetCable();//网线
        AdapterImpl adapter = new AdapterImpl(ethernetCable);//适配器
        computer.net(adapter);//执行上网方法
    }
}
```

​    

桥接模式

![image-20211226222021817](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211226222021817.png)

组合

装饰器

外观

享元

代理

### 行为型模式

责任链

命令

解释器

迭代器

中介

备忘录

#### 观察者

定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新

一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作



状态

策略

#### 模板方法

定义一个操作的一系列步骤，对于某些暂时确定不下来的步骤留给子类实现

访问者







## 面向对象程序设计OOP 七大原则

开闭原则:对扩展开放,对修改关闭

里氏替换原则:继承必须确保类所拥有的性质在子类中仍然成立(不要破坏继承关系)

依赖倒置原则:面向接口编程,不要面向实现编程

单一职责原则:控制类的粒度大小,将对象解耦,提高其内聚性(一个方法尽量值做一件事)

接口隔离原则:给各个类建立他们需要的专用接口(设计接口时保证其精简和单一)

迪米特法则:只和你的直接朋友交谈,不和陌生人说话(降低程序间的耦合度)

合成复原原则:尽量使用**组合或者聚合**等关联关系来实现,其次才考虑使用继承关系来实现



## 类加载的过程

过程:

读取class文件,把他转化为某种静态数据结构,存在方法区(1.8后元数据区存.class),在**堆**中生成一个便于用户调用的Class类型的对象

验证:

对class静态结构进行语法和语义上的分析,保证其不会

![image-20211228150542790](https://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20211228150542790.png)









Java 集合框架；

Java 多线程；

线程的五种状态；

Java 虚拟机；

MySQL （InnoDB）；

Spring 相关；

计算机网络；

MQ 消息队列
