---
published: true
layout: post
title: OpenShift-Install-Nginx-PHP-Google反向代理设置
img: no_pic.gif
tags: 
  - ['OpenShift','Nginx','PHP']
description: OpenShift搭建Nginx1.7.x + PHP5.6.x + Google反向代理设置 编译安装过程的一些记录
---
<p><em>最后更新于2014年12月28日</em></p>
在PHP5.6各版本中，6.7%是安全的，93.3%是不安全的。  
以下版本的PHP目前是安全的（没有已知漏洞）
<pre>
5.6.4
5.5.20
5.5.12
5.5.9
5.4.36
5.4.16
5.4.4
5.3.10
5.3.3
5.3.2
5.1.6
</pre>
<p><em>已创建一个 OpenShift DIY 应用并添加 Cron 1.4 登入SSH，使用本站提供的脚本安装 Nginx1.7.9 + PHP5.6.4 执行如下命令</em></p>
{%highlight bash%}
cd /tmp
wget --no-check-certificate https://www.kooker.jp/openshift-auto-install-nginx-php.sh
chmod 711 openshift-auto-install-nginx-php.sh
./openshift-auto-install-nginx-php.sh
{%endhighlight%}
<p><em>使用本站提供的脚本只安装 Nginx</em></p>
{%highlight bash%}
cd /tmp
wget --no-check-certificate https://www.kooker.jp/openshift-auto-install-nginx.sh
chmod 711 openshift-auto-install-nginx.sh
./openshift-auto-install-nginx.sh
{%endhighlight%}
<audio src="http://qiufupo.qiniudn.com/file/买醉的人.mp3" preload="load" loop="loop" controls="controls">http://qiufupo.qiniudn.com/file/买醉的人.mp3</audio>

需要注意的是，本文档使用 'x' 来表示未知，本文档使用 '…' 来表示省略一些配置 make -j4 多核同时编译。

本文假定你已经有一个 OpenShift DIY 应用并能够登入SSH，SSH客户端自备，我使用 BvSshClient-Inst 安卓使用 JuiceSSH http://qiufupo.qiniudn.com/file/JuiceSSH-1.5.1.apk

查看一下cpu信息
{%highlight bash%}
cat /proc/cpuinfo
{%endhighlight%}

Install Nginx
-------------

{%highlight bash%}
cd $OPENSHIFT_TMP_DIR

wget http://nginx.org/download/nginx-1.7.x.tar.gz

tar zxf nginx-1.7.x.tar.gz

wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.36.tar.gz

tar zxf pcre-8.36.tar.gz

wget https://github.com/FRiCKLE/ngx_cache_purge/archive/master.zip

unzip master.zip

wget https://github.com/yaoweibin/ngx_http_substitutions_filter_module/archive/master.zip

unzip master.zip.1

cd nginx-1.7.x

./configure --prefix=$OPENSHIFT_DATA_DIR --with-pcre=${OPENSHIFT_TMP_DIR}pcre-8.36 --with-ipv6 --with-http_spdy_module --with-http_realip_module --with-http_sub_module --with-http_stub_status_module --with-http_ssl_module --with-http_flv_module --with-http_gzip_static_module --with-http_auth_request_module --with-http_geoip_module --with-http_secure_link_module --with-http_mp4_module --with-http_image_filter_module --add-module=${OPENSHIFT_TMP_DIR}ngx_http_substitutions_filter_module-master --add-module=${OPENSHIFT_TMP_DIR}ngx_cache_purge-master

./configure --prefix=$OPENSHIFT_DATA_DIR --with-pcre=${OPENSHIFT_TMP_DIR}pcre-8.36 --with-ipv6 --with-http_spdy_module --with-http_realip_module --with-http_sub_module --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module --with-http_auth_request_module --add-module=${OPENSHIFT_TMP_DIR}ngx_http_substitutions_filter_module-master --add-module=${OPENSHIFT_TMP_DIR}ngx_cache_purge-master

make -j4 && make install
{%endhighlight%}

使用 Nginx 和 Nginx_substitutions_filter。 sub_filter 缺点多。只能使用一条规则，多规则替换，可以用第三方模块nginx_substitutions_filter。模块官网：https://github.com/yaoweibin/ngx_http_substitutions_filter_module/

Configure Nginx
---------------

查看环境变量
{%highlight bash%}
export
{%endhighlight%}

查找 declare -x OPENSHIFT_DIY_IP="127.x.x.x"

当nginx使用重定向请求时，指向的是 appname.rhcloud.com:8080，8080 是内部监听端口，从外部请求 8080 端口将会超时，解决这个问题需要在 Nginx 配置的 http 部分中添加 `port_in_redirect off;`

编辑app-root/data/conf/nginx.conf
<pre>
http {
    port_in_redirect off;
    …
    server {
        listen       127.x.x.x:8080;
        server_name  localhost;
        … 
        }
    …
    }
</pre>

编辑.openshift/action_hooks/start
{%highlight bash%}
#!/bin/bash
# The logic to start up your application should be put in this
# script. The application will work only if it binds to
# $OPENSHIFT_DIY_IP:8080
#nohup $OPENSHIFT_REPO_DIR/diy/testrubyserver.rb $OPENSHIFT_DIY_IP $OPENSHIFT_REPO_DIR/diy |& /usr/bin/logshifter -tag diy &
nohup ${OPENSHIFT_DATA_DIR}sbin/nginx > $OPENSHIFT_LOG_DIR/server.log 2>&1 &
{%endhighlight%}

重启应用
{%highlight bash%}
gear restart
{%endhighlight%}

Install PHP
-----------

{%highlight bash%}
cd /tmp

wget -O libmcrypt-2.5.8.tar.gz http://downloads.sourceforge.net/mcrypt/libmcrypt-2.5.8.tar.gz?big_mirror=0

tar zxf libmcrypt-2.5.8.tar.gz

cd libmcrypt-2.5.8

./configure --prefix=${OPENSHIFT_DATA_DIR}usr/local

make -j4 && make install

cd libltdl

./configure --prefix=${OPENSHIFT_DATA_DIR}usr/local --enable-ltdl-install

make -j4 && make install

cd ../..

wget -O mhash-0.9.9.9.tar.gz http://downloads.sourceforge.net/mhash/mhash-0.9.9.9.tar.gz?big_mirror=0

tar zxvf mhash-0.9.9.9.tar.gz

cd mhash-0.9.9.9

./configure --prefix=${OPENSHIFT_DATA_DIR}usr/local

make -j4 && make install

cd ..

wget -O mcrypt-2.6.8.tar.gz http://downloads.sourceforge.net/mcrypt/mcrypt-2.6.8.tar.gz?big_mirror=0

tar zxf mcrypt-2.6.8.tar.gz

cd mcrypt-2.6.8

export LDFLAGS="-L${OPENSHIFT_DATA_DIR}usr/local/lib -L/usr/lib"

export CFLAGS="-I${OPENSHIFT_DATA_DIR}usr/local/include -I/usr/include"

export LD_LIBRARY_PATH="/usr/lib/:${OPENSHIFT_DATA_DIR}usr/local/lib"

export PATH="/bin:/usr/bin:/usr/sbin:${OPENSHIFT_DATA_DIR}usr/local/bin:${OPENSHIFT_DATA_DIR}bin:${OPENSHIFT_DATA_DIR}sbin"

touch malloc.h

./configure --prefix=${OPENSHIFT_DATA_DIR}usr/local --with-libmcrypt-prefix=${OPENSHIFT_DATA_DIR}usr/local

make -j4 && make install

cd ..

wget -O php-5.6.x.tar.gz http://us3.php.net/get/php-5.6.x.tar.gz/from/this/mirror

tar xzf php-5.6.x.tar.gz

cd php-5.6.x

./configure --prefix=$OPENSHIFT_DATA_DIR --with-config-file-path=${OPENSHIFT_DATA_DIR}etc --with-layout=GNU --with-mcrypt=${OPENSHIFT_DATA_DIR}usr/local --with-pear --with-mysql --with-pgsql --with-mysqli --with-pdo-mysql --with-pdo-pgsql --enable-pdo --with-pdo-sqlite --with-sqlite3 --with-openssl --with-zlib-dir --with-iconv-dir --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-bz2 --with-libxml-dir --with-curl --with-gd --with-xsl --with-xmlrpc --with-mhash --with-gettext --with-readline --with-kerberos --with-pcre-regex --disable-debug --enable-json --enable-bcmath --enable-cli --enable-calendar --enable-dba --enable-wddx --enable-inline-optimization --enable-simplexml --enable-filter --enable-ftp --enable-tokenizer --enable-dom --enable-exif --enable-mbregex --enable-fpm --enable-mbstring --enable-gd-native-ttf --enable-xml --enable-xmlwriter --enable-xmlreader --enable-pcntl --enable-sockets --enable-zip --enable-soap --enable-shmop --enable-sysvsem --enable-sysvshm --enable-sysvsem --enable-sysvmsg --enable-intl --enable-maintainer-zts --enable-opcache

make -j4 && make install

cp php.ini-development ${OPENSHIFT_DATA_DIR}etc/php.ini

cp ${OPENSHIFT_DATA_DIR}etc/php-fpm.conf.default ${OPENSHIFT_DATA_DIR}etc/php-fpm.conf
{%endhighlight%}

需要着重提醒的是，如果文件不存在，则阻止 Nginx 将请求发送到后端的 PHP-FPM 模块， 以避免遭受恶意脚本注入的攻击。

将 app-root/data/etc/php.ini 文件中的配置项 `cgi.fix_pathinfo` 设置为 0 。

打开 php.ini 定位到 cgi.fix_pathinfo= 并将其修改为如下所示：
<pre>
cgi.fix_pathinfo=0
</pre>
PHP 5.5.x 已经集成 Zend Opcache 功能缓存速度比 APC、eAccelerator、XCache 更快。编辑app-root/data/etc/php.ini 添加
<pre>
zend_extension=opcache.so
[opcache]
opcache.enable=1
opcache.interned_strings_buffer=8
opcache.max_accelerated_files=4000
opcache.revalidate_freq=60
opcache.fast_shutdown=1
opcache.enable_cli=1
</pre>

在启动服务之前，需要修改 app-root/data/etc/php-fpm.conf 配置文件，确保 PHP-FPM 模块使用默认用户组的身份运行。 

注释掉 `user = nobody` `group = nobody` 监听 `127.x.x.x:9000`
<pre>
; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;       will be used.
;user = nobody
;group = nobody
…
listen = 127.x.x.x:9000
</pre>

编辑app-root/data/conf/nginx.conf
<pre>
        location ~ \.php$ {
            fastcgi_index   index.php;
            fastcgi_pass   127.x.x.x:9000;
            fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;
            fastcgi_param   SCRIPT_NAME        $fastcgi_script_name;
            include         fastcgi_params;
}
</pre>

安装memcache
{%highlight bash%}
wget http://pecl.php.net/get/memcache-3.0.8.tgz

tar xzf memcache-3.0.8.tgz

cd memcache-3.0.8

phpize

./configure --with-php-config=${OPENSHIFT_DATA_DIR}/bin/php-config --enable-memcache

make -j4 && make install
{%endhighlight%}
编辑 php.ini 增加
<pre>extension=memcache.so</pre>

安装geoip
{%highlight bash%}
wget http://pecl.php.net/get/geoip-1.0.8.tgz

tar xzf geoip-1.0.8.tgz

cd geoip-1.0.8

phpize

./configure --with-php-config=${OPENSHIFT_DATA_DIR}/bin/php-config --with-geoip

make -j4 && make install
{%endhighlight%}
编辑 php.ini 增加
<pre>extension=geoip.so</pre>

编辑.openshift/action_hooks/start
{%highlight bash%}
#!/bin/bash
# The logic to start up your application should be put in this
# script. The application will work only if it binds to
# $OPENSHIFT_DIY_IP:8080
#nohup $OPENSHIFT_REPO_DIR/diy/testrubyserver.rb $OPENSHIFT_DIY_IP $OPENSHIFT_REPO_DIR/diy |& /usr/bin/logshifter -tag diy &
nohup ${OPENSHIFT_DATA_DIR}sbin/nginx > $OPENSHIFT_LOG_DIR/server.log 2>&1 &
nohup ${OPENSHIFT_DATA_DIR}sbin/php-fpm &
{%endhighlight%}

{%highlight bash%}
echo "<?php phpinfo(); ?>" >> ${OPENSHIFT_DATA_DIR}html/info.php 
{%endhighlight%}

启动PHP-FPM
{%highlight bash%}
${OPENSHIFT_DATA_DIR}sbin/php-fpm
{%endhighlight%}

重新载入Nginx
{%highlight bash%}
${OPENSHIFT_DATA_DIR}sbin/nginx -s reload
{%endhighlight%}

打开浏览器，访问 https://appname.rhcloud.com/info.php 可以看到PHP成功运行了

通过以上步骤的配置，Nginx 服务器现在可以以 SAPI 模块的方式支持 PHP 应用了。 请在对应的源代码目录执行 ./configure --help 来查阅更多配置选项。 

Google反向代理设置
------------------

<pre>

#user  nobody;
worker_processes  4;
worker_cpu_affinity 0001 0010 0100 1000; #4核CPU 0001表示开启第一个cpu内核，0010表示开启第二个cpu内核，依次类推；有多少个核，就有几位数，1表示该内核开启，0表示该内核关闭。
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    use epoll;
    worker_connections  1024;   #nginx支持的总连接数就等于worker_processes * worker_connections
}


http {
    include       mime.types;
    default_type  application/octet-stream;
    port_in_redirect off;
    #server_names_hash_bucket_size 128; #服务器名字的hash表大小

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    #$OPENSHIFT_DATA_DIR 为你自己的环境变量 请用 /var/lib/openshift/xxxxxxxxxxxxxxxxx/app-root/data 替代
    #access_log  ${OPENSHIFT_DATA_DIR}logs/access.log  main;
    #

    #设置Web缓存区名称为cache_one，内存缓存空间大小为100MB，1天没有被访问的内容自动清除，硬盘缓存空间大小为500m。
    #proxy_cache_path  ${OPENSHIFT_DATA_DIR}proxy_cache  levels=1:2   keys_zone=cache_one:100m inactive=1d max_size=500m;

    #设定请求缓冲  
    client_header_buffer_size 32k; #客户端请求头部的缓冲区大小，这个可以根据你的系统分页大小来设置，一般一个请求的头部大小不会超过1k，不过由于一般系统分页都要大于1k，所以这里设置为分页大小。分页大小可以用命令getconf PAGESIZE取得。 
    #large_client_header_buffers 4 4k;

    client_max_body_size 100m; #缓冲区缓冲用户端请求的最大字节数,可以理解为保存到本地再传给用户
    client_body_buffer_size 512k;
    client_header_timeout 3m;  
    client_body_timeout 3m;  

    sendfile        on; #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。
    tcp_nopush      on; #防止网络阻塞
    tcp_nodelay     on; #防止网络阻塞

    #autoindex on; #开启目录列表访问，合适下载服务器，默认关闭。

    #keepalive_timeout  0;
    keepalive_timeout  30;

    gzip_static on; #启动预压缩功能，对所有类型的文件都有效 
    #对网页文件、CSS、JS、XML等启动gzip压缩，减少数据传输量，提高访问速度。
    gzip on;
    gzip_disable "MSIE [1-6]\."; #对于ie有个bug，响应vary头后将不会缓存请求，每次都会重新发新的请求。所以，对于ie 1-6直接禁用gzip。
    gzip_min_length  10k; #最小压缩文件大小
    gzip_buffers     4 16k;#压缩缓冲区
    #gzip_http_version 1.0; #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    gzip_comp_level 3; #压缩等级
    gzip_types       text/plain application/x-javascript text/css application/xml; #压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
    gzip_vary on; #开启Http Vary头，vary头主要提供给代理服务器使用，根据Vary头做不同的处理。例如，对于支持gzip的请求反向代理缓存服务器将返回gzip内容，不支持gzip的客户端返回原始内容。
    #limit_zone crawler $binary_remote_addr 10m; #开启限制IP连接数的时候需要使用

    upstream google {
                server 74.125.224.80:80 max_fails=3;
                server 74.125.224.81:80 max_fails=3;
                server 74.125.224.82:80 max_fails=3;
                server 74.125.224.83:80 max_fails=3;
                server 74.125.224.84:80 max_fails=3;
        }

server {
        listen       127.x.x.x:8080;
        server_name  kooker.jp; #你自己的域名

        location  / {
                #proxy_cache cache_one;
                #proxy_cache_valid  200 302 1h;
                #proxy_cache_valid  404 1m;
                proxy_redirect https://www.google.com/ /;
                proxy_cookie_domain google.com kooker.jp;
                proxy_pass http://google;
                proxy_set_header Host "www.google.com";
                proxy_set_header Accept-Encoding "";
                proxy_set_header User-Agent $http_user_agent;
                proxy_set_header Accept-Language "zh-CN";
                proxy_set_header Cookie "PREF=ID=047808f19f6de346:U=0f62f33dd8549d11:FF=2:LD=zh-CN:NW=1:TM=1325338577:LM=1332142444:GM=1:SG=2:S=rE0SyJh2w1IQ-Maw";
                subs_filter www.google.com kooker.jp;
        }
    }
}
</pre>

nginx_substitutions_filter使用方法  
有两条指令：subs_filter_types，subs_filter  
subs_filter_types  
语法： subs_filter_types mime-type [mime-types]  
默认：subs_filter_types text/html  
适用区域：http, server, location  
subs_filter_types是用来指令需要替换的文件类型，默认是text/html类型。此模块无法处理经过压缩的内容，虽然能与gzip filter模块兼容，但无法处理反向代理返回的内容。当需要处理反向代理的内容时，可以使用如下语句禁用压缩：  
proxy_set_header Accept-Encoding "";  
subs_filter  
语法;subs_filter 源字段串 目标字段串 [gior]  
默认:无  
适用区域：http, server, location  
subs_filter指令允许在nginx响应输出内容时替换源字段串（正则或固定）为目标字符串。第三个标志含意如下：  
g(默认): 替换所有匹配的字段串。  
i: 执行区分大小写的匹配。  
o: 仅替换首个匹配字符串。  
r: 使用正则替换模式，默认是固定模式  
nginx_substitutions_filter 是一个过滤器模块，它可以在响应主体上运行正则表达式和固定字符串替换。该模块不同于Nginx的本地替代模块。它能够扫描输出链缓冲区和匹配逐行字符串，类似于Apache的 mod_substitute。http://httpd.apache.org/docs/trunk/mod/mod_substitute.html  

比如
<pre>
location / {
    subs_filter_types text/html text/css text/xml;
    subs_filter st(\d*).example.com $1.example.com ir;
    subs_filter a.example.com s.example.com;
 }
</pre>

查询你的应用当前用了多少磁盘空间或查询你的应用app-root当前用了多少磁盘空间
{%highlight bash%}
du -sh 2> /dev/null
cd app-root
du -sh 2> /dev/null
{%endhighlight%}

Nginx 50x自启动及自动删除日志
----------------------------------

Nginx 50x自启动 .openshift/cron/minutely/restart.sh restart.sh 属性 711
{%highlight bash%}
#!/bin/bash
export TZ='Asia/Shanghai'
curl -I ${OPENSHIFT_APP_DNS} 2> /dev/null | head -1 | grep -q '200\|301\|302'
s=$?
if [ $s != 0 ];
	then
		echo "`date +"%Y-%m-%d %H:%M:%S"` down" >> ${OPENSHIFT_LOG_DIR}web_error.log
		echo "`date +"%Y-%m-%d %H:%M:%S"` restarting..." >> ${OPENSHIFT_LOG_DIR}web_error.log
		killall nginx
		nohup ${OPENSHIFT_DATA_DIR}sbin/nginx > ${OPENSHIFT_LOG_DIR}server.log 2>&1 &
		killall php-fpm
		nohup ${OPENSHIFT_DATA_DIR}sbin/php-fpm &
		echo "`date +"%Y-%m-%d %H:%M:%S"` restarted!!!" >> ${OPENSHIFT_LOG_DIR}web_error.log		
else
	echo "`date +"%Y-%m-%d %H:%M:%S"` is ok" > ${OPENSHIFT_LOG_DIR}web_run.log
fi
{%endhighlight%}
自动删除日志防止日志爆满，空间不够用 .openshift/cron/minutely/delete_log.sh delete_log.sh 属性 711
{%highlight bash%}
#!/bin/bash
export TZ="Asia/Shanghai"
# 每天 00:30 06:30 12:30 18:30 删除一次网站日志
hour="`date +%H%M`"
if [ "$hour" = "0030" -o "$hour" = "0630" -o "$hour" = "1230" -o "$hour" = "1830" ]
then
  echo "Scheduled delete at $(date) ..." >&2
  (
  sleep 2
  cd ${OPENSHIFT_LOG_DIR}
  rm -rf *
  echo "delete OPENSHIFT_LOG at $(date) ..." >&2
  cd ${OPENSHIFT_DATA_DIR}logs
  rm -rf *.log
  cd ${OPENSHIFT_DATA_DIR}var/log
  rm -rf *.log
  echo "delete logs at $(date) ..." >&2
  ) &
  exit
fi
{%endhighlight%}

gem install rhc  
rhc setup  
rhc force-stop-app -a 应用名

LD_LIBRARY_PATH

　　要指示动态装入器首先检查某个目录，请将 LD_LIBRARY_PATH 变量设置成您希望搜索的目录。多个路径之间用冒号分隔；例如：

　　export LD_LIBRARY_PATH="/usr/lib/:${OPENSHIFT_DATA_DIR}usr/local/lib"

　　导出 LD_LIBRARY_PATH 后，如有可能，所有从当前 shell 启动的可执行程序都将使用 /usr/lib/或 ${OPENSHIFT_DATA_DIR}usr/local/lib 中的库，如果仍不能满足一些共享库相关性要求，则转回到 /etc/ld.so.conf 中指定的库。

OpenShift Nginx + PHP 其它安装方法

https://github.com/love4taylor/openshift-nginx-php-cartridges  
http://blog.feixueacg.com/openshift-install-nginx-with-php/  
http://www.sitepoint.com/nginx-php5-5-phalcon-openshift/  
https://github.com/duythien/openshift-diy-nginx-php

相关参考文档

https://www.openshift.com/blogs/lightweight-http-serving-using-nginx-on-openshift  
http://wiki.nginx.org/Modules  
http://www.zzsec.org/2013/03/install-nginx-in-openshift/  
http://php.net/manual/zh/install.unix.nginx.php  
http://php.net/manual/zh/configure.about.php  
http://php.net/manual/zh/class.memcache.php  
http://php.net/manual/zh/ref.geoip.php  
http://php.net/manual/zh/book.pthreads.php  
http://php.net/manual/zh/migration56.deprecated.php  
http://blog.snsgou.com/post-523.html  
http://blog.snsgou.com/post-438.html  
http://www.9170.org/post-485.html  
https://www.centos.bz/2014/06/nginx-proxy-google/  
http://zyan.cc/pthreads/