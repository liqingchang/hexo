---
title: 从零开始搭建Flask+Gunicorn+Nginx环境
date: 2018-03-13 16:04:45
tags: [小程序,python,后台]
---

Nginx是轻量级、高性能的HTTP和反向代理服务器，以非阻塞方式处理请求，在高并发的情况下相比apache有很大的优势。一般情况下，处理静态文件（比如hexo博客）、反向代理使用nginx，动态处理请求的情况下用apache（咨询过专业的后台研发，一般分布式都要用反向代理做，所以直接用nginx就够了，BAT内部的后台都是自己写的，不会用到nginx或者apache）。

Gunicorn是一个python Wsgi http server，只支持在Unix系统上运行，来源于Ruby的unicorn项目。Gunicorn使用prefork master-worker模型（在Gunicorn中，master被称为arbiter），能够与各种wsgi web框架协作。

Flask是一个Python编写的Web 微框架,让我们可以使用Python语言快速实现一个网站或Web服务。

本文以腾讯云上的全新ubuntu系统为例，介绍搭建Nginx+Gunicorn+Flask提供后台服务的过程，后续会补充使用python实现处理后台以及使用微信提供的ide编写前端实现一个简单的小程序的整体过程。

1.准备python环境
<pre>$sudo apt-get update
$sudo apt-get install python-dev python-pip python-virtualenv</pre>

virtualenv是一个可以创建独立python环境的工具，使用virtualenv可以防止python版本不同导致的差异问题并且可以直接拷贝文件夹部署文件，用起来比较方便。virtualenv创建“独立”的Python运行环境的原理是把系统Python复制一份到virtualenv的环境，用命令source venv/bin/activate进入一个virtualenv环境时，virtualenv会修改相关环境变量，让命令python和pip均指向当前的virtualenv环境。
<pre>$:mkdir xiaochengxu
$:cd xiaochengxu
$:virtualenv venv
$:source venv/bin/activate</pre>

未来如果要手动启动服务器的话，都需要先source进virtualenv
然后开始安装flask

<pre>$:pip install Flask</pre>

然后直接上一个Flask的HelloWorld程序验证成果

<pre>$:vi hello.py</pre>

``` python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```

<pre>$:python hello.py</pre>

然后就可以用curl看到Hello World的效果啦

<pre>$:curl localhost:5000</pre>

下面介绍Gunicorn的安装以及配置

<pre>$:pip install gunicorn</pre>

安装完成后直接用以下指令

<pre>$:gunicorn -w 4 -b 127.0.0.1:5000 hello:app</pre>

-w表示worker个数，等同于处理请求的进程数
-b表示bind，表示把服务绑定到127.0.0.1的5000端口
输入后，我们可以同样用curl查看效果，效果跟直接用Flask运作是一样的

<pre>$:curl localhost:5000</pre>

然后我们需要利用Nginx做一个反向代理把请求转到Flask处理
先安装nginx

<pre>$:sudo apt-get install nginx</pre>

进入默认的配置路径

<pre>$:cd /etc/nginx</pre>

然后在conf.d新增一个简单的配置xiaochengxu.conf

<pre>$:cd conf.d
$:vi xiaochengxu.conf</pre>

``` nginx
server {
    listen 80;
    server_name 访问路径比如xiaochengxu.xxx.com;

    location / {
        proxy_pass http://127.0.0.1:5000; # 这里是指向 gunicorn host 的服务地址
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

然后在浏览器上用xiaochengxu.xxx.com看到Hello World就大功告成了

整体配置过程没有出现任何奇怪的错误...总觉得不是什么好事
后续根据[Flask的文档](http://flask.pocoo.org/docs/0.12/)内容用python编写后台处理的代码，然后配合微信做调试就行