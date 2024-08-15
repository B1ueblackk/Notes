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

## Lec2

### Types of Distributed Systems
* High performance distributed computing
	* Cluster/Grid/Cloud: SuperComputer/Amazon Cloud
* Distributed information systems
	* Databases/Ledgers/Storages:public Blockchain Network...
* Pervasive systems
	* Ubiquitous computing/Mobile computing: wearable devices

### real-life DS
soccer team
* player have their own local views
* global decisions are collective
* need some level of fault tolerance
* communicate with messages

### Characteristics and Design Goals
Characteristics
* Collection of autonomous computing elements
	* systems are consist of nodes which can act independently from each other.
	* however, these nodes have common goals
* Appears to the users as a single coherent system

the collective of nodes in a system represent a single-system view, where the system behaves according to the expectations of users

### Design Goals

* resource sharing
* distribution transparency: hide the details of the object that the user want to use, like hide how to access it, where it is, whether it is failed or in recovery
* being open: open distributed system offers usable components and can be easily integrated into other systems
* being scalable: size/geography/administration

### System Architecture

#### centralized organization

* client-server architecture -- request-reply
* multitiered architecture: MVC...

#### decentralized organization

* structured p2p systems -- with a specific topology for overlay
  * complex but quick

* unstructured p2p systems -- with a random graph like overlay
  * communication extremes -- flooding/random walk
  * flexible but inefficient
  * a football team is a unstructured p2p network communicate by messages
* hierarchical p2p systems -- with super-peer overlay network
  * each cluster has a routing node to communicate to other clusters
  * the selection of routing node has the "Leader Election problem"
  * scalable practical way to **model enterprise systems**

#### p2p communication

most p2p networks rely on **multicast communication** rather than point to point

* application-level multicasting: all nodes are in an overlay network
* flooding-based multicasting: broadcast to all neighbors and check duplicates
* gossip-based dissemination: exchange information with random-picked node(efficiently and pervasively in unstructured p2p network)

#### Replication

why replication?

1. reliability
2. performance -- more replication means easier to load balance



#### Fault Tolerance

goal: when faults occur, continue the system in an acceptable way to enhance robustness

#### Communication models

communication uncertainty is captured by an adversary who can delay the message in the network

**communication model defines the limit of the power of such an adversary**

* synchrony: known finite time bound *delta* -- the adversary can delay the message *delta* at most
* asynchrony: no time bound, only eventually delivered -- so the adversary can delay the message by any finite time(maybe quiet long)
* partial synchrony: asynchronous until GST(global stabilization time), then eventually synchronous with known time bound *delta* -- adversary must cause the GST event to happen after some unknown finite time. message sent at a time t -- *delta + max(t, GST)*

### K-Fault Tolerant

example: 2-fault tolerant system would fail if 3 faulty components occur at the same time.

### Consensus

consensus is different parties coming to an agreement

What counts as an agreement?

* meaningful -- everyone output their own answer according to the input
* valid -- if answer is limited to 0 or 1, 5 is an invalid answer

conditions of consensus:

* agreement: all honest nodes should decide on the same value at the end. so we don't need to focus on the faulty nodes.
* validity: if all honest nodes have input *v*, then *v* must be the decision value
* termination: honest nodes must eventually decide on a value *V* and halt



#### safety and liveness

safety: **nothing bad will happen**

* no double-spending problem
* consistency in context of CAP theorem

liveness: **some thing good will eventually happen**(liveness is only upheld in synchronous condition, where the exact meanings of safety and liveness can be understood in the context)

* termination will be guaranteed in Agreement Problem
* new blocks will always be mined
* availability in context of CAP theorem

### Byzantine Generals Problem

assuming there are **F** dishonest generals within the group, how many total generals are required in the group to reach consensus

answer: 3F + 1

### CAP theorem

* Consistency：站在客户端的角度来看，任意时刻所访问的数据总能获得相同的结果
* Availability：系统在合理的时间范围内，总是能给请求返回结果。
* Partition Tolerance：分区容错性。 某个节点故障或者网络分区，本是一个网络区域分裂成了多个，互相不能通信的时候。又或者是多个网络区域突然发生故障不能通信的时候。

要么只有AC不分区，要么就是区域网络不通时，1区域的数据发生变化，同步不到2区域，用户查询2区域数据时，要么响应旧数据AP，要么一直等下去，CP