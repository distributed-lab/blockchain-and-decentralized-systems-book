# [1 APPLYING DECENTRALIZED APPROACHES TO DIFFERENT SYSTEM DESIGNS](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/1-Applying-decentralized-approaches-to-different-system-designs.md)

# 2 SCHNORR SIGNATURES AND THEIR APPLICATION


## 2.1 Features and application of Schnorr signatures

Since 2014, the Bitcoin community has been considering the possibility and necessity of adding Schnorr signatures to the protocol of the accounting system. The main reason for adding this type of signature is the current size of coin ownership proof. In fact, more than 40% of a normal transaction is occupied by coin ownership proof, i.e. the value of the public key and digital signature. In case of multisignature mechanism implementation, such a situation becomes even worse since the signature is provided for each public key separately, which in turn leads to a proportional increase in the proof size.

What does large proof size mean for Bitcoin? Firstly, these are huge transactions that users have to pay for. Secondly, this means a small number of transactions in blocks which results in low system capacity. This is a large mempool and an increase in fees for using multisignatures.

Schnorr signature algorithm was developed back in 1980 and theoretically can save users from the above disadvantages [49]. However, why was it not implemented in the Bitcoin protocol when it was launched? The fact is that at the time of the creation of the first version of the software, the algorithm was patented and could not be used in open source software. At the same time, ECDSA was a functioning, tested standard, and therefore it was implemented in the Bitcoin protocol. In this section, we will discuss the principles of Schnorr signatures functioning, the features of their application in the Bitcoin protocol, the reasons for their application as well as how these signatures were already implemented in other accounting systems and what their addition led to.


### Schnorr signature advantages

In the second volume of this book, we examined the features of two types of multisignatures: a type that involves aggregating public keys and signature values, and a type that does not allow this (a separate signature value is verified by a separate public key). Schnorr signatures, unlike ECDSA, belong to the first type of signatures, namely they allow aggregation of public keys into a single common value, as well as aggregation of signature values. Unlike ECDSA multisignatures the size of which increases linearly depending on the number of signatories, the multisignature generated using the Schnorr algorithm does not differ in size from a single signature (Fig. 2.1).

![Figure 2.1 – Size comparison for multisignatures calculated using ECDSA and Schnorr algorithm](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.1-size-comparison-for-multisignatures.png "Figure 2.1 – Size comparison for multisignatures calculated using ECDSA and Schnorr algorithm")

This is the main and most demanded feature of Schnorr signatures: reducing the size of coin ownership proof when using multisignatures. Reducing the total size of the signature at the input of the transaction leads to an increase in the system capacity and, accordingly, to a decrease in the amount of the fee that must be paid for a place in the block.

However, this concept can be expanded even further. What if we use such signatures simultaneously for all transaction inputs? The signature procedure most often consists of representing all transaction fields in a byte string form and signing this string or, rather, its hash value. Accordingly, most often (when using signature hash type equal to SIGHASH_ALL) each input of one transaction contains the signature of the same data – all fields of this transaction – using a specific key. Thus, we can apply the principles of aggregation not only to a single transaction input but also to the transaction as a whole (Fig. 2.2).

![Figure 2.2 – Using a single signature value for all inputs of a transaction](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.2-single-signature-value-for-all-transaction-inputs.png "Figure 2.2 – Using a single signature value for all inputs of a transaction")

> _Note. There were periods in the history of Bitcoin when the number of transactions in mempool reached 140,000, although with a normal load on the system their number does not exceed 10,000 (Fig. 2.3). These were also situations when intentional flood attacks on Bitcoin were performed. To do this, many transactions with a large number of inputs were published. As a result, only a few tens of transactions were placed in the block instead of several thousand_ [50]_._
> 
> _Schnorr signatures will help avoid the unintended occurrence of such scenarios and increase the system capacity, since it is possible to implement a scheme in which each transaction will correspond to only one signature (general coin ownership proof for all inputs)._

![Figure 2.3 – Bitcoin mempool load chart](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.3-Bitcoin-mempool-load-chart.png "Figure 2.3 – Bitcoin mempool load chart")

It is also important to consider the need to implement Schnorr signatures from the standpoint of transaction confidentiality. As we mentioned earlier, the main advantage of Schnorr signatures is the ability to create a multisignature transaction that is indistinguishable from a normal transaction. At the same time, none of the network participants can see which public keys belong to a particular transaction – only the general aggregated value can be seen [51].

Today, the multisignature mechanism in Bitcoin is implemented using the P2SH mechanism. This approach allows to transfer part of the fee payment (overpayment for the use of large coin spending conditions) to the receiving party; however, at the same time, the condition for spending coins is still known to third parties (since it must be published to prove coin ownership). As an alternative, the MAST mechanism was proposed. It will be discussed in the next section.

MAST allows ensuring the confidentiality of unused conditions, but the condition that was met is available to all network participants. Also, the size of coin proof ownership may expose the presence of other conditions.

The introduction of Schnorr signatures paves way for such proposals as Taproot and Graftroot where key aggregation is required [52]. Taproot offers all of the advantages of MAST structures, but no one will know that a simple transaction hides a complex smart contract if the condition at the top of the Merkle tree is met (otherwise the third party may also find out about the existence of additional conditions). Graftroot allows to completely hide the existence of complex conditions (other users will never know about the existence of alternative scenarios). We will examine these approaches in more detail in Subsections 2.3 and 2.4.


### Schnorr signature design

In order to show what properties a Schnorr multisignature algorithm has, we first need to examine the functioning principles of a single signature. Initially, the user has a key pair consisting of private and public cryptographic keys. At the same time, the process of obtaining a public key from a private key is no different from ECDSA: the public key is equal to the private key multiplied by the value of the base point. It is important to note that implementation of Schnorr signatures does not require a change in the parameters of the elliptic curve used. Accordingly, the mechanism of forming a key pair and the final address will not change for the user.

The process of signing a message is quite simple and consists of two steps: generating a randomizer, and using a private key and the randomizer to generate a signature (Fig. 2.4).

![Figure 2.4 – Schnorr signature algorithm](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.4-Schnorr-signature-algorithm.png "Figure 2.4 – Schnorr signature algorithm")

The signature value consists of the value pair $(R,s)$, where _R_ is a curve point and _s_ is a natural number. When verifying the signature we need to check that the condition is met, as shown in Figure 2.5. If it is satisfied, then the signature is considered correct.

![Figure 2.5 – Signature verification algorithm](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.5-signature-verification-algorithm.png "Figure 2.5 – Signature verification algorithm")

> _Note. The total value size of such a signature equals 96 B. However, it can be compressed to 65 B. For this, you need to present the value of point R as the x coordinate and the sign of the y coordinate. In Volume 2 of the book, this method is discussed in the context of the compressed public key format in Bitcoin._


### Using the Schnorr algorithm in multisignatures

The multisignature mechanism is practically no different from the usual signature, except that the signature and public key values are equal to their aggregated value of all participants (Fig. 2.6). The aggregated public key is equal to the sum of public keys of all participants in the signature and also represents a point on the curve. The signature value, in turn, is the sum of the signatures of all participants modulo the order of the base point.

![Figure 2.6 – Comparison of single signature and multisignature algorithms](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.6-comparison-of-single-signature-and-multisignature-algorithms.png "Figure 2.6 – Comparison of single signature and multisignature algorithms")

You may notice that the size of the multisignature is no different from a single signature, and the verification algorithms for both signatures are completely identical. This means that for an outside observer whether it was a single signature or multisignature is not visible.


### Rogue key attack principle

The multisignature mechanism that we described earlier can be used if each participant in the system is guaranteed to know the public keys of the other participants with whom he interacts. However, in practice this approach will not be used, at least in Bitcoin and other similar systems, since it allows one of the parties to cheat at the stage of generating the aggregated public key [53].

In order to explain this kind of attack, we suggest the following example: Alice and Bob want to send their funds to an address that they can only access together (i.e. a multisignature address).

To do this, they need to form an aggregated public key to which coins will be assigned. In the process of forming the key Alice and Bob exchange public keys and calculate the aggregated value as the sum of their public keys. Then Alice and Bob form transactions (or one common transaction) that spend(s) coins on the specified aggregate value (Fig. 2.7).

![Figure 2.7 – Sending coins to aggregated public key value](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.7-sending-coins-to-aggregated-public-key-value.png "Figure 2.7 – Sending coins to aggregated public key value")

After this transaction is confirmed in the network, Alice and Bob can only spend locked coins together. For this, each of them forms their own signature value, which is then aggregated and added to the coin ownership proof. The process of spending funds using multisignature is shown in Fig. 2.8.

![Figure 2.8 – Spending funds with multisignature addresses](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.8-spending-funds-with-multisignature-addresses.png "Figure 2.8 – Spending funds with multisignature addresses")

As we mentioned earlier, the verifier can make sure that the signature is correct by checking the equation (Fig. 2.9):

![Figure 2.9 – Signature verification procedure](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.9-signature-verification-procedure.png "Figure 2.9 – Signature verification procedure")

Let’s now look at a situation in which Bob wants to trick Alice and steal her coins. To do this, at the step for exchanging public keys, he needs just to send Alice a fake, specifically chosen public key value. By contrast to the scheme in Fig. 2.9, let Bob send not his own value _X<sub>B</sub>_, but the value $X_{\text{B}}-X_{\text{A}}$ (Fig. 2.10).

![Figure 2.10 – Bob spoofs the public key value](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.10-spoofing-public-key-value.png "Figure 2.10 – Bob spoofs the public key value")

Here we see that the value of the aggregated key is equal to the value of Bob’s public key. Accordingly, Bob knows the private key, which he can use to calculate the signature that will be verified using the specified public key. Thus, Bob does not need Alice’s confirmation to unlock coins that are tied to that address.

> _Note. While conducting this attack, Bob does not know the value of the secret key that would correspond to the public key_ $X_{\text{B}}-X_{\text{A}}$_. This is due to the properties of elliptic curves. However, this attack can be performed in the basic case, since Alice does not check whether Bob actually knows the secret._

As one of the solutions to this problem one can use an additional iteration after exchanging keys, during which participants must prove that they know the corresponding private keys. To do this, each transaction participant can generate a random nonce value and ask the counterparty to sign it, and then verify the signature with the previously obtained public key (Fig. 2.11). However, this method requires four additional iterations and is not the most optimal solution (in contrast to the method discussed later).

![Figure 2.11 – Verifying that Bob owns the specified public key](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.11-verifying-ownership-of-public-key.png "Figure 2.11 – Verifying that Bob owns the specified public key")


### Bellare–Neven scheme

Another way to protect against a key-spoofing attack is to use the Bellare–Neven signature [54]. Let’s take a closer look at this scheme (Fig. 2.12).

![Figure 2.12 – Bellare–Neven signature scheme](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.12-Bellare--Neven-signature-scheme.png "Figure 2.12 – Bellare–Neven signature scheme")

As one can see, there is no aggregated value of the public key in the scheme and all public keys of the participants are used to verify the signature individually. This scheme, in fact, prohibits one of the parties to fake the public key since the corresponding secret value will be necessary to calculate the signature. Moreover, the size of the signature value does not differ from the signature calculated by one participant.

However, the obvious drawback of this scheme is that its verification algorithm is completely different from the standard signature and requires the use of public keys of all parties involved, which in turn deanonymizes the transaction participants.


### MuSig scheme

On January 15, 2018, a proposal for a new scheme was published. This proposal uses the Bellare–Neven approach, but allows multiple signatures to be indistinguishable from the standard, while maintaining protection against attacks with public key spoofing [55]. Let’s look at the algorithm for generating such a signature (Fig. 2.13).

![Figure 2.13 – MuSig signature scheme](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.13-MuSig-signature-scheme.png "Figure 2.13 – MuSig signature scheme")

The aggregated key value in this case is not just the sum of the public keys but the sum of the public keys each multiplied by an additional value (a hash value that includes the hash of all signers’ public keys as well as the particular public key).

MuSig is the scheme that is able to solve the problems associated with reducing the size of coin ownership proof (by reducing the total size of the signature) and the confidentiality of the participants in the transaction (aggregated public key and the total value of the signature). Furthermore, due to the fact that the single signature verification algorithm does not differ from the multisignature verification algorithm, validators may not even be aware of the participation of several parties in the transaction (especially if additional methods such as Taproot and Graftroot are used, and additional conditions are present).


### Schnorr signature restrictions

Despite the previously described advantages of Schnorr signatures, a number of problems arise that need to be solved for their successful application.

We mentioned earlier that a transaction can contain one signature for all of its inputs. This possibility does exist; however, it should be noted that when signing each of the inputs, the public keys of all inputs must be taken into account. As an example, we have a transaction with three inputs for two, three, and five coins. Each of these inputs (preliminary unspent outputs) is assigned to a separate public key, proof of ownership of which determines the coin ownership proof.

To aggregate signatures into one, each of the inputs should actually contain the following type of signature (Fig. 2.14).

![Figure 2.14 – Signature values for transaction inputs](/resources/img/volume-3/2.1-Features-and-application-of-Schnorr-signatures/F-2.14-signature-values-for-transaction-inputs.png "Figure 2.14 – Signature values for transaction inputs")

These values can only be aggregated if each of the signing parties has the same aggregated public key value (_X_), that is, they each know the public keys of all other inputs. Thus, users cannot sign their entries and use the inputs of other users to form a common transaction without the prior consent of all parties to it. This feature greatly complicates implementation of such approaches as CoinJoin and its modifications, CoinShuffle, etc.

The second limitation is that the signature aggregation approach in a transaction can only be used if the signature covers the entire transaction (SIGHASH_ALL). Signature Hash Types allow users to sign only certain inputs and outputs of a transaction while maintaining flexibility of interaction with other participants in the transaction (for example, the ability to add additional inputs and outputs, etc.). However, these types of signatures imply signing different data, and, accordingly, different hash values. As a result, the signature values ​​of inputs with various types of data being signed cannot be aggregated.


### Features of Schnorr signatures implementation 

As for January 2020, nothing hampers implementation of Schnorr signatures in the Bitcoin protocol (except for the need to convince the community that it is advisable).

In June 2018, a project was announced that outlined the technical implementation of Schnorr signatures [56]. Pieter Wuille, who actually unveiled the project, said that Schnorr signature is “a building block for various further improvements”. He hoped that the changes would ultimately be accepted, although it completely depends on the decision of the users. A mathematical explanation of the principles of Schnorr signatures was also co-authored by leading developers such as Gregory Maxwell and Johnson Lau [55].

The introduction of the Schnorr signatures was preceded by an update to the protocol called Segregated Witness (SegWit) which allows the transfer of transaction signatures to the structure outside the main block. Although SegWit itself is not an indispensable condition for implementing the Schnorr signature, its activation allows to make the whole process easier. The use of Schnorr multisignature algorithm in combination with SegWit allows to further reduce the size of transactions and increase the privacy level of each user due to the possibility of combining all transaction signatures into a single one.

### \*\*\*Frequently asked questions\*\*\*

_– Is it possible to develop the concept even further and allow validators to aggregate the signature values of all transactions in the block?_

Signature aggregation is only possible if the signatures cover one data set. Since users sign only their transactions (various data), one cannot aggregate their value within the boundaries of the block.

_– Is it possible to use MuSig to organize a scheme when one user created a transaction, signed it with his key and put it in a safe, and after a while another user took it, signed it with his key and published?_

Theoretically, such an opportunity exists, but this approach will not be convenient for users. The problem is that to generate the signature, the parties must first exchange with the values ​​of _R<sub>i</sub>_ and form a shared _R_, which is necessary for the calculations. Therefore, the only way to do this for this scheme is to pre-generate _R<sub>i</sub>_, exchange with them, and break up. Then, an option can be implemented when the transaction is signed first by one user, and then by another. However, at the same time each of them must store their own _r<sub>i</sub>_ and the shared _R_.


## 2.2 MAST concept in Bitcoin 

The Bitcoin protocol allows describing the coin spending terms via special Bitcoin Script language. This language is Turing-incomplete. However, the set of operations defined in the language allows the implementation of the most common scripts and ensures  functioning of top-level protocols including, but not limited to, atomic swaps and payment channels. 

The current Bitcoin protocol requires all conditions for spending coins to be published. This also encompasses instances of script containing several alternative conditions. These conditions must be present explicitly in the output of the transaction or in the input of successive transactions together with the proofs that these conditions were satisfied (P2SH approach). This approach allows making sure that conditions are met. However, this is not the most effective approach due to the following reasons.

> * _A large amount of unused data_
> * _Low privacy level_
> * _Contract size limit_

The first drawback is that the transaction must contain the whole contract (all conditions) regardless of the condition that was met. Thus, in a scenario as shown in Figure 2.15, all script data must be placed in the transaction even though coins can be unlocked when one condition is met.

![Figure 2.15 – Script placed in the condition of spending coins](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.15-script-placed-in-coin-spending-condition.png "Figure 2.15 – Script placed in the condition of spending coins")

For the same reason, there exist certain problems with the confidentiality of conditions: each member of the network sees all alternative conditions, regardless of which of them was actually met.

The Bitcoin protocol limits the maximum transaction size to 1/4 of the maximum block size, and the maximum script size to 520 bytes. Apart from this, the limitation is the price of writing data to the Bitcoin blockchain. Unlike smart contract platforms (such as Ethereum), where the fee is calculated based on resources spent to fulfill the contract, the contract in Bitcoin is paid strictly depending on its size and, accordingly, it is economically unprofitable to create contracts with a large number of conditions that have to be met as only a single condition has to be satisfied.

In this subsection we discuss one of the methods to solve these problems – the MAST concept and its application in the Bitcoin protocol. The concept of MAST involves the use of Merkle trees and abstract syntax trees to specify the conditions for spending coins on transaction outputs. Let’s discuss how the MAST concept works.


### Abstract syntax trees

First, let’s examine the concept of an _abstract syntax tree_ (AST) [57]. It is a tree that represents some algorithm. The _leaves_ of such trees are operands (variables or constant values), and the nodes are the corresponding operators (instructions or functions). Syntax trees are mainly used to simplify optimization and analysis of program code.

As an example, let’s take a simple program, the purpose of which is to find the number closest to the given one, which is divisible by 32 without a remainder (to simplify the example, suppose that the search is performed only in the upper side). The pseudocode for this subprogram is shown in Figure 2.16.

![Figure 2.16 – Pseudocode for finding the number divisible by 32](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.16-pseudocode-for-finding-number-divisible-by-32.png "Figure 2.16 – Pseudocode for finding the number divisible by 32")

Figure 2.17 shows a syntax tree that describes the same program. In the tree, the diamond nodes specify the instructions, the ellipse nodes are the variables, and the rectangles are the constant values. The tree edges define transitions between operations.

![Figure 2.17 – Abstract syntax tree for the program](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.17-abstract-syntax-tree-for-program.png "Figure 2.17 – Abstract syntax tree for the program")

At the top of the tree, there is the "while" statement. This is an instruction for creating a loop, which checks some conditions and, depending on them, executes or does not execute the body of the loop. The condition, in this case, is located on the left side of the tree. Let’s review it in more detail (Fig. 2.18).

![Figure 2.18 – Representation of program conditions](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.18-representation-of-program-conditions.png "Figure 2.18 – Representation of program conditions")

Initially, variable _A_ is divided by a constant value of 32. The "mod" instruction returns the remainder of the division. After that, the "no equal" instruction is executed, which compares the two operands and returns "true" if they are not equal. The operands of the instruction in this case are the constant value 0 and the value returned by the "mod" operator.

Thus, after completing all the instructions in this subtree, the "while" statement receives either the "false" value – if _A_ is divisible by 32 – or the "true" value – if 32 is not a divisor of _A_. If "while" receives "false", then the loop ends. If "true" is returned, then instructions in the right subtree begin to execute (Fig. 2.19).

![Figure 2.19 – Representation of program instructions](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.19-representation-of-program-instructions.png "Figure 2.19 – Representation of program instructions")

In this subtree, the input value _A_ is actually incremented. First, the "add" operation adds _A_ and 1, after which the "assign" operation assigns the result to the variable _A_. After the loop body is executed, the condition is checked again, as discussed earlier.

Thus, AST allows splitting the program into different parts and pre-programming how these parts will be executed (and whether they will be executed at all) depending on other conditions. We will use this feature of abstract syntax trees to build MAST.


### What is MAST?

The second component of the MAST concept is the use of Merkle trees. Since we have already discussed the arrangement of Merkle trees, we will not go deeper and explain the details again, just recall their key advantages: providing the ability to verify the integrity of the data included in the tree and providing authentication of a separate leaf by Merkle branch without disclosing the contents of the remaining leaves of the tree.

Now let’s define what MAST is and how this concept can be applied. MAST is the merklized abstract syntax tree, which uses the ideas of the Merkle tree and the abstract syntax tree to specify mutually exclusive conditions for spending coins [58]. At the same time, Bitcoin Script acts, as usual, as a language for describing conditions.

A scheme of the merklized abstract syntax tree is given in Figure 2.20 below:

![Figure 2.20 – Merklized abstract syntax tree (MAST)](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.20-merklized-abstract-syntax-tree.png "Figure 2.20 – Merklized abstract syntax tree (MAST)")

MAST Root is the root hash value that will be placed in the output of the transaction. Ellipses indicate the hash values of the tree nodes that lead to the coin spending conditions. Thus, subtrees contain mutually exclusive conditions under which coins can be spent. Therefore, the one who spends the coins will use either one subtree or the other.

The rectangles indicate the conditions that are set using Bitcoin Script. Furthermore, the conditions under which the coins will most likely be spent are recommended to be placed as close to the root of the tree as possible – this will make the proof of ownership of the coins smaller.

In order to prove ownership of coins under the terms of a particular subtree, the prover needs to publish a script of the used subtree (as in the case of P2SH), as well as a set of Merkle branch values that are necessary to verify the fact that the condition used is indeed among the possible alternatives.


### Simplified MAST scheme

In order to understand how MAST works, let’s discuss the following example. Suppose, in a general scenario, two mutually exclusive conditions for spending coins are specified. In the first case, coins can be spent by providing one signature and waiting for a certain period of time, and in the second case the user needs to provide several signatures (for example, 2-of-2) (Fig. 2.21).

![Figure 2.21 – Alternative conditions for spending coins](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.21-alternative-coin-spending-conditions.png "Figure 2.21 – Alternative conditions for spending coins")

Users can use one of the options, while MAST allows users to ensure that the conditions of the second option are not disclosed. We can convert these alternative conditions into a single merklized abstract syntax tree, as in Figure 2.22. We calculate the hash value of each condition, then concatenate them and get the MAST Root by re-hashing the resulting value.

![Figure 2.22 – Representation of conditions in MAST tree](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.22-representation-of-conditions-in-MAST.png "Figure 2.22 – Representation of conditions in MAST tree")

Now we can prove that the specific condition was in the MAST tree. To do this, the prover simply needs to provide the hash value of the alternative condition as a Merkle branch (for a multisigned condition, this is _H<sub>1</sub>_). The verifier calculates the hash value from the published condition, and then, using the Merkle branch, calculates the Merkle root value and compares it with the output from the previous transaction.

As we mentioned earlier, the MAST Root is a 32-byte value that is placed in the transaction output (Fig. 2.23). It is much less compared to the full script (167 bytes).

![Figure 2.23 – Putting MAST Root values instead of a script in the conditions for spending coins](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.23-putting-MAST-Root-values-instead-of-script.png "Figure 2.23 – Putting MAST Root values instead of a script in the conditions for spending coins")

Now let’s consider how to spend coins that are blocked at such an address. In the case of a standard (_pay to contract_) transaction, one would simply need to provide coin ownership: that is, either the combined value of the public key and signature (the first condition) equals 129 bytes, or two signature values equal 128 bytes.

In the case of using the P2SH approach where the previous transaction contains a 32-byte hash value of the entire script, the proof contains the entire script (167 bytes), as well as data to satisfy it, which in total equals around 300 bytes.

MAST allows submitting to the transaction input a condition that was fulfilled (either 30 bytes or 134 bytes), data that satisfy these conditions, as well as proof that the condition is one of the alternatives. To do this, a Merkle branch is added to the proof. In our scenario it equals 32 bytes – the hash value of an alternative condition.

A comparative description of standard, P2SH and MAST approaches is presented in Figure 2.24.

![Figure 2.24 – Comparative characteristics of approaches to setting spending conditions](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.24-comparison-of-approaches.png "Figure 2.24 – Comparative characteristics of approaches to setting spending conditions")

We see that MAST significantly reduces the amount of data placed in a transaction if the first condition is met. The differences when fulfilling the second condition do not seem so significant. However, let’s look at how increasing the contract (increasing alternative conditions) affects the size of the script.


### Advantages and features of MAST

In the example above, only two script execution conditions were presented. But let’s look at a scenario where there are many of them. In the case of using the standard or P2SH approach, the amount of script data will increase significantly depending on the number of conditions. However, when using MAST, the size of the script, the size of the Merkle branch will not grow much, but by a maximum of 32 bytes with each additional condition. Moreover, if the conditions in the tree are placed correctly, one can greatly save on the size of the Merkle branch. Let’s imagine that we have seven alternative conditions, but we know with what probability each of them can be used (Fig. 2.25).

![Figure 2.25 – A set of alternative spending conditions with different execution probabilities](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.25-set-of-alternative-spending-conditions.png "Figure 2.25 – A set of alternative spending conditions with different execution probabilities")

Accordingly, using the MAST concept, one can arrange these conditions in a tree in such a way that it is highly likely to pay the lowest possible fee. To do this, condition A (which may be more likely to be fulfilled) needs to be placed closer to the top of the tree, and conditions F and G should be further away (Fig. 2.26).

![Figure 2.26 – MAST consisting of many alternative conditions](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.26-MAST-with-many-alternative-conditions.png "Figure 2.26 – MAST consisting of many alternative conditions")

In this case, if condition A is used, then the proof of coin spending will contain condition A itself, proof that satisfies it, as well as one hash value that will act as a Merkle branch – _H<sub>BCDEFG</sub>_, 32 bytes in size. In the worst-case scenario, if condition G (or F) is used, the proof of coin ownership will contain the corresponding script, the data satisfying it and the value of the Merkle branch, which includes _H<sub>F</sub>_, _H<sub>E</sub>_, _H<sub>CD</sub>_, _H<sub>B</sub>_, and _H<sub>A</sub>_. If we provide a comparative description for this complex condition, we can get the following picture (Fig. 2.27, the average size of one condition was 100 bytes, 150 bytes as proof).

![Figure 2.27 – Comparative characteristics of the size of the conditions depending on the method of setting them](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.27-comparison-of-size-of-conditions.png "Figure 2.27 – Comparative characteristics of the size of the conditions depending on the method of setting them")

Let’s take a closer look at optimizing the data volume that eventually ends up in a chain of blocks. The vertical axis on the chart below indicates the amount of data in bytes. The horizontal axis shows the number of alternative conditions for spending coins. The scale itself is logarithmic (Fig. 2.28) [59].

![Figure 2.28 – Graph of the total size of data volume depending on the number of conditions](/resources/img/volume-3/2.2-MAST-concept-in-Bitcoin/F-2.28-graph-of-total-size-of-data-volume.png "Figure 2.28 – Graph of the total size of data volume depending on the number of conditions")

When MAST is not used, the data volume grows significantly and depending on the number of sub-scripts can even surpass the Bitcoin Script size limit in the witness structure. On the other hand, when MAST is implemented, the volume stays relatively small and doesn’t go higher than the Bitcoin Script size limit for P2SH.


### Practical application of MAST

MAST can be used for a more optimized implementation of HTLC (Hashed Time-Lock Contracts) which are used in the Lightning Network protocol and for the implementation of Atomic Swap. The concept can also be applied for a more optimized Escrow implementation. MAST makes it possible to implement very large structures using multisignatures.

In many cases, MAST allows to drop OP_RETURN operation that is usually used to add hash values to the blockchain. Instead, this data can be included in the tree and, if necessary, it can be proven that specific data has been recorded in the Bitcoin blockchain. Furthermore, this addition will not affect the size of the transaction.


### Concept development and current status

Russell O’Connor, Pieter Wuille, Peter Todd, and Johnson Lau are the engineers responsible for the initial development and promotion of the MAST concept in the Bitcoin community. In early 2016, a BIP114 proposal was published. It described the specifications for one of the options for implementing this approach using witness programs, which, in turn, were introduced with the SegWit update. BIP114 also offers a software update that adds new consensus rules to the Bitcoin protocol.

Later, in 2017, the developers proposed an alternative implementation of the MAST concept, described in BIP117. It is based on BIP114 with certain modifications. As of 2018, both proposals still remain under consideration.

Note that MAST can be integrated into Bitcoin using softfork protocol update, which is also a very important feature.

### \*\*\*Frequently asked questions\*\*\*

_– Will the MAST root be specified in the witness structure or elsewhere?_

MAST root, together with the data that defines it, will be indicated in the ScriptPubKey at the output of the transaction. This data will take 25–35 B and, most likely, will be easily encoded into a familiar bitcoin address. In the witness structure, where the ownership of coins is proved, the Merkle branch, conditions and data that satisfy these conditions will be indicated.

_– Will available OP_CODEs in the Bitcoin Script language be expanded?_

At the moment it is not yet clear since the proposal is still under consideration and additional changes and improvements may be made. An OP_CODE such as OP_MERKLEBRANCHVERIFY will probably be added for flexibility of using MAST.

_– Is MAST likely to be introduced in Bitcoin shortly?_

It is unlikely. This update is important yet not urgent, so it can wait while developers work on other protocol improvements. Later, the community can integrate several improvements at once in one update, as it was in the case of SegWit.


## 2.3 Taproot operational principles

Schnorr signatures provide a very important feature – the ability to aggregate public keys and signature values. This, in turn, leads to increased network capacity and increased user privacy. The increase in capacity is ensured by saving space in the transaction due to the aggregation of coin ownership proofs. The increased level of privacy means that a third-party observer cannot distinguish a transaction with a single signature from a transaction that uses multisignature.

Another concept aimed at improving these properties is MAST. It allows avoiding the addition of unused coin spending conditions to the transaction, which also saves space in the blockchain and does not reveal alternative conditions while at the same time ensures that the fulfilled condition was set correctly. However, one feature of the MAST concept is that other network members can see whether there are alternative conditions for spending coins (coin ownership proof contains the corresponding Merkle branch). The conditions themselves are not disclosed, but the fact of their presence _is_ disclosed. Therefore, the goal of Taproot is to create and add an improvement to Bitcoin that will allow users to “mask” complex contracts as ordinary transactions.

In this subsection, we discuss how Schnorr signatures can be used to solve this problem. We will consider what is the Taproot concept, how it improves the confidentiality property of contract conditions and what are the possibilities for its implementation in the Bitcoin protocol.


### Procedure for application of alternative conditions

In everyday life, third parties (and contracts) are needed to resolve conflicts between subjects only if such conflicts arise [60]. If the parties agree among themselves, then there is no need to use additional tools. In this regard, most conditions for coin spending are no different from traditional methods to resolve disagreements. The most interesting and commonly used smart contracts are constructed in such a way that one of the alternative scenarios involves distribution of value if all parties sign a transaction (Fig. 2.29).

![Figure 2.29 – A contract in which one condition involves distribution of coins if all parties agree](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.29-contract-in-which-one-condition-involves-distribution-of-coins.png "Figure 2.29 – A contract in which one condition involves distribution of coins if all parties agree")

In the Figure 2.29 above, we see that if the first condition is used, then we do not need the remaining ones and other system participants may not know that they existed. MAST showed how one can save on transaction sizes by publishing only the conditions used (and maintaining the privacy of others). However, it should be reiterated that MAST hides the conditions, but not the fact of their presence.

Let’s try to expand this concept even further and break the contract into two components: a multisignature condition and a set of alternative conditions that are compiled in MAST (Fig. 2.30).

![Figure 2.30 – Contract component separation](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.30-contract-component-separation.png "Figure 2.30 – Contract component separation")

The goal of Taproot is to combine these two alternative scenarios into a uniform coin spending condition. If it is placed in a transaction, these scenarios won’t be disclosed. Let’s look at how Schnorr signatures can be used for this purpose.


### Schnorr Signatures as a core component of Taproot

We are of the opinion that in order to better understand how a separate technology works, one needs to have a simple example that involves it. Imagine a situation where Alice and Bob want to create a custom contract with a lot of complex conditions.

Each participant owns a private key and a public key. Alice has a private key _a_ and public $A=aG$, whereas Bob has a private key _b_ and public $B=bG$. They decide to lock their coins on the contract with the following distribution conditions: either the coins can be spent by agreement between Alice and Bob (multisignature), or by Eve when she reaches the age of 18 (in two years). Alternative conditions will look as given in Figure 2.31.

![Figure 2.31 – Alternative conditions in the contract](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.31-alternative-conditions-in-contract.png "Figure 2.31 – Alternative conditions in the contract")

Suppose Alice and Bob are able to use Taproot. Instead of publishing the above condition, they do the following. First, Alice and Bob aggregate the values of their public keys. After that, they concatenate the aggregated public key with the alternative condition and calculate the hash value of the result. They multiply the resulting hash value by the base point _G_, after which the result is added up with the aggregated key (Fig. 2.32).

![Figure 2.32 – Process of creating Taproot by contracting parties](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.32-creating-Taproot-by-contracting-parties.png "Figure 2.32 – Process of creating Taproot by contracting parties")

The resulting _P_ value will be used as a public key, after which coins will be locked.

Imagine that in the first scenario, Alice and Bob both decided to spend these coins. To do this, they need to provide a signature value that satisfies the public key _P_. How can they do this? Present the value of _P_ in the following form (Fig. 2.33-A).

![Figure 2.33-A – Detailed presentation of the public key](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.33-A-detailed-presentation-of-public-key.png "Figure 2.33-A – Detailed presentation of the public key")

To correspond to the public key _P_, the transaction must be signed using the matching private key [60] (Fig. 2.33-B).

![Figure 2.33-B – Corresponding value of the private key](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.33-B-corresponding-value-of-private-key.png "Figure 2.33-B – Corresponding value of the private key")

Since Alice and Bob know their own private keys, and they also know alternative conditions, the corresponding signature can be calculated as follows: Bob signs the transaction with his own private key, and Alice adds the hash value of an alternative condition to her private key, after which she also signs the transaction with the value she has already received. After that, the parties exchange the received signature values. Since Schnorr signatures allow users to aggregate signature values, a single signature value will be placed in the proof of coin ownership, which corresponds to a single value of the aggregated public key.

However, what will happen if Alice and Bob do not spend coins together? In this case, after two years, Eve can gain access to the coins. Let’s see how this happens.

To prove ownership of the coins, Eve must prove that she knows the correct value of _AggKey_ and the alternative condition, and also that she can satisfy this condition. This scenario is similar to the P2SH approach, except that when a transaction is verified, what is inspected is not whether the condition matches the specified hash value, but whether the following equation is true (Fig. 2.33-C).

![Figure 2.33-C – Equation which is inspected during transaction verification](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.33-C-equation-inspected-during-transaction-verification.png "Figure 2.33-C – Equation which is inspected during transaction verification")

If the equation is satisfied, the verifier checks _Condition_. In our case, this is the verification that the required time period (two years) has passed and the signature verification with Eva’s public key.

> _Note. It is important that the hash value be obtained from the concatenated values of the aggregated key and the condition for spending coins. If a hash value exclusively of an alternative coin spending condition were used (Fig. 2.34), then coins could be stolen by anyone._
> 
> _To do this, an adversary would need to create any condition that he could fulfill, calculate the hash value from this condition, multiply it by the base point, and then calculate the value of the aggregated key as the difference between P and the obtained value. Then, for verifiers, the data of the aggregated key and alternative conditions would be correct._

![Figure 2.34 – Calculation of the hash value based exclusively on spending conditions](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.34-calculation-of-hash-value-based-exclusively-on-spending-conditions.png "Figure 2.34 – Calculation of the hash value based exclusively on spending conditions")

Thus, we see that there are no additional costs when setting the conditions for spending coins using Taproot. In the scenario where Alice and Bob both agreed and performed a transaction, it is no different from the usual one (P2PK or P2PKH). However, if the coins are unlocked by Eve, then the alternative condition and the very fact of using Taproot are revealed.

Note that the alternative condition may not be a script in an explicit form (as it was in the above example), but may constitute a large set of alternative conditions that are stacked in a MAST structure. Let’s take a tree from the previous subsection (the one with 7 alternative conditions) and apply Taproot to it (Fig. 2.35).

![Figure 2.35 – Condition defined using Taproot and MAST](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.35-condition-defined-using-Taproot-and-MAST.png "Figure 2.35 – Condition defined using Taproot and MAST")

When using Taproot, we can ensure that the transaction output this condition contains does not differ from the usual output, which pays a single key/address. However, this will only work if the parties agree among themselves. If the least probable condition is used, then it will be necessary to provide a Merkle branch, this alternative condition and the data that satisfies it (Fig. 2.36).

![Figure 2.36 – Comparative characteristics of the dependence of the size of the conditions on the method of setting them](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.36-comparison-of-dependence-of-conditions-size.png "Figure 2.36 – Comparative characteristics of the dependence of the size of the conditions on the method of setting them")


### Cascades of Taproot scripts

Taproot cascades can be used to build script trees. That is, the alternative branch may contain not a MAST root, but a different public key value, just like the “classic” Taproot [60]. In this case, we will have the following tree (Fig. 2.37):

![Figure 2.37 – Cascade of Taproot scripts](/resources/img/volume-3/2.3-Taproot-operational-principles/F-2.37-cascade-of-Taproot-scripts.png "Figure 2.37 – Cascade of Taproot scripts")

In this case, the proof of coin ownership is no different from the MAST approach. If all parties agree among themselves and form a suitable signature for level 0, then alternative conditions will not be disclosed. If users form a signature suitable for level 1 or a lower level, then the conditions that are located on lower tree levels are not disclosed, but it must be proved that the public key value for a givan level is in the general conditions tree.

When using Taproot cascades it is impossible to achieve much greater efficiency and confidentiality as compared to MAST, but in the next section we will discuss a scheme that allows the use of various branches (scenarios) without even disclosing the existence of alternative ones.

### \*\*\*Frequently asked questions\*\*\*

_– When can Taproot be introduced in Bitcoin?_

Cryptographers and developers will take several months to submit reviews for the proposal. After that, the testing period in Bitcoin testnet will last 2–3 months. With favorable circumstances, the softfork with the Taproot addition can happen at the end of 2021.


## 2.4 Graftroot design

The previous subsection describes how Taproot allows to disguise a complex smart contract as a normal transaction.  The transaction is not disclosed if the parties agree and spend the coins utilizing multisignature. However, Taproot bears one serious limitation: it allows setting only one condition the use of which will not reveal the presence of alternative conditions and the fact that Taproot was used.

In this subsection, we address the concept  named Graftroot, which has been proposed by Gregory Maxwell. Also, we address problems which Graftroot solves and the requirements which must be considered for implementation of the concept.


### Separate signature of each alternative condition

Graftroot, like Taproot, is built upon the assumption each contract, whatever the degree of complication, involves the distribution of coins when parties agree among themselves and collectively sign the transaction. However, unlike Taproot, Graftroot allows the number of alternative conditions to be hidden, aided further by the ability to add additional conditions after the original transaction was initiated [60].

The Taproot concept takes all alternative conditions into account upon calculation of the public key assigned to the coins. On the other hand, Graftroot uses an aggregated key to sign all alternative coin spending conditions individually.

Consider the example from the previous subsection where Alice and Bob decide to lock coins at their common address and provide access to them either through multisignature or to Eva after two years (Fig. 2.38).

![Figure 2.38 – Alternative conditions for spending coins](/resources/img/volume-3/2.4-Graftroot-design/F-2.38-alternative-conditions-for-spending.png "Figure 2.38 – Alternative conditions for spending coins")

As in the previous example, the parties form a common public key aggregating the values of their own public keys. They also create an alternative condition allowing Eva to spend coins. Subsequently, they jointly sign it and give it to Eva. To finalize the process, Alice and Bob form a transaction in which they simply lock coins for the aggregated value of the public key (Fig. 2.39).

![Figure 2.39 – Forming a transaction with locking coins for a specific key](/resources/img/volume-3/2.4-Graftroot-design/F-2.39-locking-coins.png "Figure 2.39 – Forming a transaction with locking coins for a specific key")

How can coins be spent in this scenario? In the first case, Alice and Bob both agree to spend the coins. They simply jointly sign the next transaction. The verifier checks that the signature value matches the aggregated public key value.  If the signature value is valid, coins can be spent (Fig. 2.40).

![Figure 2.40 – Spending of coins using multisignature](/resources/img/volume-3/2.4-Graftroot-design/F-2.40-spending-with-multisignature.png "Figure 2.40 – Spending of coins using multisignature")

To advance the scenario, imagine for a two-year-period Alice and Bob do not spend the coins, and Eva decides to spend them. She, in turn, provides the signed value of the script (the alternative condition), as well as proof of ownership of the coins (Eva’s signature). The verifier then checks the signature of the script. Don’t forget that this script was signed by Alice and Bob using multisignature meaning the public key to verify the signature is the aggregated value of their public keys (it is located in the coin spending terms). If the signature is correct, the verifier is convinced the alternative condition is truly signed by Alice and Bob who agree with its contents (Fig. 2.41).

![Figure 2.41 – Giving Eva a script to unlock coins](/resources/img/volume-3/2.4-Graftroot-design/F-2.41-script-to-unlock-coins.png "Figure 2.41 – Giving Eva a script to unlock coins")

After that, the execution of the script condition is verified. Herein, the verifier checks that the lock time has been reached and that the transaction is signed by Eva. With all checks met, the coins can be spent.

As a result, if all parties agree among themselves, these parties can form a transaction no different from the traditional transaction that pays for one key or address. However, if the alternative condition is incorporated, then only the details of the condition are disclosed while all other conditions maintain their confidentiality. The notable feature of Graftroot is that the size of the proof is not dependent upon the number of alternative conditions (Fig. 2.42) [60].

![Figure 2.42 – Formation of alternative conditions for unlocking](/resources/img/volume-3/2.4-Graftroot-design/F-2.42-alternative-conditions-for-unlocking.png "Figure 2.42 – Formation of alternative conditions for unlocking")


### Ability to add new conditions

Another very important feature of Graftroot is that it doesn’t need to initially identify all possible scenarios for spending coins. Users who participated in the formation of the aggregated public key value can create and sign a new coin spending scenario at any time. In this case, the only requirement is the need for a script to be signed by all participants in the transaction [60].

For example, imagine a situation where Alice and Bob locked their coins at a MultiSig address without any initial alternative conditions. After a while, they quarreled and neither side agreed to concede even a part of the coins from this address to the other side. Eva offers them to solve the situation peacefully and give all coins to her so that they will not go to either Alice or Bob. They agree with this and form a new condition according to which the coins can be spent by Eva. To do this, they do not need to create a new transaction; they just both need to sign the necessary script and pass it to Eva (Fig. 2.43).

![Figure 2.43 – Formation of a new alternative condition after transaction confirmation in the network](/resources/img/volume-3/2.4-Graftroot-design/F-2.43-new-alternative-condition-after-transaction-confirmation.png "Figure 2.43 – Formation of a new alternative condition after transaction confirmation in the network")

After receiving this script and signature, Eva can at any time form a transaction that spends these coins.

The main advantage of Graftroot is that if all parties agree, then such a transaction will not be any different from a normal P2PK transaction. If it is necessary to satisfy another condition for obtaining coins, the fact that it was satisfied will be visible, but there will be no information about other conditions and their number. This is provided because the size of the evidence does not depend on the number of alternative scenarios (in contrast to MAST and Taproot).

When using Graftroot it should be noted that signatures of alternative conditions have to be stored by the involved parties. If such a signature is lost, an alternative condition cannot be fulfilled (i.e. the signature can only be restored by the parties whose public keys were used to obtain the aggregated value).

At the same time, it is worth noting the features for removing alternative conditions, which are similar to changing the conditions for spending coins in case of using the usual multisig transaction. After alternative scripts are signed and sent, they can only be canceled by changing the aggregated value.

If Alice and Bob already sent the signed script to Eva, but changed their minds after that, the only way to save the coins is to generate and confirm the transaction, which pays for the new aggregated key value, before Eva satisfies her condition (Alice and Bob generate new key pairs and form a new aggregated public key, then coins are sent to it) (Fig. 2.44).

![Figure 2.44 – Changing the value of the public key to which coins are assigned](/resources/img/volume-3/2.4-Graftroot-design/F-2.44-changing-value-of-public-key.png "Figure 2.44 – Changing the value of the public key to which coins are assigned")

In this case, Eva will not be able to spend the coins, since the transaction created by her will be conflicting and validators won’t accept it.

### \*\*\*Frequently asked questions\*\*\*

_– Does it make sense to add MAST, Taproot and Graftroot mechanisms to Bitcoin at the same time, or is it better to implement only one of them?_

In fact, it makes sense to add _all_ of these mechanisms, since they have different architectures and properties. At the same time, not all of them can be used in combination. For example, using Taproot with MAST is a very convenient solution (we discussed how it works in subsection 2.3). However, it does not make sense to use MAST with Graftroot since the latter allows to set an unlimited number of different conditions even after the transaction is signed. This is a clear limitation of the protocol – the party to which the script was delegated cannot be sure that a new coin spending script will not be created.

Therefore, if flexibility is required when setting conditions, it is a good idea to use Graftroot. If it is important to ensure the irreversibility of the conditions, using Taproot (possibly in conjunction with MAST, if such an alternative exists) is the better option.

# [3 APPLYING THE CONCEPTS OF SHARDING, OFF-CHAIN, AND DAG](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/3-Applying-the-concepts-of-sharding-off-chain-and-DAG.md)
