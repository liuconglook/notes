## Spring

### 一、学前准备

#### 1、心态

- 戒骄戒躁
- 谨慎豁达
- 如履薄冰

#### 2、方法

基础：夯实基础，了解动态。

思考：保持怀疑，验证一切。

分析：不拘小节，观其大意。

实践：思辨结合，学以致用。

#### 3、工具

JDK：Oracle JDK 8

Spring Framework：5.2.2.RELEASE

IDE：IntelliJ IDEA 2019

Maven：3.2+

### 二、Spring特性总览

#### 1、核心特性（Core）

- IoC容器（IoC Container）
- Spring事件（Events）
- 资源管理（Resources）
- 国际化（i18n）
- 校验（Validation）
- 数据绑定（Data Binding）
- 类型转换（Type Conversion）
- Spring表达式（Spring Express Language）
- 面向切面编程（AOP）

#### 2、数据存储（Data Access）

- JDBC
- 事务抽象（Transaction）
- DAO支持（DAO Support）
- O/R映射（O/R Mapping）
- XML编列（XML Marshalling）

#### 3、Web技术（Web）

- Web Servlet技术栈
  - Spring MVC
  - WebSocket
  - SockJS
- Web Reactive技术栈
  - Spring WebFlux
  - WebClient
  - WebSocket

#### 4、技术整合（Integration）

- 远程调用（Remoting）
- Java消息服务（JMS，Java Message Service）
- Java连接架构（JCA，Java Connection Architecture）
- Java管理扩展（JMX，Java Management Extension）
- Java邮件客户端（Email）
- 本地任务（Tasks）
- 本地调度（Scheduling）
- 缓存抽象（Caching）
- Spring测试（Testing）

#### 5、测试（Testing）

- 模拟对象（Mock Objects）
- TestContext框架（TestContext Framework）
- SpringMVC测试（Spring MVC Test）
- Web测试客户端（WebTestClient）

#### 6、Java版本依赖与支持

| Spring Framework版本 | Java标准版 | Java企业版       |
| -------------------- | ---------- | ---------------- |
| 1.x                  | 1.3+       | J2EE1.3+         |
| 2.x                  | 1.4.2+     | J2EE1.3+         |
| 3.x                  | 5+         | J2EE1.4和JavaEE5 |
| 4.x                  | 6+         | JavaEE6和7       |
| 5.x                  | 8+         | javaEE7          |

#### 7、Spring模块化设计（Modular）

- Spring-aop
- Spring-aspects
- spring-context-indexer
- spring-context-support
- spring-context
- spring-core
- spring-expression
- spring-instrument
- spring-jcl
- spring-jdbc
- spring-jms
- spring-messaging
- spring-orm
- spring-oxm
- spring-test
- spring-tx
- spring-web
- spring-webflux
- spring-webmvc
- spring-websocket

### 三、对Java特性运用

#### 1、Java5语法特性（2004）

| 语法特性               | Spring支持版本 | 代表实现                   |
| ---------------------- | -------------- | -------------------------- |
| 注解（Annotation）     | 1.2+           | @Transaction               |
| 枚举（Enumeration）    | 1.2+           | Propagation                |
| for-each语法           | 3.0+           | AbstractApplicationContext |
| 自动装箱（AutoBoxing） | 3.0+           |                            |
| 泛型（Generic）        | 3.0+           | ApplicationListener        |

#### 2、Java6语法特性（2006）

| 语法特性      | Spring支持版本 | 代表实现 |
| ------------- | -------------- | -------- |
| 接口@Override | 4.0+           |          |

#### 3、Java7语法特性（2011）

| 语法特性               | Spring支持版本 | 代表实现                    |
| ---------------------- | -------------- | --------------------------- |
| Diamond语法            | 5.0+           | DefaultListableBeanFactory  |
| try-with-resources语法 | 5.0+           | ResourceBundleMessageSource |

#### 4、Java8语法特性（2014）

Lambda语法、可重复注解、类型注解

| 语法特性   | Spring支持版本 | 代表实现                      |
| ---------- | -------------- | ----------------------------- |
| Lambda语法 | 5.0+           | PropertyEditorRegistrySupport |

#### 5、Java9语法特性（2017）

模块化、接口私有方法

#### 6、Java10语法特性（2018）

局部变量类型推断

### 四、对JDK API实践

#### 1、< Java 5 API

| API类型                   | Spring支持版本 | 代表实现                   |
| ------------------------- | -------------- | -------------------------- |
| 反射（Reflection）        | 1.0+           | MethodMatcher              |
| Java Beans                | 1.0+           | CachedIntrospectionResults |
| 动态代理（Dynamic Proxy） | 1.0+           | JdkDynamicAopProxy         |

#### 2、Java 5 API

| API类型                | Spring支持版本 | 代表实现                   |
| ---------------------- | -------------- | -------------------------- |
| XML处理（DOM，SAX...） | 1.0+           | XmlBeanDefinitionReader    |
| Java管理扩展（JMX）    | 1.2+           | @ManagedResource           |
| Instrumentation        | 2.0+           | InstrumentationSavingAgent |
| 并发框架（J.U.C）      | 3.0+           | ThreadPoolTaskScheduler    |
| 格式化（Formatter）    | 3.0+           | DateFormatter              |

#### 3、Java 6 API

| API类型                      | Spring支持版本 | 代表实现                          |
| ---------------------------- | -------------- | --------------------------------- |
| JDBC4.0（JSR221）            | 1.0+           | JdbcTemplate                      |
| Common Annotations（JSR250） | 2.5+           | CommonAnnotationBeanPostProcessor |
| JAXB2.0（JSR222）            | 3.0+           | Jaxb2Marshaller                   |
| Scripting in JVM（JSR223）   | 4.2+           | StandardScriptFactory             |
| 可插拔注解处理API（JSR269）  | 5.0+           | @Indexed                          |
| Java Compiler API（JSR199）  | 5.0+           | TestCompiler（单元测试）          |

#### 4、Java 7 API

| API类型                 | Spring支持版本 | 代表实现                |
| ----------------------- | -------------- | ----------------------- |
| Fork/Join框架（JSR166） | 3.1+           | ForkJoinPoolFactoryBean |
| NIO2（JSR203）          | 4.0+           | PathResource            |

#### 5、Java 8 API

| API类型                     | Spring支持版本 | 代表实现                            |
| --------------------------- | -------------- | ----------------------------------- |
| Date and Time API（JSR310） | 4.0+           | DateTimeContext                     |
| 可重复Annotations（JSR337） | 4.0+           | @PropretySources                    |
| Stream API（JSR335）        | 4.2+           | StreamConverter                     |
| CompletableFuture           | 4.2+           | CompletableToListnableFutureAdapter |

### 五、对Java EE API整合

#### 1、Java EE Web技术相关

| JSR规范                    | Spring支持版本 | 代表实现                          |
| -------------------------- | -------------- | --------------------------------- |
| Servlet+JSP（JSR035）      | 1.0+           | DispatcherServlet                 |
| JSTL（JSR052）             | 1.0+           | JstlView                          |
| JavaServer Faces（JSR127） | 1.1+           | FacesContextUtils                 |
| Portlet（JSR168）          | 2.0 - 4.2      | DispatcherPortlet                 |
| SOAP（JSR067）             | 2.5+           | SoapFaultException                |
| WebServices（JSR109）      | 2.5+           | CommonAnnotationBeanPostProcessor |
| WebSocket（JSR356）        | 4.0+           | WebSocketHandler                  |

#### 2、Java EE数据存储相关

| JSR规范                    | Spring支持版本 | 代表实现              |
| -------------------------- | -------------- | --------------------- |
| JDO（JSR12）               | 1.0-4.2        | JdoTemplate           |
| JTA（JSR907）              | 1.0+           | JtaTransactionManager |
| JPA（EJB3.0 JSR220的成员） | 2.0+           | JpaTransactionManager |
| Java Caching API（JSR107） | 3.2+           | JCacheCache           |

#### 3、Java EE Bean技术相关

| JSR规范                                 | Spring支持版本 | 代表实现                             |
| --------------------------------------- | -------------- | ------------------------------------ |
| JMS（JSR914）                           | 1.1+           | JmsTemplate                          |
| EJB2.0（JSR19）                         | 1.0+           | AbstractStatefulSessionBean          |
| Dependency Injection for Java（JSR330） | 2.5+           | AutowiredAnnotationBeanPostProcessor |
| Bean Vaildation（JSR303）               | 3.0+           | LocalValidatorFactoryBean            |

### 六、Spring编程模型

#### 1、面向对象编程

契约接口：Aware、BeanPostProcessor...

设计模式：观察者模式、组合模式、模板模式...

对象继承：Abstract*类

#### 2、面向切面编程

动态代理：JdkDynamicAopProxy

字节码提升：ASM、CGLib、AspectJ...

#### 3、面向元编程

注解：模式注解（@Componet、@Service、@Respository...）

配置：Environment抽象、PropertySources、BeanDefinition...

泛型：GenericTypeResolver、Resolvable、ResolvableType...

#### 4、函数驱动

函数接口：

- ApplicationEventPublisher
- Reactive：Spring WebFlux

#### 5、模块驱动

Maven Artifacts

OSGI Bundles

Java 9 Automatic Modules

Spring @Enable*

### 七、IoC

#### 1、什么是IoC？

​		在软件工程中，控制反转（Inversion of Control，IoC）是一种编程风格或者原则，IoC将控制流与传统的控制流（输入/输出）进行反向。

​		在IoC中，计算机程序的自定义编程部分从通用框架接收控制流。

​		软件架构与设计反转控制相比传统的过程式编程：在传统编程中，自定义表示的目的程序调用到可重用库中照顾通用的任务。但由于控制反转，它是调用自定义的框架，或特定于任务的代码。

#### 2、IoC的简史

- 1983年，Richard E. Sweet 在《The Mesa Programming Environment》中提出“Hollywood Principle”（好莱坞原则）。
- 1988年，Ralph E. Johnson & Brian Foote 在《Designing Reusable Classes》中提出“Inversion of control”（控制反转）。
- 1996年，Michael Mattsson 在《Object-Oriented Frameworks, A survey of methodological issues》中将“Inversion of control”命名为 “Hollywood principle”
- 2004年，Martin Fowler 在《Inversion of Control Containers and the Dependency Injection pattern》中提出了自己对 IoC 以及 DI 的理解
- 2005年，Martin Fowler 在 《InversionOfControl》对 IoC 做出进一步的说明

#### 3、IoC主要实现策略

在面向对象编程中，有几种实现控制反转的基本技术。这些是：

- 使用服务定位器模式
- 依赖注入
  - 构造函数注入
  - 参数注入
  - Setter注入
  - 接口注入
- 使用上下文查找
- 使用模板方法设计模式
- 使用策略设计模式

IoC是一个宽泛的概念，可以用不同的方式实现。主要有两种类型：

- 依赖项查找：
  - 容器提供对组件的回调，以及查找上下文。这是EJB和Apache Avalon方法。
  - 它让每个组件都有责任使用容器api来查找资源和协作者。
  - 控制反转仅限于调用回调方法的容器，应用程序代码可以使用这些回调方法来获取资源。
- 依赖注入：
  - 组件不需要查找。
  - 它们提供了简单的Java方法，使容器能够解析依赖项。
  - 容器完全负责连接组件，将解析后的对象传递给JavaBean属性或构造函数。
  - 使用JavaBean属性称为Setter注入;使用构造函数参数称为构造函数注入。

#### 4、IoC容器的职责

- 将任务的执行与实现解耦。

- 将一个模块的重点放在它所设计的任务上。

- 将模块从其他系统如何工作的假设中解放出来，而不是依赖于契约。

- 防止更换模块时的副作用。

控制反转有时又被称为“好莱坞原则：别来找我们，我们会来找你”。

**通用职责：**

- 依赖处理
  - 依赖查找
  - 依赖注入
- 生命周期管理
  - 容器
  - 托管的资源（Java Beans 或其他资源）
- 配置
  - 容器
  - 外部配置
  - 托管的资源（Java Beans 或其他资源）

#### 5、IoC容器的实现

- Java SE
  - Java Beans
  - Java ServiceLoader SPI
  - JNDI（Java Naming and Directory Interface）
- Java EE
  - EJB（Enterprise Java Beans）
  - Servlet
- 开源
  - Apache Avalon（http://avalon.apache.org/closed.html）
  - PicoContainer（http://picocontainer.com/）
  - Google Guice（https://github.com/google/guice）
  - Spring Framework（https://spring.io/projects/spring-framework）

#### 6、传统IoC容器的实现

Java Beans作为IoC容器：

- 特性
  - 依赖查找
  - 生命周期管理
  - 配置元信息
  - 事件
  - 自定义
  - 资源管理
  - 持久化
- 规范
  - JavaBeans：https://www.oracle.com/technetwork/java/javase/tech/index-jsp-138795.html
  - BeanContext：https://docs.oracle.com/javase/8/docs/technotes/guides/beans/spec/beancontext.html

#### 7、轻量级IoC容器

《Expert One-on-One™ J2EE™ Development without EJB™》

认为轻量级容器的特征 ：

- 一个可以管理应用程序代码的容器。
- 快速启动的容器。
- 一个不需要任何特殊部署步骤来部署对象的容器。
- 一个容器有如此轻的足迹和最小的API依赖，它可以在各种环境中运行。
- 一个容器，它为添加一个管理对象设置了一个非常低的部署工作和性能开销的标准，因此可以部署和管理细粒度的对象，以及粗粒度的组件。

认为轻量级容器的好处：

- 逃离单片容器。
- 最大化代码的可重用性。
- 更大程度上的面向对象。
- 更大的生产力
- 更好的可测试性。

#### 8、依赖查找 VS 依赖注入

优劣对比：

| 类型     | 依赖处理 | 实现便利性 | 代码侵入性   | API依赖性     | 可读性 |
| -------- | -------- | ---------- | ------------ | ------------- | ------ |
| 依赖查找 | 主动获取 | 相对烦琐   | 侵入业务逻辑 | 依赖容器API   | 良好   |
| 依赖注入 | 被动提供 | 相对便利   | 低侵入性     | 不依赖容器API | 一般   |

#### 9、构造器注入 VS Setter注入

**Spring Framework 对构造器注入与 Setter 的论点：**

​		Spring团队通常提倡构造函数注入，因为它允许您将应用程序组件实现为不可变的对象，并确保所需的依赖项不是空的。此外，注入构造函数的组件总是以完全初始化的状态返回给客户机（调用）代码。附带说明一下，大量的构造函数参数是一种不好的代码味道，这意味着类可能有太多的责任，应该对其进行重构，以更好地解决问题的适当分离。

​		ObjectProvider类用来解决依赖项为空的问题，它是一种类型安全的方式。如果对象是一个单一的类型，可以调getIfAvailable方法进行示范性地返回，那么等于空的时候他就会返回空。

​		Setter注入应该主要用于可选的依赖项，这些依赖项可以在类中分配合理的默认值。否则，必须在代码使用依赖项的任何地方执行非空检查。Setter注入的一个好处是，Setter方法使该类对象能够在以后进行重新配置或重新注入。因此，通过JMX MBean进行管理是Setter注入的一个引人注目的用例。

**《Expert One-on-One™ J2EE™ Development without EJB™》认为 Setter 注入的优点 ：**

- JavaBean属性在IDE中得到了很好的支持。
- JavaBean属性是自文档化的。
- JavaBean属性有子类继承，不需要任何代码。
- 如果需要，可以使用标准JavaBeans属性编辑器机制进行类型转换。
- 许多现有的JavaBean可以在一个面向JavaBean的IoC容器中使用，而无需修改。
- 如果每个Setter都有相应的Getter（使用属性可读，也可写），则可以向组件询问其当前配置转态。如果我们想要持久化这种状态，这尤其有用。例如，在XML表单或数据库中，使用构造函数注入，无法找到当前状态。
- Setter注入对于具有默认值的对象工作得很好，这意味着不是所有属性都需要在运行时提供。

**认为 Setter 注入的缺点 ：**

​		调用Setter的顺序在任何契约中都没有表示。因此，我们有时需要在调用最后一个Setter来初始化组件之后调用方法。Spring提供了org.springframework.bean.factory.InitializingBean接口；它还提供了调用任意init方法的能力。但是，该合同必须形成文件，以确保在容器外正确使用。

​		在使用之前可能没有调用所有必要的Setter。因此，可以保留对象的部分配置。

**认为构造器注入的优点：**

​		在任何业务方法中调用之前，保证每个托管对象处于一致的状态全配置状态。这是构造器函数注入的主要动机。（然而，通过依赖项检测，可以用JavaBeans实现相同的结果，Spring可以选择执行。）不需要初始化方法。

​		使用多个JavaBean方法得到的代码可能比使用少个JavaBean方法得到的代码要多一些，但是在复杂性上没有区别。

**认为构造器注入的缺点：**

- 尽管也是java语言的特性，但在现有代码中，多参数构造函数可能不如使用JavaBean属性常见。
- Java构造函数参数没有自省可见的名称。
- IDE对构造函数参数列表的支持不如JavaBean Setter方法好。
- 长构造函数参数列表和大型构造函数主体可能变得笨拙。
- 具体的继承可能会有问题。
- 与JavaBeans相比，对可选属性的支持较差。
- 单元测试可能稍微有些困难。
- 当合作者被传递到对象构造时，不可能改变对象中的引用。

### 八、Spring IoC

#### 1、依赖查找

- 根据Bean名称查找
  - 实时查找：直接通过BeanFactory查找。
  - 延迟查找：通过ObjectFactory接口，实现类ObjectFactoryCreatingFactoryBean。
- 根据Bean类型查找
  - 单个Bean对象
  - 集合Bean对象
- 根据Bean名称 + 类型查找
- 根据Java注解查找
  - 单个Bean对象
  - 集合Bean对象

#### 2、依赖注入

- 根据Bean名称注入
- 根据Bean类型注入
  - 单个Bean对象
  - 集合Bean对象
- 注入容器内建Bean对象
- 注入非Bean对象
- 注入类型
  - 实时注入
  - 延迟注入

#### 3、依赖来源

- 自定义Bean
  - 一些自定义的Bean
  - 来源：Spring IoC 底层容器就是指的 BeanFactory 的实现类，大多数情况是 DefaultListableBeanFactory 这个类，它来管理 Spring Beans，而 ApplicationContext 通常为开发人员接触到的 IoC 容器，它是一个 Facade，Wrap 了 BeanFactory 的实现。
- 容器内建Bean对象
  - 实际上，内建的Bean是普通的Spring Bean，来源同上。
  - 例如：Environment。
  - 通过DefaultSingletonBeanRegistry#registerSingleton手动注册的bean。
  - @AutoWired(AutowiredAnnotationBeanPostProcessor处理)会调用DefaultListableBeanFactory的resolveDependency方法去resolvableDependencies里面找，而依赖查找不会去找的。
- 容器内建依赖
  - 内建依赖则是通过AutowireCapableBeanFactory中的resolveDependency方法来注册，并非是一个Spring Bean，无法通过依赖查找获取。
  - 例如：BeanFactory接口的实现类DefaultListableBeanFactory
  - 来源：AutowireCapableBeanFactory 中的 resolveDependency 方法来注册，其实现类是DefaultListableBeanFactory，保存在map类型的resolvableDependencies属性名中。

#### 4、配置元信息

- Bean定义配置
  - 基于XML文件
  - 基于Properties文件
  - 基于Java注解
  - 基于Java API
- IoC容器配置
  - 基于XML文件
  - 基于Java注解
  - 基于Java API
- 外部化属性配置
  - 基于Java注解

#### 5、谁是Spring IoC容器？

- BeanFactory是Spring底层IoC容器。
- ApplicationContext 是具备应用特性的BeanFactory超集。

### 九、Spring Bean基础

#### 1、定义SpringBean

> 什么是BeanDefinition？

BeanDefinition是SpringFramework中定义Bean的配置元信息接口，包含：
- Bean的类名。
- Bean行为配置元素，如作用域、自动绑定的模式，生命周期回调等。
- 其他Bean引用，又可称合作者（collaborators）或者依赖（dependencies）。
- 配置设置，比如Bean属性（Properties）。

>BeanDefinition元信息

| 属性（Property）         | 说明                                         |
| ------------------------ | -------------------------------------------- |
| Class                    | Bean全类名，必须是具体类，不能用抽象类或接口 |
| Name                     | Bean的名称或ID                               |
| Scope                    | Bean的作用域（如：singleton、Prototype等）   |
| Constructor arguments    | Bean构造器参数（用于依赖注入）               |
| Properties               | Bean属性设置（用于依赖注入）                 |
| Autowiring mode          | Bean自动绑定模式（如：通过名称byName）       |
| Lazy initialization mode | Bean延迟初始化模式（延迟和非延迟）           |
| Initialization method    | Bean初始化回调方法名称                       |
| Destruction method       | Bean销毁回调方法名称                         |

> BeanDefinition构建

- 通过BeanDefinitionBuilder。
- 通过AbstractBeanDefinition以及派生类。

#### 2、命名Spring Bean

> Bean的名称

​		每个Bean拥有一个或多个标识符（identifiers），这些标识符在Bean所在的容器必须是唯一的。通常，一个Bean仅有一个标识符，如果需要额外的，可考虑使用别名（Alias）来扩充。

​		在基于XML的配置元信息中，开发人员可用id或者name属性来规定Bean的标识符。通常Bean的标识符有字母组成，允许出现特殊字符。如果要想引入Bean的别名的话，可在name属性使用半角逗号（","）或分号（“;”）来间隔。

​		Bean的id或name属性并非必须制定，如果留空的话，容器会为Bean自动生成一个唯一的名称。Bean的命名尽管没有限制，不过官方建议采用驼峰的方式，更符合Java的命名约定。

> Bean名称生成器（BeanNameGenerator）

由Spring Framework 2.0.3引入，框架内建两种实现：

- DeFaultBeanNameGenerator：默认通用BeanNameGenerator实现。
- AnnotationBeanNameGenerator：基于注解扫描的BeanNameGenerator实现，起始于Spring Framework 2.5。

> Bean的别名

Bean别名（Alias）的价值

- 复用现有的BeanDefinition
- 更具有场景化的命名方法，比如<alias name="myApp-dataSource" alias="subsystemA-dataSource"/><alias name="myApp-dataSource" alias="subsystemB-dataSource"/>

#### 3、注册Spring Bean

> BeanDefinition注册

- XML配置元信息
  - <bean name="..." .../>
- Java注解配置源信息
  - @Bean
  - @Componet
  - @Import
- Java API 配置源信息
  - 命名方式：BeanDefinitionRegistry#registerBeanDefinition(String, BeanDefinition)
  - 非命名方式：BeanDefinitionReaderUtils#registerWithGeneratedName(AbstractBeanDefinition, BeanDefinitionRegistry)
  - 配置类方式：AnnotatedBeanDefinitionReader#register(Class...)

> 外部单例对象注册

- JavaAPI配置元信息
  - singletonBeanRegistry#registerSingleton

#### 4、实例化Spring Bean

- 常规方式
  - 通过构造器（配置元信息：XML、Java注解和Java API）
  - 通过静态工厂方法（配置元信息：XML和Java API）
  - 通过Bean工厂方法（配置元信息：XML和Java API）
  - 通过FactoryBean（配置元信息：XML、Java注解和JavaAPI）
- 特殊方式
  - 通过ServiceLoaderFactoryBean（配置元信息：XML、Java注解和Java API）
  - 通过AutowireCapableBeanFactory#createBean(java.lang.Class, int, boolean)
  - 通过BeanDefinitionRegistry#registerBeanDefinition(String, BeanDefinition)

#### 5、初始化Spring Bean

> Bean初始化（Initialization）

- @PostConstruct标注方法
- 实现InitializationBean接口的afterPropertiesSet()方法
- 自定义初始化方法
  - XML配置：<bean init-method="init".../>
  - Java注解：@Bean(initMethod="init")
  - Java API：AbstractBeanDefinition#setInitMethodName(String)

> Bean延迟初始化（Lazy Initialization）

- XML配置：<bean lazy-init="true" .../>
- Java注解：@Lazy(true)

#### 6、销毁SpringBean

> Bean销毁（Destroy）

- @PreDestroy标注方法
- 实现DisposableBean接口的Destroy()方法
- 自定义销毁方法
  - XML配置：<bean destroy="destroy" .../>
  - Java注解：@Bean(destroy="destroy")
  - Java API：AbstractBeanDefinition#setDestroyMethodName(String)

> Bean垃圾回收（GC）

1、关闭Spring容器（应用上下文）

2、执行GC

3、Spring Bean 覆盖的finalize()方法被回调











### 面试题

#### 1、什么是Spring Framework？

Spring是一个针对企业级java开发的开源框架，核心特性是开发java的应用。

Spring是一种易用性的开源框架，它能够让你的java应用变得更容易开发，它提供任何东西你想要的，并且是拥抱在企业环境的java语言上面。并且支持类似于JVM上面的第二语言，或者可选语言，比如Groovy和Kotlin这样的语言。同时它也提供了一些弹性，根据你的软件的需要，为你创造出不同的一个软件架构。

#### 2、Spring Framework有哪些核心模块？

Spring-Core：Spring基础API模块，如资源管理，泛型处理。

Spring-Beans：Spring Bean相关，如依赖查找，依赖注入。

Spring-AOP：Spring AOP处理，如动态代理，AOP字节码提升。

Spring-Context：事件驱动、注解驱动，模块驱动等。

Spring-expression：Spring表达式语言模块。

#### 3、Spring Framework的优势和不足是什么？

注：一个东西应该是优劣并存的。在收集网上答案或者学习的过程中，要多方的正向或反向地思考，反复斟酌的同时也需要对一些特性特别的了解。

#### 4、什么是IoC？

IoC是反转控制（Inversion of Control），类似于好莱坞原则，主要有依赖查找和依赖注入实现。

好莱坞原则（Hollywood principle）：不要给我们打电话，我们会给你打电话。

#### 5、依赖查找和依赖注入的区别？

依赖查找时主动或手动的依赖查找方式，通常需要依赖容器或标准API实现。而依赖注入则是手动或自动依赖绑定的方式，无需依赖特定的容器和API。

#### 6、Spring作为IoC容器有什么优势？

典型的IoC管理，依赖查找和依赖注入

AOP抽象、事务抽象、事务机制、SPI扩展、强大的第三方整合、易测试性、更好的面向对象。

#### 7、什么是Spring IoC容器？

控制反转（IoC）原理的Spring框架实现。IoC也称为依赖注入（DI）。这是一个对象仅通过构造函数参数、工厂方法的参数或对象实例构造，或从工厂方法返回后在对象实例上设置的属性来定义其依赖项（即与之一起工作的其他对象）的过程。然后容器在创建bean时注入这些依赖项。

#### 8、BeanFactory与FactoryBean？

BeanFactory是IoC底层容器。

FactoryBean是创建Bean的一种方式，帮助实现复杂的初始化逻辑。

#### 9、Spring IoC容器启动时做了哪些准备？

IoC配置元信息读取和解析、IoC容器生命周期、Spring事件发布、国际化等

#### 10、如何注册一个Spring Bean？

通过BeanDefinition和外部单体对象来注册。

#### 11、什么是Spring BeanDefinition？

回顾“定义 Spring Bean”和“BeanDefinition元信息”

#### 12、Spring容器是怎么管理注册Bean？



















