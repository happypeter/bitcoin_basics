---
layout: book
title: 貔貅搭建：拥有域名和服务器
---

peatio 是一个开源的加密货币交易所。在 github 上可以找到它的 [源代码](https://github.com/peatio/peatio) 。但是要搭建我自己的交易所，除了要有貔貅这样的交易所代码，还要有两个东西才行。第一，要有一台网络服务器，带公网 IP 的。第二，还要有一个好听好记的域名。这一期我就带你去 digitalocean 上买一台服务器，再去 godaddy 上面买一个域名，把域名指向服务器。总之就是要在浩瀚网络空间中，给貔貅安个家。

![](http://happypeter.github.io/bitcoin_basics/img/peatio_vps_domain.png)


### digitalocean 购买服务器

首先注册。到 <https://cloud.digitalocean.com/droplets> 页面上可以看到主要的步骤。到 billing ---> manage payment 下面可以找到 add credit card 的按钮，添加信用卡信息。之后，就可以来创建一个 droplet ，也就是一个服务器了。点击创建按钮。这样可以首先来设置服务器的主机名，填一个自己喜欢的就好，比如 redcat 。

[peatio 官方文档](https://github.com/peatio/peatio/blob/master/doc/deploy-ubuntu.md)建议的操作系统为 ubuntu 14.04，我这里使用一个64位版本 。同时，运行 peatio 建议的内存大小是4G 。选择新加坡机房，国内用会比其他地方的稍微快一些。

![](http://happypeter.github.io/bitcoin_basics/img/vps_settings.png)

<!-- ping 出来的结果也不可信，网站运行速度跟很多因素有关，CPU 内存 是不是 SSD 等等，所以不要以 ping 为标准，要以实用为依据，网页打开速度是不是可以接受，可以不可以播放视频？ ssh 登陆后反应慢不慢？如果实际用着没有问题，那么 ping 出来是 400ms 还是 70ms 都无所谓

ping digitalocean sinapore 120ms  ping linode tokyp 80ms

    ping railscasts.com(digitalocean US) 400ms but still play video OK， so this just does not matter


   minimum ram: 4G
  https://github.com/peatio/peatio/issues/281 -->


### godaddy  购买域名并更改域名服务器

godaddy 上面是可以用支付宝的，当然也可以用信用卡或其他方式支付。购买过程很简单，不做演示了。下面来看看，怎样把这个域名指向我刚才申请的服务器。

有了服务器之后，还需要给它绑定一个域名。参考 [如何给一个 VPS 绑定域名](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean) 。文中提到了 DO 的域名服务器是这三个

    ns1.digitalocean.com
    ns2.digitalocean.com
    ns3.digitalocean.com

在 godaddy 我的个人账户页面上，点击 domains ，找到要绑定的域名 peterandbillie.com 然后 lanuch action 打开设置页面。其实这里要做的就是一个事情，把 nameservers 改为使用 DO 的域名服务器。

### 回到 digitalocean 设置域名服务器

到 DNS 选项卡下，添加 domain，填入主机名 redcat 以及它的 IP，然后 name 这一项填入 `peterandbille.com` ，点击 create domain 就可以了。

![](http://happypeter.github.io/bitcoin_basics/img/do_dns.png)

过了3个小时，到终端中执行

    ping peterandbillie.com

可以看到域名以及指向服务器的 IP 了，Yeah...

<!-- 刚刚到 DO 添加 peterandbillie.com
    现在是下午四点半，看看多长时间能生效？
    - 还不到六点，好了 -->
