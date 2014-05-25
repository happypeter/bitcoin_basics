
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