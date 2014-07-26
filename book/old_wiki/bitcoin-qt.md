每次打开都就开始同步十几G的 blockchain，很讨厌
而且默认在 window 下还是往 C 盘同步

看了一下，bitcoin qt 只能导入钱包，不能生成钱包

- https://bitcoin.org/en/choose-your-wallet
  - bitcoin-qt is referred as the "full bitcoin client" on above page

btcoin-qt 也出过这样的问题：老钱包导出之后，交易时找零会生成新的钱包，这时再倒入老钱包，那么那些找零账号就被覆盖了

短线：在线的各种服务好一些，本地的应用，小白使用时候会出各种问题，例如中了木马，或者自己把钱包给删了，


长线：把 private key 打印到纸上，锁在保险柜。生成自己的钱包可以用 bitcoin-qt 来生成，伟伟没有自己试过



bitcoin-qt 再同步 blockchain 的时候会默认宝十几G的文件直接放在C盘，这个要提醒用户


移动端：
官网上有，可以推荐

各种钱包一般都没有找回密码功能！！！