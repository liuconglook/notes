## 问题汇总

### LayUI

~~~js
// layui基础结构
layui.config({
        base: '[[@{/layui/extend/}]]'
    }).extend({
        xmSelect: 'xm-select' // https://maplemei.gitee.io/xm-select
    }).use(['layer', 'form', 'table', 'laydate', 'element', 'xmSelect', 'upload'], function() {
        let $ = layui.jquery
        let layer = layui.layer
        let laydate = layui.laydate
        let form = layui.form;
        let table = layui.table;
        let element = layui.element;
        let xmSelect = layui.xmSelect; // 下拉框
        let upload = layui.upload;
});

// 设置表单input的label宽度
.layui-form-label{ width: 100px; }

// 解决layui下拉框在弹窗中显示问题，将不会出现滚动条，所以下拉框能显示全
// body .layer-ext-myskin .layui-layer-content { overflow: visible; }
layer.open({
    skin: "layer-ext-myskin"
});

// 获取select选中值
find("select[name='accountsCode']").next().find("dd.layui-this").attr("lay-value")

// layui获取select选中的text内容，在select监听中
data.elem[data.elem.selectedIndex].text

// xmSelect 获取选中值
let list = [];
$(this).find("td div.xm-option.selected").each(function(index, item){
    list.push($(this).attr("value"));
});

// 给弹窗的提交按钮加commit
layero.addClass('layui-form');
layero.find('.layui-layer-btn0').attr({
    'lay-filter' : 'addBtn',
    'lay-submit' : ''
});

// 获取包括本身标签的html
$("#div").prop("outerHTML")

/* 隐藏秒选项*/
.laydate-time-list{
   padding-bottom: 0;
   overflow:hidden;
}
.laydate-time-list>li{
    width:50%!important;
}
.laydate-time-list>li:last-child {
    display: none;
}

// 打印
var newWin=window.open('','Print-Window');
newWin.document.open();
newWin.document.write('<html><body onload="window.print()">'+$('#div').html()+'</body></html>');
newWin.document.close();
setTimeout(function(){newWin.close();},10);

// 上传文件
<div class="layui-upload-drag" id="uploadFile">
    <i class="layui-icon"></i>
	<p>点击上传，或将文件拖拽到此处</p>
	<div class="layui-hide" id="uploadDemoView">
    </div>
</div>
upload.render({
    elem: '#uploadFile'
    ,url: ''
    ,auto: false // 取消自动上传
    ,accept: 'file' // 普通文件
});
let formData = new FormData();
formData.append("file", $("#uploadFile").next()[0].files[0]);
~~~

### JS

~~~js
// 数组克隆
Object.assign(target, source);

// 元素克隆
$("#div").clone();

// 对象克隆
{...obj}

// 获取对象属性数量
Object.keys(obj).length

// 字符转小写
str.toLowerCase()

// 正则判断，是否是时间格式字符串，\d表数字，{4}表个数 [^]*表任意字符/任意个数
let reg = /^\d{4}-\d{2}-\d{2}[^]*$/
reg.test(reg)

// ajax上传文件
let formData = new FormData();
formData.append("file", $("#uploadFile")[0].files[0]);
$.ajax({
    processData: false,
    contentType: false,
    data: formData
});

// 格式化时间，使用：DateFormat(new Date(row.date), "yyyy-MM-dd HH:mm:ss")
window.DateFormat = function (date, fmt) { //author: meizz
    var o = {
        "M+": date.getMonth() + 1, //月份
        "d+": date.getDate(), //日
        "H+": date.getHours(), //小时
        "m+": date.getMinutes(), //分
        "s+": date.getSeconds(), //秒
        "q+": Math.floor((date.getMonth() + 3) / 3), //季度
        "S": date.getMilliseconds() //毫秒
    };
    if (/(y+)/.test(fmt)) fmt = fmt.replace(RegExp.$1, (date.getFullYear() + "").substr(4 - RegExp.$1.length));
    for (var k in o)
        if (new RegExp("(" + k + ")").test(fmt)) fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
    return fmt;
}
~~~

### JAVA

~~~java
@Transient -- 通用mapper忽略字段
@GeneratedValue(strategy = GenerationType.IDENTITY) -- 主键整型自增，批量插入时会重复
@GeneratedValue(generator = "JDBC") -- 主键整型自增，批量插入不会重复
&lt;= -- <=
&gt;= -- >=

// 获取yml属性
@Autowired
private Environment environment;

// 除法
divide(BigDecimal(5), 2, BigDecimal.ROUND_HALF_UP) -- 保留两位小数，四舍五入

// 前面补零 T普通字符，%0任意0，7个，d整数
String.format("T%07d", 1); // T0000001

// 前端json，后台接收数组
@RequestParam(value = "idList[]") Integer[] idList
~~~

#### Stream

参考：https://xie.infoq.cn/article/50324deb1efcf3845113e341f

> 使用

~~~java
// 1、遍历
list.stream().forEach(System.out::print);
list.stream().forEach(e -> System.out.print(e));
list.stream().forEach(e -> {System.out.print(e)});

// 2、list转map，使用toConcurrentMap，则返回安全的CurrentMap
List<JSONObject> list = new ArrayList<>();
Map<String, String> map = list.stream().collect(Collectors.toMap(e -> e.getString("name"), e -> e.getString("age")));

// 3、分页
list.stream().skip((page - 1) * limit).limit(limit).collect(Collectors.toList());

// 4、条件过滤
String name = "张三";
Integer age = 18;
list.stream().filter(obj -> {
    Boolean nameIsPass = true;
    if(!StringUtils.isEmpty(name)) {
        nameIsPass = obj.getName().equals(name);
    }
    Boolean ageIsPass = true;
    if(age != null) {
        ageIsPass = obj.getAge() == age;
    }
    return nameIsPass && ageIsPass;
});

// 5、分组，使用groupingByConcurrent，则返回安全的CurrentMap
Map<String, List<JSONObject>> map = list.stream().collect(
    Collectors.groupingBy(
        e -> e.getString("name"), 
    	Collectors.collectingAndThen(Collectors.toList(), es -> es)));
// 结果
/**
 * 张三:[{"name":"张三","age":18}]
 * 李四:[{"name":"李四","age":20}, {"name":"李四","age":22}]
 */
// 分组统计
Map<String, Long> map = list.stream().collect(Collectors.groupingBy(e -> e.getString("name"), Collectors.counting()));

// 6、去重
ArrayList<JSONObject> list3 = list.stream().collect(
                Collectors.collectingAndThen(
                        Collectors.toCollection(() -> new TreeSet<>(Comparator.comparing(e -> e.getString("name")))),
                        ArrayList::new)
);

// 7、求和 0为起始值
// 方式1
Integer sum = students.stream().map(Student::getAge).reduce(0, (a, b) -> a + b);
System.out.println(sum3);
// 方式2
Optional<Integer> sum2 = students.stream().map(Student::getAge).collect(Collectors.reducing(Integer::sum));
sum2.ifPresent(System.out::println);
// 方式3
Integer sum3 = students.stream().map(Student::getAge).collect(Collectors.reducing(0, Integer::sum));
System.out.println(sum3);
// 方式4
Integer sum4 = students.stream().collect(Collectors.reducing(0, Student::getAge, Integer::sum));
System.out.println(sum4);

// 8、最大/最小值
Optional<Student> studentMax = students.stream().collect(Collectors.maxBy(Comparator.comparing(Student::getAge)));
        studentMax.ifPresent(System.out::println);
        Optional<Student> studentMin = students.stream().collect(Collectors.minBy(Comparator.comparing(Student::getAge)));
        studentMin.ifPresent(System.out::println);

// 9、joining 拼接字符串
// 可选参数：分隔符、前缀、后缀
String str = students.stream().map(Student::getName).collect(Collectors.joining(","));
System.out.println(str);

// 10、mapping 聚合
List<Integer> ages = students.stream().collect(Collectors.mapping(Student::getAge, Collectors.toList()));
System.out.println(Arrays.toString(ages.toArray()));



~~~

> 技巧

~~~java
// 1、stream返回的对象通常是Optional的，也就是可以为null的，通过isPresent()判断是否为空

// 2、返回输入参数本身
Function.identity();
// 源码
static <T> Function<T, T> identity() {
    return t -> t;
}
// 使用案例
List<String> list = Arrays.asList(new String[]{"1","2","3"});
Map<String, String> map = list.stream().collect(Collectors.toMap(Function.identity(), Function.identity()));
map.forEach((key, valve) -> System.out.println(key + ":" + valve));
// 结果
//1:1
//2:2
//3:3

// 3、遍历时删除元素
// 方式1：使用迭代器删除iterator.remove();
// 方式2：java8提供的removeIf
list.removeIf(e -> e.getAge() == 20 || e.getAge() == 18);

~~~

> 总结

- 聚合后操作（类似SQL中的HAVING）
  - reducing
    - 比如求和，先聚合，再求和。

- 操作后聚合（类似SQL中的WHERE）
  - mapping
    - 比如收集age，先选择收集age，再toList。





### MSQL

#### 常用函数

~~~sql
-- 字符拼接
CONCAT('','','')

-- 按,拼接字符
CONCAT_WS(',','','')

-- 转小写字母
LOWER(str)

-- 三元运算符
IF(IFNULL(str),0,1)

-- 按，截取，获取第一组，索引从1开始
substring_index(str, ',', 1)

-- 查找以逗号分割的集合元素位置
FIND_IN_SET('b', 'a,b,c,d,e')

-- 获取字符串索引
LOCATE('a', 'a,b')

-- 1开始，截取3，索引1开始
SUBSTRING(str, 1, 3)

-- 获取字符串长度
LENGTH(str)

-- 分组拼接 
-- 修改 MySQL 配置文件 my.cnf ，增加 group_concat_max_len = 102400
GROUP_CONCAT(DISTINCT col separator ',')

-- 如果为'',就赋值null
NULLIF(str, '')

-- 格式化当前时间
DATE_FORMAT(NOW(),'%Y-%m-%d %H:%i:%s')

-- 时间范围内天数 DAY/HOUR/MINUTE/SECOND
timestampdiff(DAY,#{startDate},#{endDate}))

-- 获取星期索引，1日/2一/3二/4三/5四/6五/7六
DAYOFWEEK('2021-05-18')

-- 小数转整数，四舍五入
round()

-- 左边不满5位补零 00001
LPAD('1',5,'0')
              
-- 右边不满5位补零 10000
RPAD('1',5,'0')

-- 当前时间加6月
DATE_ADD(NOW(),INTERVAL 6 MONTH)

-- 当前时间减6月
DATE_SUB(NOW(),INTERVAL 6 MONTH)

-- 计数去重
COUNT(DISTINCT a.job_number)

-- id自增
SELECT (@rowNo := @rowNo+1) id FROM (select @rowNo :=0) b

-- 查询表字段及注释
SELECT COLUMN_NAME, COLUMN_COMMENT FROM information_schema.columns 
WHERE TABLE_SCHEMA ='DBName'  AND TABLE_NAME = 'tableName';

-- 一条数据截取成多条
-- dates:'2021-05-12 00:00:00','2021-05-16 00:00:00'
SELECT 
DISTINCT substring_index(substring_index(a.dates,',',b.help_topic_id+1),',',-1)   
FROM (SELECT dates) a 
JOIN  mysql.help_topic b 
ON b.help_topic_id < (LENGTH(a.dates) - LENGTH(REPLACE(a.dates,',',''))+1);
              
-- 行数据变纵数据，选取符合条件的行显示
CASE WHEN type = 1 THEN name ELSE '' END
~~~

#### 存储过程与函数

~~~sql
-- 创建存储过程
create procedure pro_name(param1 varchar(20), param2 varchar(20)) 
begin
end;

-- 执行存储过程
call pro_name('a','b')

-- 存在才删除存储过程
drop procedure if exists pro_name;

-- 创建存储函数
CREATE FUNCTION func_name(param1 varchar(20), param2 varchar(20))
RETURNS VARCHAR(200) -- 返回类型
NO SQL -- 没有sql
BEGIN
END;

-- 执行存储函数
SELECT func_name('a','b')

-- 删除存储函数
drop FUNCTION if exists pro_name;
~~~

> 函数案例

~~~sql
DELIMITER // -- 更改结束符，END//

-- 根据日期范围，获取日期集合
CREATE FUNCTION func_dates(startDate varchar(20), endDate varchar(20))
RETURNS TEXT
NO SQL
BEGIN
	declare days int default 1;
	declare i int default 0;
	declare dates TEXT default '';
	set days = timestampdiff(DAY,startDate,endDate);
	while i<=days DO
			if i=0 then
				set dates = startDate;
			else
				set startDate = DATE_ADD(startDate,INTERVAL 1 DAY);
				set dates = CONCAT(dates,',',startDate);
			end if;
			set i=i+1;
	end while;
	RETURN dates;
END;
-- 使用，结果：2021-05-10,2021-05-11,2021-05-12
SELECT func_dates('2021-05-10', '2021-05-12')


-- 根据日期，获取星期
CREATE FUNCTION func_week(date varchar(20))
RETURNS VARCHAR(10)
NO SQL
BEGIN
	declare i int default 0;
	declare target int default 0;
	declare weeks varchar(30) default '星期日,星期一,星期二,星期三,星期四,星期五,星期六';
	declare split varchar(10) default ',';
	set target = DAYOFWEEK(date);
	while i+1 != target DO
			set weeks = SUBSTRING(weeks, LOCATE(',',weeks) + 1);
			set i=i+1;
	end while;
	RETURN SUBSTRING(weeks, 1, 3);
END;
-- 使用，结果：星期一
SELECT func_week('2021-05-10')

-- 根据字母，获取下一个字母
DROP FUNCTION IF EXISTS func_next_letter;
CREATE FUNCTION func_next_letter(word varchar(10))
RETURNS VARCHAR(10)
NO SQL
BEGIN
	declare letter varchar(100) default 'A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z';
	declare split varchar(10) default ',';
	while word != SUBSTRING(letter, 1, 1) DO
			set letter = SUBSTRING(letter, LOCATE(split,letter) + 1);
	end while;
	set letter = SUBSTRING(letter, LOCATE(split,letter) + 1);
	RETURN SUBSTRING(letter, 1, 1);
END;
-- 使用，结果：Z
SELECT func_next_letter('Y');
~~~

### WebSocket

心跳机制：https://www.cnblogs.com/tugenhua0707/p/8648044.html

#### 整合SpringBoot案例

>导包

~~~xml
<!-- webSocket -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
~~~

>配置类

~~~java
@Configuration
public class WebConfig {
    /**
     * 支持websocket
     * 内置tomcat需要配置，否则忽略
     * @return
     */
    @Bean
    public ServerEndpointExporter createServerEndExporter(){
        return new ServerEndpointExporter();
    }
}
~~~

>Controller

~~~java
/**
 * websocket服务端
 */
@Component
@ServerEndpoint(value = "/ws/hello/{userName}")
public class MyWebSocket {

    //用来存放每个客户端对应的MyWebSocket对象
    private static CopyOnWriteArraySet<MyWebSocket> user = new CopyOnWriteArraySet<MyWebSocket>();

    //与某个客户端的连接会话，需要通过它来给客户端发送数据
    private Session session;

    @OnMessage
    public void onMessage(@PathParam("userName")String userName, String message, Session session) throws IOException {
        //群发消息
        for (MyWebSocket myWebSocket : user) {
            myWebSocket.session.getBasicRemote().sendText(userName + "说：" + message);
            //myWebSocket.session.getBasicRemote().sendText("<img src=''/>");
        }
    }

    @OnOpen
    public void onOpen(@PathParam("userName")String userName, Session session){
        System.out.println(userName+" 连接上了...");
        this.session = session;
        user.add(this);
    }
    @OnClose
    public void onClose(@PathParam("userName")String userName){
        System.out.println(userName+" 断开连接...");
        user.remove(this);
    }
    @OnError
    public void onError(@PathParam("userName")String userName, Session session,Throwable error){
        System.out.println(userName+" 连接失败...");
        error.printStackTrace();
    }

}
~~~

>界面Controller

~~~java
@Controller
@RequestMapping("/ws")
public class WebSocketController {
	@GetMapping("/{userName}")
    public ModelAndView wx(@PathVariable("userName") String userName, ModelAndView model) {
        model.setViewName("/websocket/ws");
        model.addObject("userName", userName);
        return model;
    }
}
~~~

> ws.html

~~~html
<!DOCTYPE html>
<html lang="en"xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>hello webSocket</title>
    <link rel="stylesheet" th:href="@{/layui/css/layui.css}" media="all">
    <style>
    </style>
</head>
<body>
当前用户: <span th:text="${userName}"></span>
<span onclick="send();">点我发送消息</span>: <input id="message"/>
<div id="messageDiv"></div>
</body>
<script type="text/javascript" th:src="@{/js/jquery.min.js}"></script>
<script type="text/javascript">
    if ("WebSocket" in window) {
        var ws = new WebSocket("ws://localhost:8089/hwkj/ws/hello/[[${userName}]]");
        ws.onopen = function () {
            console.log("open connect");
        };

        ws.onmessage = function (evt) {
            $("#messageDiv").html($("#messageDiv").html() + evt.data + "</br>");
        };

        ws.onclose = function () {
            console.log("close connect");
        };
    } else {
        alert("您的浏览器不支持 WebSocket!");
    }

    function send() {
        ws.send($("#message").val());
    }
</script>
</html>
~~~

> 聊天

http://localhost:8089/hwkj/ws/userName

http://localhost:8089/hwkj/ws/userName2





### 随笔

#### 缓存

缓存：一般指内存，读写速度快。或用户本地缓存，不需要存储到服务器。

缓存穿透：查询不存在的数据，数据库中数不存在自然缓存也就没有啦，比如查询id为负数不存在的数据，一般这种情况是用户恶意行为。解决方案是对查询参数做校验，对不符合的请求拦截掉。

缓存雪崩：一般是大量缓存同时过期造成的，同时又有大量请求过来就会奔溃。解决方案是随机在不同时段设置过期时间就好。

缓存击穿：跟缓存雪崩类似，都是由缓存过期造成，不同的是过期后，并发查询同一条数据导致奔溃。

总结：缓存穿透需要校验参数，处理边界条件；缓存雪崩/击穿需要随机设置缓存时间；通用解决方案是对热点数据进行缓存预热/缓存永不过期。

缓存穿透就是，学校没有围墙，而你不走大门。

缓存雪崩就是，学校饭点到了，而学校没有按年级就餐。

缓存击穿就是，学校放假，一起出校门。

#### 程序员的职业素质

专业主义就意味着担当责任。

所谓的专业人士，就是能对自己犯下的错误负责的人。

桑塔亚纳的诅咒：”不能铭记过去的人，注定重蹈先人的覆辙。“

- 如何承担责任
  - 不行损害之事
  - 道歉，且不能一而再三的犯同样的错误。
  - 测试覆盖率
  - 持续重构

设计模式和原则

方法：XP（极限编程）、Scrum、精益、看版、瀑布、结构化分析、结构化设计。

实践：测试驱动开发（TDD）、面向对象设计（OOD）、结构化编程、持续集成、结对编程。

工件：UML图、DFD图、结构图、petri网络图、状态迁移图表、流程图、决策图。

ruby语言

练习卡塔（kata、CodeKata）

了解业务领域，最不专业的做法是，简单按照规格说明来编写代码。

“能就是能，不能就是不能，不要说‘试试看’。” ——尤达

试试看，就意味着承认之前未尽全力，就意味着还得加把劲。

本质上讲，承若“尝试”就是一种不诚实的表现。你在说谎。这么做的原因，可能是为了护住面子和避免冲突。

委屈专业原则以求全，并非问题的解决之道。舍弃这些原则，只会制造出更多麻烦。

有条件的说是

定义“完成”

TDD：Spring Test（测试依赖）、TestNG（单元测试、集成测试）、Spring Restdocs（根据测试代码生成RESTful的接口文档）

TDD是专业人士的选择。它是一项能够提升代码确定性、给程序员鼓励、降低代码缺陷率、优化文档和设计的原则。对TDD的各项尝试表明，不使用TDD就说明你可能还不够专业。（如果遵循某项原则会弊大于利，专业的开发人员就当然不会选用它）

卡塔有点类型复盘，就是复盘已知的流程，从中不断优化，最终练习纯熟。例如练习热键和导航操作。特别有利于在潜意识中构筑通用的问题与解决方案的联系。

- 保龄球：http://butunclebob.com/ArticleS.UncleBob.TheBowling-GameKata
- 素因子：http://butunclebob.com/ArticleS.UncleBob.ThePrimeFactors-Kata
- 自动换行：http://thecleancoder.blogspot.com/2010/10/craftsman-62-dark-path.html

瓦萨（wasa）：两个人的kata，可以用乒乓游戏（http://c2.com/cgi/wiki?PairProgrammingPingPongPattern）来进行类似的练习。

自由练习（randori）：很多人参与的练习。

验收测试：业务方与开发方合作编写的测试，其目的在于确定需求“已经完成“。

- 完成的定义
  - 代码完成
  - 测试完成
  - QA和需求方确认完成

自动化测试工具：FitNesse、Cucumber、cuke4duke、robot framework、Selenirn。

通常，业务分析员测试“正确路径”，以证明功能的业务价值；QA则测试“错误路径”、边界条件、异常、例外情况，因为QA的职责是考虑哪些部分可能出问题。

单元测试时深入系统内部进行，调用特定类的方法；验收测试则是在系统外部，通常是在API或者UI级别进行。

单元测试和验收测试首先是文档，然后才是测试。他们的主要目的是如实描述系统的设计、结构、行为。其真正价值不在测试上，而在具体指标上。

持续集成就是确保单元测试和验收测试每天都能运行好几次。

凡是不能在5分钟内解决的争论，都不能靠辩说解决。（用数据说话）

注意力点数，精神能量

阅读具有创作性的作品

《番茄工作法图解：简单易行的时间管理方法》（25in1.com）

坑法则（The Rule of Holes）：如果你掉进了坑里，别挖。

选择那些你在危机时刻依然会遵循的纪律原则，并且在所有工作中都遵守这些纪律。遵守这些纪律原则是避免陷入危机的最好途径。


#### 代码整洁之道

《敏捷软件开发：原则、模式与实践》前传

> 整洁的定义

- 只做好一件事（职责单一）
  - 模块、类、函数都专注于一事，尽量少的依赖关系，避免代码污染。

- 简单明了（可读性高）
  - 意图明显，无可挑剔，找不出bug，最优性能。
  - 易读的代码往往也容易写。
- 有意义的命名（字面编程）
  - 让代码优雅起来，如同散文诗一般。
  - 体现系统中的全部设计理念。
- 能通过所有测试（测试驱动开发）
  - 方便改进脏代码
- 没有重复代码（重构）
  - 消除重复，尽力清晰地表达，而不是一直堆积。
- 小规模抽象（低耦合高内聚）
  - 提早构造抽象，为修改留有余地。
  - 提醒真正要做的事，避免随意实现集合行为。

> 命名

需多花点时间命名，或有意识地多重命名。

可读的，可搜索的（用英文代替数字）。

聪明程序员了解，明确是王道；专业程序员善用其能，编写其他人能理解的代码。

- 类（名词/名词短语）
- 方法/函数（动词/动词短语）
  - 起个能说清其功能的名称，写完函数后，读一遍函数名，感受是否“深合已意”了。
  - 例如：add添加、save添加或修改、registerAndLogin注册并登录。

> 函数

- 只做一件事。（看是否能再拆出一个函数）
  - 错误处理就是一件事。
    - 使用异常替代错误码，避免因错误码枚举的修改导致大量依赖类的重新编译部署。
- 理想的参数数量是0，其次是1，再次是2，应尽量避免3（多参）。
  - 多参应考虑将其封装成类。
  - 避免传入boolean类型参数，这样会使函数看起来在做多件事。
- 函数简短（行字符上限120，20行最佳）。

> 注释

- 少写注释。
- 当需要用注释来解释一段代码时，应审视这段代码是否应该重写。
- 注释不能美化糟糕的代码，与其花时间写注释，不如花时间整列那堆糟糕的代码。
- 好注释
  - 对意图的解释
    - 例如：解释了某段循环做了什么事。
  - 阐释
    - 把晦涩难懂的参数或返回值翻译为可读。
    - 例如：将60000毫秒的数字注释为1分钟。
  - 警示
    - 例如：说明SimpleDateFormat是线程不安全的，所有每次调用都返回是新的对象。
  - TODO注释
    - 工作列表
  - 凸显重要性
- 坏注释
  - 能用变量或函数说明的，就不要注释。
  - 注释代码，最好删掉。

> 结构

- 变量与函数分离
- 互相调用的函数应放在一起。
- 相同类型函数应该放在一起。

> 德墨忒尔律

- 类C的方法f只能调用以下对象的方法：
  - 本类自己的方法。
  - 由f创建的对象的方法。
  - 本类实体变量持有的对象的方法。
- 方法不应调用由任何方法返回对象的方法。

> 错误处理

- 使用异常而非返回码

- 可控异常
  - 例如：java的throws列出方法可能传递给调用者的异常。
  - 弊端：违反开放/闭合原则，例如修改底层方法，给其添加一个异常时，上层代码就必须捕获，从而形成一个修改链，导致重新构建、发布。
- 异常类
  - 来源分类：来源组件
  - 类型分类：网络错误，编程错误
- 特例对象
  - 获取某个值，如果没有则抛出异常，捕获后赋值默认。
  - 将异常封装进特例对象后
    - 获取某个值，如果没有，则返回特例对象。

- 别返回null
  - Optional
  - Collections.emptyList()
- 别传递null
  - 对null值进行抛出。

> TDD三定律

- 三定律
  - 在编程不能通过的单元测试前，不可编写生成代码。
  - 只可编写刚好无法通过的单元测试，不能编译也不算过。
  - 只可编写刚好足以通过当前失败测试的生产代码。
- 三要素
  - 可读性
  - 可读性
  - 可读性
- FIRST原则
  - 快速（Fast）：测试应该够快。
  - 独立（Independent）：测试应该相互独立。
  - 可重复（Repeatable）：测试应当可在任何环境中重复通过。
  - 自足验证（Self-Validating）：测试应该有布尔值输出。
  - 及时（Timely）：测试应及时编写。
- 每个测试一个断言
- 测试代码和生成代码一样重要

> 类

- 公共静态常量，私有静态变量，私有实体变量，公共函数。（很少会有公共变量）
- 类应该短小，更短小。
  - 衡量方法：权责。
  - 类名正好解释类的权责。
  - 单一职责原则（SRP）
    - 类或模块应有且只有一个条加以修改的理由。
  - 开放-闭合原则（OCP）
    - 类应当对扩展开放，对修改封闭。
  - 依赖倒置原则（DIP）
    - 类应当依赖于抽象而不是具体细节。

> 系统

- 构造与使用分开
  - 工厂
  - 依赖注入（DI）
    - 控制反转（IoC）
  - AOP
  - 代理
- 最佳的系统架构由模块化的关注面领域组成，每个关注面均用纯java（或其他语言）对象实现，不同的领域之间用最不具有侵害性的方面或类方面工具整合起来，这种机构能测试驱动，就像代码一样。
- 领域特定语言（DSL）
  - DSL是一种单独的小型脚本语言或以标准语言写的API。
  - 优秀的DSL填平了领域概念和实现领域概念的代码之间的“壕沟”。

> 迭进

- 简单设计规则
  - 运行所有测试
  - 重构
  - 不可重复
  - 表达力
  - 尽可能少的类和方法

> 并发编程

- 对象是过程的抽象，线程是调度的抽象。

- 并发会在性能和编写额外代码上增加一些开销。
- 正确的并发是复杂的，即便对于简单的问题也如此。
- 并发缺陷并非总能重现，所以常被看做偶发事件而忽略，未被当做真的缺陷看待。
- 并发常常需要对设计策略的根本性修改。

- 并发防御原则
  - 分离并发相关代码与其他代码。
  - 谨记数据封装；严格限制对可能被共享的数据的访问。
  - 使用数据复本。
  - 线程应尽可能地独立。
- Java库
  - ReentrantLock可重入锁
  - Semaphore
    - 经典的“信号”的一种实现，有计数器的锁。
  - CountDownLatch
    - 在释放所有等待的线程之前，等待指定数量事件发生的锁。这样，所有线程都平等地几乎同时启动。
  - java.util.concurrent/java.util.concurrent.atomic/java.util.concurrent.locks.
- 执行模型
  - 限定资源
    - 例如：固定的数据库连接数，固定大小的读写缓存等。
  - 互斥
    - 每一时刻仅有一个线程能访问共享数据或共享资源。
  - 线程饥饿
    - 一个或一组线程在很长时间内或永久被禁止。
    - 例如：总是让执行得快的线程先运行，假如执行得快的线程没完没了，其他线程就会“挨饿”。
  - 死锁
    - 两个或多个线程互相等待执行结束。每个线程都拥有其他线程需要的资源，得不到其他线程拥有的资源，就无法终止。
  - 活锁
    - 执行次序一致的线程，每个都想要起步，但发现其他线程已经“在路上”。由于竞步的原因，线程会持续尝试起步，但在很长时间内却无法如愿，甚至永远无法启动。
  - 生产者-消费者模型
    - 一个或多个生产者线程创建某些工作，并置于缓存或队列中。
    - 一个或多个消费者从队列中获取并完成这些工作。
    - 其中的队列是一种限定资源。
  - 读者-作者模型
    - 挑战之处在于平衡读者线程和作者线程的需求，实现正确操作，提供合理的吞吐量，避免线程饥饿。
  - 宴席哲学家
    - 这种竞争式系统就会遭遇死锁、活锁、吞吐量和效率降低等问题。
    - 你可能遇到的并发问题，大多数都是这三个问题的变种。
- 警惕同步方法之间的依赖
  - 避免使用一个共享对象的多个方法。
  - 基于客户端的锁定
    - 客户端代码在调用第一个方法前锁定服务端，确保锁的范围覆盖了调用最后一个方法的代码。
  - 基于服务端的锁定
    - 在服务端内创建锁定服务端的方法，调用所有方法，然后解锁。让客户端代码调用新方法。
  - 适配服务端
    - 创建执行锁定的中间层。这是一个基于服务端的锁定例子，但不修改原始服务端代码。
- 保持同步区域微小
- 尽早考虑关闭问题。
- 测试线程代码
  - 将伪失败看作可能的线程问题。
    - 不要将系统错误归咎于偶发事件。
  - 先使非线程代码可工作。
    - 不要同时追踪非线程缺陷和线程缺陷。确保代码在线程之外可工作。
  - 编写可插拔的线程代码。
    - 编写可插拔的线程代码，这样就能在不同的配置环境下运行。
  - 编写可调整的线程代码。
  - 运行多于处理器数量的线程。
  - 在不同平台上运行。
  - 调整代码并强迫错误发生。

> 逐步改进192

- Args

>JUnit251

- JUnit

> 重构267

- SerialDate

> 坏味道

- 注释
  - 不恰当的信息：例如：作者、时间、法律文件
    - 通过代码管理工具即可完成。
  - 废弃的注释：过时的注释
    - 注释也要随着代码及时更新。
  - 冗余的注释
    - 代码能解释的，绝不用注释。
  - 糟糕的注释
    - 错误的注释。
  - 注释掉的代码
    - 删除被注释的代码，代码管理工具能找到之前的代码。
- 环境
  - 需要多步才能实现的构建
    - 一键构建。
  - 需要多步才能做到的测试
    - 一键测试。
- 函数
  - 过多的参数
  - 输出参数：输入参数当做输出参数返回。
  - 标识参数：布尔参数。
  - 死函数：永远不会被调用的函数。
- 一般性问题
  - 一个源文件中存在多种语言。
  - 明显的行为未被实现。
    - 遵循“最小惊奇原则”（The Principle of Least Surprise），函数或类应该实现其他程序员有理由期待的行为。
  - 不正确的边界行为：应考虑各种情况下的边界问题，确认边界条件，并编写测试。例如null判断。
  - 忽略安全：不应忽视编译器警告、关闭失败测试等。
  - 重复：遵循DRY原则（Don't Repeat Yourself，别重复自己），考虑抽象。
  - 在错误的抽象层级上的代码：较低层级概念和较高层级概念不应混杂在一起。
  - 基类依赖于派生类：通常来说，基类对派生类应该一无所知。这样修改派生类时，无需重新构建和部署基类。
  - 信息过多：
    - 模块
      - 设计好的模块有着非常小的接口，让你事半功倍。
      - 设计低劣的模块有着广阔、深入的接口，你不得不事倍功半。
    - 接口
      - 设计良好的接口并不提供许多需要依靠的函数，所以耦合度低。
      - 设计低劣的接口提供大量你必须调用的函数，耦合度较高。
    - 优秀的软件开发人员
      - 限制类或模块中暴露的接口数量。
      - 类中方法越少越好。
      - 函数知道的变量越少越好。
      - 类拥有的实体变量越少越好。
      - 隐藏你的数据、工具函数、常量、临时变量。
      - 不要为子类创建大量受保护变量和函数。
      - 尽量保存接口紧凑。
      - 通过限制信息来空值耦合度。
  - 死代码：不执行的代码。可以在工具类方法、switch/case、if等中找到。
  - 垂直分隔：变量的定义与使用靠近，被调用函数应在调用函数的下方。
  - 前后不一致：小心选择约定，一旦选中，就小心持续遵循。
  - 混淆视听：没有实现的无参构造、从不调用的函数、没有信息量的注释等等。
  - 人为耦合：例如将一个枚举类嵌入到某个类中，只为方便使用。
  - 特性依恋：某个类中的方法依赖于其他类的方法。
  - 选择算子参数：传入一个布尔参数，如果true则这样计算，否则那样计算。
    - 正确做法应该分离为两个方法。
  - 晦涩的意图：例如只有自己才看得懂的命名缩写。
  - 位置错误的权责：例如，PI常量应该放哪？
    - 最小惊奇原则，代码应该放在读者自然而然期待它所在的位置。
  - 不恰当的静态方法：静态方法不应该有多态行为。
  - 使用解释性变量：通常先计算赋值变量后操作比直接计算后操作可读性高。
  - 函数名称应该表达其行为：如果你必须查看函数的实现（或文档）才知道它做什么的，就该换个更好的函数名。
  - 理解算法：写出算法代码后，往往需要重构其算法变地可读。
  - 把逻辑依赖改为物理依赖
  - 用多态替代if/else或switch/case
  - 遵循标准约定
  - 用命名常量替代魔术数：定义常量，可用隐藏数据，可读，方便修改。
  - 准确：别懒得理会决定的准确性。
    - 调用可能返回null的函数，需要检查null。
    - 查询数据库唯一记录，需检查不存在记录。
    - 处理货币，使用整数，并恰当处理四舍五入。
    - 如果可能并发更新，确认你实现了某种锁定机制。
  - 决定基于约定：用到良好命名的枚举的switch/case要弱于拥有抽象方法的基类。
  - 封装条件：将难以理解的表达式封装为可读的方法。
  - 避免否定性条件：`str.isNotEmpty()`要好于`!str.isEmpty()`
  - 函数只该做一件事
  - 掩盖时序耦合：通过传递上一个函数返回参数的方式使用下一个函数，使之函数调用具有时序性。
  - 别随意：不作为类工具的公共类，不应该放在其他类里面。
  - 封装边界条件：nextLevel要比level+1要好，也能够避免重复计算。
  - 函数应该只在一个抽象层级上
  - 在较高层级放置可配置数据
  - 避免传递浏览
- java
  - 通过使用通配符避免过长的导入清单，例如：`import package.*`
    - 如果需要查看清单，例如：IDEA可用使用Alt + Enter
  - 不要继承常量：应该用静态导入`import static package.*`
  - 使用枚举代替常量
- 名称
  - 采用描述性名称
  - 名称应与抽象层级相符
  - 尽可能使用标准命名法
  - 无歧义的名称
  - 为较大作用范围选用较长名称
  - 避免编码
  - 名称应该说明副作用
- 测试
  - 测试不足
  - 使用覆盖率工具
  - 别略过小测试
  - 被忽略的测试就是对不确定事物的疑问
  - 测试边界条件
  - 全面测试相近的缺陷
  - 测试失败的模式有启发性
  - 测试覆盖率的模式有启发性
  - 测试应该快速

> 并发编程II（313）

#### 敏捷软件开发：原则、模式与实践

敏捷开发（Agile Development）是一种面临迅速变化的需求快速开发软件的能力。为了获取这种敏捷性，我们需要使用一些可以保持软件的灵活、可维护的设计原则，以及针对特点问题可以平衡这些原则的设计模式，最后形成有反馈的实践。

> 敏捷软件开发宣言

- 个体和交互			胜过    过程和工具
  - 优秀的成员
  - 有效的沟通
  - 先构建团队，再构建环境。

- 可以工作的软件    胜过    面面俱到的文档
  - Martin文档第一定律（Martin's first law of document）：直到迫切需要并且意义重大时，才来编制文档。
- 客户合作			    胜过    合同谈判
  - 成功的项目需要有序、频繁的客户反馈。
- 响应变化			    胜过    遵循计划
  - 较好的计划策略：
    - 为下两周做详细的计划。
    - 为下三个月做粗略的计划。
    - 再以后就做极为粗糙的计划或模糊的想法。

> 12条原则

- 我们最优先要做的是通过尽早的、持续的交付有价值的软件来使客户满意。
  - 初期交付的系统中所包含的功能越少，最终交付的系统的质量就越高。
  - 交付得越频繁，最终产品的质量就越高。
- 即使到了开发的后期，也欢迎改变需求。敏捷过程利用变化来为客户创造竞争优势。
- 经常性地交付可以工作的软件，交付的间隔可用从几周到几个月，交付的时间间隔越短越好。
- 在整个项目开放期间，业务人员和开发人员必须天天都在一起工作。
- 围绕被激励起来的个人来构建项目。给他们提供所需要的环境和支持，并且信任它们能够完成工作。
- 在团队内部，最具有效果并且富有效率的传递信息的方法，就是面对面的交谈。
- 工作的软件是首要的进度度量标准。
- 敏捷过程提倡可持续的开发速度。责任人、开发者和用户应该能够保持一个长期、恒定的开发速度。
- 不断地关注优秀的技能和好的设计会增强敏捷能力。
- 简单——使未完成的工作最大化的艺术——是根本的。
- 最好的架构、需求和设计出自组织的团队。
- 每隔一段时间，团队会在如何才能更有效地工作方面进行反省，然后相应地对自己的行为进行调整。

> 敏捷方法

- SCRUM
- Crystal
- 特征驱动软件开发（Feature Driven Development，简称FDD）
- 自适应软件开发（Adaptive Software Development，简称ADP）
- 极限编程（eXtreme Programming, 简称XP）

> 极限编程

极限编程（eXtreme Programming, 简称XP）是敏捷方法中最著名的一个。

- 客户作为团队成员

  - 这里的客户指的是能够提供需求和问题的人。

- 用户素材

  - 用户素材（user stories）就是正在进行的关于需求谈话的助记符。它是一个计划工具，客户可用使用它并根据它的优先级来安排实现该需求的时间。

- 短交付周期

  - 迭代计划：

    - 两周迭代一次。
    - 开发人员为本次迭代设定预算，客户则选择任意数量的用户素材。
    - 一旦迭代开始则代表客户同意不再修改本次迭代中用户素材的定义和优先级别。

  - 发布计划：

    - 三个月进行一次较大的交付。
    - 制定随后的6次迭代的内容。

    - 发布计划于迭代计划类似，只不过可以随时修改计划。

- 验收测试：

  - 用户素材的验收测试（Acceptance Tests）是在就要实现该用户素材之前或实现该用户素材的同时进行编写的。
  - 验收测试使用能够自动并且反复运行的某种脚本语言编写。
  - 开发人员开发一个简单的脚本系统，或者他们拥有一个可以开发脚本系统的质量保证（Quality Assurance，QA）部门。许多客户借助QA来开发验收测试工具，并自己编写验收测试。

- 结对编程：

  - 所有的产品（production）代码都是由结对的程序员使用同一台电脑共同完成的。
  - 结对的关系每天至少要改变一次，以便每个程序员在一天中可以在两个不同的结对中工作。
  - 在一次迭代期间，每个团队成员应该和所有其他的团队成员在一起工作过，并且他们应该参与来本次迭代中所涉及的每项工作。

  - Laurie Williams和Nosck的研究表明，结对非但不会降低开发团队的效率，而且会大大减少缺陷率。

- 测试驱动的开发方法

  - 首先编写一个失败的单元测试，然后编写代码使测试通过。
    - 由于它要测试的功能还不存在，所有它会运行失败。
  - 测试用例和代码共同演化，其中测试用例循序渐进地对代码的编写进行指导。
    - 快速反馈，检查是否正常运行，利于重构，激发你去解除各个模块间的耦合。

- 集体所有权

  - 在结对编程中的每一对都具有拆出（check out）任何模块并对它进行改进的权力。没有程序员对任何一个特定的模块或技术单独负责。
    - 每个人都参与GUI方面的工作。
    - 每个人都参与中间件方面的工作。
    - 每个人都参与数据库方面的工作。
  - 没有人比其他人在一个模块或者技术上具有更多的权威。
  - 这并不意味着XP不需要专业的知识。有专业的知识只会让你最有可能从事这方面的任务，并和能够传授其他方面知识的专家一起工作。你不会被限制在自己的专业领域。

- 持续集成

  - 结对人员会在一项任务上工作1~2小时。（创建测试用例和产品代码）
  - 在某个间歇点，决定把代码拆入回去。（测试通过后，把新的代码集成进代码库中）
    - 必要时，会和先于他们拆入的程序员协商，一旦集成构建新的系统，会运行每个测试，包括验收测试。
    - 如果破坏来原先工作的部分，就会进行修正。一旦通过所有测试了，就完成了此次拆入工作。

- 可持续的开发速度

  - 软件项目不是全速的短跑，它是马拉松长跑。
  - 为了快速地完成开发，团队必须保持稳定、适中的速度。
  - XP的规则是不允许团队加班工作，团队必须保持旺盛的精力和敏锐的警觉。

- 开放的工作空间

  - 房间有一些桌子，每张桌子上有两到三台工作站。
  - 每台工作站前为结对编程的人员预备两张椅子。
  - 墙壁上挂满状态图标、任务明细表、UML图等。
  - 房间充满激烈的讨论（研究表明，在“充满积极讨论的屋子”里工作，生产率非但不会降低，反而会成倍地提高）。

- 计划游戏

  - 计划游戏（planning game）的本质是划分业务人员和开发人员之间的的职责。
    - 业务人员（客户）决定特性（feature）的重要性。
    - 开发人员决定实现一个特性所花费的代价。
  - 在每次发布和每次迭代的开始，开发人员基于最近一次迭代或发布中所完成的工作量，为客户提供一个预算。

- 简单设计

  - 考虑能够工作的最简单的事情。
    - 尽可能寻找能实现当前用户素材的最简单设计。
  - 你将不需要它。
  - 一次，并且只有一次。



39



#### 修改代码的艺术

> 修改软件的主要原因

- 增加特性
- 修正缺陷
- 改善设计
- 优化对资源的利用

>增加特性和修改缺陷

- 不同角度上的区分
  - 对于用户来说，需要完成更多的工作，就是增加特性。
  - 对于客户来说，需要修改某一功能，对于开发来说可能没那么简单，需要增加特性。
- 行为上的区分（重点）
  - 例如：增加方法并调用，就是增加行为。（注意：仅增加方法，既不是增加行为，也不是修改行为）
  - 例如：修改方法代码，就是改变行为。

> 改善设计

- 修改软件结构，使其变得易于维护的同时，需保持行为完整。
  - 测试可以说就是为了保持行为的完整而存在的。
- 改善的过程中，删除的行为叫做缺陷。
- 改善设计而不改变其行为的动作加做重构。
  - 重构就像是在整理代码，风险低，由测试提供支持。

> 优化

优化与重构类似，修改的时候都会保持行为完整，但会改变一些“其他的内容”。

- 在重构中，”其他的内容“指程序结构。

- 在优化中，“其他的内容”指某种资源，一般是时间或空间（内存）。

> 改善与优化

- 共同点：修改某些东西的同时，还会保持功能不变。
- 可改变：结构、功能、资源使用。

> 修改前，需要思考的三个问题

- 要做什么修改？
- 如何知道已经正确地做出了修改？
- 如何知道修改对其他部分没有产生破坏？

> 单元测试

- 单元，即系统的最小行为单元。

- 面向过程语言：单元通常是函数。
- 面向对象语言：单元通常是类。

- 术语：测试用例、测试用具，表示测试代码。

- 好的单元测试
  - 运行很快，如果运行的慢就不是单元测试了（需要0.1秒的测试就是很慢的测试）。
  - 能够帮助我们定位问题。
- 区分非单元测试
  - 会访问数据库。
  - 会通过网络通信。
  - 会访问文件系统。
  - 需要做特定的工作配置环境（像编辑配置文件）来运行测试。
- 高层次测试：确认一组类的行为。
- 遗留代码修改方法（测试驱动开发TDD）
  - 确定变更点
  - 找到测试点
  - 打破依赖关系
  - 编写测试
  - 做出修改并重构
- 打破依赖关系的原因
  - 感知：当无法访问我们的代码所计算的值时，我们就会打破依赖以感知。
    - 例如：网络通信，与设备相关，无法感知调用这个类的方法的效果。
  - 分离：当我们无法把一段代码放到测试用具中执行时，我们就会打破依赖以分离。
    - 例如：网络通信，与设备相关，无法使用与其应用程序的其他部分分离运行。
- 伪协作程序
  - 替换依赖对象的实现。
- 模拟对象
  - 在内部执行断言的伪对象。
  - 是一种工具/框架，例如java的Mockito。
- 接缝模型
  - 接缝是你可以在程序中变更行为而不需要编辑的地方。
  - 每个接缝都有启用点，在那里你可以决定使用哪个行为。
  - 预处理接缝
    - 例如：C和C++的宏预处理程序。
  - 链接接缝
    - 例如：profile，#if。
  - 对象接缝
    - 例如：java的多态
- 重构
  - 对软件内部结构的变更，使其更易于理解，并且修改成本更低，而不会改变既有的行为。

