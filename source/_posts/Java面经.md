---
layout: title
title: Java面经
date: 2022-03-10 14:58:06
tags: 开发岗
top: 199
categories: 开发岗
---
# 面试
## 线程

### 怎么创建多线程

```java
1 继承Thread类,1.继承Thread类，然后重写里面的run方法，创建一个子类对象，然后调用线程对象start方法也就是调用run方法启动线程，一个线程只可以调用一次start方法
2 实现Runnable接口，1.定义子类实现Runnable接口，子类中重写Runnable中run方法，通过tread类含参构造器创建线程对象，将runnable接口的子类对象作为实际的参数传递给thread类中的构造器，调用Thread类的start方法，也就是调用了Runnable中子类接口的run方法
3 实现Callable接口，1.相比runnable中实现run方法，多了一个返回值，方法可以抛出异常，支持泛型的返回值以及借助FutureTask类来获取返回结果
4 使用线程池例如用Executor框架
```
 <!--more--> 

<!-- ![image-20220304093445388](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304093445388.png) -->

<!-- ![image-20220304094837276](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304094837276.png) -->

### 进程的生命进程

**sleep()yield()方法是暂停当前线程的执行，但是不会释放锁，wait()是当前线程暂停，释放锁，suspend（）方法是将线程挂起，但是该线程不会释放锁**，反过来就是sleep时间到了，notify()唤醒正在排队等待同步资源的线程中优先级最高者结束等待，notifyall()唤醒正在排队资源的所有线程结束等待，resume()重新调用。

### 死锁，两个线程都占用对方需要的资源不放手，

解决办法：减少suspend()的使用，避免嵌套同步，减少同步资源的定义

<!-- ![image-20220304095626881](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304095626881.png) -->

### synchronized和lock的区别

- 位置：lock只能写在代码块中，syn可以写在代码块和方法中
- lock需要自己显示的释放锁，但是syn是隐式的释放锁
- syn是java关键字是JVM层面实现加锁和解锁，lock是在代码层面实现加锁和解锁，Lock锁JVM将花费较少的时间来调度线程，性能更好，具有更好的扩展性 lock->同步代码块->同步方法

## Java基础

### JVM

Jvm是java虚拟机的意思，运行钱，java源代码编译为字节码.class，程序运行时，jvm将字节码翻译成特地平台下的机器码运行，只要在不同的平台安装jvm就可以运行 jdk = jre+开发工具集 jre = jvm + javase标准类库

### 面向对象

**封装继承多态**，继承实现父类，

+ 重载：同名方法，参数个数或者参数类型不同就可以，与返回值类型无关，只看参数列表，且参数列表必须不同
+ 重写：子类重写父类方法

+ 多态：Person p = new Student()，父类的引用指向子类的对象，属性是在编译时确定的，如果编译的时候是person类型，那么就没有school成员变量，**不能访问子类添加的属性和方法**
+ 继承：单继承和多层继承，不能有多重继承

### 访问权限修饰符

private：类中 default：同一个包下面也可以 protected：不同包的子类都可以 public 同一个工程 对于class的修饰只能public和default

<!-- ![image-20220304134655669](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304134655669.png) -->

### ==和equals

+ equals，所有的类都是继承了object类，所以都有equals方法，只能比较引用类型，作用和==相同，比较的是是否指向同一个对象，但是具体到不同的类File，String，Date等等包装类中，他们都重写了该方法，所以最后的比较的是内容
+ ==在基本类型的比较中比较的就是值，在引用类型中比较的就是内存地址，也就是判断是否是一个对象

### static

+ 随着类的加载而加载，优先于对象的存在，修饰的成员被所有的对象共享，访问权限允许时，可不创建对象直接被类调用
+ static方法只可以访问类的static修饰的属性和方法，不能访问类的非static结构（类还没有创建对象）
+ static中不能用thissuper

### object中的类

+ getClass equals hashCode() toString() wait() notify() notifyall()

## 设计模式

### 单例设计模式

+ 某个类只能存在一个对线实例，该方法构造器private，就不能随便new一个对象，只能调用该类的某个静态方法返回类内部创建的对象，然而静态方法只能访问静态变量，所以变量也是静态的，

<!-- ![image-20220304141017711](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304141017711.png) -->

+ 懒汉式，存在线程安全问题

<!-- ![image-20220304141738232](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304141738232.png) -->

### 代理模式

是java开发中使用最多的一种设计模式，代理设计就是为其他对象提供一种代理控制对这个对象的访问

<!-- ![image-20220304150059365](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304150059365.png) -->

<!-- ![image-20220304150115424](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304150115424.png) -->

### 工厂设计模式

+ 简单工厂模式，通过一个共同的接口来指向新创建的对象，如果要新加入新的产品，需要修改已有代码 

  <!-- ![image-20220304150603116](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304150603116.png) -->

+ 工厂方法模式：生成具体的产品的任务发给了具体的产品工厂

<!-- ![image-20220304150726576](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304150726576.png) -->

+ 抽象工厂模式：针对多类产品，在抽象工厂中增加创建新产品的接口，并在具体子工厂中实现新加产品的创建

<!-- ![image-20220304151619291](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304151619291.png) -->

## 异常处理机制

+ try catch finally  无论是否异常，都会执行finally
+ throws 异常类型 抛出异常

## 常用类

+ 栈：基本类型变量，对象的引用
+ 堆：new出来的对象实例
+ 常量池：常量值，常量与常量的拼接还是在常量池，不重复，但是堆是可以重复的  **如果拼接的结果调用 intern() 方法 返回值 就 在常量池中**
+ 

### StringBuffer和StringBuilder 

+ 都是可变的字符序列，而且提供相关功能的方法也一样
+ Stringbuffer是效率低的线程安全的 Stringbuilder是效率高不安全的

## 集合

## Collection

分为Collection和Map两种

<!-- ![image-20220304161103475](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304161103475.png) -->



### ArrayList和LinkedList Vector的异同

+ ArrayList和LinkedList。两周都是线程不安全的但是执行效率高，ArrayList是实现了基于动态数组，LinkedList是基于链表的数据结构，随机访问get和set，linkedlist是要移动指针，所以插入删除更快
+ ArrayList和vector的区别，vector是同步类，线程安全，效率低，但是ArrayList的效率高，vector每次扩容请求其大小的两倍，Arraylist是1.5倍

### LinkedHashset

+ 先计算hashcode然后如果code相同判断是否equals如果相同则不添加，如果不同，则链表方式添加

### HashSet

+ 添加元素：先计算hashcode值，然后根据值找到散列位置，如果两个元素的哈希值相同，那么比较equals，如果也相同，那么存储失败，如果不相同，那么会保存该元素，但是该位置有元素了，那么链表方式继续连接
+ 如果equals相同但是hashcode值不同，那么会将他们存储在不同的位置依然可以添加成功

### TreeSet

是排序set的实现类，保证集合元素处于排序状态，底层是红黑树

## MAP

<!-- <img src="C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304161127219.png" alt="image-20220304161127219" style="zoom:50%;" /> -->

### Hashmap

+ 负载因子越大密度越大，碰撞的几率越高，链表越容易长，次数越多，性能下降
+ 扩容机制：达到负载因子直接翻倍
+ 添加元素：首先计算key的哈希值，然后处理哈希值得到位置，如果没有元素直接添加，如果有，则循环依次比较hash值，如果彼此hash值不同，则直接添加成功，如果hash相同，比较equals，如果相同，替换，反之添加到后端，长度达到8，转为红黑树

判断两个key相等的标准，两个key通过比较

### Hashtable

+ 线程安全但是效率低，不可以使用null作为键和值

### HashSet和HashMap的区别

+ 一个实现的是map一个是set
+ 一个存储的是键值对，一个存储的无序不重复的对象
+ HashMap是一个put方式将元素放入map中，HashSet是add()方法将元素放入set中，HashMap比较快，因为使用键来获取值，

## IO

### IO原理

+ 用于处理设备之间的数据传输，java程序中，对于数据的输入输出操作以流的方式进行，输入：外部数据到内存中，输出，内存数据输出到磁盘光盘中
+ 分类：
  + 字节流：8bit
  + 字符流：16bit

<!-- ![image-20220304212205056](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304212205056.png) -->

<!-- <img src="C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220304212659499.png" alt="image-20220304212659499" style="zoom:50%;" /> -->

### 序列化和反序列化

+ 将对象转化为字节序列，这些序列可以保存在磁盘上 ，也可以在网络中传输
+ 实现Serializable接口
+ 需要使用对象流ObjectInputStream和ObjectOutPutStream，在序列化时需要

## 反射（动态语言）

<!-- <img src="C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305095953011.png" alt="image-20220305095953011" style="zoom:80%;" /> -->

<!-- <img src="C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305100712108.png" alt="image-20220305100712108" style="zoom: 67%;" /> -->

<!-- <img src="C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305094108355.png" alt="image-20220305094108355" style="zoom: 67%;" /> -->

# Java虚拟机面试题

### java运行的步骤

1. 编写.java文件
2. 利用编译器javac讲源代码翻译为字节码文件，字节码文件的后缀为.class
3. 解释器运行字节码文件
4. JVM包含了类加载器 运行时数据区，执行引擎。类加载器将类的.class文件中的二进制数据读入到内存中，将其放在运行时数据区的方法区内

### java存在内存泄露吗

+ 不再被使用的对象或者变量一直占据在内存中，java是有GC垃圾回收机制的，

+ 长生命周期的对象持有短生命周期对象的应用就可能发生泄露

### 简述GC垃圾回收机制

+ java中程序员不需要显示的释放一个对象的内存，而是由虚拟机自行执行，JVM中，有一个垃圾回收线程，低优先级的，正常情况下不会执行，只有在虚拟机空闲或者当前堆内存不足，才会触发执行，扫描没有被引用的对象，将他们添加到要回收的集合中进行回收

### 怎么判断对象是否可以被回收？

+ 引用计数器法：每个对象创建一个引用计数，有对象引用时计数器加1，引用被释放时计数-1，当计数器为0的时候就可以被回收，但是不能解决循环引用的问题
+ 可达性分析：从GCroot开始向下搜索，搜索所走过的路径称为引用链，当一个对象没有任何引用链，说明可以被回收了

### JVM有哪些垃圾回收算法

+ 标记清除算法：标记无用对象，然后进行清除回收
+ 复制算法：将内存分为两个大小相等的内存区域，当一块用完的时候将活的对象复制到另一块上，然后把已经使用的内存空间进行清除
+ 标记整理算法：标记无用对象，让所有存活的对象都向一端移动，然后直接清除掉边界以外的内存
+ 分代算法：根据对象存活周期将内存划分为几块，一般为新生代和老生代，新生代采用复制，老生代采用整理算法

## 虚拟机内加载机制

### java类加载机制

# Spring

## IOC（BeanFactory接口）

+ IOC 控制反转，将对象创建和对象之间的调用过程，交给spring进行管理
+ 底层原理：工厂模式和反射以及xml解析

## IOC操作Bean管理XML方式

### 依赖注入

+ spring创建对象
+  spring注入属性，依赖注入

1. 基于xml方式创建对象
2. 基于xml方式注入对象
3. 注入方式
   + 使用set方法注入
     + 直接创建类然后定义属性和对应的set方法
     + 在配置文件中创建对象配置属性注入 property name value 
   + 使用有参构造器进行注入
     + 创建类然后构造有参构造器
     + spring配置文件中使用construct-arg name value 进行属性注入
   + p名称空间注入
     + 添加p名称空间在配置文件中
     + 在配置文件使用p:bxxx进行属性注入

### FactoryBean

+ 工厂bean：在配置文件中定义bean的类型可以和返回类型不一样
  + 创建类，让这个类作为工厂bean，实现FactoryBean的接口
  + 实现接口里面的方法，在实现的方法中定义返回的bean类型

### Bean作用域

+ 默认bean是单实例

+ 如果要修改：scope = prototype singleton ，多实例是在getBean方法创建多实例对象，singlton是在加载配置文件创建单实例对象

### Bean生命周期

+ 构造器创建bean实例，无参数构造器
+ 调用set方法设置属性值
+ 调用bean的初始化方法
+ bean可以使用
+ 当容器管理使用bean的销毁方法

```xml
<bean id = "orders" class = "com.atguigu.Orders" init-method = "initMethod" destroy-method ="destroyMethod" ></bean>
//配置后置处理器
<bean id="myBeanPost" class="com.atguigu.spring5.bean.MyBeanPost"></bean>
```

+ 创建类实现接口BeanPostProcessor,创建后置处理器postProcessBeforeInitialization  postProcessAfterInitialization

<!-- <img src="C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305105943035.png" alt="image-20220305105943035" style="zoom:50%;" /> -->

### XML自动装配

1. 根据指定装配规则自动将属性进行注入

```xml
// 根据名称或者根据类型进行注入
<bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byType"> </bean> 
<bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
```

### 外部属性文件

1. 直接配置数据库信息

   + 配置德鲁伊连接池
   + 引入jar包

```xml
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"> <property name="driverClassName" value="com.mysql.jdbc.Driver"></property> <property name="url" value="jdbc:mysql://localhost:3306/userDb">
    </property> <property name="username" value="root"></property> <property name="password" value="root"></property> 
</bean>
```

2. 引入外部属性文件配置数据库连接池

   + 创建外部属性文件,properties格式文件,写数据库信息

   + 外部属性引入到spring配置文件中

     + 引入context名称空间

```xml
<!--引入外部属性文件--> 
<context:property-placeholder location="classpath:jdbc.properties"/> 
<!--配置连接池--> 
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"> <property name="driverClassName" value="${prop.driverClass}"></property> <property name="url" value="${prop.url}"></property> <property name="username" value="${prop.userName}"></property> <property name="password" value="${prop.password}"></property>
</bean>
```

## IOC操作Bean管理(注解方式)

+ Spring针对Bean管理中创建对象提供注解
  （1）@Component : 托管到Spring容器进行管理
  （2）@Service:业务逻辑层
  （3）@Controller:控制器
  （4）@Repository:数据访问层

* 上面四个注解功能是一样的，都可以用来创建bean实例

1. 基于注解方式创建对象

   + 引入依赖

   + 开启组件扫描
   
   + 创建类在类上面添加创建对象的注解
   
     ```xml
     <context:component-scan base-package="com.atguigu"  use-default-filters = "false">
         // 包含什么扫描和不包含
     	<context:include-filter type = "annotation" expression="org.springframework.stereotype.Controller"/>
         <context:exclude-filter type="annotation"...
     </context:component-scan>
     ```

2. 基于注解方式实现属性注入
   + @Autowired:根据属性类型进行自动装配
   + @Qualifier:根据名称进行注入
   + @Resource:名称和类型都可以注入

## AOP

1. 面向切面编程,利用AOP可以将业务逻辑的各个部分进行隔离,
2. 不修改源代码的方法,主干功能中添加新功能

### 底层原理

1. 动态代理,代理模式

   + 有接口的情况下,jdk动态代理

     + 创建接口实现类代理对象,增强类的方法

     <!-- ![image-20220305112811071](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305112811071.png) -->

   + 没有接口的情况下,使用CGLIB动态代理

     <!-- ![image-20220305112817921](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305112817921.png) -->

#### JDK动态代理

1. 使用Proxy类里面的方法创建代理对象
   + 调用newProxyInstance方法
   + 参数:类加载器,增强方法所在类,实现这个接口InvocationHandler,创建代理对象,写增强部分

2. 编写JDK动态代理代码

```java
（1）创建接口，定义方法 public interface UserDao { public int add(int a,int b); public String update(String id); }
（2）创建接口实现类，实现方法 public class UserDaoImpl implements UserDao { @Override public int add(int a, int b) { return a+b; } @Override public String update(String id) { return id; } }
（3）使用Proxy类创建接口代理对象
    //创建代理对象
class UserDaoProxy implements InvocationHandler{
    private Object obj;
    public UserDaoProxy(Object o){
        this.obj= o;
    }
    //增强方法
    @Override public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
	//方法之前
        System.out.println("方法之前执行...."+method.getName()+" :传递的参数..."+ Arrays.toString(args));
        // 增强方法
        Object res = method.invoke(obj , args);
        // 方法之后
        System.out.println("方法之后执行...."+obj); 
        return res;
    }
	//使用Proxy类创建接口代理对象
    public class JDKProxy{
        public static void main(String[] args) { 
            Class[] interfaces =  {UserDao.class};
            UserDaoImpl user = new UserDaoImpl();
            UserDao dao = (UserDao)Proxy.newProxyInstance(JDKproxy.class.getClassLoader(),interfaces,new UserDaoProxy(user));
            int res = dao.add(1,2);
    }
}
```

### 术语

1. 连接点:可以被增强的方法,
2. 切入点:实际被增强的方法
3. 通知(增强):实际增强的逻辑部分 前置 后置 环绕 异常 最终通知
4. 切面:把通知应用到切入点的过程

### AspectJ实现AOP操作

+ 一般使用注解方式

+ 切入点表达式:（语法结构： execution([权限修饰符] [返回类型] [类全路径] [方法名称]([参数列表]) )

  execution(* com.atguigu.dao.BookDao.* (..))

```java
1.创建类:public class User { public void add() { System.out.println("add......."); } }
2.创建增强类:编写增强逻辑
public class UserProxy{
    public void before() {
        //前置通知 
        System.out.println("before......"); 
    }
}
3、进行通知的配置
    （1）在spring配置文件中，开启注解扫描
    <context:component-scan base-package="com.atguigu.spring5.aopanno"></context:component-scan>
     (2) 使用注解创建User和UserProxy对象
     (3)在增强类上面添加注解 @Aspect
        @Component 
        @Aspect 
        //生成代理对象
        public class UserProxy {
      (4) 在spring配置文件中开启生成代理对象
        <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
4.配置不同类型的通知
      （1）在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置
            @Before(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
            @AfterReturning(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
            @After(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
            @AfterThrowing(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
            @Around(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
5.相同的切入点抽取
    @Pointcut(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
     public void pointdemo() { }
     @Before(value = "pointdemo()")
 6、有多个增强类多同一个方法进行增强，设置增强类优先级
     （1）在增强类上面添加注解 @Order(数字类型值)，数字类型值越小优先级越高
```

### AspectJ 配置文件

1. 创建两个类创建方法

2. 在spring配置文件中创建两个类对象

   ```xml
   <!--创建对象--> <bean id="book" class="com.atguigu.spring5.aopxml.Book"></bean> <bean id="bookProxy" class="com.atguigu.spring5.aopxml.BookProxy"></bean>
   ```

3. 在spring配置文件中配置切入点

```xml
<aop:config>
    <!--切入点-->
	<aop:pointcut id="p"expression="execution(* com.atguigu.spring5.aopxml.Book.buy(..))"/>
    <!--配置切面-->
    <aop:aspect ref = "bookProxy">
        <!--增强作用在具体的方法上-->
    	<aop:before mothod="before" pointcut-ref ="p"></aop:before>
    </aop:aspect>
</aop:config>
```

## 事务

### 什么是事务

1. 数据库操作最基本的单元，逻辑上一组操作要么都成功要么都失败
2. 事务的特性
   + 原子性：要么都成功要么都失败
   + 一致性：事务必须使数据库从一个一致性状态变换到另外一个一致性状态，事务按照预期生效，数据的状态就是预期的状态
   + 隔离性：不同事务之间的操作互不影响
   + 持久性：产生的结果对数据库的影响是持久的

### Spring事务操作

1. 事务添加到Javaee三层结构中的service中
2. 进行事务管理
3. 声明式事务管理 编程式事务管理
   + 基于注解方式
   + 基于xml配置文件方式
4.  在Spring进行声明式事务管理，底层使用AOP原理

### 注解方式配置事务管理器

1. 在spring配置文件中配置事务管理器

   ```xml
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"> <!--注入数据源--> 		注入数据源	
   <property name="dataSource" ref="dataSource"></property> 
   </bean>
   ```

2. 在spring配置文件中开启事务注解

   + 引入名称空间 tx

   + 开启事务注解

     ```xml
     <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
     ```

3. 在service类上面添加事务注解

   （1）@Transactional，这个注解添加到类上面，也可以添加方法上面
   （2）如果把这个注解添加类上面，这个类里面所有的方法都添加事务
   （3）如果把这个注解添加方法上面，为这个方法添加事务

# SpringMVC

### 什么是MVC

MVC是一种软件架构的思想，软件按照模型，视图和控制器来划分

M：model 工程中的javabean 处理数据

+ 分为实体bean ：user
+ 业务处理bean：Service或者Dao对象，专门用于处理业务逻辑和数据访问

V：view 视图层，工程中html或者jsp页面，交互展示数据

C：控制层，controller，接受请求和响应浏览器

### 什么是SpringMVC

是一个基于java的实现了mvc设计模式的请求驱动类型的轻量级web框架，将mvc三个部分分离，将web层进行职责解耦，把复杂的web应用分成逻辑清晰的及部分

### springmvc的流程

1. 用户发送请求到前端控制器DispatcherServlet
2. DispatcherServlet收到之后，调用HandlerMapping处理器映射器，请求获取Handler
3. 处理器映射器根据请求url找到具体的处理器handler，生成处理器对象及处理器拦截器，一并返回给DispatcherServlet
4. DispatcherServlet调用HandlerAdapter处理器适配器，请求执行Handler
5. HandlerAdapter经过适配调用，具体处理器进行处理业务逻辑
6. Handler执行完成返回Modelandview
7. HandlerAdapter将handler执行结果Modelandview返回给DispatcherServlet
8. DispatcherServlet 将modelandview传给viewresolver视图解析器进行解析
9. vieresolver解析返回具体的view
10. DispatcherServlet 对view进行渲染视图
11. Dispatcherservlet 响应用户

<!-- ![image-20220307151856553](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220307151856553.png) -->

### springmvc的优点

1. 可以支持各种视图技术，不仅仅局限于jsp
2. 与spring框架集成
3. 清晰的角色分配，前端控制器，请求到处理器映射，处理器适配器，视图解析器
4. 支持各种请求资源的映射策略

### springmvc怎么样设定重定向和转发的

转发：前面加上forward 

从定向：redirect

### springmvc的常用注解有哪些

@RequestMapping 用于处理请求url映射的注解，用于类或方法上，

@RequestBody：注解实现接受http请求的json数据，将json转化为java对象

@ResponseBody:注解实现将controller方法返回对象转换为json对象相应给客户

### Springmvc的控制器的注解一般用哪个，有没有别的注解可以代替

+ 一般用controller 也可以使用restcontroller 

### 如何解决POST请求中文乱码问题，GET的又如何处理

1. 解决post请求乱码问题，配置一个CharacterEncodingFilter过滤器，设置为utf-8

```xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
 
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

2. get请求中文参数乱码解决方法

```xml
①修改tomcat配置文件添加编码与工程编码一致，如下：
<ConnectorURIEncoding="utf-8" connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>
②另外一种方法对参数进行重新编码：
String userName = new String(request.getParamter("userName").getBytes("ISO8859-1"),"utf-8")
```

### springmvc拦截器怎么写的

+ 实现HandlerInterceptor接口，一个是继承适配器类，接着在接口方法中，实现处理逻辑，然后在Springmvc的配置文件中配置拦截器就可以

```xml
<!-- 配置SpringMvc的拦截器 -->
<mvc:interceptors>
    <!-- 配置一个拦截器的Bean就可以了 默认是对所有请求都拦截 -->
    <bean id="myInterceptor" class="com.zwp.action.MyHandlerInterceptor"></bean>
 
    <!-- 只针对部分请求拦截 -->
    <mvc:interceptor>
       <mvc:mapping path="/modelMap.do" />
       <bean class="com.zwp.action.MyHandlerInterceptorAdapter" />
    </mvc:interceptor>
</mvc:interceptors>
```

### 注解原理

1. 什么是注解
   + 注解是代码一种特殊标记，用于在编译类加载运行时进行解析和使用，并执行响应的处理。本质是继承了Annotation的接口，具体实现类是JDK动态代理生成的代理类，

### Springmvc怎么和ajax相互调用的

通过jackson框架可以把java里面的对象直接转化为js可以识别的json对象

+ 引入jackson包
+ 配置文件配置json映射
+ 在接受ajax方法里面可以直接返回object list等但是方法前面要加上@ResponseBody注解

### springmvc异常处理

可以将异常抛给spring框架，由spring框架来处理，我们只需要配置简单的异常处理器，在异常处理器中添视图页面

### Springmvc的控制器是不是单例模式，如果是有什么问题，怎么解决

单例模式，在多线程访问的时候有线程安全问题，解决方案是在控制器里面不能写可变状态量，如果需要使用这些可变状态，可以使用TreadLocal机制解决，为每个线程单独生成一个变量副本









# Mybatis

### 什么是Mybatis

+ 一个半对象关系映射框架，支持定制化SQL，存储过程以及高级映射，避免了JDBC代码和手动设置参数以及获取结果集

### Mybatis优点

+ 基于SQL语句编程，不会对应用程序或者数据库的现有设计造成影响，SQL写在xml下，解除sql与程序代码的耦合

+ 减少了JDBC代码量，消除了JDBC冗余代码，不需要手动开关连接
+ 很好的和Spring集成

### 缺点

+ 对数据库的移植性差，不能随便更换数据库
+ sql编写的工作量大，对开发人员编写sql有要求

### Hibernate和Mybatis的区别

+ 同：都是对jdbc的封装，都是持久层的框架，都用于dao层的开发
+ 不同：映射关系：Mybatis是一个半自动的框架，配置java对象和sql语句执行结果的对应关系，hibernate是一个全表映射的框架，

### ORM 对象关系映射

解决关系型数据和简单java对象的映射关系的技术，扫描描述对象和数据库之间的元数据，将程序中的对象自动持久化到关系型数据库中

### 传统JDBC开发存在的问题，mybatis如何解决

+ 数据库连接创建，频繁释放系统资源浪费从而影响系统性能，使用数据库连接池 ：**在mybatis-config.xml中配置数据链接池，使用连接池管理数据库连接。**
+ Sql语句写在代码中造成代码不易维护，实际应用sql变化的可能较大，sql变动需要改变java代码  **解决：将Sql语句配置在XXXXmapper.xml文件中与java代码分离**。
+ 向sql语句传参数麻烦，**Mybatis自动将java对象映射到sql语句中**
+ 对结果集解析麻烦，sql变化导致解析代码变， **解决：Mybatis自动将sql执行结果映射至java对象。**

## Mybatis架构

### Mybatis编程步骤

1. 创建sqlsessionfactory
2. 通过sqlsessionfactory 创建sqlsession
3. 通过sqlsession执行数据库操作
4. 调用session.commit()提交事务
5. 调用session.close()关闭会话

### 说说mybatis的工作原理

<!-- ![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/4/13/1717343a66d9566c~tplv-t2oaga2asx-watermark.awebp) -->

1. 读取配置文件
2. 加载映射文件
3. 构造会话工厂
4. 创建会话对象
5. Executor执行器
6. MappedStatement对象
7. 输入参数映射 ，输出结果映射

### 缓存机制

+ 一级缓存：执行多次查询条件相同的sql，这样就会优先命中一级缓存，避免直接对数据库进行查询，提高性能
+ 二级缓存：多个Sqlsession之间要共享缓存的话，则需要使用二级缓存。

### 使用Mybatis的mapper调用接口有哪些要求

1. 接口方法名和mapper.xml中定义的每个sql的id相同
2. mapper接口方法的输入类型参数和mapper.xml中定义的每个sql的parameterType类型相同
3. mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType类型相同
4. namespace必须是mapper接口的类路径

### Dao接口的原理

是jdk动态代理，使用jdk动态代理为dao接口生成代理proxy对象，代理对象会拦截接口方法，转而执行mappedstatement所代表的sql，然后将sql执行结果返回

# Redis

+ 高性能非关系型Nosql键值对数据库
+ redis可以存储键和五种不同类型的值之间的映射，键的类型只能是字符串，值的类型为字符串，列表，集合，散列表，有序集合
+ 数据库存在内存中，读写很快，

### 优缺点

+ 优点：读写性能优异，11万次/s 写8万次/s

+ 支持数据持久化，AOF 和RDB持久化方式
+ 支持事务，所有操作都是原子性的，
+ zset list hash string set
+ 支持主从复制，主机自动将数据同步到从机，读写分离

### 为什么要用Redis 为什么要用缓存

1. 高性能

   <!-- ![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDUzNDg2OS02N2YxOGVmY2FmZTQ2NjlhLmpwZw?x-oss-process=image/format,png) -->

2. 高并发

​    直接操作缓存能够承受的请求是远远大于直接访问数据库的，所以我们可以考虑把数据库中的部分数据转移到缓存中去，这样用户的一部分请求会直接到缓存这里而不用经过数据库

<!-- ![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDUzNDg2OS0wOWIxZDI3OWEwNWVmNWJjLmpwZw?x-oss-process=image/format,png) -->

### Redis为什么这么快

1. 完全基于内存
2. 数据结构简单，对数据的操作也简单，
3. 单线程，避免了换入换出操作
4. 使用多路IO复用模型，非阻塞IO
5. 使用底层模型不同

## 持久化

### 什么是持久化

把内存中的数据写到磁盘，防止宕机内存数据丢失

### redis持久化的机制，各自优缺点

+ RDB，redisdatabase，快照，一定时间就保存在磁盘中，数据安全性低，（隔一段时间进行持久化，如果持久化之间redis发生故障会发生数据丢失）但是效率高
+ AOF持久化，所有的命令记录以redis命令请求协议的格式完全持久化存储保存为aof文件，每次写的指令都保存在日志，然后重启redis就会将持久化的日志中文件恢复数据
+ **优先使用aof恢复数据** ， 但是效率低，数据大启动效率低 aof文件比rdb大，恢复慢


# 操作系统

## 简介

### 什么是操作系统

+ 操作系统是管理计算机硬件和软件的程序
+ 向下屏蔽了硬件的复杂性，向上提供给用户对软件操作的便利性

### 操作系统的主要功能

+ 内存管理，进程管理，文件管理，设备管理，提供用户接口

### 软件访问硬件的几种方式

+ IO操作，分为串行和并行
+ 直接访问：直接访问由用户进程控制主存或者CPU和外围设备之间的信息传送
+ 中断驱动：
+ DMA直接内存访问：DMA控制方式，
+ 通道控制方式：

### 操作系统的主要目的是什么

+ 管理计算机资源，CPU 内存 磁盘 外部设备等等
+ 提供一种图形界面，方便用户
+ 方便软件使用计算机硬件设备提供的资源

### 种类有哪些

+ windows linux macos

### Linux系统下的app为什么不能在windows下运行

+ 格式不同，linux下的可执行文件是elf格式，但是windows下是pe文件格式
+ API 不同，linux下的API是系统调用，通过软中断实现，windows下的api是放在动态连接库文件中的，也就是DDL

### 操作系统的结构

+ 单体系统

+ 分层系统
+ 微内核
+ 客户服务器模式

### 什么是用户态和内核态

+ 内核态：处于内核态的CPU可以访问任意的数据，外围设备，网卡硬盘等等，从一个程序切换到另一个程序，占用CPU不会发生抢占的情况，一般处于特权级0的状态是内核态
+ 用户态：受限的访问内存，不允许访问外围设备，用户态的CPU不允许独占，CPU可以被其他程序获取

### 用户态和内核态如何切换

+ 系统调用：用户态主动切换到内核态的一种方式，fork就是创建一个新进程的系统调用
+ 中断：当外围设别完成用户请求的操作后，会向CPU发送响应的中断信号，CPU会暂停执行下一条指令而去执行与中断信号对应的处理程序。比如硬盘读写操作完成，系统会切换到硬盘读写的中断处理程序中执行后续操作等。
+ 异常：发生异常，就会触发由当前运行进程切换到处理此异常的内核相关程序，比如缺页异常

<!-- ![image-20220305215303096](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305215303096.png) -->

### 什么是实时系统

+ 软实时和硬实时，比如汽车生产车间，焊接机器必须在某一时刻内完成焊接，焊接的太早或者太晚都会对汽车造成永久性伤害。

+ 比如数字音频、多媒体、手机都是属于软实时操作系统。

### Linux操作系统的启动过程

## 进程管理

### 进程和线程，区别

+ 进程中可以包含多条线程
+ 进程之间相互独立，各个线程之间可能相互影响，共享数据
+ 进程对资源的开销更大，利于资源的保护和管理。线程的创建执行开销小，但是不利于资源的保护

线程私有的：

+ 程序计数器：接下来的执行指令的地址
+ 栈指针 所属线程的栈区
+ 函数运行使用的寄存器

共有：

+ 代码区
+ 堆区
+ 数据区

### 什么是上下文切换

+ 将 CPU资源从一个进程分配给另一个进程的机制

### 多线程的好处

+ 提高执行效率 经济适用 提高用户使用流畅感

### 进程终止的方式

+ 正常退出 进程执行完毕
+ 错误退出 文件不存在
+ 严重错误 除0 引用不存在的内存
+ 被其他进程杀死 kill 

### 进程间的通信方式

+ 匿名管道、管道：实质就是一个内核缓冲区，只能用于亲缘关系的进程通信
+ 有名管道：支持不是亲缘关系的进程之间的通信
+ 信号：终止信号，退出信号，结束进程信号 SIGINT：终止信号 SIGQUIT：退出信号 SIGKILL：结束进程信号 SIGCLD：子进程退出信号
+ 消息队列：存放在内核中的消息链表，内核重启或者显示的删除一个消息队列才会真的删除
+ 共享内存：多个进程可以直接读写同一块内存空间，需要依靠同步机制来实现进程同步
+ 信号量：信号量是一个计数器，用于多进程对共享数据的访问，信号量的意图在于进程间同步。
+ 套接字：ip加端口号

### 不同进程不能同时操作一个资源

1. 竞态条件：多个进程同时对一共享数据进行修改，从而影响程序的正常运行叫做竞态条件
2. 临界区：禁止一个或多个进程在同一时刻对共享资源进行读写 互斥条件
3. 忙等互斥：当一个进程对资源修改的时候，其他进程必须等待，进程之间要有互斥性

### 进程的状态

新建就绪运行阻塞结束

<!-- ![image-20220306100208112](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220306100208112.png) -->

### 进程的调度算法

+ 先来先服务:FIFO
+ 最短作业优先：进程运行时间最短的先
+ 时间片轮转：每个进程给固定的时间段  分时系统：Round Robin，将时间片以相同部分循环分配给每个进程
+ 优先级调度算法：哪个进程优先就调用哪个

### 影响调度程序的指标

+ CPU使用率 
+ 等待时间：进程等待时间
+ 吞吐量
+ 响应时间
+ 周转时间

## 内存管理

### 什么是虚拟内存

+ 操作系统为每一个进程提供一个独立的虚拟地址空间
+ 方便进程有进行内存的访问
+ 使有限的物理内存可以运行一个比他大很多的程序，每个程序有自己的地址空间，空间被分割成很多快，每一块叫做一页

### 分页和分段机制

+ 分段：把虚拟内存分为多个独立的地址空间，每个地址空间可以动态增长，互不影响，每个段可以单独进行控制，有助于保护和共享
+ 分页：将虚拟内存分为页面，将物理内存分为页框，实现一对一映射

### 页表的作用

+ 虚拟内存到物理内存的映射，TBL快表，当请求一个虚拟地址时，处理器检查是否缓存了该虚拟地址的映射，如果命中则直接返回物理地址，否则通过页表搜索对应的物理地址
+ 多级页表可以节约地址转化的空间

### 页面置换算法

+ 最优页面置换算法 ：将来最久没被使用的算法，不太可行
+ 最近最少未使用：LRU，
+ FIFO：最先进入的放在表头，最后进入的放在表尾，缺页中断，直接淘汰表头的页面
+ 第二次机会页面置换算法：记录每个页面的R位，如果为1表示最近有访问，将其清0，然后放入表尾，继续检查下一个表头，直到遇到R为0
+ 时钟页面置换算法：

### 虚拟内存的实现方式

1. 需要页表机制，缺页中断机制，地址变换机制

+ 请求分页存储管理：只要求将当前需要的一部分页面装入内存，请求调页和页面置换的功能
+ 请求分段存储管理：将若干个分段装入内存，便可以启动作业运行，在作业运行过程中，如果要访问的分段不在内存中，则通过调段功能将其调入，
+ 请求段页式存储管理：

## 死锁

### 什么是僵尸进程

+ 已完成处于终止状态，但在进程表中仍然存在的进程，

### 死锁产生的必要条件

+ 互斥：每个资源都被分配了一个进程或者资源是可用的
+ 保持和等待条件：已经获取资源的进程被认为可以获取新的资源
+ 不可强占：不可以强行占用其他进程所使用的资源
+ 循环等待：一定有两个或者以上的进程组成一个循环，每个进程都等待下一个进程释放的资源









### 垃圾回收机制

如何确定一个对象是否可以被回收

+ 引用计数算法,判断对象的引用数量
+ 互相引用的两个对象永远不会回收

<!-- ![image-20220305161816756](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305161816756.png) -->

+ 可达性分析算法:判断对象的引用链是否可达来决定对象是否可以被回收
+ 当一个对象到GC root 没有任何引用链相连,则证明这个对象是不可用的

### 垃圾收集算法

1. 标记清除算法
   + 从根集合扫描,对存活的对象标记,标记完毕后,再扫描整个空间中未被标记的对象进行回收,

<!-- ![image-20220305165243659](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305165243659.png) -->

2. 复制算法

   + 将内存按照容量划分为大小相等的两块,每次只使用其中的一块,当这块的内存用完了,就将还存活的复制到另外一块上面,然后把已经使用过的内存空间一次清理掉

     <!-- ![image-20220305165457566](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305165457566.png) -->

3. 标记整理算法
   + 让所有存活的对象都向一端移动,然后直接清理掉端边界以外的内存,类似于磁盘清理的过程
   + 标记整理不会产生产生内存碎片,但是标记清除算法会产生内存碎片

<!-- ![image-20220305170014813](C:\Users\lab\AppData\Roaming\Typora\typora-user-images\image-20220305170014813.png) -->

4. 分代收集算法
   + 生代对象存活率低，就采用复制算法；老年代存活率高，就用标记清除算法或者标记整理算法。Java堆内存一般可以分为新生代、老年代和永久代三个模块
   + 新生代:**新生代的目标就是尽可能快速的收集掉那些生命周期短的对象，一般情况下，所有新生成的对象首先都是放在新生代的**
   + 老年代:**老年代存放的都是一些生命周期较长的对象，就像上面所叙述的那样，在新生代中经历了N次垃圾回收后仍然存活的对象就会被放到老年代中**
   + 永久代:**永久代主要用于存放静态文件，如Java类、方法等。**

