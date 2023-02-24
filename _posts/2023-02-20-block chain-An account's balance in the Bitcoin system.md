---
layout: post
title: An account's balance in the Bitcoin system
categories: [Blog, Block Chain]
description: 区块链
keywords: computer networks network layer 
---

An account's balance in the Bitcoin system.

The nature of the Bitcoin system is a decentralized ledger, but the Bitcoin system doesn't record every account's balance explicitly. So, how do you know an account's balance in the Bitcoin system? The account's balance only can be known by calculating. All blocks in the Bitcoin system record two kinds of transactions, one is known as a kind of coin-based transaction and the other as a sort of transfer transaction. Every transaction states where the transferred Bitcoins come from and which account they were sent to. So, you can find all transactions about the account that contain inward and outward transfer transactions. The difference between inward transactions and outward transactions is the balance. Therefore, the Bitcoin system is also called the transfer-based ledger.



The full nodes in the Bitcoin system maintain a data structure famous as the UTXO, unspent transaction output, which is stored in the nodes' memory. Every element in the UTXO records the transaction hash value of generating the output transactions and the position in the transaction, the information will help to locate a UTXO's output, and a node will know the account's balance rapidly. Meanwhile, the designation can defend double-spending. A transaction might have several input and output accounts, so a transaction might need different accounts' signatures provided by the input accounts. Generally, a transaction's input Bitcoins are greater than output Bitcoins, because a little gas fee is essentially paid to miners, which is vital to encourage miners to package transactions and issue the next block. Every transaction might consume a UTXO element and generate several new UTXO elements, therefore, the memory occupied by the UTXO in a full node is increasing gradually. The UTXO elements consumed will be deleted from the UTXO and new UTXO elements will be added to the UTXO.



How do miners get Bitcoins? All miners in the Bitcoin system compete for the right of bookkeeping, once a miner gains the right, it can package the transactions and issue the next block, and it will get the block rewards. The process of getting the bookkeeping right is called mining. The mining is to find a random nonce contained in the block header, which has four bytes. When Hash(block header) is less than or equal to the target and all the transactions are valid, the node can package the transactions list, issue the next block and it will get a reward. Thus, the process is used for proof of work and is also called POW. In the Bitcoin system, the time consumption to find the next block is ten minutes on average. With the increasing mining power, the mining difficulty is also increasing gradually so that the target might be beyond the thirty-two bits unsigned integer range presently. Because of the small search space, miners might not find the nonce qualified for the demand. To solve the contradiction of enormous mining difficulty and very small search space, the system uses some coin base district's bytes in every block that can be written into any content to extend the search space. Thus, the current mining has two circulations, the outer layer circulation is to adjust the coin base district's extra nonce. After the hash value of the Merkle root is calculated, inner layer circulation will be used to compute the nonce in the block header. If a lucky miner finds a nonce that is lower and equal to the target after trying all kinds of nonces, the miner will gain the block rewards. Every attempt to find the nonce is a random experiment called Bernoulli trial with a binary outcome, the nonce is qualified or unqualified. This sequence of Bernoulli trials consists Bernoulli process that is memoryless so that every miner can gain a chance of bookkeeping right fairly according to their power percentage in the whole system. Otherwise, the miners who possess enormous power will gain more opportunities for bookkeeping right because of past work disproportionately. That is unfair to small power miners.

The time to

比特币是基于交易的去中心化账本ledger

区块中记录的交易包括铸币交易和转账交易，但是系统中并没有在某个地方显式地存储每个账户的余额。要想得知某个账户的余额，需要从区块链记录的交易中推算，转入多少和转出多少。全节点维护了一个UTXO的数据结构，Unspent Transaction Output，用来防止双花，因此全节点需要在内存中维护此数据结构，以便快速检测double spending。一个交易可能有多个输出，B拿到后花掉了，不在其中，C在。

这个数据结构中的每个元素要给出产生输出的交易的哈希值和它在这个交易中是第几个输出，这样就可以定位到这一个UTXO中的输出

![image-20230220162642779](https://wendaocsmaster.github.io/images/blog/image-20230220162642779.png)

每个交易要消耗一些UTXO同时又产生一些新的UTXO，所以UTXO所占的内存一般是逐渐增大的。

输入也是可以有多个的，而且这多个输入可能来源于不同的账户，因此一个交易也需要多个签名，每个输入地址都要提供对应的签名。总输入等于总输出

total inputs = total outputs

> if greater gas fee

基于账户的账本模式ETH，account-based ledger

难度太大，搜索空间太小，扩大搜索空间

coinbase 域

真正的mining有两层循环，外层循环调整coinbase域的extra nonce 算出merkle root value 后内层循环调整block header里的nonce

挖矿就是尝试各种各样的nonce，

Bernoulli trial is a random experiment with a binary outcome

Bernoulli process is a sequence of independent Bernoulli trials, memoryless

Poisson Process, 关心出块时间，服从指数分布 exponential distribution

将来还要挖多长时间与过去已经挖了多少时间是无关的， progress free,从任何地方垂直截断，仍然是指数分布，保证了挖矿的公平性。算力大的矿工会有不成比例的优势。

![image-20230220171422730](https://wendaocsmaster.github.io/images/blog/image-20230220171422730.png)

记账权可能落入有恶意的节点手中。 

攻击：获得记账权的节点强行写入

不当得利，扩展最长合法链，回滚交易![image-20230221101022142](https://wendaocsmaster.github.io/images/blog/image-20230221101022142.png)

后续增加数个区块，有恶意的节点获得一次记账权是不够的还需要获得连续的数个记账权。比特币中默认需要等待6个后续的区块，也叫作6个确认。平均等待时间为1h。

![image-20230221101221044](https://wendaocsmaster.github.io/images/blog/image-20230221101221044.png)

selfish mining

获得的记账权暂时保留，不发布，等获得连续的数个记账权后发布，造成最长合法In链，但是需要有恶意的节点有足够多的算力。

## Reference

- Bitcoin and Cryptocurrency Technologies A Comprehensive Introduction [Arvind Narayanan, Joseph Bonneau, Edward Felten, Andrew Miller, Steven Goldfeder]
- http://zhenxiao.com/blockchain/