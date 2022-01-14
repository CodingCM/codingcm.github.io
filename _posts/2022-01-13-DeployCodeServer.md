---
title: Ubuntu部署code-server
category: [toolswiki, other]
sidebar:
nav: TOOL
---

## 一、安装

安装[官网安装流程](https://github.com/coder/code-server)进行安装，保证在控制台中`code-server`命令可用即可。

例如：

```shell
$ code-server
[2022-01-13T09:24:25.131Z] info code-server 3.12.0 b37ff28a0a582aee84a8f961755d0cb40a4081db
[2022-01-13T09:24:25.132Z] info Using user-data-dir ~/.local/share/code-server
```

官网中推荐了好几种部署方式，这里采用Nginx反向代理code-server服务。nginx简要介绍可以参考[此文章](https://mp.weixin.qq.com/s/XoqGvYBabW8YBl9xEeNYZw)。
## 二、systemd服务配置
在`/lib/systemd/system`文件夹中创建`code-server.service`文件：
```shell
$ sudo vim /lib/systemd/system/code-server.service
```
在`/lib/systemd/system/code-server.service`文件中输入以下内容：
```shell
[Unit]
Description=code-server
After=network.target

[Service]
Type=exec
Restart=always
Environment=PASSWORD=your_password
ExecStart=/usr/bin/code-server --bind-addr 127.0.0.1:8088 --auth password
User=%i

[Install]
WantedBy=default.target
```

其中：
（1）`Environment=PASSWORD=your_password`中的`your_password`要设置为自己的密码；

（2）`ExecStart`表示服务将要启动的可执行程序；其选项`--bind-addr`是该服务所对应的本地地址，`8088`是自定义的端口号，选择一个未使用的端口号即可。后续启动nginx服务时，其代理的本地地址需要与这里一致。`--auth password`表示通过密码的方式访问code-server。
然后，通过以下命令启动code-server服务

```shell
$ sudo systemctl start code-server
```

并通过以下命令检查该服务是否已正常启动：

```shell
$ sudo systemctl status code-server
```

正常情况下的输入类似如下内容：

```

● code-server.service - code-server

Loaded: loaded (/etc/systemd/system/code-server.service; enabled; vendor preset: enabled)
Active: active (running) since Thu 2022-01-13 19:25:09 CST; 1h 40min ago
Main PID: 764 (node)
Tasks: 22 (limit: 2275)
Memory: 79.3M

CGroup: /system.slice/code-server.service
├─ 764 /usr/lib/code-server/lib/node /usr/lib/code-server --bind-addr 127.0.0.1:8088 --auth password
└─1059 /usr/lib/code-server/lib/node /usr/lib/code-server --bind-addr 127.0.0.1:8088 --auth password
...

```

当确认code-server状态正常以后，执行下述命令，以让机器在重启后会自动启动该服务：

```shell
$ sudo systemctl enable code-server
```

## 三、通过Nginx代理code-server服务

首先通过下方命令新建关于code-server的nginx配置文件

```shell
$ sudo vim /etc/nginx/sites-available/code-server
```

配置文件中的内容如下：

```shell
server {
listen 80;

location / {
proxy_pass http://127.0.0.1:8088/;
proxy_set_header Host $host;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection upgrade;
proxy_set_header Accept-Encoding gzip;
}
}
```

其中：

（1）`listen 80`设置对外监听的端口号为80。若只代理这一个服务时，只需要监听默认的80端口即可。但若需要部署多个服务时，则让不同的服务设置不同的端口即可。在使用阿里云、腾讯云等云服务器时，需要在控制台中放开对应的端口。例如，在腾讯云中，默认只有80端口是开放的；

（2）location后的`/`表示首页路径。

（3）location中的proxy_pass即反向代理的服务对应的本地地址。这里与第二部分中的`/lib/systemd/system/code-server.service`中所绑定的地址相呼应。

nginx启动代理服务时，默认是从`/etc/nginx/sites-enabled`文件夹中调用配置文件，因此还需要将配置文件链接到该文件夹中。执行以下命令：

```shell
$ sudo ln -s /etc/nginx/sites-available/code-server /etc/nginx/sites-enabled/code-server
```

通过以下命令测试是否所有的配置文件已经有效：

```shell
$ sudo nginx -t
```

正常情况下，输出以下内容，否则配置文件有错误：

```shell
Output
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

执行以下命令重启nginx服务：

```shell
$ sudo systemctl restart nginx
```

最后，通过`ufw`命令在防火墙中放开80端口：

```shell
$ sudo ufw allow 80
```

这一步执行完毕以后，理论上应该能够通过访问域名或者ip来访问code-server了。若监听端口非80，则通过`ip:端口`或`domain:端口`来访问。

**参考网站：**
https://www.digitalocean.com/community/tutorials/how-to-set-up-the-code-server-cloud-ide-platform-on-ubuntu-18-04-quickstart

