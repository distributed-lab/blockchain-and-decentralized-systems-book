# [4 PRIVACY AND ANONYMITY ON THE INTERNET](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/4-Privacy-and-anonymity-on-the-Internet.md)

# 5 ROLE OF CRYPTOGRAPHIC COMMITMENTS IN ACCOUNTING SYSTEMS

Low privacy level poses a very serious threat to the system security. For example, your competitors can reveal information about all your transactions, learn the details of your business, etc. In Bitcoin, for example, the issue of ensuring anonymity is partially resolved by assigning addresses. If a third party does not know that a particular address belongs to you, its ability to have an impact on you reduces noticeably.

However, the network implies that parties interact. Due to this interaction, the parties find out each other’s addresses/accounts. So, the third party with whom you interact can view information about your previous actions and receive the information it needs since the details of all transactions are open and transparent by default.

The means with which privacy level can be enhanced are often very expensive for the system (since they reduce performance and its throughput). However, for some platforms, privacy is a mandatory requirement that one should not cut corners on. In this section, we discuss the principles of constructing a confidential transaction (CT) as one of the methods for ensuring confidentiality (with the possibility of public audit) in open systems (Fig. 5.1).

![Figure 5.1 – Disclosure of transaction details by a third-party observer and using CT to hide these details](/resources/img/volume-3/5.0/F-5.1-disclosure-of-transaction-details-and-using-CT.png "Figure 5.1 – Disclosure of transaction details by a third-party observer and using CT to hide these details")


## 5.1 Methods of building cryptographic commitments

In this section, we discuss the fundamentals of zero-knowledge proofs – _cryptographic commitments_.

Cryptographic commitments are typically used when one wants to fix a certain value and after a certain time, when it gets disclosed, prove that it was fixed. For example, you want to bet on one of the horses which, in your opinion, is likely to reach the finish line first. At the same time, you don’t want anyone to find out on which horse you bet until the end of the race, and you want to be able to prove (as soon as the race ends) that you bet on a particular horse.

Cryptographic commitments can work only due to the existence of one-way functions: the prover must be able to provide the output value of the one-way function, which does not in itself reveal the input message (the verifier will use the same function to verify knowledge) [91].

For example, hash functions work well for this need. A user can bet on the horse with the number 2, providing the hash value of this number. After the horse wins the race, the user reveals the secret, and the verifier makes sure that this value corresponds to the previously provided commitment (Fig. 5.2).

![Figure 5.2 – Using commitment to bet on a horse number](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.2-using-commitment-to-bet.png "Figure 5.2 – Using commitment to bet on a horse number")


### Adding randomness as a component of commitments

The above-described approach has some issues since each time the commitment for the number 2 is provided, its hash value will be the same. Hence, the verifier will be able to reveal the initial knowledge.

> _Note. If the range of possible knowledge options is small (for example, only 10 horses participate in the race), then having received the hash value of a specific number, the verifier can quickly iterate over all possible values and reveal the value of the secret. However, even if the number of options is large (for example, $2^{256}$), the commitments for the same knowledge will not differ from each other. If one provides a hash value of 2, the verifier can save this mapping (commitment–knowledge). If after some time one provides a commitment for the same knowledge, the verifier can quickly reveal the secret._

What is the way to solve this? All is quite simple: imagine that the prover sends the verifier the value 2 with a few random bytes appended to it (Fig. 5.3). Since the hash function is resistant to collisions and finding the inverse image, this property can be used so that the verifier cannot disclose the knowledge when having only commitments, and the prover is unable to change this value [91].

![Figure 5.3 – Adding randomness in the formation of commitments](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.3-adding-randomness-to-commitments.png "Figure 5.3 – Adding randomness in the formation of commitments")

Although hash functions can provide these properties, they have nothing to do with homomorphism, which is a very important property for zero-knowledge proofs.

> _Note. Let us recall that homomorphism means the ability to perform certain mathematical operations with commitments and at the same time ensure the commutativity and associativity of these operations. If we use hash values as commitments, then $\mathrm{hash} (a)+ \mathrm{hash} (b) \neq \mathrm{hash} (a+b)$; this means that we cannot operate over commitments to obtain the correct result_ [92]_._


### Pedersen commitments

To provide the necessary features Pedersen commitments can be used [93]. The role of a one-way function here is performed by the operation of multiplying a point of an elliptic curve by a scalar. Pedersen commitments can be represented as the following equation: $C=rH+aG$, where _a_ is the secret (the knowledge), _r_ is a random value, and _G_ and _H_ are the generators of the points group (the curve points). Here, _r_ is used for the same purposes as when using the hash function in the example above. In addition, the prover must not be able to determine the dependence of _H_ on _G_ (below we will examine in detail why).

Pedersen commitments can provide _irreversibility_ (the inability to obtain a secret given only a commitment), _provability_ (having a commitment, the verifier can check the knowledge with a high level of certainty) and _homomorphism_. Irreversibility and provability are ensured by the complexity of the discrete logarithm problem. The homomorphism is as follows (Fig. 5.4): it is possible to generate _one commitment_ (a single point value)for proving the fulfillment of a _set of commitments_ (as long as the redundant values are also provided correctly).

![Figure 5.4 – Essence of homomorphism in Pedersen commitments](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.4-homomorphism-in-Pedersen-commitments.png "Figure 5.4 – Essence of homomorphism in Pedersen commitments")

Now, the prover can provide one commitment to the verifier and then send him a set of knowledge values which satisfy the commitment (Fig. 5.5). For instance, the prover wants to bet on 10 different races at once by providing one value.

![Figure 5.5 – Providing a single commitment to a wide range of knowledge](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.5-single-commitment-to-set-of-knowledge-values.png "Figure 5.5 – Providing a single commitment to a wide range of knowledge")

> _Note. In this subsection, we examine Pedersen commitments that use elliptic cryptography. However, there still exists a “classic” implementation of this scheme. According to the protocol, the parameters are h and g (not elliptic curve points but big prime numbers). The prover holds the secret a and generates a random value r. The proof is the value $C=g^{a}h^{r}$. The verifier, receiving the values of a and r in the same way, can verify that the commitment is satisfied._


### Knowledge substitution and "nothing up my sleeve" approaches

As noted earlier, the randomness of _H_ is important. If the prover can determine an association between points _H_ and _G_, then he is able to generate fake proofs which satisfy the commitment.

This feature lies in the fact that the commitment is a curve point obtained through adding two points – $rH$ and $aG$. _H_ is a point derived from _G_: $H=xG$, where _x_ is an unknown value. In the end, commitment looks as follows: $C=rH+aG=rxG+aG=(rx+a)G$.

The prover knows the values of _r_ and _a_. If he also knew the value of _x_, then he could modify _a_, _r_, and _x_ in such a way that they would satisfy the commitment. However, since the prover does not know _x_, he cannot choose such $x' \neq x$ with which one could generate a fake proof.

Let’s simulate this attack in our example with bets. The point _H_ is not random: the prover knows the value of the secret which, being multiplied by _G_, gives the value _H_ (for example, $H=3G$). The prover wants to bet on horse number 2 (value _a_). He generates a random value $r=2$ and transmits the commitment to the verifier (Fig. 5.6-A).

![Figure 5.6-A – Providing of commitment to the verifier](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.6-A-providing-of-commitment-to-verifier.png "Figure 5.6-A – Providing of commitment to the verifier")

Suddenly, horse number 5 wins the race. If the prover didn’t know _x_, then he would definitely lose his money. But since he knows _x_, he sends the fake values $a=5$ and $r=1$ to the verifier. As we can see in Fig. 5.6-B, the verifier accepts the proof, although the prover initially provided the commitments for completely different values. Thus, it is important that the _H_ value be set randomly.

![Figure 5.6-B – Successful use of fake commitments](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.6-B-successful-use-of-fake-commitments.png "Figure 5.6-B – Successful use of fake commitments")

To prove that _H_ was not specified in a special way, there is a set of methods named "nothing up my sleeve" [94; 95]. This set is quite big, but its basic principles can be explained by the following example.

Imagine that you simply take the hash value of any data and check whether that value is a point on an elliptic curve. If it is, you can set it as _H_. Meanwhile, although you know the initial data set, you cannot find _x_. This is very similar to creating an address from which you cannot spend coins (a non-spendable address).

In practice, to obtain the point _H_, the hash value of the _G_ point is most often taken and concatenated with a counter _i_ until a point on the curve (more precisely, a valid _x_ coordinate) is found: $x_{H}= \mathrm{hash} (G \Vert i)$.

> _Note. To check whether the x coordinate is chosen correctly, it is necessary to calculate the corresponding integer y value from the equation of the elliptic curve: $y^{2}=ax^{3}+bx+c$. The obtained coordinates will be the coordinates of the H point._


### Single commitment for a vector of values

One of the distinctive properties of Pedersen commitments is the fact that the size of commitments does not depend on whether we hide one secret or a vector of values. In any case, the commitment is the value of a single curve point (Fig. 5.7-A) [91].

![Figure 5.7-A – Commitment is a single point value regardless of the amount of hidden knowledge](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.7-A-commitment-is-single-point-value.png "Figure 5.7-A – Commitment is a single point value regardless of the amount of hidden knowledge")

In this case, these commitments can be verified if the prover provides _r_ and all values of the vector _a̅_. But again, in this case, regardless of the number of elements in the vector, the commitment will be a single point.

Furthermore, we can create the same commitments for a large number of different vectors while maintaining the size of these commitments (Fig. 5.7-B).

![Figure 5.7-B – Rough form of a commitment for different value vectors](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.7-B-commitment-for-different-value-vectors.png "Figure 5.7-B – Rough form of a commitment for different value vectors")

> **_Pedersen commitment properties_**  
> * _Perfect hiding_
> * _Computational binding_
> * _Equivocality_

_Perfect hiding_ means that a third party cannot obtain the secret having only the commitment associated with it. Even if that party finds the values which satisfy the commitment, it will not make sure that this will be exactly the secret that the prover has).

_Computational binding_ implies that an attacker cannot find other data that satisfies specific commitments without having solved the discrete logarithm problem in a group of elliptic curve points (provided that he does not have additional data about the parameters used).

_Equivocality_ is a somewhat limitation of this commitment scheme that follows from the previous properties. It implies that different data sets can satisfy specific commitments, and if generation of parameters was not random, the prover can convince the verifier that he knows the secret without actually knowing it [91].


### ElGamal commitment scheme

The _ElGamal_ scheme of commitments [96] is also used quite often, but it has slightly different properties as compared to Pedersen commitments. Let’s examine how it works.

We have the two random group generators: _g_ and _h_. The prover wants to provide a commitment for the secret _a_. Initially, he also generates a random value of _r_, with which he adds randomness to the commitment. The commitments are value pairs calculated as follows: $C=(g^{r},ah^{r})$.

To prove knowledge, the user must provide the values of _a_ and _r_. The verifier calculates the commitments in the same way. If the calculated commitment is equal to the one received earlier, the knowledge of the secret is proven.

> **_ElGamal commitment properties_**  
> * _Perfect binding_
> * _Computational hiding_
> * _Extractability_

_Perfect binding_ indicates that the commitments are strictly tied to the original secret, that is, regardless of the attacker’s computing power he will not be able to find another secret that satisfies the proof.

_Computational hiding_ means that an attacker cannot distinguish between two commitments for different messages (provided that he cannot solve the factorization problem).

_Extractability_ implies that if an attacker solves the factorization problem, then he can reveal the secret for any commitment.


## 5.2 Schnorr identification protocol as an interactive zero-knowledge proof scheme

In order to understand how interactive zero-knowledge proofs work and how the commitment schemes (the ones discussed in the previous subsection) are used in them, let’s examine one of the interactive proof schemes – the Schnorr identification scheme. The identification scheme is much like the Schnorr signature scheme, except for the fact that it is interactive (i. e., the signature scheme does not require communication between the signer and the verifier during the signature verification) and messages will not be used.


### Schnorr identification scheme

According to this protocol, the prover possesses a certain secret _x_ and the corresponding commitment $P=xG$. Here, the association between the commitment and the secret is exactly the same as between the public and private keys in the signature scheme of the same name. The prover wants to provide proof that he knows _x_ without disclosing its value [97].

The algorithm consists of four stages. At the first stage, the prover generates a new secret _r_ and forms the corresponding commitment $R=rG$. The public value is then passed on to the verifier. In response to this, the verifier generates a random scalar _e_ and returns it to the prover (the second stage of the protocol). At the third stage, the prover uses the secret _x_ as well as the values _r_ and _e_ to generate the knowledge proof and then sends it to the verifier. At the last stage, the verifier checks the received proof and, if the verification is completed successfully, makes sure that the prover knows the secret. The protocol description is presented in Fig. 5.8.

![Figure 5.8 – Schnorr identification scheme](/resources/img/volume-3/5.2-Schnorr-identification-protocol-as-an-interactive-zero-knowledge-proof-scheme/F-5.8-Schnorr-identification-scheme.png "Figure 5.8 – Schnorr identification scheme")

1. The prover generates a random value of _r_ and calculates $R=rG$. After that, he sends _R_ to the verifier. The verifier cannot calculate _r_ (he cannot solve the discrete logarithm problem in the group of elliptic curve points).
2. The verifier generates an open parameter _e_ and sends it to the prover.
3. The prover calculates $s=r+ex$ and sends it to the verifier. Given only the values _s_ and _e_, the verifier cannot obtain the secret _x_.
4. The verifier checks the proof for the knowledge of _x_ according to the expressions in Fig. 5.9.

![Figure 5.9 – Proof verification](/resources/img/volume-3/5.2-Schnorr-identification-protocol-as-an-interactive-zero-knowledge-proof-scheme/F-5.9-proof-verification.png "Figure 5.9 – Proof verification")

As we see, this protocol satisfies the requirements for zero-knowledge proofs:

* The prover is guaranteed to convince the verifier that he has the necessary secret (_x_).
* If the prover does not have the secret, he cannot convince the verifier that he does.
* When the knowledge of the secret is proven, the secret itself is not disclosed.


### Forging of proof by the prover

If the prover knows the value of _e_ (the scalar that is generated by the verifier) before starting the interaction with the verifier, he can forge the proof without knowing the secret value. Let’s analyze how this works (Fig. 5.10).

![Figure 5.10 – Flow of proof forging](/resources/img/volume-3/5.2-Schnorr-identification-protocol-as-an-interactive-zero-knowledge-proof-scheme/F-5.10-proof-forging.png "Figure 5.10 – Flow of proof forging")

1. The prover generates a random value _s_ and computes $S=sG$. After that, he calculates $R=sG-eP$ (the prover can do this because he knows the value of _e_).
2. The prover passes _R_ to the verifier.
3. The verifier returns _e_. After that, the prover sends the pre-generated _s_ to the verifier.
4. The verifier does the same check as in the previous example and makes sure that the prover knows the secret (even though he actually doesn’t).

However, the distinctive feature of the discussed scheme is not that the prover can trick the verifier if knowing the value of _e_ in advance (if the verifier knows the correct order of interaction, he will not let himself be tricked in this way) but that this protocol is _interactive_. That is, a third party cannot make sure that the protocol iterations have been performed correctly and that the verifier has not disclosed the value _e_ to the prover in advance. This protocol can only be used between two parties (in order to verify that the prover has the secret at a specific time point), and both the prover and the verifier cannot convince any third party of the correctness of the actions performed.


### Transforming an interactive protocol into a non-interactive protocol

In the identification protocol described above, the random value _e_, on which the security of the scheme is based, is provided by the verifier. Hence, it directly depends on the verifier whether the prover can forge these proofs [91].

One can change the source of randomness using, for example, a hash function. In a non-interactive protocol, _e_ is no longer generated by the verifier: it is calculated as $e= \mathrm{hash} (R)$.

What benefit is in this case? Previously, the prover would calculate the value of _R_ based on the generated _s_ and would be able to make the correct calculations only if he gained access to _e_ in advance. Now _e_ depends on _R_ and cannot be calculated prior to _R_. This, in turn, prevents potential forging of knowledge proofs.


## 5.3 Using Pedersen commitments for zero-knowledge proofs

In the previous subsections, we separately examined what Pedersen’s commitments are and how the Schnorr identification protocol works. Now let’s examine the scenario where the verifier interactively communicates with the prover in real time (as in the case of the Schnorr interactive protocol), but Pedersen commitments are used.

The knowledge proof will be provided after the prover has generated a set of commitments for each of the following _m_ vectors: $C_{i}=r_{i}H+ \overline{x_{i}G}$, $i \in \{ 1,2, {\mathellipsis} ,m \}$. After the prover has transmitted a set of the commitments $(C_{1},C_{2}, \mathellipsis{} ,C_{m})$ to the verifier, he can prove in one step that he knows the original secret vectors.

First, the prover creates a random new commitment _C<sub>0</sub>_ for the random value _x<sub>0</sub>_ and sends it to the verifier. After that, the verifier generates a random scalar value _e_ and returns it to the prover. The prover the value pair $(z,s)$ and sends it to the verifier. The calculation of these values is shown in Fig. 5.11-A.

![Figure 5.11-A – Interactive proof protocol](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.11-A-interactive-proof-protocol.png "Figure 5.11-A – Interactive proof protocol")

> _Note. The iteration steps in this protocol are similar to the steps of the Schnorr identification protocol, so that one may suppose that an attack of commitment substitution is possible here as well (further, we examine in more detail how this works)._

Vectors _x<sub>i</sub>_ are hidden from the verifier, since the addition of elements does not reveal each of them individually: $z_{i}=x_{0,i}+ex_{1,i}+e^{2}x_{2,i}+ \mathellipsis +e^{m}x_{m,i}$. The verifier checks the obtained commitments according to the expression in Fig. 5.11-B. If the equation is calculated correctly, then the verifier makes sure that the prover knows a complete set of secrets.

![Figure 5.11-B – Verification of proofs](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.11-B-verification-of-proofs.png "Figure 5.11-B – Verification of proofs")

Let’s consider an example that shows why such proofs work. We will simplify it as follows: each knowledge vector is simply one value. That is, we will work with Pedersen commitments corresponding to hiding one secret. Suppose Alice sends Bob 3 commitments _C<sub>1</sub>_, _C<sub>2</sub>_, _C<sub>3</sub>_. They match the values of _a<sub>1</sub>_, _a<sub>2</sub>_, _a<sub>3</sub>_, which she knows (Fig. 5.12-A).

![Figure 5.12-A – Alice sends a set of commitments](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.12-A-Alice-sends-set-of-commitments.png "Figure 5.12-A – Alice sends a set of commitments")

After that, she wants to prove that she knows these values. To do this, she sends Bob a new commitment _C<sub>0</sub>_ for the random value _a<sub>0</sub>_. Bob returns Alice the scalar _e_. Next, Alice calculates a value pair $(z,s)$ and sends it to Bob (Fig. 5.12-B).

![alt_text](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.12-B-knowledge-proof.png "Figure 5.12-B – Knowledge proof flow")

Having only the values _z_ and _s_, Bob, in turn, can verify the knowledge according to the calculation in Fig. 5.12-C.

![Figure 5.12-C – Proof correctness verification flow](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.12-C-proof-verification.png "Figure 5.12-C – Proof correctness verification flow")

Thus, we have a situation which resembles using the Schnorr identification protocol. Though, it has one important difference: at the last step of the iteration in the Schnorr protocol, just the value of the scalar _s_ is sent; instead, the value pair $(z,s)$ is sent in the protocol in focus. For the considered example, _z_ was also a single scalar, but it can be a value vector.

Therefore, if the prover knows the value of _e_ in advance, he can forge the proof value. To do this, the prover generates a pair of random values $(z,s)$ and then calculates the value of the commitment _C<sub>0</sub>_ as follows: $C_{0} = (sH+aG)- \sum_{i=1}^{m}{e^{i}C_{i}}$.

Consider how this will work for the following example. Alice sends Bob 3 commitments _C<sub>1</sub>_, _C<sub>2</sub>_, _C<sub>3</sub>_ which match the values of _a<sub>1</sub>_, _a<sub>2</sub>_, _a<sub>3</sub>_. After that, Alice randomly generates $(z,s)$. Having the value of _e_, Alice forms _C<sub>0</sub>_ and sends it to the verifier (Fig. 5.13-A).

![Figure 5.13-A – Flow of proof forging](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.13-A-proof-forging.png "Figure 5.13-A – Flow of proof forging")

To check the knowledge, Bob, in turn, performs the calculation as in Fig. 5.13-B. As we can see, Alice, when not having the secret but given the value of _e_ in advance, can forge proofs for commitments.

![Figure 5.13-B – Successful verification of a fake proof](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.13-B-successful-verification-of-fake-proof.png "Figure 5.13-B – Successful verification of a fake proof")


### Using Pedersen commitments in confidential transactions

In order to determine how Pedersen commitments can be used in confidential transactions (CTs), one first needs to determine which transaction details are to be hidden. Suppose we need to hide the input (_a_) and output (_b_) values of the transaction as well as the fee value (_c_). In order for the transaction to be considered correct and confirmed by validators, the following condition must be met: $a-b-c=0$. To hide each value, utilize Pedersen commitments: $A=r_{A}H+aG$, $B=r_{B}H+bG$, $C=r_{C}H+cG$. Since the commitments are homomorphic, the following transformation can be performed on them: $A-B-C=0G+(r_{A}-r_{B}-r_{C})H$.

If the prover demonstrates that he knows the final value $(r_{A}-r_{B}-r_{C})$ (either the same Schnorr identification scheme or the signature of the same name can be used), then the verifier can make sure that the inputs and outputs of the transaction are correct.

At the same time, there is a need to prove that the values of _b_ (output amount) and _c_ (fee) are non-negative. For this, range proofs are used, and these proofs can be based on Pedersen commitments.


### Using Pedersen commitments for range proofs

_Range proofs_ are used to verify that a particular number is located within certain predefined boundaries. In our case, we need to prove that the values of _b_ and _c_ are non-negative as well as that the sum of these values does not exceed the maximum allowed _n_ (the modulus value). Also, it is very important that the exact values not be revealed [98].

We can represent _b_ and _c_ as a bit sequence. If the module length is $k+2$ bit, then the maximum length of the sum of _b_ and _c_ should not exceed $k+1$ bit, and the size of each of these values should be _k_ bit. Accordingly, the task is to prove that _b_ and _c_ are _k_-bit sequences.

As an example, let’s generate such a proof for _b_. It can be presented as the following polynomial: $b=b_{0}+2b_{1}+2^{2}b_{2}+ \mathellipsis +2^{k-1}b_{k-1}$. We already have a commitment for _b_, which is presented as $B=r_{B}H+bG$. In the same manner, we can form similar commitments for each condition of the polynomial _b_. To do this, we generate a new set of values $(r_{B,0},r_{B,1}, \mathellipsis ,r_{B,k-2})$ and define $r_{B,k-1}=r_{B}- \sum_{i=0}^{k-2}{r_{B,i}}$. Next, using these values, we generate commitments for each component _b<sub>i</sub>_ of the polynomial _b_: $P_{i}=r_{B,i}H+2^{i}b_{i}G$. Since the commitments are homomorphic, the following equation holds: $P= \sum_{i=0}^{k-1}{P_{i}}$.

In such a way, we can prove that the size of _b_ does not exceed _k_ bits. However, we have not yet proven that each _b<sub>i</sub>_ is indeed a specific bit (0 or 1). To prove this, the ring signature mechanism can be used [98].

# [6 BASICS OF POST-QUANTUM CRYPTOGRAPHY](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/6-Basics-of-post-quantum-cryptography.md)
