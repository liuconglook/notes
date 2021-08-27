## MongoDB

#### 一、MongoDB介绍

* MongoDB使用C++语言编写的非关系型数据库，是一个基于分布式文件存储的开源数据库系统。
* 在高负载的情况下，添加更多的节点，可以保证服务器性能。
* MongoDB 旨在为WEB应用提供可扩展的高性能数据存储解决方案。
* MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。
* MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

| 介绍                 | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| 什么是MongoDB        | 一个以JSON为数据库模型的文档数据库                           |
| 为什么是文档数据库？ | 文档来自于“JSON Document”，并非我们一般理解的PDF、WORD文档   |
| 谁开发MongoDB？      | 上市公司MongoDB Inc，总部位于美国纽约                        |
| 主要用途             | 应用数据库，类似与Oracle、MySQL。海量数据处理，数据平台      |
| 主要特点             | 建模为可选。JSON数据模型比较适合开发者。横向扩展可以支持很大数据量和并发。 |
| MongoDB是免费的吗？  | MongoDB有两个发布版本：社区版和企业版。社区版是基于SSPL，一种和AGPL基本类似的开源协议。企业版是基于商业协议，需付费使用 |

##### 1、发展历程

- 0.x：起步阶段（2008）
- 1.x：支持复制集和分片集（2010）
- 2.x：更丰富的数据库功能（2012）
- 3.x：WiredTiger和周边生态环境（2014）
- 4.x：分布式事务支持（2018）

##### 2、主要特性：

* 文档：是MongoDB中数据的基本单元，非常类似于关系型数据库系统中的行（但是比行要复杂很多）。
* 集合：就是一组文档，如果说MongoDB中的文档类似于关系型数据库中的行，那么集合就如同表。
* MongoDB的单个计算机可以容纳多个独立的数据库，每一个数据库都有自己的集合和权限。
* MongoDB自带简洁但功能强大的JavaScript shell，这个工具对于管理MongoDB实例和操作数据库作用非常大。
* 每一个文档都有一个特殊的键"_id",它在文档所处的集合中是唯一的，相当于关系数据库中的表的主键。

##### 3、对比关系型数据库

| 对比项    | MongoDB    | MySQL、Oracle |
| ------ | ---------- | ------------ |
| 表      | 集合List     | 二维表Table     |
| 表的一行数据 | 文档Document | 一条记录Record   |
| 表字段    | 键key       | 字段Field      |
| 字段值    | 值value     | 值value       |
| 主外键    | 无          | PK，FK        |
| 灵活度扩展性 | 极高         | 差            |

> 关系数据的表的Record必须保证拥有每一个Field。MongoDB的每一个Document的key可以不一样。
>
> 关系型数据查询使用SQL。MongoDB查询使用内置Find函数，基于BSON的特殊查询工具。

|              | MongoDB                                                     | RDBMS                  |
| ------------ | ----------------------------------------------------------- | ---------------------- |
| 数据模型     | 文档模型                                                    | 关系模型               |
| 数据库类型   | OLTP                                                        | OLTP                   |
| CRUD操作     | MQL/SQL                                                     | SQL                    |
| 高可用       | 复制集                                                      | 集群模式               |
| 横向扩展能力 | 通过原生分片完善支持                                        | 数据分区或者应用侵入式 |
| 索引支持     | B-树、全文索引、地理位置索引、多键(multikey)、索引、TTL索引 | B树                    |
| 开发难度     | 容易                                                        | 困难                   |
| 数据容量     | 没有理论上限                                                | 千万、亿               |
| 扩展方式     | 垂直扩展+水平扩展                                           | 垂直扩展               |

##### 4、MongoDB优势

- 简单直观：以自然的方式来建模，以直观的方式来与数据库交互。
  - 一个模型对应一个集合
- 结构灵活：弹性模式从容响应需求的频繁变化。
  - 多形性：同一个集合中可以包含不同字段（类型）的文档对象。
  - 动态性：线上修改数据模式，修改时应用于数据库均无需下线。
  - 数据治理：支持使用JSON Schema来规范数据模式。在保证模式灵活动态的前提下，提供数据治理能力。
- 快速开发：做更多的事，写更少的代码。
  - 数据库引擎只需在一个存储区读写。
  - 反范式、无关联的组织极大优化查询速度
  - 程序API自然，快速开发。
- 原生复制集
  - Replicat Set - 2 to 50个成员
  - 自恢复
  - 多中心容灾能力
  - 滚动服务 - 最小化服务终端
- 横向扩展能力
  - 需要的时候无缝扩展
  - 应用全透明
  - 多种数据分布策略
  - 轻松支持TB - PB数据级

> 优势总结

- JSON结构和对象模型接近，开发代码量低
- JSON的动态模型意味着更容易响应新的业务需求
- 复制集提供99.999%高可用
- 分片架构支持海量数据和无缝扩容

##### 5、全家桶

| 软件模块                | 描述                                         |
| ----------------------- | -------------------------------------------- |
| monod                   | MongoDB 数据库软件                           |
| mongo                   | MongoDB命令行工具，管理MongoDB数据库         |
| mongos                  | MongoDB路由进程，分片环境下使用              |
| mongoump/mongorestore   | 命令行数据库备份与恢复工具                   |
| mongoexport/mongoimport | CSV/JSON导入导出，主要用于不同系统间数据迁移 |
| Compass                 | MongoDB GUI管理工具                          |
| Ops Manager(企业版)     | MongoDB集群管理软件                          |
| BI Connector(企业版)    | SQL解析器/BI套接键                           |
| MongoDB Charts(企业版)  | MongoDB可视化软件                            |
| Atlas(付费及免费)       | MongoDB云托管服务，包括永久免费云数据库      |

#### 二、安装步骤

##### 1、官网下载社区版

需注册登录官网：<https://www.mongodb.com/download-center/community>

下载zip格式，手动安装

##### 2、创建配置文件

新建配置文件mongod.cfg

https://docs.mongodb.com/manual/reference/configuration-options/

data、log目录需自己创建

~~~txt
# db数据存放的路径
storage:
  dbPath: C:\web\mongodb-plus-4.2.1\data
  journal:
    enabled: true
# 日志存放的路径
systemLog:
  destination: file
  logAppend: true
  path:  C:\web\mongodb-plus-4.2.1\log\mongod.log

# mongodb的ip和端口
net:
  port: 27017
  bindIp: 127.0.0.1
  
# 权限认证
# 注意：创建用户后再加
security:
  authorization: enabled
~~~

##### 3、安装/启动服务

**管理员权限**打开cmd

~~~cmd
# 注册服务到windows
mongod.exe --config "C:\MongoDB\mongod.cfg" --install --serviceName "MongoDB"

# 配置相关
https://www.jianshu.com/p/d5f0c2836421
~~~

启动服务

~~~cmd
net start MongoDB
~~~

不行就手动，任务管理器-->服务，选择MongoDB开启

##### 4、Linux下安装

~~~bash
|-opt # 软件目录
	|-mongo
		|-mongodb-linux-x86_64-rhel70-4.4.0.tgz # 下载的mongodb
		|-mongodb-linux-x86_64-rhel70-4.4.0 # 解压后的安装目录
		|-db # 存放数据的目录
		|-log # 存放日志的目录
# 创建目录
mkdir -p /opt/mongo /opt/mongo/db /opt/mongo/log
# 下载mongodb
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-4.4.0.tgz
# 解压
tar -zxvf mongodb-linux-x86_64-rhel70-4.4.0.tgz
# 配置环境
export PATH=$PATH:/opt/mongo/mongodb-linux-x86_64-rhel70-4.4.0/bin
# 运行
mongod --dbpath /opt/mongo/db --port 27017 --logpath /opt/mongo/log/mongodb.log --fork
~~~

##### 5、自启服务

/etc/systemd/system/mongodb.service

~~~c
[Unit]
Description=mongodb
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
#ExecStart=/usr/local/mongodb/bin/mongod --config /usr/local/mongodb/mongodb.conf
ExecStart=/opt/mongo/mongodb-linux-x86_64-rhel70-4.4.0/bin/mongod --dbpath=/opt/mongo/db --logpath=/opt/mongo/log/mongodb.log --logappend --fork
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/opt/mongo/mongodb-linux-x86_64-rhel70-4.4.0/bin/mongod --shutdown --dbpath=/opt/mongo/db --logpath=/opt/mongo/log/mongodb.log --logappend --fork
PrivateTmp=true

[Install]
WantedBy=multi-user.target
~~~

/opt/mongo/mongodb-linux-x86_64-rhel70-4.4.0/bin/mongodb.conf

~~~properties
#端口  
port=27017
#数据库存文件存放目录
dbpath=/opt/mongo/db
#日志文件存放路径  
logpath=/opt/mongo/log/mongodb.log
#使用追加的方式写日志  
logappend=true
# 设置为true，修改数据目录存储模式，每个数据库的文件存储在DBPATH指定目录的不同的文件夹中。
# 使用此选项，可以配置的MongoDB将数据存储在不同的磁盘设备上，以提高写入吞吐量或磁盘容量。默认为false。
# 建议一开始就配置此选项
directoryperdb=true

# 后台运行
#以守护程序的方式启用，即在后台运行  
fork=true
#最大同时连接数  
maxConns=100
#不启用验证
noauth=true
#每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
journal=true
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
#存储引擎有mmapv1、wiretiger、mongorocks
storageEngine=wiredTiger
#这样就可外部访问了，例如从win10中去连虚拟机中的MongoDB
bind_ip = 0.0.0.0
#bind_ip = 127.0.0.1
~~~

##### 6、连接MongoDB

~~~cmd
# admin登录
mongo 127.0.0.1:27017/admin
~~~

官网提供：可视化管理工具compass（2019年开始免费）

https://downloads.mongodb.com/compass/mongodb-compass-community-1.20.2-win32-x64.zip

##### 7、关闭MongoDB

~~~bash
# 命令关闭
mongod  --shutdown  --dbpath /opt/mongo/db
# 登录数据库关闭
db.shutdownServer();
~~~



#### 三、shell命令

##### 1、基本结构

|— foobar（Database）

​	|— persons（Collections）

​		|— {name:"liucong", age:20}（一行Document，JSON数据）

​		|...

​	|...

官方文档：https://docs.mongodb.com/manual/

##### 2、BSON（JSON扩展）

* null（空值）
* undefined（未定义类型）
* 布尔（true/false）
* 32/64位整数（自动转为64位浮点类型）
* 64位浮点数（数字类型）
* UTF-8（字符串类型）
* 数组（x:{gps:[20,56]}）
* 内嵌文档（x:{name:"liucong"}）
* 对象ID（默认ID对象{_id:ObjectId()}）
* 日期（x:{new Date()}）
* 正则（x:{/uspcat/i}）
* javaScript代码块（x:function(){...}）

##### 3、命名规范

- 不能是空字符串
- 不得含有空格、逗号、$、/、\、和\O（空字符）
- 应全部小写
- 最多64个字节
- 数据库不能与现有系统保留库同名，如admin、local，及config

注意：使用db-text命名数据库是合法的，但是会触发减法操作，所以使用时应该：

```shell
db.getCollection("db-text").help()
# 建议避免操作冲突，简洁操作
```

##### 4、基本操作

~~~shell
use [databseName] # 选择或创建数据库，空数据库存在缓存，没数据退出将会被删除

show dbs # 查看所有数据库
show collections # 查看所有文档

db.tropDatabase() # 删除数据库
db.[collectionName].trop() # 删除文档

db.help() # 查看数据库操作帮助
db.[collectionName].help() # 查看文档操作帮助
~~~

##### 5、添加文档数据

~~~shell
# 1、添加文档，冲突报错（过时）
db.[collectionName].insert({...})
# 例如：db.persons.insert({name:"liucong"})

# 推荐使用！
# 插入一条
db.[collectionName].insertOne({...})
# 插入多条
db.[collectionName].insertMany({...},{...},...)

# 2、冲突更新，否则添加
db.[collectionName].save({...})

# 3、固定集合,类似队列、LRU淘汰缓存策略
# 创建一个固定集合，大小100字节，存储文档10个
db.createCollection("mycoll",{size:100,capped:true,max:10})
# 普通集合转换为固定集合
db.runCommand({"convertToCapped":"persons",size:100000})
# 反向排序。默认是插入顺序排序
db.mycoll.find().sort({$natural:-1})

~~~

##### 6、查看文档数据

> 1、条件

| SQL              | MQL             |
| ---------------- | --------------- |
| a = 1            | {a:1}           |
| a <> 1（a != 1） | {a: {$ne : 1}}  |
| a > 1            | {a: {$gt : 1}}  |
| a >= 1           | {a: {$gte : 1}} |
| a < 1            | {a: {$lt : 1}}  |
| a <= 1           | {a: {$lte : 1}} |

> 2、逻辑

| SQL             | MQL                                    |
| --------------- | -------------------------------------- |
| a = 1 AND b = 1 | {a: 1, b: 1}或{$and: [{a: 1}, {b: 1}]} |
| a = 1 OR b = 1  | {$or: [{a: 1}, {b: 1}]}                |
| a IS NULL       | {a: {$exists: false}}                  |
| a IN (1, 2, 3)  | {a: {$in: [1, 2, 3]}}                  |

> 3、find

~~~shell
# 1、查看所有文档
db.[collectionName].find()
# 2、查看文档第一行数据
db.[collectionName].findOne()
# 3、投影（projection），类似sql中指定列查询
# 参数二，指定了name显示1，_id不显示0
db.[collectionName].find([查询器],{name:1,_id:0})
~~~

> 4、游标遍历

~~~shell
# 例如：查询书籍数量
var persons = db.persons.find({name:"liucong"})
while(persons.hasNext()){
  var obj = persons.next();
  print(obj.books.length);
}
# 注意：
# 1、游标只能遍历一次
# 2、游标迭代完毕销毁
# 3、客户端发来消息叫它销毁
# 4、默认游标超过10分钟销毁
~~~

> 5、子文档查询

~~~shell
# 1、通过点方式查询
# pserons->school:[
#	{school:"A",score:100},
#   {school:"B",score:80}]
# 例如：按学校为A的查询
db.persons.find({"school.school":"A"},{_id:0,school:1})

# 2、$elemMatch单条件查询
# 如果是点方式查询,会出现条件or的情况
# 例如：查询学校为A，并且成绩为100的人
db.persons.find({"school.school":"A","school.score":100})
# 正确查询，使用$elemMatch
db.persons.find({school:{$elemMatch:{school:"A",score:100}}})

# 3、万能$where(建议少使用，消耗性能)
# 查询年纪大于20、读过java书籍，在A上过学,且成绩为100的人
db.persons.find({"$where":function(){
  var books = this.books; # 所有书籍数组
  var school = this.school; # 所有学校数组
  if(this.age > 20){ # 年龄大于20
    for(var i=0;i<books.length;i++){
      if(books[i] == "java"){ # 读过java
        if(school){ # 学校数组不为空
          for(var j=0;j<school.length;j++){
            if(school[j].school == "A" and school[j].score == 100){ # 在A上过学,且成绩为100
              return true;
            }
          }
          break;
        }
      }
    }
  }
}})
~~~

> 6、常用方法

~~~shell
# 1、count求和
db.persons.find().count() # 文档数量

# 2、distinct去重
# 去重的集合persons，所有班级名称
db.runCommand({distinct:"persons",key:"classes"}).values

# 3、group分组
# 例如：根据班级分组，查询大于90分班级的名称、姓名、分数
db.runCommand({
  group:{
    ns:"persons", # 集合名
    key:{"classes":true}, # 分组的键对象
    initial:{score:0}, # 初始化累加器
    $reduce:function(doc,prev){ # 分解器，doc原生数据，prev累加器数据
    	if(doc.score > prev.score){
          prev.classes = doc.classes;
          prev.name = doc.name;
          prev.score = doc.score;
    	}
    },
    condition:{score:{$gt:90}}, # 条件
    finalize:function(prev){ # 组完成器
    	prev.score = prev.name+"math score："+prev.score
    }
  }
})
# 删除collection
db.runCommand({drop:"persons"})
# 查询runCommand提供的命令
db.listCommands()
# 查询服务器/主机操作系统信息
db.runCommand({buildinfo:1})
# 查询集合的详细信息
db.runCommand({collStats:"persons"})
# 查询操作集合的最后一次错误信息
db.runCommand({getLastError:"persons"})
~~~

> 7、根据索引查询

~~~shell
# 1、指定索引查询
db.persons.find({name:"liucong"}).hint({name:1})

# 2、查看查询时所用的索引
db.persons.find({name:"liucong"}).explain()
~~~

##### 7、更新文档数据

~~~shell
# 1、替换器模式
# 查询有，替换匹配的整个文档，冲突报错
# 查询没有，未修改
db.[collectionName].update([查询器],[替换器])

# 推荐使用！
# 修改单条
db.[collectionName].updateOne([查询器],[替换器])
# 修改多条
db.[collectionName].updateMany([查询器],[替换器])

# 2、修改器模式：{$set:[更新值]}
# 查询有，修改指定值，冲突报错
# 查询没有，未修改
db.[collectionName].update([查询器],[修改器])
# 例如：db.persons.update({name:"liucong"},{$set:{name:"LiuCong"}})
# 类似于SQL：update persons set name='Liucong' where name='liucong'

# 3、insertOrUpdate操作
# 替换器、修改器模式均可操作
# 第三个参数：true表示开启该操作
# 查询有，就更新，冲突报错
# 查询没有，就插入
db.[collectionName].update([查询器],[修改器],true)

# 4、批量更新操作
# 基于修改器模式，不支持替换式更新
# 第四个参数：true表示开启该操作
# 查询有，就更新，冲突报错
# 查询没有，未修改
db.[collectionName].update([查询器],[修改器],false,true)
~~~

##### 8、删除文档数据

~~~shell
# 已过时
db.persons.remove([查询器]) # 删除匹配的全部文档

# 推荐
db.[collectionName].deleteMany([查询器]) # 删除匹配的全部文档
db.[collectionName].deleteOne([查询器]) # 删除匹配的第一个文档
~~~

##### 9、变量存储

```shell
# 查询后的游标存储到p
var p = db.persons.findOne();
p
```

##### 10、函数存储

~~~shell
# 定义insert函数
function insert(object){
  db.persons.insert(object) # 将传入的object数据插入
}
insert({name:"liucong",age:20}) # 使用
~~~

执行javaScript字符串

~~~shell
db.eval("return 'MongoDB'")
# 结果:MongoDB
db.eval("function(name){return name}","name")
# 结果：name
~~~

##### 11、索引

> 1、索引

~~~shell
# 1:升序2:降序
db.[collectionName].createIndex({"字段",1},[options])
# options选项
# unique:boolean # 是否唯一，默认false
# name:string # 索引名
# sparse:boolean # 是否忽略不存在的字段
# expireAfterSeconds:integer # 生存时间秒单位
# background:boolean # 是否后台执行，默认false

db.[collectionName].getIndexes() # 查看索引
db.[collectionName].totalIndexSize() # 查看索引大小
db.[collectionName].dropIndexes() # 删除所有索引
db.[collectionName].dropIndex("索引名") # 删除指定索引
~~~

> 2、二维索引

~~~shell
# 准备数据
# var map = [
#	{"gis":{x:185,y:150}},
#	{"gis":{x:70,y:100}},
#	{"gis":{x:100,y:200}},
#	....
# ]
# 1、创建二维索引
db.map.createIndex({gis:"2d"},{min:-1,max:201})
# 2、离[70,180]最近的三个点（包括自己）
db.persons.find({gis:{$near:[70,180]}}).limit(3)
# 3、box查询。查询[50,50]和[190,190]为对角线的正方形中所有的点
db.map.find({gis:{$within:{$box:[[50,50],[190,190]]}}})
# 4、center查询。查询以圆心[56,80]半径为50规则下的圆形中所有的点
db.map.find({gis:{$within:{$center:[[56,80],50]}}})
~~~

##### 12、重要练习

> 1、操作符

~~~shell
>/$gt # 大于（Greater Than）
</$lt # 小于（Greater Than Equal）
>=/$gte # 大于等于（Less Than）
<=/$lte # 小于等于（Less Than Equal）
!=/$ne # 不等于（no Equal）
$in # 包含
$nin # 不包含
$or # 或
$not # 取反
$and # 数组与
$size # 数组大小
$slice # 按索引查找

# 例题
# 例如查询大于等于100分的人
db.persons.find({score:{$gte : 100})
# 查询年龄在[18,20]的人
db.persons.find({age:{$gte:18,$lte:20}})
# 查询年龄是18、20的人
db.persons.find({age:{$in:[18,20]}})
# 查询年龄大于18或者大于20的人
db.persons.find({$or:[{age:{$gt:18}},{age:{$gt:20}}]})
# 正则表达式,查询存在liu的人
db.persons.find({name:/liu/i})
# 查询书籍有js、java的人
db.persons.find({books:{$all:["js","java"]}})
# 查询第一本书是js的人
db.persons.find({"books.0":"js"})
# 查询书籍有3本的人
db.persons.find({"books":{$size:3}})

# 由于$size不能与其它比较符组合使用
# 所以手动添加一个size属性
# push一本书时，给size+1
db.persons.update({name:"liucong"},{$push:{books:"python"},$inc:{size:1}})
# 直接同操作size属性使用
# 查询书籍数量大于等于3的人
db.persons.update({size:{$gte:3}})

# 展示书籍[2,4)本，也就是1,2,3索引书籍
db.persons.find({name:"liucong"},{books:{$slice:[1,3]}})

# $type类型查看
# 查看score为string类型的人
db.persons.find({score:{$type:"string"}})

# 分页
# limit表示查询数量，skip表示跳过多少数量
# 假设：page=2，pageSize=5，先跳过，再查询
db.persons.find().limit(pageSize).skip((page-1)*pageSize)

# sort排序
# 1:升序，-1:降序
db.persons.find().sort({age:1})
~~~

> 2、聚合

~~~shell
# find高级查询，快照需要用到
# 什么用快照？
# 由于在游标遍历时，修改原有数据变大，超出预留空间
# 会为该数据从新分配一块空间，放到游标末尾
# 所以原有的位置发生改变了
$query # 默认可以不写
$orderby
$maxsan:integer # 最多扫描的文档数 
$hint # 索引
$explain:boolean # 统计
$snapshot:boolean # 一致快照
$sum # 求和
$avg # 求平均值
$min # 求最小值
$max # 求最大值

# update
$inc # 追加指定值
$unset # 删除键
$push # 追加元素给数组，不存在数组就创建，不是数组就报错
$pull # 根据元素删除
$pullAll # 根据元素批量删除
$pop # -1删除first元素，1删除last元素
$addToSet # 数组存在则不添加，否则添加
$ # 通配符，需要加""
$each # 遍历数组，例如与$addToSet结合使用完成批量更新操作

# 返回修改后的文档信息
ps = db.runCommand({
	"findAndModify":"persons",
	"query":{"name":"555"},
	"update":{"$set":{"age":"666"}},
	"new":true
}).value
~~~

##### 13、导出和备份

~~~shell
# 导出（cmd）
mongoexport -h localhost -d foobar -c persons -o E:/persons.json
# 导入（cmd）
mongoimport --db foobar --collection persons --file E:/persons.json

# 远程备份（cmd）
mongodump -h localhost:27017 -d foobar -o E:/foobar
# 远程恢复（cmd）
mongorestore -h localhost:27017 -d foobar E:/foobar/foobar
~~~

##### 14、Fsync锁/修复

~~~shell
# 为什么用锁？
# 读写操作都需要经过缓冲池，备份时有可能忽略了缓冲池里的数据
# 先上锁，写操作完后再解锁，这时备份才同步
# 注意：需要use admin
# 上锁
db.runCommand({fsync:1,lock:1})
# 解锁
db.fsyncUnlock()
db.currentOp()

# 为什么修复？
# 例如断电造成的垃圾数据（写没完全），恢复后垃圾依然存在
# 数据修复
db.repairDatabase
~~~

##### 15、权限管理

~~~shell
# 1、添加一个超级用户
db.createUser({
	user:"userName",
	pwd:"123456",
	roles:["root"]
})
# 2、roles权限设置
# 设置db01、db02数据库读写权限，其他只读权限
roles:[
  {role:"readWrite",db:"db01"},
  {role:"readWrite",db:"db02"},
  "read"
]
# 3、详细
https://segmentfault.com/a/1190000015603831

# 4、登录用户
db.auth("userName","123456")

# 5、修改密码
db.changeUserPassword("userName","xxx")
# 或修改密码及信息
db.runCommand({
  updateUser:"",
  pwd:"",
  customData:{...}
})

# 6、删除用户
db.dropUser("userName")
~~~

#### 四、GridFS文件系统

##### 1、概念

​	GridFS是mongoDB自带的文件系统，采用二进制形式存储文件，大型文件系统的绝大多特性是由GridFS完成的。

##### 2、使用

mongodbfiles.exe

~~~cmd
# 帮助(cmd)
mongofiles --help
# 上传文件到foobar集合(cmd)
mongofiles -d foobar -l "E:\a.txt" put "a.txt"
# 查看文件信息(shell)
show collections # 可以看到fs.chunks和fs.files
db.fs.chunks.find() # 对象信息
db.fs.files.find() # 文件信息
# 查看所有文件（cmd）
mongofiles -d foobar list
# 删除已经存在的（cmd）
mongofiles -d foobar delete "a.txt"
~~~

#### 五、副本集

实现集群的方式有三种，分别是主从复制、副本集、分片。

副本集也称复制集。

- 复制集的主要意义在于实现服务高可用，也就是发生故障时程序还能运行。

- 依赖于两个方面的功能：
  - 数据写入时将数据迅速复制到另一个独立的节点上。
  - 在接受写入节点发生故障时自动选举出一个新的替代节点
- 在实现高可用的同时，复制集实现了其他几个附加作用
  - 数据分发：将数据从一个区域复制到另一个区域，减少另一个区域的读延迟
  - 读写分离：不同类型的压力分别在不同的节点上执行
  - 异地容灾：在数据中心故障时候快速切换到异地
- 复制集由3个以上具有投票权的节点组成
  - 一个主节点（PRIMARY）：接受写入操作和选举时投票
  - 两个（或多个）从节点（SECONDARY）：复制主节点上的新数据和选举时投票
  - 不推荐使用Arbiter（投票节点）
- 如何复制？
  - 无论是插入、更新或删除，到达主节点时，它对数据的操作将被记录下来（经过一些必要的转换），这些记录称为oplog
  - 从节点通过在主节点上打开一个tailable游标不断获取新进入主节点的oplog，并在自己的数据上回放，以保持跟主节点的数据一致。
- 通过选举完成故障恢复
  - 具有投票权的节点之间两两互相发送心跳
  - 当五次心跳未收到时判断为节点失联
  - 如果失联的是主节点，从节点会发起选举，选出新的主节点
  - 如果失联的是从节点则不会产生新的选举
  - 选举基于RAFT一致性算法实现
  - 复制集中最多可以有50个节点，但具有投票权的节点最多7个。
  - 选举成功的必要条件是大多数投票节点存活
    - 能够与多数节点建立连接
    - 具有较新的oplog
    - 具有较高的优先级（如果有配置）
- 常用的配置选项
  - 是否具有投票权（v）：有则参与投票
  - 优先级（priority）：优先级越高的节点越优先成为主节点。优先为0的节点无法成为主节点
  - 隐藏（hidden）：复制数据，但对应用不可见。隐藏节点可以具有投票权，但优先级必须为0
  - 延迟（slaveDelay）：复制n秒之前的数据，保持与主节点的时间差。（可用于误操作恢复）
- 注意事项
  - 因为正常的复制集节点都有可能成为主节点，它们的地位是一样的，因此硬件配置上必须一致
  - 为了保证节点不会同时宕机，各节点使用的硬件必须具有独立性
  - 复制集各节点软件版本必须一致，以避免出现不可预知的问题
  - 增加节点不会增加系统写性能
  - 太多从节点会增加主节点的压力，大概是每增加一个节点会有10%左右的性能影响。

分片也就是分布式存储，将大数据分为多块分别存储在不同机器，随求高性能。

现版本主从复制已移除，而实际开发中通常两种技术结合使用，分片+副本集。

https://www.cnblogs.com/kevingrace/p/5685486.html

从服务器只能查看，不可以修改和删除。

~~~
# 1、一主（6666）一从（7777）
# 准备
1、配置文件（conf）
2、启动服务（bat）
3、启动shell（bat）
~~~

##### 1、配置文件

主（6666master.conf）

~~~txt
storage:
  dbPath: C:\web\mongodb2\6666
  journal:
    enabled: true

systemLog:
  destination: file
  logAppend: true
  path:  C:\web\mongodb2\6666\log\mongod.log

net:
  port: 6666
  bindIp: 127.0.0.1

replication:
  replSetName: test_1 # 主从复制名字一致。分库分表名字不同
~~~

从（7777slave.conf）

~~~txt
storage:
  dbPath: C:\web\mongodb2\7777
  journal:
    enabled: true

systemLog:
  destination: file
  logAppend: true
  path:  C:\web\mongodb2\7777\log\mongod.log

net:
  port: 7777
  bindIp: 127.0.0.1

replication:
  replSetName: test_1 # 主从复制名字一致。分库分表名字不同
~~~

##### 2、主服务启动

主（6666service.bat）

~~~cmd
C:/web/mongodb-plus-4.2.1/bin/mongod.exe --config C:/web/mongodb2/6666master.conf
~~~

从（7777service.bat）

~~~cmd
C:/web/mongodb-plus-4.2.1/bin/mongod.exe --config C:/web/mongodb2/7777slave.conf
~~~

##### 3、启动shell

主（6666shell.bat）

~~~cmd
C:/web/mongodb-plus-4.2.1/bin/mongo.exe 127.0.0.1:6666/admin
~~~

从（7777shell.bat）

~~~cmd
C:/web/mongodb-plus-4.2.1/bin/mongo.exe 127.0.0.1:7777/admin
~~~

##### 4、给主服务器，设置从服务器

~~~shell
# 1、登录主服务shell
# 2、开启主从复制
use admin
conf = {
	_id:"test_1",
	members:[
		{_id:0,host:"127.0.0.1:6666"},
		{_id:1,host:"127.0.0.1:7777"}
	]
}
rs.initiate(conf)

# 查看状态
rs.status()

# 添加节点
rs.add("127.0.0.1:8888")

# 删除节点
rs.remove("127.0.0.1:8888")

# 重新配置节点
rs.reconfig(conf);

# 切换节点
# 30秒不参与选举，随机选举任意从服务器为主服务器
rs.freeze(30)

# 从服务器确认,否则没有权限
rs.slaveOk()
~~~

#### 六、分片

##### 1、部署三台配置服务器（27100/27101/27102副本集）

从3.2版本开始，配置服务器必须有副本集

~~~txt
storage:
  dbPath: C:\web\mongodb3\config27100
  journal:
    enabled: true

systemLog:
  destination: file
  logAppend: true
  path:  C:\web\mongodb3\config27100\log\mongod.log

net:
  port: 27100
  bindIp: 127.0.0.1

replication:
  replSetName: config_rs # 主从复制名字一致。分库分表名字不同
  
sharding:
  clusterRole: configsvr # 配置服务
~~~

登录127.0.0.1:27100

~~~cmd
C:\web\mongodb-plus-4.2.1\bin\mongo.exe 127.0.0.1:27100/admin
~~~

开启副本集

~~~shell
use admin
conf = {
	_id:"config_rs",
	configsvr:true,
	members:[
		{_id:0,host:"127.0.0.1:27100"},
		{_id:1,host:"127.0.0.1:27101"},
		{_id:2,host:"127.0.0.1:27102"}
	]
}
rs.initiate(conf)
~~~



##### 2、部署两台分片服务器（27103/27105分担数据）

为每台分片服务器创建一个副本集（共4台）

shard_rs_1：127.0.0.1:27103/127.0.0.1:27104

shard_rs_2：127.0.0.1:27105/127.0.0.1:27106

~~~txt
storage:
  dbPath: C:\web\mongodb3\mongod27103
  journal:
    enabled: true

systemLog:
  destination: file
  logAppend: true
  path:  C:\web\mongodb3\mongod27103\log\mongod.log

net:
  port: 27103
  bindIp: 127.0.0.1

replication:
  replSetName: shard_rs_1 # 主从复制名字一致。分库分表名字不同
  
sharding:
  clusterRole: shardsvr # 分片服务
~~~

开启副本集

打开127.0.0.1:27103 shell

```shell
conf = {
	_id:"shard_rs_1",
	members:[
		{_id:0,host:"127.0.0.1:27103"},
		{_id:1,host:"127.0.0.1:27104"}
	]
}
rs.initiate(conf)
```

打开127.0.0.1:27105 shell

```shell
conf = {
	_id:"shard_rs_2",
	members:[
		{_id:0,host:"127.0.0.1:27105"},
		{_id:1,host:"127.0.0.1:27106"}
	]
}
rs.initiate(conf)
```



##### 3、部署路由服务器（27200路由分发）

创建route.conf配置文件

~~~txt
net:
  port: 27200
  bindIp: 127.0.0.1
sharding:
  configDB: config_rs/127.0.0.1:27100,127.0.0.1:27101,127.0.0.1:27102 # 配置服务器
~~~

启动路由服务

~~~cmd
C:\web\mongodb-plus-4.2.1\bin\mongos.exe --config C:\web\mongodb3\route.conf
~~~

打开路由shell

~~~cmd
C:\web\mongodb-plus-4.2.1\bin\mongo.exe 127.0.0.1:27200/admin
~~~

添加分片服务器

~~~shell
use admin
sh.addShard("shard_rs_1/127.0.0.1:27103,127.0.0.1:27104")
sh.addShard("shard_rs_2/127.0.0.1:27105,127.0.0.1:27106")
~~~

查看状态

~~~shell
sh.status()
~~~

测试分片

~~~~shell
# 准备数据
use foobar
function add(){ for(var i=0;i<800000;i++){db.persons.insert({_id:i,name:"name"+i}) }}
add()

分别查看shard_rs_1/shard_rs_2两台分片服务器的数据,注意哪台是主服务器
use foobar
db.persons.find().count()
~~~~

启动分片

~~~shell
# 启动分片，根据数据库分片
sh.enableSharding("foobar")

# 根据id分片集合
sh.shardCollection("foobar.persons",{_id:1})

# 根据索引分区
sh.enableSharding("foobar",{"id":1})

~~~

#### 七、模型设计

##### 1、设计元素

- 实体Entity
  - 描述业务的主要数据集合
  - 谁，什么，何时，何地，为何，如何
- 属性Attribute
  - 描述实体里面的单个信息
- 关系Relationship
  - 描述实体与实体之间的数据规则
  - 结构规则：1-N，N-1，N-N
  - 引用规则：电话号码不能单独存在

##### 2、传统模型设计

|            | 概念模型CDM                                        | 逻辑模型LDM                                      | 物理模型PDM                                                  |
| ---------- | -------------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------------ |
| 目的       | 描述业务系统要管理的对象                           | 基于概念模型，详细列出所有实体、实体的属性及关系 | 根据逻辑模型，结合数据库的物理结构，设计具体的表结构，字段列表及主外键 |
| 特点       | 用概念名词来描述现实中的实体及业务规则，如“联系人” | 基于业务的描述和数据库无关                       | 技术实现细节和具体的数据库类型相关                           |
| 主要使用者 | 用户需求分析师                                     | 需求分析师，架构师及开发者                       | 开发者DBA                                                    |

##### 3、文档模型设计特点

> 1、三个误区

- 不需要模型设计
- MongoDB应用一个超级大文档来组织所有数据
- MongoDB不支持关联或者事务

> 2、关于JSON文档模型设计

- 文档模型设计处于是物理模型设计阶段（PDM）
- JSON文档模型通过内嵌数组或引用字段来表示关系
- 文档模型设计不遵从第三范式，允许沉余

> 3、为什么都说MongoDB是无模式？

- 严格来说，MongoDB同样需要概念/逻辑建模

- 文档模型设计的物理结构可以和逻辑层类似
- MongoDB无模式由来
  - 可以省略物理建模的具体过程

> 4、设计原则

- 性能（Performance）
- 开发易用（Ease of Development）

> 5、关系模型VS文档模型

|              | 关系数据库                   | JSON文档模型       |
| ------------ | ---------------------------- | ------------------ |
| 模型设计层次 | 概念模型、逻辑模型、物理模型 | 概念模型、逻辑模型 |
| 模型实体     | 表                           | 集合               |
| 模型属性     | 列                           | 字段               |
| 模型关系     | 关联关系，主外键             | 内嵌数组，引用字段 |

##### 4、文档模型设计三步曲

- 业务需求及逻辑模型（逻辑导向）
  - 集合、字段、基础形状
- 技术需求读写比例、方式及数量（技术导向）
  - 工况细化
  - 引用及关联
- 经验和学习（模式导向）
  - 套用设计模式
  - 最终模式

> 第一步：建立基础文档模型

- 根据概念模型或者业务需求推导出逻辑模型（找对象）
- 列出实体之间的关系及基数（明确关系）
  - 1-1关系建模
    - 基本原则：以内嵌为主，作为子文档形式或者直接在顶级，不涉及到数据沉余
    - 例外情况：内嵌后导致文档大小超过16MB
  - 1-N关系建模
    - 基本原则：以内嵌为主，用数组来表示一对多，不涉及到数据沉余
    - 例外情况：文档大小超过16MD，数组长度太大（数万或更多），数组长度不确定
  - N-N关系建模
    - 基本原则：不需要映射表，一般用内嵌数组来表示一对多，通过沉余来实现N-N
    - 例外情况：文档大小超过16MD，数组长度太大（数万或更多），数组长度不确定
- 套用逻辑设计原则来决定内嵌方式（进行建模）
- 完成基础模型构建

> 第二步：根据读写工况细化

- 工况细化
  - 最频繁的数据查询模式
  - 最常用的查询参数
  - 最频繁的数据写入模式
  - 读写操作的比例
  - 数据量的大小
- 基于内嵌的文档模型，根据业务需求
  - 使用引用来避免性能瓶颈
  - 使用沉余来优化访问性能

- 什么时候使用引用？
  - 内嵌文档太大，数MB或者超过16MB
  - 内嵌文档或数组元素会频繁修改
  - 内嵌数组元素会持续增长并且没有封顶
- 引用设计的限制
  - MongoDB对使用引用的集合之间并无主外键检查
  - MongoDB使用聚合框架的$lookup来模仿关联查询
  - $lookup只支持left outer join
  - $lookup的关联目标（from）不能是分片表

> 第三步：套用设计模式

- 文档模型：无范式，无思维定式，充分发挥想象力

- 设计模式：实战过屡试不爽的设计技巧，快速应用

- 举例：一个IoT场景的分桶设计模式，可以帮助把存储空间降低10倍并且查询效率提升数十倍

- 案例：

  - | 场景                                 | 痛点                       | 设计模式的方案及优点                                         |
    | ------------------------------------ | -------------------------- | ------------------------------------------------------------ |
    | 时序数据：物联网、智慧城市、智慧交通 | 数据点采集频繁，数据量太多 | 利用文档内嵌数组，将一个时间段的数据聚合到一个文档里。大量减少文档数量。大量减少索引占用空间。 |

- 一个好的设计模式可以显著地：

  - 提升数据读写的效率
  - 降低资源的需求

- 更多的MongoDB设计模式

  - | 表现形式类 | 数据访问类 | 组织结构类 |
    | ---------- | ---------- | ---------- |
    | 列转行     | 子集       | 预聚合     |
    | 文档版本   | 近似处理   | 分桶       |

##### 5、问题及解决方案

> 问题一：大文档，很多字段，很多索引

解决方案：列转行

| 场景                                                         | 痛点                                                     | 设计模式方案及优点                   |
| ------------------------------------------------------------ | -------------------------------------------------------- | ------------------------------------ |
| 产品属性'color'，'size'，'dimensions'... 多语言（多国家）属性 | 文档中很多类似的字段。会用于组合查询搜索。需要键很多索引 | 转化为数组，一个索引解决所有查询问题 |

> 问题二：模型灵活了，如何管理文档不同版本？

解决方案：增加一个版本字段

| 场景                   | 痛点                                                       | 设计模式方案及优点                                           |
| ---------------------- | ---------------------------------------------------------- | ------------------------------------------------------------ |
| 任何有版本衍变的数据库 | 文档模型格式多，无法知道其合理性。升级时候需要更新太多文档 | 增加一个版本号字段。快速过滤掉不需要升级的文档。升级时候对不同版本的文档做不同的处理 |

> 问题三：统计网页点击流量

解决方案：用近似计算

| 场景                               | 痛点                     | 设计模式方案及优点                            |
| ---------------------------------- | ------------------------ | --------------------------------------------- |
| 网页计数。各种结果不需要准确的排名 | 写入太频繁，消耗系统资源 | 间隔写入，每隔10次或者100次。大量减少写入需求 |

> 问题四：业绩排名，游戏排名，商品统计等精确统计

解决方案：用预聚合字段

| 场景             | 痛点                     | 设计模式方案及优点                                     |
| ---------------- | ------------------------ | ------------------------------------------------------ |
| 准确排名。排行榜 | 统计计算耗时，计算时间长 | 模型中直接增加统计字段。每次更新数据时，同时更新统计值 |

#### 八、java操作Mongo

##### MongoClient

~~~xml
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongodb-driver</artifactId>
    <version>3.10.1</version>
</dependency>
~~~

~~~java
	// 客户端
    private MongoClient mongoClient;
    // 集合
    private MongoCollection<Document> comment;

    @Before
    public void init() {
        //1. 创建操作MongoDB的客户端
        mongoClient = new MongoClient("192.168.200.128");
        // MongoClient mongoClient = new MongoClient("192.168.200.128",27017);

        //2. 选择数据库 use commentdb
        MongoDatabase commentdb = mongoClient.getDatabase("commentdb");

        //3. 获取集合 db.comment
        comment = commentdb.getCollection("comment");
    }


    //查询所有数据db.comment.find()
    @Test
    public void test1() {
        //4. 使用集合进行查询，查询所有数据db.comment.find()
        FindIterable<Document> documents = comment.find();

        //5. 解析结果集（打印）
        // "_id" : "1", "content" : "到底为啥出错", "userid" : "1012", "thumbup" : 2020 }
        for (Document document : documents) {
            System.out.println("------------------------------");
            System.out.println("_id:" + document.get("_id"));
            System.out.println("content:" + document.get("content"));
            System.out.println("userid:" + document.get("userid"));
            System.out.println("thumbup:" + document.get("thumbup"));
        }

    }


    @After
    public void after() {
        // 释放资源,关闭客户端
        mongoClient.close();
    }


    // 根据条件_id查询数据，db.comment.find({"_id" : "1"})
    @Test
    public void test2() {
        // 封装查询条件
        BasicDBObject bson = new BasicDBObject("_id", "1");

        // 执行查询
        FindIterable<Document> documents = comment.find(bson);

        for (Document document : documents) {
            System.out.println("------------------------------");
            System.out.println("_id:" + document.get("_id"));
            System.out.println("content:" + document.get("content"));
            System.out.println("userid:" + document.get("userid"));
            System.out.println("thumbup:" + document.get("thumbup"));
        }
    }

    // 新增db.comment.insert({"_id" : "5", "content" : "坚持就是胜利123", "userid" : "1018", "thumbup" : 1212})
    @Test
    public void test3() {
        // 封装新增数据
        Map<String, Object> map = new HashMap<>();
        map.put("_id", "6");
        map.put("content", "新增测试");
        map.put("userid", "1019");
        map.put("thumbup", "666");

        // 封装新增文档对象
        Document document = new Document(map);

        // 执行新增
        comment.insertOne(document);
    }

    // 修改，db.comment.update({"_id" : "5"},{$set:{"userid" : "888"}})
    @Test
    public void test4() {
        //创建修改的条件
        BasicDBObject filter = new BasicDBObject("_id", "6");
        //创建修改的值
        BasicDBObject update = new BasicDBObject("$set", new Document("userid", "999"));

        // 执行修改
        comment.updateOne(filter, update);
    }

    // 删除，db.comment.remove({"_id" : "5"})
    @Test
    public void test5() {
        // 创建删除的条件
        BasicDBObject bson = new BasicDBObject("_id", "6");

        // 执行删除
        comment.deleteOne(bson);
    }
~~~

##### SpringDataMongoDB

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
~~~

~~~yml
spring:
  data:
    mongodb:
      database: commentdb
      host: 39.108.208.193
      port: 2717
~~~

~~~java
// 泛型<实体类型，id类型>
public interface CommentRepository extends MongoRepository<Comment, String> {
}
~~~



#### 九、Mongo优化

- 文档中的“\_id”键推荐使用默认值，禁止向_id中保存自定义的值。
  - 默认“_id”是个ObjectID对象（标识符中包含时间戳、机器ID、进程ID和计数器）
  - MongoDB在指定_id与不指定_id插入时 速度相差很大，指定_id会减慢插入的速率。

- 推荐使用短字段名。
  - MongoDB集合中的每一个文档都需要存储字段名，长字段名会需要更多的存储空间。

- 适当创建索引。
  - MongoDB索引可以提高文档的查询、更新、删除、排序操作，所以结合业务需求，适当创建索引。
  - 每个索引都会占用一些空间，并且导致插入操作的资源消耗，因此，建议每个集合的索引数尽量控制在5个以内。

- 对于包含多个键的查询，创建包含这些键的复合索引是个不错的解决方案。

  - 复合索引的键值顺序很重要，理解索引最左前缀原则。

  - 例如：在test集合上创建组合索引{a:1,b:1,c:1}。执行以下7个查询语句：

  - ~~~shell
    db.test.insert({a:"hello",b:"sogo",c:"666"})
    
    db.test.find({a:"hello"}) // 1
    db.test.find({b:"sogo", a:"hello"}) // 2
    db.test.find({a:"hello",b:"sogo", c:"666"}) // 3
    db.test.find({c:"666", a:"hello"}) // 4
    db.test.find({b:"sogo", c:"666"}) // 5
    db.test.find({b:"sogo"}) // 6
    db.test.find({c:"666"}) // 7
    ~~~

  - 以上查询语句可能走索引的是1、2、3、4，查询应包含最左索引字段，以索引创建顺序为准，与查询字段顺序无关。最少索引覆盖最多查询。

- TTL 索引（time-to-live index，具有生命周期的索引），使用TTL索引可以将超时时间的文档老化，一个文档到达老化的程度之后就会被删除。
  - 创建TTL的索引必须是日期类型。
  - TTL索引是一种单字段索引，不能是复合索引。
  - TTL删除文档后台线程每60s移除失效文档。
  - 不支持定长集合。

- 需要在集合中某字段创建索引，但集合中大量的文档不包含此键值时，建议创建稀疏索引。
  - 索引默认是密集型的，这意味着，即使文档的索引字段缺失，在索引中也存在着一个对应关系。在稀疏索引中，只有包含了索引键值的文档才会出现。

- 创建文本索引时字段指定text，而不是1或者-1。每个集合只有一个文本索引，但是它可以为任意多个字段建立索引。
  - 文本搜索速度快很多，推荐使用文本索引替代对集合文档的多字段的低效查询。

- 查询
  - 使用findOne在数据库中查询匹配多个项目，它就会在自然排序文件集合中返回第一个项目。
  - 如果需要返回多个文档，则使用find方法。

- 如果查询无需返回整个文档或只是用来判断键值是否存在，可以通过投影（映射）来限制返回字段，减少网络流量和客户端的内存使用。
  - 既可以通过设置{key:1}来显式指定返回的字段，也可以设置{key:0}指定需要排除的字段。
- 除了前缀样式查询，正则表达式查询不能使用索引，执行的时间比大多数选择器更长，应节制性地使用它们。

- 批量插入（batchInsert）可以减少数据向服务器的提交次数，提高性能。但是批量提交的BSON Size不超过48MB。

- 限制结果集中的数据量。MongoDB目前只支持对32M以内的结果集进行排序。

