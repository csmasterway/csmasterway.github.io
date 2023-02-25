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



The time consumption to find a new block obeys the exponential distribution. For a  miner, the possibility to find a new block doesn't grow up with the time consumed. However, the time consumption is ten minutes for the whole Bitcoin system on average.



The following are some attack examples.



What will happen when a malicious miner gains an opportunity to issue the next block and package some fake transactions transferring some Bitcoins to the miner from others into the block? These Bitcoins can't be stolen because other sincere miners will be refused the block. Meanwhile, the malicious miner will lose the bock rewards.



What will happen if a malicious miner transfers some Bitcoins to others and issues a new block before the previous block containing a transaction that transferred the Bitcoins to itself subsequently?  If all miners extend the chain along with the block issued by the malicious miner finally, the previous transfer transaction will be revoked. So the receiver will lose these Bitcoins. To avoid the fund risk, the receiver will confirm whether a transaction is successful according to the number of continuous blocks generally, the number is six typically, known as six confirmations. It is extremely difficult for a malicious miner to find continuous six blocks. This possibility can be ignored completely even though it has enormous power.



What will happen if a selfish miner finds a new next block but it doesn't issue, and it extends the chain along with the current block?  In the whole system, others don't know the current block except itself, so others extend the chain along with the origin chain. Assume the following situation, others find a new block but the miner finds two blocks and issued them together, which will make the block found by others invalid because the Bitcoin system is designed to extend along with the longest legal chain. In this way , the competition can be reduced to a certain extent, called selfish mining. But the key to success is that the miner possesses enough power. If a malicious miner finds a new block but doesn't issue it, others also find a new block and issue it before the malicious miner, the miner will lose block rewards.

## Reference

- Bitcoin and Cryptocurrency Technologies A Comprehensive Introduction [Arvind Narayanan, Joseph Bonneau, Edward Felten, Andrew Miller, Steven Goldfeder]
- http://zhenxiao.com/blockchain/