# User privacy in open system
## 7.1 Privacy as a concept in the digital world
At the beginning of the 21st century, making an online payment was considered risky and was usually done with great 
caution. Using real names on online resources was, at least, strange, and the need for privacy of online activity was 
considered as something by default.

In some countries, the security and well-being of users depend on the degree of protection of their personal data. 
David Crosbie, a professor at the University of Pennsylvania, noted that the introduction of truly anonymous payments 
can improve security concerns in the developing countries [99] where public confidence in the legislative, judicial, and 
executive bodies is often very low, and privacy is not always maintained when processing personal data. In developed 
countries, there is often a well-established infrastructure and regulatory framework, ensuring the fundamental 
requirement for information security (such as GDPR).

The subject of privacy and the ways to ensure it is a quite extensive topic, so it would be best to begin with the basic 
definitions.

### Importance of maintaining privacy
In practice, privacy in open systems is of paramount importance. Let's suppose there are five restaurants on one street. 
They compete for the flow of customers and each one strives to earn more revenue. Information about internal pricing 
processes and revenues remains secret from competitors. Let's imagine what would happen if someone were to make 
information about all internal transactions and other documentation of these five restaurants publicly available.

The less successful competitors might be tempted by improper intentions to harm the most successful competitor 
financially by, for example, causing a fire. If the reason for that restaurant being successful and popular with 
customers was its tasty dishes cooked using fresh and quality products, the only parties to benefit from its ruin would 
be the less successful competitors.

The question of privacy is also relevant in the field of scientific research. A group of scientists, having made 
meaningful discoveries, may wish to remain anonymous (as if with Bitcoin); perhaps each of them prefers to be considered 
a part of the team which collectively contributed to the discovery, rather than as an individual. Therefore, anonymity 
and legitimacy of financing of scientific research for the development of independent and censorship-free research 
institutions may reasonably become relevant.

Nowadays analytical capabilities allow companies not only to take into account the habits of users but also to influence 
their behavior. All this has become possible due to the capability to capture and process data on purchases and other 
user activity on the Web. Access to this data can generate huge additional revenues.

A good example is a case involving Target stores where a man complained loudly in the store that his daughter had 
received a large number of coupons for diapers and children's clothing, although she had not even finished school and 
thinking about children at her age would be, pardon, too much. The store manager agreed that it was strange, and 
apologized. However, the girl was actually pregnant, and because of a pregnancy prediction system developed by a Target 
analyst, Andrew Paul, which analyzed purchasing preferences, the store was able to identify the girl's pregnancy much 
earlier than even her father.

This story can evoke all sorts of different emotions, but most likely people would not be happy to discover that a 
significant part of their purchases can be influenced. Moreover, if you analyze the data received and used for these 
purposes, a lot of it may prove to be personal data. If you consider that an individual did not give consent to the 
processing of their personal data, it is reasonable to assume that they may feel deceived. It is most likely that people 
would want to restrict access to their personal data to protect themselves from companies that may wish to manipulate 
customers' purchasing behavior, targeting them with well-timed "advantageous offers".

Therefore, one of the most important topics to consider is privacy in the digital world. Considering the development of 
technology and analytical tools, this question is becoming more and more relevant.

### Components of privacy
The concept of privacy includes two main components: *untraceability* and *anonymity*. Untraceability implies the 
inability to attribute a series of actions to a certain user on the network. Anonymity is associated with the inability 
to reliably establish the identity of a user in the network. Figure 7.1 shows an example of how a typical person may 
wish to protect himself and his privacy on the network.

![Figure 7.1 - Principle of achieving user privacy in the world wide web](/resources/img/volume-1/7.1-privacy-as-a-concept-in-the-digital-world/7.1-principles-of-user-privacy.png)

The concept of anonymity implies the presence of both social and technical elements. Social anonymity includes 
information concerning an individual's personal information. Technical anonymity is much more complicated, and the main 
efforts are focused on ensuring anonymity in this area specifically.

When it comes to increasing the level of privacy, special mechanisms can be provided to protect the following attributes 
of user activity:
* Direct association between actions, messages, and transactions 
* Versions of applications and operating system 
* Localization of applications 
* Timestamps of messages and requests 
* Identifiers (usernames, email addresses, phone numbers)
* Fact of an action 
* Device locations 
* Network addresses of devices

Some implementations of Tor browser and BitTorrent client have already been supporting advanced privacy options that 
help to protect some of the listed attributes. The techniques can indeed be specific: for example, using random ports on 
a network level, high redundancy of requests, and changing the default screen resolution.

As we have mentioned before, Bitcoin showed that a financial system can be transparent and auditable to everyone while 
maintaining a certain level of user privacy. Nevertheless, it became clear that for some use cases, the level of 
transparency that blockchain provided is unacceptable. Thus, new methods to provide a higher level of user privacy were 
also proposed and even implemented in some cryptocurrency projects. These methods effectively solved issues of privacy 
for transactions while leaving them immutable and publicly verifiable.

## 7.2 Privacy in digital currencies
As mentioned in the payment channels section (4.8), the development of digital currencies has been complicated due to 
the problems of user privacy and scalability. Moreover, any asset accounting system that uses a public database faces 
the issue of privacy [100]. In cryptocurrencies, the principle of full transparency of transaction history is taken for 
granted. This topic answers the question how to achieve privacy in such circumstances.

Next, we will get acquainted with the model of privacy and how to achieve it. We will analyze several approaches already 
implemented in practice and will also consider their advantages and disadvantages.

> **Transaction data that should be protected first**
>> * Coin origin history 
>> * Payment amounts 
>> * User addresses (identifiers)
>> * Network addresses of user devices 
>> * Metadata (network propagation time, wallet version, etc.)

Some digital currencies use a special transaction structure and a coin registration model which allows hiding data about 
certain transaction details (Table 7.1). To hide or mix up certain data, different currencies apply different methods. 

![Table 7.1 - Privacy in digital currencies](/resources/img/volume-1/7.2-privacy-in-digital-currencies/table-7.1-privacy.png)

### Blind signatures
First, let’s consider one of the techniques that allows several parties to interact while staying private. It is called 
blind signatures.

A blind signature is a kind of digital signature which feature is that a signer of a document cannot definitely know 
what is inside this document [105]. The blind signature scheme is as follows (Fig. 7.2):

* Sender encrypts a document and sends it to the signer. 
* Signer signs the encrypted document with her key and sends back the signed document. 
* A sender decrypts the message leaving only the signature of a signer on the document and submits the message and the 
signature to a verifier.

![Figure 7.2 - Blind signature mechanism](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.2-bling-sig.png)

Blind signature protocols are the most applied in the fields of digital currencies and secret voting.

### Bitcoin privacy by default
Bitcoin comes with the anonymity feature, which is quite easy to lose in practice [101]. The property of untraceability 
in Bitcoin has not been fully achieved either. It is possible to analyze transactions and draw conclusions about their 
relation to specific anonymous wallets [102]. If at least one address has been compromised in the context of anonymity, 
you will likely find its relation to certain individuals (i.e, who stands behind this address). Thus, the simplest 
implementation of a Bitcoin wallet can provide a user only with a minimum level of privacy.

Let's assume that for each incoming payment or change, a user creates a new address. In this case, an auditor analyzing 
a transaction graph can no longer connect particular facts related to currency transfers between the users as well as 
their activity. Yet, even in these circumstances, user privacy level is not as high as it might seem at first glance.
On the Bitcoin network, the privacy of a particular user generally depends on his counterparties. A recipient will know 
the history of the coin origin, and a sender will know the corresponding address as well as to whom he transmits his 
coins. Moreover, he can trace the history of further coin transfers. In other words, the generation of new addresses for 
each incoming payment may, in fact, complicate the ability to track the coin origin but does not make them completely 
untraceable. There are a number of metadata that can be made available to the outsiders: the nature of the transaction, 
data about the wallet, information about the user's location, etc. In addition, there are methods, algorithms, and 
analyzers that allow you to build a tree from chains, find time dependencies, IP addresses, and also give quite a 
realistic picture of coin distribution with a certain probability by the method of constructing transactions [103]. 
Also, when a user wants to spend coins that were stored in multiple addresses, a transaction which contained multiple 
inputs will reveal a single owner of all of them and therefore ruin privacy.

One of the simplest ways to obscure the history of transactions is to use so-called mixers [104]. Mixers allow 
individual users to mingle their coins. This requires the creation of one transaction which at its inputs submits coins 
from different users. Coins from all inputs are added together and mixed, and then distributed among the recipients' 
outputs. This makes it impossible to definitively determine the history of the coins transferred. Mixing in shared 
transactions can be done even several times in a row. For example, you can conduct 7 such transactions and receive coins 
with a history which is completely ambiguous. This method, however, is not always preferred—some services do not even 
accept the coins that have been mixed.

In order to ensure the highest possible privacy level, we must understand which transaction data is to be hidden first. 
This is primarily related to the origin of coins, which directly concerns the fungibility property: this property is 
very important for any money and assets. The Bitcoin protocol ensures this property: all coins are the same and the 
rules for processing them are common to all. However, in practice, fungibility is easy to compromise. For example, some 
merchants can analyze the history of the origin of accepted coins and reject the payments if they raise doubts.

Another data that is worth hiding is the amount of transfer and addresses of both the sender and recipient in the body 
of a transaction. Also, hiding the network addresses of users makes sense, which is usually achieved using darknets that 
apply protocols such as Freenet, Tor, and I2P. This presents the following problem: how does one hide amounts, history, 
and addresses?

### CoinJoin
We will start with the simplest method for tangling a transaction graph called CoinJoin. Its essence is to create a 
shared transaction where coins are mixed so that their origin becomes ambiguous. Here, a group of users create a shared 
transaction where several payments are made simultaneously, so each user does not have to create a separate transaction.

This idea was first proposed by Gregory Maxwell in 2013 in the popular BitcoinTalk forum [106]. Since then, a large 
number of modifications of this method have been proposed and developed. Each of them has improved certain payment 
properties. Let's consider the *CoinJoin* method, how it works in its pure form, and some of its most interesting 
modifications.

Imagine a group of three users, where each wants to buy goods in an online store. They each create one transaction and 
three outputs, one for each online store. In addition, they create three more outputs for a change (Fig. 7.3). All 
outputs are mixed randomly. Each user double-checks the received transaction and signs the corresponding input. In case 
of success, the transaction is considered correct, then it spreads across the network and gets a confirmation.

![Figure 7.3 - Formation of a CoinJoin transaction](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.3-coinjoin.png)

Figure 7.4 shows two graphs: upper and lower. The upper graph is characterized by the fact that each transaction has one 
or two outputs. In the lower graph, transactions can have three or more outputs. The lower graph is more complicated and 
more difficult to analyze.

In practice, the application of CoinJoin method in a Bitcoin wallet generally presumes a large group of users. 
Transactions can have dozens of inputs and outputs (or even more). The graph of such transactions (that is represented 
on an image plane in Figure 7.4) will be very confusing. A coin that passed through the chain of such transactions has 
thousands of possible options of origin, making it very difficult to find the real one.

![Figure 7.4 - Transaction graph when using the CoinJoin method](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.4-coinjoin-graph.png)

Methods we will further describe are all different modifications of the CoinJoin method where creators aimed to achieve 
even higher privacy through a variety of ways.

### Chaumian CoinJoin
One of the CoinJoin modifications is called Chaumian CoinJoin, also suggested by Gregory Maxwell. This method includes a 
centralized operator and a blind signature. Here, the operator is required because it is he who mixes the inputs and 
outputs and creates the final transaction. At the same time, he cannot steal coins or violate the privacy of mixing, all 
because of a blind signature.

The user blinds the data before sending it to the operator (by using the blind signature mechanism). When an operator 
signs this data, she does not have access to the virtual content. The signed data is returned to the user who then 
removes the blinding with the result that looks like an ordinary digital signature.

What is the interaction between the user and the operator when forming a shared transaction? In advance, each user 
prepares the input where he specifies spent coins, address for receiving change, and the blind address for sending the 
payment. Next, the data is combined into one sequence and is then sent to an operator (Fig. 7.5).

![Figure 7.5 - Interaction of users when using the Chaumian CoinJoin method](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.5-chaumian-coinjoin.png)

The operator verifies the input and the payment amount, then signs the output address and returns the signature to a 
user. The operator does not have access to the payment output address because it is blinded. Next, a user removes the 
blinding from the output address, anonymously reconnects to the operator, and sends her the signed output address. The 
operator verifies that the signature of this address complies its public key. If the address was actually signed by the 
operator, then it means that she already has the corresponding input. However, she does not know which input corresponds 
to which output. After all users have performed the corresponding actions, they all anonymously reconnect to the 
operator again and provide their signatures in the input of a shared transaction to confirm that they own the coins. 
This transaction can be distributed over the network for confirmation.

In this case, neither the users nor the operator can deanonymize the coins at the output addresses. The creation of a 
transaction takes no more than one minute (under normal conditions). Users should interact through anonymous data 
transmission networks such as Tor, I2P, or Bitmessage.

It is possible, of course, that among users, there may be some with malicious intent whose purpose is to disrupt the 
process of creating a shared transaction at any cost. There is even a list of possible scenarios of user behavior 
including fraud. To avoid unfavorable scenarios, a set of security mechanisms have been developed that allow fair users 
to create a shared transaction securely. Security mechanisms use timeouts, the tracing of unused outputs, etc.

### CoinShuffle
The *CoinShuffle* technique was proposed in 2014 [107]. In this method, there is no central operator, which is an 
advantage. Users themselves generate a shared transaction while communicating with each other. They are yet not able to 
violate the privacy of the output addresses mixing process. Another advantage is that users do not have to use 
additional networks to anonymize traffic since all the necessary properties will be achieved using a single p2p user 
interaction protocol.

CoinShuffle uses directional encryption with a pair of keys, public and private. The message is encrypted using a public 
key, and only the private key owner is able to decrypt it. The DiceMix protocol is used for secure communication. 
Diagrams would help understanding how CoinShuffle works in a simplified version.

Imagine a small group of users: sly Alice, wise Bob, bearded Charlie, and orange Dave. In the Bitcoin system, each of 
them has one unspent coin (one UTXO) on the addresses A, B, C, and D respectively (Fig. 7.6). 

![Figure 7.6 - Group of users that will participate in the shared transaction creation](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.6-group-for-shared-tx.png)

They all want to spend their coins and hide the origin history. For this, each member of the group determines the 
address to which his coin should be sent, but no one discloses this address to other participants.

Next, each user generates a new key pair for the directional encryption which they then exchange with each other, whilst 
the new public key is signed with a private key that corresponds to the address which has an unspent coin. In the same 
way, all messages from the participants will be signed during the subsequent interaction. This was the first stage.

Participants mix and form a queue. Alice will be the first because she is sly, Bob will be the second because he is 
wise, and so on. Now, Alice takes A' and encrypts it directionally to Dave's public key. Alice again encrypts the 
resulting ciphertext but now directionally to Charlie. This ciphertext is then encrypted again but now directionally to 
Bob (Fig. 7.7).

![Figure 7.7 — How users interact when using the CoinShuffle method](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.7-users-interaction-coin-shuffle.png)

Alice sends the encryption result to Bob. Bob decrypts the received message with his private key. Then, he takes B' and 
encrypts it to Dave, then to Charlie, and then adds it to the list. He mixes this list randomly and passes it to 
Charlie. Charlie, in turn, decrypts the list items with his private key, adds C' encrypted to Dave into the list, and 
then mixes all the list items randomly. The list is sent to Dave who then decrypts it, receives the public data of the 
addresses for sending coins, adds the D' address, mixes them randomly, and generates a shared transaction based on these 
addresses, known transaction inputs, and sums.

Dave distributes the transaction template to other group members. After that, each user verifies that there is a 
required address in the transaction outputs and that the sum is equal. If the condition is met, the participant signs 
the transaction hence confirming the ownership of his input coins (Fig. 7.8). Participants exchange signatures, and if 
the transaction collects all the necessary signatures, it can be distributed over the network for confirmation.

![Figure 7.8 - Formation of a shared transaction](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.8-formation-of-shared-tx.png)

An important feature is that if some of the participants start avoiding the original interaction scenario, others can 
together analyze the interaction history, remove the violators from the group, and repeat the process without them.

Сomplete CoinShuffle implementations already exist. In practice, they work effectively even on groups of several dozens 
of users. Nowadays, the integration of this protocol into some Bitcoin wallets including the mobile ones is heavily 
expected.

### Disadvantages of the CoinJoin method
The off-chain interaction for the generation of a transaction is very complex. It is necessary to organize the formation 
of groups and the interaction of users with each other. However, a more significant drawback is that *CoinJoin* does not 
completely hide the payment amounts. That’s what makes it vulnerable to the CoinJoin Sudoku analysis, which is based on 
a comparison of the amounts at the transaction outputs. It potentially allows unraveling the history of coin origin even 
after multiple mixes. Nevertheless, this problem can be resolved. For example, you can use only certain amounts for the 
transaction outputs such as 0.1 BTC, 1 BTC, 10 BTC, etc. (although this, in turn, creates additional difficulties and 
limitations).

In fact, there is another method that solves the problem of public payment amounts. It is called Confidential 
Transactions. But before we proceed to it, let’s consider its underlying concept, zero-knowledge proofs.

### Concept of zero-knowledge proof
A zero-knowledge proof (ZKP) is a method by which one party can prove to the other that he knows a particular secret 
without disclosing any data about this secret.

For such proof schemes, there are various mathematical approaches that must provide the proofs with the properties 
listed below.

> **Properties of zero-knowledge proofs**
>> * Completeness 
>> * Soundness 
>> * Zero knowledge

*Completeness* presumes that if a prover and verifier follow the protocol honestly, a prover can convince a verifier 
that the statement is true.

*Soundness* means that a prover will not be able to convince a verifier that the statement is true if it is false.

*Zero knowledge* implies that while the statement is being proved, a verifier can learn nothing about it except for the 
fact whether it is true or false.

There are two kinds of zero-knowledge proofs: interactive and non-interactive. Interactive proofs presume that a prover 
and a verifier communicate with each other directly. In the course of their communication, a verifier gets more 
convinced that the statement is true. An interesting feature of this approach that this only works with the verifier. In 
other words, even using the same proofs, no other third party could convince herself that the statement is true or 
false.

In the case of non-interactive proof, a prover, according to a predefined scheme, generates a separate dataset. This 
dataset essentially is the proof of the statement that, unlike interactive proof, has no particular recipient. By having 
such proof, anyone can anytime verify whether a prover's statement is true or not.

For a better understanding of zero-knowledge proofs, consider the following example. Suppose that Alice wants to examine 
how smart and creative her friend, Bob, is. So, she shared an interesting problem—she drew a certain number of points 
and connected them with lines as in Figure 7.9.

![Figure 7.9 - Positioning of graph nodes](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.9-graph-nodes.png)

Next, Alice gave Bob three markers of different colors and asked him to color the graph points so that there are no 
connections between two points of the same color.

Bob worked really hard, and it took him four minutes to fairly state that this problem has no solution. Alice, in her 
turn, replied that not only the solution exists, but also that she can prove it to Bob without having to disclose it.

So, Alice asked Bob to leave the room for a minute and then colored the graph as in Figure 7.10.

![Figure 7.10 — State of graph nodes after being coloured](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.10-the-second-state-of-the-graph.png)

Next, in order to hide the solution (which point has which color), Alice put a cap on each point of the graph. Having 
entered the room, Bob saw the following (Fig. 7.11).

![Figure 7.11 — Hiding which graph node has which color](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.11-the-hidden-graph.png)

Understandably, Bob got very curious. "And how would these caps help you prove you know the solution?" he asked. Alice, 
in turn, asked Bob to raise any two caps that covered the connected points. Bob lifted. What he saw is that the two 
connected points were colored differently (Fig. 7.12).

![Figure 7.12 — Bob verifying Alice’s statement](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.12-verification-of-the-graph.png)

The obvious reaction of Bob was that Alice just got lucky, and other connected points could have the same color. To 
prove that he is wrong, Alice asked Bob to leave the room once more. She recolored the graph and again covered all the 
points with caps. Bob entered the room, raised two nearby caps, and made sure that the two connected points are again 
differently colored.

Bob is not among the stubborn ones, so after the 10th attempt, he finally agreed that the problem has a solution and 
Alice knows it. And so the most interesting in this example (as well as in the concept of zero-knowledge proofs) is the 
fact Alice managed to prove Bob she knows the solution to the problem without disclosing how exactly she had colored the 
graph.

### Confidential Transactions
The peculiarity of the Confidential Transactions (CTs) [94] method is that it completely hides the actual amounts in the 
transaction inputs and outputs from the third parties. Anyone can verify that the sum of all outputs does not exceed the 
sum of all inputs, which is enough to validate a transaction.

This is made possible through the use of the above-described approach, zero-knowledge proof.

> * Payment amounts are hidden from everyone except the sender and the receiver 
> * A transaction contains data sufficient for validation 
> * Zero-knowledge proof approach is applied

*Pedersen Commitment*, which is based on transformations in a group of points on an elliptical curve, is used to prove 
that the output sum does not exceed the input sum. This method allows dealing with uncontrolled coin issuance through 
the application of proofs that ensure the correctness of used transaction output amounts. To verify that used sums are 
non-negative and do not exceed the base point order, the so-called Range Proofs are applied.

The disadvantage is that creating Range Proofs is very expensive in terms of computational resources. Theoretically, 
Confidential Transactions can be integrated into the Bitcoin protocol, but this didn't happen due to their large size. 
Nevertheless, Confidential Transactions are already successfully applied in other working accounting systems.

### Ring Confidential Transactions
The following technique is called *Ring Confidential Transactions*. In order to confuse the history of the coin origin, 
this method uses ring signatures. Instead of referring to one output (UTXO), the sender refers to several. Next, using a 
ring signature, she proves that she owns coins of one of several outputs but does not disclose which one in particular. 
As a result, this makes it impossible to trace the history of coin origin definitely.

Application of ring signatures was first proposed in the CryptoNote protocol [108] on the basis of which several 
cryptocurrencies already operate. Ring Confidential Transactions use CTs. They allow users to create transactions with 
multiple inputs and outputs where it is impossible to trace the origin of each input, the payment amounts are hidden, 
and you don't need to interact with other users to create a transaction (Fig. 7.13).

![Figure 7.13 — Using ring signatures for Ring CTs](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.13-ring-signature.png)

### MimbleWimble
MimbleWimble is a format and a protocol of transactions and blocks creation. While being based on the cryptographic 
primitives [109], it provides for a high level of user privacy. In its current form, MimbleWimble is hardly compatible 
with the Bitcoin protocol and can be implemented only as a sidechain.

> **Main features of MimbleWimble**
>> * Providing privacy by default 
>> * Ability to scale despite the growing number of users 
>> * Reliable and well-tested cryptographic algorithms 
>> * Simple protocol architecture 
>> * Simple transaction verification and system maintenance

Grin is the first project which implements the MimbleWimble protocol. It is used for detecting issues and 
vulnerabilities in the protocol of a cryptocurrency even before the launch of its main network. The first test network 
was launched in November 2017 with its purpose as finding the drawbacks in the conception of MimbleWimble. After 
shutting down the second test network, a number of technical updates have been added to the protocol, and many critical 
and severe vulnerabilities were also fixed. On July 7, 2018, the third test network was launched for the purpose of 
finishing the update (in Fig. 7.14, you can see the genesis block of this network).

![Figure 7.14 — Genesis block of a third test Grin network](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.14-grin-genesis-block.png)

By November 2018, more than 53 thousand blocks have been created. However, most of these blocks contain only the reward 
for the block creation. Currently, in the network, the number of transactions between users is minimal.

### Stealth Addresses
This approach is a technique of stealth addresses calculation. This idea was first described by Peter Todd. The method 
requires a sender to create a random one-time address for each coin transfer. If several transactions are performed on 
the same stealth address, the data in the chain of blocks will, nevertheless, display that these payments were made on 
different addresses [110].

This makes it impossible to attribute particular transactions to a particular user identifier. These user identifiers 
essentially are the public keys: if you want to accept payments, you need to announce your public key. Only the owner of 
a stealth address, by using his private key, can view all the transactions addressed to him. The sender uses his key 
pair and recipient's public key to calculate a new one-time public key, which will be specified in the transaction as an 
address.

> * Address to which coins are sent can only be known to the sender and the receiver 
> * For an outsider, the relation between the user identifier and the address at the transaction output is impossible to 
establish

If the case is about, for example, an online store using stealth addresses for receiving payments, you will find it hard 
to trace its activity (Fig. 7.15). All because each customer will be supposed to generate a new one-time address for 
each payment to the online store.

![Figure 7.15 — Online store using stealth addresses for receiving payments](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.15-stealth-addresses.png)

### Concept of homomorphic encryption
Also, in the context of asymmetric cryptography, you cannot go without mentioning the homomorphic encryption. A 
homomorphic cryptosystem is a system which allows manipulating a plain text through performing operations on the 
orresponding ciphertext. This means that you can assign the other party to process your private data without worrying 
about their leak. For instance, you may encrypt the data about your incomes and expenses and send them to an accountant 
to count your current balance. Next, the accountant will return you the encrypted document containing the result which 
can be accessed only by using the same key with which you had encrypted the initial document.

**Frequently asked questions**

*– Is it possible to use Stealth Addresses in Bitcoin?*

Yes, you can use them currently. Protocol updates are not required for this. However, for wider adoption of this 
functionality, it is important that the computation order and data formats are strictly specified so that all wallets 
could operate with each other by adding this function. For introducing this specification, Peter Todd has already 
created a separate BIP, but it is still under consideration.

*– Is it effective to use CoinJoin in its pure form for bitcoins?*

No, it is ineffective in its pure form because such transactions can be easily analyzed regarding the transfer amounts. 
Alternatively, all members of the mixing scheme can use the same transfer amounts; however, it would require avoiding 
trusted mixers that potentially could either steal the coins or violate the privacy.

*– Is it possible to apply the above methods to ensure privacy in digital currencies such as Ethereum, Ripple, or 
Stellar?*

No, it is not. Ethereum, Ripple, and Stellar use an entirely different transaction model and different accounting to 
which such methods of achieving privacy cannot be applied. Certainly, you can try to integrate Stealth Addresses or 
Confidential Transactions artificially, but this will be extremely inefficient in terms of system performance. The 
reason is that these currencies use balances and accounts, while in Bitcoin the accounting is performed based on unspent 
outputs (UTXOs).

[CONCLUSION](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-1/8-conclusion.md)   