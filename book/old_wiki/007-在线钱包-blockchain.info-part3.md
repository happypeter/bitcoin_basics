- 本集讨论 blkchn 的安全性问题，先说优势，再说说不足以及解决方案

- 优势
  - blcchn 并没有在服务器上存储我们的 prvkey，存的是加密的。
    - 在 `import/export` 标签下面可以看到加密的 private key，服务器上存的就是这个
    - 客户端钱包要涉及到钱包备份等问题，很容易自己把钱包弄丢，或者由于操作不慎用老钱包的备份文件覆盖了新的内容
      - blockchain.info 这里会有相应地备份策略的

    - 手机客户端虽然方便，但是一旦手机丢了，嘿嘿
    - 但是也可以 `export unencrypted` 可见必须要用你自己的账号登陆之后才能看到，一切都发生在你自己的笔记本上面
 
  - 双保险安全选项
    - 注册之后要开启 google authenticator
      - account settings -> passwords
    - 二级密码（转账密码）需要设置
      - account settings -> passwords

  - 很多人都在用这个，所以如果有系统漏洞会被及时的发现的： https://blockchain.info/charts/my-wallet-n-users

- 不足
  - 在线钱包只是个钱包，你不会一次把一万块钱放进自己的钱包吧
  - 长期持有，我会介绍纸钱包

---
revision notes:

https://github.com/happypeter/bitcoin_basics/issues/12