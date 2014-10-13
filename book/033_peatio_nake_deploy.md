---
layout: book
title: 貔貅：基本部署
---

[官方给的部署文档](https://github.com/peatio/peatio/blob/master/doc/deploy-ubuntu.md) 上的步骤拷贝粘贴到终端中执行就好了。不用细说。

用几个 slide 把内容隔开：

- 安装 ruby 语言
  - 包括 rbenv 等等就都不细说了，懂的人自然懂，不懂的人说也更晕

- redis 必须得安装，不然后面要报错

- 这样就到了 “Nginx & Passenger”

<!-- 不需要 passenger-install-nginx-module 这一步
按照 https://github.com/peatio/peatio/blob/master/doc/deploy-ubuntu.md
安装 passenger 定制过的 nginx -->

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
