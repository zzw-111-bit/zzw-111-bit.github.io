

![在Spring中使用多个缓存管理器- 知乎](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/v2-c7d94fabd9e1251acb3bba73458e3813_720w.jpg)

# Spring5

## 1.、Spring

### 1.1、简介

+ Spring ——> 春天，为开源软件带来了春天
+ 2002，首次推出了Spring框架的雏形：interface21框架！
+ Spring框架以interface21框架为基础，经过重新设计，并不断丰富其内涵，于2004年3月24日发布了1.0正式版
+ Spring的理念：使用现有的技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架！



+ SSH：Struct2 + Spring + Hibernate（全自动持久化框架）！
+ SSM：SpringMVC + Spring + MyBatis（半自动持久化框架，可自定义性质更强）！



spring官网： https://spring.io/projects/spring-framework#overview

官方下载： https://repo.spring.io/release/org/springframework/spring/

GitHub： https://github.com/spring-projects/spring-framework

Spring Web MVC： [spring-webmvc最新版](https://mvnrepository.com/artifact/org.springframework/spring-webmvc/5.2.7.RELEASE)



Spring Web MVC和Spring-JDBC的pom配置文件：

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.7.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.7.RELEASE</version>
</dependency>
```



### 1.2 优点

+ Spring是一个开源的免费的框架（容器）！
+ Spring是一个轻量级的、非入侵式的框架！
+ 控制反转（IoC），面向切面编程（AOP）
+ 支持事务的处理，对框架整合的支持！（几乎市面上所有热门框架都能整合进去）！



=== 总结一句话：Spring就是一个轻量级的控制反转（IoC）和面向切面编程（AOP）的框架！ ===



### 1.3 组成

![img](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvU3ByaW5nNyVFNSVBNCVBNyVFNiVBOCVBMSVFNSU5RCU5Ny5wbmc)



### 1.4、扩展

现代化的java开发 -> 基于Spring的开发！

![img](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvMjAyMDA4MDEwMzA4MjAucG5n)

+ Spring Boot
  + 一个快速开发的脚手架
  + 基于SpringBoot可以快速开发单个微服务
  + 约定大于配置！
+ Spring Cloud
  + SpringCloud是基于SpringBoot实现的！

因为现在大多数公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提，需要完全掌握Spring及SpringMVC！承上启下的作用！

## 2、IoC（控制反转）理论推导

**传统**的调用

1. UserDao

   ```java
   package dao;
   public interface UserDao {
   	void getUser();
   }
   ```

2. UserDaoImp

   ```java
   package dao;
   public class UserDaoImpl implements UserDao{
   	public void getUser() {
   		System.out.println("默认获取用户数据");	
   	}
   }
   ```

3. UserSevice

   ```java
   package Service;
   public interface UserService {
   	void getUser();
   }
   ```

4. UserServiceImp

   ```java
   package Service;
   import dao.UserDao;
   import dao.UserDaoImpl;
   
   public class UserServiceImpl implements UserService{
   		UserDao userDao = new UserDaoImpl();		
   		public void getUser(){
   			userDao.getUser();
   		}	
   }
   ```

测试

```java
package holle0;
import Service.UserService;
import Service.UserServiceImpl;

public class MyTest0 {
	public static void main(String[] args) {
		// 用户实际调用的是业务层，dao层他们不需要接触
		UserService userService = new UserServiceImpl();
		userService.getUser();
	}
}
```

在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改原代码！如果程序代码量十分大，修改一次的成本代价十分昂贵！

![image-20200801122742581](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDExMjI3NDI1ODEucG5n)
**改良：**我们使用一个Set接口实现。已经发生了革命性的变化！

```java
//在Service层的实现类(UserServiceImpl)增加一个Set()方法
//利用set动态实现值的注入！
//DAO层并不写死固定调用哪一个UserDao的实现类
//而是通过Service层调用方法设置实现类！
private UserDao userDao;
public void setUserDao(UserDao userDao){
    this.userDao = userDao;
}
```

set() 方法实际上是动态改变了 UserDao userDao 的 初始化部分（**new UserDaoImpl()**）

测试中加上

```java
((UserServiceImpl)userService).setUserDao(new UserDaoImpl());
```

- 之前，程序是主动创建对象！**控制权在程序猿手上**！
- 使用了set注入后，程序不再具有主动性，而是变成了被动的接受对象！（**主动权在客户手上**）

本质上解决了问题，程序员不用再去管理对象的创建

系统的耦合性大大降低，可以更专注在业务的实现上

这是IoC（控制反转）的原型，反转(理解)：主动权交给了用户

![image-20200801122805769](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDExMjI4MDU3NjkucG5n)

### IoC本质

![image-20200801123518974](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDExMjM1MTg5NzQucG5n)
![img](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvMjAyMDA4MDExMjMyMzUucG5n)
![image-20200801123348207](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDExMjMzNDgyMDcucG5n)
![image-20200801123450897](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDExMjM0NTA4OTcucG5n)

## 3、HolleSpring

在父模块中导入jar包

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-webmvc</artifactId>
	<version>5.2.7.RELEASE</version>
</dependency>
```

pojo的Hello.java

```java
package pojo;

public class Hello {

	private String str;
	
	public String getStr() {
		return str;
	}

	public void setStr(String str) {
		this.str = str;
	}
	
	@Override
	public String toString() {
		return "Holle [str=" + str + "]";
	}
}
```

在resource里面的xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--在Spring中创建对象，在Spring这些都称为bean
    	类型 变量名 = new 类型();
    	Holle holle = new Holle();
    	
    	bean = 对象(holle)
    	id = 变量名(holle)
    	class = new的对象(new Holle();)
    	property 相当于给对象中的属性设值,让str="Spring"
    -->
    
    <bean id="hello" class="pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>
</beans>
```

测试类MyTest

```java
package holle1;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import pojo.Hello;

public class MyTest {

	public static void main(String[] args) {
		//获取Spring的上下文对象
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		//我们的对象下能在都在spring·中管理了，我们要使用，直接取出来就可以了
		Hello holle = (Hello) context.getBean("hello");
		System.out.println(holle.toString());
	}

}
```

核心用set注入，所以必须要有下面的se()方法

```java
//Hello类
public void setStr(String str) {
		this.str = str;
	}
```

**思考：**

![image-20200801165156259](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDExNjUxNTYyNTkucG5n)
IoC：对象由Spring 来创建，管理，装配！

**弹幕评论里面的理解：**

原来这套程序是：你写好菜单买好菜，客人来了自己把菜炒好招待，就相当于你请人吃饭
现在这套程序是：你告诉楼下餐厅，你要哪些菜，客人来的时候，餐厅把做好的你需要的菜送上来
IoC：炒菜这件事，不再由你自己来做，而是委托给了第三方__餐厅来做

此时的区别就是，如果我还需要做其他的菜，我不需要自己搞菜谱买材料再做好，而是告诉餐厅，我要什么菜，什么时候要，你做好送来

.

在前面第一个module试试引入Spring

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userDaomSql" class="dao.UserDaoMysqlImpl"></bean>

    <bean id="userServiceImpl" class="service.UserServiceImp">
        <!--ref引用spring中已经创建很好的对象-->
        <!--value是一个具体的值,基本数据类型-->
        <property name="userDao" ref="userDaomSql"/>
    </bean>

</beans>
```

第一个module改良后测试

```java
package holle0;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import service.UserServiceImpl;

public class MyTest0 {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		UserServiceImpl userServiceImpl = (UserServiceImpl) context.getBean("userServiceImpl");
		userServiceImpl.getUser();
	}
}
```

**总结：**

所有的类都要装配的beans.xml 里面；

所有的bean 都要通过容器去取；

容器里面取得的bean，拿出来就是一个对象，用对象调用方法即可；

## 4、IoC创建对象的方式

1. 使用无参构造创建对象，默认。
2. 使用有参构造（如下）

下标赋值

index指的是有参构造中参数的下标，下标从0开始;

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="pojo.User">
        <constructor-arg index="0" value="chen"/>
    </bean>
</beans>
```

类型赋值（不建议使用）

```xml
<bean id="user" class="pojo.User">
    <constructor-arg type="java.lang.String" value="kuang"/>
</bean>
```

直接通过参数名（掌握）

```xml
<bean id="user" class="pojo.User">
    <constructor-arg name="name" value="kuang"></constructor-arg>
</bean>
<!-- 比如参数名是name，则有name="具体值" -->
```

注册bean之后就对象的初始化了（**类似 new 类名()**）

弹幕评论：

name方式还需要无参构造和set方法,index和type只需要有参构造

就算是new 两个对象，也是只有一个实例（**单例模式：全局唯一**）

```java
User user = (User) context.getBean("user");
User user2 = (User) context.getBean("user");
system.out.println(user == user2)//结果为true
```

总结：在配置文件加载的时候，容器(< bean>)中管理的对象就已经初始化了

## 5、Spring配置

### 5.1、别名

```xml
<bean id="user" class="pojo.User">
    <constructor-arg name="name" value="chen"></constructor-arg>
</bean>

<alias name="user" alias="userLove"/>
<!-- 使用时
	User user2 = (User) context.getBean("userLove");	
-->
```

### 5.2、Bean的配置

```xml
<!--id：bean的唯一标识符，也就是相当于我们学的对象名
class：bean对象所对应的会限定名：包名+类型
name：也是别名，而且name可以同时取多个别名 -->
<bean id="user" class="pojo.User" name="u1 u2,u3;u4">
    <property name="name" value="chen"/>
</bean>
<!-- 使用时
	User user2 = (User) context.getBean("u1");	
-->
```

### 5.3、import

import一般用于团队开发使用，它可以将多个配置文件，导入合并为一个

假设，现在项目中有多个人开发，这三个人复制不同的类开发，不同的类需要注册在不同的bean中，我们可以利
用import将所有人的beans.xml合并为一个总的！

- 张三(beans.xm1)

- 李四(beans2.xm1)

- 王五(beans3.xm1)

- applicationContext.xml

  ```xml
  <import resource="beans.xm1"/>
  <import resource="beans2.xml"/>
  <import resource="beans3.xm1"/>
  ```

**使用的时候，直接使用总的配置就可以了**

弹幕评论：

按照在总的xml中的导入顺序来进行创建，后导入的会重写先导入的，最终实例化的对象会是后导入xml中的那个

## 6、依赖注入（DI）

### 6.1、构造器注入

第4点有提到

### 6.2、set方式注入【重点】

依赖注入：set注入！

- 依赖：bean对象的创建依赖于容器
- 注入：bean对象中的所有属性，由容器来注入

【环境搭建】

1. 复杂类型

   Address类

2. 真实测试对象

   Student类

3. beans.xml

4. 测试

   MyTest3

Student类

```java
package pojo;

import java.util.*;
@Get
@Set
public class Student {
//别忘了写get和set方法（用lombok注解也行）
    private String name;
    private Address address;

    private String[] books;
    private List<String> hobbies;

    private Map<String, String> card;
    private Set<String> game;

    private Properties infor;
    private String wife;

    @Override
    public String toString() {
        return "Student{" +"\n"+
                "name='" + name + '\'' +"\n"+
                ", address=" + address.toString() +"\n"+
                ", books=" + Arrays.toString(books) +"\n"+
                ", hobbies=" + hobbies +"\n"+
                ", card=" + card +"\n"+
                ", game=" + game +"\n"+
                ", infor=" + infor +"\n"+
                ", wife='" + wife + '\'' +"\n"+
                '}';
    }
}
```

Address类

```java
package pojo;

public class Address {

    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "Address{" +
                "address='" + address + '\'' +
                '}';
    }
}
```

beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="address" class="pojo.Address">
		<property name="address" value="address你好" />
	</bean>

	<bean id="student" class="pojo.Student">
		<!--第一种，普通值注入 -->
		<property name="name" value="name你好" />
		<!--第二种，ref注入 -->
		<property name="address" ref="address" />

		<!--数组注入 -->
		<property name="books">
			<array>
				<value>三国</value>
				<value>西游</value>
				<value>水浒</value>
			</array>
		</property>

		<!--list列表注入 -->
		<property name="hobbies">
			<list>
				<value>唱</value>
				<value>跳</value>
				<value>rap</value>
				<value>篮球</value>
			</list>
		</property>

		<!--map键值对注入 -->
		<property name="card">
			<map>
				<entry key="username" value="root" />
				<entry key="password" value="root" />
			</map>
		</property>

		<!--set(可去重)注入 -->
		<property name="game">
			<set>
				<value>wangzhe</value>
				<value>lol</value>
				<value>galname</value>
			</set>
		</property>

		<!--空指针null注入 -->
		<property name="wife">
			<null></null>
		</property>

		<!--properties常量注入 -->
		<property name="infor">
			<props>
				<prop key="id">20200802</prop>
				<prop key="name">cbh</prop>
			</props>
		</property>
	</bean>
</beans>
```

MyTest3

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import pojo.Student;

public class MyTest3 {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		Student stu = (Student) context.getBean("student");
		System.out.println(stu.toString());
	}	
}
```

### 6.3、拓展注入

官方文档位置

![image-20200802142717216](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIxNDI3MTcyMTYucG5n)

pojo增加User类

```java
package pojo;

public class User {
    private String name;
    private int id;
	public User() {
        
	}
	public User(String name, int id) {
		super();
		this.name = name;
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	@Override
	public String toString() {
		return "User [name=" + name + ", id=" + id + "]";
	}
}
```

注意： beans 里面加上这下面两行

使用p和c命名空间需要导入xml约束

xmlns:p=“http://www.springframework.org/schema/p”
xmlns:c=“http://www.springframework.org/schema/c”

```xml
?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间注入/set注入，可以直接注入属性的值-》property-->
    <bean id="user" class="pojo.User" p:name="cxk" p:id="20" >
    </bean>

    <!--c命名空间，通过构造器注入，需要写入有参和无参构造方法-》construct-args-->
    <bean id="user2" class="pojo.User" c:name="cbh" c:id="22"></bean>
</beans>
```

测试

```java
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
User user = context.getBean("user",User.class);//确定class对象，就不用再强转了
System.out.println(user.toString());
```

### 6.4、Bean作用域

![image-20200802143401165](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIxNDM0MDExNjUucG5n)

![image-20200802143342586](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIxNDMzNDI1ODYucG5n)

1. 单例模式（默认）

   ```xml
   <bean id="user2" class="pojo.User" c:name="cxk" c:age="19" scope="singleton"></bean>
   1
   ```

![image-20200802143802005](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIxNDM4MDIwMDUucG5n)
弹幕评论：单例模式是把对象放在pool中，需要再取出来，使用的都是同一个对象实例

1. 原型模式: 每次从容器中get的时候，都产生一个新对象！

   ```xml
   <bean id="user2" class="pojo.User" c:name="cxk" c:age="19" scope="prototype"></bean>
   1
   ```

![image-20200802143826227](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIxNDM4MjYyMjcucG5n)

1. 其余的request、session、application这些只能在web开放中使用！

## 7、Bean的自动装配

- 自动装配是Spring满足bean依赖的一种方式
- Spring会在上下文自动寻找，并自动给bean装配属性

在Spring中有三种装配的方式

1. 在xml中显示配置

2. 在java中显示配置

3. 隐式的自动装配bean 【重要】

4. 环境搭建：一个人有两个宠物

5. byType自动装配：byType会自动查找，和自己对象set方法参数的类型相同的bean

   保证所有的class唯一(类为全局唯一)

6. byName自动装配：byName会自动查找，和自己对象set对应的值对应的id

   保证所有id唯一，并且和set注入的值一致

   ```xml
   <!-- 找不到id和多个相同class -->
   <bean id="cat1" class="pojo.Cat"/>
   <bean id="cat2" class="pojo.Cat"/>
   <!-- 找不到 id=cat，且有两个Cat -->
   ```

### 7.1测试：自动装配

pojo的Cat类

```java
public class Cat {
    public void shut(){
        System.out.println("miao");
    }
}
```

pojo的Dog类

```java
public class Dog {

    public void shut(){
        System.out.println("wow");
    }

}
```

pojo的People类

```java
package pojo;
public class People {
    
    private Cat cat;
    private Dog dog;
    private String name;

    public Cat getCat() {
        return cat;
    }

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "People{" +
                "cat=" + cat +
                ", dog=" + dog +
                ", name='" + name + '\'' +
                '}';
    }
}
```

xml配置 -> byType 自动装配

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="cat" class="pojo.Cat"/>
    <bean id="dog" class="pojo.Dog"/>
    
    <!--byType会在容器自动查找，和自己对象属性相同的bean
		例如，Dog dog; 那么就会查找pojo的Dog类，再进行自动装配
	-->
    <bean id="people" class="pojo.People" autowire="byType">
        <property name="name" value="cbh"></property>
    </bean>

</beans>
```

xml配置 -> byName 自动装配

```xml
<bean id="cat" class="pojo.Cat"/>
<bean id="dog" class="pojo.Dog"/>
<!--byname会在容器自动查找，和自己对象set方法的set后面的值对应的id
  例如:setDog()，取set后面的字符作为id，则要id = dog 才可以进行自动装配
  
 -->
<bean id="people" class="pojo.People" autowire="byName">
	<property name="name" value="cbh"></property>
</bean>
```

弹幕评论：byName只能取到小写，大写取不到

### 7.2、使用注解实现自动装配

jdk1.5支持的注解，spring2.5支持的注解

The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML.（翻译：基于注释的配置的引入提出了一个问题，即这种方法是否比XML“更好”）

1. 导入context约束

**xmlns:context="http://www.springframework.org/schema/context"**

1. 配置注解的支持：< context:annotation-config/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
</beans>
```

#### 7.2.1、@Autowired

**默认是byType方式，如果匹配不上，就会byName**

在属性上个使用，也可以在set上使用

我们可以不用编写set方法了，前提是自动装配的属性在Spring容器里，且要符合ByName 自动装配

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```

> @Nullable 字段标记了这个注解，说明该字段可以为空
>
> public name(@Nullable String name){
>
> }

```java
//源码
public @interface Autowired { 
	boolean required() default true; 
}
```

如果定义了Autowire的require属性为false，说明这个对象可以为null，否则不允许为空（false表示找不到装配，不抛出异常）

#### 7.2.2、@Autowired+@Qualifier

**@Autowired不能唯一装配时，需要@Autowired+@Qualifier**

如果@Autowired自动装配环境比较复杂。自动装配无法通过一个注解完成的时候，可以使用@Qualifier(value = “dog”)去配合使用，指定一个唯一的id对象

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog")
    private Dog dog;
    private String name;
}
```

弹幕评论：

如果xml文件中同一个对象被多个bean使用，Autowired无法按类型找到，可以用@Qualifier指定id查找

#### 7.2.3、@Resource

**默认是byName方式，如果匹配不上，就会byType**

```java
public class People {
    Resource(name="cat")
    private Cat cat;
    Resource(name="dog")
    private Dog dog;
    private String name;
}
```

弹幕评论：

Autowired是byType，@Autowired+@Qualifier = byType || byName

Autowired是先byteType,如果唯一則注入，否则byName查找。resource是先byname,不符合再继续byType

#### 区别：

@Resource和@Autowired的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过byType的方式实现，而且必须要求这个对象存在！【常用】
- @Resource默认通过byname的方式实现，如果找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错！【常用】
- 执行顺序不同：@Autowired通过byType的方式实现。@Resource默认通过byname的方式实现

## 8、使用注解开发

在spring4之后，使用注解开发，必须要保证aop包的导入
![image-20200802201924490](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIyMDE5MjQ0OTAucG5n)
使用注解需要导入contex的约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

### 8.1、bean

弹幕评论：
有了< context:component-scan>，另一个< context:annotation-config/>标签可以移除掉，因为已经被包含进去了。

```xml
<!--指定要扫描的包，这个包下面的注解才会生效
	别只扫一个com.kuang.pojo包--> 
<context:component-scan base-package="com.kuang"/> 
<context:annotation-config/>
```

```java
//@Component 组件
//等价于<bean id="user" classs"pojo.User"/> 
@Component
public class User {  
     public String name ="秦疆";
}
```



### 8.2、属性如何注入@value

```java
@Component
public class User { 
    //相当于<property name="name" value="kuangshen"/> 
    @value("kuangshen") 
    public String name; 
    
    //也可以放在set方法上面
    //@value("kuangshen")
    public void setName(String name) { 
        this.name = name; 
    }
}
```

### 8.3、衍生的注解

@Component有几个衍生注解，会按照web开发中，mvc架构中分层。

- dao （@Repository）
- service（@Service）
- controller（@Controller）

**这四个注解的功能是一样的，都是代表将某个类注册到容器中**

### 8.4、自动装配置

@Autowired：默认是byType方式，如果匹配不上，就会byName

@Nullable：字段标记了这个注解，说明该字段可以为空

@Resource：默认是byName方式，如果匹配不上，就会byType

### 8.5、作用域@scope

```java
//原型模式prototype，单例模式singleton
//scope("prototype")相当于<bean scope="prototype"></bean>
@Component 
@scope("prototype")
public class User { 
    
    //相当于<property name="name" value="kuangshen"/> 
    @value("kuangshen") 
    public String name; 
    
    //也可以放在set方法上面
    @value("kuangshen")
    public void setName(String name) { 
        this.name = name; 
    }
}
```

### 8.6、小结

**xml与注解：**

- xml更加万能，维护简单，适用于任何场合
- 注解，不是自己的类使用不了，维护复杂

**最佳实践：**

- xml用来管理bean
- 注解只用来完成属性的注入
- 要开启注解支持

## 9、使用Java的方式配置Spring

不使用Spring的xml配置，完全交给java来做！

Spring的一个子项目，在spring4之后，，，它成为了核心功能

![image-20200802215752868](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIyMTU3NTI4NjgucG5n)
**实体类：pojo的User.java**

```java
//这里这个注解的意思,就是说明这个类被Spring接管了,注册到了容器中 
@component 
public class User { 
    private String name;
    
    public String getName() { 
    	return name; 
    } 
    //属性注入值
    @value("QINJIANG')  
    public void setName(String name) { 
    	this.name = name; 
    } 
    @Override 
    public String toString() { 
        return "user{" + 
        "name='" + name + '\''+ 
        '}'; 
    } 
}
```

弹幕评论：要么使用@Bean，要么使用@Component和ComponentScan，两种效果一样

**配置文件：config中的kuang.java**

@Import(KuangConfig2.class)，用@import来包含KuangConfig2.java

```java
//这个也会Spring容器托管,注册到容器中,因为他本米就是一个@Component 
// @Configuration表这是一个配置类,就像我们之前看的beans.xml，类似于<beans>标签
@Configuration 
@componentScan("com.Kuang.pojo") //开启扫描
//@Import(KuangConfig2.class)
public class KuangConfig { 
    //注册一个bean , 就相当于我们之前写的一个bean 标签 
    //这个方法的名字,就相当于bean 标签中的 id 属性 ->getUser
    //这个方法的返同值,就相当于bean 标签中的class 属性 ->User
    
    //@Bean 
    public User getUser(){ 
    	return new User(); //就是返回要注入到bean的对象! 
    } 
}
```

弹幕评论：ComponentScan、@Component("pojo”) 这两个注解配合使用

**测试类**

```java
public class MyTest { 
    public static void main(String[ ] args) { 
    //如果完全使用了配置类方式去做,我们就只能通过 Annotationconfig 上下文来获取容器,通过配置类的class对象加载! 
    ApplicationContext context = new AnnotationConfigApplicationContext(KuangConfig.Class); //class对象
    User getUser =(User)context.getBean( "getUser"); //方法名getUser
    System.out.Println(getUser.getName()); 
    } 
}
```

**会创建两个相同对象问题的说明：**

**弹幕总结 - -> @Bean是相当于< bean>标签创建的对象，而我们之前学的@Component是通过spring自动创建的这个被注解声明的对象，所以这里相当于有两个User对象被创建了。一个是bean标签创建的（@Bean），一个是通过扫描然后使用@Component，spring自动创建的User对象，所以这里去掉@Bean这些东西，然后开启扫描。之后在User头上用@Component即可达到spring自动创建User对象了**

```java
//这个也会Spring容器托管,注册到容器中,因为他本米就是一个@Component 
// @Configuration表这是一个配置类,就像我们之前看的beans.xml，类似于<beans>标签
@Configuration 
@componentScan("com.Kuang.pojo") //开启扫描
//@Import(KuangConfig2.class)
public class KuangConfig { 
    //注册一个bean , 就相当于我们之前写的一个bean 标签 
    //这个方法的名字,就相当于bean 标签中的 id 属性 ->getUser
    //这个方法的返同值,就相当于bean 标签中的class 属性 ->User
    
    //@Bean 
    public User getUser(){ 
    	return new User(); //就是返回要注入到bean的对象! 
    } 
}
```

弹幕评论：ComponentScan、@Component("pojo”) 这两个注解配合使用

**测试类**

```java
public class MyTest { 
    public static void main(String[ ] args) { 
    //如果完全使用了配置类方式去做,我们就只能通过 Annotationconfig 上下文来获取容器,通过配置类的class对象加载! 
    ApplicationContext context = new AnnotationConfigApplicationContext(KuangConfig.Class); //class对象
    User getUser =(User)context.getBean( "getUser"); //方法名getUser
    System.out.Println(getUser.getName()); 
    } 
}
```

**会创建两个相同对象问题的说明：**

**弹幕总结 - -> @Bean是相当于< bean>标签创建的对象，而我们之前学的@Component是通过spring自动创建的这个被注解声明的对象，所以这里相当于有两个User对象被创建了。一个是bean标签创建的（@Bean），一个是通过扫描然后使用@Component，spring自动创建的User对象，所以这里去掉@Bean这些东西，然后开启扫描。之后在User头上用@Component即可达到spring自动创建User对象了**

## 10、动态代理

代理模式是SpringAOP的底层

分类：动态代理和静态代理

![image-20200803101427846](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMDE0Mjc4NDYucG5n)

### 10.1、静态代理

![image-20200803101621868](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMDE2MjE4NjgucG5n)
代码步骤：

1、接口

```java
package pojo;
public interface Host {
	public void rent();
}
```

2、真实角色

```java
package pojo;
public class HostMaster implements Host{	
    
	public void rent() {
		System.out.println("房东要出租房子");
	}
}
```

3、代理角色

```java
package pojo;
public class Proxy {

	public Host host;
	
	public Proxy() {
		
	}
	
	public Proxy(Host host) {
		super();
		this.host = host;
	}
	
	public void rent() {
		seeHouse();
		host.rent();
		fee();
		sign();
	}
	//看房
	public void seeHouse() {
		System.out.println("看房子");
	}
	//收费
	public void fee() {
		System.out.println("收中介费");
	}
	//合同
	public void sign() {
		System.out.println("签合同");
	}		
}
```

4、客户端访问代理角色

```java
package holle4_proxy;

import pojo.Host;
import pojo.HostMaster;
import pojo.Proxy;

public class My {

	public static void main(String[] args) {
		//房东要出租房子
		Host host = new HostMaster();
		//中介帮房东出租房子，但也收取一定费用（增加一些房东不做的操作）
		Proxy proxy = new Proxy(host);
		//看不到房东，但通过代理，还是租到了房子
		proxy.rent();
		
	}
}
```

![image-20200803105229478](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMDUyMjk0NzgucG5n)
代码翻倍：几十个真实角色就得写几十个代理

AOP横向开发

![image-20200803111539621](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMTE1Mzk2MjEucG5n)

### 10.2、动态代理

动态代理和静态角色一样，动态代理底层是反射机制

动态代理类是动态生成的，不是我们直接写好的！

动态代理(两大类)：基于接口，基于类

- 基于接口：JDK的动态代理【使用ing】
- 基于类：cglib
- java字节码实现：javasisit

了解两个类
1、Proxy：代理
2、InvocationHandler：调用处理程序
![image-20200803112619868](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMTI2MTk4NjgucG5n)

实例：

接口 Host.java

```java
//接口
package pojo2;
public interface Host {
	public void rent();
	
}
```

接口Host实现类 HostMaster.java

```java
//接口实现类
package pojo2;
public class HostMaster implements Host{	
	public void rent() {
		System.out.println("房东要租房子");
	}
}
```

代理角色的处理程序类 ProxyInvocationHandler.java

```java
package pojo2;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

///用这个类，自动生成代理
public class ProxyInvocationHandler implements InvocationHandler {

	// Foo f =(Foo) Proxy.NewProxyInstance(Foo. Class.GetClassLoader(),
	// new Class<?>[] { Foo.Class },
	// handler);

	// 被代理的接口
	public HostMaster hostMaster ;
	
	public void setHostMaster(HostMaster hostMaster) {
		this.hostMaster = hostMaster;
	}

	// 得到生成的代理类 
	public Object getProxy() {
		// newProxyInstance() -> 生成代理对象，就不用再写具体的代理类了
		// this.getClass().getClassLoader() -> 找到加载类的位置
		// hostMaster.getClass().getInterfaces() -> 代理的具体接口
		// this -> 代表了接口InvocationHandler的实现类ProxyInvocationHandler
		return Proxy.newProxyInstance(this.getClass().getClassLoader(), hostMaster.getClass().getInterfaces(), this);


	// 处理代理实例并返回结果
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		seeHouse();
		// 动态代理的本质，就是使用反射机制实现的
        // invoke()执行它真正要执行的方法
		Object result = method.invoke(hostMaster, args);
		fee();
		return result;
	}

	public void seeHouse() {
		System.out.println("看房子");
	}

	public void fee() {
		System.out.println("收中介费");
	}

}

```

用户类 My2.java

```java
package holle4_proxy;

import pojo2.Host;
import pojo2.Host2;
import pojo2.HostMaster;
import pojo2.ProxyInvocationHandler;

public class My2 {

	public static void main(String[] args) {
        
		//真实角色
		HostMaster hostMaster = new HostMaster();
        
		//代理角色，现在没有；用代理角色的处理程序来实现Host接口的调用
		ProxyInvocationHandler pih = new ProxyInvocationHandler();
        
        //pih -> HostMaster接口类 -> Host接口
		pih.setHostMaster(hostMaster);
        
		//获取newProxyInstance动态生成代理类
		Host proxy = (Host) pih.getProxy();
		
		proxy.rent();

	}
}
```

弹幕评论：
什么时候调用invoke方法的?
代理实例调用方法时invoke方法就会被调用，可以debug试试

改为**万能代理类**

```java
///用这个类，自动生代理
public class ProxyInvocationHandler implements InvocationHandler {

	// 被代理的接口
	public Object target;

	public void setTarget(Object target) {
		this.target = target;
	}

	// 得到生成的代理类 -> 固定的代码
	public Object getProxy() {
		return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
	}

	// 处理代理实例并返回结果
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		// 动态代理的本质，就是使用反射机制实现的
		// invoke()执行它真正要执行的方法
		Object result = method.invoke(target, args);
		return result;
	}

}
```

![image-20200803133035484](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMzMwMzU0ODQlMjAtJTIwJUU1JThBJUE4JUU2JTgwJTgxJUU0JUJCJUEzJUU3JTkwJTg2LnBuZw)

## 11、AOP

### 11.1、什么是AOP

![image-20200803134502169](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMzQ1MDIxNjklMjAtJTIwQU9QLnBuZw)

### 11.2、AOP在Spring中的使用

提供声明式事务，允许用户自定义切面

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等…
- 切面(Aspect)：横切关注点 被模块化的特殊对象。即，它是一个类。（Log类）
- 通知(Advice)：切面必须要完成的工作。即，它是类中的一个方法。（Log类中的方法）
- 目标(Target)：被通知对象。（生成的代理类)
- 代理(Proxy)：向目标对象应用通知之后创建的对象。（生成的代理类）
- 切入点(PointCut)：切面通知执行的”地点”的定义。（最后两点：在哪个地方执行，比如：method.invoke()）
- 连接点(JointPoint)：与切入点匹配的执行点。

![image-20200803154043909](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxNTQwNDM5MDkucG5n)
SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice:

![image-20200803135937435](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMzU5Mzc0MzUucG5n)
**即AOP在不改变原有代码的情况下，去增加新的功能。**（代理）

### 11.3、使用Spring实现AOP

导入jar包

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

#### 11.3.1、方法一：使用原生spring接口

springAPI接口实现

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userservice" class="service.UserServiceImpl"/>
    <bean id="log" class="log.Log"/>
    <bean id="afterLog" class="log.AfterLog"/>
	<!--方式一，使用原生Spring API接口-->
    <!--配置aop,还需要导入aop约束-->
    <aop:config>
        <!--切入点：expression:表达式，execution（要执行的位置）-->
        <aop:pointcut id="pointcut" expression="execution(* service.UserServiceImpl.*(..))"/>
        <!--UserServiceImpl.*(..) -》 UserServiceImpl类下的所以方法(参数)-->
        <!--执行环绕增加-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
        <!-- 环绕,在id="pointcut"的前后切入 -->
    </aop:config>

</beans>
```

execution(返回类型，类名，方法名(参数)) -> execution(* com.service.*,*(…))

UserService.java

```java
package service;
public interface UserService {   
	    public void add() ;
	    public void delete() ;
	    public void query() ;
	    public void update();
}
```

UserService 的实现类 UserServiceImp.java

```java
package service;

public class UserServiceImpl implements UserService {

    public void add() {
        System.out.println("add增");
    }
    public void delete() {
        System.out.println("delete删");
    }
    public void update() {
        System.out.println("update改");
    }
    public void query() {
        System.out.println("query查");
    }
}
```

前置Log.java

```java
package log;
import org.springframework.aop.MethodBeforeAdvice;
import java.lang.reflect.Method;

public class Log implements MethodBeforeAdvice {
    //method：要执行的目标对象的方法
    //args：参数
    //target：目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"的"+method.getName()+"被执行了");
    }
}
```

后置AfterLog.java

```java
package log;
import java.lang.reflect.Method;
import org.springframework.aop.AfterReturningAdvice;

public class AfterLog implements AfterReturningAdvice {
    //returnVaule: 返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
    	System.out.println("执行了"+method.getName()+"方法，返回值是"+returnValue);
    }
}
```

测试类MyTest5

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import service.UserService;

public class MyTest5 {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意:动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userservice");
        userService.add();
    }
}
```

#### 11.3.2、方法二：自定义类实现AOP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:aop="http://www.springframework.org/schema/aop"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
   	https://www.springframework.org/schema/beans/spring-beans.xsd
   	http://www.springframework.org/schema/aop
   	https://www.springframework.org/schema/aop/spring-aop.xsd">

   <!--注册bean-->
   <bean id="userservice" class="service.UserServiceImpl"/>
   <bean id="log" class="log.Log"/>
   <bean id="afterLog" class="log.AfterLog"/>
   <!-- 方式二，自定义 -->
   <bean id="diy" class="diy.DiyPointcut"/>
   <aop:config>
       <!--自定义切面-->
       <aop:aspect ref="diy">
           <!--切入点-->
           <aop:pointcut id="point" expression="execution(* service.UserServiceImpl.*(..))"/>
           <aop:before method="before" pointcut-ref="point"/>
           <aop:after method="after" pointcut-ref="point"/>
       </aop:aspect>
   </aop:config>

</beans>
```

```java
package diy;
public class DiyPointcut {

    public void before(){
        System.out.println("插入到前面");
    }

    public void after(){
        System.out.println("插入到后面");
    }
}
```

```java
//测试
public class MyTest5 {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意:动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userservice");
        userService.add();
    }
}
```





#### 11.3.3、方法三：使用注解实现

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">
	
    <!-- 注册 -->
    <bean id="userservice" class="service.UserServiceImpl"/>
    <!--方式三，使用注解实现-->
    <bean id="diyAnnotation" class="diy.DiyAnnotation"></bean>
    
    <!-- 开启自动代理 
		实现方式：默认JDK (proxy-targer-class="fasle")
    			 cgbin (proxy-targer-class="true")-->
	<aop:aspectj-autoproxy/>
    
</beans>
```

DiyAnnotation.java

```java
package diy;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect  //标注这个类是一个切面
public class DiyAnnotation {
	
    @Before("execution(* service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("=====方法执行前=====");
    }

    @After("execution(* service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("=====方法执行后=====");
    }

    //在环绕增强中，我们可以给地暖管一个参数，代表我们要获取切入的点
    @Around("execution(* service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前");

        Object proceed = joinPoint.proceed();

        System.out.println("环绕后");
    }
}
```

测试

```java
public class MyTest5 {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意:动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userservice");
        userService.add();
    }
}
```

输出结果：

![image-20200803175642064](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxNzU2NDIwNjQucG5n)

## 12、整合mybatis

mybatis-spring官网：https://mybatis.org/spring/zh/

**mybatis的配置流程：**

1. 编写实体类
2. 编写核心配置文件
3. 编写接口
4. 编写Mapper.xmi
5. 测试

### 12.1、mybatis-spring-方式一

1. 编写数据源配置
2. sqISessionFactory
3. sqISessionTemplate（相当于sqISession）
4. 需要给接口加实现类【new】
5. 将自己写的实现类，注入到Spring中
6. 测试！

先导入jar包

```xml
<dependencies>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.4</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.12</version>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.12</version>
    </dependency>

</dependencies>
	
<!--在build中配置resources，来防止资源导出失败的问题-->
<!-- Maven解决静态资源过滤问题 -->
<build>
<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
    <resource>
        <directory>src/main/resources</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
</resources>
</build>
```

![文件路径](Spring5%E8%AF%BE%E5%A0%82%E7%AC%94.assets/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDQxMjMyMTA1NjAucG5n)
**编写顺序：**
**User -> UserMapper -> UserMapper.xml -> spring-dao.xml -> UserServiceImpl -> applicationContext.xml -> MyTest6**

**代码步骤：**

pojo实体类 User

```java
package pojo;
import lombok.Data;
@Data
public class User {
	private int id;
	private String name;
	private String pwd;
}
```

mapper目录下的 UserMapper、UserMapperImpl、UserMapper.xml

接口UserMapper

```java
package mapper;
import java.util.List;
import pojo.User;
public interface UserMapper {
	public List<User> getUser();
}
```

UserMapperImpl

```java
package mapper;
import java.util.List;
import org.mybatis.spring.SqlSessionTemplate;
import pojo.User;

public class UserMapperImpl implements UserMapper{
	
	//我们的所有操作，在原来都使用sqlSession来执行，现在都使用SqlSessionTemplate；
	private SqlSessionTemplate sqlSessionTemplate;

	public void setSqlSessionTemplate(SqlSessionTemplate sqlSessionTemplate) {
		this.sqlSessionTemplate = sqlSessionTemplate;
	}

	public List<User> getUser() {
		UserMapper mapper = sqlSessionTemplate.getMapper(UserMapper.class);
		return mapper.getUser();
	}
}
```

UserMapper.xml （狂神给面子才留下来的）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        
<!-- 绑定接口 -->
<mapper namespace="mapper.UserMapper">
	<select id="getUser" resultType="pojo.User">
		select * from mybatis.mybatis
	</select>
</mapper>
```

resource目录下的 mybatis-config.xml、spring-dao.xml、applicationContext.xml

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!--开启日志-->
	<settings>
		<setting name="logImpl" value="STDOUT_LOGGING" />
	</settings>
	
	<!--可以给实体类起别名 -->
	<typeAliases> 
		<package name="pojo" />
	</typeAliases>

</configuration>
```

spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">
		
	<!--DataSource:使用Spring的数帮源替换Mybatis的配置 其他数据源：c3p0、dbcp、druid 
		这使用Spring提供的JDBC: org.springframework.jdbc.datasource -->
	<!--data source -->
	<bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=Asia/Shanghai"/>
		<property name="username" value="root" />
		<property name="password" value="root" />
	</bean>
	
	<!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>

	<!-- sqlSessionTemplate 就是之前使用的：sqlsession -->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    	<!-- 只能使用构造器注入sqlSessionFactory 原因：它没有set方法-->	
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>
		
</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">
    
	<!-- 导入spring-dao.xml -->
	<import resource="spring-dao.xml"/>
	
    <bean id="userMapper" class="mapper.UserMapperImpl">
        <property name="sqlSessionTemplate" ref="sqlSession"></property>
    </bean>

</beans>
```

测试类

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import mapper.UserMapper;
import pojo.User;
public class MyTest6 {
	public static void main(String[] args) {
        
		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		UserMapper userMapper = (UserMapper) context.getBean("userMapper");
		
		for (User user : userMapper.getUser()) {
			System.out.println(user);
		}
	}
}
```

### 12.2、mybatis-spring-方式二

UserServiceImpl2

```java
package mapper;
import pojo.User;
import org.apache.ibatis.session.SqlSession;
import org.mybatis.spring.support.SqlSessionDaoSupport;
import java.util.List;
//继承SqlSessionDaoSupport 类
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {
    public List<User> getUser() {
        SqlSession sqlSession = getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.getUser();
        //或者一句话：return getSqlSession().getMapper(UserMapper.class).getUser();
    }
}
```

spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">
		
	<!--DataSource:使用Spring的数帮源替换Mybatis的配置 c3p0 dbcp druid 
		这使用Spring提供的JDBC: org.springframework.jdbc.datasource -->
	<!--data source -->
	<bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=Asia/Shanghai"/>
		<property name="username" value="root" />
		<property name="password" value="root" />
	</bean>
	
	<!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>
    
	<!-- 方法二：SqlSessionTemplate 可以不写了-->
    
</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

	<import resource="spring-dao.xml" />

	<!-- 方法二 -->
	<bean id="userMapper2" class="mapper.UserMapperImpl2">
		<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
	</bean>
</beans>
```

测试

```java
public class MyTest6 {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		UserMapper userMapper = (UserMapper) context.getBean("userMapper2");
		for (User user : userMapper.getUser()) {
			System.out.println(user);
		}
	}
}
```

## 13. 声明式事务

- 把一组业务当成一个业务来做；要么都成功，要么都失败！
- 事务在项目开发中，十分的重要，涉及到数据的一致性问题
- 确保完整性和一致性

事务的ACID原则：
1、原子性
2、隔离性
3、一致性
4、持久性

ACID参考文章：https://www.cnblogs.com/malaikuangren/archive/2012/04/06/2434760.html

Spring中的事务管理

- 声明式事务：AOP
- 编程式事务：需要再代码中，进行事务管理

**声明式事务**

先导入jar包

```xml
<dependencies>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.4</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.12</version>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.12</version>
    </dependency>

</dependencies>
	
<!--在build中配置resources，来防止资源导出失败的问题-->
<!-- Maven解决静态资源过滤问题 -->
<build>
<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
    <resource>
        <directory>src/main/resources</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
</resources>
</build>
```

**代码步骤：**

pojo实体类 User

```java
package pojo;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
	private int id;
	private String name;
	private String pwd;
}
```

mapper目录下的 UserMapper、UserMapperImpl、UserMapper.xml

接口UserMapper

```java
package mapper;
import java.util.List;
import org.apache.ibatis.annotations.Param;
import pojo.User;

public interface UserMapper {
	public List<User> getUser();
	
	public int insertUser(User user); 
	
	public int delUser(@Param("id") int id); 
}
```

UserMapperImpl

```java
package mapper;

import pojo.User;
import org.apache.ibatis.session.SqlSession;
import org.mybatis.spring.support.SqlSessionDaoSupport;
import java.util.List;

public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {
    public List<User> getUser() {
    	User user = new User(5,"你好","ok");
    	insertUser(user);
    	delUser(5);
        SqlSession sqlSession = getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.getUser();
        //或者return  getSqlSession().getMapper(UserMapper.class).getUser();
    }
    //插入
	public int insertUser(User user) {
		return getSqlSession().getMapper(UserMapper.class).insertUser(user);
	}
	//删除
	public int delUser(int id) {
		return getSqlSession().getMapper(UserMapper.class).delUser(id);
	}
}
```

UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        
<!-- 绑定接口 -->
<mapper namespace="mapper.UserMapper">
	<select id="getUser" resultType="pojo.User">
		select * from mybatis.mybatis
	</select>
	
	<insert id="insertUser"  parameterType="pojo.User" >
		insert into  mybatis.mybatis (id,name,pwd) values (#{id},#{name},#{pwd})
	</insert>
	
	<delete id="delUser" parameterType="_int">
		deleteAAAAA from mybatis.mybatis where id = #{id}
		<!-- deleteAAAAA是故意写错的 -->
	</delete>

</mapper>
```

resource目录下的 mybatis-config.xml、spring-dao.xml、applicationContext.xml

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- configuration -->
<configuration>
	
	<!--开启日志-->
	<settings>
		<setting name="logImpl" value="STDOUT_LOGGING" />
	</settings>
	
	<!--可以给实体类起别名-->
	<typeAliases> 
		<package name="pojo" />
	</typeAliases>

</configuration>
```

spring-dao.xml（已导入约束）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
		
	<!--data source -->
	<bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=Asia/Shanghai"/>
		<property name="username" value="root" />
		<property name="password" value="root" />
	</bean>
	
	<!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>
	
	<!--声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="datasource" />
    </bean>

    <!--结合aop实现事务织入-->
    <!--配置事务的通知类-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--给哪些方法配置事务-->
        <!--新东西：配置事务的传播特性 propagation-->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <!-- *号包含上面4个方法：
            <tx:method name="*" propagation="REQUIRED"/> -->
        </tx:attributes>
    </tx:advice>

    <!--配置事务切入-->
    <aop:config>
        <aop:pointcut id="txpointcut" expression="execution(* mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txpointcut"/>
    </aop:config>

</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

	<import resource="spring-dao.xml" />

	<bean id="userMapper" class="mapper.UserMapperImpl">
		<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
	</bean>
</beans>
```

测试类

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import mapper.UserMapper;import pojo.User;
public class MyTest7 {
	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		UserMapper userMapper = (UserMapper) context.getBean("userMapper");
		
		for (User user : userMapper.getUser()) {
			System.out.println(user);
		}
	}
}
```

**思考：**
为什么需要事务？

- 如果不配置事务，可能存在数据提交不一致的情况下；
- 如果不在spring中去配置声明式事务，我们就需要在代码中手动配置事务！
- 事务在项目的开发中非常重要，涉及到数据的一致性和完整性问题！