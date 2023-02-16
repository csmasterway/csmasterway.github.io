---
layout: post
title: Cryptography hash knowledge
categories: [Blog, Block Chain]
description: 区块链
keywords: computer networks network layer 
---

Cryptographic hash function

+ Collision resistance 

  ~~~
  If x != y
  	Hash(x) != Hash(y)
  ~~~

+ Hiding

  If hash(x) is known, but x can not be derived from hash(x) because Hash(x) doesn't reveal any information about x. In other words, we can only derive x by brute-force. For example, hash each value in the entire fetch space.

These  characteristics can be used to implement digital commitment or digital equivalent of a sealed envelope. But the hiding is conditional on the value space being large enough and every value equally distributed throughout the entire fetch space. Usually, if the input data is not qualified the condition, we can add a random nonce, then hash them together, such as `Hash(X||nonce)`.

+ Puzzle friendly

  The calculation of hash value is unpredictable. If we get an input value x, we can not know Hash(x) except for calculating the Hash(x). In others words, If the hash you want to calculate falls within a certain range, there is no good method but to try each input data. Then compare the calculate value and the qualified range. 

  The mining is to find a random nonce which is contained in  the block header, when Hash(block header) is less than or equal to target, the miner will get reward. Thus, the process is used for proof-of-work and is also called POW. However, if we know the nonce, everyone can prove it easily by calculating hash value—— Hash(block header). This characteristic is also known to be difficult to solve, but easy to verify.

The hash function in the bitcoin is  called SHA-256, a asymmetric encryption algorithm.

The public key is used to encrypt messages and private key is used to decrypt messages. For example, there are two people need to communicate with each other on the Internet, however, the Internet is public and not secured enough. How can they communicate with each other on the Internet ? One is to negotiate an encryption method which is called symmetric encryption. But the process of negotiating is also not secured. The other method is to adopt asymmetric encryption technology,  they can exchange their own public key on the Internet which can be made public anytime and anywhere. when sender is ready to send messages to receiver, it can encrypt the messages with receiver's public key. Only receiver with the private  key can decrypt the messages. This communication method solves the problem of  inconvenient secret key distribution. Anyone can steal the communication information from both parities, but the encrypted messages are garbage to them. Only the two communication parities who posses the private key can decrypt the messages.

How to create a bitcoin account ? We can generate a public key and private key pair.  The public key is equivalent to bank account and the private key is equivalent to password of the bank account . The private key is used to sign because only the  account owner posses the private key. When a transfer message with the private key signature is published to the blockchain, other miners can use publisher's public key to prove the legitimacy of the tansaction.

### Reference

+ Bitcoin and Cryptocurrency Technologies A Comprehensive Introduction [Arvind Narayanan, Joseph Bonneau, Edward Felten, Andrew Miller, Steven Goldfeder]
+ http://zhenxiao.com/blockchain/
