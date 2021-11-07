## Servlet笔记

### 一、Servlet

#### 1、概念

- 运行在服务器端的小程序。
- 是一个接口，定义了java类被浏览器访问到（tomcat识别）的规则。

#### 2、Demo

- 实现Servlet接口，实现接口中的抽象方法。

- 配置web.xml

  - ~~~xml
    <servlet>
    	<servlet-name>demo</servlet-name>
    	<servlet-class>com.belean.ServletDemo</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>demo</servlet-name>
        <url-pattern>/demo</url-pattern>
    </servlet-mapping>
    ~~~

#### 3、原理

- 服务器接收客户端请求，会解析请求URL路径，获取访问的servlet资源路径。（/demo）
- 查找web.xml，寻找servlet映射标签（servlet-mapping）下，资源路径（url-pattern）是否存在。
- 如果存在，则根据对应servlet名称（servlet-name），再找到对应servlet类（servlet-class）。
- tomcat通过反射将字节码文件加载进内存，创建其对象。
- 调用其方法。

#### 4、生命周期

- init：初始化方法，只执行一次。
  - 默认，第一次被访问时（servlet被创建时）执行。
  - 第一次访问时创建：servlet标签下，`<load-on-startup>`值为负数。
  - 服务器启动时创建：servlet标签下，`<load-on-startup>`值为正数。
- service：提供服务方法，执行多次。
- destroy：销毁方法，只执行一次。
  - 在servlet被销毁之前执行，一般用于释放资源。
  - 服务器正常关闭，才会执行destroy方法。

#### 5、Servlet3.0

- Servlet3.0支持注解配置，可以不需要web.xml了。
- `@WebServlet("资源路径")` # 注解url资源路径。
  - `urlPatterns` # 默认，资源路径集。
  - `loadOnStartup` # 创建时机。

#### 6、结构

~~~
|-- Servlet(接口)
	|-- GenericServlet(抽象类)
	|-- HttpServlet(抽象类)
~~~

- `GenericServlet`将Servlet接口中其他的方法做了默认空实现，只将service方法作为抽象类。
- `HttpServlet`是对http协议的一种封装、简化操作。

### 二、HTTP

#### 1、概念

- 超文本传输协议（Hyper Text Transfer Protocol）。
  - 传输协议：定义了，客户端和服务器端通信时，发送数据的格式。
  - 特点：
    - 基于TCP/IP的高级协议。
    - 默认端口号80。
    - 基于请求/响应模型。（一次请求对应一次响应）
    - 无状态的。（每次请求之间相互独立，不能交互数据）
  - 历史版本：
    - 1.0：每一次请求响应都会建立新的连接。
    - 1.1：复用连接。

#### 2、请求消息数据格式

- 请求消息（字符串格式）

~~~
POST /login.html	HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:60.0) Gecko/20100101 Firefox/60.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Referer: http://localhost/login.html
Connection: keep-alive
Upgrade-Insecure-Requests: 1

username=zhangsan
~~~

- 请求行
  - 格式：`请求方法 url 协议/版本`
  - 例如：`GET /login.html HTTP/1.1`
  - 请求方式：
    - GET：请求参数拼接在url后，且有长度（数据大小）限制，不太安全。
    - POST：请求参数在请求体中，没有长度（数据大小）限制，相对安全。
- 请求头（告诉服务器一些信息）
  - 格式：`头名称: 值`
  - `User-Agent`：浏览器版本信息。
    - 可以在服务器端获取该头的信息，解决浏览器的兼容问题。
  - `Referer`：访问来源
    - 可以防盗链，统计工作。
- 请求空行：用于分割请求头和请求体的。
- 请求体：封装POST请求消息的请求参数的。

#### 3、响应消息数据格式

- 响应消息（字符串格式）

~~~
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 101
Date: Wed, 06 Jun 2018 07:08:42 GMT

<html>
    <head>
    	<title>$Title$</title>
    </head>
    <body>
    	hello , response
    </body>
</html>
~~~

- 响应行
  - 格式：`协议/版本 状态码 状态码描述`
  - 例如：`HTTP/1.1 200 OK`
  - 状态码：
    - 1xx：服务器接收客户端消息，但没有接收完成，等待一段时间后，发送1xx状态码。
    - 2xx：成功。例如：200成功。
    - 3xx：重定向。例如：302重定向，304访问缓存。
    - 4xx：客户端错误。例如：404请求路径没有对应的资源，405请求方式没有对应的处理方法。
    - 5xx：服务器端错误。例如：500服务器内部出现异常。
- 响应头
  - 格式：`头名称: 值`
  - `Context-Type`：响应体的数据格式，及编码格式。
  - `Context-disposition`：打开方式。
    - `in-line`：默认值，在当前页面内打开。
    - `attachment;filename=xxx`：以附件形式打开响应体。（文件下载）
- 响应空行：分割响应头和响应体的。
- 响应体：传输的数据。

### 三、Request

#### 1、结构

~~~
|-- ServletRequest(接口)
	|-- HttpServletRequest(接口)
		|-- RequestFacade(类，tomcat)
~~~

#### 2、方法

- 获取请求消息数据
  - 请求行数据
    - `String getMethod()` # 获取请求方式。
    - `String getContextPath()` # 获取虚拟目录（项目名）。
    - `String getServletPath()` # 获取servlet资源路径。
    - `String getQueryString()` # 获取请求参数（问号后面的一截）。
    - 获取请求URL：
      - `String getRequestURL()` # /虚拟目录/资源目录
      - `StringBuffer getRequestURL()` # http://localhsot/虚拟目录/资源目录
      - URL：统一资源定位符（http://localhsot/虚拟目录/资源目录）
      - URI：统一资源标识符（/虚拟目录/资源目录），范围更大。
    - `String getProtocol()` # 获取协议及版本。
    - `String getRemoteAddr()` # 获取客户机IP地址。
  - 请求头数据
    - `String getHeader(String name)` # 根据头名称获取值。
    - `Enumeration<String> getHeaderNames()` # 获取所有的请求头名称。
  - 请求体数据（POST）
    - `BufferedReader getReader()` # 获取字符输入流。
    - `ServletInputStream getInputStream()` # 获取字节输入流。
- 其他功能
  - `String getParameter(String name)` # 根据参数名获取值。
  - `String[] getParameterValues(String name)` # 根据参数名，获取值的集合。
  - `Enumeration<String> getParameterNames()` # 获取所有请求参数名。
  - `Map<String, String[]> getParameterMap()` # 获取所有参数的Map集合。
  - `setCharacterEncoding(String charcter)` # 设置字符集。
- 请求转发
  - `RequestDispatche getRequestDispatcher(String path);` # 获取转发器对象
  - `requestDispatche.forward(ServletRequest request, ServletResponse response);` # 转发
  - 特点：
    - 地址栏不发生改变。
    - 只能转发当前服务器内部资源。
    - 转发是一次请求。可以使用request对象共享数据。
- 共享数据（request域）
  - `void setAttribute(String name, object obj)` # 存储数据
  - `Object getAttribute(String name)` # 根据键获取值数据
  - `void removeAttribute(String name)` # 移除键值对
  - `ServletContext getServletContext()` # 获取ServletContext对象

### 四、Response

- 设置响应行
  - `setStatus(int sc)` # 设置状态码。
- 设置响应头
  - `setHeader(String name, String value)` # 设置响应头。

- 设置响应体
  - `PrintWriter getWriter()` # 获取字符流。
  - `ServletOutputStream getOutputStream()` # 设置响应体，获取字节流。
- 重定向
  - 第一种：`response.setStatus(302);response.setHeader("location","/demo/login");`
  - 第二种：`response.sendRedirect("/demo/login");`
  - 特点
    - 地址栏发生变化。
    - 可以访问其他站点资源。
    - 是两次请求。不能使用request对象来共享数据。
- 设置编码
  - `response.setContextType("text/html;charset=utf-8");`

### 五、ServletContext

#### 1、概念

- 代表整个web应用，可以和程序的容器（服务器）来通信。

#### 2、方法

- 获取对象
  - `request.getServletContext();` # 通过`request`对象获取。
  - `this.getServletContext();` # 通过`HttpServlet`对象获取。
- 获取MIME类型
  - 格式：`大类型/小类型`
  - 比如：`text/html`、`image/jpeg`
  - `String getMimeType(String file)` 
- 共享数据（ServletContext域）
  - `setAttribute(String name, Object value)`
  - `getAttribute(String name)`
  - `removeAttribute(String name)`
- 获取文件的真实路径
  - `String getRealPath(String path)`

### 六、Cookie

- 客户端会话技术：Cookie。
  - 数据保存在客户端，在一次会话内，多次请求间，共享数据。
- 使用：
  - 创建：`new Cookie(String name, String value);`（绑定数据）
  - 发送：`response.addCookie(Cookie cookie);`
  - 获取：`Cookie[] request.getCookies()`

- 原理：
  - 基于响应头set-cookie和请求头cookie实现。
- 细节：
  - 可以发送多个cookie。
  - 默认浏览器关闭后，cookie销毁。
  - 持久化设置`setMaxAge(int seconds)`。（正数毫秒，0销毁，负数则默认值）
  - tomcat8之后虽然支持中文数据了，但特殊字符还是不可以，所有建议使用URL编解码。
  - 共享问题
    - 一台tomcat服务器中，多项目共享cookie。
      - 默认不能共享。`setPath(String path)` # 设置为`"\"`，就可以共享了。
    - 不同的tomcat服务器间共享cookie。
      - `setDomain(String path)` # 如果设置一级域名相同，则可以共享。
        - 例如`setDomain(".baidu.com")`，那么news.baidu.com中cookie可以共享。
- 特点：
  - 数据存储在客户端浏览器。
  - 单个cookie大小限制4kb，以及同一域名下的总cookie数限制20个。
- 作用
  - 一般用于存储少量的不太敏感的数据。
  - 在不登录的情况下，对客户端的身份识别。

### 七、Session

- 服务端会话技术：Session。

  - 数据存储在服务端对象中，在一次会话内，多次请求间，共享数据。

- 使用：

  - 获取：`HttpSession session = request.getSession();`
  - 操作：
    - `Object getAttribute(String name)` # 获取数据
    - `void setAttribute(String name, Object value)` # 更新数据
    - `void removeAttribute(String name)` # 删除数据

- 细节

  - 客户端关闭后，服务器不关闭。

    - 默认，获取的session不是同一个。（两次会话）

    - 设置最大存活时间

      - ~~~java
        Cookie c = new Cookie("JSESSIONID", session.getId());
        c.setMaxAge(60*60);
        response.addCookie(c);
        ~~~

  - 客户端不关闭，服务器关闭后。

    - 不是同一个，因为服务器端的session对象被销毁了，在次获取的是服务器活化后的新session对象。
    - 为了确保数据不丢失，tomcat自动完成以下工作：
      - 钝化：在服务器正常关闭之后，将session对象序列化到硬盘上。
      - 活化：在服务器启动后，将session文件转换为内存红的session对象。

  - session销毁。

    - 服务器关闭。

    - `session.invalidate()`

    - 默认30分钟自动失效。

      - ~~~xml
        <session-config>
        	<session-timeout>30</session-timeout>
        </session-config>
        ~~~

- 特点：

  - 数据存储在服务器端。
  - 可以存储任意类型、大小数据。
  - session数据安全，Cookie相对不安全。

### 八、JSP

- java服务端页面（Java Server Pages，jsp）。
  - 特殊的页面，可以指定html标签，又可以定义java代码，用于简化书写。
- 原理：本质上就是一个servlet。
- 脚本：
  - `<% java 代码 %>`：逻辑代码，定义在service方法中。
  - `<%! java 代码 %>`：声明代码，成员位置。
  - `<%= java代码 %>`：输出代码，输出到页面。
- 9个内置对象：
  - pageContext（`PageContext`），当前页面共享数据，可以获取其他八个内置对象。
  - application（`ServletContext`），所有用户间共享数据。
  - page（`Object`），当前页面的对象。
  - config（`ServletConfig`），配置对象。
  - exception（`Throwable`），异常对象。
  - request（`HttpServletRequest`），一次请求访问多个资源（转发）。
  - response（`HttpServletResponse`），响应对象。
  - session（`HttpSession`），一次会话的多个请求间共享数据。
  - out：类似`response.getWriter()`的`out.write()`
    - 区别：先找response缓冲区数据，再找out缓冲区数据。

- 指令：
  - `<%@ [指令名称] [属性名=属性值]... %>`
  - page：配置页面。
    - `contentType` # 等同于`response.setContentType()`。
    - `import` # 导包。
    - `errorPage` # 异常页面。
    - `isErrorPage` # 标记异常页面，true表示可以使用内置对象exception。
  - include：导入资源文件。
    - `<%@include file="xx.jsp"%>`
  - taglib：导入资源。
    - `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`

### 九、EL

- 表达式语言（Expression Language）。
- 作用：替换和简化jsp页面中java代码的编写。
- 语法：`${表达式}`。
- 忽略el表达式：
  - jsp默认支持el表达式。
  - 设置jsp中page指令属性：`isELIgnored="true"` # 忽略jsp页面中所有el表达式。
  - `\${表达式}` # 忽略当前el表达式。
- 使用：
  - 运算符：
    - 算术运算符：+、-、*、/（div）、%（mod）。
    - 比较运算符：>、<、>=、<=、==、!=。
    - 逻辑运算符：&&（and）、||（or）、!（not）。
    - 空运算符：empty、not empty。（是否为null或者长度为0）
  - 获取值：
    - `${域对象.key}/${key}` # 获取域对象存储的值
      - `pageScope`=>`requestScope`=>`sessionScope`=>`applicationScope`。
    - `${obj.field}` # 对象
    - `${key[index]}` # 集合
    - `${key.keyName}`或`${key["keyName"]}` # map

### 十、JSTL

- JSP标准标签库（Java Server Pages Tag Library，JSTL）。

- 作用：简化和替换jsp页面上的java代码。

- 使用

  - 导入JSTL相关jar包。
  - 引入标签库。`<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
  - 使用标签。

- 常用标签

  - ~~~xml
    <c:if test="表达式"></c:if> # 相当于if语句，表达式为true才显示。
    <c:choose> # 相当于 switch
        <when></when> # 相当于 case
        <otherwise></otherwise> 相当于 default
    </c:choose>
    <c:foreach start="" end="" step=""> 相当于for
    </c:foreach>
    <c:foreach item="" var=""> # 相当于 for :
    </c:foreach>
    ~~~

### 十一、Filter

#### 1、概念

- 过滤器Filter。
- 作用：
  - 登录验证。
  - 统一编码处理。
  - 敏感字符过滤。

#### 2、Demo

- 定义一个类，实现接口Filter，实现里面的方法。

- 配置web.xml。

  - ~~~xml
    <filter>
    	<filter-name>demo</filter-name>
        <filter-class>com.belean.FilterDemo</filter-class>
    </filter>
    <filter-mapping>
    	<filter-name>demo</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ~~~

#### 3、配置

- 拦截路径配置
  - `/index.jsp` # 拦截具体资源。
  - `/user/*` # 拦截目录。
  - `*.jsp` # 后缀名拦截。
  - `/*` # 拦截所有。
- 拦截方式配置
  - web.xml配置
    - 设置`<dispatcher></dispatcher>`。
    - 值：
      - REQUEST：默认值，浏览器直接请求资源。
      - FORWARD：转发访问资源。
      - INCLUDE：包含访问资源。
      - ERROR：错误跳转资源。
      - ASYNC：异步访问资源。
  - 注解配置（`@WebFilter`）
    - 设置`dispatcherTypes`属性。
- 过滤器链
  - 假设有过滤器1、过滤器2。
  - 执行顺序：过滤器1 => 过滤器2 => 资源执行 => 过滤器2 => 过滤器1。

#### 4、生命周期

- `init`：初始化方法，只执行一次。
- `doFilter`：拦截请求资源时执行，执行多次。
- `destroy`：服务器正常关闭后执行，只执行一次。

### 十二、Listener

#### 1、概念

- 事件监听机制
  - 事件：一件事。如：单击事件。
  - 事件源：事件发生的地方。如：按钮。
  - 监听器：一个对象。如：Listener，触发登录逻辑。
  - 注册监听：将事件、事件源、监听器绑定在一起。当事件源上发生某个事件后，执行监听器代码。

#### 2、ServletContextListener

- 作用：监听ServletContext对象的创建和销毁。

- 方法：

  - `void contextInitialized(ServletContextEvent sce)` # ServletContext对象创建后调用。
  - `void contextDestroyed(ServletContextEvent sce)` # ServletContext对象销毁前调用。

- 使用：

  - 定义一个类，实现ServletContextListener接口，实现其方法。

  - 配置web.xml

    - ~~~xml
      <listener>
      	<listener-class>com.belean.ContextLoaderListener</listener-class>
      </listener>
      <!-- <context-param>指定初始化参数 -->
      ~~~

  - 注解：`@WebListener`

