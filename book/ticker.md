views/private/markets/show.html.slim: 中调用了一个 partial： _ticker.html.slim

/Users/peter/toys/peatio-va/peatio/app/assets/javascripts/component_ui/market_ticker.js.coffee

- 前端 js 中得 ticker 的数据应该是来自 pusher

      channel = @attr.pusher.subscribe("market-#{gon.market.id}-global")
