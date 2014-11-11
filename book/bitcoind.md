---
layout: book
title: 貔貅搭建：bitcoind
---

- 设置 bitcoin.conf 中得 rpcuser & rpcpassword 应该就是瞎填一个就行了，因为现在 bitcoind 自己是 sever 所以这个信息应该是给请求方用的。


- 可以工作的命令

    - bitcoind getinfo
    -

- rpc 成功啦

      deploy@redcat:~$ bitcoin-cli -rpcuser=happypeter -rpcpassword=p111111 -rpcconnect=127.0.0.1 -rpcport=18332 getinfo
      {
          "version" : 90300,
          "protocolversion" : 70002,
          "walletversion" : 60000,
          "balance" : 0.00000000,
          "blocks" : 304936,
          "timeoffset" : 0,
          "connections" : 9,
          "proxy" : "",
          "difficulty" : 1.00000000,
          "testnet" : true,
          "keypoololdest" : 1414457663,
          "keypoolsize" : 101,
          "paytxfee" : 0.00000000,
          "relayfee" : 0.00001000,
          "errors" : ""
      }

  - 用户名密码都是我在 bitcoin.conf 中自己设置的，对上就行
  - 本机操作，也可以不用 rpc，`bitcoin-cli  getinfo` 得到的结果是一样的
  - 本地向服务器发请求：

        bitcoin-cli -rpcuser=happypeter -rpcpassword=p111111 -rpcconnect=128.199.196.148 -rpcport=18332 getinfo

    不行，因为默认就是不让其他机器访问的

        Bitcoin disallows rpc requests from IP addresses other than 127.0.0.1 Try adding

            rpcallowip=<your ip addres>

        to your bitcoin configuration file.
