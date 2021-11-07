## 搜索引擎

### 一、全文检索

#### 1、数据的分类

- 结构化数据
  - 格式固定、长度固定、数据类型固定。
  - 例如：数据库中的数据。
- 非结构化数据
  - word文档、pdf文档、邮件、html。
  - 格式不固定、长度不固定、数据类型不固定。

#### 2、数据的查询

- 结构化数据的查询
  - SQL语句，查询结构化数据的方法。简单、速度快。
- 非结构化数据的查询
  - 从多个文本文件中找出包含某个单词的文件。
  - 顺序扫描
    - 把文档读取到内存中，然后匹配字符串。
  - 把非结构化数据变成结构化数据（全文检索）
    - 英文：根据空格分词，得到一个单词列表，基于单词列表创建一个索引。
      - 查询索引，根据单词和文档的对应关系找到文档列表。
    - 中文：根据单词库分词。

#### 3、全文检索

先创建索引，然后查询索引的过程叫做全文检索。

索引一次创建可以多次使用，每次查询速度都很快。

索引：一个为了提高查询速度，创建某种数据结构的集合。

#### 4、应用场景

- 索引引擎：Google、百度、搜狗、360搜索等。
- 站内搜索：论坛搜索、微博、文字搜索等。
- 电商搜索：淘宝搜索、京东搜索。

### 二、Lucene

Lucene是一个基于Java开发的全文检索工具包。通俗点说就是提供一套全文检索的API给我们使用。

#### 1、创建索引

##### 获取文档

- 原始文档：基于数据进行搜索。
- 搜索引擎：爬虫获取原始文档。
- 站内搜索：数据库中的数据。
- 例如：通过IO流读取文档数据。

##### 构建文档对象

- 每个原始文档会创建一个Document对象，且有一个唯一的ID。
- 每个Document对象中包含多个域（Field）。
- 域中保存的就是原始文档的数据（Key-Value）。

##### 分析文档

- 就是分词的过程。
- 根据空格进行字符串拆分，得到一个单词列表。
- 单词统一转小写。
- 去除标点符号。
- 去除停用词。
  - 无意义的词。
- 每个关键词都封装成一个Term对象。
  - 关键词所在的域。
  - 关键词本身。
- 关键词是从域中拆分的。
  - 不同域，拆分出相同的关键词，属于不同的Term。

##### 创建索引

- 基于关键词列表创建一个索引，保存到索引库中。
- 索引库
  - 索引。
  - Document对象。
  - 关键词和文档的对应关系。
- 通过词语找文档，这种索引结构叫**倒排索引**结构。

#### 2、查询索引

- 用户查询接口
  - 用户输入查询条件的地方。
  - 例如：百度的搜索框。
- 把关键词封装成一个查询对象。
  - 要查询的域。
  - 要搜索的关键词。
- 执行查询。
  - 根据要查询的关键词到对应的域上进行搜索。
  - 找到关键词，根据关键词找到对应的文档。
- 渲染结果。
  - 根据文档Id找到文档对象。
  - 对关键词进行高亮显示。
  - 分页处理。
  - 最终展示给用户看。

#### 3、入门程序

##### 环境

- 下载lucene。

- java1.8及以上版本。

##### 创建索引

~~~java
@Test
public void createIndex() throws Exception {
    // 1、创建一个Director对象，指定索引库保存的位置。
    // 将索引库保存到内存中
    // Directory directory = new RAMDirectory();
    // 将索引库保存到磁盘中
    Directory directory = FSDirectory.open(new File("C:\\web\\index").toPath());
    // 2、基于Directory对象创建一个IndexWriter对象
    // 默认采用StandardAnalyzer分词器
    // IndexWriter indexWriter = new IndexWriter(directory, new IndexWriterConfig());
    // 使用IKAnalyzer分词器
    IndexWriterConfig config = new IndexWriterConfig(new IKAnalyzer());
    IndexWriter indexWriter = new IndexWriter(directory, config);
    // 3、读取磁盘上的文件，对应每个文件创建一个文档对象。
    File dir = new File("E:\\web_video\\12-lucene\\02.参考资料\\searchsource");
    File[] files = dir.listFiles();
    for (File file: files) {
        String fileName = file.getName(); // 文件名
        String filePath = file.getPath(); // 文件路径
        String fileContent = FileUtils.readFileToString(file, "UTF-8"); // 文件内容
        long fileSize = FileUtils.sizeOf(file); // 文件大小
        // 创建域，域的名称，域的值，是否存储
        Field fieldName = new TextField("name", fileName, Field.Store.YES);
        Field fieldPath = new TextField("path", filePath, Field.Store.YES);
        Field fieldContent = new TextField("content", fileContent, Field.Store.YES);
        Field fieldSize = new TextField("size", fileSize + "", Field.Store.YES);
        // 创建文档对象
        Document document = new Document();
        // 4、向文档对象中添加域
        document.add(fieldName);
        document.add(fieldPath);
        document.add(fieldContent);
        document.add(fieldSize);
        // 5、把文档对象写入索引库
        indexWriter.addDocument(document);
    }
    // 6、关闭indexwriter对象
    indexWriter.close();
}
~~~

- 可以使用Luke查看索引库中的内容。

##### 查询索引库

~~~java
@Test
public void searchIndex() throws Exception {
    // 1、创建一个Director对象，指定索引库的位置
    Directory directory = FSDirectory.open(new File("C:\\web\\index").toPath());
    // 2、创建一个IndexReader对象
    IndexReader indexReader = DirectoryReader.open(directory);
    // 3、创建一个IndexSearcher对象，构造方法中的参数indexReader对象。
    IndexSearcher indexSearcher = new IndexSearcher(indexReader);
    // 4、创建一个Query对象，TermQuery
    Query query = new TermQuery(new Term("content", "spring"));
    // 5、执行查询，得到一个TopDocs对象
    TopDocs topDocs = indexSearcher.search(query, 10);
    // 6、取查询结果的总记录数
    System.out.println("查询总记录数：" + topDocs.totalHits);
    // 7、取文档列表
    ScoreDoc[] scoreDocs = topDocs.scoreDocs;
    // 8、打印文档中的内容
    for (ScoreDoc doc: scoreDocs) {
        // 取文档Id
        int docId = doc.doc;
        Document document = indexSearcher.doc(docId);
        System.out.println(document.get("name"));
        System.out.println(document.get("path"));
        //System.out.println(document.get("content"));
        System.out.println(document.get("size"));
        System.out.println("--------------------");
    }
    // 9、关闭IndexReader对象
    indexReader.close();
}
~~~

##### 分析器

~~~java
@Test
public void testTokenStream() throws Exception {
    // 1、创建一个Analyzer对象，StandardAnalyzer对象
    Analyzer analyzer = new StandardAnalyzer();
    // 2、使用分析器对象的tokenStream方法获得一个TokenStream对象
    TokenStream tokenStream = analyzer.tokenStream("", "The Spring Framework provides a comprehensive programming and configuration model.");
    // 3、向TokenStream对象中设置一个引用，相当于数一个指针
    CharTermAttribute charTermAttribute = tokenStream.addAttribute(CharTermAttribute.class);
    // 4、调用TokenStream对象的reset方法。如果不调用抛异常
    tokenStream.reset();
    // 5、使用while循环遍历TokenStream对象
    while(tokenStream.incrementToken()) {
        System.out.println(charTermAttribute.toString());
    }
    // 6、关闭TokenStream对象
    tokenStream.close();
}
~~~

- 默认使用标准分析器StandardAnalyzer
- IKAnalyzer分词器
  - `Analyzer analyzer = new IKAnalyzer();`
- 注意：扩展词典严禁使用windows记事本编辑保证扩展词典的编码格式是utf-8
- 扩展词典：添加一些新词。
- 停用词词典：无意义的词或者是敏感词汇。

#### 4、维护索引

##### 添加文档

| Field       | 数据类型     | 是否分析 | 是否索引 | 是否存储 | 说明                             |
| ----------- | ------------ | -------- | -------- | -------- | -------------------------------- |
| StringField | 字符串       | N        | Y        | Y或N     | 索引不分析，比如订单号、姓名。   |
| LongPoint   | Long类型     | Y        | Y        | N        | 不存储，单纯索引。               |
| StoredField | 支持多种类型 | N        | N        | Y        | 只存储。                         |
| TextField   | 字符串或流   | Y        | Y        | Y或N     | 如果是一个流，会采用Unstored策略 |

~~~java
@Before
public void init() throws Exception {
    // 创建一个IndexWriter对象，需要使用IKAnalyzer分析器
    indexWriter = new IndexWriter(
        FSDirectory.open(new File("C:\\web\\index").toPath()),
        new IndexWriterConfig(new IKAnalyzer()));
}
~~~

~~~java
@Test
public void addDocument() throws Exception {
    // 创建一个Document对象
    Document document = new Document();
    // 往document对象中添加域
    document.add(new TextField("name", "新添加的文件", Field.Store.YES));
    document.add(new TextField("content", "新添加的文件内容", Field.Store.NO));
    document.add(new StoredField("path", "C:/web/hello"));
    // 将文档写入索引库
    indexWriter.addDocument(document);
    // 关闭索引库
    indexWriter.close();
}
~~~

##### 删除文档

~~~java
@Test
public void deleteAllDocument() throws Exception {
    // 删除所有文档
    indexWriter.deleteAll();
    // 关闭索引库
    indexWriter.close();
}
~~~

~~~java
@Test
public void deleteDocumentByQuery() throws Exception {
    indexWriter.deleteDocuments(new Term("name", "apache"));
    indexWriter.close();
}
~~~

##### 更新文档

~~~java
@Test
public void updateDocument() throws Exception {
    // 创建一个新的文档对象
    Document document = new Document();
    // 向文档对象中添加域
    document.add(new TextField("name", "更新之后的文档", Field.Store.YES));
    document.add(new TextField("name1", "更新之后的文档2", Field.Store.YES));
    document.add(new TextField("name2", "更新之后的文档3", Field.Store.YES));
    // 更新操作,实际上是先删除后添加
    indexWriter.updateDocument(new Term("name", "spring"), document);
    // 关闭索引库
    indexWriter.close();
}
~~~

#### 5、索引查询

~~~java
private IndexReader indexReader;
private IndexSearcher indexSearcher;

@Before
public void init() throws Exception {
    indexReader = DirectoryReader.open(
        FSDirectory.open(new File("C:\\web\\index").toPath()));
    indexSearcher = new IndexSearcher(indexReader);
}
~~~

##### Query查询

- 根据关键字查询
  - `TermQuery` # 指定域和关键词
- 范围查询
  - `RangeQuery` # 指定域和范围值

~~~java
@Test
public void testRangeQuery() throws Exception {
    // 创建一个Query对象
    Query query = LongPoint.newRangeQuery("size", 0L, 100L);
    // 执行查询
    printResult(query);
}
~~~

```java
private void printResult(Query query) throws Exception {
    TopDocs topDocs = indexSearcher.search(query, 10);
    System.out.println("总记录数" + topDocs.totalHits);
    ScoreDoc[] scoreDocs = topDocs.scoreDocs;
    for (ScoreDoc doc : scoreDocs) {
        // 取文档Id
        int docId = doc.doc;
        Document document = indexSearcher.doc(docId);
        System.out.println(document.get("name"));
        System.out.println(document.get("path"));
        //System.out.println(document.get("content"));
        System.out.println(document.get("size"));
        System.out.println("--------------------");
    }
    indexReader.close();
}
```

##### QueryParser查询

- 添加一个jar包：`lucene-queryparser-7.4.0.jar`
- 先对内容分词，然后基于分词后的结果进行查询。

~~~java
@Test
public void testQueryParser() throws Exception {
    // 创建一个QueryParser对象
    // 参数1：默认搜索域，参数2：分析器对象
    QueryParser queryParser = new QueryParser("name", new IKAnalyzer());
    // 使用QueryParser对象创建一个Query对象
    Query query = queryParser.parse("lucene是一个Java开发的全文检索工具包");
    // 执行查询
    printResult(query);
}
~~~



### 三、ElasticSearch

#### 1、介绍

ElasticSearch简称ES，是一个开源的分布式全文检索引擎，它可以近乎实时的存储、检索数据，分布式集群可处理PB级数据。ES基于Lucene作为其核心来实现所有索引和搜索的功能，旨在通过简单的RESTFul API来隐藏Lucene的复杂性，使全文搜索变得简单。

##### 安装

- 下载解压即可。

- 双击bin目录下的elasticsearch.bat文件运行。

- 修改配置文件config/elasticsearch.yml
  - network.host指定ip，默认本机地址。
  - http.port指定端口，默认端口9200。

#### 2、可视化界面

elasticsearch-head的安装依赖于node.js

~~~bash
# 将grunt-cli安装为全局命令
npm install -g grunt-cli

# cd到elasticsearch-head目录，安装依赖包
npm install
# 启动服务，默认localhost:9100
grunt server
~~~

启动不成功？

在ElasticSearch安装目录config/elasticsearch.yml文件中添加允许跨域：

~~~
http.cors.enabled: true
http.cors.allow.orign: "*"
~~~

#### 3、核心概念

##### 索引（Index）

- 相当于数据库。

- 索引名必须全小写。
- 通过索引获取所有文档。

##### 类型（Type）

- 相当于表结构。
- 为每个字段定义一个类型。

##### 字段（Field）

- 相当于列字段。
- 根据字段名获取对应的值。

##### 映射（Mapping）

- 处理数据的方式及规则。
- 如：字段数据的类型、默认值、分析器、是否被索引等。

##### 文档（Document）

- 文档实际上就是一个JSON格式的数据。
- 一个索引对应一个文档。
- 在物理存储上，文档存储在索引中。

##### 接近实时NRT

- 接近实时的搜索平台，通常是1秒以内。

##### 集群（Cluster）

- 一个集群是由一个或多个节点组成，它们共同持有整个的数据，并一起提供索引和搜索功能。
- 一个集群有一个唯一标识。

##### 节点（Node）

- 一个节点就是一个服务器，它存储数据，参与集群的索引和搜索功能。
- 一个节点有一个唯一标识。

##### 分片和复制（Shard & Replicas）

- 一个节点无法存储超过自身容量的数据，这时就需要分片。
  - 水平分割扩展容量，分布式存储。
  - 分布式并行操作，提高性能、吞吐量。
- 一个节点有可能出现宕机等意外情况，导致服务不可用，这时就需要复制。
  - 故障转移，提高可用性。

#### 4、RESTAPI

##### 创建索引和映射

~~~json
PUT http://127.0.0.1:9200/blog
{
	"mappings": {
		"article": {
			"properties": {
				"id": {
                    "type": "long"
                    "store": true
                },
                "title": {
                    "type": "text",
                    "store": true,
                    "index": true,
                    "analyzer": "standard"
                },
                "content": {
                    "type": "text",
                    "store": true,
                    "index": true,
                    "analyzer": "standard"
                }
			}
		}
	}
}
~~~

- blog：索引名
- mappings：有多个表的映射。
- article：表名Type
- properties：字段
- id：属性
- type：类型
- store：是否存储
- index：是否索引
- analyzer：分析器， “standard”为标准分析器，只分析英文

##### 设置Mapping

可以先创建索引，后设置Mappings

~~~json
POST http://127.0.0.1:9200/blog/hello/_mappings
{
	"hello": {
        "properties": {
            "id": {
                "type": "long",
                "store": true
            },
            "title": {
                "type": "text",
                "store": true,
                "index": true,
                "analyzer": "standard"
            },
            "content": {
                "type": "text",
                "store": true,
                "index": true,
                "analyzer": "standard"
            }
        }
    }
}
~~~

##### 删除索引

~~~json
DELETE http://127.0.0.1:9200/blog
~~~

##### 创建/修改文档

~~~json
POST http://127.0.0.1:9200/blog/hello/1
{
	"id": 1,
	"title": "文档1",
	"content": "新添加的文档1"
}
~~~

如果不指定文档id，则会自动生成一个_id

##### 删除文档

~~~json
DELETE http://127.0.0.1:9200/blog/hello/1
~~~

##### 查询文档

~~~json
# 根据文档ID查找文档
GET http://127.0.0.1:9200/blog/hello/1
~~~

~~~json
# 根据字段关键词查找文档
POST http://127.0.0.1:9200/blog/hello/_search
{
    "query": {
        "term": {
            "title": "1"
        }
    }
}
~~~

~~~json
# 根据搜索词查找文档
POST http://127.0.0.1:9200/blog/hello/_search
{
    "query": {
        "query_string": {
            "default_field": "title",
            "query": "1 2 3"
        }
    }
}
~~~

##### 分词效果测试

~~~json
GET http://127.0.0.1:9200/_analyzer
{
    "analyzer": "ik_smart",
	"text": "我是程序员"
}
~~~

- 标准分析器："standard"
- IK分析器：
  - 最小切分："ik_smart"
  - 最细粒度划分："ik_max_word"

#### 5、IK分析器

较好的中文分析器

- 安装位置：解压到elasticsearch安装位置的plugins目录下即可。
- 自定义分词库：
  - config目录下新建xx.dic文件。
  - 编辑添加单词，每行一个。
  - 编辑IKAnalyzer.cfg.xml文件，扩展配置自己的分词库。
    - `<entry key="ext_dict">my.dic</entry>`
- 分词算法：创建索引时指定analyzer值为
  - ik_smart：最小切分
  - ik_max_word：最细粒度划分。

#### 6、Kibana

专为ElasticSearch量身打造的开发工具。

- 下载解压即可。
- 双击bin目录下的kibana.bat文件运行。
- 修改配置文件config/kibana.yml
  - elasticsearch.url指定要连接elasticsearch服务器地址，默认http://localhost:5601。

其实记笔记也是分离关注点，我们只需要关注理解和实践，无需刻意记忆，将记忆的事交给笔记，这样就可以提高我们学习的效率，哪天不记得了，观看笔记就能很快联想起当时学习成果，由于更专注于理解和实践，联想起来也会更加深刻。虽然我们的记忆不是鱼的七秒记忆，但也不是永久记忆，一但长久不去回想就有可能忘记。记笔记除了简单的记忆以外还可以让我们将学到的知识更加的结构化，更加书面化的语言展示，结构化的知识更容易理解，更容易展开深入学习。

#### 7、集群

ES集群是一个P2P类型（使用gossip协议）的分布式系统。

- 集群状态管理。
- 节点通信
  - 2.0之前：cluster.name的节点自动归属到一个集群中。
  - 2.0之后：处于安全考虑改为了单播（unicast）方式，各节点配置相同的几个节点作为router。
- 大于等于两个节点是集群，一般节点数量是3个及以上。
- 集群Cluster
  - 默认集群名称：elasticsearch
- 节点Node
- 分片和复制Shard&Replicas
  - 分片：分布式存储和计算，目的是处理大数据。
  - 复制：备份节点，目的是高可用、故障隔离。

##### 准备

- 复制elasticsearch三份
  - 127.0.0.1:9201
  - 127.0.0.1:9202
  - 127.0.0.1:9203

- 删除data目录
- 删除logs下的所有日志

##### 节点配置

不能有中文注解

~~~
cluster.name: my-application # 集群名称，唯一
node.name: node-1 # 节点名称
network.host: 127.0.0.1 # 节点ip
http.port: 9201 # 节点端口

# 集群通信端口号
transport.tcp.port: 9301
# 集群自动发现的机器地址
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9301", "127.0.0.1:9302", "127.0.0.1:9303"]
~~~

#### 8、TransportClient

##### 导入依赖

~~~xml
<dependencies>
    <dependency>
        <groupId>org.elasticsearch</groupId>
        <artifactId>elasticsearch</artifactId>
        <version>5.6.8</version>
    </dependency>
    <dependency>
        <groupId>org.elasticsearch.client</groupId>
        <artifactId>transport</artifactId>
        <version>5.6.8</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-to-slf4j</artifactId>
        <version>2.9.1</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.24</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.21</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.12</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.11.2</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-annotations</artifactId>
        <version>2.11.2</version>
    </dependency>
</dependencies>
~~~

##### 创建客户端连接

~~~java
private TransportClient client;

/**
 * 初始化，创建客户端连接
 * @throws Exception
 */
@Before
public void init() throws Exception {
    // 1、创建一个Settings对象，相当于是一个配置信息。主要配置集群的名称。
    Settings settings = Settings.builder()
        .put("cluster.name", "my-application")
        .build();
    // 2、创建一个客户端Client对象
    client = new PreBuiltTransportClient(settings)
        .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9301))
        .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9302))
        .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9303));

}
~~~

##### 创建索引

~~~java
@Test
public void createIndex() throws Exception {
    // 3、使用client对象创建一个索引库
    client.admin().indices().prepareCreate("index_hello")
        // 执行方法
        .get();
    // 4、关闭client对象
    client.close();
}
~~~

##### 设置Mappings

~~~java
	@Test
    public void setMappings() throws Exception {
        // 3、创建一个Mapping信息
        XContentBuilder builder = XContentFactory.jsonBuilder()
                .startObject()
                    .startObject("article")
                        .startObject("properties")
                            .startObject("id")
                                .field("type", "long")
                                .field("store", true)
                            .endObject()
                            .startObject("title")
                                .field("type", "text")
                                .field("store", true)
                                .field("analyzer", "ik_smart")
                            .endObject()
                            .startObject("context")
                                .field("type", "text")
                                .field("store", true)
                                .field("analyzer", "ik_smart")
                            .endObject()
                        .endObject()
                    .endObject()
                .endObject();
        // 4、执行
        client.admin().indices()
                .preparePutMapping("index_hello")
                .setType("article")
                .setSource(builder)
                .get();
        // 5、关闭连接
        client.close();
    }
~~~

##### 使用XContent添加文档

~~~java
@Test
public void testAddDocument() throws Exception {
    // 准备数据
    XContentBuilder builder = XContentFactory.jsonBuilder()
        .startObject()
        	.field("id", 2L)
            .field("title", "程序员hello world2")
            .field("content", "这个一个hello world 程序2")
        .endObject();
    // 发送请求
    client.prepareIndex()
        .setIndex("index_hello")
        .setType("article")
        .setId("2")
        .setSource(builder)
        .get();
    // 关闭客户端
    client.close();
}
~~~

##### 使用Json字符串添加文档

~~~java
@Test
public void testAddDocument2() throws Exception {
    // 创建Article对象
    Article article = new Article();
    article.setId(3L);
    article.setTitle("程序员hello world3");
    article.setContent("这个一个hello world 程序3");
    // 使用jackson将对象转换成json
    ObjectMapper objectMapper = new ObjectMapper();
    String jsonDocument = objectMapper.writeValueAsString(article);
    // 使用client对象将文档写入索引库
    client.prepareIndex("index_hello", "article", "3")
        .setSource(jsonDocument, XContentType.JSON)
        .get();
    // 关闭连接
    client.close();
}
~~~

##### 根据Id查询文档

~~~java
@Test
public void testSearchById() throws Exception {
    // 创建一个查询对象
    QueryBuilder queryBuilder = QueryBuilders.idsQuery().addIds("1", "2");
    // 执行查询
    search(queryBuilder);
}
~~~

##### 根据关键字查询文档

~~~java
@Test
public void testQueryTerm() throws Exception {
    // 创建QueryBuilder对象
    // 1：搜索字段，2：关键字
    TermQueryBuilder queryBuilder = QueryBuilders.termQuery("title", "程序员");
    // 执行查询
    search(queryBuilder);
}
~~~

##### 根据搜索字符串查询文档

~~~java
@Test
public void testQueryStringQuery() throws Exception {
    // 创建QueryBuilder对象
    QueryStringQueryBuilder queryBuilder = QueryBuilders.queryStringQuery("内容内容")
        .defaultField("content");
    // 执行查询
    search(queryBuilder, "content");
}
~~~

##### 执行查询

- 高亮显示
  - HighlightBuilder对象
    - `field(highlightField)` # 设置高亮显示的字段
    - `preTags("<em>")` # 设置字段前缀
    - `postTags("</em>")` # 设置字段后缀
  - `highligter(highlightBuilder)` # 设置高亮
  - `Map searchHit.getHighlightFields()` # 获取所有高亮字段
    - `HighlightField highlightFieldMap.get(highlightField);` # 根据字段名获取高亮字段
    - `Text[] field.getFragments();` # 获取分段高亮
    - `fragments[0].toString();` # 取第一个
  - 
- 分页
  - `setFrom(0)` # 起始索引
  - `setSize(5)` # 分页大小

~~~java
private void search(QueryBuilder queryBuilder, String highlightField) {
    HighlightBuilder highlightBuilder = new HighlightBuilder();
    highlightBuilder.field(highlightField);
    highlightBuilder.preTags("<em>");
    highlightBuilder.postTags("</em>");

    SearchResponse searchResponse = client.prepareSearch("index_hello")
        .setTypes("article")
        .setQuery(queryBuilder)
        .setFrom(0)
        .setSize(5)
        .highlighter(highlightBuilder)
        .get();
    // 获取结果列表
    SearchHits searchHits = searchResponse.getHits();

    System.out.println("查询总记录数：" + searchHits.getTotalHits());
    Iterator<SearchHit> iterator = searchHits.iterator();
    while(iterator.hasNext()) {
        SearchHit searchHit = iterator.next();
        // 打印文档对象，json格式
        System.out.println(searchHit.getSourceAsString());
        Map<String, Object> document = searchHit.getSource();
        System.out.println("id：" + document.get("id"));
        System.out.println("title：" + document.get("title"));
        System.out.println("content：" + document.get("content"));
        System.out.println("----------------高亮显示------------");
        Map<String, HighlightField> highlightFieldMap = searchHit.getHighlightFields();
        System.out.println(highlightFieldMap);
        HighlightField field = highlightFieldMap.get(highlightField);
        Text[] fragments = field.getFragments();
        if (fragments != null) {
            String title = fragments[0].toString();
            System.out.println(title);
        }
        System.out.println("-----------------------------------");
    }
    // 关闭连接
    client.close();
}
~~~

#### 9、高级客户端

在 Elasticsearch 7.0 中不建议使用TransportClient，并且在8.0中会完全删除TransportClient。因此，官方更建议我们用Java High Level REST Client，它执行HTTP请求，而不是序列号的Java请求。

##### 引入依赖

~~~xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>6.5.3</version>
</dependency>
~~~

##### 连接Rest接口

~~~java
private RestHighLevelClient restHighLevelClient;
@Before
public void init() {
    // 1、连接Rest接口
	RestClientBuilder restClientBuilder = RestClient.builder(
    				new HttpHost("127.0.0.1", 9201, "http"),
                    new HttpHost("127.0.0.1", 9202, "http"),
                    new HttpHost("127.0.0.1", 9203, "http"));
	restHighLevelClient = new RestHighLevelClient(restClientBuilder);
}

~~~

##### 添加一条文档记录

~~~java
@Test
public void testAddOne() throws Exception {
    // 2、封装请求对象
    IndexRequest indexRequest = new IndexRequest("index_hello", "article", "101");
    Article article = new Article();
    article.setId(101L);
    article.setTitle("程序员hello world101");
    article.setContent("这个一个hello world 程序101");
    indexRequest.source(JSON.toJSONString(article), XContentType.JSON);

    // 3、 获取执行结果
    IndexResponse indexResponse = restHighLevelClient.index(indexRequest, RequestOptions.DEFAULT);
    int status = indexResponse.status().getStatus();
    System.out.println(status);
    
	// 4、关闭连接
    restHighLevelClient.close();
}
~~~

##### 添加多条文档记录

~~~java
@Test
public void testAddMany() throws Exception {
    // 2、封装请求对象
    BulkRequest bulkRequest = new BulkRequest();

    // 添加第一条
    IndexRequest indexRequest = new IndexRequest("index_hello", "article", "204");
    Article article = new Article();
    article.setId(204L);
    article.setTitle("程序员hello world204");
    article.setContent("这个一个hello world 程序204");
    indexRequest.source(JSON.toJSONString(article), XContentType.JSON);

    // 添加第二条
    IndexRequest indexRequest2 = new IndexRequest("index_hello", "article", "205");
    Article article2 = new Article();
    article.setId(205L);
    article.setTitle("程序员hello world205");
    article.setContent("这个一个hello world 程序205");
    indexRequest2.source(JSON.toJSONString(article2), XContentType.JSON);

    bulkRequest.add(indexRequest);
    bulkRequest.add(indexRequest2);

    // 3、 获取执行结果
    BulkResponse bulkResponse = restHighLevelClient.bulk(bulkRequest, RequestOptions.DEFAULT);
    int status = bulkResponse.status().getStatus();
    System.out.println(status);

    restHighLevelClient.close();
}
~~~

##### 根据关键词查询

~~~java
@Test
public void testQueryByKeyword() throws Exception {
    // 文档及类型
    SearchRequest searchRequest = new SearchRequest("index_hello");
    searchRequest.types("article"); // 设置查询的类型

    // 搜索字段及关键词
    SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
    searchSourceBuilder.query(QueryBuilders.matchQuery("title", "程序员"));

    // 添加搜索字段及关键词
    searchRequest.source(searchSourceBuilder);

    // 搜索
    SearchResponse searchResponse = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);
    // 获取搜索记录
    SearchHits searchHits = searchResponse.getHits();
    // 打印总记录数
    long totalHits = searchHits.getTotalHits();
    System.out.println("总记录数：" + totalHits);
    // 打印搜索的文档信息
    SearchHit[] hits = searchHits.getHits();
    for (SearchHit hit: hits) {
        String source = hit.getSourceAsString();
        System.out.println(source);
    }
    restHighLevelClient.close();
}
~~~

##### 布尔查询

~~~java
@Test
public void testQueryByBoolean() throws Exception {
    SearchRequest searchRequest = new SearchRequest("index_hello");
    searchRequest.types("article"); // 设置查询的类型

    SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();

    BoolQueryBuilder boolQueryBuilder = QueryBuilders.boolQuery(); // 布尔查询构建器

    // must &&关系
    MatchQueryBuilder matchQueryBuilder = QueryBuilders.matchQuery("title", "程序员");
    boolQueryBuilder.must(matchQueryBuilder);

    TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("content", "程序");
    boolQueryBuilder.must(termQueryBuilder);

    searchSourceBuilder.query(boolQueryBuilder);

    searchRequest.source(searchSourceBuilder);

    // 获取查询结果
    SearchResponse searchResponse = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);

    SearchHits searchHits = searchResponse.getHits();
    long totalHits = searchHits.getTotalHits();
    System.out.println("总记录数：" + totalHits);
    SearchHit[] hits = searchHits.getHits();
    for (SearchHit hit: hits) {
        String source = hit.getSourceAsString();
        System.out.println(source);
    }
    restHighLevelClient.close();
}
~~~

#### 10、SpringData

##### 引入依赖

~~~xml
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-elasticsearch</artifactId>
    <version>3.0.5.RELEASE</version>
</dependency>
~~~

##### 配置连接

~~~xml
<!-- elasticsearch 客户端配置 -->
    <elasticsearch:transport-client id="esClient"
                                    cluster-name="my-application"
                                    cluster-nodes="127.0.0.1:9301,127.0.0.1:9302,127.0.0.1:9303"/>
    <!-- 包扫描器 扫描dao接口-->
    <elasticsearch:repositories base-package="com.belean.es.repositories"/>
    <!-- elasticsearch 模板 -->
    <bean id="elasticsearchTemplate" class="org.springframework.data.elasticsearch.core.ElasticsearchTemplate">
        <constructor-arg name="client" ref="esClient"/>
    </bean>
~~~

##### 创建实体类

~~~java
@Document(indexName = "sdes_hello", type = "article")
public class Article {

    @Id
    @Field(type = FieldType.keyword, store = true)
    private Long id;

    @Field(type = FieldType.text, store = true, analyzer = "ik_smart")
    private String title;

    @Field(type = FieldType.text, store = true, analyzer = "ik_smart")
    private String content;
}
~~~

##### 创建查询库

~~~java
public interface ArticleRepository extends ElasticsearchRepository<Article, Long> {

}
~~~

##### 添加索引/映射

~~~java
// 创建索引，并配置映射关系
elasticsearchTemplate.createIndex(Article.class);
// 配置映射关系
elasticsearchTemplate.putMapping(Article.class);
~~~

##### 文档操作

~~~java
// 1、添加文档
Article article = new Article();
article.setId(1L);
article.setTitle("title标题标题");
article.setContent("content内容内容");
articleRepository.save(article);

// 2、删除文档
articleRepository.deleteById(1L);

// 3、修改即添加，id相同，会覆盖原来的

// 4、查询所有
Iterable<Article> articles = articleRepository.findAll();

// 4、根据Id查询
Optional<Article> optional = articleRepository.findById(1L);
Article article = optional.get();
~~~

##### 自定义查询方法

~~~java
public interface ArticleRepository extends ElasticsearchRepository<Article, Long> {
	// 默认查询都是10条
    // 根据title查询
    List<Article> findByTitle(String title);
    // 根据title或content查询，分页查询
    List<Article> findByTitleOrContent(String title, String content, Pageable pageable);
}
~~~

~~~java
// 注意：默认是先分词，再查询，且每个词之间是And关系
// 使用分页1、当前页，2、页大小
Pageable pageable = PageRequest.of(0, 15);
articleRepository.findByTitleOrContent("title", "content", pageable);
~~~

##### 原生查询

~~~java
// 创建一个查询对象
NativeSearchQuery query = new NativeSearchQueryBuilder()
    .withQuery(QueryBuilders.queryStringQuery("标题title").defaultField("title"))
    .withPageable(PageRequest.of(0, 15))
    .build();
List<Article> articleList = elasticsearchTemplate.queryForList(query, Article.class);
~~~

















































