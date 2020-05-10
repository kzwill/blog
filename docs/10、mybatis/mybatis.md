# Mybatis学习

### 01、mybatis是什么？

- Mybatis是一个半ORM（对象关系映射）框架，它内部封装了JDBC，开发时只需要关注SQL语句本身，不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。程序员直接编写原生态sql，可以严格控制sql执行性能，灵活度高(支持动态SQL语句)。

- MyBatis 使用 XML 或注解来配置和映射原生信息，将 POJO映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

- 通过xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过java对象和 statement中sql的动态参数进行映射生成最终执行的sql语句，最后由Mybatis框架执行sql并将结果映射为Java对象并返回。（从执行sql到返回result的过程）

**Mybatis相对于直接使用JDBC，具有简单易上手，开发速度快，面向对象和数据库可移植的优势。**

我们知道，Hibernate是一种完全面向对象，并且对象和数据库是一种全表映射关系，使用了HQL语句来避免了原生的MySQL语句的编写。但是Hibernate存在无法灵活使用原生的SQL语句，全表映射效率低下，并且存在N+1的问题。MyBatis框架对对象关系映射做出了改进，并且支持动态SQL，可以更加灵活的对SQL语句进行优化，大大提高了其执行效率，逐渐称为了持久层框架的主流选择。

### 02、mybatis的核心组件

- **SqlSessionFactoryBuilder：**
         SqlSessionFactoryBuilder的作用是一个构建器，通过XML配置文件或者Java编码获得资源来构建SqlSessionFactory，通过Builder可以构建多个SessionFactory。其生命周期一般只存在于方法的局部，用完即可回收。一般通过如下语句建立：

```java
SqlSessionFactory  factory  = SqlSessionFactoryBuilder.build(input stream); 
```

- **SqlSessionFactory：**
          SqlSessionFactory的作用就是创建SqlSession，也就是创建一个会话。每次程序需要访问数据库，就需要使用到SqlSession。所以SqlSessionFactory应该在Mybatis应用的整个生命周期中。为了减少每次都创建一个会话带来的资源消耗，一般情况下都会使用单例模式来创建SqlSession。创建方式如下：

```
SqlSession sqlSession = SqlSessionFactory.openSession(); 
```

- **SqlSession：**
         SqlSession就是一个会话，相当于JDBC中的Connection对象，既可以发送SQL去执行并返回结果，也可以获取Mapper接口。SqlSession是一个享存不安全的对象，其生命周期应该是请求数据库处理事务的过程中。每次创建的SqlSession对象必须及时关闭，或者会使得数据库连接池的活动资源减少，影响系统性能。 

- **Mapper：**
          Mapper也叫做映射器，由Java接口和XML文件（或者是注解）共同组成，给出了对应的SQL和映射规则，主要负责发送SQL去执行，并且返回结果。通过下边的语句来获取Mapper，接下来就可以调用Mapper中的各个方法去获取数据库结果了。

```
XXMapper xxMapper = sqlSession.getMapper(XXMapper.class);
```

​       由于MyBatis是一个持久层框架，所以经常和Spring MVC等Web层框架搭配使用。但是，其实MyBatis其实是可以独立使用的，我们在一个普通的非Web类型的Java程序中即可使用，因为说白了，MyBatis就是对JDBC进行了一个封装而已。 

### 03、mybatis的运行原理

- **创建SqlSessionFactory的过程**

1. 通过XMLConfigBuilder解析配置的XML文件，读出配置参数，封装到Configuration类中。


2. 使用Configuration对象去创建SqlSessionFactory，SqlSessionFactory在MyBatis中是一个接口而不是一个类，所以提供了一个其默认实现类DefaultSqlSessionFactory

3. 创建SqlSessionFactory的本质是一种Builder模式。

4. Configuration类的作用：

   读入配置文件。

   初始化基础配置：

   ​	包括全局配置，别名，类型处理器，环境数据库标识以及Mapper映射器等的初始化配置。

   提供单例。

   执行一些重要的对象方法和初始化配置信息。

### 04、映射器的工作原理

映射器Mapper文件的内部组成：
 一个映射器Mapper文件一般由以下的的三个部分组成：

**MapperStatement：（MapperStatement）**
保存映射器的一个节点（select/insert/delete/ipdate）
**SqlSource：**
是提供BoundSql对象的地方，是MappedStatement的一个属性。
**BoundSql：**
	建立SQL和参数的地方，有3个属性，即SQL，parameterObject，parameterMappings 
  	  SQL属性就是我们映射器中的一条SQL语句。
  	  后边两个属性是对传入参数进行规范与约束的。
        我们知道每一个Dao接口即是Mapper接口，接口的全限定名和xml映射文件中的namespace(命名空间) 唯一对应。接口中的方法名就是映射文件中Mapper的Statement的id属性值。接口方法中的参数就是传递给映射文件中SQL的参数。

在Mybatis中，每一个<select>、<insert>、<update>、<delete>标签，都会被解析为一个MapperStatement对象。

       Mapper接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为key值，可唯一定位一个MapperStatement。

举例：com.mapper.UserMapper.getUserInfo，可以唯一找到namespace为com.mapper.UserMapper下面 id 为 getUserInfo的 MapperStatement。 

Mapper 接口的工作原理是JDK动态代理：
Mybatis运行时会使用JDK动态代理为Mapper接口生成代理对象proxy，代理对象会拦截接口方法，转而执行MapperStatement所代表的sql，然后将sql执行结果返回。

Dao接口中的方法重载？
那么问题来了，当Dao接口中的方法，参数不同时，可以构成重载吗？我们可以先来温习下何为重载？

何为重载。

重载是指一个类里面（包括父类的方法）存在方法名相同，但是参数不一样的方法，参数不一样可以是不同的参数个数、类型或顺序
如果仅仅是修饰符、返回值、throw的异常 不同，那么这是2个相同的方法
了解了映射器的工作原理之后，我们应该可以得出下边的结论：

 Mapper接口里的方法，是不能重载的，因为是使用 全限名+方法名 的保存和寻找策略。

我们继续来看下边的的问题。

不同的映射文件xml中的id值可以重复吗？
前面我们说了，MyBatis使用全限名+方法名来寻找对应的MapperStatement，那么看起来我们是不可以定义相同的id值的。

不同的Xml映射文件，如果配置了namespace，那么id可以重复；
如果没有配置namespace，那么id不能重复；
原因：

namespace+id是作为Map<String, MapperStatement>的key使用的，如果没有namespace，就剩下id，那么，id重复会导致数据互相覆盖。有了namespace，自然id就可以重复，namespace不同，namespace+id自然也就不同。 
Mapper中常见的标签：
select | insert | updae | delete

<resultMap>、<parameterMap>、<sql>、<include>、<selectKey>

trim | where | set | foreach | if | choose | when | otherwise | bind   （动态SQL语句相关标签）

我们来看下边这两个和标签相关的问题。

resultMap和resultType的差别：

两者都是表示查询结果集与java对象之间的一种关系，处理查询结果集，映射到java对象。
resultMap：
表示将查询结果集中的列一一映射到bean对象的各个属性。
resutType:
表示的是bean中的对象类，此时可以省略掉resultMap标签的映射，但是必须保证查询结果集中的属性 和 bean对象类中的属性是一一对应的，此时大小写不敏感，但是有限制。
parameterMap和parameterType的差别：

parameterMap：
与resultMap方法类似，表示将查询结果集中列值的类型一一映射到java对象属性的类型上，在开发过程中不推荐这种方式。 
parameterType： 
parameterType直接将查询结果列值类型自动对应到java对象属性类型上，不再配置映射关系一一对应。 
Mapper文件中的反射技术：
Mybatis是如何将sql执行结果封装为目标对象并返回的？都有哪些映射形式？

第一种是使用<resultMap>标签，逐一定义数据库列名和对象属性名之间的映射关系。
第二种是使用sql列的别名功能，将列的别名书写为对象属性名。
有了列名与属性名的映射关系后，Mybatis通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。
MyBatis中SqlSession的运行原理总结：
接下来，我们对SqlSession的运行原理做一个简单的总结。

SqlSession的运行过程解析：
映射器的动态代理：
动态代理对接口的绑定，它的作用就是生成动态代理对象（占位），而代理的方法则被放入了MapperProxy类中。
为什么MyBatis中只用Mapper接口就可以运行SQL？
映射器的XML文件的命名空间对应的便是这个接口的全路径，根据全路径和方法名便能够绑定起来，通过动态代理技术，让这个接口跑起来。
然后采用命令模式，最后使用SqlSession接口的方法使得它能够执行查询。 

### 05、总结

> 映射器其实就是一个动态代理对象，进入到了MapperMethod的execute方法，然后就进入了SqlSession的删除，更新，插入，选择等方法。

SqlSession下的四大对象：
Mapper执行的过程是通过Executor，StatementHandler，ParameterHandler和ResultHandler来完成数据库操作和结果返回的。
Executor：执行器，由执行器来调度三个Handler来执行对应的SQL语句。
三种执行器：SIMPLE；REUSE；BATCH
StatementHandler：
数据库会话器，专门处理数据库会话的。
是四大对象的核心，作用是使用数据库的Statement（PreparedStatement）执行操作。
对应执行器，也有三种数据库会话器。
ParameterHandler：参数处理器，用于SQL对参数的处理，对预编译语句进行参数设置的。
ResultHandler：结果处理器，进行最后数据集（ResultSet）的封装返回处理的。
SqlSession运行过程总结：
动态代理创建代理对象，使得这个接口跑起来，开始调用SqlSession中的方法。
Executor会先调用StatementHandler的prepare()方法预编译SQL语句，同时设置一些基本运行的参数。
然后parameterrize()方法启动ParameterHandler设置参数，完成预编译，接着就是执行查询。
最后通过ResultSetHandler封装结果返回给调用者。
Other Question：
**Mybatis中 # 和 $ 的区别**：（能用#号就不要用$符号）
	#符号将传入的数据都当做一个字符串，会对自动传入的数据加一个双引号
	$符号将传入的数据直接显示生成SQL中
	#符号存在预编译的过程，对问号赋值，防止SQL注入
	$符号是直译的方式，一般用在order by ${列名}语句中

#### **Mybatis的一级、二级缓存：**

**一级缓存（同一个SqlSession）:**
	基于 PerpetualCache 的 HashMap 本地缓存，其存储作用域为 Session，当 Session flush 或 close 之后，该 Session 中的所有 Cache 就将清空，默认打开一级缓存。
**二级缓存（同一个SqlSessionFactory）：**
二级缓存与一级缓存其机制相同，默认也是采用 PerpetualCache，HashMap 存储，不同在于其存储作用域为 Mapper(Namespace)，并且可自定义存储源，如 Ehcache。
默认不打开二级缓存，要开启二级缓存，使用二级缓存属性类需要实现Serializable序列化接口(可用来保存对象的状态)，可在它的映射文件中配置<cache/>。 对于缓存数据更新机制，当某一个作用域(一级缓存 Session/二级缓存Namespaces)的进行了C/U/D 操作后，默认该作用域下所有 select 中的缓存将被 clear。

MyBatis的接口绑定？有哪些实现方式？
接口绑定：

就是在MyBatis中任意定义接口,然后把接口里面的方法和SQL语句绑定, 我们直接调用接口方法就可以,这样比起原来了SqlSession提供的方法我们可以有更加灵活的选择和设置。

接口绑定有两种实现方式：

通过注解绑定，就是在接口的方法上面加上 @Select、@Update等注解，里面包含Sql语句来绑定
通过xml里面写SQL来绑定, 在这种情况下,要指定xml映射文件里面的namespace必须为接口的全路径名。
接口绑定方式的选择：

当Sql语句比较简单时候,用注解绑定, 当SQL语句比较复杂时候,用xml绑定,一般用xml绑定的比较多。 