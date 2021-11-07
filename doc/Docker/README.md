## Docker

### 简介

远程仓库https://hub.docker.com/

#### 虚拟化

​		虚拟化（英语：Virtualization）是一种资源管理技术，一般所指的虚拟化资源包括计算能力和资料存储，虚拟部份是不受现有资源的架设方式，地域或物理组态所限制。

虚拟化技术种类很多，例如：软件虚拟化、硬件虚拟化、内存虚拟化、网络虚拟化(vip)、桌面虚拟化、服务虚拟化、虚拟机等等。

#### Docker

​		Docker 是一个开源项目，诞生于 2013 年初，最初是 dotCloud 公司内部的一个业余项目。它基于 Google 公司推出的 Go 语言实现。 项目后来加入了 Linux 基金会，遵从了 Apache 2.0 协议，项目代码在 [GitHub](https://github.com/docker/docker) 上进行维护。

Docker 项目的目标是实现轻量级的操作系统虚拟化解决方案。 Docker 的基础是 Linux 容器（LXC）等技术。

Docker依赖于“写时复制”（copy-on-write）模型。

#### 镜像与容器

用户基于镜像来运行自己的容器。容器基于镜像启动，一旦容器启动完成后，我们就可以登录到容器中安装自己需要的软件或者服务。

镜像就相当于容器的“源代码”。而容器就相当于集装箱，可以放任何东西，不管是web服务器，还是数据库，或者是应用程序服务器什么的。

### 安装

~~~shell
# 1、更新包
yum update
# 2、安装软件包
# utils提供yum-config-manager功能
# device-mapper-persistent-data、lvm2是devicemapper驱动的依赖
yum install -y yum-utils device-mapper-persistent-data lvm2
# 3、配置下载源
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 4、安装docker社区版
yum install docker-ce
# 5、查看版本
docker -v
# 6、设置ustc镜像，ustc是老牌的linux镜像服务提供者
vim /etc/docker/daemon.json
{
	"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
# 6、启动docker服务
systemctl start docker
~~~

### 常用命令

#### 基本

~~~shell
# 1、docker概要信息
docker info
# 2、查看帮助
docker --help
~~~

#### 镜像

~~~shell
# 1、查看镜像(REPOSITORY：镜像名称，TAG：镜像标签，IMAGE ID：镜像ID，CREATED：镜像的创建日期（不是获取该镜像的日期），SIZE：镜像大小)
docker images
# 2、搜索镜像(NAME：仓库名称，DESCRIPTION：镜像描述，STARS：用户评价，反应一个镜像的受欢迎程度，OFFICIAL：是否官方，AUTOMATED：自动构建，表示该镜像由Docker Hub自动构建流程创建的)# 6、拉取镜像
docker search [镜像名称]
# 3、拉取镜像
docker pull [镜像名称:标签]
# 4、删除镜像（需要先删除容器）
docker rmi [镜像id]
# 删除所有
docker rmi `docker images -q`
~~~

#### 容器

~~~shell
# 1、查看容器,-a所有、-l最后一次运行的容器
docker ps [-a][-l] # 没有参数则默认是查看运行中的容器。
docker ps -f status=exited # 查看停止的容器
# 2、创建容器，-i运行容器、-t启动后进入命令行、--name容器名、-v目录映射、-d守护式后台运行、-p端口映射
docker run
# 交互式创建容器（进入命令行）
docker run -it --name=容器名 [镜像名称:标签] /bin/bash
# 守护式创建容器（后台运行）
docker run -di --name=容器名称 [镜像名称:标签]

# 3、删除容器
docker rm 容器名称

# 4、登录/退出
# 登录守护式容器
docker exec -it 容器名称(或容器ID) /bin/bash
# 退出当前容器
exit

# 5、停止/启动
# 停止容器
docker stop 容器名称(或容器ID)
# 启动容器
docker start 容器名称(或容器ID)

# 6、重命名容器
docker rename 原容器名 新容器名
~~~

#### 拷贝

~~~shell
# 文件拷贝（不管容器是否启动都可以拷贝）
docker cp 文件或目录 容器名称:容器目录
docker cp 容器名称:容器目录 文件或目录
~~~

#### 目录挂载

~~~shell
# 操作宿主机目录就相当于操作容器目录，反过来同理
docker run -di --name=容器名称 -v /usr/local/myhtml:/usr/local/myhtml centos:7
~~~

#### 查看容器ip

~~~shell
# 查看容器信息
docker inspect 容器名称
# 查看容器ip
docker inspect --format='{{.NetworkSettings.IPAddress}}' 容器名称
~~~

#### 迁移与备份

~~~shell
# 1、将容器保存为镜像（copy镜像及容器）
docker commit mynginx mynginx_i
# 2、镜像备份保存为tar文件
docker save -o mynginx.tar mynginx_i
# 3、镜像恢复与迁移
docker load -i mynginx.tar
~~~

### Dockerfile

#### 简介

Dockerfile是由一系列命令和参数构成的脚本，这些命令应用于基础镜像并最终创建一个新的镜像。

- 对于开发人员：可以为开发团队提供一个**完全一致的开发环境**； 
- 对于测试人员：可以直接拿开发时所构建的镜像或者通过Dockerfile文件构建一个新的镜像开始工作了； 
- 对于运维人员：在部署时，可以实现应用的**无缝移植**。

#### 使用脚本构建镜像

~~~shell
# 1、创建目录
mkdir -p /usr/local/dockerjdk8/
mv jdk-8u171-linux-x64.tar.gz /usr/local/dockerjdk8/
# 2、创建脚本文件
cd /usr/local/dockerjdk8/
vim Dockerfile
~~~

~~~c
FROM centos:7 # 基础镜像
MAINTAINER belean # 声明创建者
WORKDIR /usr # 工作目录
RUN mkdir /usr/local/java # 创建目录，命令可以写多条
ADD jdk-8u171-linux-x64.tar.gz /usr/local/java/ # 添加文件并解压到容器，使用COPY则不会解压

# 环境变量
ENV JAVA_HOME /usr/local/java/jdk1.8.0_171
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH $JAVA_HOME/bin/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
ENV PATH $JAVA_HOME/bin:$PATH
~~~

~~~shell
# 3、执行命令构建镜像
docker builder -t='jdk1.8' .
# 4、查看镜像
docker images
~~~

### 私有仓库

#### 创建私有仓库

~~~shell
# 1、拉取仓库镜像
docker pull registry
# 2、启动私有仓库容器
docker run -di --name=registry -p 5000:5000 registry
# 3、访问仓库{"repositories":[]}
http://127.0.0.1:5000/v2/_catalog
# 4、修改vi /etc/docker/daemon.json，添加私有仓库
{"insecure-registries":["127.0.0.1:5000"]}
# 5、重启docker
systemctl restart docker
~~~

#### 上传和拉取

~~~shell
# 标记镜像为私有仓库镜像
docker tag jdk1.8 127.0.0.1:5000/jdk1.8
# 启动私有仓库
docker start registry
# 上传镜像到私有仓库
docker push 127.0.0.1:5000/jdk1.8
# 拉取私有仓库的镜像
docker pull 127.0.0.1:5000/jdk1.8
~~~

### 应用部署

#### mysql57

~~~shell
# 拉取myslq5.7镜像
docker pull centos/mysql-57-centos7
# 创建容器
docker run -di --name=容器名 -p 宿主端口:容器端口 -e MYSQL_ROOT_PASSWORD=密码 镜像名
docker run -di --name=tensquare_mysql -p 33306:3306 -e MYSQL_ROOT_PASSWORD=123456 centos/mysql-57-centos7
~~~

#### tomcat

~~~shell
# 拉取tomcat8.5镜像
docker pull tomcat:8.5-jdk8-adoptopenjdk-hotspot
# 创建容器
docker run -di --name=mytomcat -p 9000:8080 -v /usr/local/webapps:/usr/local/tomcat/webapps tomcat:7-jre7
~~~

#### nginx

~~~shell
# 拉取nginx镜像
docker pull nginx
# 创建容器
docker run -di --name=nginx -p 80:80 -p 443:443 -v /opt/nginx:/etc/nginx nginx
# 进入容器
docker exec -it nginx /bin/bash
# 主配置文件/etc/nginx/nginx.conf，从配置server：/etc/nginx/conf.d/*.conf
apt-get update
apt-get install vim
~~~

#### redis

~~~shell
# 拉取redis镜像
docker pull redis
# 创建容器
docker run -di --name=myredis -p 6379:6379 redis
~~~

#### mongo

~~~shell
# 拉取mongodb镜像
docker pull mongo
# 创建容器
docker run -di --name=mongo -p 2717:27017 mongo
~~~

#### RabbitMQ

~~~shell
# 拉取rabiitMQ镜像
docker pull rabbitmq
# 创建容器
docker run -id --name=tensquare_rabbit -p 5501:5671 -p 5502:5672 -p 5503:4369 -p 5504:15672 -p 5505:25672 rabbitmq:management
~~~

#### gogs

私有仓库

~~~shell
# 拉取gogs镜像
docker pull gogs/gogs
# 创建容器
docker run -d --name=gogs -p 10022:22 -p 3001:3000 -v /var/gogsdata:/data gogs/gogs
~~~

用户名密码：liuconglook@gmail.com

#### 配置Maven

~~~shell
vim /usr/lib/systemd/system/docker.service
在ExecStart后添加
-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock

~~~

### 问题汇总

#### 创建容器报错

WARNING: IPv4 forwarding is disabled. Networking will not work.

解决：

~~~shell
vim /etc/sysctl.conf # 或者vi /usr/lib/sysctl.d/00-system.conf
# 添加如下代码：
net.ipv4.ip_forward=1

# 重启network服务
systemctl restart network

# 查看是否修改成功，如果返回为“ net.ipv4.ip_forward = 1 ”则表示成功了
sysctl net.ipv4.ip_forward
~~~

#### 同步容器时间

~~~shell
docker cp /etc/localtime 【容器ID或者NAME】:/etc/localtime
docker cp -L /usr/share/zoneinfo/Asia/Shanghai 【容器ID或者NAME】:/etc/localtime
~~~

































