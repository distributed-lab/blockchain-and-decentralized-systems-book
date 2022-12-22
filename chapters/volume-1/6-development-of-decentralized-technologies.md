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

![Figure 6.7 - Litecoin](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.1-disagreements.png)

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

![Figure 6.2 - Maintaining transaction history by each node independently](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.2-maintaining-txs-history.png)

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

![Figure 6.3 - Partial protocol update by part of network participants](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.3-particular-protocol-update.png)

In this way, if owners of a certain number of full nodes (this is mostly the concern of validators) have doubts about 
whether or not to install a particular update, they will probably not take the risk and will continue working according 
to the old rules.

Consequently, the entire network can be divided into two subnets: some users will update the software and wait for the 
update to take effect, while the others simply will not. When the update takes effect, some nodes will have new rules 
for creating and verifying blocks and transactions, and other nodes will have different (old) rules. Since blocks with 
different rules will be generated at one height, certain conflicts can arise.

One of the scenarios is that old-protocol network nodes will not accept the blocks that have been formed by the 
new-protocol network nodes. As a result, two different chain versions will be built (Fig. 6.4).

![Figure 6.4 - Network division as a result of non-acceptance of protocol update by part of network participants](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.4-network-division.png)

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

![Figure 6.5 - Protocol update through softfork](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.5-protocol-update.jpg)

A *hardfork* is a protocol change that makes previously invalid blocks and transactions valid. Therefore, a hardfork 
requires that **all** nodes update the software to avoid the splitting of the chain. It can be related to introducing 
new data formats or new verification rules. Accordingly, a block that will be generated by the old software will not be 
supported by the new one and vice versa. Obviously, there are two possible outcomes for a hardfork: all nodes are 
updated, and the chain is further built according to the new rules; the participants differ in each other's opinions, 
the chain is forked, and the community breaks up into groups each of which works according to its own protocol 
(Fig. 6.6).

![Figure 6.6 - Protocol update through hardfork](/resources/img/chapter-1/6.1-bitcoin-forks-and-clones/6.6-hardfork.jpg)

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

![Figure 6.7 - Litecoin](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.7-litecoin.png)

For the proof of performed work, Litecoin uses the script hash function. This hash function uses SHA-256 as a 
subprogram, relying on a large number of arithmetic calculations but also requiring quick access to large amounts of 
memory, which makes the creation of ASICs less profitable. Although Litecoin has only a few differences from Bitcoin, 
it has found an audience and community which permanently supports it. 

### Dash
The Dash project (Fig. 6.8) was launched in 2014. In Dash, the period of block creation is also shortened, which in turn 
contributes to increased network capacity. The hashing algorithm is PoW X11, which initially allowed solving the 
resource-intensive task energy-efficiently and only on graphics processors, but later integrated circuits (ASICs) were 
designed and released for this purpose.

![Figure 6.8 - Dash](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.8-dash.png)

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

![Figure 6.9 - NXT](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.9-nxt.png)

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

![Figure 6.10 - BitShares](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.10-bitshares.png)

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

![Figure 6.11 - Monero](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.11-monero.png)

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

![Figure 6.12 - Ethereum](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.12-ethereum.png)

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

![Figure 6.13 - Cardano](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.13-cardano.png)

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

![Figure 6.14 - Stellar](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.14-stellar.png)

![Figure 6.14 - Ripple](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.14-ripple.png)

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

![Figure 6.15 - ZCash](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.15-zcash.png)

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

![Table 6.1 - Comparison of digital currencies](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/table-6.1-comp.png)

Having analyzed, for example, the base currency of Ripple (XRP), you can see that processes such as issuance and 
transaction confirmation are performed centrally. Therefore Ripple is a digital currency, not a cryptocurrency because 
not all processes are decentralized.

Let's now move on to another type of digital asset, tokens, and define the main features by which they can be 
distinguished from digital currencies (Fig. 6.16).

![Figure 6.16 - Classification of digital assets](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/6.16-classification.png)

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

![Table 6.2 - Distinction between groups of digital assets](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/table-6.2-dist.png)

### Conclusion
We ultimately give the following definition of a cryptocurrency that demonstrates as accurately as possible its essence 
and properties. *A cryptocurrency is an independent digital currency in which the management of the following processes 
is decentralized: coin issuance, transaction confirmation, data storage, audit of an accounting system, and governance 
(decision-making about the updates). Anyone can become a member of the system, own the coins, and perform transactions 
under the common-for-all rules; the cost of coins is not affected by the creator*.

Knowing the correct terminology is essential in order to accurately perceive and present information. In Table 6.3, 
there is a comparison of various digital assets according to certain criteria. Studying this table will allow you to 
distinguish the main groups of assets as well as to better navigate the entire field of digital financial technologies. 

![Table 6.3 - Сomparison of digital assets](/resources/img/chapter-1/6.2-alternative-digital-currencies-and-tokens/table-6.3-comp.png)

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

## 6.3 Introduction to smart contracts
In a way, we can view blockchain technology as a way to agree about the past. This agreement is based on the security of 
cryptographic algorithms that protect the integrity of transactions history (events). The usage of smart contracts will 
allow us to agree about the future. This is achieved through programming the conditions that will or potentially can 
happen. Smart contracts allow us to assign different outcomes with which all parties explicitly agree. This is why they 
are considered one of the most promising technologies that can change the traditional way of doing business. The 
practical impact of using smart contracts is the automation of financial and legal relationships between two or more 
parties. In order to thoroughly understand the possibilities, principles, and limitations of the technology, let's first 
review the disadvantages of traditional (paper) contracts.

We will start with a focus on the complexities that often accompany working with traditional contracts. Problems can 
arise at all levels including drafting, verifying, executing, and even simply understanding.

> **Some challenges in drafting a traditional contract**
>> * In many cases, you need a qualified lawyer 
>> * It is difficult to achieve contract terms which are complete and comprehensive 
>> * It is virtually impossible to quickly check whether the contract is consistent with other contracts and whether it 
is compliant with all the requirements of current legislation

When all conditions are taken into consideration and the parties have signed the contract, there are still a number of 
other problems that may arise later.

> **Some challenges in understanding and verifying contracts**
>> * Contract language is very often hard to understand for people who do not have a legal background 
>> * Lawyers often disagree on the language and meaning of contracts 
>> * Unsigned contract pages may be substituted 
>> * Signatures can be falsified

It is difficult to actually assess the scale of time and money wasted by participants due to unfit contracts. If you 
consider the number of forged documents, it would amount to something like an economic or social disaster. The majority 
of issues arise at the execution stage and in litigation. Analyzing what can go wrong during execution identifies many 
potential problems.

> **Challenges in executing a contract**
>> * Success often depends on the skills of lawyers (not the contract quality)
>> * Unpredictable duration and costs of legal proceedings 
>> * Possibility of bribery 
>> * No guarantee of compensation if the other party goes bankrupt

A solution for the future has emerged. A contract could be written digitally using a programming language to implement 
all the necessary conditions. 

> **Features of a contract written in a code language**
>> * Universal code language will be recognized by the computer explicitly 
>> * Program code always returns the same result if the input data is the same (Fig. 6.17)
>> * Contract is signed using a digital signature, which is difficult to forge 
>> * Need for human comprehension and execution is excluded 
>> * Such a contract can be tested

![Figure 6.17 - Result of smart contract execution is the same on all devices](/resources/img/chapter-1/6.3-introduction-to-smart-contracts/6.17-result-of-execution.png)

### What is a smart contract?
A *smart contract* is an agreement for the redistribution of value, which implies a strict and unambiguous assignment of 
conditions, automation of execution processes, and minimal involvement of a trust relationship between contracting 
parties. Most often, this is achieved by combining the mechanisms for setting conditions with mechanisms for their 
strict implementation since such protocols are usually used to describe the procedure for creating, processing, and 
implementing smart contracts. A system managed by such protocols is called a platform of smart contracts, where 
contracts are put in special transactions (or requests) that must be signed by all parties involved in the transaction. 
After such a transaction is transferred to the platform of smart contracts, it is impossible to make any changes to the 
contract or to stop or cancel the execution. The idea of smart contracts was proposed by the researcher Nick Szabo 
in 1994. His paper, "The Idea of Smart Contracts" [62], was published in 1997 (Fig. 6.18).

![Figure 6.18 - Idea of smart contracts by Nick Szabo](/resources/img/chapter-1/6.3-introduction-to-smart-contracts/6.18-nick-szabo.png)

> **What do you need to know about smart contracts?**
>> * Smart contracts allow counterparties to interact without having third parties involved 
>> * Smart contracts are easier to audit than traditional contracts 
>> * There are different types of smart contract protocols and different implementation methods 
>> * They allow the management of digital assets that are stored on a specific platform 
>> * Initiation of the contract can be performed either manually or automatically

Execution of smart contracts implies that the platform has one validator (or a network of validators) and a database 
that stores all smart contracts that are submitted for execution in strict chronological order. This database has to 
contain all the data that trigger conditions for the execution of the smart contract as well as manage all the assets 
that it can redistribute.

In other words, smart contract validators must have access to **all** the data that may act as a trigger for the smart 
contract. For example, if there is a database used to account for the digital currencies, user balances, user 
ransactions, and timestamps, then all this data can act as a trigger for the smart contract: user's balance in a certain 
currency, certain timestamps, or the fact of implementation of a transaction, but nothing more.

Smart contracts imply automation of the value distribution, which depends only on specific conditions that are 
determined in advance. In the simplest version, it is a programmable contract with strictly defined conditions, which is 
usually verified by all parties involved in the transaction using a digital signature (Fig. 6.19).

![Figure 6.19 - Validation of a smart contract by involved parties](/resources/img/chapter-1/6.3-introduction-to-smart-contracts/6.19-validation-od-a-smart-contract.png)

Smart contracts are designed to minimize reliance on third parties. Sometimes even the decision-making center on which 
everything depends is completely excluded. In the case of smart contracts, audits are easier and faster to perform, due 
to the design features of such systems. A smart contract also acts in a decentralized environment and has the 
functionality which allows anyone to analyze the database and conduct a full audit of the performance of contracts. 
Thus, there is a protection against retroactive data changes, which could entail a change in the performance of the 
contract itself. The maintenance costs are reduced since the creation and execution of a contract are digitized 
processes.

### Role of oracles for smart contracts
The implementation of a smart contract heavily depends on the data with which it operates—namely, where this data is 
stored. If this is within a single accounting system, then the data is verified and a smart contract can operate with it 
without any intermediaries: the case is about a primary accounting system which is self-sufficient. This case may prove 
to be more reliable regarding the level of required trust; it is minimal. However, from a practical perspective, the 
capabilities of a smart contract operating only with internal data is much limited.

In a number of cases, a contract would need certain verified external data—the fact that a certain event happened, a 
certain threshold reached, etc. Oracles are the sources of this external data.

Oracles transmit the data from the outside world into a decentralized system. Commonly, oracles have verified identities 
and they require a particular trust from users to the data they transmit. All the data broadcast should be signed with a 
digital signature for two purposes: the integrity of data and non-repudiation. The last one relates to the fact that 
each participant can definitely prove the provision of particular data by a particular oracle.

To minimize the level of required user trust, a smart contract can receive the data from a number of independent 
oracles, coordinate the data, and then reach the decision (Fig. 6.20). 

![Figure 6.20 - Smart contract receiving data from independent oracles](/resources/img/chapter-1/6.3-introduction-to-smart-contracts/6.20-smart-contracts-and-oracles.png)

### Example of purchase in an online store
Let's consider a simple example. It will help to understand the functionality of smart contracts and suggest in which 
cases they should be used. In fact, smart contracts can also be implemented using Bitcoin, but nowadays Bitcoin cannot 
be considered a fully-fledged platform for smart contracts.

Imagine a buyer and online store. The buyer wants to buy a display (Fig. 6.21). In the simplest case, the buyer creates 
and sends the payment, while the online store accepts it, confirms, and then sends the display. However, this situation 
requires a high level of confidence—the buyer must trust the online store with the full cost of the display. If the 
online store has a bad reputation, then there is a risk that after accepting the payment, the store will refuse to send 
the goods to the buyer. The buyer may wonder (as may the online store) what measures can be taken in order to minimize 
the risk and make such transactions more reliable.

![Figure 6.21 - Using a smart contract for buying goods online](/resources/img/chapter-1/6.3-introduction-to-smart-contracts/6.21-smart-contract-for-buying-good.png)

In the context of Bitcoin, you can provide an opportunity for the buyer and seller to independently choose a mediator. 
Commonly, there are many people engaged in resolving disputes, so the participants should find it easy to choose from a 
general list of mediators those who they both trust. Together they create a 2-of-3 multisignature address, where there 
are three keys and you need two signatures with any two keys in order to spend coins from this address. One key will 
belong to the buyer, the second to the online store, and the third to the mediator. With such a multisignature address, 
the buyer will send the necessary amount in order to pay for the display. When the seller is convinced that the coins 
have been blocked in the multisignature address, he can send the display with confidence.

Meanwhile, the buyer receives the delivery, inspects the goods and decides to complete the purchase. He can be satisfied 
with the service provided, sign the transaction with his key, and transfer the coins from the multisignature address to 
the seller. Should the buyer be unsatisfied with the goods, he can contact the mediator to create an alternative 
transaction, which will distribute these coins according to the mutual decision.

Let's imagine that the display arrived scratched. In this case, the buyer collects the evidence necessary for the 
mediator: he takes a photo of the check from the post and a photo of the scratches on the monitor. The online store, if 
considering necessary, collects its evidence and gives it to the mediator.

The mediator is interested in satisfying both the angry buyer and the online store (we will explain why further). The 
mediator conducts a transaction in which coins from the multisignature address will be spent in a certain proportion 
between the buyer, the online store, and the mediator (who takes a part as a reward for his work). Let's say 90% of the 
total amount goes to the seller, 5% to the mediator, and 5% to the buyer. The mediator signs this transaction with his 
own key (the transaction still cannot be finalized since it requires two signatures). This transaction is sent to both 
the buyer and the seller. If at least one of them is satisfied with this option of coins redistribution, the transaction 
will be signed and distributed to the network. To validate it, it is enough that one of the participants (the buyer or 
the online store) in the transaction agrees with the mediator option.

It is important to initially choose a mediator that both sides trust. He will act independently and objectively to 
assess the situation. If the mediator does not provide a coin distribution option that will satisfy at least one 
participant, then, by mutual agreement, both the buyer and the online store can transfer the coins to a new multiSig 
address by placing their two signatures. The new multiSig address will be created with another mediator, who it is hoped 
will be more competent and will provide a better solution to the conflict. In this example, there are two crucial 
features:

* Mediator cannot steal users’ money 
* It is possible to change the mediator during the process of conflict resolution by the agreement of both parties

### Example of a contract for mutual purchase
Let's look at a more complex example that shows the capabilities of a smart contract more clearly. Imagine there are 
three students who have recently settled in one room in a house. All three are interested in buying a fridge, which they 
will use together. One of them volunteered to collect and store the necessary amount of money to buy a fridge and 
negotiate with the seller. However, they recently met and there is an insufficient trust between them. Two of them take 
a risk by giving the money to the third. At the same time, they need to reach an agreement in choosing a seller.

The students can use an escrow service, meaning that they can choose a mediator who will monitor the transaction and 
settle any disputes. After agreeing, they use a smart contract and prescribe certain conditions (Fig. 6.22). One of the 
conditions is that by a certain time, let's say one week, the corresponding account of the smart contract should receive 
three payments from certain addresses in a certain amount. If this does not happen, the smart contract terminates and 
returns the coins to all the participants. If all conditions are present and there are no misunderstandings, then all 
the participants agree with the choice of the seller and mediator. When all conditions are fulfilled, the funds will be 
transferred to the indicated addresses.

![Figure 6.22 - Using smart contracts for a mutual purchase](/resources/img/chapter-1/6.3-introduction-to-smart-contracts/6.22-mutual-purchase.png)

Such an approach can protect participants from fraud on either side and generally excludes the need to trust. This 
example clearly demonstrates the main principle: the possibility to set parameters for the fulfillment of each condition 
step by step. This allows for the creation of contracts of any complexity and depth of nested levels. In addition, first 
of all, in a smart contract you can define the condition, and only after its execution, you have to set parameters for 
the next condition, which is formally prescribed, and parameters can be set already during its operation.

> **Other difficulties**
>> * Error in the smart contract code can lead to unavoidable consequences 
>> * Data for all the triggers must be in one accounting system 
>> * Assets need to be digitized and accounted on the platform 
>> * In some cases, the trustless property is impossible to achieve

### Classification of smart contracts
For the classification, there are different groups of criteria to specify. Smart contracts can be distinguished by 
their:
* Execution environment 
* Execution method 
* Initiation method 
* Level of privacy

As to privacy, contracts can either be completely confidential (outsiders cannot see the conditions of smart 
contracts) or completely or partially public. Below, we will review the rest of the options.

### Differences between platforms according to their execution environment
By execution environment, smart contracts platforms are divided into centralized and decentralized ones. In the case of 
centralized digital contracts, there is a service with one validator. Additionally, there may be a backup restore 
service that is also centrally managed according to the same rules. There is one database that stores all the necessary 
information for setting the conditions of the smart contract and distributing the "value" (a digitized ownership right 
for an asset). Such a service has a client, and since the platform is centralized, the authentication mechanisms can be 
more primitive than in Bitcoin (Fig. 6.23).

![Figure 6.23 - Centralized smart contract platform](/resources/img/chapter-1/6.3-introduction-to-smart-contracts/6.23-centralized-smart-contract.png)

Let's take, for example, different mobile operators. Suppose a certain operator accounts mobile traffic of different 
formats (e.g., voice calls, SMS, mobile Internet traffic) according to different standards on its servers in a 
centralized way. It also maintains a balance on users' accounts. Accordingly, the mobile operator can create contracts 
for a specific amount of services and their costs with the help of different conditions. Specifying conditions in such a 
case is much easier (e.g., in order to get a fixed service, a user should pay a fixed amount).

Another good example are the traditional banks using Internet banking and conventional contracts such as regular (e.g., 
monthly) payments, automatic conversion of incoming payments, automatic deduction of interest on a specified account, 
etc.

If the smart contract execution environment is decentralized, then we deal with a group of validators who process 
contracts that are stored in an immutable way (Fig. 6.24). Ideally, in such a permissionless environment, anyone can be 
the validator. 

![Figure 6.24 - Decentralized smart contract platform](/resources/img/chapter-1/6.3-introduction-to-smart-contracts/6.24-decentralized-smart-contract.png)

In this case, transactions will contain instructions for executing the contract according to a strict specification. 
This specification is open and platform users can independently audit and validate the smart contracts. Decentralized 
platforms exceed the centralized ones in parameters such as *independence* and *fault tolerance*, but their design and 
maintenance are much more difficult.

### Differences between platforms in the way contracts are executed
Let's consider how smart contracts are different by the way you specify and fulfill conditions there.

*Turing-complete* smart contract language allows you to specify almost any algorithm as a condition for executing a 
contract: you can use iteration loops, your own algorithms for digital signatures, functions for calculating 
probabilities, etc. The idea is that you basically can prescribe any logic, even a custom smart contract. The benefit of 
this approach is greater flexibility when programming. The drawback is that there is no way to fully guarantee the 
security of the platform itself since Turing complete language allows finding unpredictable ways of execution of the 
code. Among arbitrary Turing complete contracts, there are platforms such as Ethereum [115] and RootStock.

*Turing-incomplete* smart contracts language (such as Bitcoin and Litecoin with their own scripts) allow you to use only 
certain operations in an arbitrary order but not iteration loops or your own algorithms. This allows creating a more 
secure environment.

*Pre-defined (template)* smart contracts support a limited list of templates, usually a few dozen of the most popular 
contracts that are natively implemented. It also gives greater control over security. Examples are Stellar, Bitshares, 
Steemit. Bitshares has smart contracts for trading, managing accounts, managing the platform and its parameters. Steemit 
is a similar platform, but, unlike Bitshares, it is no longer focused on issuing and trading tokens; it is used for 
blogging; namely, it stores and processes the content in a decentralized manner.

### Differences between platforms in the way smart contracts are initiated
Smart contracts can also be divided into at least two groups according to the method of initiation: *automated* and 
*manual (non-automated)*. For those which are automated, it is typical that all parameters are known and the conditions 
of the smart contract are fully automatically executed. This does not require sending any additional transactions and 
spending additional fees for each subsequent execution. The platform has all the necessary data to compute how the smart 
contract execution will end. Logic, in this case, is not arbitrary but predetermined, which means that the result is 
predictable. This allows for assessing the complexity of the smart contract implementation and, for example, calculating 
the fees for its execution in advance.

For the smart contracts that are programmed arbitrarily, execution is not automated. To initiate such a contract, you 
need to create a new transaction at each step. It will recall the next stage of the implementation or the next smart 
contract method, pay the relevant fee, and wait for the transaction confirmation. The execution, in such cases, is 
either successful or not. This is because the smart contract code is arbitrary, and unpredictable events may occur such 
as infinite loop, a lack of certain parameters and arguments, or other exceptional circumstances.

### Summary
Businesses have many expectations from the introduction of smart contracts. However, a smart contract itself is a piece 
of code programmed by somebody. The result of this code should be the consent of all participants of the system 
regarding account balances (mutual settlements). This leads to several conclusions:

* A contract can be considered smart only when all interested parties simultaneously perform it and when a sufficient 
number of them agree with the results of executing it 
* The result of the smart contract implementation are the changes in the ledger containing digitized ownership rights 
for assets

Thus, a smart contract can only manage assets that have already been digitalized. The register in which these assets are 
recorded should be the only primary accounting system for **all** its participants. Without meeting these requirements, 
smart contracts are absolutely useless. Inside a single business, they are equivalent to a regular system without such 
properties as *transparency*, *auditability*, and *trustlessness*.

The creation of conditions for the operation of smart contracts is the necessary step towards the automation of business 
processes.

Nevertheless, we must underline the notion of a social consensus, which may influence expected smart contract outcomes 
by rewriting the blockchain. These situations happened a few times even in permissionless networks, and the most known 
event was The DAO hardfork in Ethereum (we have already described it in section 6.1).

**Common myths**

*Smart contracts are designed to function autonomously.*

It is a mistake to believe that smart contracts are able to analyze the environment and act according to the introduced 
changes. Their code is triggered when a payment is received or a message about a certain condition fulfilled. It is 
activated through the external account of the company or through other mechanisms for initiating the execution. Thus, 
not all platforms are capable of fully autonomous implementation of smart contracts.

*Smart contracts can be changed when an error is detected in the same way as a classical paper contract.*

If during the drawing up of a traditional contract something needs to be corrected, the contract can be enhanced with 
the supplementary clauses or edits. Decentralized environment, however, implies no default "administrator" who could 
make corrections to the contract, and there is no central decision-maker who can stop the running program. The only 
solution if a mistake is detected is to create a "corrective" smart contract if all parties of the transaction agree.

By the time this book was written, over the course of the existence of smart contracts, there has only been one known 
case that required manual regulation. In June 2016, attackers hacked The DAO and started withdrawing the tokens from 
their wallets, which not only caused the collapse of The DAO but also a price drop of ether and even bitcoin. An error 
in the smart contract has created an opportunity for the attack. While there were attempts to correct this situation, 
smart contracts were not rewritten—this is impossible. The decision was to conduct a hardfork, as a result of which 
Ethereum Classic appeared.

**Frequently asked questions**

*– Do smart contracts exclude trust from relationships between parties? After all, they include oracles—aren't they 
third parties?*

The principal difference is in the choice. In the case of an ordinary store, the buyer must trust the administration of 
this store. He can only believe that the goods in the shop windows are of high quality, the markings are not forged, 
etc. Administrators can force employees to give false data to the customers in order to get rid of stale products as 
soon as possible. In the case of a smart contract, you can choose trusted parties who may not even know each other, so 
it will be difficult for them to conspire. The decision in the event of certain conditions will be taken by voting. 
While an automated audit will make it able to ensure that voting and calculation of results is performed according to 
the protocol.

*– Is it possible to use a timer in a smart contract so that the platform can know when to make a payment?*

No, timers are not used in smart contracts. Instead, you can program to check the current time for exceeding a certain 
constant value before performing a certain operation or payment, while the interested party may be able to initiate the 
execution of this verification.

## 6.4 Introduction to asset tokenization
When one says "blockchain" or "tokenization", many people tend to think about cryptocurrency and token sales, which made 
headlines in 2017. Tokenization was mostly viewed as simply a process of issuance and sale of utility tokens (see 
below), which bluntly copied the Bitcoin idea of a decentralized currency. This was applied to payments between users of 
a particular business application. And that was how the world has witnessed tokens for cannabis sale, car washes, 
exchanges, IoT, browsers, social networks, data storages, phones, investment funds, and many other things, both weird 
and not. The message that the startups which created such projects were trying to convey was the democratization of 
investment, transparency of operations, and smart contracts between people and machines. In fact, many of these ideas 
made sense perfectly. But unfortunately, the real implementations mostly failed due to reasons such as incompetence, 
lack of a sustainable business model, sometimes even greed or fraud. This is, however, just one version of the story.

The idea of digitalization of all assets and applications with programmable logic to the operations with them is very 
prominent, and it is getting more and more traction. Once or twice every decade, the IT marketplace experiences a major 
innovation that shakes the entire data management infrastructure. The appearance of cryptocurrencies has provided an 
example of how a financial system can be made robust in a completely hostile environment:   fully anonymous, 
permissionless, distributed, and without any security administrators, firewalls or physical perimeter protection. Many 
ideas can be borrowed from that scenario and applied to the much more user-friendly (regarding work conditions) 
environment of enterprise asset management.

Initially, the word token was used when people talked about projects issuing their own coins. In such cases, a token 
refers to a property right that is created and transferred in the accounting system and could be managed by both a 
single company and a group of independent validators. If this system becomes the primary source of information for 
determining the ownership of an asset, then it is called a tokenized asset. In the financial world, people often call it 
a security token, which implies that the underlying asset is a security (regulated tradable financial instrument)—but 
this is only a particular case.

The *goal of tokenization* is to accelerate and improve the security of operation with assets [97].

*Tokenization is the process of transformation of an accounting system so that all balances are managed by users through 
cryptographic keys, and the ownership of an asset is represented by a digital token.*

The concept of token is hardly new: a voucher for a haircut in a barbershop can be considered a token; another example 
is a subway token. Even the US dollar before the cancellation of the gold standard was a token for a certain amount of 
gold.

The term *token* is also used in other fields: in the context of authentication, it is used as an identifier or secret. 
By itself, a token is either a string of data or a physical device for validating access to certain systems, information 
resources, etc. If you try to formulate a concise and comprehensive definition of a token in the context of digital 
assets, it would be similar to the one below.

*A token (as a digital asset) is an accounting unit used to represent a user's balance in a digital accounting system 
which allows proving ownership of a corresponding asset using cryptographic mechanisms, for example, a digital 
signature* [98].

*Tokens* and tokenization can be viewed at four conceptually different levels:

* *User experience*. The token owner is provided with a legal title to a corresponding asset and is also able to quickly 
and reliably transfer this right to other users without having to transfer the asset directly. Owners of a token are 
assumed to recognize the legitimacy and uniqueness of the registry where the record of tokens is maintained; also they 
should trust the custodian of the physical assets (in case a token is backed by any). 
* *Business operations*. Primarily, tokenization implies a blockchain-based ledger (registry) of ownership rights. 
Transferring a token "hand-to-hand" means changing the owner of an asset in the registry which is considered the 
primary source of information for all. 
* *IT infrastructure*. A token can be viewed as a recording method in the registry, which reflects a user's balance in 
the system. At the same time, data backups, role and infrastructure management, integrity of the transaction history, 
and automated real-time audit are related to using a token.
* *Technology*. A token is an account structure where all fields are protected with cryptographic mechanisms such as 
digital signature, zero-knowledge proofs, etc. The account supports operations for updating its state and presumes 
particular allowed transactions, their lifecycle model, rules for processing, etc.

### Issues of existing accounting systems
Digitalization of an accounting system is quite a recent trend, which started around the 1970s. Internet attacks, 
obviously, were not a concern that days, which is one of the reasons why security mechanisms were not implemented in the 
original architecture. Communication processes were not digitalized until the 1990s, and regulation (reporting, AML, 
KYC, etc.) was affected even later.

From the architectural perspective, trust to a party interacted with was not considered an issue since the mentality of 
business behavior was based on confidence supported by legislation and law enforcement. Another common practice which 
was viewed as a good method for achieving security was the software closeness.

The result was that in the majority of cases, accounting was so far performed on paper, while most of the systems, 
having inherited certain legacy mechanisms, were not yet ready for the digital age. And if you summarize the issues of 
existing systems, you will most likely come up with the following list:

* Lack of transparency in the accounting process, particularly in the history of changes 
* Low level of interoperability with other platforms 
* Need to trust the management of a system to a third party 
* High audit complexity 
* Vulnerability to unintended changes

From the technical perspective, these problems were caused by the following issues:

* Technical incompatibility of data formats and a lack of open secure API 
* Lack of strict ordering of transactions as well as of transaction integrity verification 
* Absence of guaranteed data synchronization methods between parties who don't trust each other and who interact within 
a hostile Internet environment

All the above-mentioned factors lead to an inefficient operation, the need for manual intervention, and additional 
expenses on audit and insurance.

### What is a tokenization platform?
*A tokenization platform is a set of components that allows keeping records and performing operations with a particular 
asset through the use of a digital token as well as provides for the reliable storage, processing, and management of 
assets*. The components of a tokenization platform are as follows:
* Ledger 
* Internal payment system 
* Internal exchange 
* Accounts management module 
* Gateways and integration modules 
* Wallets 
* Asset lifecycle management module

In order to reduce the risk of fraud and collusion, different types of actions have to be performed by different 
(sometimes independent) entities. Therefore, there is a set of roles that a tokenization platform needs to support. The 
most important among them are as follows.

*A validator* maintains the normal functioning of a system in accordance with the protocol of token accounting. The role 
of a validator can either be performed by one person or by an entire organization.

*An auditor* is a designated party that has the right to verify transactions, and it is he who “raises a red flag” if 
notices something suspicious. An auditor stores a full copy of a transaction ledger and is also required to prove the 
legality of actions performed on the platform.

*A custodian* is responsible for the provision of external assets tokenized on the platform (if there are any). This 
role is important since an IT platform cannot prevent theft by itself.  

*An issuer* is a party that performs issuance based on the information delivered from the custodian (in some cases, 
roles of issuer and custodian could be performed by one party). Tokens can be issued either in a centralized (the amount 
to be issued is set by the responsible party) or decentralized (several validators reach consensus about how to issue 
tokens) way.

*An administrator* takes decisions on updating and configuring the platform as well as on setting business rules. 

> **Basic principles of tokenization**
>> * Direct asset management by an asset owner 
>> * Reliable and automated audit of the entire transaction history 
>> * Distribution of responsibilities for the platform process management according to roles 
>> * Increasing fault tolerance of tools for storing, transferring, and exchanging assets 
>> * Openness of specification of an accounting system and digital wallets 
>> * Separating asset storage processes from asset management processes

### Operating principles of tokenization platforms
Having acquainted with the tasks of the tokenization platform and the roles on it, we can now list the main processes:
* Governance (system management)—performed by parties who are assigned by the creator/owner of the system 
* Asset storing—performed by a custodian 
* Issuance—performed by an issuer (issued tokens are backed by a custodian on a tokenization platform)
* Transaction validation—performed by validators 
* Audit—performed by an auditor

A simple example of a physical commodity that could be effectively tokenized are the grain receipts. The warehouse 
manages the system and stores the grain. An issuer may be another organization certified to perform corresponding 
activities—it will make an initial assessment of the grain quality and confirm that the warehouse actually stores the 
physical grain. Transaction validation can be performed by both a warehouse and a group of entities responsible for the 
transaction correctness (this group may include the warehouse, the issuer, third-party organizations, etc.). An audit of 
transaction correctness can be performed by users of the system as well as by designated organizations that users trust.

The big difference for users is that now they can store signed versions of all transactions on their devices. Therefore, 
in case of any disagreements, users can provide the court with these versions and indisputably prove their balance (as 
it is a logical result of performed transactions).

If you draw an analogy with a permissionless accounting system such as Bitcoin, it turns out that all the processes in 
it are decentralized. There is no physical provision, and therefore the role of a custodian is not relevant. Issuance is 
performed through a "pre-established formula". The system is publicly available and anyone can be a validator. All 
processes are transparent, and anyone can verify the integrity of the data and perform an audit. User trust to 
validators is practically not required. Such a system is most resistant to censorship.

Notably, both types of accounting systems, permissionless and permissioned, solve their assigned tasks, namely ensure 
transparent audit and secure transactions. However, they are drastically different regarding their application areas.

### Opportunities that tokenization can bring
Technical features of the new infrastructure provide it with a number of advanced features.

> * Creating global decentralized data registries 
> * Creating closely integrated systems with high modularity and distribution of operational responsibility 
> * Real-time and simple audit of an accounting system 
> * Moving and trading assets on the Internet that has no borders (the opportunity to create the Financial Internet)

### Transparency of accounting system processes 
A tokenization platform makes it possible to have most business processes transparent. As practice shows, it is more 
effective to prevent dishonest behavior at root rather than deal with its consequences. A tokenization platform can 
operate under strict rules, the purpose of which is to prevent opportunities for malicious acts. In such an accounting 
system, business partners can always check each other's actions.

The more there are independent validators in the system, the more stable it is against failures. Tokenization increases 
the reliability of data because validators' database copies are all synchronized with each other in real-time, and 
automatic backups are permanently created. 

### How does tokenization lead to increasing the cost of assets?
Tokenization can increase the credibility of an accounting system data through the digital signature and by means of 
validators who reach consensus between each other. This indirectly affects the value of tokenized assets compared to 
those that are accounted for in a traditional way.

A simple example would be suitable here. Imagine that you go to a drugstore to buy medicine. They give you a transparent 
box without identification marks containing white pills. Will you take these pills? Of course you won't because there is 
nothing written to say what they are. What if your pharmacist friend worked in the drugstore and said that these pills 
would really help you? Most likely, you would still have doubts. After all, it is still unclear what they contain, where 
they came from, and who produced them.

What if these were the pills in factory packaging with labeling stating that they are certified by leading 
pharmaceutical manufacturers and that active substance has been tested and proved to be clinically effective? Such pills 
inspire more confidence. Also, if you have a doctor's prescription for this medicine, then there should be almost no 
doubt.

How can the medicine be even more reliable? Let's think a little futuristically and imagine that you can reliably check 
the pills for compatibility of the substances they contained with the biochemical processes that occur in your body.

For example, the World Health Organization has ensured that in every drugstore there is an apparatus that, at the 
request of a buyer, takes a small drop of her blood (or any other material for analysis) and a tiny part of a medicine 
sample from the package. Then, it analyzes the state of health of the buyer and the quality of substances contained in 
the pills. Having evaluated the data, the device then displays the results of the benefits of the drug in the case.

What do you think? Would the medicine in this scenario be more valuable to you? Certainly it would.

The value of the medicine increases according to how much valuable information there is about it. Note that the 
properties of pills have not changed. This is based on the fact that confidence and independent verification is possible 
in all operations with the asset, so its *provenance* is established with reliable authentication of data sources.

### Conditions for effective application of tokenization platforms
To make the application of a tokenization platform more effective, there is a set of necessary conditions that have to 
be fulfilled:

* Implementation of a digital identity which could bind actions of a  particular participant in the system to their 
actual identity 
* Effective methods for decentralized decision-making and resolving conflicts 
* Established scheme of key storage and management

This is not the full list. There are also other conditions which must be fulfilled before systems of tokenized assets 
work properly and in full measure. In fact, they match those required for the blockchain implementation—we have 
mentioned them in section 5.3.

### Risks
As we have already mentioned, tokenization assumes that digitized property rights registries are the primary sources of 
information about the asset owners. This means that from a legal perspective, transferring a token within the system is 
equal to changing the legitimate owner of an asset.

Therefore, some of the primary risks are those related to the provision of tokens. For example, while the accounts of 
users display correct balances in the accounting system, real assets could be replaced, destroyed, or stolen. This 
problem mostly relates to properties of tangible assets rather than a tokenization platform—digitization allows many 
advantages but does not guarantee the safeness of the provision. Another risk is that the private keys of users could be 
lost or stolen, leading to a permanent loss of asset ownership. This risk is hard to estimate and protect against.

### Difference between tokenization and digitization
A straightforward digitization of ledgers leaves open the question of security. *Tokenization transforms asset 
management: an order execution model is replaced with a model of direct asset management using cryptographic 
mechanisms*. The main difference is the elimination of a role (e.g., administrator or a module that operate with 
corresponding permissions) that changes balances on users' accounts directly—this is what the order execution model is 
based on. Tokenization presumes that users control their balances using cryptographic keys that no one else, including 
admins of the system, knows.

There is also a crucial difference between the registry of authenticity and the title registry (ownership right 
registry). A registry of authenticity associates a specific asset (e. g., a designer bag) with its manufacturer and 
allows verifying its authenticity. This registry contains only objects without current owners. A title registry is an 
information resource that contains data about existing and past ownership rights to certain assets.

### Why use blockchain technology?
The key question is, why is blockchain better than the traditional approach to building and managing a database? There 
are several reasons to use blockchain for tokenization and for the ledger protection. In section 5.1, we described some 
of the fundamental properties that blockchain may provide for depending on the environment. Here, we would like to focus 
on some of the more practical and specific features.

* Every token holder can prove that his balance represents the result of correct execution of a set of 
transactions—*auditability*
* No one can change the balance of accounts unnoticed—*integrity*
* It is hard to delete the state of the ledger since there are multiple copies of it synchronized real-time with no 
central point of *failure—robustness* and *accessibility*
* It is easy to prove who initiated which *action—non-repudiation*
* It is possible to guarantee that a transaction was approved by all the necessary parties—decentralized 
*decision-making*
* Some participants in the system may be able to perform an audit but without the ability to modify data—*trustless 
transparency*

If you try to achieve all these properties using the possibilities of a traditional database, it would rather be wheel 
reinventing—you would eventually come up with the blockchain solution.

### Summary
In general, tokenization is a new trend in improving the *accounting systems* and the IT infrastructure.

Advantages of tokenization platforms become primarily evident when working with digital assets: transparency of 
accounting (as well as of business processes), instant and easy audit, reliable storage and synchronization of data 
between parties (validators of the system), and the ability to prove the data integrity and immutability. These features 
make it possible to create accounting systems in which trust between parties is either not necessary or is at the 
minimum level—*honesty* will be guaranteed by the rules of the system. Therefore, predictably, all assets will be 
tokenized in the future.

**Frequently asked questions**

*– What is the difference between tokens and currencies (money)?*

Despite the fact that people already use this word in their everyday life, a token is a technical term that in practice 
denotes the right to any asset, including a currency. Tokens are a digitized right of ownership of an asset and can 
also be used as a means of payment. Exchange of tokens or payment in tokens for other goods and services are somewhat 
equal to barter.

*– How are tokenization and blockchain technology related?*

A digital accounting of value creates a number of risks. It is important to have a system where issuance, storing, and 
transferring of value is performed with a sufficient level of reliability, efficiency, and transparency for all 
participants. For this reason, blockchain technology is well suited: security of data is guaranteed by cryptography, 
while any attempts to violate the accounting rules are particularly apparent to an auditor. Moreover, ordinary users, if 
they have detected any violation in the operation of a tokenization platform, now have an opportunity to undeniably 
prove to an auditor that they have been deceived.

*– Who controls the tokenization platform?*

If a company decides to tokenize its asset and controls the validators of a tokenization platform, then essentially it 
is the owner of a platform who controls it. The company centrally makes decisions on updating the protocol and relevant 
rules for token accounting. If the task is to decentralize the accounting system, then the validator role can be 
distributed among partners or even clients of the company. In this case, the platform becomes independent with regards 
to processing and confirmation of transactions. Therefore, it provides the trustless property for dealing with 
corresponding tokens.

*– What is a utility token?*

Utility tokens represent the right to use a system functionality. They can simultaneously possess various properties: 
claim for a service, part of the value created by the ecosystem, and certain properties of an internal currency. 
Decision-making and issuance are usually centralized.

*– Who is responsible for errors on the tokenization platform?*

If a tokenization platform is centralized, then it is the owner who is responsible. If a platform is decentralized, then 
the responsibility is distributed among its validators or users.

*– Who can be the validator in the token accounting system?*

The validator could be anyone who is able to maintain the normal functioning of a computing system in accordance with 
the protocol of token accounting. The role of a validator can either be performed by one person or by an entire 
organization. In general, transaction validators could be auditors, regulators, as well as partners of the creator 
(owner) of a tokenization platform (e.g., members of a consortium performing a particular business function).

[USER PRIVACY IN OPEN SYSTEMS](https://gitlab.com/oleksandr.kurbatov/blockchain-and-decentralized-systems-book/-/blob/main/chapters/volume-1/7-user-privacy-in-open-systems.md) 




















