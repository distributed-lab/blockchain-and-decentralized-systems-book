# [2 SCHNORR SIGNATURES AND THEIR APPLICATION](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/2-Schnorr-signatures-and-their-application.md)

# 3 APPLYING CONCEPTS OF SHARDING, OFF-CHAIN, AND DAG

One of the most complex tasks in building an accounting system is pre-programming the ability to scale it. By scalability, we mean the ability of a system to process larger amounts of data (requests, transactions, account states) while increasing the number of its servers (auditor nodes, validators). In the context of a blockchain-based accounting system, its scalability can be measured using three metrics: throughput, block confirmation time, and requirements for node hardware and software.

Depending on the system architecture, increasing the system load and the number of validators can lead to a number of issues, including but not limited to:

> * _The inability of the system to process the entire transaction flow_
> * _More memory required to store history_
> * _Longer transaction confirmation time_
> * _Higher fees_

The first problem is related to system throughput. The throughput of any accounting system is limited (do not believe it if the system owner claims the opposite – every system has a threshold it cannot overcome). In the case of a decentralized accounting system, throughput depends on the time required to reach consensus between the validators, and the number of actions (such as transactions) on which consensus has to be reached.

Different systems have different maximum capacities: for Bitcoin, it is 3–7 transactions per second, EOS and Bitshares can process up to several thousand transactions per second. However, if the load exceeds the maximum throughput, then some transactions simply will not be confirmed by the system.

Blockchain-based accounting systems need to store the entire transaction history to ensure the integrity of the ledger’s final state. Obviously, the transaction history can only increase in size. So, the more transactions per time unit the system is able to process, the faster the database size increases. This also leads to increasing the hardware requirements for system maintenance. This means that the number of nodes able to store a complete transaction history decreases over time, which in turn affects the decentralization level and fault tolerance of the system in general.

The decentralization level of the system depends on the number of validators. When PoW and PoS are used as consensus algorithms, the increased number of validators does not affect the time needed to reach consensus - but at the same time, due to the specifics of the protocols, the immutability of history is not ensured. However, when BFT-based algorithms that imply the irrevocability of the system’s final state are used the increased number of validators affects the time needed to reach consensus, thus influencing the system capacity.


## 3.1 Using off-chain protocols

The development of off-chain protocols for accounting system scalability started with payment channels and the Lightning Network [62]. As previously discussed, off-chain protocols are second-level protocols that work on top of a specific accounting system or multiple systems. In fact, such solutions do not imply changes to the underlying protocol of the accounting system but allow moving user interaction beyond the boundaries of the main chain of blocks while preserving the properties of the main protocol.

The main idea of off-chain protocols is that users lock coins in the main chain and exchange transactions that can change the output distribution of coins. Thus we can say that the approach to consensus changes: after funds in the channel are locked, there is no longer global consensus regarding updating the state of the entire accounting system, but a consensus directly between the channel participants (Fig. 3.1). However, the math underlying the off-chain interactions does not allow counterparties to cheat.

![Figure 3.1 – The process of reaching consensus in the accounting system and in payment channels](/resources/img/volume-3/3.1-Using-off-chain-protocols/F-3.1-reaching-consensus-in-accounting-system-and-in-payment-channels.png "Figure 3.1 – The process of reaching consensus in the accounting system and in payment channels")

Protocols that implement similar concepts include:

> * _Lightning Network_
> * _Plasma_
> * _Celer Network_
> * _Raiden Network_
> * _Trinity_
> * _Interledger_
> * _GEO Protocol_

Recall that the Lightning Network is a protocol that already runs on top of Bitcoin, and can operate on top of similar accounting systems. The Lightning Network uses the bidirectional channel switching mechanism and allows creating a trustless infrastructure for fast transfers within these channels since transaction processing time depends only on processing and transferring the data between participants. We examined the principles and properties of LN in Volume 2 of the book.

The main idea of the Plasma protocol is to use sidechains to increase the bandwidth of the Ethereum network [63]. Plasma uses a structure consisting of many chains of blocks that are united in trees. The state of each child chain is fixed in its parent’s state, thus going up to the main chain of blocks (Fig. 3.2). Each child chain can have its own consensus mechanism and methods of communication with the parent chain.

![Figure 3.2 - Plasma protocol architecture](/resources/img/volume-3/3.1-Using-off-chain-protocols/F-3.2-Plasma-protocol-architecture.png "Figure 3.2 - Plasma protocol architecture")

> _Note. Another example of off-chain protocols is the GEO protocol_ [64]_. GEO is a decentralized peer-to-peer network that allows its members to account for assets among themselves, including the exchange of assets from various accounting systems. The protocol uses only payment channels and trust channels between nodes (the weight of trust to each other is determined independently). In the GEO protocol, the consensus is reached only between the parties involved in a particular transaction. At the same time, they have no information about the rest of the network status. There is also no global ledger for assets that are represented in the system and there is no single source of information about its last state._


## 3.2 Sharding concept

Initially, the concept of _sharding_, or _segmenting_, was applied to databases [65]. This process involved dividing a large database into disjointed _shards_, or _segments_, and providing faster access to each particular shard with the ability to easily manage it. An important feature is that datasets in shards do not overlap, i.e. there are no duplications in shards.

Sharding is usually divided into two types: vertical and horizontal. The idea of _vertical sharding_ is to extract certain tables from the database and transfer them to other servers (Fig. 3.3) [66]. In this case, queries to different tables will be processed by different database servers, which can increase the capacity of the whole system. At the same time, it is very important to provide redundancy (replication) of individual shards to increase the fault tolerance of the system, since limited access to one of the shards can critically affect operations with others.

![Figure 3.3 - The process of vertical sharding](/resources/img/volume-3/3.2-Sharding-concept/F-3.3-vertical-sharding.png "Figure 3.3 - The process of vertical sharding")

_Horizontal sharding_ implies splitting a single table into components based on common, predefined features (Fig. 3.4) [67]. In this case, the data is divided according to the following principle:

> * _Each shard has the same structure as the original table_
> * _Separation is performed based on specific attributes of elements_
> * _When someone accesses the system, it determines which shard should be addressed_

![Figure 3.4 - The process of horizontal sharding](/resources/img/volume-3/3.2-Sharding-concept/F-3.4-horizontal-sharding.png "Figure 3.4 - The process of horizontal sharding")

Another advantage of horizontal sharding is that the processing of different data segments can be distributed among individual servers or server groups which leads to increased system throughput. However, along with the advantages that this approach provides in its classic form it has several limitations [66]:

> * _The sampling of elements, as well as searching and filtering them is more complicated_
> * _Rebalancing (redistributing) data in tables is required_
> * _Increased shard replication requirements_

The first limitation means that it is more difficult to compile a sample of elements from the whole database. For example, the task to access several latest records is way more complicated since records are added to different shards, each with its own records. It also complicates the procedures for searching and filtering the data by shards (unless the features by which these shards were divided are taken into account).

> _Note. Imagine a table is divided into 5 shards. We want to make a selection of the last 100 elements that were added to the general database. In this case, it is very simple to determine the last 100 elements for each shard separately, but since shards keep their own records, it is quite difficult to select 100 common elements since an element-by-element comparison must be performed. Moreover, since the elements are distributed unevenly across shards, we cannot select the last 20 from each shard to obtain the described selection._

The second important point to consider is data rebalancing when new shards are added. Sharding rules must be set before the splitting process is commenced. If the sharding architecture is not thought through, it can lead to data loss or damage the table contents. In the best-case scenario, a situation may arise when it will be necessary to analyze all the available shards to add a new one.

Also, with an increased number of shards, the probability that each of them fails increases. 

If the shards are located on different servers/nodes, then each shard keeper must provide and ensure replication (redundancy) of the relevant shard.


### Sharding in blockchain-based systems

In blockchain-based systems, sharding means dividing the system into smaller components, each of which is engaged in processing a separate (i.e. non-intersecting with the rest) set of transactions and stores history related to its shard (transactions and account states).

Each segment consists of a relatively small subset of validators that reach consensus regarding their shard. Due to the fact that the number of shards increases in proportion to the increased load on the accounting system, this allows to cope with the transaction flow and maintain the confirmation time within acceptable limits.

Thus, instead of one single chain of blocks, the system is divided into several chains, each of them being a shard (Fig. 3.5) [68].

![Figure 3.5 – Dividing an accounting system into shards](/resources/img/volume-3/3.2-Sharding-concept/F-3.5-dividing-into-shards.png "Figure 3.5 – Dividing an accounting system into shards")

Each shard, in turn, has its own set of validators that reach consensus on updating its state. However, this leads to a number of tasks that must be solved when using sharding in an accounting system:

> * _Providing a high level of security for individual shards_
> * _Distribution of validators between shards_
> * _Ensuring data availability_
> * _Confirming transactions related to several shards_

The first limitation is associated with a lower level of shard security compared to a single chain with all validators working on it. For example, if we imagine a scenario where consensus in the network is achieved using PoW, then splitting the system into shards will lead to a proportional decrease in computing power for individual shards (Fig. 3.6). As a result, the 51% attack is much easier to perform [69].

![Figure 3.6 – Reduced security of an individual shard compared to a consolidated chain](/resources/img/volume-3/3.2-Sharding-concept/F-3.6-reduced-security-of-individual-shard.png "Figure 3.6 – Reduced security of an individual shard compared to a consolidated chain")

The second task is related to solving the following problem: who will work on specific shards? In the previous paragraph, we mentioned a 51% attack assuming that the validators’ power distributes evenly between the shards. If validators choose themselves which shard they are to maintain, this can make the situation worse and lead to even more attacks. Accordingly, the system must have a mechanism for distributing validators among shards on which they will reach consensus.

The third task is related to data access between shards. Validator nodes of an individual shard create their own chain of blocks which contains transactions that affect the local state of the shard. However, there are a number of cases where in order to make a decision validators need to obtain information from another shard.

If a payment transaction only affects accounts that are inside the same shard, everything is fine and the transaction is processed by the current shard’s validators. However, it is necessary to ensure compatibility when the transaction is related to accounts which have their data stored in different shards. If shards cannot “communicate” with each other, they are no different from two independent accounting systems each of which keeps its own history of events.


### TON architecture

One of the most prominent examples of platforms that use sharding technology is TON (Telegram Open Network) [70]. The technology implies the possibility of dividing a single system into 2<sup>92</sup> subsystems that function independently but still can communicate with each other. In this section, we review the fundamental construction components of the TON network and discuss if sharding can actually solve the scalability problems that most modern accounting systems are prone to.

Telegram Open Network contains a significant number of components:

> * _Accounting system_
> * _Distributed file storage_
> * _Network users anonymization instruments_
> * _DHT_ 
> * _DNS_
> * _A set of tools for the functioning of payment channels and communication with other accounting systems_

The main component (the core) of TON is the accounting system. The remaining components are auxiliary and allow users to expand the platform’s functionality.


### Structure of shared database in TON

Unlike “traditional” blockchains utilized by most existing systems TON provides a multi-level architecture. For this purpose, several TON-specific blockchain types are introduced d:

> * _Masterchain_
> * _Workchain_
> * _Shardchain_

_Masterchain_ is a unified root chain that is used to store the state of all child chains. Masterchain stores information about the current version and parameters of the TON protocol, the current set of validators and the size of their stake, the set of currently active workchains and shardchains, as well as hash values ​​of the actual blocks of these chains. To achieve consensus, the masterchain uses a PoS-based algorithm [70].

The masterchain is divided into _workchains_ – subsidiary systems, the maximum number of which can be 2<sup>32</sup> (Fig. 3.7). Each workchain is a subsystem that has its own accounting protocol, its own format of accounts and transactions, a virtual machine for executing contracts, internal currency, etc. Each workchain only keeps track of accounts related to it, so it can be represented as a separate accounting system. At the same time, each of these systems has to satisfy a number of requirements to be able to communicate with other workchains and the masterchain (these requirements are discussed in more detail further below).

![Figure 3.7 – Dividing the masterchain into workchains](/resources/img/volume-3/3.2-Sharding-concept/F-3.7-dividing-into-workchains.png "Figure 3.7 – Dividing the masterchain into workchains")

Workchains, in turn, can be divided into _shardchains_. Each shardchain has the same architecture as the parent workchain, but it processes only a specific subset of accounts. The maximum number of shardchains in one workchain can be up to 2<sup>60</sup>.

> _Note. In fact, a workchain is an abstract concept – it is just a collection of shardchains that function in a uniform fashion according to a set of predefined rules. To explain this concept with a simpler example, imagine a table divided into shards. In this case, each individual shard is an independent table, and the global state is a set of such shard tables. The only case in which a workchain can be represented as a single data structure is when that workchain consists of one single shardchain._

Each workchain has its own 32-bit identifier (_workchain_id_). To create a workchain, an appropriate transaction is created, which must be confirmed in the masterchain. By default, from the moment of initialization, TON supports Workchain Zero (_workchain_id_ = 0). This workchain is designed to work with TON contracts, as well as to account and transfer coins of the base currency (Gram).

The shardchain, in turn, is identified using a pair of values: _workchain_id_ – the identifier of the parent workchain and _shard_prefix_ – a value from 1 to 2<sup>60</sup>, which determines the array of accounts that will be processed by this shardchain.


### Splitting and merging shardchains

There are two ways of dividing workchains into shardchains: _static_ and _dynamic_. _Static splitting_ involves splitting a workchain into a predefined number of possible shardchains based on certain features. For example, we want to divide the system into 8 shards. For each of them we specify a subset of accounts that will be processed (Fig. 3.8).

![Figure 3.8 – Static splitting into shardchains](/resources/img/volume-3/3.2-Sharding-concept/F-3.8-static-splitting-into-shardchains.png "Figure 3.8 – Static splitting into shardchains")

This is the simplest sharding method, but not the most effective. The inefficiency of this approach lies in the fact that assumptions regarding the system load need to be made in advance. If a user creates a small number of shardchains, then with a large load on the system not all of them will be able to process transactions related to them. If, on the contrary, too many shardchains are specified, then in each of them transactions will appear very rarely, which will lead to additional expenses for maintaining the system.

Dynamic sharding allows users to, instead of choosing the required number of shards in advance, split/merge shardchains in real time. This feature entails that each individual shard can be automatically divided at any time if its throughput is insufficient (Fig. 3.9). When creating a new workchain, TON assumes the initial creation of one shardchain, which can be divided in the future.

![Figure 3.9 – Principles of dynamic sharding](/resources/img/volume-3/3.2-Sharding-concept/F-3.9-principles-of-dynamic-sharding.png "Figure 3.9 – Principles of dynamic sharding")


### Making decisions regarding the state of the system

We mentioned earlier that each masterchain block contains the up-to-date hash values ​​of the blocks of all shardchains in the system, recorded at the time of their creation. As soon as the hash value of the shardchain block is included in the masterchain block, this block is considered confirmed in the system and subsequent blocks can refer to it.

Each new block in a shardchain also contains a hash of the most recent masterchain block, which confirms all previously created shardchain blocks. Such a connection between masterchain and shardchains is needed so that shardchains can rely on each other’s states (Fig. 3.10). In fact, the masterchain is needed to synchronize all child shardchains (and, accordingly, workchains) and determine the global state of the system.

![Figure 3.10 – Relation between masterchain and shardchains](/resources/img/volume-3/3.2-Sharding-concept/F-3.10-relation-between-masterchain-and-shardchains.png "Figure 3.10 – Relation between masterchain and shardchains")

> _Note. It is assumed that several hundred participants owning the biggest stakes will act as TON platform validators and send the corresponding transactions to the masterchain. All validators are divided into subsets in a pseudo-random manner, where each validator validates blocks within a separate shardchain during a certain time period, and each validator can be included in several subsets. The consensus is achieved using the BFT algorithm. If one of the validators signs an invalid block, it can be fined – completely or partially lose its stake – and excluded from the validator set._

After all new blocks in shards are generated or the time allocated for their generation has passed, a new masterchain block containing the hash value of shardchains’ blocks is generated. Consensus on adding a new block to the masterchain is achieved using the BFT algorithm among all validators.


### Communication between shardchains

A mandatory requirement for an accounting system that supports sharding is the ability for shards to send messages to each other. There are two approaches to constructing this interaction: _asynchronous_ and _atomic_ transactions [68].

Let’s explore the idea of asynchronous transactions between shards using the example of transferring coins between accounts. Imagine that Alice wants to send Bob 100 coins, but their balances are stored in different shards. In this case, in the shard, responsible for sending funds to Bob, validators will confirm the transaction only after they have proof that the corresponding transaction in Alice’s shard that is responsible for subtracting her funds is confirmed. Let us examine the user interaction in this scenario in more detail in Figure 3.11.

![Figure 3.11 - Conducting asynchronous transactions](/resources/img/volume-3/3.2-Sharding-concept/F-3.11-conducting-asynchronous-transactions.png "Figure 3.11 - Conducting asynchronous transactions")

1. Alice sends the transaction to Shard A, which subtracts 100 coins from her account.
2. Shard A validators confirm the transaction. Alice receives the corresponding proof (for example, the authentication path in the Merkle tree for this transaction).
3. Alice sends another transaction to Shard B, which contains the confirmation proof of a previous transaction.
4. Validators check the proof and confirm the transaction in Shard B.

However, this approach is not perfect and cannot be applied in every accounting system. One of the reasons is that the user cannot be sure that after the transaction in one of the shards was confirmed, the corresponding confirmation will be executed in the other shard. Moreover, if shards can fork and merge (according to the protocol rules), then the situation when confirmation is executed only in one of the shards is quite likely to happen.

> _Note. In TON, the general form of the message depends on the recipient shard and recipient account or contract. However, the message must necessarily support common fields that will allow this sending operation between workchains._
> 
> _Each message contains the account IDs of the sender and recipient. In this case, the account identifier is represented by two values: workchain_id (to determine which workchain the account belongs to) and account_id (in the basic case it’s a 256-bit value that identifies the account inside the workchain)._
> 
> _Account IDs may vary across workchains. The only mandatory requirement is that the identifier must not be shorter than 64 bits (necessary for sharding)._
> 
> _Some messages may not have a sender or recipient (at least one of them must be present). Such messages are used to communicate with external systems and applications. A message that does not contain a sender is directly included in the corresponding shardchain (if it’s correct). The fee for adding this message will be charged to the recipient account. A message that does not contain a recipient is added to the shardchain of its sender. However, there is no guarantee that this message was received by the targeted external system._


## 3.3 Design and application of DAG

As blockchain technology was developing, many have tried to improve various aspects of accounting systems based on it, but the main idea remained unchanged – the system always has a single chain of blocks while the blocks are added one by one.

In many ways, systems based on the Directed Acyclic Graph (DAG) concept are very similar to the blockchain-based ones. Transactions are also linked by hash values, there are validators that add and confirm new transactions. However, due to certain distinctive features of this branched data structure, it is necessary to provide a suitable algorithm for achieving consensus. To do this, different mechanisms, such as _weights_ and _graph traversals_, are used, and consensus algorithms in such systems are based on these mechanisms.

One of the first attempts to introduce DAG in an accounting system was IOTA – a system designed to meet the IoT industry needs. Specifically for IoT, it is extremely important to ensure high scalability, low resource consumption, and liveness even in conditions of unstable connection without sacrificing system security. But have developers of IOTA and other accounting systems delivered on the benefits they sought to achieve?


### Origin of graph as a concept

The concept of graphs appeared a long time ago. Mathematician Leonard Euler introduced it back in the 18th century while working on the famous Königsberg bridges problem [71–72]. The idea was to cross the seven bridges in the city and return to the starting point. However, each bridge can be crossed only once. As it turned out, this is impossible. Let’s see why and how the graphs helped Euler to solve the problem.

> _Note. If we picture Königsberg bridges as edges and starting places in the city as vertices, we will get the following graph (Fig. 3.12). As a result of his thinking, Euler came up with the reason why it is impossible to traverse all bridges without crossing them twice: any graph with more than two vertices, to which an odd number of edges lead, cannot be drawn with one stroke. In this case, we have an odd number of edges leading to all of the vertices; thus, it is impossible to traverse them._

![Figure 3.12 – The Königsberg bridges scheme](/resources/img/volume-3/3.3-Design-and-application-of-DAG/F-3.12-Königsberg-bridges-scheme.png "Figure 3.12 – The Königsberg bridges scheme")

Since then, graph theory has come a long way and is now used in various fields, from economics to computer science. For example, in computer science, graphs are the foundation of many tree data structures. In physics and circuit engineering, graphs are used to depict circuit diagrams. In economics, graphs are used to make locally optimal solutions at each stage of the problem solving process. Graphs are also applied in data routing and navigation.


### What is a DAG?

_A directed acyclic graph (DAG) is a directed graph with no possible closed walk._ This definition may sound rather confusing, so let’s first recall some basic definitions of graph theory.

First of all, a _graph_ is an abstract structure consisting of many _vertices_ and _edges_ that connect these vertices (Fig. 3.13). The vertices have an arbitrary structure and can represent various objects (cities, automaton states, network switches, etc.).

![Figure 3.13 – An undirected graph](/resources/img/volume-3/3.3-Design-and-application-of-DAG/F-3.13-undirected-graph.png "Figure 3.13 – An undirected graph")

Graphs are divided into the following categories:

* Undirected graph – a graph in which edges link two vertices symmetrically (Fig. 3.13)
* Directed (oriented) graph – a graph with asymmetrically linked vertices

A directed graph may have a _cycle_ (a _closed walk_) – a walk along the sequence of edges that returns to the original vertex. A graph is called _acyclic_ when there are no closed walks. For a better understanding, we depict the directed cyclic graph in Fig. 3.14 and the directed acyclic graph in Fig. 3.15.

![Figure 3.14 – An example of a directed cyclic graph](/resources/img/volume-3/3.3-Design-and-application-of-DAG/F-3.14-directed-cyclic-graph.png "Figure 3.14 – An example of a directed cyclic graph")

![Figure 3.15 – An example of a directed acyclic graph](/resources/img/volume-3/3.3-Design-and-application-of-DAG/F-3.15-directed-acyclic-graph.png "Figure 3.15 – An example of a directed acyclic graph")


### DAG-based decentralized accounting system architecture

DAG-based accounting systems, unlike blockchain-based ones, can confirm transactions concurrently, in several independent flows. Objects in DAG-based accounting systems are linked using hash values, i.e., new transactions refer to those that are already included in the structure.

Each new object has to refer to several previous ones. This allows the graph to remain sufficiently connected while getting bigger.

To describe the relations between transactions, the concepts of direct and indirect confirmation are introduced. If you represent transactions as vertices and links as edges, one transaction will directly confirm the other if there is an edge between them. An indirect confirmation exists when two transactions do not have an edge between them, but there is a path that leads from one transaction to another. A clear example is shown in Fig. 3.16.

![Figure 3.16 – Relation of transactions in a graph](/resources/img/volume-3/3.3-Design-and-application-of-DAG/F-3.16-relation-of-transactions-in-graph.png "Figure 3.16 – Relation of transactions in a graph")

As we can see from the figure, transaction A _directly_ confirms transactions B and C, and also _indirectly_ confirms transactions D and E. The genesis transaction (G) is confirmed by _all_ subsequent ones either directly or indirectly.

A transaction that has only been added to the graph and has no confirmations is called a _tip_. In our case, transactions A and I are tips. After a transaction is added, information about it is relayed over the network.

When a new transaction is received, the validator checks its structure and other parameters that depend on the particular accounting system, for example, if there is proof-of-work and whether all signatures are valid [73–74].


### Reaching consensus and resolving disagreements

When adding a new transaction to the graph, any previous transactions can be referenced, but does it make sense? Most certainly not, since it does not bring any benefit to the system. The most logical way is to confirm and reference the tip transactions. However, the number of tips at a given point in time is usually much higher than 1, so there must be a way to select which tip to reference.

The most obvious option would be to randomly traverse the graph in search of unconfirmed transactions. However, in this case, there is no way to prevent tips that reference transactions that have been confirmed a long time ago, since it is impossible to say exactly when a particular transaction occurred which forces validators to select only the most recent ones. The solution to this problem is to use the _cumulative weight_ and _weighted random walk_ mechanisms.

The _cumulative weight_ of a particular transaction is the number of transactions that directly or indirectly confirm it, plus the transaction itself.

The _weighted random walk_ mechanism ensures that the walker will most likely go to the transaction with the highest cumulative weight.

The main idea of ​​this approach is that honest validators will not confirm incorrect transactions since their weight will be lower and they will be discarded eventually. This approach not only forces validators to select transactions that are likely to be correct, but also prevents situations where the validator decides to refer to a transaction that has already been confirmed a long time ago, thus contributing nothing to the system.

Let’s look at an example of how these mechanisms work (Fig. 3.17).

![Figure 3.17 – An example scenario when a transaction refers to already confirmed transaction](/resources/img/volume-3/3.3-Design-and-application-of-DAG/F-3.17-transaction-referring-to-already-confirmed-transaction.png "Figure 3.17 – An example scenario when a transaction refers to already confirmed transaction")

The validator who created transaction 16 decided to reference transaction 7 that was already confirmed instead of confirming and referencing transactions 15 or 8. When the next validator starts to traverse the graph and reaches transaction 7, the probability that he will go to 16 is extremely low because its total weight is 1. Instead, he will go to 9 and continue his path.

This approach provides protection against invalid transactions. Imagine that transaction 16 is not correct and even if one of the honest validators goes to it (after all, the walk mechanism has some degree of randomness), it will not confirm and reference it.

However, if a transaction was added into the graph and someone referenced it, this does not automatically make it confirmed. To prevent double-spending attacks the _confirmation confidence_ mechanism is used. Confirmation confidence is a value that is calculated for each specific transaction and indicates the degree of transaction acceptance by the network participants. The confirmation confidence value varies from 0 to 100% and is calculated as follows: the graph traversal algorithm is executed a certain number of times, then the number of transactions that reference the current one is counted and used to calculate the confirmation confidence.

The probability that transactions with a high value of confirmation confidence (more than 95%) will be discarded is extremely low. The only way to perform a double-spending attack in this situation is to reference two old transactions and then start generating a large number of new ones, increasing the total weight of the new branch. However, this scenario is possible only if the adversary is able to generate more transactions than all other system participants combined.

> _Note. In some systems (IOTA, for example) a more centralized approach is used. It involves publishing special transactions that record the state of the system by a designated participant._

Having figured out the basics of the architecture and functioning of accounting systems based on directed acyclic graphs, let’s look at a few examples.


### IOTA

IOTA is an asynchronous permissionless accounting system designed to meet the needs of the IoT industry, which requires scalability, low resource consumption, no fees, stable operation during communication interruptions and a high level of security. IOTA is based on Tangle, as IOTA developers call their distributed DAG ledger [75].


### Transactions in IOTA

Before moving on, let’s take a closer look at the structure and the lifecycle of a transaction, as well as the steps it must go through before being considered valid. The general transaction structure is shown in Fig. 3.18.

![Figure 3.18 – IOTA transaction structure](/resources/img/volume-3/3.3-Design-and-application-of-DAG/F-3.18-IOTA-transaction-structure.png "Figure 3.18 – IOTA transaction structure")

The _hash_ field contains a hash value that covers all transaction fields.

> _Note. IOTA uses the Curl-P-R hashing algorithm. Curl-P-R is a hashing algorithm designed specifically to work with the ternary numeral system. Essentially, Curl-P-R is a modified sponge function (a class of cryptographic algorithms). The number R, in this case, indicates the number of hashing rounds._
> 
> _In 2019, researchers from Boston University, MIT Media Lab and Harvard University presented a publication proving that the Curl-P-27 algorithm is vulnerable to collisions_ [76]_. As the developers themselves said, in real-life conditions it is almost impossible to implement a collision attack because of the protection methods implemented in a centralized transaction confirmation mechanism._
> 
> _Notwithstanding this, currently Curl-P-81 is used instead of Curl-P-27 to calculate the transactions hash and proof-of-work value_ [77]_._

The _signatureMessageFragment_ field contains the signature value (WOTS) or a user-defined message. The signature covers the hash value of the transaction bundle and, depending on the length, belongs to one of three security levels: first (2187 trytes), second (4374 trytes) or third (6561 trytes). At the same time, the size of the _signatureMessageFragment_ field is limited to 2187 trytes, and if this size is exceeded, the signature will need to be divided into several parts.

The _address_ field indicates recipient’s or sender’s address, depending on the type of transaction. Transaction types will be discussed later in this subsection.

The _value_ field sets the number of sent or received coins. If a participant wants to send a specific message over the network without attaching coins to it, he can set this field to be zero.

The _timestamp_ contains the transaction creation time. However, it is not verified and may actually contain an arbitrary value.

The _currentIndex_ and _lastIndex_ fields contain indices of the current and last transaction in the bundle respectively.

The _trunkTransaction_ field contains the hash value of the next transaction in the bundle, or the hash value of another transaction in Tangle (only for the last transaction). We will discuss how this field is formed further below.

The _branchTransaction_ field is used to refer to one of the published transactions in the graph. Later we will describe in detail what bundle is, and also examine how transactions reference each other.

The _bundle_ field contains the hash value needed to determine which bundle this transaction belongs to. To do this, the hash value covers the fields _address_, _value_, _currentIndex_, _lastIndex_, and _timestamp_ for all transactions in the specific bundle [79].

The _attachmentTag_ field allows setting an arbitrary service label for the message. It is mostly used to identify messages when exchanging information between IoT devices.

The _attachmentTimestamp_ field indicates the time the transaction was generated.

The value in the _nonce_ field is incremented after each attempt to generate a valid PoW value [73]. Currently, the difficulty is a constant value. However, IOTA developers are working on the implementation of the Coordicide protocol, which, in particular, will introduce an adaptive complexity mechanism [74].

Transactions themselves can belong to one of three types: _input_, _output_ and _zero-value_ (Fig. 3.19). The transaction type depends on its purpose. To transfer funds, each input transaction must correspond to an output transaction.

![Figure 3.19 – Transaction types in IOTA and their required fields](/resources/img/volume-3/3.3-Design-and-application-of-DAG/F-3.19-transaction-types-in-IOTA-and-their-required-fields.png "Figure 3.19 – Transaction types in IOTA and their required fields")

The input transaction has a negative _value_ field, indicating that the coins are subtracted from a specific address. It also must contain the address with a sufficient number of coins, the signature or its fragment and the correct nonce value.

The output transaction, in turn, must contain a positive value in the _value_ field, a valid recipient’s address, and nonce. Zero-value transactions have 0 in the _value_ field, and may not have a valid address field. This transaction type is used to exchange messages between network participants.


### The structure and purpose of the bundle

Earlier in the section, we mentioned the term _bundle_. A bundle is a group of interdependent transactions [81]. For example, a transaction transferring coins to Alice’s address directly depends on the transaction which subtracts these coins from Bob’s balance. Therefore, they will both be placed in the same bundle.

The bundle consists of three parts: _head_, _body_, and _tail_. The first transaction in the bundle is placed in the tail (its _currentIndex_ equals zero), and the last transaction goes to the head (its _currentIndex_ equals _lastIndex_).

All transactions in the bundle, except for the head, are linked through the _trunkTransaction_ field. This relation allows nodes to determine whether transactions belong to the same bundle and validate them. The _branchTransaction_ field is used to refer to one of the transactions previously added to the graph. For a better understanding, let’s take a closer look at the bundle example in Fig. 3.20.

![Figure 3.20 – Bundle structure](/resources/img/volume-3/3.3-Design-and-application-of-DAG/F-3.20-bundle-structure.png "Figure 3.20 – Bundle structure")

As you can see from the figure above, all four transactions are interconnected – the _trunkTransaction_ field of every transaction contains the hash value of the next one. The _branchTransaction_ field contains the hash of trunk tip – the transaction from another bundle that was added to Tangle. Since TX(3, 3) is the last in the bundle, its _trunkTransaction_ field references the same trunk tip, while the _branchTransaction_ field references the branch tip – the second transaction from Tangle.

Bundles are also divided into types: _transfer bundles_ and _zero-value bundles_. In the first case, the bundle contains at least one input and one output transaction. Zero-value bundles contain zero-value transactions with informational messages. Let's look at a brief example.

Alice wants to send Bob one coin using a 2-level signature to sign transactions. There will be three transactions in the bundle: the first one adds the coin to Bob’s balance, the second one subtracts it from Alice’s balance and contains a fragment of the signature that is used so that Alice can prove that she owns her private key. The third transaction contains the remaining fragment of the signature (Fig. 3.21) since the signature is too big and cannot fit in one transaction. Note that the transaction that adds coins to Bob’s balance does not require a signature, so the _signatureMessageFragment_ field remains empty.

![Figure 3.21 – An example of a bundle with three transactions transferring one coin from Alice’s address to Bob’s address](/resources/img/volume-3/3.3-Design-and-application-of-DAG/F-3.21-bundle-with-three-transactions-transferring-one-coin.png "Figure 3.21 – An example of a bundle with three transactions transferring one coin from Alice’s address to Bob’s address")

### \*\*\*Frequently asked questions\*\*\*

_– Why does IOTA use the ternary numeral system?_

The ternary numeral system not only offers greater calculation efficiency but also allows working with negative numbers at the arithmetic level without having to use additional code, as is the case with binary.

_– Who is the Coordinator and what is its role in IOTA?_

Since the IOTA network is still at an early stage of development, it cannot afford to be completely decentralized, which is why developers had to introduce the _Coordinator_ mechanism. A coordinator is a validator node owned by the IOTA Foundation. All transactions that are referenced by Coordinator’s transactions (called _milestones_) are considered as finally confirmed.

# [4 PRIVACY AND ANONYMITY ON THE INTERNET](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/4-Privacy-and-anonymity-on-the-Internet.md)
