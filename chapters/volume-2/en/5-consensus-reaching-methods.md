# CONSENSUS REACHING METHODS

## 5.1 Proof-of-stake consensus algorithms
The PoS (proof-of-stake) describes the proof of coin ownership in a specific digital accounting system. The PoS-based consensus algorithm was created as a less resource-intensive alternative for proof-of-work. In this approach to building a consensus-reaching algorithm, the probability of a node gaining the right to create a new block depends on its coin balance. In this section, we will look at the basic principles behind PoS consensus algorithms and the features of using this approach in accounting systems.

### Operation principles and block structure of proof-of-stake
When a proof-of-stake consensus algorithm is used, the validator node is chosen in a pseudo-random manner based on a combination of factors including the age of particular coins, certain random values and a node state. It is also worth noting that in proof-of-stake systems, the process of generating a block is called *forging* or *minting*.

In the generalized form, the order of the new block’s formation in PoS is as follows. A block header, the current time, the validator’s balance (his stake) and other specific data such as calculated coinage (see description below) or a certain random variable is input in a function, which ensures that the validator is chosen in a fair manner. This function is called at a certain interval and each time the validator tries to execute it, it gives back a certain result (whether the request was successful or not).

Digital currencies that use a proof-of-stake consensus algorithm include the following.

> * Peercoin
> * Nxt
> * NEM
> * Novacoin
> * Cardano

When the proof-of-stake system starts, coins are not distributed. The creators usually start with a pre-issuance of coins or start with proof-of-work and then switch to the pure proof-of-stake. But then, why do validators get the reward? In the first case when coins are pre-issued, validators get rewards from the transaction fees. When PoW and PoS are combined, validators can receive their rewards both for the generation of new coins and for accepting transactions.

To participate in the creation of new blocks, there must be some coins on a participant's balance — this is called stake. The stake size affects the probability of a node being selected as the next validator for forging a block (fig. 5.1). Hence, the larger the stake, the greater the chance of forging the next block.

<img width="50%" alt="Figure 5.1 – Distribution of the probability to add a new block among validators depending on their stake" src="/resources/img/volume-2/5.1-Proof-of-stake-consensus-algorithms/Figure-5.1-Distribution-of-the-probability-to-add-a-new-block-among-validators-depending-on-their-stake.png"/> 

Let's observe the differences between the block structure in proof-of-stake and proof-of-work systems using Peercoin as an example (Fig. 5.2). The Peercoin system itself will be discussed in detail further.

<img width="50%" alt="Figure 5.2 – Comparison of PoW and PoS block structures" src="/resources/img/volume-2/5.1-Proof-of-stake-consensus-algorithms/Figure-5.2-Comparison-of-PoW-and-PoS-block-structures.png"/> 

On the left, there is a scheme of a PoW block. Its header is similar to the one in Bitcoin. Coinbase transaction is a degenerate transaction, which has only one null input, containing no links but an unlimited number of outputs. The sum of the output values must not exceed the amount of the reward, which is provided for creating this block. A coinbase transaction is followed by other users' transactions. According to the original Bitcoin rules, these transactions pay a fee, which is added to the reward for creating the block.

On the right, there is a proof-of-stake block. There are already two degenerate transactions in its header: the first one is coinbase, which is mandatory according to the protocol rules, and the second one is coinstake, which is needed when the block is generated according to the PoS rules. A coinstake transaction has its own formation rules. A coinbase transaction here contains one input and one output, both of which are null, whereas a coinstake transaction contains at least one input, where a link to the unspent output of the transaction located at a certain depth is indicated (coinage parameter is calculated for these coins), and two or more outputs, the first of which is null while the other ones spend and redistribute coins. The reward for creating a proof-of-stake block will be summed up with the coins that the author directs to himself in the outputs.

In addition, this block contains the block author's signature, which must be verified by the public key specified in the first input of the coinstake transaction. The signature proves that exactly this participant created the block. The coinstake transaction is followed by users' transactions that are also signed, that is, the entire block is signed.

This is done so that no outsider can impropriate a new PoS block and use it to confirm and distribute his transactions. If a user does not own a specific key, she cannot generate the necessary signature.

### Peercoin
Peercoin [38] is the first cryptocurrency, which introduced the proof-of-stake algorithm. However, it is worth noting that in Peercoin a combination of PoS and PoW algorithms is used. This means that some blocks in the chain are created using PoW, while others are created using PoS. Both options confirm transactions and provide the reward in the form of fees and new (issued) coins. However, when blocks are created using PoW, the number of new coins is much bigger since the probability of a successful generation, in this case, does not depend on the validator’s balance. It is also worth noting that the difficulty parameters for PoW and PoS are calculated and changed independently of each other. As in proof-of-work, the proof-of-stake difficulty is responsible for the probability of proof generation on the first try and is used to control the block generation frequency in order to keep it consistent over a long period of time. For example, if too many or too few blocks were generated recently, the complexity will increase or decrease accordingly.

As already mentioned, the chance of a node to forge a block in a proof-of-stake depends on the number of coins in its balance. However, this is not the only metric since it would give preference to the richest validators who decide to keep more coins in their wallets. To avoid this situation, additional methods are used to select validators.

Peercoin implements the *coinage* mechanism for this. The age is determined by multiplying the number of coins in a stake by the number of days they stay there. However, the coin must stay in the wallet for 30 days before it can take part in forging. Users who keep a lot of coins with high age in the stake will more probably have the right to forge the next block.

After the node has validated the next block, the coinage is reset and it needs to wait 30 days again. At the same time, the probability of finding the next block maximizes after 90 days. This prevents users with high stakes and coinages from dominating.

> **_Note._** *The coinage spent on the orphan block is renewed. It leads to lower attack costs and means that an attacker can try to generate blocks until he succeeds. To prevent risks, Peercoin implements a centralized checkpoint translation mechanism that records the state of a blockchain and notifies about the danger of alternative history versions.*

If the wallet owner wants his coins to stop participating in the validation process and return to the main balance, he will have to wait for a certain time. This is done to make sure that the blocks added with the participation of these coins won’t appear invalid.

### Nxt
Nxt [39] is a digital currency based on the pure proof-of-stake algorithm, which means that there is no way to mine new coins and the entire supply (1 billion) is available from the beginning of the system. The main difference of Nxt is how it provides protection against a Nothing-at-Stake- and other types of attacks.

The block is generated based on the unique information from the previous block, which is easy to verify and almost impossible to predict. The node’s chance of forging a new block depends on its effective balance (which is different for each account), time after the previous block was added, and a certain base value. All nodes in the system know the last two parameters. Based on this data, it is easy to predict who will receive the right to generate the next block.

The coins are added to the account’s effective balance if they didn’t participate in transactions during the time needed to generate 1440 blocks. Only coins on the effective balance take part in the forging process. To prevent long-range attacks, participants can reorganize only 720 previous blocks. A transaction is considered ultimately valid if it is enclosed in a block that is located 10 blocks away from the current one.

It is worth noting that Nxt instead of the UTXO mechanism (like in Bitcoin and Peercoin) uses the account and balance model, similar to Stellar and Ethereum.

### NEM
Initially NEM [40] was designed as a clone of Nxt but later was significantly redesigned and grew into an independent system. Instead of proof-of-stake, NEM uses a modification called proof-of-importance (PoI).

PoI is quite similar to PoS, but additional factors are used to determine the next validator. The node’s importance is determined by the following three characteristics: the number of coins, the number of transactions and the age of the account itself. In such a way the creators of NEM encourage to not only accumulate coins but also to use them.

Just like in Nxt, there is no coin mining in NEM. Validators receive a reward only from fees of confirmed transactions.

In NEM terminology, receiving a reward for generating a block is called *harvesting*. In order to be able to participate in the block generation process, the user must have at least 10,000 coins on his account balance. Moreover,  coins do not start to participate in the process right after they were transferred to the balance. Every day only 10% of coins from the current balance become “competent”.

You can see that harvesting is a lot like forging, which is implemented in other PoS cryptocurrencies. However, it has certain differences, the most important of which is that the device doesn’t have to be turned on all the time to participate in the process of transaction validation.

NEM has a *delegated harvesting* mechanism. The idea of it is that the participants can connect their account to an existing supernode and use its computing power to generate blocks together. It means that you can let the other node use your POI rating, thus you increase the chances of generating a new block without the need to increase computing power.

As soon as a new transaction appears on the NEM network, the first node that detects it, verifies it and notifies others. Hereby, it cascades information through the network.

To maintain a “reputation system” among nodes, NEM uses the Eigentrust$++$ algorithm. It distributes the load across the system and eliminates nodes that are not involved in the validation process.

### Ouroboros
Ouroboros protocol was developed in 2016 [41]. It was designed to eliminate the shortcomings of first-generation systems. Ouroboros is the first provably robust PoS protocol. The right to generate the next block is determined randomly, but this process is verifiable and provably fair. When the majority of participants are honest, an attacker will not be able to influence the randomness of an algorithm.

Ouroboros introduces the concept of an epoch (Cardano, based on this protocol, uses epochs that last 5 days). Each epoch is, in turn, divided into slots lasting 30 seconds (Fig. 5.3). For each slot, a leader is selected who receives the right to add a block to the current slot. Leaders are rewarded regardless of the fact whether they have issued a block. The security model assumes that an attacker can expand an arbitrary number of chains to her time slot.

<img width="50%" alt="Figure 5.3 – Epochs, slots and leaders" src="/resources/img/volume-2/5.1-Proof-of-stake-consensus-algorithms/Figure-5.3-Epochs,slots-and-leaders.png"/> 

The slot leader selection is based on the Follow the Satoshi (FTS) algorithm [42]. Its idea is that one coin out of all coins in the stake is randomly selected, and its owner becomes the leader with all the privileges and obligations arising herein. Therefore, the more coins of a particular node participate in the share, the more chances it has to become a leader.

Since the leader has significant authority, there must be a way to make sure that he has been chosen truly randomly. To ensure randomness, the special distributed cryptographic protocol, which ensures random output values, is used. The so-called voters (users with a certain stake size, for example, 2% of the total number of coins) make a notional “coin toss” and share the result with other voters. The idea is that each participant generates an input value, and the participants ultimately obtain a shared random output value, which cannot be affected by an attacker provided that there is at least one honest participant in the system.

The selection is divided into three stages:

1. Commitment phase. Every participant generates a secret, and then forms a "commitment" containing encrypted stakes for the other participants and signs it with her private key, specifies the epoch number and attaches her public key so every participant can verify that all the data of her message is correct.
2. Reveal phase. The participants reveal their commitments formed at the previous phase. Voters add this value to the blockchain.
3. Recovery phase. If, for some reason, some promises remain unrevealed (some participants have network issues and so on), then the remaining participants augment their shares, obtained at the first stage and open all the remaining commitments. In the end, every participant must have a full set of commitments and revealed values with proofs of their correctness. All the voters receive the same seed value, which is used in the FTS algorithm.

>**_Note._** *The fundamental assumption of the Ouroboros protocol is that there should be a majority of honest nodes, specifically 50% + 1. In this case, the protocol guarantees that attackers will not be able to violate the persistence and liveness of the system.*

### Ouroboros Praos
Ouroboros Praos [43] is the first proof-of-stake protocol aimed at a widespread usage. The main difference of Praos is that it works in networks with unknown upper message delivery delay. It provides guaranteed security in case an attacker chooses participants who follow his rules and generate blocks beneficial for the attacker. However, this is only possible if more than 50% of participants are honest.

Ouroboros Praos uses verifiably random functions to ensure randomness. The function receives the secret key and input data and outputs a pseudorandom number and the proof. Anyone with a public key and proof can check if the number was actually obtained from the given input but cannot receive it preemptively.

For each epoch in Praos, there is pre-agreed nonce value, which all participants have to use as the input data for their verifiable random functions. In each slot, each participant uses their verifiable random function and nonce to generate a random number. If the generated number is less than the threshold value for the participant’s stake, he becomes the leader for a given slot. Since random values are generated independently by each participant, there may be a situation when there are several or no leaders. The nonce for the next epoch is calculated from values of verifiable random functions built in the headers of blocks in the previous epoch.

If several leaders are selected, a fork occurs, even if they are all honest. In the case of a fork, honest parties should choose the chain they received first. It means that leaders who transfer their blocks faster have a greater chance of being in the longest chain.

### Ouroboros Genesis
Ouroboros Genesis [44] was developed with a goal to create a PoS protocol that could work correctly in conditions similar to Bitcoin. To do this, the concept of Dynamic Availability was introduced. This involves an analysis of the blockchain protocol in the environment with the following characteristics:

> * Parties connect and disconnect as they wish
> * The number of online and offline participants is dynamically changing
> * At any given moment in time, nothing is known apriori

The Ouroboros Genesis protocol is aimed at protecting the system from long-range attacks, which are implemented when an attacker gets access to private keys of users who spent their coins a long time ago and uses these coins to create an alternative history by diving much deeper into the blockchain.

The main improvement that Ouroboros Genesis brought is the new chain selection principle. The leader chooses the longest chain that doesn’t exceed the preassigned length or the one that has more blocks on the interval after the fork (fig. 5.4).

<img width="50%" alt="Figure 5.4 – Main chain selection scheme" src="/resources/img/volume-2/5.1-Proof-of-stake-consensus-algorithms/Figure-5.4-Main-chain-selection-scheme.png"/> 

### Main advantages and disadvantages of proof-of-stake
Now that we have considered the basic principles of proof-of-stake and the specific implementations of accounting systems based on this protocol, let's highlight its advantages, which are more notable when comparing this protocol with proof-of-work.

> * No extensive computational power needed
> * The right to create blocks depends not on the computational power but on the amount of coins on a particular balance (and in some cases on the characteristics of these coins)
> * No dedicated hardware is needed to generate blocks

The main advantage of proof-of-stake systems is that extensive computational power is not needed to participate in the transaction validation: any personal computer with an appropriate software and enough coins on the wallet balance can participate in forging. Moreover, as we mentioned earlier, in systems such as NEM, coins can be delegated to a supernode; in this case, the participant does not even need to constantly keep the computer turned on.

The next advantage significantly increases the cost of performing a 51% attack for an adversary. However, this will be true only if the characteristics of the coins are reasonably chosen that are considered when choosing the next validator as well as if some randomness is introduced to protect against precomputation attacks. We will discuss this and other attacks in more detail later.

The last important advantage is directly related to the previous two. In order to become a validator in a proof-of-stake system, one does not need to buy ASICs, an extensive GPU, or an extensive CPU.

However, proof-of-stake systems have the following disadvantages:

> * A one-time issuance is a more centralized approach compared to the permissionless coin mining, which is extended over a certain period of time.
> * Systems based on the proof-of-stake protocol are prone to more attacks that are easier to implement than, for example, proof-of-work-specific attacks.

### Basic types of attacks on proof-of-stake systems
Now let's look at some specific attacks aimed at proof-of-stake systems:

> * Nothing-at-stake
> * Precomputing
> * Fake stake
> * Coin age accumulation attack
> * Short-range attacks
> * Long-range attacks

### Nothing-at-stake attack
The idea of nothing-at-stake is based on an assumption that an attacker can easily build the alternative chain of blocks that other validators will support since they can easily work on both chains simultaneously and it costs them nothing in terms of computing power. Thus, a situation can occur when all validators are working on several chains at the same time (fig, 5.5 and 5.6).

<img width="40%" alt="Figure 5.5 – An example when the node behaves correctly in the case of a fork" src="/resources/img/volume-2/5.1-Proof-of-stake-consensus-algorithms/Figure-5.5-An-example-when-the-node-behaves-correctly-in-the-case-of-a-fork.jpg"/> 

<img width="43%" alt="Figure 5.6 – Nothing-at-stake situation when the node works simultaneously on several alternative chains" src="/resources/img/volume-2/5.1-Proof-of-stake-consensus-algorithms/-Figure-5.6-Nothing-at-stake-situation-when-the-node-works.png"/> 

The second reason why this can occur is that validators have the financial motivation to work on several chains at the same time since they will generate more blocks and receive more rewards.

However, it is important to note that in order to implement a nothing-at-stake attack, the following assumptions must be true:

1. A validator will seek profit in any possible situation even if this will jeopardize network security and quality.
2. None of the validators will behave in accordance with the system rules
3. Validators modify their software or use a third-party one that is deliberately modified for incorrect behavior.

The main risk of nothing-at-stake is that an attacker can perform double spending having only 1% of the total stake in the system as long as all nodes support the fork. Let’s take a closer look at this situation (fig. 5.7).

<img width="55%" alt="Figure 5.7 – Double spending based on nothing-at-stake" src="/resources/img/volume-2/5.1-Proof-of-stake-consensus-algorithms/Figure-5.7-Double-spending-based-on-nothing-at-stake.png"/> 

1. Alice creates a fork of the chain and sends a certain number of coins to the exchange.
2. Alice adds a transaction to the main chain, in which she transfers funds to her alternative account.
3. Alice exchanges her coins; the exchange transaction is recorded in an alternative chain, which also looks valid since it is supported by all participants.
4. If the exchange was successful, Alice withdraws the received coins to her wallet in another system.
5. Alice stops validating blocks of an alternative chain, which leads to the chain being dismissed.

In real conditions, such a situation is extremely unlikely since it assumes that absolutely all validators will support each new block. In existing systems, there will always be honest nodes that will not support alternative chains. Thus, Alice will have to put some of the validators up to work on her chain or increase her stake.

### Precomputation attack
If a validator node has significant processing power, it can affect the hash of the current block in order to increase the chance to generate the next one. If calculations show that the next block will belong to another user, the attacker changes the transaction parameters and tries again. The effectiveness of this attack depends on the attacker's stake and the total number of validators in the system.

To prevent such attacks, a certain degree of randomness should be added to the block generation process.

### Fake stake attack
This attack relies on the fact that some consensus algorithm implementations based on proof-of-stake do not verify a coinstake transaction before placing a block in RAM or on a hard disk. Coinstake transaction is a special transaction that a node sends to itself when generating a block. As a result, an attacker can cause the victim node to fail by filling its hard drive or RAM with fake data even with a very small number of coins (or no coins at all).

The problem is that in many PoS systems (for example, HTMLCoin, Emercoin, Qtum, etc.), when blocks are distributed over the network they are divided into two separate messages: Block and Header. Nodes request Block only after Header is validated and included in the longest chain. Since the coinstake transaction is contained in Block, the node will not be able to validate Header without it. Therefore, it has to store Header directly in RAM, which gives an attacker the possibility to fill the victim’s RAM with counterfeit transactions.

### Coin age accumulation attack
This attack was possible in early versions of systems that used the coin age mechanism. An attacker with a certain stake could wait for a long time and gain almost complete control over the network.

Currently, in such systems as Peercoin, Novacoin, and Blackcoin, there is a maximum coin age restriction equal to 90 days.

### Short-range attacks
To perform a short-range attack, the malefactor creates an alternative chain starting with one of the recent blocks and tries to overtake the main one, convincing participants to switch to his version. It is more profitable for validators to work on several chains at the same time since it increases their income and the chances that the version they are working on will turn out correct.

However, as you remember from the previous sections, some PoS-systems, such as Nxt, use mechanisms based on specific characteristics that allow predicting who will work on the new block next.

An alternative approach to protect against short-range attacks is forcing validators to pledge a certain amount of coins before they get the right to participate in the process of generating blocks. Validators that sign conflicting blocks will lose their pledge.

### Long-range attacks
The long-range attack is based on the method of trying to rewrite the blockchain history starting with the genesis block, since unlike in PoW-based systems, it does not require extensive computational resources. Moreover, an attacker may also try to gain access to the private keys of the early system participants in order to use the corresponding coins for building an alternative chain.

This attack can be prevented by limiting the depth at which validators can create an alternative blockchain. This method is used by many currently operating PoS systems.

**Frequently asked questions**

*– Why can a wallet owner who has a lot of coins in the system not  use them for cheating?*

If the wallet owner with a lot of coins tries to cheat, the price of these coins will drop and thus be disadvantageous for him.

*– If 100 proof-of-stake network nodes generate and send their block proposals once per second, one selected block will remain. Who will choose which of these blocks will be correct and exclude the remaining 99 ones?*

The consensus algorithm will be based on the block selection rule. In order for everyone to be guided by this rule and choose one block, certain parameters are provided. For example, in Peercoin, it is coinage. In the case of PoW, if someone generated a correct block, but another one was accepted, the creators of the block would waste computational power. In classic PoS, the block creator does not spend his coins. In Peercoin, they will remain unspent at the same chain depth and can be used for the next attempt to generate a block.

*– Can a PoS system be protected from a situation when malicious nodes modify the block rules?*

The rules cannot be modified without modifying the source code of already running nodes. Decentralized systems assume the presence of certain accounting rules, and there are always nodes that follow these rules. Of course, some malicious nodes may modify their software, but they will not be able to work with honest nodes, and any attempts to modify the protocol or its rules will fail.

## 5.2 Delegated proof-of-stake as a consensus algorithm
DPoS (delegated proof-of-stake) is a consensus algorithm for the decentralized environment that was created as an alternative to PoW (Bitcoin proof-of-work) and PoS (Peercoin or NXT proof-of-stake). DPoS was developed in 2014 in the context of the Graphene project. It was used in Bitshares for the first time and later in Steemit and EOS. DPoS solves the main problem of PoW — significant energy consumption. To solve it, DPoS provides the right to generate blocks only to validator nodes that were chosen during the voting process and have the hardware that meets specific computational power requirements.

DPoS also solves the main problem of PoS that is the need for the user to launch a full node so that his coins can take part in the consensus process. In DPoS this problem is solved by the presence of full-node validators that process transactions. To allow for his coins taking part in the consensus process, a user just needs to send a special transaction with a vote.

Compared to the proof-of-stake, users’ coins in DPoS systems can take part in the voting process and at the same time be used for transfers. As a result of the balance change, the weight in the voting will change accordingly.

This consensus algorithm works best for accounting systems designed for the presence of both ordinary users (mobile clients) and various companies (full-fledged nodes that act as validators). DPoS is designed to maintain a high frequency of generating new blocks and has a high transaction capacity compared to other consensus algorithms that operate in a distributed, trustless environment. DPoS is well suited for systems with open access that don’t need user identification.

### DPoS algorithm
The working conditions of this consensus algorithm differ from PoW and PoS. Validators need to disclose their identities and declare their readiness to continuously support full nodes, timely verify transactions, and form new blocks.

The consensus algorithm, which is based on the modified proof-of-stake, allows every user to be elected as a validator. Voting is performed among all users and the vote weight is defined by the voter’s asset amount. Since users can see who got the right to generate new blocks, they can form a strict sequence of validators and optimize the block generation and validation process. This enables the platform to reduce the block generation time and increase capacity. It was tested in practice that with several dozens of validators, it is possible to achieve a block generation frequency of 1–2 Hz.

Depending on the DPoS implementation, a user can also determine the weight of the bet, but the bet, in any case, cannot exceed the amount of assets the user has. There is another feature that can be available depending on the implementation of DPoS. The feature is called proxy voting. The idea is that if a user cannot decide who to vote for, he can pass his vote weight to another regular user who he considers to be more competent in this specific case.

Based on the voting results, N (a natural number decided by the community, usually between 20 and 50) candidates who receive the right to form new transaction blocks are selected. Interestingly, committee members (if any) and validators are not anonymous. In a digital currency system, they have accounts associated with their identity. They have a certain reputation, participate in the project’s development, present on forums, offer improvements, etc. Their activity is visible. The protocol rules guarantee the correct decision-making if the majority of the assets participating in the voting are controlled by honest users. Based on the voting results, delegates who have become validators begin to publish blocks one by one. The validators that were selected in the voting process are shuffled in a pseudo-random manner forming a queue. Shuffling is performed by a special algorithm so it is impossible to predict the queue in advance. However, it is the same for all honest network participants. After the last validator in the queue has published a block, the group cycle ends and a new one begins in the same order. The validators list is valid during the epoch of validator work cycles. After each epoch (fig. 5.8), the list of validators is updated accordingly to the voting results and the votes of users who voted during the validators' work come into effect. Later these actions are repeated.

<img width="50%" alt="Figure 5.8 – Consensus achievement stages" src="/resources/img/volume-2/5.2-Delegated-proof-of-stake-as-a-consensus-algorithm.png/Figure-5.8-Consensus-achievement-stages.png"/> 

Validators participate in:
> * Supporting full nodes in a stable operating condition
> * Collecting and verifying transactions from all users 
> * Block generation
> * Signing and publishing blocks
> * Difficult recovery of a lost dataset

Validators publish blocks with a hash value of the last block they agree with. If all validators are honest and agree with each other, then the blockchain looks as follows (Fig. 5.9 (1)). If there is a disagreement, for example, validator C does not agree with the block of validator B, then the chain will fork (Fig. 5.9 (2)). The main chain will be determined according to decisions of other validators.

<img width="42%" alt="Figure 5.9 – Blockchain structure: (1) – validators are in agreement; (2) – validators disagree about the next block" src="/resources/img/volume-2/5.2-Delegated-proof-of-stake-as-a-consensus-algorithm.png/Figure-5.9-Blockchain-structure-(1)-validators-are-in-agreement-(2)-validators-disagree-about-the-next-block.png"/> 

Delegates who have launched full nodes and are ready to provide their services but have not become validators yet remain to wait to become a validator. The delegate node is actually an auditor node but can at any time become a validator node if it rises in the queue of candidates.

Depending on the implementation of DPoS, for example in BitShares, users may also have the option to transfer their vote weight to certain proxies whose sole purpose is to monitor the system and vote. This option is called proxy voting.

It is worth noting that, for example, BitShares users, in addition to selecting validators, also have an option to select committee members and workers. Committee members are the users who can vote to change protocol characteristics, such as block size, intervals between blocks, as well as to determine the transaction fees. Workers are users that provide offers to implement the actual work to update the protocol. If the worker’s proposal receives enough votes from committee members, then he begins the development and receives rewards for each stage of the implementation.

In this algorithm, the weight of each delegate is directly dependent on users. By voting, users can take away the weight from one by redirecting or removing bids if delegates begin to act maliciously or stop performing their work. Notably, changes in the votes come into effect after the epoch ends. Over time, the structure of the validator group can be changed by the voting as a result of which the inactive participants lose their rights while the working ones are awarded.

### How is DPoS system launched?
At the beginning of the DPoS system, when there are no users who trust each other, there are no validators among them to generate blocks. Thus, the system lacks the ability for users to become validators, and it becomes impossible to launch the system. That’s why before launching a system with this consensus algorithm, the community usually votes on a different platform. This voting allows choosing users who will work as validators and play other system roles. Community members also can announce the first list of validators and other participants that contains their trusted users.

### How does DPoS work?
Users who want to be validators run a full node and declare their willingness to generate blocks. To do this, they form transactions by creating a new delegate. After the delegate was nominated, users who want to vote form a transaction with a corresponding operation.

For example, Alice and Bob want to become validators (Fig. 5.10). To do this they form transactions that contain the operation of a new delegate creation. These transactions are added to the ledger with the publication of block n. Eve and Carol have 115 and 564 coins on their balance respectively, and they both want to vote. To do this, they form transactions with a voting operation that appear in the ledger with the publication of one of the following blocks, for example, n + 1.

<img width="42%" alt="Figure 5.10 – Voting example" src="/resources/img/volume-2/5.2-Delegated-proof-of-stake-as-a-consensus-algorithm.png/Figure-5.10-Voting-example.png"/> 

Let’s take a look at how DPoS works using a general example with three validators and three cycles of their work:
    
Users: Alice, Bob, Carol, Eve, Will, Joe and Marie.
Balance: Alice - 1; Bob - 2; Carol - 3; Eva - 4; Will - 5; Joe - 6; Marie - 7.

You can see that among all users, the most influential at the voting stage is Marie. Then the voting takes place (Fig. 5.11):

Voted for Carol: Eve (4), Alice (1), Will (5), Joe (6).
Voted for Eve: Carol (3), Bob (2), Marie (7).
Voter for Alice: Carol (3), Eve (4), Bob (2), Joe (6).
Voted for Bob: Will (5).

<img width="30%" alt="Figure 5.11 – The voting results" src="/resources/img/volume-2/5.2-Delegated-proof-of-stake-as-a-consensus-algorithm.png/Figure-5.11-The-voting-results.png"/> 

In the end, Carol, Eve and Alice are considered to be equal validators since they have the largest number of votes, and Bob remains a delegate. In this case, the consensus is achieved by publishing blocks by an alternating group of validators. Suppose some user wants to conduct two conflicting transactions, TxA and TxB. Validators Alice, Caro, and Eva verify them and publish only the transaction they agree with. In this situation, there are two options:

#### Option 1
Validators agree with TxB (fig. 5.12). Eve agrees with TxB; Alice agrees with TxB, meaning she agrees with Eve; Carol agrees with TxB and therefore agrees with Alice. The chain does not fork.

<img width="50%" alt="Figure 5.12 – Block formation if every validator agrees with the previous block state" src="/resources/img/volume-2/5.2-Delegated-proof-of-stake-as-a-consensus-algorithm.png/Figure-5.12-Block-formation-if-every-validator-agrees-with-the-previous-block-state.png"/> 

#### Option 2
Carol agrees with TxA, while Eve and Alice agree with TxB. Eve forms a block with her transaction and then Alice, if she agrees with Eve’s decision, forms her block based on Eve's block. Otherwise, Alice forms a separate block that she considers correct. Carol then forms a block based on one of Eve’s or Alice’s blocks — the one she agrees with. In this case, the chain forks. Now, for all network nodes, there are two alternative chains to choose from. If they both meet the protocol rules, then honest nodes are interested in making the same choice. That’s why a protocol has the rule that states that the chain is considered a priority if it is supported by a bigger number of validators. In this example (Fig. 5.13), Alice and Eve chose the transaction TxB, and since it is the majority of validators, the chain with TxB is considered relevant, while the chain with TxA is considered to be a fork.

<img width="50%" alt="Figure 5.13 – Block formation while one of the validators disagree with the transaction set" src="/resources/img/volume-2/5.2-Delegated-proof-of-stake-as-a-consensus-algorithm.png/Figure-5.13-Block-formation-while-one-of-the-validators-disagree-with-the-transaction-set.png"/> 

Suppose Option 2 happened, and Carol denigrated herself by publishing block with TxA and not TxB; hence users decide to redistribute their votes (fig. 5.14):

Voted for Carol: Bob (2).
Voted for Eve: Carol (3), Alice (1), Will (5), Joe (6).
Voted for Alice: Eva (4), Will (5), Joe (6).
Voted for Bob: Will (5), Joe (6), Marie (7).

<img width="30%" alt="Figure 5.14 – Second vote results" src="/resources/img/volume-2/5.2-Delegated-proof-of-stake-as-a-consensus-algorithm.png/Figure-5.14-Second-vote-results.png"/> 

As a result, Bob, Eve, and Alice are considered equal validators since they have the largest number of votes and Carol is no longer a validator. The list of validators is updated and the whole process repeats.

**Frequently asked questions**

*— What consensus algorithm should I choose for my accounting system?*

There are several main criteria. Do users trust validators? Are validators anonymous? Is their number constant or does it change over time? How are validators connected? Only by using these indications can you distinguish different types of accounting systems with different consensus algorithms.

## 5.3 BFT-class algorithms
Not all accounting systems require the same decentralization level as in Bitcoin, but at the same time, they need much higher capacity and a lower transaction confirmation time. Moreover, it often makes sense to create a permissioned accounting system with an ability to assign validators exclusively in the manual mode. BFT consensus protocols work in these specific conditions.

In this section, we will take a look at the operation principles of the most widespread protocols of such class. We will also make a comparative characteristic of these protocols and explain what ensures the fault tolerance of the BFT-type systems.

### Practical BFT algorithm
Practical Byzantine Fault Tolerance (pBFT) [45] is the consensus algorithm designed to work in asynchronous decentralized systems. All nodes in the system are connected, and at each moment of time, one of them is the leader. The purpose of the protocol is to achieve agreement between all honest nodes when the number of failed nodes is no more than *(n − 1) / 3*, where n is the total number of validator nodes in the system. In BFT terms, failed nodes are not only those who are inactive or unpredictable but also those who intentionally collude to disrupt the work of honest nodes.

The algorithm consists of 5 main stages, from the proposal to add transactions to the final update of the accounting system’s state.

> * Request
> * Pre-prepare
> * Prepare
> * Commit
> * Reply

Next, we will take a look at the characteristics of these stages in pBFT, and then we will examine how the number of honest nodes affects the ability of the system to reach an agreement on updating the state of the accounting system.

At the *request stage*, the leader node receives the transaction to be confirmed (fig. 5.15). Once the leader node receives a transaction, it verifies the transaction and combines it into a block. Upon completion, the block is signed and can be distributed to other validators.

<img width="40%" alt="Figure 5.15 – Leader receives transactions" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.15-Leader-receives-transactions.png"/> 

Next comes the *pre-prepare* stage, during which the leader node sends the formed block to all other validators (Fig. 5.16). The block is signed by the leader and sent with the pre-prepare message that helps other nodes to authenticate the sender and understand which stage of consensus process this block is related to. Note that the total number of messages transmitted at this stage is *n − 1*, where *n* is the total number of validators.

<img width="40%" alt="Figure 5.16 – Block transmission scheme" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.16-Block-transmission-scheme.png"/> 

During the next step (*prepare*), the remaining validators exchange the received block with each other (each validator sends a block to all other validators). The block is sent with the prepare message and the signature of a validator that sent it (Fig. 5.17). Thus, each validator states that it has verified this block and is ready to confirm it. The maximum number of messages at this stage is *(n − 1) ∗ (n − 1)*, where $n$ is the total number of validators. If some nodes are failed and do not respond, the minimum number of messages in the network is *(n − 1 − f) ∗ (n − 1)*, where *f* is the number of failed nodes *(fmax = (n − 1) / 3)*.

<img width="40%" alt="Figure 5.17 – Block exchange scheme" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.17-Block-exchange-scheme.png"/> 

Now, it’s important to note that the network is asynchronous, meaning that there will be delays in the transmission of messages between validators. Under these conditions, the decision to move to the next stage is reached independently by each individual node according to the *2f + 1* received confirmations from other validators. In other words, as soon as one of the nodes receive a prepared message (regarding the same block) from *2f + 1* different validators, it can move on to the next step.

When the required number of confirmations is received, the node proceeds to the *commit* stage at which it states that it confirms a specific block. At this stage, it forms a commit message, signs it and forwards to all other validators (Fig. 5.18). As soon as the individual node receives commit messages from *2f + 1* validators, it updates its local database state according to the confirmed transactions. The maximum number of messages at this stage is *n ∗ (n − 1)*, and the minimum is *(n − f) ∗ n*.

<img width="40%" alt="Figure 5.18 – Commit-messages exchange scheme" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.18-Commit-messages-exchange-scheme.png"/> 

After the local copies of the validators’ databases are updated, users can query them to get the current state and make sure that the next block has been confirmed by most validators (Fig. 5.19). Note that the pBFT consensus algorithm ensures that if validators add another block to the chain, this block cannot be changed/replaced.

<img width="40%" alt="Figure 5.19 – New system state distribution scheme" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.19-New-system-state-distribution-scheme.png"/> 

A certain limitation of the practical BFT protocol is a large number of messages that validators exchange among each other. Note that the maximum and minimum possible number of such messages that allows nodes to reach consensus is:

$$Messages_{max} = n + (n − 1) * (n − 1) + n * (n − 1),$$

$$Messages_{min} = n + (n − 1 − f) * (n − 1) + (n − f) * (n − 1),$$

where $n$ is the number of all validators and *f* being the maximum number of failed nodes (Table 5.1).

Table 5.1
<img width="50%" alt="Table 5.1" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Table-5.1.png"/> 

Let's take a closer look at how nodes reach consensus, and what happens if multiple validator nodes fail or behave maliciously.

### Consensus reaching process
Imagine a situation when 5 validators take part in reaching an agreement using the pBFT protocol, while one of them is a leader. In such conditions, the largest number of failed nodes is 1 (if 2 or more nodes fail, consensus will not be reached).

First, let’s look at an example when all nodes are honest and none of them failed (Fig. 5.20). The process of reaching consensus can be broken down into the following steps.

1. *Request*. At the first stage, the leader receives transactions from system users, verifies them according to the protocol rules and forms a block.
2. *Pre-prepare*. Next, the leader node distributes this block to other validators. Since the leader behaves honestly, all other validators receive the same block.
3. *Prepare*. The nodes exchange received block with each other. Since all nodes are honest and have not failed, at the end of this stage, each validator has the same acknowledgments from all other nodes. Therefore, they can go to the commit step.
4. *Commit*. Validators exchange block value that they are ready to confirm. At the end of this stage, each validator receives the absolute majority of confirmations for the single block and updates its registry state.
5. *Reply*. System users query validators to get the current status. Since all validators updated their registry state based on the same block, other participants (auditors and ordinary users) can update their local status.

<img width="50%" alt="Figure 5.20 – Message exchange scheme for honest validators" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.20-Message-exchange-scheme-for-honest-validators.png"/> 

Now, let’s look at the situation (Fig. 5.21) when one of the five validators will try to interfere with the consensus process. In fact, this node may simply fail; however, we look at the situation when it behaves maliciously — at the consensus level both cases are the same.

1. *Request*. The leader receives transactions and forms them into a block.
2. *Pre-prepare*. In this example, the leader node is honest, so it sends the same block value to all other nodes.
3. *Prepare*. Nodes exchange messages with each other and everyone is waiting for *2f + 1* confirmations of a specific block (from *2f + 1* honest validators). Note that despite the malicious behavior of one of the validators and the fact that it sends a block value that is different from the original one to all other nodes, honest validators can still gather the necessary number of confirmations and proceed to the next stage.
4. *Commit*. Validators once again exchange messages containing the current block with each other. Even when the malicious validator sends an invalid block value to everyone, the remaining validators still receive the required number of confirmations and update their local registry states.
5. *Reply*. When querying validators, users choose the block that was confirmed by the majority, so an attacker’s invalid block does not influence when users will update the registry state.

<img width="50%" alt="Figure 5.21 – Message exchange scheme with one malicious validators" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.21-Message-exchange-scheme-with-one-malicious-validators.png"/> 

In this example, problems begin when there is more than one malicious node (Fig. 5.22). In this case, the remaining nodes will not be able to reach consensus (even though there are more honest nodes than intruders). Therefore, we consider how the system works when two malicious validators are present.

1. *Request*. The leader receives transactions and forms them into the block.
2. *Pre-prepare*. The leader node is still honest, so it sends the same block value to all other nodes.
3. *Prepare*. Nodes exchange messages with each other and everyone is waiting for *2f + 1* confirmations. However, the number of malicious nodes exceeds *f*, so honest nodes will not receive enough confirmations and will not be able to move on to the next stage of consensus (commit and reply stages are skipped, a new block is not created, and the nodes try again).

It is important to note that if the leader was guaranteed to be honest, the network could reach consensus (since each one of the other honest nodes would have three out of five confirmations). But since nodes are not sure that the leader is acting honestly (in fact, he can change her mind), a consensus cannot be reached in such conditions.

<img width="50%" alt="Figure 5.22 – Message exchange scheme with two malicious nodes" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.22-Message-exchange-scheme-with-two-malicious-nodes.png"/> 

As the last example, let’s look at a situation when the leader is malicious (Fig. 5.23). In this case, it can transmit different block values to all other nodes so the agreement will not be reached.

1. *Request*. The leader receives transactions and forms 4 different blocks.
2. *Pre-prepare*. The leader sends different block values to the other nodes. Currently, nodes do not know that they have received different block values, so they proceed to the next stage.
3. *Prepare*. Nodes exchange messages with each other (each sends messages to each other) and wait for *2f + 1* confirmations. Since all of them have received different block values, they exchange different values, which ultimately leads to the fact that none of them can get the required number of confirmations and will not be able to proceed to the next stages of consensus.

<img width="50%" alt="Figure 5.23 – Message exchange scheme with a malicious leader" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.23-Message-exchange-scheme-with-a-malicious-leader.png"/> 

### HotStuff consensus algorithm
HotStuff [46] is another BFT-like consensus algorithm which also implies a leader. However, in this scheme, each step in reaching consensus depends on the leader. Each node does not exchange messages with others, but it transfers it to the leader who later distributes it. At each stage, the leader proposes a change in the consensus state (transition to a new stage), and the nodes confirm or reject it.

This approach can significantly reduce the overall network load by reducing the number of messages transmitted by the nodes.

The algorithm implies 4 stages through which nodes reach consensus regarding the registry state change (Fig. 5.24).

> * Prepare
> * Pre-commit
> * Commit
> * Decide

At each of these stages, the leader node forms the so-called quorum certificate, which contains the signatures of the validators. A threshold signature mechanism is used here: nodes proceed to the next stage when the certificate gains the number of signatures that exceeds the required threshold *2f + 1*. Before the beginning of each next stage, the leader sends the certificate of the previous stage to validators. Nodes verify that the required threshold in the certificate has been reached and vote regarding the next stage of consensus.

<img width="50%" alt="Figure 5.24 – Message exchange scheme" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.24-Message-exchange-scheme.png"/> 

*Prepare* means that the leader node receives a set of transactions from the network, forms a confirmation block and distributes this block to the other validator nodes with the prepared message included.

At the *pre-commit* stage, nodes verify that the block they received at the previous stage meets the protocol rules and then send a confirmation to the leader saying that they are ready to validate this block. The leader receives the prepared votes from the validators, combines them into a pre-commit quorum certificate and distributes it along with the pre-commit message for validators to accept.

At the *commit* stage, validators check the pre-commit certificate, and if they agree with it, they sign the pre-commit message and return it to the leader. For this pre-commit certificate to be considered valid, it must be signed by the threshold number of validators. The validator’s signature covers all transactions so when the leader collects votes from validators regarding the entire block, he cannot change it as it will lead to the violation of the signatures’ validity.

After the leader receives the necessary amount of confirmations, he forms quorum certificate again, but this time for the commit stage, and sends it to other validators, after which nodes come to the next voting stage.

At the decide (decision) stage, a similar process takes place. Validators receive a commit quorum certificate from the leader, verify that it is valid and that it contains the certificate of the previous stage signed by the necessary number of validators, and then sign it. After the leader receives a sufficient number of commit quorum certificate confirmations, he forms a decide certificate and passes it to all the validator nodes.

When validators receive such a certificate, they consider all consensus stages to be completed and update the registry state according to the transactions.

Now let's look at the number of messages that are transmitted between validators in this case (Table 5.2).

Table 5.2
<img width="50%" alt="Table 5.2" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Table-5.2.png"/> 

As you can see on the graph, the HotStuff protocol allows the number of transmitted messages to be significantly reduced compared to pBFT. For comparison, the graph below shows the ratio of the number of validators and the number of transmitted messages (Fig. 5.25).

<img width="35%" alt="Figure 5.25 – Relationship between the number of transmitted messages and the algorithm used" src="/resources/img/volume-2/5.3-BFT-class-algorithms/Figure-5.25-Relationship-between-the-number-of-transmitted-messages-and-the-algorithm-used.png"/> 

That said, it is important to understand that if the leader fails at one of the stages, the following happens:

* pBFT protocol implies that the consensus will still be reached if there are enough honest nodes (and if the leader does not fail at the first stage);
* HotStuff implies that the consensus cannot be reached since the transition to each next stage is initiated by the leader only.

**Frequently asked questions**

*– What happens if the node fails for a long time?*

Very often, the consensus protocol stipulates that the leadership role is transferred to a new node at each stage of a new block formation. Thus, if the leader fails and consensus is not reached on one of the blocks, the next block will be initiated by the next node.

*– What is the maximum number of validators for consensus algorithms of the BFT class?*

In theory, there are no severe restrictions. However, the most effective number of validators for BFT-based consensus algorithms is up to several dozen for  pBFT-like algorithms, and several hundreds for HotStuff and similar protocols. These values are optimal from the point of view of the accounting system’s security. At the same time, these values require only small amounts of transmitted messages and thereby allow for a quick consensus-reaching.

*– How can you become a validator in a system that is based on a consensus algorithm belonging to the BFT class?*

To add or delete validators, *2f + 1* other validators must agree to it. Therefore, in order to become a validator, you need to get the approval from the current validators and establish connections with them.

## 5.4 FBA as a consensus algorithm
FBA (Federated Byzantine Agreement) is a BFT-based consensus algorithm. The main difference of FBA is that the interaction of nodes in the system is determined dynamically by the participants themselves. Here the node becomes a validator for those who trust it. In the financial technology field, the FBA algorithm was first successfully implemented in the Stellar platform.

In FBA, the node owner must independently select a group of nodes whose owners she trusts (expecting that they will not violate the rules). Thus, trust can be established in one direction, so node A can trust decisions of node B, but B will not necessarily trust A.

FBA allows consensus to be reached among an unlimited number of validators who do not know each other; moreover, the total number of validators may not be known.

### Terms quorum, quorum slice, and quorum intersection
The set that includes the node itself and nodes that it trusts is called *quorum slice*. One node may have several slices.

The quorum for node A are all nodes from its slice as well as the nodes in slices of these nodes, in other words, everyone who A trusts and those who trust A. Moreover, a quorum can be deeper, including those trusted by nodes that A trusts. Quorum is a closed set of nodes, which is sufficient to reach an agreement in a distributed system.

Quorum intersection is a node or several nodes that belong to several slices simultaneously. Quorum and quorum slice are relative sets that are defined for each node independently. Thus, the number of all possible quorums (or slices) may be greater than the number of nodes in the entire network. 

Figure 5.26 shows an example of a network with a quorum slice and the quorum for node F. Arrows indicate the direction of trust. If there are two opposite arrows between the nodes, they mutually trust each other. If there is only one arrow, the second node does not trust the first.

<img width="42%" alt="Figure 5.26 – Quorum slices and quorum" src="/resources/img/volume-2/5.4-FBA-as-a-consensus-algorithm/Figure-5.26-Quorum-slices-and-quorum.png"/> 

Figure 5.27 displays the quorum intersection where node B is an intersection for {A, B, C} and {G, D, E, F}, and node G is an intersection for {H, I}, and {G, D, E, F}.

<img width="42%" alt="Figure 5.27 – An example of quorum intersection" src="/resources/img/volume-2/5.4-FBA-as-a-consensus-algorithm/Figure-5.27-An-example-of-quorum-intersection.png"/> 

In fact, a node can belong to several quorum slices, and the quorum slice itself can contain another quorum slice. When nodes try to reach consensus, they exchange messages with each other and agree only if the number of those agreed exceeds a predetermined threshold. The threshold should be no less than ⅔ of the number of nodes in the quorum slice.

### Blocking set
FBA also introduces the concept of a *blocking set*. The *blocking set* for the node N is a set that contains at least one node from each of N’s quorum slices (Fig. 5.28). The blocking set can convince a participant to make a certain decision.

<img width="30%" alt="Figure 5.28 – An example of a blocking set consisting of nodes B, D, F" src="/resources/img/volume-2/5.4-FBA-as-a-consensus-algorithm/Figure-5.28-An-example-of-a-blocking-set-consisting-of-nodes-B,D,F.png"/> 

### Disjoint quorums and divergent state
*Disjoint quorums*: two quorum slices that have no nodes intersected directly or indirectly form individual quorums and as a result, come to different decisions, leading to the *divergent state*. The *divergent state* is the state of a system after it has been divided into several independent quorums; while each of them maintains its own transaction history and does not affect the rest.

In PBFT systems, fault tolerance is guaranteed as long as the number of faulty nodes does not exceed ⅓ of the total number of nodes in the system. In the case of FBA, this number varies from 0 to ⅓ depending on the structure of quorum slices. Fault tolerance of FBA will approach zero if a considerable number of nodes create quorum slices with the same number of nodes. If these nodes fail, the system will stop functioning.

An example of such a situation is shown in Figure 5.29. In this case, two quorum slices have only one common node. If an attacker gains control over this node or it fails for some reason, other nodes won’t be able to reach consensus.

<img width="42%" alt="Figure 5.29 – An example of disjoint quorums
Federated voting" src="/resources/img/volume-2/5.4-FBA-as-a-consensus-algorithm/Figure-5.29-An-example-of-disjoin-quorums.png"/> 

### Federated voting
To achieve consensus in the FBA systems, a federated voting mechanism is used (Fig. 5.30). It consists of three stages: voting, accepting and confirming [46; 47]. First, each node can *vote* for a specific statement regarding, for example, adding a new transaction block. By voting for the statement, the node (if it’s not malicious) will never vote for the contradicting one.

<img width="40%" alt="Figure 5.30 – Possible node stated in the federated voting process" src="/resources/img/volume-2/5.4-FBA-as-a-consensus-algorithm/Figure-5.30-Possible-node-stated-in-the-federated-voting-process.png"/> 

After the voting phase, the nodes proceed to the *acceptance* phase. A node can accept the statement if it has not accepted a contradictory statement earlier and there is a quorum where each participant voted for or accepted the statement. Regardless of the quorum decision, the node accepts the same statement as its blocking set. It follows that the node can vote for one statement but accepts another.

However, the blocking set is not a quorum since an interested party could impose its decisions on the node taking control over the nodes from its blocking set. For this reason, there is another step called *confirmation*. The idea is that a node must detect a *quorum* where all nodes accept this statement. A node confirms the statement if all nodes of its quorum also confirm it.

### Using FBA in Stellar
SCP (Stellar Consensus Protocol) [47] is a consensus protocol based on FBA. The Stellar Consensus Protocol uses a federated voting mechanism to ensure consistent results and system operability. During the process, a large number of rounds of federated voting are conducted for various statements until one of them passes through all the stages.

The first round of federated voting is the *nomination* phase when the nodes vote to accept a specific transaction block. At the first stage, nodes generate candidate values through federated voting. The value is considered to be a candidate if the node has *confirmed* its nomination. A node without candidates can nominate any value. After that, it can only accept and confirm nomination statements from other nodes according to the federated voting procedure.

To reduce the number of candidates, a temporary priority mechanism is used. Initially, the node determines the quorum slices in which it consists and selects the node with the highest priority from this set, that is, the one whose value it will nominate. A node can nominate its own value only if it does not find higher priority neighbors. To calculate the priority, a hash function is used. A block number and the public key of the given node are used for the input. The more often a node appears in quorum slices of neighbors, the higher its priority.

Under the influence of the blocking set, the node can vote and accept the nomination of different candidates but will, nevertheless, confirm only one. Let’s look at an example in Figure 5.31.

<img width="40%" alt="Figure 5.31 – An example of the nomination process considering that the node initially voted to nominate A" src="/resources/img/volume-2/5.4-FBA-as-a-consensus-algorithm/Figure-5.31-An-example-of-the-nomination-process.png"/> 

In the nomination process, there may be several confirmed candidates. Later they are combined into a *composite*. The method depends on the individual system it is being implemented in. For example, in Stellar, the set of transactions is merged, and the resulting composite has the maximum timestamp of all the transactions in the set.

When the nomination phase ends and a composite is formed, nodes proceed to the balloting phase (Fig. 5.32), which has three stages: *prepare*, *commit*, *externalize*. During the last stage, nodes actually execute the command they voted for, such as operations in the transaction block. First, nodes have to *vote*, *accept* and *confirm* the *prepare* status for the ballot and then these three phases are repeated for the *commit* state (i.e., federated voting is performed for the ballot).

The ballot itself is a data structure that consists of two fields: a positive integer counter and an object of the class Vote that contains the candidate value from the last stage.

If, for certain reasons, the ballot is stuck in an undefined state, the *abort* operation is performed so that the counter is incremented and the nodes move to the next ballot (the one with a higher counter). When nodes exchange information about ballots’ states, they can attach a range of ballots to the message instead of one specific state.

A ballot gets the *prepared* status when the node is confident that the rest of the validators will not vote to *commit* the ballots with different values. At the same time, the *abort* operation is performed for all the ballots with lower counter values. When the *prepare* statement for the ballot is *confirmed*, nodes proceed to vote on the *commit* status. This allows making sure that everyone agrees with this ballot and in the end, its commands will be executed.

After passing all federated voting rounds and confirming the *commit* statement, nodes execute the command specified in the ballot.

<img width="40%" alt="Figure 5.32 – The example of the balloting process" src="/resources/img/volume-2/5.4-FBA-as-a-consensus-algorithm/Figure-5.32-The-example-of-the-balloting-process.png"/> 

### Centralization problem

The number of malicious nodes with which an FBA system is able to reach consensus varies from 0 to ⅓ of the total number of participants, depending on the structure of quorum slices. The fault tolerance will converge to zero when a large number of nodes create quorum slices with the same several nodes. If these several nodes fail, the system will not be able to operate. An example of such a situation is shown in Figure 5.33. In this case, several quorum slices have only one common node, and if it falls under the control of an interested party or for some reason stops working, the other nodes will also not be able to work.

<img width="30%" alt="Figure 5.33 – Example of a network with one common node" src="/resources/img/volume-2/5.4-FBA-as-a-consensus-algorithm/Figure-5.33-Example-of-a-network-with-one-common-node.png"/> 

### Decentralization level of the Stellar network
During the analysis of interactions between Stellar nodes in 2019, it became evident that there are about 35 active validators with a monthly uptime of above 90%. In addition, more than half of the quorum slices consist of 10 or less validators [73]. The first reason for the small number of validators is the lack of motivation to participate in the system in this role.

Most of currently active validator nodes are owned either by the Stellar Development Foundation itself or by partner companies. For example, IBM uses Stellar in a blockchain-based payment system, while Satoshi Pay and tempo.eu.com are developing applications based on the Stellar platform. They are motivated to keep the system operating because their services depend on Stellar. However, they do not receive any actual rewards as, for example, in proof-of-work or proof-of-stake systems.

The second reason there are so few validators is that the system is dependent on trust towards validators. According to the structure of the current quorum slices, system participants choose from a relatively small group of validators who they trust based on their influence or directly collaborate with. Due to this model of trust, the formation of quorum slices is a biased process, and it inevitably leans towards centralization. It’s also worth noting that all quorum slices have at least one Stellar Development Foundation (SDF) validator in them.

This kind of structure leads to a higher risk of system failure: in case one node is unavailable, the system continues to work normally; however, when any two SDF nodes become unavailable, a complete cascade system failure occurs, i.e. none of the validators are able to reach consensus. If the *eno* node and one of the SDF nodes stop operating, the failure will occur for 90% of the nodes.

One potential solution would be to bring the Stellar structure closer to the one used in pBFT. However, in this case, participants will have to constantly monitor messages from the selected validators and change the quorum slices structure depending on which nodes behave incorrectly. Also, participants can lower the agreement threshold in their quorum slices, which, by itself, will increase the liveness of the system but will, however, lower its security.

At the moment, the vulnerability associated with the failure of the entire system due to the failure of the two main nodes has been eliminated. Now the failure of any node pair will not lead to a total system failure. However, the structure is still fairly centralized. If any two SDF nodes and two nodes owned by the Keybase social network fail, the system will also stop operating. There are other combinations that can put the entire system out of action.

Among other things, it has been found that quorum slices of most IBM nodes consist only of SDF1, SDF2, SDF3 and have a threshold of 2. Thus, it is more difficult now to completely disable the system. Nevertheless, it is worth considering the risk that influential SDF nodes could fall under the control of any interested party or collude. The last statement is unlikely to be true since Stellar is interested to keep the network and its nodes healthy.

In the Stellar case, a security failure (such as the possibility of double-spending) is more critical than the loss of liveness (in traditional banking systems transaction delays are still way longer). Therefore, participants choose (and must choose) large quorum slices, which are more likely to be in the state of agreement than in the working state.

### FBA compared to other consensus algorithms
We reviewed the basic concepts, operating principles, and limitations of the Federated Byzantine Agreement. Now let's compare it to other common consensus algorithms to get a better understanding of the types of systems it is applicable to.

Compared to Practical Byzantine Fault Tolerance, another protocol from the BFT family, the Federated Byzantine Agreement provides a distinct advantage in scalability because it does not require nodes to exchange a lot of messages between each other. It allows building a network with a large number of participants.

However, when comparing FBA and proof-of-work, the latter has an advantage in terms of anonymity of participants, and it provides greater decentralization since everyone with an appropriate hardware can participate in the block generation process. As was described earlier in this section, Stellar, which is the most prominent example of the FBA protocol usage, has a tendency towards a centralized structure since validators prefer to include nodes in their quorum slices that they definitely trust.

## 5.5 Hashgraph
Hashgraph is both a way to organize a transaction database and a consensus algorithm. The first version of this algorithm assumes that there is a fixed number of validators and more than ⅔ of them are honest. The database organization method in hashgraph is similar to constantly branching blockchain, where each block from each chain is connected not only with the previous one but also with the blocks of other chains.

*Hashgraph* is a data structure that stores the history of data distribution between nodes (Fig. 5.34).

<img width="20%" alt="Figure 5.34 – Node’s database structure" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.34-Node's-database-structure.png"/> 

This algorithm was developed by Dr. Leemon Baird, co-founder and technical director of Swirlds in 2016, as an alternative to consensus algorithms involving a limited number of known validators and a leader. The disadvantage of leader-based algorithms is that leaders are prone to DoS attacks, and as a result, the entire system stops working until the leader is changed [49]. One of the advantages of this algorithm is that it provides the asynchronous byzantine fault tolerance, which means that its operation doesn’t depend on the time spent on message delivery. In the current version of the algorithm, the validation process is permissioned, meaning that in order to become a validator, the node needs to get permission from other validators.

### Working principle of hashgraph
In hashgraph, each validator node generates events instead of blocks. An event is an entry that a validator creates and distributes to confirm transactions. The hash graph state is then exchanged with another validator.

The lifecycle of an event is as follows:

> * Creation
> * Distribution
> * Confirmation

### Event creation
Each validator node can create a signed event at any moment of time. It can include both its own transactions, the ones it receives from other nodes or may not include any transactions at all (Table 5.3). Typically, an event does not need to include many transactions, since in a hashgraph, each validator can create a new event at any given time.

Table 5.3
<img width="50%" alt="Table 5.3" src="/resources/img/volume-2/5.5-Hashgraph/Table-5.3.png"/> 

Table 5.3 shows that each event contains links to two previous events, while each of these two previous events contain links to additional two previous relatives, and so on. Thus, when having one event, it is possible to track all of its ancestor‘s events (Fig. 5.35).

<img width="42%" alt="Figure 5.35 – Forming a new event based on A and B" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.35-Forming-a-new-event-based-on-A-and-B.png"/> 

### Event distribution
For event distribution, the *gossip protocol* is used [50]. Each node is connected to other nodes. According to the protocol, when one node receives a message from another node, it starts transmitting this message to others in a random order. So, in the first place, one node sends a message, then two, four, eight, and so on. As a result, the number of nodes that are receiving the message increases exponentially.

Suppose Alice wants to distribute her new event on the network using the gossip protocol:

> Step 1: Alice creates the event and plans to distribute it to the other nodes (Fig. 5.36).

<img width="36%" alt="Figure 5.36" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.36.png"/> 

> Step 2: Alice sends the event to a randomly selected host; in this case, Carol (Fig. 5.37).

<img width="36%" alt="Figure 5.37" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.37.png"/> 

> Step 3: Alice and Carol send this event to other randomly selected nodes, in this case, Bob and Eve (Fig. 5.38).

<img width="36%" alt="Figure 5.38" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.38.png"/> 

> Step 4: Now Alice, Bob, Carol and Eve distribute this message to randomly selected members of the network. As a result, all nodes will know about this message (Fig. 5.39).

<img width="36%" alt="Figure 5.39" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.39.png"/> 

*Gossip about gossip*: when the validator node receives the event from the other validator, it checks what events it doesn’t know using the links of the event it received. Then it requests the events it doesn’t know. Thus, by exchanging events, validator nodes synchronize their hash graph states. Also, each validator node that receives an event verifies it in accordance with its hash graph. Also, each validator that receives an event verifies it according to its hash graph. If it agrees with this event, it also creates a new event and includes the *other-parent hash* value from the received event. Then, the validator node that receives the message also distributes its last created event, the *other-parent hash* of which refers to the event received earlier. Let’s look at the general principle in the following example (Fig. 5.40), which shows the history of the gossip protocol as a hash graph:

1. Alice created the event and informed Bob about it as well as about all the events that she knows about but Bob does not know. Bob then creates his event that is related to Alice’s event as a way to confirm that he received data from Alice.
2. Bob tells Carol about his event and about all the events that he knows, but she does not know. After that, Carol creates an event related to Alice's event through Bob's event. This serves as a confirmation that she received the information from Bob. Almost at the same time, Alice and Dave perform similar actions.
3. Carol informs Dave about her event as well as about all the events that she knows about but he does not know. Next, Dave creates an event related to Alice's event through the events of Carol and Bob to prove that he received information from Carol.
4. Dave tells Alice about the events that are known to him but aren’t known to Alice. Alice creates an event related to her event and the events of Dave, Carol and Bob. Almost at the same time, Carol and Bob are doing similar things.

<img width="42%" alt="Figure 5.40 – An illustration of the gossip about the gossip protocol" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.40-An-illustration-of-the-gossip-about-the-gossip-protocol.png"/> 

Each node stores a hash graph data structure obtained by exchanging information about it with other nodes. Thus, the data structure stored in each node is constantly updated and synchronized with the others. As a result, most nodes have the same data structure. Each event is stored in memory as a byte sequence signed by the author.

### Event confirmation
Having a set of events with transactions, it is important to know which one was earlier and which was later. Therefore, events should be ordered in time, and this order should be the same for each node. The order is established by consensus achieved through timestamps for each event. Once an event has a timestamp accepted by the majority of nodes, it can be considered confirmed.

In order to reach a consensus on event timestamps, the hash graph is divided into rounds (Fig. 5.41).

<img width="20%" alt="Figure 5.41 – An illustration of a graph being divided into rounds through witness events and strong connection" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.41-An-illustration-of-a-graph-being-divided.png"/> 

A round starts and ends as soon as only one of the events (the top of the graph in Figure 5.42) is connected with ⅔ of the first events of the first round (selected events on the bottom layer of the graph) through most events of different creators (highlighted in the center). This connection is called strongly seeing. The first events of each validator node in a round are called witness events; virtual voting is also conducted in regards hereto.

<img width="65%" alt="Figure 5.42 – An example of strongly seeing" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.42-An-example-of-strongly-seeing.png"/> 

The first events in the network do not have ancestor events, and each of them is determined by the first round as a witness event. A round for each event is defined as a function of its ancestor events. Simply put, if the last witness events are below the events in the graph belongs to round R, then the events before the next witness event also belong to round R. The next witness event belongs to round R + 1.

To achieve consensus on the ordering of events for each round, each validator locally conducts virtual voting in which the median is selected from the timestamps of the neighboring events of the specific event being checked.

*Virtual voting*: each node has a copy of the hash graph, and each hash graph is ordered, meaning that if two different hash graphs A and B have the same event X, then each of them also has the same ancestors of this event with the same connections between them. Alice is able to compute Bob’s vote through a hash graph since she sees that if the event is one of the ancestor events of Bob’s event, then Bob agrees with this event and his vote is considered “vote for”. Bob does not have to vote himself. Thus, each node can reach agreement on any number of decisions, while, in fact, not sending a single message with their votes but simply locally calculating the result of the vote through its hash graph. Moreover, if Bob and Alice are honest, then Bob who does not know about Event X can quickly find out about it thanks to the distribution protocol. The consensus algorithm suggests that this will happen sooner or later but does not make any assumptions about the time required.

To determine the order of events and transactions included herein, Alice conducts round-based virtual voting. Bob and the rest of the nodes are voting the same way. In each vote, it is assumed that validator votes will be sent through some witness events in the Alice hash column and some of the witness events in the same hash column will receive them.

Bob can cheat and create two witness events *X* and *Y* with the same ancestor events, while *X* and *Y* will not be ancestor events to each other. Then there will be an event *W*, the ancestor events of which will be *X* and *Y*. Event *W* will see that *X* and *Y* are created by one node but none of them is the ancestor of the other, which means that this forking of history into two alternative event branches will be recognized. In this case, *W* will see an event branch with *X*, an event branch with *Y* or none of these branches (Fig. 5.43).

<img width="30%" alt="Figure 5.43 – An example of event W creation" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.43-An-example-of-event-W-creation.png"/> 

An event *W* is strongly related to event *X* if it is associated with it at least through ⅔ events of different nodes. As a result, it turns out that both Alice and Bob having synchronized hash graphs can calculate Carol's virtual voice regarding a specific decision and that both will get the same result.

To reach consensus on the order of events for each round, each validator node locally conducts virtual voting through unique famous witness events from famous witness events.

*Famous witness event* is a witness event associated with the majority of witness events in the next round. To identify such events, it is enough to execute the byzantine agreement protocol only for each witness event, solving only one question – whether this witness event is popular. Once agreement has been reached on several popular witness events, it is easy to reproduce the reliable order of all events from the hash graph.

A Witness event is considered popular if there is a witness event above in this hash column that is associated with the verified witness event through at least ⅔ other witness events.

Having a hash graph, you can calculate the votes of all validators through virtual voting. Each witness event located a round above the *“yes”* votes to the question of whether a particular witness event is popular if it has a verified event in its ancestor events; otherwise, it is assumed that the witness event votes *“no”*. In virtual voting, when the witness event votes *“yes”* or *“no”*, the vote is calculated as a function of its ancestor events. The result is computed through the witness event (created by the round later), which has voting witness events in the ancestor events. For example, let’s determine if Alice's first event is a famous witness event.

1. Each validator has a hash graph in which, using virtual voting, it calculates how Dave would vote on the issue whether Alice's first event is a popular witness event (Fig. 5.44).

<img width="20%" alt="Figure 5.44 –  Calculation of Dave’s vote regarding a certain event" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.44-Calculation-of-Dave’s-vote-regarding-a-certain-event.png"/> 

2. Each validator, using virtual voting, calculates how Carol would vote on whether Alice's first event is a popular witness event (Fig. 5.45).

<img width="20%" alt="Figure 5.45 – Calculation of Carol’s vote regarding a certain event" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.45-Calculation-of-Carol’s-vote-regarding-a-certain-event.png"/> 

3. Each validator, using virtual voting, calculates how Bob would vote on whether Alice’s first event is a popular witness event (Fig. 5.46).

<img width="20%" alt="Figure 5.46 –  Calculation of Bob’s vote regarding a certain event" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.46-Calculation-of-Bob’s-vote-regarding-a-certain-event.png"/> 

4. Each validator, using virtual voting, calculates how Alice would vote regarding whether Alice's first event is a popular witness event (Fig.  5.47).

<img width="20%" alt="Figure 5.47 –  Calculation of Alice’s vote regarding a certain event" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.47-Calculation-of-Alice’s-vote-regarding-a-certain-event.png"/> 

Whether Bob's first event is a popular witness event is calculated by the same principle; Similarly, the famous witness event is calculated for all witness events.

As soon as all famous witness events in the round are determined, all famous witness events with the same creator are removed until each creator of the famous witness event has one such event. The remaining famous witness events are called unique famous witness events. In the absence of forks, each famous witness event is a unique famous witness event. Through the data of unique famous witness events conformed timestamps are defined for each of their ancestor events.

Agreed timestamps are calculated as follows. Imagine that tested event *X* is in round *R*. In the same round, Alice created a unique famous witness event *A*. The algorithm finds the event *Y* that is the closest one to Alice's famous witness event, which she created in the same round and the one that an event *X* is associated with (as other-parent or as one of the nearest ancestor events). For the creator of event *X*, his event *Y* relative to his famous witness event will be event *X*. The timestamp of event *Y* is considered to be the time when the node that generated the event *Y* found out about this event. The most common timestamp for event *X* will be the median of all such *Y* events for each creator of a unique famous witness event. Consider the following example:

1. Let’s find the nearest self-parent event *Y<sub>А</sub>* for Alice’s unique famous witness event (*A*), which is connected to *X*, and save its timestamp *t<sub>А</sub> = 3* (Fig. 5.48).

<img width="20%" alt="Figure 5.48 – Creating the timestamp for Alice’s unique famous witness event" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.48-Creating-the-timestamp-for-Alice’s-unique-famous-witness-event.png"/> 

2. Let’s find the nearest self-parent event *Y<sub>B</sub>*  for Bob’s unique famous witness event (*B*), which is connected to *X*, and save its timestamp *t<sub>B</sub> = 1* (Fig. 5.49).

<img width="20%" alt="Figure 5.49 – Creating the timestamp for Bob’s unique famous witness event" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.49-Creating-the-timestamp-for-Bob’s-unique-famous-witness-event.png"/> 

3. Let’s find the nearest self-parent event *Y<sub>C</sub>*  for Carol’s unique famous witness event (*C*), which is connected to *X*, and save its timestamp *t<sub>C</sub> = 2* (Fig. 5.50).

<img width="20%" alt="Figure 5.50 – Creating the timestamp for Carol’s unique famous witness event" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.50-Creating-the-timestamp-for-Carol’s-unique-famous-witness-event.png"/> 

4. Let’s find the nearest self-parent event *Y<sub>D</sub>* for Dave’s unique famous witness event (*D*), which is connected to *X*, and save its timestamp *t<sub>D</sub> = 4* (Fig. 5.51).

<img width="20%" alt="Figure 5.51 – Creating the timestamp for Dave’s unique famous witness event" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.51-Creating-the-timestamp-for-Dave’s-unique-famous-witness-event.png"/> 

5. The agreed timestamp of event *X* is the median from *t<sub>А</sub>*, *t<sub>B</sub>*, *t<sub>C</sub>*, *t<sub>D</sub>* (Fig. 5.52).

<img width="40%" alt="Figure 5.52 – Calculating the agreed timestamp for an event" src="/resources/img/volume-2/5.5-Hashgraph/Figure-5.52-Calculating-the-agreed-timestamp-for-an-event.png"/> 

Of course, Alice and Bob will not have the same hash graphs at any given time. Their graphs will be agreed mainly according to earlier events. However, for the most recent events, each of them may know about events that others do not know about. As a result, validators will have different subsets of confirmed transactions, even though a significantly larger part of this subset will be the same. Moreover, if Alice does not know about some event from earlier events, then in the future it will still reach Alice. In this case, Alice is synchronized with the majority of the other nodes.

**Frequently asked questions**

*– Can you create rules to change the validator?*

The main problem with hashgraph is that the current version does not define rules for changing a validator set. One of the solutions to this extremely important issue may be to include votes for adding or removing participants in the validators’ messages. Thus, if most validators support this solution, a new node will be added or one of the current ones will be deleted. Also, if the validators determine that one of them behaves dishonestly, they will be able to vote to take away its validator rights.

*– For what cases does hashgraph work best?*

Hashgraph works well for cases when there is a fixed number of validators and no leader needed. For example, several companies cooperate and they need a system to record data regarding interactions among them, so each company could have equal rights and third parties cannot influence its operation.

*– Can the validation process in the hashgraph algorithm be permissionless?*

This is not possible, unless modifications are added to the algorithm. The problem is that each auditor must know the total number of validators. Therefore, the list of validators must be well-known and protected from modification, i.e., no one  should be able to change it.

[METHODS FOR ENSURING PRIVACY IN MODERN ACCOUNTING SYSTEMS](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-2/en/6-methods-for-ensuring-privacy-in-modern-accounting-systems.md)
