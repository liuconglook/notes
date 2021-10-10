## 测试笔记

### SpringBootTest

#### pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
~~~

- `junit-jupiter-api`：JUnit断言（Assert）
- `hamcrest`：匹配器（Matcher）
- `Mockito`：模拟对象（Mockito框架）

~~~java
import static org.junit.Assert.*;
import static org.hamcrest.CoreMatchers.*;
import static org.mockito.Mockito.*;
~~~

#### Annotation

~~~java
// 忽略方法
@Ignore("not implemented yet")
// 执行超时时间
@Test(timeout = 1000)
// 测试抛出异常
@Test(expected = IllegalArgumentException.class)
// 本类，初始化方法，每个方法执行前
@Before
// 本类，销毁释放资源方法，每个方法执行后
@After
// 静态，初始化方法，所有方法执行前
@BeforeClass
public static void test() {
}
// 静态，销毁释放资源方法，所有方法执行后
@AfterClass
public static void test() {
}
~~~

#### Assert

https://juejin.im/post/6844903953948213262

> true则通过，false则输出Message

- `assertArrayEquals(String msg, Object[] arr1, Object[] arr2);` # 数组值比较，是否相等。
- `assertEquals(String msg, String str1, String str2);` # 各种类型值比较，是否相等。
- `assertFalse(String msg, boolean b);` # 断言布尔类型，是否正确。
- `assertNotNull(String msg, Object o);` # 断言非空，是否有值。
- `assertNull("should be null", null);` # 断言空值，是否为空。
- `assertNotSame(String msg, Object o1, Object o2);` # 断言值不相等，是否不相等。
- `assertSame(String msg, Integer i1, Integer i2);` # 断言相等，是否相等。
- `assertThat("albumen", both(containsString("a")).and(containsString("b")));`

>`assertSame`与`assertEquals`的区别

- `assertSame`
  - 只支持Object。
  - ==比较

- `assertEquals`
  - 只支持基本数据类型。
  - equals比较

### 单元测试（Unit Test）

即对某个单元代码进行测试，一般是对某个方法进行测试，需要去除相关依赖。

要想测试代码执行快，就需要编写的代码职责单一，也就代码少。

要想测试覆盖率高，单元测试就需要足够多。

#### JUnit4

~~~xml
<!-- junit4 -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
~~~

~~~java
import static org.junit.Assert.*; // 断言
import static org.hamcrest.CoreMatchers.*; // 匹配器
public class XxxTest extends TestCase {
    
    public XxxTest(String name) {
        super(name)
    }
    
    @Override
    public void setUp() {
        
    }
    
    public void testXxx() {
        
    }
}
~~~

#### JUnit5

https://www.cnblogs.com/bolingcavalry/p/14461719.html

> 形参注入

~~~
@Test
public void test(@Autowrid XxxService xxxService) {

}
~~~

#### SpringTest

~~~xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>4.2.4.RELEASE</version>
</dependency>
~~~

~~~java
@RunWith(SpringRunner.class)
public class XxxTest {
    
}
~~~

#### SpringBootTest

~~~java
// SpringJUnit4ClassRunner // 1.4版本之前使用
@RunWith(SpringRunner.class) // spring应用上下文环境
@SpringBootTest(classes = Application.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT) // servlet环境，随机端口
@ActiveProfiles("test") 
public class XxxTest {
    
    @Test
    public void testXxx() throws Exception {
        
    }
    
}
~~~

### 集成测试（Integration Test）

与单元测试类似，区别在于不需要去除相关依赖的类，而是作为整体一起测试。

#### 测试Controller

~~~java
@SpringBootTest(classes = Application.class)
@RunWith(SpringRunner.class)
@ActiveProfiles("it")
public class EsProductControllerIT {

    private MockMvc mockMvc;

    //@Autowired
    //private WebApplicationContext wac;

    @Autowired
    private XxxController xxxController;

    @Before
    public void setup() {
        // 模拟整个上下文环境
        //this.mockMvc = MockMvcBuilders.webAppContextSetup(wac).build();
        // 模拟某个Controller上下文环境
        this.mockMvc = MockMvcBuilders.standaloneSetup(this.xxxController).build();
    }

    @Test
    public void testSearch() throws Exception {
        MvcResult mvcResult = mockMvc
                .perform(MockMvcRequestBuilders.get("/xxx/search")
                        .contentType(MediaType.APPLICATION_JSON)
                        .characterEncoding("UTF-8")
                        .param("page", "0")
                        .param("size", "5"))
                .andExpect(MockMvcResultMatchers.status().isOk()).andReturn();
        MockHttpServletResponse response = mvcResult.getResponse();
        response.setCharacterEncoding("UTF-8");
        System.out.println(response.getContentAsString());
    }

}
~~~

#### 模拟异步调用

~~~java
@Test
public void testSearchSync() throws Exception {
	mockMvc.perform(get("/xxx/search").contentType(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.*.title", hasItem(is("Hokuto no Ken"))));
}
    
@Test
public void testSearchASync() throws Exception {
    MvcResult result = mockMvc.perform(get("/manga/async/ken")
        .contentType(MediaType.APPLICATION_JSON))
        .andDo(print())
        .andExpect(request().asyncStarted())
        .andDo(print())
        .andReturn();
    mockMvc.perform(asyncDispatch(result))
            .andDo(print())
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.*.title", hasItem(is("Hokuto no Ken"))));
}
~~~

### 接口测试（Interface Test）

Postman

chrome插件：

- ModHeader（设置头信息）
- Postman Interceptor（结合postman客户端，收录接口请求信息）

### Maven

#### 生命周期

后面的阶段都会执行一遍前面的阶段。

~~~
validate

initialize
generate-sources
process-sources
generate-resources
process-resources
compile

process-classes
generate-test-sources
process-test-sources
generate-test-resources
process-test-resources
test-compile
process-test-classes
test

prepare-package
package

pre-integration-test
integration-test
post-integration-test
verify

install
~~~

#### 命令

Maven常用命令：https://www.jianshu.com/p/6f57c322e50e

> 通过参数来控制单元测试和集成测试的执行（需配置下面三个maven插件）

~~~xml
<properties>
    <skipUTs>false</skipUTs>
    <skipITs>false</skipITs>
</properties>
~~~

~~~shell
# maven-clean3.1插件默认会先执行mvn clean

# 运行单元测试和集成测试
mvn integration-test

# 只运行单元测试（集成测试的阶段还在后面，所以只运行单元测试）
mvn test
# 只运行单元测试
mvn integration-test -DskipITs=true

# 只运行集成测试（失效）
mvn failsafe:integration-test
# 只运行集成测试
mvn integration-test -DskipUTs=true
~~~

#### maven-surefire-plugin

该插件用于配置跳过单元测试用例，默认绑定在test阶段。

`maven-failsafe-plugin`默认执行`src/test/java`下符合条件的测试类，在configuration标签下配置：

~~~xml
<!-- 单元测试类 -->
<includes>
  <include>**/Test*.java</include>
  <include>**/*Test.java</include>
  <include>**/*TestCase.java</include>
</includes>
~~~

- skipTests：跳过所有单元测试。

~~~xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <skipTests>${skipUTs}</skipTests>
    </configuration>
</plugin>
~~~

#### maven-failsafe-plugin

故障安全插件用于配置执行集成测试用例，默认绑定在integration-test阶段

`maven-failsafe-plugin`默认执行`src/test/java`下符合条件的测试类，在configuration标签下配置：

~~~xml
<!-- 集成测试类 -->
<includes>
  <include>**/IT*.java</include>
  <include>**/*IT.java</include>
  <include>**/*ITCase.java</include>
</includes>
~~~

- skipITs：跳过集成测试。

~~~xml
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-failsafe-plugin</artifactId>
    <executions>
        <!-- 在integration-test 和 verify 阶段都会执行集成测试 -->
		<execution>
			<id>integration-tests</id> <!-- id唯一随意写 -->
            <goals>
                <goal>integration-test</goal>
                <goal>verify</goal>
            </goals>
            <configuration>
                <skipITs>${skipITs}</skipITs>
            </configuration>
        </execution>
    </executions>
</plugin>
~~~

#### build-helper-maven-plugin

该插件用于加载和编译资源。

- 添加源程序目录：add-source
- 添加资源目录：add-resources
- 添加测试源程序：add-test-source
- 添加的测试资源目录：add-test-resources

> 单元测试和集成测试源代码分离

这里采用了单元测试和集成测试源代码分开的方式，实际编译后都会在同一目录（target/test-classes）。`maven-surefire-plugin`和`maven-failsafe-plugin`插件会根据默认的命名规则区分单元测试和集成测试。

~~~
project
	|- src
		|- main
			|- java
			|- resources
		|- test
			|- java
			|- resources
		|- integration-test
			|- java
			|- resources
~~~

~~~xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>build-helper-maven-plugin</artifactId>
    <executions>
        <execution>
            <id>add-integration-test-source</id> <!-- id唯一随意写 -->
            <phase>generate-test-sources</phase> <!-- 生命周期阶段 -->
            <goals>
                <goal>add-test-source</goal> <!-- 目标固定：添加测试源文件目录 -->
            </goals>
            <configuration>
                <sources>
                    <source>src/integration-test/java</source> <!-- 指定测试目录 -->
                </sources>
            </configuration>
        </execution>
        <execution>
            <id>add-integration-test-resources</id>
            <phase>generate-test-resources</phase>
            <goals>
                <goal>add-test-resource</goal> <!-- 目标固定：添加测试资源目录 -->
            </goals>
            <configuration>
                <resources>
                    <resource>
                        <directory>src/integration-test/resources</directory>
                    </resource>
                </resources>
            </configuration>
        </execution>
    </executions>
</plugin>
~~~

### Mockito

mockito是一个模拟对象（Mocking）框架，SpringBootTest自带。

https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html

#### 模拟对象

~~~java
// 模拟对象
Mockito.mock(class<T>  classToMock);
@Mock // 模拟对象，需初始化
@MockBean // 模拟对象，从容器中取得

// 真实对象，当spy接口或抽象类调用未实现的方法时会报错，这时应该do-when，而不是when-then
// 或者根本就不能spy接口或抽象类
Mockito.spy(class<T> classToMock); 
@Spy // 真实对象，需初始化
@SpyBean // 真实对象，从容器中取得

// 初始化Mock 或使用@RunWith(MockitoJUnitRunner.class) 或 @RunWith(SpringRunner.class)
public MockitoTest() {
    MockitoAnnotations.initMocks(this);
}

// 重置Mock
Mockito.reset(obj);

// 一行代码初始化存根
List listMock = Mockito.when(Mockito.mock(List.class).get(0)).thenReturn("first").getMock();

// 模拟对象，拥有对象行为的空壳，需预期行为才能正常使用。
// 真实对象，原对象的行为，可以预期部分或全部行为。

// 预期行为，可以称为“打桩”，而用来替换原行为内容的代码称其“桩代码”
// 又名存根(stubbing)，意思是用伪对象替换原有实现就是存根。
~~~

> 总结

- `Mockito.mock` # 模拟对象（伪对象）
  - `@Mock` // 需初始化
  - `@MockBean` // 从容器中取得
    - 测试service层时，可以注解dao层，模拟行为的返回，而不是真实的CRUD。
- `Mockito.spy` # 真实对象（间谍）
  - `@Spy` // 需初始化
  - `@SpyBean` // 从容器中取得
    - 测试service层时，可以注解service层，模拟部分依赖的方法，从而测试单一的目标方法。
- `Mockito.reset(obj)` # 重置mock
- `Mockito.mock(List.class, Mockito.withSettings().serializable());` # 序列化模拟对象

#### 预期行为

~~~java
// 校验行为是否被执行过1次
Mockito.verify(obj).doSomething();
// 校验行为被执行1次，且执行时间不超过100毫秒
// Mockito.never() 验证执行0次
// Mockito.atMostOnce() 最多执行1次
// Mockito.atMost(2) 最多执行2次
// Mockito.atLeastOnce() 最少执行1次
// Mockito.atLeast(2) 最少执行2次
// Mockito.description("描述")
Mockito.verify(obj, Mockito.timeout(100).times(1)).doSomething();

// 多模拟对象，按顺序验证
List a = Mockito.mock(List.class);
List b = Mockito.mock(List.class);
a.add(1);
b.add(2);
InOrder inOrder = Mockito.inOrder(a, b);
// 验证a集合先添加1，b集合再添加2
inOrder.verify(a).add(1);
inOrder.verify(b).add(2);

// 验证多个模拟对象从未进行过交互，可忽略某些存根
List a = Mockito.mock(List.class);
List b = Mockito.mock(List.class);
List c = Mockito.mock(List.class);
a.add(1); // 只有a进行了交互
Mockito.verifyNoInteractions(b, c);

// 确保验证全覆盖，防止漏掉对某些交互的验证
List list = Mockito.mock(List.class);
list.add(1);
list.add(2);
Mockito.verify(list).add(1);
Mockito.verify(list).add(2);
Mockito.verifyNoMoreInteractions(list);

// 当调用obj.doSomething()时，返回预期结果
Mockito.when(obj.doSomething()).thenReturn("result");
// 或 
// 注意：当使用spy真实对象时，这种方式可以避免因调用真实对象的行为无果而报错
// 例如：list.get(1)实际没有该元素，使用doReturn-when方式会先赋值，然后调用。
Mockito.doReturn("result").when(obj).doSomething();

// 第一次调用返回result1，第二次和第N次调用返回result2
Mockito.when(obj.doSomething()).thenReturn("result1").thenReturn("result2");
// 或
Mockito.when(obj.doSomething()).thenReturn("result1","result2");

// 抛出异常，当调用obj.doSomething时
Mockito.doThrow(new IOException()).when(obj).doSomething();
// 抛出异常，需要有返回值，且是该方法支持抛出的异常
Mockito.when(list.add(1)).thenThrow(new RuntimeException());

// 火车残骸，返回深层次调用结果
A a = Mockito.mock(A.class, Mockito.RETURNS_DEEP_STUBS);
Mockito.when(a.getB().doSomething()).thenReturn("result");
// 或者
A a = Mockito.mock(A.class);
B b = Mockito.mock(B.class);
Mockito.when(a.getB()).thenReturn(b);
Mockito.when(a.getB().doSomething()).thenReturn("result");

// 什么也不做，仅限void方法使用；当调用obj.setName("name")，什么也没干。
Mockito.doNothing().when(obj).setName(Mockito.anyString());

// 调用真实方法（调用行为被模拟之前的真实方法）
Mockito.doCallRealMethod().when(obj).getName();
// 或
Mockito.when(obj.getName()).doCallRealMethod();

~~~

> 总结

- `verify` # 验证行为（是否被调用，调用多少次）
  - 最少、最多、零调用次数。
  - 多模拟对象按顺序验证。
  - 验证多个模拟对象从未进行过交互。
  - 确保验证全覆盖，防止漏掉对某些交互的验证
- `when-thenReturn` # 预期行为结果
- `doThrow` # 模拟抛出异常
- `Mockito.RETURNS_DEEP_STUBS` # 模拟深层次调用
  - `RETURNS_DEFAULTS` # 默认，未存根默认返回0、空集合、null等
  - `RETURNS_SMART_NULLS` # 返回SmartNull而非null，提供了良好的异常堆栈信息。

#### 匹配参数

~~~java
// 固定值匹配
Mockito.when(list.get(1)).thenReturn("result for index 1");

// 参数类型匹配（参数匹配器）
// Mockito.anyInt() 任意int值
// Mockito.any(A.class) 只能传递A对象
// 注意：方法中只要有一个参数使用了参数匹配器，其他参数也都要使用
// 如果其中有固定值，需要使用Mocktio.eq("xxx")
Mockito.when(list.get(Mockito.anyInt())).thenReturn("result");

// 自定义参数匹配器
Mockito.when(list.contains(Mockito.argThat(new IsValid()))).thenReturn(true);
class IsValid implements ArgumentMatcher<Integer> {
    @Override
    public boolean matches(Integer index) {
        return index == 1 || index == 2;
    }
}
// 或 使用jdk8的lambda语法
Mockito.when(
    	list.contains(Mockito.argThat(e -> (Integer) e == 1 || (Integer) e == 2))
	).thenReturn(true);


// 传递参数调用方法的同时模拟其行为（预期回调接口生成期望值）
Mockito.doAnswer(new CustomAnswer()).when(list).get(Mockito.anyInt());
// 或
Mockito.doAnswer(invocation -> "arg index for " + invocation.getArguments()[0])
    .when(mockList)
    .get(Mockito.anyInt());
// 或
Mockito.when(list.get(Mockito.anyInt())).thenAnswer(new CustomAnswer());
// 可使用匿名内部类
class CustomAnswer implements Answer<String> {
    @Override
    public String answer(InvocationOnMock invocation) throws Throwable {
        Object[] args = invocation.getArguments();
        return "arg index for " + args[0];
    }
}
~~~

> 总结

- 参数传递
  - 固定值 # 例如：`list.get(1)`
  - 参数匹配器 # 例如：`list.get(Mockito.anyInt())`可传递任意int数值
  - 自定义参数匹配器 # 例如：`list.get(Mockito.argThat(new IsValid()))`
- 行为回调（可获取传递的参数，模拟行为，及设置返回值）
  - `Mockito.doAnswer(new CustomAnswer())`























