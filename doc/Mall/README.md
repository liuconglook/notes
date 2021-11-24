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

> my-template

.meta.xml

~~~xml
<?xml version="1.0" encoding="utf-8" ?>
<templates>
    <template>
        <!-- 注意: 实体类无论如何都会生成 -->
        <!--domain 如果勾选则覆盖默认实体类生成规则-->
        <property name="configName" value="domain"/>
        <property name="configFile" value="domain.ftl"/>
        <property name="fileName" value="${domain.fileName}"/>
        <property name="suffix" value=".java"/>
        <property name="packageName" value="${domain.basePackage}.model"/>
        <property name="encoding" value="UTF-8"/>
        <property name="basePath" value="${domain.basePath}"/>
    </template>
    <template>
        <property name="configName" value="mapperInterface"/>
        <property name="configFile" value="mapperInterface.ftl"/>
        <property name="fileName" value="${domain.fileName}Mapper"/>
        <property name="suffix" value=".java"/>
        <property name="packageName" value="${domain.basePackage}.mapper"/>
        <property name="encoding" value="UTF-8"/>
        <property name="basePath" value="${domain.basePath}"/>
    </template>
    <template>
        <property name="configName" value="mapperXml"/>
        <property name="configFile" value="mapperXml.ftl"/>
        <property name="fileName" value="${domain.fileName}Mapper"/>
        <property name="suffix" value=".xml"/>
        <property name="packageName" value="mapper"/>
        <property name="encoding" value="UTF-8"/>
        <property name="basePath" value="src/main/resources/${domain.basePath}"/>
    </template>
</templates>
~~~

domain.ftl

~~~java
package ${domain.packageName};

<#list tableClass.allFields as field>
    <#if !field.fullTypeName?starts_with("java.lang") && !(field.columnIsArray)>
import ${field.fullTypeName};
    </#if>
</#list>
import com.baomidou.mybatisplus.annotation.IdType;
import org.hibernate.validator.constraints.Length;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import io.swagger.v3.oas.annotations.media.Schema;
import lombok.*;

/**
* ${tableClass.remark!}
* @TableName ${tableClass.tableName}
*/
@Schema(description = "${tableClass.remark!}")
@TableName(value = "${tableClass.tableName}")
@Getter
@Setter
@ToString
public class ${tableClass.shortClassName} {

<#list tableClass.allFields as field>

    /**
    * ${field.remark!}
    */<#if field.autoIncrement>${"\n    "}@TableId(value = "${field.columnName}", type = IdType.NONE)</#if>
    @Schema(description = "${field.remark!}")<#if field.jdbcType=="VARCHAR">${"\n    "}@Length(max= ${field.columnLength},message="编码长度不能超过${field.columnLength}")</#if>
    private ${field.shortTypeName} <#if field.fieldName?index_of("_") != -1>${field.fieldName?keep_before("_") + field.fieldName?keep_after("_")?capitalize?replace("_", "")}</#if><#if field.fieldName?index_of("_") == -1>${field.fieldName}</#if>;
</#list>
}
~~~

mapperInterface.ftl

~~~java
package ${mapperInterface.packageName};

import ${tableClass.fullClassName};
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.belean.mall.tiny.config.mybatis.MybatisRedisCache;
import org.apache.ibatis.annotations.CacheNamespace;

/**
 * @Entity ${tableClass.fullClassName}
 */
@CacheNamespace(implementation = MybatisRedisCache.class)
public interface ${mapperInterface.fileName} extends BaseMapper<${tableClass.shortClassName}> {
}
~~~

mapperXml.ftl

~~~java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="${mapperInterface.packageName}.${baseInfo.fileName}">

    <resultMap id="BaseResultMap" type="${tableClass.fullClassName}">
    <#list tableClass.pkFields as field>
        <id property="${field.fieldName}" column="${field.columnName}" jdbcType="${field.jdbcType}"/>
    </#list>
    <#list tableClass.baseFields as field>
    <#if field.fieldName?index_of("_") != -1>
        <result property="${field.fieldName?keep_before("_") + field.fieldName?keep_after("_")?capitalize?replace("_", "")}" column="${field.columnName}" jdbcType="${field.jdbcType}"/>
    </#if>
    <#if field.fieldName?index_of("_") == -1>
        <result property="${field.fieldName}" column="${field.columnName}" jdbcType="${field.jdbcType}"/>
    </#if>
    </#list>
    </resultMap>

    <sql id="Base_Column_List">
        <#list tableClass.allFields as field>`${field.columnName}`<#sep>,<#if field_index%3==2>${"\n        "}</#if></#list>
    </sql>
</mapper>
~~~

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

### 版本

~~~xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
~~~

#### 2020.0.4

spring-boot-starter-parent:2.5.5

https://cloud.tencent.com/developer/article/1770680

https://juejin.cn/post/6909609435946024974

| Netflix                     | 推荐替代品                               | 说明                                   |
| --------------------------- | ---------------------------------------- | -------------------------------------- |
| Hystrix                     | Resilience4j                             | Hystrix自己也推荐你使用它代替自己      |
| Hystrix Dashboard / Turbine | Micrometer + Monitoring System           | 说白了，监控这件事交给更专业的组件去做 |
| Ribbon                      | Spring Cloud Loadbalancer                | 忍不住了，Spring终究亲自出手           |
| Zuul 1                      | Spring Cloud Gateway                     | 忍不住了，Spring终究亲自出手           |
| Archaius 1                  | Spring Boot外部化配置 + Spring Cloud配置 | 比Netflix实现的更好、更强大            |

#### Hoxton.SR12

spring-boot-starter-parent:2.3.12.RELEASE

### Eureka

服务注册与发现

注册服务：用于保存各服务的ip地址等信息，同时监听各服务状态。

发现服务：可通过服务列表获取某个服务的实例地址进行服务间的调用。

#### Server

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 8001
spring:
  application:
    name: eureka-server # 服务名称
eureka:
  instance:
    hostname: localhost # 服务地址
  client:
    fetch-registry: false # 指定是否要从注册中心获取服务（注册中心不需要开启）
    register-with-eureka: false # 指定是否要注册到注册中心（注册中心不需要开启）
  server:
    enable-self-preservation: false # 关闭保护模式
~~~

- 保护模式
  - 默认90秒没有接受到某个服务的心跳时会注销这个服务，如果在某一时刻丢失过多（网络出故障了）服务就会进行保护模式，不再注销任何服务，直到故障恢复。
  - 该模式是针对网络异常的安全措施，以保证Eureka服务的健壮和稳定。

> 启动eureka服务

~~~java
@EnableEurekaServer
~~~

> 集群

- application-replica1.yml

~~~yml
server:
  port: 8002
spring:
  application:
    name: eureka-server
eureka:
  instance:
    hostname: replica1
  client:
    serviceUrl:
      defaultZone: http://replica2:8003/eureka/ #注册到另一个Eureka注册中心
    fetch-registry: true
    register-with-eureka: true
~~~

- application-replica2.yml

~~~yml
server:
  port: 8003
spring:
  application:
    name: eureka-server
eureka:
  instance:
    hostname: replica2
  client:
    serviceUrl:
      defaultZone: http://replica1:8002/eureka/ #注册到另一个Eureka注册中心
    fetch-registry: true
    register-with-eureka: true
~~~

- 配置本地host
  - 127.0.0.1 replica1
  - 127.0.0.1 replica2

#### Client

> pom.xml

~~~xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 8101 #运行端口号
spring:
  application:
    name: eureka-client #服务名称
eureka:
  client:
    register-with-eureka: true #注册到Eureka的注册中心
    fetch-registry: true #获取注册实例列表
    service-url:
      defaultZone: http://localhost:8001/eureka/ #配置注册中心地址
~~~

> 启动Eureka客户端

~~~java
@EnableDiscoveryClient
~~~

#### Security

带认证的Eureka服务注册中心

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 8004
spring:
  application:
    name: eureka-security-server
  security: #配置SpringSecurity登录用户名和密码
    user:
      name: mall
      password: mall
eureka:
  instance:
    hostname: localhost
  client:
    fetch-registry: false
    register-with-eureka: false
~~~

> 忽略对Eureka的跨站伪造，因为访问Eureka不需要跨站

~~~java
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().ignoringAntMatchers("/eureka/**");
        super.configure(http);
    }
}
~~~

> 启动Eureka服务端

~~~java
@EnableEurekaServer
~~~

> 客户端认证格式

~~~
http://${username}:${password}@${hostname}:${port}/eureka/
~~~

#### 常用配置

~~~yml
eureka:
  client: #eureka客户端配置
    register-with-eureka: true #是否将自己注册到eureka服务端上去
    fetch-registry: true #是否获取eureka服务端上注册的服务列表
    service-url:
      defaultZone: http://localhost:8001/eureka/ # 指定注册中心地址
    enabled: true # 启用eureka客户端
    registry-fetch-interval-seconds: 30 #定义去eureka服务端获取服务列表的时间间隔
  instance: #eureka客户端实例配置
    lease-renewal-interval-in-seconds: 30 #定义服务多久去注册中心续约
    lease-expiration-duration-in-seconds: 90 #定义服务多久不去续约认为服务失效
    metadata-map:
      zone: jiangsu #所在区域
    hostname: localhost #服务主机名称
    prefer-ip-address: false #是否优先使用ip来作为主机名
  server: #eureka服务端配置
    enable-self-preservation: false #关闭eureka服务端的保护机制
~~~

### Ribbon

负载均衡：主要给服务间调用及API网关转发提供负载均衡功能。

> 2020.0.0版本及之后版本，该模块被移除，替代品：Spring Cloud Loadbalancer

#### RestTemplate

RestTemplate是一个HTTP客户端，使用它可以方便的调用HTTP接口，支持GET/POST/DELETE等方法。

使用RestTemplate来调用其他服务，Ribbon可以很方便的实现负载均衡功能。

- 请求方法
  - get
  - post
  - delete
- 返回值
  - ForObject：将结果转换为指定的类型对象。
  - ForEntity：返回ResponseEntity对象。
- 参数
  - url：请求地址（支持字符串和URI对象）
  - responseType：指定响应的对象类型。
  - uriVariables：请求地址参数（支持可变参、Map）。
  - request：请求体对象。

#### Ribbon

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 8301
spring:
  application:
    name: ribbon-service
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8001/eureka/
service-url:
  user-service: http://user-service
~~~

> 赋予RestTemplate负载均衡@LoadBalanced

~~~java
@Configuration
public class RibbonConfig {

    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
~~~

#### 测试

> user-service（8201）

创建UserController

> ribbon-service（8301）

使用RestTemplate重写UserController

> 配置host域名

~~~
127.0.0.1 user-service
~~~

> 运行测试

启动eureka-server（8001）

启动ribbon-service（8301）

启动两个user-service（8201、8202）

访问`http://localhost:8301/user/1`，查看两个user-service后台打印。

#### 常用配置

~~~yml
ribbon:
  ConnectTimeout: 1000 #服务请求连接超时时间（毫秒）
  ReadTimeout: 3000 #服务请求处理超时时间（毫秒）
  OkToRetryOnAllOperations: true #对超时请求启用重试机制
  MaxAutoRetriesNextServer: 1 #切换重试实例的最大个数
  MaxAutoRetries: 1 # 切换实例后重试最大次数
  NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule #修改负载均衡算法
~~~

> 可对某个服务做负载均衡时，单独配置：

~~~yml
user-service:
  ribbon:
    # 同上
~~~

>NFLoadBalancerRuleClassName（负载均衡策略）

- com.netflix.loadbalancer.RandomRule：随机。
- com.netflix.loadbalancer.RoundRobinRule：轮询。
- com.netflix.loadbalancer.RetryRule：在RoundRobinRule的基础上添加重试机制，即在指定的重试时间内，反复使用线性轮询策略来选择可用实例；
- com.netflix.loadbalancer.WeightedResponseTimeRule：对RoundRobinRule的扩展，响应速度越快的实例选择权重越大，越容易被选择；
- com.netflix.loadbalancer.BestAvailableRule：选择并发较小的实例；
- com.netflix.loadbalancer.AvailabilityFilteringRule：先过滤掉故障实例，再选择并发较小的实例；
- com.netflix.loadbalancer.ZoneAwareLoadBalancer：采用双重过滤，同时过滤不是同一区域的实例和故障实例，选择并发较小的实例。

### Hystrix

服务容错保护，具备服务降级、服务熔断、线程隔离、请求缓存、请求合并及服务监控等功能。

> 2020.0.0版本及之后版本，该模块被移除，替代品：Resilience4j

#### pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
~~~

#### application.yml

~~~yml
server:
  port: 8401
spring:
  application:
    name: hystrix-service
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8001/eureka/
service-url:
  user-service: http://user-service
~~~

#### 启用

~~~java
@EnableCircuitBreaker
~~~

#### Annotation

@HystrixCommand

- fallbackMethod：指定服务降级处理方法；
- ignoreExceptions：忽略某些异常，不发生服务降级；
- commandKey：命令名称，用于区分不同的命令；
- groupKey：分组名称，Hystrix会根据不同的分组来统计命令的告警及仪表盘信息；
- threadPoolKey：线程池名称，用于划分线程池。

> 服务降级

- 关闭user-service服务后，访问不到就会执行降级的方法。
- 调用服务后抛出异常也会降级，可通过ignoreExceptions忽略异常，不降级处理。

~~~java
@HystrixCommand(fallbackMethod = "getDefaultUser")
public CommonResult getUser(Long id) {
    return restTemplate.getForObject(userServiceUrl + "/user/{1}", CommonResult.class, id);
}

public CommonResult getDefaultUser(@PathVariable Long id) {
    User defaultUser = new User(-1L, "defaultUser", "123456");
    return new CommonResult<>(defaultUser);
}
~~~

> 请求缓存

- @CacheRresult：开启缓存，默认所有参数作为缓存key。cacheKeyMethod指定缓存key的生成方法。
- @CacheKey：指定缓存的key，注解参数，可以指定参数或参数值为缓存key。
- @CacheRemove：移除缓存，需指定commandKey。

~~~java
@CacheResult(cacheKeyMethod = "getCacheKey")
@HystrixCommand(fallbackMethod = "getDefaultUser", commandKey = "getUserCache")
@Override
public CommonResult getUserCache(Long id) {
    logger.info("getUserCache id:{}", id);
    return restTemplate.getForObject(userServiceUrl + "/user/{1}", CommonResult.class, id);
}

@CacheRemove(commandKey = "getUserCache", cacheKeyMethod = "getCacheKey")
@HystrixCommand
@Override
public CommonResult removeCache(Long id) {
    logger.info("removeCache id:{}", id);
    return restTemplate.getForObject(userServiceUrl + "/user/{1}", CommonResult.class, id);
}

public String getCacheKey(Long id) {
    return String.valueOf(id);
}
~~~

需要在每次使用缓存的请求前后对HystrixRequestContext进行初始化和关闭，否则会报`Request caching is not available. Maybe you need to initialize the HystrixRequestContext?`错误。

~~~java
@Component
@WebFilter(urlPatterns = "/*",asyncSupported = true)
public class HystrixRequestContextFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HystrixRequestContext context = HystrixRequestContext.initializeContext();
        try {
            filterChain.doFilter(servletRequest, servletResponse);
        } finally {
            context.close();
        }
    }
}
~~~

> 请求合并

- @HystrixCollapser
  - batchMethod
  - collapseProperties
  - timerDelayInMilliseconds

~~~java

@HystrixCollapser(batchMethod = "getUserByIds", collapserProperties = {
            @HystrixProperty(name = "timerDelayInMilliseconds", value = "100")
})
@Override
public Future<User> getUserFuture(Long id) {
    return new AsyncResult<User>() {
        @Override
        public User invoke() {
            CommonResult commonResult = restTemplate.getForObject(userServiceUrl + "/user/{1}", CommonResult.class, id);
            Map data = (Map) commonResult.getData();
            User user = BeanUtil.mapToBean(data, User.class, true, CopyOptions.create());
            logger.info("getUserById username:{}", user.getUsername());
            return user;
        }
    };
}
@HystrixCommand
public List<User> getUserByIds(List<Long> ids) {
    logger.info("getUserByIds:{}", ids);
    CommonResult commonResult = restTemplate.getForObject(userServiceUrl + "/user/getUserByIds?ids={1}", CommonResult.class, CollUtil.join(ids, ","));
    return (List<User>) commonResult.getData();
}
~~~

#### 常用配置

~~~yml
hystrix:
  command: #用于控制HystrixCommand的行为
    default:
      execution:
        isolation:
          strategy: THREAD #控制HystrixCommand的隔离策略，THREAD->线程池隔离策略(默认)，SEMAPHORE->信号量隔离策略
          thread:
            timeoutInMilliseconds: 1000 #配置HystrixCommand执行的超时时间，执行超过该时间会进行服务降级处理
            interruptOnTimeout: true #配置HystrixCommand执行超时的时候是否要中断
            interruptOnCancel: true #配置HystrixCommand执行被取消的时候是否要中断
          timeout:
            enabled: true #配置HystrixCommand的执行是否启用超时时间
          semaphore:
            maxConcurrentRequests: 10 #当使用信号量隔离策略时，用来控制并发量的大小，超过该并发量的请求会被拒绝
      fallback:
        enabled: true #用于控制是否启用服务降级
      circuitBreaker: #用于控制HystrixCircuitBreaker的行为
        enabled: true #用于控制断路器是否跟踪健康状况以及熔断请求
        requestVolumeThreshold: 20 #超过该请求数的请求会被拒绝
        forceOpen: false #强制打开断路器，拒绝所有请求
        forceClosed: false #强制关闭断路器，接收所有请求
      requestCache:
        enabled: true #用于控制是否开启请求缓存
  collapser: #用于控制HystrixCollapser的执行行为
    default:
      maxRequestsInBatch: 100 #控制一次合并请求合并的最大请求数
      timerDelayinMilliseconds: 10 #控制多少毫秒内的请求会被合并成一个
      requestCache:
        enabled: true #控制合并请求是否开启缓存
  threadpool: #用于控制HystrixCommand执行所在线程池的行为
    default:
      coreSize: 10 #线程池的核心线程数
      maximumSize: 10 #线程池的最大线程数，超过该线程数的请求会被拒绝
      maxQueueSize: -1 #用于设置线程池的最大队列大小，-1采用SynchronousQueue，其他正数采用LinkedBlockingQueue
      queueSizeRejectionThreshold: 5 #用于设置线程池队列的拒绝阀值，由于LinkedBlockingQueue不能动态改版大小，使用时需要用该参数来控制线程数
~~~

### Hystrix Dashboard

熔断器监控

#### pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

#### application.yml

~~~yml
server:
  port: 8501
spring:
  application:
    name: hystrix-dashboard
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8001/eureka/
~~~

#### 启动

~~~java
@EnableHystrixDashboard
~~~

http://localhost:8501/hystrix

#### 监控单实例

被监控的实例需要暴露监控端点

~~~yml
management:
  endpoints:
    web:
      exposure:
        include: 'hystrix.stream' # 暴露hystrix监控端点
~~~

输入要监控的单实例地址，例如：http://localhost:8201/hystrix.stream

#### 监控集群实例

使用turbine监控集群实例，创建turbine-service服务

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-turbine</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 8601
spring:
  application:
    name: turbine-service
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8001/eureka/
turbine:
  app-config: hystrix-service #指定需要收集信息的服务名称
  cluster-name-expression: new String('default') #指定服务所属集群
  combine-host-port: true #以主机名和端口号来区分服务
~~~

> 启动

~~~java
@EnableTurbine
~~~

> 测试

测试集群hystrix-service（8401、8402）

需添加本地主机域名：hystrix-service

输入集群监控地址，例如：http://localhost:8601/turbine.stream

### OpenFeign

声明式的服务调用工具，它整合了Ribbon和Hystrix，拥有负载均衡和服务容错功能。

#### pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
~~~

#### application.yml

~~~yml
server:
  port: 8701
spring:
  application:
    name: feign-service
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8001/eureka/
~~~

#### 启用

~~~java
@EnableFeignClients
~~~

#### 使用

将UserController复制过来，改为UserService接口，保留其springMVC注解。

~~~java
@FeignClient(value = "user-service")
public interface UserService {
    
    @PostMapping("/user/create")
    public CommonResult create(@RequestBody User user);

    @GetMapping("/user/{id}")
    public CommonResult<User> getUser(@PathVariable Long id);

    @GetMapping("/user/getUserByIds")
    public CommonResult<List<User>> getUserByIds(@RequestParam List<Long> ids);

    @GetMapping("/user/getByUsername")
    public CommonResult<User> getByUsername(@RequestParam String username);

    @PostMapping("/user/update")
    public CommonResult update(@RequestBody User user);

    @PostMapping("/user/delete/{id}")
    public CommonResult delete(@PathVariable Long id);
}
~~~

再复制UserController，修改为直接调用UserService的方法。

~~~java
@RestController
@RequestMapping("/user")
public class UserController {
    
    @Autowired
    private UserService userService;

    @PostMapping("/create")
    public CommonResult create(@RequestBody User user) {
        return userService.create(user);
    }

    @GetMapping("/{id}")
    public CommonResult<User> getUser(@PathVariable Long id) {
        return userService.getUser(id);
    }

    @GetMapping("/getUserByIds")
    public CommonResult<List<User>> getUserByIds(@RequestParam List<Long> ids) {
        return userService.getUserByIds(ids);
    }

    @GetMapping("/getByUsername")
    public CommonResult<User> getByUsername(@RequestParam String username) {
        return userService.getByUsername(username);
    }

    @PostMapping("/update")
    public CommonResult update(@RequestBody User user) {
        return userService.update(user);
    }

    @PostMapping("/delete/{id}")
    public CommonResult delete(@PathVariable Long id) {
        return userService.delete(id);
    }
}
~~~

#### 测试负载均衡

启动两个user-service（8201、8202）

调用多次http://localhost:8701/user/1

#### 服务降级

创建UserFallbackService实现UserService接口

~~~java
@Component
public class UserFallbackService implements UserService {
    @Override
    public CommonResult create(User user) {
        User defaultUser = new User();
        defaultUser.setId(-1L);
        defaultUser.setUsername("defaultUser");
        defaultUser.setPassword("123456");
        return CommonResult.success(defaultUser);
    }

    @Override
    public CommonResult<User> getUser(Long id) {
        User defaultUser = new User();
        defaultUser.setId(-1L);
        defaultUser.setUsername("defaultUser");
        defaultUser.setPassword("123456");
        return CommonResult.success(defaultUser);
    }

    @Override
    public CommonResult<List<User>> getUserByIds(List<Long> ids) {
        return CommonResult.success(Collections.emptyList());
    }

    @Override
    public CommonResult<User> getByUsername(String username) {
        User defaultUser = new User();
        defaultUser.setId(-1L);
        defaultUser.setUsername("defaultUser");
        defaultUser.setPassword("123456");
        return CommonResult.success(defaultUser);
    }

    @Override
    public CommonResult update(User user) {
        return CommonResult.failed("调用失败，服务被降级");
    }

    @Override
    public CommonResult delete(Long id) {
        return CommonResult.failed("调用失败，服务被降级");
    }
}
~~~

修改UserService的@FeignClient注解指定fallback为UserFallbackService

~~~java
@FeignClient(value = "user-service", fallback = UserFallbackService.class)
~~~

在feign中开启hystrix

~~~yml
feign:
  hystrix:
    enabled: true #在Feign中开启Hystrix
~~~

#### 日志

> 配置日志级别

- NONE：默认的，不显示任何日志；
- BASIC：仅记录请求方法、URL、响应状态码及执行时间；
- HEADERS：除了BASIC中定义的信息之外，还有请求和响应的头信息；
- FULL：除了HEADERS中定义的信息之外，还有请求和响应的正文及元数据。

~~~java
@Configuration
public class FeignConfig {
    @Bean
    Logger.Level feignLoggerLevel() {
        return Logger.Level.FULL;
    }
}
~~~

> application.yml

~~~yml
logging:
  level:
    com.belean.cloud.feignservice.service.UserService: debug
~~~

> 示例

http://localhost:8701/user/1

~~~
<--- HTTP/1.1 200 (549ms)
 connection: keep-alive
 content-type: application/json
 date: Sat, 23 Oct 2021 15:30:52 GMT
 keep-alive: timeout=60
 transfer-encoding: chunked
{"code":200,"message":"操作成功","data":{"id":1,"username":"macro","password":"123456"}}
<--- END HTTP (92-byte body)
~~~

#### feign常用配置

~~~yml
feign:
  hystrix:
    enabled: true #在Feign中开启Hystrix
  compression:
    request:
      enabled: false #是否对请求进行GZIP压缩
      mime-types: text/xml,application/xml,application/json #指定压缩的请求数据类型
      min-request-size: 2048 #超过该大小的请求会被压缩
    response:
      enabled: false #是否对响应进行GZIP压缩
logging:
  level: #修改日志级别
    com.belean.cloud.feignservice.service.UserService: debug

~~~

### Zuul

集成了hystrix、ribbon

API网关服务，它为服务提供了统一的访问入口，支持动态路由与过滤功能。它实现了请求路由、负载均衡、校验过滤、服务容错、服务聚合等功能。

#### pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
</dependency>
~~~

#### applicatiton.yml

~~~yml
server:
  port: 8801
spring:
  application:
    name: zuul-proxy
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8001/eureka/
~~~

#### 启用

~~~java
@EnableZuulProxy
~~~

- @EnableZuulServer：只支持基本的route与filter功能.
- @EnableZuulProxy：Zuul Server+服务发现与熔断等功能的增强版,具有反向代理功能。

#### 路由规则

~~~yml
zuul:
  routes: #给服务配置路由
    user-service:
      path: /userService/** # 默认/user-service/**
    feign-service:
      path: /feignService/**
  # ignored-services: user-service,feign-service #关闭默认路由配置
  prefix: /api # 前缀
  # 配置过滤敏感的请求头信息，设置为空就不会过滤
  # sensitive-headers: Cookie,Set-Cookie,Authorization 
  # add-host-header: true # 设置为true重定向是会添加host请求头
  # retryable: true # 关闭重试机制
  # PreLogFilter: # 过滤器类名
    # pre: # 过滤器类型
      # disable: false # 是否禁用过滤器
~~~

通过网关访问user-service服务：http://localhost:8801/api/userService/user/1

#### 查看路由信息

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

> application.yml

~~~yml
management:
  endpoints:
    web:
      exposure:
        include: 'routes'
~~~

查看简单路由信息：http://localhost:8801/actuator/routes

查看路由详细信息：http://localhost:8801/actuator/routes/details

#### 过滤器

> 过滤类型

- pre：在请求被路由到目标服务前执行。
  - 比如：权限校验、打印日志等功能。
- routing：在请求被路由到目标服务时执行。
  - 这是使用Apache HttpClient或Netflix Ribbon构建和发送原始HTTP请求的地方。
- post：在请求被路由到目标服务后执行。
  - 比如：给目标服务的响应添加头信息，收集统计数据等功能；
- error：请求在其他阶段发生错误时执行。

> 自定义过滤器（继承ZuulFilter）

在请求前，打印请求信息的日志。

~~~java
@Component
public class PreLogFilter extends ZuulFilter {
    private Logger LOGGER = LoggerFactory.getLogger(this.getClass());

    /**
     * 过滤器类型，有pre、routing、post、error四种。
     */
    @Override
    public String filterType() {
        return "pre";
    }

    /**
     * 过滤器执行顺序，数值越小优先级越高。
     */
    @Override
    public int filterOrder() {
        return 1;
    }

    /**
     * 是否进行过滤，返回true会执行过滤。
     */
    @Override
    public boolean shouldFilter() {
        return true;
    }

    /**
     * 自定义的过滤器逻辑，当shouldFilter()返回true时会执行。
     */
    @Override
    public Object run() throws ZuulException {
        RequestContext requestContext = RequestContext.getCurrentContext();
        HttpServletRequest request = requestContext.getRequest();
        String host = request.getRemoteHost();
        String method = request.getMethod();
        String uri = request.getRequestURI();
        LOGGER.info("Remote host:{},method:{},uri:{}", host, method, uri);
        return null;
    }
}
~~~

> 核心过滤器

优先级：数值越小优先级越高。

| 过滤器名称              | 过滤类型 | 优先级 | 过滤器的作用                                                 |
| ----------------------- | -------- | ------ | ------------------------------------------------------------ |
| ServletDetectionFilter  | pre      | -3     | 检测当前请求是通过DispatcherServlet处理运行的还是ZuulServlet运行处理的。 |
| Servlet30WrapperFilter  | pre      | -2     | 对原始的HttpServletRequest进行包装。                         |
| FormBodyWrapperFilter   | pre      | -1     | 将Content-Type为application/x-www-form-urlencoded或multipart/form-data的请求包装成FormBodyRequestWrapper对象。 |
| DebugFilter             | route    | 1      | 根据zuul.debug.request的配置来决定是否打印debug日志。        |
| PreDecorationFilter     | route    | 5      | 对当前请求进行预处理以便执行后续操作。                       |
| RibbonRoutingFilter     | route    | 10     | 通过Ribbon和Hystrix来向服务实例发起请求，并将请求结果进行返回。 |
| SimpleHostRoutingFilter | route    | 100    | 只对请求上下文中有routeHost参数的进行处理，直接使用HttpClient向routeHost对应的物理地址进行转发。 |
| SendForwardFilter       | route    | 500    | 只对请求上下文中有forward.to参数的进行处理，进行本地跳转。   |
| SendErrorFilter         | post     | 0      | 当其他过滤器内部发生异常时的会由它来进行处理，产生错误响应。 |
| SendResponseFilter      | post     | 1000   | 利用请求上下文的响应信息来组织请求成功的响应内容。           |

#### Hystrix和Ribbon

> Hystrix

~~~yml
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
          	# 配置HystrixCommand执行的超时时间，执行超过该时间会进行服务降级处理
            timeoutInMilliseconds: 1000 
~~~

> Ribbon

~~~yml
ribbon: #全局配置
  ConnectTimeout: 1000 # 服务请求连接超时时间（毫秒）
  ReadTimeout: 3000 # 服务请求处理超时时间（毫秒）
~~~

### Config

集中化配置管理。服务端又称分布式配置中心，用于给客户端提供配置信息使用。

#### 配置中心

使用github作为配置中心

#### 服务端

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 8901
spring:
  application:
    name: config-server
  cloud:
    config:
      server:
        git: #配置存储配置信息的Git仓库
          uri: https://gitee.com/macrozheng/springcloud-config.git
          username: macro
          password: 123456
          clone-on-start: true #开启启动时直接从git获取配置
          search-paths: '{application}' # 从应用名称的子目录中搜索配置
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8001/eureka/
~~~

> 启用

~~~java
@EnableConfigServer
~~~

> 访问规则

获取master分支下的config-dev**配置信息**：

/{label}/{application}-{profile}

http://localhost:8901/master/config-dev

~~~json
{
    "name":"master",
    "profiles":["config-dev"],
    "label":null,
    "version":"caf6549c807a6dde1fbbe399ffca18dc886ac03d",
    "state":null,
    "propertySources":[]
}
~~~

获取master分支下的config-dev**配置文件信息**：

/{label}/{application}-{profile}.yml

http://localhost:8901/master/config-dev.yml

~~~yml
config:
  info: config info for dev(master)
~~~

#### 客户端

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
~~~

> bootstrap.yml

~~~yml
server:
  port: 9001
spring:
  application:
    name: config-client
  cloud:
    config: # Config客户端配置
      profile: dev # 启用配置后缀名称
      label: dev # 分支名称
      uri: http://localhost:8901 # 配置中心地址
      name: config # 配置文件名称，对应application
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8001/eureka/
~~~

> 刷新配置

客户端通过actuator刷新配置

~~~xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

~~~yml
management:
  endpoints:
    web:
      exposure:
        include: 'refresh' # 刷新配置
~~~

- Controller层注解刷新配置：@RefreshScope
- 手动刷新配置：POST http://localhost:9001/actuator/refresh

#### 添加安全认证

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
~~~

> application.yml

只需要添加spring-security配置即可。

~~~yml
server:
  port: 8905
spring:
  application:
    name: config-security-server
  cloud:
    config:
      server:
        git:
          uri: https://gitee.com/macrozheng/springcloud-config.git
          username: macro
          password: 123456
          clone-on-start: true # 开启启动时直接从git获取配置
  security: # 配置用户名和密码
    user:
      name: macro
      password: 123456
~~~

> config-client

客户端只需配置其用户名和密码。

添加bootstrap-security.yml，使用该配置文件启动服务

~~~yml
server:
  port: 9002
spring:
  application:
    name: config-client
  cloud:
    config:
      profile: dev #启用配置后缀名称
      label: dev #分支名称
      uri: http://localhost:8905 #配置中心地址
      name: config #配置文件名称
      username: macro
      password: 123456
~~~

#### 集群

> 服务端

分别启动8902和8903两个服务。

> 客户端

去除了直接从注册中心（uri）获取配置，而是通过服务名称（service-id）获取集群中任意服务的配置。

需配置本地host：127.0.0.1 config-server

服务端添加bootstrap-cluster.yml，使用该配置文件启动服务。

~~~yml
spring:
  cloud:
    config:
      profile: dev #启用环境名称
      label: dev #分支名称
      name: config #配置文件名称
      discovery:
        enabled: true
        service-id: config-server
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8001/eureka/
~~~

### Bus

消息总线，使用轻量级的消息代理来连接各个服务，可以用于广播状态更改（例如配置中心更改）或其他管理指令。

Spring Cloud支持两种消息代理：RabbitMQ和Kafka。

配置中心更改：通过RabbitMQ消息代理实现配置的动态刷新。

#### 服务端

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

> application.yml

增加对RabbitMQ和Actuator端点的配置。

~~~yml
spring:
  rabbitmq: #rabbitmq相关配置
    host: localhost
    port: 5672
    virtual-host: /
    username: guest
    password: guest
management:
  endpoints: #暴露bus刷新配置的端点
    web:
      exposure:
        include: 'bus-refresh'
~~~

#### 客户端

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
~~~

>bootstrap-amqp.yml

增加对RabbitMQ的配置。

~~~yml
spring:
  cloud:
    config:
      discovery:
        enabled: true
        service-id: config-server
spring:
  rabbitmq: #rabbitmq相关配置
    host: localhost
    port: 5672
    username: guest
    password: guest
~~~

> 忽略CSRF验证

用于测试手动刷新配置，生产环境需拿掉。

~~~java
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests().anyRequest().authenticated().and().httpBasic().and().csrf().disable();
    }
}
~~~

#### 测试

启动服务端8901，两个客户端9004/9005。

RabbitMQ：一个springCloudBus交换机，三个springCloudBus.anonymous开头的队列

修改config/config-dev.yml配置文件

服务端刷新配置：

- 刷新所有：POST http://localhost:8901/actuator/bus-refresh Basic Auth
- 刷新指定客户端：POST http://localhost:8901/actuator/bus-refresh/config-client:9004 Basic Auth

访问http://localhost:9004/configInfo和http://localhost:9005/configInfo查看刷新

#### WebHooks

> Gitee

管理 => WebHooks

URL：http://localhost:8901/actuator/bus-refresh # 需配置为公网IP

事件：Push

当仓库Push时触发刷新配置

### Sleuth

分布式请求链路跟踪。Spring Cloud Sleuth是分布式系统中跟踪服务间调用的工具，可以跟踪客户端的请求，得到该请求调用的服务链路，以帮助我们解决问题。

#### 为服务添加功能

下面将为user-service和ribbon-service添加请求链路跟踪功能的支持。

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
~~~

> application.yml

~~~yml
spring:
  zipkin:
    base-url: http://localhost:9411
  sleuth:
    sampler:
      probability: 0.1 #设置Sleuth的抽样收集概率
~~~

#### 整合ZipKin获取及分析日志

> 下载

https://repo1.maven.org/maven2/io/zipkin/java/zipkin-server/

选择版本：zipkin-server-2.9.4-exec.jar

> 运行

~~~shell
java -jar zipkin-server-2.9.4-exec.jar
~~~

> 使用

http://localhost:9411

#### 测试

启动服务：eureka-server:8001、user-service:8201、rabbon-service:8301

多次调用：http://localhost:8301/user/1

#### ES存储日志

> 运行ElasticSearch

安装后`bin/elasticsearch.bat`启动。

> 重新运行zipkin

- STORAGE_TYPE：表示存储类型（ES集群名）

- ES_HOSTS：表示ES的访问地址（ES地址）

~~~shell
java -jar zipkin-server-2.9.4-exec.jar --STORAGE_TYPE=elasticsearch --ES_HOSTS=localhost:9200 
~~~

> 重新测试后可以查看ES是否存储了。

### Consul

服务治理与配置中心。Spring Cloud Consul提供了服务治理、配置中心、控制总线等功能。这些功能中的每一个都可以根据需要单独使用，也可以一起使用以构建全方位的服务网格。

- 支持服务治理：Consul作为注册中心。
- 支持客户端负载均衡：支持Ribbon和Spring Cloud LoadBalancer。

- 支持Zuul。
- 支持分布式配置管理：Consul作为配置中心。
- 支持控制总线：通过Control Bus分发事件消息。

#### 下载

下载：https://www.consul.io/downloads.html

~~~shell
# cd到解压后的目录

# 查看版本
consul --version

# 开发模式：htt://localhost:8500，依赖tcp端口8301
consul agent -dev
~~~

#### 注册服务

改造user-service和ribbon-service，将其Eureka更改为Consul。

consul-user-service

consul-ribbon-service

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 8206
spring:
  application:
    name: consul-user-service
  cloud:
    consul: #Consul服务注册发现配置
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
~~~

> 负载均衡

运行consul-user-service（8206、8207）和consul-ribbon-service（8306）

访问http://localhost:8306/user/1

#### 配置中心

consul-config-client

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-config</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

> application.yml

~~~yml
spring:
  profiles:
    active: dev
~~~

> bootstrap.yml

~~~yml
server:
  port: 9101
spring:
  application:
    name: consul-config-client
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        serviceName: consul-config-client
      config:
        enabled: true #是否启用配置中心功能
        format: yaml #设置配置值的格式
        prefix: config #设置配置所在目录
        profile-separator: ':' #设置配置的分隔符
        data-key: data #配置key的名字，由于Consul是K/V存储，配置存储在对应K的V中
~~~

> Controller

~~~java
@RestController
@RefreshScope
public class ConfigClientController {

    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo() {
        return configInfo;
    }
}
~~~

> Consul

配置key

~~~bash
config/consul-config-client:dev/data
~~~

配置value

~~~yml
config:
  info: "config info for dev"
~~~

> 测试

http://localhost:9101/configInfo

Consul自带Control Bus，修改配置文件会自动刷新

### Gateway

新一代API网关，具有强大的智能路由与过滤功能。

- 基于Spring Framework5，Project Reactor和SpringBoot2.0进行构建。
- 动态路由：能够匹配任何请求属性。
- 可以对路由指定Predicate（断言）和Filter（过滤器）。
- 集成Hystrix的断路器、Spring Cloud服务发现功能。
- 请求限流功能。
- 支持路径重写。

#### pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
~~~

#### application.yml

~~~yml
server:
  port: 9201
service-url:
  user-service: http://localhost:8201
spring:
  cloud:
    gateway:
      # 全局超时配置
      httpclient:
        connect-timeout: 1000
        response-timeout: 5s
      routes:
        - id: path_route #路由的ID
          uri: ${service-url.user-service}/user/{id} #匹配后路由地址
          predicates: # 断言，路径相匹配的进行路由
            - Path=/user/{id}
logging:
  level:
    org.springframework.cloud.gateway: debug
~~~

访问：http://localhost:9201/user/1，将路由到：http://localhost:8201/user/1

routes路由，匹配客户端请求，配置由以下几部分组成：

- id：唯一标识
- uri：路由后的地址（需使用lb协议才能使用负载均衡和过滤器，例如：lb://user-service）
- predicates：断言，例如请求头、请求参数等是否匹配。
- filter：过滤请求，修改请求或响应。

#### Bean方式配置

使用Bean方式配置路由

~~~java
@Configuration
public class GatewayConfig {
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                .route("path_route2", r -> r.path("/user/getByUsername")
                        .uri("http://localhost:8201/user/getByUsername"))
                .build();
    }
}
~~~

访问：http://localhost:9201/user/getByUsername?username=macro

将路由到：http://localhost:8201/user/getByUsername?username=macro

#### predicates

> 时效性

After：指定时间之后的请求，生效该路由配置

~~~yml
predicates:
  - After=2019-09-24T16:30:00+08:00[Asia/Shanghai]
~~~

Before：指定时间之前的请求，生效该路由配置

~~~yml
predicates:
  - After=2019-09-24T16:30:00+08:00[Asia/Shanghai]
~~~

Between：指定时间区间的请求，生效该路由配置

~~~yml
predicates:
  - Between=2019-09-24T16:30:00+08:00[Asia/Shanghai], 2019-09-25T16:30:00+08:00[Asia/Shanghai]
~~~

> Cookie

带指定Cookie键值的请求会匹配该路由。

~~~yml
predicates:
  - Cookie=username,macro
~~~

- postman => cookies
  - domain：localhost
  - username=macro; Path=/; Expires=Fri, 28 Oct 2022 23:28:02 GMT;

> Header

带指定请求头的请求会匹配该路由。

~~~yml
predicates:
  - Header=X-Request-Id, \d+ # 正则表达式一个或多个数值
~~~

- postman => Headers
  - key：X-Request-Id
  - value：123

> Host

带指定Host的请求会匹配该路由。

~~~yml
predicates:
  - Host=**.macrozheng.com
~~~

postman => Headers

- key：Host
- value：www.macrozheng.com

>Methon

发送指定方法的请求会匹配该路由。

~~~yml
predicates:
  - Method=GET
~~~

> Path

发送指定路径的请求会匹配该路由。

~~~yml
predicates:
  - Path=/user/{id}
~~~

例如：http://localhost:9201/user/1

> Query

带指定查询参数的请求可以匹配该路由。

~~~yml
predicates:
  - Query=username
~~~

例如：http://localhost:9201/user/1?username

> RemoteAddr

从指定ip地址发送的请求可以匹配该路由。

~~~yml
predicates:
  - RemoteAddr=192.168.31.1/30
~~~

ip地址共四段十进制，转二进制后共32位。30表示前30位不变。

所以剩余两位可变范围：192.168.31.252~192.168.31.254的ip可访问

- 11111111.11111111.11111111.111111 11

- 从右到左按位权：1+2+4+8+16+32+64+128

访问http://192.168.31.194:9201/user/1

postman => Headers

- key：remoteAddress
- value：192.168.31.253

> Weight

Weight=分组, 权重值

group1这组路由，8/(8+2)的请求会被路由到weight_high，2/(8+2)的请求会被路由到weight_low

~~~yml
- id: weight_high
  uri: http://localhost:8201/user/2
  predicates:
    - Path=/user/2
    - Weight=group1, 8
- id: weight_low
  uri: http://localhost:8202/user/2
  predicates:
    - Path=/user/2
    - Weight=group1, 2
~~~

多次访问http://localhost:9201/user/2

#### filters

> AddRequestParameter

为Get请求添加参数

~~~yml
filters:
  - AddRequestParameter=username, macro
predicates:
  - Method=Get
~~~

访问http://localhost:9201/user/3，路由到http://localhost:8201/user/3?username=macro

> StripPrefix

去除指定数量的路径前缀

~~~yml
filters:
  - StripPrefix=2
~~~

访问：/user-service/user/1，则路由到：/1

> PrefixPath

添加指定路由前缀

~~~yml
filters:
  - PrefixPath=/user
~~~

访问：/1，/user/1

> Retry

对路由请求进行重试，可以根据路由请求返回的HTTP状态码来确定是否进行重试

~~~yml
filters:
- name: Retry
  args:
    retries: 1 #需要进行重试的次数
    statuses: BAD_GATEWAY #返回哪个状态码需要进行重试，返回状态码为5XX进行重试
    backoff:
      firstBackoff: 10ms
      maxBackoff: 50ms
      factor: 2
      basedOnPreviousValue: false
~~~

当调用http://localhost:9201/user/111返回500时会进行重试。

#### Hystrix GatewayFilter

Hystrix过滤器允许你将断路器功能添加到网关路由中，使你的服务免受级联故障的影响，并提供服务降级处理。

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
~~~

> Controller

降级处理

~~~java
@RestController
public class FallbackController {

    @GetMapping("/fallback")
    public Object fallback() {
        Map<String,Object> result = new HashMap<>();
        result.put("data",null);
        result.put("message","Get request fallback!");
        result.put("code",500);
        return result;
    }
}
~~~

> application.yml

~~~yml
filters:
  - name: Hystrix
    args:
      name: fallbackcmd
      fallbackUri: forward:/fallback
~~~

#### RequestRateLimiter GatewayFilter

请求限流，当请求太多默认会返回HTTP 429。

> pom.xml

通过Redis进行限流。

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
</dependency>
~~~

> Config

添加限流策略。userKeyResolver根据请求参数username限流，ipKeyResolver根据IP进行限流。

~~~java
@Configuration
public class RedisRateLimiterConfig {
    @Bean
    @Primary
    KeyResolver userKeyResolver() {
        return exchange -> Mono.just(exchange.getRequest().getQueryParams().getFirst("username"));
    }

    @Bean
    public KeyResolver ipKeyResolver() {
        return exchange -> Mono.just(exchange.getRequest().getRemoteAddress().getHostName());
    }
}
~~~

> application.yml

~~~yml
# 使用Redis来进行限流
spring:
  redis:
    host: localhost
    password: 123456
    port: 6379

filters:
- name: RequestRateLimiter
  args:
    redis-rate-limiter.replenishRate: 1 #每秒允许处理的请求数量
    redis-rate-limiter.burstCapacity: 2 #每秒最大处理的请求数量
    key-resolver: "#{@ipKeyResolver}" #限流策略，对应策略的Bean
~~~

#### 注册到Eureka

可配置实现按服务名称路由。

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 9201
spring:
  application:
    name: api-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
          lower-case-service-id: true #使用小写服务名，默认是大写
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8001/eureka/
logging:
  level:
    org.springframework.cloud.gateway: debug
~~~

> 测试

访问：http://localhost:9201/user-service/user/1

### Admin

微服务应用监控。Spring Boot Admin可以对SpringBoot应用的各项指标进行监控。除了监控单体应用，还可作为监控中心来使用，与Spring Cloud的注册中心相结合来监控微服务应用。

- 监控应用运行过程中的概览信息。
- 度量指标信息，比如：JVM、Tomcat及进程信息。
- 环境变量信息，比如：系统属性、系统环境变量以及应用配置信息。
- 查看所有创建的Bean信息。
- 查看应用中的配置信息。
- 查看应用运行日志信息。
- 查看可以访问的Web端点。
- 查看HTTP跟踪信息。

#### admin-server

作为监控中心使用

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-server</artifactId>
    <version>2.5.3</version>
</dependency>
~~~

> application.yml

~~~yml
spring:
  application:
    name: admin-server
server:
  port: 9301
~~~

> 启用

~~~java
@EnableAdminServer
~~~

> 监控页

http://localhost:9301

#### admin-client

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-client</artifactId>
    <version>2.3.1</version> <!-- 需与spring-boot版本对应 -->
</dependency>
~~~

> application.yml

~~~yml
spring:
  application:
    name: admin-client
  boot:
    admin:
      client:
        url: http://localhost:9301 #配置admin-server地址
server:
  port: 9305
management:
  endpoints:
    web:
      exposure:
        include: '*'
  endpoint:
    health:
      show-details: always
logging:
  file:
    name: admin-client.log # 添加开启admin的日志监控
~~~

#### 注册到Eureka

修改admin-server，整合Eureka后会自动从注册中心获取服务列表，然后挨个获取监控信息。

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
~~~

> application.yml

~~~yml
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8001/eureka/
~~~

> 启动

~~~java
@EnableDiscoveryClient
~~~

admin-client修改同上，需要删除spring.boot.admin配置。

#### 安全认证

修改admin-server

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
~~~

> application.yml

~~~yml
spring:
  application:
    name: admin-server
  security: # 配置登录用户名和密码
    user:
      name: macro
      password: 123456
  boot:  # 不显示admin-server的监控信息
    admin:
      discovery:
        ignored-services: ${spring.application.name}
~~~

> config

~~~java
@Configuration
public class SecuritySecureConfig extends WebSecurityConfigurerAdapter {
    private final String adminContextPath;

    public SecuritySecureConfig(AdminServerProperties adminServerProperties) {
        this.adminContextPath = adminServerProperties.getContextPath();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        SavedRequestAwareAuthenticationSuccessHandler successHandler = new SavedRequestAwareAuthenticationSuccessHandler();
        successHandler.setTargetUrlParameter("redirectTo");
        successHandler.setDefaultTargetUrl(adminContextPath + "/");

        http.authorizeRequests()
                //1.配置所有静态资源和登录页可以公开访问
                .antMatchers(adminContextPath + "/assets/**").permitAll()
                .antMatchers(adminContextPath + "/login").permitAll()
                .anyRequest().authenticated()
                .and()
                //2.配置登录和登出路径
                .formLogin().loginPage(adminContextPath + "/login").successHandler(successHandler).and()
                .logout().logoutUrl(adminContextPath + "/logout").and()
                //3.开启http basic支持，admin-client注册时需要使用
                .httpBasic().and()
                .csrf()
                //4.开启基于cookie的csrf保护
                .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
                //5.忽略这些路径的csrf保护以便admin-client注册
                .ignoringAntMatchers(
                        adminContextPath + "/instances",
                        adminContextPath + "/actuator/**"
                );
    }
}
~~~

> admin-client

认证授权

~~~yml
boot:
    admin:
      client:
        username: macro
        password: 123456
~~~

### Security

Spring Cloud Security为构建安全的SpringBoot应用提供了一系列解决方案，结合Oauth2可以实现单点登录、令牌中继、令牌交换等功能。

#### OAuth2介绍

OAuth 2.0是用于授权的行业标准。

> 术语

- Resource owner（资源拥有者）：拥有该资源的最终用户，他有访问资源的账户密码。

- Resource server（资源服务器）：拥有受保护资源的服务器，如果请求包含正确的访问令牌，可以访问资源。

- Client（客户端）：访问资源的客户端（如浏览器、移动设备等），会使用访问令牌去获取资源服务器的资源。
- Authorization server（认证服务器）：用于认证用户的服务器，如果客户端认证通过，发放访问资源服务器的令牌。

> 四种授权模式

- Authorization Code（授权码模式）：
  - 客户端先将用户导向认证服务器，登录后获取授权码，然后进行授权，最后根据授权码获取访问令牌。
- Implicit（简化模式）：和授权码模式相比，取消了获取授权码的过程，直接获取访问令牌。
- Resource Owner Password Credentials（密码模式）：
  - 客户端直接向用户获取用户名和密码，之后想认证服务器获取访问令牌。
- Client Credentials（客户端模式）：
  - 客户端直接通过客户端认证（比如client_id和client_secret）从认证服务器获取访问令牌。

#### 认证服务器

oauth2-server

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-oauth2</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 9401
spring:
  application:
    name: oauth2-server
~~~

> UserDetailsService

用于加载用户信息

~~~java
@Service
public class UserService implements UserDetailsService {
    private List<User> userList;
    @Autowired
    private PasswordEncoder passwordEncoder;

    @PostConstruct
    public void initData() {
        String password = passwordEncoder.encode("123456");
        userList = new ArrayList<>();
        userList.add(new User("macro", password, AuthorityUtils.commaSeparatedStringToAuthorityList("admin")));
        userList.add(new User("andy", password, AuthorityUtils.commaSeparatedStringToAuthorityList("client")));
        userList.add(new User("mark", password, AuthorityUtils.commaSeparatedStringToAuthorityList("client")));
    }

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        List<User> findUserList = userList.stream().filter(user -> user.getUsername().equals(username)).collect(Collectors.toList());
        if (!CollectionUtils.isEmpty(findUserList)) {
            return findUserList.get(0);
        } else {
            throw new UsernameNotFoundException("用户名或密码错误");
        }
    }
}
~~~

> config

认证服务器配置

~~~java
@Configuration
@EnableAuthorizationServer
public class AuthorizationServerConfig extends AuthorizationServerConfigurerAdapter {

    @Autowired
    private PasswordEncoder passwordEncoder;

    @Autowired
    private AuthenticationManager authenticationManager;

    @Autowired
    private UserService userService;

    /**
     * 使用密码模式需要配置
     */
    @Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) {
        endpoints.authenticationManager(authenticationManager)
                .userDetailsService(userService);
    }

    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
        clients.inMemory()
                .withClient("admin")//配置client_id
                .secret(passwordEncoder.encode("admin123456"))//配置client_secret
                .accessTokenValiditySeconds(3600)//配置访问token的有效期
                .refreshTokenValiditySeconds(864000)//配置刷新token的有效期
                .redirectUris("http://www.baidu.com")//配置redirect_uri，用于授权成功后跳转
                .scopes("all")//配置申请的权限范围
                .authorizedGrantTypes("authorization_code","password");//配置grant_type，表示授权类型
    }
}
~~~

资源服务器配置

~~~java
@Configuration
@EnableResourceServer
public class ResourceServerConfig extends ResourceServerConfigurerAdapter {

    @Override
    public void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .anyRequest()
                .authenticated()
                .and()
                .requestMatchers()
                .antMatchers("/user/**");//配置需要保护的资源路径
    }
}
~~~

Security配置

~~~java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }

    @Override
    public void configure(HttpSecurity http) throws Exception {
        http.csrf()
                .disable()
                .authorizeRequests()
                .antMatchers("/oauth/**", "/login/**", "/logout/**")
                .permitAll()
                .anyRequest()
                .authenticated()
                .and()
                .formLogin()
                .permitAll();
    }
}
~~~

> Controller

测试用的登录接口

~~~java
@RestController
@RequestMapping("/user")
public class UserController {
    @GetMapping("/getCurrentUser")
    public Object getCurrentUser(Authentication authentication) {
        return authentication.getPrincipal();
    }
}
~~~

#### 授权码模式测试

> 登录页授权：

- http://localhost:9401/oauth/authorize?response_type=code&client_id=admin&redirect_uri=http://www.baidu.com&scope=all&state=normal
- 手动授权

> 登录后重定向到指定地址（redirect_uri），并携带授权码：

- https://www.baidu.com/?code=vSClWE&state=normal

> Basic认证获取访问令牌：

- POST http://localhost:9401/oauth/token
- Authtorization Basic Auth
  - username：admin
  - password：admin123456
- Body
  - grant_type=authorization_code
  - code=vSClWE
  - client_id=admin
  - redirect_uri=http://www.baidu.com
  - scope=all

~~~json
{
    "access_token": "1a84dc91-e65c-4c25-8d95-6d613b9e2248",
    "token_type": "bearer",
    "expires_in": 3599,
    "scope": "all"
}
~~~

> 请求接口携带token成功访问：

- GET http://localhost:9401/user/getCurrentUser
- Headers
  - key：Authorization
  - value：bearer 1a84dc91-e65c-4c25-8d95-6d613b9e2248

~~~json
{
    "password": null,
    "username": "macro",
    "authorities": [
        {
            "authority": "admin"
        }
    ],
    "accountNonExpired": true,
    "accountNonLocked": true,
    "credentialsNonExpired": true,
    "enabled": true
}
~~~

#### 密码模式测试

使用密码请求该地址获取访问令牌

> Basic认证

POST http://localhost:9401/oauth/token

- Authorization Basic Auth
  - username：admin
  - password：admin123456
- Body
  - grant_type=password
  - username=andy
  - password=123456
  - scope=all

~~~json
{
    "access_token": "ddc32833-cd67-4180-8d84-737c376fd614",
    "token_type": "bearer",
    "expires_in": 3599,
    "scope": "all"
}
~~~

> 请求接口携带token成功访问：

- GET http://localhost:9401/user/getCurrentUser
- Headers
  - key：Authorization
  - value：bearer ddc32833-cd67-4180-8d84-737c376fd614

~~~json
{
    "password": null,
    "username": "andy",
    "authorities": [
        {
            "authority": "client"
        }
    ],
    "accountNonExpired": true,
    "accountNonLocked": true,
    "credentialsNonExpired": true,
    "enabled": true
}
~~~

#### 使用Redis存储令牌

修改oauth2-server

> pom.xml

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

> application.yml

~~~yml
spring:
  redis: 
    password: redis
~~~

> config

~~~java
/**
 * 使用redis存储token的配置
 */
@Configuration
public class RedisTokenStoreConfig {

    @Autowired
    private RedisConnectionFactory redisConnectionFactory;

    @Bean
    public TokenStore redisTokenStore (){
        return new RedisTokenStore(redisConnectionFactory);
    }
}
~~~

修改AuthorizationServerConfig认证服务器配置

~~~java
@Autowired
@Qualifier("redisTokenStore")
private TokenStore tokenStore;

/**
 * 使用密码模式需要配置
 */
@Override
public void configure(AuthorizationServerEndpointsConfigurer endpoints) {
    endpoints.authenticationManager(authenticationManager)
        .userDetailsService(userService)
        .tokenStore(tokenStore);//配置令牌存储策略
}
~~~

> 测试

密码模式测试

查看redis是否存储

#### 使用JWT存储令牌

> config

~~~java
/**
 * 使用Jwt存储token的配置
 */
@Configuration
public class JwtTokenStoreConfig {

    @Bean
    public TokenStore jwtTokenStore() {
        return new JwtTokenStore(jwtAccessTokenConverter());
    }

    @Bean
    public JwtAccessTokenConverter jwtAccessTokenConverter() {
        JwtAccessTokenConverter accessTokenConverter = new JwtAccessTokenConverter();
        accessTokenConverter.setSigningKey("test_key");//配置JWT使用的秘钥
        return accessTokenConverter;
    }
}
~~~

修改AuthorizationServerConfig认证服务器配置

~~~java
@Autowired
@Qualifier("jwtTokenStore")
private TokenStore tokenStore;
@Autowired
private JwtAccessTokenConverter jwtAccessTokenConverter;
@Autowired
private JwtTokenEnhancer jwtTokenEnhancer;

/**
 * 使用密码模式需要配置
 */
@Override
public void configure(AuthorizationServerEndpointsConfigurer endpoints) {
    endpoints.authenticationManager(authenticationManager)
        .userDetailsService(userService)
        .tokenStore(tokenStore) //配置令牌存储策略
        .accessTokenConverter(jwtAccessTokenConverter);
}
~~~

> 测试

密码模式测试

授权：http://localhost:9401/oauth/token

~~~json
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX25hbWUiOiJhbmR5Iiwic2NvcGUiOlsiYWxsIl0sImV4cCI6MTYzNTc2OTIwMywiYXV0aG9yaXRpZXMiOlsiY2xpZW50Il0sImp0aSI6IjI2MWZlYTY1LWQxZWItNDQ0Yi1iNGEwLTA3YzIyNzU1NjRkZiIsImNsaWVudF9pZCI6ImFkbWluIiwiZW5oYW5jZSI6ImVuaGFuY2UgaW5mbyJ9.jnFcGE0reSbmA6uGLFt1IEeUgo84MwRYU2zeqGPH8oc",
    "token_type": "bearer",
    "expires_in": 3599,
    "scope": "all",
    "enhance": "enhance info",
    "jti": "261fea65-d1eb-444b-b4a0-07c2275564df"
}
~~~

http://localhost:9401/user/getCurrentUser

~~~json
{
    "user_name": "andy",
    "scope": [
        "all"
    ],
    "exp": 1635769203,
    "authorities": [
        "client"
    ],
    "jti": "261fea65-d1eb-444b-b4a0-07c2275564df",
    "client_id": "admin",
    "enhance": "enhance info"
}
~~~

> 扩展JWT内容

~~~java
/**
 * Jwt内容增强器
 */
public class JwtTokenEnhancer implements TokenEnhancer {
    @Override
    public OAuth2AccessToken enhance(OAuth2AccessToken accessToken, OAuth2Authentication authentication) {
        Map<String, Object> info = new HashMap<>();
        info.put("enhance", "enhance info");
        ((DefaultOAuth2AccessToken) accessToken).setAdditionalInformation(info);
        return accessToken;
    }
}
~~~

修改JWT配置（JwtTokenStoreConfig）

~~~java
@Bean
public JwtTokenEnhancer jwtTokenEnhancer() {
    return new JwtTokenEnhancer();
}
~~~

修改AuthorizationServerConfig认证服务器配置

~~~java
/**
 * 使用密码模式需要配置
 */
@Override
public void configure(AuthorizationServerEndpointsConfigurer endpoints) {
    TokenEnhancerChain enhancerChain = new TokenEnhancerChain();
    List<TokenEnhancer> delegates = new ArrayList<>();
    delegates.add(jwtTokenEnhancer); //配置JWT的内容增强器
    delegates.add(jwtAccessTokenConverter);
    enhancerChain.setTokenEnhancers(delegates);
    endpoints.authenticationManager(authenticationManager)
        .userDetailsService(userService)
        .tokenStore(tokenStore) //配置令牌存储策略
        .accessTokenConverter(jwtAccessTokenConverter)
        .tokenEnhancer(enhancerChain);
}
~~~

#### 解析JWT

> pom.xml

~~~xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.0</version>
</dependency>
~~~

> Controller

获取请求中的token，解析并返回。

~~~java
@RestController
@RequestMapping("/user")
public class UserController {
    @GetMapping("/getCurrentUser")
    public Object getCurrentUser(Authentication authentication, HttpServletRequest request) {
        String header = request.getHeader("Authorization");
        String token = StrUtil.subAfter(header, "bearer ", false);
        return Jwts.parser()
                .setSigningKey("test_key".getBytes(StandardCharsets.UTF_8))
                .parseClaimsJws(token)
                .getBody();
    }
}
~~~

#### 刷新令牌

修改AuthorizationServerConfig认证服务器配置

~~~java
@Override
public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
    clients.inMemory()
        .withClient("admin")
        .secret(passwordEncoder.encode("admin123456"))
        .accessTokenValiditySeconds(3600)
        .refreshTokenValiditySeconds(864000)
        .redirectUris("http://www.baidu.com")
        .autoApprove(true) //自动授权配置
        .scopes("all")
        .authorizedGrantTypes("authorization_code","password","refresh_token"); //添加授权模式
}
~~~

> 测试

- POST http://localhost:9401/oauth/token
- Authorization Basic Auth
  - username：admin
  - password：admin123456
- Body
  - grant_type=refresh_token
  - refresh_token=xxxxxxxxxxxxxxxx

#### 单点登录

单点登录（Single Sign On）指的是当有多个系统需要登录时，用户只需要登录一个系统，就可以访问其他需要登录的系统而无需登录。

##### oauth2-client

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-oauth2</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.0</version>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 9501
  servlet:
    session:
      cookie:
        name: OAUTH2-CLIENT-SESSIONID #防止Cookie冲突，冲突会导致登录验证不通过
oauth2-server-url: http://localhost:9401
spring:
  application:
    name: oauth2-client
security:
  oauth2: #与oauth2-server对应的配置
    client:
      client-id: admin
      client-secret: admin123456
      user-authorization-uri: ${oauth2-server-url}/oauth/authorize
      access-token-uri: ${oauth2-server-url}/oauth/token
    resource:
      jwt:
        key-uri: ${oauth2-server-url}/oauth/token_key
~~~

> 启动

~~~java
@EnableOAuth2Sso
~~~

> Controller

~~~java
@RestController
@RequestMapping("/user")
public class UserController {

    @GetMapping("/getCurrentUser")
    public Object getCurrentUser(Authentication authentication) {
        return authentication;
    }
}
~~~

##### oauth2-server

> config

修改AuthorizationServerConfig认证服务器配置

~~~java
@Override
public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
    clients.inMemory()
        .withClient("admin")
        .secret(passwordEncoder.encode("admin123456"))
        .accessTokenValiditySeconds(3600)
        .refreshTokenValiditySeconds(864000)
        // .redirectUris("http://www.baidu.com")
        .redirectUris("http://localhost:9501/login") //单点登录时配置
        .autoApprove(true) // 自动授权配置
        .scopes("all")
        .authorizedGrantTypes("authorization_code","password","refresh_token");
}

@Override
public void configure(AuthorizationServerSecurityConfigurer security) {
    security.tokenKeyAccess("isAuthenticated()"); // 获取密钥需要身份认证，使用单点登录时必须配置
}
~~~

##### 测试

访问http://localhost:9501/user/getCurrentUser跳到授权页自动授权

- Authorization Oauth2
  - Token Name：sso
  - Gant Type：Authorization Code
  - Callback URL：http://localhost:9501/login
  - Auth URL：http://localhost:9401/oauth/authorize
  - Access Token URL：http://localhost:9401/oauth/token
  - Client ID：admin
  - Client Secret：admin123456
  - Scope：all
  - State：normal
  - Client Authentication：Send as Basic Auth header
- Get New Access Token
- Use Token
- Send

##### oauth2-client添加权限校验

> config

~~~java
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
@Order(101)
public class SecurityConfig extends WebSecurityConfigurerAdapter {
}
~~~

> Controller

需admin权限

~~~java
@RestController
@RequestMapping("/user")
public class UserController {

    @PreAuthorize("hasAuthority('admin')")
    @GetMapping("/auth/admin")
    public Object adminAuth() {
        return "Has admin auth!";
    }
}
~~~

### Alibaba Nacos

可作为注册中心和配置中心使用。

- 服务发现和服务健康监测：支持基于DNS和基于RPC的服务发现，支持对服务的实时的健康检查，阻止向不健康的主机或服务实例发送请求；
- 动态配置服务：动态配置服务可以让您以中心化、外部化和动态化的方式管理所有环境的应用配置和服务配置；
- 动态 DNS 服务：动态 DNS 服务支持权重路由，让您更容易地实现中间层负载均衡、更灵活的路由策略、流量控制以及数据中心内网的简单DNS解析服务；
- 服务及其元数据管理：支持从微服务平台建设的视角管理数据中心的所有服务及元数据。

#### 运行Nacos服务端

> 下载

https://github.com/alibaba/nacos/releases

> bin/startup.cmd

指定java版本

~~~cmd
set JAVA_HOME=C:\Program Files\Java\jdk1.8.0_144
~~~

> 数据库

创建nacos数据库，执行conf/nacos-mysql.sql

conf/application.properties

~~~
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
db.user=root
db.password=root
~~~

bin/startup.cmd运行

> 访问

http://localhost:8848/nacos

账号和密码：nacos

#### 注册中心

将换成nacos服务发现

> pom.xml

~~~xml
<dependencies>
	<dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
</dependencies>
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.1.0.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
~~~

> application.yml

~~~yml
spring:
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #配置Nacos地址
management:
  endpoints:
    web:
      exposure:
        include: '*'
~~~

#### 配置中心

使用nacos配置中心的配置

> pom.xml

~~~xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
~~~

> application.yml

~~~yml
spring:
  profiles:
    active: dev
~~~

> boostrap.yml

~~~yml
spring:
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #Nacos地址
      config:
        server-addr: localhost:8848 #Nacos地址
        file-extension: yaml #这里我们获取的yaml格式的配置
~~~

> Controller

~~~java
@RestController
@RefreshScope
public class ConfigClientController {

    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo() {
        return configInfo;
    }
}
~~~

> 配置格式

key，例如：nacos-config-client-dev.yaml

~~~
${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}
~~~

value：

```yaml
config:
  info: "config info for dev"
```

测试：http://localhost:9101/configInfo

### Alibaba Sentinel

熔断与限流。Sentinel以流量为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。

- 丰富的应用场景：如秒杀，可以实时熔断下游不可应用。
- 完备的实时监控：提供实时的监控功能，可以在控制台中看到接入应用的单台机器秒级数据，甚至500台规模的集群的汇总运行情况。
- 广泛的开源生态：提供开箱即用的与其它开源框架/库的整合模块，例如Spring Cloud、Dubbo、gRPC等。
- 完善的SPI扩展点：提供简单易用、完善的SPI扩展点。

#### 运行Sentinel

> 下载

https://github.com/alibaba/Sentinel/releases

> 访问

http://localhost:8080

#### 限流

给服务添加熔断与限流

> pom.xml

~~~xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
~~~

> application.yml

~~~yml
server:
  port: 8401
spring:
  application:
    name: sentinel-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #配置Nacos地址
    sentinel:
      transport:
        dashboard: localhost:8080 #配置sentinel dashboard地址
        port: 8719
service-url:
  user-service: http://nacos-user-service
management:
  endpoints:
    web:
      exposure:
        include: '*'
~~~

> 限流

Sentinel默认为所有的HTTP服务提供了限流埋点，可以通过@SentinelResource自定义限流行为。

~~~java
/**
 * 限流功能
 */
@RestController
@RequestMapping("/rateLimit")
public class RateLimitController {

    /**
     * 按资源名称限流，需要指定限流处理逻辑
     */
    @GetMapping("/byResource")
    @SentinelResource(value = "byResource",blockHandler = "handleException")
    public CommonResult byResource() {
        return new CommonResult("按资源名称限流", 200);
    }

    /**
     * 按URL限流，有默认的限流处理逻辑
     */
    @GetMapping("/byUrl")
    @SentinelResource(value = "byUrl",blockHandler = "handleException")
    public CommonResult byUrl() {
        return new CommonResult("按url限流", 200);
    }

    public CommonResult handleException(BlockException exception){
        return new CommonResult(exception.getClass().getCanonicalName(),200);
    }

}
~~~

value根据资源名限流

> 自定义限流

处理逻辑

~~~java
public class CustomBlockHandler {

    public CommonResult handleException(BlockException exception){
        return new CommonResult("自定义限流信息",200);
    }
}
~~~

使用

~~~java
@RestController
@RequestMapping("/rateLimit")
public class RateLimitController {

    /**
     * 自定义通用的限流处理逻辑
     */
    @GetMapping("/customBlockHandler")
    @SentinelResource(value = "customBlockHandler", blockHandler = "handleException",blockHandlerClass = CustomBlockHandler.class)
    public CommonResult blockHandler() {
        return new CommonResult("限流成功", 200);
    }
}
~~~

#### 熔断（RestTemplate）

> SentinelRestTemplate

~~~java
@Configuration
public class RibbonConfig {

    @Bean
    @SentinelRestTemplate
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
~~~

> Controller

~~~java
/**
 * 熔断功能
 */
@RestController
@RequestMapping("/breaker")
public class CircleBreakerController {

    private Logger LOGGER = LoggerFactory.getLogger(CircleBreakerController.class);
    @Autowired
    private RestTemplate restTemplate;
    @Value("${service-url.user-service}")
    private String userServiceUrl;

    @RequestMapping("/fallback/{id}")
    @SentinelResource(value = "fallback",fallback = "handleFallback")
    public CommonResult fallback(@PathVariable Long id) {
        return restTemplate.getForObject(userServiceUrl + "/user/{1}", CommonResult.class, id);
    }

    @RequestMapping("/fallbackException/{id}")
    @SentinelResource(value = "fallbackException",fallback = "handleFallback2", exceptionsToIgnore = {NullPointerException.class})
    public CommonResult fallbackException(@PathVariable Long id) {
        if (id == 1) {
            throw new IndexOutOfBoundsException();
        } else if (id == 2) {
            throw new NullPointerException();
        }
        return restTemplate.getForObject(userServiceUrl + "/user/{1}", CommonResult.class, id);
    }

    public CommonResult handleFallback(Long id) {
        User defaultUser = new User(-1L, "defaultUser", "123456");
        return new CommonResult<>(defaultUser,"服务降级返回",200);
    }

    public CommonResult handleFallback2(@PathVariable Long id, Throwable e) {
        LOGGER.error("handleFallback2 id:{},throwable class:{}", id, e.getClass());
        User defaultUser = new User(-2L, "defaultUser2", "123456");
        return new CommonResult<>(defaultUser,"服务降级返回",200);
    }
}
~~~

> 测试

#### 熔断（OpenFeign）

> pom.xml

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
~~~

> application.yml

~~~yml
feign:
  sentinel:
    enabled: true #打开sentinel对feign的支持
~~~

> 启动

~~~java
@EnableFeignClients
~~~

> 降级

~~~java
@FeignClient(value = "nacos-user-service",fallback = UserFallbackService.class)
public interface UserService {
    @PostMapping("/user/create")
    CommonResult create(@RequestBody User user);

    @GetMapping("/user/{id}")
    CommonResult<User> getUser(@PathVariable Long id);

    @GetMapping("/user/getByUsername")
    CommonResult<User> getByUsername(@RequestParam String username);

    @PostMapping("/user/update")
    CommonResult update(@RequestBody User user);

    @PostMapping("/user/delete/{id}")
    CommonResult delete(@PathVariable Long id);
}
~~~

~~~java
@Component
public class UserFallbackService implements UserService {
    @Override
    public CommonResult create(User user) {
        User defaultUser = new User(-1L, "defaultUser", "123456");
        return new CommonResult<>(defaultUser,"服务降级返回",200);
    }

    @Override
    public CommonResult<User> getUser(Long id) {
        User defaultUser = new User(-1L, "defaultUser", "123456");
        return new CommonResult<>(defaultUser,"服务降级返回",200);
    }

    @Override
    public CommonResult<User> getByUsername(String username) {
        User defaultUser = new User(-1L, "defaultUser", "123456");
        return new CommonResult<>(defaultUser,"服务降级返回",200);
    }

    @Override
    public CommonResult update(User user) {
        return new CommonResult("调用失败，服务被降级",500);
    }

    @Override
    public CommonResult delete(Long id) {
        return new CommonResult("调用失败，服务被降级",500);
    }
}
~~~

~~~java
@RestController
@RequestMapping("/user")
public class UserFeignController {
    @Autowired
    private UserService userService;

    @GetMapping("/{id}")
    public CommonResult getUser(@PathVariable Long id) {
        return userService.getUser(id);
    }

    @GetMapping("/getByUsername")
    public CommonResult getByUsername(@RequestParam String username) {
        return userService.getByUsername(username);
    }

    @PostMapping("/create")
    public CommonResult create(@RequestBody User user) {
        return userService.create(user);
    }

    @PostMapping("/update")
    public CommonResult update(@RequestBody User user) {
        return userService.update(user);
    }

    @PostMapping("/delete/{id}")
    public CommonResult delete(@PathVariable Long id) {
        return userService.delete(id);
    }
}
~~~

#### Nacos存储规则

默认情况下，当我们在Sentinel控制台中配置规则时，控制台推送规则方式是通过API将规则推送至客户端并直接更新到内存中。一但我们重启应用，规则将消失。

> pom.xml

~~~xml
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
~~~

> application.yml

~~~yml
spring:
  cloud:
    sentinel:
      datasource:
        ds1:
          nacos:
            server-addr: localhost:8848
            dataId: ${spring.application.name}-sentinel
            groupId: DEFAULT_GROUP
            data-type: json
            rule-type: flow
~~~

> Nacos配置

Data ID：sentinel-service-sentinel

Group：DEFAULT_GROUP

格式：JSON

~~~json
[
    {
        "resource": "/rateLimit/byUrl",
        "limitApp": "default",
        "grade": 1,
        "count": 1,
        "strategy": 0,
        "controlBehavior": 0,
        "clusterMode": false
    }
]
~~~

- resource：资源名称。
- limitApp：来源应用。
- grade：阈值类型，0表示线程数，1表示QPS。
- count：单机阈值。
- strategy：流程模式，0表示直接，1表示关联，2表示链路。
- controlBehavior：流控效果，0表示快速失败，1表示Warm Up，2表示排队等待。
- clusterMode：是否集群。

### Seata

分布式事务。在微服务架构中由于全局数据一致性没法保证生产的问题是分布式事务问题。简单来说，一次业务操作需要操作多个数据源或需要进行远程调用，就会产生分布式事务问题。

Seata是一款的分布式事务解决方案，提供了AT、TCC、SAGA和XA事务模式，为用户打造一站式的分布式解决方案。

- Transaction Coordinator（TC）：事务协调器，维护全局事务的运行状态，负责协调并驱动全局事务的提交或回滚。
- Transaction Manager（TM）：控制全局事务的边界，负责开启一个全局事务，并最终发起全局提交或全局回滚的决议。
- Resource Manager（RM）：控制分支事务，负责分支注册、状态汇报，并接收事务协调器的指令，驱动分支（本地）事务的提交和回滚。

> 事务过程

- TM想TC申请开启一个全局事务，全局事务创建成功并生成一个全局唯一的XID。
- XID在微服务调用链路的上下文中传播。
- RM向TC注册分支事务，将其纳入XID对应全局事务的管辖。
- TM向TC发起对XIDI的全局提交或回滚决议。
- TC调度XID下管辖的全部分支事务完成提交或回滚请求。

#### seata-server运行

> 下载

https://github.com/seata/seata/releases

> 配置（/conf/file.conf）

主要修改自定义事务组名称、事务日志存储模式为db及数据库连接信息。

~~~bash
service {
  #vgroup->rgroup
  vgroup_mapping.fsp_tx_group = "default" #修改事务组名称为：fsp_tx_group，和客户端自定义的名称对应
  #only support single node
  default.grouplist = "127.0.0.1:8091"
  #degrade current not support
  enableDegrade = false
  #disable
  disable = false
  #unit ms,s,m,h,d represents milliseconds, seconds, minutes, hours, days, default permanent
  max.commit.retry.timeout = "-1"
  max.rollback.retry.timeout = "-1"
}

## transaction log store
store {
  ## store mode: file、db
  mode = "db" #修改此处将事务信息存储到数据库中

  ## database store
  db {
    ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
    datasource = "dbcp"
    ## mysql/oracle/h2/oceanbase etc.
    db-type = "mysql"
    driver-class-name = "com.mysql.jdbc.Driver"
    url = "jdbc:mysql://localhost:3306/seat-server" #修改数据库连接地址
    user = "root" #修改数据库用户名
    password = "root" #修改数据库密码
    min-conn = 1
    max-conn = 3
    global.table = "global_table"
    branch.table = "branch_table"
    lock-table = "lock_table"
    query-limit = 100
  }
}
~~~

存储日志需创建数据库seata-server数据库，建表语句`/conf/db_store.sql`

> 配置注册中心（/conf/registry.conf）

指明nacos作为注册中心。

~~~bash
registry {
  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
  type = "nacos" #改为nacos

  nacos {
    serverAddr = "localhost:8848" #改为nacos的连接地址
    namespace = ""
    cluster = "default"
  }
}
~~~

> 启动

`/bin/seata-server.bat`

#### 数据库

- seat-order

~~~sql
CREATE TABLE `order` (
  `id` bigint(11) NOT NULL AUTO_INCREMENT,
  `user_id` bigint(11) DEFAULT NULL COMMENT '用户id',
  `product_id` bigint(11) DEFAULT NULL COMMENT '产品id',
  `count` int(11) DEFAULT NULL COMMENT '数量',
  `money` decimal(11,0) DEFAULT NULL COMMENT '金额',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8;

ALTER TABLE `order` ADD COLUMN `status` int(1) DEFAULT NULL COMMENT '订单状态：0：创建中；1：已完结' AFTER `money` ;
~~~

- seat-account

~~~sql
CREATE TABLE `storage` (
                         `id` bigint(11) NOT NULL AUTO_INCREMENT,
                         `product_id` bigint(11) DEFAULT NULL COMMENT '产品id',
                         `total` int(11) DEFAULT NULL COMMENT '总库存',
                         `used` int(11) DEFAULT NULL COMMENT '已用库存',
                         `residue` int(11) DEFAULT NULL COMMENT '剩余库存',
                         PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

INSERT INTO `seat-storage`.`storage` (`id`, `product_id`, `total`, `used`, `residue`) VALUES ('1', '1', '100', '0', '100');
~~~

- seat-storage

~~~sql
CREATE TABLE `account` (
  `id` bigint(11) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `user_id` bigint(11) DEFAULT NULL COMMENT '用户id',
  `total` decimal(10,0) DEFAULT NULL COMMENT '总额度',
  `used` decimal(10,0) DEFAULT NULL COMMENT '已用余额',
  `residue` decimal(10,0) DEFAULT '0' COMMENT '剩余可用额度',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

INSERT INTO `seat-account`.`account` (`id`, `user_id`, `total`, `used`, `residue`) VALUES ('1', '1', '1000', '0', '1000');
~~~

> 需为每个数据库中创建日志回滚表

`/conf/db_undo_log.sql`

#### 客户端配置

> application.yml

~~~yml
spring:
  cloud:
    alibaba:
      seata:
        tx-service-group: fsp_tx_group #自定义事务组名称需要与seata-server中的对应
~~~

> file.conf

自定义事务组名称

~~~bash
service {
  #vgroup->rgroup
  vgroup_mapping.fsp_tx_group = "default" #修改自定义事务组名称
  #only support single node
  default.grouplist = "127.0.0.1:8091"
  #degrade current not support
  enableDegrade = false
  #disable
  disable = false
  #unit ms,s,m,h,d represents milliseconds, seconds, minutes, hours, days, default permanent
  max.commit.retry.timeout = "-1"
  max.rollback.retry.timeout = "-1"
  disableGlobalTransaction = false
}
~~~

> registry.conf

注册中心

~~~bash
registry {
  # file 、nacos 、eureka、redis、zk
  type = "nacos" #修改为nacos

  nacos {
    serverAddr = "localhost:8848" #修改为nacos的连接地址
    namespace = ""
    cluster = "default"
  }
}
~~~

> Application.java

取消数据源自动创建

~~~java
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
~~~

> config

使用seate对数据源进行代理

~~~java
/**
 * 使用Seata对数据源进行代理
 * Created by macro on 2019/11/11.
 */
@Configuration
public class DataSourceProxyConfig {

    @Value("${mybatis.mapperLocations}")
    private String mapperLocations;

    @Bean
    @ConfigurationProperties(prefix = "spring.datasource")
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }

    @Bean
    public DataSourceProxy dataSourceProxy(DataSource dataSource) {
        return new DataSourceProxy(dataSource);
    }

    @Bean
    public SqlSessionFactory sqlSessionFactoryBean(DataSourceProxy dataSourceProxy) throws Exception {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSourceProxy);
        sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver()
                .getResources(mapperLocations));
        sqlSessionFactoryBean.setTransactionFactory(new SpringManagedTransactionFactory());
        return sqlSessionFactoryBean.getObject();
    }

}
~~~

> Service

@GlobalTransactional # 开启事务

- name = "fsp-create-order" # 事务名称
- rollbackFor = Exception.class # 触发回滚的异常

~~~java
/**
 * 订单业务实现类
 */
@Service
public class OrderServiceImpl implements OrderService {

    private static final Logger LOGGER = LoggerFactory.getLogger(OrderServiceImpl.class);

    @Autowired
    private OrderDao orderDao;
    @Autowired
    private StorageService storageService;
    @Autowired
    private AccountService accountService;

    /**
     * 创建订单->调用库存服务扣减库存->调用账户服务扣减账户余额->修改订单状态
     */
    @Override
    @GlobalTransactional(name = "fsp-create-order",rollbackFor = Exception.class)
    public void create(Order order) {
        LOGGER.info("------->下单开始");
        //本应用创建订单
        orderDao.create(order);

        //远程调用库存服务扣减库存
        LOGGER.info("------->order-service中扣减库存开始");
        storageService.decrease(order.getProductId(),order.getCount());
        LOGGER.info("------->order-service中扣减库存结束:{}",order.getId());

        //远程调用账户服务扣减余额
        LOGGER.info("------->order-service中扣减余额开始");
        accountService.decrease(order.getUserId(),order.getMoney());
        LOGGER.info("------->order-service中扣减余额结束");

        //修改订单状态为已完成
        LOGGER.info("------->order-service中修改订单状态开始");
        orderDao.update(order.getUserId(),0);
        LOGGER.info("------->order-service中修改订单状态结束");

        LOGGER.info("------->下单结束");
    }
}
~~~

~~~java
/**
 * 账户业务实现类
 */
@Service
public class AccountServiceImpl implements AccountService {

    private static final Logger LOGGER = LoggerFactory.getLogger(AccountServiceImpl.class);
    @Autowired
    private AccountDao accountDao;

    /**
     * 扣减账户余额
     */
    @Override
    public void decrease(Long userId, BigDecimal money) {
        LOGGER.info("------->account-service中扣减账户余额开始");
        //模拟超时异常，全局事务回滚
        try {
            Thread.sleep(30*1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        accountDao.decrease(userId,money);
        LOGGER.info("------->account-service中扣减账户余额结束");
    }
}
~~~

### Swagger







### Loadbalancer



### 搭建自己的微服务网格

#### 注册/配置中心

nacos

#### 服务网关

- Gateway
  - 路由
  - 熔断、降级
  - 限流
- Swagger 
  - knife4j整合微服务接口文档

#### 服务监控

- Spring Boot Admin：SpringBoot单体应用监控。还可配合注册中心做监控中心使用，监控微服务应用。
- Spring Cloud Sleuth：分布式系统中跟踪服务间调用的工具

#### 接口鉴权

Oauth2

- 密码模式：登录
- 授权码模式：适用于开放的、安全性较高的API接口
- 资源服务器：JWT鉴权，接口可允许访问的角色列表

#### 解决分布式问题

##### 服务间调用

OpenFeign+OpenFeign

- OpenFeign
  - Ribbon：负载均衡
  - Hystrix：服务熔断、降级
  - 开启Sentinel
- Sentinel
  - 限流
  - 负载均衡
  - 服务熔断、降级

##### 分布式事务

Seata

























