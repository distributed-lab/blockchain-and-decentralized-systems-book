# 4 BITCOIN AS A PLATFORM

## 4.1 Sidechains mechanism
The idea of sidechains first appeared in 2014 when the trend of improving the original Bitcoin protocol became more widely known. These attempts were usually undertaken by other projects that were creating their own currencies and which were supposed to work according to their own rules. Beyond the desire to get richer with the price growth of their currencies (which, in fact, had a serious influence on the situation), this trend undoubtedly made sense. By that time, there had already been a lot of proposals for improving the protocol and expanding its functionality, but implementing everything straight away in Bitcoin would have been extremely impractical for many reasons (including security considerations). All upgrades must first be thoroughly tested and only then implemented. But how can you conduct these tests considering that not many people would want to try and use the alternative digital currency?

In this way, community members who wanted to try new algorithms of anonymization, smart-contracts or consensus mechanism with high capacity and low fees could use existing bitcoins in the secondary accounting system with the ability to send them back to the main one. Ideas that were proposed and developed in different sidechain projects had a serious influence on the future of Bitcoin, but they still aren’t used since developers sought to create their own independent currencies.

The idea of sidechains in real life can be remotely compared to using euro or dollar in states that do not emit these currencies. In this case, their governments do not have the ability to print money but still can confirm transactions and maintain their own internal records. For example, euro is widely used in Montenegro, while dollars are used in Panama.

A *sidechain* is a separate blockchain in which transactions with coins obtained from the mainchain are added, as shown in Fig. 4.1. 

The operation rules in it can differ from the mainchain and be determined by the new protocol (for example, there may be a different consensus algorithm, different transaction structure, etc.). A sidechain does not have its own currency but implements a mechanism for transferring value from the main blockchain. Sidechains are used to implement new coin accounting rules (script expansion, smart contracts) or to test new technologies (signature algorithms, transaction models). Note that sidechains can be active or inactive. Being in the active state means that the sidechain is supported by a certain validator community. When members stop participating in decision-making and confirm transactions, the sidechain is considered inactive (Fig. 4.1).

<img width="50%" alt="Figure 4.1 – Sidechains structure" src="/resources/img/volume-2/4.1-Sidechains-mechanism/Figure-4.1-Sidechains-structure.png"/> 

The idea of sidechain was first introduced by Gregory Maxwell. The first document describing this idea was “Enabling Blockchain Innovations with Pegged Sidechains” [37] published on October 22, 2014, by a group of authors, one of which is Adam Back. Another famous paper called “Sidechains not a direct means for solving any of the scaling problems Bitcoin has” was published on June 13, 2015, by Pieter Wuille [72]. On June 14, 2015, Gregory Maxwell released his work “Security limitations of pegged chains”.

### One-way peg and two-way peg sidechains
To transfer value, sidechains can be tethered to a *mainchain* in two ways: *one-way peg* or *two-way peg*. When the *one-way peg* is used, digital assets can be transferred only in one direction – from the main accounting system’s blockchain to the secondary one. This way of asset transfer is rarely used so we will look at the second option in more detail. In the case of *two-way peg*, digital assets can be transferred back and forth.

A two-way peg can be implemented in several ways; the most common are *federated sidechains* and *merged-mined sidechains*. *Sidechains* with a *two-way peg* work as follows. A user must first send his assets to a special address in the main accounting system so they can’t be spent in the *mainchain*. After that, the user will confirm the desired transaction in the *sidechain* network. It will let nodes that process *sidechain* blocks know that certain coins in the main accounting system were frozen. This allows sidechain validators to issue an equivalent amount of coins in the secondary network and send them to the address that corresponds to the user’s public key that he used in the main accounting system. Further, the user can manage this value under the new rules.

In order to transfer digital assets from the sidechain back to the mainchain, the user must freeze them in the secondary accounting system and send proofs to the validators in the main accounting system. They check that coins are really frozen and the corresponding value is locked (or destroyed), unfreeze the equivalent amount of coins in the *mainchain* and send them to the user’s address.

Let’s say that Alice and Bob are users of the Bitcoin accounting system (Fig. 4.2). Alice owns a certain number of bitcoins and wants to send them to Bob, but for her own reasons, she can’t do this in the *mainchain*. Thus, she creates a transaction that freezes some bitcoins in the *mainchain* and another one inside the sidechain to create equal value there that she can manage. When validators in the *mainchain* confirm the freeze transaction, *sidechain* validators will receive the proof that funds are frozen and confirm the transaction in the *sidechain*.

<img width="50%" alt="Figure 4.2 – Transferring value from the mainchain to a sidechain" src="/resources/img/volume-2/4.1-Sidechains-mechanism/Figure-4.2-Transferring-value-from-the-mainchain-to-a-sidechain.png"/> 

Imagine that Alice sends coins to Bob in the *sidechain*, but Bob wants to spend them in the mainchain. For this, he creates a transaction to freeze coins in the sidechain and another transaction in the *mainchain* (that is, the Bitcoin *accounting system*) notifying about his intent to transfer coins from the *sidechain*. After the proof that *sidechain* coins were frozen is received, Bitcoin transaction is accepted and coins appear on Bob’s account balance. This is how it works in the most general case.

The biggest problem is to implement the interaction between main network and sidechain validators to successfully freeze and unfreeze assets. The solution to this problem is exactly the main difference between *federated* and *merged-mined sidechains*.

### Federated sidechains
*Federated sidechains* use *multiSig-addresses* to freeze *mainchain* assets. To do this, the sidechain creators select federation members – trustees who determine when the user’s coins are frozen and released (Fig. 4.3). They use special addresses and multi-signature based contracts for this. The reliability of this approach depends on the probability of federation members' malicious collusion.

<img width="50%" alt="Figure 4.3 – Federated sidechains structure" src="/resources/img/volume-2/4.1-Sidechains-mechanism/Figure-4.3-Federated-sidechains-structure.png"/> 

This approach does not require any changes in the *mainchain* protocol. If the *sidechain* is attacked or its operation is compromised, this will not affect the *mainchain* state; however, this will allow detecting and considering vulnerabilities of the *sidechain* protocol. A practical implementation for this approach already exists, and its *mainchain* is the Bitcoin blockchain.

### Merged-mined sidechains
A *merged-mined sidechain* uses *merged mining* technology that allows using the same PoW task solution to generate blocks in both main and side blockchains. The operational principle of this technology has the following features. In the process of new blocks generation, the validator first forms a block for an alternative chain that uses *merged mining*. This block’s title is placed in its new mainchain block. Then the *mainchain* solves the proof-of-work task for the new block. If the validator finds a solution for the task in the *mainchain*, this block is distributed among the *mainchain* nodes. Other *mainchain* participants ignore the block header for the *sidechain*. If the found solution satisfies the sidechain difficulty, then the header for a new *mainchain* block and SPV-proof are specified in the new *sidechain* block. Such PoW verification is done by the *sidechain* rules that provide for the option to accept the puzzle solution for the *mainchain* if the hashing algorithm is the same and the solution satisfies the *sidechain* difficulty.

To implement this approach in the *mainchain* scripting language, an operation similar to OP_COUNT_ACKS, as in Bitcoin, is required. This operation involves the verification of some additional conditions that come from the external accounting system to unfreeze *mainchain* assets. This works by the merged mining data going from the *sidechain* to the *mainchain* block, and *mainchain* validators checking the corresponding data according to the *mainchain* protocol rules. If coins can be spent, they are unfrozen in the main accounting system. The reliability of this approach depends on the participants who own the majority of computing power. To add this operation into the main accounting system protocol, the softfork is needed (Fig. 4.4).

<img width="50%" alt="Figure 4.4 – The structure of merged-mined sidechains" src="/resources/img/volume-2/4.1-Sidechains-mechanism/Figure-4.4-The-structure-of-merged-mined-sidechains.png"/> 

One of essential *sidechain* components is the SPV method (described in more detail in volume 1 of the textbook and in section 2.3), which allows verifying whether certain transactions are confirmed without downloading the entire blockchain.

Validators of the side accounting system need Bitcoin SPV nodes to check that Bitcoin coins can be locked. Validators make a decision to unlock the corresponding amount of coins in the *sidechain* based on this data. Remember, a simplified verification assumes that a list of block headers exist that confirm that proof-of-work was done and that the unspent output was created in one of the blocks in this list. But in order to work in the *sidechain*, *SPV-proof* must be small enough to fit into one *coinbase transaction*. Any validator with SPV-proofs can make sure that the transaction was confirmed without the need to verify each block again.

Nowadays, projects that use *sidechain* already exist. For example, Blockstream developed its sidechain for the project Liquid. Its goal is to have faster bitcoin transfers between exchanges. Rootstock developed a sidechain that uses *merged mining* to manage bitcoins with smart contracts written in Solidity.

Thus, a *sidechain* allows developers not only to test new implementations or protocol changes before activating them but also to enable new tools for working with an existing currency. There is a perception that with reliable *two-way pegs*, the sidechain technology can significantly reduce the mainnet and allow maintaining the majority of users within accounting systems that are connected with the main one and work alongside each other. If so, this technology can solve the problem of excessive load by processing certain types of transactions in separate accounting systems and preserving the basic principles of decentralization.

### Sidechains disadvantages
To implement the reliable network-level interaction, it is quite challenging to have several decentralized accounting systems that support transactions between each other. They must support transactional scripts, which may later become invalid due to a subsequent change of proofs. This entails the need to have software that can detect potential attacks and block them. It should also be considered that the wallet software must be configured to support several blockchains.

In the case of a two-way peg sidechain that uses SPV-proofs, transferring value from the main accounting system to the secondary one and vice versa is less secure compared to payments within the same accounting system. The problem is that the recipient has confirmation from the validator who only checked the SPV-proof. As for the value itself, coins can be sent at any ratio and with any coefficient (for example, 1:1, 1:10 or 1:100).

### Summary
Applying the sidechain approach allows adding alternative rules for the accounting of existing coins. The disadvantage is that in order to use alternative accounting rules, the user generally needs to support two full nodes: one for the main accounting system and the second for the additional one.

Nevertheless, the value of the sidechain approach is that it offers a mechanism for introducing new technologies without exposing the main accounting system to risks. Most likely, in the future, it will help to find a solution to many issues, particularly insufficient capacity of the accounting system.

## 4.2 Lightning Network design
In the previous volume of this textbook, we first looked at a concept called Lightning Network. Now we will examine in more detail how this concept works as well as how a  trustless operation is realized.

### Bidirectional payment channel structure
To explain the Lightning Network (LN), a preliminary understanding of bidirectional payment channel structure is necessary. In fact, the LN is formed by switching these channels together.

The main advantage of bidirectional channels is that participants can transfer coins in both directions without the need to trust each other and broadcast intermediate transactions into the network. If one of the parties tries to outwit, then according to the protocol he loses all his coins. Noteworthy, this does not require any intervention by a third party: a protocol based on cryptographic proofs allows penalizing an adversary.

In case this kind of payment channel is used, interacting parties can exchange coins with each other an unlimited number of times. Thus, it will be necessary to publish only two transactions to the network: funding, the one that locks coins in the network and opens a channel, and refunding, which returns coins to the parties based on the result of interaction between them. In this case, there can be 3 scenarios for unlocking channel participants’ coins. We will discuss them further.

In general, the scheme of work of payment channel can be represented as in Figure 4.5.

<img width="50%" alt="Figure 4.5 – Bidirectional payment channel scheme" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.5-Bidirectional-payment-channel-scheme.png"/> 

> **_Attention!_** *It looks quite unwieldy, complicated and frightening; however,  below we will look at  each step of the interaction and describe what actually happens in detail. By the end of this chapter the reader will completely understand how bidirectional payment channels in the Lightning Network work.*

### Opening a payment channel
To open a payment channel, Alice and Bob contact each other (this interaction happens within the network of lightning-nodes) and exchange their public keys to create a multisig address. The parties also exchange links to unspent outputs with coins that they want to block in a joint channel. Then, one of the parties initiates  a transaction, the inputs of which refer to the mentioned unspent outputs. The generated multisig address is added to the output of a new transaction. In our example, Alice and Bob initially have 5 BTC each, and the transaction output has 10 BTC (fig. 4.6).

<img width="40%" alt="Figure 4.6 – Channel opening and transaction generation scheme" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.6-Channel-opening-and-transaction-generation-scheme.png"/> 

This is a funding transaction that the parties must send to the network to open a payment channel. In this case, the transaction must be signed by the owners of both inputs. This means that each side proves that it owns the input and reports that it is ready to spend coins on the specified multisig address.

> **_Note:_** *We have to highlight one important aspect: after this transaction was confirmed, if one of the parties stops interacting (doesn’t sign the coin spending transaction from the shared address for some reason), then both parties lose their coins. However, this behavior will be disadvantageous for both parties since they can’t steal another person’s coins and will lose their own.*

Before Segregated Witness was implemented, this problem could be solved only by creating additional conditions: if coins are not spent from the multisig address, then after some time parties can unlock them by using their private keys. However, this approach has two disadvantages: first, it limits the channel lifetime, and secondly, the parties must wait a long time for coins to be available (the payment channel is usually opened for a long period of time).

Segregated Witness allowed to solve this issue in a more elegant manner by eliminating transaction malleability (segwit allows going without the signature value in tx_id, through which the chain of unconfirmed transactions can be built). Now, parties can create a multisig transaction but not sign it until transactions are created that send back coins to their owners that are put on hold in the channel.

Thus, Alice and Bob need to create the first transaction in the channel that will pay coins back to their owners. Before that, each party has to generate a random secret and send its hash value to the other (fig. 4.7).

<img width="40%" alt="Figure 4.7 – Secrets generation and hash values exchange scheme" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.7-Secrets-generation-and-hash-values-exchange-scheme.png"/> 

After that, Alice and Bob create one transaction each (fig. 4.8). These transactions will spend coins from the multisig addresses in the following way:

1. Alice creates a transaction with two inputs: the first one has 5 coins and can be spent with her private key and the second one has the remaining 5 coins and two mutually exclusive spending conditions: either Bob can spend them using his private key after some time passes (for example, 100 blocks since this transaction was confirmed) or Alice can spend them immediately using her private key and the secret that Bob generated (since Bob’s hash value is included in the condition).
2. Bob forms a transaction with the same output amounts but with the opposite spending conditions: Bob can spend the first output using his private key and the second output under two mutually exclusive conditions: either Alice can spend them using her private key after the 100 blocks or Bob can spend them using his private key and Alice’s secret.

<img width="50%" alt="Figure 4.8 – Commitment transaction generation and exchange" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.8-Commitment-transaction-generation-and-exchange.png"/> 

After that, each channel participant signs a transaction (adds one of the two necessary signatures), which he or she has created and sends it to the other party. Then, each participant checks the transaction received from its counterparty. If everything is ok at that point, the payment channel is cleared for internal payments and the first transaction paying on the multisig address (the first transaction created) can be sent. Since for a refunding transaction become valid it should be signed by both sides, the channel can be closed at this stage, provided the second party signs the transaction and publishes it to the network.

If Alice completes the signature and publishes the transaction that she received from Bob, the first output will send 5 coins to Bob instantly and the second output  will send 5 coins to Alice however, only upon the confirmation of 100 blocks since the initial transaction (Bob will not be able to get coins from the second output because he doesn’t know Alice’s secret).

In case Bob signs and publishes the transaction he received from Alice, then it is the same scenario vice versa: Alice will immediately receive her 5 coins, and Bob will receive his 100 blocks later.

As a result, if at least one party will now publish the transaction, coins will be sent to their owners and the channel will be closed. As we have discussed, no one can cheat and steal coins of their counterparty (it can be done only if one of the parties publishes the transaction to the network and then also discloses its own secret).

### Transferring coins within the channel
After the channel was opened and transactions were created that return coins to users, Alice and Bob can start transferring coins within the channel. Let's look at how this works.

The first step is to generate a new secret and transfer its hash value to the counterparty (as in the case with forming the first transaction in the channel).

Then each party creates a new transaction in the channel but with a different distribution of coins between the two. For example, Alice decided to pay 2 BTC to Bob. In this case, after they exchanged the hash value of the generated secret, they form the following transactions:

1. Alice forms a transaction, the first output of which transfers 3 coins to Alice's address, and the second output again contains two conditions for spending 7 remaining coins: either to Bob after a certain time or to Alice but only if she knows Bob‘s secret at the second stage.
2. Bob creates the same transaction, where in the first output, 7 coins are sent to Bob, and in the second output, 3 coins are transferred to Alice, after 100 blocks (alternatively the coins are transferred to Bob if he finds out Alice's secret) (fig. 4.9).

<img width="50%" alt="Figure 4.9 – Creating and exchanging alternative commitment transactions" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.9-Creating-and-exchanging-alternative-commitment-transactions.png"/> 

After these transactions are created, channel members exchange them. Then, any of these transactions can be signed and sent to the network to be confirmed and the channel is closed. Thus, parties can execute an infinite number of payments with one another within a channel by creating a new secret and creating a new transaction each time.

Note that after exchanging these transactions, the previous state of refunding transactions in the channel can also be published. Thus, at any moment of time, parties have two ways to close the channel. To ensure the irreversibility of the channel state (only the last state is considered valid), parties exchange secrets they generated at the first stage. Currently, if counterparties get these values, they can’t cheat but can initially get all the coins of the party that would try to outwit. Therefore, Alice gives Bob the first generated secret, and Bob gives Alice his in return. Then each of them checks whether this secret corresponds to the hash value added to the first channel transaction.

### The punishment mechanism for cheating in the channel
This type of bidirectional payment channels that the party that is trying to cheat loses all its coins in the channel. Now we will look at how this mechanism works.

For example, take a situation that was considered earlier: Alice and Bob placed 5 coins in a payment channel, and then created transactions that return coins to their addresses (5 each). Then Alice made a 2 coin payment so the distribution in the channel has changed. Let’s imagine this interaction stage using the previously introduced scheme (fig. 4.10).

<img width="50%" alt="Figure 4.10 – Opening the payment channel and executing the first payment" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.10-Opening-the-payment-channel-and-executing-the-first-payment.png"/> 

The final distribution transaction implies that after it closes the channel, 3 coins will go to Alice, and 7 to Bob. What now prevents Alice from sending the first transaction into the channel, upon which she will receive 5 coins?

In fact, one of two transactions can be signed in addition and sent to the network in order to distribute coins equally between parties: either from Bob or from Alice. It makes no sense for Bob to finish the signature and send the transaction that he stores since it is disadvantageous for him (furthermore, if he does this, he will lose all his coins in the channel). To distribute coins equally, Alice can sign and send the transaction shown in figure 4.11:

<img width="40%" alt="Figure 4.11 – Commitment transaction that Alice can publish" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.11-Commitment-transaction-that-Alice-can-publish.png"/> 

Now let’s look at what happens if this transaction is sent to the network. As soon as it gets confirmed, Bob immediately receives access to coins on the first output. The second output can only be unlocked if Alice waits for the confirmation of the next 100 blocks or if Bob finds out Alice’s secret that was created in the first stage. This is the underlying  protection mechanism against cheating: Alice publishes her secret after the parties agreed on a new channel distribution. Since the channel already has a transaction that sends 3 coins to Alice and 7 to Bob, Alice already published her private value that can be used to get access to the remaining 5 coins of the first transaction. Since Bob knows the secret value, he uses it together with his private key to retrieve the remaining 5 coins. As a result, he takes all the coins in the channel.

Therefore, after the parties have agreed on a new state of value distribution in a channel, the previous state cannot be published. Otherwise the party signing and sending the previous transaction to the network will simply lose all its coins in the channel.

### Closing the payment channel
We examined how the payment channel is created, how payments are executed and how the mechanism to protect against cheating works. Now, let’s look at how to safely close an active channel.

Once more, let's say we have the state of the last transactions in the channel (from Alice and Bob) that transfer 3 coins to Alice and 7 to Bob. The channel in this case can be closed in three ways (fig. 4.12).

<img width="50%" alt="Figure 4.12 – Payment channel closing scheme" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.12-Payment-channel-closing-scheme.png"/> 

The first way to close the channel is for Alice to finish the signature and publish her last transaction to the channel. Let's look at this transaction and what will happen after it gets published (fig. 4.13).

<img width="40%" alt="Figure 4.13 – Commitment transaction published by Alice" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.13-Commitment-transaction-published-by-Alice.png"/> 

After this transaction will be accepted in the network, Bob immediately gets access to his 7 coin output. The second output is available to Alice after 100 confirmed blocks or to Bob if he knows Alice's secret. However, in this case, since this is the last transaction in the channel, Alice didn’t disclose her secret and Bob can’t get coins of the respective output. The only drawback of this approach is the long period of time that Alice has to wait to be able to use her coins.

The second way to close the channel is similar to the first one, but it assumes that not Alice but Bob will send the refunding transaction (fig. 4.14). In this case, Alice’s output immediately becomes available to her and Bob will be able to collect his coins after 100 confirmed blocks.

<img width="40%" alt="Figure 4.14 – Commitment transaction published by Bob" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.14-Commitment-transaction-published-by-Bob.png"/> 

However, there is a third way to close the channel when both parties immediately gain access to their coins. To do this, they collectively form a transaction that spends coins from a multisig address on two outputs, one belonging only to Alice and the other only to Bob. After they sign this transaction and publish it to the network, they immediately gain access to their outputs.

### How does Lightning Network use payment channels?
We fully understood how bidirectional payment channels work. Now, it’s time to explain how this approach is used in the Lightning Network.

To understand why the Lightning Network concept is necessary, let’s consider the situation when Alice wants to transfer coins to Carol, but they don’t have an open payment channel. The easiest way is to open one, but it can be ineffective for a number of reasons (for example, it’s a one-time micropayment and Alice doesn’t want to pay the fee for a regular transaction or to open and then close the channel).

However, it suddenly turns out that Alice and Carol have open channels with Bob. They can use these channels and actually transfer coins through Bob (fig. 4.15).

<img width="40%" alt="Figure 4.15 – Transferring coins through several channels" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.15-Transferring-coins-through-several-channels.png"/> 

The main goal here is to ensure that Bob can’t steal coins while they pass through his channels. The Lightning Network solves this problem by switching and modifying the mechanism of bidirectional payment channels. Let’s look at how it works.

As in the previous case, let's introduce the full scheme in Figure 4.16 and then look closely at all its components to get a complete picture of how the Lightning Network works.

<img width="50%" alt="Figure 4.16 – Lightning Network operation scheme" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.16-Lightning-Network-operation-scheme.png"/> 

It looks even more complicated than the structure of payment channels, but trust us, if you understood the material above, you will have no problems with the structure of the Lightning Network.

In order to be able to transfer coins from Alice to Carol, the necessary payment channels between Alice and Bob, as well as between Bob and Carol, must exist. Therefore, the first step is to create these channels (note that Figure 4.17 shows the process of channel creation at one point in time; in fact, these channels can be created long before the actual payment; also note that the process of creating one channel doesn’t influence any other channel).

<img width="50%" alt="Figure 4.17 – Opening two independent channels" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.17-Opening-two-independent-channels.png"/> 

In summary, what actually happens is the creation of bidirectional payment channels — as we described earlier. Initially, multisig addresses are created to which parties send coins through an on-chain transaction. Then parties generate secrets among themselves and exchange hash values of these secrets. When this is done, refunding transactions are formed. They distribute coins from multisig addresses to the counterparties’ addresses. Each of these transactions is signed by the party that generated it and transferred to the counterparty. As a result of this interaction (publishing a transaction on multisig), two channels are created: one between Alice and Bob, and the other between Bob and Carol. Well, nothing new so far, and now parties can make payments within the established channels.

Now let’s look at how the coin transfer through an intermediary occurs between these two channels. If Alice, on the one hand, simply transfers coins within her channel, she has to be sure that Bob, in turn, will send these coins to Carol through their channel. On the other hand, if Bob transfers coins to Carol first, then he must be sure that Alice will return the necessary amount of coins within their own channel. Evidently, each of these approaches requires trust at least on one side.

The Lightning Network allows executing a truly trustless translation. To do this Carol — the coin recipient — initially generates a new secret and passes its hash value to Alice (fig. 4.18). At the same time, she asks Alice to transfer coins to Bob only if he proves that he knows the secret that was generated by Carol.

<img width="50%" alt="Figure 4.18 – Transfer of the hash value from the sender to recipient" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.18-Transfer-of-the-hash-value-from-the-sender-to-recipient.png"/> 

After Alice receives the hash value of the secret, Bob and her collectively change the state of the refunding transactions in their channel. To do this, they generate new secrets and exchange their hash values. Next, each of them creates a new transaction, signs it and transfers it to the counterparty as shown in Figure 4.19.

Alice’s transaction states that after it was confirmed:

1. The first output immediately transfers 4 coins to Alice's address.
2. The second output of 5 coins can be spent either by Bob after 100 blocks since the transaction confirmation or by Alice if she finds out Bob's secret.
3. A third exit is added to one coin. It can be spent in three ways: a) Bob can spend it after 100 blocks and if he knows the secret that Carol generated for the transfer, b) by Alice if she knows the secret Bob generated before the transaction formation, and c) Alice can spend it after a certain time has passed since the transaction was confirmed (for example, let’s say that this value is equal to 24 hours).

The transaction that Bob formed is similar to Alice's transaction, except for the following differences:

1. The first output pays 5 coins to Bob's address.
2. The second output for 4 coins can be spent by Alice after 100 blocks or by Bob if he finds out Alice’s secret.
3. The third output also contains three conditions for spending coins that apply a) to Bob if he knows the secret generated by Carol, b) to Bob if he knows Alice's secret and c) to Alice after 24 hours (that includes a timelock of 100 blocks).

<img width="40%" alt="Figure 4.19 – Exchange of new refunding transactions" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.19-Exchange-of-new-refunding-transactions.png"/> 

After each transaction was transferred to the other side and the secrets that invalidate the previous state were published, the parties can publish the transactions to the network and thereby close the channel. If Bob finishes the signature of the transaction he received and publishes it to the network (Fig. 4.20), Alice can spend the first output right away. Since Alice doesn’t know the secret that Bob generated, she will get access to the second output only after 100 confirmed blocks. If Bob finds out the secret that Carol generated after 100 blocks, he will gain access to the third output. If this secret is not revealed, then Alice will receive her coins after a certain period of time.

<img width="50%" alt="Figure 4.20 – Commitment transaction that Bob can send" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.20-Commitment-transaction-that-Bob-can-send.png"/> 

If Alice confirms the receipt of the transaction and sends it to the network, Bob immediately gains access to the first output (5 coins). Alice will be able to unlock the second output after 100 blocks (since Bob doesn’t know the secret Alice generated, he cannot gain access to the second output). If Bob revealed Carol’s value (he could only do this by giving her the coins — later on we will look at how it works) then he can take coins from the 3rd output. Otherwise, coins will become available for Alice after 24 hours (100 blocks included). If Alice doesn’t cheat, Bob won’t know her secret and won’t be able to take coins from the third output (fig. 4.21).

<img width="40%" alt="Figure 4.21 – Commitment transaction that Alice can send" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.21-Commitment-transaction-that-Alice-can-send.png"/> 

Despite the fact that these transactions can be confirmed on the network immediately, it is disadvantageous for the parties to do this, since the transfer from Alice to Carol would not complete and the channel between her and Bob would be closed. These actions were necessary to “lock” one coin in the channel that may go to Bob if he finds out a secret. 

Now, let’s look at how he finds out this secret. Assume, Bob and Carol are interacting. They also update their own refunding transactions in the channel. Thus, they first generate new secrets and create new refunding transactions using the received hash values of the secrets.

The transaction that Bob generates contains the following:

1. The first output transfers 4 coins to Bob's address.
2. The second output can be obtained either by Carol after 100 blocks or by Bob if he finds out Carol’s secret value that was generated to change the channel state.
3. One coin from the third output can be allocated in one of three ways: either to Carol’s address after 100 blocks if she publishes the secret value or to Bob if he finds out Carol’s secret (that is used to change the channel state) or to Bob after some time passes (say 12 hours; this interval has to be less than the one for Alice to get access to coins in the channel).

This is what Carol’s transaction looks like:

1. The first output transfers 5 coins to Carol's address.
2. The second output can be accessed either by Bob after 100 additional blocks are generated or by Carol if she finds out the secret value that Bob generated to change the channel state.
3. The coin in the third output can be taken either by Carol if she publishes the secret value that she generated to receive coins, or by Carol if she discloses Bob’s secret value, or by Bob after 100 blocks pass/the locktime value is reached (fig. 4.22).

<img width="40%" alt="Figure 4.22 – Carol and Bob exchange refunding transactions" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.22-Carol-and-Bob-exchange-refunding-transactions.png"/> 

After these transactions are formed, Carol and Bob sign them and send to one another. Let's look at what happens if one of them tries to publish this state of the refunding transaction to the Bitcoin network.

If Carol finishes the signature and publishes the transaction she received from Bob, the coins from the first output will go to Bob's address immediately. The second output for 5 coins will be available to Carol after the blocking time has ended (Bob will not be able to retrieve the coins since he does not know Carol’s secret value). 

The most interesting part is the third output. The first way to unlock it, is if Carol publishes her secret to which 1 coin is pegged after the locking time has passed. Actually, it is taken into account that Carol didn’t confirm Alice‘s payment. Finally, each channel participant will get their coins and won’t be able to take away someone else’s coins (fig. 4.23).

<img width="50%" alt="Figure 4.23 – Commitment that Carol can send" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.23-Commitment-that-Carol-can-send.png"/> 

If Carol still publishes the secret value on time and gets one coin, Bob can use this secret to take coins in the channel between him and Alice (we described this process earlier when we were talking about transactions in Bob’s and Alice’s channel).

What would happen if not Carol but Bob sends the transaction to the network? In this case, the first output for 5 coins can be spent only by Carol. The second output can be spent by Bob after the locking time ends (or by Carol if Bob tries to cheat). The third output (Fig. 4.24) can be spent by Carol if she discloses her secret or by Bob after the locking time ends (or, by Carol if Bob tries to cheat).

<img width="40%" alt="Figure 4.24 – Commitment transaction that Bob can send" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.24-Commitment-transaction-that-Bob-can-send.png"/> 

As in the previous case (when Carol publishes the transaction), Bob can take away the coin from the channel between him and Alice if Carol discloses her secret. If the secret isn’t published within the specified timeframe, the  parties will have their coins returned.

An important feature is that if Carol wants to get her coins when a secret is disclosed, she does not need to publish a refunding transaction to the network. She can simply tell the secret to Bob who, in turn, will send it to Alice so the parties can agree and create new refunding transactions in the channel (fig. 4.25).

<img width="40%" alt="Figure 4.25 – New coin distribution in channels after the payment was executed" src="/resources/img/volume-2/4.2-Lightning-Network-design/Figure-4.25-New-coin-distribution-in-channels-after-the-payment-was-executed.png"/> 

### Advantages and disadvantages of Lightning Network
After we examined the principles of LN, we can highlight some key advantages of this technology, which are the following:

> * Solving the capacity problem in accounting systems
> * Privacy of channel participants
> * Saving transaction confirmation fees
> * Preventing fraud attempts

The main advantage of LN is an increased capacity of accounting systems. Actually, channel capacity is not limited and depends entirely on the way users communicate. Users can send payments to each other every second (and even every millisecond) and do not have to wait for validators to confirm transactions, except for funding and refunding transactions.

The second advantage is the confidentiality of transfers between channel users. The details of all transactions are visible only to the involved parties. If you use only the payment channel, then network validators only see the details of funding and refunding transactions.

Details of all other transactions can be hidden. If you use the Lightning Network to transfer coins to other parties, then all intermediaries will know about the transactions. At the same time, Bitcoin nodes do not process these transactions and do not know their details (provided none of the intermediaries disclose transaction details).

The third advantage is savings on fees. Channel members only pay fees two times: to open the channel and to close it. Other transactions can even be free (in the case of Lightning Network, fees can be minimal and only go to intermediaries to change the channel state).

The last important advantage of Lightning Network is the prevention of potential fraud. If one of the parties decides to act fraudulently, she may lose all her coins in the channel.

However, Lightning Network payments have some disadvantages compared to on-chain transactions. These disadvantages are user-related, who, in addition to maintaining the software of a complete Bitcoin network node (or using a trusted one) also needs to support the LN node.

Some challenges associated with that are:

> * Difficulties with backing up and transferring the wallet to another device
> * User wallet must work permanently as it is necessary to monitor on-chain transaction to track channel states
> * Cold storage wallets cannot be used

**Frequently asked questions**

*— How many users does Lightning Network have?*

As of September 2019, the Lightning Network has about 4,500 nodes with more than 32,000 open channels. The number of locked bitcoins for the Lightning Network operation equals about 850 BTC.

## 4.3 Operation principles and use of atomic swap
By definition, a cryptocurrency owner can (and this is probably something he is motivated to do) operate with his coins without a trusted party, namely, in a trustless way. In reality, this property is often ignored since users entrust key management to a centralized server and use centralized exchanges to exchange cryptocurrencies, etc. Imagine this situation when you bought a new electric car: fast, comfortable, and energy-efficient. 

However, instead of taking advantage of its features, you harness a  horse to the car. That sounds pretty absurd, doesn't it? The same situation occurs when a user who owns crypto coins entrusts their storage to some service. You have to agree that in this case some of the main cryptocurrency properties will disappear, and what is even worse is that the user will no longer own these coins turning into the owner of digital obligations: centralized, prone to censorship, non-anonymous, etc.

It is natural that many conscious users still enjoy the properties of cryptocurrencies as they were intended to. Such users want to directly manage coins in a trustless way. Traditional centralized exchanges cannot meet these requirements. That’s why it became necessary to have a tool that allows you to maintain cryptocurrency properties while exchanging coins of different accounting systems. This tool is an atomic swap.

This section will describe the principles of an atomic swap, the relevance of this mechanism and what requirements digital currencies have to meet in order to work. In addition, we will talk about decentralized exchange technology and consider its main advantages and disadvantages.

### How do centralized exchanges work?
A centralized approach assumes that users must entrust their money to the service. In this case, the user transfers coins to the balance of the exchange, which, in turn, must fulfill its obligations to return the equivalent amount of money in the other currency. In any case, all this time the money is under the control of an exchange, and the user only has its obligations to return the funds. It is also assumed that the centralized exchange does not make transfers but only rewrites the balances of its users (fig. 4.26).

<img width="50%" alt="Figure 4.26 – Centralized exchange operation scheme" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.26-Centralized-exchange-operation-scheme.png"/> 

To perform the exchange, Alice creates an account in the exchange service and places an order to buy a certain amount of crypto coins. Bob, who already has the account, sees her order and, if he is satisfied with the price, accepts it. Bob’s and Alice’s account balances are changed and they can withdraw the funds to their wallets.

> **_Note:_** *an order describes the reservation to buy a digital asset. It contains the amount of coins to be bought, the currency used to buy them and the desired price.*

In this situation, both Alice and Bob trust the exchange. More precisely, they trust that the exchange staff will fulfill their obligations under any circumstances instead of taking the money and disappearing.

Circumstances also include engineers who have designed and developed the exchange, as well as professionals who provide fraud protection. The latter is especially relevant since hackers do not need to spend a lot of time and effort to attack individual users (and get little to no profit from it) when there are centralized exchanges that store coins of thousands of users in one place (why steal from one person when you can rob a bank). Hence, such attacks are very frequent and sometimes quite successful (fig. 4.27).

<img width="40%" alt="Figure 4.27 – The history of stolen coins from centralized exchanges" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.27-The-history-of-stolen-coins-from-centralized-exchanges.png"/> 

Another noteworthy event once happened to a centralized exchange Quadriga CX. The problem is that all user coins (about $200 million) were stored in cold wallets, access to which was lost due to the death of the chief executive officer of this exchange. This situation proves that the risk of centralized storage losing coins is quite high, and it results in a lot of people suffering from the consequences.

### The idea of atomic swaps and accounting system requirements
*Atomic swaps* offer the concept of digital assets being exchanged among each other in such a way that the exchange process will be conducted either completely or will not happen at all. This approach allows you to perform an exchange even if users do not trust each other and at the same time don’t want to have an intermediary.

In order for two different accounting systems to successfully support atomic swaps between one another, they must meet some fundamental requirements.

> **The requirements to support atomic swaps**
>> * The option to create timelock contracts
>> * The support of the same hash function to lock coins
>> * The existence of off-chain channels between participants of the exchange

The main requirement is the ability to create a locking smart contract that prevents exchange participants from taking all the money for themselves (we will look at how it works later).

Besides, to complete a transaction between two different accounting systems, both of them have to use the same cryptographic hash function while setting coin spending conditions (for example, SHA-256). This is required for the contract to be executed correctly when the user provides the result of the hash function execution.

Also, to successfully perform atomic swaps, it is necessary to have an off-chain communication channel that parties use to discuss the exchange conditions.

### Atomic swaps operation principles
Let's take a closer look at how atomic swaps work. Suppose Alice and Bob want to exchange coins, for example, 2 BTC at 44 LTC. First, they need to make sure that atomic swaps are possible between Bitcoin and Litecoin. Since both systems support time-locked contracts and use the same hash function (let’s take SHA-256 as an example), such kind of exchange is possible.

First, Alice and Bob generate the addresses to which they want to receive coins and exchange these addresses (Fig. 4.28). By doing so, they also agree on the exchange terms. Note that at this stage, they interact off-chain.

<img width="50%" alt="Figure 4.28 – Participants exchange their addresses in accounting systems" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.28-Participants-exchange-their-addresses-in-accounting-systems.png"/> 

Then transactions which send the value to the counterparty are formed. First, Alice generates a random number x and retrieves its hash value. Then she forms transaction #1, in which she sends 2 BTC and at the same time sets two conditions for how these coins can be spent.

The first condition is that Bob can take these coins if he provides a number which hash value matches the one agreed upon (this number will be the same that Alice chose). The second condition states that Alice can take all the coins back only if signatures of both parties are present. A scheme of a generated transaction is similar to the one shown in Figure 4.29.

<img width="40%" alt="Figure 4.29 – Alice’s transaction scheme" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.29-Alice’s-transaction-scheme.png"/> 

Note that the transaction was formed but Alice has not yet signed it, thus still doesn’t publish it to the Bitcoin network. After the transaction is formed, it is sent to Bob so that he can look at its content. Bob will not be able to send this transaction to the Bitcoin network since it is not signed by Alice and cannot be confirmed.

Alice creates transaction #2 that actually spends the first transaction. However, she also sets a timelock, which means that the transaction cannot be confirmed within the next 24 hours. Remember that this transaction requires the multi-signature of Alice and Bob to confirm it. Therefore, Alice signs it and sends to Bob. Bob checks the correctness of the received transaction, signs it and sends it back to Alice. Schematically, the contents of this transaction are shown in Figure 4.30.

<img width="50%" alt="Figure 4.30 – Alice’s second transaction (it sends coins back after 24 hours)" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.30-Alice’s-second-transaction-(it-sends-coins-back-after-24-hours).png"/> 

After transaction #2 was formed and signed by both participants, it can be confirmed within 24 hours and the output of transaction #1 can be spent. Since all conditions are set, Alice signs transaction #1 (the one that transfers coins to Bob if he knows the secret value) and publishes it to the Bitcoin network (fig. 4.31).

<img width="50%" alt="Figure 4.31 – Alice is publishing her transaction to the network" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.31-Alice-is-publishing-her-transaction-to-the-network.png"/> 

According to transaction terms, Bob can spend coins within 24 hours after Alice's published it in the Bitcoin network if he finds out the corresponding secret value. What does Bob need to do to find out this value? To do this, he forms transaction #3, which spends his 44 LTC and also sets two conditions for spending coins (Fig. 4.32).

<img width="40%" alt="Figure 4.32 – Bob’s first transaction scheme" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.32-Bob’s-first-transaction-scheme.png"/> 

The first condition states that Alice can take these coins if she provides a value, the hash value of which is equal to the hash value that is set in Bob’s transaction output. As you may have guessed, there Bob put the same hash value that Alice set in her transaction. By doing that, Bob asks Alice to prove that she knows the value that she generated. The second condition states that Bob can take the coins back but only if both parties sign the transaction.

Bob also does not sign this transaction and only sends it to Alice to review the content. After that, Bob forms transaction #4 that spends funds from transaction #3 and sends coins to him, but only after 12 hours have passed. This transaction must be signed by both parties. Then Bob signs the transaction and sends it to Alice. Alice checks its contents, signs it with her own key, and sends it back to Bob. The transaction structure is shown in Figure 4.33.

<img width="40%" alt="Figure 4.33 – The second transaction Bob has formed (sends coins back after 24 hours)" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.33-The-second-transaction-Bob-has-formed-(sends-coins-back-after-24-hours).png"/> 

After transaction #4 is formed, Bob signs and sends transaction #3 to the Litecoin network.

Further on, the exchange depends directly on Alice. If she did not change her mind, she needs to prove that she owns coins at the output of Bob's transaction using a secret value. After she publishes a new transaction (this time to the Litecoin network), in which she proves the ownership of coins, the secret value is revealed and Bob can use it for his own proof and take the coins (fig. 4.34).

<img width="50%" alt="Figure 4.34 – Alice publishes the secret and performs trustless exchange" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.34-Alice-publishes-the-secret-and-performs-trustless-exchange.png"/> 

If Alice changed her mind and does not want to conduct the exchange anymore, then she doesn’t publish the transaction with a secret value, and after the time passes, she gets her coins back.
To better understand the mechanics, let's look at the sequence diagram that describes the main interaction steps (fig. 4.35).

<img width="40%" alt="Figure 4.35 – Atomic swap scheme" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.35-Atomic-swap-scheme.png"/> 

> Step 1. Alice generates a random value of x (secret).

> Step 2. Alice creates transaction #1 with two conditions for coin spending: 1) by Bob if he knows the secret; 2) by Alice if they both sign the next transaction. Then the transaction is sent to Bob so that he agrees to the specified conditions.

> Step 3. Alice creates transaction #2 that spends transaction #1 and sends coins to Alice after 24 hours.

> Step 4. Alice signs transaction #2 and sends it to Bob.

> Step 5. Bob checks transaction #2, signs it with his own key and returns it to Alice.

> Step 6. Alice publishes transaction #1 to the Bitcoin network.

> Step 7. Bob creates transaction #3 with two conditions for spending coins:1) by Alice if she knows the secret; 2) by Bob if they both sign the next transaction. Then the transaction is sent to Alice so she can review it and agree on its conditions.

> Step 8. Bob creates transaction #4, which spends transaction #3 and returns coins to Bob after 12 hours.

> Step 9. Bob signs Transaction #4 and sends it to Alice.

> Step 10. Alice checks the contents of transaction #4, signs it with her own key and returns it to Bob.

> Step 11. Bob publishes transaction #3 to the Litecoin network.

> Step 12. Alice spends transaction #3 while publishing a secret value.

> Step 13. Bob receives the secret value and uses it to unlock transaction #1.

It is important to note that if Bob does not take his bitcoins within 24 hours, then Alice can get them back. But if Bob takes his money on time, then neither side can cheat.

### Atomic swap limitations
Like any technology, the atomic swap mechanism has several advantages and disadvantages. Above, we examined the advantages of this technology such as trustless exchange in a decentralized environment without the need to rely on intermediaries. Now, it’s the time to consider the limitations:

> * Exchange confirmation takes a lot of time
> * High fees when exchanging 
> * Necessity to find a counterparty and negotiate on the exchange terms every time

Centralized exchange can settle transactions in  split seconds; atomic swaps depend on platforms and participants and can take a comparatively long time. For example, for the case that we considered, a secure exchange cannot be finished in less than 2 hours (the time of complete confirmation of 2 blocks in Bitcoin and 2 blocks in Litecoin) and that requires the parties act quickly. The upper limit of exchange time in our case is 12 hours.

The next limitation is a high fee. To perform an exchange, each participant has to execute two transactions: one in the Bitcoin network and one in Litecoin, and, as you may know, these systems have quite high fees. On the other hand, centralized exchanges offer fees as low as a few cents.

The last limitation is that you need to search for the counterparty yourself. Decentralized exchanges can solve this problem in some way; however, the speed of order matching is still low compared to centralized platforms. Further, it’s  worth mentioning the inability to execute atomic swaps with fiat currencies.

We should also mention the risk of losing coins due to using a smart contract with a bug. If the user does not conduct a qualified audit of the smart contract of the transaction that was sent to the network, she may lose her money (and in this situation, there will be no party to address with such a claim).

### How decentralized exchanges use atomic swaps 
Decentralized exchanges that enable you to work with several accounting systems with their own transaction histories can be built based on the principles of atomic swaps. However, when such decentralized exchanges are designed, it should be remembered that anyone must be able to publish their buy or sell offer. Therefore, you must have a protocol that allows you to create a decentralized orderbook first.

As for guarantees of order fulfillment, there are some things to consider. Centralized exchanges store all balances on their servers. Hence, even though the user can cancel the order at any moment, the exchange will complete it in any case unless it was canceled by the user. Decentralized exchanges need fees for obligation violations. This is the best approach to solving this problem in 2019.

Trades on decentralized exchanges happen directly between network participants on a peer-to-peer basis. The figure shows subnets that interact within their own protocols. Some nodes contain several different components (software for different accounting systems) to interact with different networks. Thus, the exchange of various digital assets between nodes is provided (fig. 4.36). 

<img width="50%" alt="Figure 4.36 - Decentralized exchanges operation scheme" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.36-Decentralized-exchanges-operation-scheme.png"/> 

It is worth noting that there is still no single standard for an atomic swap. Everyone who currently uses atomic swaps can use cryptography and smart contracts on their own without any common approach.

Decentralized exchanges that use atomic swaps technology include Barter DEX, Blockchain.io, Atom DEX, etc.

### Panic sales problem
With the massive use of atomic swaps, there is a problem that is hard to solve. Suppose there is an accounting system with a very high transaction processing fee, and the time for transaction confirmation is quite long (e.g. Bitcoin). Users start selling this currency (since the system has a low capacity) and create orders on a decentralized exchange, but these orders, when executed, create smart contracts in the same system as the currency they are trying to sell. Thus, the growing network load becomes even higher and pending transactions form an even larger queue so users start to sell this currency more actively, place more orders and increase the transaction queue. This is quite similar to the example from nuclear physics (fig. 4.37).

<img width="40%" alt="Figure 4.37 – Nuclear fission scheme" src="/resources/img/volume-2/4.3-Operation-principles-and-use-of-atomic-swap/Figure-4.37-Nuclear-fission-scheme.png"/> 

In the nuclear fission of 235 uranium isotope, it usually releases 1 to 8 free neutrons. Each neutron formed during the fission can cause the fission of the neighboring nucleus. This phenomenon is called a chain reaction of nuclear fission. Actually, this is the principle of the atomic bomb explosion. Moreover, the problem of extinguishing nuclear explosions has not yet been successfully resolved. Of course, the panic sales problem in decentralized exchanges based on atomic swaps is less significant compared to the problem of atomic bombs, but this analogy very clearly reflects the existence of an avalanche-like spreading of panic on the market.

[CONSENSUS REACHING METHODS](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-2/en/5-consensus-reaching-methods.md)
