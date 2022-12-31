## Nginx

### 安装

#### 解压安装

~~~bash
# 下载
wget http://nginx.org/download/nginx-1.22.0.tar.gz
# 解压
tar -zxvf nginx-1.22.0.tar.gz
# 安装
cd nginx-1.22.0
./configure
./configure --prefix=/usr/local/nginx --with-http_realip_module --with-http_image_filter_module=dynamic --with-http_ssl_module
make && make install

# 启动
cd /usr/local/nginx/sbin/
./nginx

# 安装openSSL
wget http://downloads.sourceforge.net/gnuwin32/openssl-0.9.8h-1-bin.zip
zip -r openssl-0.9.8h-1-bin.zip
cd openssl-0.9.8h-1-bin
./config --prefix=/usr/local/ --openssldir=/usr/local/openssl -g3 shared zlib-dynamic enable-camellia
make && make install

# 安装zlib
wget http://www.zlib.net/zlib-1.2.12.tar.gz
tar -zxvf zlib-1.2.12.tar.gz
cd zlib-1.2.12
./configure
make && make install
~~~



### 常用命令

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
netstat -ano | findstr 0.0.0.0:8080
# windows关闭进程
taskkill /pid [pid]


# 
tail -f /usr/local/nginx/logs/access.log
~~~

### 配置

tomcat+nginx负载均衡+SSL

#### 1、部署多个tomcat

- tomcat-8080

  - `conf/server.xml`配置启动端口：`8080`、关闭端口：`8005`。

  - `bin/catalina.bat`配置，在最上方添加：

  - ~~~text
    @echo off
    CATALINA_HOME=C:\java\tomcat-8080
    CATALINA_BASE=C:\java\tomcat-8080
    ~~~

- tomcat-9001

  - `conf/server.xml`配置启动端口：`9001`、关闭端口：`8006`。

  - `bin/catalina.bat`配置，在最上方添加：

  - ~~~text
    @echo off
    CATALINA_HOME=C:\java\tomcat-9001
    CATALINA_BASE=C:\java\tomcat-9001
    ~~~

- tomcat-9002

  - `conf/server.xml`配置启动端口：`9002`、关闭端口：`8007`。

  - `bin/catalina.bat`配置，在最上方添加：

  - ~~~text
    @echo off
    CATALINA_HOME=C:\java\tomcat-9002
    CATALINA_BASE=C:\java\tomcat-9002
    ~~~

#### 2、nginx配置

~~~conf

#user  nobody;
worker_processes  1; # 工作线程数，auto自动

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"'; # 日志格式

    #access_log  logs/access.log  main;

    sendfile        on; # 开启发送文件
    client_max_body_size 20m; # 设置请求体大小mb
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65; # 心跳超时时间

    #gzip  on;

    upstream www.liuconglook.cn { # 域名
    	least_conn; # 最小连接
        server 127.0.0.1:8080;
        server 127.0.0.1:9001;
        server 127.0.0.1:9002;
    }

    server {
        
	    listen 80;
        server_name www.liuconglook.cn liuconglook.cn;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            proxy_set_header HOST $host; # :$server_port
            proxy_http_version 1.1; # 配置wss需要
            proxy_set_header Upgrade $http_upgrade; # 配置wss需要
            proxy_set_header Connection "upgrade"; # 配置wss需要
	    	proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	    	proxy_pass http://www.liuconglook.cn; # 代理到上面配置的服务器地址
        }
		
		# 静态资源
        location /files/image/ {
            alias C:\\file\\;
            autoindex on;
        }
        
 		# 配置SSL证书
		listen 443 ssl;
		ssl_certificate C:\\Users\\Administrator\\Desktop\\nginx-1.18.0\\lce\\www.liuconglook.cn.pem; # 证书
	ssl_certificate_key C:\\Users\\Administrator\\Desktop\\nginx-1.18.0\\lce\\www.liuconglook.cn.key; # 证书key
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers   on;

    }
}
~~~

#### 3、负载均衡策略

##### I、默认：轮询

##### II、ip_hash

~~~ 
upstream www.liuconglook.cn {
    ip_hash; # 根据ip，hash到指定服务地址
    server 127.0.0.1:8080;
    server 127.0.0.1:9001;
    server 127.0.0.1:9002;
}
~~~

##### III、least_conn

~~~
upstream www.liuconglook.cn {
    least_conn; # 最小连接
    server 127.0.0.1:8080;
    server 127.0.0.1:9001;
    server 127.0.0.1:9002;
}
~~~

##### IIII、权重

~~~
upstream www.liuconglook.cn {
    server 127.0.0.1:8080 weight=1; # 总权重6，1/6的访问概率
    server 127.0.0.1:9001 weight=2; # 总权重6，2/6的访问概率
    server 127.0.0.1:9002 weight=3; # 总权重6，3/6的访问概率
    # 数字越大权重越大，访问的概率也就越大
}
~~~

#### 4、访问静态资源

~~~
location /files/image/ { # 拦截路径
	alias C:\\file\\; # 服务器静态文件的地址
	autoindex on; // 开启目录索引
}
~~~

配置静态文件地址有两种方式：

- root

  - 当访问：`https://www.liuconglook.cn`+`/files/image/`+`xxx.jpg`

    - 实际上访问：`C:\file\`+`/files/image/`+`xxx.jpg`
    - 访问不到。

  - 要想访问到`https://www.liuconglook.cn`+`/file/`+`xxx.jpg`：

  - ~~~
    location /file/ { # 拦截路径
    	root C:\\; # 服务器静态文件的地址
    	autoindex on; // 开启目录索引
    }
    ~~~

  - 实际访问：`C:\`+`/file/`+`xxx.jpg`

- alias

  - 当访问：`https://www.liuconglook.cn/files/image/xxx.jpg`
    - 实际上访问：`C:\\file\\xxx.jpg`

- 区别

  - root：地址会加上所拦截的路径，配置地址+拦截地址，拼接。
  - alias：地址不会加上所拦截的路径，直接映射目录。

#### 5、ICO

~~~
location = /favicon.ico {
	root  /etc/nginx;
}
~~~

#### 6、SSL

> default.conf

~~~shell
server { 
    listen 80;
    server_name  liuconglook.cn www.liuconglook.cn; 
    return 301 https://www.liuconglook.cn$request_uri;
}
server {
    # ssl cert
    listen 443 ssl;
    server_name  liuconglook.cn www.liuconglook.cn;
    # ssl on;
    ssl_certificate /etc/nginx/www.liuconglook.cn.pem;
    ssl_certificate_key /etc/nginx/www.liuconglook.cn.key;
    #ssl_session_cache shared:SSL:1m;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    access_log  /etc/nginx/log/host.access.log  main;

    location / {
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        #proxy_set_header Connection "upgrade";
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded $proxy_add_x_forwarded_for;
        proxy_pass http://127.0.0.1:8881;
     }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
    
}
~~~

#### 7、wss

~~~shell
		# 拦截websocket请求
        location /websocket {
           proxy_pass http://127.0.0.1:8001;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection "upgrade";
        }
~~~

