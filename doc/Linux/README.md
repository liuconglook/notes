# 一、Linux终端命令

## 1、语法

命令 选项 参数

ls/ll # 当前文件夹下内容

pwd # 当前文件夹路径

cd # 切换文件夹

touch # 不存在则创建文件

mkdir # 创建目录

rm # 删除文件 -r 删除包含多级目录

clear # 清屏

## 2、帮助

命令 --help（命令选项）

man 命令（命令手册）

空格 # 下一屏

enter # 下一行

b # 上一屏

f # 下一屏

q # 退出

/word # 搜索字符串

## 3、查看文件/目录

### 3.1、查看子项（ls）

- ls 
  - -a # all所有，包括隐藏文件
  - -l # 详情
  - -h # 人性化，显示文件大小

### 3.2、查看结构（tree）

tree（以树状图显示）

tree -d（只显示目录）

### 3.3、查看文件（cat）

cat a.txt（查看文件内容）

cat -b a.txt（显示行数，不包括回车）

cat -n a.txt（显示行数，包括回车）

### 3.4、分屏查看文件（more）

more a.txt（分屏查看文件内容，按空格继续）

### 3.5、查找并高亮（grep）

grep a a.txt（显示a被高亮显示）

grep -n a a.txt（显示包含a的行和行号）

grep -v a a.txt（显示不包含a的行和行号）

grep -i a a.txt（显示忽略a的大小写）

grep ^a a.txt（行首a高亮显示）

grep a$ a.txt（行尾a高亮显示）

## 4、文件/目录操作

### 4.1、创建文件

touch a.txt（不存在创建否则不创建）

### 4.2、创建目录

mkdir -p a/b/c（递归创建目录）

### 4.3、删除文件/目录

rm a.txt（删除不能恢复）

rm -r a.txt（删除目录）

rm -f a.txt（强制删除，无提示）

### 4.4、复制文件/目录

cp 源文件 目标文件（复制文件）

cp -i 源文件 目标文件（覆盖提示，y/n）

cp -r 源文件 目标文件（复制目录）

### 4.5、移动（mv）

mv 源文件 目标文件

mv -i 源文件 目标文件（覆盖提示，y/n）

### 4.6、切换目录（cd）

cd ~ # 主目录：/home/user

cd . # 当前目录，不变

cd .. # 上级目录

cd - # 在最近两次目录之间切换

### 4.7、重定向（写入文件）

echo aa（输出aa到终端）

echo aa > a.txt（将原本要输出到终端的aa覆盖进a.txt文件中）

echo aa >> a.txt （追加）

## 5、管道（|）

将一个命令的输出可以通过管道作为另一个命令的输入

ls -lha |more（分屏显示目录信息）

## 6、系统命令

### 6.1、关机和重启

shutdown 选项 时间（默认关机，一分钟）init 0

shutdown -c（取消）

shutdown -r（重启）reboot/init 6

shutdown +10（十分钟后关机）

shutdown 20:00（20:00关机）

### 6.2、查看网卡信息

ifconfig（查看网卡信息）

ping ip地址（查看与目标主机的连接）

### 6.3、通配符

- `*`：表任意个字符
- `?`：任意一个字符
- `[]`：匹配字符组的任意一个
  - `[abc]`：匹配a、b、c中任意一个
  - `[a-f]`：匹配a到f范围内的任意一个

### 6.4、文件大小计算单位

字节：B（Byte）：1B = 8bit（位）

千：K（Kibibyte）：1KB = 1024B（千字节）

兆：M（Mebibyte）：1MB = 1024KB（百万字节）

千兆：G（Gigabyte）：1GB = 1024MB（十亿字节、千兆字节）

太：T（terabyte）：1TB = 1024GB（万亿字节、太字节）

拍：P（Petabyte）：1PB = 1024TB（千万亿字节，拍字节）

艾：E（Exabyte）：1EB = 1024PB（百亿亿字节，艾字节）

泽：Z（Zettabyte）：1ZB = 1024EB（十万亿亿字节，泽字节）

尧：Y（Yottabyte）：1YB = 1024ZB（一亿亿亿字节，尧字节）

### 6.7、常见端口号

22：SSH服务器

80：web服务器

443：HTTPS

21：FTP服务器

## 7、权限

### 7.1、文件/目录权限

chmod +rwx a.txt（加权限）

chmod -rwx a.txt（减权限）

chmod -R 755 用户名 文件/目录（递归修改所属文件权限）

三组权限：拥有者、组、其他

- r：4
- w：2
- x：1

7：rwx

6：rw-

5：r-x

4：r--

3：-wx

2：-w-

1：--x

### 7.2、用户

#### 7.2.1、sudo 超级用户

#### 7.2.2、管理用户

> 添加用户

useradd -m -g [组] [用户名]

- -m # 自动建立用户家目录
- -g # 指定组

> 设置密码

passwd [用户名]

> 删除用户

userdel -r [用户名]

> 查看用户信息

cat /etc/passwd | grep [用户名]

#### 7.2.3、查看用户信息

id [用户名] # 查看用户UID和GID信息

who # 查看当前所有登录的用户列表

whoami # 查看当前登录用户的账号名

标题：用户名、密码（x表示加密的密码）、UID（用户表示）、GID（组标识）、用户名、家目录、终端shell。

#### 7.2.4、切换用户

su - [用户名]

exit # 退出当前登录账户

#### 7.2.5、添加组

usermod -g 组 用户名（修改主组）

usermod -G 组 用户名（添加附加组）

例如：usermod -G sudo 用户名（添加root权限组）

#### 7.2.6、Shell

usermod -s /bin/bash 用户名（指定用户使用的shell）

which 命令（查询执行命令所在位置）

### 7.3、创建组

(主组：etc/passwd，与用户同名)

groupadd 组名（添加组到etc/group下）

groupdel 组名（删除组）

### 7.4、修改文件/目录所属组

chgrp -R 组名 文件/目录（递归修改所属组）

### 7.5、修改文件/目录所属用户

chown 用户名 文件/目录（修改所属用户）

## 8、vi（编辑器）

### 8.1、打开和新建文件

vi 文件名（如果存在就打开，否则创建并打开）

vi 文件名 +行（打开并跳转到指定行，当没指定行时跳转到文末）

### 8.2、常用命令

#### 8.2.1、上下左右移动

h（左）、j（下）、k（上）、l（右）

#### 8.2.2、行内移动

按单词移动：`w`（Word）、`b`（Back）

行首：`0`、`^`（不是空白符位置）

行尾：`$`

#### 8.2.3、行数移动

顶部：`gg`（go）

末尾：`G`

指定行：例如`5gg`、`5G`，或者`:5`

#### 8.2.4、屏幕移动

上翻页：`Ctrl+b`（back）

下翻页：`Ctrl+f`（forward）

顶部：`H`（Head）

中间：`M`（Middle）

底部：`L`（Low）

#### 8.2.5、段落移动

上移动：`{`

下移动：`}`

#### 8.2.6、括号切换

切换：`%`+`h/l`（移动光标到左/右括号位置）

#### 8.2.7、标记

标记：`mx`（x范围a-z或A-Z）

定位：`'x`（定位到标记所在位置）

#### 8.2.8、选中文本

可视模式：`v`（View）

可视行模式：`V`

可视块模式：`Ctrl+v`

#### 8.2.9、撤销和恢复撤销

撤销：`u`（undo）

恢复撤销：`Ctrl+r`（redo）

#### 8.2.10、删除

删除光标所在字符，或选中字符：`x`（Cut剪切）

删除移动命令内容：`d+移动命令`

删除行：`dd`（Delete）

删除至行尾：`D`

实用技巧：

- `5x`（删除从光标开始五个字符）
- `5dd`（删除5行）

#### 8.2.11、复制、粘贴

复制：`y+移动命令`（Copy）

复制行：`yy`

粘贴：`p`（Paste）

实用技巧：`5yy`（复制5行）

#### 8.2.12、替换

替换：`r`（命令模式，替换当前字符）

替换模式：`R`（替换光标后的字符）

#### 8.2.13、缩排和重复

增加缩进：`>>`

减少缩进：`<<`

重复上次命令：`.`

#### 8.2.14、查找

查找：`/str`（查找str）

下一个：`n`

上一个：`N`

向后查找当前光标所在单词：`*`

向前查找当前贯标所在单词：`#`

#### 8.2.15、查找替换

全局替换：`:%s/查找/替换/g`

可视区替换：`:s/查找/替换/g`

确认替换：`:%s/查找/替换/gc`（y替换，n不替换，a全部替换，q全部不替换）

### 8.3、三种模式

编辑模式：`i`

末行模式：`:`

可视块模式：`v`

命令模式（退出模式）：ESC

- w：保存
- q：退出
- q!：强行退出，不保存
- wq/x：保存并退出

### 8.3.1、末行命令（末行模式）

`:e .` # 打开内置浏览器，浏览当前目录下的文件

`:n [文件名]` # 新建文件

`:w [文件名]` # 另存为，仍然是编辑当前文件，不会切换文件。

分屏命令

​	:sp 文件名（横向增加分屏）

​	:vsp 文件名（纵向增加分屏）

​	切换分屏（ctrl+w）

w # 切换到下一个窗口

r # 互换窗口

c # 关闭当前窗口，不能关闭最后一个窗口

q # 退出当前窗口，如果是最后一个窗口，则关闭vi

o # 关闭其他窗口

### 8.3.2、插入命令（编辑模式）

i # 当前位置插入

I # 首行插入

a # 当前字符后添加

A # 行末添加

o # 行后添加

O # 行前添加

### 8.3.3、组合案例

`10i*`（输入十个\*)

- ^ # 首行

- v # 可视块

- j # 向下选中

- I # 在首行插入

- 输入#

- ESC返回命令模式

多行注释

进入可视模式v 再i

## 9、SSH

ssh -p 端口 用户名@ip地址

exit退出当前用户的登录

### 9.1、免密登录

ssh-keygen（生成秘钥）

ssh-copy-id 用户名@ip（将公钥拷贝到远程.ssh目录下）

### 9.2、别名

1、在.ssh目录下创建config文件

2、config文件

Host 别名

HostName ip

User 用户名

Port 22

3、ssh 别名（使用）

### 9.3、scp（远程拷贝文件）

scp -P 22 port 源文件 user@ip:目标文件（将本机文件拷贝到远程服务器）

scp -P 22 port user@ip:源文件 目标文件（将远程服务器文件拷贝到本机）

scp -r -P 22 port 源文件 user@ip:目标文件（拷贝目录）

windows下用FileZilla软件进行FTP文件传输协议

## 10、系统信息

### 10.1、时间信息


date 时间

cal 日历（-y选项当前一年的日历）

~~~shell
# 修改系统时间
cp  /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
~~~

### 10.2、磁盘信息


df -h（磁盘剩余空间）

du -h 目录名（目录下的文件大小）

注：-h，以人性化的方式显示。

free

### 10.3、进程信息

- ps aux（进程详细状况）
  - a：所有进程，包括其他用户进程。
  - u：显示详细信息
  - x：显示没有控制终端的进程
- top（动态显示运行中的进程并且排序）
- kill -9 进程代号（强制终止）

### 10.4、查找文件

find [路径] -name "*.py"

### 10.5、软链接（快捷方式）

ln -s 源文件绝对路径 链接文件

### 10.6、硬链接


ln 源文件绝对路径 链接文件

### 10.7、打包压缩

windows ：rar

Mac ：zip

Linux ：tar.gz、tar.bz2

**打包**

tar -cvf 打包文件.tar 被打包文件 ...（打包.tar）

tar -xvf 打包文件.tar（解包）

**gzip压缩**

tar -zcvf 压缩文件.tar.gz 被压缩文件 ...（gzip压缩）

tar -zxvf 压缩文件.tar.gz -C 目标路径（解压到指定路径，该路径必须存在）

**bzip2压缩**

tar -jcvf 压缩文件.tar.bz2 被压缩文件 ...（bzip2压缩）

tar -jxvf 压缩文件.tar.bz2 -C 目标路径（解压到指定路径，该路径必须存在）

**rar压缩**

unrar e 压缩文件.rar

- c # 生成档案文件，创建打包文件。
- x # 解开档案文件
- v # 列出归档解挡的详细过程，显示进度
- f # 指定档案文件名称，该选项放最后

### 10.8、软件安装

htop（一款比较漂亮的查看当前进程的软件）

sudo apt install 软件包（安装）

sudo apt remove 软件名（卸载）

sudo apt upgrade（更新）

## yum

~~~bash
# 查看已安装的软件
yum list installed
# 筛选查看
yum list tomcat
# 安装软件
yum install nginx
# 卸载软件
yum remove nginx
# 查看软件包依赖
yum deplist nginx
# 全部同意
yum -y install nginx
# 查看软件包描述信息
yum info nginx
# 升级所有软件包
yum update
# 升级某一个软件包
yum update nginx
# 检查更新
yum check-update
# yumex可视化图形界面
yum install yumex
~~~

## curl

~~~bash
# 安装curl
yum -y install curl
# 查看版本
curl -V
# 测试连接
curl www.baidu.com
~~~

## 自启动配置

以`testjar.jar`为例

~~~bash
vim /etc/systemd/system/testjar.service
~~~

~~~c
# 1、创建启动文件
#表示基础信息
[Unit]
#描述
Description=testjar
#在哪个服务之后启动
After=network-online.target
#需要哪些服务
Wants=network-online.target

#表示服务信息
[Service]
#PIDFile=/var/run/testjar.pid
#User=workspace
#Group=workspace
#WorkingDirectory=/home/workspace/testjar
ExecStart=/usr/bin/java -server -Xms256m -Xmx256m -jar /root/testjar.jar
#ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
#PrivateTmp=true
StandOutput=syslog
StandError=inherit
#安装相关信息
[Install]
WantedBy=multi-user.target
~~~

~~~bash
# 2、创建软连接，等同于开启自启的命令systemctl enable testjar
ln -s /etc/systemd/system/testjar.service /etc/systemd/system/multi-user.target.wants/testjar.service
~~~

~~~bash
# 3、刷新配置
systemctl daemon-reload
# 4、启动服务
systemctl start testjar
# 重启服务
systemctl restart testjar
# 停止服务
systemctl stop testjar
# 开机自启
systemctl enable testjar
# 禁止开机自启
systemctl disable testjar
~~~

- `service`命令对应`/etc/init.d`目录

- `systemctl`命令对应`/etc/systemd`目录
- `systemctl`兼容service，即也会去`/etc/init.d`目录下，查看，执行相关程序。
- `systemd`是Linux系统最新的初始化系统(init),作用是提高系统的启动速度，尽可能启动较少的进程，尽可能更多进程并发启动。

## nginx

~~~bash
# 查看版本
nginx -V
# 开启nginx
start nginx
# 快速关闭nginx
nginx -s quit
# 正常关闭
nginx -s stop
# 检查配置
nginx -t
# 重新加载
nginx -s reload
# windows查看端口是否开启
netstat -ano | findstr 0.0.0.0:8081
# windows关闭进程
taskkill /pid [pid]
~~~

~~~c
# wss配置
location /chat/ {
    proxy_pass http://backend;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    # 默认60s断开连接
    proxy_read_timeout 60s;；
}
~~~

## 查看端口

~~~bash
# 查看端口
netstat -tunlp | grep 27017
# 结束进程
kill -9 PID
# 查看pid相关的文件
ll proc/PID
# cwd符号链接的是进程运行目录；
# exe符号连接就是执行程序的绝对路径；
# cmdline就是程序运行时输入的命令行命令；
# environ记录了进程运行时的环境变量；

# Windows
# 查看端口 对应 PID
netstat -ano | findstr "8081"
# 查看PID 对应 进程
tasklist | findstr "3764"
# 结束进程 /T进程及子进程 /F强制结束 /PID
taskkill /T /F /PID 3764
~~~

## 安装JDK

卸载

~~~bash
# 查看jdk
rpm -qa | grep java
# 卸载删除，带noarch不用删除
rpm -e --nodeps java-1.4.2-gcj-compat-1.4.2.0-40jpp.115
~~~

安装

下载：https://mirrors.tuna.tsinghua.edu.cn/AdoptOpenJDK/

~~~bash
# 1、解压到/usr/java目录
# 修改/etc/profile文件
export JAVA_HOME="/usr/java/jdk-9.0.4"
# java8
export PATH="$PATH:$JAVA_HOME/bin"
export CLASSPATH=".:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar"
# java9
export PATH="$PATH:$JAVA_HOME/bin"
export CLASSPATH=".:$JAVA_HOME/lib"
# 2、生效配置
source /etc/profile
~~~

RPM方式安装

~~~shell
# 1、下载安装rpm安装：https://mirrors.tuna.tsinghua.edu.cn/AdoptOpenJDK/rpm
# 2、安装
rpm -ivh

# 1、查看安装的JDK
rpm -qa | grep jdk
# 2、复制刚安装的jdk包名，执行命令
rpm -ql jdk包名 | grep bin
~~~



## Tomcat

/etc/systemd/system/tomcat8.5.service

~~~c
[Unit]
Description=tomcat8.5
After=syslog.target network.target

[Service]
Type=forking
Environment='JAVA_HOME=/usr/java/jdk-9.0.4'
#Environment='CATALINA_PID=/opt/apache-tomcat-8.5.57/tomcat.pid'
Environment='CATALINA_HOME=/opt/apache-tomcat-8.5.57'
Environment='CATALINA_BASE=/opt/apache-tomcat-8.5.57'
#Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'
ExecStart=/opt/apache-tomcat-8.5.57/bin/catalina.sh start
ExecStop=/opt/apache-tomcat-8.5.57/bin/catalina.sh stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target
~~~

#### 设置用户名密码

/conf/tomcat-user.xml

~~~xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<user username="tomcat" password="tomcat" roles="manager-status,manager-gui,manager-script,manager-jmx"/>
~~~

## 安装Redis

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

## Mysql5

下载：https://downloads.mysql.com/archives/community/，。

~~~bash
# 1、放到/opt目录
# 2、解压
# 3、移动到/usr/local目录
mv mysqlxxxx /usr/local/mysql
# 4、创建用户和用户组
cat /etc/group | grep mysql # 查看mysql组
cat /etc/passwd | grep mysql # 查看mysql用户
groupadd mysql # 添加分组
useradd -r -g mysql mysql # 添加用户密码 -r禁止登陆 -g添加到组
# 5、创建data文件夹
mkdir {/usr/local/mysql/data,/usr/local/mysql/logs,/usr/local/mysql/tmp}
# 6、授权目录和用户
cd /usr/local/mysql
chown -R mysql:mysql data # 授权data、log、tmp
# 可选
chgrp -R mysql . # mysql组所有
chown -R root  . # 将文件root权限
chown -R mysql data # 将data目录mysql权限 
# 7、初始化配置文件
配置/etc/my.cnf文件见下方
# 安装libaio
yum install -y libaio
# 8、运行
/usr/local/mysql/bin/mysqld --initialize --user=mysql --datadir=/usr/local/mysql/data/ --basedir=/usr/local/mysql/
# 9、配置环境
export MYSQL_HOME=/usr/local/mysql
export PATH="$PATH:$MYSQL_HOME/bin"
# 10、记录初始密码
mysqld # 启动服务
# 修改初始密码
# 11、开启远程连接
mysql -uroot -proot
use mysql;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
FLUSH PRIVILEGES;
SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
exit;
systemctl restart mysql
# 可选
# 查找文件
find / -name mysql.sock
# 查找mysql进程
ps -ef|grep mysql | grep -v grep
~~~

~~~
# /etc/systemd/system/mysql.service
[Unit]
Description=MySQL
After=network.target

[Install]
WantedBy=multi-user.target

[Service]
User=mysql
Group=mysql
LimitNOFILE = 5000
PIDFile=/usr/local/mysql/tmp/mysql.pid
ExecStart=/usr/local/mysql/bin/mysqld --pid-file=/usr/local/mysql/tmp/mysql.pid $MYSQLD_OPTS

[Install]
WantedBy=multi-user.target
~~~

~~~
# /etc/my.cnf
[client]
port=3306
socket=/usr/local/mysql/tmp/mysql.sock

[mysql]
prompt="\u@mysqldb \R:\m:\s [\d]> "
no-auto-rehash

[mysqld]
sql_mode=ONLY_FULL_GROUP_BY,NO_AUTO_VALUE_ON_ZERO,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,PIPES_AS_CONCAT,ANSI_QUOTES
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
socket=/usr/local/mysql/tmp/mysql.sock
log-error=/usr/local/mysql/logs/mysql.log
pid-file=/usr/local/mysql/tmp/mysql.pid
user=mysql
port=3306
character-set-server=utf8mb4
explicit_defaults_for_timestamp=true
skip_name_resolve=1
default-time_zone = '+8:00'
~~~

## Dig

Dig是一个在类Unix命令行模式下查询DNS包括NS记录，A记录，MX记录等相关信息的工具。

~~~
# 重启网络
service network restart
# 安装dig
yum install bind-utils
# 验证DNS服务
dig www.domain.com +short
~~~

## Jenkins

持续集成服务器

~~~shell
# 1、下载：https://mirrors.tuna.tsinghua.edu.cn/jenkins/redhat/?C=N&O=D
# 2、安装
rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
rpm -ivh jenkins-2.268-1.1.noarch.rpm
# 3、配置jenkins
vim /etc/sysconfig/jenkins
# 修改用户名、端口
JENKEINS_USER="root"
JENKEINS_PORT="8888"
# 4、启动服务
systemctl start jenkins
~~~

~~~shell
# 浏览器访问http://localhost:8888
# 查看密码登录ca11b4308af840c98e73a6887d248cf2
cat /var/lib/jenkins/secrets/initialAdminPassword
~~~

#### 卸载

~~~shell
# 1、卸载
rpm -e jenkins
# 2、检查是否卸载成功：
rpm -ql jenkins
# 3、彻底删除残留文件：
find / -iname jenkins | xargs -n 1000 rm -rf
~~~

#### 报错解决

/usr/bin/java: No such file or directory

注意只支持8/11的JDK

~~~shell
# 搜索/usr/bin/java修改为/usr/java/jdk8/bin/java
vim /etc/rc.d/init.d/jenkins
# 刷新配置
systemctl daemon-reload
# 启动服务
systemctl start jenkins
~~~

#### 构建项目

~~~
# 清除、打包、构建docker镜像
clean package docker:build -DpushImage
~~~





## Maven

下载：https://maven.apache.org/download.cgi

~~~shell
# 解压
tar zxvf apache-maven-3.6.3-bin.tar.gz
# 移动
mv apache-maven-3.6.3 /usr/local/maven
# 修改配置<localRepository>/usr/local/repository</localRepository>
vim /usr/local/maven/conf/settings.xml

~~~

## CentOS8

### 运行模式

~~~bash
# 查看默认的运行模式
systemctl get-default

# 切换为命令行模式
systemctl set-default multi-user.target

# 切换为图形模式
systemctl set-default graphical.target
~~~

### 静态IP

~~~bash
# 1、编辑网卡配置文件，修改IPADDR为192.168.1.105
vim /etc/sysconfig/network-scripts/ifcfg-ens160
# IPADDR IP地址
# GETEWAY 网关
# NETMASK 虚拟网段
BOOTPROTO=static
IPADDR=192.168.1.105
GETEWAY=192.168.1.1
NETMASK=255.255.255.0


# 2、重启网卡
nmcli c reload ens160

# 查看状态
nmcli c up ens160

# 查看DNS
cat /etc/resolv.conf

# 重新分配额外的dhcp地址
dhclient

# 安装telnet
yum -y install telnet*

telnet nps.liuconglook.cn 8084


# SELinux或Security-Enhanced Linux是提供访问控制安全策略的机制或安全模块。 简而言之，它是一项功能或服务，用于将用户限制为系统管理员设置的某些政策和规则。
# 查看SELinux状态
sestatus
# 暂时禁用，下次重启失效
setenforce 0
# 或
setenforce Permissive

# 永久禁用或启用SELINUX=disabled/enforcing
# 修改后重启
vim /etc/selinux/config

# nmcli
nmcli
# 开启nm管理
nmcli n on
# 查看
nmcli d
~~~

### 设置时区

~~~bash
# 模糊查询时区
timedatectl list-timezones | grep -i shang
# 设置时区
timedatectl set-timezone Asia/Shanghai
# 查看当前时间
date
~~~



### 防火墙

~~~bash
# 进程与状态相关
# 启动防火墙
systemctl start firewalld.service
# 停止防火墙 
systemctl stop firewalld.service 
# 查看防火墙状态
systemctl status firewalld
# 设置防火墙随系统启动
systemctl enable firewalld
# 禁止防火墙随系统启动
systemctl disable firewalld
# 查看防火墙状态  
firewall-cmd --state
# 更新防火墙规则
firewall-cmd --reload   
#查看所有打开的端口
firewall-cmd --list-ports
#查看所有允许的服务
firewall-cmd --list-services
# 获取所有支持的服务
firewall-cmd --get-services

# 区域相关
# 查看所有区域信息  
firewall-cmd --list-all-zones
# 查看活动区域信息  
firewall-cmd --get-active-zones
# 设置public为默认区域  
firewall-cmd --set-default-zone=public

# 查看默认区域信息  
# 接口相关
firewall-cmd --get-default-zone
# 将接口eth0加入区域public
firewall-cmd --zone=public --add-interface=eth0
# 从区域public中删除接口eth0  
firewall-cmd --zone=public --remove-interface=eth0
# 修改接口eth0所属区域为default
firewall-cmd --zone=default --change-interface=eth0
# 查看接口eth0所属区域
firewall-cmd --get-zone-of-interface=eth0

# 端口控制
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 永久添加8080端口例外(全局)
firewall-cmd --add-port=8080/tcp --permanent
# 永久删除8080端口例外(全局)
firewall-cmd --remove-port=8800/tcp --permanent
# 永久添加8080端口例外(区域public)
firewall-cmd  --zone=public --add-port=8080/tcp --permanent
# 永久删除8080端口例外(区域public)
firewall-cmd  --zone=public --remove-port=8080/tcp --permanent
# 永久增加65001-65010例外(区域public) 
firewall-cmd  --zone=public --add-port=65001-65010/tcp --permanent


# 添加端口
firewall-cmd --zone=public --add-port=5051/tcp --permanent
# 生效配置
firewall-cmd --reload   
# 查看开放端口
firewall-cmd --list-ports


~~~



### 主机名

~~~bash
# 查看主机名
hostname
hostnamectl

# 设置主机名
hostnamectl set-hostname belean

# 配置域名对应ip
vim /etc/hosts
192.168.1.105 belean
~~~

#### yum

~~~bash
# 备份
cd /etc/yum.repos.d
zip CentOS-Base.repo.zip CentOS-Base.repo CentOS-AppStream.repo

# 删除
rm Centos-Base.repo
rm CentOS-AppStream.repo

# 下载 阿里云centos源
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo


# 清理缓存
yum clean all
~~~

### Podman

~~~bash
# dnf缓存
dnf makecache
# 安装容器工具模块
dnf install -y @container-tools

# 查看版本
podman version

# 搜索官方镜像
podman search alpine --filter is-official=true

# 拉取镜像
podman pull xxx

# 查看镜像
podman images

# 删除镜像
podman rmi xxx

# 查询镜像详情
podman inspect xxx

# 创建容器并运行
podman run -it --rm xxx /bin/bash

# 后台运行--no-healthcheck
podman run -d xxx

# 进入容器
podman exec -it gitlab /bin/bash

# 查看容器
podman ps -a

# 删除一个容器
podman container rm xxx
# 删除所有容器
podman container rm ${podman ps -a -q}

# 导出容器
podman save
# 导入容器
podman load
~~~

#### 设置镜像源

vim /etc/containers/registries.conf

~~~bash
# 使用以下配置的仓库顺序去获取
unqualified-search-registries = ["docker.io", "registry.access.redhat.com"]
 
# 配置仓库的地址
[[registry]]
prefix = "docker.io"
location = "docker.io"

[[registry]]
prefix = "k8s.gcr.io"
location = "registry.aliyuncs.com/google_containers"

[[registry.mirror]]
location = "docker.mirrors.ustc.edu.cn"
[[registry.mirror]]
location = "registry.docker-cn.com"
~~~

#### Gitlab

~~~bash
podman pull docker.io/gitlab/gitlab-ce

podman run -d \
  --no-healthcheck \
  -p 4433:443 -p 8888:80 -p 222:22 -p 5051:5050 \
  --name gitlab \
  --restart always \
  --privileged \
  -v /home/gitlab/etc:/etc/gitlab \
  -v /home/gitlab/logs:/var/log \
  -v /home/gitlab/data:/var/opt/gitlab \
  docker.io/gitlab/gitlab-ce
  
# 账号:root 初始密码:IT8RXXpSq1vK43KCsQyiBEzHEP1MNdbnUB/TCVhkvO8=
podman exec -it gitlab /bin/bash
cat /etc/gitlab/initial_root_password



# 设置clone地址
vim /home/gitlab/etc/gitlab.rb
# 设置https：
external_url: "gitlab.liuconglook.cn"
# 设置ssh
gitlab_rails['gitlab_ssh_host'] = 'gitlab.liuconglook.cn'
# 禁用nginx
nginx['enable'] = false
# 查看
vim /home/gitlab/data/gitlab-rails/etc/gitlab.yml

# 开机自启
# /usr/lib/systemd/system/podman_gitlab.service
[Unit]
Description=Podman Gitlab Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/podman start -a gitlab
ExecStop=/usr/bin/podman stop -t 20 gitlab
Restart=always

[Install]
WantedBy=multi-user.target
# systemctl 激活开机自启
systemctl enable podman_gitlab.service
# 设置开启自启，并立即启动
systemctl enable --now podman_gitlab.service

~~~

#### nohub

~~~bash
yum install coreutils

~~~



## NSSM

下载：https://nssm.cc/download

windows服务（管理员方式运行）

~~~bash
# 注册服务
nssm install


# 启动服务
nssm start <servicename>
# 停止服务：
nssm stop <servicename>
# 重启服务：
nssm restart <servicename>
# 查看状态：
nssm status <servicename>
# 滚动输出：
nssm rotate <servicename>

# 删除服务
nssm remove <servicename>
~~~























