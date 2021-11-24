## MySQL笔记

### 一、命令行

#### 1、登录命令

~~~shell
# 本地登录
mysql -uroot -proot # 明文
mysql -uroot -p # 密文

# 允许远程登录
update user set Host="%", Password=PASSWORD("123456") WHERE User="root";
# 生效
flush privileges

# 远程登录
mysql -hip -uroot -proot
mysql --host=ip --user=root --password=root
~~~

#### 2、退出

~~~shell
# 退出
exit
quit
~~~

#### 3、备份和还原

~~~shell
# 备份
mysqldump -uroot -proot --default-character-set=utf8 [databaseName] > [pathName]

# 还原
# 1、登录数据库
# 2、创建数据库
# 3、使用数据库
source [pathName/file]
~~~

### 二、SQL

#### 1、什么是SQL？

Structured Query Language 结构化查询语言。

**作用：**

- 是一种所有关系型数据库的查询规范，不同的数据库都支持。
- 每一种数据库操作的方式存在不一样的地方，称为“方言”。
- 通用的数据库操作语言，可以用在不同的数据库中。

#### 2、语法规范

- SQL语句可以单行或多行书写，以分号结尾。
- MySQL数据库不区分大小写，关键字建议使用大写。
- 注释
  - 单行注释：-- 或 #
  - 多行注解：/**/

#### 3、SQL分类

- DDL（Data Definition Language）数据库定义语言：
  - 用来定义数据库对象：数据库、表、字段等。关键字：create、drop、alter等。
- DML（Data Manipulation Language）数据库操作语言：
  - 用来对数据库中表的数据进行增删改。关键字：insert、delete、update等。
- DQL（Data Query Language）数据库查询语言：
  - 用来查询数据库中表的记录（数据）。关键字：select、where等。
- DCL（Data Control Language）数据库控制语言：
  - 用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT、REVOKE等。

### 三、DDL

#### 1、操作数据库

~~~sql
-- 1、创建（Create）
create database [databaseName]; -- 创建数据库
create database if not exists [databaseName]; -- 不存在则创建
create database [databaseName] character set utf8md4; -- 设置字符集编码

-- 2、查询（Retrieve）
show databases; -- 查询所有数据库
show create database [databaseName]; -- 查询某个数据库的创建语句

-- 3、修改（Update）
alter database [databaseName] character set utf8md4;

-- 4、删除（Delete）
drop database [databaseName]; -- 删除数据库
drop database if exists [databaseName]; -- 存在就删

-- 5、使用
select database(); -- 查询当前使用的数据库名称
use [databaseName]; -- 使用数据库
~~~

#### 2、操作表

~~~sql
-- 1、创建
create table [tableName](
	[fieldName] [dataType],
    ...
);
/* 1.1、数据类型
	int整数
	double小数,double(4,1)总长度4，保留1位小数
	date日期，yyyy-MM-dd
	datetime日期，yyyy-MM-dd HH:mm:ss
	timestamp时间戳，yyyy-MM-dd HH:mm:ss，赋值null或不赋值时，自动赋值当前系统时间。
	varchar字符串
*/

-- 2、查询
show tables; -- 查询当前数据库中所有的表
desc [tableName]; -- 查询某个表结构

-- 3、修改
alter table [tableName] rename to [newTableName]; -- 修改表名
alter table [tableName] character set utf8md4; -- 修改字符集
alter table [tableName] add [fieldName] [dataType]; -- 添加列
alter table [tableName] change [fieldName] [newFieldName] [newDataType]; -- 修改列名、类型
alter table [tableName] modify [fieldName] [newDataType] -- 修改数据类型
alter table [tableName] drop [fieldName]; -- 删除列

-- 4、删除
drop table [tableName]; -- 删除表
drop table if exists [tableName]; -- 存在，就删除

-- 5、主键
-- 5.1建表前
id int primary key -- 给id添加主键
-- 5.2建表后
alter table [tableName] modify id int primary key; -- 给id添加主键
-- 5.3删除
alter table [tableName] drop id int primary key; -- 删除主键

-- 6、自增
-- 6.1、建表前
id int primary key auto_increment -- 给id添加主键约束，并自增
-- 6.2、建表后
alter table [tableName] modify id int auto_increment; -- 添加id自增
-- 6.3、删除
alter table [tableName] modify id int; -- 删除自增

-- 7、非空
-- 7.1、建表前
name varchar(20) not null -- 非空约束
-- 7.2、建表后
alter table [tableName] modify name varchar(20) not null; -- 非空约束
-- 7.3、删除
alter table [tableName] modify name varchar(20); -- 删除非空约束

-- 8、唯一
-- 8.1、建表前
name varchar(20) unique -- 唯一约束
-- 8.2、建表后
alter table [tableName] modify name varchar(20) unique; -- 唯一约束
-- 8.3、删除
alter table [tableName] modify name varchar(20); -- 删除唯一约束

-- 9、外键
-- 9.1、建表前
constraint 外键名 foreign key(外键列名) references 主表名(主表列名) -- 添加外键
-- 9.2、建表后
alert table [tableName] add constraint 外键名 foreign key(外键列名) references 主表名(主表列名); -- 添加外键
-- 9.3、删除
alter table [tableName] drop foreign key 外键名; -- 删除外键关联
-- 9.4、级联
on update cascade -- 级联更新
on delete cascade -- 级联删除
~~~

### 四、DML

~~~sql
-- 1、添加
insert into [tableName]([fieldName],...) values([value],...);

-- 2、删除
delete from [tableName] [where ?];
truncate table [tableName]; -- 删除表，再创建一个空表。

-- 3、修改
update [tableName] set [fieldName]=[value],... [where ?];
~~~

### 五、DQL

#### 1、查询

~~~sql
-- 1、查询的格式
select [fields] -- 字段列表
from [tables] -- 表名列表
where ? -- 条件列表
group by [fieldName] -- 分组
having ? -- 分组后的条件
order by [asc/desc] -- 默认升序asc
limit [0,3] -- 分页0开始，3条

-- 2、条件
>、<、>=、<=、=、<>/!=
between ... and -- between 1 and 3 表示[1~3]
in(集合) -- in(1,2,3)
like -- _单个任意字符、%多个任意字符、[1~3]范围、[^1~3]不在范围
is null、is not null -- 不可以用 = null
and 或 &&
or 或 ||
not 或 !

-- 3、排序
select * from [tableName] order by [field] asc, ...;

-- 4、聚合函数
count -- 计数 一般count(id)主键
max -- 最大值
min -- 最小值
sum -- 合计
avg -- 平均值

-- 5、分组
select 分组字段或聚合函数 group by 分组字段;
-- where 在分组之前进行限定，不可以进行聚合函数的判断。
-- having 在分组之后进行限定，可以进行聚合函数的判断。

-- 6、分页
select * from [tableName] limit 0,3 -- 0开始，查询3条
-- 开始索引 = 当前页码 - 1 * 每页条数

-- 7、去重
distinct

-- 8、别名
as 或 空格

-- 9、默认值
ifnull(表达式1, 表达式2) -- 判断表达式1为null时，表达式2作为替换


~~~

#### 2、多表

~~~sql
-- 1、笛卡尔积
-- a x b 的数据，行数据不重复。
-- a表和b表的数据会重复。
select * from a,b;
select * from a sross join b;

-- 2、内连接
-- 2.1、隐式内连接
select * from a,b where a.b_id = b.id; -- 非空交集部分数据
-- 2.2、显示内连接
select * from a inner join b on a.b_id = b.id; -- 非空交集部分数据
select * from a join b on a.b_id = b.id; -- 非空交集部分数据

-- 3、外连接
-- 3.1、左外连接
select * from a left join b on a.b_id = b.id; -- 以左表为基准的交集部分数据
-- 3.2、右外连接
select * from a rigth join b on a.b_id = b.id; -- 以右表为基准的交集部分数据
-- 3.3、全外连接
select * from a full join b on a.b_id = b.id; -- 以左右表为基准的所有交集部分数据

-- 4、子查询
select * from a,(select * from b); -- 子查询的结果做为一个表
select * from a where score = in (select score from b where id=1); -- 子查询的结果作为一个或多个值

-- 5、自连接
select * from a a1,a a2; -- 当有字段关联时，可以使用自连接
~~~

### 六、DCL

#### 1、管理用户

~~~sql
-- 1、添加用户
create user '用户名'@'主机名' identified by '密码';

-- 2、删除用户
drop user '用户名'@'主机名';

-- 3、修改用户密码
-- 首次设置密码
SET PASSWORD = PASSWORD('your_new_password');
-- 不是第一次设置密码
update user set password = password('新密码') where user = '用户名';
set password for '用户名'@'主机名' = password('新密码');

-- 4、忘记密码
-- 4.1、cmd -> net stop mysql # 停止MySQL服务
-- 4.2、mysqld --skip-grant-tables或mysqld-nt # 使用无验证方式启动MySQL服务
-- 4.3、打开新的cmd窗口，输入mysql登录
-- 4.4、use mysql;
-- 4.5、update user set password = password('新的密码') where user = 'root';
-- 4.6、关闭cmd窗口，结束mysqld.exe进程。
-- 4.7、启动mysql服务，使用新密码登录。

-- 5、查询用户
-- 5.1、use mysql; # 切换到mysql数据库
-- 5.2、select * from user;
-- 注：%表示可以在任意主机使用用户登录数据库

-- 设置字符集
-- 查看
show variables like 'character_set%';
-- 设置客户端字符集
set character_set_client=utf8;
~~~

#### 2、权限管理

~~~sql
-- 1、查询权限
show grants for '用户名'@'主机名';

-- 2、授予权限
grant 权限列表 on 数据库.表名 to '用户名'@'主机名';

-- 3、撤销权限
revoke 权限列表 on 数据库.表名 from '用户名'@'主机名';

-- 4、权限列表
all -- 授权所有

-- 5、例子(授予root用户所有sql权限)
grant all privileges on *.* to root@"%" identified by 'root';
flush privileges;

-- 选项
grant select,insert,update,delete,create,drop,
reload,shutdown,process,file,references,
index,alter,show databases,super,create temporary tables,
lock tables,execute,replication slaue,replication client,
create view,show view,create routine,alter routine,create user,
event,trigger,create tablespace,create role,drop role
on *.* 'root'@'localhost' with grant option

grant application_password_admin,audit_admin,backup_admin,binlog_admin,binlog_encryption_admin,connection_admin,encryption_key_admin,group_replication_admin,innodb_redo_log_archive,persist_ro_variables_admin,replication_applier,replication_slave_admin,resource_group_admin,resource_group_user,role_admin,seruice_connection_admin,session_vrriables_admin,set_user_id,system_user,system_variables_admin,table_encryption_admin,xa_recover_admin
on *.* to 'localhost' with grant option

grant proxy on ''@'' to 'root'@'localhost' with grant option
~~~



### 七、数据库设计

#### 1、三范式

- 第一范式（1NF）：数据库表的每一列都是不可分割的基本数据项。（列数据的不可分割）
- 第二范式（2NF）：满足第一范式的前提下，数据库表中的每行必须可以被唯一地区分。（主键）
- 第三范式（3NF）：满足第二范式的前提下，数据库表中不包含其他表中已包含的非主关键字信息。（外键）

反三范式：有时候为了效率，允许冗余或者可以推导出的字段。

#### 2、一对一

例如：人和身份证。

分析：一个人只有一个身份证，一个身份证只能对应一个人。

实现：任意一方添加唯一外键指向另一方的主键。

> 注：一般一对一的情况，字段都在一个表中。

#### 3、一对多

例如：部门和员工。

分析：一个部门有多个员工，一个员工只能对应一个部门。

实现：在多的一方建立外键，指向一的一方的主键。

#### 4、多对多

例如：学生和课程。

分析：一个学生可以选择多门课程，一个课程也可以被很多学生选择。

实现：中间表至少有两个字段，分别指向两张表的主键，联合主键。

### 八、事务

如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。

#### 1、操作

~~~sql
start transaction; -- 开启事务
rollback; -- 回滚事务
commit; -- 提交事务
~~~

**事务提交方式：**

- 自动提交：
  - MySQL默认自动提交。
  - 一条DML语句会自动提交一次事务。
- 手动提交：
  - Oracle默认手动提交。
  - 需要先开启事务，再提交。

~~~sql
-- 查看事务的默认提交方式
select @@autocommit; -- 1表示自动提交，0表示手动提交

-- 修改事务的默认提交方式
set @@autocommit = 0;
~~~

#### 2、四大特性

- 原子性：是不可分割的最小操作单位，要么同时成功，要么同时失败。
- 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
- 隔离性：多个事务之间，相互独立。
- 一致性：事务操作前后，数据总量不变。

#### 3、隔离级别

​		多个事务之间隔离的，相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。

**存在的问题：**

- 脏读：一个事务，读取到另一个事务中没有提交的数据。
- 不可重复读（虚读）：在同一个事务中，两次读取到的数据不一样。
- 幻读：一个事务操作数据库表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。

**隔离级别：**

- read uncommitted：读未提交。
  - 产生的问题：脏读、不可重复读、幻读。
- read committed：读已提交（Oracle默认）。
  - 产生的问题：不可重复读、幻读。
- repeatable read：可重复读（MySQL默认）。
  - 产生的问题：幻读。
- serializable：串行化。
  - 可以解决所有的问题。

> 注：隔离级别从小到大安全性越来越高，但是效率越来越低。

~~~sql
-- 查看数据库隔离级别
select @@tx_isolation;
-- 或 5.7以上版本
select @@transaction_isolation;
-- 设置数据库隔离级别
-- READ-UNCOMMITTED、READ-COMMITTED、REPEATABLE-READ、SERIALEZABLE
set global transaction isolation level 级别字符串;
~~~

## JDBC的使用

### 一、什么是JDBC？

​		JDBC（Java DataBase Connection）是java访问数据库的标准规范（接口），操作数据库的是其具体的实现类，也就是数据库驱动。每个数据库厂商会根据JDBC规范编写自己的数据库驱动，所以我们只需要通过JDBC，即可操作不同的数据库。

#### url参数配置

~~~
jdbc:mysql://localhost:3306/db?characterEncoding=utf-8&serverTimezone=GMT%2b8&autoReconnect=true&failOverReadOnly=false&useSSL=false
~~~

- characterEncoding=utf-8 # 编码utf-8
- serverTimezone=GMT%2b8 # 时区 格林威治时间
- autoReconnect=true # 重连
- failOverReadOnly=false # 重连后不只读
- useSSL=false # 不使用ssl

### 二、使用步骤

~~~java
// 1、注册驱动
// 注：在mysql5之后的jar包，存在静态代码块，不需要自己再加载了。
Class.forName("com.mysql.jdbc.Driver");
// 2、获取连接对象
Connection conn = 
    DriverManager.getConnection("jdbc:mysql://localhost:3306/db","root","root");
// 3、获取预编译sql执行对象
PrepareStatement stmt = conn.getPrepareStatement(sql);
// 4、执行sql，获取结果集
String sql = "select * from tableName;";
ResultSet rs = stmt.executeQuery(sql);
// 5、处理结果
while(rs.next()){
    int id = rs.getInt(1);
}
// 6、关闭资源
rs.close();
stmt.close();
conn.close();
~~~

### 三、相关对象

- DriverManager（驱动管理对象）
  - 注册驱动方法：static void registerDriver(Driver driver)
  - 获取连接方法：static Connection getConnection(String url, String user, String password)
- Connection（数据库连接对象）
  - 获取执行sql对象：Statement createStatement()
  - 获取执行预编译sql对象：PrepareStatement prepareStatement(String sql)
  - 开启事务：setAutoCommit(boolean autoCommit) // false开启事务
  - 提交事务：commit()
  - 回滚事务：rollback()
- Statement（执行sql对象）
  - 执行任意sql：boolean execute(String sql)
  - 执行DML、DDL：int executeUpdate(String sql)
  - 执行DQL：ResultSet executeQuery(String sql)
- PrepareStatement（执行预编译sql对象）
  - sql语句预编译，效率更高。
  - sql语句使用`?`作为占位符，从而避免sql注入。
  - 给?赋值方法：setXxx(1, value) // ?索引，从1开始
- ResultSet（结果集对象）
  - 游标下移：boolean next()
  - 获取数据：xxx getXxx(int index) //列索引，从1开始
  - 获取数据：xxx getXxx(String field) //列名称

### 四、终极写法

#### 1、properties

~~~properties
# jdbc.properties
jdbc.driverClassName = com.mysql.cj.jdbc.Driver
jdbc.url = jdbc:mysql:///db?useUnicode=true
							&characterEncoding=utf8
							&autoReconnect=true
							&rewriteBatchedStatements=true
jdbc.userName = root
jdbc.password = root
~~~

#### 2、JDBCUtils

~~~java
public class JDBCUtils {
    private static String driverClassName = null; //可有可无
    private static String url = null;
    private static String userName = null;
    private static String password = null;
    
    //省略异常
    static {
        Properties properties = new Properties();
        InputStream is = 
            JDBCUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
        properties.load(is);
        driverClassName = properties.getProperty("jdbc.driverClassName");
        url = properties.getProperty("jdbc.url");
        userName = properties.getProperty("jdbc.userName");
        password = properties.getProperty("jdbc.password");
    }
    
    public static Connection getConnection() {
        return DriverManager.getConnection(rul, user, password);
    }
    
    public static void close(Statement stmt, Connection conn) {
        close(null, stmt, conn);
    }
    
    public static void close(ResultSet rs, Statement stmt, Connection conn) {
        if (rs != null) {
            rs.close();
        }
        if (stmt != null) {
            stmt.close();
        }
        if (conn != null) {
            conn.close();
        }
    }
}
~~~

### 五、JdbcTemplate

​		JdbcTemplate是Spring框架对JDBC的简单封装，用于简化JDBC的开发。

#### 1、获取JdbcTemplate对象

~~~java
JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
~~~

#### 2、常用方法

- int update(String sql) # 执行DML
- Map<String, Object> queryForMap() # 查询结果，封装为map集合。注意：仅一条结果。
- List<Map<String, Object>> queryForList() # 查询结果，每一条结果封装为map，再装进List中。
- List<T> query(String sql, RowMapper<T> rm) # 查询结构，每一条结果封装为Bean对象，再装进List中。
  - 一般使用BeanPropertyRowMapper实现类。
  - 很少手动实现RowMapper。
- queryForObject(String sql, Class c) # 查询单个数据转换为对象。

## 连接池的使用

​		连接池其实就是一个容器（集合），存放数据库连接的容器。当系统初始化之后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器。

### 一、C3P0

#### 1、配置文件

文件名不能改变

c3p0-config.xml

~~~xml
<c3p0-config>
  <!-- 使用默认的配置读取连接池对象 -->
  <default-config>
  	<!--  连接参数 -->
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/db1</property>
    <property name="user">root</property>
    <property name="password">root</property>
    
    <!-- 连接池参数 -->
    <!--初始化申请的连接数量-->
    <property name="initialPoolSize">5</property>
    <!--最大的连接数量-->
    <property name="maxPoolSize">10</property>
    <!--超时时间-->
    <property name="checkoutTimeout">3000</property>
  </default-config>

  <!-- 自定义的 -->
  <named-config name="otherc3p0"> 
    <!--  连接参数 -->
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/db2</property>
    <property name="user">root</property>
    <property name="password">root</property>
    
    <!-- 连接池参数 -->
    <property name="initialPoolSize">5</property>
    <property name="maxPoolSize">8</property>
    <property name="checkoutTimeout">1000</property>
  </named-config>
</c3p0-config>
~~~

c3p0.properties

~~~properties
c3p0.driverClass=com.mysql.cj.jdbc.Driver
c3p0.jdbcUrl=jdbc:mysql:///db
c3p0.user=root
c3p0.password=root

c3p0.acquireIncrement=3	# 
c3p0.idleConnectionTestPeriod=60	
c3p0.initialPoolSize=10	
c3p0.maxIdleTime=60	
c3p0.maxPoolSize=20	
c3p0.maxStatements=100	
c3p0.minPoolSize=5
~~~

#### 2、获取数据源

~~~java
// 默认加载default-config
DataSource ds = new ComboPooledDataSource();
// 指定加载otherc3p0
DataSource ds = new ComboPooledDataSource("otherc3p0");
~~~

### 二、Druid

#### 1、配置文件

druid.properties

~~~properties
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql:///db
username=root
password=root
# 初始池的大小
initialSize=5
# 最大活动的连接
maxActive=10
# 等待超时时间
maxWait=3000
~~~

#### 2、获取数据源

~~~java
Properties properties = new Properties();
InputSteam is = JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
properties.load(is);
DataSource ds = DruidDataSourceFactory.createDataSource(properties); 
~~~



















































































