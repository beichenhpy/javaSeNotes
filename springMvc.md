# SpringMvc



![01](./img/springMVC/01.bmp)

# 需求：

![02](./img/springMVC/02.bmp)

# 入门程序编写流程

#### 1.新建一个webapp项目

#### 2.添加archetypeCatalog:internal 键值对 防止下载慢

#### 3.配置pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.hpy</groupId>
  <artifactId>springMVC01start</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>springMVC01start Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <spring.version>5.0.2.RELEASE</spring.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>

  <build>
    <finalName>springMVC01start</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

#### 4.新建springMVC的配置文件 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.hpy"/>
    <!--视图解析器-->

    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--目录-->
        <property name="prefix" value="/WEB-INF/pages/"/>
        <!--后缀名-->
        <property name="suffix" value=".jsp"/>
    </bean>
    <!--开启springMVC注解支持-->
    <mvc:annotation-driven/>
</beans>
```

#### 5.配置web.xml

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  
  <!--配置前端控制器-->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--加载spring ioc的配置文件-->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:SpringMVC.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>

```

#### 6.新建Controller

```java
package com.hpy.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * 控制器类
 */
@Controller
public class helloController {

    @RequestMapping("/hello")
    public String sayHello(){
        System.out.println("hello SpringMVC");
      //return 相当于跳转到success.jsp这个网页 具体位置和后缀在spring ioc.xml配置
        return "success";
    }
}

```

#### 7.index.jsp success.jsp

```jsp
index.jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>hello springMVC</title>
</head>
<body>
<h3>入门程序</h3>
<a href="hello">入门程序</a>
</body>
</html>


success.jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>跳转成功</title>
</head>
<body>
<h3>跳转成功</h3>
</body>
</html>
```

# RequestMapping注解

```java
1. RequestMapping注解的作用是建立请求URL和处理方法之间的对应关系 
2. RequestMapping注解可以作用在方法和类上
		1. 作用在类上:第一级的访问目录
		2. 作用在方法上:第二级的访问目录
		3. 细节:路径可以不编写 / 表示应用的根目录开始
		4. 细节:${ pageContext.request.contextPath }也可以省略不写，但是路径上不能写 /
3. RequestMapping的属性
1. path 指定的路径url
2. value 和path一样
3. mthod 该方法的请求方式 //RequestMethod.POST 枚举类
4. params 限定参数请求条件 //params = {"username"}
5. headers 发送的请求中必须包含的请求头
```

# 请求参数绑定

```java
1. 请求参数的绑定说明 
		1. 绑定机制
					1. 表单提交的数据都是k=v格式的 username=haha&password=123
					2. SpringMVC的参数绑定过程是把表单提交的请求参数，作为控制器中方法的参数进行绑定的 
					3. 要求:提交表单的name和参数的名称是相同的
		2. 支持的数据类型
					1. 基本数据类型和字符串类型
					2. 实体类型(JavaBean)
					3. 集合数据类型(List、map集合等)
2. 基本数据类型和字符串类型
		1. 提交表单的name和参数的名称是相同的
		2. 区分大小写
		3. 实体类型(JavaBean)
					1. 提交表单的name和JavaBean中的属性名称需要一致
					2. 如果一个JavaBean类中包含其他的引用类型，那么表单的name属性需要编写成:对象.属性 例如:
																																							address.name
```

### 解决request 中文乱码

```xml
 <!--在web.xml中添加-->
<!-- 配置过滤器，解决中文乱码的问题 --> <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-
class>
<!-- 指定字符集 --> <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

### 数据传送

```java
package com.hpy.controller;

import com.hpy.domain.Account;
import com.hpy.domain.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * 请求参数绑定
 */
@Controller
@RequestMapping("/param")
public class ParamsController {
    @RequestMapping(path = "/testParam")
    public String testParams(String username,String password){
        System.out.println("执行了。。"+username+" : "+password);
        return "success";
    }

    /**
     * 数据封装到User bean类中
     * @param user bean user
     * @return string
     */
    @RequestMapping(path = "/testBean",method = RequestMethod.POST)
    public String testBean(User user){
        System.out.println("执行了。。"+user);
        return "success";
    }

    /**
     * 数据封装到User bean类中
     * @param account bean account
     * @return string
     */
    @RequestMapping(path = "/testBeanList",method = RequestMethod.POST)
    public String testBeanList(Account account){
        System.out.println("执行了。。"+account);
        return "success";
    }
}

```

```jsp
<%--
  Created by IntelliJ IDEA.
  User: beichen
  Date: 2020-02-13
  Time: 14:40
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>请求参数绑定</title>
</head>
<body>
<a href="param/testParam?username=hpy&password=1234">请求参数绑定</a>
<%--表单--%>
<%--<form action="param/testBean" method="post">
   姓名 <input type="text" name="username"><br>
   密码 <input type="password" name="password"><br>
   年龄 <input type="text" name="age"><br>
 账户id <input type="text" name="account.id"><br>
   金额 <input type="text" name="account.money"><br>
       <input type="submit" value="提交bean">
</form>--%>
<br>
account实体类
<form action="param/testBeanList" method="post">
    姓名 <input type="text" name="users[0].username"><br>
    密码 <input type="password" name="users[0].password"><br>
    年龄 <input type="text" name="users[0].age"><br>
    账户id <input type="text" name="id"><br>
    金额 <input type="text" name="money"><br>
    <input type="submit" value="提交beanList">
</form>
</body>
</html>

```

### 自定义类型转换

```xml
 <!--配置自定义类型转换器-->
    <bean id="factoryBean" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="com.hpy.utils.StringToDateConvert"/>
            </set>
        </property>
    </bean>
    <!--开启springMVC注解支持-->
    <mvc:annotation-driven conversion-service="factoryBean"/>
```

```java
public class StringToDateConvert implements Converter<String, Date> {
    /**
     *
     * @param s 传入的字符串
     * @return 返回date
     */
    @Override
    public Date convert(String s) {
        if (s == null){
            throw new RuntimeException("请你传入参数");
        }
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
        try {
            return df.parse(s);
        } catch (Exception e) {
            throw new RuntimeException("你的数据格式不正确");
        }
    }
}
```

### 想用原生Servlet直接在参数中加上HttpServletRequest就行

### 常用注解：

#### 1.RequestParam

```java
作用：
			把请求中指定名称的参数给控制器中的形参赋值
属性：
			value:请求参数的名称
			required:请求参数是否必须提供此参数，默认true 
@RequestMapping("/requestParam")
    public String requestParam(@RequestParam(value = "username",required = true) String name, String age){
        System.out.println(name + age);
        return "success";
    }
```

#### 2.RequestBody

```java
1. 作用:用于获取请求体的内容(注意:get方法不可以)  得到 key=value&key=value形式
2. 属性
		1. required:是否必须有请求体，默认值是true
@RequestMapping("/requestBody")
    public String requestBody(@RequestBody String body){
        System.out.println(body);
        return "success";
    }
```

####  3.PathVariable

```java
1. 作用:拥有绑定url中的占位符的。例如:url中有/delete/{id}，{id}就是占位符 
2. 属性
			1. value:指定url中的占位符名称
3. Restful风格的URL
		1. 请求路径一样，可以根据不同的请求方式去执行后台的不同方法 
		2. restful风格的URL优点
				1. 结构清晰 2. 符合标准 3. 易于理解 4. 扩展方便
 @RequestMapping("/findUser/{id}")
    public String pathVariable(@PathVariable(value = "id") String id){
        System.out.println(id);
        return "success";
    }
```

#### 4.RequestHeader

```java
1. 作用:获取指定请求头的值 
2. 属性
		1. value:请求头的名称
@RequestMapping("/requestHeader")
    public String requestHeader(@RequestHeader(value = "accept") String header){
        System.out.println(header);
        return "success";
    }
```

#### 5.CookieValue

```java
1. 作用:用于获取指定cookie的名称的值 
2. 属性
		1. value:cookie的名称
@RequestMapping("/cookiesValue")
    public String cookiesValue(@CookieValue("JSESSIONID") String cookies){
        System.out.println(cookies);
        return "success";
    }
```

#### 6.ModelAttribute

```java
1. 作用
			1. 出现在方法上:表示当前方法会在控制器方法执行前线执行。
			2. 出现在参数上:获取指定的数据给参数赋值。
2. 应用场景
			1. 当提交表单数据不是完整的实体数据时，保证没有提交的字段使用数据库原来的数据
//两种情况 一种是有返回值
 @ModelAttribute
    public User showUser(String username){
        System.out.println("我先执行了。。");
        /*
         * 模拟查询数据库,有返回值
         */
        Account account = new Account();
        User user = new User();
        user.setUsername(username);
        user.setAge(20);
        user.setPassword("1234");
        user.setAccount(account);
        user.setDate(new Date());
        return user;
    }
 @RequestMapping("/testModelAttribute")
    public String testModelAttribute(User user){
        System.out.println("执行了。。"+user);
        return "success";
    }



//第二种 没有返回值 使用map 存储
@ModelAttribute
   public void showUser(String username, Map<String,User> userMap){
       System.out.println("我先执行了。。");
       Account account = new Account();
       User user = new User();
       user.setUsername(username);
       user.setAge(20);
       user.setPassword("1234");
       user.setAccount(account);
       user.setDate(new Date());
       userMap.put("user",user);
   }
 @RequestMapping("/testModelAttribute")
    public String testModelAttribute(@ModelAttribute("user") User user){
        System.out.println("执行了。。"+user);
        return "success";
    }
```

#### 7.SessionAttribute

```java
1. 作用:用于多次执行控制器方法间的参数共享 //只能作用于类上
2. 属性
				1. value:指定存入属性的名称 
@SessionAttributes(value = {"msg"})
public class annoController {
@RequestMapping("/testSessionAttribute")
    public String testSessionAttribute(Model model){
        model.addAttribute("msg","我是信息");
        return "success";
    }
@RequestMapping("/testGetSessionAttribute")
   public String testGetSessionAttribute(ModelMap modelMap){
       Object msg = modelMap.get("msg");
       System.out.println(msg);
       return "success";
   }
}
```

# 响应数据和结果视图

### 1.返回值分类

#### 1.返回值为String

```java
@RequestMapping("/testString")
    public String testString(Model model){
        System.out.println("方法执行了");
        //模拟从数据库查询到一个对象
        User user = new User();
        user.setUsername("张三");
        user.setPassword("1234");
        user.setAge(30);
        model.addAttribute("user",user);
        return "success";
    }
```

#### 2.返回值为void

```java
@RequestMapping("/testVoid")
    public void testVoid(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("方法执行了");
        //转发
        //request.getRequestDispatcher("/WEB-INF/pages/success.jsp").forward(request,response);
        //重定向
        //response.sendRedirect(request.getContextPath()+"/index.jsp");
        //直接响应
        response.setContentType("text/html;charset=utf-8");
        response.getWriter().write("你好");
    }
```

#### 3.返回值是ModelAndView对象

```java
 @RequestMapping("/testModelAndView")
    public ModelAndView testVoid() {
        //模拟从数据库查询到一个对象
        User user = new User();
        user.setUsername("张三");
        user.setPassword("1234");
        user.setAge(30);
        ModelAndView modelAndView = new ModelAndView();
        //添加到request
        modelAndView.addObject("user",user);
        //跳转到那个页面 通过视图解析器
        modelAndView.setViewName("success");
        return modelAndView;
    }
```

### 2.转发和重定向 关键字 forward ,redirect

```java
  @RequestMapping("/testForwardRedirect")
    public String testForwardRedirect() {
        //转发 不能用视图解析器
        //return "forward:/WEB-INF/pages/success.jsp";
        //重定向
        return "redirect:/index.jsp";
    }
```

### 3.ResponseBody 响应json数据

##### 【注意：由于DispatcherServlet把所有/下的路径静态资源全都拦截】

#### 1.在springMVC.xml中添加（在webapp下新建 js css images文件夹 ）

```xml
  <mvc:resources location="/css/" mapping="/css/**"/> <!-- 样式 -->
    <mvc:resources location="/images/" mapping="/images/**"/> <!-- 图片 -->
    <mvc:resources location="/js/" mapping="/js/**"/> <!-- javascript -->
```

#### 2.处理json数据 用到Jackson 导入坐标-spring自动封装

```xml
 <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-annotations</artifactId>
      <version>2.9.0</version>
    </dependency>
```

#### 3.使用ajax请求

```js
$(function () {
        $("#btn").click(function () {
            $.ajax({
                type:"post",
                url:"user/testAjax",
                contentType:"application/json;charset=utf-8",
                data:'{"username":"韩鹏宇","password":"1234","age":30}',
                dataType:"json",
                success:function (data) {
                    alert(data.username);
                }
            });
        });
    });
```

#### 4.服务器反馈

```java
@RequestMapping("/testAjax")
    public @ResponseBody User testAjax(@RequestBody User user){
        //模拟数据库查数据
        user.setAge(40);
        return user;
    }
```

# 文件上传

### 1.传统模式：

#### 1.导入环境 坐标

```xml
 
		<dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.3.1</version>
    </dependency>
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.4</version>
		</dependency>
```

#### 2.前端页面编写

```jsp
 
<h3>文件上传</h3>
<form action="user/fileupload" method="post" enctype="multipart/form-data"> 
  选择文件:<input type="file" name="upload"/><br/>
	<input type="submit" value="上传文件"/>
</form>
```

```java
@RequestMapping("/upload1")
    public String testUpload(HttpServletRequest request) throws Exception {
        //1.获得文件路径
        String path = request.getSession().getServletContext().getRealPath("/uploads/");
        File file = new File(path);
        if (!file.exists()){
            file.mkdirs();
        }
        //解析request对象
        DiskFileItemFactory factory = new DiskFileItemFactory();
        ServletFileUpload upload = new ServletFileUpload(factory);
        //解析
        List<FileItem> files = upload.parseRequest(request);
        for (FileItem item : files) {
            //判断是否为上传文件项目
            if (!item.isFormField()){
                //是上传文件项
                String filename = item.getName();
                //设置文件名唯一值
                String uuid = UUID.randomUUID().toString().replace("-", "");
                filename += "_"+uuid;
                //完成上传
                item.write(new File(path,filename));
                //删除临时文件
                item.delete();
            }
        }
        return "success";
    }
```

### 2。springMVC方法

#### 1.先配置文件解析器 在springMVC.xml中

```xml
 
<!-- 配置文件解析器对象，要求id名称必须是multipartResolver --> <bean id="multipartResolver"
class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="10485760"/>
</bean>
```

#### 2控制器

```java
@RequestMapping("/upload2")
    public String testUpload2(HttpServletRequest request, MultipartFile upload) throws Exception {
        //1.获得文件路径
        String path = request.getSession().getServletContext().getRealPath("/uploads/");
        File file = new File(path);
        if (file.exists()){
            file.mkdirs();
        }
        String filename = upload.getOriginalFilename();
        //设置文件名唯一值
        String uuid = UUID.randomUUID().toString().replace("-", "");
        filename += "_"+uuid;
        upload.transferTo(new File(file,filename));
        return "success";
    }
```

### 3.跨服务器传文件

#### 1.导入配置坐标

```xml
<dependency>
    <groupId>com.sun.jersey</groupId>
    <artifactId>jersey-core</artifactId>
    <version>1.18.1</version>
</dependency>
<dependency>
    <groupId>com.sun.jersey</groupId>
    <artifactId>jersey-client</artifactId>
    <version>1.18.1</version>
</dependency>
```

#### 2.jsp界面

```jsp
 <h3>跨服务器的文件上传</h3>
<form action="user/fileupload3" method="post" enctype="multipart/form-data"> 选择文件:<input type="file" name="upload"/><br/>
<input type="submit" value="上传文件"/>
</form>
```

#### 3.控制器

```java
/**
* SpringMVC跨服务器方式的文件上传 *
* @param request
* @return
* @throws Exception
*/
@RequestMapping(value="/fileupload3")
 public String fileupload3(MultipartFile upload) throws Exception { 
   System.out.println("SpringMVC跨服务器方式的文件上传...");
	// 定义图片服务器的请求路径
	String path = "http://localhost:9090/day02_springmvc5_02image/uploads/";
	// 获取到上传文件的名称
	String filename = upload.getOriginalFilename();
	String uuid = UUID.randomUUID().toString().replaceAll("-", "").toUpperCase(); // 把文件的名	称唯一化
	filename = uuid+"_"+filename;
	// 向图片服务器上传文件
	// 创建客户端对象
	Client client = Client.create();
	// 连接图片服务器
	WebResource webResource = client.resource(path+filename); // 上传文件
	webResource.put(upload.getBytes());
	return "success";
}
```

# 异常处理

### 1.异常处理思路

```
Controller调用service，service调用dao，异常都是向上抛出的，最终有DispatcherServlet找异常处理器进 行异常的处理。
```

### 2.springMVC的异常处理

#### 1.自定义异常类

```java
package com.hpy.domian;

public class SysException extends Exception{
    private static final long serialVersionUID = 4055945147128016300L;
    // 异常提示信息
    private String message;

    public SysException(String message) {
        this.message = message;
    }

    @Override
    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}

```

#### 2.自定义异常处理器

```java
package com.hpy.Exception;

import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class SysExceptionResolver implements HandlerExceptionResolver {
    /**
     * 自定义异常处理器
     * @param httpServletRequest
     * @param httpServletResponse
     * @param handler
     * @param ex
     * @return
     */
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object handler, Exception ex) {
        SysException e = null;
        if (ex instanceof SysException){
            e = (SysException) ex;
        }else{
            e = new SysException("系统正在维护");
        }
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg",e.getMessage());
        mv.setViewName("erros");
        return mv;
    }
}

```

#### 3.配置异常处理器

```xml
<!--配置异常处理器-->
    <bean id="sysExceptionResolver" class="com.hpy.Exception.SysExceptionResolver"/>
```

#### 4.controller代码

```java
@RequestMapping("/testException")
    public String testException() throws SysException {
        try {
            int i = 10/0;
        } catch (Exception e) {
            //控制台打印异常信息
            e.printStackTrace();
            throw new SysException("查询所有用户的错误");
        }
        return "success";
    }
```

# 拦截器

```
1. SpringMVC框架中的拦截器用于对处理器进行预处理和后处理的技术。
2. 可以定义拦截器链，连接器链就是将拦截器按着一定的顺序结成一条链，在访问被拦截的方法时，拦截器链中的拦截器会按着定义的顺序执行。
3. 拦截器和过滤器的功能比较类似，有区别
			1. 过滤器是Servlet规范的一部分，任何框架都可以使用过滤器技术。 
			2. 拦截器是SpringMVC框架独有的。
3. 过滤器配置了/*，可以拦截任何资源。
4. 拦截器只会对控制器中的方法进行拦截。
4. 拦截器也是AOP思想的一种实现方式
5. 想要自定义拦截器，需要实现HandlerInterceptor接口。
```

### 1. HandlerInterceptor接口中的方法

```
1. preHandle方法是controller方法执行前拦截的方法
		1. 可以使用request或者response跳转到指定的页面
		2. return true放行，执行下一个拦截器，如果没有拦截器，执行controller中的方法。 
		3. return false不放行，不会执行controller中的方法。
2. postHandle是controller方法执行后执行的方法，在JSP视图执行前。 
		1. 可以使用request或者response跳转到指定的页面
		2. 如果指定了跳转的页面，那么controller方法跳转的页面将不会显示。 
3. postHandle方法是在JSP执行后执行
    1. request或者response不能再跳转页面了
```

### 2.配置拦截器

```xml
 
<!-- 配置拦截器 --> <mvc:interceptors>
<mvc:interceptor>
<!-- 哪些方法进行拦截 -->
<mvc:mapping path="/user/*"/>
<!-- 哪些方法不进行拦截
<mvc:exclude-mapping path=""/>
-->
<!-- 注册拦截器对象 -->
<bean class="com.hpy.Interceptor.MyInterceptor1"/>
    </mvc:interceptor>
</mvc:interceptors>
```

### 3.自定义拦截器类

```java
package com.hpy.Interceptor;

import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyInterceptor1 implements HandlerInterceptor {
    /**
     * controller方法执行前，进行拦截的方法
     * return true放行
     * return false拦截
     * 可以使用转发或者重定向直接跳转到指定的页面。 */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("拦截器执行了...");
        return true;
    }
}
```

# SSM框架整合

### 1.先对spring框架进行配置：

#### 1.先配置pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.hpy</groupId>
    <artifactId>ssm01</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <name>ssm01 Maven Webapp</name>
    <!-- FIXME change it to the project's website -->
    <url>http://www.example.com</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <spring.version>5.0.2.RELEASE</spring.version>
        <slf4j.version>1.6.6</slf4j.version>
        <log4j.version>1.2.12</log4j.version>
        <mysql.version>5.1.6</mysql.version>
        <mybatis.version>3.4.5</mybatis.version>
    </properties>

    <dependencies>
        <!-- spring -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.6.8</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.0</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <!-- log start -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <!-- log end -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis.version}</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.0</version>
        </dependency>
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
            <type>jar</type>
            <scope>compile</scope>
        </dependency>
    </dependencies>

    <build>
        <finalName>ssm01</finalName>
        <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
            <plugins>
                <plugin>
                    <artifactId>maven-clean-plugin</artifactId>
                    <version>3.1.0</version>
                </plugin>
                <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
                <plugin>
                    <artifactId>maven-resources-plugin</artifactId>
                    <version>3.0.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.0</version>
                </plugin>
                <plugin>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>2.22.1</version>
                </plugin>
                <plugin>
                    <artifactId>maven-war-plugin</artifactId>
                    <version>3.2.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-install-plugin</artifactId>
                    <version>2.5.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-deploy-plugin</artifactId>
                    <version>2.8.2</version>
                </plugin>
                <plugin>
                     <groupId>org.apache.maven.plugins</groupId>
                     <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.2</version>
                     <configuration>
                         <target>1.8</target>
                         <source>1.8</source>
                         <encoding>utf-8</encoding>
                     </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>

```

#### 2.然后新建 domain dao service 包 和对应实体类

```java
//新建domain实体类
//新建dao层的接口
//新建service接口和实现类
```

#### 3.配置 applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd
    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!-- 开启注解扫描，要扫描的是service和dao层的注解，要忽略web层注解，因为web层让SpringMVC框架 去管理 -->
    <context:component-scan base-package="com.hpy">
        <!-- 配置要忽略的注解 -->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>


</beans>
```

### 2.对SpringMVC层进行配置

#### 1.先在webapp下新建 css js images 等静态文件夹

#### 2.新建controller层及其类

```java
package com.hpy.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class AccountController {
    @RequestMapping("/findAll")
    public String testFind(){
        return "success";
    }
}
```

#### 3.配置SpringMVC.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">


    <!--扫描Controller-->
    <context:component-scan base-package="com.hpy">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    <!--开启视图解析器-->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- JSP文件所在的目录 -->
        <property name="prefix" value="/WEB-INF/pages/" /> <!-- 文件的后缀名 -->
        <property name="suffix" value=".jsp" />

    </bean>
    <!-- 设置静态资源不过滤 -->
    <mvc:resources location="/css/" mapping="/css/**" />
    <mvc:resources location="/images/" mapping="/images/**" />
    <mvc:resources location="/js/" mapping="/js/**" />
    <!-- 开启对SpringMVC注解的支持 -->
    <mvc:annotation-driven />
</beans>
```

#### 4.配置web.xml--可以读取SpringMVC.xml

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <!--读取springMVC文件-->
      <param-value>classpath:SpringMVC.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>

```

### 3.用Spring整合SpringMVC

#### 【注意】由于启动tomcat时加载的web.xml并没有加载Spring的配置文件，所以就不会加载Spring的注解。下面给出解决办法

#### 1.在web.xml中加入监听器-最终的web.xml

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <!--设置配置文件路径-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>
  <!--过滤器 -->
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

  <!--监听器 用于读spring的配置文件 默认只去加载WEB-INF下的applicationContext.xml-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <!--springMVC-->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <!--读取springMVC文件-->
      <param-value>classpath:SpringMVC.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>


</web-app>

```

### 4.配置Mybaitis框架--使用注解开发

#### 1.配置总配置文件 SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///mybaitis"/>
                <property name="username" value="root"/>
                <property name="password" value="1234"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 使用的是注解 -->
    <mappers>
        <package name="com.hpy.dao"/>
    </mappers>
</configuration>
```

### 5.Spring整合Mybaitis

#### 1.在applicationContext.xml中 加入 Mybatis的总配置文件和事务管理最终的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd
    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!-- 开启注解扫描，要扫描的是service和dao层的注解，要忽略web层注解，因为web层让SpringMVC框架 去管理 -->
    <context:component-scan base-package="com.hpy">
        <!-- 配置要忽略的注解 -->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <!-- 配置C3P0的连接池对象 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver" />
        <property name="jdbcUrl" value="jdbc:mysql:///mybaitis" />
        <property name="user" value="root" />
        <property name="password" value="1234" />
    </bean>
    <!--创建工厂-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--设置扫描的dao下的所有包 就是相当于mapping-->
    <bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.hpy.dao"/>
    </bean>
    <!--事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--事务通知-->
    <tx:advice id= "txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="find*" read-only="true"/>
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>
    <!--aop增强-->
    <aop:config>
     <aop:advisor 
       advice-ref="txAdvice"pointcut="execution(*com.hpy.service.Impl.*ServletImpl.*(..))"/>
    </aop:config>
</beans>
```

