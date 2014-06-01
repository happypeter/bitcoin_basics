---
layout: book
title: 比特币的百分百准备金证明-基础篇
---

当我们把比特币存入一个平台（包括比特币交易所，off-chain 钱包等，但是不包括 on-chain 钱包）以后，一个担心就有了。这个平台会不会挪用我们储户的币去做一些有风险的事情呢？所以需要有一种方式可以证明平台手里持有的币不少于所有用户存入的币，这个就是所谓的百分百准备金证明了。今天这个是基础篇，后面还会有进阶内容给大家讲解更为深入的原理和细节。

如果不考虑隐私问题，这个证明就极为简单了。平台公布他们自己的钱包地址，这样我们每个人都可以到 blockchain.info 上查询到他们的资产总额了。然后，再把所有用户账号和他们的余额也都列出来，公布到网上。大家上去看看自己的账号上的余额对不对，这样只要有个别的储户看到自己的余额有问题，就可以说明机构造假了。即便有几个储户是机构的自己人，我们来想一下其实他们说谎也没有什么意义：把自己的余额往多里说，那么最终总账上的钱就不够数了，这不是自己给自己找麻烦吗？往少处说倒是可以，但是就更没有意义了。总之，这种方法从逻辑上是可以证明准备金率是百分百的，只要有大量的用户都来查对自己的余额，那么造假的可能性是没有的。

这种”无隐私“的证明方法，虽然在实际中是不能用的，但实际中的证明机制的确实也是基于这个逻辑。只不过是在此之上又用 Merkle Tree 这样的数学方法来进行了对储户和机构的隐私保护。来一起看一下，哪些隐私，如何保护？

先看看储户有哪些隐私？账号和余额。所以我们用 hash 值来表示，这样既可以验证，因为不同的内容不可能得到相同的哈希，同时也不会暴漏意思，其他用户看到我的哈希也不可能推算出我的信息，因为哈希是个”单向算法“，更多关于哈希的知识，参考我前面的一期《什么是哈希》。

本文里我给大家展示一下这个 Merkle Tree 方法，我做的是个简化版啊，实际使用的方法还有很多细节要注意，下一篇里再给大家填充上。

首先必须要公开的就是机构的冷钱包地址了，这样我们可以看到机构的总账户余额，这个是一切一切的基础，不能当做隐私保护起来。

前面的视频中我们介绍过 Merkle tree 的结构，给定一个 Merkle Tree，只要给我 Merkle root，那么你就不可能对 tree 上的其他节点来进行伪造。  

很多情况下用户的账号就是他们的邮箱，比如 happypeter@example.com，那么对应的余额就是一个数了，34.5 代表我有 34个半的比特币。比如我们现在就成立一家机构叫做 happycoin，目前的用户很少就是下面六个人：

billie@example.com  34.3
satoshi@example.com  3
happypeter@example.com 10001
nils@example.com  0 
jeff@example.com  50 
wladimir@example.com  600
greg@example.com  2.718282  
  

这样我给大家整个的算一遍这个 Merkle tree 啊，哈希算法，我们就采用 sha256 了，ready? 走起！

 

>>> hashlib.sha256("billie@example.com34.3").hexdigest()
'bf8f58d92a7b8fe14cc4f4f98b371019eff5f1f32cbc6a7cd7ba18752a62aea1'
>>> hashlib.sha256("satoshi@example.com3").hexdigest()
'a4c86f4a299329cdd3c36c7f2b849ba9932340297bf0be80fc5e0b661eada948'
>>> hashlib.sha256("happypeter@example.com10001").hexdigest()
'942f02635462859ccf47bfe6c60df5550846737f897fb0dafc9727b5bc32747f'
>>> hashlib.sha256("nils@example.com0").hexdigest()
'7f42b833e4821c318fc7ba8a687e493e9417d19dd4599912328c918bc9928cec'
>>> hashlib.sha256("jeff@example.com50").hexdigest()
'8147db65d4660351aca44eba15a1cb64ce625dce743cd3f2cc3f73a1d3aca896'
>>> hashlib.sha256("wladimir@example.com600").hexdigest()
'b2757cc85f4784c88f41398535e36931d4f5aa9ab8ef5d33007c1c2f765f0d3e'
>>> hashlib.sha256("greg@example.com2.718282").hexdigest()
'ed1eb92b03418c5fb66a5159d24515b8e04440f6bdde87806f616d0e2264e5d2'


父节点余额等于左节点余额加上右节点的余额
    parent.sum = left.sum + right.sum = 2.718282 + 1 = 3.718282

父节点的哈希，这样算：
父节点余额，左边的哈希，右边的哈希拼成一个字符串，然后对这个字符串运算哈希：

parent.hash = sha256( concat( parent.sum, left.hash, right.hash ))


这棵树最终算出来之后，如果 sotoshi 想要来进行验证，我就只需要给他，他自己的 path to root 就可以了，这样平台总的用户数就被隐藏了，当然有心的用户还是可以通过 Merkle tree 的高度来推算大概的平台用户数，但是这时我们可以考虑用非平衡数的形式来解决这个问题。
展示一下，我的 https://www.bifubao.com/tree


这里大家了解了一个简化版的 Merkle Tree 验证方法的基本逻辑，下一篇跟大家一起分析一下这样的做法有什么好处，另外有什么缺点。


- IT protection
  或者是开篇说话的时候，就提一句欢迎大家收看这期的knewcoin的###视频，类似这样的话哈
  ![](http://peterpic.qiniudn.com/knewcoin.png)


###  参考：
- <https://iwilcox.me.uk/2014/proving-bitcoin-reserves>