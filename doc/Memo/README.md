## 问题汇总

### Google Hacking

https://zhuanlan.zhihu.com/p/22161675

~~~
// 默认And，联合几个关键字一起搜索
keyword keyword2 keyword3
// 或
keyword | keyword2 | keyword3

// 指定网站搜索
site:www.baidu.com

// 指定文件
fileType:pdf

// 搜索标题 allintitle可混合操作符使用
intitle:java
// google搜索的标题
insubject:java
// 搜索正文 allintext可混合操作符使用
intext:优点

// 时间范围，工具=>选择时间范围

// 指定公司股票市场信息
stocks:百度
~~~

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
$.ajax({
    processData: false,
    contentType: false,
    data: formData
});
@RequestParam(value = "file", required = false) MultipartFile file
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

#### npm

~~~shell
# 安装windows编译环境
npm install --global --production windows-build-tools
npm install -g node-gyp
npm install -g node-sass --save-dev

# 编译并安装程序
npm install
# Failed at the node-sass@4.12.0 postinstall script.
# 查看node与node-sass版本对应关系：https://www.npmjs.com/package/node-sass
# 我node:v14.15.4，所以需要node-sass:v4.14+
# 查看所有版本
npm view node-sass versions
# 最新版
npm view node-sass version
# 查看本地安装的版本
npm ls node-sass
# 查看全局安装的版本
npm ls node-sass -g
# 查看更多信息
npm info node-sass

npm uninstall node-sass
npm cache clean --force
npm install node-sass@4.14.1
npm install

# 运行
npm run serve
~~~



### JAVA

~~~java
@Transient -- 通用mapper忽略字段
@GeneratedValue(strategy = GenerationType.IDENTITY) -- 主键整型自增，批量插入时会重复
@GeneratedValue(generator = "JDBC") -- 主键整型自增，批量插入不会重复
&lt;= -- <=
&gt;= -- >=

// 获取属性
// 优先级：jar命令行参数 > 系统变量 > properties/yml
@Autowired
private Environment environment;

// 除法
divide(BigDecimal(5), 2, BigDecimal.ROUND_HALF_UP) -- 保留两位小数，四舍五入

// 前面补零 T普通字符，%0任意0，7个，d整数
String.format("T%07d", 1); // T0000001

// 前端json，后台接收数组
@RequestParam(value = "idList[]") Integer[] idList
    
// transient关键字：忽略序列号字段，需实现Serializable接口
public class User implements Serializable{
    transient private String password;
}
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

#### Enum

> mysql enum

~~~sql
-- 存储 java enum 的 name 值
`status` enum('PENDING','APPROVED','REJECTED') NOT NULL COMMENT '状态：PENDING-审核中、APPROVED-已审核、REJECTED-已退回',

-- 存储 java enum 的 value 值
`status` int NOT NULL COMMENT '状态：0-审核中、1-已审核、2-已退回',
~~~

> java enum

~~~java
// 配合mysql enum使用
public enum ReviewStatus {
    PENDING("审核中"),
    APPROVED("已审核"),
    REJECTED("已退回");
    
    private String status;
    
    ReviewStatus(String status) {
        this.status = status;
    }
    
    public String getStatus() {
        return this.status;
    }
}
// 如需存储数值value，需编写枚举转换器
public enum ReviewStatus {
    PENDING(0, "审核中"),
    APPROVED(1, "已审核"),
    REJECTED(2, "已退回");
    
    private int value;
    private String status;
    
    ReviewStatus(int value, String status) {
        this.value = value;
        this.status = status;
    }
    
    public int getValue() {
        return this.value;
    }
    
    public String getStatus() {
        return this.status;
    }
}
~~~

> mybatis 枚举转换器

~~~java
public class Article {
    // Other Field...
	private ReviewStatus status;
    // Getter/Setter...
}
// article.setStatus(ReviewStatus.REJECTED);
~~~

- `org.apache.ibatis.type.EnumTypeHandler`（默认）
  - 存储枚举name值。
- `org.apache.ibatis.type.EnumOrdinalTypeHandler`
  - 按枚举顺序，存储枚举的下标（从0开始）。
- 自定义枚举转换器
  - 继承`org.apache.ibatis.type.BaseTypeHandler`

~~~java
public class ReviewStatusHandler extends BaseTypeHandler<ReviewStatus> {
    /**
     * 设置配置文件设置的转换类以及枚举类内容，供其他方法更便捷高效的实现
     *
     * @param type 配置文件中设置的转换类
     */
    public ReviewStatusHandler(Class<ReviewStatus> type) {
        if (type == null)
            throw new IllegalArgumentException("Type argument cannot be null");
        this.type = type;
        this.enums = type.getEnumConstants();
        if (this.enums == null)
            throw new IllegalArgumentException(type.getSimpleName()
                    + " does not represent an enum type.");
    }

    // 预编译语句参数，java类型转数据库类型
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, ReviewStatus parameter, JdbcType jdbcType) throws SQLException {
        ps.setInt(i, parameter.getValue());
    }

    // 结果集，根据 字段名 数据库类型转java类型
    @Override
    public ReviewStatus getNullableResult(ResultSet rs, String columnName) 
        throws SQLException {
        // 根据 字段名 获取审核状态int类型
        int value = rs.getInt(columnName);
        if (rs.wasNull()) {
            return null;
        } else {
            // 返回value对应枚举
            return locateEnum(value);
        }
    }

    // 结果集，数据库类型转java类型
    @Override
    public ReviewStatus getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        // 根据 索引 获取审核状态int类型
        int value = rs.getInt(columnIndex);
        if (rs.wasNull()) {
            return null;
        } else {
            // 返回value对应枚举
            return locateEnum(value);
        }
    }
    
    // 存储过程，数据库类型转java类型
    @Override
    public ReviewStatus getNullableResult(CallableStatement cs, int columnIndex) 
        throws SQLException {
        // 根据 索引 获取审核状态int类型
        int value = cs.getInt(columnIndex);
        if (cs.wasNull()) {
            return null;
        } else {
            // 返回value对应枚举
            return locateEnum(value);
        }
    }


    /**
     * 根据Value获取对应的枚举
     * @param value 
     * @return 枚举
     */
    private ReviewStatus locateEnum(int value) {
        for (ReviewStatus status : ReviewStatus.values()) {
            if (status.getValue() == value) {
                return status;
            }
        }
        throw new IllegalArgumentException("未知的枚举类型：" + value);
    }
}
~~~

~~~xml
<!-- 方式一：mapper配置（局部） -->
...
<resultMap id="BaseResultMap" type="com.belean.project.entity.Article">
    ...
    <result column="status" property="status" 
            typeHandler="com.belean.project.entity.enum.ReviewStatusHandler"/>
</resultMap>
...
~~~

~~~yml
# 方式二：application.yml配置（全局）
mybatis:
	type-handlers-package: com.belean.project.entity.enum
~~~

~~~xml
<!-- 方式三：不使用 -->
<insert id="add" parameterType="com.belean.project.entity.Article">
    INSERT INTO article(status) VALUES(#{article.status.value})
</insert>
~~~

~~~java
// 方式四：手动设置Getter/Setter
public class Article {
	private ReviewStatus status;
    
    public int getStatus() {
        return status.getValue();
    }
    
    public void setStatus(int value) {
        for (ReviewStatus status : ReviewStatus.values()) {
            if (status.getValue() == value) {
                this.status = status;
                break;
            }
        }
    }
}
~~~
#### UML

- 类
  - 类名
  - 抽象类（斜体）
  - `<<interface>>`加接口名
- 修饰符
  - +：public
  - -：private
  - #：protected
- 属性，例如：- name:string
- 方法，例如：+ setName(name:string)

- 关系：
  - 实现（implements）/继承（extends）：空心三角形+虚线/实线
    - 实现类或继承类可统称泛化（Generalization），一般虚线指实现，实线指继承。
    - 是实现还是继承，通过类（普通类、抽象类、接口）来区分。
    - 例如：B实现了A，==B 空心三角形+虚线/实线 A==
  - 依赖（dependency）：虚线箭头
    - 例如：学生依赖电脑。==Student 虚线箭头 Computer==
  - 关联（association）：实线箭头
    - 关联比依赖强。
    - 例如：学生可以没有电脑，但学生不能没有老师。==Student 实线箭头 Teacher==
    - 单向关联（实线箭头）、双向关联（实线）、自身关联（实线连自己）、多维关联（实线，中间空心菱形）
  - 聚合（aggregation）：空心菱形+实线
    - 聚合表示集体和个体之间的关联关系
    - 例如：学生属于班级这个集体。==Classes 空心菱形+实线 Student==
  - 组合（composition）：菱形+实线
    - 组合表示个体与组成部分之间的关联关系。
    - 例如：鼠标是电脑的一部分。==Computer 菱形+实线 Mouse==

### Python

~~~shell
# 设置源
pip3 install cryptography -i http://mirrors.aliyun.com/simple/ --trusted-host=mirrors.aliyun.com

# 更新pip
python -m pip install -U pip

# 安装模块
pip install pyautogui

# 运行程序
python autoClick.py
~~~

#### 基础

~~~python
# bool值判断，以下判断为False，其他为True
False, None, 0, "", [], {}, ()

# bool转string
str(isEmpty)

# 与或非
and or not

~~~

#### 作用域

~~~python
isEmpty = False
def fuc():
	global isEmpty # 申明使用全局变量
    print(isEmpty) # False
~~~

#### pyautogui

https://pypi.org/project/PyAutoGUI/

~~~python
import pyautogui

# 点击坐标、点击次数、点击间隔、左键
pyautogui.click(1900, 10, clicks=2, interval=0.0, button='left')

# 获取屏幕宽高
width, height = pyautogui.size()
print(width, height)
~~~

#### keyboard

https://pypi.org/project/keyboard/

~~~python
import keyboard

def event():
	print('enter f9')

# 设置热键，event事件
keyboard.add_hotkey('f9', event)
~~~

#### 多线程

~~~python
import _thread

def task(threadName, delay):
	print('run')

# 启动线程，运行task函数
_thread.start_new_thread(task, ("mouse_move", 2, ))
~~~

#### 监听鼠标移动

https://pypi.org/project/pynput/

~~~python
from pynput.mouse import Listener

def on_move(x, y):
    print(x, y)

# join监听事件，开始监听
with Listener(on_move=on_move) as listener:
		listener.join()
~~~

#### 自动点击程序

按`f9`可`开始/结束`自动点击（移动到右上角），控制鼠标`悬停/离开`（右上角）可`开始/结束`自动点击

~~~python
import pyautogui
import keyboard
from pynput.mouse import Listener
import _thread

isClick = False

def event():
	global isClick
	if isClick:
		isClick = False
	else:
		isClick = True
	print(str(isClick) + '\t', end = "\r")

def on_move(x, y):
	global isClick
	if isClick and (x < 1895 or y > 20):
		isClick = False
		print(str(isClick) + '\t', end = "\r")
	elif x > 1895 and y < 20:
		isClick = True
		print(str(isClick) + '\t', end = "\r")

def startClick():
	global isClick
	while(True):
		if isClick:
			pyautogui.click(1900, 10, clicks=1, interval=0.0, button='left')

def mouse_move(threadName, delay):
	with Listener(on_move=on_move) as listener:
		listener.join()

keyboard.add_hotkey('f9', event)
_thread.start_new_thread(mouse_move, ("mouse_move", 2, ))
startClick()
~~~

### Shell

Shell：一种应用程序，可通过指令来访问操作系统内核的服务。（一般指shell程序界面）

Shell脚本：编写的shell指令集。（用Shell语言编写的脚本程序）

sh：早期的shell。

bash：为GNU计划编写的Unix shell，遵循POSIX标准，兼容sh。

POSIX可移植操作系统接口（Portable Operating System Interface of UNIX）。

~~~bash
#!/bin/bash


#!/bin/sh
# 功能较少，不支持数组
# 在linux中，/bin/sh软连接的/bin/bash,实际使用的还是bash，等价于#!/bin/bash --posix
~~~

变量名：只能英文字母/数字/下划线，首字母不能以数字开头。

- 局部变量：当前脚本中定义的。
- 环境变量：所有程序都能使用，在系统环境变量中定义。
- shell变量：由shell程序设置的特殊变量。

~~~bash
# set：一般写在最前面
set -e # 若指令返回值不等于0（0表示成功执行），则立即退出shell
set -x # 输出每个操作的结果详细

# 赋值
name="belean"
# 删除遍历
unset name
# 标记只读，不可变，不可删除
readonly name

# 多行注释，EOF可替换为'或!
:<<EOF
for in 循环
do done 循环体括号
$file或${file} 使用变量
遍历C://Users下的目录，并打印
EOF
for file in $(ls /c/Users); do 
	echo ${file};
done
~~~

#### 字符串

~~~bash
# 单引号原样输出
name='hello'

# 双引号，可转义、拼接字符变量
str="${name} \"world\"!"
# hello "world"!

# 字符串长度
echo ${#str}

# 截取字符串,索引0开始
echo ${str:7:${#str}}

# 查找str变量中l或o的位置（哪个先出现的就计算哪个，索引位置从1开始）
echo `expr index "${str}" lo`

# 开启转义 \n换行 \c不换行
echo -e "hello \n world"

# 结果导向文件
echo "hello world" > myfile

# 显示命令执行结果
echo `date`

# 查找替换
IGNORE_FILES="index.html,README.md"
${IGNORE_FILES//,/"\n"} # index.html\nREADME.md

# 按字符串分隔
files=(${IGNORE_FILES//,/"\n"})  
for file in ${files[@]}
do
   echo "$file"
done
~~~

#### 数组

~~~bash
# 赋值
arr=(1 2 3 4)
arr[4]=5

# 数组长度
echo ${#arr[@]}
# 或
echo ${#arr[*]}
~~~

~~~bash
#!/bin/bash

# 遍历数组
arr=(1 2 3 4)
for i in ${arr[@]}
do 
	echo ${i}
done
~~~

#### 参数传递

- $#：参数个数。
- $*：例如"$1"是第一个参数。
- $$：脚本运行的当前进程ID号。
- $!：后台运行的最后一个进程ID号。
- $-：显示Shell使用的当前选项。
- $?：显示最后命令的退出状态。0表示没有错误，其他任何值表示有错误。

~~~bash
# demo.sh
#!/bin/bash
echo "Shell";
echo "文件名：$0";
echo "参数1：$1";
echo "参数2：$2";
echo "参数3：$3";
echo "$$";
echo "$!";
echo "$-";
echo "$?";
# 执行结果
$ demo.sh 1 2 3
Shell
文件名：./demo.sh
参数1：1
参数2：2
参数3：3
283

hB
0
~~~

#### 运算符

~~~bash
# + - * / %
val=`expr 2 + 2`

# == !=
bool=`expr 2 == 2`

# 表达式用[]包裹或(())
# []适用于-lt等，(())适用于<等
a=2
b=2
if [ $a == $b ]
then
	echo "a = b"
else
	echo "a != b"
fi

# 关系运算符 只支持数字比较
# -eq -ne 相等、不等
# -gt -lt 大于、小于
# -ge -le 大于等于、小于等于
if [ $a -ge $b ]
then
	echo "a >= b"
fi

# 布尔运算 !非、-o或、-a与
# 非0即真
if [ $a == $b -a $a == 2 ]
then
	echo "a = b = 2"
fi
# &&与、||或

# 字符串运算符
# = != 相等、不等
# -z 长度是否为0
# -n 长度是否不为0
# $ 是否为空

# 文件测试运算符
# -r -w -x 是否可读、可写、可执行
# -d -f 是否是目录、文件
# -s 是否为空（文件大小）
# -e 是否存在

# -b 是否是块设备文件
# -c 是否是字符设备文件
# -g 是否设置了SGID位
# -k 是否设置了粘着位(Sticky Bit)
# -p 是否有名管道
# -u 是否设置了SUID位
# -S 是否是socket
# -L 是否存在并且是一个符号链接
~~~

#### 流程控制

- break：退出循环
- continue：结束本次循环，继续下次循环

~~~bash
# if
if 条件表达式 
then
	条件成立
fi

# if else
if 条件表达式
then
	条件成立
else
	条件不成立
fi

# elif
if 条件表达式
then
	条件成立
elif
then
	条件成立
else
	条件不成立
fi

# case esac：类似switch case
a=1
case ${a} in
	0)
		echo 'case 0'
	;;
	1)
		echo 'case 1'
	;;
	2)
		echo 'case 2'
	;;
esac

# for in
arr=(1 2 3 4)
for item in ${arr[@]}
do
	echo ${item}
done
# 或 写成一行
for item in ${arr[@]}; do echo ${item}; done;

# for
for(( i=0; ${i}<${#arr[@]}; `expr i++` ))
do
	echo ${arr[i]}
done

# while
i=0
while(( ${i} < ${#arr[*]} ))
do
	echo ${arr[i]}
	let "i++"
done

# until 循环 条件为false时循环
a=0
until [ ! ${a} -lt 10 ]
do
	echo ${a}
	let "a++"
done
~~~

#### 函数

~~~bash
# 获取参数从1开始
printer(){
	echo "print:${1}"
}
printer "hello world" "abc"

# 返回值通过$?调用
inputPrint() {
	echo "请输入："
	read content
	return ${content}
}
inputPrint
echo "print：$?"
~~~

#### 输入/输出重定向

~~~bash
# 1、将命令执行的结果输出到文件
# 追加写入>
ls > outFile.text
# 查看
cat outFile.text

# 2、输入重定向
# 查看文件行数
wc -l outFile.text
# 结果：3 outFile.text
# 重定向后，取文件内容
wc -l < outFile.text
# 结果：3
# 对outFile.text文件进行模糊查询并显示
cat |grep 'demo' < outFile.text

# 3、写入指定文件
# 标准输入文件(stdin)：0
# 标准输出文件(stdout)：1
# 标准错误文件(stderr)：2
# 默认输入：0<
# 默认写入：1>

# 将stderr与stdout合并，写入file
ls > file 2>&1

# 4、执行命令，不想要结果
ls > /dev/null
~~~

#### 文件包含

引入sh脚本

~~~bash
. fileName
# 或
source fileName
~~~

> bash1.sh

~~~bash
#!/bin/bash
str="hello world"
~~~

> bash2.sh

~~~bash
#!/bin/bash
echo "bash1.sh: ${str}"
~~~

> 执行

~~~bash
$ ./bash2.sh
source bash1.sh: hello world
~~~

### Dockerfile

https://yeasy.gitbook.io/docker_practice/introduction/what

使用dockerfile脚本构建镜像

`项目目录/Dockerfile`

- FROM：基于某个镜像，以便进行操作

- COPY：拷贝文件到容器

- ADD：拷贝文件到容器，如果是压缩包，则会解压

- RUN：执行操作，可以有多行，在docker build时执行

  - 通常用于安装软件包。
  - 每一个RUN都会启动一个容器，都是新建的一层。所以RUN之间是没有关联的。

- CMD：执行操作，在docker run时运行

  - 通常用于运行程序，多个CMD只会生效最后一个。

  - 如果docker run指定了参数，则CMD将被忽略。

  - ~~~dockerfile
    CMD echo "hello world" # 执行shell命令
    CMD ["/demo.sh", "hello", "world"] # 执行shell脚本文件，及设置的参数
    CMD ["hello", "world"] # 为ENTRYPOINT提供默认参数
    ~~~

- ENTRYPOINT：执行操作，在docker run时运行

  - 与CMD用法类似。
  - 多个ENTRYPOINT只会生效最后一个。
  - 不同在于，其一定会执行。

- ENV：环境变量

  - ~~~dockerfile
    ENV key value
    ENV key1=value1 key2=value2
    ~~~

  - 后续命令中使用：`$key`

- ARG：参数

  - 用法与ENV一致。
  - 仅在dockerfile中有效，在运行的容器中无效。

- VOLUME：匿名数据卷

  - 设置默认要挂着的目录，挂载到匿名数据卷。

  - 防止启动时忘挂载目录，导致数据丢失和容器变大。

  - ~~~dockerfile
    VOLUME ["",""]
    VoLUME ""
    ~~~

- EXPOSE：挂载端口，随机挂载端口

  - ~~~dockerfile
    # 容器内8080，随机挂载到8080或8081
    EXPOSE 8080 [8080,8081]
    ~~~

- WORKDIR：指定工作目录

  - ~~~dockerfile
    WORKDIR /
    ~~~

  - 类似cd，统一不同层的工作目录

- USER：指定用户和组

  - 用户和组必须已经存在

  - ~~~dockerfile
    USER userName[:group]
    ~~~

### GitHub Actions

https://docs.github.com/cn/actions/learn-github-actions/understanding-github-actions

工作流程：由事件驱动的工作流程，可在github上进行构建、测试、打包、发布或部署项目。

在仓库创建目录：`.github/workflows`

在目录下创建文件：`config.yml`，名称随意

~~~yml
# 工作流名称
name: GitHub Actions 
# 触发事件
on: [push]
# 工作流
jobs:
  # 工作名称
  Explore-GitHub-Actions:
    # 所在的运行器
    runs-on: ubuntu-latest
    # 步骤
    steps:
      - run: echo "触发事件：${{ github.event_name }}"
      - run: echo "运行所在的机器：${{ runner.os }}"
      - run: echo "分支名称：${{ github.ref }} ，及仓库名称：${{ github.repository }}"
      - name: Check out repository code
        # 检出仓库，才能对仓库进行操作
        uses: actions/checkout@v2 
      - run: echo "已克隆目标仓库，准备测试你的代码"
      - name: List files in the repository
        # 将查看到该仓库下的目录列表
        run: |
          ls ${{ github.workspace }}
      - run: echo "工作状态：${{ job.status }}"
~~~

一个工作流下有多个工作（jobs），每个工作下有多个步骤（steps），每个步骤可以执行多个操作（action）。

> 运行器（runs-on）

https://docs.github.com/cn/actions/using-github-hosted-runners/about-github-hosted-runners

GitHub 托管的运行器基于 Ubuntu Linux、Microsoft Windows 和 macOS。

- Ubuntu Linux：ubuntu-latest
- Microsoft Windows：windows-latest
- macOS：macos-latest

#### 事件（on）

~~~yml
# 单一事件
on: push

# 事件列表
on: [push, pull_request]

# main分支push事件
on:
  push:
    branches:
      - main
      
# 创建，触发发布事件
on:
  release
    types: [created]
~~~

#### 环境变量（env）

https://docs.github.com/cn/actions/learn-github-actions/environment-variables

环境变量区分大小写

> github环境变量

~~~bash
# 触发事件
${{ github.event_name }}
# 所在运行器
${{ runner.os }}
# 分支名称
${{ github.ref }}
# 仓库名称
${{ github.repository }}
# 工作目录
${{ github.workspace }}
# 工作状态
${{ job.status }}
~~~

> 自定义环境变量

`jobs.<job_id>.steps[*].env`、`jobs.<job_id>.env` 和 `env`

~~~bash
env:
	ROOT_ENV: "ROOT ENV"
jobs:
  first-job:
      env:
        COMMON_ENV: "COMMON ENV"
      steps:
        - name: test env
          run: echo "${{ env.ROOT_ENV }}"
          run: echo "${{ env.COMMON_ENV }}"
          run: echo "${{ env.NAME }}"
          env:
            NAME: "Hello World"
~~~

#### 步骤（steps）

~~~yml
steps:
  - name: step1 # 步骤名称
    uses: actions/checkout@v2 # 选择某个仓库中定义的操作，@可用选择commit/分支/tag
    
  - name: step2
  	if: ${{ failure() }} # 条件满足才执行该步骤，例如：failure()表示上一个步骤是否失败
  	shell: bash # 指定shell
  	run: echo "failure !!" # 执行shell命令
~~~

在工作流程所在仓库中操作

~~~yml
steps:
  - name: Check out repository
    uses: actions/checkout@v2
  - name: step1 # 使用所在仓库中的操作，必须有上面步骤的检出仓库操作
    uses: ./.github/actions/my-action
    with: # 传递参数，myaction使用INPUT_USER_NAME、INPUT_PASSWORD作为环境变量
      user_name: root
      password: 123	
~~~

> Note.js环境

~~~yml
- name: install Note.js
  uses: actions/setup-node@v2-beta
  with:
    node-version: '12'
~~~

> Python3环境

~~~yml
- name: install Python3
  uses: actions/setup-python@v1
  with:
    python-version: '3.x'
    architecture: 'x64'
~~~

> Git环境





#### 元数据（action）

Docker 和 JavaScript 操作需要元数据文件。文件名时必须是`action.yml`或`action.yaml`，其主要定义操作的输入、输出和进入点。

~~~yml
name: 'git-flow'
description: 'git-flow and actions test'
author: 'liucong'
branding:
  icon: 'git-branch'
  color: 'gray-dark'
# 输入：uses本操作时，可通过with传参
inputs:
  user_name:
    description: 'The is username'
    require: true
    # default: root
  password:
    description: 'The is password'
    require: true
outputs:
  is_login:
    description: 'is login in successfully'
    value: ${{ steps }}
~~~

#### 自动拉取代码

https://github.com/liuconglook/git-pull-action

> 使用ssh登录git

entrypoint.sh

~~~bash
#!/bin/bash
# 任意语句执行的失败都会退出shell
set -e
# 创建ssh秘钥文件
if [ -n "$SSH_PRIVATE_KEY" ]
then
  mkdir -p /root/.ssh
  echo "$SSH_PRIVATE_KEY" > /root/.ssh/id_rsa
  chmod 600 /root/.ssh/id_rsa
fi
# 追加主机认证
if [ -n "$SSH_KNOWN_HOSTS" ]
then
  mkdir -p /root/.ssh
  echo "StrictHostKeyChecking yes" >> /etc/ssh/ssh_config
  echo "$SSH_KNOWN_HOSTS" > /root/.ssh/known_hosts
  chmod 600 /root/.ssh/known_hosts
else
  echo "WARNING: StrictHostKeyChecking disabled"
  echo "StrictHostKeyChecking no" >> /etc/ssh/ssh_config
fi
# 复制id_rsa、ssh_config、known_hosts到~/.ssh目录下，不需要异常信息，也不管是否复制成功
mkdir -p ~/.ssh
cp /root/.ssh/* ~/.ssh/ 2> /dev/null || true
# 执行拉取代码的脚本
chmod 700 /git-pull.sh
source "/git-pull.sh" $*
~~~

> git-pull.sh

~~~bash
#!/bin/bash

set -e

SOURCE_REPO=$1 # 源仓库地址git@github.com:liuconglook/notes.git
DESTINATION_REPO=$2 # 目标仓库地址
IGNORE_FILES=$3 # 忽略文件列表
SOURCE_DIR=$(basename "$SOURCE_REPO") # notes.git

GIT_SSH_COMMAND="ssh -v"

echo "SOURCE=$SOURCE_REPO"
echo "DESTINATION=$DESTINATION_REPO"
echo "IGNORE_FILES=$IGNORE_FILES"

git config --global user.name liucong
git config --global user.email liuconglook@gmail.com

# 拷贝忽略更新的文件
git init ignore && cd ignore
git config core.sparsecheckout true
echo -e "${IGNORE_FILES//,/"\n"}" >> .git/info/sparse-checkout
git remote add origin "$DESTINATION_REPO"
git pull origin master

echo "backups finish."

# 镜像仓库
cd ../

git clone --mirror "$SOURCE_REPO" "$SOURCE_DIR" && cd "$SOURCE_DIR"
git remote set-url --push origin "$DESTINATION_REPO"
git fetch -p origin
# Exclude refs created by GitHub for pull request.
git for-each-ref --format 'delete %(refname)' refs/pull | git update-ref --stdin
git push --mirror

echo "mirror finish."

# 恢复忽略文件
cd ../
rm -r "$SOURCE_DIR"

git clone "$DESTINATION_REPO" "$SOURCE_DIR" && cd "$SOURCE_DIR"

files=(${IGNORE_FILES//,/ })
for file in ${files[@]}
do
   mv "../ignore/$file" "./$file"
done

echo "reset ignore file finish."

git add -A
git commit -am'update'
git push origin master

echo "finish."
~~~

> action.yml

~~~yml
name: 'Pull a repository using SSH'
description: 'Action for mirroring a repository in another location (Bitbucket, GitHub, GitLab, …) using SSH.'
branding:
  icon: 'copy'
  color: 'orange'
inputs:
  source-repo:
    description: 'SSH URL of the source repo.'
    required: true
    default: ''
  destination-repo:
    description: 'SSH URL of the destination repo.'
    required: true
    default: ''
  ignore-files:
    description: 'Ignore updating the file list.'
    required: true
    default: ''
runs:
  using: 'docker'
  image: 'Dockerfile'
  args:
    - ${{ inputs.source-repo }}
    - ${{ inputs.destination-repo }}
    - ${{ inputs.ignore-files }}
~~~

>Dockerfile

~~~dockerfile
# 基于alpine操作系统
FROM alpine
# 安装bash
RUN apk update \
        && apk upgrade \
        && apk add --no-cache bash \
        && rm -rf /var/cache/apk/* \
        && /bin/bash
# 安装 Git和ssh客户端
RUN apk add --no-cache git openssh-client
# 拷贝仓库下shell脚本，到系统根目录
ADD *.sh /
# 执行入口shell
RUN chmod +x entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
~~~

>.github/workflows/config.yml

~~~yml
name: Keep the major version tag up-to-date

on:
  release:
    types: [published, edited]

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: false

jobs:
  update-major-tag:
    runs-on: ubuntu-latest

    steps:
      - uses: Actions-R-Us/actions-tagger@v2
~~~

> 从Github拉取最新代码到Gitee，并自动部署Pages

- 需Github和Gitee都有相同的仓库，且启动Pages
  - Gitee可通过导入Github仓库进行首次同步。
- 在Github仓库Setting设置Secrets，保存Gitee登录所需的秘钥和密码。

- 在仓库下创建：.github/workflows/config.yml

~~~yml
name: Config

on: page_build

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Sync to Gitee
        uses: liuconglook/git-pull-action@master # wearerequired/git-mirror-action@v1.0.1
        env:
          # 注意在 Settings->Secrets 配置 GITEE_RSA_PRIVATE_KEY
          SSH_PRIVATE_KEY: ${{ secrets.GITEE_RSA_PRIVATE_KEY }}
        with:
          # 注意替换为你的 GitHub 源仓库地址
          source-repo: git@github.com:liuconglook/notes.git
          # 注意替换为你的 Gitee 目标仓库地址
          destination-repo: git@gitee.com:cleve/notes.git
          # 你需要忽略更新的文件列表，多个文件用逗号隔开
          ignore-files: index.html

      - name: Build Gitee Pages
        uses: yanglbme/gitee-pages-action@main
        with:
          # 注意替换为你的 Gitee 用户名
          gitee-username: cleve
          # 注意在 Settings->Secrets 配置 GITEE_PASSWORD
          gitee-password: ${{ secrets.GITEE_PASSWORD }}
          # 注意替换为你的 Gitee 仓库，仓库名严格区分大小写，请准确填写，否则会出错
          gitee-repo: notes
          # 要部署的分支，默认是 master，若是其他分支，则需要指定（指定的分支必须存在）
          branch: master
~~~

### WebFlux

响应式的web框架

https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html

https://wiki.jikexueyuan.com/project/reactor-2.0/01.html

需了解：lambda、stream、reactor

响应式编程（reactive programming）是一种基于数据流（data stream）和变化传递（propagation of change）的声明式（declarative）的编程范式。

响应式流(Reactive Streams)通过定义一组实体，接口和互操作方法，给出了实现异步非阻塞**背压**的标准。第三方遵循这个标准来实现具体的解决方案，常见的有Reactor，RxJava，Akka Streams，Ratpack等。

- Publisher（发布者/生产者）
- Subscriber（订阅者/消费者）
- Processor（发布者与订阅者之间处理数据的缓冲区）

https://mercyblitz.github.io/2018/07/25/Reactive-Programming-%E4%B8%80%E7%A7%8D%E6%8A%80%E6%9C%AF-%E5%90%84%E8%87%AA%E8%A1%A8%E8%BF%B0/

> 例子

optional之前，我们需要主动做空值校验，现在则是被动根据校验结果作为出响应。

stream之前，我们需要主动遍历对每个元素进行多个操作，现在则是流式地做出多个操作。

基于HTTP的主动轮询和基于Netty的被动响应。

命令式编程，大多是阻塞的。响应式编程是非阻塞的。

发布者和订阅者；生产者和消费者；观察者和被观察者。

函数式编程，减少模板代码的编写，规范构建条件，降低编写错误。

#### Reactor

Reactor是一个基础库，可用它构建时效性流式数据应用，或者有低延迟和容错性要求的微/纳/皮级服务。

- reactor-core
- reactor-bus
- reactor-streams
- reactor-net

Spring + Reactor-Stream (Core)： 用 Stream 和 Promise 做后台处理。

#### WebFlux

WebFlux默认集成的是Reactor

- 发布者
  - Flux：监听n个元素的异步序列流。onNext、onComplete、onError。
  - Mono：监听一个元素的异步序列流。onComplete、onError。
- 订阅者
  - 由Spring Reactive实现。

##### 调用服务

由于网关gateway使用的是webflux，与传统web调用不一样，需要响应式调用。

下面是gateway结合OpenFeign进行服务间调用。

> pom.xml

~~~xml
<dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
~~~

>Application.java

~~~~java
@SpringBootApplication
@EnableFeignClients // 开启OpenFeign
public class Application {
	...
        
    // 使用WebClient调用Rest服务
    @Bean
    @LoadBalanced // 开启负载均衡
    public WebClient.Builder webClientBuilder(){
        return WebClient.builder();
    }
}
~~~~

> Service/Controller

~~~java
@Service
public class ServiceImpl implements Service{
    
    @Autowired
    private WebClient.Builder webClientBuilder;

    public Result test(){
        return webClientBuilder.build()
                .get()
                .uri("http://service-name/test)
                .retrieve()
                .bodyToMono(Result.class)
                .block();
    }
}
~~~

### MapStruct

java bean映射，例如VO/BO/DTO等，告别BeanUtils.CopyProperties.

使用非常简单，只需配置实体类之间的映射关系即可。

#### 引入

> pom.xml

可配合lombok使用

~~~xml
<properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <org.mapstruct.version>1.4.2.Final</org.mapstruct.version>
</properties>

<dependencies>
        <!-- junit4 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <!-- mapstruct -->
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct</artifactId>
            <version>${org.mapstruct.version}</version>
        </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>${maven.compiler.source}</source>
                <target>${maven.compiler.target}</target>
                <annotationProcessorPaths>
                    <path>
                        <groupId>org.mapstruct</groupId>
                        <artifactId>mapstruct-processor</artifactId>
                        <version>${org.mapstruct.version}</version>
                    </path>
                    <!-- lombok -->
                    <path>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok</artifactId>
                        <version>${org.projectlombok.version}</version>
                    </path>
                    <!-- lombok and mapstruct binding -->
                    <path>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok-mapstruct-binding</artifactId>
                        <version>${lombok-mapstruct-binding.version}</version>
                    </path>
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>
~~~

#### 准备

> 实体类

~~~java
public class Student {
    private String name;
    private Integer age;
    private Gender gender;
    private Address address;
    // all args constructor
    // getter and setter
}
~~~

~~~java
public class Address {
    private String name;
    // all args constructor
    // getter and setter
}
~~~

~~~java
public enum Gender {
    MAN("男"),
    WOMAN("女");
    
    private String name;
    Gender(String name) { this.name = name; }
    public String getName() { return name; }
}
~~~

~~~java
public class Classes {
    private String name;
    // all args constructor
    // getter and setter
}
~~~

> VO

~~~java
public class StudentVO {
    private String name;
    private Integer age;
    private String gender;
    private String address;
    private String classes;
    // all args constructor
    // getter and setter
}
~~~

#### 一对一映射

~~~java
@Mapper
public interface StudentMapper {
	StudentMapper INSTANCE = Mappers.getMapper(StudentMapper.class);

    /**
     * 转VO
     * @param student
     * @return
     */
    @Mapping(target = "address", source = "address.name")
    @Mapping(target = "gender", source = "gender")
    @Mapping(target = "classes", ignore = true)
    StudentVO studentToStudentVO(Student student);
    
    /**
     * VO逆转
     * @param studentVO
     * @return
     */
    @InheritInverseConfiguration(name = "studentToStudentVO")
    Student studentVOToStudent(StudentVO studentVO);
    
    /**
     * 转list
     * @param students
     * @return
     */
    @InheritConfiguration(name = "studentToStudentVO")
    List<StudentVO> studentsToStudentVOs(List<Student> students);
    
    /**
     * 传入参数：source
     * 输出参数：target
     * @param gender
     * @return
     */
    default String getGender(Gender gender) {
        return gender.getName();
    }

    /**
     * 传入参数：source
     * 输出参数：target
     * @param gender
     * @return
     */
    default Gender setGender(String gender) {
        if(Gender.MAN.getName().equals(gender)){
            return Gender.MAN;
        }else {
            return Gender.WOMAN;
        }
    }
}
~~~

> 测试

~~~java
public void testStudentToStudentVO() {
    Student student = new Student("belean", 21, Gender.MAN, new Address("home"), null);

    StudentVO studentVO = StudentMapper.INSTANCE.studentToStudentVO(student);
    studentCompareVO(student, studentVO);
}
public void testStudentVOTOStudent() {
    StudentVO studentVO = new StudentVO("belean", 21, "男", "home", null);

    Student student = StudentMapper.INSTANCE.studentVOToStudent(studentVO);
    assertNotNull(student);
    studentCompareVO(student, studentVO);
}
public void testStudentsToStudentVOs() {
    Student student = new Student("belean", 21, Gender.MAN, new Address("home"));
    Student student2 = new Student("hello", 22, Gender.WOMAN, new Address("my home"));

    List<Student> students = new ArrayList<>();
    students.add(student);
    students.add(student2);

    List<StudentVO> studentVOs = StudentMapper.INSTANCE.studentsToStudentVOs(students);
    assertNotNull(studentVOs);
    assertEquals(2, studentVOs.size());
    studentCompareVO(student, studentVOs.get(0));
    studentCompareVO(student2, studentVOs.get(1));
}
private void studentCompareVO(Student student, StudentVO studentVO) {
    assertNotNull(student);
    assertNotNull(studentVO);
    assertEquals(student.getName(), studentVO.getName());
    assertSame(student.getAge(), studentVO.getAge());
    assertEquals(student.getGender().getName(), studentVO.getGender());
    assertEquals(student.getAddress().getName(), studentVO.getAddress());
}
~~~

> 总结

- @Mapper：注解这个映射接口
  - componentModel = "spring" // 交由Spring容器管理，通过@Autowired注入使用
- StudentMapper INSTANCE = Mappers.getMapper(StudentMapper.class);
  - 通过该方式获取实例，交由spring管理后可省略。
- @Mapping：配置映射关系
  - target：输出参数
  - source：输入参数
  - ignore：忽略对输入参数的映射
- getGender/setGender
  - 由于枚举类型无法通过构造方法创建，所以需要手动转换。
  - setGender是VO逆转才需要的。

#### 原理

mapstruct本质上是通过反射生成Mapper接口对应的实现，从而省去了我们手动编写转换代码的工作。

~~~java
/**
 * 转VO
 * @param student
 * @return
 */
@Mapping(target = "address", source = "address.name")
@Mapping(target = "gender", source = "gender")
@Mapping(target = "classes", ignore = true)
StudentVO studentToStudentVO(Student student);
~~~

> 在target目录下，通过反编译查看StudentMapperImpl.class

~~~java
public StudentVO studentToStudentVO(Student student) {
    if (student == null) {
        return null;
    } else {
        String address = null;
        String gender = null;
        String name = null;
        Integer age = null;
        address = this.studentAddressName(student);
        gender = this.getGender(student.getGender());
        name = student.getName();
        age = student.getAge();
        String classes = null;
        StudentVO studentVO = new StudentVO(name, age, gender, address, (String)classes);
        return studentVO;
    }
}
~~~

#### 多对一映射

~~~java
/**
 * 多对象转VO
 * @param student
 * @param classes
 * @return
 */
@Mapping(target = "name", source = "student.name")
@Mapping(target = "address", source = "student.address.name")
@Mapping(target = "gender", source = "student.gender")
@Mapping(target = "classes", source = "classes.name")
StudentVO studentAndClassesToStudentVO(Student student, Classes classes);
~~~

> 测试

~~~java
public void testStudentAndClassesToStudentVO(){
    Student student = new Student("belean", 21, Gender.MAN, new Address("home"));
    Classes classes = new Classes("六年一班");

    StudentVO studentVO = StudentMapper.INSTANCE.studentAndClassesToStudentVO(student, classes);
    studentCompareVO(student, studentVO);
    assertEquals(classes.getName(), studentVO.getClasses());
}
~~~

#### 总结

除了必要的实体类和转换类之外，只需简单的编写映射所需的Mapper接口即可。

学习以上案例基本上够用了，更多使用案例请参考：https://github.com/mapstruct/mapstruct-examples

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
    - 刚好解决就好，除非有必要，否则不必为未来提前买单。
  - 一次，并且只有一次。
    - 消除重复代码。

- 重构
  - 重构就是在不改变代码行为的前提下，对其进行一系列小的改造。
  - 每次细微改造之后，通过运行单元测试以确保改造没有造成任何破坏，然后再去做下一次改造。
  - 每个改造都微不足道，但所有的这些改造叠加在一起，就形成来对系统设计和架构显著的改进。
  - 重构是我们每隔一个小时或者半个小时就要去做的事情。
  - 通过重构，我们可用持续地保持尽可能干净、简单并且具有表现力的代码。

- 隐喻
  - 隐喻（metaphore）是系统的未来景图。通常可用归结为一个名字系统，这些名字提供了一个系统组成元素的词汇表，并且有助于定义它们之间关系。
  - 例如：以每秒60个字符的速度将文本输出到屏幕的系统。程序将产生的文本放到一个缓冲区，当满了后，则放入磁盘，当缓冲区快空时，再切换回来。用卡车托运垃圾来比喻，缓冲区就是小卡车，屏幕是垃圾场，程序是垃圾制造者。
  - 例如：分析网络流量的系统。每30分钟，系统会轮询许多网络适配器，并从中获取监控数据。每个网络适配器提供一小块由几个单独的变量组成的数据，我们这些数据块为“面包切片”，分析程序“烤制”这些切片，因被称为“烤面包机”，而数据块中的单个变量被称为“面包屑”。

> 计划41

对极限编程中计划游戏部分的描述。

可衡量的计划，是用数字说话。

- 用户素材
  - 通过“点数”来确定实现用户素材的相对时间。
    - 例如：实现8个点的用户素材所需的时间是4个点的用户素材的两倍。
  - 分解大的素材，合并小的素材。
    - 分解和合并用户素材后，应重新估算点数，而不是简单的加减。
  - 确认速度因子，比如2天实现一个素材点，事先可先给出一个猜测值。
    - 随着项目的进展，对于速度的度量会越来越准。
- 发布计划
  - 与客户确认发布时间，通常2~4个月后。
  - 让业务人员来选定哪些会给他们带来最大利益的素材。
    - 挑选本次发布中它们想要实现的素材，并大致确定这些素材的实现顺序。
    - 且不能选择与当前开发速度不符的更多素材。
    - 当速度变得更准确一点时，可用再对发布计划进行调整。
- 迭代计划
  - 与客户决定迭代规模，一般需两周。
  - 客户选择本次迭代中实现的素材。
  - 开发人员可以采用技术意义的顺序来串行地实现，或分摊素材，进行并行开发。
  - 一旦迭代开始，客户就不能再改变迭代期内需要实现的素材。
  - 即使没有完成所有的用户素材，迭代也要在先前指定的日期结束。
    - 根据首次迭代完成的点数，确认下次迭代计划的点数。
- 任务计划
  - 新的迭代开始，开发人员把素材分解成开发任务。
    - 一个任务是一个开发人员能够在4~16小时之内实现的一些功能。
  - 开发人员签订一项任务的时候，也要对任务进行估算。
    - 这样每个人就知道在最近一次的迭代中所完成的任务点数，可作为下次迭代的个人预算。
  - 分发所有任务，或用完所有人预算任务的点数为止。
  - 迭代的中点，需要开一次会，重新调整任务（增加素材或去除优先级较低的素材），以便完成素材，而不是任务。
- 迭代
  - 每次迭代的结束，给客户演示当前可运行的程序。
  - 要求客户对项目程序的外观、感觉和性能进行评价。
  - 客户会以新的用户素材的方式提供反馈。

> 测试

- 测试的好处
  - 无论是增加功能或修改程序，测试都能告诉我们程序是否仍具有正确的行为。
  - 通过调用者的角度去编写程序。
  - 首先测试，就需要把程序设计为可测试的。
    - 而为了易于调用和可测试的，就必须解除软件中的耦合。
  - 测试可用作为一种的文档，里面包含了代码的正确和失败的使用，可用帮助其他程序员了解如何使用代码。
- 测试优先设计
- 测试促使模块之间隔离

> 验收测试

- 单元测试是用来验证系统中个别机制的白盒测试（white-box tests）。
- 验收测试是用来验证系统满足客户需求的黑盒测试（black-box tests）。
- 验收测试由不了解系统内部机制的人编写。
  - 可以是客户或和一些技术人员（QA）一起编写。
- 单元测试是可编译、运行的有关系统内部结构的文档。
- 验收测试是有关系统特性的可编译、执行的文档。

> 重构（55）

PrimeGenerator

>编程实践

保龄球比赛（66）

- 积分规则
  - 共十轮，每轮10个球瓶，有两次投球机会。
  - 每轮的第一次投球，如果击倒10个球瓶，记“全中”。
    - 10分+之后两次投球得分
  - 每轮的第二次投球，如果击倒全部球瓶，记“补中”。
    - 下一轮的第一次击倒的球瓶数+10，作为本轮“补中”得分。
  - 如果直至第二次投球结束，还未击倒全部球瓶，则算“失误”。
    - 两次投球击倒的球瓶数量和作为本轮”失误“得分。

- 简单理解
  - 累计每次投球数，如果上一轮是”全中“或“补中”，则本轮第一次投球数得分翻倍。

> 敏捷设计

- 拙劣设计的症状
  - 僵化性（Rigidity）：设计难以改变。
    - 因为每个改动都会迫使许多对系统其他部分的其他改动。
    - 如果单一的改动会导致依赖关系的模块中的连锁改动，那么设计就是僵化的。
  - 脆弱性（Fragility）：设计易于遭到破坏。
    - 在进行一个改动时，程序的许多地方就可能出现问题。
  - 牢固性（Immobility）：设计难以重用。
    - 系统中包含了其他系统有用的部分，但是要把这些部分从系统中分离出来所需要的努力和风险是巨大的。
  - 粘滞性（Viscosity）：做正确的事情比做错误的事情要困难。
    - 当那些可以保持系统设计的方法比那些生硬手法更难应用时，就表明设计具有高的粘滞性。
  - 不必要的复杂性（Needless Complexity）：过分设计。
    - 如果设计中包含当前没有用的组成部分，它就含有不必要的复杂性。
  - 不必要的重复（Needless Repetition）：设计中包含重复的结构，而该重复的结构本可以使用单一的抽象进行统一。
    - 你永远也不知道这块代码最初编写在哪，又被复制了多少次，一旦这块代码出了问题就得修改多处。
  - 晦涩性 （Opacity）：模块难以理解。
    - 代码应用清晰、富有表现力的方式编写。

- SOLID原则
  - 单一职责原则（The Single Responsiblity Principle，简称SRP）
    - 就一个类而言，应该仅有一个引起它变化的原因。
  - 开放-封闭原则（The Open-Close Principle，简称OCP）
    - 软件实体（类、模块、函数等等）应该是可以扩展的，但是不可修改的。
  - 里氏替换原则（The Liskov Substitution Principle，简称LSP）
    - 子类型必须能够替换掉它们的基类型。
    - 模型的有效性只能通过它的客户端程序来表现。
    - OOD中is-a关系是就行为方式而言的。
    - 可替换性可用通过显式或者隐式的契约来定义。
  - 接口隔离原则（The Interface Segregation Interface，简称ISP）
    - 不应该强迫客户依赖于它们不用的方法。
  - 依赖倒置原则（The Dependency Inversion Principle，简称DIP）
    - 高层模块不应该依赖于底层模块。二者都应该依赖于抽象。
    - 抽象不应该依赖于细节。细节应该依赖于抽象。
    - Hollywood原则：“Don't call us, we'll call you.”（不要调用我们，我们会调用你。）
    - 依赖于抽象
      - 任何变量都不应该持有一个指向具体类的指针或者引用。
      - 任何类都不应该从具体类派生。
      - 任何方法都不应该覆写它的任何基类中的已经实现了的方法。

- 什么是敏捷设计
  - 设计
    - 设计是一个抽象的概念。
      - 它和程序的概括形状、结构以及每一个模块、类和方法的详细形状和结构有关。
      - 可用使用不同的媒介去描述它。
      - 最后，源代码就是设计。
    - 用来描绘源代码的图示只是设计的附属物，而不是设计本身。
  - 软件的腐化
    - 在非敏捷环境中，由于需求没有按照初始设计预见的方式进行变化，从而导致了设计的退化。
    - 敏捷团队依靠变化来获取活力。
      - 团队几乎不进行预先设计。
      - 更愿意保持系统设计尽可能的干净、简单，并使用许多单元测试和验收测试作为支援。
      - 每次迭代结束所生成的系统都具有最合适于那次迭代中需求的设计。

> 设计模式

不存在完美的结构，只存在那些试图去平衡当前的代价和收益的结构。随着时间的过去，这些结构肯定会随着系统需求的改变而改变。管理这种变化的诀窍是尽可能地保持系统简单、灵活。

- 命令（command）模式
  - 将请求（命令）封装为一个对象，这样可以使用不同的请求参数化其他对象（将不同请求依赖注入到其他对象），并且能够支持请求（命令）的排序执行、记录日志、撤销等（附加控制）功能。
  - 事务操作：校验命令。
  - 时间上解耦：校验后，可自动执行。
  - UNDO：撤销，用栈存储命令的执行，出栈后调用对象的undo(()方法可进行撤销。
- 主动对象（active object）模式（==168==）
  - 主动对象模式基于命令模式，是实现多线程控制的一项古老的技术。
  - ActiveObject维护一个Command对象的链表，当调用run()方法时遍历链表执行命令。
- 模板方法（template method）模式
  - 模板方法模式在一个方法中定义一个算法骨架，并将某些步骤推迟到子类中实现。
  - 模板方法模式可以让子类在不改变算法整体结构的情况下，重新定义算法中的某些步骤。
  - 将通用的算法放到一个基类（抽象类），子类继承基类来实现变动的步骤。
- 策略（strategy）模式
  - 定义一组算法类，将每个算法分别封装起来，让他们可以互相替换。
  - 策略模式可以使算法的变化独立于使用他们的客户端（指使用算法的代码）。
  - 编写模板类，利用多态传递接口的实现类，委托接口完成变动的步骤。
- 门面（facade）模式
  - 也叫外观模式，它为子系统提供一组统一的接口，定义一组高层接口让子系统更易用。
  - 为一组具有复杂且全面的接口对象提供一个简单且特定的接口。
  - 上层的管道策略。
- 中介（mediator）模式
  - 下层的管道策略。
- 单例（singleton）模式
  - 私有构造，提供一个静态的实例化对象的方法。
  - 跨平台、延迟创建。
- 单态（monostate）模式
  - 静态成员变量，使得不管多少个对象，其属性都共享。
  - 其方法不是静态的，可派送类。因此，不同的派生类基于同样的属性表现不同的行为。
- 空对象（null object）模式
  - 返回空对象，而不是null，空对象的行为什么也不做。
- 工厂（factory）模式
  - 将对象的创建交由工厂。
- 组合（composite）模式
  - 将一组对象组织成树形结构，以表示一种“部分-整体”的层次结构。
  - 将一对多的关系转换为一对一的关系。
- 观察者（observer）模式
  - 观察者模式也被称为发布订阅模式（Publish-Subscribe Design Pattern）和回归模式。在对象之间定义一个一对多的依赖，当一个对象状态改变的时候，所有依赖的对象都会自动收到通知。
  - 数字时钟（288），观察者模式的演化过程。
- 抽象服务（abstract server）模式
  - 例如MVC三层架构，Controller层不应直接依赖具体的Service，而是依赖其抽象的服务接口。
- 适配器（adapter）模式
  - USB转接头充当适配器，将两种不兼容的接口，通过转接变得可以一起工作。
- 桥接（bridge）模式
  - 一个类存在两个（或多个）独立变化的维度，我们通过组合的方式，让这两个（或多个）维度可以独立进行扩展。
- 代理（proxy）模式
  - 在不改变原始类代码的情况下，通过引入代理类来给原始类附加功能。
- stairway to heaven模式
  - 是另一个可以完成和Proxy模式一样的依赖倒置的模式。
- 访问者（visitor）模式
  - 允许一个或者多个操作应用到一组对象上，解耦操作和对象本身。
  - 访问者（visitor）模式
    - 双重分发，分发accept类型对象，分发visit访问（执行特定函数）。
  - 无环（acyclic visitor）模式
    - accept单独的访问对象。
  - 装饰者（decorator）模式
    - 基于组合，对功能的增强。
  - 扩展对象（extension object）模式
- 有限状态机（finite state machine，FSM）模式
  - 实现方式：嵌套switch语句，state模式。

> 包结构设计（251）

六个设计原则

- 内聚性
  - 重用发布等价原则（REP）：重用的粒度就是发布的粒度。
    - 重用一个类库时，希望这个类库有人维护。
    - 并且有拒绝新版本的权利，或要求对旧版提供一段时间的维护。
  - 共同重用原则（CRP）：一个包中的所有类应该是共同重用的。如果重用了包中的一个类，那么就要重用包中的所有类。
    - 在这样的一个包中，我们会看到类之间的很多的相互依赖。
  - 共同封闭原则（CCP）：包中的所有类对于同一类性质的变化应该是共同封闭的。一个变化若对一个包产生影响，则将对该包中的所有类产生影响，而对其他的包不造成任何影响。
- 耦合性
  - 无环依赖原则（ADP）：在包的依赖关系图中不允许存在环。
    - 每周构建、消除依赖环
    - 依赖倒置原则（DIP）
  - 稳定依赖原则（SDP）：朝着稳定的方向进行依赖。
    - 如果包依赖的包越多就越不稳定。
  - 稳定抽象原则（SAP）：包的抽象程度应该和其稳定程度一致。
    - 稳定的包应该包含抽象类。
    - 不稳定的包应该是具体的，使其内部的具体代码易于修改。

##### 薪水支付案例

第18章（160）

- 钟点工
  - 每天提交工作时间卡。
  - 每天8小时，超出部分按1.5倍工资计算。
  - 每周五支付。
- 月薪
  - 每月的最后一个工作日支付。
- 酬金
  - 每隔一周的周五支付。
  - 根据销售情况，支付一定数量的酬金。
- 支付方式
  - 邮寄支付支票
  - 指定银行账户
- 加入协会
  - 记录每周的服务费用，下个月从薪水中扣除。
- 薪水支付程序
  - 每个工作日运行一次，为相应的雇员进行支付。

> 测试用例

~~~shell
// 增加雇员 H（钟点工）、S（月薪）、C（月薪+酬金）
AddEmp <EmpID> "<name>" "<address>" H <hourly-rate> 
AddEmp <EmpID> "<name>" "<address>" S <monthly-rate> 
AddEmp <EmpID> "<name>" "<address>" C <monthly-rate> <commission-rate>

// 删除雇员
DelEmp <EmpID>

// 登记时间卡(H钟点工)
TimeCard <EmpID> <date> <hours>

// 登记销售凭条(C酬金)
SalesReceipt <EmpID> <date> <amount>

// 登记协会服务费
ServiceCharge <memberID> <amount> <date>

// 更改雇员明细
ChgEmp <EmpID> Name <name> // 雇员名
ChgEmp <EmpID> Address <address> // 雇员地址

ChgEmp <EmpID> Hourly <hourlyRate> // 每小时报酬
ChgEmp <EmpID> Salaried <salary> // 薪水
ChgEmp <EmpID> Commissioned <salary> <rate> // 酬金

ChgEmp <EmpID> Hold // 邮寄支票到收纳人地址
ChgEmp <EmpID> Direct <bank> <account> // 存入指定的银行帐号
ChgEmp <EmpID> Mail <address> // 邮寄支票到指定地址

ChgEmp <EmpID> Member <memberID> Dues <rate> // 加入协会
ChgEmp <EmpID> NoMember <noMember> // 退出协会

// 每日支付
PayDay <date>
~~~

- 设计实现（200）
- java版实现：https://github.com/liuconglook/payroll

##### 气象站案例

第27章（338）

nimbus

weatherStation

- 监控屏
- 调度器
- 大气压传感器
- 温度传感器
- 闹钟

大气压趋势计算：每小时0.06英寸的上升或下降并且在观测的时刻（每3小时进行一次观测）压力的变化合计为0.02英寸活着更多，那么就应该报告一次压力变化（稳定、上升、下降）

> 需求概述

- 使用需求（测量值）
  - 风速和风向
  - 温度
  - 大气压力
  - 相对湿度
  - 风寒指数
  - 露点温度
  - 当前大气压测量值趋势（稳定、上升、下降）。
    - 例如：当前大气压为29.95英寸汞柱（IOM）并且呈下降趋势。
  - 持续显示测量值以及当前的时间和日期。

- 24小时历史数据
  - 温度
  - 大气压力
  - 相对湿度
  - 以曲线图形式展示。
- 用户设置
  - 设置当前时间、日期以及时区
  - 设置显示单位（英制或公制）
- 管理需求
  - 把传感器校对到已知值
  - 复位系统

> 用例

- 用户
  - 观察系统测量的实时天气信息。
  - 查看某个传感器的历史数据。
- 管理者
  - 校正单个传感器、设置时间/日期、设置测量单位以及在需要时复位系统。

- 用例
  - 系统显示当前的温度、大气压力、相对湿度、风速、风向、风寒温度以及大气压趋势。
  - 点击某个传感器，查看其24小时测量值的曲线图。显示当前时间已经前24小时中最高和最低测量值。
  - 可设置单位、日期、时间。
  - 复位气象站，恢复到出厂时的缺省设置。
  - 校对传感器，输入实际值替换掉当前测量值。

> 发布计划

- 发布I
  - 目标
    - 创建一个使大部分应用程序独立于Nimbus硬件平台的架构。
  - 风险
    - Nimbus1.0 API是否可工作在新操作系统上。
    - JVM是否能在嵌入式电路板上使用。
  - 交付
    - 运行着新系统和新版本JVM的硬件。
    - 显示当前的温度以及大气压力测量值。
    - 当大气压力有变化时，系统会通知我们有关压力的上升、下降或者平衡的情况。
    - 每个小时，显示过去24小时的温度和大气压力测试值。（持久化）
    - 每天上午的12点，系统显示前一天的最高和最低的温度以及大气压力。
    - 所有测量值都以公制表示。
- 发布II
  - 目标
    - 增加用户界面。
    - 设置单位、日期时间。
    - 查看温度、大气压力传感器历史数据。
    - 校准温度、大气压力传感器。
  - 风险
    - 液晶屏/触摸屏接口的软件。
  - 交付
    - 可观看的用户界面。
    - 实现目标功能。
- 发布III
  - 目标
    - 监控天气数据
    - 查看湿度传感器历史数据。
    - 复位气象站应用系统。
    - 校准相对湿度、风速、风向、露点传感器。
    - 记录校准日志。
  - 风险
    - 需求变化。
    - 硬件限制（如：内存、CPU等）。
  - 交付
    - 实现目标功能。
    - 可部署运行的软件。

##### ETS案例

教育考试中心（ETS）

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
- 打破依赖
  - 当要测试一个类时，必定要实例化一个类，而实例化一个类时，就有可能会引入依赖的类。
    - 而测试就需要打破这些依赖。
  - 打破依赖的方法就是：尽可能的使用接口或抽象分离实现。
    - 在测试时就可以使用伪对象替换实现了。
    - 由于接口与实现分离，类只依赖其接口，所以对实现的修改不会导致重新构建，这就是解耦。
- 测试驱动开发
  - 编写失败的测试案例。
    - 编写测试（测试预期值），被测试的类还未编写（返回空值）。
  - 对其进行编译。
    - 编写被测试的类。
  - 使其通过。
  - 去除重复的内容。
    - 重构，使其变得简洁、更具表达力。
  - 重复以上步骤。
- 

#### 重构-改善既有代码的设计

##### 实践重构

如果你发现自己要为程序添加一个特性，而代码结构使你无法很方便地达成目的，那就先重构那个程序，使特性的添加比较容易进行，然后再添加特性。

重构第一步：为即将修改的代码建立一组可靠的测试环境。

重构就是以微小的步伐修改程序。如果你犯下错误，很容易便可发现它。

任何一个傻瓜都能写出计算机可以理解的代码，唯有写出人类容易理解的代码，才是优秀的程序员。

> 重构步骤

- 为要修改的代码编写测试用例。
- 小步重构：
  - 拆分大函数，提取函数和变量。
  - 重命名函数和变量，使之易于理解。
  - 考虑将提取出的函数和变量放置在合适的类中。
  - 消除变量。
- 频繁运行测试。

> 重构手法

- Idea
  - 提取方法：Ctrl+Alt+M
  - 提取属性：Ctrl+Alt+F
  - 移动方法：选中方法，F6
  - 重命名：Shift+F6

> 使用多态取代switch-case

策略模式就是利用多态替换不同实现。

这样的好处是，当要添加新的case时，不是去修改代码，而是添加一个策略类。

##### 重构

重构（Refactor）：对软件内部结构的一种调整，目的是在不改变软件可观察行为的前提下，提高其可理解性，降低其修改成本。

重构（Refactoring）：使用一系列重构手法，在不改变软件可观察行为的前提下，调整其结构。

> 重构的目的：

- 改进软件设计。
- 使软件更容易理解。
- 帮助找到bug。
- 提高编程速度。

> 何时重构：

- 随时随地。
  - 整理代码。
- 写重复代码时重构。
  - 事不过三，三则重构。
- 添加功能时重构。
  - 代码的设计无法帮助我轻松添加特性时重构。
- 修补错误时重构。
  - 通常因为代码表达的还不够清晰，所以导致bug的出现。
- 复审代码时重构。
  - 重构可以帮助复审别人的代码。
  - 我不必想象代码应有的样子，而是通过重构一步一步到达。
  - 以获得更高层次的认识，想到更多可能的点子。

> 为何重构有用？

如果没有重构，恶劣的设计会让你的开发速度慢下来。

难以阅读的程序，难以修改；逻辑重复的程序，难以修改；添加新行为时需要修改已有代码的程序，难以修改；带复杂条件逻辑的程序，难以修改。

如果要修补错误，就得先理解软件的工作方式，而重构是理解软件的最快方式。事实证明在实际的工作中，阅读代码的时间远多于写代码的时间。

> 间接层

计算机科学是这样一门学科：它相信所有问题都可以通过增加一个间接层来解决。

间接层往往是一式两份，如果某个对象委托另一对象，后者又委托一对象，程序愈加难以阅读。

好处是

- 允许逻辑共享。
  - 函数可以被不同地方调用。超类中的函数可以被子类共享。
- 分开解释意图和实现。
  - 接口表示意图，其实现类、函数的名称，以及具体实现都可以解释这意图。
- 隔离变化。
  - 一个对象在多出引用，而我只想改变某一处的行为时，只需派生子类，在那一处引用即可。
- 封装条件逻辑。
  - 多态消息，可以灵活而清晰地表达条件逻辑。

> 接口

接口一经发布，一般都不变，也很难改变，因为使用的人很多。即使改变，也不要直接复制新接口的实现，而是让旧的接口调用新的接口，同时标记旧接口为deprecated，声明将在下次某个版本中移除。

> 重构与设计

预先设计可用让开发更加规范和快速，同时也意味着要对原始设计做出修改，其代价都是非常高昂的。

重构可以带来更简单的设计，同时又不损失灵活性，这也降低来设计过程的难度，减轻来设计的压力。

设计像是画图纸，而编码就像施工。重构只需要不断保持简单的设计，更容易适应变化。

> 重构与性能

编写快速软件的秘密：首先写出可调的软件，然后调整它以求获得足够速度。

有三种编写快速软件的方法：

- 时间预算法
  - 预先给系统分配一定资源，监控其执行时间和轨迹，比较预算的时间。
  - 一般适用于性能极高的实时系统。

- 持续关注法
  - 要求程序员在任何时间做任何事情时，都要设法保持系统的高性能。
  - 一般适用于编写高性能框架。
  - 任何修改如果是为了提高性能，通常会使程序难以维护。

- 统计数据法
  - 不对性能特别关注，直接进入性能优化阶段，通常是在开发后期。
  - 通过一个度量工具来监控程序的运行，让它来告诉你程序中哪些地方大量消耗时间和空间。
  - 由于注意力都集中在热点上，较少的工作量便可显现较好的成果。
  - 和重构一样，需要小幅度修改。每一步都需要编译、测试、再次度量，如果没能提高性能，就应该撤销此次修改。

重构可以帮助写出更快的软件。短期来看，重构的确可能使软件变慢，但它使优化阶段的软件性能调整更容易，最终还是会得到好的效果。

##### 代码坏味道

- 重复代码（Duplicated Code）
  - 一但代码出bug，需要同时修复多处。
- 过长函数（Long Method）
  - 函数越长越难理解。
- 过大的类（Large Class）
  - 过多的成员变量，也会导致重复代码。
- 过长参数列（Long Parameter List）
  - 难以理解，不易使用。
- 发散式变化（Divergent Change）
  - 某个类经常因为不同的原因在不同的方向上发生变化。
  - 一个类受多种变化的影响。
- 散弹式修改（Shotgun Surgery）
  - 遇到某种变化，都必须在许多不同的类内做出许多小修改。
  - 一种变化引发多个类相应修改。
- 依恋情结（Feature Envy）
  - 函数对某个类的兴趣高过对自己所处类的兴趣。
- 数据泥团（Data Clumps）
  - 两个类中相同的字段、许多函数的形参相同。
- 基本类型偏执（Primitive Obsession）
  - 不愿使用对象替换基本类型。
- switch惊悚现身（Switch Statements）
  - 使用多态来替换switch。
- 平行继承体系（Parallel Inheritance Hierarchies）
  - 添加一个子类同时，需要在另一个继承体系中也添加一个子类。
- 冗余类（lazy Class）
  - 没用的类、函数。
- 夸夸其谈未来性（Speculative Generality）
  - 做了多余的事，增添了用不到的东西。
- 令人迷惑的临时字段（Temporary Field）
  - 为某种特定情况而设的变量。
  - 拆分语义不同的函数或许更好。
- 过度耦合的消息链（Message Chains）
  - 一个对象请求另一个对象，然后再请求另一个对象的方法。
  - 可用通过一个函数来委托实现，避免过长的消息链调用。
- 中间人（Middle Man）
  - 过度使用委托。
- 狎昵关系（Inappropriate Intimacy）
  - 两个类过分亲密。例如继承体系中的子类。
  - 可用独立出来。
- 异曲同工的类（Alternative Classes with Different Intefaces）
  - 两个函数做同一件事，却有不同的形参。
  -  根据用途重命名，或提取出函数调用。
- 不完美的库类（Incomplete Library Class）
  - 引入外部方法，引入本地扩展。
- 纯稚的数据类（Data Class）
  - 拥有一些字段，以及用于读写这些字段的函数，可能有public字段。
- 被拒绝的遗赠（Refused Bequest）
  - 子类继承超类的函数和数据，却只需要超类的个别函数和数据。
  - 拒绝支持超类接口的子类，应该用委托实现。
- 过多的注释（Comments）
  - 过多的注释代表代码很糟糕，应试着重构，让代码自解释。

##### 测试

编写代码的同时编写测试代码，必要时可用测试驱动开发，使代码变的可测试。

测试你最担心出错的部分。

每当收到bug报告，请先写一个单元测试来暴露bug。

考虑可能出错的边界条件，把测试火力集中在那儿。

##### 重构手法

> 重新组织函数

- Extract Method：提取方法。
  - 将一块代码封装为一个可读的方法。

- Inline Method：内联方法。
  - 将一个函数调用动作替换为该函数本体。也就是撤销提取方法。
- Inline Temp：内联临时变量。
  - 撤销对临时变量的提取。
- Introduce Explaining Variable：引入解释性变量。
  - 声明该变量为final。
- Replace Temp with Query：用查询替换临时变量。
  - 声明临时变量为final，检查是否被赋值一次。
  - 临时变量会驱使你写更长的函数。

- Split Temporary Variable：分解临时变量。
  - 很多地方使用了某个临时变量，临时变量应该职责单一，使用final。
- Separate Query from Modifler：将查询与修改分开。
- Replace Method with Method Object：用方法对象替换方法。
  - 将方法提取到类中。
- Remove Assignments to Parameters：删除对参数的赋值。
  - 声明形参为final，不要对参数赋值。
- Substitute Algorithm：替换算法。
  - 将函数本体替换为另一个算法。

> 对象之间的搬移

- Move Method：搬移方法。
  - 将方法搬移到其他类，旧方法则委托那个类的方法实现或是删除。
- Move Field：搬移属性。
  - 根据职责搬移属性到合适的类。
- Encapsulate Field：封装属性。
  - 将属性变为私有，仅暴露需要的Getter和Setter。
- Extract Calss：提取类。
  - 将属性和函数提取到新的类。
- Inline Class：内联类。
  - 将这个类的所有属性和方法搬移到另一个类中，然后移除原类。
  - 将原类声明为私有的，可帮助捕捉对原类的隐藏引用点。
- Extract Interface：提取接口。
- Hide Delegate：隐藏委托。
  - 在本对象中建立一个方法去调用委托的另一个类的方法。
- Remove Middle Man：移除中间人。
  - 撤销对委托的隐藏，从而可以去除掉这个多余的中间人。

- Introduce Foreign Method：引入外加方法。
  - 提取为服务方法，以后使用就会方便很多，也可以减少重复代码。
- Introduce Local Extention：引入本地扩展。
  - 将多个相同类型的服务方法提取到类，作为工具类使用。
  - 可以使用子类化或包装类实现。

> 重新组织数据

- Self Encapsulate Field：自封装属性。
  - 将其属性封装为方法，并且只以这个方法来访问这个属性。
- Replace Data Value with Object：以对象取代数据值。
  - 例如：Person的address字段可以替换为Address，其字段name表示地址。
- Change Value to Reference：将值对象改为引用对象。
  - 值对象只对当前类有用，而引用对象对所有类共享。
  - 通过工厂方法获取引用对象，而不是创建或传递值对象。
- Change Reference to Value：将引用对象改为值对象。
  - 值对象不可变。
- Replace Constructor with Factory Method：用工厂方法替换构造函数。
  - 将构造函数私有化，提供一个工厂方法。

- Replace Value with Object：用对象替换值。
- Replace Array with Object：将数组替换为对象。
  - 将数组封装成对象使用。
- Replace Magic Number with Symbolic Constant：用常量替换魔术数字。
- Change Unidirectional Association to Bidirectional：将单向关联更改为双向关联。
  - 添加一个反向指针，并使修改函数能够同时更新两条连接。

- Change Bidirectional Association to Unidirectional：将双向关联更改为单向关联。
  - 找到并去除你想去除的指针。
- Duplicate Observed Data：复制“被监视数据”。
  - 有一些领域数据置身于GUI控件中，而领域函数需要访问这些数据。
  - 将该数据复制到一个领域对象中，建立一个Observer模式，用以同步领域对象和GUI对象内的重复数据。
- Encapsulate Collection：封装集合。
  - 例如：Collection、List、Vector等。
- Replace Record with Data Class：用数据类替换记录。
- Replace Type Code with Class：用类替换类型代码。
  - 使用枚举类。
- Replace Type Code with Subclasses：用子类替换类型代码。
  - 使用多态。
- Push Down Method：下移方法。
  - 将方法从父类推到子类。
- Push Down Field：下移字段。
  - 将字段从父类推到子类。
- Replace Type Code with State/Strategy：用状态或策略替换类型代码。
  - 以状态对象替换类型码。
- Replace Subclass with Fields：以字段取代子类。
  - 将对象类型替换为字段，通过工厂方法创建特定类型的类，从而去掉子类。

> 简化条件表达式

- Decompose Conditional：分解条件表达式。
  - 将条件表达式提取为方法调用，使之可读。
- Consolidate Conditional Expression：合并条件表达式。
  - 将共同结果的条件合并起来，并提炼出一个方法进行调用。
- Consolidate Duplicate Conditional Fragments：合并重复的条件片段。
  - 如果每个条件分支上都会执行某段代码，请将这段代码放在条件之外。
- Replace Nested Conditional with Cuard Clauses：用卫语句替换嵌套条件句。
  - 卫语句：要么返回，要么抛出异常。
  - 如果条件符合就直接返回，否则进入下一个条件。
- Remove Control Flag：删除空值标记。
  - 使用break或continue跳出循环。
  - 使用return退出循环并返回标记。
- Introduce Null Object：去除对null值的检验。
  - Null Object模式。
- Replace Conditional with Polymorphism：使用多态替换条件。
- Introduce Assertion：引入断言。
  - 一般编写测试代码时使用，用于检测一定为真的条件。

> 简化函数调用

- Rename Method：重命名方法。
  - 先给方法加上一个注释，然后再将注释变成方法名称。
  - 你的代码首先是为人写的，其次是为计算机写的。

- Add Parameter：添加参数。
- Remove Parameter：删除参数。

- Preserve Whole Object：保持整个对象。
  - 如果参数都来自一个对象，那么就只传递这个对象。
- Introduce Parameter Object：引入参数对象。
  - 将多个参数封装成对象进行传递。
- Replace Parameter with Method：以函数取代参数。
  - 如果函数可以自己取得值，那么就不需要传递参数。
- Replace Parameter with Explicit Method：以明确函数取代参数。
  - 例如Getter、Setter。一个函数处理一种变化。
- Parameterize Method：令函数携带参数。
  - 通过参数来处理多种变化。
- Separate Query from Modifier：将查询函数和修改函数分离。
- Hide Method：隐藏函数。
  - 将函数变为私有的。
- Remove Setting Method：移除设值函数。
  - 当对象创建后不允许修改值时，应去掉设值函数，通过构造初始化。
- Encapsulate Downcast：封装向下转型。
  - 不把向下转型的工作推给用户。
- Replace Exception with Test：以测试取代异常。
  - 先检查后执行，而不是捕捉由执行错误的参数导致的异常。
- Replace Error Code with Exception：以异常取代错误码。

> 处理泛化关系（Generalization）

- Pull Up Field：字段上移。
  - 将重复的字段从子类移到超类。
- Pull Up Method：方法上移。
  - 将重复的方法从子类移到超类。
- Push Down Method：方法下移。
  - 将不通用的方法从超类下移到子类。
- Push Down Field：字段下移。
  - 将不通用的字段从超类下移到子类。
- Pull Up Constructor Body：构造函数本体上移。
  - 子类通过super调用超类构造。
- Form Template Method：塑造模板函数。
  - 模板方法模式。
- Extract Subclass：提炼子类。
  - 将一部分特性移到子类中。方法下移、字段下移。
- Extract Superclass：提炼超类。
  - 为两个或多个类建立一个超类，将相同特性移到超类。方法上移、字段上移、构造函数本体上移 。
- Extract Class：提炼类。
  - 将一部分特性移到一个新类中，本类组合新类使用。
- Extract Interface：提炼接口。
- Collapse Hierarchy：折叠继承体系。
  - 超类与子类之间无太大区别，将它们合为一体。
- Replace Inheritance with Delegation：以委托取代继承。
  - 优先使用组合。
- Replace Delegation with Inheritance：以继承取代委托。

> 大型重构

- Tease Apart Inheritance：梳理并分解继承体系。
  - 一个继承体系承担两个职责。建立两个继承体系，并通过委托关系让其中一个可用调用另一个。
- Convert Procedural Design to Objects：将过程化设计转化为对象设计。
  - 将数据记录变成对象，将大块的行为分为小块，并将行为移入相关对象之中。
- Separate Domain from Presentation：将领域和表述/显示分离。
  - 将领域逻辑分离出来，为它们建立独立的领域类。
- Extract Hierarchy：提炼继承体系。
  - 建立继承体系，以一个子类表示一种特殊情况。

##### 总结

> 重构是什么？

按我的理解，重构就是整理代码、改善设计。

在还没看过《重构》这本书时我一直都认为自己不会编程，用着面向对象语言写着面向过程的代码。直到看到这本书时我才真正意识到重构和面向对象编程风格的魅力，原来软件可以写的如此之美。

在我看来，本书的很多重构手法都是一种编程经验，与实际编程中所感悟的有一定重合，毕竟好的编程经验会被使用，并被记录下来。

https://www.kancloud.cn/sstd521/refactor/194259

#### 奇特的一生

唐·吉诃徳在一次历险中，他把风车当作抗争对象，却无论如何都不明白他的敌人实际上是那看不见的“风”，还有那原本应该隶属于他的、却竟然完全不受他控制、反倒成了他的主人的“他的大脑”。



13



~~~java
// frame同源访问：表示该页面可以在相同域名页面的 frame 中展示。
// https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Frame-Options
X-Frame-Options: sameorigin
// https://docs.spring.io/spring-security/site/docs/5.0.x/reference/html/headers.html#headers-hsts
ServerHttpSecurity.headers().frameOptions().mode(XFrameOptionsServerHttpHeadersWriter.Mode.SAMEORIGIN)
// https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/config/web/server/ServerHttpSecurity.html#headers()
HttpSecurity.headers().frameOptions().sameOrigin()
                .httpStrictTransportSecurity().disable();
~~~

