今天为啥又聊 Merkle Tree 呢？ 这个我们地球上大部分人应该连名字都没有听过。而且也是个比较传统的概念了，是由 Ralph Merkle 这位计算机科学家在很多年前提出的。但是 Merkle 确实涉及到了很有意思的应用，其中一个例子。比特币钱包服务用 Merkle Tree 的机制来作 http://blog.bifubao.com/2014/03/16/proof-of-reserves/。

先说 hash
再说 hash list
最后是 merkle tree

还是从一个问题出发，如何来快速验证一个大的数据块，重新计算整个的 hash 会太慢！

- hash list 是 hash tree 的简化形式，可以说说先
  - 用 hash list 比用一个 hash 的好处是，如果数据有损坏，只需要重新下载那个有损坏得小块就行了。
  - root hash 必须得从一个可信的来源获取，有了这个 hash list 从哪里获取都无所谓了
  - bt 的种子文件就是一个 hash list，这个可以讲讲，大家会感兴趣


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