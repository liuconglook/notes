[TOC]
## Redis简介
* Redis是一个开源的高性能的key-value存储系统（数据结构服务器,数据结构数据库）


* 异常快速：Redis的速度非常快，每秒能执行约11万集合，每秒约81000+条记录。
* 共享内存：持久化一秒读写超过十万键值
* Redis是单线程模型，而memcached支持多线程模式,所以在多核服务器上后者性能更高一些。

#### 特性

* 存储结构上的特点
  * 远程字典服务器。并允许其他应用通过tcp协议读取字典中的内容
  * 字符串类型 String
  * 散列类型 hash 【map、object】
  * 列表类型 List 【链表、队列】，支持重复元素。
  * 集合类型 Set 【List、数组】，不允许重复元素。
  * 有序集合类型 Sorted Set【有序List】，不允许重复元素。
* 内存存储与持久化
  * 数据都存在内存中,速度快
  * 持久化到硬盘
* 功能丰富
  * 可做为database,cache,message broker( queue)，发布订阅MQ
  * MQ产品(Rabbitmq,AtivieMQ,zeroMQ)
  * 限定占用内存大小,数据达到空间限制后可以按照一定规则自动淘汰掉不需要的键值
* 简单稳定

#### 应用场景

- 缓存（数据查询、短连接、新闻内容、商品内容等等）
- 聊天室的在线好友列表
- 任务队列。（秒杀、抢购、12306等等）
- 应用排行榜
- 网站访问统计
- 数据过期处理（可以精确到毫秒）
- 分布式集群架构中的session分离

#### 主流的NOSQL产品

- 键值(Key-Value)存储数据库
  - 相关产品： Tokyo Cabinet/Tyrant、Redis、Voldemort、Berkeley DB
  - 典型应用： 内容缓存，主要用于处理大量数据的高访问负载。
  - 数据模型： 一系列键值对
  - 优势： 快速查询
  - 劣势： 存储的数据缺少结构化
- 列存储数据库
  - 相关产品：Cassandra, HBase, Riak
  - 典型应用：分布式的文件系统
  - 数据模型：以列簇式存储，将同一列数据存在一起
  - 优势：查找速度快，可扩展性强，更容易进行分布式扩展
  - 劣势：功能相对局限
- 文档型数据库
  - 相关产品：CouchDB、MongoDB
  - 典型应用：Web应用（与Key-Value类似，Value是结构化的）
  - 数据模型： 一系列键值对
  - 优势：数据结构要求不严格
  - 劣势： 查询性能不高，而且缺乏统一的查询语法
- 图形(Graph)数据库
  - 相关数据库：Neo4J、InfoGrid、Infinite Graph
  - 典型应用：社交网络
  - 数据模型：图结构
  - 优势：利用图结构相关算法。
  - 劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。

## 安装Redis

### Linux下安装

~~~bash
# 安装gcc编译器
yum install gcc-c++
# 安装tcl
yum install -y tcl

# 下载
wget http://download.redis.io/releases/redis-4.0.6.tar.gz|tar xz
# 编译
cd redis-4.0.6/
make
# 安装
cd src
make install PREFIX=/usr/local/redis

# 测试
make test
~~~

~~~bash
# 清理上次编译，并重新编译
make distclean  && make
~~~

~~~bash
# copy配置文件
cp /安装目录/reids.conf /usr/local/redis
# 修改配置文件
masterauth redis # 设置密码
logfile /usr/local/redis/logs/redis.log # 设置日志文件路径，前提是要事先创建好目录和文件
~~~

#### 启动文件

/lib/systemd/system/redis.service

~~~
[Unit]
Description=Redis
After=network.target

[Service]
ExecStart=/usr/local/bin/redis-server /usr/local/redis/redis.conf  --daemonize no
ExecStop=/usr/local/bin/redis-cli -h 127.0.0.1 -p 6379 shutdown

[Install]
WantedBy=multi-user.target
~~~

- 刷新配置
  - `systemctl daemon-reload`
- 创建软连接，自启
  - `systemctl enable redis`

### Windows下安装

https://redis.io/download下载压缩包，解压到安装目录

打开`redis.windows-service.conf`配置文件，搜索`requirepass`配置密码

~~~shell
requirepass xxx
~~~

#### 启动文件

> startup.bat

~~~bat
redis-server.exe redis.windows-service.conf
~~~

双击`startup.bat`运行即可

### 登录

#### Linux登录

~~~shell
# 登录
redis-cli
# 或
redis-cli -h 127.0.0.1 -p 6379 -a "mypass"

# 查看密码
config get requirepass
# 设置密码
config set requirepass xxx
~~~

#### Windows登录

~~~shell
# 进入安装目录后
redis-cli.exe -h 127.0.0.1 -p 6379 -a "mypass"
~~~



## Redis命令大全

#### key（键）

- **key操作（7）**
  - ==**TYPE key**==    查看key的value类型
  - ==**KEYS pattern**==    模糊查询key
  - ==**DEL key [key ...]**==    删除一个或多个key【返回删除数量】
  - **EXISTS key**    判断key是否存在
  - **RANDOMKEY** 随机返回一个key
  - **RENAME key newkey**    重命名key
  - **RENAMENX key newkey**    newkey不存在时重命名

- **key的value操作（3）**
  * **SORT key [pattern] \[limit offset count]**    排序和分页
  * **DUMP key**    序列化key的值【返回序列化值】
  * **RESTORE key ttl serialized-value**    将value反序列化后给定key且设置过期时间

- **key的生存时间（7）**
  * **EXPIRE key seconds**    设置key的过期时间【当前秒】
  * **PEXPIRE key milliseconds**    设置key的过期时间【当前毫秒】
  * **EXPIREAT key timestamp**    设置key的过期时间戳【日期戳秒】
  * **PEXPIREAT key milliseconds-timestamp**    设置key的过期时间戳【日期戳毫秒】
  * **PERSIST key**    移除key的生存时间
  * **TTL key**    查询key的剩余时间【秒】
  * **PTTL key**    查询key的剩余时间【毫秒】

- **其他操作（3）**
  * **MOVE key db**    移动指定key到db中【0-15】

  * **MIGRATE host port key destination-db timeout [copy]\[replace]**    将key原子性迁移【默认replace】

  * **OBJECT subcommand [arguments [arguments]]**    查看redis对象【REFCOUNT引用次数、ENCODING编码类型、IDLETIME空转时间】

#### String（字符串）

* **get操作（6）**
* **APPEND key value**    在key的值后面追加value【返回总长度】
  * ==**GET key**==    获取key的值
  * **STRLEN key**    返回key所存储的字符串值的长度
  * **MGET key [key ...]**    返回一个或多个key的值
  * **GETRANGE key start end**    截取指定范围字符串
  * **GETSET key value**    设置key的新值，返回旧值
  
* **set操作（7）**

  * ==**SET key value [EX seconds]\[PX milliseconds]\[NX|XX]**==    设置key-value
  * **SETEX key seconds value**    设置key的生存时间及value【秒】
  * **PSETEX key milliseconds value**    设置key的生存时间及value【毫秒】
  * **MSET key value [key value ...]**    设置一个或多个key和值
  * **SETNX key value**    key不存在才设置
  * **MSETNX key value [key value ...]**    key不存在才设置
  * **SETRANGE key offset value**    设置覆盖offset开始的value值

* **bit操作（4）**

  * **SETBIT key offset value**    设置key的offset位置0或1
  * **GETBIT key offset**    获取key的offset位置值
  * **BITCOUNT key [start] \[end]**    指定范围的，设置为1的bit位数量
  * **BITOP operation destkey key [key ...]**    除not外都可以接受多个key【and、or、xor、not】

* **增减操作（5）**

  * **DECR key**    value递减

  * **DECRBY key decrement**    value根据decrement递减

  * **INCR key**    value递增

  * **INCRBY key increment**    value根据increment递增

  * **INCRBYFLOAT key increment**    value根据increment浮点数递增

#### Hash（哈希表）

* ==**HDEL key field [field ...]**==    删除key中的一个或多个指定域，忽略不存在域

* **HEXISTS key field**    判断key中的指定域是否存在

* ==**HGET key field**==    返回key中的指定域的值

* ==**HGETALL key**==    返回key中所有的域和值

* **HINCRBY key field increment**    key中指定域增量increment

* **HINCRBYFLOAT key field increment**    key中指定域浮点数增量increment

* **HKEYS key**    返回key中的所有域

* **HVALS key**    返回key中所有域的值

* **HLEN key**    返回key中域的数量

* **HMGET key field [field ...]**    返回一个或多个域的值

* **HMSET key field value [field value ...]**    设置一个或多个域和值

* ==**HSET key field value**==   设置key中域field的值

* **HSETNX key field value**    key中域field不存在，才设值

#### List（列表）

* ==**LLEN key**==    返回列表长度

* ==**LRANGE key start stop**==    返回指定范围的元素

* ==**LINDEX key index**==    返回下标元素

* ==**LSET key index value**==    设置key下标元素

* **LTRIM key start stop**    修剪指定范围元素

* push操作

  * **LINSERT key BEFORE|AFTER pivot value**    插入元素，位于元素pivot之前或之后
  * **RPUSHX key value**    当列表存在，才右推送元素
  * **LPUSHX key value**    当列表存在，才左推送元素
  * ==**LPUSH key value [value ...]**==    左推送一个或多个元素
  * ==**RPUSH key value [value ...]**==    右推送一个或多个元素

* pop操作

  - **LREM key count value**    移除列表中count个与参数value相等的元素

  * ==**LPOP key**==    左弹出元素
  * ==**RPOP key**==    右弹出元素
  * **BLPOP key [key ...] timeout**    阻塞版本左弹出，等待timeout结束，0表示无限等待
  * **BRPOP key [key ...] timeout**    阻塞版本右弹出，等待timeout结束，0表示无限等待

* 转移操作
  * **RPOPLPUSH source destination**    source右弹出给新list左推送
  * **BRPOPLPUSH source destination timeout**    阻塞版本右弹出给新list左推送

#### Set（集合）

* ==**SADD key member [member ...]**==    将一个或多个member元素加入到集合key
* **SCARD key**    返回集合中元素的数量
* **SISMEMBER key member**    判断member是否在key中
* ==**SMEMBERS key**==    返回集合key中的所有成员
* ==**SREM key member [member ...]**==    移除一个或多个member
* 多个集合操作
  * **SDIFF key [key ...]**    返回差集（A-B）
  * **SDIFFSTORE destination key [key ...]**    返回差集到destination
  * **SINTER key [key ...]**    返回交集（AUB）
  * **SINTERSTORE destination key [key ...]**    返回交集到destination
  * **SUNION key [key ...]**    返回并集（A+B去重）
  * **SUNIONSTORE destination key [key ...]**    返回并集到destination
  * **SMOVE source destination member**    移动source中member元素到destination集合中
* 随机返回
  * **SPOP key**    随机弹出元素并返回
  * **SRANDMEMBER key [count]**    返回一个随机元素，count范围内的一个元素，否则返回整个集合

#### SortedSet（有序集合）

* ==**ZADD key score member [[score member] ...]**==    将一个或多个member元素添加到key中
* ==**ZCARD key**==    返回有序集合中的元素数量
* **ZCOUNT key min max**    返回score值在min和max之间的成员数量【包含min和max】
* **ZINCRBY key increment member**    score值增量increment
* ==**ZRANGE key start stop [WITHSCORES]**==    返回指定区间的成员【所有score】
* **ZRANGEBYSCORE key min max [WITHSCORES] \[LIMIT offset count]**    返回score值在min和max之间的成员【包含min和max】
* **ZRANK key member**    返回（小到大）member的排名【0开始】
* ==**ZREM key member [member ...]**==    移除有序集key中一个或多个member
* **ZREMRANGEBYRANK key start stop**    移除排名区间内的所有成员
* **ZREMRANGEBYSCORE key min max**    移除score值在min和max之间的成员【包含min和max】
* **ZREVRANGE key start stop [WITHSCORE]**    返回key中指定区间内的成员
* **ZREVRANGEBYSCORE key max min [WITHSCORE] \[LIMIT offset count]**    返回score值在max和min之间的成员【包含min和max】
* **ZREVRANK key member**    返回（大到小）member的排序【0开始】
* **ZSCORE key member**    返回member的score
* **ZUNIONSTORE destination numkeys key [key ...] \[WEIGHTS weight [weight ...]]\[AGGREGATE SUM|MIN|MAX]**    将并集结果放到destination，numkeys指定key的数量，weights乘法因子默认1，aggregate聚合方式score
* **ZINTERSTORE destination numkeys key [key ...] \[WEIGHTS weight [weight ...]] \[AGGREGATE SUM|MIN|MAX]**    将交集结果放到destination

#### Pub/Sub（发布/订阅）

* **PSUBSCRIBE pattern [pattern ...]**    订阅一个或多个符合给定模式的频道

* **PUBLISH channel message**    发布message到指定频道channel

* **PUBSUB \<subcommand> [argument [argument ...]]**
  * **PUBSUB CHANNELS [pattern]**    匹配pattern频道，且活跃的用户（至少有一个订阅者）
  * **PUBSUB NUMSUB [channel-1 ... channel-N]**    返回给定频道的订阅数
  * **PUBSUB NUMPAT**    返回订阅模式的数量（所有客户端订阅的总和）

* **PUNSUBSCRIBE [pattern [pattern ...]]**    退订一个或多个给定模式

* **SUBSCRIBE channel [channel ...]**    订阅给定的一个或多个频道信息

* **UNSUBSCRIBE [channel [channel ...]]**    指定客户端退订给定的频道

#### Transaction（事务）

* **DISCARD**    取消事务

* **EXEC**    执行事务

* **MULTI**    开启事务

* **WATCH key [key ...]**    监视一个或多个key，在开启事务之前key有所改动，那么事务将会被打断

* **UNWATCH**    取消对所有key的监视

#### Connection（连接）

* **AUTH password**    认证密码
* **ECHO message**    打印message
* **PING**    测试连接，成功返回pong
* **QUIT**    退出客户端
* **SELECT index**    切换数据库【0-15】

## 服务端配置

### 一、持久化机制

#### 1、RDB（默认）

全量持久化

在一定的间隔时间中，检测key的变化情况，然后持久化数据。

> 编辑配置文件（redis.windows.conf）

~~~text
#   after 900 sec (15 min) if at least 1 key changed
save 900 1
#   after 300 sec (5 min) if at least 10 keys changed
save 300 10
#   after 60 sec if at least 10000 keys changed
save 60 10000
~~~

配置完需要重启redis服务器，并指定配置文件

~~~shell
redis-server.exe redis.windows.conf
~~~

#### 2、AOF

增量持久化

日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据。

> 编辑配置文件（redis.windows.conf）

~~~
appendonly no/yes

# appendfsync always # 每一次操作都进行持久化
appendfsync everysec # 每隔一秒进行一次持久化
# appendfsync no	 # 不进行持久化
~~~

#### 3、远程访问

~~~
# 任意ip都可以访问
bind 0.0.0.0
~~~

## 性能测试

~~~shell
# 测试get/set请求，10万次请求
redis-benchmark -t set,get -n 100000
# 测试get请求，10万请求，并发100，2048字节数据
redis-benchmark -t get -n 100000 -c 100 -d 2048
# 登录：redis-cli -a "password",输入info，查看Memory内存部分
connected_clients:101 #redis连接数

used_memory:8367424 # Redis 分配的内存总量 
used_memory_human:7.98M
used_memory_rss:11595776 # Redis 分配的内存总量(包括内存碎片) 
used_memory_rss_human:11.06M
used_memory_peak:8367424
used_memory_peak_human:7.98M # Redis所用内存的高峰值
used_memory_peak_perc:100.48%

~~~

吞吐量（TPS）：指系统在单位时间内处理请求的数量。

响应时间（RT）：指系统对请求作出响应的时间。一般讨论的是所有功能的平均响应时间或者最大响应时间。

每秒查询率（QPS）：对一个特定的查询服务器在规定时间内所处理流量多少的衡量标准。

## 集群

#### 主从复制

~~~shell
# 复制redis
cd /usr/local/redis
copy -R bin/ redis01
# 删除aof文件
rm -rf appendonly.aof
rm -rf dump.rdb
# 配置文件,设为从服务器
slaveof [主服务器ip] [主服务器port]
port 6380
bind 0.0.0.0
# 运行
./redis-server redis.conf
# 另起一个终端，查看是否启动
ps -ef | grep redis
# 登录
cd /usr/local/redis/redis01
./redis-cli -h ip -p 6380
# 主服务器查看从服务器信息
info replication
~~~

#### 高可用

- 保证高可用的核心准则是冗余。
- 避免单点故障，自动故障转移。
- 分担压力，读写分离。

#### 手动故障转移

~~~shell
cd /usr/local/redis
cp -R redis01/ redis02
# 修改端口为6381
# 关闭主服务器6379
shutdown
# 手动将6380切换成主服务器
slaveof no one
# 手动将6381的主服务切换成6380
slaveof ip 6380
~~~

#### Sentinel哨兵

~~~shell
cd /usr/local/redis/
# 复制安装目录下的sentinel.conf
cp /opt/redis-4.0.6/sentinel.conf sentinel01.conf
# 编辑
vim sentinel.conf
~~~

~~~
# 补充下面内容
bind 0.0.0.0
# 哨兵 监听 主节点名 主节点ip 端口 主节点宕机，选举新的主节点至少有1个sentinel同意
sentinel monitor mymaster ip 6379 1
# 响应超时时间，在该时间内未响应则说明宕机了，单位毫秒
sentinel down-after-milliseconds mymaster 10000
# 故障转移超时时间，在规定时间内未触发failover操作，则说明故障转移失败，单位毫秒
sentinel failover-timeout mymaster 60000
# 故障转移时，有多少个服务器同时从主服务器同步，数字越小表示同时同步的从服务器越少，故障转移的时间也就越长
sentinel parallel-syncs mymaster 1
~~~

~~~shell
# 启动sentinel
redis-sentinel sentinel.conf
~~~

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~

~~~yml
# SpringDataRedis
spring:
  redis:
    password: 123456
    sentinel:
      master: mymaster
      nodes: 192.168.200.129:26379
~~~

#### 设置密码

Redis默认开启保护模式protected-mode yes

> 在所有服务器redis.conf都配置上

~~~shell
# 设置密码
requirepass password
# 设置访问主服务器密码
masterauth password
~~~

> sentinel.conf配置

~~~shell
# 哨兵节点密码
sentinel auth-pass mymaster password
~~~

#### 内置集群（Cluster）

7001/7002/7003/7004/7005/7006

> 主节点配置

```shell
# 不能设置密码requirepass，否则集群启动时会连接不上
bind 0.0.0.0
# 修改端口号
port 7001
# Redis后台启动
daemonize yes
# 开启aof持久化
appendonly yes
# 开启集群
cluster-enabled yes
# 集群的配置 配置文件首次启动自动生成
cluster-config-file nodes.conf
# 请求超时
cluster-node-timeout 5000
```

> 启动集群

redis集群的管理工具使用的是ruby脚本语言，安装集群需要ruby环境：

~~~shell
# 安装ruby
yum -y install ruby ruby-devel rubygems rpm-build
# 查看ruby版本
ruby -v
# 升级ruby版本，redis4.0.14集群环境需要2.2.2以上的ruby版本
# 添加阿里镜像
gem sources -a http://mirrors.aliyun.com/rubygems/
# 删除原镜像，否则下载缓慢
gem sources --remove https://rubygems.org/
# 安装RAM(Ruby Version Manager)
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
curl -sSL https://get.rvm.io | bash -s stable
# 更新配置
source /etc/profile.d/rvm.sh
# 查看ruby所有版本
rvm list known
# 安装ruby2.5
rvm install 2.5
# 下载符合环境要求的gem，下载地址如下：https://rubygems.org/gems/redis/versions/4.1.0
# 安装redis集群环境
gem install redis # -v 4.1.0
# 查看是否安装
gem list | grep redis
~~~

~~~shell
# 进入redis安装包
cd /root/redis-4.0.14/src/
# 使用集群管理脚本启动集群
# 注意：如有安全组，需要开放集群端口，如redis端口7001，则总线端口为7001+1000，开放17001/17006
./redis-trib.rb create --replicas 1 ip:7001 ip:7002 ip:7003 ip:7004 ip:7005 ip:7006 
~~~

~~~shell
# ERR Slot 0 is already busy (Redis::CommandError)
# slot插槽被占用了、这是因为 搭建集群前时，以前redis的旧数据和配置信息没有清理干净。
# 每个节点重置集群，重新启动集群
redis-cli -p 7001
flushall
cluster reset
~~~

~~~shell
# 登录集群
redis-cli -h ip -p 7001 -c # -a 123
# 登录后设置密码
config set masterauth 123
config set requirepass 123
~~~

```yml
# SpringDataRedis
spring:
  redis:
    cluster:
      nodes: ip:7001,ip:7002,ip:7003,ip:7004,ip:7005,ip:7006
```

> 集群特点

- 所有的redis节点彼此互联(PING-PONG机制)，内部使用二进制协议优化传输速度和带宽。

- 节点的fail是通过集群中超过半数的master节点检测失效时才生效。

- 客户端与redis节点直连，不需要连接集群所有节点，只需要连接集群中任意可用节点即可。

- 集群把所有的物理节点映射到[0-16383]slot上,cluster 负责维护node<>slot<>key关系

> 哈希槽

~~~shell
# 查看集群的每个节点负责的哪一部分哈希槽
redis-cli -p 7001 cluster nodes | grep master
~~~

- 集群搭建的时候分配哈希槽到节点上

- 使用集群的时候，先对数据key进行CRC16的计算

- 对计算的结果求16384的余数，得到的数字范围是0~16383

- 根据余数找到对应的节点（余数对应的哈希槽在哪个节点）

- 跳转到对应的节点，执行命令。

> 集群的主从复制模型

- 保证其可用性，每个节点都会有一个或多个复制品。

> 一致性保证

- 主节点对命令的复制工作发生在返回命令回复之后。

- 无法保证在复制是否成功。

- 如果在复制之后返回命令回复，那么主节点处理命令请求的速度将极大地降低 。

> 集群维护

~~~shell
# 1、分片重哈希，可以连接任意节点
./redis-trib.rb reshard ip:7001
# 提示需要移动多少个hash槽，直接输入要移动的hash槽数量即可，例如移动1000个
# How many slots do you want to move (from 1 to 16384)?1000
# 提示接受的节点id是多少，我们使用7002接受1000个hash槽，填写对应节点的id
# What is the receiving node ID? 0eba44418d7e88f4d819f89f90da2e6e0be9c680
# 提示移出hash槽的节点id，all表示所有节点都移出插槽，也可以填写单独节点id，最后键入done
# 这里填写all
Please enter all the source node IDs.
  Type 'all' to use all the nodes as source nodes for the hash slots.
  Type 'done' once you entered all the source nodes IDs.
Source node #1:all
# 最后，要是否确认这样进行重哈希，填写yes
Do you want to proceed with the proposed reshard plan (yes/no)? yes
~~~

~~~shell
# 2、移除节点
# 注意：如果主节点不是空的,需要将这个节点的数据重新分片到其他主节点上
./redis-trib.rb del-node 192.168.200.129:7001 cbd415973b3e85d6f3ad967441f6bcb5b7da506a
~~~

~~~shell
# 3、添加主节点
# 注意：新节点默认是没有哈希槽的，需要手动分配哈希槽
./redis-trib.rb add-node 192.168.200.129:7005 192.168.200.129:7001
# 添加从节点
# 注意：集群默认自动分配对应的主节点。
./redis-trib.rb add-node --slave 192.168.200.129:7005 192.168.200.129:7001
~~~

> 集群现状

- redis3.0才有的cluster集群。
- Cluster是无中心节点的集群架构，依靠Gossip协议（谣言传播）协同自动化修复集群的状态。
  - Gossip有消息延时和消息冗余的问题，在集群节点数量过多的时候，节点之间需要不断进行PING/PANG通讯，不必须要的流量占用了大量的网络资源。虽然Redis4.0对此进行了优化，但这个问题仍然存在。
- Cluster可以进行节点的动态扩容缩容，在扩缩容的时候，就需要进行数据迁移。
  - Redis 为了保证迁移的一致性， 迁移所有操作都是同步操作，执行迁移时，两端的 Redis 均会进入时长不等的 阻塞状态。对于小 Key，该时间可以忽略不计，但如果一旦 Key 的内存使用过大，严重的时候会接触发集群内的故障转移，造成不必要的切换。

- 消息的延迟
  - 由于 Gossip 协议中，节点只会随机向少数几个节点发送消息，消息最终是通过多个轮次的散播而到达全网的。因此使用 Gossip 协议会造成不可避免的消息延迟。不适合用在对实时性要求较高的场景下。

- 消息冗余
    - Gossip 协议规定，节点会定期随机选择周围节点发送消息，而收到消息的节点也会重复该步骤。
        因此存在消息重复发送给同一节点的情况，造成了消息的冗余，同时也增加了收到消息的节点的处理压力。
        而且，由于是定期发送而且不反馈，因此即使节点收到了消息，还是会反复收到重复消息，加重了消息的冗余。

#### 一致性哈希

- 分片
  - 使用twemproxy实现hash分片的Redis集群方案。
  - twemproxy作为代理服务器，将数据分片存储在集群节点上。
  - 而如何做到平均分布，就要涉及到一致性哈希算法。
- 传统哈希方案
  - 取对象的哈希值，对节点个数取模，再映射到相应编号的节点。
  - 缺点是当节点个数变动时，大多数映射关系会失效，需要迁移。
- 一致性哈希
  - 一致性哈希算法（Consistent Hashing Algorithm）是一种分布式算法，常用于负载均衡。
  - 将节点ip哈希到这个0~2^32^的环型空间中。
  - 当有请求进来时，根据key计算hash值，再顺时针找到最近的一个节点处理。
  - 新增和删除节点只需要把受影响的数据迁移到新节点即可。
- 如何判断一个哈希算法的好坏
  - 平衡性（Balance）
    - 尽可能均匀分散到不同的服务器。
    - 一致性哈希也不能保证每个服务器处理的请求数量大致相同。
  - 单调性（Monotonicity）
    - 新增节点时，原有请求不会被重新映射到其他服务器上去。
  - 分散性（Spread）
- 虚拟节点
  - 当一部分节点下线之后，剩余机器的负载就会出现不均衡，也就是hash倾斜。
  - 增加虚拟节点也就均衡了剩余节点所占的范围。
  - 假设原本四个节点，每个节点占25%，后下线两个。
  - 剩余2个节点，a节点占25%，b节点占75%，这就会出现负载不均衡。
  - 当再加入虚拟节点a2、b2时，通过hash，a2占25%，b2占25%。这样就可以均衡不同请求了。

#### Twemproxy集群

Twemproxy由Twitter开源，是一个redis和memcache快速/轻量级代理服务器，利用中间件做分片的技术。twemproxy处于客户端和服务器的中间，将客户端发来的请求，进行一定的处理后（sharding），再转发给后端真正的redis服务器。

官方网址：https://github.com/twitter/twemproxy

**作用：** Twemproxy通过引入一个代理层，可以将其后端的多台Redis或Memcached实例进行统一管理与分配，使应用程序只需要在Twemproxy上进行操作，而不用关心后面具体有多少个真实的Redis或Memcached存储 **特性：**

- 支持失败节点自动删除

  可以设置重新连接该节点的时间

  可以设置连接多少次之后删除该节点

- 减少客户端直接与服务器的连接数量

  自动分片到后端多个redis实例上

- 多种哈希算法

  md5，crc16，crc32，crc32a，fnv1_64，fnv1a_64，fnv1_32，fnv1a_32，hsieh，murmur，jenkins

- 多种分片算法

  ketama（一致性hash算法的一种实现），modula，random

~~~shell
# 1、准备redis实例7601/7602/7603
# 关闭保护模式
protected-mode no
daemonize yes # 后台启动
appendonly yes # aof持久化
~~~

~~~shell
# 2、安装twemproxy
# 准备环境
yum  -y install install autoconf automake libtool
# 项目地址：https://github.com/twitter/twemproxy
# 下载
wget https://github.com/twitter/twemproxy/archive/v0.4.1.tar.gz
# 解压
tar -zxvf twemproxy.tar
# 安装
cd twemproxy
autoreconf -fvi
./configure --enable-debug=full --prefix=/usr/local/twemproxy
make
make install
# 编写配置文件
cd /usr/local/twemproxy/
# 创建两文件夹
mkdir conf run
vim conf/nutcracker.yml
# 配置如下
alpha:
  listen: ip:22121
  hash: fnv1a_64
  distribution: ketama
  auto_eject_hosts: true
  redis: true
  server_retry_timeout: 2000
  server_failure_limit: 1
  servers:
   - ip:7601:1
   - ip:7602:1
   - ip:7603:1
   
# 测试配置文件
./sbin/nutcracker -t
# 启动
./sbin/nutcracker -d -c /usr/local/twemproxy/conf/nutcracker.yml -p /usr/local/twemproxy/run/redisproxy.pid -o /usr/local/twemproxy/run/redisproxy.log
# 查看启动信息
ps -ef | grep nutcracker
# 查看端口使用情况
netstat -nltp | grep nutcrac
# 登录
redis-cli -p 22121
~~~

~~~yml
# SpringDataRedis
spring:
  redis:
    host: 192.168.200.129
    port: 22121
~~~

#### 使用场景

- 单机版：数据量，QPS不大的情况使用
- 主从复制：需要读写分离，高可用的时候使用
- Sentinel哨兵：需要自动容错容灾的时候使用
- 内置集群：数据量比较大，QPS有一定要求的时候使用，但集群节点不能过多
- twemproxy集群：数据量，QPS要求非常高，可以使用

## Jedis

### 一、概念

- 一款java操作redis数据库的工具。（java客户端）

### 二、使用

- 获取连接
  - `Jedis jedis = new Jedis();` # 无参构造，默认“localhost”，6379端口。
- 操作
  - `jedis.set("key","value")` # 存储数据

### 三、连接池

#### 1、JedisPool

~~~java
// 1、配置类对象
JedisPoolConfig config = new JedisPoolConfig();
config.setMaxTotal(50);
config.setMaxIdle(10);
// 2、连接池对象
JedisPool jedisPool = new JedisPool(config, "localhost", 6379);
// 3、获取连接
Jedis jedis = jedisPool.getResource();
//...
// 4、关闭
jedis.close();
~~~

#### 2、JedisPoolUtils

~~~java
public class JedisPoolUtils {
    private static JedisPool jedisPool;
    static{
        // 1、读取配置文件
        InputStream is = JedisPoolUtils.class
            .getClassLoader().getResourceAsStream("jedis.properties");
        Properties pro = new Properties();
        pro.load(is);
        
        // 2、获取数据，初始化配置类
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));

        // 3、初始化JedisPool
        jedisPool = new JedisPool(config,
                                  pro.getProperty("host"),
                                  Integer.parseInt(pro.getProperty("port")));
    }
    //获取连接方法
    public static Jedis getJedis(){
        return jedisPool.getResource();
    }
}
~~~

## 整合Springboot

### SpringDataRedis

Jedis：redis直连客户端，多线程环境下不安全。

Lettuce：分布式的redis连接池，多线程安全的。

#### pom.xml

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

#### application.xml

~~~yml
spring:
  redis:
    database: 0  # Redis几号库
    host: localhost  # Redis服务器地址
    port: 6379  # Redis服务器连接端口
    password: redis # Redis服务器连接密码（默认为空）
    timeout: 10000 # 超时时间（毫秒）
    lettuce:
      pool:
        max-active: 200 # 连接池最大连接数（使用负值表示没有限制）
        max-wait: -1 # 连接池最大阻塞等待时间（使用负值表示没有限制）
        max-idle: 8 # 连接池中的最大空闲连接
        min-idle: 0 # 连接池中的最小空闲连接
~~~

#### Config

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

#### 工具类

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

### Mybatis二级缓存

#### application.yml

~~~yml
mybatis-plus: # 或mybatis
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath*:com/**mapper/*.xml
  type-aliases-package: com.belean.mall.tiny.mbg.model
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    cache-enabled: true # 开启二级缓存
    map-underscore-to-camel-case: true # 开启驼峰命名
~~~

#### 重写Mybatis缓存

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

#### 使用

注解Mapper接口 或 配置Mapper.XML

~~~java
@CacheNamespace(implementation = MybatisRedisCache.class)
public interface PmsBrandMapper extends BaseMapper<PmsBrand> {
}
~~~

~~~xml
<cache type="com.belean.mall.tiny.config.mybatis.MybatisRedisCache"/>
~~~



