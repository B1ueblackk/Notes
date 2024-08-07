## Assessment Criteria

quiz1 & quiz2: 30% each; about 20 m-choices and some short-answereds; on paper/close-booked

* quiz1: 19 aug
* quiz2: 9 sep
  term paper: 40%; about 3 weeks from the end of the course
* survey on blockchain tech.....
* max 7 least 4-5

# Lec1

Fiat Money

## Centralized Transaction

某个媒介作为中心，记录交易
如A从B处买物品，A通过银行向B汇款，汇款记录被存于银行

## Decentralized Ledger

A从B处买物品，A向B的地址转移一串token，代表A所付的价值，这笔交易由A的私钥进行签名；交易被记录于公共交易日志；B可以通过他的私钥来使用该token

## Hash Function

feature:

* one-way
* easy to calculate
* hard to invert

## Hash pointer

can be used to verify the info has been changed or not

Blockchain: many hash pointers link together
the more previous, the safer -- the attack should make the whole chain matched, so the more recent block will be less secure than the previous ones

public & private key

## Goofycoin

* goofy is god, he is in charge of all the coins' creation
* the spending of coins will be broadcasted to all by “**Pass on this coin to X**” 
* anyone can verify the transaction

**Why goofycoin still has the double-spending?**
Goofycoin does't limit whether a block can be added to another. That is to say, several blocks that implies the spending of the same coin can be inserted to the definition block of the coin, which will cause double-spendings. If there is a rule that a block can only be added to one specific block, there will no double-spendings.

## Proof of Work

Because of the **collision resistance**, correct hash block can be regarded as the outcome of hard work

Miners work hard to find the value of nonce. Nonce is used to calculate the hash together with the info of the block

### Mining

#### Mining difficulty

increase gradually

#### Mining Revolution

* CPU mining
* GPU mining
* FPGA mining
* ASIC mining

#### Mining pools

miners group together to mine

* pros: reduce the variance of the rewards; easy to upgrade the network
* cons: actually centralized, and the manager musted be trusted

### PoW Problems

* the cost of energy on mining makes the currency unsustainable
* the original goal is decentralization, but the large-scale mining pools are against the intention
* with the increase of transaction, the process speed of Pow becomes the bottleneck. It will take a long time to confirm the transaction, which brings users bad experiences.

### Alternatives to PoW

* PoS: Proof of Stake
* Proof of Space:  computation is replaced by storage
* ...

## Type of Blockchain

* public blockchain: Anyone can run code/verify/validate on the blockchain
* private blockchain: R/W permission are kept centralized by one organization
* Consortium blockchain: controlled by a set of pre-selected nodes, members of these consortium can do all the things