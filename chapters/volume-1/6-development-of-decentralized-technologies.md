# 6 Development of decentralized technologies
## 6.1 Bitcoin forks and clones
A situation common in the natural world for both wild animals and human beings is that disagreements have always 
existed. If people's opinions fundamentally diverge, it becomes more difficult for them to effectively interact, 
exchange and coordinate information. Diversity makes it hard for us to come up with definitive statements.

Contradictions can occur between members of the Bitcoin network as well. Due to the fact that communities in truly 
decentralized systems are open and can consist of participants with completely different backgrounds, interests, and 
goals, even Buddy the dog and Pluto can bravely defend the right to fulfil their needs and instincts in such disputes 
(Fig. 6.1). In this section, we will consider the concept of *forks* and also briefly touch on the topic of conscious 
(planned) divisions that can occur in open communities.

[Figure 6.7 - Litecoin](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.1-disagreements.png)

In the context of cryptocurrencies, there are two different meanings of *fork*.

*A fork (first meaning) is the process of cloning the source code of a currency into a new repository*. 
A new currency is launched from scratch; a new genesis block is formed, and a new network with new nodes and users is 
launched. Litecoin, Dash, Peercoin, Bytecoin, and Zcash are examples of such forks (we have already mentioned some of 
these currencies in previous topics and will mention them in the following ones).

*A fork (second meaning) is when several blocks appear at one height of the chain*. Such forks can be either planned or 
unplanned. We will consider specifically both types of forks in more detail.

### Planned forks
Some nodes can modify the rules for block creation intentionally and continue working with the same digital currency. 
However, if you violate some protocol rules, then there may be disagreements concerning both the previous and the 
following blocks in the chain. This is one more cause why several blocks can appear at the same height.

There are a lot of nodes which interact and form the Bitcoin network. They work according to common rules for reaching 
consensus and maintaining one transaction history, which is recorded in the same sequence for all of the blocks, as 
shown in Figure 6.2.

[Figure 6.2 - Maintaining transaction history by each node independently](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.2-maintaining-txs-history.png)

There is a special Bitcoin repository where you can document and post your proposal for improving the Bitcoin protocol 
(BIP) [87]. After a proposal is published in the repository, the most active users of the relevant forums conduct 
discussions about viability, safety, etc. The remaining members of the community follow the discussions and decide 
either to accept or reject a proposal for an improvement.

Anyone from the community can propose changes in the protocol, which, for example, might increase the capacity and level 
of user privacy. One part of the community may believe that the proposal is worth accepting, while for other 
participants it may not seem so clear-cut.

Although a decentralized system is made up of an open community of members who are usually competent in the relevant 
technical and organizational issues, it does not have any authority. The final decision is made by the participants 
independently of one another (Fig. 6.3).

[Figure 6.3 - Partial protocol update by part of network participants](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.3-particular-protocol-update.png)

In this way, if owners of a certain number of full nodes (this is mostly the concern of validators) have doubts about 
whether or not to install a particular update, they will probably not take the risk and will continue working according 
to the old rules.

Consequently, the entire network can be divided into two subnets: some users will update the software and wait for the 
update to take effect, while the others simply will not. When the update takes effect, some nodes will have new rules 
for creating and verifying blocks and transactions, and other nodes will have different (old) rules. Since blocks with 
different rules will be generated at one height, certain conflicts can arise.

One of the scenarios is that old-protocol network nodes will not accept the blocks that have been formed by the 
new-protocol network nodes. As a result, two different chain versions will be built (Fig. 6.4).

[Figure 6.4 - Network division as a result of non-acceptance of protocol update by part of network participants](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.4-network-division.png)

Accordingly, we have examined problems that the community can face during the software update. Now, let's take a closer 
look at exactly how the protocol updates occur.

### Software update methods: softfork & hardfork
There are two ways to update the software: a *softfork* and a *hardfork*. In fact, both are protocol updates, which 
involve changing certain rules for generating and verifying transactions as well as blocks. Such updates may either be 
backward-compatible or not. Now let's dive in.

A *softfork* is a protocol change in which the updated rules are prioritized and previously valid blocks and 
transactions are supported yet considered deprecated by nodes with updated software. The value of compatibility can 
hardly be overestimated when updating the protocol. As a typical example of a *softfork*, consider the development of 
updates for the popular USB and HDMI standards. In their development, much attention is paid to the backward 
compatibility of updated versions with the old ones, which continue to be actively used. Without this property, the 
spreading of new standards would reach a deadlock since all parties' switching to a new version is a process taking 
quite much time (during which the interaction must go on).

In other words, the set of allowable blocks and transactions grows narrow as the update is applied. Since the old nodes 
will recognize new blocks as valid (see Fig. 6.5), a softfork is backward-compatible. To have a softfork carried out 
without the network being divided, the nodes that control most of computational power of the network must be updated.

[Figure 6.5 - Protocol update through softfork](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.5-protocol-update.jpg)

A *hardfork* is a protocol change that makes previously invalid blocks and transactions valid. Therefore, a hardfork 
requires that **all** nodes update the software to avoid the splitting of the chain. It can be related to introducing 
new data formats or new verification rules. Accordingly, a block that will be generated by the old software will not be 
supported by the new one and vice versa. Obviously, there are two possible outcomes for a hardfork: all nodes are 
updated, and the chain is further built according to the new rules; the participants differ in each other's opinions, 
the chain is forked, and the community breaks up into groups each of which works according to its own protocol 
(Fig. 6.6).

[Figure 6.6 - Protocol update through hardfork](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.6-hardfork.jpg)

Above, we have described planned forks. However, there are other situations in which a fork needs to be brought into 
effect and quickly. For example, once a critical vulnerability was found in Bitcoin, and an unplanned softfork had to be 
carried out quickly. This case is worth considering in more detail.

### Unplanned softfork in Bitcoin
On August 15, 2010, a suspicious transaction was found in one of the Bitcoin blocks. It contained a lot of coins of 
unknown origin, which were being sent to several addresses [88]. Notably, these coins had not been mined. This 
transaction conflicted with the protocol rules and the entire issuance schedule was completely disrupted. The 
transaction had one input and two outputs. Each of these outputs was sending 92 billion bitcoins to each address (at 
that, the total issuance of coins in Bitcoin has a maximum limit of 21 million). How did this happen?

Someone found a vulnerability in the code: the transaction outputs were not checked separately but only the condition 
that the sum of coins at the output does not exceed the sum of the coins at the input. Further, this user picked output 
values in such a way that their sum exceeded limits of the integer data type; a huge number of coins of each output were 
transformed into a small number that passed the verification so that the transaction became valid.

When this transaction was discovered, a new topic on the relevant forum was created and the suggestion was to solve this 
problem by a softfork. Users were asked to change the software code and manually check each output for all transactions. 
Those who could not do this by themselves were asked to suspend their mining activity and wait for the finished 
executable file with the new software.

Nodes with the new software began to create new blocks starting at the height where there was no block with a "bad" 
transaction. Nodes that were not notified of the incident continued the chain of blocks with this critical transaction. 
As the number of nodes with the updated software gradually grew, they eventually began to control most of the 
computational power in Bitcoin. The honest chain became longer, and the nodes that had not been updated switched to it. 
However, those who mined in the chain with an incorrect transaction lost the received transaction fees due to transition 
to the "correct" chain.

In this case, it was the social consensus that worked. Everyone understood the need to update and supported it. This, 
however, does not always happen, especially when the fork is planned. In the next example, social consensus did not 
work. 

### Appearance of planned forks
There are well-founded reasons for the appearance of planned forks. An example is the case of Ethereum when part of the 
community decided to update the protocol, changing the history of the blocks to artificially influence the final state 
of the system. A part of the community did not support this update and retained the original protocol rules to support 
the decentralized system in accordance with the original principles, which was later called Ethereum Classic. Even so, 
Ethereum remained the first of the best-known cases of planned forks, where the basic principle of invariability of the 
history of the final database state formation was violated.

In 2016, attackers committed a successful attack on the decentralized organization, DAO. This company has been operating 
on the Ethereum platform and its main idea was to create a full-fledged organization without using a hierarchical 
management model. However, there was a vulnerability in the source code which was found and exploited. Until it was 
eliminated, the attackers managed to transfer ether coins to their account with an equivalent value of $60 million.

Since many users were affected and the attack was the most significant so far, some members of the Ethereum community 
suggested rolling back the chain of blocks to the point where the vulnerability had not been exploited. But the proposal 
was not supported by everyone and community opinion was divided: some thought that the status of smart contracts needed 
to be redistributed, while others said that it was contrary to the principles of decentralization and did not support 
this initiative. As a result, the community was divided into two groups, each of which was guided by its own ideology.

The update was released. Some nodes updated, but others did not. The network was divided into two groups of nodes, and 
the chain of blocks continued developing in parallel in two different branches respectively. This is an example of a 
planned fork. There was a cloning of the source code of the protocol, modification of some rules, split of the chain of 
blocks, and the split of the network. These were not originally envisaged but are a consequence of the fact that the 
community members did not reach agreement.

### Examples of planned forks of Bitcoin
Let's consider the cases when a major division of Bitcoin's blockchain took place. One of them was Bcash, a hardfork 
update of the protocol that was conducted to change the basic parameters. Some active community members suggested 
increasing the capacity by expanding the block size. Not everyone in the community agreed that was the best way. Many of 
them preferred the activation of Segregated Witness, but some nevertheless insisted on alternative ways to increase 
capacity.

The community members who supported the increase in the block size updated the software and began working on a parallel 
branch of the chain of blocks. In fact, this meant working with an alternative currency.

The planned fork of Bcash took place in the summer of 2017. After the division of Bitcoin's chain of blocks and the 
appearance of Bcash at a certain point in time, it turned out that the profitability of mining in the alternative chain 
was higher than in the main chain. Some validators decided to switch to creating blocks for the Bcash system without 
actually promoting the fork (particularly, the big blocks).

> **_NOTE:_** *Big blocks pretend to be a problem in system architecture terms rather than in the context of solving 
> disagreements between the network participants: as the block size grows, the competition for the place in a block 
> gets weaker and results in the place used less effectively (e.g. the amount of spam get bigger).*

How did this affect the frequency of block generation in the original Bitcoin chain? Suppose that the owners of 50% of 
the processing power in the Bitcoin network switched their equipment to the maintenance of another network. Then, the 
average period of creation of the block in the Bitcoin chain became approximately 20 minutes. Accordingly, the 
transactions took longer to confirm and the network capacity decreased by half. This situation would continue until the 
complexity parameter is recalculated.

Recent studies of the Bcash network have shown that it had a high level of centralization. On the basis of conducted 
research, the anonymous BitPico group asserted that the majority of active nodes were connected with each other. After a 
stress test, it turned out that 98% of the full Bcash nodes were in the same server room [89]. The aim of the team was 
to demonstrate how much the Bcash network is actually centralized, which threatens the stability of this digital 
currency.

Consequently, there are now two networks, Bcash and Bitcoin. Each shares exactly the same history until the fork, and 
after the fork, each of them has their subsequent chain with different rules.

A fork called SegWit2x was planned a long time ago. The supporters of this fork wanted to double the size of the block 
and simultaneously support Segregated Witness. When they realized that this would lead to a split in the community, they 
refused to implement it.

In all cases, the mechanism for achieving consensus remains the same:  proof-of-work. But the hash functions that are 
used to hash the headers of blocks when solving the task of proof-of-work are different. In Bitcoin and Bcash, a SHA-2 
hash function is used at a length of 256 bits (hence, you can mine the currency on specialized ASIC chips in both 
cases). Another fork, Bitcoin Gold, has implemented a hash function used in Zcash—EQUIHASH. It is designed specifically 
for computing on graphics processors.

To regulate the frequency of block creation, there is a certain parameter of mining complexity, which is updated with a 
certain frequency. In the Bitcoin protocol, this is approximately equal to a two-week time interval. In the Bcash 
protocol, it is equal to two weeks plus EDA (Emergency Difficulty Adjustment), which is a certain amount of time that is 
needed to protect yourself from a swing in the complexity parameter. This swing can be beneficial to validators but is 
inconvenient for users because the transaction confirmation time can change violently every two weeks. In Bitcoin Gold, 
this option is updated with every new block. All listed forks except Bcash support the SegWit update.

**Frequently asked questions**

*– Is Litecoin considered a fork of Bitcoin?*

Yes, it is. Again, we have considered two different interpretations of the concept of a fork. The first is copying of 
the source code of the project and maintaining it by a different team; it is a separate network and a new database. The 
result is the new currency with new parameters. Another interpretation is the fork of the actual chain of blocks. For 
example, Litecoin (a cryptocurrency we will consider in the next section) refers to the first interpretation of a fork.

*– Is the SegWit2x fork the Segregated Witness technology itself?*

No, it isn't. And, in fact, it would be more proper to call it an update proposal which implies a fork. It was proposed 
even before the Segregated Witness support was added to Bitcoin in August 2017. And after that, carrying out another 
fork for increasing the block size was essentially not practical. 

*– In the case of a softfork, what happens to transactions from the "bad" chain after the "good" one overtakes it?*

If transactions from the “bad” chain do not conflict with transactions in the core network, they will very likely be 
added to the main chain since validators are interested in fees for them.

*– What was the maximum gap between the "bad" and "good" chains? Were there more than 6 confirmations, which actually 
means full confirmation of transactions? (a question regarding the softfork in 2010)*

In this case, the vulnerability was found manually and a social consensus worked. Indeed, those who knew about this 
problem could take advantage of it and conduct double-spending, and those who did not know could become victims. 
However, transactions from the "bad" chain that did not conflict were added to the "good" chain.

*– Is Dogecoin also a Bitcoin fork?*

Yes, in a sense that the source code was copied. Some parameters have been changed. These changes, however, did not 
concern the mechanism of reaching consensus, but rather they concerned the rules for forming a chain of blocks: the 
frequency of block creation, the maximum size of the block, etc.

*– How are the coins in Bitcoin forks created?*

If you had coins in Bitcoin before the fork, then you automatically get a proportional number of coins in the 
alternative branch. The program code sets the block height, after which the alternative rules come into effect.

*– Can I become a member of the Bitcoin community?*

Yes, of course. You can become a member of the Bitcoin community to influence decisions about the development of Bitcoin 
and offer new updates and technological innovations. 

## 6.2 Alternative digital currencies and tokens
Certain approaches to decentralization discussed in this book have either been first proposed or tested in Bitcoin. 
Everything that has already been implemented is not the limit of engineers' and professional developers' capabilities, 
and even more so, it is not the limit of users' dreams. There is a fairly large number of people interested in testing 
more innovative mechanisms, and not all of them can be implemented in Bitcoin.

Thus, a wide variety of alternative digital currencies have appeared. To grasp the basic content of their innovation, we 
will briefly consider some of them. This list includes digital currencies such as Litecoin, Dash, NXT, Bitshares, 
Monero, Ripple, Stellar, Ethereum, Cardano, and ZCash. Prior to reviewing each of them separately, we will explain what 
a cryptocurrency is.

### What is a cryptocurrency?
*A cryptocurrency is an independent digital currency in which the management of the following processes is decentralized: 
coin issuance, transaction confirmation, data storage, audit of an accounting system, and governance (decision-making 
about the updates); anyone can become a member of the system*. The price of a cryptocurrency coin is determined 
exclusively by the ratio of demand and supply on the respective platforms.

*Decentralization* is a distinctive feature of a cryptocurrency. In a particular digital currency, if at least one of 
the above-mentioned processes is centralized, it cannot be called a cryptocurrency. Such a system would not be as stable 
and reliable as the decentralized one.

In fact, the interest in cryptocurrencies is driven mostly by their properties which allow making payments that are 
reliable and free from the bureaucratic component (e.g. registrations and restrictions). Moreover, these currencies are 
sufficiently resistant to censorship and various kinds of attack.

Each cryptocurrency user can audit all the transactions, without having to get permission from anyone. The same goes for 
the confirmation of new transactions. Users of cryptocurrency do not need to pass any identification procedures such as 
KYC (Know Your Customer), neither for creating new transactions nor for confirming them.

> **_NOTE:_** *KYC is a process maintained by an organization to verify identities of its clients and to start 
> interacting with them correctly.*

One of the attributes of every cryptocurrency is completely decentralized issuance. New coins appear in the system 
according to a specific algorithm that underlies the internal monetary policy.

Voting for the protocol update and the overall decision-making process in the system are also decentralized. This is the 
key factor that determines how a cryptocurrency will be improving. The more participants can offer improvements, the 
better are the chances of an optimal protocol update.

All the above-mentioned properties can be achieved only if the specification and implementation of the protocol are open 
and there is a sufficiently large open community around a cryptocurrency.

### Litecoin
We will start our review of alternative coins with Litecoin (Fig. 6.7). This is one of the earlier cryptocurrencies, 
which appeared in 2011. The currency is based on the Bitcoin source code but with some changes. The block creation time 
was reduced fourfold (to 2.5 minutes). Accordingly, the maximum issuance amount has increased by 4 times.

[Figure 6.7 - Litecoin](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.7-litecoin.png)

For the proof of performed work, Litecoin uses the script hash function. This hash function uses SHA-256 as a 
subprogram, relying on a large number of arithmetic calculations but also requiring quick access to large amounts of 
memory, which makes the creation of ASICs less profitable. Although Litecoin has only a few differences from Bitcoin, 
it has found an audience and community which permanently supports it. 

### Dash
The Dash project (Fig. 6.8) was launched in 2014. In Dash, the period of block creation is also shortened, which in turn 
contributes to increased network capacity. The hashing algorithm is PoW X11, which initially allowed solving the 
resource-intensive task energy-efficiently and only on graphics processors, but later integrated circuits (ASICs) were 
designed and released for this purpose.

[Figure 6.8 - Dash](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.8-dash.png)

Dash supports *PrivatSend* and *InstantSend* technologies. PrivatSend is the technology of mixing coins; in other words, 
the technology of entangling the history of coins origin. *InstantSend* is an instant payment technology, where the 
recipient does not need to wait for transaction confirmation because it is guaranteed to be added to the mainchain.

Consensus-reaching in Dash also has its peculiarities: *PrivatSend* and *InstantSend* options are implemented using the 
so-called masternodes, which are a separate group of validators that reach consensus between themselves [90].

The Dash project has made its priority to increase user privacy. For this, they have implemented a modification of the 
*CoinJoin* approach. This approach involves pooling several payments in one common transaction that mixes coins and 
prevents the tracing of a straightforward history of their origin.

The basic principle of CoinJoin can be explained with a practical example: imagine four people, each of whom first put 
their money in one common safe and then independently took the same amount of money from it. An outsider, even if had 
known the total sum of money which was put in the safe, would have never known the share of each person, namely who has 
how much. In Dash, this principle is implemented through the subnet of *masternodes*. In fact, any node can become a 
masternode: all that is required is to have a certain amount of coins on your address. These coins will be frozen and 
will serve as a deposit.

Change management in Dash also has its peculiarities. The total mining profit is distributed: 10% goes to founding 
members (for the development of the Dash project), 45% to regular nodes, and 45% to masternodes. Managing the budget for 
the development of the Dash project is done through voting of masternodes. In this way, all developers' proposals for 
the improvement of the protocol (e.g., improving the network quality, capacity, etc.) can either be accepted or rejected 
by masternodes. Yet the system is not completely decentralized because not all users can vote for the improvements. 
However, any user can suggest her own improvements, and this may potentially be supported and sponsored. For this, a 
user needs to leave a *treasury* request with a suggestion for an idea, and the owners of masternodes will vote for it.

### Difference between mining algorithms of Litecoin, Dash, and Bitcoin 
The above-mentioned altcoins are different from Bitcoin by the hashing algorithms. This means that, for example, bitcoin 
mining equipment is not suitable for mining of alternative digital currencies with different hashing algorithms. The 
purpose of such measures has been reaching the conditions under which validators of the Bitcoin network cannot attack 
the network of Litecoin, Dash, or any other cryptocurrency. In other words, separate currencies generally use different 
hashing algorithms, making 51% attack technically impossible to conduct.

Let's imagine that hashrate of the Litecoin network is 10% compared to that of Bitcoin and that both currencies use the 
same algorithm to solve the task of block creation. In this case, Bitcoin validators could easily switch to the Litecoin 
network and very quickly build an alternative chain that could significantly surpass the mainchain by length. This would 
allow Bitcoin validators to conduct a 51% attack because the more computing power is involved, the faster the chain is 
created. 

### NXT
NXT (Fig. 6.9) was launched in 2013 by an anonymous developer. It was one of the first well-known protocols that used 
the proof-of-stake consensus mechanism. Originally, NXT was designed as a versatile platform allowing for the 
implementation of various financial applications and services.

[Figure 6.9 - NXT](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.9-nxt.png)

The NXT infrastructure is quite complex compared to Bitcoin. The platform features an integrated digital asset exchange 
and a messaging system [91]. Issuance in the system was performed only once and in a centralized way. NXT allows anyone 
to create colored coins which represent certain assets. These assets can be traded on an internal exchange.

The messaging system allows sending plain or encrypted messages within the system and storing up to 1,000 bytes of data 
permanently. If the data is up to 42 kilobytes, it can only be stored for a limited time.

The disadvantages of the platform are low capacity (4–5 transactions per second) and relatively long transaction 
confirmation time (10 to 15 minutes).

### BitShares
The idea of the BitShares protocol was announced in 2013. It was created as a tool for trading different assets and 
currencies in a decentralized environment without having to deposit them on the trading platforms. So, essentially, 
BitShares is a decentralized exchange (Fig. 6.10). BitShares was the first protocol that used DPoS consensus mechanism, 
which focus is on capacity.

The BitShares platform allows anyone to create the so-called user-issued assets (UIA), known as digital tokens. This 
means that the platform accounts a base currency, BitShares (BTS), and a lot of user-issued tokens.

[Figure 6.10 - BitShares](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.10-bitshares.png)

In addition, BitShares is a smart contract platform. Smart contracts here are preinstalled and their number is limited 
(i.e., only the most popular contracts are implemented on the platform), but they are more cost and energy effective.

An additional feature for the users of the platform are the so-called Stealth Transfers, which allow for the payments 
with increased user privacy.

### Monero
In Monero, the primary focus is increased privacy for users (Fig. 6.11). The project appeared in 2014 as a Bytecoin 
fork, another 2014 project, which was promoted as an anonymous payment system.

A part of the community became concerned when they discovered that 80% of Bytecoin coins (BCN) had already been mined. 
The prospects were disappointing: there was only 20% of the planned issuance left to reward validators who mine coins; 
the other 80% were accumulated in the hands of the early participants [92]. This was putting the price of BCN at threat 
because it could potentially be manipulated. That's how a part of the community created its own project, Monero.

Monero operates according to the rules defined in the CryptoNote protocol [93]. It allows anonymization of traffic at 
the network level with the origin of coins hidden in each transaction.

[Figure 6.11 - Monero](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.11-monero.png)

At the level of the transaction model, there is a mechanism for entangling the history of the origin of coins that does 
not involve any third parties. Unlike Dash, where a specific server or group of servers act as a mixer, monero coins are 
mixed through cryptographic algorithms. It works as follows: each transaction (that transfers a certain amount of coins 
to a certain address) is placed into a sequence of transfers. This complicates the history so much that tracing an 
actual sender's address is virtually impossible.

This mechanism uses a *ring* signature, which implies that the sender forms on his own a group of addresses among which 
to hide the transfers. The sender creates transactions and signs them on behalf of the group without the consent of 
other group members. Also, Monero implements the *confidential transactions* function [94], which, in addition to the 
address, hides the amount of transfer. However, since the size of the group is limited, the probability that a 
transaction can be traced yet exists (the complexity of tracing directly depends on the group size).

As to the anonymization at the network level, Monero has implemented an optimized version of the I2P protocol (Invisible 
Internet Project) [95], which is more advanced than Tor.

Block creation in the network takes approximately two minutes, and full transaction confirmation takes up to 30 minutes.

### Ethereum
The feature of Ethereum (see Fig. 6.12) is supporting a programmable digital currency. Ethereum is described in 
different ways—some people call it a platform for smart contracts, and others as a decentralized computer. The 
accounting platform has a native ether currency.

[Figure 6.12 - Ethereum](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.12-ethereum.png)

Issuance in Ethereum is not limited. 18 million new coins are issued each year; therefore inflation is infinite. The 
block creation period is 10–20 seconds. Ethereum operates on the proof-of-work consensus algorithm, but the possibility 
of switching to proof-of-stake is currently being actively discussed.

The idea of Ethereum is that transactions in the network can be performed according to the rules that participants 
determine in advance.

For example, Alice sends coins to Bob. She sets a certain condition that supposes certain events that will act as a 
trigger for the validators to confirm the transaction. For instance, in the Bitcoin transaction, there is only one 
trigger condition, a valid digital signature. Whereas in Ethereum, you can specify any required condition, even 
including the source of external data. In the latter case, the furnishing of this external data is entrusted to the 
so-called *oracles*.

This idea can be explained empirically. Let's suppose there is a wheat field which is insured in case of fire, and you 
have to create a *smart contract* (more details in 6.3) that will pay compensation for the lost crop to its owner. Thus, 
a number of owners will need to create an insurance budget, which will be shared among those whose crop suffered due to 
fire. This budget will be managed exclusively by the smart contract, namely according to the conditions which the owners 
have previously specified. Here, you need *oracles* to furnish the smart contract with the trigger condition, fire 
situation data.

It is always better to use more than one oracle. The more independent oracles you have, the more objective the data, and 
the more you minimize the risk of bribery and collusion. Oracles will independently transmit the fire situation data to 
the smart contract. Each of the oracles will use their own methods of observation: satellite data, local fire safety 
points, data from the drones, surveillance cameras, etc. If the contract receives the trigger (a message from the 
majority of oracles that informs about the fire situation on a particular field), then the contract pays the insured 
amount to the owner of the field.

If a fire did not occur during the predetermined period of time, the budget is returned to the accounts of depositors. 
As to the oracles reward, it is even possible to specify in the contract that oracles receive a fixed reward for their 
work regardless of the outcome.

### Cardano
The Cardano project (Fig. 6.13) is a decentralized platform of *smart contracts* that can be programmed by 
Turing-complete languages. The protocol of the full network nodes is implemented in Haskell, a functional programming 
language. As the developers say, their platform uses a persistently strong consensus algorithm based on the idea of 
proof-of-stake. The algorithm is named Ouroboros and was designed and implemented specifically for the use in this 
decentralized system.

[Figure 6.13 - Cardano](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.13-cardano.png)

The currency supports governance through voting for all changes. The maximum issuance of coins of the platform's base 
currency is 45 billion coins; there was a one-off pre-issuance of a certain amount of coins in a centralized way, and 
further issuance has been performed as a reward for the validators who mine blocks.

### Stellar and Ripple

The Ripple digital currency was launched in 2011. Initially, the project was called Opencoin, and in 2013 it was 
renamed, Ripple. Some ideas behind Ripple appeared even before Bitcoin (in 2005–2006), while the principles of work are 
similar to the middle-aged money transfer system, Hawala. However, after Bitcoin appearance, the founder of Ripple 
decided to implement these ideas in his own project.

The founder of Ripple had a conflict with other members of the team. As a result of this conflict, in 2014 a fork 
occurred and was called Stellar (Fig. 6.14). Stellar began to develop separately. Although initially, the code was the 
same, Stellar, unlike the Bcash fork, which up to a certain block shares a transaction history with Bitcoin, has its own 
transaction history.

[Figure 6.14 - Stellar](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.14-stellar.png)

[Figure 6.14 - Ripple](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.14-ripple.png)

The purpose of Ripple as a system is to create a decentralized exchange and a settlement mechanism between parties. All 
the coins of native currencies in both accounting systems were issued and distributed centrally. Users are able to issue 
their own assets (IOU) and trade them on a decentralized exchange.

To summarize, the main differences between these digital currencies and Bitcoin are as follows: the issuance of coins is 
performed centrally; the consensus mechanism is based on FBA; and validators are not anonymous (instead, each of them is 
identified).

### ZCash
ZCash is a cryptocurrency, which is mostly focused on making transactions confidential. Results of transactions are 
published in the chain of blocks, but details such as the sender's and receiver's data and the transaction amount remain 
hidden. This cryptocurrency was announced in 2016 (Fig. 6.15).

[Figure 6.15 - ZCash](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.15-zcash.png)

ZCash is the first cryptocurrency based on the *zero-knowledge proof* protocol, which allows hiding the transaction 
details. Its architecture is identical to that of Bitcoin. The key differences are the decreased block creation time and 
increased block size (the latter measure is required because transactions with zero-knowledge proofs are much larger in 
size). The difference from Monero is the fact that space of addresses among which transactions "hide" is equal to the 
entire amount of "hidden" coins.

The ZCash cryptocurrency has an important mechanism called *trusted setup*. Its zero-knowledge proofs are based on a set 
of public parameters which allow creating and verifying confidential transactions. These parameters are the ones to be 
generated at the initial configuration stage of trusted setup (in which there are six participants) [96]. It is supposed 
that these six participants delete the intermediate key material (because if they don't, the privacy and total issuance 
in ZCash would be broken). Obviously, there is no way to verify whether this parameter is deleted or not (for example, 
their hardware devices could be modified upfront by the government agencies).

One of the peculiarities of ZCash is that transaction audit is permissioned, which means that you can only see the 
details of a transaction if you are granted corresponding permission by the creator of the transaction. Also, it stands 
to mention that 20% of all mined coins are automatically deposited to developers' addresses. 

### Other digital currencies
To compare the above-listed digital currencies, we have decided to use the following criteria: coin issuance, 
transaction confirmation, transaction audit, update management, capacity, full transaction confirmation time, and user 
privacy (Table 6.1).

[Table 6.1 - Comparison of digital currencies](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/table-6.1-comp.png)

Having analyzed, for example, the base currency of Ripple (XRP), you can see that processes such as issuance and 
transaction confirmation are performed centrally. Therefore Ripple is a digital currency, not a cryptocurrency because 
not all processes are decentralized.

Let's now move on to another type of digital asset, tokens, and define the main features by which they can be 
distinguished from digital currencies (Fig. 6.16).

[Figure 6.16 - Classification of digital assets](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.16-classification.png)

### Tokens
At the end of 2016, there was a sharp increase in the number of projects launching their own currency to finance the 
development of a certain product or service. Most often, this was justified by the need to have a network that would 
provide these services in a decentralized manner. Among the most prominent projects are Filecoin, OmiseGo, Steemit, 
Golem, Basic Attention Token, Bancor, and others. Among the overwhelming majority, their goal was not the creation of a 
cryptocurrency in its full meaning but rather an internal means of payment through which users could have access to 
services (a utility token).

Almost all similar projects were aimed at launching their own network using the blockchain technology. Before the 
launch, however, their tokens generally were issued on the Ethereum platform and traded on exchanges. Once their own 
system was launched (e.g., as a smart contract or a digital currency platform), these Ethereum-based tokens were 
replaced with coins of the base currency.

Tokens can be issued either in a *centralized* (under the management of one organization) or *decentralized* (under the 
management of a predetermined algorithm) manner. This essentially is the main difference between tokens and 
cryptocurrencies, where all the processes are decentralized.

> **_NOTE:_** *Unlike a digital currency, a token is a digitalized ownership right on a asset and can also be used as a 
> payment tool. The essence of exchanging tokens or paying in them for other goods or services is equivalent to the 
> essence of bartering.* 

In Table 6.2, you can see how digital assets are different according to the nature of processes that occur in their 
accounting systems.

[Table 6.2 - Distinction between groups of digital assets](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/table-6.2-dist.png)

### Conclusion
We ultimately give the following definition of a cryptocurrency that demonstrates as accurately as possible its essence 
and properties. *A cryptocurrency is an independent digital currency in which the management of the following processes 
is decentralized: coin issuance, transaction confirmation, data storage, audit of an accounting system, and governance 
(decision-making about the updates). Anyone can become a member of the system, own the coins, and perform transactions 
under the common-for-all rules; the cost of coins is not affected by the creator*.

Knowing the correct terminology is essential in order to accurately perceive and present information. In Table 6.3, 
there is a comparison of various digital assets according to certain criteria. Studying this table will allow you to 
distinguish the main groups of assets as well as to better navigate the entire field of digital financial technologies. 

[Table 6.3 - Сomparison of digital assets](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/table-6.3-comp.png)

**Frequently asked questions**

*– What is the fundamental difference between Ripple and Stellar?*

The difference is that Ripple is a commercial company that sells its currency for profit. Stellar is registered as a 
non-profit organization that sells the currency's issuance only partially, for covering their expenses. The principle of 
their operation is quite the same, but Ripple primarily focuses on banks and Stellar on payment systems. 

*– How reliable is a digital currency that does not implement mining?*

Reliability can be understood as the inability to falsify transactions. This feature does not depend on the presence of 
mining and instead is determined by the protocol that specifies the mechanism of protection against counterfeit 
transactions. The advantage of currencies with PoW-based mining is that reaching consensus among validators does not 
depend on individuals, while the subjectivity of decision-making is minimal.

*– It is known that ZCash supports addresses of two types, one start with "t" and other with "z". What is the difference 
between these addresses?*

Addresses starting with "t" are actually no different than Bitcoin addresses. They are called transparent addresses: all 
actions under such addresses are visible to all network users.

Addresses starting with "z" include the confidentiality improvements, which are provided by zero-knowledge proofs; they 
are called *shielded addresses*.

*– Does ZCash support multisignature transactions?*

The public ZCash addresses support multisignature transactions. However, the shielded addresses do not support this 
functionality for now.

*– Does BitShares support SPV nodes?*

At present, BitShares does not support SPV clients—every its user must have a local copy of the database to interact 
reliably on the network.

*– Why do users in their transactions set GasLimit parameter manually? Why isn’t it determined automatically?*

The GasLimit parameter carries the maximum number of coins that a user is ready to spend for the execution of a smart 
contract. For ordinary coin transfers between the network users, this parameter is generally computed automatically. The 
smart contract case is a bit different. This is because it is very hard to determine upfront whether your smart contract 
will loop or not (if there is a mistake in the code, it can be executed forever). In such a case, if your coins are 
withdrawn progressively as the contract is executed, then there is a risk that you will lose all your coins. The 
GasLimit parameter may serve as a protection against endless loops in the smart contracts. This is why it must be set by 
a user directly.

*– Are there lightweight nodes in Monero?*

Monero does not support SPV clients. However, there is the implementation of a so-called lightweight wallet. The feature 
of its operation as follows. In your wallet software, you determine the node to which you connect and which will trace 
your transactions in the chain of blocks. Although this node will know when you received the money, yet the amounts and 
senders’ addresses will remain unknown. For increased privacy, you can launch your own full network node to which you 
would connect your own lightweight wallets.

*– What is a masternode in Dash?*

Masternodes play an important role in the Dash infrastructure since they allow for certain functions which are either 
impossible or extremely challenging for p2p decentralized systems. Essentially, a masternode is a computer connected to 
the Internet and which performs functions such as blocking transactions, supporting instant transactions, mixing coins, 
voting the network financing, etc. A masternode must be backed with a certain number of DASH coins (in 2018, it is 
1,000), be online 24 hours, and ultimately prevent its connection losses.












