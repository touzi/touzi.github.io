---
date: 2014/12/26 10:27:25 
layout: post
title: Ubuntu下nginx设置多端口转发(反向代理)
thread: 1
categories: Ubuntu
tags: nginx

---

### Ubuntu下nginx设置多端口转发(方向代理)

## 0. 实现目标

1. 有域名a.com,b.com
2. 分别指向服务器的200端口和2000端口
3. 也就是说当用户访问a.com时会被跳转到ip:200端口,当访问b.com时跳转到ip:2000端口

## 1. 安装nginx

    sudo apt-get install nginx

因为是自动安装所以目录结构有所变化,根据Ubuntu官方给的文档介绍如下
[http://wiki.ubuntu.org.cn/Nginx](http://wiki.ubuntu.org.cn/Nginx)

Ubuntu安装之后的文件结构大致为：

* 所有的配置文件都在`/etc/nginx`下，并且每个虚拟主机已经安排在了`/etc/nginx/sites-available`下
* 程序文件在`/usr/sbin/nginx`
* 日志放在了`/var/log/nginx`中
* 并已经在`/etc/init.d/`下创建了启动脚本nginx
* 默认的虚拟主机的目录设置在了`/var/www/nginx-default` (有的版本 默认的虚拟主机的目录设置在了`/var/www`, 请参考`/etc/nginx/sites-available`里的配置)

## 2. 启动nginx

    sudo /etc/init.d/nginx start

* 然后就可以访问了，`http://localhost/` ， 一切正常！
* 如果不能访问，先不要继续，看看是什么原因，解决之后再继续。 
* 启动时候若显示端口80被占用： `Starting nginx: [emerg]: bind() to 0.0.0.0:80 failed (98: Address already in use)` 修改文件：`/etc/nginx/sites-available/default`,去掉 `listen` 前面的 `#` 号 , # 号在该文件里是注释的意思 , 并且把 listen 后面的 80 端口号改为自己的端口，访问是需要添加端口号。
* （安装完后如出现403错误，那可能是nginx配置文件里的网站路径不正确）

如果看到一下结果说明安装成功.
![http://tblogmarkdown.qiniudn.com/0.png](http://tblogmarkdown.qiniudn.com/0.png)

## 3. 修改nginx.conf

    cd /etc/nginx
    sudo vim nginx.conf

**在http{}中加入下面的代码,注意这里要加在这两天命令之前**

`include /etc/nginx/conf.d/*.conf;`

`include /etc/nginx/sites-enabled/*;`

        ##
        #touzi --- 122614
        ##
        client_max_body_size 50m;
        #缓冲区代理缓冲用户端请求的最大字节数,可以理解为保存到本地再传给用户
        client_body_buffer_size 256k;
        client_header_timeout 3m;
        client_body_timeout 3m;
        send_timeout 3m;
        proxy_connect_timeout 300s;
        #nginx跟后端服务器连接超时时间(代理连接超时)
        proxy_read_timeout 300s;
        #连接成功后，后端服务器响应时间(代理接收超时)
        proxy_send_timeout 300s;
        proxy_buffer_size 64k;
        #设置代理服务器（nginx）保存用户头信息的缓冲区大小
        proxy_buffers 4 32k;
        #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
        proxy_busy_buffers_size 64k;
        #高负荷下缓冲大小（proxy_buffers*2）
        proxy_temp_file_write_size 64k;
        #设定缓存文件夹大小，大于这个值，将从upstream服务器传递请求，而不缓冲到磁盘
        proxy_ignore_client_abort on;
        #不允许代理端主动关闭连接
        server {
                listen 80;
                server_name localhost;
        location / {
            root html;
            index index.html index.htm;
        }
                error_page 500 502 503 504 /50x.html;
        location = /50x.html {
            root html;
        }
        }

        ##
        #end
        ##

## 4. 配置vhost.conf

默认conf.d目录下是没有文件的
所以我们这里直接用这个命令

    sudo vim conf.d/vhost.conf

然后加入一下代码

    server
    {
        listen 80;
        server_name a.com;
        location / {
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://127.0.0.1:200;
        }
        access_log /var/log/nginx/a.com_access.log;
    }
    
    
    server
    {
        listen 80;
        server_name b.com;
        location / {
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://127.0.0.1:2000;
        }
        access_log /var/log/nginx/b.com_access.log;
    }

**注意:**

1. 因为我的两个项目都在本机所以`proxy_pass`用了`127.0.0.1`,如果你们的项目在其他服务器上可以直接写ip地址.
2. `access_log` 一定要配置对,不然的话会因找不到这个日志路径而无法重启nginx
3. 多个server就可以配置多个

## 5. 重启nginx

    sudo /etc/init.d/nginx restart

看到状态OK说明重启成功,否则找找原因吧.

-----------------------------

end

12/26/2014