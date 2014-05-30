

- bifubao proof-of-reserves
  - get my account hash 
    hashlib.sha256("happypeter1983@gmail.com0278730000000014990000").hexdigest()

  - 然后我的父节点的 hash 这样得到

			hexstr(
			    first8bytes(
			        sha256(_8bytes(left.sum + right.sum) + _8bytes(left.hash) + _8bytes(right.hash))
			    )
			)

  - 这样一路算上去，是没有任何人能够对节点的余额 sum 造假的，因为没有人可以随意拼凑 sha256 的输入
    - 最终得到 merkle root 的 sum，和比特币冷钱包中的 sum 对照一下，就知道币付宝有没有挪用资金了

  - 反过来讲，如果储户一共存到币付宝 8000 btc，但是币付宝挪用了一千，那么用 7000 这个数做 merkle root
    - 那么是按照一个严格的运算逻辑，绝对不可能反推出 “happypeter1983@gmail.com0278730000000014990000" 这个字符串的 sha256 值的



潘志彪kevin
既然用了merkle tree证明机制，就应该公开tree的部分数据和冷钱包地址，否则根本不具备逻辑完整性，说白了就是忽悠 //@宋欢平:大猫的审计太专业。巴比特我上次报销的钱，他说这个不能报，那个不能报，然后都我自己承担了。
@比特币三六零
BTC360第一次100％准备金审计完成。此次审计在2014年5月25日进行。审计结果为BTC360.com交易平台的比特币有稍多于100％的准备金。担任此次独立审计者的是自由经济信仰者，巴比特专栏作家 @BigChubbyCat 。审计流程及审计者报告PDF，详见http://t.cn/RvbpC69

- IT protection
  或者是开篇说话的时候，就提一句欢迎大家收看这期的knewcoin的###视频，类似这样的话哈
  ![](http://peterpic.qiniudn.com/knewcoin.png)
