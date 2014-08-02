edit： 这个题目还是挺难的，回头再弄吧，这周没空了


先从一个小故事开始讲起，前几天见一个朋友，他看见我衣服上挂着一个比特币的胸牌，就跟我说：这就是你的比特币吧？我笑着跟他说，比特币是电子货币，没有真正的硬币或是钞票。也有人说:很多比特币的活动上面，主办方会发一些看上去很像钞票的一个硬卡片，那个应该是比特币吧？其实也不是，那上面只是印的一个比特币地址和能够动用这个地址上面资金的私钥，换句话说上面只是印的比特币的账户名，也不是真正的比特币。

# 一枚比特币到底是什么？

话说最最早，比特币是怎么诞生的呢？故事要说到2009年的时候网上有一个化名叫做中本聪的人发了一篇论文，题目叫做《比特币-一个点对点的电子货币系统》，很多人把这个叫做创世纪论文，比特币从此就诞生了。今天这篇文章就是我的创世纪导读系列的第一期，从另外一个角度来看看庐山，每一期只回答一个具体的问题。今天的内容我觉得是挺有意思的，给大家讲讲，什么叫一个或者说是一枚“比特币”？



### 一个币和一个单位的币是不同的概念

We define an electronic coin as a chain of digital signatures.


### 比特币是怎么存在的？
世上本无币，有的只是交易记录。

There are no bitcoins, only records of bitcoin transactions

Here’s the funny thing about bitcoins: they don’t exist anywhere, even on a hard drive. We talk about someone having bitcoins, but when you look at a particular bitcoin address, there are no digital bitcoins held in it, in the same way that you might hold pounds or dollars in a bank account. You cannot point to a physical object, or even a digital file, and say “this is a bitcoin”.

Instead, there are only records of transactions between different addresses, with balances that increase and decrease. Every transaction that ever took place is stored in a vast general ledger called the block chain. If you want to work out the balance of any bitcoin address, the information isn’t held at that address; you must reconstruct it by looking at the block chain.


在 coindesk 的一篇文章中 <http://www.coindesk.com/information/how-do-bitcoin-transactions-work/> 有这样的话：
如果你给个一个一元钱的硬币，我就有了一个沉甸甸的 coin，那为啥我买了一个
bitcoin 之后却没有给我一个硬币呢？

很多人推断说，比特币就是一串加密数字，任何人见了这串数字之后就拥有了比特币，那么这就很难解释为什么比特币花钱的时候能找零。

本文给大家抽取 bitcoin.pdf 中的相关内容，图解白话一下，什么才是比特币的一个
coin?

不要直接上来就给出最原始的答案，那个答案肯定也不是太好理解，先给出几种类比的简化版的答案。


- http://www.coindesk.com/information/how-do-bitcoin-transactions-work/
  Because bitcoins exist only as records of transactions, you can end up with many different transactions tied to a particular bitcoin address

给大家看看，只有一个输入的交易：https://blockchain.info/zh-cn/tx/75470bffa56e72baf7152691691ebe5549c9575c1ddca41c2f0e1a876f1454f7

多个输入的交易：

Peter，一个地址上到底有几个单位的币很容易查到，但是一个地址上到底有多少个你说的这种真正的币呢？


### 我可以使用非整数个币吗？

可以的，目前比特币最小的分割到小数点后8位，也就是你可以花  0.00000001 个比特币，但是如果未来必要还可以分得更小。