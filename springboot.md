# springBoot-约定大于配置

# 微服务

```
微服务是一种架构风格，它要求我们在开发一个应用的时候，这个应用必须构建成一系列小服务的组合；
可以通过http的方式进行互通。
```

### 微服务架构

all in one的架构方式，我们把所有的功能单元放在一个应用里面。然后我们把整个应用部署到服务器上。如果负载能力不行，我们将整个应用进行水平复制，进行扩展，然后在负载均衡。

所谓微服务架构，就是打破之前all in one的架构方式，把每个功能元素独立出来。把独立出来的功能元素的动态组合，需要的功能元素才去拿来组合，需要多一些时可以整合多个功能元素。所以微服务架构是对功能元素进行复制，而没有对整个应用进行复制。

这样做的好处是：

（1）节省了调用资源。

（2）每个功能元素的服务都是一个可替换的、可独立升级的软件代码。

# springBoot入门

### 1.开始

1.使用idea创建springBoot项目

2.可以再**resources**下新建一个**banner.txt**替换 服务启动图标

### 2.注解

#### 1.@RestController

可以实现对字符串的返回到前台页面--相当于@ResponseBody

#### 2.@SpringBootApplication

程序的入口 本身就是一个@Component

### 3.原理初探

#### 自动配置：

pom.xml:

- Spring-boot-dependencies :核心依赖在父工程中
- 1.我们在写或者引入一些springboot依赖时，不需要指定版本，因为有版本仓库管理



#### 启动器：

- ```xml
  <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter</artifactId>
  </dependency>
  ```

- SpringBoot的启动场景

- spring-boot-starter-web 导入所有web启动的依赖

- 如果要使用什么功能 只需要找到对应启动器就可以了 starter



#### 主程序

- ```java
  //SpringBootApplication 标志是一个spring的应用
  @SpringBootApplication
  public class Springboot01FistApplication {
  
      public static void main(String[] args) {
          SpringApplication.run(Springboot01FistApplication.class, args);
      }
  
  }
  ```

- ```java
  @SpringBootConfiguration:
  		springBoot的配置
        @Configuration
        		@Component
  @EnableAutoConfiguration：自动配置
        @AutoConfigurationPackage 自动配置包
        		@Import({Registrar.class}) 自动配置包 包注册 将主配置类所在的包的所有组件扫描到spring中
        @Import({AutoConfigurationImportSelector.class})导入选择器
        		List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);获取所有配置
        
  ```

- 获取候选的配置

- ```java
  protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
    //getSpringFactoriesLoaderFactoryClass()
    //返回的就是我们最开始看的启动自动导入配置文件的注解类；EnableAutoConfiguration
          List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
          Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
          return configurations;
      }
  ```

  getSpringFactoriesLoaderFactoryClass()--->loadFactoryNames():

  ```java
  public static List<String> loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader) {
          String factoryTypeName = factoryType.getName();
          return (List)loadSpringFactories(classLoader).getOrDefault(factoryTypeName, Collections.emptyList());
      }
  ```

  loadFactoryNames() ---->loadSpringFactories

  ```java
  private static Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) {
          MultiValueMap<String, String> result = (MultiValueMap)cache.get(classLoader);
          if (result != null) {
              return result;
          } else {
              try {
                //从自动装配的配置包中找到 这个spring.factories这个文件转成properties
                  Enumeration<URL> urls = classLoader != null ? classLoader.getResources("META-INF/spring.factories") : ClassLoader.getSystemResources("META-INF/spring.factories");
                  LinkedMultiValueMap result = new LinkedMultiValueMap();
  
                  while(urls.hasMoreElements()) {
                      URL url = (URL)urls.nextElement();
                      UrlResource resource = new UrlResource(url);
                      Properties properties = PropertiesLoaderUtils.loadProperties(resource);
                      Iterator var6 = properties.entrySet().iterator();
  
                      while(var6.hasNext()) {
                          Entry<?, ?> entry = (Entry)var6.next();
                          String factoryTypeName = ((String)entry.getKey()).trim();
                          String[] var9 = StringUtils.commaDelimitedListToStringArray((String)entry.getValue());
                          int var10 = var9.length;
  
                          for(int var11 = 0; var11 < var10; ++var11) {
                              String factoryImplementationName = var9[var11];
                              result.add(factoryTypeName, factoryImplementationName.trim());
                          }
                      }
                  }
  
                  cache.put(classLoader, result);
                  return result;
              } catch (IOException var13) {
                  throw new IllegalArgumentException("Unable to load factories from location [META-INF/spring.factories]", var13);
              }
          }
      }
  ```

  找到spring.factories：里面配置了好多信息

**所以，自动配置真正实现是从classpath中搜寻所有的`META-INF/spring.factories`配置文件 ，并将其中对应的 org.springframework.boot.autoconfigure. 包下的配置项，通过反射实例化为对应标注了 @Configuration的JavaConfig形式的IOC容器配置类 ， 然后将这些都汇总成为一个实例并加载到IOC容器中。**

#### 结论

1. SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值
2. 将这些值作为自动配置类导入容器 ， 自动配置类就生效 ， 帮我们进行自动配置工作；
3. 以前我们需要自己配置的东西 ， 自动配置类都帮我们解决了
4. 整个J2EE的整体解决方案和自动配置都在springboot-autoconfigure的jar包中；
5. **它将所有需要导入的组件以全类名的方式返回 ， 这些组件就会被添加到容器中 ；**
6. 它会给容器中导入非常多的自动配置类 （xxxAutoConfiguration）, 就是给容器中导入这个场景需要的所有组件 ， 并配置好这些组件 ；
7. **有了自动配置类 ， 免去了我们手动编写配置注入功能组件等的工作；**