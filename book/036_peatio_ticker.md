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

下一步，到 global.rb#ticker 中添加一条调试语句。

    Rails.logger.info ticker.inspect

这样，需要重启 daemons 才能生效。这样到 tail -f log/production.log 中可以看到 `ticker` 中存放的内容，并且会
看到每隔3秒会重新执行一次。这是因为 global.rb#ticker 是在 daemons/global_state.rb 中一直轮询执行的。


### pusher 相关

同时也会被每三秒执行一次的还有 global.rb#trigger 其中的关键语句是把心跳信息发送到 pusher.com

    def trigger(event, data)
      Pusher.trigger_async(channel, event, data)
    end

所以现在来配置一下 pusher，到我自己的 Pusher 账号，新建一个应用叫 peterandbillie.com 拿到 app_id&key&secret 三项信息，填入
application.yml 的相应位置。然后要 `touch tmp/restart.txt` 重启服务器。

这样到 pusher.com 的 debug console 我就可以看到每隔三秒收到了新的信息了。同时 <http://peterandbillie.com/markets/btccny> 也就是交易大厅页面也会看到信息更新了。

再次到 rails console 中写入其他数据到 cache，可以看到页面上又了及时的变化。


