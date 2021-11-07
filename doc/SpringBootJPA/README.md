## SpringDataJPA

#### 一、ORM思想

​		ORM（Object-Relational-Mapping）表示对象关系映射。在面向对象的软件开发中，通过ORM，就可以把对象映射到关系型数据库中。只要有一套程序能够做到建立对象与数据库的关联，操作对象就可以直接操作数据库数据，就可以说这套程序实现了ORM对象关系映射。

- 为什么用ORM？
  - 减少大量重复性代码
- 常见ORM框架
  - mybatis
  - hibernate
  - jpa

#### 二、JPA规范

​		JPA的全称是Java Persistence API，即java持久化API，是SUM公司推出的一套基于ORM的规范，内部是由一系列的接口和抽象类构成。JPA利用jdk1.5时的注解来描述【对象-关系】表的映射关系，并将运行期的实体对象持久化到数据库中。

**JPA优势：**

- 标准化
  - JPA是JCP组织发布的Java EE标准之一
  - 因此任何声称符合JPA标准的框架都遵循同样的架构，提供相同的访问API
  - 确保了基于JPA开发的企业应用能够经过少量的修改就能够在不同的JPA框架下运行。
- 容器级特性的支持
  - JPA框架中支持大数据集、事务、并发等容器级事务
  - 这使得JPA超越了简单持久化框架的局限，在企业应用中发挥更大的作用。
- 简单方便
  - JPA的主要目标之一就是提供更加简单的编程模型
  - 在JPA框架下创建实体和创建java类一样简单，没有任何的约束和限制。
  - JPA的框架和接口也都非常简单，没有太多特别的规则和设计模式的要求。
  - JPA基于非侵入式原则设计，因此可以很容易的和其他框架或者容器集成。
- 查询能力
  - JPA的查询语言是面向对象的，而非面向数据库的。
  - 提供了JPQL（Java Persistence Query Language）查询语句，类似Hibernate的HQL。
- 高级特性
  - JPA中能够支持面向对象的高级特性，如类之间的继承、多态和类之间的复杂关系。
  - 让开发者最大限度的使用面向对象的模型设计企业应用，不需要自行处理这些特性在数据库的持久化。

#### 三、JPA实战

JPA+Hibernate+mysql

##### 1、导入依赖jar

~~~xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.hibernate.version>5.0.7.Final</project.hibernate.version>
</properties>
<dependencies>
    <!-- 1、hibernate对jpa的支持包 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>${project.hibernate.version}</version>
    </dependency>
    <!-- 2、c3p0 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-c3p0</artifactId>
        <version>${project.hibernate.version}</version>
    </dependency>
    <!-- 3、Mysql and MariaDB -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.6</version>
    </dependency>
    <!-- 4、junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <!-- 5、log日志 -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
</dependencies>
~~~

##### 2、实体类

- @Entity：注解实体类（与表的映射关系）
- @Table(name="")：注解实体类（映射对应表的名称）
- @Id：注解属性（对应主键Id）
- @Column(name ="")：注解属性（对应表的列名）
- @GeneratedValue(strategy=)：注解属性（主键的生成策略）
  - GenerationType.IDENTITY：自增（MySQL），要求数据库必须有自增设置。
  - GenerationType.SEQUENCE：序列（Oracle），要求数据库必须支持序列。
  - GenerationType.TABLE：JPA提供的，通过一个额外的自增表来实现自增。
  - GenerationType.AUTO：由程序自动选择主键生成策略。

##### 3、JPA配置文件

|-META-INF

​		|-persistence.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://java.sun.com/xml/ns/persistence" version="2.0">
    <!--需要配置persistence-unit节点
        持久化单元：
            name：持久化单元名称
            transaction-type：事务管理的方式
                    JTA：分布式事务管理
                    RESOURCE_LOCAL：本地事务管理-->
    <persistence-unit name="myJpa" transaction-type="RESOURCE_LOCAL">
        <!--jpa的实现方式 -->
        <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
        <!--可选配置：配置jpa实现方的配置信息-->
        <properties>
            <!-- 数据库信息-->
            <property name="javax.persistence.jdbc.user" value="root"/>
            <property name="javax.persistence.jdbc.password" value="111111"/>
            <property name="javax.persistence.jdbc.driver" 
                      value="com.mysql.jdbc.Driver"/>
            <property name="javax.persistence.jdbc.url" 
                      value="jdbc:mysql:///jpa"/>
            <!--配置jpa实现方(hibernate)的配置信息
                显示sql           ：   false|true
                自动创建数据库表    ：  hibernate.hbm2ddl.auto
                        create      : 程序运行时创建数据库表（如果有表，先删除表再创建）
                        update      ：程序运行时创建表（如果有表，不会创建表）
                        none        ：不会创建表-->
            <property name="hibernate.show_sql" value="true" />
            <property name="hibernate.hbm2ddl.auto" value="update" />
        </properties>
    </persistence-unit>
</persistence>
~~~

##### 4、测试

~~~java
public void testSave() {
    //1.加载配置文件创建工厂（实体管理器工厂）对象
    EntityManagerFactory factory = 
        		Persistence.createEntityManagerFactory("myJpa");//持久化单元名称
    //2.通过实体管理器工厂获取实体管理器
    EntityManager em = factory.createEntityManager();
    //3.获取事务对象，开启事务
    EntityTransaction tx = em.getTransaction(); //获取事务对象
    tx.begin();//开启事务
    //4.完成增删改查操作：保存一个客户到数据库中
    Customer customer = new Customer();
    customer.setCustName("name1");
    customer.setCustIndustry("Industry");
    //保存，
    em.persist(customer); //保存操作
    //5.提交事务
    tx.commit();
    //6.释放资源
    em.close();
    //factory.close();
}
~~~

- 涉及类
  - EntityManagerFactory（实体管理器工厂，线程安全）
    - Persistence.createEntityManagerFactory("myJpa");//通过持久化对象创建工厂
  - EntityManager（实体管理器）
    - factory.createEntityManager();//通过实体管理器工厂创建实体管理器
  - EntityTransaction（事务对象）
    - em.getTransaction();//通过实体管理器获取事务对象
- 基本操作
  - em.persist(customer);//插入操作，需要事务
  - em.find(Customer.class, 1l);//根据id查找，直接加载
  - em.getReference(Customer.class, 1l);//根据id查找，延迟加载
  - 删除操作（需要事务）
    - em.find(Customer.class,1l);//先查询
    - em.remove(customer);//再删除
  - 更新操作（需要事务）
    - em.em.find(Customer.class,1l);//先查询，再修改
    - em.merge(customer);//然后更新

#### 四、SpringDataJPA

##### 1、SpringData

- Spring的一个子项目，其主要目标是使数据库的访问变得方便快捷，支持以下数据库：
  - NoSQL存储：
    - MongoDB（文档数据库）
    - Neo4j（图形数据库）
    - Redis（键/值存储）
    - Hbase（列族数据库）
  - 关系型存储：
    - JDBC
    - JPA

##### 2、SpringData JPA

​		Spring Data JPA 是 Spring 基于 ORM 框架、JPA 规范的基础上封装的一套JPA应用框架，可使开发者用极简的代码即可实现对数据库的访问和操作。它提供了包括增删改查等在内的常用功能，且易于扩展！学习并使用 Spring Data JPA 可以极大提高开发效率！

- 致力于减少数据库访问层（DAO）的开发量，开发者唯一要做的，就是要声明持久层的接口，其他都交给SpringData JPA来帮你完成。
- 框架怎么可能代替开发者实现业务逻辑呢？比如：当有一个UserDao.findUserById()这样一个方法声明，大致应该判断出这是根据给定条件的ID查询出满足条件的User对象。SpringData JPA做的便是规范方法的名字，根据符合规范的名字来确定方法需要实现什么样的逻辑。

#### 五、SpringDataJPA实战

Spring+SpringDataJPA+Hibernate+mysql

##### 1、添加依赖jar

~~~xml
<properties>
    <spring.version>5.0.2.RELEASE</spring.version>
    <hibernate.version>5.0.7.Final</hibernate.version>
    <slf4j.version>1.6.6</slf4j.version>
    <log4j.version>1.2.12</log4j.version>
    <c3p0.version>0.9.1.2</c3p0.version>
    <mysql.version>5.1.6</mysql.version>
</properties>
<dependencies>
    <!-- 织入切面，可以在编译时、编译后、加载时进行代码织入 -->
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.6.8</version>
    </dependency>
    <!-- spring aop -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!-- spring comtext扩展，mvc方面 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context-support</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!-- spring对orm框架的支持包-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-orm</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!-- spring ioc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!-- spring ioc扩展，JNDI定位，EJB集成、远程访问、缓存以及多种视图层框架的支持。-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!-- spring的核心工具包 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!-- spring data jpa-->
    <dependency>
        <groupId>org.springframework.data</groupId>
        <artifactId>spring-data-jpa</artifactId>
        <version>1.9.0.RELEASE</version>
    </dependency>
    <!-- hibernate核心工具包 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-core</artifactId>
        <version>${hibernate.version}</version>
    </dependency>
    <!-- hibernate实现的jpa规范 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>${hibernate.version}</version>
    </dependency>
    <!-- hibernate注解 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-validator</artifactId>
        <version>5.2.1.Final</version>
    </dependency>
    <!-- el 表达式语言，Hibernate Validator需要实现统一表达式语言的实现 -->
    <dependency>
        <groupId>javax.el</groupId>
        <artifactId>javax.el-api</artifactId>
        <version>2.2.4</version>
    </dependency>
    <dependency>
        <groupId>org.glassfish.web</groupId>
        <artifactId>javax.el</artifactId>
        <version>2.2.4</version>
    </dependency>
    <!-- mysql 驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
    </dependency>
    <!-- c3p0 -->
    <dependency>
        <groupId>c3p0</groupId>
        <artifactId>c3p0</artifactId>
        <version>${c3p0.version}</version>
    </dependency>
    <!-- junit单元测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <!-- 对junit等测试框架的简单封装-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!-- log4j 日志框架-->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
    </dependency>
    <!-- log4j 日志接口-->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>${slf4j.version}</version>
    </dependency>
    <!-- log4j 适配器-->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>${slf4j.version}</version>
    </dependency>
</dependencies>
~~~

##### 2、spring配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:jdbc="http://www.springframework.org/schema/jdbc" 
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:jpa="http://www.springframework.org/schema/data/jpa" 
       xmlns:task="http://www.springframework.org/schema/task"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/aop
                           http://www.springframework.org/schema/aop/spring-aop.xsd
                           http://www.springframework.org/schema/context
                         http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/jdbc
                           http://www.springframework.org/schema/jdbc/spring-jdbc.xsd
                           http://www.springframework.org/schema/tx
                           http://www.springframework.org/schema/tx/spring-tx.xsd
                           http://www.springframework.org/schema/data/jpa
                          http://www.springframework.org/schema/data/jpa/spring-jpa.xsd">
    <!--spring 和 spring data jpa的配置-->
    <!-- 1.创建entityManagerFactory对象交给spring容器管理-->
    <bean id="entityManagerFactoty" 
          class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <!--配置的扫描的包（实体类所在的包） -->
        <property name="packagesToScan" value="com.lc.entity" />
        <!-- jpa的实现厂家 -->
        <property name="persistenceProvider">
            <bean class="org.hibernate.jpa.HibernatePersistenceProvider"/>
        </property>

        <!--jpa的供应商适配器 -->
        <property name="jpaVendorAdapter">
            <bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
                <!--配置是否自动创建数据库表 -->
                <property name="generateDdl" value="false" />
                <!--指定数据库类型 -->
                <property name="database" value="MYSQL" />
                <!--数据库方言：支持的特有语法 -->
                <property name="databasePlatform" value="org.hibernate.dialect.MySQLDialect" />
                <!--是否显示sql -->
                <property name="showSql" value="true" />
            </bean>
        </property>
        <!--jpa的方言 ：高级的特性 -->
        <property name="jpaDialect" >
            <bean class="org.springframework.orm.jpa.vendor.HibernateJpaDialect" />
        </property>
    </bean>
    <!--2.创建数据库连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="user" value="root"></property>
        <property name="password" value="111111"></property>
        <property name="jdbcUrl" value="jdbc:mysql:///jpa" ></property>
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
    </bean>
    <!--3.整合spring dataJpa-->
    <jpa:repositories base-package="cn.itcast.dao" 
                      transaction-manager-ref="transactionManager"
                      entity-manager-factory-ref="entityManagerFactoty" />
    <!--4.配置事务管理器 -->
    <bean id="transactionManager" 
          class="org.springframework.orm.jpa.JpaTransactionManager">
        <property name="entityManagerFactory" ref="entityManagerFactoty"></property>
    </bean>
    <!-- 4.txAdvice-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="save*" propagation="REQUIRED"/>
            <tx:method name="insert*" propagation="REQUIRED"/>
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="delete*" propagation="REQUIRED"/>
            <tx:method name="get*" read-only="true"/>
            <tx:method name="find*" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <!-- 5.aop-->
    <aop:config>
        <aop:pointcut id="pointcut" 
                      expression="execution(* com.lc.service.*.*(..))" />
        <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut" />
    </aop:config>
    <!-- 6. 配置包扫描-->
    <context:component-scan base-package="com.lc" ></context:component-scan>
</beans>
~~~

可选项（测试用）

~~~xml
<!--注入jpa的配置信息
    加载jpa的基本配置信息和jpa实现方式（hibernate）的配置信息
    hibernate.hbm2ddl.auto : 自动创建数据库表
    create ： 每次都会重新创建数据库表
    update：有表不会重新创建，没有表会重新创建表
-->
<property name="jpaProperties" >
    <props>
    	<prop key="hibernate.hbm2ddl.auto">update</prop>
    </props>
</property>
~~~

##### 3、实体类（同上）

##### 4、接口类

~~~java
public interface CustomerDao 
    extends JpaRepository<Customer,Long>,JpaSpecificationExecutor<Customer> {}
~~~

##### 5、测试

> 1、使用JpaRepository封装的基本操作

~~~java
@RunWith(SpringJUnit4ClassRunner.class) //声明spring提供的单元测试环境
@ContextConfiguration(locations = "classpath:applicationContext.xml")//spring容器的配置信息
public class CustomerDaoTest {
    @Autowired
    private CustomerDao customerDao;
    //根据id查询
    @Test
    public void testFindOne() {
        Customer customer = customerDao.findOne(4l);
        System.out.println(customer);
    }
}
~~~

- 基本操作
  - customerDao.findOne(4l);//根据id查询，立即加载
  - customerDao.findAll();//查询所有
  - customerDao.save(customer);//存在更新，不存在插入
  - customerDao.delete(3l);//根据id删除
  - customerDao.count();//查询总数量
  - customerDao.exists(4l);//是否存在
  - customerDao.getOne(4l);//根据id查询，延迟加载

> 2、使用JPQL及sql

添加接口方法

~~~java
//JPQL查询，兼容不同sql
@Query(value="from Customer where custName = ?")
public Customer findJpql(String custName);
//指定索引参数，模糊查询时推荐，例如%?1%
@Query(value = "from Customer where custName = ?2 and custId = ?1")
public Customer findCustNameAndId(Long id,String name);
//指定命名参数,推荐
@Query(value = "from Customer where custName = ? and custId = ?")
public Customer findCustNameAndId(@Param("custId") Long id, 
                                  @Param("custName") String name);
//修改需要指定为@Modifying
@Query(value = " update Customer set custName = ?2 where custId = ?1 ")
@Modifying
public void updateCustomer(long custId,String custName);
//sql语句查询，nativeQuery = true
@Query(value="select * from cst_customer where cust_name like ?1",nativeQuery = true)
public List<Object [] > findSql(String name);
~~~

注意事务

~~~java
@Test
@Transactional //添加事务的支持
@Rollback(value = false)//默认true回滚
public void testUpdateCustomer() {
    customerDao.updateCustomer(4l,"CustomerName");
}
~~~

> 3、JPA命名规则查询

~~~java
方法名的约定：
	findBy : 查询
		对象中的属性名（首字母大写） ： 查询的条件
		CustName
        默认情况 ： 使用 等于的方式查询
        特殊的查询方式
        
    findByCustName   --   根据客户名称查询
        再springdataJpa的运行阶段
        会根据方法名称进行解析  findBy    from  xxx(实体类)
        属性名称  where  custName =
            1.findBy  + 属性名称 （根据属性名称进行完成匹配的查询=）
            2.findBy  + 属性名称 + “查询方式（Like | isnull）”
            findByCustNameLike
            3.多条件查询
			findBy + 属性名 + “查询方式”   + “多条件的连接符（and|or）”  + 属性名 + “查询方式”
public Customer findByCustName(String custName);
public List<Customer> findByCustNameLike(String custName);
public Customer findByCustNameLikeAndCustIndustry(String custName, String custIndustry);
~~~


> 3、使用JpaSpecificationExecutor

~~~java
@Test
public void testSpec() {
    //匿名内部类，自定义查询条件
    //1.实现Specification接口（提供泛型：查询的对象类型）
    //2.实现toPredicate方法（构造查询条件）
    //3.需要借助方法参数中的两个参数
    //	root：获取需要查询的对象属性
    //	CriteriaBuilder：构造查询条件的，内部封装了很多的查询条件（模糊匹配，精准匹配）
    Specification<Customer> spec = new Specification<Customer>() {
        @Override
        public Predicate toPredicate(Root<Customer> root,
                                     CriteriaQuery<?> query, CriteriaBuilder cb) {
            //1.获取比较的属性
            Path<Object> custName = root.get("custId");
            //2.构造查询条件select * from cst_customer where cust_name = 'customerName'
            Predicate predicate = cb.equal(custName, "cutomerName");
            return predicate;
        }
    };
    Customer customer = customerDao.findOne(spec);
    System.out.println(customer);
}
//匿名内部类，多条件查询
Specification<Customer> spec = new Specification<Customer>() {
    @Override
    public Predicate toPredicate(Root<Customer> root,
                                 CriteriaQuery<?> query, CriteriaBuilder cb) {
        Path<Object> custName = root.get("custName");//客户名
        Path<Object> custIndustry = root.get("custIndustry");//所属行业
        
        Predicate p1 = cb.equal(custName, "customeName");
        Predicate p2 = cb.equal(custIndustry, "indeustryName");
        
        Predicate and = cb.and(p1, p2);
        return and;
    }
};
//匿名内部类，模糊查询，排序
Specification<Customer> spec = new Specification<Customer>() {
    @Override
    public Predicate toPredicate(Root<Customer> root,
                                 CriteriaQuery<?> query, CriteriaBuilder cb) {
        Path<Object> custName = root.get("custName");
        Predicate like = cb.like(custName.as(String.class), "custmerName%");
        return like;
    }
};
Sort sort = new Sort(Sort.Direction.DESC,"custId");
List<Customer> list = customerDao.findAll(spec, sort);
//匿名内部类，查询所有，分页Page对象
Pageable pageable = new PageRequest(0,2);
Page<Customer> page = customerDao.findAll(null, pageable);
~~~

> 4、一对多

~~~java
//客户与联系人，一个客户可以有多个联系人
/*一、Customer类
 *使用注解的形式配置多表关系
 *	1.声明关系
 *		@OneToMany：配置一对多关系
 *			targetEntity：对方对象的字节码对象
 *	2.配置外键（中间表）
 *		@JoinColumn : 配置外键
 *			name：外键字段名称
 *			referencedColumnName：参照的主表的主键字段名称
 */
//@OneToMany(targetEntity = LinkMan.class)
//@JoinColumn(name = "lkm_cust_id",referencedColumnName = "cust_id")
/*放弃外键维护权
 * mappedBy：对方配置关系的属性名称
 * cascade ：配置级联（可以配置到设置多表的映射关系的注解上）
 *      CascadeType.all         ：所有
 *                  MERGE       ：更新
 *                  PERSIST     ：保存
 *                  REMOVE      ：删除
 *		fetch : 配置关联对象的加载方式
 *          EAGER   ：立即加载
 *          LAZY    ：延迟加载
 */
@OneToMany(mappedBy = "customer",cascade = CascadeType.ALL)
private Set<LinkMan> linkMans = new HashSet<LinkMan>();
/*二、LinkMan类
 * 配置联系人到客户的多对一关系
 *     使用注解的形式配置多对一关系
 *      1.配置表关系
 *          @ManyToOne : 配置多对一关系
 *              targetEntity：对方的实体类字节码
 *      2.配置外键（中间表）
 * 配置外键的过程，配置到了多的一方，就会在多的一方维护外键
 */
@ManyToOne(targetEntity = Customer.class,fetch = FetchType.LAZY)
@JoinColumn(name = "lkm_cust_id",referencedColumnName = "cust_id")
private Customer customer;
/*三、CustomerDao
 *四、LinkManDao
 *五、测试
 *1、导航查询（将维护对象也一并查出）
 * 需要配置fetch
 */
customerDao.getOne(1l);//默认延迟加载，因为有关联对象，用到的时候才查询，节省资源
linkManDao.findOne(2l);//默认立即查询，只有一个客户
/*2、添加操作
 */
//>save1插入客户表，插入联系人表，更新联系人表
customer.getLinkMans().add(linkMan);//在客户中添加联系人，会将联系人也关联客户，需要有联系人的维护权
//>save2插入客户表，插入联系人表
linkMan.setCustomer(customer);//在联系人中添加客户，会将客户也关联联系人，需要有客户的维护权
//>delete1如果有联系人，将联系人外键置为null再删除客户，没有联系人直接删除，需要有联系人的维护权
customerDao.delete(customer);//先查询再删除
//>delete1如果有联系人，就不能删除，没有联系人才删除，需要放弃联系人的维护权
customerDao.delete(customer);//先查询再删除
/*3、级联操作
 *需要配置级联cascade
 */
//>save3效果与save1一样，配置了客户到联系人的维护，多了一个update语句，放弃维护权即可同save2效果一样
linkMan.setCustomer(customer);
customer.getLinkMans().add(linkMan);
//>delete2删除客户表，同时删除所有联系人
customerDao.delete(customer);//先查询再删除
~~~

> 5、多对多

~~~java
/*一、User类
 *1.声明表关系的配置
 *	@ManyToMany(targetEntity = Role.class)  //多对多
 *		targetEntity：代表对方的实体类字节码
 *2.配置中间表（包含两个外键）
 *@JoinTable
 *	name : 中间表的名称
 *	joinColumns：配置当前对象在中间表的外键
 *		@JoinColumn的数组
 *			name：外键名
 *			referencedColumnName：参照的主表的主键名
 *			inverseJoinColumns：配置对方对象在中间表的外键
 */
@ManyToMany(targetEntity = Role.class,cascade = CascadeType.ALL)
@JoinTable(name = "sys_user_role",
	//joinColumns,当前对象在中间表中的外键
	joinColumns = {@JoinColumn(name = "sys_user_id",referencedColumnName = "user_id")},
	//inverseJoinColumns，对方对象在中间表的外键
	inverseJoinColumns = {
        @JoinColumn(name = "sys_role_id",referencedColumnName = "role_id")
    })
private Set<Role> roles = new HashSet<Role>();
//用户与角色，一个用户可以有多重角色，一个角色可以作用多个人
/*二、Role类
 */
@ManyToMany(mappedBy = "roles")  //配置多表关系
private Set<User> users = new HashSet<User>();
/*三、UserDao
 *四、RoleDao
 *五、测试同上，注意双向关系即可
 */
~~~

##### 6、总结

- 测试时加@Transactional，防止no session错误
- @Rollback(false) ，不自动回滚
- @Modifying，更新语句注解
- 尽量使用JPQL语句，屏蔽不同厂商之间的sql差异
- 分清主次表关系，警惕级联操作





















































