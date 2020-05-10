# Spring 框架

## 01、spring框架的介绍

### （1）、什么是spring

​		Spring是一个开源框架，Spring是于2003 年兴起的一个轻量级的Java 开发框架，由Rod Johnson 在其著作Expert One-On-One J2EE Development and Design中阐述的部分理念和原型衍生而来。它是为了解决企业应用开发的复杂性而创建的。框架的主要优势之一就是其分层架构，分层架构允许使用者选择使用哪一个组件，同时为 J2EE 应用程序开发提供集成的框架。Spring使用基本的JavaBean来完成以前只可能由EJB完成的事情。然而，Spring的用途不仅限于服务器端的开发。从简单性、可测试性和松耦合的角度而言，任何Java应用都可以从Spring中受益。Spring的核心是**控制反转（IoC）**和**面向切面（AOP）**。简单来说，Spring是一个分层的JavaSE/EE full-stack(一站式) 轻量级开源框架。

### （2）、spring的优点

**方便解耦，简化开发 （高内聚低耦合）** 
Spring就是一个大工厂（容器），可以将所有对象创建和依赖关系维护，交给Spring管理 
spring工厂是用于生成bean
**AOP编程的支持** 
Spring提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能
**声明式事务的支持** 
只需要通过配置就可以完成对事务的管理，而无需手动编程
**方便程序的测试** 
Spring对Junit4支持，可以通过注解方便的测试Spring程序
**方便集成各种优秀框架** 
Spring不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如：Struts、Hibernate、MyBatis、Quartz等）的直接支持
**降低JavaEE API的使用难度** 
Spring 对JavaEE开发中非常难用的一些API（JDBC、JavaMail、远程调用等），都提供了封装，使这些API应用难度大大降低

### （3）、spring的体系结构

![](G:\13、md学习文件\09、spring\images\spring的组织结构.png)

## 02、控制反转案例（ioc）

### （1）、所需包

4 + 1 ： 4个核心（beans、core、context、expression） + 1个依赖（commons-loggins…jar）

![](G:\13、md学习文件\09、spring\images\ioc-jar包.png)

### （2）、目标类

提供UserService接口和实现类
获得UserService实现类的实例 
之前开发中，直接new一个对象即可。学习spring之后，将由Spring创建对象实例–> IoC 控制反转（Inverse of Control） 
之后需要实例对象时，从spring工厂（容器）中获得，需要将实现类的全限定名称配置到xml文件中

```java
public interface UserService {
    public void addUser();
}
public class UserServiceImpl implements UserService {
    @Override
    public void addUser() {
        System.out.println("a_ico add user");
    }
}
```

### （3）、 配置文件

位置：任意，开发中一般在classpath下（src）
名称：任意，开发中常用applicationContext.xml
内容：添加schema约束 
约束文件位置：spring-framework-3.2.0.RELEASE\docs\spring-framework-reference\html\ xsd-config.html

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 配置service 
        <bean> 配置需要创建的对象
            id ：用于之后从spring容器获得实例时使用的
            class ：需要创建实例的全限定类名
    -->
    <bean id="userServiceId" class="com.itheima.a_ioc.UserServiceImpl"></bean>
</beans>
```

### （4）、测试

```java
@Test
    public void demo02(){
        //从spring容器获得
        //1 获得容器
        String xmlPath = "com/itheima/a_ioc/beans.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        //2获得内容 --不需要自己new，都是从spring容器获得
        UserService userService = (UserService) applicationContext.getBean("userServiceId");
        userService.addUser(); 
      }
```

## 03、DI

```java
class BookServiceImpl{
    //之前开发：接口 = 实现类  （service和dao耦合）
    //private BookDao bookDao = new BookDaoImpl();
    //spring之后 （解耦：service实现类使用dao接口，不知道具体的实现类）
    private BookDao bookDao;
    setter方法
}
模拟spring执行过程
创建service实例：BookService bookService = new BookServiceImpl()     -->IoC  <bean>
创建dao实例：BookDao bookDao = new BookDaoImple()                -->IoC
将dao设置给service：bookService.setBookDao(bookDao);             -->DI   <property>

```

（1）目标类

- 创建BookService接口和实现类
- 创建BookDao接口和实现类
- 将dao和service配置 xml文件
- 使用api测试

（2）Dao

```java
public interface BookDao {
    public void save();
}
public class BookDaoImpl implements BookDao {
    @Override
    public void save() {
        System.out.println("di  add book");
    }
}
```

（3）Service

```java
public interface BookService {

    public abstract void addBook();

}
public class BookServiceImpl implements BookService {

    // 方式1：之前，接口=实现类
//  private BookDao bookDao = new BookDaoImpl();
    // 方式2：接口 + setter
    private BookDao bookDao;
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    @Override
    public void addBook(){
        this.bookDao.save();
    }

}
```







