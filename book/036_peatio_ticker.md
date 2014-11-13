---
layout: book
title: 貔貅搭建： 心跳数据显示
---

本期介绍交易大厅中的数据心跳（ ticker ）的原理。从后到前走出一条线来，因为创建 trade 需要比较多的准备工作。所以这条线的最后端就从有程序写了 cache 来开始走起。今天的讨论基于 2014年11月13日的代码，后面随着 peatio 的开发，一些 json 接口的数据格式可能会稍作调整。

首先看一下 slide ，了解基本流程。

有了新的交易之后，models/worker/market_ticker.rb 中的

    def update_ticker(trade)
      ...
      Rails.logger.info ticker.inspect
      Rails.cache.write "peatio:#{trade.market.id}:ticker", ticker
    end

会把实时数据写入到 cache 中，现在我来手动写入 cache 数据，也就是在这一点之前发生的，如何创建的 trade，如何 update_ticker 被触发，这些事情这一集里先不关心。本视频只是来关注，实时数据写入 cache 之后，程序读取数据并及时推送到浏览器中显示的这个过程。

启动  rails console 执行

    Rails.cache.write('peatio:btccny:ticker',  {low: 100.2, high: 200.2, last: 1.2, volume: 333})

如果写入报错，那就来看一下 redis 是否在正常运行。

    ps aux|grep redis

否则，参考 peatio 官方文档对 redis 进行安装和启动。


收据写入 cache 之后，就看看读取的代码有没有在正确轮询了。

    ps aux|grep peatio

查看后台任务是否都在正常运行，尤其是 global_state 。如果没有就启动一下

    bundle exec rake daemons:start

下一步，到 daemons/global_state.rb 中添加一条调试语句。


>目前可以确认 models/global.rb 中写入的 ticker 的原始数据，可以通过 pusher 通道，传递到前端显示。

现在动手中保障这一步，也就是保障 pusher 已经在正常工作了。
- 我把 application.yml 中的 pusher 设置都 comment 了，但是 global.rb 中的默认值一样可以显示在页面上。
- 填入 app key 之后，从 pusher.com 的 console 中填写数据，peatio.dev 的前端是有反应的。


- 查看后台进程是否在正常的轮询
  - rake daemon:start 之后，下面的操作每个两三秒就会有输出的

     vagrant@vagrant-ubuntu-trusty-64:~/peatio/log$ tail -f peatio:amqp:market_data.output
    D, [2014-11-12T09:30:23.146402 #8490] DEBUG -- : SlaveBook (btccny) updated
    D, [2014-11-12T09:30:28.149196 #8490] DEBUG -- : SlaveBook (btccny) updated
    D, [2014-11-12T09:30:33.152712 #8490] DEBUG -- : SlaveBook (btccny) updated
    D, [2014-11-12T09:30:38.155199 #8490] DEBUG -- : SlaveBook (btccny) updated


  - 上面的信息来自一个 worker, slave_book.rb

        Rails.logger.debug "SlaveBook (#{market}) updated"

    同样的语句，我可以加到 market_ticker 这个 worker 里面试试



- rails c 中执行这样的语句就可以创建 trade 了
  t = Trade.create(price: 22.2, volume: 22.3, funds: 2222)

