## Mall项目学习

项目地址：https://github.com/macrozheng/mall

项目文档：http://www.macrozheng.com/#/README

演示地址：http://www.macrozheng.com/admin/index.html#/login user：`admin` password：`macro123`

### 搭建骨架

`Group`：`com.belean.mall.tiny`

`ArtifactId`：`mall-study`

#### 项目结构

~~~
com.belean.mall.tiny
	|-common # 通用类
	|-component # 组件
	|-config # 配置
	|-controller # 控制器
	|-dao # mapper接口
	|-dto # 传输对象
	|-mbg # mybatis generator
		|-mapper # 生成的mapper接口
		|-model # 生成的实体类
		|-Commentgenerator.java # 配置生成注解
		|-Generator.java # 生成入口
    |-service # 服务类
    	|-impl # 实现服务
    |-Application.java # springboot程序入口
~~~

#### pom.xml

~~~xml
	<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.3.RELEASE</version>
        <relativePath/>
        <!-- lookup parent from repository -->
    </parent>

    <dependencies>
        <!--SpringBoot通用依赖模块-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!--MyBatis分页插件-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.10</version>
        </dependency>

        <!--集成druid连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>

        <!-- MyBatis 生成器 -->
        <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.3.3</version>
        </dependency>

        <!--Mysql数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.15</version>
        </dependency>

    </dependencies>
~~~

#### application.yml

~~~yml
server:
  port: 8081
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mall?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai&useSSL=false
    username: root
    password: root
mybatis:
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath*:com/**mapper/*.xml
~~~

#### Mybatis Generator

SQL：https://github.com/macrozheng/mall-learning/blob/master/document/sql/mall.sql

##### generator.properties

> resources/generator.properties

~~~properties
jdbc.driverClass=com.mysql.cj.jdbc.Driver
jdbc.connectionURL=jdbc:mysql://localhost:3306/mall?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai&useSSL=false
jdbc.userId=root
jdbc.password=root
~~~

> 报错：javax.net.ssl.SSLException: closing inbound before receiving peer's close_notify
>
> 解决：加上useSSL=false

##### generatorConfig.xml

> resources/generatorConfig.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <properties resource="generator.properties"/>
    <context id="MySqlContext" targetRuntime="MyBatis3" defaultModelType="flat">
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>
        <property name="javaFileEncoding" value="UTF-8"/>
        <!-- 为模型生成序列化方法-->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>
        <!-- 为生成的Java模型创建一个toString方法 -->
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin"/>
        <!--可以自定义生成model的代码注释-->
        <commentGenerator type="com.belean.mall.tiny.mbg.CommentGenerator">
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true"/>
            <property name="suppressDate" value="true"/>
            <property name="addRemarkComments" value="true"/>
        </commentGenerator>
        <!--配置数据库连接-->
        <jdbcConnection driverClass="${jdbc.driverClass}"
                        connectionURL="${jdbc.connectionURL}"
                        userId="${jdbc.userId}"
                        password="${jdbc.password}">
            <!--解决mysql驱动升级到8.0后不生成指定数据库代码的问题-->
            <property name="nullCatalogMeansCurrent" value="true" />
        </jdbcConnection>
        <!--指定生成model的路径-->
        <javaModelGenerator targetPackage="com.belean.mall.tiny.mbg.model" targetProject="mall-study\src\main\java"/>
        <!--指定生成mapper.xml的路径-->
        <sqlMapGenerator targetPackage="com.belean.mall.tiny.mbg.mapper" targetProject="mall-study\src\main\resources"/>
        <!--指定生成mapper接口的的路径-->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.belean.mall.tiny.mbg.mapper"
                             targetProject="mall-study\src\main\java"/>
        <!--生成全部表tableName设为%-->
        <table tableName="pms_brand">
            <generatedKey column="id" sqlStatement="MySql" identity="true"/>
        </table>
    </context>
</generatorConfiguration>
~~~

> 注意：targetProject使用绝对路径

##### CommentGenerator.java

> com.belean.mall.tiny.mbg.CommentGenerator.java

~~~java
/**
 * 自定义注释生成器
 */
public class CommentGenerator extends DefaultCommentGenerator {
    private boolean addRemarkComments = false;

    /**
     * 设置用户配置的参数
     */
    @Override
    public void addConfigurationProperties(Properties properties) {
        super.addConfigurationProperties(properties);
        this.addRemarkComments = StringUtility.isTrue(properties.getProperty("addRemarkComments"));
    }

    /**
     * 给字段添加注释
     */
    @Override
    public void addFieldComment(Field field, IntrospectedTable introspectedTable,
                                IntrospectedColumn introspectedColumn) {
        String remarks = introspectedColumn.getRemarks();
        //根据参数和备注信息判断是否添加备注信息
        if (addRemarkComments && StringUtility.stringHasValue(remarks)) {
            addFieldJavaDoc(field, remarks);
        }
    }

    /**
     * 给model的字段添加注释
     */
    private void addFieldJavaDoc(Field field, String remarks) {
        //文档注释开始
        field.addJavaDocLine("/**");
        //获取数据库字段的备注信息
        String[] remarkLines = remarks.split(System.getProperty("line.separator"));
        for (String remarkLine : remarkLines) {
            field.addJavaDocLine(" * " + remarkLine);
        }
        addJavadocTag(field, false);
        field.addJavaDocLine(" */");
    }

}
~~~

##### Generator.java

> com.belean.mall.tiny.mbg.Generator.java

~~~java
public class Generator {
    public static void main(String[] args) throws Exception {
        //MBG 执行过程中的警告信息
        List<String> warnings = new ArrayList<String>();
        //当生成的代码重复时，覆盖原代码
        boolean overwrite = true;
        //读取我们的 MBG 配置文件
        InputStream is = Generator.class.getResourceAsStream("/generatorConfig.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(is);
        is.close();

        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        //创建 MBG
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        //执行生成代码
        myBatisGenerator.generate(null);
        //输出警告信息
        for (String warning : warnings) {
            System.out.println(warning);
        }
    }
}
~~~

##### MyBatisConfig.java

> com.belean.mall.tiny.config.MyBatisConfig.java

~~~java
@Configuration
@MapperScan("com.belean.mall.tiny.mbg.mapper")
public class MyBatisConfig {
}
~~~

#### Swagger2

##### pom.xml

~~~xml
<!-- swagger2 生成api文档 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- swagger ui 展示-->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
~~~

##### Config

~~~java
@Configuration
@EnableSwagger2
public class Swagger2Config {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                // 为当前包下controller生成API文档
                .apis(RequestHandlerSelectors.basePackage("com.belean.mall.tiny.controller"))
                // 为有@Api注解的Controller生成API文档
//                .apis(RequestHandlerSelectors.withClassAnnotation(Api.class))
                // 为有@ApiOperation注解的方法生成API文档
//                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Mall Api Document") // 项目标题
                .description("mall-study") // 项目描述
                // 联系人 网站 邮箱
                .contact(new Contact("belean", "https://github.com/liuconglook", "liuconglook@gmail.com"))
                .license("Apache LICENSE 2.0") // 开源协议
                .licenseUrl("http://www.apache.org/") // 开源协议地址
                .termsOfServiceUrl("") // 服务条款地址
                .version("1.0") // 版本
                .build();
    }
}
~~~

##### Annotation

> io.swagger.annotations

~~~java
// 接口信息
@Api(tags = "用户相关的接口", description = "包括用户登录、注册等...") // 类
@ApiOperation("用户登录接口") // 方法
@ApiParam("用户ID") // 形参

@ApiImplicitParams({
            @ApiImplicitParam(name = "pageNum", value = "页码", dataTypeClass = Integer.class, paramType = "query", example = "1"),
            @ApiImplicitParam(name = "pageSize", value = "页数", dataTypeClass = Integer.class, paramType = "query", example = "3")
    }) // 方法，详细描述形参

@ApiIgnore // 方法/形参，忽略

// 实体类信息
@ApiModel("用户实体类") // 类
@ApiModelProperty("用户ID") // 属性
~~~

##### 生成Model文档注释

修改CommentGenerator文件，重新生成带有文档注释的Model

> 报错：Result Maps collection already contains value for com.belean.mall.tiny.mbg.mapper.PmsBrandMapper.BaseResultMap
>
> 注意：先删除mapper.xml文件后再生成，由其在原有内容上追加导致，错误提示id重复。

```java
/**
 * 自定义注释生成器
 */
public class CommentGenerator extends DefaultCommentGenerator {
    private boolean addRemarkComments = false;
    private static final String EXAMPLE_SUFFIX="Example";
    private static final String API_MODEL_PROPERTY_FULL_CLASS_NAME="io.swagger.annotations.ApiModelProperty";

    /**
     * 设置用户配置的参数
     */
    @Override
    public void addConfigurationProperties(Properties properties) {
        super.addConfigurationProperties(properties);
        this.addRemarkComments = StringUtility.isTrue(properties.getProperty("addRemarkComments"));
    }

    /**
     * 给字段添加注释
     */
    @Override
    public void addFieldComment(Field field, IntrospectedTable introspectedTable,
                                IntrospectedColumn introspectedColumn) {
        String remarks = introspectedColumn.getRemarks();
        //根据参数和备注信息判断是否添加备注信息
        if (addRemarkComments && StringUtility.stringHasValue(remarks)) {
            // addFieldJavaDoc(field, remarks);
            // 数据库中特殊字符需要转义
            if(remarks.contains("\"")) {
                remarks = remarks.replace("\"","'");
            }
            // 给model的字段添加swagger注解
            field.addJavaDocLine("@ApiModelProperty(value = \"" + remarks + "\")");
        }
    }

    /**
     * 给model的字段添加注释
     */
    private void addFieldJavaDoc(Field field, String remarks) {
        //文档注释开始
        field.addJavaDocLine("/**");
        //获取数据库字段的备注信息
        String[] remarkLines = remarks.split(System.getProperty("line.separator"));
        for (String remarkLine : remarkLines) {
            field.addJavaDocLine(" * " + remarkLine);
        }
        addJavadocTag(field, false);
        field.addJavaDocLine(" */");
    }

    @Override
    public void addJavaFileComment(CompilationUnit compilationUnit) {
        super.addJavaFileComment(compilationUnit);
        // 只在model中添加swagger注解类的导入
        if(!compilationUnit.isJavaInterface() && !compilationUnit.getType().getFullyQualifiedName().contains(EXAMPLE_SUFFIX)) {
            compilationUnit.addImportedType(new FullyQualifiedJavaType(API_MODEL_PROPERTY_FULL_CLASS_NAME));
        }
    }
}
```

##### 查看文档

- http://localhost:8081/mall/v2/api-docs
- http://localhost:8081/mall/swagger-ui.html

#### Spring-Security

##### 权限管理

RBAC模型，基于角色的访问控制（Role-Based Access Control）

> 原始七张表实现：用户-角色-菜单-权限，及其中间表共七张。



> mall项目权限表结构

- ums_admin # 用户对应的角色、及其额外权限。
  - ums_admin_permission_relation
  - ums_admin_role_relation

- ums_menu # 多级菜单

- ums_permission # 权限列表

- ums_role # 角色对应的菜单、权限、资源
  - ums_role_menu
  - ums_role_permission_relation
  - ums_role_resource_relation

- ums_resource # 资源及分类
  - ums_resource_category

##### JWT

JWT（JSON WEB TOKEN）基于 RFC 7519 标准定义的一种可以安全传输的的JSON对象。简单的说就是进行JSON对象传输时，保证数据加密且不被篡改。

> 结构

解析JWT：https://jwt.io/

~~~json
eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNyZWF0ZWQiOjE2Mjc3MzA4MTMxNDUsImV4cCI6MTYyODMzNTYxM30.YHsWOamgWl41FIMXrRydax3S_3mH4yKoM8ldywXD-bmv79ZbIjC1zSx-eNF4ltRnotaCmjf0cH_1M4sHYfelGw
# 解析结果，以.分割的三段字符串：header.payload.signature
# header定义了加密算法
{
  "alg": "HS512"
}
# payload定义了用户token创建时间和存活时间
{
  "sub": "admin",
  "created": 1627730813145,
  "exp": 1628335613
}
# signature由 header+"."+payload和自定义的秘钥secret，使用HMACSHA512加密而成
# 除非有秘钥，否则无法篡改Token的任意部分
HMACSHA512(base64UrlEncode(header) + "." +base64UrlEncode(payload),secret)
~~~

> 数字签名

数字签名（又称公钥数字签名）通常使用私钥生成签名，使用公钥验证签名。

签名及验证过程：

- 发送方用一个哈希函数（例如MD5）从报文文本中生成**报文摘要**，然后用自己的私钥对这个摘要进行加密。
- 将加密后的摘要作为报文的**数据签名**和**报文**一起发送给接收方。
- 接收方用同样的方式获取**报文摘要**，再用公钥解密得到**报文摘要**。
- 如果两个摘要相等，则说明在传输过程中报文未被篡改。

数字签名验证的作用：

- 确定消息确实是由发送方签名并发出来的。
- 确定消息的完整性。

##### SpringSecurity

SpringSecurity是一个强大的可高度定制的认证和授权框架，对于Spring应用来说它是一套Web安全标准。

结合JWT的加密传输，即可认证登录，验证权限。

##### pom.xml

~~~xml
<!--SpringSecurity依赖配置-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<!--JWT(Json Web Token)登录支持-->
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>
~~~

##### application.yml

~~~yml
# 自定义jwt key
jwt:
  tokenHeader: Authorization # JWT存储的请求头
  secret: mySecret # JWT加解密使用的密钥
  expiration: 604800 # JWT的超期限时间(60*60*24),单位秒（工具类*1000）
  tokenHead: Bearer  # JWT负载中拿到开头
~~~



##### Config

~~~java
/**
 * SpringSecurity的配置
 */
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled=true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    private UmsAdminService adminService;
    @Autowired
    private RestfulAccessDeniedHandler restfulAccessDeniedHandler;
    @Autowired
    private RestAuthenticationEntryPoint restAuthenticationEntryPoint;

    @Override
    protected void configure(HttpSecurity httpSecurity) throws Exception {
        httpSecurity.csrf()// 由于使用的是JWT，我们这里不需要csrf
                .disable()
                .sessionManagement()// 基于token，所以不需要session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                .antMatchers(HttpMethod.GET, // 允许对于网站静态资源的无授权访问
                        "/",
                        "/*.html",
                        "/favicon.ico",
                        "/**/*.html",
                        "/**/*.css",
                        "/**/*.js",
                        "/swagger-resources/**",
                        "/v2/api-docs/**"
                )
                .permitAll()
                .antMatchers("/admin/login", "/admin/register")// 对登录注册要允许匿名访问
                .permitAll()
                .antMatchers(HttpMethod.OPTIONS)//跨域请求会先进行一次options请求
                .permitAll()
//                .antMatchers("/**")//测试时全部运行访问
//                .permitAll()
                .anyRequest()// 除上面外的所有请求全部需要鉴权认证
                .authenticated();
        // 禁用缓存
        httpSecurity.headers().cacheControl();
        // 添加JWT filter
        httpSecurity.addFilterBefore(jwtAuthenticationTokenFilter(), UsernamePasswordAuthenticationFilter.class);
        //添加自定义未授权和未登录结果返回
        httpSecurity.exceptionHandling()
                .accessDeniedHandler(restfulAccessDeniedHandler)
                .authenticationEntryPoint(restAuthenticationEntryPoint);
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService())
                .passwordEncoder(passwordEncoder());
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        //获取登录用户信息
        return username -> {
            UmsAdmin admin = adminService.getAdminByUsername(username);
            if (admin != null) {
                List<UmsPermission> permissionList = adminService.getPermissionList(admin.getId());
                return new AdminUserDetails(admin,permissionList);
            }
            throw new UsernameNotFoundException("用户名或密码错误");
        };
    }

    @Bean
    public JwtAuthenticationTokenFilter jwtAuthenticationTokenFilter(){
        return new JwtAuthenticationTokenFilter();
    }

    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }

}
~~~

~~~java
/**
 * JWT登录授权过滤器
 * @author belean
 */
@Slf4j
public class JwtAuthenticationTokenFilter extends OncePerRequestFilter {
    @Autowired
    private UserDetailsService userDetailsService;
    @Autowired
    private JwtTokenUtil jwtTokenUtil;
    @Value("${jwt.tokenHeader}")
    private String tokenHeader;
    @Value("${jwt.tokenHead}")
    private String tokenHead;

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain chain) throws ServletException, IOException {
        String authHeader = request.getHeader(this.tokenHeader);
        if (authHeader != null && authHeader.startsWith(this.tokenHead)) {
            // The part after "Bearer "
            String authToken = authHeader.substring(this.tokenHead.length());
            String username = jwtTokenUtil.getUserNameFromToken(authToken);
            log.info("checking username:{}", username);
            if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
                UserDetails userDetails = this.userDetailsService.loadUserByUsername(username);
                if (jwtTokenUtil.validateToken(authToken, userDetails)) {
                    UsernamePasswordAuthenticationToken authentication = new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                    authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                    log.info("authenticated user:{}", username);
                    SecurityContextHolder.getContext().setAuthentication(authentication);
                }
            }
        }
        chain.doFilter(request, response);
    }
}
~~~

~~~java
/**
 * 当未登录或者token失效访问接口时，自定义的返回结果
 */
@Component
public class RestAuthenticationEntryPoint implements AuthenticationEntryPoint {
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        response.setCharacterEncoding("UTF-8");
        response.setContentType("application/json");
        response.getWriter().println(JSONUtil.parse(CommonResult.unauthorized(authException.getMessage())));
        response.getWriter().flush();
    }
}
~~~

~~~java
/**
 * 当访问接口没有权限时，自定义的返回结果
 */
@Component
public class RestfulAccessDeniedHandler implements AccessDeniedHandler {
    @Override
    public void handle(HttpServletRequest request,
                       HttpServletResponse response,
                       AccessDeniedException e) throws IOException, ServletException {
        response.setCharacterEncoding("UTF-8");
        response.setContentType("application/json");
        response.getWriter().println(JSONUtil.parse(CommonResult.forbidden(e.getMessage())));
        response.getWriter().flush();
    }
}
~~~

~~~java
/**
 * SpringSecurity需要的用户详情
 */
public class AdminUserDetails implements UserDetails {
    private UmsAdmin umsAdmin;
    private List<UmsPermission> permissionList;
    public AdminUserDetails(UmsAdmin umsAdmin, List<UmsPermission> permissionList) {
        this.umsAdmin = umsAdmin;
        this.permissionList = permissionList;
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        //返回当前用户的权限
        return permissionList.stream()
                .filter(permission -> permission.getValue()!=null)
                .map(permission ->new SimpleGrantedAuthority(permission.getValue()))
                .collect(Collectors.toList());
    }

    @Override
    public String getPassword() {
        return umsAdmin.getPassword();
    }

    @Override
    public String getUsername() {
        return umsAdmin.getUsername();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return umsAdmin.getStatus().equals(1);
    }
}
~~~

> 总结

- RestAuthenticationEntryPoint：切入点，认证失败返回JSON。
  - 实现AuthenticationEntryPoint切入点类。
- RestfulAccessDeniedHandler：拒绝访问处理，返回JSON。
  - 实现AccessDeniedHandler处理器类。
- OncePerRequestFilter：过滤器，校验并获取token中的用户信息，校验通过则认证完成。
  - 继承OncePerRequestFilter过滤器类
- SecurityConfig：配置类，过滤请求，认证授权。
  - 继承WebSecurityConfigurerAdapter配置适配器类
- AdminUserDetails：用户及权限。
  - 实现UserDetails接口

##### JWTUtils

~~~java
/**
 * JwtToken生成的工具
 */
@Component
@Slf4j
public class JwtTokenUtil {
    private static final String CLAIM_KEY_USERNAME = "sub";
    private static final String CLAIM_KEY_CREATED = "created";
    @Value("${jwt.secret}")
    private String secret;
    @Value("${jwt.expiration}")
    private Long expiration;

    /**
     * 根据负责生成JWT的token
     */
    private String generateToken(Map<String, Object> claims) {
        return Jwts.builder()
                .setClaims(claims)
                .setExpiration(generateExpirationDate())
                .signWith(SignatureAlgorithm.HS512, secret)
                .compact();
    }

    /**
     * 从token中获取JWT中的负载
     */
    private Claims getClaimsFromToken(String token) {
        Claims claims = null;
        try {
            claims = Jwts.parser()
                    .setSigningKey(secret)
                    .parseClaimsJws(token)
                    .getBody();
        } catch (Exception e) {
            log.info("JWT格式验证失败:{}",token);
        }
        return claims;
    }

    /**
     * 生成token的过期时间
     */
    private Date generateExpirationDate() {
        return new Date(System.currentTimeMillis() + expiration * 1000);
    }

    /**
     * 从token中获取登录用户名
     */
    public String getUserNameFromToken(String token) {
        String username;
        try {
            Claims claims = getClaimsFromToken(token);
            username =  claims.getSubject();
        } catch (Exception e) {
            username = null;
        }
        return username;
    }

    /**
     * 验证token是否还有效
     *
     * @param token       客户端传入的token
     * @param userDetails 从数据库中查询出来的用户信息
     */
    public boolean validateToken(String token, UserDetails userDetails) {
        String username = getUserNameFromToken(token);
        return username.equals(userDetails.getUsername()) && !isTokenExpired(token);
    }

    /**
     * 判断token是否已经失效
     */
    private boolean isTokenExpired(String token) {
        Date expiredDate = getExpiredDateFromToken(token);
        return expiredDate.before(new Date());
    }

    /**
     * 从token中获取过期时间
     */
    private Date getExpiredDateFromToken(String token) {
        Claims claims = getClaimsFromToken(token);
        return claims.getExpiration();
    }

    /**
     * 根据用户信息生成token
     */
    public String generateToken(UserDetails userDetails) {
        Map<String, Object> claims = new HashMap<>();
        claims.put(CLAIM_KEY_USERNAME, userDetails.getUsername());
        claims.put(CLAIM_KEY_CREATED, new Date());
        return generateToken(claims);
    }

    /**
     * 判断token是否可以被刷新
     */
    public boolean canRefresh(String token) {
        return !isTokenExpired(token);
    }

    /**
     * 刷新token
     */
    public String refreshToken(String token) {
        Claims claims = getClaimsFromToken(token);
        claims.put(CLAIM_KEY_CREATED, new Date());
        return generateToken(claims);
    }
}
~~~

##### 接口授权

~~~java
// 注解Controller方法 pms:brand:read为权限值，通过数据库可以查到真实的权限接口路径
@PreAuthorize("hasAuthority('pms:brand:read')")
~~~

##### 测试

> 获取用户对应的接口权限

~~~sql
-- 获取用户对应的权限
-- getPermissionList
SELECT p.*
-- 用户对应的角色
FROM ums_admin_role_relation ar 
LEFT JOIN ums_role r ON ar.role_id = r.id 
-- 角色对应的权限
LEFT JOIN ums_role_permission_relation rp ON r.id = rp.role_id
LEFT JOIN ums_permission p ON rp.permission_id = p.id
WHERE ar.admin_id = 3 AND p.id IS NOT NULL
AND p.id NOT IN ( -- 排除减去的权限
		-- 查询用户对应的权限，-权限(type=-1)
		SELECT p.id
		FROM ums_admin_permission_relation pr
		LEFT JOIN ums_permission p ON pr.permission_id = p.id
		WHERE pr.type = - 1 AND pr.admin_id = 3
)
UNION
-- 查询用户对应的权限，额外+权限(type=1)
SELECT p.*
FROM ums_admin_permission_relation pr
LEFT JOIN ums_permission p ON pr.permission_id = p.id
WHERE pr.type = 1 AND pr.admin_id = 3
~~~

>登录测试

~~~json
POST http://localhost:8081/mall/admin/login
{
    "username": "admin",
    "password": "macro123"
}

Response:
{
    "code": 200,
    "message": "操作成功",
    "data": {
        "tokenHead": "Bearer",
        "token": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNyZWF0ZWQiOjE2Mjc3MzA4MTMxNDUsImV4cCI6MTYyODMzNTYxM30.YHsWOamgWl41FIMXrRydax3S_3mH4yKoM8ldywXD-bmv79ZbIjC1zSx-eNF4ltRnotaCmjf0cH_1M4sHYfelGw"
    }
}
~~~

> 请求登录受限资源

~~~json
GET http://localhost:8081/mall/brand/list
Authorization: Bearer # 设置请求头 bearer授权
eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNyZWF0ZWQiOjE2Mjc3MzA4MTMxNDUsImV4cCI6MTYyODMzNTYxM30.YHsWOamgWl41FIMXrRydax3S_3mH4yKoM8ldywXD-bmv79ZbIjC1zSx-eNF4ltRnotaCmjf0cH_1M4sHYfelGw

Response:
{
    "code": 200,
    "message": "操作成功",
    "data": {...}
}

# 没登录/无token 响应：
{
    "code": 401,
    "data": "Full authentication is required to access this resource",
    "message": "暂未登录或token已经过期"
}
# 无权限 响应：
# 全局异常需捕获AccessDeniedException
{
    "code": 403,
    "message": "没有相关权限",
    "data": null
}
~~~

#### Elasticsearch

##### pom.xml

~~~xml
<!-- Elasticsearch相关依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
~~~

##### application.yml

~~~yml
spring:
  data:
    elasticsearch:
      client:
        reactive:
          endpoints: 127.0.0.1:9200
          username: elasticsearch
~~~

##### 报错

> 可以忽略，表示pms这个索引未找到。

URI [/pms?ignore_throttled=false&include_type_name=true&ignore_unavailable=false&expand_wildcards=open&allow_no_indices=true], status line [HTTP/1.1 400 Bad Request]]; nested: ResponseException[method [HEAD], host [http://localhost:9200]

>版本不匹配

Elasticsearch exception [type=illegal_argument_exception, reason=request [/pms/_refresh] contains unrecognized parameter: [ignore_throttled]]

|                  Spring Data Elasticsearch                   | Elasticsearch | Spring Boot |
| :----------------------------------------------------------: | :-----------: | :---------: |
|                            4.2.1                             |    7.12.1     |    2.5.x    |
|                            4.1.x                             |     7.9.3     |    2.4.x    |
|                            4.0.x                             |     7.6.2     |    2.3.x    |
|                            3.2.x                             |    6.8.12     |    2.2.x    |
| 3.1.x[[1](https://docs.spring.io/spring-data/elasticsearch/docs/4.2.3/reference/html/#_footnotedef_1)] |     6.2.2     |    2.1.x    |
| 3.0.x[[1](https://docs.spring.io/spring-data/elasticsearch/docs/4.2.3/reference/html/#_footnotedef_1)] |     5.5.0     |    2.0.x    |
| 2.1.x[[1](https://docs.spring.io/spring-data/elasticsearch/docs/4.2.3/reference/html/#_footnotedef_1)] |     2.4.0     |    1.5.x    |

原本：spring-boot2.2，安装elasticsearch6.5.2。所以springboot对应的elasticsearch6.8.12对不上，需要覆盖高版本依赖：

~~~xml
<!-- 与安装版本一致 -->
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>6.5.2</version>
</dependency>
~~~

#### MongoDB

##### pom.xml

~~~xml
<!---mongodb相关依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
~~~

##### application.yml

~~~yml
spring:
  data:
    mongodb:
      host: localhost
      port: 27017
      database: mall-port
~~~

#### RabbitMQ

##### Docker安装

~~~shell
# 1、拉取镜像
docker pull rabbitmq:management

# 2、运行容器
# -e RABBITMQ_DEFAULT_USERR=guest -e RABBITMQ_DEFAULT_PASS=guest
docker run -di --hostname mall --name rabbitmq -p 15672:15672 -p 5672:5672 rabbitmq:management

# 3、登录
http://127.0.0.1:15672/

# web页面进不去？
docker exec -it rabbitmq /bin/bash
# 启动web插件
rabbitmq-plugins enable rabbitmq_management
# 退出容器
exit


# 登录不了？可能是guest不允许远程访问
# 进入容器
docker exec -it rabbitmq /bin/bash

# 解决方式一（推荐）
# 创建用户 mall,密码mall
rabbitmqctl add_user mall mall
# 设置角色tags，多个用空格隔开
rabbitmqctl set_user_tags mall administrator
# 设置用户权限
rabbitmqctl set_permissions -p "/" mall ".*" ".*" ".*"

# 查看用户列表
rabbitmqctl list_users
# 重置密码
rabbitmqctl change_password guest 'guest'

# 解决方式二
# 进入容器
docker exec -it rabbitmq /bin/bash
# /etc/rabbitmq/conf.d/xxx.conf 设置允许远程访问loopback_users = none
vim xxx.conf
# 更新软件包
apt-get update
# 安装vim
apt-get install vim

~~~

https://www.rabbitmq.com/access-control.html

> 端口

- 5672：rabbitMQ的编程语言客户端连接端口
- 15672：rabbitMQ管理界面端口
- 25672：rabbitMQ集群的端口

> Tags

- 超级管理员(administrator)
  - 可登陆管理控制台，可查看所有的信息，并且可以对用户，策略(policy)进行操作。

- 监控者(monitoring)
  - 可登陆管理控制台，同时可以查看rabbitmq节点的相关信息(进程数，内存使用情况，磁盘使用情况等)

- 策略制定者(policymaker)
  - 可登陆管理控制台, 同时可以对policy进行管理。但无法查看节点的相关信息(上图红框标识的部分)。

- 普通管理者(management)
  - 仅可登陆管理控制台，无法看到节点信息，也无法对策略进行管理。

- 其他
  - 无法登陆管理控制台，通常就是普通的生产者和消费者。

##### pom.xml

~~~xml
<!-- 消息队列相关依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
~~~

##### application.yml

~~~yml
spring:
  rabbitmq:
    host: localhost # rabbitmq的连接地址
    port: 5672 # rabbitmq的连接端口号
    virtual-host: /mall # rabbitmq的虚拟host，需在Admin => Virtual Hosts中创建
    username: mall # rabbitmq的用户名
    password: mall # rabbitmq的密码
    publisher-confirms: true #如果对异步消息需要回调必须设置为true
~~~

##### 报错

> Rabbit health check failed

关闭Actuator监听RabbitMQ

~~~yml
management:
  health:
    rabbit:
      enabled: false
~~~

##### 命名

- 命名规范：平台名称/作用/[队列特点/路由特点]/容器名称
  - mall.order.direct (订单，交换机)
    - 绑定队列mall.order.cancel（订单取消，队列）
  - mall.order.direct.ttl（订单，延时，交换机）
    - 绑定队列mall.order.cancel.ttl（订单取消，延时队列）
    - 超时自动发送给mall.order.cancel队列，自动取消订单。
- 容器名称：
  - queue（队列）
  - exchange（交换机）
- 队列特点：
  - 非持久化标记（undurable）
  - 延时队列（delay）
  - 优先队列（priority）
- 路由特点：
  - direct（直接）
  - topic（主题）
  - fanout（扇区）
  - headers（头信息）
- 作用（举例）：
  - order（订单）



### 问题汇总

#### JDK版本

> Handler dispatch failed； nested exception is java.lang.NoClassDefFoundError: javax/xml/bind/**

缺少jaxb-api包导致。在JDK8以及之前是包含这个包的，而在JDK8之后的版本中是不再包含这个包了。一般常用JDK8，而我使用的是JDK11，所有在使用比JDK8更高版本的时候，就有可能会遇到这种问题。需手动引入相关包：

~~~xml
<dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>2.3.0</version>
</dependency>
<dependency>
    <groupId>com.sun.xml.bind</groupId>
    <artifactId>jaxb-impl</artifactId>
    <version>2.3.0</version>
</dependency>
<dependency>
    <groupId>com.sun.xml.bind</groupId>
    <artifactId>jaxb-core</artifactId>
    <version>2.3.0</version>
</dependency>
<dependency>
    <groupId>javax.activation</groupId>
    <artifactId>activation</artifactId>
    <version>1.1.1</version>
</dependency>
~~~

#### Mybatis-plus

> java.lang.IllegalArgumentException: Could not find class [org.mybatis.spring.boot.autoconfigure.MybatisAutoConfiguration]

由于使用mybatis-plus必须移除mybatis，导致缺少mybatis的自动配置，所以需要手动引入：

~~~xml
<!-- mybatis autoconfigure -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-autoconfigure</artifactId>
    <version>2.1.1</version>
</dependency>
~~~

#### Springboot版本

由于从springboot2.1.x升级到了springboot2.2.8，导致编译报错，缺少实体类的校验相关注解，所以需要手动引入：

~~~xml
<!-- validator -->
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>7.0.0.Final</version>
</dependency>
~~~





### 拓展学习

#### Mybatis-Plus

##### pom.xml

~~~xml
<!-- MyBatis分页插件 -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.10</version>
    <!-- 去除MyBatis -->
    <exclusions>
        <exclusion>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<!-- 替换为MyBatis-plus -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
~~~

##### application.yml

~~~yml
#mybatis:
#  mapper-locations:
#    - classpath:mapper/*.xml
#    - classpath*:com/**mapper/*.xml
mybatis-plus: # mybatis/mybatis-plus皆可
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath*:com/**mapper/*.xml
  type-aliases-package: com.belean.mall.tiny.mbg.model
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    cache-enabled: false # 是否开启二级缓存
    map-underscore-to-camel-case: true # 开启驼峰命名
~~~

##### BaseMapper

~~~java
public interface PmsBrandMapper extends BaseMapper<PmsBrand> {
}
// Mapper CRUD: select、insert、update、delete 
// Wrapper: QueryWrapper、UpdateWrapper
~~~

##### Config

~~~java
@Configuration
@MapperScan("com.belean.mall.tiny.mbg.mapper")
public class MyBatisConfig {
    // 返回map自动转驼峰命名
    @Bean
    public ConfigurationCustomizer mybatisConfigurationCustomizer(){
        return new ConfigurationCustomizer() {

            @Override
            public void customize(MybatisConfiguration configuration) {
                // TODO Auto-generated method stub
                configuration.setObjectWrapperFactory(new MybatisMapWrapperFactory());
            }
        };
    }
}
~~~

##### 注解实体类

~~~java
@TableName(value = "pms_brand") // 注解类
// ASSIGN_ID雪花算法，ASSIGN_UUID算法
@TableId(value = "id", type = IdType.ASSIGN_ID) // 注解ID属性
// exist是否为数据库表字段，默认true
@TableField(value = "name", exist = true) // 注解属性
~~~

##### 连接加密

~~~java
/**
 * Created by belean on 2021/7/15.
 **/
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = Application.class)
public class EncryptTest {

    @Autowired
    private Environment environment;
    
    @Test
    public void testEncrypt() {
        String url = environment.getProperty("spring.datasource.url");
        String userName = environment.getProperty("spring.datasource.username");
        String password = environment.getProperty("spring.datasource.password");
        // 生成 16 位随机 AES 密钥
        String randomKey = AES.generateRandomKey();
        System.out.println(randomKey);
        // 随机密钥加密
        String urlEncrypt = AES.encrypt(url, randomKey);
        String userNameEncrypt = AES.encrypt(userName, randomKey);
        String passwordEncrypt = AES.encrypt(password, randomKey);
        System.out.println(urlEncrypt);
        System.out.println(userNameEncrypt);
        System.out.println(passwordEncrypt);
    }
    
    @Test
    public void testDecrypt() {
        String url = AES.decrypt("3IhnGN9tTjg6gBUNhSNYO8uIIj9ESlebse0dyxHUcXlRMQacFowlmXgYbe/EACZCAvrxM3Yy2TLoci2Ep661NY/9G1jxdvX3RK5l0bKwNY7vtd46Gd4a6v2lvqXsAp9Z8ez15jcFmyEQ3hI6eqokp23ZNNluTDWpeG1moLDr5LY="
                , "7e7275f1c40b844d");
        System.out.println(url);
    }
}
7e7275f1c40b844d // 保存好秘钥以便解密
3IhnGN9tTjg6gBUNhSNYO8uIIj9ESlebse0dyxHUcXlRMQacFowlmXgYbe/EACZCAvrxM3Yy2TLoci2Ep661NY/9G1jxdvX3RK5l0bKwNY7vtd46Gd4a6v2lvqXsAp9Z8ez15jcFmyEQ3hI6eqokp23ZNNluTDWpeG1moLDr5LY=
LIu3l07HH9BIQmUvHLBUDQ==
LIu3l07HH9BIQmUvHLBUDQ==
~~~

~~~yml
#spring:
#  datasource:
#    url: jdbc:mysql://localhost:3306/mall?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai&useSSL=false
#    username: root
#    password: root
spring:
  datasource:
    url: mpw:3IhnGN9tTjg6gBUNhSNYO8uIIj9ESlebse0dyxHUcXlRMQacFowlmXgYbe/EACZCAvrxM3Yy2TLoci2Ep661NY/9G1jxdvX3RK5l0bKwNY7vtd46Gd4a6v2lvqXsAp9Z8ez15jcFmyEQ3hI6eqokp23ZNNluTDWpeG1moLDr5LY=
    username: mpw:LIu3l07HH9BIQmUvHLBUDQ==
    password: mpw:LIu3l07HH9BIQmUvHLBUDQ==
~~~

> 注意：都以mpw:开头



> IDEA Run/Debug Configurations => Program arguments

~~~shell
--mpw.key=7e7275f1c40b844d # 或 java -jar mall.jar --mpw.key=7e7275f1c40b844d
~~~

##### 二级缓存Redis

> application.yml

~~~yml
mybatis-plus:
  configuration:
    cache-enabled: true # 开启二级缓存
~~~

> redis相关整合，请往下翻

> MybatisRedisCache.java

~~~java
/**
 * MyBatis二级缓存Redis实现
 * 重点处理以下几个问题
 * 1、缓存穿透：存储空值解决，MyBatis框架实现
 * 2、缓存击穿：使用互斥锁，我们自己实现
 * 3、缓存雪崩：缓存有效期设置为一个随机范围，我们自己实现
 * 4、读写性能：redis key不能过长，会影响性能，这里使用SHA-256计算摘要当成key
 */
@Slf4j
public class MybatisRedisCache implements Cache {

    /**
     * 统一字符集
     */
    private static final String CHARSET = "utf-8";
    /**
     * key摘要算法
     */
    private static final String ALGORITHM = "SHA-256";
    /**
     * 统一缓存头
     */
    private static final String CACHE_NAME = "MyBatis:";
    /**
     * 读写锁：解决缓存击穿
     */
    private final ReadWriteLock readWriteLock = new ReentrantReadWriteLock();
    /**
     * 表空间ID：方便后面的缓存清理
     */
    private final String id;
    /**
     * redis服务接口：提供基本的读写和清理
     */
    private static volatile RedisUtils redisUtils;
    /**
     * 信息摘要
     */
    private volatile MessageDigest messageDigest;

    /////////////////////// 解决缓存雪崩，具体范围根据业务需要设置合理值 //////////////////////////
    /**
     * 缓存最小有效期 秒 默认60分钟
     */
    private static final int MIN_EXPIRE_SECOND = 60 * 60;
    /**
     * 缓存最大有效期 秒 默认120分钟
     */
    private static final int MAX_EXPIRE_SECOND = 120 * 60;

    /**
     * MyBatis给每个表空间初始化的时候要用到
     * @param id 其实就是namespace的值
     */
    public MybatisRedisCache(String id) {
        if (id == null) {
            throw new IllegalArgumentException("Cache instances require an ID");
        }
        this.id = id;
    }

    /**
     * 获取ID
     * @return 真实值
     */
    @Override
    public String getId() {
        return id;
    }

    /**
     * 创建缓存
     * @param key 其实就是sql语句
     * @param value sql语句查询结果
     */
    @Override
    public void putObject(Object key, Object value) {
        try {
            String strKey = getKey(key);
            // 有效期为1~2小时之间随机，防止雪崩
            int expireMinutes = RandomUtil.randomInt(MIN_EXPIRE_SECOND, MAX_EXPIRE_SECOND);
            getRedisUtils().set(strKey, value, expireMinutes);
            log.debug("Put cache to redis, id={}", id);
        } catch (Exception e) {
            log.error("Redis put failed, id=" + id, e);
        }
    }

    /**
     * 读取缓存
     * @param key 其实就是sql语句
     * @return 缓存结果
     */
    @Override
    public Object getObject(Object key) {
        try {
            String strKey = getKey(key);
            log.debug("Get cache from redis, id={}", id);
            return getRedisUtils().get(strKey);
        } catch (Exception e) {
            log.error("Redis get failed, fail over to db", e);
            return null;
        }
    }

    /**
     * 删除缓存
     * @param key 其实就是sql语句
     * @return 结果
     */
    @Override
    public Object removeObject(Object key) {
        try {
            String strKey = getKey(key);
            getRedisUtils().del(strKey);
            log.debug("Remove cache from redis, id={}", id);
        } catch (Exception e) {
            log.error("Redis remove failed", e);
        }
        return null;
    }

    /**
     * 缓存清理
     * 网上好多博客这里用了flushDb甚至是flushAll，感觉好坑鸭！
     * 应该是根据表空间进行清理
     */
    @Override
    public void clear() {
        try {
            log.debug("clear cache, id={}", id);
            String hsKey = CACHE_NAME + id;
            // 获取CacheNamespace所有缓存key
            Map<Object, Object> idMap = getRedisUtils().hmget(hsKey);
            if (!idMap.isEmpty()) {
                // 清空CacheNamespace所有缓存
                Set<Object> keySet = idMap.keySet();
                keySet.forEach(item -> {
                    getRedisUtils().hdel(hsKey, item);
                });
                // 清空CacheNamespace
                getRedisUtils().del(hsKey);
            }
        } catch (Exception e) {
            log.error("clear cache failed", e);
        }
    }

    /**
     * 获取缓存大小，暂时没用上
     * @return 长度
     */
    @Override
    public int getSize() {
        return 0;
    }

    /**
     * 获取读写锁：为了解决缓存击穿
     * @return 锁
     */
    @Override
    public ReadWriteLock getReadWriteLock() {
        return readWriteLock;
    }

    /**
     * 计算出key的摘要
     * @param cacheKey CacheKey
     * @return 字符串key
     */
    private String getKey(Object cacheKey) {
        String cacheKeyStr = cacheKey.toString();
        log.debug("count hash key, cache key origin string:{}", cacheKeyStr);
        String strKey = byte2hex(getSHADigest(cacheKeyStr));
        log.debug("hash key:{}", strKey);
        String key = CACHE_NAME + strKey;
        // 在redis额外维护CacheNamespace创建的key，clear的时候只清理当前CacheNamespace的数据
        getRedisUtils().hset(CACHE_NAME + id, key, "1");
        return key;
    }

    /**
     * 获取信息摘要
     * @param data 待计算字符串
     * @return 字节数组
     */
    private byte[] getSHADigest(String data) {
        try {
            if (messageDigest == null) {
                synchronized (MessageDigest.class) {
                    if (messageDigest == null) {
                        messageDigest = MessageDigest.getInstance(ALGORITHM);
                    }
                }
            }
            return messageDigest.digest(data.getBytes(CHARSET));
        } catch (Exception e) {
            log.error("SHA-256 digest error: ", e);
            throw new GlobalException(500, "SHA-256 digest error, id=" + id +  ".");
        }
    }

    /**
     * 字节数组转16进制字符串
     * @param bytes 待转换数组
     * @return 16进制字符串
     */
    private String byte2hex(byte[] bytes) {
        StringBuilder sign = new StringBuilder();
        for (byte aByte : bytes) {
            String hex = Integer.toHexString(aByte & 0xFF);
            if (hex.length() == 1) {
                sign.append("0");
            }
            sign.append(hex.toUpperCase());
        }
        return sign.toString();
    }

    /**
     * 获取Redis服务接口
     * 使用双重检查保证线程安全
     * @return 服务实例
     */
    private RedisUtils getRedisUtils() {
        if (redisUtils == null) {
            synchronized (RedisUtils.class) {
                if (redisUtils == null) {
                    redisUtils = SpringUtil.getBean(RedisUtils.class);
                }
            }
        }
        return redisUtils;
    }
}
~~~

> 使用

注解Mapper接口 或 配置Mapper.XML

~~~java
@CacheNamespace(implementation = MybatisRedisCache.class)
public interface PmsBrandMapper extends BaseMapper<PmsBrand> {
}
~~~

~~~xml
<cache type="com.belean.mall.tiny.config.mybatis.MybatisRedisCache"/>
~~~

##### MybatisX

安装插件：IDEA `File` => `Settings` => `Plugins` => 搜索`MybatisX` => `install`

调出视图：IDEA `View` => `Tool Windows` => `Database`

连接数据库：Open `Database` => + Data Source

生成代码：选择数据库表（可多选）右击 => `MybatisX-generator` 

~~~
# 例如，要生成的的目录：
# com.belean.mall.entity
# com.belean.mall.mapper
# 注：xml使用与mapper同包名，编译后会使两者在一起
# resources/com.belean.mall.mapper

base package：com.belean.mall # 起始包路径
relative package: entity # 实体类包

Next

annotation：Mybatis-Plus3
options：Comment（注释）、Lombok、Actual Column
template：mybatis-plus3
删除serviceImpl和serviceInterface，或修改其模板，因为生成后的代码不太懂

也就是说我们这里只生成了entity、mapper、mapper.xml
~~~

> 修改模板

随便选择一个表右击，选择`Jump to Query Console...`  => `Jump to Query Console Files`

定位到目录后，找到MybatisX

`Scratches and Consoles` => `Extensions` => `MybatisX`

修改对应模板代码

比如使用`template`：`custom-model-swagger`，或者Copy一个模板`my-template`

- 修改`mapperInterface.ftl`，给其加上缓存注解
- 修改`domain.ftl`，修改为swagger3注解，注解lombok，生成时则取消选择lombox
  - `import`也要贴上

> 可以大胆的修改，如需重置模板，可以先删除模板，然后选择`MybatisX`目录右击`Restore Default Extension`

Freemarker手册

http://www.kerneler.com/freemarker2.3.23/ref_builtins_string.html

https://www.jianshu.com/p/c488709d6430

#### BindingResult 参数校验

##### Annotation

~~~java
// 实体类属性注解
@Null // 必须为null
@NotNull // 不能为null
@AssertTrue // 必须为true
@AssertFalse // 必须为false
@Min(value) // 数字且必须大于等于这个最小值
@Max(value) // 数字且必须小于等于这个最大值
@DecimalMin("value") // 校验同上，精确到小数
@DecimalMax("value") // 校验同上，精确到小数
@Size(max,min) // size必须是该范围内的值，可以是字符串（length）、数组、集合、map等
@Digits(integer,fraction) // integer最大整数位数，fraction最大小数位数
@Past // 必须是过去的日期
@Future // 必须是将来的日期
@Pattern(regexp="[abc]") // 正则校验
@Email // 必须是电子邮箱
@Length(max=5,min=1,message="长度必须在1~5") // 校验字符长度
@Range(max=5,min=1,message="长度必须在1~5") // 校验数值范围
@CreditCardNumber // 对信用卡号大致校验
@NotBlank // 不能为空null or ' '.trim()
@NotEmpty // 不能为空''，空字符
// 以上注解都有message参数，可设置对应提示消息
~~~

##### Controller

~~~java
@ApiOperation("添加品牌")
    @PostMapping("/create")
    public CommonResult createBrand(@ApiParam("品牌实体") @Valid @RequestBody PmsBrand pmsBrand, BindingResult result) {
        // 校验
        if(result.hasErrors()) { // 是否有参数异常
            List<FieldError> fieldErrors = result.getFieldErrors();// 获取异常参数集
            fieldErrors.forEach(fieldError -> {
                String field = fieldError.getField(); // 异常参数名
//                Object value = fieldError.getRejectedValue();// 异常参数值
//                boolean bindingFailure = fieldError.isBindingFailure(); // true表示类型不匹配，false表示校验失败
                String message = fieldError.getDefaultMessage();// 异常消息
                LOGGER.error("参数：{}，{}", field, message);
            });
            for (FieldError fieldError : fieldErrors) {
                return CommonResult.validateFailed(fieldError.getDefaultMessage());
            }
        }

        CommonResult commonResult;
        int count = demoService.createBrand(pmsBrand);
        if (count == 1) {
            commonResult = CommonResult.success(pmsBrand);
            LOGGER.debug("createBrand success:{}", pmsBrand);
        } else {
            commonResult = CommonResult.failed("操作失败");
            LOGGER.debug("createBrand failed:{}", pmsBrand);
        }
        return commonResult;
    }
~~~

~~~
# 日志输出
参数：productCount，产品数量不能为负数！

# 请求响应数据
{
  "code": 404,
  "message": "产品数量不能为负数！",
  "data": null
}
~~~

- @Valid // 注解要校验的实体参数

- BindingResult result // 紧跟@Valid（要校验的实体类）参数后。

> 缺点：一但注解，全部地方都得按这套规则校验。比如：添加需要校验全部，而按条件查询则不需要校验那么多。
>
> 不过，一般添加、修改可以用固定一套校验，而查询、删除则一般不需要校验，或手动少量校验。

#### Swagger3

基于OpenApi3，由Swagger维护，OpenApi被Linux列为Api标准。Swagger2也基于OpenApi3，但于2017年停止维护。最新的Swagger3提供了新一批注解，更加简单规范化，同时也兼容Swagger2注解。

##### pom.xml

~~~xml
<dependency>
     <groupId>io.springfox</groupId>
      <artifactId>springfox-boot-starter</artifactId>
      <version>3.0.0</version>
</dependency>
<!-- 更新依赖的插件 -->
<dependency>
    <groupId>org.springframework.plugin</groupId>
    <artifactId>spring-plugin-core</artifactId>
    <version>2.0.0.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.plugin</groupId>
    <artifactId>spring-plugin-metadata</artifactId>
    <version>2.0.0.RELEASE</version>
</dependency>
~~~

##### application.yml

~~~yml
springfox:
  documentation:
    swagger-ui:
      enabled: false # 关闭文档
~~~

##### Config

~~~java
@Configuration
@EnableOpenApi
public class Swagger3Config {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.OAS_30)
                .apiInfo(apiInfo())
                .select()
                // 为当前包下controller生成API文档
                .apis(RequestHandlerSelectors.basePackage("com.belean.mall.tiny.controller"))
                // 为有@Api注解的Controller生成API文档
//                .apis(RequestHandlerSelectors.withClassAnnotation(Api.class))
                // 为有@ApiOperation注解的方法生成API文档
//                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Mall Api Document") // 项目标题
                .description("mall-study") // 项目描述
                // 联系人 网站 邮箱
                .contact(new Contact("belean", "https://github.com/liuconglook", "liuconglook@gmail.com"))
                .license("Apache LICENSE 2.0") // 开源协议
                .licenseUrl("http://www.apache.org/") // 开源协议地址
                .termsOfServiceUrl("") // 服务条款地址
                .version("1.0") // 版本
                .build();
    }
}
~~~

##### Annotation

> io.swagger.v3.oas.annotations

~~~java
// 接口信息
@Tag(name = "品牌管理接口") // 类 hidden=true忽略
@Operation(tags = "品牌管理接口", summary = "分页查询") // 方法
@Parameter(description = "页码") // 形参 hidden=true忽略

@Parameters({
    @Parameter(name = "pageNum", description = "页码", example = "1"),
    @Parameter(name = "pageSize", description = "页数", example = "3")
}) // 方法

// 忽略
@Hidden // 方法

// 实体信息
@Schema(description = "品牌实体") // 类/属性
~~~

##### 查看文档（地址有变）

- http://localhost:8081/mall/v3/api-docs
- http://localhost:8081/mall/swagger-ui/index.html

##### knife4j

对springfox支持的swaggerUI

~~~xml
<!--整合Knife4j-->
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>3.0.3</version>
</dependency>
~~~

地址：http://localhost:8081/mall/doc.html

#### Druid

Druid最好的数据库连接池，虽然比性能最好的连接池HikariCP差点，但好在有丰富的监控统计等功能。

##### pom.xml

~~~xml
<!--集成druid连接池-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.10</version>
</dependency>
~~~

##### application.yml

~~~yml
spring:
  datasource:
    druid:
      url: mpw:3IhnGN9tTjg6gBUNhSNYO8uIIj9ESlebse0dyxHUcXlRMQacFowlmXgYbe/EACZCAvrxM3Yy2TLoci2Ep661NY/9G1jxdvX3RK5l0bKwNY7vtd46Gd4a6v2lvqXsAp9Z8ez15jcFmyEQ3hI6eqokp23ZNNluTDWpeG1moLDr5LY=
      username: mpw:LIu3l07HH9BIQmUvHLBUDQ==
      password: mpw:LIu3l07HH9BIQmUvHLBUDQ==
      driver-class-name: com.mysql.cj.jdbc.Driver
      initialSize: 5 #初始化连接大小
      minIdle: 5     #最小连接池数量
      maxActive: 20  #最大连接池数量
      maxWait: 200000 #获取连接时最大等待时间，单位毫秒
      timeBetweenEvictionRunsMillis: 60000 #配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
      minEvictableIdleTimeMillis: 300000   #配置一个连接在池中最小生存的时间，单位是毫秒
      validationQuery: SELECT 1 from DUAL  #测试连接
      testWhileIdle: true                  #申请连接的时候检测，建议配置为true，不影响性能，并且保证安全性
      testOnBorrow: false                  #获取连接时执行检测，建议关闭，影响性能
      testOnReturn: false                  #归还连接时执行检测，建议关闭，影响性能
      poolPreparedStatements: false       #是否开启PSCache，PSCache对支持游标的数据库性能提升巨大，oracle建议开启，mysql下建议关闭
      maxPoolPreparedStatementPerConnectionSize: 20 #开启poolPreparedStatements后生效
      filters: stat,wall,slf4j   #配置扩展插件，常用的插件有=>stat:监控统计  slf4j:日志  wall:防御sql注入
      connectionProperties: 'druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000'
      #配置监控属性： 在druid-starter的： com.alibaba.druid.spring.boot.autoconfigure.stat包下进行的逻辑配置
      web-stat-filter:  # WebStatFilter配置，
        enabled: true #默认为false，表示不使用WebStatFilter配置，就是属性名去短线
        url-pattern: /* #拦截该项目下的一切请求
        exclusions: /druid/*,*.js,*.gif,*.jpg,*.bmp,*.png,*.css,*.ico  #对这些请求放行
        session-stat-enable: true
        principal-session-name: session_name
        principal-cookie-name: cookie_name
      stat-view-servlet: # StatViewServlet配置
        enabled: true #默认为false，表示不使用StatViewServlet配置，就是属性名去短线
        url-pattern: /druid/*  #配置DruidStatViewServlet的访问地址。后台监控页面的访问地址
        reset-enable: false #禁用HTML页面上的“重置”功能，会把所有监控的数据全部清空，一般不使用
        login-username: admin #监控页面登录的用户名
        login-password: 123456 #监控页面登录的密码
        allow: 127.0.0.1  #IP白名单(没有配置或者为空，则允许所有访问)。允许谁访问druid后台，默认允许全部用户访问。
        deny:  #IP黑名单 (存在共同时，deny优先于allow)。不允许谁访问druid后台
~~~

##### 监控

http://localhost:8081/mall/druid/index.html

#### Lombok

开发工具安装对应Lombok插件

##### pom.xml

~~~xml
<!-- Lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.20</version>
</dependency>
~~~

##### Annotation

~~~java
@NoArgsConstructor // 无参构造
@AllArgsConstructor // 全参构造
@RequiredArgsConstructor // 为私有、常量、not null的属性构造，一般用于替代@Autowired

@Getter // get method
@Setter // set method
@ToString // toString
@EqualsAndHashCode // equals、hashCode

@Data // getter、setter、toString、EqualsAndHashCode、NoArgsConstructor

// 对log4j/Slf4j等日志框架的支持，使用：log.info("");
@Log4j 或 @Slf4j
~~~

#### Log4j2

##### pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <!-- 去除默认的logback -->
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<!-- log4j2 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
~~~

##### log4j2-spring.xml

> 自定义配置文件名，需配置application.yml

~~~yml
logging:
  config: classpath:log4j2-spring.xml # 默认 log4j2-spring.xml
  level:
    cn.jay.repository: trace
~~~

> log4j2-spring.xml

https://logging.apache.org/log4j/2.x/manual/layouts.html

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!--Configuration后面的status，这个用于设置log4j2自身内部的信息输出，可以不设置，当设置成trace时，你会看到log4j2内部各种详细输出-->
<!--monitorInterval：Log4j能够自动检测修改配置 文件和重新配置本身，设置间隔秒数-->
<configuration monitorInterval="5">
    <!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL -->

    <!-- 定义变量 -->
    <Properties>
        <!-- 格式化输出：%date表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度 %msg：日志消息，%n是换行符-->
        <!-- %logger{36} 表示 Logger 名字最长36个字符 -->
        <property name="LOG_PATTERN" value="%highlight{%date{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %m%n}{FATAL=red, ERROR=red, WARN=yellow, INFO=green}" />
        <!-- 定义日志存储的路径 -->
        <property name="FILE_PATH" value="E://mallLogs" />
        <property name="FILE_NAME" value="mall" />
    </Properties>

    <!-- 输出源 -->
    <Appenders>
        <!-- 控制台日志输出 -->
        <Console name="Console" target="SYSTEM_OUT">
            <!--输出日志的格式-->
            <PatternLayout pattern="${LOG_PATTERN}"/>
            <!--控制台只输出level及其以上级别的信息（onMatch），其他的直接拒绝（onMismatch）-->
            <ThresholdFilter level="info" onMatch="ACCEPT" onMismatch="DENY"/>
        </Console>

        <!-- 保存日志到文件，每次运行append追加或不追加，一般测试用 -->
        <File name="FileLog" fileName="${FILE_PATH}/test.log" append="false">
            <PatternLayout pattern="${LOG_PATTERN}"/>
        </File>

        <!-- INFO LOG -->
        <!-- name:名称，fileName：日志文件名，filePattern：日志压缩文件名，%i：文件序号 -->
        <RollingFile name="RollingFileInfo" fileName="${FILE_PATH}/info.log" filePattern="${FILE_PATH}/${FILE_NAME}-INFO-%d{yyyy-MM-dd}_%i.log.gz">
            <!-- 接受（ACCEPT）level及以上级别的信息（onMatch），拒绝（DENY）其他级别信息（onMismatch），NEUTRAL中立-->
            <ThresholdFilter level="info" onMatch="ACCEPT" onMismatch="DENY"/>
            <!-- 日志格式 -->
            <PatternLayout pattern="${LOG_PATTERN}"/>
            <!-- 滚存策略：每过1小时 或 日志达到 10MB 则压缩一次-->
            <Policies>
                <!-- 基于时间：interval默认是1 hour -->
                <TimeBasedTriggeringPolicy interval="24"/>
                <!-- 基于文件大小：size文件大小KB/MB/GB -->
                <SizeBasedTriggeringPolicy size="10MB"/>
            </Policies>
            <!-- 默认的滚存策略：max默认7，与%i相呼应，当计数到15时，旧文件会被删除-->
            <DefaultRolloverStrategy max="15"/>
        </RollingFile>

        <!-- WARN LOG -->
        <RollingFile name="RollingFileWarn" fileName="${FILE_PATH}/warn.log" filePattern="${FILE_PATH}/${FILE_NAME}-WARN-%d{yyyy-MM-dd}_%i.log.gz">
            <ThresholdFilter level="warn" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="${LOG_PATTERN}"/>
            <Policies>
                <TimeBasedTriggeringPolicy interval="24"/>
                <SizeBasedTriggeringPolicy size="10MB"/>
            </Policies>
            <DefaultRolloverStrategy max="15"/>
        </RollingFile>

        <!-- ERROR LOG -->
        <RollingFile name="RollingFileError" fileName="${FILE_PATH}/error.log" filePattern="${FILE_PATH}/${FILE_NAME}-ERROR-%d{yyyy-MM-dd}_%i.log.gz">
            <ThresholdFilter level="error" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="${LOG_PATTERN}"/>
            <Policies>
                <TimeBasedTriggeringPolicy interval="24"/>
                <SizeBasedTriggeringPolicy size="10MB"/>
            </Policies>
            <DefaultRolloverStrategy max="15"/>
        </RollingFile>

    </Appenders>

    <!--Logger节点用来单独指定日志的形式，比如要为指定包下的class指定不同的日志级别等。-->
    <!--然后定义loggers，只有定义了logger并引入的appender，appender才会生效-->
    <Loggers>

        <!--过滤掉spring和mybatis的一些无用的DEBUG信息-->
        <Logger name="org.mybatis" level="info" additivity="false">
            <AppenderRef ref="Console"/>
        </Logger>
        <!--监控系统信息-->
        <!--若是additivity设为false，则 子Logger 只会在自己的appender里输出，而不会在 父Logger 的appender里输出。-->
        <Logger name="org.springframework" level="info" additivity="false">
            <AppenderRef ref="Console"/>
        </Logger>

        <Root level="info">
            <appender-ref ref="Console"/>
            <appender-ref ref="FileLog"/>
            <appender-ref ref="RollingFileInfo"/>
            <appender-ref ref="RollingFileWarn"/>
            <appender-ref ref="RollingFileError"/>
        </Root>
    </Loggers>

</configuration>
~~~

> 日志目录结构

~~~
|-mallLogs
	|-error.log
	|-info.log
	|-test.log
	|-warn.log
	|-mall-INFO-2021-07-18_1.log.gz # %i序号
~~~

- 日志级别：`FATAL` > `ERROR >` `WARN` > `INFO` > `DEBUG` > `TRACE` > `ALL`
  - 致命错误（EOF）、异常/错误（Error）、警告（Warning）、记录信息（INFO）、调试信息（Debug）、追踪信息（TRACE）、所有信息（ALL）
  - 级别机制：只会输出`大于等于`配置级别的日志信息
- Appenders（输出源）
  - Console：控制台
  - File：文件
  - RollingFile：文件，与File不同的是，能滚存。
  - SMTP：邮箱
  - Async：异步地写入多个不同输出地
  - Flume：汇总不同源的日志
- 格式
  - SimpleLayout：简单格式
  - HTMLLayout：HTML表格
  - PatternLayout：自定义格式

##### 输出格式

https://www.docs4dev.com/docs/zh/log4j2/2.x/all/manual-layouts.html#patterns

~~~
%d/%date{yyyy-MM-dd HH:mm:ss SSS} # 时间格式
%-Slevel/%p # 日志级别
# 正数右边数显示10个层级；负数左边数删除10个层级
# {1.}除类名，包名缩写；{.}除类名，包名全省略
%c/%logger{10} # 日志名称 打印由使用时传进来的类
%t/%thread # 当前线程名称
%tp/%threadPriority # 线程优先级
%m/%msg/%message # 日志内容 
%n # 换行

%C/%class # 类名
%fqcn # 全限定类名
%M # 方法名
%L # 行号
%l # 类名、方法名、文件名、行号

hostName # 主机名称
hostAddress # 主机IP

%20m # 日志消息，不满20字符，右补空格
%-20m # 日志消息，不满20字符，左补空格
%.20m # 日志消息，最多保存/显示20字符
# 正数：右补白；负数：左补白；小数：可显示字符数

# 判断
%replace{pattern}{regex}{substitution} # 正则查找替换
%maxLen/%maxLength{10} # 长度超过10，则显示...省略号
~~~

##### 彩色日志

> IDEA Run/Debug Configurations => VM options

~~~shell
-Dlog4j.skipJansi=false # 不关闭ANSI颜色库
~~~

> 修改日志格式

~~~shell
# 根据不同日志级别显示不同颜色
# FATAL=red, ERROR=red, WARN=yellow, INFO=cyan, DEBUG=cyan,TRACE=blue
%highlight{格式}{FATAL=red, ERROR=red, WARN=yellow, INFO=black}
~~~

##### 邮箱日志

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
~~~

> log4j2-spring.xml

QQ邮箱：开启POP3/SMTP服务，获取密码(授权码)，端口465/587

~~~xml
		<!-- 1、SMTP：邮件发送日志-->
        <!-- subject:邮箱标题；to:收件人列表逗号隔开；from:发件人；smtpUsername:smtp账号；smtpPassword:smtp密码 -->
        <!-- smtpHost:主机ip；smtpPort:端口；smtpProtocol:协议默认smtp -->
        <!-- smtpDebug:是否调试；bufferSize:默认512(最大日志事件数) -->
        <SMTP name="Mail" subject="MALL系统异常信息" to="liuconglook@gmail.com,lceclipse@outlook.com" from="liuconglook@qq.com"
              smtpUsername="liuconglook@qq.com" smtpPassword="db2dcz*****eruse" smtpHost="smtp.qq.com"
              smtpDebug="false" smtpPort="587" bufferSize="512" smtpProtocol="smtp">
            <!-- 默认HTMLayout -->
            <!--<PatternLayout pattern="${LOG_PATTERN}" />
            <ThresholdFilter level="error" onMatch="ACCEPT" onMismatch="DENY"/>-->
        </SMTP>
        <!-- 2、定义异步发通知邮件AsyncAppender属性-->
        <Async name="AsyncMail">
            <appender-ref ref="Mail"/>
        </Async>

		<!-- 3、-->
		<Root level="info">
            <appender-ref ref="AsyncMail"/>
        </Root>
~~~

##### 使用

~~~java
// 使用lombok注解代替
@Slf4j
public class PmsBrandController {
    // private static final Logger log = LoggerFactory.getLogger(PmsBrandController.class);
}

// log.info("info:{}", "msg");
~~~

#### Redis

Jedis：redis直连客户端，多线程环境下不安全。

Lettuce：分布式的redis连接池，多线程安全的。

##### pom.xml

~~~xml
<!-- redis springboot2.x默认lettuce客户端 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!-- lettuce pool 缓存连接池 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>

<!-- 切换jedis客户端 -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
	<exclusions>
		<exclusion>
			<groupId>io.lettuce</groupId>
			<artifactId>lettuce-core</artifactId>
		</exclusion>
	</exclusions>
</dependency>
<dependency>
	<groupId>redis.clients</groupId>
	<artifactId>jedis</artifactId>
</dependency>
~~~

##### application.yml

~~~yml
spring:
  redis:
    database: 0  # Redis几号库
    host: 127.0.0.1  # Redis服务器地址
    port: 6379  # Redis服务器连接端口
    password: redis # Redis服务器连接密码（默认为空）
    timeout: 10000 # 超时时间（毫秒）
    lettuce: # 或 jedis
      pool:
        max-active: 200 # 连接池最大连接数（使用负值表示没有限制）
        max-wait: -1ms # 连接池最大阻塞等待时间（使用负值表示没有限制）
        max-idle: 8 # 连接池中的最大空闲连接
        min-idle: 0 # 连接池中的最小空闲连接
~~~

##### Config

~~~java
@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate redisTemplate(LettuceConnectionFactory connectionFactory) {
        RedisTemplate redisTemplate = new RedisTemplate<>();

        // key serializer
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        redisTemplate.setKeySerializer(stringRedisSerializer);
        redisTemplate.setHashKeySerializer(stringRedisSerializer);

        // value serializer
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);

        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        // 将类名称序列化到json串中，去掉会导致得出来的的是LinkedHashMap对象，直接转换实体对象会失败
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        //设置输入时忽略JSON字符串中存在而Java对象实际没有的属性
        objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);

        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);

        // 开启事务
        redisTemplate.setEnableTransactionSupport(true);

        redisTemplate.setConnectionFactory(connectionFactory);

        redisTemplate.afterPropertiesSet();

        return redisTemplate;
    }

    @Bean
    @ConditionalOnMissingBean(StringRedisTemplate.class)
    public StringRedisTemplate stringRedisTemplate(LettuceConnectionFactory connectionFactory) throws UnknownHostException {
        StringRedisTemplate template = new StringRedisTemplate();
        template.setConnectionFactory(connectionFactory);
        return template;
    }
}
~~~

##### 工具类

~~~java
/**
 * redis 工具类
 **/
 @Component
 public class RedisUtils {
   /**
    * 注入redisTemplate bean
    */
   @Autowired
   private RedisTemplate<String,Object> redisTemplate;

   /**
    * 指定缓存失效时间
    *
    * @param key  键
    * @param time 时间(秒)
    * @return
    */
   public boolean expire(String key, long time) {
     try {
       if (time > 0) {
         redisTemplate.expire(key, time, TimeUnit.SECONDS);
       }
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 根据key获取过期时间
    *
    * @param key 键 不能为null
    * @return 时间(秒) 返回0代表为永久有效
    */
   public long getExpire(String key) {
     return redisTemplate.getExpire(key, TimeUnit.SECONDS);
   }

   /**
    * 判断key是否存在
    *
    * @param key 键
    * @return true 存在 false不存在
    */
   public boolean hasKey(String key) {
     try {
       return redisTemplate.hasKey(key);
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 删除缓存
    *
    * @param key 可以传一个值 或多个
    */
   @SuppressWarnings("unchecked")
   public void del(String... key) {
     if (key != null && key.length > 0) {
       if (key.length == 1) {
         redisTemplate.delete(key[0]);
       } else {
         redisTemplate.delete(CollectionUtils.arrayToList(key));
       }
     }
   }
   // ============================String(字符串)=============================

   /**
    * 普通缓存获取
    *
    * @param key 键
    * @return 值
    */
   public Object get(String key) {
     return key == null ? null : redisTemplate.opsForValue().get(key);
   }

   /**
    * 普通缓存放入
    *
    * @param key   键
    * @param value 值
    * @return true成功 false失败
    */
   public boolean set(String key, Object value) {
     try {
       redisTemplate.opsForValue().set(key, value);
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 普通缓存放入并设置时间
    *
    * @param key   键
    * @param value 值
    * @param time  时间(秒) time要大于0 如果time小于等于0 将设置无限期
    * @return true成功 false 失败
    */
   public boolean set(String key, Object value, long time) {
     try {
       if (time > 0) {
         redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
       } else {
         set(key, value);
       }
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 递增
    *
    * @param key   键
    * @param delta 要增加几(大于0)
    * @return
    */
   public long incr(String key, long delta) {
     if (delta < 0) {
       throw new RuntimeException("递增因子必须大于0");
     }
     return redisTemplate.opsForValue().increment(key, delta);
   }

   /**
    * 递减
    *
    * @param key   键
    * @param delta 要减少几(小于0)
    * @return
    */
   public long decr(String key, long delta) {
     if (delta < 0) {
       throw new RuntimeException("递减因子必须大于0");
     }
     return redisTemplate.opsForValue().increment(key, -delta);
   }
   // ================================Hash(哈希)=================================

   /**
    * HashGet
    *
    * @param key  键 不能为null
    * @param item 项 不能为null
    * @return 值
    */
   public Object hget(String key, String item) {
     return redisTemplate.opsForHash().get(key, item);
   }

   /**
    * 获取hashKey对应的所有键值
    *
    * @param key 键
    * @return 对应的多个键值
    */
   public Map<Object, Object> hmget(String key) {
     return redisTemplate.opsForHash().entries(key);
   }

   /**
    * HashSet
    *
    * @param key 键
    * @param map 对应多个键值
    * @return true 成功 false 失败
    */
   public boolean hmset(String key, Map <String, Object> map) {
     try {
       redisTemplate.opsForHash().putAll(key, map);
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * HashSet 并设置时间
    *
    * @param key  键
    * @param map  对应多个键值
    * @param time 时间(秒)
    * @return true成功 false失败
    */
   public boolean hmset(String key, Map <String, Object> map, long time) {
     try {
       redisTemplate.opsForHash().putAll(key, map);
       if (time > 0) {
         expire(key, time);
       }
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 向一张hash表中放入数据,如果不存在将创建
    *
    * @param key   键
    * @param item  项
    * @param value 值
    * @return true 成功 false失败
    */
   public boolean hset(String key, String item, Object value) {
     try {
       redisTemplate.opsForHash().put(key, item, value);
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 向一张hash表中放入数据,如果不存在将创建
    *
    * @param key   键
    * @param item  项
    * @param value 值
    * @param time  时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
    * @return true 成功 false失败
    */
   public boolean hset(String key, String item, Object value, long time) {
     try {
       redisTemplate.opsForHash().put(key, item, value);
       if (time > 0) {
         expire(key, time);
       }
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 删除hash表中的值
    *
    * @param key  键 不能为null
    * @param item 项 可以使多个 不能为null
    */
   public void hdel(String key, Object... item) {
     redisTemplate.opsForHash().delete(key, item);
   }

   /**
    * 判断hash表中是否有该项的值
    *
    * @param key  键 不能为null
    * @param item 项 不能为null
    * @return true 存在 false不存在
    */
   public boolean hHasKey(String key, String item) {
     return redisTemplate.opsForHash().hasKey(key, item);
   }

   /**
    * hash递增 如果不存在,就会创建一个 并把新增后的值返回
    *
    * @param key  键
    * @param item 项
    * @param by   要增加几(大于0)
    * @return
    */
   public double hincr(String key, String item, double by) {
     return redisTemplate.opsForHash().increment(key, item, by);
   }

   /**
    * hash递减
    *
    * @param key  键
    * @param item 项
    * @param by   要减少记(小于0)
    * @return
    */
   public double hdecr(String key, String item, double by) {
     return redisTemplate.opsForHash().increment(key, item, -by);
   }
   // ============================Set(集合)=============================

   /**
    * 根据key获取Set中的所有值
    *
    * @param key 键
    * @return
    */
   public Set<Object> sGet(String key) {
     try {
       return redisTemplate.opsForSet().members(key);
     } catch (Exception e) {
       e.printStackTrace();
       return null;
     }
   }

   /**
    * 根据value从一个set中查询,是否存在
    *
    * @param key   键
    * @param value 值
    * @return true 存在 false不存在
    */
   public boolean sHasKey(String key, Object value) {
     try {
       return redisTemplate.opsForSet().isMember(key, value);
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 将数据放入set缓存
    *
    * @param key    键
    * @param values 值 可以是多个
    * @return 成功个数
    */
   public long sSet(String key, Object... values) {
     try {
       return redisTemplate.opsForSet().add(key, values);
     } catch (Exception e) {
       e.printStackTrace();
       return 0;
     }
   }

   /**
    * 将set数据放入缓存
    *
    * @param key    键
    * @param time   时间(秒)
    * @param values 值 可以是多个
    * @return 成功个数
    */
   public long sSetAndTime(String key, long time, Object... values) {
     try {
       Long count = redisTemplate.opsForSet().add(key, values);
       if (time > 0)
         expire(key, time);
       return count;
     } catch (Exception e) {
       e.printStackTrace();
       return 0;
     }
   }

   /**
    * 获取set缓存的长度
    *
    * @param key 键
    * @return
    */
   public long sGetSetSize(String key) {
     try {
       return redisTemplate.opsForSet().size(key);
     } catch (Exception e) {
       e.printStackTrace();
       return 0;
     }
   }

   /**
    * 移除值为value的
    *
    * @param key    键
    * @param values 值 可以是多个
    * @return 移除的个数
    */
   public long setRemove(String key, Object... values) {
     try {
       Long count = redisTemplate.opsForSet().remove(key, values);
       return count;
     } catch (Exception e) {
       e.printStackTrace();
       return 0;
     }
   }
   // ===============================List(列表)=================================

   /**
    * 获取list缓存的内容
    *
    * @param key   键
    * @param start 开始
    * @param end   结束 0 到 -1代表所有值
    * @return
    */
   public List<Object> lGet(String key, long start, long end) {
     try {
       return redisTemplate.opsForList().range(key, start, end);
     } catch (Exception e) {
       e.printStackTrace();
       return null;
     }
   }

   /**
    * 获取list缓存的长度
    *
    * @param key 键
    * @return
    */
   public long lGetListSize(String key) {
     try {
       return redisTemplate.opsForList().size(key);
     } catch (Exception e) {
       e.printStackTrace();
       return 0;
     }
   }

   /**
    * 通过索引 获取list中的值
    *
    * @param key   键
    * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
    * @return
    */
   public Object lGetIndex(String key, long index) {
     try {
       return redisTemplate.opsForList().index(key, index);
     } catch (Exception e) {
       e.printStackTrace();
       return null;
     }
   }

   /**
    * 将list放入缓存
    *
    * @param key   键
    * @param value 值
    * @return
    */
   public boolean lSet(String key, Object value) {
     try {
       redisTemplate.opsForList().rightPush(key, value);
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 将list放入缓存
    *
    * @param key   键
    * @param value 值
    * @param time  时间(秒)
    * @return
    */
   public boolean lSet(String key, Object value, long time) {
     try {
       redisTemplate.opsForList().rightPush(key, value);
       if (time > 0)
         expire(key, time);
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 将list放入缓存
    *
    * @param key   键
    * @param value 值
    * @return
    */
   public boolean lSet(String key, List <Object> value) {
     try {
       redisTemplate.opsForList().rightPushAll(key, value);
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 将list放入缓存
    *
    * @param key   键
    * @param value 值
    * @param time  时间(秒)
    * @return
    */
   public boolean lSet(String key, List <Object> value, long time) {
     try {
       redisTemplate.opsForList().rightPushAll(key, value);
       if (time > 0)
         expire(key, time);
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 根据索引修改list中的某条数据
    *
    * @param key   键
    * @param index 索引
    * @param value 值
    * @return
    */
   public boolean lUpdateIndex(String key, long index, Object value) {
     try {
       redisTemplate.opsForList().set(key, index, value);
       return true;
     } catch (Exception e) {
       e.printStackTrace();
       return false;
     }
   }

   /**
    * 移除N个值为value
    *
    * @param key   键
    * @param count 移除多少个
    * @param value 值
    * @return 移除的个数
    */
   public long lRemove(String key, long count, Object value) {
     try {
       Long remove = redisTemplate.opsForList().remove(key, count, value);
       return remove;
     } catch (Exception e) {
       e.printStackTrace();
       return 0;
     }
   }
 }
~~~







#### 异常处理

##### GlobalException

~~~java
/**
 * 全局异常
 * Created by belean on 2021/7/25.
 **/
public class GlobalException extends RuntimeException {

    private long code;

    private String message;

    /**
     * 传递 自定义、已知的 异常枚举类
     * @param iErrorCode
     */
    public GlobalException(IErrorCode iErrorCode){
        super(iErrorCode.getMessage());
        this.code = iErrorCode.getCode();
        this.message = iErrorCode.getMessage();
    }

    /**
     * 传递 异常枚举之外的 或 需要细分的 异常码和信息
     * @param code
     * @param message
     */
    public GlobalException(long code, String message){
        super(message);
        this.code = code;
        this.message = message;
    }

    public long getCode() {
        return code;
    }

    @Override
    public String getMessage() {
        return message;
    }
}
~~~

~~~java
/**
 * 封装API的错误码
 */
public interface IErrorCode {
    long getCode();

    String getMessage();
}
/**
 * 枚举了一些常用API操作码
 */
public enum ResultCode implements IErrorCode {
    SUCCESS(200, "操作成功"),
    FAILED(500, "操作失败"),
    VALIDATE_FAILED(404, "参数检验失败"),
    UNAUTHORIZED(401, "暂未登录或token已经过期"),
    FORBIDDEN(403, "没有相关权限");
    private long code;
    private String message;

    private ResultCode(long code, String message) {
        this.code = code;
        this.message = message;
    }

    public long getCode() {
        return code;
    }

    public String getMessage() {
        return message;
    }
}
~~~

##### GlobalExceptionHandler.java

~~~java
/**
 * 全局异常处理
 * Created by belean on 2021/7/25.
 **/
@ControllerAdvice
@Slf4j
public class GlobalExceptionHandler {

    /**
     * 全局异常处理，返回JSON
     * @param request
     * @param exception
     * @return
     */
    @ExceptionHandler(value = GlobalException.class)
    @ResponseBody
    public CommonResult globalException(HttpServletRequest request, GlobalException exception) {
        log.error("全局的异常消息：{}", exception.getMessage());
        return new CommonResult(exception.getCode(), exception.getMessage(), null);
    }

    /**
     * 未知或未捕捉到的异常，统一返回失败
     * @param request
     * @param exception
     * @return
     */
    @ExceptionHandler(value = Exception.class)
    @ResponseBody
    public CommonResult exception(HttpServletRequest request, Exception exception) {
        log.error("未处理的异常消息：{}", exception);
        return CommonResult.failed();
    }

}
~~~

~~~java
/**
 * 通用返回对象
 */
@Schema(description = "通用返回对象")
public class CommonResult<T> {
    @Schema(description = "响应码")
    private long code;
    @Schema(description = "响应消息")
    private String message;
    @Schema(description = "响应数据")
    private T data;

    protected CommonResult() {
    }

    public CommonResult(long code, String message, T data) {
        this.code = code;
        this.message = message;
        this.data = data;
    }

    /**
     * 成功返回结果
     *
     * @param data 获取的数据
     */
    public static <T> CommonResult<T> success(T data) {
        return new CommonResult<T>(ResultCode.SUCCESS.getCode(), ResultCode.SUCCESS.getMessage(), data);
    }

    /**
     * 成功返回结果
     *
     * @param data 获取的数据
     * @param  message 提示信息
     */
    public static <T> CommonResult<T> success(T data, String message) {
        return new CommonResult<T>(ResultCode.SUCCESS.getCode(), message, data);
    }

    /**
     * 失败返回结果
     * @param errorCode 错误码
     */
    public static <T> CommonResult<T> failed(IErrorCode errorCode) {
        return new CommonResult<T>(errorCode.getCode(), errorCode.getMessage(), null);
    }

    /**
     * 失败返回结果
     * @param message 提示信息
     */
    public static <T> CommonResult<T> failed(String message) {
        return new CommonResult<T>(ResultCode.FAILED.getCode(), message, null);
    }

    /**
     * 失败返回结果
     */
    public static <T> CommonResult<T> failed() {
        return failed(ResultCode.FAILED);
    }

    /**
     * 参数验证失败返回结果
     */
    public static <T> CommonResult<T> validateFailed() {
        return failed(ResultCode.VALIDATE_FAILED);
    }

    /**
     * 参数验证失败返回结果
     * @param message 提示信息
     */
    public static <T> CommonResult<T> validateFailed(String message) {
        return new CommonResult<T>(ResultCode.VALIDATE_FAILED.getCode(), message, null);
    }

    /**
     * 未登录返回结果
     */
    public static <T> CommonResult<T> unauthorized(T data) {
        return new CommonResult<T>(ResultCode.UNAUTHORIZED.getCode(), ResultCode.UNAUTHORIZED.getMessage(), data);
    }

    /**
     * 未授权返回结果
     */
    public static <T> CommonResult<T> forbidden(T data) {
        return new CommonResult<T>(ResultCode.FORBIDDEN.getCode(), ResultCode.FORBIDDEN.getMessage(), data);
    }

    public long getCode() {
        return code;
    }

    public void setCode(long code) {
        this.code = code;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }
}
~~~

##### 总结

- IErrorCode
  - ResultCode

- GlobalException
- GlobalExceptionHandler
- CommonResult

首先枚举了一些常见异常信息（ResultCode），然后定义一个运行时异常（GlobalException）用于程序手动抛出，然后交由GlobalExceptionHandler异常处理器，捕获到指定异常（GlobalException）后，返回JSON数据（CommonResult）。

#### 部署

##### spring-boot-maven-plugin

> pom.xml

~~~xml
<!-- 指定打包方式 -->
<packaging>jar</packaging>
<build>
    <plugins>
        <!-- springboot整合maven打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>2.3.12.RELEASE</version>
        </plugin>
    </plugins>
</build>
~~~

~~~shell
# 执行了两个动作
# mvn package:maven打包jar/war，例如：xxx.jar.origin
# spring-boot:repackage在此基础上打包成可执行jar/war，例如：xxx.jar
mvn package spring-boot:repackage
~~~

##### profile

>不同环境下的配置文件

~~~
application.yml # 主配置文件，存放公共配置
application-dev.yml # 开发环境
application-test.yml # 测试环境
application-prod.yml # 生产环境
~~~

~~~yml
# application.yml
spring:
  profiles:
    active: dev # 指定开发环境，实际配置为：主配置+application-dev.yml的配置
~~~

或者

~~~java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = Application.class)
@ActiveProfiles({"test"}) // 启用测试环境配置
public class MybatisRedisCacheTest {}
~~~

> 不同环境下的配置类

~~~java
@Profile({"dev","test"}) // 仅开发、测试时启用
@Configuration
@EnableOpenApi
public class Swagger3Config {}
~~~

> 不同环境下的依赖项

~~~xml
<packaging>${package.target}</packaging>
<profiles>
    <profile>
        <!-- dev 开发环境 jar -->
        <id>dev</id>
        <!-- 默认启用 -->
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <!-- 开发环境下的属性配置：打包、包名、激活配置 -->
        <properties>
            <package.target>jar</package.target>
            <package.name>${project.artifactId}-${project.version}</package.name>
            <profiles.active>dev</profiles.active>
        </properties>
        <!-- 开发环境下的依赖项 -->
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </dependency>
        </dependencies>
    </profile>
    <profile>
        <!-- test 测试环境 jar -->
        <id>test</id>
        <properties>
            <package.target>jar</package.target>
            <package.name>${project.artifactId}-${project.version}</package.name>
            <profiles.active>test</profiles.active>
        </properties>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </dependency>
        </dependencies>
    </profile>
    <profile>
        <!-- prod 生成环境 -->
        <id>prod</id>
        <properties>
            <package.target>war</package.target>
            <package.name>${project.artifactId}</package.name>
            <profiles.active>prod</profiles.active>
        </properties>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>javax.servlet</groupId>
                <artifactId>javax.servlet-api</artifactId>
                <version>4.0.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>javax.websocket</groupId>
                <artifactId>javax.websocket-api</artifactId>
                <version>1.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>
    </profile>
</profiles>
<build>
    <finalName>${package.name}</finalName>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>true</filtering><!-- 开启过滤替换@package.target@ -->
        </resource>
    </resources>
</build>
~~~

~~~yml
# application.yml
spring:
  profiles:
    active: @profiles.active@ # 动态加载profile配置的环境变量，打包时换上
~~~

~~~shell
# 激活对应的profile
mvn package -Ptest
~~~

总结：打包时指定对应profile，根据profile加载对应环境的配置文件、依赖项、环境参数，最终打包。

> 不同环境下的部署

~~~shell
# IDEA Run/Debug Configurations
-Dspring.profiles.active=test # VM Options
--spring.profiles.active=prod # Program Arguments

# jar
java -jar xxx.jar --spring.profiles.active=prod # Program Arguments
java -jar xxx.jar -Dspring.profiles.active=prod # VM Options

# war tomcat catalina.bat
set JAVA_OPTS = "-Dspring.profiles.active=prod"
~~~

> VM Options & Program Arguments & ENV

VM Options（JVM启动参数）

- 设置：-Dspring.profiles.active=test -Dkey=value # -D表示设置JVM系统属性
- 通过`System.getProperty("spring.profiles.active")`获取

Program Arguments（应用参数）

- 设置：--spring.profiles.active=test key=value key2=value2 # 多参数空格分隔

- 由主程序main接收args

- ~~~java
  public static void main(String[] args) {
      // spring实现了应用参数对yml配置参数的覆盖，JVM参数同样
      // 双杠--表示spring支持的特定参数传递方式
      SpringApplication.run(Application.class, args);
  }
  ~~~

ENV（Environment Variable系统环境变量）

- 设置：

  - windows环境配置key和value；

  - ~~~shell
    # Linux centOS
    # 方式一，临时立即生效，终端关闭后失效
    export key='value' # 如有空格等特殊字符需单引号包含
    # 方式二，编辑文件，刷新后永久生效，只对当前用户生效，所有用户/etc/profile
    vim ~/.bash_profile
    source ~/.bash_profile
    ~~~

- 通过`System.getenv("key")`获取

优先级：

- Program Arguments > VM Options > ENV > application.yml

#### Postman

- chrome插件：
  - ModHeader（设置头信息）
  - Postman Interceptor（结合postman客户端，收录接口请求信息）



#### 限流

http://www.gxitsky.com/article/1602940204731496#menu_14

https://blog.csdn.net/weixin_39723655/article/details/111395043

https://www.pdai.tech/md/spring/springboot-data-ratelimit.html

## Spring-Cloud







