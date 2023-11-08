# 3 TECHNOLOGICAL DETAILS OF BITCOIN OPERATION  


## 3.1 Bitcoin Script design and features

There are three main approaches to setting coin distribution conditions:

* Turing-complete languages (Ethereum – Solidity)
* Non-Turing-complete languages (Bitcoin – Bitcoin Script)
* Template smart contracts (Bitshares, Stellar, etc.)

For a better understanding of the differences between each of them, let’s examine the following figure (Fig. 3.1):

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.1-Three-approaches-to-setting-conditions.png" alt="Figure 3.1 – Three approaches to setting conditions"/> 

*Turing-complete languages* allow one to set arbitrary coin spending conditions and implement complex smart contracts with cycles, function calls, and much more. A certain limitation is the need for a strict code audit, as it is unclear, in advance, what malfunction any error in the smart contract code may result in.

*Non-Turing-complete languages*, such as Bitcoin Script, allow one to use a set of predefined operations in any sequence. Typically, the set of such operations is limited to several dozen, but at the same time, the operations are implemented safely and with their help, you can set the most popular (and most frequently used) conditions without any problems.

*Template smart contracts* are a set of ready-made solutions. This set is very limited and the user cannot add any additional functionality (at least quickly – for this she will need to update the protocol). However, the implementation of these functions is the most optimal and secure.

In this section, we will examine the Bitcoin Script structure in detail. The section will be broken down into two parts: first, the main available operations and the underlying principles and secondly the application features and real-life examples of how  Bitcoin Script is used to set conditions for performing transactions. 

Despite the fact that Bitcoin Script does not allow you to write an arbitrary smart contract, it is actively used by developers to implement protocols that work on top of Bitcoin. Thus, it is possible to implement a scenario in which coins can be spent either if there are several digital signatures or under any other conditions (only after a certain amount of time has passed).

As was noted in the first volume of the book, Bitcoin transaction verification involves checking the coins ownership proof (evidence) that must satisfy the conditions for coins spending. It should be noted that this evidence is contained in the input of the transaction, and the spending conditions are contained in the corresponding outputs (Fig. 3.2).

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.2-Bitcoin-Transaction-Structure.png" alt="Figure 3.2 – Bitcoin Transaction Structure"/> 

These conditions and proof are usually generated automatically by digital wallets. To describe them, the Bitcoin protocol uses a special language, Bitcoin Script. In addition to payments onto usual addresses, this language allows implementing a quite complex coin spending terms, particularly those which can be satisfied only at a specific moment in time.

As you may know, each transaction has fields called scriptPubKey and scriptSig. The scriptPubKey field contains a description of some terms which a user must satisfy in order to spend coins. The scriptSig field contains the data necessary to satisfy such terms. Together, both fields are Script. Nodes of the network execute this script and, based on the results of the execution, decide whether the transaction is valid. In essence, Bitcoin Script allows you to verify transactions in which conditions can be described in arbitrary order.

### How is Bitcoin Script executed?

Bitcoin Script is a non-Turing-complete instruction description language. In Bitcoin, it is used to both set and satisfy the rules for spending coins. The language is stack-based and uses the "Reverse Polish notation" for operands.

Non-Turing-complete means that the language has limited functionality and does not support the execution of jumps and loops. Consequently, the script excludes the possibility of entering an infinite loop, which allows you to limit malicious parties’ ability to create complex transactions and slow down the entire system.

"Reverse Polish notation" implies that the operator follows the operands, and the expression is read from left to right. This type is much simpler than ordinary algebraic, therefore, it entails fewer computational errors. As an example, let's consider the performing of a few arithmetic operations on numbers. In a normal algebraic representation, the expression looks as follows:

**(2 + 4) * 5 / 10**

If we present this example as a stack operations set, then it will have the following form:

**24 + 5 * 10 /**

Slightly unusual, right? However, the diagram below clearly shows the sequence of all the computational steps (Fig. 3.3).

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.3-Example-of-processing-Stack-Elements.png" alt="Figure 3.3 – Example of processing Stack Elements"/> 

Step by step, everything is done as follows:
1. First, the value 2 is placed on the stack.
2. Next, the value 4 is pushed to the stack. Now at the top of the stack is the value 4, and below it, there is a value 2.
3. Next, the addition operation is performed. To do this, two upper stack values are taken out, added together and the result is placed back to the top of the stack. Now at the top of the stack is a value of 6.
4. Next, the value 5 is placed on top of the stack.
5. The product of the upper two values of the stack 5 and 6 is executed and the result (30) is placed on top of the stack.
6. A value of 10 is placed on the stack.
7. For the division operation, the upper operand (10) acts as a divisor, and the result (3) is placed on top of the stack.
	
As we can see, the calculations using this scheme are quite simple and unambiguous. However, above we examined an example with simple arithmetic operations, where operands were natural numbers. In Bitcoin Script, operations and operands are more complex and diverse.

### Operations in Bitcoin Script

For operations in Bitcoin Script, there is a special form of writing - OP_code (hereinafter OP-code). Each OP-code can be split into two parts: «OP_» prefix and name directly operation. A specific OP-code tells the computer (virtual processor) which particular sequence of actions should be performed during its execution. Each operation is represented by a set of bits that are read and executed by a virtual processor.

> **Operations in Bitcoin Script**
>> *  *7 execution control operations*
>> *  *19 stack interaction operations*
>> *  *27 arithmetic operations*
>> *  *10 cryptographic operations*

Execution control operations include OP-codes such as OP_IF, OP_ELSE, OP_NOTIF, OP_ENDIF, OP_RETURN, which enables Bitcoin script branching operations (and thus provides the ability to create multiple independent coin spending conditions). The logic of such operations is not different from the logic of executing if / else statements in familiar programming languages. Remember that Bitcoin Script is a  non-Turing-complete language, so there are no loop operators, such as for, while, and others.

Stack interaction operations are represented by OP-codes such as OP_DROP, OP_DUP, OP_ROLL, OP_SWAP, OP_ROT, etc. They are designed to perform with values from the stack and allow you to control its elements (delete, move, duplicate elements, etc.).

Stack item comparison operations include operations OP_EQUAL and OP_EQUALVERIFY. The difference between these operations is that OP_EQUAL only returns a true/false value (comparing operands result), and based on it OP_EQUALVERIFY terminates or continues to execute the next script operations.

Arithmetic operations include addition (OP_ADD), comparison (OP_NUMEQUAL), the minimum and maximum numbers finding operations (OP_MIN, OP_MAX) and many others.

Cryptographic operations include the operations OP_RIPEMD160, OP_SHA1, OP_SHA256, which allow you to calculate the corresponding hash values. Operations OP_HASH160 and OP_HASH256 calculate a double hash (in the first case, this is SHA-256 and RIPEMD sequential calculation, in the second, a double-taken hash SHA-256). Single signature and multi-signature verifications are also included  (OP_CHECKSIG, OP_CHECKMULTISIG).

Noteworthy, all operations, except for the operations of pushing values to the stack, are argumentless. This means that these operations work only with values that are on the stack, and it is impossible to input a value that is outside the stack. Therefore, when using the Bitcoin script and setting the conditions, you need to consider that the maximum stack capacity is limited to 520 bytes.

In conclusion, the material presented above gives you an idea about the features of Bitcoin Script and computing in it with existing operations. Now let’s have a look at how it is used in real bitcoin transactions.

### Example of Bitcoin Script execution for P2PKH

Let’s look at the case when coins are sent to a regular address that is associated with the specific hash of one public key (P2PKH, or pay-to-public-key-hash). The script that sets the spending conditions, combined with a script that satisfies these conditions, is presented in Figure 3.4.

You see a data stack on the left-hand side, on the right-hand side the script itself. The first two components of the script line are the value of the signature and public key - the so-called unlocking script, that is, the script that is indicated in the transaction that spends coins input (located in the scriptSig field). Followed by the data set is the locking script that indicates the transaction output (transaction scriptPubKey field).

In other words, the diagram shows the concatenation of two scripts: a script that unlocks coins, and a script that locks coins. When the transaction is validated by a node, these two scripts are combined to perform a check of the conditions for coin spending.

After the scripts‘ concatenation, the complete set of operands is sequentially executed. The execution pointer goes sequentially for each operand and each piece of data. If the execution pointer points to a piece of data, then it is pushed onto the stack. As we see in the diagram, the script execution pointer at the top indicates the signature data, which is later pushed onto the stack. Next, the execution pointer points to the public key - it is also pushed onto the stack. The third step is to perform the duplication operation, which implies copying the value that is on the top of the stack and placing this data again on the stack (at the top was the value of the public key).

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.4-The-first-step-of-script-execution.png" alt="Figure 3.4 - The first step of script execution"/> 

This means that the top value of the stack is hashed by the SHA-2 algorithm at a length of 256 bits first, and then by the RIPEMD-160 function at a length of 160 bits. The operation is exactly the same as hashing the public key when receiving the address. The actual value received is the address.

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.5-The-second-step-of-script-execution.png" alt="Figure 3.5 – The second step of script execution"/> 

We have a signature, a public key, and a hash value of the public key in the stack. The script execution pointer points to the address that was specified in the transaction output. This piece of data also gets on the stack (Fig. 3.6).

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.6-The-third-step-of-script-execution.png" alt="Figure 3.6 – The third step of script execution"/> 

The next operation is the comparison operation OP_EQUALVERIFY. Two top elements of the stack are compared. If they are equal (byte by byte), then this data is deleted from the stack and true is returned (this value is not placed on the stack, but it determines whether the script will be executed further) and it is believed that the check was successful. After that, the signature and public key values remain on the stack. Accordingly, the OP_CHECKSIG operation takes these two operands and verifies the signature using the public key (Fig. 3.7).

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.7-The-fourth-step-of-script-execution.png" alt="Figure 3.7 – The fourth step of script execution"/> 

If the transaction signature is valid (the signature covers the fields of the transaction), then the result of the verification is true, and this value is pushed to the stack. This completes the execution of the script. Data is passed to the calling function and checked there. If the stack is true, then the verification of this transaction input was correct. If all transaction inputs have been correctly verified, then the entire transaction is considered correct.

### Example with multisignature

Now, let's look at how multi-signature works using the Bitcoin Script. A condition that requires several keys is blocked by the following script (as an example, we implement 2 of 3 multi-signature):

**scriptPubKey : OP_0 OP_2 \<pubKeyA> \<pubKeyB> \<pubKeyC>  OP_3 OP_CHECKMULTISIG**

The unlock script will look as following:

**scriptSig : \<sig1> \<sig2>**

In this case, OP_2 indicates the number of signatures that must be provided in scriptSig, and OP_3 indicates the number of public keys provided that the signatures must match.

The script is initiated based on the fact that the values of the signatures that are included in the scriptSig field are pushed onto the stack (Fig. 3.8).

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.8-The-first-step-of-script-execution.png" alt="Figure 3.8 – The first step of script execution"/> 

The values 0 and 2 are placed on the stack (according to the number of signatures that should be verified), as depicted in Figure 3.9. The value 0 does not mean anything; it is added due to the peculiarity of the OP_CHECKMULTISIG operation, which, when executed, deletes one additional value on the stack.

<img width="60%" alt="Figure 3.9 – The second step of script execution" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.9-The-second-step-of-script-execution.png"/> 

After that, all public key values (Fig. 3.10) and a value of 3 (according to the number of public key values) are pushed onto the stack.

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.10-The-third-step-of-script-execution.png" alt="Figure 3.10 – The third step of script execution"/> 

Then the operation OP_CHECKMULTISIG is performed (Fig. 3.11).

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.11-The-fourth-step-of-script-execution.png" alt="Figure 3.11 – The fourth step of script execution"/> 

The OP_CHECKMULTISIG operation is performed as follows: First, this operation checks if the key located at the top of the stack matches the signature that is located closer to the top of the stack. After that, in any case, the key is deleted (since each of the keys is checked only once, so their location should correspond to the location of the provided signatures). The process is repeated until all signatures are verified. If all signatures are valid, then true is returned; otherwise, false.

Such a scheme is rarely used, because in this case the amount of data in the scriptPubKey field is quite large and, accordingly, the transaction fee also increases. Instead of such a scheme, the P2SH (pay-to-script-hash) scheme is often used, which transmits the entire condition in the form of its hash value. The party that needs to unlock the coins must provide the script itself and the data to execute it.

### Using the locktime mechanism 

Using Bitcoin Script, we can also set a condition under which coins will be spent only after a certain amount of time. To set  such a condition the following script is used:

**\<time> OP_CHECKLOCKTIMEVERIFY OP_DROP OP_DUP OP_HASH160 \<address_В> OP_EQUALVERIFY OP_CHECKSIG**

It is worth noting that the content differs from the P2PKH script by having the following part: \<time> OP_CHECKLOCKTIMEVERIFY OP_DROP. Let's examine how this specific part is executed since the rest of the script is executed identically to the above example.

First, the lock time value is placed at the top of the stack (Fig. 3.12).

<img width="60%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.12-The-first-step-of-script-execution.png" alt="Figure 3.12 – The first step of script execution"/> 

Next, the OP_CHECKLOCKTIMEVERIFY operation compares the \<time> parameter with the lock_time field, which is contained in the transaction header (Fig. 3.13). Coins are spent only if the value of lock_time is greater than the value of the top element of the stack (\<time>).

<img width="30%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.13-The-second-step-of-script-execution.png" alt="Figure 3.13 – The second step of script execution"/> 

After that, the OP_DROP operation removes the top value of the stack (Fig. 3.14), which will contain the time value for unlocking, and which we no longer need if OP_CHECKLOCKTIMEVERIFY returns TRUE and thereby continues to execute the script.

<img width="30%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.14-The-third-step-of-script-execution.png" alt="Figure 3.14 – The third step of script execution"/> 

### Unusual transactions using Bitcoin Script

On December 13, 2012, a transaction was added to the Bitcoin blockchain. One bitcoin was transferred to the person that provided the data the hash value was corresponding to.

The transaction contained the following script:

**OP_HASH256
6fe28c0ab6f1b372c1a6a246ae63f74f931e8365e15a089c68d6190000000000 
OP_EQUAL**

The transaction that became known as the "Transaction puzzle", demanded the data from the recipient that when hashed gives the value that is stored in the terms of coin spending. Schematically, the script can be represented in Figure 3.15.

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.15-Hash-function-preimage-validation-scheme.png" alt="Figure 3.15 – Hash function preimage validation scheme"/> 

The variety of transactions does not end here. There are transactions on the network requiring to satisfy certain conditions that in turn require a certain signature value. There are also transactions, which conditions require to contain multiple branches. The Bitcoin Script is a powerful tool for setting the coin spending conditions. Its structure and organization do not allow malicious parties to set up a scenario that can significantly overload the network. At the same time, users can specify exactly how coins are spent, when using all features correctly.

### Bitcoin Transactions Statistics

If we analyze the transactions on the Bitcoin blockchain for the period of August 2018 to August 2019, the statistics on the use of certain types of Bitcoin Script and the related transaction types looks as follows (Fig. 3.16) [23]:

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.16-Transactions-Outputs-Type-Statistics.png" alt="Figure 3.16 - Transactions Outputs Type Statistics"/> 

It is important to pay attention to P2SH transactions. Many have probably already wondered that over the last year not a single P2SH_MULTISIG transaction has been sent. However, that is not true. The fact remains that until the outputs of the P2SH transaction are spent, you cannot say which script it contained (only the hash value of the script is indicated there) and, in consequence, what type of operation was performed. If we look at the statistics of transaction inputs over the last year, then we can already see the number of each type of P2SH transactions (Fig. 3.17).

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.17-Transaction-Output-Type-Statistics.png" alt="Figure 3.17 - Transaction Output Type Statistics"/> 

However, in this case, the number of transactions sent does not always match the amount of coins that is transmitted. Therefore, if we analyze the transaction statistics in terms of  the amount of coins transferred, we realize that the volume of payments accounted for by P2SH transactions (more than 58%) is larger; although, the total number of transactions sent indicates that P2PKH transactions are only about 50% (Fig. 3.18).

<img width="50%" src="/resources/img/volume-2/3.1-Bitcoin-Script-design-and-features/Figure-3.18-Transferred-Coins-Amount-by-Transaction-Type-Statistics.png" alt="Figure 3.18 - Transferred Coins Amount by Transaction Type Statistics"/> 

**Frequently Asked Questions**

*— What other “Transaction puzzles” can be found in Bitcoin transactions?*

In 2013, a transaction was added to the network to receive funds from it; however, it was necessary to provide two different lines of text that would produce the same hash using the SHA-1 algorithm. In other words, in order to unlock these funds, it was necessary to detect a collision. Nevertheless, in 2017, a conflict was discovered and the funds were transferred to the address of the user who managed to solve this "riddle".

## 3.2 Key formats in Bitcoin

First of all, it worth noting that the global trend in digitizing assets is likely to have users ask for means to manage their digital assets directly, sooner or later. Just as a browser allows you to simply work with websites while performing complex interactions at the program level, a digital wallet is a convenient way to manage digitized assets. However, in this case, the user needs to be able to operate with keys since access to assets is carried out through the keys. Therefore, the question of key formats only seems secondary, and sufficient awareness, in this case, will be required even by an average user.

So, in order to manage the coins, three objects are used in the Bitcoin: a private key, a public key, and a bitcoin address. At this stage, the simplest definitions of each component will do. A private key is a randomly generated number of 256 bits; the public key is calculated from the private key (the length of the public key is 512 bits), and the bitcoin address is the hash value of the public key (160 bits) obtained by using two different hashing algorithms.

Later we consider the private and public key encoding formats that are most often used in Bitcoin. Each of the formats has its own distinctive features, so they should be studied separately.

> **Key encoding formats in Bitcoin**
>> * *Hex (Base16 encoding)*
>> * *WIF (Wallet Import Format)*
>> * *BIP38 (encrypted private keys)*

Since Bitcoin uses cryptography on elliptic curves, the public key is a point on the curve that has two coordinates. Thus, we can describe any public key in the form of just two coordinates: *X* and *Y*. However, this is not the only possible form of presenting a public key.

Let’s consider how the public key looks like in the Cartesian coordinate system (Fig. 3.19). On the elliptic curve graph, there is a point *P* (*X*; *Y*), which is the user's public key. This point has two coordinates: the abscissa - *X*, and the ordinate - *Y*. Accordingly, it is possible to calculate the value of *Y* by argument *X*, substituting the value of the coordinate *X* of the point *P* in the equation of the curve.

<img width="30%" src="/resources/img/volume-2/3.2-Key-formats-in-Bitcoin/Figure-3.19-X-coordinate-point-calculation-scheme.png" alt="Figure 3.19 – X coordinate point calculation scheme"/> 

Since *Y<sup>2</sup>* is present in the equation of the elliptic curve *Y<sup>2</sup> = X<sup>3</sup> + 7</nobr>*, for each argument *X*, there are two values of *Y*: positive and negative. In order to strictly set and then distinguish the desired point *(P)* from symmetric *(-P)*; in addition to argument *X*, you also need to know the sign of the value of the function *Y*. Based on these rules, a compressed public key format is constructed.

### Compressed key format

So, first, a regular public key is formed from the private key, which has the *X* and *Y* coordinates. After that, the coordinate values are concatenated, and a special prefix is added at the beginning of a serialized key, which allows strictly distinguishing this format from others. In the hexadecimal notation, such a prefix has the value "04". As a result, the uncompressed public key is a sequence of 65 bytes.

To get a compressed format for such a public key, the *X* coordinate, the P point, and a special prefix that indicates the sign of the *Y* coordinate (and also that it is not just a compressed public key format) are enough. The prefix “02” is used for a positive value of *Y*, and the prefix “03” is used for a negative value (Fig. 3.20). At the same time, the format of the public key presented is changed, which is now a sequence of 33 bytes in length. Since the address is the result of double hashing the public key, it also changes.

<img width="50%" src="/resources/img/volume-2/3.2-Key-formats-in-Bitcoin/Figure-3.20-Public-Key-Formats.png" alt="Figure 3.20 – Public Key Formats"/> 

Accordingly, there can be two addresses for the same private key. Therefore, the key formats, that will be discussed later, will have two options: compressed and regular. This is necessary so that we can clearly understand which public keys we need to calculate based on this format: compressed or uncompressed.

### Private key formats

There are many private keys presentation formats. Almost every day there are proposals for various improvements or new private key formats since there are many problems for which they can be applied. Nevertheless, in any format, the same data is often used, but only the way they are encoded is different. Below we consider three main formats in their usual and concise versions.

Hex, or Base16, is a format that involves writing a number in hexadecimal notation. It is used mainly in software, for example, during communication between network nodes and mobile wallets. It simplifies the search for errors and debugging applications by increasing data readability.

It is worth noting that a regular hex is just a private key, but there is a hex option for a compressed private key. In this option, in addition to the private key, at the end one byte of data is added, which in the hexadecimal number system has the form “01” (Fig. 3.21). At first glance, there is some discrepancy here, since the compressed private key is longer than a regular private key. However, this is done so that users can determine which public keys should be generated from this private key.

<img width="50%" src="/resources/img/volume-2/3.2-Key-formats-in-Bitcoin/Figure-3.21-Private-key-presentation-in-hex-encoding.png" alt="Figure 3.21 – Private key presentation in hex encoding"/> 

Another important private key format is the wallet import format (WIF). It allows you to conveniently create backup copies of private keys or transfer them from one wallet to another. Therefore, one of the main features of this format is its increased readability for humans. For this, a special Base58Check coding system is used. It involves the existence of a checksum and an additional byte indicating the version (it helps both the user to visually recognize the format and understand what exactly is in front of him and the software to identify what exactly was received as input).

In the case of a normal WIF, the version byte encoded in Base58Check is converted to the character "5". The compressed WIF version is similar to the usual one, but there is one difference: the input is a compressed private key, where there is a combination of characters “01” at the end. Despite the fact that the version byte remains the same as in a regular WIF, the prefix in the encoded value changes from “5” to “K” or “L” (Fig. 3.22) — in fact, the length of the input data changes. Due to this, the discharge increases and the first character changes, despite the fact that the version byte remains the same.

<img width="50%" src="/resources/img/volume-2/3.2-Key-formats-in-Bitcoin/Figure-3.21-Private-key-presentation-in-hex-encoding.png" alt="Figure 3.22 – Private key presentation in WIF"/> 

The final of the private key formats is an encrypted private key. A common and basic problem of all previous formats is the storage of private keys. Accordingly, the very important question of private key storage security remains, since the private key provides full access to the user's coins. Therefore, the developers of BIP38 proposed to introduce a new format for encoding private keys — the so-called encrypted private key [50]. It allows you to backup your private key or transfer it to another wallet or another system, and, importantly, do it in a secure way.

Here, a special encryption algorithm is used, which receives a private key and a passphrase known only to the user at the input. Usually, in this case, the private key is presented in the WIF format, but any other format is also used. Using a passphrase, the private key is encrypted, and the output is encoded using Base58Check. In this case, the version byte is selected so that the output line starts with “6P” (Fig. 3.23).

<img width="50%" src="/resources/img/volume-2/3.2-Key-formats-in-Bitcoin/Figure-3.23-Encrypted-private-key.png" alt="Figure 3.23 – Encrypted private key"/> 

If a user sees any data that is related to Bitcoin and starts with “6P”, most likely it will be an encrypted private key. To use it, you will have to get the passphrase with which it is encrypted.

You can decrypt an encrypted private key in the reverse order. To do this, the user just needs to enter an encrypted data into his wallet (most wallets support decryption of a key in this format), and then you need to enter the passphrase. The wallet decrypts the private key and gives it to the user in an open form, accessible for use.

Above, we considered the formats of private keys. Now let's move on to the public key formats.

### Public key formats

Public keys are used primarily in software. End users usually do not operate with a public key, because if you are a Bitcoin user, depending on your goals, it will be convenient for you to use either a private key or a bitcoin address, while the public key is an intermediate link.

There are two public key formats: hex and hex-compressed (compressed version of hex). Like a private key, hex is a public key record in hexadecimal notation. The only difference is that a slightly different scheme is used here to distinguish a compressed public key from a regular one - a special prefix is added. In the case of a regular public key, the prefix “04” is added, and in the case of a compressed one, one of the prefixes is added: “02” or “03” (Fig. 3.24).

<img width="50%" src="/resources/img/volume-2/3.2-Key-formats-in-Bitcoin/Figure-3.24-Public-key-in-compressed-and-uncompressed-forms.png" alt="Figure 3.24 – Public key in compressed and uncompressed forms"/> 

Why are there two of these prefixes? The presence of the two prefixes for the compressed version of the public key is due to the fact that the Y coordinate can be positive or negative. Accordingly, the prefix "02" indicates a positive Y, and the prefix "03" indicates a negative one.

**Frequently asked questions**

*– What is the private key encryption algorithm used in BIP38?*

The AES algorithm is used in Cipher Block Chaining (CBC) mode, where a 256-bit private key is used. You can read more about this directly in BIP38 [24].

*– In one wallet there can be many bitcoin addresses and many private keys, does this mean that you need to export and import all of them separately?*

There are three options for import/export of private keys: individually, by list, and import/export of the main secret in the case of hierarchical generation of keys. If the keys are not interconnected (each of them was generated separately), then transferring them to another device is possible only individually or as a set. However, if a hierarchical generation is used, then after the transfer of the main secret, all keys can be locally generated from it and restored.

*– What other private key formats do exist?*

In addition to those listed in this section, there are also mini-keys and hierarchical keys. The first format is generated in a certain way in order to get a key with a length of 30 characters. Its main applications are QR codes and physical coins, such as Casascius. The principle of generating hierarchical keys is that a certain seed value is used, from which all used private keys are generated. The advantage of this method is that it suffices to make a backup copy of the main secret from which the rest of the keys can be restored.

## 3.3 Serialization formats of transactions and blocks in Bitcoin

Bitcoin transactions are transmitted among nodes in a serialized form (raw format), namely in the form of a byte sequence of data. This sequence is also used to hash and further obtain a transaction identifier. Therefore, for the correct transaction processing by the network nodes, the serialization format is required.

A user can then see a transaction in a human-readable form (transformed by a blockchain explorer or a bitcoin wallet). In this section, we will teach you how to read serialized transactions and blocks at a glance, to understand in what form they are transmitted over the network and how they are processed by nodes.

### Bitcoin transaction serialization

For a better understanding, we suggest starting with an example and review a serialized transaction, which was confirmed on the Bitcoin network. First, let's examine it in the JSON format (Fig. 3.25) [25].

<img width="40%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.25-JSON-Bitcoin-Transaction.png" alt="Figure 3.25 – JSON Bitcoin Transaction"/> 

In the first part of the book, we already looked at the transaction fields in this form. Now let's look at how the same transaction in serialized form (Fig. 3.26) and where the relevant information is located [26].

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.26-Serialized-transaction.png" alt="Figure 3.26 – Serialized transaction"/> 

The first four bytes determine the version of the Bitcoin protocol according to which the transaction was compiled.

This is followed by a byte, which indicates the number of transaction inputs. Next is the hash value of the previous transaction (where the coins were received from). This value takes 32 bytes (Fig. 3.27).

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.27-Number-of-inputs-and-link-to-the-previous-transaction.png" alt="Figure 3.27 – Number of inputs and link to the previous transaction"/> 

The next 4 bytes are responsible for the exit index of the previous transaction (Fig. 3.28). In this case, “00000000” means that the index of the previous output is zero.

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.28-The-output-index-of-the-previous-transaction.png" alt="Figure 3.28 – The output index of the previous transaction"/> 

The next byte indicates the size of the scriptSig, i.e. proof of coin ownership. In this case, it is equal to "8c", that is, the next 140 bytes will contain a script for proof of coin ownership (Fig. 3.29 - A).

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.29-А-ScriptSig-value-of-the-transaction.png" alt="Figure 3.29 – А – ScriptSig value of the transaction"/> 

Let find out what is contained in these 140 bytes (Fig. 3.29): The bytes can be divided into 4 parts. The first part consists of a single byte “49” and indicates the operation “OP_PUSHDATA (73)”, which pushes 73 bytes of data that will follow this operation onto the stack. This will be the transaction digital signature data. After the signature value, byte “41” follows - the operation “OP_PUSHDATA (65)”, followed by 65 bytes of the public key value.

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.29-B-The-detailed-script.png" alt="Figure 3.29 – B – The detailed script"/> 

After that 4 bytes follow that locate the sequence value (Fig. 3.30). Remember that this value is used to indicate the version of the input that spends the same output (this is relevant for the replace-by-fee mechanism). By default, this value consists of “ffffffff” and decreases with each next version of the input (respectively, with each subsequent version of this transaction).

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.30-Sequence.png" alt="Figure 3.30 – Sequence"/> 

This value is followed by a byte, which indicates the number of transaction outputs. In our case, this is “01”, which means that the transaction has exactly one exit. Then the transfer amount is indicated, under which 8 bytes are allocated (Fig. 3.31). This transaction translates to 0.01 BTC (more precisely, 999938 satoshi).

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.31-Number-of-Outputs-and-Transfer-Amount.png" alt="Figure 3.31 – Number of Outputs and Transfer Amount"/> 

Following that a byte in the body of the transaction indicates the size of scriptPubKey (conditions for spending coins). In our case, it is “19” (Fig. 3.32 - A), which is the size of the conditions, 25 bytes.

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.32-А-ScriptPubKey-Size.png" alt="Figure 3.32 – А – ScriptPubKey Size"/> 

Let’s examine the coin spending conditions more closely. The data can be divided into 6 parts, as shown in Figure 3.32 - B. Byte “76” indicates the operation “OP_DUP”, that is, duplication of the stack upper value (public key of the user). Next, the byte "a9" indicates the operation "OP_HASH", that is, the calculation of the address from the public key. After that, byte “14” is responsible for the operation “OP_PUSHDATA (20)”, which pushes the next 20 bytes (recipient address) onto the stack. Next, the operation "OP_EQUAL" (byte "88") is performed, after which the digital signature is checked - "OP_CHECKSIG". This corresponds to the byte "ac".

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.32-B-Coins-Spending-Conditions.png" alt="Figure 3.32 – B – Coins Spending Conditions"/> 

The final four bytes of the transaction “00000000” are a relative lockTime equal to zero (Fig. 3.33).

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.33-Relative-lockTime.png" alt="Figure 3.33 – Relative lockTime"/> 

### Bitcoin Block Serialization
Now, let's move on to serializing the Bitcoin block. To do this, take the block that contains the transaction, as discussed above. The serialized block is shown in Figure 3.34 [27].

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.34-Serialized-block.png" alt="Figure 3.34 – Serialized block"/> 

Looks complicated, right? However, it is worth mentioning that this block contains 4 transactions, which are also included in the serialized value. These transactions are shown in Figure 3.35.

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.35-Placement-of-Transactions-in-a-transaction-block-in-a-block.png" alt="Figure 3.35 – Placement of Transactions in a transaction block in a block"/> 

Since we have already considered the transaction serialization format, we still have to look at what is hiding behind the next fragment (Fig. 3.36).

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.36-Serialized-Bitcoin-Block-Header.png" alt="Figure 3.36 – Serialized Bitcoin Block Header"/> 

The first 4 bytes are also the value of the protocol version indicating the rules according to which the block was formed. In our case, this is version “1” (Fig. 3.37), which was supported from the moment the genesis block was formed until September 2012 (Bitcoin Core 0.7.0). The latest version of the protocol is version 4, which was introduced in November 2015. The following is the 256-bit hash value of the previous block.

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.37-Hash-value-of-the-previous-block.png" alt="Figure 3.37 – Hash value of the previous block"/> 

After that, the 256-bit Merkle Root value from all transaction identifiers is placed (Fig. 3.38). Note that when forming a block, a coinbase transaction is always used as the first transaction, the rest can be arranged in random order; the only requirement is that the inputs are placed below the outputs they spend.

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.38-Merkle-root-value.png" alt="Figure 3.38 – Merkle root value"/> 

The next value in the block is the formation timestamp in UNIX format (Fig. 3.39), which consists of four bytes.

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.39-Block-Formation-Unix-Timestamp.png" alt="Figure 3.39 – Block Formation Unix Timestamp"/> 

Two values follow: the first of them is the difficulty parameter, which determines the complexity of solving the current proof-of-work task; the second is the nonce, which is considered as proof of solution to this problem. The last value determines the number of transactions in the block (Fig. 3.40).

<img width="50%" src="/resources/img/volume-2/3.3-Serialization-formats-of-transactions-and-blocks-in-Bitcoin/Figure-3.40-The-difficulty-parameter-nonce-value-and-the-number-of-transactions-in-the-block.png" alt="Figure 3.40 – The difficulty parameter, nonce value, and the number of transactions in the block"/> 

## 3.4 Messaging between Bitcoin network nodes

The architecture of the Bitcoin network provides a peer-to-peer network, where each node is equal and self-sufficient. There are no additional restrictions on the interaction between Bitcoin network nodes, as well as on other processes of this system. The consensus is reached by independent participants, and the exchange of messages between them is independent and decentralized. Each network participant independently decides which block to use as a base for further history while the network nodes independently decide which nodes to communicate with and how to process the received information.

In the first part of the book, we have already examined important points of the Bitcoin network structure and the role of nodes in its functioning. In this section, we will consider the technical details of how nodes interact with each other, specifically how the exchange of messages between participants in the Bitcoin network occurs. We will also characterize the state of the Bitcoin network based on data from September 2019 as well as consider the information dissemination protocols, their requirements, problematic issues of these protocols and possible solutions.

### The roles of nodes in Bitcoin

Earlier, we have determined that nodes can be divided into three groups:

> * *Full node (auditor)*
> * *Validator node*
> * *SPV-node*

Now we try to expand the classification a little bit by considering the functions that can be implemented by network nodes. Each Bitcoin node can consist of a number of modules that differ in terms of the functions performed.

> * *A module that stores the entire transaction history*
> * *Transaction verification module*
> * *The module responsible for communication with other network nodes*
> * *A module forming proof-of-work for mining*
> * *The module that implements a user wallet*

An example of a node containing all the modules is an individual user's PC with Bitcoin Core implementation or a similar one. Such a node can be represented schematically in Figure 3.41

<img width="40%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.41-Bitcoin-Full-Node.png" alt="Figure 3.41 – Bitcoin Full Node"/> 

Each of these nodes stores the entire transaction history, so they can autonomously verify subsequent blocks in the chain, without having to trust a third party as a data source. These nodes participate in consensus-building, meaning they participate in decision-making on the confirmation of transactions along with all other nodes. The presence of a network interaction module allows nodes to communicate directly with other participants in the system, and the wallet module uses a local copy of the database to process the required transactions.

The second type is a node that supports the Bitcoin network and participates in decision-making, hence basically being a validator node. However, these nodes may not have a wallet module (Fig. 3.42).

<img width="40%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.42-Bitcoin-Validator-Node.png" alt="Figure 3.42 – Bitcoin Validator Node"/> 

The third type is an auditor node. This type contains two mandatory modules: a module for storing the entire transaction history and a network module that allows the exchange of messages with other nodes for obtaining the current status of the blockchain. This type has found its application in exchanges and observers of accounting systems. Such a kind of node supports the Bitcoin network by storing a local version of the blockchain. However, if a node does not have a module that allows creating blocks, this node does not participate in reaching consensus (Fig. 3.43).

<img width="40%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.43-Auditor-Node.png" alt="Figure 3.43 – Auditor Node"/> 

The next type of node also has two modules (a wallet and a network module): SPV node. We considered the operational principles of such a node earlier (in subsection 2.3 of the first part of the book). This type of node does not have its own transaction history, but stores only the block headers (received from full nodes or other SPV nodes). At the same time, it personally addresses full nodes and, based on the obtained values ​​of the Merkle Branch, determines whether a particular transaction was actually added to the blockchain or not. Schematically, the location of an SPV node in the Bitcoin network is depicted in Figure 3.44.

<img width="40%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.44-Bitcoin-SPV-node.png" alt="Figure 3.44 – Bitcoin SPV-node"/> 

There are two more network elements, which are actually not nodes since they do not support the network and do not directly participate in reaching consensus. The first type is a wallet that uses a trusted node. As the name suggests, this network element only has a wallet module. It communicates with specific full nodes (often with only one) and receives all the data about transactions related to user addresses from them (Fig. 3.45). However, it does not store the history of all transactions and does not perform verification.

<img width="50%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.45-Digital-Wallet-Connected-to-the-Network.png" alt="Figure 3.45 – Digital Wallet Connected to the Network"/> 

The second type contains only a module that calculates the solution for a resource-intensive task, which means it does the mining. Such nodes receive the task from the full node (mining pool leader) and are engaged in the formation of proof-of-work, which is sent to the full node in case of success. Such nodes are participants in the mining pool (its customers), and the owner of the validator node is its operator (Fig. 3.46).

<img width="35%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.46-Mining-pool-structure.png" alt="Figure 3.46 – Mining pool structure"/> 

> **_NOTE:_** *According to estimations from April 2019, more than 30 million users use Bitcoin. The number of full nodes, at the same time, is about 9.5 thousand, which is more than three thousand users per one full node (by the way, the number of Internet providers in the world is also close to 10 thousand, with more than 3.9 billion active users).* 

In the last two types of nodes, there was no module responsible for exchanging messages on the Bitcoin network. Next, we will look at the functioning of this particular module; hence we will also cover concepts such as a full and SPV node.

### The current state of the Bitcoin network

Conventionally, there are two types of nodes in the Bitcoin network: public and private. The public node (public-IP node) does not require permission to connect to it. The software of such a node “listens” to a port on the global Internet network and accepts connection requests. Public nodes help new members to find nodes to join. If there is a sufficiently small number of public sites, then it will be difficult for new participants to find who to join and as a result, the network will not be able to grow or might even break up into subnets.

The private node (private-IP node) joins other public nodes but does not accept incoming connections. A private node is sufficient to audit the accounting system, send your transactions and generally support the full functionality of a digital wallet (Fig. 3.47).

<img width="40%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.47-Bitcoin-network-structure.png" alt="Figure 3.47 – Bitcoin network structure"/> 

In practice, there are more types of nodes due to the features of the subnet organization. For example, there is a subnet of interconnected universities to which only they have access. These universities can run public nodes, but only for their private subnet, and join the global network only through private nodes.

> **_NOTE:_** *Further, we will use only the concepts of public and private nodes as these are the most common types.*

By default, in the Bitcoin network node software, there are restrictions on the number of network connections with other nodes:

* each node seeks to create 8 outbound connections, meaning that after launch, it finds 8 and connects to them;
* public nodes allow up to 117 inbound connections.

Satoshi Nakamoto chose values ​​of 8 and 117 as a tradeoff between reliable network connectivity and optimal network load. High connectivity makes the network more secure. For example, if nodes joined to one node instead of eight, then there would be a high probability of situations where attackers would create nodes that transmit false data about the state of the system and conduct various kinds of attacks. On the other hand, if the nodes would connect to 1000 other nodes instead of 8, then everyone would have to store the state of these connections. And this, in turn, entails high memory costs for storing data about the node activity, the stages of message transfer, etc. Moreover, if the node had 1000 network connections, then in addition to storing data on the states of these connections, it also needed to synchronize new transactions and blocks with each of them as well as respond to incoming requests, which effectively would create extremely high traffic load.

Today, the total number of active nodes in the Bitcoin network varies from 60 to 100 thousand nodes, while the latest version of Bitcoin 0.17 is supported only by one-third of the nodes. The previous version of Bitcoin 0.16 is launched on the other third of the nodes, and the remaining are even older versions and alternative protocol implementations (Fig. 3.48) [28].

<img width="50%" alt="Figure 3.48 – Using protocol versions (September 2019)" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.48-Using-protocol-version-(September-2019).png"/> 

If we analyze the number of public nodes, we can see that from 60-100 thousand nodes in the network today, only 10 thousand are public. Therefore, the ratio of public nodes to the rest is approximately 1/8. This ratio is due to the two values ​​8 and 117 mentioned above as well as because these 10 thousand public nodes need to serve about 80 thousand private nodes. Therefore, the number 117 was chosen, which allows you to maintain the integrity of the network in such conditions. The map in Figure 3.49 shows that public sites are available in different countries around the world [29].

<img width="50%" alt="Figure 3.49 – Concentration map of available Bitcoin Nodes around the world" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.49-Concentration-map-of-available-Bitcoin-Nodes-around-the-world.png"/> 

### Bitcoin Message Structure

Let look at the structure of the messages exchanged between Bitcoin network members. All messages in Bitcoin have the structure as presented in Table 3.1.

Table 3.1.
<img width="50%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Table-3.1.png" alt="Table 3.1."/> 

One of the most important messages without which the nodes cannot start full interaction is the version message. Nodes exchange a version message or handshake when they connect to each other for the first time. The message has the structure shown in Table 3.2.

Table 3.2.
<img width="50%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Table-3.2..png" alt="Table 3.2."/> 

### Information Distribution Protocol

Information distribution is the main task of the Bitcoin network and system messages are needed to implement it. Thus, there are 27 types of messages in the Bitcoin network. The main of them are shown in Figure 3.50 [31].

<img width="20%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.50-The-main-Types-of-Messages-on-the-Bitcoin-Network.png" alt="Figure 3.50 – The main Types of Messages on the Bitcoin Network"/> 

In this list, you can see that the biggest amount of data is used by messages related to the transaction transfer (inv, tx, getdata). For example, messages that check the network connection (ping, pong) take only a small part of all traffic.

### Launching a node in the Bitcoin network

A node that is launched for the first time and starts interacting with other network nodes has to go through the following steps:

> * *Sending the request to a DNS server (supported by community members) to get network addresses of active nodes in the network*
> * *Connecting to the nodes*
> * *Downloading the transaction history (initial blocks downloading)*
> * *Exchanging blocks and transactions with other nodes*

During the first step, the node needs to send a request to one of the DNS servers that are supported by the Bitcoin core developers and community members. It also needs to store a database of existing active nodes (the network addresses of these servers are embedded in the node software).

After sending the request to the DNS server, the node gets IP addresses of the nodes to which it can connect. Then it connects to these nodes and the protocol of reconciliation of versions occurs.

Then the new node needs to request and download all the blocks from the existing blockchain. To do this, the Initial Block Download method is used. After the existing blocks are synchronized, the new node begins to take part in the protocol for transmitting data over the network – to exchange new blocks and transactions with other nodes.

### Flooding Information Distribution Protocol

To transfer blocks and transactions over the Bitcoin network, a modification of the Flooding protocol is used [32]. According to this protocol, nodes transmit new / received blocks and transactions to all their nodes, except for those that already know about these blocks and transactions (Fig. 3.51).

<img width="30%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.51-General-Flooding-Protocol.png" alt="Figure 3.51 – General Flooding Protocol"/> 

To save data, a protocol consisting of three messages is used. Its essence is that as soon as a node receives a new transaction or block, it sends a hash of this object INV(hash(tx)) to the connected nodes. Each node addresses its local database to check whether it has this hash; if it does not find it, it asks the node to provide GETDATA(hash(tx)) for the necessary block or transaction corresponding to the received hash. Having received this request, the node sends a complete block or a complete transaction in response. Thanks to this modification with three messages, it is possible to avoid duplication of an existing transaction or block, and since the size of each transaction is usually 150–500 bytes, significant amounts of network data are saved. Let’s look at an example of how a transaction in the Bitcoin network is spread (Figure 3.52).

<img width="40%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.52-Network-Transaction-Distribution-Example.png" alt="Figure 3.52 – Network Transaction Distribution Example"/> 

Let’s say node A received transaction tx from a mobile client and is going to share it with nodes B, C, and D. The nodes do not have this transaction, so they will receive it at time t1. At time t2, node B will try to send this transaction to node C, but since node C has already received this transaction, it will not request the full version of this transaction, and therefore the transaction will not be transmitted over this connection. Nodes E and G don’t know about this transaction, and in response to a message about a new transaction, they will request it and receive it at time t2. This process continues until all nodes in the network have the transaction.

### Diffusion – Flooding Extension

In fact, Bitcoin uses the Diffusion protocol [32] to transfer transactions, which is an extension of the Flooding protocol. According to this protocol, after receiving a new transaction, each node in the network will not send it immediately but will rather wait for a random period of time between 0-5 seconds. Thus, the node accumulates transactions before distributing them further. This helps to save traffic associated with message headers, complicate spying associated with tracking the time at which a transaction was received, and avoid collisions (when two nodes simultaneously send one transaction to each other).

There are several difficulties associated with data transfer protocols in the Bitcoin network.

> * *Respecting user anonymity*
> * *Resistance to various kinds of attacks*
> * *Hardware power and capacity requirements*
> * *Network distribution time*

Further, we will consider how each of the listed problems is solved.

### Examples of problems in the protocols and their solutions

Consider the problem of anonymity. It can be formulated in the following way: how to make it impossible (or very expensive) to determine the origin of transactions in the network, which means making it impossible to link a specific transaction in the Bitcoin network to a network address of the node from which it began to spread across the network?

In the diagram below (Fig. 3.53), you can see that the supernode is a node connected to all nodes in the network. From the point in time at which this node first learns about a transaction from other nodes, it can determine its source. Thus, supernode can associate this transaction with a specific IP address.

<img width="30%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.53-Transaction-Source-Disclosure-Example.png" alt="Figure 3.53 – Transaction Source Disclosure Example"/> 

To prevent such attacks, the Dandelion protocol [32] and later the Dandelion ++ [33] were proposed. Let’s look at how this protocol works.

This protocol distributes transactions over the network in two phases:

> *Anonymous phase* (stem phase) – each node sends a transaction to only one of the nodes associated with it.

> *Distribution phase* (fluff phase) – each node transmits a transaction to the network passing it to all connected nodes.

Each node spreads the transaction in an anonymous phase with a probability of 0.9, and in the propagation phase with a probability of 0.1 (Fig. 3.54). As a result, it turns out that the transaction follows one path until the moment it begins to spread. Thus, there is a 10% probability that a transaction will be distributed from the zero node, 10% from the first, 10% from the second, etc. Therefore, it becomes very difficult to determine the real source of the transaction. On average, after 10 such jumps from one node to another, a transaction is distributed using the usual Flooding protocol.

<img width="50%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.54-Dundelion++Protocol.png" alt="Figure 3.54 – Dundelion++ Protocol"/> 

Now let’s consider the security problems in the Bitcoin network. One of the main tasks of ensuring security is to increase the cost of an attack to the point where it becomes economically disadvantageous. As an example, let’s consider the Eclipse attack.

The scenario of this attack implies that the nodes controlled by an attacker isolate honest nodes from all other parts of the Bitcoin network and transmit a false state of the database to them.

Since honest nodes are attached only to the attacker’s nodes, they have no chance of finding out which blocks are a part of this chain if an attacker shows them only the blocks made by himself (Fig. 3.55). Thus, it is possible to perform double-spending and some other attacks.

<img width="30%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.55-Eclipse-attack.png" alt="Figure 3.55 – Eclipse attack"/> 

As a counteraction to this attack, the authors themselves proposed several methods for modifying the protocol, for example, diversifying IP connections (connecting to different regions). Thus, if each node has 8 outgoing connections, then the attacker would need to control a very large number of nodes from different parts of the world, making the attack much more expensive to execute.

Now let’s consider the problem of resource consumption. How to reduce hardware and traffic requirements for a full Bitcoin node? One of the basic principles of Bitcoin is its availability for both high and low computing power machines. Therefore, it is very important to develop Bitcoin in such a way that it remains operational, does not overload the global network and does not require expensive equipment to maintain nodes. An example of a problem in this context is situations in which nodes that are closer (across the network) to validator nodes receive new blocks before the rest. As soon as the blocks are received and verified, the nodes should immediately send this block to all their neighboring nodes. Since most of the neighbor nodes have not heard about this block, they will send a request to get it, and thus a jump in data consumption will happen.

This jump leads to difficulties with the Internet connection since at one point in time you need to use a very large part of the bandwidth. As a solution to this problem, BIP 152: Compact Block Relay (Matt Corallo) was formed [34]. Let's look at how this works.

Instead of one block transfer protocol on the network, there will be three of them and they will be used depending on the context.

First let’s look at the Legacy Relaying (Figure 3.56 – A), i.e., the old way to distribute blocks. Node A receives a new block, validates it and sends headers of inv to node B. Since node B does not have this block, it will request its data (getdata) and node A will respond with the contents of a full block.

Then two new types of triggering of this protocol appeared, namely High Bandwidth Relaying (with high traffic consumption) and Low Bandwidth Relaying (with low traffic consumption).

<img width="45%" src="/resources/img/volume-2/3.4-Messaging-between-Bitcoin-network-nodes/Figure-3.56-Compact-Block-Relay.png" alt="Figure 3.56 – Compact Block Relay"/> 

Now let’s look at the new types of protocol: High Bandwidth Relaying and Low Bandwidth Relaying. In each of these protocol types, it is assumed that the first step for node B is to tell A what type of protocol they will work on. In the case of high traffic consumption, node B reports to node A (sendcmpct(1)), in the case of low traffic, node B reports to (sendcmpct(0)).

Let’s consider High Bandwidth Relaying (Fig. 3.56 – B). Node B tells node A that they will operate according to the protocol with high traffic consumption (sendcmpct(1)). Working according to this protocol type, node A does not perform full block verification after receiving it; rather it only checks the header. Next, node A announces this block to node B and, based on previous data transfers, tries to predict which transactions are missing in node B (cmpctblock). Then, after verifying the block with what it has, node B requests the needed transactions (getblocktxn), and after the block has been completely verified, node A sends the remaining transactions to node B.

Now moving to Low Bandwidth Relaying (Fig. 3.56 – C). Node B tells node A that they will operate as a protocol with low traffic consumption (sendcmpct(0)). In the same case, node A announces a block to node B only after full verification of this block.

Now let’s consider the problems of rapid distribution of data over the network. How to distribute transactions and blocks over the network as quickly as possible? The question is this case speed plays an extremely important role. For example, the slower the blocks are distributed, the higher the Orphan rate becomes (the number of blocks that had to be dropped from the mainchain because they were not created based on the last known block). As a result of such delays, the volume of useless work (proof-of-work) increases leading to a reduced security level of the accounting system.

As a solution to this problem, Bitcoin FIBER [35], an additional network, was proposed. Only validator nodes are present and exchange blocks in this network. This ensures low Orphan rate.

**Frequently asked questions**

*– What are the benefits of launching a full Bitcoin node?*

In some cases, using a trusted node or SPV node is not enough, so a full node should be launched. For example, if you want to provide the utmost protection of your funds from attacks, then you need a full node (a private node that does not accept incoming connections is enough). This is necessary in order to independently check the entire history of transactions and PoW, monitor alternative branches of the chain of blocks and temporarily switch to the latest current state of the network. You cannot be confident as to the reliability of these processes if you use a third-party source. If you want to help the network (make it more connected and reliable), you can start a public site. Then other nodes will be able to join you (which will increase the total number of connections of your node), this will help the network grow, and all the described advantages of a private node will also be available.

*– Why it is impossible to structure a Bitcoin network (e.g., star topology)?*

A star topology assumes a central computer to which everyone else is joined. Since Bitcoin is aimed at decentralizing the accounting system and minimizing the necessary level of trust, its topology must be distributed (close to a random graph). In a star topology, the central node will be potentially vulnerable, which, if damaged or attacked, threatens the operability of the entire system.

*– Why is it bad to disclose the network topology?*

Among developers of the Bitcoin protocol, there is an opinion that the well-known network topology makes it more vulnerable to a number of network attacks. For example, if someone finds out that there is a “bottleneck” in the network (situation when the network is divided into two subsets of nodes that are connected by only one connection), then having gained control over two target nodes, an attacker can disrupt the process of reaching consensus and implement double-spending or similar attacks.

*– Why can't only blocks be distributed (without a separate transaction transfer)?*

A few years ago, an optional blocks-only mode of operation was proposed in Bitcoin. This mode allows nodes to ignore the new transactions, and it presumes that nodes listen only to the blocks which are already created by validators. This option is relevant for nodes that intend to save their traffic. If all nodes in the network would use this option, then there would be surges in the traffic consumption of public nodes that entirely transmit new block to everyone else. Here, the biggest problem, however, is that new transactions will not be able to reach validator nodes in order to receive confirmation. So in order to transfer the transaction, you would have to connect the digital wallet directly to the validator node.

*– Is the traffic generated by the nodes encrypted?*

No, it is not encrypted. This issue has been the subject of debate over the past few years. The traffic encryption protocol was proposed that would, for example, help you to hide the fact of using Bitcoin from providers. Right now your internet service provider or administrator of your router can see that you use Bitcoin and, possibly, even determine which of the transactions on the network are yours and which are not. It is believed that this reveals part of the identity of the owners of the nodes. Therefore, at the moment, the development of new alternative encryption protocols for channels that will work in this specific environment is actively underway.

*– What type of update is needed to change the messaging protocol?*

Neither hardfork nor softfork is needed to change these protocols, but it is necessary to develop the protocols and corresponding software updates of the node in such a way that they are backward compatible and that non-updated nodes can join the updated and work normally. An example of such a protocol is Compact Blocks Relay [34] discussed above. It provides messaging with old nodes as well as with the new ones.

## 3.5 Testnet and challenges of protocol updating

The testnet is an alternative network that runs the same protocol as Bitcoin but is supported by fewer participants and acts as a kind of "sandbox", which anyone can join to conduct tests. The testnet has its own genesis block, whereas coins are separated from those in the mainnet and have no real value.

This allows application developers or testers to experiment without having to conduct operations with real coins and clog the mainchain with transactions. The testnet is where the team of Bitcoin protocol developers tests updates and new software as well as conducts network loading tests prior to introducing updates in the mainnet. The testnet is divided into two types: private testnet and public testnet.

### Testnet in Bitcoin and its purpose

Bitcoin’s public testnet works just like the mainnet;  the only difference is that people don’t presume the value of coins in this system. At the same time, users can both mine test coins on their own — for the testing purpose — and contact participants who distribute small fractions of coins to those who wish to test the software of nodes, wallets, etc.

In addition to the public test network — where everyone can operate — you can create a private testnet (regtest, Regression Test Mode), a local (private) version of Bitcoin. This version of the test network is convenient; you can quickly update the software of all network nodes and demonstrate or test the changes without the need for approval from other participants. This method has three main advantages:

> * *No need to connect to a public network*
> * *Full control over the network and its transactions*
> * *Most network parameters are saved, which allows you to independently change the tested parameters or introduce new functions*

### Protocol update

While updating the protocol of a decentralized system, developers have to deal with certain difficulties. A particularly difficult situation is when an update must be carried out in a permissioned system and its operation cannot be allowed to stop even for a short period of time. Traditional protocols allow different versions on the network, but, for example, in Bitcoin and similar accounting systems, nodes must process blocks of the same format. Consequently, a certain group of participants cannot freely switch to a new protocol version without considering versions of the other network nodes' software (in such cases, forks typically appear).

To implement any protocol change, the following questions must be taken into account: how will the nodes come to an agreement regarding the new rules? Will there be vulnerabilities or bugs as a result of the update? How safe will the updated protocol be?

The reasons for updating may be different, for example, adding new functionality, eliminating known vulnerabilities, or changing a key rule in the protocol. Figure 3.57 shows a generalized scheme for updating the protocol of a decentralized system.

<img width="60%" src="/resources/img/volume-2/3.5-Testnet-and-challenges-of-protocol-updating/Figure-3.57-Protocol-update-steps-diagram.png" alt="Figure 3.57 – Protocol update steps diagram"/> 

The update process begins with the announcement of a new version of the protocol to network participants. Thereafter, some of the nodes begin to generate blocks of a new format, while the rest can remain on the previous version. In addition, each node makes the decision to upgrade independently.

After a certain time, more and more validator nodes begin to create blocks with a mark that they are ready to switch to new rules. Then the nodes track when a critical mass of such blocks is accumulated and, as a general rule, calculate the height of the block or timestamp in the future when they switch to the new rules. This should happen simultaneously for all nodes, then the update is considered as active. In general, there are two types of protocol updates - a softfork, when backward compatibility is maintained, which means blocks of older versions are accepted if they do not contradict the new rules, and a hardfork, which is a more “hard” update that does not provide backward compatibility and, thus, all nodes must switch to the one new version; otherwise, their blocks will be rejected.

In order to better understand the process of updating the protocol, consider how this happens in Bitcoin. Let's define the key characteristics of each update first and then examine the parameters that each update has in table 3.3.

Table 3.3
<img width="50%" src="/resources/img/volume-2/3.5-Testnet-and-challenges-of-protocol-updating/Table-3.3.png" alt="Table 3.3"/> 

The update can be in one of 5 statuses (table. 3.4).

Table 3.4
<img width="50%" src="/resources/img/volume-2/3.5-Testnet-and-challenges-of-protocol-updating/Table-3.4.png" alt="Table 3.4"/> 

We illustrate the process in more detail using the scheme shown in Fig. 3.58. Initially, the update is assigned the defined status, the values of starttime and timeout are determined. At the end of starttime, if the elapsed time does not exceed timeout, the update switches to the started status. If there is a majority of validators publishing blocks that indicate the new version to the network before the end of the timeout, the update will go into the locked-in status, after which it will automatically switch to active. Otherwise, it will be considered failed and will go into the failed status.

<img width="40%" src="/resources/img/volume-2/3.5-Testnet-and-challenges-of-protocol-updating/Figure-3.58-The-life-cycle-of-updates.png" alt="Figure 3.58 – The life cycle of updates"/> 

### Most Important Bitcoin Protocol Updates

Since the advent of the very first version, Bitcoin has gone through many changes. Let's look at the most significant of them.

The first version of the protocol, Version 0.1, was released on January 8, 2009. This event was preceded by the publication of the whitepaper on November 1, 2008, in which Satoshi Nakamoto described the principles of the future system.

*The block size limit* is 1 MB. The line of code in which the size restriction appeared was added on July 15, 2010 (version 0.3.1 rc1). However, it is not known when the decision was made to move forward with this and who participated in the discussion. The main motivation for the restriction was to reduce the effectiveness of DDoS attacks. However, such a strict restriction led to incompatibility with previous versions, which is why many owners of validator nodes reacted negatively to it.

*Pay-to-Script Hash*. A Pay-to-Script Hash (BIP 16) mechanism that allows you to send transactions to a secure address was added. The first attempt to implement it failed, the second was undertaken on April 1, 2012, in version 0.6.0rc2. Over the next few months, 45% of validators that were not updated, continued to produce incorrect blocks, which caused activation problems.

*Segregated Witness*. The Segregated Witness (SegWit) mechanism, described in BIP 141, 143, and 147 [68–70], was designed to optimize transactions and blocks by issuing signatures in a separate structure. SegWit was first published at the end of 2015, and the release took place in October 2016 (version 0.13.1). The activation threshold has been set to 95%. However, owners of large mining pools, including F2Pool, HaoBTC, AntPool, said they would support the update only if the block size increased to 2 MB.

### Adaptation of new versions of Bitcoin among nodes

Despite the fact that not all Bitcoin protocol updates were successful, they play an extremely important role, introducing new functionality and correcting old shortcomings. Nevertheless, statistics show that the owners of some nodes react very slowly to the introduction of new versions whilst they continue to use the old ones.

Figure 3.59 shows a graph that depicts the distribution history of versions from August 2017 to August 2019. The graph highlights how, after updating, the number of nodes supporting the new version begins to grow gradually [36].

However, it can be noted that even after the release of version 0.18.0, the nodes of version 0.17.0 were more likely to be updated, while the number of nodes running on older versions of the protocol continued to decline gradually, as they did before.

The main reason why users do not switch to a new version of the protocol immediately after the release is due to concerns about its security and vulnerabilities. Owners of nodes decide to wait for a certain time to make sure that there are no critical vulnerabilities in the new version.

<img width="50%" src="/resources/img/volume-2/3.5-Testnet-and-challenges-of-protocol-updating/Figure-3.59-Bitcoin-Version-Distribution-Timeline.png" alt="Figure 3.59 – Bitcoin Version Distribution Timeline"/> 

Nevertheless, some still use extremely outdated versions of the protocol, up to 0.8.1 (Fig. 3.60) [28].

<img width="50%" src="/resources/img/volume-2/3.5-Testnet-and-challenges-of-protocol-updating/Figure-3.60-Bitcoin-node-version-ratio.png" alt="Figure 3.60 – Bitcoin node version ratio"/> 

### Risk of the community split

Any change in the distributed accounting system is made only when the open community of the most active users reaches agreement on this change. Projects of such a kind are difficult to monetize directly, so it is very important to have a big community of volunteers who will support the operation of the accounting system.

When disputes appear in a community, they are usually resolved by the opinion of the majority. But it may happen that community members do not reach an agreement. The contradictions between parties grow and may result in a community split (Fig. 3.61). Examples are the splits of the following projects.

> * *Ripple and Stellar*
> * *Ethereum and Ethereum Classic*
> * *Bitcoin and Bcash*
> * *Monero and Monero Classic*

<img width="45%" src="/resources/img/volume-2/3.5-Testnet-and-challenges-of-protocol-updating/Figure-3.61-Accounting-system-community-split.png" alt="Figure 3.61 – Accounting system community split"/> 

It turns out that one of the biggest advantages can sometimes turn into a crucial disadvantage. Instead of working together on the issue and looking for ways to optimally update the protocol, the community can get stuck in disputes and split into small groups that start to move towards their goals.

**Frequently asked questions**

*— Can attackers use the testnet to mislead a user and sell test coins to them? How can this be avoided?*

Often, attackers take advantage of the user's carelessness or their desire to obtain more favorable conditions for the purchase of coins by transferring them to a test wallet. In order not to fall for such a trick, you should pay attention to which wallet you are using to receive funds and under no condition allow an outsider to create a wallet for you. Attackers can also create a multisig wallet and then send funds to another address that you do not own.

## 3.6 Basic classes of attacks on Bitcoin

The emergence of Bitcoin caused real concern among computer scientists. Many of them were initially skeptical about it. On the one hand, it seemed that this was another attempt to involve people in a dubious financial enterprise; on the other hand, people would notice that Bitcoin combines well-known standards of cryptographic schemes – hashing and signature algorithms – without introducing anything new. For cryptographers, this was too easy, and for Internet security researchers, it was too abstract. However, after 2014 the situation changed dramatically. 

People realized that Bitcoin occupied a certain niche within the Internet payment sphere and is constantly growing its user base. At some point, Bitcoin became a testing ground for attacks and methods of protecting decentralized systems (most importantly for anonymous attack testing). This chapter will examine the main classes of attacks on Bitcoin and deep dive into why Bitcoin was designed in that exact setup that we see today.

For several decades, engineers from all over the world have been looking for a way to reach consensus on correct transactions in the context of full decentralization and anonymity of participants. This was one of the most difficult tasks ever since, and eventually, it was the creator of Bitcoin who proposed an effective solution first. In fact, Satoshi Nakamoto was nominated for the Nobel Prize for his achievement [71].

The essence of the solution is to allow anyone to participate in the formation of a list of confirmed transactions. The easiest way to achieve this is to have participants exchange confirmations on transactions that they consider to be correct. However, everyone must also reliably protect oneself from fake messages and overloading honest network nodes with fake data of malicious nodes. The details of how such attacks are conducted and how to protect against them will be described further.

### Flood attack and protection mechanism

Satoshi Nakamoto proposed a very original solution: each potential validator must provide proof of work done to obtain the right to vote. In fact, this is the essence of the proof-of-work consensus algorithm. The participant must spend some resources and offer her own set of correct transactions, which serves as a means of protection against false opinions.

The use of proof-of-work to protect against unwanted mailings was proposed by Adam Back back in 1997; then it was a solution to protect users of email protocols from spam. Later, this approach formed the basis of the HashCash architecture — one of Adam's early attempts to create an independent digital currency. At the time of 2019, Adam Back remains one of the leading cryptographers and ideologists in the Bitcoin community.

### Spam attack and its consequences

The term spam usually refers to intrusive spam messages in large numbers. The goal of a spam attack is to remove one or more network nodes from the state of normal functioning. When organizing such an attack, some prerequisites have to be met. You can take a software wallet and make changes to it in a way that it creates a lot of transactions, sending coins from its old addresses to the new ones. Each transaction should include a fee, ultimately leading to three different scenarios.

In the first case, the attacker includes a very low or zero commission. Then its transactions will not disperse over the network at all since each node sets the value of the minimum required fee. If the actual transaction fee is below this threshold, then the node immediately rejects the transaction. Thus, an attacker can send such transactions only to those nodes with which he has a direct connection, and then not for a sustained amount of time. In addition, it will be ineffective because its transactions will be immediately rejected. And after some time, these nodes will also trigger a filter that will break the connection with useless nodes or nodes transmitting incorrect messages.

In the second case, an attacker includes an average fee (above the threshold value but not sufficient for quick confirmation). His transactions spread throughout the network, and this attack will be slightly more effective than the previous one. All nodes in the network will verify, store and broadcast the transaction of an attacker. This way you can consume a significant amount of memory at the bottom of the unconfirmed transaction queue. But the nodes have a limit on the maximum size of the queue, and after a while, such transactions will be deleted if they are not confirmed. In this case, the transactions of other users with a sufficient fee will be confirmed properly.

In the third case, an attacker includes a sufficient commission. Then his transactions can practically fill the entire bandwidth of the currency. It is important to understand that other users' wallets will also begin to inflate the transaction fee in order to receive service. Therefore, an attacker will need to pay more and more for each subsequent transaction so that the remaining transactions remain at the bottom of the queue. The costs of such a flood attack are very high.

### Suspension of new transactions’ confirmation (DoS)

Is it possible to create an attack on Bitcoin that will stop validators and new transactions will no longer be confirmed? Theoretically, such an attack is possible. It may turn out that someone who is well versed in programming and software security will find critical vulnerabilities in the source code of full nodes. He might want to exploit them and harm the network instead of offering a fix. However, it is almost impossible to predict such a scenario or to guarantee protection against it.

Nevertheless, the likelihood that this will actually happen is very small. Since Bitcoin has been around for quite some time, attempts to find vulnerabilities have been made many times and researchers continue to work in this direction. In addition, before the adoption of the protocol updates, the source code undergoes a rigorous audit and testing by many independent community members.

### Long-range attack

The underlying principle of this attack is easy to understand when put in perspective of the well-known 51% attack. 

In a traditional way, this would be as follows. An attacker sends 50 bitcoins to an address just created. After that, he sends the coins to a merchant with whom he exchanges them for another currency (for example, litecoins). The seller waits for 6 transaction confirmations and sends another currency to the user's wallet.

In parallel, an attacker works on a new chain based on the block, which is located in front of the block with the transaction in which he sends 50 bitcoins to the merchant. The user forms a new transaction in which he sends coins to himself to another address. With more than half the processing power of the network, the user redirects it to a new chain. Soon, the user’s chain could become the main chain and all nodes would switch to it — according to the rules of the Bitcoin protocol. As a result of this successful attack, the user would save his bitcoins and acquire litecoins. In the case of a long-range attack, the user would not start his chain 6 blocks back but rather start from the genesis block.

Having a computer with a processing power of 1 TH/s, it is possible in just 5 minutes to build a new chain from the genesis block to a block with the serial number 300,000. By the way, as of September 2019, mining equipment for Bitcoin with a processing power of 14 TH/s could be purchased at a price of 3000 USD. The minimum value of the mining difficulty parameter is equal to the number, which is presented below in the base 16 notation.

**00000000ffffffffffffffffffffffffffffffffffffffffffffffffffffffff**

When a user builds her chain, she can record any time spent writing a block, while the mining complexity remains minimal. If you look at such a chain of blocks in a finished form, then it will not differ in any way from the real Bitcoin blockchain.

When the new nodes join the network, can they distinguish a true block history from a fake one? Even with medium power equipment, it does not take a long time to create such a chain. So, would  Bitcoin be able to resist such an  attack in the thousands from such chains?

In reality this is not a vulnerability for the Bitcoin protocol since the developers took into account the possibility of such an attack from the beginning. Since the rules were written in a way that they do not only take into account the length of the chain but also the complexity parameter (which determines the work performed to build the main chain), nodes can very quickly distinguish a real chain from a fake one.

The checkpoints mechanism (discussed in detail in the first part of this tutorial) also helps to combat this threat. However, remember that checkpoints are not a trustless solution. Anyone who adds them to the software may be an attacker, so there has to be a certain level of trust towards both software developers and an author of checkpoints.

### Routing attacks

Despite the fact that a Bitcoin node can be controlled from anywhere in the world, at the moment, nodes are unevenly distributed globally. According to research (Fig. 3.62), most Bitcoin nodes are located in a few Internet service providers (ISPs): 13 Internet providers (which makes up 0.026% of all Internet providers) accommodate 30% of the nodes of the entire Bitcoin network (shown on the left graph). Moreover, most of the traffic exchanged between Bitcoin nodes crosses several Internet providers. Studies show that 60% of all possible Bitcoin connections are run across 3 different Internet providers. That means that these 3 Internet providers can see 60% of all Bitcoin traffic (shown on the right graphic).

<img width="50%" src="/resources/img/volume-2/3.6-Basic-classes-of-attacks-on-Bitcoin/Figure-3.62-Ratio-between-the-total-number-of-nodes-and-connections-between-them.png" alt="Figure 3.62 – Ratio between the total number of nodes and connections between them"/> 

There is a BGP trick — this refers to  a routing attack in which the Internet provider incorrectly forwards traffic based on fake routing messages in the Internet routing system. Attacks of this type are quite common (Fig. 3.63): up to hundreds of thousands occur every month. Some of them are aimed at a huge number of network addresses — up to 30,000 IP prefixes (shown on the left graph).

These attacks affect the Bitcoin network today. Each month, at least 100 Bitcoin nodes become victims of BGP tricks, and in November 2015, 447 nodes were captured, which is 8% of Bitcoin nodes (shown on the right graphic).

<img width="50%" src="/resources/img/volume-2/3.6-Basic-classes-of-attacks-on-Bitcoin/Figure-3.63-Statistics-of-BGP-tricks-performed.png" alt="Figure 3.63 – Statistics of BGP tricks performed"/> 

An attacker that uses a routing attack can have two goals. The first is to split the Bitcoin network into parts, not allowing the nodes to synchronize with each other. Thus, an attacker forces the nodes to create alternative branches, instead of a joint chain of blocks. After such an attack ends, all blocks that were formed by a smaller group of nodes will be discarded. Transactions of smaller branches will be unconfirmed, and the reward for the formation of blocks will be invalid. When carrying out such an attack, it typically happens similar to the following scenario (Fig. 3.64).

> Step 0: nodes of the left and right sides interact
through the Bitcoin connections indicated by blue lines.

> Step 1: attacker wants to break the network into two disjoint parts: one on the left side and one on the right.

> Step 2: attacker captures traffic directed to the left nodes by performing a BGP trick.

> Step 3: shortly after capture, all traffic sent from right to left will go through the attacker (red lines).

> Step 4: attacker breaks these connections, effectively breaking the network into two parts.

> Step 5: during the attack, nodes in each part continue to communicate with nodes on the same side.

<img width="50%" src="/resources/img/volume-2/3.6-Basic-classes-of-attacks-on-Bitcoin/Figure-3.64-Attack-routing-scheme.png" alt="Figure 3.64 – Attack routing scheme"/> 

The second goal that an attacker can pursue is a delay of 20 minutes for the block to be delivered to the victim node. In this case, the attacker remains completely unnoticed. During this period, the victim is unaware of the most recently mined block and related transactions. The consequences of this attack depend on the type of victim. If the victim is a merchant, then he is subject to a double-spending attack. If the victim is a validator, then he is wasting the computing power of his equipment in vain. Finally, if the victim is an ordinary user, then he cannot contribute to the network by distributing the latest relevant blocks. Such an attack would be  carried out in the following way (Fig. 3.65)

> Step 0: Nodes A and B announce the same block to victim C.

> Step 1: Node C requests a block using GETDATA from node A. The attacker changes the contents of GETDATA to deliver the old block from node A.

> Step 2: the old block is delivered.

> Step 3: shortly before 20 minutes after the request from node C, the attacker starts its delivery by modifying another GETDATA message created by node C.

> Step 4: the unit is delivered in 20 minutes. The victim does not disconnect from node A.

<img width="50%" src="/resources/img/volume-2/3.6-Basic-classes-of-attacks-on-Bitcoin/Figure-3.65-Block-Delay-Attack-Scheme.png" alt="Figure 3.65 – Block Delay Attack Scheme"/> 

### Other technical and social attacks

Network traffic analysis (packet sniffing). Anyone who can see all of your Internet traffic can determine when you submit a transaction that you did not receive from another node (this implies that you created it). Internet service providers or national firewalls can filter Bitcoin protocol traffic, thereby causing some inconvenience to ordinary users. But the issue of filtering network packets can be resolved. For example, Bitcoin Core has good integration with Tor (a decentralized traffic anonymization network).

A block withholding (selfish mining) attack implies that there is a time delay for newly formed blocks before they are sent to other nodes. The attacker is a validator with sufficiently large computing power who creates new blocks of the chain but does not immediately redirect them to other network nodes. These blocks are published later one by one or in groups; that is why the blocks of the remaining validators are rejected by the network as the ones belonging to another, a shorter chain (orphan blocks). In this attack, other validators become victims; however, in the end they either take a lesser part in reaching consensus or cannot create any relevant block at all.

Other diverse attacks include planned forks, various kinds of restrictions on the use of currencies of certain states while exchanging bitcoins, blocking network traffic of a full node by ISPs as well as bans on the production of specialized equipment or its use.

The most specific attacks include the kidnapping of important community members and the introduction of malicious elements into hardware or software.

### Bitcoin alert system and how it is turned down

The alert system in Bitcoin allows you to broadcast messages to network clients about cases of detection of problems and potential threats (Fig. 3.66).

<img width="50%" src="/resources/img/volume-2/3.6-Basic-classes-of-attacks-on-Bitcoin/Figure-3.66-An-example-of-Bitcoin-alert-system-message.png" alt="Figure 3.66 – An example of Bitcoin  alert system message"/> 

As Satoshi himself stated on the bitcointalk.org forum even at the stage of developing the signal system, only he has the private key with which he signs messages (Fig. 3.67).

<img width="50%" src="/resources/img/volume-2/3.6-Basic-classes-of-attacks-on-Bitcoin/Figure-3.67-Message-on-the-forum-about-the-alarm-system.png" alt="Figure 3.67 – Message on the forum about the alarm system"/> 

In other words, this add-in is centralized. However, Satoshi did not see this as a big problem because it was more than overlapping with social consensus. Any user could check the data on the forum and other sources if he doubted the reliability of the problem (Fig. 3.68).

<img width="50%" src="/resources/img/volume-2/3.6-Basic-classes-of-attacks-on-Bitcoin/Figure-3.68-Message-about-the-possibility-of-checking-data-by-each-user.png" alt="Figure 3.68 – Message about the possibility of checking data by each user"/> 

Satoshi claimed that he could not carry out any actions remotely. And this innovation is designed to ensure the safety of user coins. If something suspicious happens during the operation of the protocol that could threaten the state of users' balances, then each of them will receive a message about this. Ignoring messages about potential danger, the owner of the coins may lose them (Fig. 3.69).

<img width="50%" src="/resources/img/volume-2/3.6-Basic-classes-of-attacks-on-Bitcoin/Figure-3.69-An-explanation-of-the-need-for-a-signal-system.png" alt="Figure 3.69 – An explanation of the need for a signal system"/> 

Satoshi later transferred his secret key with several trusted Bitcoin Core developers. Community members began to express their anxiety about the fact that if an attacker took possession of the key, he would be able to spread messages that could cause panic. Moreover, there were incidents where with the help of these keys, attempts to influence the rules of the network, the fees, and the complexity parameter were made.

This state of affairs was contrary to the Bitcoin security model. And in the Bitcoin Core 0.13.0 update, in 2016, the alert system was completely eliminated.

### Some discovered and resolved protocol problems

BIP-0050. In May 2013, the BIP 50 was proposed to address one of these problems. In the network, someone created and distributed blocks that had a larger number of total transaction inputs than the previous one. 0.8-Bitcoin nodes were able to handle this, but some of the nodes before version 0.8 did not, causing an unexpected split in blockchain. In the incompatible pre-0.8 chain at that time there was about 60% hashrate, which was not enough to automatically resolve the split.

In order to restore the canonical chain as soon as possible, BTCGuild and Slush lowered their 0.8-Bitcoin nodes to 0.7, due to which their pools also rejected a block containing a larger number of inputs. During this time, one big double-spend was made. However, this was done for the purpose of experimenting, not for malicious reasons.

Due to the fact that the network split was quickly noticed, serious consequences were avoided. As a result, an updated version of protocol 0.8.1 appeared, which included the following rules:

* reject blocks that can cause more than 10,000 locks;
* limit the maximum size of the created block to 500,000 bytes;
* create a security update for older versions that implements the same rules but with a limit on the number of maximum locks to 120,000;
* сreate a web page on bitcoin.org, forcing users to upgrade to version 0.8.1. The page also shows how to set DB_CONFIG on 120,000 locks.
* over the next 2 months, a series of alerts were sent to users of older versions about the need for updates.

CVE-2013-4165. Another vulnerability that was discovered on the Bitcoin network is that an attacker could send a series of messages that lead to an integer division by zero error in the Bloom Filter processing code, resulting in a denial of service in Bitcoin-Qt or bitcoind. Bloom Filters were introduced in version 0.8; therefore, versions 0.8.0 to 0.8.3 are susceptible to this denial of service attack.

After this vulnerability was discovered, a constant-time algorithm began to be used to verify attempts to guess the RPC password.

CVE-2014-0160. This vulnerability is not related directly to the Bitcoin network but to OpenSSL library. It can allow a remote attacker to obtain confidential data, including user credentials and private keys, by improperly handling memory in this library.

To eliminate this vulnerability, the OpenSSL protocol was updated to version 1.0.1g. All keys that are generated in older versions are considered compromised and must be regenerated using the new version of the protocol.

CVE-2016-10724. Prior to version 0.13.0, Bitcoin Core was vulnerable to DoS attacks caused by a remote network notification system when an attacker signs a message with a specific private key known to victim participants. This affects source code clones such as Bitcoin Knots up to version 0.13.0.knots20160814 and some altcoins.

**Common myths**

*Any programmer can verify that there are no vulnerabilities in Bitcoin.*

Theoretically, this is possible, but in reality, the analysis of such an amount of source code is a work for a large and professional team that would take a fairly long time.

**Frequently asked questions**

*– If a user has a lot of bitcoins in his wallet and is ready to spend it, how can he technically spam the Bitcoin mempool with transactions to increase the transaction cost?*

Indeed, if the user has a lot of bitcoins, he can spend them on a stress test. To do this, he needs to modify the software of his full node. A fairly simple option is to add a cycle with the generation of a new address and sending a random number of coins to it (for example, 0.5–2 BTC). This cycle is better to start in a separate thread and repeat 2 to 3 times per second. At the same time, calculate the optimal fee so that these transactions fall in the middle of mempool but are not confirmed in the first block. In other words, this will allow you to not overpay fees and effectively load all nodes in the network. This is the best way to fill mempool so that users who want to make urgent payments will have to include a very large fee so that their transactions are at the top of the queue and are likely to be included in the next block.

*– It is said that quantum computers will be a future threat to Bitcoin. What does this mean?*

This means quantum computing may pose an attack angle on the digital signature algorithm that Bitcoin is using now. At the moment, there are no quantum computers capable of implementing such an attack (it is still not clear whether they will be created at all). Even if they appear, the cryptography in Bitcoin will be updated and will be more resistant to quantum computing. There are algorithms for post-quantum cryptography already.

*– Can a manufacturer of mining chips take control of capacities through a built-in backdoor and conduct a 51% attack?*

It is potentially possible, although it is difficult enough and most likely to be economically unreasonable for a manufacturer. Nevertheless, finding a backdoor in mining chips is quite difficult. Therefore, it is better when there are many independent chip manufacturers and none of them surpasses the rest, put together, in terms of production. In addition, many altcoins use ASIC-resistant hash functions, for which creating hardware miners is extremely difficult, so mining is conducted on GPUs or general-purpose processors.

*– What tools are used to attack Bitcoin?*

Most often they are specialized software created based on the source code of the full network node.

*– What will happen to Bitcoin if the Internet between the continents disappears and then appears again?*

There will be one transaction history on one continent and another on the other. When the connection is restored, there will be only one chain, the one for which more computing power was spent. For people from a continent with a smaller chain of transactions who do not conflict with the main chain, they will again become unconfirmed, but they will be later confirmed in subsequent blocks.

*– Is the guarantee that there is no backdoor in the Bitcoin code?*

Specialists' opinion is that there is no intentionally built-in backdoor since the source code is publicly available, meaning everyone can see and pen-test it. However,  there may be unintentional errors that might make the protocol vulnerable. The probability of this is small since rigorous testing is carried out and malicious scenarios of outer nodes' behaviour are worked through. On the other hand, you can additionally verify everything by self-auditing the source code or contacting a trusted party for help.

[BITCOIN AS A PLATFORM](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-2/en/4-bitcoin-as-a-platform.md)
