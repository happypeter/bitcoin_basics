---
layout: book
title: 貔貅搭建：基本部署过程
---

前面已经申请了服务器，本期视频里面，就把 peatio 代码部署到服务器之上。参考资料是 [官方给的部署文档](https://github.com/peatio/peatio/blob/master/doc/deploy-ubuntu.md) 。


### 安装 ruby 语言和数据库

按照文档上的这几步直接执行就可以了，没有问题

    1. Setup deploy user
    2. Install Ruby
    3. Install MySQL
    4. Install Redis

<!--  包括 rbenv 等等就都不细说了，懂的人自然懂，不懂的人说也更晕 -->

<!-- redis 必须得安装，不然后面要报错 -->

### 安装后台工具

下面安装

    5. Install RabbitMQ -- 系统上各个部分功能模块之间进行通信
    6. Install Bitcoind -- 提供比特币相关的各种服务

<!-- 不需要 passenger-install-nginx-module 这一步
按照 https://github.com/peatio/peatio/blob/master/doc/deploy-ubuntu.md
安装 passenger 定制过的 nginx -->


### 安装配置服务器

- nodejs/imagemagick 都装上吧

- pusher 相关的设置要是不改，后面运行不起来吧
  - 不行，必须得有

- bitcoind 的设置，先不弄

- config database.yml 这个是必须改了
  - 修改 push 在 application.yml 和 这个 database.yml 的数据之后，不用重启服务器

        bundle exec rake db:setup

     就可以成功

- 不启动 deamon ，行不行？
  - 可以
  - 对应的是一个具体功能，网站的基本运行用不着，这个等到相应的部分再安装

- SSL Certificate setting 这个先不弄
  - 可以

- passenger & nginx

- Liability Proof 是啥玩意？
  - 先不弄

- 现在 nginx 已经正常运行了，但是报错了，到哪里看 log 呢？
  - 就直接看 log/production.log 就行了
  - redis 没装造成的

- 试着来注册
  - 又 error 了
  - amqp_queue 的问题，这个是个大话题了，后面再讲吧

---

上面那些“先不装”的东西，步骤很麻烦吗？不然的话，要不给装上
？

- bitcoind
  - 整个过程看起来是没有问题的
    - happypeter
    - p111111

- Setup bitcoind rpc endpoint
  - vim config/currencies.yml

      rpc: http://happypeter:p111111@127.0.0.1:18332

-  deamon start done

- 总之，这些步骤是都能执行成功的。
- 注册，amqp_queue 没有报错，但是我有被重定向到了 http://peatio.dev/settings
  -  shit, why this?
    - https://github.com/peatio/peatio/issues/288
