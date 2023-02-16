---
layout: post
title: Cryptography hash knowledge
categories: [Blog, Block Chain]
description: 区块链
keywords: computer networks network layer 
---

Cryptographic hash function

- Collision resistance 

  ```
  If x != y, then
  	Hash(x) != Hash(y)
  ```

- Hiding

  If hash(x) is known, but x can not be derived from the hash(x) because Hash(x) doesn't reveal any information about x. In other words, we can only derive x by brute force. For example, hash each value in the entire fetch space.

These characteristics can be used to implement digital commitment or the digital equivalent of a sealed envelope. But the hiding is conditional on the value space being large enough and every value equally distributed throughout the entire fetch space. Usually, if the input data is not qualified for the condition, we can add a random nonce, then hash them together, such as `Hash(X||nonce)`.

- Puzzle friendly

  The calculation of the hash value is unpredictable. If we get an input value x, we can not know Hash(x) except for calculating the Hash(x). In others words, If the hash you want to calculate falls within a certain range, there is no good method but to try each input data. Then compare the calculated value and the qualified range. 

  The mining is to find a random nonce contained in the block header, when Hash(block header) is less than or equal to the target, the miner will get a reward. Thus, the process is used for proof of work and is also called POW. However, if we know the nonce, everyone can prove it easily by calculating hash value—— Hash(block header). This characteristic is also known to be difficult to solve, but easy to verify.

The hash function in Bitcoin is called SHA-256, an asymmetric encryption algorithm.

The public key is used to encrypt messages and the private key is used to decrypt messages. For example, two people need to communicate with each other on the Internet, however, the Internet is public and not secure enough. How can they communicate with each other on the Internet? One is to negotiate an encryption method which is called symmetric encryption. But the process of negotiating is also not secured. The other method is to adopt asymmetric encryption technology, they can exchange their own public key on the Internet which can be made public anytime and anywhere. when a sender is ready to send messages to a receiver, it can encrypt the messages with the receiver's public key. Only the receiver with the private key can decrypt the messages. This communication method solves the problem of inconvenient secret key distribution. Anyone can steal communication information from both parties, but the encrypted messages are garbage to them. Only the two communication parties who possess the private key can decrypt the messages.

How to create a Bitcoin account? We can generate a public key and private key pair. The public key is equivalent to a bank account and the private key is equivalent to the bank account's password. The private key is used to sign because only the account owner possesses the private key. When a transfer message with the private key signature is published to the blockchain, other miners can use the publisher's public key to prove the legitimacy of the transaction.

### Reference

- Bitcoin and Cryptocurrency Technologies A Comprehensive Introduction [Arvind Narayanan, Joseph Bonneau, Edward Felten, Andrew Miller, Steven Goldfeder]
- http://zhenxiao.com/blockchain/
