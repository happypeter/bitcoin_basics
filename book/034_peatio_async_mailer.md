---
layout: book
title: 貔貅搭建：邮件发送机制
---

好书接上文。前面基本跑起来了 peatio，但是这不表示每个功能都能用了。这次继续以让不能用的功能变得能用为主线，以功能为单位继续演示搭建貔貅的过程。关注邮件发送。


来到页面，创建用户，`tail -f log/production.log` 报错

    E, [2014-10-23T04:36:09.680510 #11084] ERROR -- : Could not establish TCP connection to 127.0.0.1:5672:
    E, [2014-10-23T04:36:09.739548 #11084] ERROR -- : Unable to enqueue :mailer: Could not establish TCP connection to 127.0.0.1:5672: , fallback to synchronous mail delivery
    I, [2014-10-23T04:36:09.761065 #11084]  INFO -- :   Rendered token_mailer/activation.text.erb (1.5ms)
    I, [2014-10-23T04:36:09.821529 #11084]  INFO -- :
    Sent mail to aa@aa.com (56.5ms)
    I, [2014-10-23T04:36:09.826371 #11084]  INFO -- : Completed 500 Internal Server Error in 289ms
    F, [2014-10-23T04:36:09.829332 #11084] FATAL -- :
    SocketError (getaddrinfo: Name or service not known):
      app/models/amqp_queue.rb:94:in `deliver!'
      app/models/amqp_queue.rb:88:in `rescue in deliver'
      app/models/amqp_queue.rb:84:in `deliver'


### 先来保证同步的可以发出去

报错信息中可以看出是邮件的问题，所以先来配置邮件服务。我这里选择的服务是 mailgun 的免费方案，当然 mailgun 对国内的 qq 邮箱支持不太好，所以可能你需要选择国内的某个邮件服务。

<!--  开发模式下调试邮件有技巧

https://github.com/peatio/peatio/issues/170

If it is running under development mode, it would not sending mail out, but you can review all sent mail through browser and the url is /mails

 -->


application.yml 文件中填入

      # below settings only in production env
      # system notify mail settings
      # --------------------------------------------------------------
      SMTP_DOMAIN: peterandbillie.com
      SMTP_ADDRESS: smtp.mailgun.org
      SMTP_USERNAME: postmaster@peterandbillie.com
      SMTP_PASSWORD: e748325xxxxxxxxxa6d79b8f1cfbc
      SMTP_PORT: 587

      SYSTEM_MAIL_FROM: system@peterandbillie.com
      SYSTEM_MAIL_TO: group@peterandbillie.com

<!--
production.rb 中还用到了：

:port                 => ENV["SMTP_PORT"],

但是 application.yml 中没有这个环境变量，得给他们提一个 Bug

-->


另外还要填写

      URL_HOST: peterandbillie.com

这些变量都会在 production.rb 的 mailer 设置中用到。 重启服务器

    touch tmp/restart.txt

这次收到邮件了。

<!--
     alias cleandb="bundle exec rake db:reset db:migrate"
 -->

<!--
mailgun/peterandbillie.com
SMTP_PASSWORD: e748325e8020e3f87b7a6d79b8f1cfbc
-->

不过这样发送虽然成功了，用户体验不太好，用户点击按钮之后需要等待一会儿。在 log 中也可以看到

    E, [2014-10-23T05:11:20.434603 #12150] ERROR -- : Could not establish TCP connection to 127.0.0.1:5672:
    E, [2014-10-23T05:11:20.568388 #12150] ERROR -- : Unable to enqueue :mailer: Could not establish TCP connection to 127.0.0.1:5672: , fallback to synchronous mail delivery
    I, [2014-10-23T05:11:20.611862 #12150]  INFO -- :   Rendered token_mailer/activation.text.erb (3.0ms)

所以，世界本应是异步的，进入下一步。

### 再来实现后台异步发送邮件

异步操作是这样一个流程：<!-- https://github.com/happypeter/bitcoin_basics/issues/41 -->

![](http://media.haoduoshipin.com/pic/peterpic/async_mail.png)

主要需要两个操作：

第一步，安装 rabbitmq 就是按照 [官方文档上的步骤](https://github.com/peatio/peatio/blob/stable/doc/deploy-ubuntu.md#5-install-rabbitmq) 装好之后

    ps aux|grep rabbitmq

看到相应的进程正在运行，那这一步就完事了。


第二步，启动 daemons

执行

    bundle exec rake daemons:start

可以使用

    ps aux|grep peatio

来查看后台进程起来没有。

全部做好后，清空一下数据库

    bundle exec rake db:reset db:migrate

再次使用 happypeter1983@gmail.com 来注册一下。这次反应就快多了。

到邮箱之中，打开邮件完成一下激活，操作没有问题。

好，这一集就是这些了，拜拜！
<!-- https://github.com/peatio/peatio/issues/298 can be helpful for future ep -->
