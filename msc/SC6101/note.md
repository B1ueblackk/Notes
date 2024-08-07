## Assessment Criteria

quiz1 & quiz2: 30% each; about 20 m-choices and some short-answereds; on paper/close-booked

* quiz1: 19 aug
* quiz2: 9 sep
  term paper: 40%; about 3 weeks from the end of the course
* survey on blockchain tech.....
* max 7 least 4-5

# Lec1

Fiat Money
Centralized Transaction
某个媒介作为中心，记录交易
如A从B处买物品，A通过银行向B汇款，汇款记录被存于银行
Decentralized Ledger
A从B处买物品，A向B的地址转移一串token，代表A所付的价值，这笔交易由A的私钥进行签名；交易被记录于公共交易日志；B可以通过他的私钥来使用该token

哈希函数
特点：单向，简单计算，很难反向

hash pointer
can be used to verify the info has been changed or not

Blockchain
many hashpointer link together
the more previous, the safer -- the attack should make the whole chain matched, so the more recent block will be less secure than the previous ones

public & private key

Goofycoin 
why goofycoin still has the double-spending?
goofycoin does't limit whether a block can be added to another. That is to say, if a block can only be added to a specific block, there will no double-spendings.

Proof of Work