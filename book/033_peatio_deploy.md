---
layout: book
title: 貔貅搭建：基本部署过程
---

前面已经申请了服务器也绑定了域名，本期视频里面，我就来就把 peatio 代码部署到服务器之上。参考资料是 [官方给的部署文档](https://github.com/peatio/peatio/blob/master/doc/deploy-ubuntu.md) 。文档上的内容都是精心整理过的，直接执行都是可以成功的。内容很多，本视频中我先把要让程序跑起来的各个基本工具的安装和配置操作一下。其他的工具，等后面演示网站相应部分的功能时再去安装，这样比较能直观的看出它们各自的作用。

本期中会完成下面步骤：

    1. Setup deploy user
    2. Install Ruby
    3. Install MySQL
    4. Install Redis
    7. Install Nginx with Passenger
    10. Configure Peatio # 相关的部分

暂时不做：

    5. Install RabbitMQ
    6. Install Bitcoind
    9. Install ImageMagick


<!-- 本文档中把都运行了那些具体的命令都记录一下吧，便于后面对照，或者重装服务器的话，复现整个场景 -->

除非特殊提出，下面的操作步骤是和文档上 100% 一样的。

### 安装 ruby 语言和数据库

![](http://media.happycasts.net/pic/peterpic/ruby-lang.png)

首先执行 `1. Setup deploy user` 中的内容

    sudo adduser deploy
    sudo usermod -a -G sudo deploy
    su deploy

接下来，执行 `2. Install Ruby`

    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get install git-core curl zlib1g-dev build-essential \
                         libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 \
                         libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties

    cd
    git clone git://github.com/sstephenson/rbenv.git .rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc
    exec $SHELL

    git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
    exec $SHELL

    rbenv install 2.1.2
    rbenv global 2.1.2
    echo "gem: --no-ri --no-rdoc" > ~/.gemrc
    gem install bundler
    rbenv rehash


下面安装数据库 `3. Install MySQL`

    sudo apt-get install mysql-server  mysql-client  libmysqlclient-dev

过程中设置 mysql 的 root 用户密码为：111111 。当然建议你选择一个更为安全的。


再来 `4. Install Redis`

    sudo apt-add-repository -y ppa:rwky/redis
    sudo apt-get update
    sudo apt-get install redis-server

<!-- redis 必须得安装，不然后面要报错 -->

### 安装服务器及其他工具

执行

    7. Installing Nginx & Passenger
    8. Install JavaScript Runtime # 这个是 rails 程序自己要用的

<!-- 不需要 passenger-install-nginx-module 这一步
按照 https://github.com/peatio/peatio/blob/master/doc/deploy-ubuntu.md
安装 passenger 定制过的 nginx -->


### 配置数据库和服务器

也就是这一部分

    10. Setup production environment variable

database.yml 的 `production` 部分改为：

    production:
      <<: *defaults
      database: peatio_production
      password: 111111

<!-- - 修改 push 在 application.yml 和 这个 database.yml 的数据之后，不用重启服务器 后续 rake 命令就可以成功-->

启用后台 deamons 和 “SSL Certificate setting” 先不弄，后面的视频中再演示。 bitcoind 和 Liability Proof 相关操作也一样。

<!--
- bitcoind
  - 第6步中，填入
    - happypeter
    - p111111

- Setup bitcoind rpc endpoint
  - vim config/currencies.yml

      rpc: http://happypeter:p111111@127.0.0.1:18332 -->

准备配置文件，运行 `bin/init_config` 。 Pusher 也是后面再细聊，不过这里的配置文件先修改一下，不然后面程序会运行不起来。
<!-- 缺少 pusher 配置 rake db:setup 这一步会报错 -->

这样打来浏览器访问 peterandbilli.com 可以看到，我自己的这个交易所已经开始运行啦。huhaha...

![](http://media.happycasts.net/pic/peterpic/peatio_shot.png)

### 结语

基本程序运行起来了，不代表所有的功能有已经能够正常使用了，后面的视频中，我会针对具体的功能点来作专门的讲解，敬请期待。

<!-- - 注册，amqp_queue 没有报错，但是我有被重定向到了 http://peatio.dev/settings
  -  shit, why this?
    - https://github.com/peatio/peatio/issues/288
 -->

<!--  添加 peatio.cong 之后 nginx 重启失败
deploy@redcat:~$ sudo service nginx restart
 * Restarting nginx nginx                                                                                                                              [ OK ]
deploy@redcat:~$ sudo rm /etc/nginx/sites-enabled/default
deploy@redcat:~$ sudo ln -s /home/deploy/peatio/current/config/nginx.conf /etc/nginx/conf.d/peatio.conf
deploy@redcat:~$ sudo service nginx restart
 * Restarting nginx nginx                                                                                                                              [fail]

 deploy@redcat:/etc/nginx/conf.d$ nginx -v
nginx version: nginx/1.6.2
deploy@redcat:/etc/nginx/conf.d$ uname -a
Linux redcat 3.13.0-32-generic #57-Ubuntu SMP Tue Jul 15 03:51:08 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux -->
