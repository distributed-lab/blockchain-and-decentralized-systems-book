# 2 Cryptography in decentralized systems


## 2.1 Key data generation and processing

Cryptography is the most popular data protection mechanism today. It relies only on mathematical rules and does not require the physical security of transmission channels or storage servers. The effectiveness of cryptography is based on two components: the strength of cryptographic algorithms and the security of the key data (keys) [16].

In this subsection, we will look at the basic aspects related to the generation and processing of the key data.

### Basic functions of keys

Before we understand the basic functions of keys, let's first understand what a cryptographic key is. A cryptographic key is a sequence of bits that controls the execution of cryptographic data transformation (cryptographic algorithm) [17].

> * *Data encryption*
> * *Data decryption*
> * *Digital signature calculation (generation)*
> * *Digital signature verification*
> * *MAC (message authentication code) calculation*
> * *MAC verification*
> * *Shared secret calculation*
> * *Additional key data generation*

Let's consider how the security of keys can affect the usage of these cryptographic algorithms. First of all, encryption ensures data confidentiality (the content is hidden from the third party). If a cryptographic algorithm is considered strong enough, then the original message (plain text) can be seen only using an appropriate decryption key.

In the context of digital signatures, cryptographic keys also play an important role. A user signs the initial message with their private key, and the second party verifies the signature with the user's public key. In this case, if an attacker gains access to the user's private key, he will also be able to send messages on the user’s behalf. 
Let's define what a *MAC* (Message Authentication Code) is and why the reliability of a cryptographic key is important for cryptographic algorithms of this kind. The MAC value is much related to the digital signature value, except for the fact that a MAC is calculated and verified using a secret key (Fig. 2.1).

<img align="center" width="70%" src="/resources/img/volume-2/2.1-Key-data-generation-and-processing/Figure-2.1-MAC-function-operation-scheme.png" alt="Figure 2.1 – MAC function operation scheme"/>

From the explanation (and the figure) above you can see that the secret key is involved in both calculating and verifying the message authentication code. This means that if an attacker gains your key, he can send messages on your behalf as well.

Keys are also used to create a shared secret when a shared encryption key is generated using the keys of several users. 

Another option for using the key is to generate child keys, which, in turn, are used in the aforementioned cryptographic operations. This approach is used, for example, in HD wallets.

### Key lifecycle

A cryptographic key passes through a sequence of states that determine its life cycle (Fig. 2.2). The main states are:

* Pending: the key is generated but not activated for usage
* Active: the key is used for cryptographic transformation of data
* Post-active: the key should only be used for decryption or verification

<img align="center" width="40%" src="/resources/img/volume-2/2.1-Key-data-generation-and-processing/Figure-2.2-The-lifecycle-of-a-key.png" alt="Figure 2.2 – The lifecycle of a key"/>

Now, let’s look at processes related to the usage of cryptographic keys:
 * Generation
 * Activation: the key is available for cryptographic transformations and is distributed to the network
 * Suspension: the key usage is limited (the reason may that the key is expired or revoked)
 * Renewal: a previously deactivated key can be used again for cryptographic transformations
Destruction: the end of the key lifecycle (logical key destruction);

First, the key data is generated, then it goes to the pending state, which presumes that it is not being used for cryptographic operations. If the keys have not been used for a long period of time, they can be destroyed.

When the keys are used for cryptographic transformation of data (encryption, signature, etc.), they become active.

When keys are in a post-active state, they can only be used to decrypt messages and to verify digital signatures (or MAC values). The key can move to an active state (when it is used to cryptographically modify data) or be destroyed.

### Key generation principles

Cryptographic keys can be obtained in several ways:
> * *Using a random/pseudo-random number generator*
> * *Derivation from another key (or password)*
> * *Setting a shared key*

### Random sequence generators

Structurally, the main difference between a random sequence generator and a pseudo-random sequence generator is that in the first case, the source of entropy must have a physical noise sensor. 

Examples of random mechanical movements are coin tossing, dices, optoelectronic systems for the Brownian motion tracking.

Due to this difference, random sequence generators are mainly implemented in hardware. In modern random sequence generators, electronic noise sensors with a wide range of frequencies are most often used. Random changes in parameters (thermal noise) are observed in all electronic components at temperatures above Kelvin absolute zero. That’s why any electronic components, such as resistors, silicon diodes, electron tubes, etc., can be used as physical noise sensors. In practice, building a good hardware generator with unbiased and non-correlated sequences is a complex process.

### Pseudo-random sequence generators

As we mentioned earlier, a pseudo-random sequence is a sequence with characteristics that are close to a random sequence, but which can be repeated under certain conditions.

Figure 2.3 shows a model of a deterministic (pseudo-random) sequence generator. By default, each of the elements shown below must be present in the generator.

<img align="center" width="40%" src="/resources/img/volume-2/2.1-Key-data-generation-and-processing/Figure-2.3-Deterministic-sequence-generator-model.png" alt="Figure 2.3 – Deterministic sequence generator model"/>

Taking this into account, let’s describe how a pseudo-random sequence generator works: 

1. The initial value is put into the internal state by the special function (seed) that can be accessed only by authorized entities.

2. The internal state is modified by the internal state modification function that takes the current internal state, any additional data provided by a user, and the output data of the entropy source as input.

3. The output data is calculated by the output data generation function that takes a modified internal state as input.

4. The internal state modification function (re-seed) updates the internal state when a new initial value is given, and depending on this function, the current internal state may either depend on the previous one or not.

The main requirements for pseudo-random sequence generators are unpredictability, recoverability, time complexity, irreversibility, statistical properties that are not distinguishable from the random sequence, and the repetition period that must be larger or equal to the specified one.

### Testing random and pseudo-random sequence generators

It is worth noting that for random and pseudo-random sequence generators, it is required to verify the randomness of an output.

As an example, we use NIST STS, the most widely used statistical test among the developers of tools for cryptographic information protection. It is used to evaluate the randomness of random and pseudo-random sequence generators. The NIST STS package has 15 statistical tests, according to which the probability vector *P = {P1, P2, …, Pn}* is generated as a result of the test; the analysis of the *Pi* components allows indicating specific defects of randomness within the tested sequence. The list of tests and their general description is provided in Table 2.1 [67].

Table 2.1 — List of tests and their general description

| # |  Test name  | Test description |
|:-:|-------------|------------------|
| 1 | *Bits frequency*|The essence of this test is to determine the ratio between zeros and ones in the entire binary sequence. The goal is to find out if the number of zeros and the number of ones in the sequence is nearly the same.|
| 2 | *Frequency within a block*| The essence of the test is to determine the fraction of ones inside a block with a length of m bits. The goal is to find out if the repetition frequency of units in a block of the m bits length is approximately equal to m/2.|
| 3 | *Sequence of the same bits*      | The essence is to calculate the total number of rows in the original sequence, where a row means a continuous subsequence of the same bits. The purpose of this test is to conclude whether the number of rows consisting of ones and zeros with different lengths actually corresponds to their number in a random sequence.     |
| 4 |*Longest run of ones*| This test determines the longest run of ones inside a block with a length of m bits. The goal is to find out if the length of such a run actually meets the expectations for the length of the longest run of ones in an absolutely random sequence.|
|5  | *Binary matrix rank*              | Here, the ranks of disjoint submatrices constructed from the original binary sequence are calculated. The goal of this test is to check for a linear dependence of substrings of fixed length that make up the original sequence. |
|6  | *Spectral*                        | The essence of the test is to estimate the height of the original sequence’s discrete Fourier transformation peaks. The goal is to identify the periodic properties of the input sequence, for example, closely located repeating sections.|
|7  | *Non-overlapping templates*       |  This test counts the number of predetermined templates found in the original sequence. The goal is to identify random or pseudo-random number generators that produce non-periodic templates that are set frequently.|
|8  | *Overlapping templates*           | The essence of this test is to count the number of predetermined templates found in the original sequence. The difference between this test and test # 7 is that if a template is found, the frame moves only one bit forward, and after that the search continues.|
|9  |*Maurer's universal*              | Here, the number of bits between the same templates in the original sequence is determined (a measure directly related to the length of the compressed sequence). The purpose of the test is to find out if a given sequence can be significantly compressed without the loss of information. |
|10  | *Linear complexity*               | The test is based on the principle of operation of a linear feedback shift register (LFSR). The goal is to find out if the input sequence is complex enough to be considered absolutely random.|
|11  | *Serial*                          | This test implies counting the frequency of all possible overlapping templates of the m bits length throughout the initial sequence of bits. The goal is to determine whether the number of occurrences of 2m overlapping templates of the m bits length is approximately the same as with a completely random input bit sequence|
|12  |*Approximate entropy*                | Like the serial test, this test focuses on counting the frequency of all possible overlapping templates of the m bits length throughout the initial sequence of bits. The purpose of the test is to compare the overlap frequencies of two consecutive blocks of the original sequence with the lengths m and m+1 with the overlap frequencies of similar blocks in an absolutely random sequence.|
|13  | *Cumulative sums*                   | The test implies the maximum deviation (from zero) during an arbitrary round determined by the cumulative sum of the given (-1, +1) digits in the sequence. The purpose of this test is to determine whether the cumulative sum of the partial sequences occurring in the input sequence is too large or too small compared to the expected sum for an absolutely random input sequence. |
|14  | *Random excursions*                  | The essence of this test is to calculate the number of loops that have strictly k hits for an arbitrary round of a cumulative sum. The purpose of this test is to determine whether the number of hits of a certain state within a loop is different from the corresponding number for an absolutely random input sequence. |
|15  | *Random excursions variant*         | In this test, the total number of hits of a certain state for an arbitrary round of a cumulative sum is calculated. The goal is to determine deviations from the expected number of hits to various states during an arbitrary round. |

### Key derivation function (KDF)

A key derivation function is a function that generates one or more secret keys based on a specific secret value using a pseudo-random function. The operation principles of such functions are very similar to the principles of hash functions, which means that the key derivation is a one-way function (memory-hard cryptographic hash functions are often used as a pseudo-random function). The fact that the key derivation function only works one way does not allow us to find out the original secret value or any other derived keys. 

Password-based KDFs are often used to hash passwords and verify them later. In this case, the salt value is used as a parameter along with the secret password.

**Frequently asked questions**

*– How secure is it to use a password to derive a secret key?*

To derive the key data from a password, a random long password is recommended to use. Thus, if you use a random password with a length of, for example, 20 characters (while using upper and lower case letters and numbers), then in order to find out this password and get the key information, an attacker will need about $2^{120}$ attempts, which is quite acceptable (especially when using memory hard hash functions). However, in practice, such kind of password is difficult to remember, users often choose a much smaller value, which, in addition, is not random but is associated with a birth date, a pet name, etc.

*– What tests for random sequences exist, except for NIST STS?*

In addition to NIST STS, there are many different tests for random sequence generators. The most popular ones are the  following:

* Diehard: the "classic" set of statistical tests developed by George Marsaglia and first published in 1995
* TestU01: a set of statistical tests similar to Diehard. It includes implementations of "classical" and several third-party statistical tests proposed in the literature
* Dieharder: A set of statistical tests, including all tests from Diehard, NIST STS, and several additional tests developed by RGB

*– What is the time and memory compromise attack?*

The time and memory compromise is an approach that allows increasing or decreasing the computation time by reducing or, respectively, increasing the amount of memory used. In cryptography, an attack based on the compromise of time and memory is used to speed up the brute force attack. For example, consider the lookup tables for finding the preimage of a hash function. If you initially perform an extensive brute force, you can store the obtained values in memory and perform the subsequent lookup brute force instantly.

## 2.2 Key exchange protocols

One of the main tasks when exchanging keys or other information is to carry out the exchange process in a way that no unauthorized party can access the data contents in the channel. Typically, this requires the presence of a trusted third party or any other secure channel.

*Key exchange protocols* are used to create a secure communication channel between users. They include key distribution protocols and key agreement protocols.

*A key agreement protocol* is an established sequence of user actions to create a secure communication channel by generating a shared secret key. The main feature is that each party makes an equal contribution to the creation of a shared secret key.

*A key distribution protocol* is the established sequence of user actions to create a secure communication channel, which implies generating and exchanging session keys and authenticating messages.

The main objective of the key distribution protocols is the derivation of a shared key by the participants (Alice and Bob). At the same time, both Bob and Alice must ensure that the communication is conducted only between themselves, and not with an attacker or a dummy. Most of such protocols are based on the existence of a trusted authority (Trent), and it is assumed that Trent derives a secret key for every user. Thus, before the protocol starts operating, all keys are already possessed by the users.

All key distribution protocols are divided into the following (overlapping) categories:
> * *Protocols based on symmetric cryptography*
> * *Protocols based on asymmetric cryptography*
> * *Protocols using a certification authority (trusted authority)*

There are also the following key distribution models:
> * *Exchanging keys in person*
> * *Using the previous key*
> * *Using a trusted third party*

However, all these methods ultimately come down to the subjects' physical presence.

With symmetric encryption, two participants who want to exchange confidential information must have the same key. The key must be changed frequently so that an attacker does not have enough time to find the key through complete bruteforce. Consequently, the strength of any cryptosystem primarily depends on the key distribution technology. This term means transmitting a key to the respective  participants who want to exchange data in a way that no one else can capture or modify this key. For two participants, Alice and Bob, the key distribution can be done in one of the following ways:

> * *The key can be created by Alice and physically transferred to Bob (or vice versa)*
> * *A third party can create a key and physically transfer it to Alice and Bob*
> * *Alice and Bob have a previously created key;*
> * *If both Alice and Bob have a secure connection with a third participant (Carol), then she can transfer the key through this secure channel from Alice to Bob*

The first and second methods are called *manual key distribution*. These are the most reliable ways to distribute a key, but in many cases they are inconvenient or even impossible to use. In a distributed system, any node or server must be able to exchange confidential information with many authenticated nodes and servers. Thus, each node must have a set of keys that is supported dynamically. The problem is especially relevant in large distributed systems.

The number of keys required depends on the number of participants who need to interact. If encryption is performed at the network or transport level, a key is required for each pair of network nodes. Thus, if there are *N* nodes, then the required number of keys is
$\frac{N(N-1)}{2}$. If encryption is performed at the application level, then the key is needed for each pair of application processes, whereas there are significantly more processes than nodes.

The third key distribution method can be applied at any level of the protocol stack, but if an attacker gains access to one key, then the entire sequence of keys will be revealed. Moreover, the initial distribution of a large number of keys must still be carried out.

Therefore, in large automated systems, various options of the fourth method are commonly  used. These options assume the presence of a so-called key distribution authority (KDA), which is responsible for the key distribution for nodes, processes, and applications. Each participant must share a unique key with the KDA.

Using a KDA relies on using a key hierarchy. At least two types of keys are used: master keys and session keys.

To ensure confidential communication between end systems, a temporary key, the session key, is used. Typically, the session key is used to encrypt the connection at the transport level; after that, it is destroyed. Every session key must be received over the network from a key distribution authority. Session keys are transmitted in encrypted form using a master key that is shared between the key distribution authority and end users.

These master keys must also be distributed in some secure way. At the same time, the number of keys requiring manual distribution is significantly reduced. If there are *N* participants who want to establish connections, then at each moment in time $\frac{N(N-1)}{2}$ session keys are needed, but only *N* master keys (one for each participant).

The lifetime of a session key is usually equal to the lifetime of the session. The more often session keys are changed, the more secure they are, since an adversary has less time to hack the key of the current session. On the other hand, the session key distribution delays the start of any exchange and congests the network. A security policy is required to balance these conditions and to determine the optimal lifetime of a particular session key.

If the connection has a long lifetime, then there must be the possibility to periodically change the session key.

For non-connection protocols, such as transaction-oriented protocols, there is no explicit initialization or interruption of the connection. Therefore, it is unclear how often the session key must be changed. Most approaches rely on the use of a new session key for each new exchange. The most commonly used strategy is to use the session key only for a fixed period of time or only for a certain number of messages signed.

### Diffie–Hellman protocol

The Diffie–Hellman Protocol (DH) is a cryptographic key agreement protocol that allows two or more parties to get a shared secret key using a communication channel that is not protected from eavesdropping. The obtained key is used to encrypt the further exchange of keys using symmetric encryption algorithms.

The open key distribution scheme proposed by Bailey Whitfield Diffie and Martin Edward Hellman was revolutionary in the sphere of encryption, as it solved the main problem of classical cryptography – the problem of key distribution.

In its pure form, the Diffie–Hellman protocol is vulnerable to data modification in the communication channel, including the man-in-the-middle attack, therefore, schemes using it apply additional methods of unidirectional or bidirectional authentication.

In the Diffie–Hellman protocol, which is used to generate a shared secret of long-term keys, there are two big prime numbers as the public parameters $p$ and *q* (*p* is the module value, and *q* is the group generator).

Imagine that there is a transmission channel between three subjects: Alice, Bob, and Carol. Alice and Bob need to create a shared secret that Carol will not know; at the same time, Carol can see everything they transmit over the communication channel. Here are the steps to create a shared secret:
1. Alice and Bob randomly generate private keys *1 < a < p* and *1 < b < p* respectively.
2. Using the private keys, each of the users creates its own public key *A = q<sup>a</sup> mod p* (Alice), *B = q<sup>b</sup> mod p* (Bob). 
3. The public keys are transmitted over the data channel.
4. Having exchanged public keys, Alice and Bob can form a shared secret *S = B<sup>a</sup>  mod p = A<sup>b</sup> mod p = g<sup>ab</sup> mod p* (Fig. 2.4).

<img align="center" width="50%" src="/resources/img/volume-2/2.2-Key-exchange-protocols/Figure-2.4-Diffie-Hellman-protocol-operation-scheme.png" alt="Figure 2.4 – Diffie–Hellman protocol operation scheme"/>                     

Using the shared secret *S* and one key derivation function (we call it *fk* here), each of the parties can generate an agreed key *K*, for example, *K = fk* (*S*, *par*), where par is the parameters of the agreed key. Greater cryptographic strength can be achieved by separately generating session keys according to the same principle at each communication session.

Note that this algorithm is vulnerable to man-in-the-middle attacks. If the opponent can carry out an active attack, it means he can not only capture messages but also substitute them, he can capture the public keys of the participants *A* and *B*, create his own pair of a public and a private key, and send his own public key to each participant. After that, each participant will calculate a key that will be shared with the adversary and not with another participant. If there is no integrity control, then participants will not be able to detect such a substitution. The implementation of such an attack requires a large amount of resources, and such attacks occur rarely in the real world.

### Elliptic curve Diffie–Hellman protocol

The Diffie–Hellman protocol can also work on elliptic curves (ECDH). In this case, Alice and Bob have the private key *a*, *b* and the public key *Q<sub>a</sub>*, *Q<sub>b</sub>*, respectively. Each of the public keys is obtained by performing the addition of point *G* *n* times. 

1. Alice and Bob generate public keys *Q<sub>a</sub> = а ∗ G*, *Q<sub>b</sub> = b ∗ G*. 
2. Alice and Bob exchange public keys.
3. Having received each other's public keys, each of them calculates (*X<sub>s</sub>*, *Y<sub>s</sub>*) = *a ∗ Q<sub>b</sub> = b ∗ Q<sub>a</sub> = a ∗ b ∗ G*, where point *X<sub>s</sub>* is the shared secret. Most standard protocols based on ECDH use key derivation functions to obtain a symmetric key from a value *X<sub>s</sub>* (Fig. 2.5).

<img align="center" width="50%" src="/resources/img/volume-2/2.2-Key-exchange-protocols/Figure-2.5-Scheme-of-interaction-according-to-ECDH-protocol.png" alt="Figure 2.5 – Scheme of interaction according to ECDH protocol"/>                     

Regarding all the information associated with the private key, Alice and Bob only disclose their public keys. In order for Carol to find out a shared secret as well as Alice's and Bob's secret keys, she will need to solve the problem of finding a discrete logarithm on an elliptic curve, which can take quite long time.

### Encrypted key exchange protocol

*The encrypted key exchange (EKE) protocol* is *a key distribution protocol* that provides the ability to securely exchange data between network users using symmetric and asymmetric cryptography. The protocol assumes that Alice and Bob share a secret $S$ (it can even be the one with low entropy, e.g., a password) and that by using this protocol, they can authenticate each other and generate a session key $K$ (Fig. 2.6). 

<img align="center" width="50%" src="/resources/img/volume-2/2.2-Key-exchange-protocols/Figure-2.6-Scheme-of-operation-of-the-EKE-protocol-participants.png" alt="Figure 2.6 – Scheme of operation of the EKE protocol participants"/>    

Let's describe each of the steps of the protocol operation:

1. Alice generates a key pair and encrypts the public key *A* from this pair with shared secret key *S*. After that, Alice sends an encrypted message to Bob. It is worth noting that public key *A* is transmitted in encrypted form so that only Bob will be able to generate a message encrypted with this key.
2. Having received a message from Alice, Bob decrypts it and obtains *A*; after that, he generates symmetric key *K*, which will be the session key shared with Alice. Bob encrypts *K* with key *A*, received from Alice, and, after that, with shared secret key *S*.
3. Having received a response from Bob, Alice decrypts this message and obtains session key *K*. After setting the session key, the process of mutual authentication is initiated. Alice generates a random value *R<sub>a</sub>*, encrypts it with *K*, and sends it to Bob.
4. Bob decrypts this message, obtains *R<sub>a</sub>*, and generates his random value *R<sub>b</sub>*. Then Bob concatenates *R<sub>b</sub>* with *Ra* and encrypts the result with key *K*. Next, he transmits the final value to Alice.
5. Alice decrypts Bob's message, makes sure that *R<sub>a</sub>* is the same as she sent to Bob in Step 3, and answers Bob with a message encrypted with *K* and containing his *R<sub>b</sub>*.
6. Bob decrypts Alice's answer and makes sure that *R<sub>b</sub>* is the same as he sent to Alice in Step 4. 
7. Once the protocol is completed, both parties further use session key *K* to exchange information.

**Frequently asked questions**

*— How is a man-in-the-middle attack conducted?*

As we mentioned earlier, each party transmits its public key to the other party. In fact, if there is no mechanism for ensuring authenticity of the messages transmitted, then the party cannot make sure that the key it received really belongs to the second side. As a result, the attacker can send his public key values to both parties and generate a shared secret with each of them. After that, he can act as a repeater: receive a message from one party, read it, and transmit it to the other party. Moreover, the attacker can modify these messages.

*— Can I use DH protocol for more than two participants?*

Yes, this protocol allows generating a shared secret between any number of parties. However, for a larger number of participants, the interaction protocol is more complex and requires performing a larger number of subsequent steps.

*— What are the benefits of using a public key infrastructure together with DH protocol?*

When using DH protocol with no authentication mechanisms, the man in the middle can impersonate the target party with which the key is agreed. Thus, Alice exchanges public keys and generates a shared secret not with Bob but with an adversary. Therefore, the DH protocol is often used in connection with a public key infrastructure, which is a source of information about public keys and their owners.

## 2.3 Merkle tree concept and application

The concept of building data trees was published in 1979 by Ralph Merkle. Further, it has been widely used both for checking the integrity of large data amounts and in version control systems and cryptographic digital signature algorithms.

We already mentioned Merkle trees in the first part of the textbook. In this section, we will describe in detail the process of verifying data using Merkle trees, delve into the properties that such a structure can provide, and consider the main areas of application of this concept.

### Structure of a Merkle tree
The Merkle tree is one of the basic methods to hash large amounts of data. In general, the Merkle tree structure consists of the components depicted in Figure 2.7.

>**Components of the Merkle tree structure**
>> * *Merkle root*
>> * *Merkle nodes*
>> * *Merkle leaves*

<img align="center" width="60%" src="/resources/img/volume-2/2.3-Merkle-tree-concept-and-application/Figure-2.7-Merkle-tree-structure.png" alt="Figure 2.7 – Merkle tree structure"/>    

Merkle leaves are hash values of some data. Each leaf and node of one tree is equal in size, and this size depends on the hash function used. The number of leaves in a Merkle tree is determined by the value equal to 2h, where h is the Merkle tree height (in the Figure 2.7, the Merkle tree height is 2).

*A Merkle node* is a hash value from the concatenation of two child leaves (or nodes). The size of each node is the same as the size of the leaves and is also determined by the parameters of the used hash function.

*A Merkle root* is a node that is placed on the top of a tree. A feature of a Merkle root is its association with all the child nodes and leaves (changing one of the nodes or leaves of the tree will change the root value).

The basic idea of building a Merkle tree can be described with one equation:

**Hash<sub>i+1, j</sub> = Hash(Hash<sub>i, 2j</sub> || Hash<sub>i, 2j+1</sub>)**

In plain words, in order to obtain nodes of the next level, nodes (or leaves) at the current level are pairwise concatenated and hashed.

The structure of the Merkle tree has a number of useful features:
> * Changing even just one bit in any of the data blocks will significantly (and unpredictably) change the Merkle root value
> * When the integrity of one of the data blocks is violated, it can be accurately and quickly determined which block has been modified
> * Authentication is easy and it requires only a small size for the proof of the presence of a particular block in the Merkle tree structure

### Building a Merkle tree

Let's look at an example of how exactly a Merkle tree is constructed. Suppose we have 4 data blocks containing numbers: "1", "2", "3", and "4". To build the Merkle tree, we need to calculate the hash value for each of the data blocks, as in Figure 2.8 (here, SHA-256 hash function was used).

<img align="center" width="40%" src="/resources/img/volume-2/2.3-Merkle-tree-concept-and-application/Figure-2.8-Hashing-of-data-blocks.png" alt="Figure 2.8 – Hashing of data blocks"/> 

The resulting hash values are the Merkle leaves. Next, they need to be linked into nodes. For this, value pairs are concatenated and the resulting value is also hashed (Fig. 2.9).

<img align="center" width="40%" src="/resources/img/volume-2/2.3-Merkle-tree-concept-and-application/Figure-2.9-Obtaining-of-Merkle-nodes.png" alt="Figure 2.9 – Obtaining of Merkle nodes"/> 

As a result, we get two nodes, which now in the same way need to be linked into one Merkle root value. Obtaining the Merkle root value is identical to obtaining a particular Merkle node (Fig. 2.10).

<img align="center" width="40%" src="/resources/img/volume-2/2.3-Merkle-tree-concept-and-application/Figure-2.10-Obtaining-of-Merkle-root.png" alt="Figure 2.10 – Obtaining of Merkle root"/> 

Now we get one root value that links all the blocks of the source data. Using this example suffices to explain how the integrity of all the components of a Merkle tree is checked.

Suppose an attacker wanted to modify the last data block to "5". The attacker's motivation in this case is not clear; however, it matters not. It is important that he cannot convince everyone that the data has not been modified. This feature is achieved through the use of hash functions in the Merkle tree.

When an attacker changes the target data, the hash value changes making it visible. Since this leaf is concatenated with the neighboring one and the resulting value is hashed, the node value will also change. As a result, the Merkle root value changes completely (Fig. 2.11).

<img align="center" width="60%" src="/resources/img/volume-2/2.3-Merkle-tree-concept-and-application/Figure-2.11-Merkle-root-value-modification-depending-on-the-input-data-block-modification.png" alt="Figure 2.11 – Merkle root value modification depending on the input data block modification"/> 

You can test this example yourself and make sure that the root value is indeed completely changed. An attacker will succeed in modifying the data only if a collision is found (such a dataset which, after being hashed, gives the value identical to the target hash value) – in fact, only if the cryptographic hash function is hacked.

### Authentication in a Merkle tree

Another feature of the Merkle tree is a quick and simple — even for large amounts of data — verification that allows one to know whether some specific set of data is present in the tree structure. For this, the verifier only needs a Merkle root value and a set of values called Merkle branch.

What is a Merkle branch? For example, the verifier wants to make sure that a certain dataset is present in the Merkle tree structure. He has the data (and, accordingly, its hash value) as well as the target Merkle root value.

One of the options is to request all data blocks from the node, recalculate the value and compare it with your own Merkle root. The method is reliable but requires a lot of data to be transmitted.

Therefore, there is another way to check the presence of a data block in the Merkle tree structure while having a small dataset called Merkle branch. A Merkle branch contains a set of hash values (leaves and nodes) sufficient to authenticate a specific data block. How is this set of values obtained? The node containing the necessary data blocks recalculates the Merkle tree for them and selects only the values that the verifier will need to obtain the Merkle root (pairwise concatenation and hashing) for a particular leaf; then it transmits the selected values. Having received a set of values and having its own data block, the verifier calculates the Merkle root value and compares it with the one that it stores by itself (Fig. 2.12).

<img align="center" width="60%" src="/resources/img/volume-2/2.3-Merkle-tree-concept-and-application/Figure-2.12-Merkle-branch-calculation.png" alt="Figure 2.12 – Merkle branch calculation"/> 

The figure above shows the verification of the third dataset (in our example, the value is "3") for the presence in the Merkle tree structure. The verifier stores the hash value of this data block (4E0740…) and obtains the Merkle branch, consisting of the hash value of the neighbouring data block (4B2277…) and one of the Merkle nodes (in this case, F5FC2A…). The verifier concatenates the stored value with the first Merkle branch value and hashes the result. The resulting value is concatenated with the second Merkle branch value, and the result is hashed again. The resulting value is compared with an existing Merkle root, and if the values are equal, then such a data block is actually present in the Merkle tree structure.

> **_NOTE:_** *There are multiple authentication schemes that allow checking the presence of not just one but several data blocks at once (thereby* 
> *increasing the algorithm performance). Such schemes include, for example, the Octopus Authentication scheme.*

### Application area of Merkle trees

Now let's observe the areas in which the Merkle Tree concept has been applied.

> * *Digital signature algorithms*
> * *Decentralized file sharing systems (BitTorrent, IPFS)*
> * *Blockchain and SPV nodes*
> * *Version control systems (Git)*

*Digital signatures* based on hash functions can potentially be used instead of the ones that are common today, as traditional digital signatures are vulnerable to quantum computer attacks. The peculiarity of this type of signatures is that they do not require expensive calculations and are based only on the strength of the hash functions used.

Another area of application for Merkle trees are *decentralized file sharing protocols* such as BitTorrent and IPFS. The BitTorrent protocol allows checking the downloaded file fragments for integrity by assigning a digital fingerprint to each data block (the result of the SHA-1 hash function). A related problem is that some amounts of related data are very big and, accordingly, the *"imprint"* of these data is also big.

To keep the size of a torrent file small, the Merkle tree scheme is used. We can use it to join hashes from different fragments into a single Merkle root. As a result, we get one hash value that allows checking the integrity of the entire data block. In the same way, according to the hierarchical scheme, particular Merkle nodes enable  integrity checks of particular blocks. 

In IPFS, all data is represented as a merkelized directed acyclic graph (MDAG), which allows a user to retrieve the entire dataset (down to the contents of directories) associated with a single unique identifier.

The Merkle tree construction scheme has received the most attention since January 3, 2009, with the appearance of Bitcoin. In "Bitcoin: A Peer-to-Peer Electronic Cash System" [18], the following scheme is given (Fig. 2.13).

<img align="center" width="50%" src="/resources/img/volume-2/2.3-Merkle-tree-concept-and-application/Figure-2.13-Use-of-Merkle-trees-in-Bitcoin.png" alt="Figure 2.13 – Use of Merkle trees in Bitcoin"/> 

This diagram depicts the block structure in Bitcoin. In its header, there is a field called *"Root Hash"*. What does it mean?

Every block in Bitcoin contains up to several thousand transactions. These transactions are hashed and form the leaves of the future Merkle tree. Further, the leaves are pairwise concatenated and hashed until a single root value is obtained. It is exactly the value placed in the block header, in the field called "Root Hash". This scheme allows checking whether a transaction is included into a block.

According to this principle, *the SPV (simplified payment verification)* method works. Every SPV node stores all the block headers (each with a size of 80 bytes). The block headers contain the Merkle root value from all transactions that have been added to the corresponding blocks. If an SPV node wants to make sure that the transaction has been confirmed, it addresses the full node and requests the Merkle branch for a specific transaction (Fig. 2.14). As soon as the Merkle Branch is received, the verifier will build the authentication path to the Merkle root value (which is stored by it, since it synchronizes with other network nodes).

<img align="center" width="40%" src="/resources/img/volume-2/2.3-Merkle-tree-concept-and-application/Figure-2.14-SPV-node-operation-scheme.png" alt="Figure 2.14 – SPV node operation scheme"/> 

**Frequently asked questions**

*– Why use the Merkle tree if the entire data sequence can be hashed?*

If we hash the entire dataset, we can ensure its integrity as well. However, in this case, there is no flexibility in disclosing part of this data. If we hash the entire dataset, the verifier can inspect that the data matches the hash value only with the entire set. The use of Merkle trees allows proving that one data fragment is included in the entire structure. This approach is much less demanding to the total amount of proofs, and it also makes it possible to ensure the confidentiality of the entire dataset by just proving the integrity of its fragment.

*– Can any node have more than two child nodes?*

In fact, there is such a case where we can build a tree in which each node will have more than two children. However, this will significantly reduce the effectiveness of this approach and increase the size of proofs for authenticating a particular dataset.

## 2.4 Digital signature types

A digital signature is a mechanism that allows ensuring the integrity and authorship of a signed message. However, some areas of their application require ensuring additional properties that digital signatures typically cannot provide. Therefore, a number of new approaches have been proposed, which we examine in this subsection.

> * *One-time signature*
> * *Multisignature*
> * *Threshold signature*
> * *Group signature*
> * *Ring signature*
> * *Blind signature*

### One-time signature schemes

A one-time signature scheme was first proposed by Leslie Lamport in 1979. The foundation of such schemes is the use of one-way functions (hash functions). A one-time signature implies using one key pair to sign only one message. During signing, the schemes imply the publication of private key fragments. If the same key pair is used several times, then more and more parts of the private key are disclosed, and then they can be used by an attacker to recover someone else's private key and generate signatures for other messages on behalf of the target user.

To understand the basic principles of the operation of one-time signatures, we will consider two basic algorithms for one-time signing (LOTS (Lamport One-Time Signature) [19] and WOTS (Winternitz One-Time Signature) [20]), the features of calculation and verification of signatures in both algorithms, their basic features as well as what the attacker can exploit to forge particular user's signatures.

### Lamport one-time signature

First of all, a user needs to generate a key pair with which signatures will be generated and verified. Lamport signature scheme [21] assumes that the user's private key consists of a paired set of randomly generated secret values. Their number is determined by the size of the signed message (namely, by the length of the hash value). If the output value length of the used hash function is n bits, then 2n secret values must be generated to sign the corresponding message. The private key is shown in Figure 2.15.

<img align="center" width="60%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.15-Private-key-formation-scheme.png" alt="Figure 2.15 – Private key formation scheme"/> 

For example, if we generate every secret value of 256-bit length and use a hash function of 256-bit length, then the length of our private key should be about 8 kilobytes, which is a fairly big data amount compared to the keys used 256–512 bit long in traditional schemes.

The public key is calculated as the concatenated value from the hash values of the generated secrets. Its length depends on the hash function used to generate it (if the output of a hash function is equal to the size of the secret, then in this case, the sizes of public and private keys will be the same). The process of generating a public key from a private key is shown in Figure 2.16.

<img align="center" width="70%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.16-Scheme-of-generating-a-public-key-from-a-private-key.png" alt="Figure 2.16 – Scheme of generating a public key from a private key"/> 

After the public key is calculated, it can be published, transmitted to the verifier, propagated over the network, etc. How is the signature value calculated in this case? After the signed message is generated, a hash value is calculated from it. To sign a message, we must publish one of the secret values in a pair (Fig. 2.17) depending on a specific bit of the hash value (whether it equals "0" or "1").

<img align="center" width="60%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.17-Digital-signature-calculation-scheme.png" alt="Figure 2.17 – Digital signature calculation scheme"/> 

As a result, exactly half of the initially generated secrets is published. How can it now be verified whether the signature is valid? The verifier can calculate the message hash and get the same bit sequence. Depending on this sequence, the verifier selects a set of public keys according to the value of specific bits of the obtained hash value (Fig. 2.18).

<img align="center" width="80%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.18-Public-key-selection-scheme.png" alt="Figure 2.18 – Public key selection scheme"/> 

After the verifier has generated a set of necessary public keys, he calculates the hash values from parts of the digital signature, and if the hash values of all these values match the public key generated in the previous step (Fig. 2.19), then the signature is valid.

<img align="center" width="60%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.19-Signature-verification-scheme.png" alt="Figure 2.19 – Signature verification scheme"/> 

### Why is a signature called one-time?

Let's look at a simple example in order to understand how a one-time Lamport signature works and why it is impossible to sign several messages with one key. We will use a hash function that returns a four-bit value (we omit the calculation algorithm: we only know that the hash function output is the same for the same input data).

First, we generate a private key. Since the hash length is four bits, we need to generate eight secret values (Fig. 2.20).

<img align="center" width="50%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.20-Example-of-generating-a-private-key.png" alt="Figure 2.20 – Example of generating a private key"/> 

After that, it is necessary to calculate the value of the public key by hashing the secret values (Fig. 2.21) and send it to the verifier.

<img align="center" width="50%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.21-Example-of-public-key-calculation.png" alt="Figure 2.21 – Example of public key calculation"/> 

When a user forms a message, he calculates its hash value. Next, the digital signature is *generated*. For example, the resulting hash value is "0110". In this case, the signature value constitutes parts of the private key depending on the hash value bits (Fig. 2.22).

<img align="center" width="60%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.22-Example-of-calculating-the-first-digital-signature.png" alt="Figure 2.22 – Example of calculating the first digital signature"/> 

After publishing this digital signature value together with the message, virtually everyone will recognize half of the generated secret values: *X<sub>0</sub>*, *Y<sub>1</sub>*, *Y<sub>2</sub>*, *X<sub>3</sub>*. An attacker can save these values; after that, it will be easier for him to guess the remaining parts of the victim’s private key.

If the same user took the same key sequence to sign another message, then the hash value of this message will also be different. Consequently, the positions of secrets necessary for publication change as well. This means that following the publication of a new value of the digital signature, another portion of the private key components will be disclosed. For example, if a user takes the same private key for the second time to sign a message with hash value "1001" (Fig. 2.23), then he needs to disclose the secrets at the following positions: *Y<sub>1</sub>*, *X<sub>1</sub>*,  *X<sub>2</sub>*, *Y<sub>3</sub>*.

<img align="center" width="60%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.23-Example-of-calculating-the-second-digital-signature.png" alt="Figure 2.23 – Example of calculating the second digital signature"/> 

If an attacker has captured both messages and the values of their signatures, then he owns a full set of initially generated secrets. It means that he can sign any messages on behalf of the target user. Since the owner of the private key is not interested in disclosing it, he can use one key to generate no more than one signature.

### Winternitz one-time signature

Another example of a one-time signature is the Winternitz OTS [20]. This approach is very similar to the Lamport signature; however, it assumes a separate secret value for one message block rather than for each bit of a signed message. Therefore, the Winternitz parameter (width) is initially determined, which reflects the size of message blocks. For example, if we set the Winternitz width to 16 (4 bits) and use the SHA-256 hash function, then the original hash value is divided into 64 blocks of 4-bit length.

After that, we generate private key values. In this case, the number of keys matches the number of blocks (Fig. 2.24) and is also equal to 64.

<img align="center" width="60%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.24-Private-key-generation-scheme.png" alt="Figure 2.24 – Private key generation scheme"/> 

*A public key* is calculated as a concatenation of hash values of the private key parts. Moreover, the number of necessary calculations of the hash function is equal to the value of the Winternitz width. Based on this, the public key fragments are equal to the private key fragments used to calculate the hash value 16 times (Fig. 2.25).

<img align="center" width="60%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.25-Public-key-calculation-scheme.png" alt="Figure 2.25 – Public key calculation scheme"/> 

*The public key* is generated and can be transmitted to verifiers. When a message is signed, its hash value is pre-calculated and divided into 4-bit blocks. After that, each block is converted to a decimal value (Fig. 2.26).

<img align="center" width="60%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.26-Message-pre-processing-scheme.png" alt="Figure 2.26 – Message pre-processing scheme"/> 

*A digital signature* is a private key value for each block that is hashed as many times as the decimal value obtained after the conversion indicates (Fig. 2.27).

<img align="center" width="80%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.27-Digital-signature-calculation-scheme.png" alt="Figure 2.27 – Digital signature calculation scheme"/> 

When the verifier receives the signature value, it pre-calculates the hash value of the received message and divides it into blocks. After that, it converts the value of each block into a decimal form, subtracts the decimal values from the Winternitz width value, and gets its own set of natural numbers. Further, the verifier hashes the values of the signature fragments according to an obtained set of natural numbers (as many times as the result of subtracting the block decimal values from the Winternitz width indicate). If each resulting hash is equal to the corresponding part of the public key, then the verified signature is correct (Fig. 2.28).

<img align="center" width="60%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.28-Signature-verification-scheme.png" alt="Figure 2.28 – Signature verification scheme
Multisignature"/> 

### Multisignature

*Multisignature* is a digital signature scheme that requires an interaction of several parties (while applying their private keys) to calculate the signature. There are two main types of multisignature. The first assumes that the signature is verified according to the set of public keys. In this case, the interacting parties determine the public keys and the required number of keys to verify the signature. The signature itself consists of a set of values each of which is verified by a separate public key (Fig. 2.29).

<img align="center" width="50%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.29-Multisignature-of-the-first-type.png" alt="Figure 2.29 – Multisignature of the first type"/> 

The second type of multisignature allows aggregating public key values into one common value, which will be used to check a single aggregated signature value. In this case, the interacting parties initially form the shared public key value and publish it. During the signing process, each party separately signs the required message, but the received signature values can be "added", and the result can be verified using the shared public key (Fig. 2.30).

<img align="center" width="50%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.30-Multisignature-of-the-second-type.png" alt="Figure 2.30 – Multisignature of the second type"/> 

What are the advantages of this type of signature? The main advantage is reducing the amount of memory for storing the multisignature. Unlike standard multisignatures, which size increases linearly depending on the number of signers, the size of a signature calculated by several participants, e.g. using the Schnorr algorithm [22] does not differ from the size of a single signature.

Another advantage is the confidentiality of participants in such schemes. Since the shared public key is an aggregated value, it is quite difficult for a third party to restore the public keys of the signers and associate them with the owners of these keys. All system participants only see the shared public key and the aggregated value of the signature, and it does not visually differ from the usual single signature.

### Threshold signature

A threshold signature is a variation of multisignature with one important difference: public keys of interacting participants have different weight values.

This approach — namely, determining the required weight for a specific operation and assigning weight values to public keys — is often used in accounting systems. So a user, having one key pair with the maximum weight value, can initially create the so-called signers (a set of public keys) for his account and set its own weight value for each.

When verifying the signature, the verifier inspects that the sum of the weights of the signatures with which the message was signed meets the required threshold (Fig. 2.31). In this case, the threshold is 1.

<img align="center" width="50%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.31-Threshold-signature-verification-scheme.png" alt="Figure 2.31 – Threshold signature verification scheme"/> 

### Group signature

A group signature is a mechanism that allows a user to sign a specific message on behalf of a group. A group signature has the following properties:
> * *Only the group members can generate the correct signature*
> * *The verifier can make sure whether a particular signature is calculated by one of the group members*
> * *The verifier cannot determine which group member signed the message.*
> * *In case of dispute, the group administrator can disclose the signer's identity*

The first proposed kind of group signature required the presence of a fully trusted *group administrator*.

The group administrator initially generates a large number of key pairs. Next, he distributes private keys among the group members. After that, he forms a list of public keys of the group members and shuffles these keys in a random order.

Finally, he publishes this list.

When one of the group members wants to sign a document, he randomly selects one of his private keys and calculates the signature. When the verifier checks the value of the signature, he, in the list of public keys, searches for the one that matches the signature. If he finds a suitable public key, then the signature was generated by one of the group members (Fig. 2.32).

<img align="center" width="45%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.32-Group-signature-scheme.png" alt="Figure 2.32 – Group signature scheme"/> 

It is the simplest implementation of a group signature scheme, and it has a number of disadvantages:
* The group administrator can sign messages on behalf of the group members
* It is difficult to add a new group member
* The subset of keys of each group member should be large enough to complicate the analysis of an association of the keys with their owners

At the moment, there are a large number of group signature algorithms that can eliminate the listed disadvantages. The issue of trust of the group members to an administrator is solved when users personally generate their secrets. At that, an administrator provides them only with a set of values that are necessary for the generation of private keys and allow calculating the signature verifiable with the group public key.

In this case, the verifier checks the signature using the group public key and cannot discover its association with a particular participant. Such a scheme supports a simple mechanism for adding new members to a group. At the same time, the basic properties of the group signature, which we considered earlier, are provided.

### Ring signature

*A ring signature* is a type of digital signature that allows one of the members of a group (called a ring) to sign a message on behalf of the entire group. To generate such a signature, the user needs public keys of other users and his key pair. When verifying the signature, the verifier can check whether it was calculated by one of the ring members but cannot discover by whom exactly.

The ring signature algorithm was proposed by Adi Shamir, Yael Tauman, and Ron Rivest and announced in 2001 at the Asiacrypt international conference [22]. In the title of their paper, founders tried to emphasize the absence of a central coordinating structure when generating this signature: "the ring is a geometric figure with a homogeneous periphery and without a center."

Unlike the group signature, there is no prepared group of participants in the ring signature scheme; no preparatory procedures are required to create or modify such a group. The main requirement is that each participant must be associated with a key pair. This allows the signer to choose an arbitrary set of possible signers (in which he includes himself) and independently calculate the signature using possible other participants' public keys and his private key.

Imagine a group of n users, as in Figure 2.33. Each user has their own key pair – *private* and *public keys* (sk, PK). *Private keys* are known only to their respective owners, while *the public* ones are known to all the system participants.

<img align="center" width="50%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.33-Scheme-of-forming-a-group-for-a-ring-signature.png" alt="Figure 2.33 – Scheme of forming a group for a ring signature"/> 

To create a signature on behalf of the group, the user must submit the public keys of all ring participants including his own as input of the algorithm and use his private key as a secret. The public keys of each participant are available for everyone in the system. Figure 2.34 schematically shows how a ring signature is formed for the User 4.

<img align="center" width="50%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.34-Ring-signature-formation-scheme.png" alt="Figure 2.34 – Ring signature formation scheme"/> 

When the verifier checks the value of a signature, he can make sure that the signature was generated by one of the group members but cannot find out by whom exactly. He can determine that the signature was calculated by a particular ring participant only with probability 1/n. A signer can be surely disclosed only if all other group members collude (Fig. 2.35).

<img align="center" width="50%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.35-Ring-signature-verification-scheme.png" alt="Figure 2.35 – Ring signature verification scheme"/> 

Thus, the ring signature scheme can be used when it is required to ensure the anonymity of the signer as well as his independence from other participants while ensuring the integrity and authenticity of the message; the recipient will be confident that the message was sent by a member of a certain group and not by someone outside the group.

It is worth mentioning that there exists a one-time ring signature. Such algorithms imply the use of a public key image value, which allows tracking (linking) all the signatures generated using one private key, even if different rings were used.

Such a property is useful in many areas such as e-voting (each participant can vote only once), digital money (e-coins can be spent only once), etc.

One example of using ring signatures is a decentralized accounting system called Monero and its Ring Confidential Transactions (RingCT) mechanism. It is based on a multilayered linkable spontaneous anonymous group (MLSAG) signature, which is an improved version of regular ring signatures with additional cryptographic mechanisms on top. It protects confidentiality of transacting parties by hiding amounts, origins and destinations of transactions while being verifiable and trustless.

Instead of the actual transaction amount, RingCT have a mathematical proof that transaction inputs and outputs are equal (this is a fundamental protocol requirement that prevents coins emerging from the thin air). The recipient, in turn, can reveal the value of the transaction and use its outputs.

One important feature of MLSAG signatures is that a signer uses a key pair vector instead of a single key. This vector is a collection of signer’s keypairs. This allows proving the knowledge of both the input address and input commitment secret keys.

To be able to sign transactions, a user first needs to generate keys. MLSAG generation algorithm produces the following key pair vector with input 1k, where k is a security parameter.

**[(sk1, pk1),...,(skm, pkm)]**, where

*ski* is a secret key, a random value chosen from ℤq (which is the Ed25519 base field),
*pki=xiG* is a public key and G is a base-point.

Additionally, a key-image I is calculated as follows.

**I = skiHp(pki)**, where

*Hp* is a secure hash function returning a point, the logarithm of which with respect to G is unknown.

A vector of key images for signer’s key pair vector is defined as 

**I[I1,...,Im][sk1Hp(pk1),...,skmHp(pkm)]**

It enforces the signer to use a key only once, otherwise, the linking phase of the algorithm will reject it. This also prevents double spending the value in a given key.

A signer Aj can sign a message M on behalf of the ring A1,...An having a key pair vector and key image vector. First, it needs to choose a random value  from ℤq. Then it calculates the following values.

> **Lj G**

> **RjHp(pkj)**

> **cj+1Hs(M,Lj,Rj)**

Hs is a cryptographic hash function returning a value from ℤq.
Then

> **Lj+1 sj+1G+cj+1pkj+1**

> **Rj+1sj+1Hp(pkj+1)+cj+1Ij**

> **cj+2Hs(M,Lj+1,Rj+1)**

…

> **Lj-1 sj-1G+cj-1pkj-1**

> **Rj-1sj-1Hp(pkj-1)+cj-1Ij**

> **cjHs(M,Lj-1,Rj-1)**, where

For all *c1,...cn*.

**sj=-cjxj mod l**, where 
l is the curve order of Ed25519.

Based on this, the following is true

> **Lj=G=sjG+cjskjG=sjG+cjpkj**

> **Rj=Hp(pkj)=sjHp(pkj)+cjI**

> **cj + 1 = Hs(M,Lj,Rj)**

The signature is then generated as follows.

**=(I,c1,s1,...sn)**

*Signature verification* is performed in the following way. Verifier calculates Li, Ri can ci for all i and checks that cn + 1 = ci. For a signature to be considered valid the following must be true.

**cn+1=Hs(M,Li,Ri)**

For all i mod n.

### Blind signature

The blind signature mechanism is used when one party forms a message, and the other party signs (confirms) it, while the creator of the message wants to hide some of its parts from the signer. The blind signature mechanism can be used in banking structures to transfer funds between users. In fact, one can imagine such an interaction as a blinded check transmitted to the other party: the owner of funds generates such a check, and the bank signs it without seeing the unique check identifier; after that, this check can be transmitted to any party, and it can be cashed without disclosing its sender.

The blind signature mechanism works as follows:

> 1. The sender generates* n different transactions with the same transfer amount but with different unique identifiers. Before sending them, the user blinds (encrypts) the transactions using a randomly generated multiplier. After that, all transactions are transferred to the signer.

> 2. The signer requests* from the sender n−1 blinding factors that correspond to the transactions selected by the signer. In fact, it determines which transactions he wants to disclose.

> 3. The sender transmits* a set of blinding factors. The signer uses them to deblind the blinded transactions and verifies that the amount of transfers is the same and that the user has enough funds to make it. 

> 4. If amounts in some transactions are not equal, the signer denies the user. Otherwise, he signs the remaining transaction without seeing its content and sends it back to the user.

> 5. User removes the blinding from the transaction. At the same time, the signature also remains correct for the transaction from which the blinding was removed.

> 6. User can send this "check" to anyone. The recipient at any time can transmit this check to the party that signed it and receive the amount specified in it. The verifier can only make sure that the signature is correct, while he does not know to whom the check with such an identifier was issued (Fig. 2.36).

<img align="center" width="60%" src="/resources/img/volume-2/2.4-Digital-signature-types/Figure-2.36-Blind-signature-generation-and-verification-scheme.png" alt="Figure 2.36 – Blind signature generation and verification scheme"/> 

**Frequently asked questions**

*— What systems currently use threshold signature and multisignature mechanisms?*

Multisignatures and threshold signatures are used in systems to ensure fault tolerance and the separation of responsibilities for the management of processes. Many cryptocurrencies allow users to block funds at multisignature addresses, which allows them to access their funds even if they lose one (or several) keys. Also, the use of multisignature allows building a large number of protocols on top of some already existing accounting systems (atomic swap, payment channels, etc.). Threshold signature mechanisms are used when an even greater level of delimitation of user permissions is required. Also, the weight of each particular key can be configured (often depending even on a single operation) so that the access policy to the accounting system services can be configured as efficiently as possible. A similar approach is used in many asset management platforms, such as Stellar.

*— How does the signer's anonymity level and the ring size correlate when using the ring signature?*

When using a ring signature, the verifier can determine who signed a particular message only with little probability. The larger the ring is, the less likely a particular message was to be signed by a particular ring participant. For example, if a ring consists of 10 participants, then it can be assumed with a 10% probability only by whom exactly the signature was calculated. Of course, the signer's identity can also be revealed, but it is possible only if all other ring participants collude.

## 2.5 Shamir’s secret sharing scheme

Let’s imagine a situation when we need to protect some data and at the same time provide its confidentiality and availability. For example, Alice owns a bakery and wants to keep her pie recipe secret. Let’s say she encrypts the recipe and stores the key somewhere near, providing the access to it when it’s needed. After some time she realises that this solution is not convenient because she needs to be physically present in the vault each time. Furthermore, what about the vacation? Or even worse – what if the key is lost?

Then Alice could make a copy of the key and entrust it to a bakery employee, however, in this case, the risk of its theft also increases.

There is also an option to not duplicate the original key but to split it in half and give a part to a trusted person. Now it’s clear that both parts must be physically present to combine them in a key and open the storage. Therefore, in order to steal a recipe, an attacker needs to have both parts of the key, which is more difficult than to steal one key. However, this scheme is not much better than using a single key because if one of them loses half of the key, the full key cannot be restored.

Alice realizes that ideally the key has to be split between several employees so that reliability doesn’t depend on one person. Also there must be a certain limit for the number of employees so that in case when one part is lost or becomes inaccessible, the whole key can still be restored.

The solution to this problem was presented in Adi Shamir’s work in 1979 [78]. He proposed a secret sharing scheme also called the interpolation polynomial scheme.

### Threshold secret sharing scheme

*A threshold secret sharing scheme* is a scheme in which the number of participants required to restore a secret can be less than the total number of participants. Let’s say that *n* is the total number of participants, and *t* is the number of participants needed to restore the secret. Any group of *t* or more participants can restore the secret, while any less number of participants will not be able to learn anything about the secret. The implementation of Adi Shamir’s scheme can be divided into the following phases: 

> * *Preparation phase*
> * *Secret parts generation*
> * *Reconstruction of a secret*

The first phase is *preparation*. Let’s say there exists a secret *S* that has to be divided into n parts so that the secret itself can only be obtained if there are at least *t* parts. First we need to choose a prime number *p*, so that *p > S*, *p > n*. It will set the size of the field. We need to build a polynomial that will pass through the poin (*0*, *S*) over this field. It will look as follows 

**F(x)=(a<sub>t-1</sub>x<sup>t-1</sup> + a<sub>t-2</sub>x<sup>t-2</sup> + ... + a<sub>1</sub>x + S)**, where *a<sub>1</sub>, a<sub>2</sub>, ... , a<sub>t-1</sub>* are randomly selected natural numbers. The secret can be restored later if we calculate *F(0)*.

The amount of points necessary to reconstruct the secret will depend on the degree of the polynomial. If we need the secret to be reconstructed with *t* parts, then the degree of polynomial must be *t - 1*.

What is the reason for this? Let’s imagine that we have the first degree polynomial of the following form *y = S + a<sub>1</sub>x*, such as *y = 3 + 2x*. It corresponds to a graph that we can uniquely identify having at least two points. At the same time, having only one point, one can draw an infinite number of lines through it (Fig. 2.37).

<img align="center" width="50%" src="/resources/img/volume-2/2.5-Shamirs-secret-sharing-scheme/Figure-2.37-A-possible-number-of-straight-lines-that-pass-through-the-necessary-number-of-points-and-through-fewer-points.png" alt="Figure 2.37 – A possible number of straight lines that pass through the necessary number of points and through fewer points"/> 

Similarly, if we have the second degree polynomial, we need at least three points, because we can draw an infinite number of curves of the second degree through two points (Fig. 2.38).

<img align="center" width="50%" src="/resources/img/volume-2/2.5-Shamirs-secret-sharing-scheme/Figure-2.37-A-possible-number-of-straight-lines-that-pass-through-the-necessary-number-of-points-and-through-fewer-points.png" alt="Figure  2.38 – A possible number of graphs that pass through the necessary number of points and through fewer points"/> 

Thus, if we want to restore the secret using $t$ parts, we will have to construct a polynomial of degree *t-1*.

The next phase is *the secret parts generation*. Each part of the secret is a point that belongs to the graph that is built using the polynomial. In order to get the next point, we need to generate a natural number then substitute it into a *polynomial*. During this process n different points are generated. This phase is performed by the owner of the secret to prevent its disclosure. 

When secret parts are generated, each participant receives one part *(x<sub>i</sub>, f(x<sub>i</sub>))*. All participants know the degree of the polynomial *t-1* and the size of the field *p*.

The last phase is *the reconstruction of a secret*. There are two ways to reconstruct a secret: solve the system of linear equations or calculate the Lagrange interpolation polynomial. The second method is easier for the software implementation and can be used to calculate the secret itself without calculating the polynomial coefficients. This phase is performed by any group of *t* participants when they need to know the secret.

The Lagrange interpolation equation is shown in Fig. 2.39. It can be used to construct the polynomial. All that is left to reconstruct the secret is to calculate *F(0)*.

<img align="center" width="45%" src="/resources/img/volume-2/2.5-Shamirs-secret-sharing-scheme/Figure-2.39-Lagrange-interpolation-equation-Secret-sharing-example.png" alt="Figure  2.39 – Lagrange interpolation equation
Secret sharing example"/> 

### Secret sharing example

Let’s consider how this scheme works using an example. Imagine that one needs to divide the secret *S = 7* between six participants so that any four of them can restore it.

> Step 1. The first thing to do is to choose a prime number *p* so that *p > S*. Let’s take *p = 13* as an example. Then you need to construct a polynomial of the form as in Fig. 2.40, where *a<sub>1</sub>, a<sub>2</sub>, a<sub>3</sub>* are randomly selected natural numbers that are used as coefficients.

<img align="center" width="50%" src="/resources/img/volume-2/2.5-Shamirs-secret-sharing-scheme/Figure-2.40-Making-a-polynomial-with-certain-coefficients.png" alt="Figure 2.40 – Making a polynomial with certain coefficients"/> 

> Step 2. Here, in order to get secret parts you need to substitute six different randomly chosen natural numbers in the polynomial. Let’s take *{1,2,3,4,5,6}* for convenience. These numbers cannot be in order and they all should have different values modulo p. As a result, we get six points, which need to be distributed among the participants (Figure 2.41). Then the coefficients of the polynomial can be destroyed and any four participants out of six can restore the secret. Depending on the goals, the secret can also be destroyed. Then it can be reconstructed only by using its parts.

<img align="center" width="50%" src="/resources/img/volume-2/2.5-Shamirs-secret-sharing-scheme/Figure-2.41-Calculation-of-secret-parts.png" alt="Figure  2.41 – Calculation of secret parts"/> 

> Step 3. Now using any four points one can construct a system of equations and solve it or restore the original polynomial using the Lagrange interpolation formula (Figure 2.42). In this example points p are used.

<img align="center" width="50%" src="/resources/img/volume-2/2.5-Shamirs-secret-sharing-scheme/Figure-2.42-Reconstruction-of-the-polynomial-and-secret.png" alt="Figure 2.42 – Reconstruction of the polynomial and secret"/> 

Knowing that *S = F(0)*, one can easily calculate the secret: *F(0) = 7*.

### Advantages of Shamir’s scheme 
> * *Resilience*
> * *Dynamic nature*
> * *Scalability*

*Resilience*. To reconstruct the secret, an attacker needs *t* parts. Any less number of parts won’t let him reconstruct even the part of the secret value. In this case, even if an attacker has the significant computing power, he can do nothing with it since there is no way to verify the correctness of calculated parts.

*Dynamic nature*. One can apply the scheme several times for the same secret and each time get different sets of parts, create an infinite number of different polynomials and corresponding points. If an attacker knows secret parts based on different polynomials, it will not help him to gain any knowledge of the secret itself. However, the creation of a new set of parts doesn’t negate the possibility to restore the secret using its old parts. Imagine the situation: Alice is going to leave a will for her secret recipe. For her relatives, she can use the scheme (2,3), for her friends – (5,7), and for staff – (10,14). Then only 2 relatives or 5 friends, or 10 employees can get the recipe.

*Scalability* is ensured by the fact that the number of secret holders can be made arbitrarily large by choosing a polynomial of the appropriate degree.

### Disadvantages of Shamir’s scheme
> * *The secret must be on the same device during separation and reconstruction*
> * *It’s impossible to verify the accuracy of the provided part of the secret*

To split the secret using this scheme, *the secret must be on the same device during the splitting*. The secret parts must also be on the same device during restoration. For example, when Alice leaves her will for the recipe, she needs to keep the whole recipe until she shares it. There is also the risk of stealing a secret after it was reconstructed. At these moments the risk that a competitor can steal a recipe is particularly high.

The owner of one of these parts can submit an incorrect value. Thus, the secret will be restored incorrectly and the rest will not know it. They can’t even check whose part was wrong.

> **_NOTE:_** *There are modifications of Shamir’s scheme that use checksums to minimize the probability of using incorrect parts. There are also secret sharing schemes where each participant can verify the validity of secret parts. One example is PVSS – Publicly Verified Secret Sharing.*
 
### Shamir’s scheme application

Most often, the threshold cryptography is used to store the private key of a certification authority, as well as in cloud environments and electronic voting schemes.

In addition, Shamir’s scheme can be applied to hierarchical access structures. These structures are trees where each node has access to a smaller amount of data than its parent. The root of the tree has access to all data. Shamir presented the method to implement it in his work [78].

Let’s consider a specific example of using Shamir’s scheme. The scheme is used by the Paris company SatoshiLabs to solve the problem of storing private wallet keys. It allows users to “break” their keys into several parts. Then you can restore them by combining some predetermined subset of these parts. A backup algorithm from SatoshiLabs allows you to create up to 16 parts (which in turn can also be further divided). This backup is an open standard so other companies can use it in their wallets in the future.

[TECHNOLOGICAL DETAILS OF BITCOIN OPERATION](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-2/en/3-technological-details-of-bitcoin-operation.md)
