# 5 Blockchain technology


## 5.1 Blockchain technology and its capabilities

Discussion of blockchain technology as a stand-alone topic separate from the Bitcoin context began early 2014. At that 
time people started to recognize that Bitcoin’s decentralized approach could be applied in other areas, and that the 
underlying blockchain technology was the key to this. For example, the use of this technology in the accounting systems 
could protect data against unauthorized changes. Providing the properties such as *immutability*, *integrity*, 
*trustlessness*, *reliable reconciliation* and so on in systems resonated broadly in the community.

Before Bitcoin, all financial systems were protected with "traditional" methods such as firewalls, access control 
systems, and security admins. Very few organizations supported an open API or allowed third-party developers to create 
extensions or alternative applications. For the first time ever, Bitcoin showed the world that a robust financial system 
did not require a single decision-making center. It also showed that such a system could be transparent to everyone and 
be available to be audited as well as provide a higher level of user privacy.

*What blockchain technology has enabled is the way of storing and synchronizing data between parties which do not trust 
each other*. Note that absence of trust between participants here is not mandatory. You can trust specific network 
nodes. Though, the main blockchain's advantage is that even in the case when there is no one to trust, you can fully 
work with a system. Rather than by relying on trusted parties and communications to achieve and maintain integrity, 
integrity is achieved with blockchain technology by grouping all the data about existing transactions (or other records) 
into blocks, each of which is cryptographically linked to the previous one.

In everyday speech, the word blockchain can be used with completely different meanings. Therefore, this difference 
should be bespoken and understood. We will observe the three most common meanings. The word blockchain can be used to 
refer to a specific accounting system (e. g., Bitcoin). Another usage of blockchain is to describe the implementation of 
the technology itself as relating to a particular use-case. The most accurate usage of the term blockchain is as an 
approach to data organization and decision-making in general. Given this range of possible meanings, this term should be 
used thoughtfully with paying attention to the context.

In fact, millions of discussions about why blockchain is useful can be expressed in one phrase: unlike traditional 
accounting systems, where all data is hinged on blind trust—no matter whether the data is obtained within a system or 
externally—*accounting systems based on blockchain technology make it possible to verify the integrity and the 
chronological ordering of the data*. As a result, the data may prove to be more reliable for both end users and even for 
the system owners.

> **_NOTE:_** *"More reliable" should be understood as the ability to independently store and verify the integrity and 
> ordering of data while relying on some external data, such as public key certificates, third-party cryptographic 
> libraries, etc. This also implies the presence of the strictly determined rules with which you can verify the 
> mathematical correctness of transactions based on them.
> 
> *Blockchain is not a magic bullet which makes all the data within a system authentic.*

### Degrees of decentralization

The degree to which a system is decentralized is not always easily measurable. The ultimate goal for any secure system 
is not decentralization per se, but rather to achieve the optimal combination of required properties such as privacy, 
integrity, accessibility, resilience, trustworthiness, and performance. Decentralization is beneficial to the extent it 
facilitates these results. For example, if a system is fully anonymous, it can more easily provide for censorship 
resistance, trustless verification of data, and economic incentives to distribute decision-making.

If we try to graph the possible degrees of the system decentralization on a linear scale, it might resemble Figure 5.1.

![Figure 5.1- Degree of decentralization on a linear scale](/resources/img/volume-1/5.1-blockchain-technology-and-its-capabilities/5.1-degree-of-decentalization.jpg)

All these cases can be evaluated, compared, and contrasted by evaluating them with respect to a few criteria. The first 
such criteria is the number of validators; the second is the requirement that validators maintain their anonymity; the 
third is the need for the validators to maintain their reputations and for their reputations to be known to system 
participants.

*Digital ownership ledger*. On the left side of the scale, there's a system with a single owner who manages all of the 
assets of the business. Such an owner makes all of the business’ decisions. Thus reaching consensus in such a system is 
not required, while the issue of trust is not only relevant but is rather critical. This (leftmost) dot can be described 
as an application of blockchain technology in a centralized environment (in fact, it could provide certain benefits; see 
section 6.4).

If there are two partners who jointly manage a company account, but who do not trust each other to properly make 
payments, authorize transactions, and maintain an accurate history of such transactions, then the situation is very 
different. They would both have to sign each transaction and store the record of each transaction in their own copy of 
the ledger. Blockchain technology would help to automate this process. The simplest way to reach consensus in this 
scenario is using multisignature—each transaction will correspondingly be signed with two signatures. There is no 
anonymity requirement because both partners must see the transactions they sign. The reconciliation of transactions 
occurs very quickly, and they can be easily ordered and sorted because each of them contains a timestamp. Protection 
against censorship is not a scenario requirement. Also, there is no requirement that partners be motivated to maintain 
their nodes.

*Reconciliation*. This case will be reviewed below in detail. Its key feature as compared to the previous one is the big 
number of participants which are to reach consensus; it can be up to several decades (in most cases, their number is 
limited and known from the beginning). In this case, the system becomes more decentralized and the multisignature 
approach is no longer enough to reach consensus. Rather, consensus reaching algorithms such as BFT, FBA, PoI, and PoA 
become applicable (for further details, see 5.2).

*Tokenization platform*. Here, a higher level of decentralization can be achieved, and the number of parties involved in 
reaching consensus can vary and is not limited. This case requires the application of the FBA consensus mechanism.

*Voting platform*. Public voting implies that all participants have to reach consensus and be able to independently 
validate the result of the election. Nevertheless, all participants have an identity, which can be validated, even 
though all the people cannot possibly know each other (and therefore fully trust). The FBA consensus algorithm is the 
best for this scenario.

*Cryptocurrency*. The design of the first cryptocurrency carried with it the implication that the number of participants 
is not defined and that they are all anonymous and have no reputation. To achieve this, there remains only one existing 
approach for reaching consensus—the one based on proof-of-work. In such cases, capacity is limited compared to the 
algorithms applicable for environments where the participants know each other’s identities. Also, an economic mechanism 
for incentivizing proper participant behavior is required in order to ensure that the network remains decentralized over 
time. This essentially leads to the liability of having a means of payment with the same properties (so far it is being 
solved by the presence of an internal currency). There are a couple of issues inherent to the Bitcoin architecture. 
First, transactions are not fully private and can be traced (in fact, this issue is partially solved by other 
cryptocurrencies such as Monero and Zcash (see section 6.2). Second, there is a tendency for a large amount of 
computational power to become concentrated in the hands of a small number of collective pools.

*?—new cryptocurrencies* (not yet invented). The last dot on the scale shows the case where decentralization and privacy 
are at the highest possible level. Regarding their features, the systems are ideal and exist only theoretically, but we 
can formulate a number of requirements for creating new models similar to such systems. It would enable *full privacy of 
transactions* (something like ZCash, but without trusted setup), which means that an outsider wouldn’t even be able to 
observe the fact of a transaction. This cryptocurrency would support fully decentralized mining, but without incentives 
to create pools, thus resulting in consensus (similar to what DAG provides for but without the need for a centralized 
coordinator). Also, users will be able to communicate in a fully p2p manner using the mesh network approach, instead of 
a centralized internet provider that today stands in the way between users and a decentralized environment.

To conclude, any system in which it is necessary to find agreement between a set or rarely changing number of 
participants will most likely use a version of the BFT consensus algorithm. An example is any kind of voting: from a 
board of directors to a nationwide council (obviously you do not need an internal currency in such a case). The 
censorship issue is resolved not by the technical capability of preventing its possibility, as in Bitcoin, but by 
immediate disclosure of the fact itself because "everyone knows everything".

Depending on the tasks that an accounting system is to solve, its database will vary widely according to the properties. 
In the community, several terms are used to distinguish each type by the level of availability.

*Permissionless*. Anyone can participate in any process of the system without any restrictions or permissions. If a 
system is claimed to be permissionless, then this statement should hold true for every single process occurring in this 
system. Sometimes these systems are referred to as *public blockchains*.

*Permissioned*. In order to participate in a certain process, a permission is required. If some system is declared to be 
permissioned, it means that at least one or more processes require additional authorization. Sometimes these systems are 
referred to as *private blockchains*.

### Architecture of blockchain

In the chain of blocks, each subsequent block contains a hash value of the previous one (Fig. 5.2). As a consequence, an 
attempt to change the data recorded on a historical block will cause changes to all subsequent blocks. This change will 
be noticed by other participants (network nodes) and can be rejected if improper, ensuring database integrity.

![Figure 5.2- Link between blocks in the chain](/resources/img/volume-1/5.1-blockchain-technology-and-its-capabilities/5.2-link-between-blocks.png)

A sequence of blocks is thus formed—a chain (hence the name "blockchain"), where each new block contains a hash value 
derived from the prior block. Of course, the hash function which links all of these blocks must be consistent. If you 
make changes to previous blocks, then every single subsequent, existing block will become incorrect, meaning that all 
blocks would have to be recreated. This feature makes it impossible to backdate the history without this being noticed 
by all other participants.

### Properties of blockchain

Theoretically, blockchain technology can be used to organize data in any database management system (DBMS). The chain of 
blocks can be organized at an abstract level, and the software does not have to store transactions in blocks. It is 
enough that the block header is formatted correctly. Therefore, a network node can organize the storage and the 
processing of transactions in any convenient way, as well as synchronize with other nodes; the data is serialized into 
blocks of a certain format and transmitted over the network.

> **Properties of an accounting system that can be provided through using blockchain**
>> * *Integrity of database history* 
>> * *Minimization of delays related to synchronization and backup* 
>> * *Ability to work with a group of validators simultaneously* 
>> * *Ability to perform real-time audit* 
>> * *Ability to use light clients and SPV nodes* 
>> * *Timestamping (anchoring and time-based data fixation)*
>> * *Trustlessness (the required level of user trust is minimal)*

The features of implementing blockchain properties for different environments are shown in Table 5.1.

![Table 5.1- Properties that blockchain may provide](/resources/img/volume-1/5.1-blockchain-technology-and-its-capabilities/table-5.1.png)

When the decentralization degree of an accounting system increases, i. e. the number of validators goes up, the need 
appears for a mechanism for reaching consensus regarding the correct system state.

The owner of a full node is able to independently verify the accuracy of the complete transaction history. This in turn 
renders system operations transparent to an auditor. If a user creates and submits to the network a transaction that, 
for example, tries to spend non-existent coins, this incorrect transaction will be immediately noticed and discarded by 
all honest users. If a malicious validator creates a block with an incorrect transaction, this block will be noticed and 
entirely discarded by all honest users as well.

Another possibility offered by blockchain is the provability of fraud on the part of validators of an accounting system. 
If a transaction which was once accepted in the system is later canceled by validators, a user will have an indisputable 
proof that she has been cheated. The corresponding block contains a proof (a digital signature or a PoW) that validators 
voted for this transaction before. Thus, a user has an opportunity to prove that validators are engaged in fraudulent 
behavior such as rewriting the history of certain blocks.

In this way, even in the case of a centralized system—where for some reason the chain of blocks had been rewritten—a 
user in her digital wallet will have proofs in the form of transactions authenticated by a digital signature of the 
organization that manages this system.

Therefore, the issue of trust between parties in the blockchain-based system is no longer that pressing. Its operational 
reliability can be ensured by the protocol rules.

A user has an option to install a light client application, which would allow her to see the current actual state of the 
database without having to run a full node. This property, however, makes the most sense in a permissionless 
environment, but only under the condition that a user trusts that the system is working reliably (e.g., in the Bitcoin 
case, it means that a user trusts that the majority of hashing power is controlled by honest validators).

When an accounting system reaches a sufficient level of decentralization and records immutability, what becomes of great 
value is a timestamp. It allows logging the fact of knowledge of particular data at a particular time moment and—what is 
the most important—afterward prove this fact to anyone. As a result, users can be highly confident that the changes to 
the database state are very likely to match the sequence in which these changes have been initiated.

### Applications of blockchain technology

Now, let's discuss in which situations blockchain technology is worth applying. After all, blockchain allows you to 
design a reliable system, where the human factor is minimized. Let's consider five of the most suitable and intriguing 
application areas of blockchain technology.

> * *E-voting* 
> * *Supply chains* 
> * *Decentralized trading*
> * *Public registries* 
> * *Mutual settlements*

E-voting. Every voter can be sure that their vote will be taken into account. Only the sanction number of votes are 
allowed and it is difficult to steal someone’s vote. All transactions on the network could be confidential, which 
reduces the probability of establishing the identity of a voter. During voting, anyone can check the correctness of all 
transactions containing votes to make sure that they have all been taken into account (see Fig. 5.3).

![Figure 5.3- E-voting](/resources/img/volume-1/5.1-blockchain-technology-and-its-capabilities/5.3-evoting.png)

A voter must be able to prove that he has the right to vote, and will be constrained to only the allowed number of votes 
(usually one vote). Most likely, this would entail the need to create certification centers where voters register their 
public keys. These centers could be managed by an organization that would verify that one person has not registered 
multiple keys. Therefore, trust shifts from electoral authority to certification centers, which have to be audited as 
well. Digital identities in some countries, such as Estonia, partially solve this issue.

It is necessary to choose a consensus reaching algorithm which allows each participant to tally the votes and match her 
result with the other participants' ones. Suitable algorithms for that are PoS, FBA, BFT (more details in 5.2).

Another problem is to find a way to distribute the software for voting and auditing because threats can arise at all 
stages: from design to installation and execution.

Thus, with the blockchain application, an e-voting system obtains valuable properties. But, there are a number of tasks 
that are to be solved in order to have such projects successfully and securely implemented.

*Supply chains*. From the producer to the consumer, goods pass through a sequence of intermediaries—the so-called supply 
chain. Yet, the presence of the manufacturer's logo on the product does not necessarily mean that the product itself is 
original. There are many risks along the way, including the risk of substitution.

The implementation of blockchain into the supply chain accounting system can protect the goods (namely, the data about 
their state, location, etc.) on their way. It would be possible for a buyer to obtain exhaustive data considering the 
product. This data is entered into the database by specified persons responsible for production, assembly, loading on a 
vessel, transportation to the next point, etc.

To forge transactions that have already been added to the chain of blocks, an intruder would need to force validators to 
join a conspiracy or hack into several different servers on which node-validators run. In addition, the use of reliable 
mechanisms of data synchronization between participants in the chain helps prevent inconsistencies in their accounting 
systems. This approach will give certain guarantees to buyers and make the supply chain management system more secure. 
Suitable consensus algorithms for this task are FBA, BFT.

Ultimately, it is the consumer who can raise a red flag if counterfeit is found. With blockchain-based supply management 
systems, it is easy to verify which product (with unique barcode) should be delivered and sold through which store. So, 
if a consumer buys a pack of pills in Rome, but the verification system—which the consumer accesses through scanning the 
barcode via mobile app—says that this particular pack was delivered to Paris, it raises obvious concerns at least about 
the honesty of the importer (basically, if the barcodes are unique, it means that the pack of pills delivered to Paris 
is fake).

*Decentralized trading*. With the application of blockchain technology and decentralized approach, it is possible to 
conduct auctions on many small trading platforms. A product that is put up for sale on one of the trading sites will be 
available on all others (Fig. 5.4). Thus, it allows sellers to present goods to a wider audience of buyers.

![Figure 5.4 - Example of a decentralized auction](/resources/img/volume-1/5.1-blockchain-technology-and-its-capabilities/5.4-auction.png)

A seller saves a lot of time since there is no need to manually duplicate information about the goods on each site. 
Moreover, trades are conducted on the sites simultaneously, and the data about bids is synchronized in real time. 
Therefore, each site constantly receives information about the bids on each lot. Also, the winner is evident to all the 
users.

Therefore you obtain a system where the human factor is minimized, performing an audit is neither difficult nor 
time-consuming, and the data of the entire transaction history cannot be changed unnoticed. Suitable consensus 
algorithms for this task depend on the requirements for anonymity and censorship resistance—PoW, PoS, FBA, BFT.

*Public registries*. A public registry is accessible to everyone by definition. If such a register is managed by one 
organization, then obviously there is no guarantee whether the data is backdated or not; you trust this data just as 
much as you trust the organization.

The main reason why public registries are maintained by government agencies is that their users do not want commercial 
organizations to control a black box system (a system in which its user knows only the input and output data and does 
not know all the processes running within it). A blockchain-based registry could solve the issue of trust by 
decentralizing the database and the process of synchronization, preventing any single organization from making hidden 
changes. The database copy can be maintained by several organizations or by a wide public (Fig. 5.5). Hence, single 
censorship becomes impossible—there are many registry owners who validate all changes.

Therefore, maintenance of public registries can be transferred to non-government organizations, which results in the 
reduction of state spendings and increased citizen satisfaction. The most suitable consensus algorithm for this task is 
FBA.

![Figure 5.5 - Decentralized public registries](/resources/img/volume-1/5.1-blockchain-technology-and-its-capabilities/5.5-registry.png)

*Mutual settlements*. Let's imagine that ten banks want to build a partnership to provide the service for their 
customers faster and cheaper. For this, they will need to create a shared payment system for arranging direct settlement 
with businesses and individuals they work with. Therefore, they can build a private blockchain-based system in which 
they store all the necessary data and perform mutual settlements.

Each bank will be able to maintain its network node and check the entire transaction history. Since each bank has its 
own interests, the probability of collusion in the system is low (much lower than in the case where a single corporation 
provides the settlement). Moreover, such an accounting system can make the settlement processes much cheaper and safer 
mostly because banks involved do not need to pay third-parties for providing the settlements. Transaction history is 
stored on the blockchain, which is managed by a number of companies, making forges extremely difficult. Also, it is much 
easier to conduct a fairly reliable and automated audit compared to traditional systems. Suitable consensus algorithms 
for this task are FBA, BFT.

### Summary

Having analyzed the cases of technology application, the advantages of a blockchain-based system become more evident:

* Simple verification of the database integrity 
* Timestamping of all changes 
* Simple real-time backup 
* Secure synchronization of data in a decentralized environment 
* Simple audit of an accounting system in real time

Blockchain technology can provide reliable data storage and give the system a number of valuable properties. However, 
the complexity of designing, configuring, and maintaining it is usually much higher than for the centralized 
alternatives; thus it is necessary to approach this issue consciously.

It is also worth noting that all the above-listed advantages are not 100% guaranteed—blockchain is not a magic pill; 
success depends on the correctness of rules and implementation of a particular blockchain-based system.

**Common myths**

*Blockchain can verify any data that is stored in it.*

The peculiarity of this particular myth is that the mistake is in the very formulation; also, some people suppose this 
by default. It even happened that people wanted to build an accounting system, where the stock index would be checked 
and recorded on the blockchain. On our question, "What is the need for blockchain here?" the company replied, "We need 
to make our data trusted."

When a new block is created, validators can only rely on the data of the previous blocks, and the only thing they are 
guided by are the protocol rules, which verify the correctness of transactions. It is impossible to verify the 
authenticity and correctness of data that somebody records in the blockchain in any other way. The trust shifts to that 
somebody because, obviously, as with the example with stock index, validators cannot know the price, let along that it 
is correct (this is not their job). The only fact validators can assure is that the data recorded in the blockchain was 
not changed at a particular moment of time in the past.

*Blockchain is slow and requires a lot of energy.*

This could be true if you mean Bitcoin but not blockchain technology as such. Bitcoin uses a consensus mechanism based 
on PoW, which indeed has this limitation. An accounting system that uses blockchain technology in general can have a 
consensus mechanism that provides for a sufficiently high capacity, making it possible that transactions are fully 
confirmed within a few seconds. Moreover, this consensus mechanism will not necessarily require abundant computational 
resources from validators. This means that it is specifically the proper design of the consensus mechanism that allows a 
blockchain-based system to achieve required specifications (capacity specifications in our case).

*Blockchain ensures data reliability and transparency.*

Transaction history, in fact, cannot be changed. However, in order to alter the state of the actual data, it is not 
necessary to alter existing blocks and their connections. It is enough to create an alternative chain and compel other 
network participants to consider this chain the primary one. So basically, it all depends solely on the consent of 
parties that reach consensus.

Suppose that five banks have decided to create a blockchain-based payment system. In such a system, the banks need to 
negotiate in order to make payments. To facilitate this, they determine the rules by which they will reach agreement. 
Each of the banks acts according to its own interests or by mutual decision: if the banks collude, they can very simply 
rewrite the chain of blocks, that is, replace it with an alternative one. Thus, the fact of using blockchain does not 
necessarily guarantee transparency and reliability of the accounting system for an outsider. However, auditors who have 
access to the transaction history and even the clients will notice the attack and will be able to undeniably prove its 
fact.

*Bitcoin blockchain is encrypted.*

Despite a mistake in the formulation, such an opinion—that a shared database is encrypted—is widespread and common: some 
even claim that bitcoins are encrypted money. Of course, this is not correct. The blockchain technology, by itself, can 
be used to organize any data (including encrypted data). However, if the data is used for a fully transparent coin 
accounting, the transaction data must be in an open form so that anyone could verify its correctness. We also need to 
mention that certain extensions such as confidential transaction mechanism allows making the data of a particular 
transaction private (see section 7.2).

*Blockchain will replace servers.*

Such an opinion is quite common among people who have no idea about what blockchain is about. Even if some business 
logic will be implemented by blockchain nodes (such as execution of smart contracts), there is no way around one simple 
fact: “nodes” are servers. A similar misbelief—that servers are not needed anymore—was also popular during cloud 
computing boom. There again, there is always a server but probably in another place (Amazon, IBM, Google). 

*Blockchain will replace notaries.*

This myth relates not only to notaries but essentially to any intermediaries. This is not true. If your accounting 
system deals with any data from the exterior world, blockchain won’t exclude those who would verify it and register in 
the main database.

Imagine that Alice sells her house to Bob. Blockchain technology could optimize this process, but it won’t replace 
intermediaries such as notaries. There is no way for validators to find out that it was really Alice who used her key to 
create a transaction for selling her house, that Bob is actually a real person, that Alice is doing this according to 
her own will (not at the gunpoint for example), etc.

In blockchain, validators only know that math adds up. But it is never responsible for what is going on outside its 
database. In fact, parties who transmit the data from the external world into the database are called oracles (for 
further details, see 6.3).

**Frequently asked questions**

*– Does blockchain as a technology need standardization?*

Within decentralized systems protocols, standardization may not bring the desired result since each system is designed 
in its own way. But, for solving problems common to a majority of systems, standardization will be useful. For example, 
the procedures for the generation, storage, and backup of keys.

*– Are there any versions or models of blockchain technology in which users are granted different levels of access?*

It would definitely be convenient if, for example, notaries could have access to all data about transactions, while 
other users could only have access to the data which directly concerns them. However, blockchain technology only 
determines the manner in which data is organized and is not directly related to managing access levels to this data. 
Obviously, using a combination of different techniques, one can create a comprehensive solution for this task (yet the 
description of such solution is not the primary goal of this book).

## 5.2 Differences in consensus approaches

It would be safe to say that the consensus mechanism is a cornerstone of any decentralized accounting system. Without 
understanding the work principles of these mechanisms, you cannot design a secure and efficient system. Sometimes 
different decentralized systems are blindly compared against each other according to their performance parameter, even 
if they use different mechanisms for reaching consensus. However, if these mechanisms are fundamentally different, then 
the accounting systems also differ in a variety of criteria, such as reliability, efficiency of resource usage, 
limitations, level of security, and performance. This, in turn, strongly affects the scope of application of the 
systems. Let's figure out which characteristics are the most important for typical decentralized systems and how they 
depend on the approach to reaching consensus.

![Figure 5.6 - Reaching consensus regarding house repairs](/resources/img/volume-1/5.2-difference-in-consensus-approaches/5.6-house-repairs.png)

### Proof-of-work

The consensus mechanism based on PoW has become popular due to cryptocurrencies. It is the simplest and at the same time 
the most reliable approach in the case of complete decentralization and anonymity. Though, the operational principle of 
PoW is generally difficult to explain to an unprepared person. We will try to do it through a simple example.

Let's imagine that in order to determine the color in which the house needs to be painted, its owners decide to hold a 
competition. According to the rules, each of them takes a ride on a tram in order to obtain the ticket. The participant 
who gets the ticket with the smallest number will be the one to decide what color the house should be painted in. 
Suppose that tickets are sold in random order. Obviously, the owner of the most tickets has a *proportionately greater 
probability* to get lucky.

The more essential are the consequences of taken decisions, the higher is the motivation of owners to buy more tickets 
for each next vote. This turns into a kind of an "arms race". In fact, the increase of computing power among the Bitcoin 
network participants proceeds in a similar way. However, if there is any suspicion that someone colluded with the seller 
to supply the required tickets, then some of the owners can build a new house where they will make decisions according 
to other rules.

### Proof-of-stake

The idea of PoS is similar to the voting of shareholders of a company—the one with a larger stake in the company 
correspondingly has a greater voice in making the final decision. In this way, decisions in the system are not really 
determined by the number of votes but rather by their weight (Fig. 5.7).

![Figure 5.7 - PoS functioning principle](/resources/img/volume-1/5.2-difference-in-consensus-approaches/5.7-pos.png)

The specific threshold of this weight can differ and depends solely on the rules in a particular group: some would 
require the simple majority (i.e., 51%), others, two thirds (66%); and there could even be cases when 90% of the voters 
must support a decision in order to have it made.

The time of block creation with PoS can be much less as compared to one with PoW. However, designing a sufficiently 
reliable consensus reaching algorithm based on PoS for a decentralized accounting system, where anyone can become a 
validator, is quite complex.

### Delegated proof-of-stake

This approach to reaching consensus is alternative to those mentioned above. The main idea of delegated proof-of-stake 
(DPoS) is that here decision-making is performed only by the delegates [79; 80]. Usually, those who become delegates are 
known from the beginning. Very often these are the people known for their achievements in a particular industry. In the 
system, they have accounts associated with their identity. Delegates are chosen by the users themselves.

The weight of the vote depends on the number of coins on a user's balance only at the stage of choosing the delegate. 
Delegates themselves vote on equal terms, that is the outcome of voting is determined by a simple majority of delegates 
(validators). Since the number of delegates is limited, their activities are easier to optimize, and as a result, this 
consensus reaching algorithm allows a system to achieve high capacity.

The idea of DPoS development and implementation belongs to the team of developers of the decentralized platform 
Bitshares.

### Proof-of-importance

In order to increase the democratic approach to decision-making, the PoS algorithm was elaborated, and eventually what 
is now called proof-of-importance (PoI) [81] has come into existence. The difference of PoI is that unlike PoS, where 
weight is measured by the stack, the weight of a participant's vote depends on a number of various factors. The 
algorithm considers the degree of user participation in the system operation, and thus everything matters here: the 
nature of user interaction, the origin of coins, whom they are sent to, and even the general user behavior in the 
system. Therefore, the situation when the rich become richer is excluded. The first working implementation of PoI was 
used in the NEM protocol.

### BFT

The name BFT (Byzantine Fault Tolerance) derives from a comic description of one problem that was implemented in the 
1980s under the development of a protocol for highly reliable systems [82]. The description was based on the story of 
Byzantine generals, who organize an attack on a certain city. The task of generals is to reach a common solution given 
that it is known in advance that among generals there are several traitors who can behave arbitrarily (in order to 
disrupt the operation of a system). In addition, it must be considered that traitors have an ability to influence the 
transfer of messages between honest generals for whom it is important to either simultaneously attack the city, or 
simultaneously retreat. If a part of the honest generals retreats, then the enemy will defeat the army by parts. 
Therefore, all generals have to come to an agreed decision. The statement of the above-described problem was published 
in one scientific paper. After it, the name of the algorithm for achieving consensus was published, BFT [111]. Nowadays 
such a problem can be relevant not only for generals, but, say, for a football team as well (Fig. 5.8).

![Figure 5.8 - Reaching consensus using BFT](/resources/img/volume-1/5.2-difference-in-consensus-approaches/5.8-bft.png)

> **Working conditions for a BFT-class consensus reaching mechanism**
>> * *Nodes which interact are equal in rights* 
>> * *Some of the participants may be adversaries who act in a coordinated manner* 
>> * *It is unknown which validators are adversaries* 
>> * *Honest participants are more than 2/3 of the total number* 
>> * *Network may experience interruptions and delays* 

We also note that the number of participants is known in advance and they know each other. In the solution to a problem, 
there are two options: to attack or to retreat. It is assumed that honest nodes comprise more than 2/3 of all 
participants.

However, while implementing the BFT idea, there is a restriction on the number of steps for which all honest validators 
must come to a common solution. Another important fact is that the participants of a BFT protocol are guaranteed to come 
to a common solution, which cannot be changed or canceled afterward. For non-BFT protocols (in the strict definition), 
the probability that the solution will be canceled decreases exponentially, but it is never zero.

### FBA

The Federated Byzantine Agreement (FBA) was first introduced in Ripple and later finalized in Stellar [83]. It allows 
reaching agreement within a large (potentially unknown) number of participants (about a few thousand) each of which does 
not know any other participant personally.

Each participant trusts only a limited circle of other participants (3–10) and therefore achieves consensus in this 
narrow circle. But due to the fact that social ties overlap, the agreement can be reached in the entire system 
(Fig. 5.9).

![Figure 5.9 - Reaching consensus using FBA](/resources/img/volume-1/5.2-difference-in-consensus-approaches/5.9-fba.png)

There are very few cases of real implementations or analogs of such a method in the world. For example, revolutionary 
ideas spread in a similar way—when people share news and infect others with ideas. A final decision in such a system 
requires more time as compared to the systems where each validator knows each other one.

### Protocols for reaching consensus based on DAG

Directed acyclic graph (DAG) is not a method for reaching consensus, yet with this data organization method, the 
existing mechanisms can be improved. It is primarily used in systems requiring asynchronous participation of a large 
number of validators.

This idea has been first proposed to be applied in two protocols: SPECTRE [84] and PHANTOM [85].

The point is that a validator includes to the header of his block links to the blocks which he considers correct and 
which have not been linked to by other validators (according to the current system state, observed by him). Then the 
validator submits his block to the network. This creates a structure which includes all blocks that validators see at 
the current time. Therefore, all blocks that would otherwise become orphan blocks will now be included in the graph 
(Fig. 5.10). This means that honest validators' votes will not be wasted. Potentially, it is one of the most efficient 
methods for organizing the interaction of a large number of simultaneously working validators. This positively impacts 
the reliability of accounting and the level of system protection against double-spending attacks and similar attacks.

![Figure 5.10 - Structure of DAG](/resources/img/volume-1/5.2-difference-in-consensus-approaches/5.10-dag.png)

There is an issue with current implementations of systems using DAG that lies in the centralization of the process of 
resolving conflicts (double-spending transactions). This process is performed by *witness nodes* in ByteBall and the 
coordinator in IOTA. Anyway, DAG is a relatively new approach with the potential to be applied widely in permissioned 
systems.

### Basic criteria for the classification of mechanisms for reaching consensus
Having considered the specific features of each of the mechanisms for reaching consensus, it is possible to single out 
the criteria by which they can be classified:
* Need to trust validators 
* Anonymity of validators 
* Requirement for validators to have a reputation 
* Right to become a validator 
* Capacity 
* Full transaction confirmation time

Note the table below (Table 5.2). It shows that there are mechanisms where trust to validators is not required. This is 
facilitated by a number of certain circumstances such as the system’s rules that motivate validators to be honest and 
also by the interchangeability of these validators—when it does not matter who will participate in the confirmation.

![Table 5.2 - Structure of DAG](/resources/img/volume-1/5.2-difference-in-consensus-approaches/table-5.2-comparison.png)

Another important criterion is the right to become a validator. This process can both occur on a public basis or either 
require the approval of a group of existing validators.

In practice, consensus reaching algorithms can provide different capacity. Theoretically, if necessary, the capacity can 
be increased for any of the mechanisms (e. g., by increasing the computational resources of node-validators or by 
increasing the block size). The side effect is that this can significantly reduce the degree of decentralization of the 
system. In some cases, such modification is rather a disadvantage.

It should be understood that for PoW and PoS, it is basically very difficult to reduce the transaction confirmation 
time. Meanwhile, the DPoS and PBFT algorithms are designed in such a way that a sufficient number of validators are able 
to reach agreement in a short time.

### Summary

The appropriate mechanism for reaching consensus is critical for the reliable operation of a decentralized accounting 
system. When choosing an accounting system depending on the needs and tasks of a particular business case, a specific 
mechanism is considered. Based on it, a system possesses its particular properties.

In their diversity, mechanisms for reaching consensus are aimed at finding the suitable trade-off between the required 
level of trust, the time needed to make a decision, the degree of irreversibility of the made decisions, and the system 
capacity. The decision to select a particular consensus algorithm should be based on results of a thorough analysis of 
the security requirements of a system.

**Common myths**

*Bitcoin source code can be optimized for any system.*

Apparently, this is a widespread misconception that you can take the source code of Bitcoin or Ethereum and freely apply 
it to any new project. In practice, there are examples when developers aim to design an asset-backed digital currency 
and make a Bitcoin fork (where mining is initially provided). In this situation, however, it is not clear who will act 
as a validator; moreover, the presence of mining as such is not always necessary in this type of system. 

*A blockchain-based centralized accounting system makes no sense.*

Let's focus on an example. People call Ripple (XRP) a centralized digital currency. In fact, they are right when it 
comes to coin issuance and transaction validation, but they are wrong regarding audit and ledger management. Anyone can 
launch a Ripple network node and verify the transactions, but only designated parties can create blocks and issue new 
XRP coins.

Also, it is a mistake to treat an accounting system which is decentralized but with limited access as a centralized one. 
There is, in fact, little sense in reaching consensus when all validators are under the control of one entity. But even 
in the case of a bank with many branches, suitable BFT consensus can prevent fraud or hacker attack on a single point of 
failure (which is an issue in many banks). Also, correct usage of the blockchain features can help a system to provide 
its users with the following basic security services: integrity, non-repudiation, and accessibility.

*PoW means consuming energy uselessly.*
The only appropriate way to satisfy the requirements for the Bitcoin environment (we have mentioned them in 2.3 and 4.2) 
is, in fact, PoW. This mechanism makes network spamming unprofitable as well as the attempts to confirm invalid 
transactions. All because miners-entrepreneurs pay a high price for an opportunity to participate in this process and 
are at great risk of not getting a reward if they try to confirm transactions that are not compliant with the protocol 
rules. This is not useless energy consuming, but rather a price we pay to see Bitcoin as it is, an independent 
censorship-resistant payment system.

**Frequently asked questions**

*– Is a hybrid proof-of-work and proof-of-stake possible?*

Surely, it is. Examples of systems that work in such a way are Peercoin and Decred.

*– Is it possible to organize the two-level consensus to improve the characteristics of an accounting system?*

Yes, this is essentially the DPoS case. Such an approach is already used in some systems; one example is BitShares, 
which unlike Bitcoin has a different security model. The idea is that participants, on the basis of the PoS principle, 
select other participants for the validator post; those who are chosen subsequently create blocks. Therefore, the 
frequency of new blocks creation in BitShares is hundreds of times higher. 

*– In the PoS case, can a user with the highest balance collect a vast majority of coins and break down the consensus?*

First of all, a user does not collect coins on his own. They are collected by all users proportionally to their 
balances. Secondly, everyone has a need to spend coins. Thirdly, it is not profitable for validators to violate the 
process of reaching consensus because all their coin savings are stored in this system.

The case when a user has no coins at all but participates in mining blocks is worth considering separately. Does he have 
any chance? Typically, if a system uses PoS consensus reaching mechanism, its initial issuance is centralized. Here, the 
user will have to purchase coins. Another approach for building systems implies using a hybrid scheme (PoW and PoS), 
with which every user can initially mine coins and afterward switch to PoS consensus reaching algorithm.

## 5.3 Limitations of blockchain technology and its application challenges

From the perspective of implementation into existing regulated infrastructure, blockchain and smart contracts are, for a 
reason, perceived as promising technologies. Though, at the moment things are slightly more complicated than it may seem 
at first glance.

To begin with, we are talking about a regulated environment, where responsibility for risks is managed by determined 
parties—this is generally the way things work today. The transition to an era of digital interaction requires applying 
the most reliable and robust technologies for data authenticity. The information stored in digital registries should be 
protected from unauthorized changes.

Blockchain technology allows for the creation of such systems. Moreover, it does not necessarily require participants to 
trust each other or a third party. However, to incorporate such systems into everyday life, there is a number of complex 
tasks that have to be solved.

> **Tasks that complement the introduction of blockchain-based accounting systems**
>> * *Implementation of a digital identity* 
>> * *Digitization of all processes* 
>> * *Adoption of common rules for data processing* 
>> * *Keeping the record of all assets in one system* 
>> * *Organizing decentralized decision-making*

### Implementation of a digital identity

A *digital identity* is required because it allows associating users' actions with their identities.

Within one organization (centralized environment), implementation of a digital identity is not so much a complex task. 
But when it comes to a consortium of companies or a state, it becomes very complicated. It happens due to one 
fundamental issue: the relationship between the identities of users and actions they perform in a digital world must be 
legally binding. This is because you need to prevent disagreements about the history of certain users’ interaction.

Estonia is intensively progressing in this area. The government has been actively introducing digital passports for 
several years—it already enables citizens to vote using their digital passports.

To conclude, digital identity is a key element of interaction in the digital world.

### Digitalization of all processes

This requirement is no less important than digital identity implementation. Any innovations usually imply a gradual 
transition and some stageness. However, when blockchain is used, it is obvious that nothing will work until all internal 
accounting processes of a company are digitalized. Imagine a situation when an organization needs to check the presence 
of a particular client in some external database: currently, it is often performed through a telephone call. It would be 
optimal to implement an automatic request within a timeframe that corresponds to the expected time of decision-making 
(Fig. 5.11).

![Figure 5.11 - Digitalization of processes](/resources/img/volume-1/5.3-limitations-of-blockchain-technology-and-its-application-challenges/5.11-digitalization-of-process.png)

### Adoption of common rules for data processing

First, common rules should be established for resolving issues such as those related to confirmation of transactions, 
system updating, determining responsibility in the events of unforeseen consequences, etc. All these rules must be 
approved in advance.

Then, a uniform legal framework should be created. The updated approach should consider the existence of decentralized 
systems with all their features, including distributed responsibility and decision-making. Cryptocurrencies are a good 
example that shows how complex this task essentially is. Many regulators have already started moving towards updating 
their legislation so that it takes into account the turnover of cryptocurrencies in the world economy. However, the 
community has more questions than answers so far.

### Keeping the record of all assets in one system

In a way, implementation of blockchain technology implies that ownership of all assets is processed in the same 
accounting system. In fact, this is what makes its use so effective.

The point is that a blockchain-based system (where rules of operation are well-defined) can guarantee correctness and 
safety of operation. However, this is only possible if the system itself is self-sufficient.

Here, the concept of self-sufficiency can be illustrated with the example of Bitcoin. It does not use any external 
information resources to validate transactions. This is because all the information about addresses and UTXOs is stored 
in its internal database since the very beginning of Bitcoin. The data obtained from it can be considered sufficiently 
reliable; mathematics and cryptography do not require trust—they work consistently in either case and thus a user can be 
sure that she is working with a transparent and correct system.

One example is the smart contracts platform, Ethereum, which allows you to trade any tokenized assets which are issued 
in it. If, for example, we take any separate accounting system (e.g., land titles accounting) which is recognized by the 
government, then there is a challenge of synchronizing the data between the two systems. At present day, the only way to 
do this is using trusted third parties—oracles (for further details, see 6.3).

### Organizing decentralized decision-making

The fifth aspect is decentralized decision-making, namely the *agreement* of all parties as to how decisions will be 
made. In practice, this agreement is very difficult to achieve. People usually find it hard to reach consensus when it 
comes to their personal benefits that depend on the taken decision. This can be due to anything from political ambitions 
to personal interests. Within one enterprise, it is yet possible to make an agreed decision, but as soon as competition 
and different points of view appear, it starts resembling a political wrestling match. This is probably the main reason 
why there are so few working implementations of decentralized systems in the enterprise world.

> **Factors hampering application of blockchain technology**
>> * *High redundancy*
>> * *Node synchronization delays*
>> * *Governance issue* 
>> * *Responsibility issue*

These limitations are fundamental since they stem from the very decentralized approach itself. Let's examine them in 
more detail.

### Capacity of accounting systems

The *capacity* of an accounting system is usually determined as the number of confirmed transactions or the total amount 
of data in these transactions per unit of time. Accordingly, the capacity is measured in tps (transactions per second) 
or B/s (bytes per second). Problems related to performance limitations and scalability of decentralized systems are 
interrelated, and the connection is very close. Yet, these are abstract quantities that cannot be measured, and 
therefore there is no point in using them. Instead, there are more accurate indicators: capacity, the time required for 
transaction confirmation, and the maximum number of validators that can participate in reaching a consensus at the same 
time.

For an easier understanding of capacity, consider Fig. 5.12. The idea is that two different delivery services for one 
period of time can perform work on moving different quantities of cargo. Their transmission capacity will be affected by 
the type of transport selected.

![Figure 5.12 - Low and high capacities exemplified](/resources/img/volume-1/5.3-limitations-of-blockchain-technology-and-its-application-challenges/5.12-low-and-high-capacities.png)

The capacity of a system is influenced by the consensus mechanism and the maximum number of validators working at the 
same time. In general, a decentralized system has lower capacity than a system with a single control center since making 
changes in such case takes more time. It is necessary to distribute data among all participants, and they must reach 
consensus regarding the information: the decision is no longer taken in one place but instead in several. These delays 
lead to an increase in response time of the system.

### Transaction confirmation time 

In order to understand the limitation of confirmation time, you can follow an example in Figure 5.13. A task of the same 
volume can either be solved in a short or long time, depending on the chosen solution approach. In the case of 
delivering cargo, the type of transport will influence duration. Whereas in the case of a decentralized accounting 
system, the time of transaction confirmation is affected by the rules for selecting validators and the mechanism for 
reaching consensus.

![Figure 5.13 - Transaction confirmation time limitation exemplified](/resources/img/volume-1/5.3-limitations-of-blockchain-technology-and-its-application-challenges/5.13-transaction-cong-time.png)

The problem of storing full amount of data also imposes additional limitations. Generally, every participant must have a 
copy of the system database, which constantly updates through synchronizing with other nodes of the network (Fig. 5.14).

![Figure 5.14 - Issue of redundant data storing](/resources/img/volume-1/5.3-limitations-of-blockchain-technology-and-its-application-challenges/5.14-redundancy.png)

During February–August 2018, depending on the conditions in the Bitcoin network (the flow of new transactions, the hash 
rate, the difficulty parameter, etc.), the average time of block confirmation ranged from 6 to 18 minutes, while the 
highest delays were almost up to 30 minutes.

### Governance

Software development within one company is a process which features responsible parties and a clear plan and is managed 
in a centralized way. Any suggestions for making changes are approved by a narrow circle of persons involved in the 
development of rules of the system.

For a properly applied decentralized accounting system, democratization of governance is beneficial. The larger the open 
community is, which argues and proposes solutions, the more opportunities there are for valuable improvements. If the 
majority votes for a particular improvement, it will be added to the next update. Thus, everyone can offer his updates 
and openly discuss its advantages and disadvantages (Fig. 5.15).

![Figure 5.15 - Protocol updating in a decentralized system](/resources/img/volume-1/5.3-limitations-of-blockchain-technology-and-its-application-challenges/5.15-protocol-update.png)

The problem, however, is that joint management is always more difficult than personal one; it is difficult for people to 
reach consensus fast. The decision in such a system is always made slower since participants need time to negotiate.

### Distributed responsibility

A huge issue on the way to practical implementation of decentralized technologies is the unwillingness of society's 
mindset to accept the consequences of a decentralized approach. It manifests in the fact that we are used to having a 
specific responsible person. This is how it works in a system with centralized management, which has become natural for 
us.

If there are any misunderstandings while working with a centralized organization, we can always identify a responsible 
person and claim to resolve the problem. For example, if you bought a car which, a week later, refused to start, you 
call the company and get a refund. And even if there is no response from this company, you go to the court where a judge 
will solve the dispute by law, namely force the company to return the money and compensate you for the damage. In any 
case, you find the guilty one, who is responsible for the made decision.

Unlike a centralized management center, where responsibility is clearly defined, in a distributed system, it is rather 
blurred. In fact, it would be more appropriate to replace the word responsibility with risk because this is specifically 
what participants take, a risk. Essentially, the only guarantee of system's correct operation are the rules by which it 
operates. Rules are known to all the participants: everyone can check the actions of others and check whether or not 
they meet the protocol rules.

Risks lie either in the behavior of users themselves or in the vulnerabilities which could make it possible to affect 
the rules (e.g., attacks on the protocol). By the way, in the last sense, a decentralized system is much more stable 
than the centralized one. Given that a system is truly decentralized, no one, including the developers, is able to make 
changes on their own. Therefore, there is no such court that could implement a decision which is contrary to the rules 
of the protocol. The only way you can change the state of the system is to have the agreement of a majority of 
participants; that said, this agreement should be achieved without violations of the rules, which are set in advance.

In such a case, there should be a different legislative approach. Society needs new legal frameworks that would 
acknowledge the existence of such systems. One way it could possibly work is that ensuring security and meeting specific 
requirements rests on the creators of this system only before a certain moment. Once this "moment" comes, responsibility 
transfers to users who accepted the rules and possible risks.

### Protocol updating

Another important feature of decentralized systems is the approach to adopting new updates. Any new proposal for the 
improvement of system's rules can result in either a solid acceptance or disagreement among the participants. This 
essentially is the problem of governance in a decentralized accounting system.

If these disagreements are rather serious, then the case could end up with a fork, which is a splitting of the chain 
that provokes the appearance of another system which is based on the old one but with different rules.

This happened to Bitcoin in 2017 when its fork, Bcash, appeared. It was suggested to increase the block size in the 
Bitcoin network from 1 MB to 8 MB in order to increase the capacity. Participants who supported it had updated their 
software and started working according to the new rules. Сonsequently, there are currently two systems: Bitcoin, where 
participants operate under the old rules (with a 1 MB block), and Bcash with different rules, where the block is 8 MB 
(for further details, see 6.1).

Another example is the incident with Ethereum. In 2016, cyber-criminals stole about $60 million, which had been invested 
in the development of DAO, the project developed on the Ethereum platform [86]. The development team decided to 
interfere and performed the update in which it rolled back the system and revoked the smart contracts involved in the 
theft due to the presence of the vulnerability. As a result, all the funds were returned to the balance of the project, 
yet such a course was not appreciated by everyone. The opponents of this update and the fork caused by it continued 
operating in the old system and named it Ethereum Classic, while the supporters of the rollback began operating in the 
new one, Ethereum.

### Conclusion

Blockchain technology, if correctly implemented, can allow for a transparent business model with the possibility of a 
transparent audit. This opens up new prospects for business development of partnership companies due to increased trust 
among users. However, on the way to implementation of this idea, there is a number of difficulties that we have to first 
consider and then overcome: designing, configuring, and maintaining a decentralized accounting system is usually a much 
more complex task than for the centralized alternatives.

For the application of blockchain in the real economy, it is necessary to create conditions for practical 
implementation. The plan would be to digitize the majority of processes of interaction in the society, define digital 
identification standards, and develop a legal framework to resolve responsibility issues.

[DEVELOPMENT OF DECENTRALIZED TECHNOLOGIES](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-1/en/6-development-of-decentralized-technologies.md)