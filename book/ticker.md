views/private/markets/show.html.slim: 中调用了一个 partial： _ticker.html.slim

/Users/peter/toys/peatio-va/peatio/app/assets/javascripts/component_ui/market_ticker.js.coffee


现在我不用关心数据是怎么显示的，代码没有问题，所以只要原始数据进了 rails，那肯定是能正常显示的。
源头，源头在哪里？


- 前端 js 中得 ticker 的数据应该是来自 pusher

      channel = @attr.pusher.subscribe("market-#{gon.market.id}-global")


- pusher 的数据还是来自于自己的 rails app： /Users/peter/toys/peatio-va/peatio/app/models/global.rb

          Pusher.trigger_async(channel, "update", data)

- 到这里，就能很清楚的看出 ticker 的数据来源了

- daemon 中也有 Pusher 相关内容
