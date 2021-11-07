## SpringBoot

#### 一、简介

##### 1、介绍

​		SpringBoot是Spring家族中的一个全新的框架，它用来简化Srping应用程序的创建和开发过程，也就是说可以简化我们之前采用SpringMVC+Spring+MyBatis框架进行开发的过程。它抛弃了烦琐的xml配置过程，采用大量的默认配置简化我们的开发过程，所以采用SpringBoot可以非常容易和快速地创建基于Spring框架的应用程序。

​		正因为SpringBoot它化繁为简，让开发变得极其简单和快速，所以在业界备受关注，SpringBoot在国内的关注趋势图[Http://t.cn/ROQLquP](Http://t.cn/ROQLquP)。

##### 2、特性

- 能够轻松创建基于Spring的应用程序。
- 能够直接使用java main方法启动内嵌的Tomcat，jetty服务器运行SpringBoot程序，不需要部署war包文件。
- 提供约定的starter POM来简化Maven配置，让maven的配置变得简单。
- 根据项目的maven依赖配置，SpringBoot自动配置Spring、Spring mvc等。
- 提供了程序的健康检查等功能。
- 基本可以完全不使用xml配置文件，采用注解配置。

##### 3、四大核心

- 自动配置：针对很多Spring应用程序和常见的应用功能，SpringBoot能自动提供相关配置。
- 起步依赖：告诉SpringBoot需要什么功能，他就能引入需要的依赖库。
- Actuator：让你能够深入运行中的SpringBoot应用程序，一探SpringBoot程序的内部信息。
- 命令行界面：这是SpringBoot的可选特性，主要针对Groovy语言使用。

注：Groovy是一种基于JVM（java虚拟机）的敏捷开发语言。它结合了python、Ruby和Smalltalk的许多强大特性，Groovy代码能够与java代码很好的结合，也能用于扩展现有代码。由于其运行在JVM上的特性，Groovy可以使用其他java语言编写的库。

#### 二、开发环境

1、推荐使用Spring Boot最新版本。

2、如果是使用eclipse，推荐安装Spring Tool Suite（STS）插件。

3、如果是使用IDEA，自带了SpringBoot插件。

4、推荐使用Maven3.2。

5、推荐使用java8。

#### 三、HelloWorld

##### 1、创建SpringBoot项目

- Spring Initializr插件生成
- Maven项目配置

##### 2、添加依赖

~~~xml
<!-- 1、springboot父级依赖 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.9.RELEASE</version>
    <relativePath/>
</parent>
<!-- 2、springboot的web起步依赖 -->
<dependency >
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency >
~~~

##### 3、main入口

创建SpringbootApplication.java

~~~java
//开启springboot自动配置
@SpringBootApplication
public class SpringbootApplication{
    public static void main(String[] args){
        //启动了springboot程序，启动spring容器，启动内嵌的tomcat
        SpringApplication.run(SpringbootApplication.class,args);
    }
}
~~~

##### 4、访问Controller

> 1、创建HelloWorldController.java

~~~java
@Controller
public class HelloWorldController {
    @RequestMapping("/boot/hello")
    public String helloWorld(){
        return "Hello Spring Boot";
    }
}
~~~

> 2、运行main方法

> 3、访问http://localhost:8080/boot/hello

#### 四、核心配置文件

##### 1、两种格式的配置文件。

- application.properties

  - key-value形式

  - 例如：

  - ~~~properties
    server.port=8080
    server.context-path=/springboot-demo
    ~~~

- application.yml

  - 类似json对象形式

  - 例如：

  - ~~~yml
    server:
    	port: 900
    	context-path: /springboot-demo
    ~~~

  - 注意冒号后的空格、换行

##### 2、多配置文件激活

- 规定主配置文件名为application
- 规定其他配置文件名为application-xxx

~~~properties
#主配置文件application
#激活application-test配置文件
spring.profiles.active=test
~~~

##### 3、自定义配置

写入属性值

~~~properties
boot.name=bootName
~~~

方式一：注入获取

~~~java
@Value("${boot.name}")
private String name;
~~~

方式二：配置类获取

~~~java
@Component
@ConfigurationProperties(prefix="boot")
public class myConfig{
    private String name;
    public String getName(){
    	return name;
    }
    public void setName(String name){
    	this.name = name;
    }
}
~~~

方式三：Environment获取

~~~java
@Component
public class myConfig{
    @Autowired
	private Environment environment;
    
    public void getName(String key){
    	String name = environment.getProperty(key);
    }
}
~~~

#### 五、SpringMVC注解

- @Controller：即为Springmvc的注解，处理http请求。
- @RestController：Spring4后新增注解。是@Controller与@ResponseBody的组合注解，用于返回字符串或json数据。
- @GetMapping：@RequestMapping和Get请求方法的组合。
- @PostMapping：@RequestMapping和Post请求方法的组合。
- @PutMapping：@RequestMapping和put请求方法的组合。
- @DeleteMapping：@RequestMapping和Delete请求方法的组合。

#### 六、使用JSP

##### 1、添加依赖

~~~xml
<!-- 1、引入Springboot内嵌的tomcat对jsp的解析包 -->
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
<!-- 2、servlet依赖的jar包start-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
</dependency>
<!-- 3、jsp依赖jar包start-->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>javax.servlet.jsp-api</artifactId>
    <version>2.3.1</version>
</dependency>
<!-- 4、jstl标签依赖jar包start-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>
~~~

##### 2、配置视图

~~~properties
spring.mvc.view.prefix=/
spring.mvc.view.suffix=.jsp
~~~

##### 3、webapp资源目录

~~~
# 项目路径例图
|-src
	|-main
        |-java
        |-resources
        |-webapp
            |-index.jsp
# target路径，编译后的路径
|-classes
	|-com(包路径)
	|-META-INF
		|-resources
			|-index.jsp
~~~

配置pom，添加静态资源路径，确保编译后找得到

~~~xml
<resources>
    <!-- 1、主要防止集成mybatis时，mapper配置文件找不到-->
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.xml</include>
        </includes>
    </resource>
    <!-- 2、主要防止静态资源文件找不到-->
    <resource>
        <directory>src/main/resources</directory>
        <includes>
            <include>**/*.*</include>
        </includes>
    </resource>
    <!-- 3、指定新建的webapp目录到目标编译目录下-->
    <resource>
        <directory>src/main/webapp</directory>
        <targetPath>META-INF/resources</targetPath>
        <includes>
            <include>**/*.*</include>
        </includes>
    </resource>
</resources>
~~~

#### 七、集成Mybatis

##### 1、添加依赖

~~~xml
<!-- 1、加载mybatis整合springboot-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.1</version>
</dependency>
<!-- 2、Mysql的jdbc驱动包-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
~~~

##### 2、配置核心文件

~~~properties
# application.properties
# 指定mapper文件位置
mybatis.mapper.location=classpath:com/lc/springboot/mapper/*.xml
# 配置数据源
spring.datasource.username=root
spring.datasource.password=123456
# mysql8以下版本配置
#spring.datasource.driver-class-name=com.mysql.jdbc.Driver
#spring.datasource.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8&userSSL=false
# mysql8以上版本配置
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/test?characterEncoding=utf8&useSSL=false&serverTimezone=UTC&rewriteBatchedStatements=true
~~~

##### 3、扫描mapping接口

两种方式：

- 接口类注解：@Mapper
- 主类注解：@MapperScan(basePackages = "com.lc.springboot.mapper")

##### 4、事务支持

- 主类注解：@EnableTransactionMannagement
- service类注解使用：@Transactionl

##### 5、逆向工程

~~~xml
<!-- mybatis代码自动生成插件（逆向工程）build>plugins下 -->
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.6</version>
    <configuration>
        <configurationFile>GeneratorMapper.xml</configurationFile>
        <verbose>true</verbose>
        <overwrite>true</overwrite>
    </configuration>
</plugin>
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator=config_1_0.dtd">
<generatorConfiguration>
    <!--指定连接数据库的JDBC驱动包所在位置，指定到你本机的完整路径-->
    <classPathEntry location="E:/java/jar包/web/mysql-connector-java-5.1.39-bin.jar"/>
    <!--配置table表信息内容体，targetRuntime指定采用MyBatis3的版本-->
    <context id="tables" targetRuntime="MyBatis3">
        <!--抑制生成注释，由于生成的注解都是英文的，可以不让它生成-->
        <commentGenerator>
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>
        <!--配置数据库连接信息-->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://127.0.0.1:3306/test"
                        userId="root"
                        password="123456">
        </jdbcConnection>
        <!--生成model类，targetPackage指定model类的包名，targetProject指定生成的model放在eclipse的哪个工程下面-->
        <javaModelGenerator targetPackage="com.lc.com.lc.springboot.entity" targetProject="src/main/java">
            <property name="enableSubPackages" value="false"/>
            <property name="trimStrings" value="false"/>
        </javaModelGenerator>
        <!--生成MyBatis的Mapper.xml文件，targetPackage指定mapper.xml文件的包名，targetProject指定生成的mapper.xml放在-->
        <sqlMapGenerator targetPackage="com.lc.com.lc.springboot.mapper" targetProject="src/main/java">
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>
        <!--生成MyBatis的Mapper接口类文件，targetPackage指定接口类的包名，targetProject指定生成的Mapper接口放在eclipse的哪个工程下面-->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.lc.com.lc.springboot.mapper" targetProject="src/main/java">
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>

        <!--数据库表名及对应的java模型类名-->
        <table tableName="student"
               domainObjectName="Student"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>
    </context>
</generatorConfiguration>
~~~

#### 八、实现REST风格

​		RESTFull一种互联网软件架构设计的风格，但它并不是标准，它只是提出了一组客户端和服务器交互时的架构理念和设计原则，基于这种理念和原则设计的接口可以更简洁，更有层次。

​		任何的技术都可以实现这种理念。REST这个词，是Roy Thomas Fielding在他2000年的博士论文中提出的。如果一个架构符合REST原则，就称它为RESTFull架构。

> 例如：

- 我们要访问一个http接口：http://localhost:80080/api/order?id=1521&status=1
- 采用RESTFull风格则http地址为：http://localhost:80080/api/order/1521/1

> 常用rest注解：

- @PathVariable：获取url中的数据。该注解是实现RSTFulll最主要的一个注解。
- @PostMapping：添加。接收和处理Post方式的请求。
- @DeleteMapping：删除。接收delete方式的请求，可以使用@GetMapping代替。
- @PutMapping：修改。接收put方式的请求，可以用@PostMapping代替。
- @GetMapping：查询。接收get方式的请求。

#### 九、热部署插件

​		在实际开发中，我们修改某些代码逻辑功能或页面都需要重启应用，这无形中降低了开发效率。热部署是指当我们修改代码后，服务器自动重启加载新修改的内容，这样大大提高了我们开发的效率。Springboot热部署通过添加一个插件实现。插件为：sping-boot-devtools，在maven中配置如下：

~~~xml
<!-- spingboot 开发自动热部署-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
~~~

该热部署插件在实际使用中会有一些小问题，明明已经重启，但没有生效，这种情况下，手动重启一下程序。

#### 十、集成Redis

##### 1、添加依赖

~~~xml
<!-- 加载springboot redis包-->
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~

##### 2、核心配置文件

~~~properties
spring.redis.port=6379
spring.redis.password=123456
spring.redis.host=localhost

#redis集群中的哨兵模式配置
#spring.redis.password=123456
#spring.redis.sentinel.master=mymaster
#spring.redis.sentinel.nodes=192.168.106.128:26380,192.168.106.128:26380
~~~

##### 3、自动装配

~~~java
//service
//泛型<Object, Object>或<String, String>
@Autowired
private RedisTemplate<Object, Object> redisTemplate;
~~~

#### 十一、集成Dubbo

~~~
# 创建三个项目（Module）
# 服务接口项目
spring-dubbo-interface
# 服务提供者项目
spring-dubbo-provider
# 服务消费者项目
spring-dubbo-consumer
~~~

##### 1、开发Dubbo服务接口

~~~
|-com.lc.springboot
				|-model # 实体类包
				|-service # service接口包
~~~

> 1、service接口类

~~~java
public interface StudentService {
    public String sayHi (String name);
}
~~~

> 2、利用maven的plugins-->install生成jar包

##### 2、开发Dubbo服务提供者

~~~
|-com.lc.springboot
				|-service.impl # service实现包
				|-SpringbootDubboProviderApplication.java # 主类
~~~

> 1、添加依赖

~~~xml
<!--springboot的父级依赖、起步依赖、测试依赖-->
<!-- 集成dubbo起步依赖 -->
<dependency>
    <groupId>com.alibaba.spring.boot</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>1.0.0</version>
</dependency>
<!-- 
	使用到zookeeper作为注册中心
	需要zookeeper的客户端jar
-->
<dependency>
    <groupId>com.101tec</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.10</version>
    <!-- 过滤掉log4j的依赖包 -->
    <exclusions>
        <exclusion>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<!-- dubbo接口项目的jar依赖 -->
<dependency>
    <groupId>com.lc.springboot</groupId>
    <artifactId>com.lc.springboot-dubbo-interface</artifactId>
    <version>1.0.0</version>
</dependency>
~~~

> 2、核心配置文件

~~~properties
server.port=8080 # web服务端口
spring.dubbo.appname=com.lc.springboot-dubbo-provider # dubbo应用名
spring.dubbo.registry=zookeeper://192.168.230.128:2181 # 注册中心服务的地址
~~~

> 3、service实现类

~~~java
@Component
@Service() //com.alibaba.dubbo.config.annotation.Service
public class UserServiceImpl implements StudentService {
    @Autowired
    private StudentMapper studentMapper;
    @Override
    public String sayHi(String name) {
        return "Hi, SpringBoot :" + name;
    
}
~~~

> 4、主类注解：@EnableDubboConfiguration //开启dubbo的自动配置

##### 3、开发Dubbo服务消费者

~~~
|-com.lc.springboot
				|-controller # 请求控制包
				|-SpringbootDubboConsumerApplication.java # 主类
~~~

> 1、添加依赖与提供者一致

> 2、核心配置文件

~~~properties
#内嵌的tomcat服务端口
server.port=8080
#dubbo配置
spring.dubbo.appname=com.lc.springboot-dubbo-consumer
spring.dubbo.registry=zookeeper://192.168.160.128:2181
~~~

> 3、controller

~~~java
import com.alibaba.dubbo.config.annotation.Reference;
@Controller
public class StudentController{
    @Reference //使用dubbo的注解引用远程的dubbo服务
    private StudentService studentService;
    @RequestMapping("/sayHi”)
    public @ResponseBody String sayHi(){
    	return studentService.sayHi("spring boot dubbo......”);
    }
}
~~~

> 4、主类注解：@EnableDubboConfiguration //开启dubbo的自动配置

#### 十二、Interceptor

##### 1、创建一个拦截器类

~~~java
public class LoginInterceptor  implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        System.out.println("已经进入了登录拦截器");
        //拦截逻辑代码
        return true;//通过
    }
}
~~~

##### 2、创建拦截器配置类

~~~java
@Configuration //配置类注解
public class WebConfig extends WebMvcConfigurerAdapter {
    String[] addPathPatterns = {"/boot/**"};
	String[] excludePathPatterns = {"/boot/hello","/boot/index"};
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //注册登录拦截器
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns(addPathPatterns) //拦截路径
                .excludePathPatterns(excludePathPatterns); //不拦截路径
    }
}
~~~

#### 十三、Servlet

##### 1、注解方式

> 1、创建一个servlet类

~~~java
@WebServlet(urlPatterns = "/myServlet")
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().print("hello world");
        resp.getWriter().flush();
        resp.getWriter().close();
    }
}
~~~

> 2、主类添加servlet扫描

~~~java
@ServletComponentScan(basePackages = "com.lc.springboot.servlet") //servlet组件包扫描
~~~

##### 2、配置类方式

> 1、servlet类同上

> 2、创建配置类

~~~java
@Configuration
public class ServletConfig {
	//相当于spring配置文件里的一个bean
    @Bean
    public ServletRegistrationBean myServletRegistrationBean(){
        ServletRegistrationBean registration = new ServletRegistrationBean(new HeServlet(),"/myServlet");
        return registration;
    }
}
~~~

#### 十四、Filter

##### 1、注解方式

> 1、创建一个filter类

~~~java
@WebFilter(urlPatterns = "/*")
public class MyFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("filter过滤器");
        filterChain.doFilter(servletRequest,servletResponse);//通过
    }
}
~~~

> 2、主类注解包扫描

~~~
@MapperScan(basePackages = {"com.lc.springboot.mapper", "com.lc.springboot.filter"})
~~~

##### 2、配置类方式

> 1、filter类同上

> 2、创建配置类（可以是与servlet同一个配置类）

~~~java
@Configuration
public class ServletConfig {
	@Bean
    public FilterRegistrationBean heFilterRegistrationBean(){
        FilterRegistrationBean registrationBean = 
            				new FilterRegistrationBean(new Hefilter());
        registrationBean.addUrlPatterns("/*");
        return registrationBean;
    }
}
~~~

#### 十五、字符编码Filter

##### 1、配置类方式（Spring提供的）

> 1、创建配置类（可以是与servlet同一个配置类）

~~~java
@Configuration
public class ServletConfig {
	@Bean
    public FilterRegistrationBean filterRegistrationBean(){
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();

        //字符编码过滤器
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();
        characterEncodingFilter.setForceEncoding(true);
        characterEncodingFilter.setEncoding("UTF-8");

        //添加spring提供的传统字符编码过滤器
        filterRegistrationBean.setFilter(characterEncodingFilter);
        filterRegistrationBean.addUrlPatterns("/*");//设置过滤路径，可变长度
        return filterRegistrationBean;
    }
}
~~~

> 2、主类注解包扫描

~~~
@MapperScan(basePackages = {"com.lc.springboot.mapper", "com.lc.springboot.filter"})
~~~

> 3、核心配置文件

~~~properties
# 使springboot的encoding机制失效，这样配置类encoding才有效
spring.http.encoding.enabled=false
~~~

##### 2、springboot自带的

~~~properties
# 核心配置文件
spring.http.encoding.enabled=true
spring.http.encoding.force=true
spring.http.encoding.charset=UTF-8
~~~

#### 十六、java项目

##### 1、方式一

> 1、添加依赖，方式二同样需要

~~~xml
<!-- SpringBoot开发java项目 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId><!--相比springboot起步依赖少了一个web-->
</dependency>
~~~

> 2、主类实现CommandLineRuner接口

~~~java
public class SpringbootJavaApplication implements CommandLineRunner {
	public static void main(String[] args) {
        //启动springboot，启动spring容器
        SpringApplication springApplication = 
            				new SpringApplication(SpringbootJavaApplication.class);
        springApplication.setBannerMode(Banner.Mode.OFF);//关闭spring启动log
        springApplication.run(args);
        //SpringApplication.run(SpringbootJavaApplication.class, args);
    }
    /**
     * 相当于main方法
     * @param args
     */
    @Override
    public void run(String... args) {
        System.out.println("hello world");
    }
}
~~~

##### 2、方式二

~~~java
public static void main(String[] args) {
    	//spring容器
        ConfigurableApplicationContext context = 
            				SpringApplication.run(SpringbootJavaApplication.class, args);
        System.out.println("hello world");
}
~~~

#### 十七、打包部署

##### 1、打war包

> 1、主类继承SpringBootServletInitializer类

~~~java
@SpringBootApplication
public class Application extends SpringBootServletInitializer {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(Application.class);
    }
}
~~~

> 2、项目包

~~~xml
<groupId>com.lc.com.lc.springboot</groupId>
<artifactId>com.lc.springboot-war</artifactId>
<version>1.0.0</version>
<packaging>war</packaging><!--指定war包-->
~~~

> 3、打包插件

~~~xml
<!-- springboot提供的项目编译打包插件 build>plugins下-->
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
~~~

> 4、利用maven的plugins-->install生成war包

> 5、解压到tomcat>wabapp路径下，运行tomcat即可访问

##### 2、打jar包

> 1、打包插件

~~~xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <version>1.4.2.RELEASE</version><!--其他版本有问题-->
</plugin>
~~~

> 2、利用maven的plugins-->install生成jar包

> 3、打包的jar文件内嵌tomcat，运行cmd>java -jar c:/xx.jar访问即可

#### 十八、Thymeleaf

##### 1、认识Thymeleaf

* 模板引擎
  * 是一个技术名词，是跨领域跨平台的概念。
  * 在java语言体系下有模板引擎，在C#、PHP语言体系下也有模板引擎，甚至在javascript中也会用到模板引擎技术。
  * Java生态下的模板引擎有Thymeleaf、Freemaker、Velocity、Beetl（国产）等。
* Thymeleaf模板引擎
  * 是一个流行的模板引擎，该模板引擎采用java语言开发
  * 基于HTML的，以HTML标签为载体，Thymeleaf要寄托在HTML的标签下实现对数据的展示。
  * 拥有显示静态数据和替换更新静态数据的能力，且在web环境和非web环境下都适用。
  * 官方网站：http://www.thymeleaf.org
* SpringBoot集成了thymeleaf模板技术
  * SpringBoot官方推荐使用thymeleaf来代替jsp技术。
  * Thymeleaf是另一种模板技术，它本身并不属于springboot。

##### 2、集成Thymeleaf

> 1、添加依赖

~~~xml
<!-- thymeleaf起步依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
~~~

> 2、核心配置文件

~~~properties
#开发阶段，建议关闭thymeleaf的缓存
spring.thymeleaf.cache=false
#使用遗留的html5以去掉对html标签的效验
spring.thymeleaf.mode=LEGACYHTML5
~~~

> 3、去掉对html标签的效验，则需要添加自动补全的依赖。Thymeleaf要求严格的HTML5格式

~~~xml
<!--NekoHTML是一个java语言的HTML扫描器和标签补全器，这个解析器能够扫描HTML文件并"修改”HTML文档中的常见错误。NekoHTML能增补缺失的父元素，自动用结束标签关闭相应的元素，修复不匹配的内嵌元素标签等。-->
<dependency>
    <groupId>net.sourceforge.nekohtml</groupId>
    <artifactId>nekohtml</artifactId>
</dependency>
<dependency>
    <groupId>org.unbescape</groupId>
    <artifactId>unbescape</artifactId>
    <version>1.1.5.RELEASE</version>
</dependency>
~~~

> 4、Contoller

~~~java
@RequestMapping("/index")
public String index(Model model) {
	model.addAttribute("data","恭喜，Springboot基础Thymeleaf成功！")；
	//return中就是你页面的名字（不带html后缀）
	return "index";
}
~~~

> 5、index.html

~~~html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8"/>
        <tiltle>springboot集成thymeleaf</title>
    </head>
    <body>
        <p th:text="${data}">spring boot集成Thymeleaf</p>
    </body>
</html>
~~~

> 6、资源目录

~~~
|-resources
		|-static # 静态资源目录
			|-css
			|-js
		|-templates # html目录
			|-index.html
~~~

##### 3、标准变量表达式

~~~html
<!--th:text用于将给定值替换标签text内容，类似$("td").text("xxxx")操作
	语法：${}，与jstl类似，作用一样-->
<td th:text="${user.nick}">Bangalore</td>
<td th:text="${user.phone}">560001</td>
<td th:text="${user.email}">560001</td>
<!--th:utext 用于解析html-->
<td th:utext="${nick}">Bangalore</td>
~~~

##### 4、选择变量表达式

~~~html
<!--th:object用于接收对象（从后台Model获取），其子标签可以使用到该对象
	语法：*{}，通过选择该对象的属性进行获取（从父类object获取）-->
<div th:object="${user}">
    <p>nick：<span th:text="*{nick}">xxx</span></p>
    <p>phone：<span th:text="*{phone}">137xxxxxxx</span></p>
    <p>email：<span th:text="*{email}">xxx@xx.com</span></p>
    <p>address：<span th:text="*{address}">xxx</span></p>
</div>
~~~

##### 5、URL表达式

~~~html
<!--th:href：也就是可以使用Thymeleaf表达式的href属性
	语法：@{}，需字符串拼接表达式-->
<!-- 1、绝对路径-->
<a href="info.html" th:href="@{'http://localhost:8080/boot/user/info?id='+${user.id}'}">查看</a>

<!-- 2、相对路径
	例如：原本http://localhost:8080/boot/，执行th:href="@{'user/info"}
结果：http://localhost:8080/boot/user/info，再执行th:href="@{'edit"}
结果：http://localhost:8080/boot/user/edit-->
<a href="info.html" th:href="@{'user/info?id='+${user.id}}">查看</a>

<!-- 3、相对于项目上下文,项目的上下文名会被自动添加,注意这里开头加了斜杠-->
<a href="info.html" th:href="@{'/user/info?id='+${user.id}}">查看</a>
~~~

##### 6、常见属性

~~~html
<!-- 1、th:action/form表单提交用的action属性
		th:method/请求方式-->
<from th:action="@{/login}" th:method="post">...</from>

<!-- 2、th:each/类似c:foreach。userStat可以省略，默认user+Stat-->
<div th:each="userStat,user : ${userList}"><!--map集合同样，取值使用迭代变量.key/value即可-->
    <!--userStat.count表示从1开始第几个元素，
		.index表示从0开始的索引
		.size表示集合数量
		.current表示当前迭代变量
		.even/.odd表示是否是偶数、奇数
		.first/.last表示是否是第一个、最后一个-->
    <span th:text="${userStat.count}">Tanmay</span>
    <span th:text="${user.nick}">Bangalore</span>
    <span th:text="${user.phone}">560001</span>
    <span th:text="${user.email}">560001</span>
    <span th:text="${user.address}">560001</span>
</div>

<!-- 3、th:herf/a标签的href属性-->
<!-- 4、th:id/id属性-->
<!-- 5、th:object/用于对象绑定，其子标签通过选择变量表达式使用对象属性-->
<!-- 6、th:src/通常与URL表达式配合使用-->
<!-- 7、th:text/用于标签文本显示-->
<!-- 8、th:value/用于标签value属性-->
<!-- 9、th:name/用于标签name属性-->

<!-- 10、th:if/判断分支-->
<span th:if="${sex} == 1">
男：<input type="radio" name="se" th:value="男"/>
</span>
<span th:if="${sex} == 2">
女：<input type="radio" name="se" th:value="女"/>
</span>
<!-- 11、th:unless/判断分支（将表达式取反）-->
<span th:unless="${sex} == 2">
男：<input type="radio" name="se" th:value="男"/>
</span>
<span th:unless="${sex} == 1">
女：<input type="radio" name="se" th:value="女"/>
</span>

<!-- 12、th:switch/th:case java中的通道-->
<div th:switch="${sex}">
    <p th:case="1">性别：男</p>
    <p th:case="2">性别：女</p>
    <p th:case="*">性别：未知</p>
</div>

<!-- 13、th:attr/用于标签通用属性-->
<input type="text" th:attr="value=${userId}">

<!-- 14、th:onclick/点击事件,注意加单引号，因为是字符串-->
<input type="button" th:onclick="'myclick()'">

<!-- 15、th:style/设置样式,注意加单引号，因为是字符串-->
<input type="button" th:style="'display:none;'">

<!-- 16、th:inline
	 th:inline="text"内联文本，作用th标签之外的文本
	 其子类任意地方都可以使用，需要加两个中括号-->
<body th:inline="text">
    …
    <span>[[${user.nick}]]</span>
    …
</body>
<!-- th:inline="javascript"内联脚本,作用javascript，需要加两个中括号-->
<script th:inline="javascript" type="text/javascript">
    var user = [[${user.phone}]];
    alert(user);
</script>
~~~

##### 7、字面量

~~~html
<!-- 1、文本字面量,单引号包裹的为文本-->
<a th:href="@{'api/getUser?id='+${user.id}}">修改</a>
<!-- 2、数字字面量-->
<p>今年是<span th:text="2019">xxx</span>年</p>
<p>20年后，将是<span th:text="2019+20">xxx</span>年</p>
<!-- 3、boolean字面量-->
<p th:if="${isFlag == true}">执行操作</p>
<!-- 4、null字面量-->
<p th:if="${user == null}">user为空</p>
~~~

##### 8、字符串拼接

~~~html
<!-- 1、字面量拼接-->
<psan th:text="'当前是第'+${sex}+'页'"></span>
<!-- 2、优雅拼接-->
<span th:text="|当前是第${sex}页，共${sex}页|"></span>
~~~

##### 9、运算和关系判断

~~~html
<!--
	算术运算：+、-、*、/、%
	关系比较：>/gt、</lt、>=/ge、<=/le
	相等判断：==/eq、!=/ne
-->
<!-- 三元运算符-->
<span th:text="${sex eq 1}?'男':'女'">未知</span>
~~~

##### 10、表达式基本对象

官方手册：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#expression-basic-objects

~~~html
<!-- 1、#request这是3.x版本，若是2.x版本使用#httpServletRequest-->
${#request.getContextPath()}
${#request.getAttribute("phone")}
<!-- 2、#session这是3.x版本，若是2.x版本使用#httpSession-->
${#session.getAttribute("phone")}
${#session.id} # session的唯一标识id
${#session.lastAccessedTime} # 最后访问时间
~~~

##### 11、表达式功能对象

官方手册：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#expression-utility-objects

~~~html
#dates：java.util.Date对象的实用方法，
<span th:text="${#dates.format(curDate,'yyyy-MM-dd HH:mm:ss')}"></span>
#calendars：和dates类似，但是java.util.Calendar对象
#numbers：格式化数字对象的实用方法。
#strings：字符串对象的实用方法：contains，startsWith，prepending/appending等。
#objects：对objects操作的实用方法。
#bools：对布尔值求值的实用方法。
#arrays：数组的实用方法。
#lists：list的实用方法，比如<span th:text="${#lists.size(datas)}"></span>
#sets：set的实用方法。
#maps：map的实用方法。
#aggregates：对数组或集合创建聚合的实用方法。
~~~

#### 十九、Actuator

​		在生产环境中，需要实时或定期监控服务的可用性，spring-boot的actuator功能提供了很多监控所需的接口。Actuator是springboot提供的对应用系统的自省和监控的集成功能，可以对应用系统进行配置查看、健康检查、相关功能统计等。

##### 1、添加依赖

~~~xml
<!--  springBoot提供的Actuator起步依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

##### 2、核心配置文件

~~~properties
#actuator监控配置
management.server.port=8100 # 监控服务端口
mangeement.server.servlet.context-path=/springboot # 监控服务的上下文路径
management.endpoints.web.exposure.include=* # 监控包括所有
~~~

访问：http://localhost:8100/springboot/actuator/health

##### 3、主要功能

> 1、/info（信息）

~~~properties
# 配置信息才有显示
info.contact.email=liuconglook@gmail.com
info.contact.phone=110-120119
~~~

> 2、/shutdown（关闭监控）

~~~properties
# 配置启动才生效
management.endpoint.shutdown.enabled=true
~~~



