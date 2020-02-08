# Mybatis

1、什么是框架？
	它是我们软件开发中的一套解决方案，不同的框架解决的是不同的问题。
	使用框架的好处：
		框架封装了很多的细节，使开发者可以使用极简的方式实现功能。大大提高开发效率。
2、三层架构
	表现层：
		是用于展示数据的
	业务层：
		是处理业务需求
	持久层：
		是和数据库交互的

![01三层架构](.\img\mybatis\01三层架构.png)

3、持久层技术解决方案
	JDBC技术：
		Connection
		PreparedStatement
		ResultSet
	Spring的JdbcTemplate：
		Spring中对jdbc的简单封装
	Apache的DBUtils：
		它和Spring的JdbcTemplate很像，也是对Jdbc的简单封装

	以上这些都不是框架
		JDBC是规范
		Spring的JdbcTemplate和Apache的DBUtils都只是工具类

![02持久层总图](.\img\mybatis\02持久层总图.jpg)

4、mybatis的概述

​	mybatis是一个持久层框架，用java编写的。
​	它封装了jdbc操作的很多细节，使开发者只需要关注sql语句本身，而无需关注注册驱动，创建连接等繁杂过程
​	它使用了ORM思想实现了结果集的封装。

	ORM：
		Object Relational Mappging 对象关系映射
		简单的说：
			就是把数据库表和实体类及实体类的属性对应起来
			让我们可以操作实体类就实现操作数据库表。
	
			user			User
			id			userId
			user_name		userName
	今天我们需要做到
		实体类中的属性和数据库表的字段名称保持一致。
			user			User
			id			id
			user_name		user_name
### pom.xml依赖管理

```xml
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>
    </dependencies>
```

5、mybatis的入门
	mybatis的环境搭建
		第一步：创建maven工程并导入坐标
		第二步：创建实体类和dao的接口
		第三步：创建Mybatis的主配置文件
				SqlMapConifg.xml
		第四步：创建映射配置文件
				UserDao.xml

### 	SqlMapConifg.xml---主配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--mybatis的主配置文件-->
<configuration>
<!--   配置properties 可以放在外部的配置文件
resource 用于指定文件位置 用文件方式写地址
url:属性 按照url属性写地址 统一资源定位符 可以唯一标志一个资源的位置
 写法：
        http://localhost:8080:mybatis/demo1Servlet
        协议      主机     端口      URI:统一资源标志符 可以在应用中定位一个资源
      url = file://E:/javaSeLearn/mybatis1/src/main/resources/jdbcConfig.properties
-->
    <properties resource="jdbcConfig.properties">
    <!--    <property name="driver" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql:///mybatis"/>
        <property name="username" value="root"/>
        <property name="password" value="1234"/>-->
    </properties>
<!-- 使用 typeAliases 配置别名 只能配置domain中的别名   -->
    <typeAliases>
    <!--   用于设置别名  type是实体类全部类名 alias是别名  当指定别名则不再区分大小写  -->
<!-- <typeAlias type="com.hpy.domain.User" alias="User"></typeAlias>-->
<!-- 用于要配置指定别名的包，当指定后改包下的实体类都会注册别名 类名就是别名 不再区分大小写-->
        <package name="com.hpy.domain"/>
</typeAliases>
        <!--   配置环境    -->
    <environments default="mysql">
        <!--  配置mysql环境      -->
        <environment id="mysql">
        <!--  配置事务类型       -->
            <transactionManager type="JDBC"></transactionManager>
            <!--配置数据源 也叫连接池-->
            <dataSource type="POOLED">
                <!--配置连接数据库的基本信息-->
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>

        </environment>
    </environments>
<!-- 指定映射配置文件，指的是每个dao独立的配置文件   -->
    <mappers>
<!-- <mapper resource="com/hpy/dao/UserDao.xml"/>-->
<!-- 指定接口所在的包 就不需要再写mapper 或者 resource 了-->
        <package name="com.hpy.dao"/>
    </mappers>
<!--    如果使用注解方式-->
<!--    <mappers>-->
<!--        <mapper class="com.hpy.dao.UserDao"/>-->
<!--    </mappers>-->
</configuration>
```

### 	UserDao.xml--dao接口对应的数据库操作--应该叫UserMapper.xml更合适

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--记得添加resultType为要封装的实体类-->
 <mapper namespace="com.hpy.dao.UserDao" resultType="com.hpy.domain.User">
<!--    配置查询所有  id为方法名-->
    <select id="findAll">
        select * from user
    </select>
</mapper>
```

环境搭建的注意事项：
		**第一个：创建UserDao.xml 和 UserDao.java时名称是为了和我们之前的知识保持一致。**
			在Mybatis中它把持久层的操作接口名称和映射文件也叫做：Mapper
			所以：UserDao 和 UserMapper是一样的
		**第二个：在idea中创建目录的时候，它和包是不一样的**

```
包在创建时：	com.itheima.dao它是三级结构
目录在创建时：com.itheima.dao是一级目录
```

​		**第三个：mybatis的映射配置文件位置必须和dao接口的包结构相同**
​		**第四个：映射配置文件的mapper标签namespace属性的取值必须是dao接口的全限定类名**
​		**第五个：映射配置文件的操作配置（select），id属性的取值必须是dao接口的方法名**

```
当我们遵从了第三，四，五点之后，我们在开发中就无须再写dao的实现类。
mybatis的入门案例
	第一步：读取配置文件
	第二步：创建SqlSessionFactory工厂
	第三步：创建SqlSession
	第四步：创建Dao接口的代理对象
	第五步：执行dao中的方法
	第六步：释放资源
```

![入门案例的分析](.\img\mybatis\入门案例的分析.png)

```java

	//1.读取配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);
        //3.使用工厂生产SqlSession
        SqlSession sqlSession = factory.openSession();
        //4.使用SqlSession创建Dao接口的代理对象
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        //5.使用代理对象执行方法
        List<User> all = mapper.findAll();
        for (User user : all) {
            System.out.println(user);
        }
        //6.释放资源
        sqlSession.close();
        in.close();
	注意事项：
		不要忘记在映射配置中告知mybatis要封装到哪个实体类中
		配置的方式：指定实体类的全限定类名
	
	mybatis基于注解的入门案例：
		把UserDao.xml移除，在dao接口的方法上使用@Select注解，并且指定SQL语句
		同时需要在SqlMapConfig.xml中的mapper配置时，使用class属性指定dao接口的全限定类名。
明确：
	我们在实际开发中，都是越简便越好，所以都是采用不写dao实现类的方式。
	不管使用XML还是注解配置。
	但是Mybatis它是支持写dao实现类的。
```

6、自定义Mybatis的分析：
	mybatis在使用代理dao的方式实现增删改查时做什么事呢？
		只有两件事：
			第一：创建代理对象
			第二：在代理对象中调用selectList
		

	自定义mybatis能通过入门案例看到类
		class Resources
		class SqlSessionFactoryBuilder
		interface SqlSessionFactory
		interface SqlSession
![03mapper配置文件的创建要求](.\img\mybatis\03mapper配置文件的创建要求.jpg)

![04mybatis的分析](.\img\mybatis\04mybatis的分析.png)

![查询所有的分析](.\img\mybatis\查询所有的分析.png)

<img src=".\img\mybatis\自定义Mybatis分析.png" alt="自定义Mybatis分析" style="zoom:200%;" />

OGNL表达式：
	Object  Graphic  Navigation  Language
	对象	     图	         导航	      语言
	

	它是通过对象的取值方法来获取数据。在写法上把get给省略了。
	比如：我们获取用户的名称
		类中的写法：user.getUsername();
		OGNL表达式写法：user.username
	mybatis中为什么能直接写username,而不用user.呢：
		因为在parameterType中已经提供了属性所属的类，所以此时不需要写对象名
```xml
1.对于增删改三种操作，如果对象的名字和列名不一致，只需要改一下传入参数的变量名 #{这里的名}
2.对于查询操作：
		1.设置别名  select id as uid,username as userName,sex as Sex,address as Address...
		2.mybatis 提供的配置：
			配置查询结果的列名和实体类的属性名对应关系
			<resultMap id="UserMap" type="com.hpy.domain.User">
				<!--主键字段对应-->
				<id property ="userid" column="id"></id>
				<!--非主键字段对应-->
				<result property ="userName" column="username"></result>
				<result property ="Sex" column="sex"></result>
				<result property ="Address" column="address"></result>
                  ...
			</resultMap>
			使用这个配置时，要将原来的 resultType属性替换为resultMap属性
```

### mybatis连接池

```
mybatis中的连接池
	mybatis连接池提供了3种方式的配置：
		配置的位置：
			主配置文件SqlMapConfig.xml中的dataSource标签，type属性就是表示采用何种连接池方式。
		type属性的取值：
			POOLED	 采用传统的javax.sql.DataSource规范中的连接池，mybatis中有针对规范的实现
			UNPOOLED 采用传统的获取连接的方式，虽然也实现Javax.sql.DataSource接口，但是并没有使用池的思想。
			JNDI	 采用服务器提供的JNDI技术实现，来获取DataSource对象，不同的服务器所能拿到DataSource是不一样。
				 注意：如果不是web或者maven的war工程，是不能使用的。
				 我们课程中使用的是tomcat服务器，采用连接池就是dbcp连接池。
```

![无标题2](.\img\mybatis\无标题2.png)

#### 3、mybatis中的事务

​	什么是事务
​	事务的四大特性ACID
​	不考虑隔离性会产生的3个问题
​	解决办法：四种隔离级别

	它是通过sqlsession对象的commit方法和rollback方法实现事务的提交和回滚
	openSession(bool autoCommit) true就打开了自动提交 不用每次提交----一般不适用
#### 4、mybatis中的多表查询

​	表之间的关系有几种：
​		一对多
​		多对一
​		一对一
​		多对多
​	举例：
​		用户和订单就是一对多
​		订单和用户就是多对一
​			一个用户可以下多个订单
​			多个订单属于同一个用户

		人和身份证号就是一对一
			一个人只能有一个身份证号
			一个身份证号只能属于一个人
	
		老师和学生之间就是多对多
			一个学生可以被多个老师教过
			一个老师可以交多个学生
	特例：
		如果拿出每一个订单，他都只能属于一个用户。
		所以Mybatis就把多对一看成了一对一。
	
	mybatis中的多表查询：
		示例：用户和账户
			一个用户可以有多个账户
			一个账户只能属于一个用户（多个账户也可以属于同一个用户）
		步骤：
			1、建立两张表：用户表，账户表
				让用户表和账户表之间具备一对多的关系：需要使用外键在账户表中添加
			2、建立两个实体类：用户实体类和账户实体类
				让用户和账户的实体类能体现出来一对多的关系
			3、建立两个配置文件
				用户的配置文件
				账户的配置文件
			4、实现配置：
				当我们查询用户时，可以同时得到用户下所包含的账户信息
				当我们查询账户时，可以同时得到账户的所属用户信息
	
		示例：用户和角色
			一个用户可以有多个角色
			一个角色可以赋予多个用户
		步骤：
			1、建立两张表：用户表，角色表
				让用户表和角色表具有多对多的关系。需要使用中间表，中间表中包含各自的主键，在中间表中是外键。
			2、建立两个实体类：用户实体类和角色实体类
				让用户和角色的实体类能体现出来多对多的关系
				各自包含对方一个集合引用
			3、建立两个配置文件
				用户的配置文件
				角色的配置文件
			4、实现配置：
				当我们查询用户时，可以同时得到用户所包含的角色信息
				当我们查询角色时，可以同时得到角色的所赋予的用户信息
## 一对一

##### 查询账户信息和对应的用户信息

```mysql
sql:内连接
select u.*,a.id as aid,a.uid,a.money form user u,account a where a.uid = u.id;
```

#### 需要在对应Account实体类中添加主表的实体类对象User user

#### 然后在对应mapper中添加配置信息

```xml
    <!-- 定义封装account和user的resultMap -->
    <resultMap id="accountUserMap" type="account">
        <id property="id" column="aid"></id>
        <result property="uid" column="uid"></result>
        <result property="money" column="money"></result>
        <!-- 一对一的关系映射：配置封装user的内容-->
        <association property="user" column="uid" javaType="user">
            <id property="id" column="id"></id>
            <result column="username" property="username"></result>
            <result column="address" property="address"></result>
            <result column="sex" property="sex"></result>
            <result column="birthday" property="birthday"></result>
        </association>
    </resultMap>
<!-- 查询所有 -->
    <select id="findAll" resultMap="accountUserMap">
        select u.*,a.id as aid,a.uid,a.money from account a , user u where u.id = a.uid;
    </select>
```

## 一对多

#### 查询用户的所有信息及其之下的所有账户信息

```mysql
select * from user u left outer join account a u.id = a.uid; 
```

#### 需要在对应的User实体类中添加对应的子类关系的List集合

#### 然后在对应mapper中添加配置信息

```xml
<!--一对多 使用左外链接 现在配置resultMap-->
    <resultMap id="UserAccount" type="user">
        <id property="id" column="id"></id>
        <result property="username" column="username"></result>
        <result property="birthday" column="birthday"></result>
        <result property="sex" column="sex"></result>
        <result property="address" column="address"></result>
        <!--处理一对多  配置User对象中的Account映射  ofType是集合所在的类-->
        <collection property="accountList" ofType="account">
            <id property="id" column="aid"></id>
            <result property="uid" column="uid"></result>
            <result property="money" column="money"></result>
        </collection>
    </resultMap>
    <select id="findUserAndAccount" resultMap="UserAccount">
        select * from user u left outer join account a on u.id = a.UID
    </select>
```

## 多对多->拆解为两个一对多

#### 查询对应角色表其中用户表和其多对多 在查询时也应该显示出来

```mysql
sql
select u.* ,r.id rid,r.ROLE_NAME,r.ROLE_DESC
from role r left outer join user_role ur
on ur.rid = r.id
left outer join user u 
on u.id = ur.uid;

```

#### 在role表中添加对应的user集合

#### 创建roleDao.xml

```xml
<!--多对多-->
<mapper namespace="com.hpy.dao.RoleDao">
    <resultMap id="roleMap" type="role">
        <id property="roleid" column="rid"></id>
        <result property="roleName" column="ROLE_NAME"></result>
        <result property="roleDesc" column="ROLE_DESC"></result>
        <collection property="userList" ofType="user">
            <id property="id" column="id"></id>
            <result property="username" column="username"></result>
            <result property="birthday" column="birthday"></result>
            <result property="sex" column="sex"></result>
            <result property="address" column="address"></result>
        </collection>
    </resultMap>
    <select id="findRole" resultMap="roleMap">
        select u.*,r.ID rid,r.ROLE_NAME,r.ROLE_DESC from role r left outer join user_role ur on r.ID = ur.RID
         left outer join user u on ur.UID = u.id;
    </select>
```

#### 查询user表的话和role表相同 只是改变左链接的顺序

```mysql
sql
select u.* ,r.id rid,r.ROLE_NAME,r.ROLE_DESC
from user u left outer join user_role ur
on ur.uid = u.id
left outer join role r 
on r.id = ur.rid;

```

### 1、Mybatis中的延迟加载

​	问题：在一对多中，当我们有一个用户，它有100个账户。
​	      在查询用户的时候，要不要把关联的账户查出来？
​	      在查询账户的时候，要不要把关联的用户查出来？
​		![延迟加载](.\img\mybatis\延迟加载.png)

	      在查询用户时，用户下的账户信息应该是，什么时候使用，什么时候查询的。
	      在查询账户时，账户的所属用户信息应该是随着账户查询时一起查询出来。
	
	什么是延迟加载
		在真正使用数据时才发起查询，不用的时候不查询。按需加载（懒加载）
	什么是立即加载
		不管用不用，只要一调用方法，马上发起查询。
	
	在对应的四种表关系中：一对多，多对一，一对一，多对多
		一对多，多对多：通常情况下我们都是采用延迟加载。 
		多对一，一对一：通常情况下我们都是采用立即加载。 直接查了两张表

### 懒加载实现

#### 1.现在总配置文件中加上 

```xml
<settings>
        <!--  开启懒加载模式      就是只有需要想要的参数才会加载方法-->
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>
```

#### 2.更改dao.xml，通过调用别的dao层的方法来实现一对多懒加载

##### UserDao.xml

```xml

  <!--一对多 实现懒加载-->
    <resultMap id="UserAccountLazyLoad" type="user">
        <id property="id" column="id"></id>
        <result property="username" column="username"></result>
        <result property="birthday" column="birthday"></result>
        <result property="sex" column="sex"></result>
        <result property="address" column="address"></result>
        <!--处理一对多  配置User对象中的Account映射  ofType是集合所在的类注意加上culumn要和User中的对应 -->
        <collection property="accountList" column="id" ofType="account" select="com.hpy.dao.AccountDao.findByUid"></collection>
    </resultMap>
    <select id="findUserAndAccountLazyLoad" resultMap="UserAccountLazyLoad">
        select * from user
    </select>
```

##### AccountDao.xml

```xml
    <select id="findByUid" resultType="account" parameterType="int">
        select * from account where uid = #{id}
    </select>
```

### 2、Mybatis中的缓存



#### 	什么是缓存

​		存在于内存中的临时数据。

#### 	为什么使用缓存

​		减少和数据库的交互次数，提高执行效率。
​	什么样的数据能使用缓存，什么样的数据不能使用

#### 		适用于缓存：

​			经常查询并且不经常改变的。
​			数据的正确与否对最终结果影响不大的。

#### 		不适用于缓存：

​			经常改变的数据
​			数据的正确与否对最终结果影响很大的。
​			例如：商品的库存，银行的汇率，股市的牌价。

#### 	Mybatis中的一级缓存和二级缓存

##### 		一级缓存：

​			它指的是Mybatis中SqlSession对象的缓存。
​			当我们执行查询之后，查询的结果会同时存入到SqlSession为我们提供一块区域中。
​			该区域的结构是一个Map。当我们再次查询同样的数据，mybatis会先去sqlsession中
​			查询是否有，有的话直接拿出来用。
​			当SqlSession对象消失时，mybatis的一级缓存也就消失了。
​		一级缓存是 SqlSession 范围的缓存，当调用 SqlSession 的修改，添加，删除，commit()，close()等方法时，就会清空一级缓存。 

##### 		二级缓存:

​		二级缓存是 mapper 映射级别的缓存，多个 SqlSession 去操作同一个 Mapper 映射的 sql 语句，多个 SqlSession 可以共用二级缓存，二级缓存是跨 SqlSession 的。 

​		它指的是Mybatis中SqlSessionFactory对象的缓存。由同一个SqlSessionFactory对象创建的SqlSession共享其缓存。

​	

```xml
	二级缓存的使用步骤：
			第一步：让Mybatis框架支持二级缓存（在SqlMapConfig.xml中配置）
			<setting name="cacheEnable" value="true"/>
			第二步：让当前的映射文件支持二级缓存（在IUserDao.xml中配置）
			<cache/>
			第三步：让当前的操作支持二级缓存（在select标签中配置）
			useCache="true"
```

#### 注意：在多表查询中会出现无法及时更新的情况，多表查询尽量避免使用

### 3、Mybatis中的注解开发

​	环境搭建
​	单表CRUD操作（代理Dao方式）
​	多表查询操作
​	缓存的配置