## 比特币钱包原理 - 转账

这是《比特币钱包原理》系列文章第一篇，咱们从『转账』开始，这是钱包的核心功能。

在比特币系统里转账功能的实现是创建一笔交易（Transaction）并把交易广播到网络上，每笔交易包含若干个输入和若干个输出。输出概念很简单，就是比特币地址，表示这笔交易的比特币要流向哪里。输入当然就是要使用的比特币的来源，这个来源是先前某笔交易的输出，是不是有点绕？咱们一起看看这张示例图：

![tx](https://raw.githubusercontent.com/simon-liu/blockchain-consult/master/images/tx.png)

Transaction1 和 Transaction2 的输出都是 5 个比特币，Transaction3 的 2 个输入分别是 Transaction1 和 Transaction2 的输出，一共是 10 个比特币，输出分别是 6 个、3 个比特币，剩余 1 个比特币作为矿工挖矿所得的交易费，这就是一个完整的交易流程。Transaction1 的输入又是先前某笔交易的输出，如此环环相扣，构成了比特币系统的账本。

Transaction3 第二个输出（3 个比特币）的备注是找零（Change），意思是用户给别人转账 6 个比特币，剩余 3 个作为『找零』还放在自己钱包里，这 3 个比特币对应的输出地址可以是用户钱包里的任何地址，当然为了更好的匿名性，最好新建一个钱包地址。每次转账都可能会创建新的钱包地址（用来存放找零），所以交易后最好备份钱包，以免出现故障导致找零的比特币不能使用。这里又引出另一个问题：每次转账后备份钱包太麻烦，于是社区提出了更高级的钱包方案 [Hierarchical Deterministic Wallets](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)，有兴趣的可以看我的[这篇文章](https://github.com/simon-liu/blockchain-consult/blob/master/articles/bitcoin_wallet_principle.generate_key_and_address.md)。

交易的一个有趣地方是：输入的比特币数量等于输出的比特币数量，那如果输入比输出多呢？多出来的会全部作为交易费奖励给矿工。

有朋友问，顺着交易的输入一直向前找，总会有第一笔交易吧？没错，第一笔交易就是挖矿所得，这笔交易很特殊，没有输入只有输出，输出的地址就是矿工自己的钱包地址，比如这笔交易：[ffeb626a814334761798b3c6057d0952926be129c01778ad650829640522d3eb](https://www.blockchain.com/btc/tx/ffeb626a814334761798b3c6057d0952926be129c01778ad650829640522d3eb)

关于交易还有个疑问：如何证明我可以使用某个交易的某个输出呢？了解非对称加密的朋友很容易想到，其实就是私钥签名，公钥验证，每一笔交易的输入不仅包含先前某笔交易的输出，还要附上对应的签名，矿工用公钥检查签名来确认交易的合法性。

文章结尾加个小插曲。比特币刚诞生的时候，比特币的私钥是用很简单的办法生成的：把用户指定的一个短语做 SHA256 HASH，用 HASH 值来做私钥。这种方法的风险在于很多用户用常见的单词或歌词作为短语生成私钥，黑客用庞大的字典去尝试，结果可想而知，reddit 上就有用户抱怨自己的『歌词钱包』被破解。这里有一个示例，这个钱包地址对应的私钥是『password』单词的 HASH 值：[16ga2uqnF1NqpAuQeeg7sTCAdtDUwDyJav](https://www.blockchain.com/btc/address/16ga2uqnF1NqpAuQeeg7sTCAdtDUwDyJav) 可以发现这个地址每收到一笔钱，几乎立即就被转出，可想而知肯定有黑客在监控这个钱包地址。

喜欢我的文章可以在[这里](https://github.com/simon-liu/blockchain-consult)和我交流。