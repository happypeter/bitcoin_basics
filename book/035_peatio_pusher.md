---
layout: book
title: 貔貅搭建：pusher 实时信息推送
---

随着后台交易数据的改变，貔貅前端的数据，例如交易大厅里的这些数据 <https://yunbi.com/markets/btccny> 会自动实时刷新。这个的实现是采用 pusher。本级来把 pusher 功能单独抽出来讲一讲，理清一下思路，展示一点调试技巧。不涉及实质性的部署步骤，所以如果你已经对 pusher 很熟悉了，可以略过这个视频。

### 注册账号

来到 pusher.com 创建一个账号，如果只是测试目的，可以先不付费，免费版每天最多可以发十万条信息。登陆进来创建应用，这里就不叫 Peatio 了，叫 pusher_demo 吧。

这样就可以拿到 app key&secrect 等各项内容了。

### 安装 gem

现在服务器上启动一个全新的 rails 应用，让它跑在 development 模式下。我本地笔记本上测试的时候，从后台 ruby 代码中向 pusher.com 发数据的时候总会出现连接超时，但是在服务器上就没有这个问题了。这样第一步，来到 application.rb 中添加这样几行：

    config.generators do |g|
        g.assets false
        g.helper false
        g.test_framework false
    end

这样运行 rails generators 的时候就不会生成那些我不要的文件了。

添加

    gem 'pusher'

到 Gemfile 之中。运行 bundle 命令进行安装。(需要重启服务器吗？)

新建文件  config/initializers/pusher.rb 其中添加这些内容

    Pusher.app_id = '94573'
    Pusher.key = 'd2c10aba8ee2d9de4da5'
    Pusher.secret = '2ee92e8ef3589712e436'
    Pusher.host   = 'api.pusherapp.com'
    Pusher.port   =  80

创建 Trade controller

    rails g controller trade new

到  views/trade/new.html.erb 中，添加这些代码

    <%= form_tag("/trade", method: "post") do %>
      <%= label_tag(:price, "New Price:") %>
      <%= text_field_tag(:price) %>
      <%= submit_tag("Submit") %>
    <% end %>

就是要提交一个最新交易到服务器，信息只有一项就是 price ，这时候需要到 route.rb 中作相应修改

    root 'trade#new'
    post 'trade' => 'trade#create'

添加上面两行，沿着第二行，就来到 trade_controller.rb，添加

    def create
      Pusher.trigger('price_channel', 'update', {price: params[:price]})
      redirect_to :root
    end

其中 `price_channel` 是要在 pusher 服务器上创建的通道的，`update` 是事件名。这样价格数据传到了 pusher 的服务器上。到 pusher 的网站上，打开 debug console 页面，就可以看到发送过来的数据了。但是发现发送过程还是要花一些时间的，这是因为 `Pusher.trigger` 会等待 pusher 服务器的 response，但是事件上这里是不需要返回什么相应的，所以可以改为 `Pusher.trigger_async` 。


收到数据后， pusher 就会及时的把信息推送到前端页面来显示。但是现在还没有来得及写页面。现在就来写。

添加 public/market.html

    <!DOCTYPE html>
    <html>
    <head>
      <title>Pusher Test</title>
      <script src="//js.pusher.com/2.2/pusher.min.js" type="text/javascript"></script>
    </head>

    <body>
      <span id="price"> </span>

      <script type="text/javascript">
        // Enable pusher logging - don't include this in production
        Pusher.log = function(message) {
          if (window.console && window.console.log) {
            window.console.log(message);
          }
        };

        var pusher = new Pusher('d2c10aba8ee2d9de4da5');
        var channel = pusher.subscribe('price_channel');
        channel.bind('update', function(data) {
          document.getElementById("price").innerHTML = data.price;
          console.log(data);
        });
      </script>
    </body>
    </html>

注意，步骤基本就是 a) 创建 pusher b) 连接 channel c) 接收事件

打开一个新的 chrome 窗口，来看一下效果。这边有人提交新的交易，那么所有打开 market 页面的人，不用刷新页面就能看到最新的价格了。

到这里，要演示的功能就结束了。

在来分析一个小技巧，就是可以不从后台 ruby 发数据，而在 puser 网站的 debug console 中直接发数据。这个可以辅助调试。
