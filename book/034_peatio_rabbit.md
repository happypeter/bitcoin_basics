---
layout: book
title: 貔貅搭建：邮件发送机制
---

好书接上文。前面基本跑起来了 peatio，但是这不表示每个功能都能用了。所有这次继续以让不能用的功能变得能用为主线，来演示搭建貔貅的过程。


创建用户，tail -f 报错

    E, [2014-10-22T03:17:11.904023 #4676] ERROR -- : Could not establish TCP connection to 127.0.0.1:5672:
    E, [2014-10-22T03:17:12.023875 #4676] ERROR -- : Unable to enqueue :mailer: Could not establish TCP connection to 127.0.0.1:5672: , fallback to synchronous mail delivery
    I, [2014-10-22T03:17:12.042747 #4676]  INFO -- : Completed 500 Internal Server Error in 436ms
    F, [2014-10-22T03:17:12.047003 #4676] FATAL -- :
    ArgumentError (Missing host to link to! Please provide the :host parameter, set default_url_options[:host], or set :only_path to true):


### 先来保证同步的可以发出去

到 production.rb 文件中

application.yml 文件中填入

      SMTP_PORT: 587
      SMTP_DOMAIN: sandbox591c991b6ae54699941132d085beb2ed.mailgun.org
      SMTP_ADDRESS: smtp.mailgun.org
      SMTP_USERNAME: postmaster@sandbox591c991b6ae54699941132d085beb2ed.mailgun.org
      SMTP_PASSWORD: 1xxxxxxxxd1e802d9c742016ea77e0

      SYSTEM_MAIL_FROM: system@peterandbillie.com
      SYSTEM_MAIL_TO: group@peterandbillie.com

<!-- SMTP_PASSWORD: 131e1421ad1e80322d9c742016ea77e0 -->



### 再来实现后台异步发送邮件

异步操作是这样一个流程：

【图]


- 安装 rabbitmq
  - 就是官方文档上的步骤
  - 涉及到了 rabbitmq_management 回头介绍一下使用

- 启动 daemons





