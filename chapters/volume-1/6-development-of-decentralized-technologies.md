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

[Figure 6.7] - Litecoin

For the proof of performed work, Litecoin uses the script hash function. This hash function uses SHA-256 as a 
subprogram, relying on a large number of arithmetic calculations but also requiring quick access to large amounts of 
memory, which makes the creation of ASICs less profitable. Although Litecoin has only a few differences from Bitcoin, 
it has found an audience and community which permanently supports it. 

### Dash
The Dash project (Fig. 6.8) was launched in 2014. In Dash, the period of block creation is also shortened, which in turn 
contributes to increased network capacity. The hashing algorithm is PoW X11, which initially allowed solving the 
resource-intensive task energy-efficiently and only on graphics processors, but later integrated circuits (ASICs) were 
designed and released for this purpose.

[Figure 6.8] - Dash

Dash supports *PrivatSend* and *InstantSend* technologies. PrivatSend is the technology of mixing coins; in other words, 
the technology of entangling the history of coins origin. *InstantSend* is an instant payment technology, where the 
recipient does not need to wait for transaction confirmation because it is guaranteed to be added to the mainchain.

Consensus-reaching in Dash also has its peculiarities: PrivatSend and InstantSend options are implemented using the so-called masternodes, which are a separate group of validators that reach consensus between themselves [90].
The Dash project has made its priority to increase user privacy. For this, they have implemented a modification of the CoinJoin approach. This approach involves pooling several payments in one common transaction that mixes coins and prevents the tracing of a straightforward history of their origin.
The basic principle of CoinJoin can be explained with a practical example: imagine four people, each of whom first put their money in one common safe and then independently took the same amount of money from it. An outsider, even if had known the total sum of money which was put in the safe, would have never known the share of each person, namely who has how much. In Dash, this principle is implemented through the subnet of masternodes. In fact, any node can become a masternode: all that is required is to have a certain amount of coins on your address. These coins will be frozen and will serve as a deposit.
Change management in Dash also has its peculiarities. The total mining profit is distributed: 10% goes to founding members (for the development of the Dash project), 45% to regular nodes, and 45% to masternodes. Managing the budget for the development of the Dash project is done through voting of masternodes. In this way, all developers' proposals for the improvement of the protocol (e.g., improving the network quality, capacity, etc.) can either be accepted or rejected by masternodes. Yet the system is not completely decentralized because not all users can vote for the improvements. However, any user can suggest her own improvements, and this may potentially be supported and sponsored. For this, a user needs to leave a treasury request with a suggestion for an idea, and the owners of masternodes will vote for it.













