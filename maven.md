# maven

本地仓库在

 ${user.home}/.m2/repository

 ${user.home}对应的是自己的用户

beiche/.m2/repository

![maven仓库的种类和关系](E:\javaSe学习\javaSeNotes\img\maven仓库的种类和关系.png)

![maven概念模型图](E:\javaSe学习\javaSeNotes\img\maven概念模型图.png)

![](E:\javaSe学习\javaSeNotes\img\maven生命周期.png)

### 标准目录结构

```
src/main/java 核心java代码
src/main/resources 放置配置文件部分
src/test/java 测试代码部分
src/test/resources 测试配置文件
src/main/webapp web资源 js css html 图片等
```

### maven命令

```
mvn clean 清除target 删除本地的编译环境，在自己的电脑上重新编译
mvn compile 编译 生成target 重新编译src/main下的代码
mvn test 编译了src/main 和 src/test代码
mvn package 打包war
mvn install 完成上面的所有操作并且把这个包安装到本地仓库
```

## jar包冲突

```xml
tomcat中的包和maven导入的包冲突解决：
<scope>provided</scope> 只在项目编译的时候起作用



    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
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
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
```

```xml
还有报错 1.8jdk和 tomcat6不兼容  添加
 		<plugin>
          <groupId>org.apache.tomcat.maven</groupId>
          <artifactId>tomcat7-maven-plugin</artifactId>
          <version>2.2</version>
        </plugin>
		<plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <configuration>
            <target>1.8</target>
            <source>1.8</source>
            <encoding>utf-8</encoding>
          </configuration>
        </plugin>
 使用tomcat7:run执行命令
```

