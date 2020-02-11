# spring

## 1.spring是什么

Spring 是分层的 Java SE/EE 应用 **full-stack** 轻量级开源框架，以 **IoC(Inverse Of Control: 反转控制)**和 **AOP(Aspect Oriented Programming:面向切面编程)**为内核，提供了展现层 Spring MVC 和持久层 Spring JDBC 以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多 著名的第三方框架和类库，逐渐成为使用最多的 Java EE 企业应用开源框架。

## 2.spring的优势

### 方便解耦，简化开发

```
通过 Spring 提供的 IoC 容器，可以将对象间的依赖关系交由 Spring 进行控制，避免硬编码所造 成的过度程序耦合。用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可 以更专注于上层的应用。
```

### AOP 编程的支持

```
通过 Spring 的 AOP 功能，方便进行面向切面的编程，许多不容易用传统 OOP 实现的功能可以通过 AOP 轻松应付。
```

### 声明式事务的支持

```
可以将我们从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活的进行事务的管理， 提高开发效率和质量。
```

####  方便程序的测试

```
可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可 做的事情。
```

#### 方便集成各种优秀框架

```
Spring 可以降低各种框架的使用难度，提供了对各种优秀框架(Struts、Hibernate、Hessian、Quartz 等)的直接支持
```

####  降低 JavaEE API 的使用难度

```
Spring 对 JavaEE API(如 JDBC、JavaMail、远程调用等)进行了薄薄的封装层，使这些 API 的 使用难度大为降低。
```

# 控制反转-Inversion Of Control==解耦

**就是通过工厂模式+单例 解耦**

```java
解耦：
 /**
 * 程序的耦合
 * 耦合：
 *      类之间的依赖
 *      方法之间的依赖
 * 解耦：
 *      降低程序之间的依赖
 * 实际开发：
 *      编译期不依赖，运行时才依赖
 * 解耦思路
 *      避免使用new关键字，使用反射创建
 *      通过读取配置文件获取要创建的对象全限定类名
 */
```

# Spring ioc 思想 通过map<String,Object>实现

```java
package com.hpy.factory;

import java.io.IOException;
import java.io.InputStream;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

/**
 * @author hpy
 * 创建Bean对象的工厂
 * Bean:在计算机英语中有 可重用组件的定义。
 * javaBean:用java语言编写的可重用组件
 *      javaBean  >>  实体类 (远大于)
 * 1.需要一个配置文件 配置Service和dao
 *      配置文件内容：
 *              1.全限定类名
 *              2.唯一标志 = 全限定类名 (key:value)
 *      选择 properties或xml
 * 2.通过读取配置文件的中内容，反射创建Bean对象
 */
public class BeanFactory {
    /**
        定义一个properties对象
     */
    private static Properties properties;
    /**
     * 定义 一个map 存放我们要创建的对象 称之为容器
     *
     */
    private static Map<String,Object> beans;

    static {
        try {
            //实例化对象
            properties = new Properties();
            //获取properties流对象 用类加载器
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("factory.properties");
            properties.load(in);
            //实例化容器
            beans = new HashMap<String, Object>();
            //取出所有的keys
            Enumeration keys = properties.keys();
            //遍历枚举
            while (keys.hasMoreElements()){
                //取出每个key
                String key = keys.nextElement().toString();
                //根据key获取value
                String property = properties.getProperty(key);
                //反射创建对象
                Object newInstance = Class.forName(property).newInstance();
                //放入beanMap
                beans.put(key,newInstance);
            }
        } catch (IOException e) {
            throw new ExceptionInInitializerError("初始化失败");
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    /**
     * 根据Bean名称获取对象
     * @param beanName Bean名称
     * @return 方法
     */
    public static Object getBean(String beanName){
        return beans.get(beanName);
    }


}

```



### 通过spring 的 bean.xml配置bean对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--把对象的创建交给spring管理-->
    <bean id="userService" class="com.hpy.service.Impl.UserServiceImpl"/>
    <bean id="userDao" class="com.hpy.dao.impl.UserDaoImpl"/>


</beans>
```

### spring bean对象管理--如何创建获取容器和bean对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--把对象的创建交给spring管理-->
    <!--spring对bean的管理细节
        1.创建bean的三种方法
        2.bean对象的作用范围
        3.bean对象的生命周期
    -->
    <!--1.创建bean的三种方法-->
    <!--第一种 使用默认方式创建
        使用bean标签 配以 id class 属性 没有其他属性和标签时，采用的就是默认构造函数创建bean对象
        如果没有默认构造函数 则无法创建对象
    -->
    <bean id="userService" class="com.hpy.service.Impl.UserServiceImpl"/>
    <!--第二种 (在jar包中) 如果要拿到某个工厂中的方法创建对象 （使用某个类中的方法创建对象 创建容器）-->
    <bean id="InstanceFactory" class="com.hpy.factory.factory"/>
    <bean id="userService1" factory-bean="InstanceFactory" factory-method="getUserService"/>
    <!--使用某个类中的静态方法创建对象 -->
    <bean id="userService2" class="com.hpy.factory.factory" factory-method="getUserServiceStatic"/>


    <!--bean的作用范围 默认是单例模式的
        bean的scope属性
            作用：
                用于指定bean的作用范围
            取值：
                singleton:单例(默认) 常用
                prototype 多例 常用
                request 作用于web应用的请求范围
                session 作用于web应用的会话范围
                global-session 作用于集群环境的会发范围(全局会话范围，当不是集群时就是session)
    -->
    <bean id="userService3" class="com.hpy.service.Impl.UserServiceImpl" init-method="init" destroy-method="destroy"/>
    <bean id="userService4" class="com.hpy.service.Impl.UserServiceImpl" scope="prototype"
          init-method="init" destroy-method="destroy"/>
    <!--bean的生命周期-->
    <!--
    单例对象：
        出生：在容器的创建时就出生
        活着：只要容器在，就一直活着
        死亡：在容器销毁则对象消亡
        总结：单例对象的生命周期和容器相同
    多例对象：
        出生：使用对象时 创建对象
        活着：使用时一直活着
        死亡：当对象长时间不用 且没有别的对象引用时 由jvm回收
        总结：
    -->

</beans>
```

# 依赖注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--spring中的依赖注入
        依赖注入
            Dependency injection
        IOC
            降低程序间的耦合
        依赖关系管理：
            以后交给spring管理
        当前类中需要用到其他类的对象，由spring为我们提供，我们只需要配置bean.xml文件
        依赖关系维护：
            依赖注入
                能注入的类型：
                    基本类型和String
                    其他的Bean类型(在配置文件中活着注解配置过的bean)
                    复杂类型/集合类型
                注入方式：
                    构造函数
                    set方法
                    注解提供
    -->
    <!--构造函数注入  【一般不用】
        使用的标签是：
            constructor-arg
        位置：
            bean内部
        属性：
            type:用于指定要注入数据的数据类型，该数据类型也是构造函数中某个或某些数据类型
            value:注入的值  用于基本类型和String类型的数据
            index:指定索引位置的赋值，从0开始
            name:用于指定给构造函数中指定参数赋值
            ref:引用关联的bean对象 用于指定其他的bean类型 只要配置过的bean都可以引用
       优势：
            在获取bean对象时，创建时一定要注入参数，否则无法创建
       弊端：
            改变了bean对象的实例化方式，使我们在创建Bean对象时，如果用不到数据必须提供
    -->
    <!--配置一个Date对象 相当于new Date()-->
    <bean id="now" class="java.util.Date"> </bean>
    <bean id="userService" class="com.hpy.service.Impl.UserServiceImpl">
        <constructor-arg name="name" value="测试"> </constructor-arg>
        <constructor-arg name="age" value="18"> </constructor-arg>
        <constructor-arg name="birthday" ref="now"> </constructor-arg>
    </bean>

    <!--set方法注入 【常用】
        使用类的set方法注入
        property:set方法名称
        ref:引用关联的bean对象 用于指定其他的bean类型 只要配置过的bean都可以引用
        name:用于指定给构造函数中指定参数赋值
        优势：创建对象无限制，可以使用默认构造函数
        弊端：如果有个成员必须有值，不能保证一定赋值
    -->
    <bean id="userService1" class="com.hpy.service.Impl.UserServiceImpl2">
        <property name="name" value="测试"> </property>
        <property name="age" value="13"> </property>
        <property name="birthday" ref="now"> </property>
    </bean>

    <!--复杂类型或集合 使用set方法
        用户给list结构集合注入的：
            list array set
        用于给map结构集合注入的
            map props
        结构相同可以互换
      -->
    <bean id="userService2" class="com.hpy.service.Impl.UserServiceImp3">
        <property name="myStrs">
            <array>
                <value>AAA</value>
                <value>AAB</value>
                <value>AAC</value>
            </array>
        </property>
        <property name="myList">
            <list>
                <value>AAA</value>
                <value>AAB</value>
                <value>AAC</value>
            </list>
        </property>
        <property name="mySet">
            <set>
                <value>AAA</value>
                <value>AAB</value>
                <value>AAC</value>
            </set>
        </property>
        <property name="myMap">
            <map>
                <entry key="ccc" value="23"> </entry>
            </map>
        </property>
        <property name="myprops">
            <map>
                <entry key="ccc" value="23"> </entry>
            </map>
        </property>
    </bean>
</beans>
```

# 通过spring dbutils 实现数据库的crud

## 首先要对bean.xml进行配置

### 由于是在service中申请新的dao对象，所以采用set的依赖注入的思想实现

```java
private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
```



### 在dao层又要注入dbutils的对象 由于封装的dbutils是使用构造函数创建对象，所以使用构造函数依赖注入

```java
private QueryRunner runner;

    public void setRunner(QueryRunner runner) {
        this.runner = runner;
    }
```



### 对于jdbc的信息,使用c3p0连接池对象，c3p0连接池使用set进行依赖注入代码如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userService" class="com.hpy.service.impl.UserServiceImpl">
        <property name="userDao" ref="userDao"> </property>
    </bean>
    <bean id="userDao" class="com.hpy.dao.impl.UserDaoImpl">
        <property name="runner" ref="runner"> </property>
    </bean>


    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
        <!--使用构造函数添加数据源 jdbc的相关信息-->
        <constructor-arg name="ds" ref="jdbc"></constructor-arg>
    </bean>

    <bean id="jdbc" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--set依赖注入-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"> </property>
        <property name="jdbcUrl" value="jdbc:mysql:///mybaitis"> </property>
        <property name="user" value="root"> </property>
        <property name="password" value="1234"> </property>
    </bean>
</beans>
```

# spring注解

## xml配置--注意依赖注入时集合还是需要用xml注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
    <!--告诉spring在哪添加了注解 这样就会扫描包下的所有注解-->
    <context:component-scan base-package="com.hpy"> </context:component-scan>
  
		 <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
        <!--使用构造函数添加数据源 jdbc的相关信息-->
        <constructor-arg name="ds" ref="jdbc"></constructor-arg>
    </bean>

    <bean id="jdbc" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--set依赖注入-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"> </property>
        <property name="jdbcUrl" value="jdbc:mysql:///mybaitis"> </property>
        <property name="user" value="root"> </property>
        <property name="password" value="1234"> </property>
    </bean>
</beans>

```

## java注解详情

```java
package com.hpy.service.Impl;

import com.hpy.dao.UserDao;
import com.hpy.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
import javax.annotation.Resource;

/**
 * 用户业务层实现类
 * @author hpy
 * 注解实现创建对象
 *
 * 用于创建对象的注解
 *          和<bean id="" class="" scope="" init-method ="" destroy-mehod=""></bean>相同
 *      1.    Component;
 *              属性：
 *                  value:指定bean的id 默认值是当前类名的首字母小写 当前类即 userService
 *      2.    Controller：一般用于表现层
 *      3.    Service：一般用于业务层
 *      4.    Repository：一般用于持久层
 *          以上三个注解他们的作用和属性和Component一样，他们三个是Spring框架为我们提供的明确的三层使用注解
 *          使我们的三层对象更加清晰
 * 用于注入数据的注解
 *          作用相当于 ；<property name="" value="" ref=""></property>
 *      5.    Autowired
 *              作用：自动按照类型注入，只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功
 *                  如果没有任何bean的类型和要注入的类型匹配 则报错
 *                  如果有多个类型匹配时：先用变量名查找key 如果Key有一样的也会匹配注入
 *              出现位置：可以是变量上可以是方法上
 *      6.    Qualifier
 *              作用：在按照类中注入的基础上再按照名称注入，它给类成员注入时不能单独使用，但是给方法参数注入时可以使用
 *              属性
 *                  value:用于指定bean的id
 *              给类成员变量使用时 要配合 @Autowired
 *      7.     Resource:@Resource(name = "userDao1")
 *              作用直接按照id注入
 *              属性：
 *                  name:bean的id
 *						以上三个都只能注入bean类型的数据，而基本类型和String无法实现
 *      8.    Value:
 *              作用：用于基本类型和String类型的注入
 *              属性：
 *                  value:用于指定数据的值，可以使用spEL
 *                          spEL:${表达式}
 *          【在使用注解注入时，set方法不是必须的】
 *      
 *          【集合等只能用xml实现】
 * 用于改变作用范围的
 *          <bean scope=""></bean>
 *      9.    Scope:
 *              作用：用于指定bean的作用范围
 *              属性：
 *                  value: singelton prototype
 * 生命周期相关[了解]
 *          <bean init-method ="" destroy-mehod=""></bean>
 *     10.    PreDestroy:用于指定销毁方法
 *     11.    PostConstruct:用于指定初始化方法
 */
@Service("userService")
//@Scope("prototype")
public class UserServiceImpl implements UserService {
   /* @Autowired
    @Qualifier("userDao1")*/
    @Resource(name = "userDao1")
    private UserDao userDao;
    @PostConstruct
    public void init(){
        System.out.println("初始化方法");
    }
    @PreDestroy
    public void destroy(){
        System.out.println("销毁");
    }
    public void saveUser() {
        userDao.saveUser();
    }
}

```

# 使用注解改造之前用xml完成的项目--见springjdbcproject项目

# 实际上 对于集合和已经封装起来的类的新建对象使用注解更麻烦 最好使用xml 开发最好xml和注解混合

# 新注解 -为了替换剩余的xml

#### 使用新建一个config包 这里代码是包下的SpringConfiguration

```java
package config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.springframework.context.annotation.PropertySource;

/**
 * 是一个配置类，他的作用和bean.xml是一样的
 * 为了把剩下的两个标签解决
 * spring中的新注解
 *
 *   1.Configuration
 *          作用：指定当前类是一个配置类
 *          当配置类作为AnnotationConfigApplicationContext()的参数时，可以不写,不想写的话可以在调用时添加类
 * <context:component-scan base-package="com.hpy"> </context:component-scan>
 *   2.ComponentScan:
 *          作用：通过注解指定spring在容器中扫描的包
 *          属性 ；
 *              value: 他和basePackages作用是一样的
 *   3.Bean
 *          作用：
 *              把当前的方法的返回值放到spring的IOC容器的value中
 *          属性：
 *              name:用于指定bean的id  默认值是 当前方法的名称
 *          当我们使用注解配方法时，当方法有参数，会去容器查找有没有可用的Bean对象
 *   4.Import
 *          作用：用于导入其他的配置类
 *          属性
 *              value: 放入其他配置文件的字节码  有import的类为父配置类 导入的都是子配置类
 *   5.PropertySource
 *          作用：
 *              指定properties文件的位置
 *          value:
 *              指定文件的名称和路径
 *              关键字：classpath,表示类路径下
 */

@ComponentScan("com.hpy")
@Import(JDBCconfig.class)
@PropertySource("classpath:jdbcConfig.properties")
public class SpringConfiguration {

}

```

#### 这里是导入的子配置类 jdbcConfig.properties--需要在resources下新建配置文件

```java
package config;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import org.apache.commons.dbutils.QueryRunner;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.*;

import javax.sql.DataSource;

/**
 * 和spring连接数据库相关的配置类
 */

public class JDBCconfig {

    @Value("${driver}")
    private String driver;
    @Value("${jdbcUrl}")
    private String url;
    @Value("${userName}")
    private String username;
    @Value("${passWord}")
    private String password;
    /**
     * 创建一个QueryRunner对象r
     * @param dataSource 数据源
     * @return QueryRunner
     * 若有两个数据源 使用 @Qualifier("dataSource")
     */
    @Bean(name = "runner")
    @Scope("prototype")
    public QueryRunner createQueryRunner(@Qualifier("dataSource") DataSource dataSource){
        return new QueryRunner(dataSource);
    }

    /**
     * 创建数据源对象
     * @return 数据源
     *
     */
    @Bean(name = "dataSource")
    public DataSource createDataSource(){
        ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();
        try {
            comboPooledDataSource.setDriverClass(driver);
            comboPooledDataSource.setJdbcUrl(url);
            comboPooledDataSource.setUser(username);
            comboPooledDataSource.setPassword(password);
            return comboPooledDataSource;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}

```



# 针对spring Junit整合问题

```java

import config.SpringConfiguration;
import com.hpy.domain.User;
import com.hpy.service.UserService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.Date;
import java.util.List;

/**
 * Spring整合的junit
 *      1.通过注解替换junit的main
 *       RunWith
 *      2.告知Spring运行器 Spring和ioc创建时xml还是注解 并说明位置
 *         ContextConfiguration：
 *                  value: 指定xml的文件位置 加上classpath关键字 表示在类路径下
 *                  classes 执行的注解配置文件字节码
 *         当使用spring 5.x版本 要求junit的jar版本必须是4.12和以上
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class test {
    @Autowired
    private UserService userService;

    @Test
    public void testSel(){
        List<User> all = userService.findAll();
        for (User user : all) {
            System.out.println(user);
        }
    }

    @Test
    public void testUpd(){
        User user = new User();
        user.setUsername("韩鹏宇upd");
        user.setSex("男");
        user.setBirthday(new Date());
        user.setAddress("哈尔滨冠希");
        user.setId(41);
        userService.updateUser(user);
    }

    @Test
    public void testIns(){
        User user = new User();
        user.setUsername("韩鹏宇ins");
        user.setSex("男");
        user.setBirthday(new Date());
        user.setAddress("哈尔滨冠希");
        userService.addUser(user);
    }

    @Test
    public void testDel(){
        userService.delUser(46);
    }
}

```

