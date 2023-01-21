## 建站笔记

### 一、云产品

#### 1、域名

> DNS

DNS域名服务器，存储着域名与IP映射关系的数据，同时负责域名的解析工作。

> 记录

记录域名与谁（ip、域名）的映射关系

- A记录：配置域名与IP的映射
- CNAME记录：配置域名与域名的映射

> TTL

TTL（Time To Live）生存时间，单位是秒；域名服务商通常会将域名解析的结果保留一段时间，当更新域名配置时，域名服务商解析访问不了或经过设置的TTL周期时，会递归往上查找。

#### 2、云服务器

> 服务器地址

内网IP：云服务器并不是单独的一台物理机，有可能是多云服务器共用一台物理机。总之该服务器处于一个小的局域网，而内网IP就是这个局域网中的一个IP，外网是访问不到的。

公网IP：对外可见的IP，对于连接了Internet网络的用户来说，可直接访问的到。

> SSL

SSL（Secure Sockets Layer）证书，对客户端与服务器之间数据进行加密传输。例如访问HTTPS的网站时，其服务器就配备了SSL证书。

> CDN

CDN（Content Distribution Network）内容分发网络，意思是在不同地域都有你网站数据的缓存，以提高用户访问你网站的速度。

#### 3、云数据库

### 二、服务器

- `webLogic`：Oracle公司，大型的`JavaEE`服务器，支持所有的`JavaEE`规范，收费的。
- `webSphere`：IBM公司，大型的`JavaEE`服务器，支持所有的`JavaEE`规范，收费的。
- `JBOSS`：JBOSS公司，大型的`JavaEE`服务器，支持所有的`JavaEE`规范，收费的。
- `Tomcat`：Apache基金组织，中小型的`JavaEE`服务器，仅仅支持少量的`JavaEE`规范servlet/jsp。开源的，免费的。

> `JavaEE`：Java语言在企业级开发中使用的技术规范的总和，一共规定了13项大的规范

### 三、Tomcat

#### 1、下载安装

- 下载：http://tomcat.apache.org/
- 解压

#### 2、启动关闭

- bin/startup.bat # 双击启动
- bin/shutdown.bat # 双击关闭 或 ctrl+c

#### 3、修改端口

- `conf/server.xml`
  - Connector标签修改port属性值。

#### 4、部署项目

- war包
  - 将项目打成war包，放置到`webapps`目录下。（自动解压缩）
- 配置
  - `conf/server.xml`文件
  - 添加`<Context docBase="D:\hello" path="/hehe" />`
  - `docBase`：项目存放的路径。
  - `path`：虚拟目录。
- 文件
  - `conf/Catalina/localhost`目录下创建xml文件。
  - 内容是`<Context docBase="D:\hello" />`
  - 虚拟目录：xml文件的名称。

#### 5、编译目录

```
|-- 项目根目录
    |-- WEB-INF
        |-- web.xml # web项目的核心配置文件
        |-- classes # 字节码文件目录
        |-- lib # 依赖的jar包
```

#### 6、配置管理密码

tomcat-users.xml

```
<role rolename ="manager-gui"/>
 <role rolename ="manager-script"/>
 <user username="you user name" password="you password" roles ="manager-gui,manager-script"/>
```

context.xml

远程访问

```
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
```

### 四、GitHub

#### 开源协议

> GPL3.0 & Apache LICENSE 2.0

GPL3.0全称为GNU通用公共授权3.0，Apache LICENSE 2.0顾名思义就是Apache制定的开源协议2.0，它们两的协议内容基本一个意思：

- ==软件可以随便用，但不能随便改，比如原商标一般不让修改，你如果修改了某个地方，必须进行突出的通知。==

- 可以免费，可以收费。

- 软件的源文件里必须有这个许可证文档;

- 我提供这个软件不是为了犯法，你要用它来犯法，那与我无关。

- 你用这个软件犯事了，责任全在你自己，与其他贡献者无关。

> MIT LICENSE

 即 麻省理工学院许可证

- ==软件可以随便用，随便改。==
- 可以免费，可以收费。
- 软件的源文件里必须有这个许可证文档。
- 我提供这个软件不是为了犯法，你要用它来犯法，那与我无关。
- 你用这个软件犯事了，责任全在你自己，与其他贡献者无关。

> 总结

如果想彻底开源就用MIT LICENSE

否则就用GPL3.0或Apache LICENSE 2.0，目的可以有以下几点：

- 不能随便改，确保项目朝着可控方向发展。
- 不能改的它原作者都不认识了，有损著作者权益，必须醒目标示出处。
- 你的改动也应该是开源的，不能白嫖。

#### 开源项目

简约的在线简历

https://github.com/gwuhaolin/resume

Spring Boot 2.4.2，Shiro1.6.0 & Layui 2.5.6 权限管理系统

https://github.com/febsteam/FEBS-Shiro

互联网 Java 工程师进阶知识完全扫盲

https://doocs.gitee.io/advanced-java

自动部署 Gitee Pages

https://github.com/yanglbme/gitee-pages-action

基于Spring+SpringMVC+Mybatis分布式敏捷开发系统架构，提供整套公共微服务服务模块：集中权限管理（单点登录）、内容管理、支付中心、用户管理（支持第三方登录）、微信平台、存储系统、配置中心、日志分析、任务和通知等，支持服务治理、监控和追踪，努力为中小型企业打造全方位J2EE企业级开发解决方案。

https://github.com/shuzheng/zheng

SpringBoot的各种整合应用

https://github.com/xkcoding/spring-boot-demo

git fork项目

https://xkcoding.com/2018/09/18/how-to-update-the-fork-project.html

mall电商系统项目

https://github.com/macrozheng/mall

Spring Boot中使用LDAP来统一管理用户信息

轻量级web富文本框

https://github.com/wangeditor-team/wangEditor

程序员简历模板

https://github.com/geekcompany/ResumeSample

程序员英语进阶

https://byoungd.gitbook.io/english-level-up-tips/

nps内网穿透

https://github.com/ehang-io/nps

docsify动态生成文档网站

https://github.com/docsifyjs/docsify

python爬虫教程

https://github.com/Kr1s77/Python-crawler-tutorial-starts-from-zero

python模拟登陆一些大型网站

https://github.com/Kr1s77/awesome-python-login-model

Git技巧篇

https://github.com/521xueweihan/git-tips

### 五、静态网站

- 常见的用于搭建静态网站的开源项目：
  - Jekyll
  - Hexo
  - Hugo
- 静态网站托管平台
  - GitHub Pages
  - GitLab Pages
  - Gitee Pages
  - Netlify

#### 1、Jekyll

使用[Jekyll](https://github.com/barryclark/jekyll-now)搭建静态博客，

### 六、博客系统Halo

项目地址：https://github.com/halo-dev/halo

项目文档：https://docs.halo.run/zh/install/index

#### 安装

> Docker安装

```shell
# 拉取镜像

# 运行
```

#### 后台管理

> 好看的评论样式

https://cdn.jsdelivr.net/gh/coortop/halo-comment-alex@latest/dist/halo-comment.min.js

### 七、docsify

动态生成文档网站

开源项目：https://github.com/docsifyjs/docsify

项目文档：https://docsify.js.org/#/zh-cn/

沙盒试炼：https://codesandbox.io/s/307qqv236

#### docsify-cli

可以方便创建及本地预览文档网站

##### 安装

```shell
# 全局安装，需安装node.js其中就内置了npm
npm i docsify-cli -g
# 查看版本
docsify -v
```

##### 案例

```shell
# 1、创建项目目录并进入（docsify）
cd docsify
# 2、初始化项目
docsify init ./
# 3、查看项目结构
|- docsify
    |-.nojekyll # 用于阻止GitHub Pages会忽略下划线开头的文件
    |-index.html # 入口文件
    |-README.md # 主页内容渲染
# 4、可以将index.html中提到的docsify.min.js或 docsify@4.js、vue.css下载下来
|- docsify
    |-css
        |-vue.css
    |-js
        |-docsify.min.js # 或 docsify@4.js
    |-.nojekyll
    |-index.html # 修改为本地引入
    |-README.md
# 5、启动本地服务
docsify serve ./
# 6、访问 http://localhost:3000
```

#### 使用技巧

##### 文档路径URL

```shell
./README.md             => http://localhost:3000 # 忽略README.md主说明文件
./2021-07-07/article.md => http://localhost:3000/2021-07-07/article # 忽略md后缀
./2021-07-07/README.md  => http://localhost:3000/2021-07-07/ # 每个路径都可以有说明文件
```

##### 侧边栏

- 默认根据标题生成。

- 自定义生成（修改index.html）
  
  - ```html
    <script>
        window.$docsify = {
            loadSidebar: true // 开启侧边栏自定义
        }
    </script>
    ```
  
  - 项目根目录创建`_sidebar.md`，内容如下（[]表示标题名，()表示文件路径）:
  
  - ```
    * [2021-07-07](/2021-07-07/)
    * [2021-07-07/article](/2021-07-07/article)
    ```
  
  - 每个路径都可以有一个`_sidebar.md`，否则将往上寻找配置并显示。
  
  - ```html
    <script>
        window.$docsify = {
            loadSidebar: true, // 开启侧边栏自定义
            alias: { // 配置别名，将子目录的配置，映射到使用根目录的配置
                  '/.*/_sidebar.md': '/_sidebar.md'
            },
            subMaxLevel: 3 // 设置 最大显示 页面中的标题 层级（页面中标题转换成目录）
        }
    </script>
    ```
  
  - ```
    注：忽略标题目录Header
    ### Header {docsify-ignore} 
    ```

##### 导航栏（横向）

- 默认导航栏

```html
<body>
  <nav>
    <a href="#/">快速启动</a>
    <a href="#/home">首页</a>
    <a href="#/start/install">安装/</a>
  </nav>
</body>
```

- 自定义导航栏

```html
<script>
  window.$docsify = {
    loadNavbar: true // 开启自定义导航栏
  }
</script>
```

在根目录创建`_navbar.md`（与自定义目录同理）

```
* [2021-07-07](2021-07-07/)
* [2021-07-07/article](2021-07-07/article)
```

下拉形式

```
* 首页
  * [下载](download)
  * [安装](install)
* 文档
  * [2021-07-07](2021-07-07/article)
```

##### 设置封面

```html
<script>
  window.$docsify = {
      coverpage: true, // 开启封面渲染
    onlyCover: true // 只显示封面，默认是封面在上，首页在下的
  }
</script>
```

根目录创建`_coverpage.md`

```
![logo](_media/icon.svg)
# 我的文档网站
## 个人文档网站
> 一个神奇的文档网站生成巩固

* Simple and lightweight (~12kb gzipped)
* Multiple themes
* Not build static html files

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#quick-start)
[Get Started](#quick-start)

<!-- 背景图片 
![](_media/bg.png)
-->
<!-- 背景色
![color](#2f4253)
 -->
```

> 多封面

```
|-docs
    |-README.md
    |-article.md
    |-_coverpage.md
    |-zh-cn
        |-README.md
        |-article.md
        |-_coverpage.md
```

```html
<script>
  window.$docsify = {
      coverpage: ['/', '/zh-cn/'] // 默认文件名_coverpage.md
  }
</script>
// 或指定文件名
<script>
  window.$docsify = {
    coverpage: {
        '/': 'cover.md', // _coverpage.md修改后的文件名
        '/zh-cn/': 'cover.md'
    }
  }
</script>
```

#### 插件

##### gitalk

GitHub评论插件

项目地址：https://github.com/gitalk/gitalk

```xml
<link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
<script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
<script>
  const gitalk = new Gitalk({
    clientID: '801bfb923418263efabc',
    clientSecret: 'b1370e24ae2bd0450eba3481a8a1172ea26acad6',
    repo: 'notes', // 仓库名
    owner: 'liuconglook', // github名称
    admin: ['liuconglook'], // 管理人
    // facebook-like distraction free mode
    distractionFreeMode: false
  })
</script>
```
