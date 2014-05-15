---
layout: book
title: 有意思的数字签名
---


前面我们曾经提过的互联网上的加密通信。收信人把自己的公钥发送给发信人，发信人用公钥加密信息后发送给收信人，这样，收信人用私钥来解密信息后就可以读到信了。这个过程应该说是简单而巧妙，基于的一个技术就是，公钥加密过的信息，是可以通过prvkey来解密的。这也就是为什么我们也会把公钥叫做“加密钥”，而把prvkey叫做“解密钥”了。但是，逻辑之美，美在对称，其实图景的另一半是：prvkey也可以加密文件，加密后可由公钥来解密。读到这里你肯定会奇怪：我为何要加密一个文件，而这个文件却是地球人都可以解密的（公钥是公开的，对公钥和prvkey不了解的话，可有空看看我的《有意思的加密通信》那篇文章）？

这个的实际应用就是数字签名。


- 蜡封的比喻也是值得讲
- prvkey签名不但可以证明文件是谁发出的，而且可以保证文件从未被改动过
- 比特币，实际应用的例子

### 参考：
- <http://en.wikipedia.org/wiki/Digital_signature>
-  <https://www.khanacademy.org/economics-finance-domain/core-finance/money-and-banking/bitcoin/v/bitcoin-digital-signatures>
   - 签名不仅仅是你的名字，还有那份文件，是二者的一个结合
   - SK（签名钥匙）就是 Private key