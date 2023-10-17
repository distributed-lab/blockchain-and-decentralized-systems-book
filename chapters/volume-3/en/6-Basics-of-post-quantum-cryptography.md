# [5 ROLE OF CRYPTOGRAPHIC COMMITMENTS IN ACCOUNTING SYSTEMS](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/5-Role-of-cryptographic-commitments-in-accounting-systems.md)

# 6 BASICS OF POST-QUANTUM CRYPTOGRAPHY


## 6.1 Introduction to post-quantum cryptographic algorithms

Although there is no stable working version of quantum computers, a corresponding mathematical apparatus (_quantum algorithms_) has already been developed. Its implementation on a quantum computer will allow reducing the complexity of attacks on widely used cryptographic algorithms.

Quantum computations are able to execute some problem solving algorithms with quadratic or even exponential acceleration as compared to the algorithms designed to perform the same tasks on a classic computer (Table 6.1). For example, quantum computers utilizing quantum algorithms are able to solve complex problems of certain classes, including factorization of numbers and search for the discrete logarithm, with efficiency far exceeding the capabilities of classic computers [99–103].

Table 6.1 – Comparison of the time complexity for execution of algorithms for classic and quantum computers  
| Problem type | Algorithm for a classic computer | Algorithm for a quantum computer |
|---|---|---|
| Factorization | Classic Pollard–Strassen algorithm<br>$O(n^{\frac{1}{4}}(\log{n})^{4})$ | Shor’s quantum algorithm<br>$O(\log{n})$ |
| Pell–Fermat equation | Classic AKS primality test algorithm<br>$O((\log{n})^{6})$ | Quantum primality test algorithm<br>$O(n^{2}(\log{\log{n}})(\log{n})^{3})$ |
| Search | Classic binary search algorithm<br>$O(\log{n})$ | Grover’s quantum algorithm<br>$O(n^{\frac{1}{2}})$ |

> _Note. Time complexity means the number of elementary operations performed by the algorithm. Elementary operations for classic computers include the following ones: setting the register to zero, bit shift by one bit, logical addition, logical multiplication. Elementary operations for a quantum computer are the so-called quantum gates._
> 
> _The concept of computational complexity, although described by the number of elementary operations, does not cover the execution time of such operations on a classic or quantum processor. It is because of this there exist misconceptions that a quantum computer will always work faster._

The most famous quantum algorithms – the Shor algorithm and the Grover algorithm – are used to quickly factor a number and speed up a search, respectively. These algorithms threaten many widely used cryptographic primitives of encryption and digital signatures. The security of these primitives is based on the assumption that certain complex computational problems (for instance, the factorization of a number or finding a discrete logarithm) are difficult to solve using available computational resources. Though, for effective implementation of such algorithms, a quantum computer must have a register with a sufficient size (with a sufficient number of qubits).


### Cryptographic primitives vulnerable to quantum computer attacks

The question of exactly when a quantum computer with a sufficiently large number of qubits for the effective implementation of attacks on existing cryptographic algorithms will be built is complex and controversial.

Emergence of such a computer is potentially dangerous for modern cryptosystems. It is worth noting, however, that it took almost twenty years to deploy modern infrastructure using classic crypto primitives, which includes not only mathematical algorithms, but also a set of technical tools for their implementation, as well as related normative regulation. Based on this experience, we can conclude that regardless of the speed of development of quantum computing it is currently necessary to commence preparations for a smooth transition of existing information security systems to more robust algorithms.

> _Note. Scientists from Google AI noticed a pattern (the so-called Neven’s law)_ [104] _that quantum computers are gaining computational power at a double exponential rate._
> 
> _Double exponential growth rate means that a certain quantity grows as a power of a power of two: $2^{2^{1}}$, $2^{2^{2}}$, $2^{2^{3}}$, $2^{2^{4}}$. This rate is the result of a combination of two exponential factors. The first of them indicates that quantum computers have an internal exponential advantage over classic ones: if, for example, there are four qubits in a quantum circuit, then its computational power is comparable to a circuit of 16 ordinary bits. That would be true even without an improvement in quantum technology. The second exponential factor appears due to the rapid improvement of quantum processors. Neven says Google’s best quantum processors  have been improving at an exponential rate lately due to fewer errors, which already allows engineers to build larger quantum processors. In return, if classic computers require exponentially more computational power to simulate quantum processors, and the power of these quantum processors grows exponentially over time, the result is a double exponential relation between quantum and classical computing machines._

Table 6.2 shows the algorithms that are potentially vulnerable to attacks using quantum computers, as well as possible ways to eliminate vulnerabilities for some of them.

Table 6.2 – Possible consequences of quantum computer attacks on most common cryptographic algorithms  
| Cryptographic algorithm | Type | Purpose | Methods for protecting from quantum computer attacks |
|---|---|---|---|
| AES | Symmetric-key cryptography | Encryption | Increasing the sizes of key parameters |
| SHA-2, SHA-3 | — | Hash functions | Increasing the size of output |
| RSA | Asymmetric-key cryptography | Digital signature, key installation algorithms | Cannot be kept secure |
| ECDSA, ECDH | Asymmetric-key cryptography | Digital signature, key exchange algorithms | Cannot be kept secure |
| DSA | Asymmetric-key cryptography | Digital signature, key exchange algorithms | Cannot be kept secure |

As we see, symmetric cryptography can remain resistant to quantum attacks, if one decides to increase the sizes of key parameters. It is also relatively easy to adapt the hash mechanism and continue to use it safely. But with asymmetric cryptography algorithms, such manipulations will be ineffective [105].

Cryptographic algorithms such as RSA, DSA and ECDSA are not quantum-secure, because they cannot be adjusted (by increasing the sizes of key parameters) so as to outpace the development of quantum computing. For example, to attack a 3072-bit RSA key, a quantum computer must have several thousand logical qubits. In general, there is a linear relationship between the RSA key length (in bits) and the required number of qubits in a quantum computer to perform an effective attack on the algorithm. When a quantum computer becomes available, switching to a larger RSA key will prevent a quantum attack until a quantum computer with a bigger number of qubits is invented. However, doubling the RSA or ECC key size increases the operating time of the crypto-primitive on a regular computer by eight times. If, in order to maintain the robustness of such algorithms against quantum attacks, it is necessary to double the size of key parameters every two years, then the execution time of the algorithms on a regular computer increases by eight times every two years. Accordingly, these algorithms quickly become impractical both in terms of speed and in terms of the necessary channel bandwidth for transfering key information.


### Urgency of switching to post-quantum algorithms

In order to continue to safely use public key cryptography even in the presence of quantum computer attacks, it is necessary to apply fundamentally different algorithms that will be based on tasks that are complex even for quantum computers. However, switching to new algorithms necessitates time to review and test all security aspects of new algorithms.

> _Note. Algorithms that will be resistant to quantum computer attacks are called post-quantum algorithms._

There is still a lot of skepticism about quantum computers capable of performing more than one task (for example, running Shor’s algorithm and Grover algorithm on one device without the need to change architectural components). Accordingly, the point how urgently one needs to switch to algorithms that are resistant to quantum computer attacks depends on the following factors [106]:

1. How long is it necessary to ensure the security of cryptographic keys? Denote this value, “term of safe use”, by _x_. There exists data that loses its value in a short period of time (several minutes for a one-time password), and then there is information that must be kept secret for decades (for example, medical history data or strategic information related to national security). For the first data type, _x_ equals 0 years, while for the second data type, it can reach 10, 20, 100 years, etc. The value of _x_ is commonly the system owner’s personal decision for every system requiring protection, or it can be set by the regulator.
2. How long will it take to introduce post-quantum cryptography?” Denote this value, “migration time”, by _y_. For example, _y_ can equal 0 years if it is just a matter of deploying the auto-update that replaces AES-128 with AES-256 in a system that is fully controlled by one vendor. However, _y_ can be no less than 15 years if it involves a relatively unverified public key encryption method that has to be adapted for a limited environment with many participants (interested parties) who need to agree on a standard.
3. Finally, how long will it take until a quantum computer or some other method hacks the currently used public key cryptography tools? Denote this value, “collapse time”, as _z_. If the sum of _x_ and _y_ is greater than _z_, a serious issue arises, since information protected by quantum-vulnerable tools at the end of the next _y_ years can be hacked by quantum attacks in less than _x_ years.


### Potential solutions

Currently, there are two complementary families of potential solutions to the above-discussed risks.

One group of solutions is called _post-quantum cryptography_. It includes ordinary crypto primitives based on mathematical problems, which, in the scientific community’s opinion (based on theoretical calculations), are protected from attacks performed with quantum computers. These solutions are convenient in the sense that the corresponding algorithms can work on classic computers as well. In turn, this simplifies the research processes even before the advent of sufficiently powerful quantum computers. However, application of these solutions does not mitigate the issue of “computational security”, that is, we assume that the complexity of brute force attacks using a quantum computer will be high enough to ensure the stability of systems using such algorithms (discussed in more detail below).

> _Note. According to the classic definition, an algorithm is considered to be secure if the most effective attack on it is the direct iteration over key parameters (the brute force attack). However, for post-quantum algorithms, it is also necessary to fulfill one more condition, namely, resistance to attacks using the Shor_ [107] _and the Grover_ [108] _algorithms._

Another family of solutions is known as _quantum cryptography_ [109] – a method of protecting communications based on the principles of quantum physics. Unlike traditional cryptography which uses mathematical methods to ensure the secrecy of information, quantum cryptography considers the cases for which information is transferred using the quantum mechanics objects, such as electrons in electric currents or photons in fiber-optic communication lines, since the process of sending and receiving information is always performed by physical means.

An attack on such systems (in terms of quantum cryptography, “eavesdropping” is called an attack) may be performed through modifying certain parameters of physical objects (information carriers).

The technology of quantum cryptography is based on the Heisenberg’s uncertainty principle [110]: it is impossible to simultaneously obtain the coordinates and the momentum of a particle; it is impossible to measure one photon parameter without distorting the other.

Using quantum phenomena, one can design and create a communication system that can always detect eavesdropping. This is ensured by the fact that an attempt to measure interrelated parameters in a quantum system introduces distortions into it, destroying the original signals. Hence, honest users can recognize the presence of an eavesdropper by the noise level in the channel.

The disadvantage of these solutions is the need to set up a quantum channel (Fig. 6.1). In order to send qubits, both interacting parties are required to have equipment specifically designed for creating and maintaining quantum channels. Such equipment is very expensive, and it is currently unavailable for widespread use.

![Figure 6.1 – Quantum channel setup scheme](/resources/img/volume-3/6.1-Introduction-to-post-quantum-cryptographic-algorithms/F-6.1-quantum-channel-setup.png "Figure 6.1 – Quantum channel setup scheme")

In the short term, such quantum channels may become accessible from point to point at relatively short distances. It may be possible that in the medium and long term quantum communication and quantum repeaters will allow setting up safe communication channels over long distances.

Next, we will discuss what tasks are computationally complex for quantum computers. To date, there are four main tasks in different mathematical apparati which underlay the families of post-quantum primitives.


### Lattice-based cryptography (LB cryptography)

LB cryptography is an approach to building asymmetric encryption and digital signature algorithms using problems of the theory of algebraic lattices.

An algebraic lattice is a discrete subset of vectors in the Euclidean space $\mathbb{R}^{n}$, which is closed with respect to the addition and subtraction of vectors. We say that a lattice has dimension _n_ if it does not fit in any subspace of $\mathbb{R}^{n}$. Geometrically, a lattice is a set of points equally placed in space through which vectors pass. An algebraic lattice can be considered as an abelian group.

The basis of the lattice _L_ is the set of vectors _B_ such that any other vector in the lattice can be represented as a linear combination (with integer coefficients) of the elements from set _B_. If the dimension of the lattice is at least 2, then there always exists an infinite set of different lattice bases (Fig. 6.2).

![Figure 6.2 – Many bases in the lattice](/resources/img/volume-3/6.1-Introduction-to-post-quantum-cryptographic-algorithms/F-6.2-many bases-in-lattice.png "Figure 6.2 – Many bases in the lattice")

Algebraic lattices, as a mathematical construction, were first studied by Lagrange and Gauss. In cryptography, they were used, for example, for cryptanalysis of congruent generators. In 1996, M. Aitai showed how lattices can be used as a new crypto conversion solution. In 2009, Gentry used lattices to build a new homomorphic cryptosystem.

The stability of LB cryptosystems is based on the complexity of solving two combinatorial problems:

1. Shortest vector problem (SVP): search for the shortest vector in the lattice _L_ presented in the basis _B_. The lattice encryption algorithms are constructed based on this problem. The private key in this system is the lattice basis _B_ and a specially selected _unimodular_ matrix _U_ (a square matrix with integer coefficients for which the determinant equals +1 or −1). The public key is the basis $B'=BU$. Knowing the basis, we can generate a vector close to the given point, but knowing the close vector, we need an initial basis to find the starting point. An example of such a system is NTRUEncrypt [111].
2. Closest vector problem (CVP): for a given basis _B_ of the lattice _L_ and some vector $v \in L$, it is necessary to find the vector $v' \in L$ which is the closest one to the vector _v_. Algorithms of digital signatures on lattices are built on this problem. The idea is to use lattices for which the “bad” basis (long and almost parallel vectors) is a public key, and the “good” basis (short and almost orthogonal vectors) is a private key. A relevant example is the NTRUSign digital signature algorithm [112].

The advantages of LB algorithms are their relative simplicity and ease of parallelizing the pre-computations necessary for encryption/decryption operations and generation/verification of digital signatures. On the other hand, it turned out to be rather difficult to give accurate estimates of the security of “lattice schemes” when compared to known cryptanalysis methods.


### Code-based cryptography (CB cryptography)

CB cryptography includes error correction code-based systems, such as McEliece [113] and Niederreiter encryption algorithms, and Courtois, Finiasz, and Sendrier signature schemes. The main technique is to append, in a special way, some redundant information (e. g., a checksum) to the payload when writing or transmitting it. When reading or receiving the data, one can use this redundant information to detect and correct errors. The number of errors that can be corrected is limited and depends on the specific code applied.

CB cryptography uses the complexity of decoding linear codes. The public key in such a system is a matrix _K_ with the dimension $dt \times n$ and coefficients in the field $\mathrm{GF}(2)$. The message (_m_) is an _n_-bit string with a weight of _t_, that is, an _n_-bit string in the composition has exactly _t_ bits, which are equal to 1; it must be pre-formatted. In order to encrypt the message _m_, the sender forms a $dt$-bit ($d= \lg{n}$) ciphertext $C=Km$.

The main task for the attacker is finding a decoding syndrome for the matrix _K_. That is, it means to obtain the result of the multiplication of _K_, given a vector with weight _t_ as input. This can be done by linear algebra methods: one can perform the inverse operation for restoring _C_ to an _n_-bit vector _v_ such that $Kv=C$. However, there exists a very large number of such vectors _v_, and the search for a _t_-vector among them is extremely hard. The most famous attack on this problem today is of exponential complexity for most _K_.

The McEliece cryptosystem was first introduced in 1978; since then no effective attacks have appeared. Later other systems based on error correction codes were proposed. Although code-based primitives are pretty fast, they use keys with too big sizes. In most cases, use of the code apparatus turned out to be more convenient for implementing encryption schemes rather than digital signature algorithms.


### Multivariate polynomial cryptography (MQ cryptography)

The safe use of crypto primitives of this group in conditions of attacks using quantum computers is based on the assumption that solving multidimensional polynomial systems over finite fields is a complex task.

The public key in this system is the sequence $(P_{1} P_{2}, \mathellipsis ,P_{2b})$ of $2b$ polynomials with $4b$ variables $(ω_{1},ω_{2}, \mathellipsis ,ω_{4b})$ with coefficients in a field $\mathrm{GF}(2)$. Each polynomial is presented as a sequence $(1,ω_{1},ω_{2}, \mathellipsis ,ω_{4b},ω_{1}ω_{2},ω_{1}ω_{3}, \mathellipsis ,ω_{4b-1}ω_{4b})$. The public key consists of $16b^{3}+4b^{2}+2b$ bits. For example, for $b=128$, the key size will be 4 MB.

The advantage of MQ cryptography is namely the size of signatures and public keys.

The main task for an attacker is to find a sequence $(ω_{1},ω_{2}, \mathellipsis ,ω_{4b})$ of $4b$ bits, which yields the original sequence $(P_{1} P_{2}, \mathellipsis ,P_{2b})$ of $2b$ bits. The probability of guessing a sequence of $4b$ bits can be estimated as $2^{-2b}$.


### Hash-based cryptography (HB cryptography)

HB cryptography uses one-time signature schemes such as Lamport–Diffie or Winternitz. The stability of these schemes is based solely on the stability of the hash function used. A feature of the Lamport–Diffie and Winternitz schemes is the inability of their secure use more than once. To eliminate these shortcomings, binary trees are used. The basic idea is that each position on the tree is calculated as the hash value of the concatenation of the child nodes of the tree. The root node of the tree is a public key, which is calculated sequentially (Fig. 6.3). The leaves of the tree store the values of one-time keys.

![Figure 6.3 – Architecture of hash-based signature algorithms](/resources/img/volume-3/6.1-Introduction-to-post-quantum-cryptographic-algorithms/F-6.3-architecture-of-hash-based-signature-algorithms.png "Figure 6.3 – Architecture of hash-based signature algorithms")

The idea of using the tree architecture was proposed in 1979 by Ralph C. Merkle. It has a number of disadvantages, such as the big size of keys and the long signature generation time. Currently, known and used algorithms include: modifications of the Merkle algorithm, such as the Leighton–Micali algorithm; XMSS algorithm; and the SPHINCS algorithm discussed further in this book.


### Alternative groups

Many systems that are not included by the above-mentioned groups have been proposed. One such proposal is based on the estimation of isogenies of supersingular elliptic curves.

> _Note. The Hasse’s theorem states that for the number of points N of a curve defined over the field $\mathrm{GF}(q)$, the following equation is true: $|N-(q+1)| \leq 2 \sqrt{q}$. For a curve defined over the binary finite field $\mathrm{GF}(2^{n})$, it can be stated that $N=2^{n}+1-t$, where $|t| \leq \sqrt{2^{n}}$._
> 
> _If t is divisible by the field characteristic, an elliptic curve over a binary finite field is called supersingular_ [114]_._

The discrete logarithm problem on elliptic curves can be effectively solved using the Shor algorithm on a quantum computer. However, for the problem of finding an _isogeny_ (that is, a rational mapping that turns points of one curve to points of an isogenic curve, while leaving the point at infinity “unaffected”) on supersingular curves, there is no similar quantum attack.

### \*\*\*Common myths\*\*\*

_As soon as a quantum computer is created that effectively solves the problem of finding a discrete logarithm in a group of elliptic curve points, ECDSA will immediately become useless._

This statement will not be true in all cases. In some cases, ECDSA can remain secure enough, for example, if the lifetime of a public key is shorter than the attack time or if keys are used only once and the public key is published simultaneously with or later than the digital signature. This is exactly why ECDSA and hash values of public keys as addresses can continue being used in Bitcoin.

### \*\*\*Frequently asked questions\*\*\*

_– Is it possible now to use post-quantum algorithms in practice without waiting for the actual emergence of quantum computers?_

Post-quantum cryptography can be used today, however, modern infrastructure is not yet ready for its use. For example, in order to implement the use of hash-based signatures in an existing PKI, it is necessary to solve a number of issues, including at least the following: change the formats of public key certificates and define the policy for updating them. After all, there is a fundamental difference: according to the current approach, the key pair is issued for a fixed time period, whereas in the case of hash-based signatures, the expiration time of a public key depends on the number of signatures applied.


## 6.2 Hash-based signature algorithms

In Volume 2 of the book, we discussed one-time signature algorithms such as Lamport OTS and WOTS. The use of these algorithms in conjunction with Merkle trees formed the basis of post-quantum hash-based digital signature algorithms. In this section, we discuss these algorithms, their properties, and features of their architecture.

Today, the most well-known and promising digital signature algorithms that are built based on hash functions are the SPHINCS family of algorithms [115–118]. Moreover, SPHINCS+ already entered the second round of the NIST Post-Quantum Cryptography Standardization Process [119]; it competes with eight other projects for the title of the new digital signature standard. In this section, we discuss how it was possible to develop a digital signature algorithm that can provide a sufficient level of security even against a quantum computer using a combination of OTS (WOTS), HORS (discussed further below) and Merkle trees.


### HORS construction

Before considering the SPHINCS design, it makes sense to describe how one component of this algorithm works, namely HORS (Hash to Obtain Random Subset) [120]. This few-time signature scheme was described in 2002, and it extends the Lamport OTS scheme, allowing for a smaller signature size. The security of the algorithm is based only on the hash functions used [121].

To begin with, let’s examine how a user generates keys. Suppose that the algorithm uses two hash functions with an output length of 160 bits (while HORS does not actually determine which specific hash functions should be used, for our example, it will be enough to just indicate the length of the output hash value). Also, set the length of the private key parts to 160 bits.

In addition to these parameters, we are interested in two more parameters: _t_ and _k_. The parameter _t_ determines the number of parts of the private and public keys, and the parameter _k_ determines the number of blocks into which the hash value of the message to be signed will be divided. In this case, the following dependence exists between the parameters: $t=2^{\frac{l}{k}}$, where _l_ is the length of the hash value of the message being signed. Note how important the choice of these parameters is. If we define $k=160$, then $t=2$. This means that the private key will consist of only two values, each of which we will sign either “0” or “1”. In this case, the signature size will be more than 3 kilobytes (as if we used Lamport OTS), and the value of the public key will take 40 bytes. If one chooses $k=1$, it will be necessary to generate $2^{160}$ parts of the private key, which is quite problematic in practice.

For our example, we will try to choose the optimal values of these parameters: $k=16$, $t=1024$. When the parameters are selected and agreed between the participants in the system, the user who needs to sign the message generates a private key. In this case, the number of parts is 1024 (Fig. 6.4).

![Figure 6.4 – Private key structure](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.4-private-key-structure.png "Figure 6.4 – Private key structure")

After the private key is generated, the user calculates the public key, which also has 1024 parts. To obtain the public key from the private key, a hash function is used, in our case with an output length (in the context of the example) of 160 bits (Fig. 6.5).

![Figure 6.5 – Public key computation](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.5-public-key-computation.png "Figure 6.5 – Public key computation")

The public key is distributed among the verifiers so that they can use it to check the signature calculated by the user.

> _Note. In the described example, the value of the public key (a set of its parts) is quite large (about 20 kilobyte), which is not very convenient for verifiers to store. There is a way to compress the public key (for our example, up to 160 bit), by slightly modifying the signature algorithm. For this purpose, the Merkle tree is used, which collects the subkeys into one root value. Here, we skip the details of key compression, however, we will surely examine them later._

Let’s consider how the signing flow is performed. The user calculates the hash value of the message. After that, he splits the resulting hash value into _k_ blocks of equal size. In our example, there will be 16 10-bit blocks (Fig. 6.6).

![Figure 6.6 – Dividing the message hash value into blocks](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.6-dividing-message-hash-value-into-blocks.png "Figure 6.6 – Dividing the message hash value into blocks")

Each of these blocks will be processed separately, and the signature value will contain _k_ private keys of equal size. Further, each of the blocks is considered as a natural number (Fig. 6.7).

![Figure 6.7 – Conversion of blocks to integer values](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.7-conversion-of-blocks-to-integer-values.png "Figure 6.7 – Conversion of blocks to integer values")

When using the HORS scheme, parts of the signature are parts of the private key that have previously obtained indices (integer values of blocks). That is, in our example, the signature is a set of values as shown in Fig. 6.8.

![Figure 6.8 – Signature structure](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.8-signature-structure.png "Figure 6.8 – Signature structure")

Now consider how the obtained signature value can be verified using the public key. To do this, the verifier repeats exactly the same operations for the message: calculates its hash value, divides it into blocks and considers each block as a natural number (Fig. 6.7, 6.8). Given the obtained indices, the verifier selects parts of the public key that need to be used for verification. For our example, a set of these parts is shown in Fig. 6.9.

![Figure 6.9 – Public key for signature verification](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.9-public-key-for-signature-verification.png "Figure 6.9 – Public key for signature verification")

The signature verification here is very similar to the Lamport one-time signature verification: each part of the signature is hashed and compared with the corresponding part of the public key (Fig. 6.10). If all relevant parts are equal, then the signature is considered correct.

![Figure 6.10 – Signature verification flow](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.10-signature-verification.png "Figure 6.10 – Signature verification flow")


### Merkle signature scheme

Now we will consider the Merkle signature scheme (MSS), as SPHINCS is an extension of this algorithm. The Merkle signature scheme creates a few-time signature. This means that unlike the algorithms examined earlier, it allows signing several different messages and utilizing the same public key value for their verification.

This signature algorithm uses keys and a one-time signature mechanism (Lamport OTS, WOTS, or any other similar algorithm), which in turn are linked into a tree structure with a single root value.

Let’s start with how a user generates keys. A parameter is chosen that determines the number of messages that can be verified with one public key. Suppose this parameter is equal to 4 (we can sign 4 different messages that will be verified with the same public key). Therefore, the next step is to create 4 pairs of OTS keys (Fig. 6.11).

![Figure 6.11 – Generation of keys for signature](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.11-generation-of-keys-for-signature.png "Figure 6.11 – Generation of keys for signature")

> _Note. Recall that the Lamport OTS scheme involves generation of $2n$ parts of a private key, where n is the length of the output value of the used hash function. The signature value is the values ​​of the private key parts that match each bit of the hash function output (depending on the output bit value). During the verification process, the verifier calculates hash values ​​from the parts of the signature and checks them against the parts of the public key. Using Lamport OTS is optional; alternatively, WOTS can be used (both algorithms are discussed in Volume 2 of the book)._

With each one-time signature private key, the user will actually sign the message. However, using the Merkle tree allows sending verifiers one value instead of 4 different public keys. How exactly? The calculated public keys are considered as the leaves of the Merkle tree. Next, the leaves of the tree are hashed and their hash values ​​are concatenated with each other to be hashed once again and form tree nodes. In the end, a single root value (a Merkle root) is obtained, which is the “global” public key for all OTS public keys (Fig. 6.12).

The global public key is transmitted by the verifier and is used to check the signatures of messages that were signed by any of the one-time-signature child private keys.

![Figure 6.12 – Public key formation procedure](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.12-public-key-formation.png "Figure 6.12 – Public key formation procedure")

How is signing performed? The user calculates the hash value of the message and a one-time signature (OTS) for this hash value. The user attaches the value of the corresponding public key and Merkle branch to this value to authenticate it in the Merkle tree. This entire data set is attached to the message as a digital signature (Fig. 6.13).

![Figure 6.13 – Signature structure](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.13-signature-structure.png "Figure 6.13 – Signature structure")

If we sign the message using a _private\_key<sub>1</sub>_, then the value of _public\_key<sub>1</sub>_, as well as _H<sub>1</sub>_ and _H<sub>11</sub>_, which are the Merkle branches for _public\_key<sub>1</sub>_, are attached to the OTS signature value.

The signature verification consists of two stages: verification of a one-time signature and checking that the specified public key belongs to the global public key (authentication in the Merkle tree). For this purpose, the verifier submits the signature value itself and the value of the specified public key to the input of the OTS verification function (Fig. 6.14).

![Figure 6.14 – Signature verification scheme](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.14-signature-verification.png "Figure 6.14 – Signature verification scheme")

If the value of the one-time signature is correct, the public key is checked for existing in the Merkle tree, in which the root stores the value of the global public key. That is, given the Merkle branch values, the verifier performs the check of a specific public key in the Merkle tree (Fig. 6.15).

![Figure 6.15 – Verification of the existence of a public key in a tree](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.15-verification-of-existence-of-public-key-in-tree.png "Figure 6.15 – Verification of the existence of a public key in a tree")

> _Note. In 2011, an extended version of the Merkle signature – XMSS – was proposed (Fig. 6.16). Its main distinction is the use of the WOTS+ algorithm_ [122]_. In addition, it uses additional masks for the nodes of the Merkle tree before their concatenation and further hashing. This feature must ensure security in case the used hash function is non-resistant to second-type collisions_ [123; 124]_._
> 
> _Since we are considering the SPHINCS algorithm further, note that, like XMSS, it uses bit masks before the hashing of the Merkle tree nodes._

![Figure 6.16 – XMSS signature scheme](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.16-XMSS-signature-scheme.png "Figure 6.16 – XMSS signature scheme")


### SPHINCS algorithm family

Now let’s examine the design of SPHINCS algorithms that use the previously described schemes. An important feature of these algorithms is that they consist of a large number of trees, which together make up a _hypertree_. The proposed hypertree height in various family algorithms is in the range of 50–70 (for example, the proposed height for SPHINCS-256 [118] is 60). However, for an example, let’s take a hypertree with a height of 9.

This hypertree, in turn, is divided into layers (the number of which is usually around 10), each of which stores instances of Merkle subtrees. For example, let the number of layers be equal to 3.

What are the Merkle subtrees that we use for an example? Each  tree has a height equal to 3. This value is equal to the total height of the hypertree divided by the number of layers in it. These subtrees store compressed open WOTS (WOTS+) keys in their leaves.

There are HORST public keys in the hypertree leaves; each of them is a HORS public key value compressed using the Merkle tree. At the top of the hypertree there is a value of the global public key. A full scheme of the SPHINCS hypertree is shown in Fig. 6.17.

![Figure 6.17 – Hypertree architecture](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.17-hypertree-architecture.png "Figure 6.17 – Hypertree architecture")

Let’s take a closer look at the components of this scheme. At the tree peak, there is a global public key value that is passed to the verifier. This value is obtained by concatenating and hashing the leaves of the subtree of the top layer of the hypertree (layer 2 in Fig. 6.17).

What are subtree leaves? The subtree leaves are compressed WOTS public keys. What is a compressed public key? Let’s recall the point: an ordinary WOTS key is a set of key parts that have a large size (in the range of 2–8 kB). However, parts of the key can be added to the structure of the Merkle tree and get a compressed value with the size of 128–256 bits. At the same time, the Merkle branch value is added to the signature, which allows proving the fact that the key parts match the compressed value.

These compressed public keys are used to verify the signature of the public keys, which are located on the lower layer of the hypertree. The lower layers of the hypertree function in the same way up to the HORST public key instances, which match the private keys with which the final messages are signed.

Consider the key generation process for the SPHINCS algorithm. Initially, the user generates two secret values ​​(most often 256-bit ones) – _S<sub>1</sub>_ and _S<sub>2</sub>_. The first value will later be used to generate auxiliary keys (for WOTS and HORS signatures). The second is used to select the hypertree leaf that will be used for signature and to add randomness when hashing a message. Following this, a set of _Q_ masks is generated, which will be used to obtain WOTS and HORS signatures and to build Merkle trees. A private key actually consists of the obtained components ​​(Fig. 6.18).

![Figure 6.18 – Private key components](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.18-private-key-components.png "Figure 6.18 – Private key components")

To obtain the public key, one needs to build a subtree that is located on the hypertree peak. To do this, we generate WOTS private keys for the leaves of this subtree. Note that such keys are generated in a deterministic manner, depending on the index of the WOTS instance and the secret value _S<sub>1</sub>_. For our example, we generate 8 secret keys for a one-time WOTS signature, then, given them, we calculate the values ​​of the public keys (the compressed ones) which will be leaves of the top-level subtree. Further, these keys are added to the tree structure in which the root stores the SPHINCS public key value (Fig. 6.19).

![Figure 6.19 – Public key in SPHINCS](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.19-public-key-in-SPHINCS.png "Figure 6.19 – Public key in SPHINCS")

A list of masks _Q_ is added to the value of the public key. This list is necessary for signature verification. As a result, the public key consists of the values of _global\_public\_key_ and _Q_.

Now let’s move on to calculating the signature. An important feature of the SPHINCS algorithm is that calculation of the signature starts at the bottom of the hypertree (on the leaf level) and moves toward the subtree on the hypertree peak. In our example (a hypertree with a height of 9 and 3 layers), a total of 512 leaves which are compressed HORS public keys are obtained. Therefore, the public key must be first used to select from which leaf the message will be signed.

The required leaf is selected based on the hash value of the signed message itself and the secret value _S<sub>2</sub>_. To do this, we use a function that generates a pseudo-random sequence (in most cases, it is a hash function), which takes a message and _S<sub>2</sub>_ as input and returns the tree leaf index value (_index_) and the additional value of _R_ (Fig. 6.20).

![Figure 6.20 – Obtaining the index of a tree leaf for signature](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.20-obtaining-index-of-tree-leaf.png "Figure 6.20 – Obtaining the index of a tree leaf for signature")

What is _R_ used for? This value is a kind of salt, which we attach to the message to be signed before calculating its hash value to add entropy (_R_ is also attached to the signature value). Due to this, the index is a completely deterministic value and is the same for multiple signatures of one message. In turn, the value of _R_ attached to the signature allows calculating the correct signed hash value by the party that verifies the signature.

After the index of the HORS instance for message signature is chosen, the corresponding key pair of this instance must be generated. This pair is also deterministically generated from the _S<sub>1</sub>_ secret. Using the generated key pair, the hash value of the message is signed (Fig. 6.21). _Index_, _R_, and the HORS value obtained at this stage are parts of the final SPHINCS signature.

![Figure 6.21 – Addition of signature values](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.21-addition-of-signature-values.png "Figure 6.21 – Addition of signature values")

Next, we compute the compressed public key of the HORS instance. We will sign this public key using the WOTS instance, which is located above HORS in the hierarchy. To do this, we generate a new key pair for this WOTS instance and calculate the signature value for the HORS key. The value of this signature is also added to the SPHINCS global signature.

However, the procedure is not over yet. The public key of the WOTS signature must be authenticated in its own subtree (Fig. 6.22). Therefore, we calculate the Merkle branch which leads from this key to the subtree root value. To do this, we need to generate key pairs for each WOTS leaf and build the corresponding tree.

![Figure 6.22 – Key authentication flow](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.22-key-authentication.png "Figure 6.22 – Key authentication flow")

In addition to the global SPHINCS signature value, a corresponding Merkle branch subtree for WOTS is added. In the same manner, a signature is created at each subsequent level of the hypertree, i.e.,  each subsequent WOTS instance signs the value of the public key at the lower level. Ultimately, for our example, the signature value will look as shown in Fig. 6.23.

![Figure 6.23 – SPHINCS signature value](/resources/img/volume-3/6.2-Hash-based-signature-algorithms/F-6.23-SPHINCS-signature-value.png "Figure 6.23 – SPHINCS signature value")

On each hypertree layer, the WOTS signature value and the authentication path are added to the global signature value. The authentication path is a set of values that prove that a specific key belongs to the root value.

The signature verification procedure consists of fewer steps since it does not require key generation for all leaves of a particular tree. The input parameters for verification are the global public key and the signature for a specific message.

In the first step, the verifier calculates the hash value of the message. To do this, he uses the message itself and the value of _R_, which is contained in the signature. Next, the HORS signature is checked for the obtained value. Note that the value of the signature does not include the value of the HORS public key. However, due to the fact that the HORS public key is the compressed hash value of the signature value, given the signature value, we can easily obtain the necessary public key.

After that, we need to verify the signature of the obtained key with the top-level WOTS instance (to calculate the corresponding public key given the signature). Next, we need to verify that the obtained value is part of the subtree structure, using the Merkle branch. Since the Merkle branch is in the signature, we can calculate the root value of the subtree. This public key value is similarly signed with the higher-level WOTS instance.

We move up the hypertree using the authentication paths and the values ​​of the lower-layer leaves. In the end, we obtain the root value, which is then compared with the value of the user’s global public key. If the values ​​are equal, the signature is considered correct.

# [7 STABLECOINS AND DECENTRALIZED FINANCE](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/7-Stablecoins-and-decentralized-finance.md)
