# 4 Technological details of Bitcoin operation
## 4.1 How do Bitcoin transactions work?
A lack of registration requirements for users in Bitcoin has drastically affected the format of transactions—storage and 
processing is much different compared to transactions in existing financial systems. Hence, the unique format of bitcoin 
transactions is designed to maintain the anonymity of users and validators. The major differences are how transactions 
are linked to each other (so-called triple-entry accounting), the concept of an unspent output, and how a change (the 
rest of the money after a transaction) is formed. We will study how and why these mechanisms work.

### Transaction structure
Schematically (Fig. 4.1), a transaction consists of three components: the header, input coins, and output coins.

Going a bit further, every transaction can have several inputs and outputs in its structure; their number is not 
limited. In Bitcoin, what is limited is only the overall size of a transaction, which is 25% of the maximum block size. 
For example, if the maximum block size is 1 MB, then the maximum transaction size is 250 kB.

As you can see in Fig. 4.1, each transaction contains an input and an output. An input contains a reference to some 
previous transaction (a coin source). An output is created in the current transaction in order to spend coins.

[Figure 4.2] - Simplified structure of Bitcoin transaction 

Each input contains a proof that the creator of the transaction owns the coins that she wants to spend. In Bitcoin, 
coins are not recorded into any balance. Instead, there is a reference to the transaction from which the coins were 
received. This creates a chain of ownership of coins, which makes it easy to trace the history of coin transfer.

Each output contains a payment amount and *terms of coin spending*, which the receiver must fulfill with the *proof of 
coin ownership* in order to be able to send the coins further.

Let's consider the body of one particular transaction that was once confirmed in the Bitcoin network. Figure 4.2 
presents it in the JSON format.

[Figure 4.2] - Example of a transaction in the Bitcoin network

> * ScriptPubKey—a field containing terms of coin spending
> * ScriptSig—a field containing proof of coin ownership
> * Bitcoin Script—a special language used to fill in these fields

There is a header that contains two fields: the version of a transaction and the parameter locktime (it will be observed 
in more detail further). The transaction also contains two inputs and two outputs. The inputs field has the biggest 
volume. Within the inputs field, the field scriptSig is the largest, because it contains a signature, which takes 64 B, 
and a compressed public key, which takes 33 B. In a serialized form, the entire transaction occupies 373 B (the 
transaction is transmitted over the network exactly in this form).

> **_NOTE:_** *Serialization is the transformation of some data structure into a bit sequence.*

Most often, terms of coin spending are encoded into a readable bitcoin address, and when the transaction is created, 
they are decoded and specified in *scriptPubKey*.

If you consider the *transaction input* structure (vin) in detail, you will see the fields shown in Figure 4.3. In the 
field *hash*, the identifier of the previous transaction in which the coins were received is indicated. *Index* is the 
ordinal number of the output of the previous transaction from which the coins will be spent.

[Figure 4.3] - Structure of Bitcoin transaction inputs

*ScriptSig* contains a proof of coin ownership. This field is called so because it generally contains a digital 
signature and can also contain a public key to verify this signature.

The next field is called *sequence*. It indicates the version of the transaction and is set by the sender. By using 
sequence, you can replace a transaction that has been sent to the network but has not been confirmed yet. Suppose that a 
user has created a transaction, but it is not yet included in the block—for certain reasons (e.g. the time did not come 
or the fee has been set too low). A user decides to modify this transaction (e.g. update the coin recipient address, 
specify a different recipient, add more fees, change the payment amount, etc.), so she creates a new, alternative 
transaction that spends the same coins and has the same inputs as the previous one. The difference is that the 
*sequence* value specified in the inputs is incremented by one. According to the protocol rules, validators who confirm 
this transaction can replace the old transaction. After that, the new transaction is most likely to be confirmed.

The structure of a transaction output (vout) contains two fields: value, which indicates the number of coins intended 
for the recipient, and scriptPubKey, where the terms of coin spending are specified (Fig. 4.4). The second field, 
scriptPubKey, has such a name because it generally contains either a public key or a hash value of a public key.

[Figure 4.4] - Structure of Bitcoin transaction outputs

In this way, unlike the design of transactions in traditional payment systems that essentially carry one primary 
message—the transfer of a particular amount from balance A to balance B,—a bitcoin transaction is a complex set of data 
that contains the proof that the initiator of a transaction owns the coins transferred. This approach can prove to be 
more reliable in particular cases, but it is more demanding in the context of capacity.

The limited capacity of Bitcoin as a payment system became an issue that required a solution; the protocol improvement 
was then presented (the Segregated Witness update). The proposition was to exclude the *proof of coin ownership* from 
the transaction and put it in a separate *witness data structure* (Fig. 4.5). In this way, the data would not need to be 
added into every transaction input and a significant amount of memory could be freed up. In this way, a transaction 
contains all the necessary data, including the origin of coins and where they are distributed, but the proof of 
ownership is stored outside the transaction. In some cases, you can use the main part of the transaction without the 
witness data (see 4.6).

[Figure 4.5] - Simplified structure of a Bitcoin transaction after the Segregated Witness update

### Unspent outputs
*An unspent transaction output (UTXO) is an output that contains unspent coins; these coins can only be spent when 
specified in the input of a new transaction.*

We have considered the structure of a transaction and, as you can see, at the protocol level, there are no traditional 
accounts or balances in Bitcoin; there are only transactions with their inputs and outputs. Each output may have two 
states: spent and unspent.

Unspent outputs can be compared to cash money. Owning bitcoins is similar to owning ordinary currency notes that you 
received but haven't yet spent. You cannot say exactly how much money is in your wallet until you open and count all 
your currency notes. When you own a bitcoin wallet, you are supposed to count all unspent outputs from transactions that 
were addressed to you, sum them, and only after that, you will know your balance. Of course, most bitcoin wallets do 
this automatically for you, but the principle of how this "balance" is formed is as described. The essence of the 
analogy of bitcoin outputs with currency notes is that you cannot divide a banknote when you pay for something less than 
its denomination—you need to give the banknote of a particular value and receive a change with some banknotes of smaller 
values.

Despite the fact that there are no balances at the Bitcoin protocol level, the current balance of an address is the sum 
of all coins from unspent transaction outputs that were sent to this bitcoin address. It should be understood that there 
are generally two balances for each address: a conditionally (with a high probability) confirmed one and an unconfirmed 
one.

In the implementation of the full Bitcoin network node, there is the separate coins database module for storing and 
processing the actual status of the entire UTXO set.

### Receiving a change and setting fees

Let's say that a user once accepted a payment of 10 coins and now wants to spend only 3 of them. The fact is that in 
Bitcoin, you cannot only spend part of your coins; you have to spend the whole amount.

The question introduces the concept of a change. A user cannot spend a part of the 10 received coins, however, he can 
add several recipients to the transaction. So, the solution is that a user sends 3 coins to the recipient and the 
remaining 7 coins to himself; hence, a change is an output addressed to oneself (*a change output*). A user has a new 
unspent output of 7 coins.

Now, it makes sense to clarify how fees work since the situation described above is reasonable only if the transaction 
is eventually confirmed. Therefore it is necessary to somehow motivate the owner of a full node to validate your 
transaction. This essentially is the role of fees.

We have already observed the transaction structure, but as you may have noticed, it does not include any special field 
where a fee is indicated. However, each user in Bitcoin has the ability to specify a fee in the transaction himself.

There is one very simple and logical rule within the Bitcoin protocol: the sum of all transaction outputs must not 
exceed *the sum of all inputs*. The number of coins before and after a transaction remains the same; it is just that 
they can be distributed between different addresses.

In Bitcoin, the fee for a transaction is determined by the difference between the sum of inputs and the sum of outputs. 
Imagine that you have 5 coins and want to spend 2 coins. You create a transaction in which you specify one input—the 
output of a particular previous transaction where you have received the 5 coins—and two outputs. The first output sends 
2 coins as a payment for some service, and the second output sends a change of 2 coins to your different address. As a 
result, there are 5 coins at the input of your transaction and 4 at the output. Then 1 coin will be considered as a 
transaction fee.

The remaining difference does not belong to anyone until the transaction is confirmed and included in the block. When 
the block containing this transaction is created, its creator will be able to collect the fee as a reward.

### Example of coin transfers 
Suppose Bob has one unspent output (UTXO) on 100 bitcoins: he received them as a birthday present. He decides to use 
these coins to pay Alice and Charlie, which is schematically pictured in Figure 4.6.

[Figure 4.6] - Transfer of funds to two recipients within one transaction

Bob creates a transaction with one input in which he refers to the transaction from which he received these 100 coins 
and adds two outputs to the transaction: one output sends 45 coins to Alice, the other sends 50 coins to Charlie. Note 
that the difference between the sum of inputs and the sum of outputs is a fee, which in our example is 5 coins.

Alice and Charlie will spend the coins as they wish to (Fig. 4.7). Alice pays 20 coins to the gardener. She refers to 
the transaction in which she received 45 coins (Bob's transaction) and spends all of them in the current transaction 
indicating the address of the gardener as a first recipient (20 coins, as in the figure below) and her own address as a 
second recipient (also 20 coins). 5 coins remain as a fee. Charlie, who received 50 coins, also spends his money: he 
pays 10 coins for the new windows, 35 coins for car repair services, and 5 coins as a fee.

[Figure 4.7] - Using of the previous transaction outputs to make payments

Now let's imagine that the glazier and the owner of a car workshop turned out to be friends and agreed to make a gift to 
their mutual fellow. They have decided to spend 10 coins each and buy a painting from a local artist. For this, they 
will need to create a shared transaction with two inputs: 10 and 35 coins, and two outputs: 20 coins to the artist, and 
the other 20 coins as a change to the car workshop owner (Fig. 4.8). The difference of 5 coins remains as a fee, which, 
in our case, is paid by the car workshop owner (let's say he was indebted to the glazier for a new window).

[Figure 4.8] - Creation of a transaction with the inputs from different senders which pay to one recipient

### Creation of transactions in bitcoin wallets
How does a bitcoin wallet work and what processes occur during its operation? The typical processes are the generation 
of a key pair and bitcoin addresses, and also the generation, the storage, the signing, and the sending of transactions. 
To get up-to-date information about the new blocks (i. e., new confirmed transactions), all nodes of the Bitcoin network 
synchronize. A wallet displays current balance and makes up a list of accomplished transactions. Furthermore, the 
correctly implemented wallet software follows the particular rules while generating transactions.

> **Common rules for transaction creation**
>> * Create a new address for each incoming payment and change
>> * Optimally select UTXOs for the target payment amount
>> * Sort transaction inputs and outputs by a common for all rule
>> * Include a minimum fee regardless of the size of a transaction

Imagine a traveler who has a bitcoin wallet with a certain set of UTXOs: 1 BTC, 3.5 BTC, 0.3 BTC, and 0.4 BTC. The 
traveler is ready to conquer new horizons, and all that's left for him is to pay for the transport (Fig. 4.9). It is 
known that a one-way ticket costs 0.5 BTC, and the traveler does not have a coin of suitable nominal value. Therefore 
the software of the wallet uses an algorithm that searches for the best combination of UTXOs in the traveler’s wallet: 
0.3 BTC and 0.4 BTC and creates a new transaction referring to these coins.

[Figure 4.9] - Choosing proper unspent outputs to perform fund transfer

The new transaction will include two new outputs: the zero output goes to the ticket seller for the transport (0.5 BTC), 
and the first one goes back to the traveler as the change (0.1 BTC). Obviously, the transaction fee is 0.1 BTC.

After a transaction is created and signed by a wallet, it should be propagated over the network. For this, in the 
Bitcoin network, there is a default mechanism which works similar to how you spread interesting news in the real world, 
by word of mouth.

Consider the scheme of the Bitcoin network (Fig. 4.10); nodes are randomly connected to each other. So, some network 
node receives the traveler’s transaction, in which he wants to pay his fare with some coins. The node independently 
verifies the transaction, adds it to its mempool (if the transaction is considered correct), and then transfers it to 
the closest nodes. Each of these closest nodes does the same (that said, after having verified the transaction, they 
all pass the transaction to their closest nodes). As a result, the transaction is eventually propagated in the entire 
Bitcoin network.

[Figure 4.10] - Propagation of a transaction in the Bitcoin network

However, this process cannot last forever. Note that each transaction is identified with its hash value. Using this 
value, each node can determine whether it has already verified a transaction or not. In case a node receives a 
transaction which is already in its pool (i.e., a transaction that this node has already verified), it will not forward 
the transaction to other nodes. This scheme allows avoiding an infinite transaction propagation loop.

We have already mentioned that using a particular address, you can trace the entire history of payments in 
Bitcoin—history of coin origin. As in the above example (see section “Example of coin transfers”): there were 100 coins, 
which were in a certain way transferred between different addresses. Having a full copy of the database, you can trace 
the transaction history up to the very transaction where the coins were mined.

What should you do to prevent your entire financial history in Bitcoin from being fully disclosed? Imagine that every 
Bitcoin user has only one address both for receiving and spending coins. Having disclosed the association between this 
address and the identity of a particular user, you can trace her entire financial history in Bitcoin.

Figure 4.11 schematically shows the relationship between different transactions in the chain of blocks. It is a question 
of no difficulty to restore the history of the origin of coins and trace the entire chain of transactions through which 
a user received the payment.

[Figure 4.11] - Link between transactions in the chain of blocks

> **_NOTE:_** *In Fig. 4.11, the transaction that is zero by order is a coinbase transaction; it contains the reward 
> provided by the protocol rules for the validator who created this block (for more detail, see 4.3). A coinbase 
> transaction includes a special parameter called coinbase maturity, which, according to the Bitcoin protocol rules, 
> is 100. This means that after the block with the coinbase transaction, 100 more blocks must be added to the mainchain 
> so that the participant who mined this block could spend coins from the coinbase transaction output. In the above 
> scheme, coinbase maturity is equal to 1.*

In most cases, tracing financial history by unauthorized persons is unacceptable to users. Therefore there is a way that 
significantly complicates the unraveling of all chains of transactions, tracing the history of payments, and 
establishing the ownership of specific coins by specific users: most bitcoin wallets automatically create a new address 
for each new incoming payment as well as for receiving a change.

### Locktime mechanism
There is a way to restrict the confirmation of a bitcoin transaction by time, such that even if it is published on the 
network, validators cannot confirm it until a certain moment. For this purpose, the special parameter *locktime* is 
used. It is a 4-byte value which is included in the header of each transaction in Bitcoin. This value allows specifying 
the moment till which the confirmation of a transaction will be delayed. The parameter *locktime* can be set with a value 
of either of the two data types: the time in the Unix timestamp format or the block height. According to the protocol 
rules, if the parameter locktime is set, a validator cannot include the transaction in his block until a certain moment 
of time or until the block with a certain height appears.

This is how the delayed transactions mechanism is implemented if you assume that you will not potentially spend your 
unspent coins (e.g., keys will be lost or users will not reach consensus and the terms of coin spending will not be 
satisfied) for some reason. In this case, you create the so-called backup transactions that will transfer these coins to 
a “backup” address as soon as the specified time interval has passed; until that moment, you will be able to use the 
coins as usual. Schematically, it is shown in Figure 4.12.

[Figure 4.12] - Transaction with delayed confirmation

Suppose that in the eighteenth block, a specific unspent output was created. If the transaction that spends coins of 
this UTXO has the parameter locktime equal to 404 (and it has passed the preliminary verification), it will remain in 
the mempool as unconfirmed. It can be stored there until the moment of confirmation in the block with a height of more 
than 404, or it could be replaced by a conflicting transaction (e.g., the one that spends coins from the same UTXO and 
includes a greater fee).

> **_NOTE:_** *A user can store a transaction on a paper piece in a printed form as well. For instance, he has no 
> internet connection and cannot restore it. In this case, the user can create and sign a transaction offline and then 
> print it on a piece of paper. Next, he sends the print-out to a trusted party with an internet connection (it can be 
> the recipient of coins as well) that will transfer the data (of the already signed transaction with the recipient's 
> address) from the paper piece to his own device and propagate the transaction on the network to confirm it.* 

### Off-chain protocols
It is possible to create a chain of coin spending which can be confirmed afterward. On the right in Figure 4.13, you can 
see a chain of blocks which are already created and contain on-chain transactions. On the left, you can see the 
off-chain transactions—the ones which already refer to the UTXO of a specific on-chain transaction but have not yet been 
added to the mainchain (for instance, due to the locktime parameter set). Further, you can also create any number of 
transactions that will refer,  accordingly, to the outputs of the off-chain transactions. This will result in one or 
several alternative chains of coin spending. Though, no off-chain transaction can be added to the block of the mainchain 
until the transaction preceding it (the one having the output to which it refers) is added.

[Figure 4.13] - How off-chain protocols work

Which of the off-chain transaction chains receives confirmation depends on the validators. Some of them will create a 
new block, where one of the alternative chains will be partially or fully confirmed. Thus, even if the transaction is 
not confirmed, it does not block the coins, which means that you can build a chain of other transactions based on it. 
This is the main principle of the off-chain protocol.

### Signature hash types
The process of signing a bitcoin transaction can be divided into two steps. The first one is the creation of a message 
which is to be digitally signed. The second one is the computation of the digital signature.

In fact, users can sign only a part of a transaction to be able to modify it later. Generally, this makes sense when the 
transaction is created with several inputs, all belonging to independent users.

Let's observe what transaction part can be covered by a signature, how the transaction templates for each signature type 
are generated, and when a specific signature type should be used. There are three basic signature types: *SIGHASH_ALL*, 
*SIGHASH_NONE*, and *SIGHASH_SINGLE* [40].

*SIGHASH_ALL* is the most commonly used type. It covers all inputs and outputs of a transaction and implies that neither 
of the transaction elements can further be modified, except for a signature script (scriptSig).

*SIGHASH_NONE* allows signing all inputs without signing any output. In such a case, anyone can change the transaction 
outputs before they are locked by a signature of another type. It is no use creating a transaction with one input and 
the *SIGHASH_NONE* signature type because a validator will be able to simply rewrite the outputs of a transaction, and 
the coins will be transferred to a wrong address. This approach is convenient if you create a shared transaction with 
several inputs and it is enough that just one of the signers use *SIGHASH_ALL*.

*SIGHASH_SINGLE* allows signing all the inputs and only one output, the index of which matches the index of an input 
containing a signature. This case implies that a user is ready to pay on the corresponding output address only if all 
other transaction participants spend their bitcoins as well. When might this be necessary? For example, a husband and 
wife want to pay 5 bitcoins for their child’s studying. Let’s suppose that the husband has an unspent output of 3 BTC, 
and the wife has an unspent output of 2.5 BTC. They agree and create a shared transaction. First, the husband submits 
his UTXO to the input. Then, he signs all the inputs and the output where he specifies the amount of 5 BTC and the 
address of the educational institution. The wife agrees with the conditions set by the husband so she can sign the 
transaction (either using *SIGHASH_ALL* type or *SIGHASH_SINGLE*), but before that, she needs to specify another output, 
which will pay 0.5 BTC to her address as a change.

### Writing arbitrary data to the chain of blocks
As the popularity of the Bitcoin accounting system grew, its users started to wonder how to use it to record not only 
coin accounting data but also completely arbitrary data.

Going a bit further, Bitcoin block essentially contains two parts: a block header and the transaction data. The header 
of the block contains strictly 80 B of data and each field is validated, meaning that you cannot add arbitrary data 
there. However, the structure of a transaction in Bitcoin allows you to write data in it, namely in its fields. So, 
let's consider this in more detail (Fig. 4.14) [41].

[Figure 4.14] - Example of a transaction containing arbitrary data

In the *transaction header*, there are 4 B of data with which the version of a transaction is indicated.

There is also the field *lock_time*, where you can arbitrarily set the time moment till which a transaction cannot be 
added to the block. In this case, you need to remember that this can greatly limit the confirmation time of a 
transaction. However, you can actually manipulate this value by specifying a moment of time which has already passed. In 
such a way, you can enter only approximately 1 B of arbitrary data, which is little as compared to the size of a 
transaction. Hence, this method is considered inefficient.

*Transaction inputs* contain hash values of the previous transactions, which must necessarily match existing 
transactions; the output indices of the previous transactions must be the same as well.

The data of the *proof of coin ownership* must be integral, hence, modifying them is not an option.

As for the *transaction output*, the situation is more favorable. There is a certain amount of coins for the recipient, 
which is set arbitrarily in a certain acceptable range. In other words, arbitrary data can be entered by manipulating 
the *output amount*. In addition, there is an address, for which it is not inspected during transaction verification 
whether it has been generated correctly; it is simply a 20-byte hash value of the data which have not yet been 
published. Accordingly, they can be set arbitrarily. There are many practical examples where redundant outputs were 
added to the transaction and arbitrary data were injected instead of addresses; those arbitrary data were relevant for 
some third-party applications (e. g., traders’ anonymous signals as well as songs, poems, etc.). The third-party 
applications, in turn, were able to verify that the data were actually stored in the Bitcoin accounting system.

Bitcoin protocol developers have noticed that there is a demand for writing arbitrary data and thus they have added a 
special operation. It can be used to specify the coin spending terms allowing to inject arbitrary data into an output. 
However, the output of a transaction with such an operation is always considered invalid according to verification 
rules. This means that coins with such UTXOs cannot be spent no matter how many of them there are. This operation is 
called OP_RETURN, in which the output verification function immediately returns a FALSE value. The output that utilizes 
such an operation allows you to add up to 80 B of arbitrary data [42]. Thus, many applications which worked on top of 
Bitcoin, including the ones implementing anonymous data transmission, were using this operation specifically.

One of them was the Colored Coins protocol—a kind of additional logic on top of the bitcoin wallet. With the Colored 
Coins protocol, you can supplement a transaction with additional (redundant for Bitcoin) data, which "color" the coins 
and indicate their additional features. These colors are associated with certain logic: first of all, peculiarities of 
transaction validation and, of course, the purpose of coins transferred in this transaction. The idea of this action is 
that once some small amount of bitcoins is colored, it starts representing the value as not only bitcoins but also, for 
example, as the company's stocks. If you save these additional data, the modified wallets which support the Colored 
Coins protocol will detect additional purposes of particular coins as well as display their different cost. Meanwhile, 
such coins can be transferred as “usual” bitcoins.

There are protocols, such as Omni Layer and Counterparty. They operate on top of the Bitcoin protocol and allow their 
users to issue custom tokens (as well as exchange and trade them). These protocols support their own transaction logic, 
consensus mechanism, transaction data structure, and verification rules. The peculiarity is that transactions in such 
protocols are serialized and divided into 80-byte datasets. This is done so as to put these transactions into bitcoin 
transactions. However, users of these protocols pay an additional fee for having their transactions stored in Bitcoin 
because one such transaction is generally “bound” to several bitcoin transactions. Hence, in the Omni Layer or 
Counterparty protocols, the transaction price will be higher than in Bitcoin.

### Summary
One wallet can generate and process an unlimited number of addresses. This raises user privacy through an increased 
complication of tracing the transaction history by outsiders. Noteworthy, coins received on one wallet or address are 
not combined together and are not recorded in one balance in the database—they are used separately. You can have 
different coins in one wallet: each coin will be associated with its own history. You can use them for different 
purposes and never disclose any information about the ownership of coins to others.

Each transaction output is linked to a specific address. In other words, coins are locked according to particular terms 
of their spending. For instance, coins can be sent not just to a usual address but rather to the address from which the 
coins can be spent only if some specific terms are fulfilled. You can specify that your coins can be spent by the one 
who provides a solution to an equation (e.g., if the equation is 5 + 7, then the one who writes 12 in *scriptSig* will 
be able to spend the coins). The conditions can be different: providing a public digital key, a preimage of a hash 
function, or a solution to a specific mathematical problem (for more details, see 4.5).

**Frequently asked questions**

*– Do unconfirmed transactions lock the amount on the account?*

No. Before the transaction is confirmed, you can create an alternative one with different conditions for spending the 
same coins. Among all the alternative transactions, only one can be considered valid and will eventually be included in 
a block. The decision to include a particular transaction in a block is made by a validator who creates a block. 
A transaction included in a block will be considered confirmed if the corresponding block is accepted by other 
validators.

*– Can the recipient of a transaction increase the fee to contribute towards its confirmation?*

A recipient cannot add a fee to the transaction from which she receives the coins, but she can create a transaction in 
which she spends the coins from this unconfirmed transaction and specify a higher fee in it—this will motivate 
validators to confirm both transactions. This functionality is already implemented, and it is called 
child-pays-for-parent (see 4.7). Some wallets and most validators support it. You can pay for the old transaction 
with a new one so that both transactions will be included in the block.

*– How to calculate a fee intelligently?*

The fee in Bitcoin is not calculated on the principle of a bid for a transaction or as a percentage of the payment 
amount; it depends on how active the Bitcoin network is at the time, namely on the number of transactions in the network 
and the transaction size in bytes. First of all, you should focus on the cost of adding 1 B of data to the Bitcoin 
database, and then multiply this cost by the size of your transaction in bytes. There are many resources on the Internet 
that count the price of adding data. Usually, it ranges from 10 to 500 satoshi per byte, depending on the current load 
on the network. In some cases, wallets use the dynamic value of this price, and in some, they use a constant value. 
Sometimes users can specify the fee value themselves.

*– Can a validator confirm transactions regardless of the size of the fee?*

Validators are free to choose which transactions they will confirm. They can filter small transactions and add only the 
larger ones, or only add transactions of their friends, and even choose transactions with lower fees—it all depends on 
the owner’s capacity and the preferences; he uses his software independently. Most often validators chase the profit and 
confirm only those transactions that pay the highest fee for the unit of their data. If, however, you are a validator 
and you want to confirm transactions with zero fees, you can independently confirm your transactions that do not pay 
fees. It is better not to propagate such transactions on the network but only add them to your own blocks.

*– Why don’t wallets include functionality to send coins to multiple recipients with one transaction?*

Indeed, wallets usually allow for the creation of transactions on two outputs: for the payment and for the change. 
Wallets rarely implement the functionality to include multiple recipients in a single transaction because it is only 
relevant in rare cases. However, in Bitcoin Core, Electrum, and some other desktop wallets, this functionality is 
implemented and you can add as many recipients to the transaction as you want. You can also choose coins from the 
previously received ones and decide which one you will spend. So, if you need a flexible way of creating a transaction, 
it exists.

*– If it is possible to complicate the tracing of transaction chains, then what is the purpose of mixers?*

Indeed, the generation of new addresses effectively complicates chain tracing but does not exclude this possibility. 
There are methods that, with a certain probability, allow restoring the actual distribution of coins among users. 
A centralized mixer is just one of the options, yet it is considered unreliable as compared to other options because it 
is not trustless (for more details, see 7.2).

*– Can I make two different transactions that will spend the same coins?*

Yes, you can. In such a case, there would be two conflicting transactions. You can specify the rules according to which 
one of the transactions will have a higher priority to be added to a block using the parameter sequence. If these rules 
are not set, validators will add one of them to a block at their own discretion.

*– How do you optimally create transactions to reduce the fees?*

It is necessary to select outputs optimally: you either do this manually or use wallets with a special algorithm 
implemented. The analogy is when people select cash banknotes when paying for goods in the store. It is easier to pay 
the amount of $30 with two banknotes (worth $20 and $10) rather than with thirty banknotes worth $1. With bitcoins, 
similarly, you need to choose the unspent outputs optimally. If you want to pay several people at once, it is better to 
do it by one transaction rather than by the separate ones. In this case, the size of one transaction will be less than 
the total size of separate transactions even if they use the same set of UTXOs. This is because the change output will 
be included only once. Due to this optimization of data, you save money on fees.

## 4.2 Mining in Bitcoin