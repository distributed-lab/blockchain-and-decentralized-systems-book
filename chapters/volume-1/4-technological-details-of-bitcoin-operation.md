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

![Figure 4.1 - Simplified structure of Bitcoin transaction](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.1-simplified-structure-of-tx.png)

Each input contains a proof that the creator of the transaction owns the coins that she wants to spend. In Bitcoin, 
coins are not recorded into any balance. Instead, there is a reference to the transaction from which the coins were 
received. This creates a chain of ownership of coins, which makes it easy to trace the history of coin transfer.

Each output contains a payment amount and *terms of coin spending*, which the receiver must fulfill with the *proof of 
coin ownership* in order to be able to send the coins further.

Let's consider the body of one particular transaction that was once confirmed in the Bitcoin network. Figure 4.2 
presents it in the JSON format.

![Figure 4.2 - Example of a transaction in the Bitcoin network](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.2-example-of-tx.png)

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

![Figure 4.3 - Structure of Bitcoin transaction inputs](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.3-structure-of-in.png)

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

![Figure 4.4 - Structure of Bitcoin transaction outputs](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.4-structure-of-out.png)

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

![Figure 4.5 - Simplified structure of a Bitcoin transaction after the Segregated Witness update](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.5-simplified-tx-with-seg-witness.png)

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

![Figure 4.6 - Transfer of funds to two recipients within one transaction](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.6-transfers-within-tx.png)

Bob creates a transaction with one input in which he refers to the transaction from which he received these 100 coins 
and adds two outputs to the transaction: one output sends 45 coins to Alice, the other sends 50 coins to Charlie. Note 
that the difference between the sum of inputs and the sum of outputs is a fee, which in our example is 5 coins.

Alice and Charlie will spend the coins as they wish to (Fig. 4.7). Alice pays 20 coins to the gardener. She refers to 
the transaction in which she received 45 coins (Bob's transaction) and spends all of them in the current transaction 
indicating the address of the gardener as a first recipient (20 coins, as in the figure below) and her own address as a 
second recipient (also 20 coins). 5 coins remain as a fee. Charlie, who received 50 coins, also spends his money: he 
pays 10 coins for the new windows, 35 coins for car repair services, and 5 coins as a fee.

![Figure 4.7 - Using of the previous transaction outputs to make payments](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.7-using-tx-to-make-payments.png)

Now let's imagine that the glazier and the owner of a car workshop turned out to be friends and agreed to make a gift to 
their mutual fellow. They have decided to spend 10 coins each and buy a painting from a local artist. For this, they 
will need to create a shared transaction with two inputs: 10 and 35 coins, and two outputs: 20 coins to the artist, and 
the other 20 coins as a change to the car workshop owner (Fig. 4.8). The difference of 5 coins remains as a fee, which, 
in our case, is paid by the car workshop owner (let's say he was indebted to the glazier for a new window).

![Figure 4.8 - Creation of a transaction with the inputs from different senders which pay to one recipient](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.8-creation-tx-from-diff-senders.png)

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

![Figure 4.9 - Choosing proper unspent outputs to perform fund transfer](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.9-choosing-proper-outputs.png)

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

![Figure 4.10 - Propagation of a transaction in the Bitcoin network](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.10-propagation-of-tx.png)

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

![Figure 4.11 - Link between transactions in the chain of blocks](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.11-link-between-txs.png)

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

![Figure 4.12 - Transaction with delayed confirmation](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.12-tx-with-delayed-confirmation.png)

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

![Figure 4.13 - How off-chain protocols work](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.13-how-off-chain-protocol-work.png)

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

![Figure 4.14 - Example of a transaction containing arbitrary data](/resources/img/chapter-1/4.1-how-do-bitcoin-transactions-work/4.14-arbitary-data.png)

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
One of the tasks that the creator of Bitcoin faced was to guarantee the synchronization of data between an unknown set 
of participants who cannot be identified and do not trust each other. The existing approaches to consensus building were 
not suitable since an attacker could create fictional participants and act on their behalf, imposing his version of the 
transaction history on the honest participants (a so-called Sybil attack). As a result, this task was solved in a way 
that, although does not guarantee the finality of transaction history, makes the attack economically unprofitable under 
normal conditions. The idea was that instead of a simple voting for transactions by the majority of participants, the 
voters were required to provide the proof of the ownership of a physical resource (that is, the proof of the performed 
work), which represented direct costs of real money.

Thus, for his opinion to prevail, an attacker would need more resources than all other honest participants combined. In 
a sense, mining plays the role of a completely honest roulette wheel that points at the participant (among those who 
provided the proof of their work) who gets the right to create a new block and receive a reward for his work.

In this subsection, we will consider the mining mechanism in Bitcoin and its role in the decentralized environment as 
well as its features and tasks it solves.

### Goals of mining in Bitcoin 
*Mining in Bitcoin is the process of creating a block and confirming transactions, which is rewarded with coins* [43]. 
Mining in Bitcoin has several goals. One of them is to motivate users to run the network nodes and support correct 
operation of the payment network. Another its goal is that it protects the transaction history in Bitcoin from changes. 
A user who has successfully created a block of the chain receives a certain amount of new bitcoins provided by the 
protocol rules. Mining has also solved the problem of a fair initial distribution of bitcoins among users: it was 
available for anyone with a computer.

> **Problems that mining solves**
>> * Decision-making in a decentralized environment
>> * Choosing the main version of transaction history
>> * Transaction confirmation
>> * Issuance

### Classification of network nodes
The Bitcoin network can be represented schematically as in Figure 4.15. Independent people and organizations maintain 
full nodes, but only a few of them are engaged in mining (known as node-validators).

![Figure 4.15 - Scheme of Bitcoin network](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.15-scheme-of-bitcoin-network.png)

There are many users on the network and each pursues their own goals. Some deploy a full node and mine, thus actively 
supporting the network; some simply store and process all transactions; some work as a payment gateway operator, and 
some, without a node, simply use a wallet and do not directly participate in the confirmation of transactions. Thus, all 
nodes of the Bitcoin network can be divided into at least three groups (Fig. 4.16).

![Figure 4.16 - Classification of the Bitcoin network nodes](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.16-classification-of-nodes.png)

*A full node is a node that performs verification and storage of all transactions.*

*A node-validator is a full node that creates blocks.*

*A lightweight node is a node that synchronizes and only checks its own transactions.*

### Concept of a resource-intensive problem
The essence of a *resource-intensive problem* in Bitcoin lies in finding the preimage of a hash value obtained through 
the SHA-2 algorithm on the length of 256 bits. The solution to the problem can only be found with brute-force search, 
thus finding it requires a lot of resources. In other words, it requires sorting input values until you find the one 
which when hashed gives the outcome of a certain pattern (Fig. 4.17).

![Figure 4.17 - Brute-force search of values to find the hash value preimage](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.17-brute-force-search.png)

Moreover, this hash value must satisfy a certain difficulty parameter. According to the rules of the Bitcoin protocol, 
the number of zero higher-order hexadecimal digits in the final hash value increases along with the difficulty 
parameter.

The difficulty parameter is recalculated every 2016 blocks with certain restrictions on the maximum change (this is done 
for protection against sudden difficulty surges in a short time) [44]. The difficulty recalculation algorithm compares 
the time spent on the creation of the last 2016 blocks with the time of two weeks, which is 1,209,600 seconds. If the 
creation of blocks takes less time than in the past two weeks, then the difficulty parameter increases proportionally; 
correspondingly, if it takes more time, the difficulty is proportionally reduced. 

> **_NOTE:_** *The difficulty parameter cannot be increased or decreased by more than 4 times.* 
> 
> *A resource-intensive problem is unique for each block since the data in the block is the input for the creation of a 
> new problem.*

> * It is impossible to steal someone else's solution to the problem and apply it to your block
> * It is impossible to create a new block without having the data of the previous block

In the context of Bitcoin, a resource-intensive problem is also called a *proof-of-work (PoW) problem* because the 
solution to it is the proof that the author of the block has done certain work which corresponds to the specified 
difficulty parameter.

> **Requirements for a PoW problem**
>> * There is an ability to set yourself a task independently 
>> * Solution verification must be performed very fast 
>> * Anyone within the network can verify the solution 
>> * Predicting the setting of a future problem is impossible 
>> * Task for the block is different for all participants 
>> * Difficulty of the task is the same for all participants 
>> * Prime cost of solving the task is proportional to the difficulty parameter

Based on the above information, it's fair to say that anyone can create the new block first; it is just that each one 
has a different probability, which primarily depends on the amount of computational power, which a participant owns.

Since each participant forms their version of the next block, then the task for each participant will be unique. 
However, the difficulty of this task will be the same for all. And given that the problem can only be solved with the 
brute force technique, the probability for each individual to solve it first will increase proportionately to the growth 
of the share of her computing power compared to all power involved in the network.

Mining is rewarded with bitcoins, which is actually a charge for maintaining the secure and reliable operation of a 
decentralized system. The principle is that participants who did provide the proof of the solution to a complex problem 
earn the right to confirm the transaction and be rewarded. The more there are independent and honest validators working 
on the network, the more reliable Bitcoin is [45].

As a result, such a simple mechanism for motivating people to mine creates a desire to earn more and thereby stimulates 
entrepreneurs to purchase and launch more mining power. They are competing for a reward, and, accordingly, the 
complexity of a resource-intensive problem increases [46].

### Frequency limitation of block creation
The protocol specifies a rule that an average time for block creation is 10 minutes. To ensure this, the difficulty 
parameter for block creation is recalculated every 2016 blocks (given that the creation of each block takes around 10 
minutes, the creation of 2016 blocks will take approximately 2 weeks). Therefore, if the network capacity, for some 
reason, increases sharply so that 2016 blocks are found much earlier than in 2 weeks, then the difficulty parameter 
increases in order to normalize the frequency of block creation—this is done to keep the average time of block creation 
close to the specified 10 minutes, which is fundamentally important for Bitcoin to decrease the number of orphan blocks 
appearing.

### Orphan blocks
A great number of validators working on the creation of a new block are dispersed across the globe [47]. And since the 
distribution of messages through data transmission channels is not instantaneous, there could be certain synchronization 
delays between the network nodes. Roughly speaking, nodes located on the same continent synchronize the states of their 
databases (newly created blocks) faster than those on the other continents, leading to a problem of latency. Therefore, 
there could be situations when local databases of nodes in different continents have different end states (a situation 
similar to the one we described in section 2.5 about the deception of Captain Jack Sparrow and his crew).

As you already know, each validator chooses the block that he considers correct (e. g., the one which has been received 
earlier than others), adds it to his database replica. Further, on the basis of the last added block, he starts creating 
a new block. The blocks with the same height compete with each other to become a continuation of the mainchain. Which of 
the chains “wins” depends on how much computational power has been spent on its creation. Eventually, the “winning” 
chain extends the mainchain and all the blocks that were in the alternative chain become *orphan*. *Orphan blocks* are 
valid blocks which, however, do not get to the mainchain. Transactions in orphan blocks are correct; they can be 
confirmed and added to a block (and, accordingly, to the mainchain) later.

Sometimes the appearance of orphan blocks causes undesired effects. One of them is that a validator that has created the 
block which later became an orphan block will lose his mining reward (Figure 4.18 shows the number of orphan blocks 
created for the period from March 2014 to July 2017 [48]).

![Figure 4.18 - Graph of orphan blocks creation frequency](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.18-orphan-blocks-freq.png)

Also, the occurrence of a large number of orphan blocks can potentially contribute to the possibility of a 51% attack. 
This is because the entire computing power of the network gets distributed between the alternative chains that compete 
against each other, resulting in a higher possibility for the bad actor to take control of more than half of the 
processing power. As we have mentioned in 2.5, the result of 51% attack may be a double spending (that is, an adversary 
may be able to spend the same coins more than once). Noteworthy, one of the purposes that the difficulty parameter in 
Bitcoin is recalculated every 2016 blocks is to equalize the powers of validators and thus reduce the number of orphan 
blocks appearing.

> **_NOTE:_** *Sometimes people confuse the following two concepts: orphan blocks and stale blocks. These terms are 
> actually different. Orphan blocks are correctly created blocks which happened to be in the abandoned chain. Stale 
> blocks are incomplete blocks which did not even get to the chain; creators of these blocks did not complete them due 
> to the fact that another validator created a correct block (was the first to solve a particular resource-intensive 
> problem).*

### Double-spending attack
We can indicate one attack against which any financial accounting system must be protected. It is the so-called 
*double-spending attack*. Its essence is that a bad actor spends the same money twice (or more). In the context of 
cryptocurrency, this means that a user sends the same coins to two (or more) different recipients so that they all 
accept the payments. Of course, there will not be twice as many coins, yet the sender can try to outwit recipients by 
making them believe that they already own these coins.

In the context of Bitcoin, there are several methods by which an adversary can potentially implement a double-spending 
attack. One and the most trivial is that a person could potentially find a vulnerability in the software of nodes that 
maintain the Bitcoin network that would technically allow him to double-spend his coins. The other possibility is when 
at least 51% of power engaged in mining belongs to one party (group, organization, etc.). In such a case, this party can 
easily censor transactions by using his majority of computational power.

As an example of a double-spending attack, imagine a user named Taras who wants to buy a computer in the E1 internet 
shop which costs 4 bitcoins, and also he wants to buy a phone in the E2 internet shop for the same price. The problem is 
that Taras only has 4 coins. Since he is a tricky guy, he decides to perform a double-spending attack. (Fig. 4.19). 

![Figure 4.19 - Creation of two transactions that spend the same coins](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.19-conflicting-txs.png)

So, Taras creates two transactions. In the first transaction, he sends coins to the wallet of E1, and in the second 
transaction, the same coins are sent to the wallet of E2. These transactions are obviously conflicting and cannot be 
confirmed simultaneously. Therefore, Taras propagates the first transaction to the network and keeps the second one in 
secret.

While E1 waits for the full confirmation of the first transaction, Taras quickly creates a block which includes his 
second, conflicting transaction (and, optionally, unconfirmed transactions of other users). To make the block with a 
conflicting transaction correct in terms of the Bitcoin protocol, Taras in this block refers to the state of the chain 
when he still had 4 coins.

Immediately after that, Taras starts mining in order to create blocks based on the one that contains a transaction for 
E2. His idea is to secretly build an alternative chain where the same 4 coins are sent to another internet shop, E2. 
Obviously, he will not publish this alternative chain on the network until the first internet shop, E1, accepts the 
payment for the computer (Fig. 4.20). 

![Figure 4.20 - One internet shop accepting the payment](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.20-acceptance-of-payment-by-the-shop.png)

When E1 internet shop makes sure that the payment is submitted (the transaction has received enough confirmations), it 
sends the computer to Taras. Without wasting time, Taras publishes his alternative chain. If this chain exceeds other 
validators' chains, all nodes will switch to it and will consider it is the main one (Fig. 4.21). This will only be 
possible if we assume that Taras controls more than half of the computing power in the Bitcoin network.

![Figure 4.21 - Taras double-spending his coins](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.21-doublespending.png)

According to this alternative chain, the 4 coins now belong to E2 internet shop, which will send a smartphone to Taras. 
The fact that the previous chain with the first transaction is now considered invalid indicates that E1 will no longer 
have Taras' original payment and hence will lose the money (Fig. 4.22). If this is the case then we can affirm that 
Taras had performed a 51% attack as a result of which he had double-spend his coins.

![Figure 4.22 - Result of performed double-spending attack](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.22-result-of-attack.png)

### Appearance of a specific equipment
Over time, it has become apparent that specialized equipment helps you mine most effectively [49]. In times of rapid 
growth in the coin price, mining turned out to be a profitable activity. People have been trying to acquire as much 
computing resources as possible in order to compete with each other for a reward while solving a particular PoW problem. 
It became clear that solving it on a central processor of a conventional computer is non-effective. So over the recent 
history of Bitcoin, we have seen other methods adopted.

> * CPU—central processing unit (2009–2010)
> * GPU—graphics processing unit (2010–2012)
> * FPGA—field-programmable gate array (2011–2013)
> * ASIC—application-specific integrated circuit (2012–present)

Later the idea came up to perform calculations on the GPU, as it would allow performing a search in several streams in 
parallel and with lower energy costs. To achieve this, the source code of a resource-intensive task was adapted for the 
GPU. Since then, the demand for more energy-efficient mining equipment has been progressively growing. As a result, 
companies have appeared who design and produce optimized chips (ASICs) for solving uniform tasks (Fig. 4.23). 

![Figure 4.23 - Mining equipment](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.23-mining-equip.jpg)

The first such chips began to appear in 2012. They significantly outperformed the processing power of regular computers 
(by thousand times). The financial benefit attracted many participants who aimed to increase their capacity (hash rate). 
Slowly, this trend began to resemble a race of capacities. The graph below (Fig. 4.24) shows how the hash rate in the 
Bitcoin network has been increasing over time [50].

![Figure 4.24 - How the Bitcoin hash rate changes over time](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.24-hashrate.png)

Given that the difficulty parameter in Bitcoin is permanently adjusted (Fig. 4.25) [50], all participants have to 
constantly improve their mining equipment to solve their tasks more energy-efficiently.

![Figure 4.25 - How difficulty parameter in Bitcoin changes over time](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.25-difficulty.png)

### Mining pools and their tasks
Let's suppose there are 1 million identical computers on the network. In this case, the probability of one computer to 
find the block would be 0.000001. Hence, the owner of the equipment with such power would create one block on average 
per 10 million minutes, which is slightly more than 19 years.

Apparently, no one would be satisfied with this doubtful investment cycle of 19 years. Therefore, people would combine 
their mining powers to solve one shared problem more quickly and then distribute the reward. In such a way, bitcoin 
miners started cooperating in mining pools.

*A mining pool is a group of mining equipment owners who work together on a shared problem and generally have one leader 
to represent the pool*. The leader is the one who creates the blocks and gives assignments to all participants in the 
group. If one of the participants finds a solution, the reward is distributed among all of them proportional to the 
power of particular engaged equipment. Hence, pool members receive a reward of a smaller size but more frequently than 
if they worked individually (Fig. 4.26).

![Figure 4.26 - Comparing solo mining and pool mining according to frequency of block creation and the received reward](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.26-comparing-solo-and-pool-mining.png)

In this approach, we should note one important detail: none of the participants of the mining pool decide which 
transactions to include in the block—all they do is solve the resource-intensive task. So the only thing a participant 
can do if she has noticed that the leader confirms transactions suspiciously (or you could say subjectively) is to 
connect to another mining pool, which in her opinion is more honest and independent, and further work with the latter 
pool. Another risk for the pool participants is that their leader may simply omit paying them their share of rewards.

The two diagrams below show the distribution of computational power in the Bitcoin network among validators [51]. Some 
validators revealed their identities, and some did neither (“Unknown”). In the ideal case regarding *decentralization* 
and *independence of decision-making*, validators should remain anonymous (this cannot be considered a guaranteed 
recipe, however; below we will explain why). Let's analyze how the situation changed for a short time period.

As you can see from the diagram (Fig. 4.27), the total processing power of anonymous validators in practice does not 
exceed 1% (the provided data is relevant as of the end of August 2018). Most of the blocks are created by publicly known 
node-validators, which lead the mining pools. You can also see that pools such as BTC.com, AntPool, SlushPool, and BTC.
TOP accumulate more than half of the total processing power. Although there have been no conspiracy issues to date, the 
current situation is not really a perfect example of applied decentralization principles.

![Figure 4.27 - Distribution of mining capacities in the Bitcoin network as of October, 2018](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.27-distributing-the-mining-capacities.png)

Technically, together they could perform a 51% attack, but if the attack happened, it would immediately be visible on 
the network. This would very likely outrage the community, and the participants of these pools would turn to another 
group with another leader. The pools would lose their reputation, clients, and business.

An interesting fact is that at the beginning of December 2018 (Fig. 4.28), the share of anonymous validators grew from 
less than 1% to 22.6%. On the one hand, such a situation may seem favorable from the perspective of a decentralized 
system. But, having analyzed the possible threats, you realize that the fact that validators are anonymous does not 
necessarily mean that they are independent of each other. So, technically, it is possible that they form into anonymous 
pools, which are hard to deanonymize and could conduct a double-spending attack after having reached some threshold 
size. Such a pool could be owned by both a group of validators or even one person. 

![Figure 4.28 - Distribution of mining capacities in the Bitcoin network as of December, 2018](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.28-distributing-th-mining-cap-december.png)

### Mining statistics and estimation of energy consumption
In April 2018, electricity consumed by mining was estimated at 110 GW per day, while the total daily profit of the 
entrepreneurs involved in mining was $32 million [52; 53]. The percentage of fees from the total profit is 21%, that is, 
about one fifth. The fee for an average transaction during the previous year ranged from $0.1 to $40. The size of all 
data in the Bitcoin blockchain currently exceeds 175 GiB.

As for energy consumption, as of August 31, 2018, the estimated figure for electricity consumed by Bitcoin mining was 
200.3 GW per day, that is, 73.1 TW per year.

To put this into perspective, let's see how much electricity the most powerful plant in the world (as considered in 
August 2018) can produce. Through 2017, the indicator of produced electricity on the Chinese Three Gorges Dam [113] 
showed 97.6 TW. The graph in Figure 4.29 is no less vivid. It shows the energy consumption indicators of Bitcoin as 
compared to some countries. As you can see, the total energy required to power Bitcoin exceeds the national needs of the 
Czech Republic, Chile, or Austria [51].

![Figure 4.29 - Energy consumption by different states as compared to that by Bitcoin](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.29-energy-consumption.png)

Based on this data, it has been calculated that a confirmation of a single transaction in Bitcoin consumes about 921 kW 
per day. Approximately the same amount is needed to supply 30 average US houses for one day. On the graph in Figure 
4.30, you can see the comparison of electricity consumed for 1 transaction in Bitcoin against 100,000 transactions in 
Visa. Based on the data from the graph, it is possible to count that 1 transaction in Bitcoin costs more than 500,000 
Visa transactions.

![Figure 4.30 - Comparison of energy consumption by Bitcoin and Visa transactions](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.30-bitcoin-vs-visa.png)

In fact, the energy consumption in Bitcoin does not always increase. If you look at how indicator changed from August to 
December 2018 [54], you will see how sharply it decreased since mid-November to 53.04 TW per year (Fig. 4.31).

![Figure 4.31 - Energy consumption in Bitcoin](/resources/img/chapter-1/4.2-mining-in-bitcoin/4.31-energy-in-bitcoin.png)

Despite the seemingly wasteful energy consumption in Bitcoin, it could be said though that this cost does not come from 
nowhere. Essentially, this is the price we pay for one of Bitcoin's primary properties—*protection against transaction 
censorship*. At the same time, there is the community that permanently supports and develops the protocol and offers new 
improvements and ways to optimize the operation of the Bitcoin network.

**Common myths**

*Mining will terminate in the year 2140.*

After 2140 mining will not be suspended and the validators will continue receiving rewards for block creation. However, 
after 2140, the reward will consist of transaction fees exclusively, without the newly issued coins.

*The price of bitcoin entirely depends on the cost of mining it.*

In fact, there is no direct correlation between the price of a coin and the mining costs by the same reason the price of 
gold in USD/XAU is not directly determined by the cost of physically mining gold. Yet, there is an indirect 
relationship: if the bitcoin price increases, then there may be more willing to mine it. Equally, if the cost of 
electricity increases globally, there should be a knock-on effect on the price of bitcoin.

*If you buy a fair amount of equipment, you can mine all the coins that will ever be issued in the network much earlier 
than the year 2140.*

If you use more computing power, you can increase your share of the reward, but it is impossible to get more coins than 
is determined by the protocol. The rate of issuance does not depend on the current computational power of the network.

*The difficulty of mining always increases.*

Actually, it isn’t. It can decrease as well. The difficulty of mining has been growing because bitcoins have been 
becoming more expensive, which led to an increase in the competition for the reward. If, however, the reward in the 
dollar equivalent is reduced, then there will be fewer people willing to mine. It's even possible that some part of the 
current operating equipment will have to be switched off because the reward will not be enough to pay for the 
electricity and other maintenance expenses.

*If you mine with a supercomputer, you always create the new blocks first.*

The most powerful supercomputer in the world will not equal even one percent of the total processing power of the 
Bitcoin network. The reason is very simple—supercomputers use conventional processors of general purpose, which are not 
designed for a PoW problem in Bitcoin. Therefore, companies like Google or IBM cannot use their data centers to 
interfere with, or censor confirmed transactions.

**Frequently asked questions**

*– Is it possible to rewrite the existing blocks if you are able to compute the hash value faster than others?*

It is possible in theory, but there are certain peculiarities. It is necessary to consider the percentage of available 
computing power relative to the power of all the others in the network, costs of maintaining the equipment, and the 
period of time needed. Therefore, you initially have to calculate the economic feasibility of rewriting the history of 
the last blocks since the key factor in mining is energy efficiency. In theory and in practice, the cost of an attack 
which attempts to rewrite the existing blocks exceeds the potential gain.

*– How has the date when the last bitcoins are mined been calculated if the frequency of block creation is variable?*

The frequency of block creation can vary significantly but not for long periods of time. Generally, over the course of 
Bitcoin history, block creation time is almost the same. According to the protocol rules, the network strives to ensure 
that the average block creation period is 10 minutes. This is achieved by means of adjustment of the difficulty 
parameter of a PoW problem.

*– Why is there so little competition in the production of computers for mining bitcoins?*

In 2018, there are 5 major companies designing mining equipment, and 2 large companies producing specialized chips. This 
production is rather complicated because you need investment (actually, millions of dollars) for the development of the 
chip and its production. The production cycle can take more than a year, while the industry is high-risk since these 
chips can only be sold to bitcoin miners, so understandably fresh entrants are reluctant to enter the equipment 
industry. Nevertheless, the competition among the mining hardware producers is after all present.

*–What is the difference between a miner and a validator in Bitcoin?*

Some people confuse these terms. Strictly speaking, a validator is a role of a system participant, that is, an 
entrepreneur who configures his equipment and maintains its correct operation. A miner is what solves a 
resource-intensive problem, that is, the equipment itself.

## 4.3 How is blockchain implemented in Bitcoin?
The approach to storing data in an interconnected way is hardly new.  Even so, Bitcoin has revolutionized how to 
organize a database of an entire accounting system. In fact, it didn't happen as a result of some scientific 
developments but rather as an attempt to respond to the security threats that can be caused by the participants of the 
anonymous distributed network. Security of existing systems is often based on organizational approaches to protecting 
information and resolving conflicts. Maintaining an accounting system has only been possible in a non-anonymous 
environment where you have predetermined parties, such as courts, police, etc. that regulate conflicting situations. 
Satoshi, like some other engineers, attempted to create a *financial accounting system* in which everyone would be equal 
and could maintain privacy. In these circumstances, it is not possible to resort to one arbiter, for example, court 
assistance (depending on the situation and the user's goals, this can be an advantage or a disadvantage).

An anonymous distributed network which is, by the way, resistant to censorship requires a completely different approach 
to solving this problem. In the real world, many undesirable things (crimes) are, generally, not prone to happen since 
there is a risk they will be disclosed to publicity, leading to a reputation loss and even imprisonment. Therefore, 
there is a lack of protection from many forms of attack—a society in which laws work effectively simply does not need 
this protection. For example, in the US, if you lose your bank card or a PIN-code, the bank may return any stolen money 
to you because generally, it is all insured. Even if a few percent of banks' profits are spent on insurance, it is still 
cheaper than providing a straightforward protection against all possible fraud scenarios. In the case of Bitcoin, there 
is no one to solve most conflicts. Therefore, it is designed in a way to *avoid the very essence of conflicts* without a 
need to create mechanisms for dealing with them. In Bitcoin, it is impossible to send non-existent coins (however, there 
is a threat of a so-called successful double-spending attack [55]). The study of this section will provide answers to 
questions of how and why blockchain technology was implemented in Bitcoin.

First of all, let's define the concept of *blockchain* in the context of Bitcoin. It is a database that contains 
transactions and which is common for all nodes involved in the Bitcoin system. The peculiarity is that each next block 
confirms the integrity of the previous block, which in its turn confirms the integrity of the previous one, and so on up 
to the genesis block; by this means, a one-way communication of blocks is furnished (i.e., the fact that a particular 
block is created after the previous one). This method of organizing data ensures that each block is only created under 
the recognition of the entire transaction history since Bitcoin has existed.

Now let's slightly enlarge the idea of data structure in the chain of blocks (Fig. 4.32). Recall that each block 
consists of two parts: the block header and the included transactions.

![Figure 4.32 - Structure of the chain of blocks](/resources/img/chapter-1/4.3-how-is-blockchain-implemented-in-bitcoin/4.32-structure-of-the-chain-of-blocks.png)

*A genesis block is the first block created in the blockchain, after which all parties may create subsequent blocks*. 
Its peculiarity is that it is not propagated when nodes are synchronized with each other because it is built in the 
software of the network node and has an ordinal number 0 [56].

To conduct a base verification of the system, it is sufficient for the nodes to only download the block headers; this 
already allows them to check the creation time of the block, the hash value of the previous block, and how the 
complexity parameter changes over time. However, to carry out a more profound verification, such as on double-spending, 
a node would need to download the body of the block with transactions, in other words, the entire block.

### Block structure
Let's take a detailed look at the structure of blocks that are transmitted over the Bitcoin network. The corresponding 
data order is shown in Table 4.1.

Table 4.1 — Block structure in Bitcoin

| field        | value                                         | size      |
|--------------|-----------------------------------------------|-----------|
| MagicNo      | 0xD9B4BEF9                                    | 4 Bytes   |
| BlockSize    | number of bytes following up to the end block | 4 Bytes   |
| BlockHeader  | consists of 6 items                           | 80 Bytes  |
| TxCounter    | positive integer                              | 1-9 Bytes |
| Transactions | the (non empty) list of transactions          | N/A       |

The structure of a block provides several fields. The first one, *magicNo*, is a special constant number. For the 
Bitcoin protocol, it always has this value and takes 4 bytes of space; it is used to identify the data stream. If, for 
example, there is a data transfer channel with data passing from different protocols, then it is specifically this value 
by which you can search and identify that the Bitcoin block started at a certain time (for different protocols, this 
value is different).

After that, there's the field *blockSize*, which also uses 4 bytes and indicates the number of bytes contained in this 
block, including the transaction data. It is followed by the block header, which consists of six fields and is always 
equal to 80 bytes.

Then goes *txCounter* (the transaction counter). The number of transactions in the block can be so large that the 
transaction counter can have a size of 1 to 9 bytes. After that, there is the transaction data—the size is not defined. 
In practice, a block can be 100 bytes or even up to 1 MB: the number of transactions contained in the block can also 
vary.

The key component of the block is its header. The field *blockHeader* contains 6 fields. They are verified by all nodes 
of the network including lightweight clients. Verification of each field is performed according to strictly defined 
rules, the primary of which are unlikely to be changed in future. The characteristics of all fields are presented in 
Table 4.2.

One of them is called *nonce* (an arbitrary number which can only be used once), which contains the number of attempts 
of solving a resource-intensive problem. There is also the difficulty parameter, which denotes the mining difficulty and 
is placed in the field bits (is present in a block header). It changes approximately once every two weeks (time period 
for which 2016 blocks are created).

Table 4.2 — Structure of a block header in Bitcoin

| field          | size                                     | updated value                                           | size     |
|----------------|------------------------------------------|---------------------------------------------------------|----------|
| Version        | Block version number                     | You upgrade the software and it specifies a new version | 4 Bytes  |
| HashPrevBlock  | 256-bit hash of the previous blockheader | With the generation of each new block                   | 32 Bytes |
| HashMerkleRoor | 256-bit hash of txes                     | A transaction is accepted                               | 32 Bytes |
| Time           | Current timestamp                        | Every few seconds                                       | 4 Bytes  |
| Bits           | Current target in the compat format      | The difficulty is adjusted                              | 4 Bytes  |
| Nonce          | 32-bit number (starts at 0)              | Increments after each hash is checked                   | 4 Bytes  |

So, the first field contains the block *version*, which uses 4 bytes. It corresponds to the version of the protocol on 
which a validator (the creator of the block) has been working. Next comes *hashPrevBlock*, that is the hash value from 
the header of the previous block with the length of 256 bits. Note that this hash value was obtained through double 
hashing, using the SHA-2 hash function. This field contains 32 bytes of data. Below goes the specially-taken hash value 
(Merkle tree) from all transactions in the block, which also takes 32 bytes. Then comes the *timestamp* (the Unix 
timestamp), which is usually set equal to the block creation time; it takes 4 bytes. After that, there is a compressed 
complexity parameter, so-called *bits*; it also takes 4 bytes. The last parameter is *nonce*, which is the solution to 
the PoW task for a particular block. It also has a size of 4 bytes. As a result, the block header in Bitcoin always 
takes 80 bytes.

Thus, we have observed the block structure and figured out that blocks can be of two types: the genesis block, which is 
the zero block in the chain, and all the subsequent blocks, which are downloaded and processed by the software of the 
network node.

### Examples of blocks in Bitcoin
Let's examine some particular Bitcoin blocks. Table 4.3 shows the Bitcoin genesis block [56]. Its peculiarity is that 
the value of the previous block is zero, meaning that all 256 bits are zero.

Table 4.3 — Genesis block in Bitcoin

| Version             | 01000000                                                         |
|---------------------|------------------------------------------------------------------|
| Previous block hash | 0000000000000000000000000000000000000000000000000000000000000000 |
| MerkleRoot          | 4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b |
| Timestamp           | 1231006505                                                       |
| Bits                | 486604799                                                        |
| Nonce               | 2083236893                                                       |
| Transaction count   | 1                                                                |
| -                   | Coinbase transaction (reward)                                    |

The genesis block was created on January 3, 2009. The difficulty parameter, which was installed initially, was 
calculated by Satoshi in such a way that the complexity of mining corresponds to the capabilities of a conventional 
desktop computer. Genesis block, similar to all the subsequent blocks, was created through mining, namely through 
finding the prototype of a certain type of hash function. This block contains only one transaction in which Satoshi, 
according to the protocol rules, sent the first 50 bitcoins to himself. In fact, the hash value of all transactions of 
this block (Merkle root) is the hash value of the coinbase transaction.

Table 4.4 shows another block, which refers to the previous one that has already been appended to the shared Bitcoin 
database. By contrast to the above-mentioned example, the hash value of the previous block is set. It contains 2702 
transactions (other blocks can contain far more transactions). Here the version is different, and a corresponding 
timestamp is present as well as an increased complexity parameter and a new nonce value.

Table 4.4 — Example of a block in Bitcoin

| Version             | 613687296                                                        |
|---------------------|------------------------------------------------------------------|
| Previous block hash | 00000000000000000007c0fb40d7d6225edaa5da8f43490cc56e60c34082e39b |
| MerkleRoot          | 167167cde7164fe0005a71e8b75a8da1e760deb546a944d67d7e3f15dcb60d45 |
| Timestamp           | 1671542973                                                       |
| Bits                | 386397584                                                        |
| Nonce               | 971790600                                                        |
| Transaction count   | 2702                                                             |
| -                   | ...                                                              |

Let's consider timestamps in more detail. Each block header contains a timestamp according to the Unix timestamp 
standard. The Unix timestamp is the time representation format which shows a number of seconds since midnight on January 
1, 1970 (00:00:00 UTC).

Bitcoin has its own rules to verify timestamps. Those rules particularly describe the limits for a block timestamp for 
block verification. The lower bound is calculated as the median of the last eleven blocks that were found before this 
block. The upper limit is calculated as follows: when the node receives a new block, the software verifies that the 
timestamp is less than the median value of a current time on all nodes connected to this one, plus 2 hours.

Therefore, there is a fairly wide window for the timestamp of each new block loaded by the network node. This allows 
avoiding conflicts in the transaction history in case there are time lags between different machines and thus ensures a 
specific order of accepted correct blocks. Theoretically, the timestamp of a previous block can outrun that of the next 
one, but in small ranges this is acceptable. Bitcoin uses these stamps only for long periods of time to update the 
mining difficulty parameter—in small intervals, timestamping does not play a particular role.

*A coinbase transaction is a transaction in the block with an ordinal number zero*; it is present in every block. In 
this transaction, the validator, who found the solution and created a new block, specifies a reward for her work and 
assigns fees from all the transactions that she has included in this block.

A сoinbase transaction does not have inputs (they are filled with other values), while the outputs only contain the 
rewards for a validator. Bitcoin uses the coinbase maturity parameter, which determines the number of confirmations that 
a coinbase transaction must receive to allow its creator to spend the earned coins. Satoshi set the value of this 
parameter at 100.

### Concept of mempool
*Mempool* is another key concept in the Bitcoin protocol. This is a separate module contained in each full network node 
that stores and processes unconfirmed transactions. Each Bitcoin network node has its own mempool, where it stores the 
queue of transactions that it has checked and considers valid. Broadly speaking, mempool is an organized queue 
(Fig. 4.33), where transactions are stored and sorted before being added to the newly created blocks.

![Figure 4.33 - Operation of mempool](/resources/img/chapter-1/4.3-how-is-blockchain-implemented-in-bitcoin/4.33-operation-of-mempool.png)

Mempool directly displays the network capacity status (the larger the queue of transactions is in the mempool, the less 
effective the network capacity happens to be). During 2017, the average load of the queue was 30,000 transactions with 
an approximate size of about 40 MB. This is quite a number if you consider that a block of 1 MB that confirms 1500 
transactions (for further detail, see section 4.7) is created every 10 minutes.

### Lifecycle of a block
> * Formation
> * Creation
> * Propagation
> * Verification
> * Connection
> * Disconnection

*Block formation*. A validator selects transactions from the mempool, calculates the MerkleRoot value, sets the hash 
value of the previous block, specifies the timestamp, transaction counter, and the block size, then units this data and 
forms a block.

*Block creation*. The process of block creation essentially is mining and is described in detail in 4.2.

*Block propagation*. After a new block is created by a node-validator, it is propagated over the Bitcoin network. The 
creator of a block transmits it to the nearby nodes to which it is directly connected. When receiving a block, every 
node verifies if a block matches the protocol rules. If the block received is valid (does not contradict Bitcoin 
protocol rules), a node adds the block to its own database and propagates it further over the network. Also, this node 
stops creating its own block at the same height, deletes conflicting transactions, transmits the rest transactions back 
to the mempool, and then starts forming the next block based on the one it has just propagated over the network.

Figure 4.34 shows how a block is propagated over the network. This process is not instantaneous; this is why the state 
of the database seems unclear at a particular time point when some nodes have not yet received the last block created.

![Figure 4.34 - Block propagation over the network](/resources/img/chapter-1/4.3-how-is-blockchain-implemented-in-bitcoin/4.34-block-propagation.png)

*Block verification*. In order to verify that a particular coin is not being double-spent, there is a separate database 
used along with the main one and maintained by each Bitcoin network node. It is called coins database. It stores the 
current state of all unspent outputs, meaning that within all existing transactions, there are a lot of outputs: some of 
them have already been spent, and some have not. Outputs that have not been spent are stored and indexed by the software 
separately. When a new block is received—which has been previously verified: that the structure of the block is the 
same, the complexity of mining is correct, the task on PoW is solved, the digital signatures are correct, etc. (in fact, 
there are many more checks but these are the main ones)—each network node, when adding the block to its local copy of 
the database, verifies each transaction to ensure that it spends existing coins from this coins database. Hence the 
protection against double-spending is guaranteed. Also, the overall process is optimized: there is no need to access the 
input of each transaction, monitor a transaction that is being spent and check it for double-spending. Instead, 
unpublished coins are indexed, speeding up the process of checking new blocks.

*Block connection*. This is an important process which defines how blocks are verified and stored on each node in its 
local copy of the chain of blocks. For this, a node chooses the block (among those it has received and considers 
correct) as well as the corresponding chain and then appends it to its local copy of the database.

*Block disconnection*. The process opposite to connecting blocks is essentially the disconnection of blocks. Let's 
consider the situation when this can happen.

When the node-validator refers to the block (it is indicated by the arrow in Figure 4.35), the current state of its 
database corresponds to this block; meanwhile, the node-validator works on creating the next block. Let's suppose that 
for some reason (there could be many), an alternative block is published on the network. After a while, another block 
appears, which is created on the basis of this alternative block (this branch is shown in the figure below). When it 
becomes known which of the chains is longer, then, according to the protocol rules, node-validators have to detach from 
the shorter chain and switch to the longest one. However, the organization of this process is not as simple as it might 
seem at first glance.

![Figure 4.35 - Switching to alternative chain of blocks](/resources/img/chapter-1/4.3-how-is-blockchain-implemented-in-bitcoin/4.35-switching-between-blockchains.png)

There is a process called *reorganization*. It allows users to cancel a particular version of event history and switch 
to another one. How does it work? Reorganization includes the block disconnection process, which exposes some key 
complexity. The network node should return all the coins spent in the disconnected block and again label them as unspent 
(i.e., return them to its coins database). In addition, all the transactions that have been confirmed in this block have 
to be returned to the mempool to regain the status of unconfirmed transactions. Only after that is it possible to 
connect the subsequent block, where the same transactions will be confirmed and the same coins spent. All network nodes 
are liable to perform this process strictly to have the final state of their local copy of the database identical to 
that of other honest nodes.

### Initial node synchronization
We have already considered the concept of a block in Bitcoin, and how a number of such data structures eventually form 
into an entire chain, which is synchronized among independent participants. Now, it would be interesting to define how 
actually this database “looks” (how it is stored and organized) on the local users’ computers.

Among different wallet software developers, the method for organizing the chain of blocks can also differ. For instance, 
the Bitcoin Core developers use LevelDB database, which supports key–value relations for structuring data. Also, this 
depends on your file system: if you use Windows OS, then your chain of blocks would be located in "%APPDATA%\bitcoin"; 
correspondingly, it is "~/.bitcoin" if you use Linux OS, and "~/Library/Application Support/Bitcoin" for the Mac OS.

Yet, whatever is the software, the basic requirement is always the same: reaching consensus regarding the final database 
state between the network users. The most important is that the transaction in which Alice sends Bob, for example, 7 
bitcoins would be processed correctly on all computers regardless of their particular implementation of the chain of 
blocks.
Imagine that a user has decided to launch a Bitcoin network node. Apparently, before having the database stored on her 
node, she has to download the entire chain of blocks and make sure that all unspent outputs are up to date. This process 
is called initial node synchronization. During the synchronization, a user is exposed to certain risks—a malicious 
person can exploit certain vulnerabilities (such as those related to the network connection) and conduct a number of 
attacks.

One such attack is the spamming by an attacker's chain. In the process of downloading the chain of blocks, you can 
potentially have an alternative version of the database imposed on you. It is possible that in this version, an attacker 
has all your coins. What is noteworthy is that your software may not detect such deception because this version—just 
like the honest participants' chain, which you haven’t downloaded—is nevertheless correct in terms of the protocol.

The question is, how to prevent situations when your node gets isolated from other honest nodes of the network? How is 
it possible to guarantee that what you download is actually a long-established and correct chain?

In the Bitcoin protocol, there is a special checkpoints mechanism, which acts as a security measure against similar 
situations.

### Checkpoints
*Checkpoints is a mechanism that represents matching of a block height to a correct hash value of the block at the same 
height*. This matching is used to verify the correctness of blocks which a certain node receives from other nodes during 
the initial synchronization with the network.

These checkpoints are already in the core of the software of the node. During the initial synchronization, the node 
checks the match between the received hash value and the checkpoint values which are built into its software.

In Figure 4.36, you can see that among all the blocks that a node downloads, only some of them are checked [57]. If the 
values match, the software continues to function normally; if not, a user gets notified. This notification indicates 
that either the checkpoint is incorrect or that a user is imposed with an incorrect transaction history.

![Figure 4.36 - Correspondence of checkpoints to blocks at particular height](/resources/img/chapter-1/4.3-how-is-blockchain-implemented-in-bitcoin/4.36-checkpoints.png)

The author of these checkpoints is the community, namely, the software developers. Due to this, you have to trust that 
they have selected hash values from correct blocks (that received enough confirmations) and that the probability of this 
history being later rewritten in a way predetermined in the protocol is extremely low.

Checkpoints cannot be linked to the blocks that have not yet been created as well as to those that are recently 
published—the probability of rewritten history and a subsequent creation of an alternative chain is much higher with new 
blocks. Since the chain of blocks permanently grows, each new version of the software contains more checkpoints.

### Features of the shared Bitcoin database
Given the fact that Bitcoin implements a decentralized accounting system supported by tens of thousands of independent 
nodes, its database has acquired truly valuable properties.

> * Ability to verify the integrity of transactions
> * Synchronization and backup in real time
> * Ability to perform an audit in real time
> * Cooperative decision-making regarding the transactions
> * Transparency of accounting
> * Trustlessness (minimum level of required trust)
> * Immutability

A database that features *integrity* and *availability* of data is extremely valuable for applications that need to work 
with guaranteed data (timestamping, adding hash values of important documents, and so on). This is driven by the 
necessity of having indisputable proof that a particular data was added at a specified time interval, not later or 
earlier.

There is indeed a great demand for utilizing the idea of a shared database (i.e., the database which is stored by 
multiple participants): it is extremely difficult to completely destroy the database or backdate the data in it since it 
is stored on a large number of computers dispersed across the globe. We have already described the technical aspects of 
writing arbitrary data to the Bitcoin blockchain in section 4.1.

Correct usage of blockchain technology may help to achieve the following properties.

> * Decentralization of the decision-making process 
> * Higher fault tolerance of a system
> * Complicating fraud
> * Higher level of user trust towards the system
> * Immutability and irreversibility of transactions

These factors are the main reason why people started talking about blockchain separated from cryptocurrency. There were 
a lot of speculations about whether blockchain technology is applicable beyond Bitcoin. We will cover this topic 
extensively in 5.1 and 5.2.

**Common myths**

*If there have been no transactions in the network, the next block will not be created.*

Indeed, such an opinion is quite widespread. The question is, does mining make sense when there are no transactions in 
the network? Yes, it makes. A new block will be created because there is still a reward for this. Every block contains 
at least one transaction, a coinbase transaction. Users can either vote for certain existing transactions or even for 
their absence. Regardless of what a user votes for, her vote is always accounted. A validator can create a block and 
receive a reward while not adding any transactions to it (except for a coinbase one).

*If a validator does not have a hardware powerful enough, then he will create blocks containing only a few 
transactions.*

It is impossible. Regardless of the capacity of equipment, a validator can select any transactions for confirmation. To 
create a block, you need to solve the problem of finding the preimage of a hash value. This task will be formed 
considering the complexity parameter, which is the same for all network participants. Therefore, the probability of 
creating a block using specific equipment is proportional to its share in the total processing power of the network and 
does not depend on the block size.

**Frequently asked questions**

*– What is the difference between block version 1 and version 2?*

During one of the updates of the Bitcoin protocol, the block version was incremented by one. In this version, rules 
concerning the formation of versioned bytes in block headers are different, and they are imposed in accordance with 
BIP9 [58].

*– Can a validator in the coinbase transaction indicate the amount of reward greater than it is determined by the 
protocol? Will such a block be adopted by the network?*

Yes, a validator can specify this. He can even create such a block by solving the PoW task and distribute it over his 
network connections. However, the block will immediately be verified by other nodes that will detect the protocol 
mismatch. This block will be discarded by honest participants and will not be spread further through the network.

*– When you launch a new network node, how do you know the network addresses of other nodes to which to connect for the 
initial synchronization?*

You need to know the addresses of nodes that you trust. It could be your friends or certain trusted services. When 
launching a new node, you will need to open a configuration file and manually enter these addresses. Also, you can use 
the so-called Bitcoin seeds. These are addresses of Bitcoin Core developers, which are included in the software of the 
node by default.

*– What is the maximum number of entries in the coins database?*

There is no limit on the maximum number of records in the coins database. Otherwise, transactions in which the number of 
outputs exceeds the number of inputs would not even get into the mempool, not to mention to the mainchain.

*– Is it true that the coins database grows as transactions get confirmed?*

It grows and decreases at the same time because new unspent outputs are created and so are also spent. With an 
increasing number of users, this database should increase. The more there are users, the more unspent outputs each has, 
and the greater is the database. Yet, it has no limits.

*– Do full nodes have the same level of influence on the network state as node-validators?*

Full nodes that do not participate in transaction confirmation do not affect the state of the network. They can verify 
transactions for their own purposes or transmit data to other users. However, the actual effect on the final state of 
the shared database depends only on validator-nodes.

*– What is the story behind the appearance of Bitcoin blocks with size exceeding 1 MB?*

In August 2017, the protocol update was released that allowed separating proofs of coin ownership from the main 
transaction data. It is called Segregated Witness. The maximum base size of the block remained upper-limited to 1 MB, 
but the proof-adjusted size can exceed this value and be up to 4 MB (see section 4.6).

## 4.4 Approaches to network synchronization and SPV nodes

Security in Bitcoin is mostly based on the fact that transaction history is transparent. Anyone can see it and ensure 
its reliability. In fact, ensuring reliable storage and authenticity of data under such conditions is both an ambitious 
and complex task. To achieve this goal, it is necessary to have truly independent modules that verify each other's work. 
The interaction of user software with other nodes on the network is a vital function, which we will consider in this 
section.

Earlier we noted that nodes in the Bitcoin network are constantly verifying the compliance of their local database copy 
with that of other nodes of the network. In case these versions do not comply, each node makes decisions independently. 
This approach allows each user to be certain as to the relevance and the storage reliability of data he receives. 
Despite this process appearing quite simple at first glance, it is actually complicated and has many sophisticated 
factors, which have to be considered before you start synchronization with the network nodes. It is at precisely this 
stage that malicious actors can exploit the window and exclude users from a shared network by broadcasting, for example, 
fake transactions [59]. Below, we will explore primary methods of wallet synchronization with the Bitcoin network and 
respective features of the methods.

If you recall the basic functions of a digital wallet and the way it is organized, it is easy to see how often the 
wallet needs to communicate with the network in order to synchronize current data. 

> **Basic functionality of a digital wallet**
>> * Backing up private keys
>> * Receiving payments
>> * Displaying the balance 
>> * Displaying the transaction history
>> * Sending payments

The first functionality (backing up private keys) does not require a connection with the Bitcoin network—all that you 
need is a module responsible for storing and managing keys. Conversely, to perform all other functions, your wallet 
needs to interact with the network. To access a shared database of transactions and have the ability to change it, you 
must be one of the nodes of the Bitcoin network and follow its processing and communication specifications (protocol).

To function properly, a wallet needs to have data for all the transactions in which its addresses have been involved. 
Recall that transaction inputs are immediately linked to outputs of the previous transaction; therefore, it is important 
that coins from those outputs can be spent for sure. It is extremely abnormal to initiate a new transaction and make 
payments without first knowing the state of confirmation of the old ones. The most important information you should know 
at the time of receiving and sending payments is the status of your transactions. If a transaction has not received a 
sufficient number of confirmations, a recipient cannot consider the payment fully confirmed and, for example, release 
the goods to a buyer with no risk to be cheated.

### Challenges of working in a distributed network

Communication with a distributed network is a fairly complex topic, and there are several inherent problems that usually 
occur. As noted above, all nodes on the network regularly check their state of the database compared to others and 
synchronize it in cases of non-compliance. There are two problem to determine. Firstly, how to check the data received 
from the distributed network? Secondly, how to optimize the storage of this data? The difficulty stems from the fact 
that these requirements somehow conflict with each other. To complicate matters further, there is no perfect solution 
because it is impossible to verify transactions as reliable as possible and at the same time use as less resources as 
possible for the problems. The more complete data a user has, the more authentic verification result she can get. 
Therefore we have to strike a balance in the trade-off between simplicity and security.

There is a further problem: if the owner of the full network node needs to get the transaction history at a specific 
address and check it, then he will have to go through the entire chain of blocks (from genesis to the last known block), 
which is quite time-consuming. Brute force is ineffective because the case is about a large amount of data. To optimize 
the queries, there are alternative approaches. For example, network nodes apply special add-on modules. This, in turn, 
helps index the data of all the blocks and cache some of them to speed up the search for popular metrics. Any blockchain 
explorer works by this principle, and even those that implement public interfaces for general use can quickly return the 
data about almost any block, address or transaction, etc.

Let’s now consider the several and fundamentally different approaches to the interaction and synchronization of a 
digital wallet with a distributed network.

### Approaches to synchronizing wallets with the payment network
There are three main approaches to synchronizing a wallet with the payment network (Fig. 4.37). The first assumes that 
the wallet itself is a full node of the Bitcoin network. The second uses a so-called trusted node (usually, this is 
someone else's network node that a user of the wallet trusts). The third approach, Simplified Payment Verification 
(SPV), involves direct interaction with the rest of the network nodes but in a simplified manner. It allows verifying 
the validity of transactions with a sufficiently high degree of reliability yet without resorting to launching a full 
network node. Let’s take a more detailed look at each of these approaches.

![Figure 4.37 - Approaches to wallet synchronization with the network](/resources/img/chapter-1/4.4-approaches-to-network-synchronization-and-spv-nodes/4.37-approaches-for-wallet-synch.png)

### Working with full nodes
In this case, everything is simple enough. A wallet implements the mechanisms for storing and processing the entire 
chain of blocks including the module for the network interaction (p2p messaging) with other nodes of the Bitcoin 
network. A user can always check the new incoming transaction to verify whether it complies with the rules of the 
protocol or if it is a double spend case, and so on. The owner of a full node can perform a detailed verification of all 
data.

However, to store the full and constantly increasing history of all transactions, you have to have a large amount of 
disk space, which is rather a disadvantage. In addition, a full node is liable to support the uninterrupted network 
operation. Therefore, it must constantly synchronize with the rest of the nodes and update the software when required.

Users who accept and send high-value payments require a higher level of reliability (authenticity of the transaction 
statuses), and this approach would be more desirable for them. In addition, all major services (wallet services, large 
merchants, etc.) that provide the ability to store bitcoins, use this approach inevitably due to their high requirements 
for security and independence when working with Bitcoin.

Thus, maintaining a full node (or even several full nodes with alternative implementations of the protocol) is the most 
reliable approach to synchronization with other nodes of the network. Bitcoin community aims to maintain the 
availability of this approach for ordinary users who run regular PCs: all that is required is to take ready binary files 
(for example, the ones of Bitcoin Core) and run them. Experienced users can take advantage of the source code, which is 
publicly available. Essentially, the only disadvantage is a high disk space requirement.

### Working with trusted nodes

The difference between this approach and the previous one is that the wallet logic and transaction verification logic 
are separated. This approach involves a trusted node, usually a common full node that users trust to verify their 
transactions. In particular, it can be a network node maintained by a users' friend.

Quite often this approach is used in mobile applications of digital wallets. For instance, a company that develops such 
applications supports trusted nodes and promises that it will ensure the correctness of transaction verification. In 
this case, users who work with such a digital wallet trust the company developer; in other words, believe their promise. 
A user has an option to independently store his private keys and certify the transactions on his device. However, the 
status of transaction confirmation is provided by the trusted node and is not checked by a user independently.

What are the advantages and disadvantages of this approach? The advantage is that it sets a user free of the need to 
store the entire chain of blocks on her device. On the other hand, the disadvantage is the dependency on a trusted node, 
because if it fails, a user will not always be able to rapidly switch to another trusted node.

In practice, there has been a case when a node of a private company, Blockchain.com, experienced denial of service for 
two days due to DNS problems (Fig. 4.38) [60]. Applications that used this node as a trusted host could not synchronize 
with the Bitcoin network and maintain regular operation.

![Figure 4.38 - Service denial of one of the wallet services](/resources/img/chapter-1/4.4-approaches-to-network-synchronization-and-spv-nodes/4.38-dos.png)

Since transaction verification takes place on a remote server, you need to be able to verify the reliability of the data 
transfer channel between a wallet and the server on which verification is performed. This approach carries the risk of 
the *man-in-the-middle attack*, which causes a user to receive a distorted version of transaction history.

Since it is the user's wallet and not a trusted node that processes private keys, the wallet software must on a regular 
basis transmit to the node the list of addresses to use in order to get the actual transaction history and status of 
transactions. In other words, a user depends heavily on a trusted node, and this dependency represents several risks for 
him. A user receives only the data that the trusted node broadcasts to him, and therefore, there is a possibility that 
the node can send fake data to the wallet. Also, the fact a wallet discloses all of its transactions to the node owner 
shows that the privacy of user's transactions is under risk. In case a user loses access to the trusted node (e.g., the 
node denies the service), then he can neither receive nor send any payments.

To reduce risks related to dependency on a single trusted node, you can opt to use several trusted nodes. In practice, 
this means that, under normal conditions, a user connects to and works with one trusted node. But if the connection is 
broken and there is a loss of access, the digital wallet automatically connects to another trusted node. In fact, with 
each connection, the wallet randomly selects one node from the trusted list.

This method is mainly used by mobile wallets because it allows for the verification of transactions and simplification 
of operation of a mobile application. Examples of such mobile wallets for Bitcoin include Mycelium and Coinomi. 
Distributed Lab has developed and supports the Bitxfy wallet, where this approach is also applied.

### Working with SPV nodes
The third most commonly used approach to synchronization between a digital wallet and the Bitcoin network is using a 
lightweight network node. This is referred to as an SPV node. This approach may not require a user to run her full nodes 
or to select the trusted nodes. It assumes that a digital wallet directly communicates with other nodes of the network: 
a wallet selects several dozens of someone else's full nodes and maintains a connection with them. The difference is 
that a lightweight node communicates with other nodes on equal terms. It exchanges p2p messages yet doesn’t require 
storing the entire transaction history. In this approach, the SPV method is used to verify incoming transactions. Unlike 
a full network node, the lightweight node performs only several important checks.

So, how does the work get done in this scenario? Instead of receiving blocks as does the full node, the SPV node only 
receives the block headers which are much smaller (80 bytes). These headers contain the necessary data that allows 
verifying that a particular transaction in the block has been confirmed. This is done without having to have any details 
of other transactions in this block. In this way, a digital wallet can ensure that a particular transaction in a 
particular block has actually been confirmed.

Why can't this be considered a full transaction verification? After all, a user still achieves independence from 
specific network nodes and can still communicate with a distributed network directly, but without having to store the 
entire transaction history. The drawback, however, stems from the fact that a lightweight client does not have the 
ability to fully verify the transaction on its own because it does not have all the necessary data (e.g. unspent coins 
data). It only verifies the fact that the transaction has been confirmed by validators; namely, it verifies that other 
nodes and owners of the majority of network processing capacity (validators) have completely checked this transaction. 
In this scenario, reliability again comes down to the issue of confidence that most of the hashing power is controlled 
by honest validators. In other words, you do not check a transaction yourself, instead, you put your trust for others to 
do it.

One of the peculiarities of the SPV node approach is the need of the wallet to maintain a network connection with a 
large number of full network nodes. This is necessary to minimize the probability that the node will connect to and only 
receive data from a malicious node, which may allow an attacker to impose an alternative (fake) state of the transaction 
history to a user. As a consequence, the victim can receive a payment from the transaction, which is confirmed according 
to the version of an attacker, but in fact, it is not. To increase the chance of getting actual data about the network 
state, a user should maximize the number of independent nodes available to him for communication. The more of them 
available, the lower the probability these nodes collude with each other against a user.

SPV nodes are less demanding for the stable operation of the network connection and often used in mobile applications. 
This approach to synchronization with the system is quite widespread and is used in a number of popular Bitcoin wallets 
including, Bitcoin Wallet, Electrum, and Bread Wallet.

### Operation of SPV nodes
As has been described, the main idea behind this approach is that a client using the SPV mechanism for initial 
synchronization only needs to download the headers of all blocks of the mainchain; given that the block header size is 
80 bytes, it is easy to calculate the required data size for the download. This can be achieved simply by multiplying 
the height of the last known block by the size of its header. Considering that in mid-August 2018, a block at an 
altitude of 537,000 was produced, from this logic it can be deduced that the total size of headers in the entire chain 
does not exceed 43 MB. In comparison, the size of a local copy of the Bitcoin database was 179,000 MB. For further 
synchronization with the network, an SPV node only requires headers of further blocks (80 bytes in size) [61]. A diagram 
describing the functioning of an SPV node is shown in Figure 4.39.

![Figure 4.39 - Operating model of an SPV node](/resources/img/chapter-1/4.4-approaches-to-network-synchronization-and-spv-nodes/4.39-spv-node.png)

In order for the SPV node to consider the transaction as having been confirmed, two conditions must be met:
* A transaction must be added to the block.
* A block containing the transaction must be included into the mainchain.

To satisfy the first condition, you need a *Merkle root*, which is located in the header of the block. The Merkle root 
value is calculated as shown in the figure below (Fig. 4.40). Firstly, hash values from each transaction are computed, 
then they are concatenated into pairs with the result being hashed again. This reduced output is concatenated again into 
pairs and then submitted to the hash function input. This process repeats until one hash value remains, the Merkle root. 
If you imagine this schematically, the intermediate hash values are iteratively added to a tree structure, the Merkle 
tree, which contains the Merkle root value in its vertex.

![Figure 4.40 - How Merkle root is linked to each transaction in a block](/resources/img/chapter-1/4.4-approaches-to-network-synchronization-and-spv-nodes/4.40-merkle-root.png)

The advantage here is that in order to verify the inclusion of a transaction in a block, you do not need the entire 
block. Values for pairwise hashing is enough and will allow you to make it to the root of the Merkle tree.

Having the Merkle root value, an SPV node need only request from a full node the hash value required for the 
verification of a particular transaction (the *Merkle branch*). Given the received values, a user can independently 
calculate the value of a Merkle root and compare it with the one stored in the header of the block. If the received and 
available hash values match, there is a fairly high probability the transaction will be included in this block.

The second condition to satisfy is that a user must be sure that the block actually exists in the mainchain. This is 
quite simple to establish: a user of an SPV node has all the values of block headers of the mainchain; each block header 
contains a link to the previous block as its hash value, so the user only needs to check whether the subsequent blocks 
refer to the one checked or not.

The operation of the SPV node can be summarised as follows. Only block headers throughout the chain are verified, not 
the entire block of transactions. To verify the correctness of a transaction, the node need only request some values 
from the full nodes to allow it to verify that a transaction is actually in the block. Only after the node has verified 
that each block belongs to the mainchain, it can then verify the belonging of particular transactions to this block.

### Conclusion
Having considered three main approaches to wallet synchronization with the network, you can evaluate the advantages and 
disadvantages of each of them respectively. The most reliable approach for Bitcoin operation, yet the most 
resource-demanding, is a full node. Alternatively, interaction with the network through a trusted node does not require 
to have much disk space for storing data yet is only viable if a user is ready to entrust the anonymity of her 
transactions to an owner of a trusted node. For users who do not find either the first or the second approach 
acceptable, there is a Simple Payment Verification (SPV) method, which allows a user to independently check the current 
confirmation status of relevant transactions without the need to load contents of the entire chain of blocks. Such 
verification requires trust as well, but in this case, it is about trusting the majority of network participants. Note 
that trust to majority is also required when using a full node, but an SPV node owner can verify only the presence of a 
particular block (with transactions included to it) in the mainchain rather than the data of transactions and blocks.

The theoretical threat remains that if the majority of participants collude with each other, they will be able to 
broadcast a fake transaction history to SPV wallet users. Therefore it is very important to consider the importance of 
secure synchronization when dealing with a digital wallet.

**Frequently asked questions**

*– Can I run the same wallet on three different computers simultaneously and start synchronization?*

Most likely we are talking about some kind of a network node, which is either an SPV or a full node. The case is about 
several different computers and the one and the same wallet, namely the same private keys. There are several full nodes 
that implement the wallet functionality. On these nodes, we insert the same private keys and start synchronization. Most 
likely, during full synchronization with the network, the same balance will be displayed on each wallet that this node 
uses. If a user sees changes on one node, then after synchronizing with the network, he will see exactly the same 
changes on all other nodes. The transaction does not remain secret. It is distributed to all nodes that verify it and 
display the corresponding changes (if it relates to their addresses). You can use one key on several nodes, but all 
transactions will also be synchronized automatically. Bitcoins, of course, will not double or moreover triple due to 
this.

*– Where can I find full nodes that I can trust?*

It is difficult to answer which nodes can be trusted. This is a matter of personal preference. There are services that 
provide open access to their node, but they are rarely trusted, especially when it comes to making high-value payments. 
You need to be very careful in choosing a trusted host because it is the mediator between you and a decentralized 
accounting system. In this case, there is no universal service that can be trusted. Most likely this is more a question 
of social trust. If the host belongs to a user’s friend, then the answer to this question is mostly the level of trust 
toward this friend. Using your own full node as the trusted one for the rest of your devices (e. g., a mobile wallet) is 
considered to be the most reliable option.

*– What guarantees does a particular developer team give? What is it responsible for?*

This is down to the individual service a vendor may offer and is of a legal rather than a technical nature. Any software 
distributor has their terms of use, defining responsibilities and what a user can make claims for in case of certain 
issues being encountered. Terms of use are different among various applications. As always, it is up to a user to decide 
whether to trust developers or not prior to using their product.

*– How many trusted nodes can I choose?*

You may choose any number of trusted nodes in order to minimize the risk of network access loss, three nodes for 
example. If there are any issues with accessing the first node, you connect to the second one and keep on working with 
it in the same manner.

*– What will happen to coins if servers on which they are stored will be completely or partially destroyed by an 
earthquake?*

First of all, it is not the question of coins—they do not exist—it is the question of keys that are used to access these 
coins. If the server that provides access to the Bitcoin network is destroyed, this will not affect the functioning of 
the digital wallet in any way. A user will temporarily lose access to the distributed database, but he will have keys 
that will allow him to spend coins. If the server on which user's keys were stored is destroyed, and the user does not 
have a backup of these keys, he will not be able to access his coins.

*– Will there still be access to coins if you install a software wallet, send coins to it, write a mnemonic phrase on 
paper, and delete the application?*

In fact, yes. Keep the mnemonic phrase safely—this is a sufficient minimum. The shared database stores information about 
how many coins were sent to relevant addresses. Roughly speaking, to prove ownership of coins and send them to another 
address, it is enough to know private keys, in this case, have a paper with a mnemonic phrase.

*– Is it possible to locally place a superstructure over a decentralized accounting system in order to be able to 
analyze actual data in real time in detail?*

Yes, it is. Anyone with access to the necessary software can perform this type of analysis. This can be a software 
written by a user herself or be a ready-made solution. It is down to the motivation of a particular user. If she needs 
to optimize queries to find certain transactions that are bound to specific addresses—as blockchain explorer does—she 
can apply software that will index all the blocks and cache certain data. An example of such a software is BitCore 
(supported by BitPay). BitCore has an open source code and is simple to use with a full network node.

## 4.5 Multisignature mechanism and Bitcoin Script
The possibility of setting conditions for performing transactions (*programmable money*) had for a long time troubled 
the minds of researchers, the first of them being Nick Szabo with the idea of smart contracts [62]. However, almost 
right after the appearance of Bitcoin, it became clear that the risk of keys being compromised was much higher than it 
might have seemed at first glance. It was necessary to solve the problem with keys before further implementation of new 
features. The solution was found and turned out to be very elegant.

As a phenomenon, multisignature has existed for centuries. For documents on paper, it has been habitual to put several 
signatures on a contract to make it legally significant. However, for signing digital transactions, it turned out to be 
something new (collective signature in banking systems does not count since it is not implemented cryptographically). 
Among cryptographers, Shamir's secret sharing scheme (SSSS) [63] was popular, which presumed that a key is 
mathematically divided into parts: a certain subset of these parts can restore the original key. The method required a 
trusted party to provide the initial generation and distribution of shares; for this reason, it did not suit an 
anonymous environment. Bitcoin, however, proposed the simplest method: a transaction must be authenticated by several 
signatures. This approach also helps solving the problem of reliable storage of coins and makes it possible to manage 
the balance of a wallet mutually. Subsequently, the idea of *multisignature* has been widely used in smart contracts.

We will consider the multisignature mechanism which is implemented in the Bitcoin protocol. Note that in the case of 
other digital currencies, these mechanisms can be implemented differently since a lot depends on the transaction model.

*A multisignature address (a MultiSig address) is a bitcoin address with which several pairs of keys are associated*. 
Each pair consists of a private and a public key; the combinations in which these keys are used can be different. 
Moreover, it is possible to set conditions under which several signatures must be required in order to spend coins from 
one address.

### Bitcoin multisignature transaction 
Recall that a regular bitcoin address is generated through double hashing of a public key (see 3.2). In general, a 
multisignature address is generated in the same way, but, in this case, there are several public keys that are 
concatenated before being hashed. In Figure 4.41, you can see schematically a transaction that spends coins from a 
multisignature address.

![Figure 4.41 - Multisignature transaction structure](/resources/img/chapter-1/4.5-multisignature-mechanism-and-bitcoin-script/4.41-multisig-tx-structure.png)

Above (Fig. 4.41), there is a header area that contains two fields. There are two inputs on the left and two outputs on 
the right. The first input contains filled fields: a hash value of the previous transaction that spends coins, an output 
number, etc. The field *scriptSig* contains a public key and a signature, which is typical for a regular transaction.

Let's observe the second input of a transaction. The field *scriptSig* contains another combination of data: two public 
keys and two signatures; they should be verified with these public keys in an appropriate order. This combination is 
essentially the input of a transaction that spends coins from a multisignature address, and it is basically what the 
proof of coin ownership appears to be.

In the figure below (Fig. 4.42), you can see an example of a real MultiSig transaction, consider the field *script* in 
this transaction. This field contains signatures and public keys of holders of the MultiSig address to which the coins 
are being sent. Compared to the field *script* of the usual bitcoin transaction, *script* of a MultiSig transaction 
contains more data and correspondingly occupies more space. This is because there can be at least two or even up to 15 
signers. 

![Figure 4.42 - Real multisignature transaction](/resources/img/chapter-1/4.5-multisignature-mechanism-and-bitcoin-script/4.42-real-multisig-tx.png)

Different key combinations are possible when using a multisignature address; thus, depending on the circumstances, any 
combination can be worthwhile. It should be noted that the scheme using the most possible number of mandatory signatures 
and public keys is 15-of-15. We will consider some of the most frequent options: 2-of-2, 2-of-3, and 3-of-3.

### 2-of-2 multisignature 
The 2-of-2 combination is the simplest case. It implies that there is a particular multisignature address to which two 
key pairs are attached. In fact, a hash value of a concatenation of two public keys is obtained as a corresponding 
address. To spend the coins from this address, two signatures are necessary; these signatures will be verified with the 
two public keys, which, when concatenated and hashed in a proper order, must return the same address. To generalize the 
facts described above, there are two predetermined keys and two mandatory signatures verified by these keys accordingly.

Imagine that a couple (Fig. 4.43) has decided to maintain a common budget. They agree that funds from the budget will be 
spent only with the consent of both of them. In Bitcoin, this can be implemented quite simply. They create a 
multisignature address, where one key is controlled by the wife and another by the husband. All family's income will be 
transferred to this address, and the coins will be spent only by their mutual agreement.

![Figure 4.43 - Husband and wife have decided to create a multisignature address](/resources/img/chapter-1/4.5-multisignature-mechanism-and-bitcoin-script/4.43-h-and-w.png)

Imagine that wife considers it necessary to buy a washing machine because she is tired of handwashing, while her husband 
believes that it would be wiser to spend all the money on a new PlayStation. She takes offense at her husband and 
destroys the paper carrier with her private key and thus makes it completely impossible to spend funds from their mutual 
address. How can this situation be avoided to protect yourself from a final loss of coins?

There is a way to create a transaction that can spend all coins from a proper address even if the amount is not known in 
advance. The signature for a transaction is created beforehand, while the missing data can be filled in later. In other 
words, if you do not know the data that should be specified at the input, you can enter it later in the already signed 
transaction. You can even create a transaction that will spend coins from the existing transaction, apply all the 
necessary signatures to it, and postpone it. When the time comes (for example, the necessary transaction appears), you 
will need to fill in the necessary data and send it to the Bitcoin network for confirmation.

In such a transaction, you can set the locktime field, which will postpone its confirmation for a certain time. So, 
right after the creation of a MultiSig address, even before receiving any payments, the couple can create two locktime 
transactions in which all future coins will thereafter be redirected to both of their personal addresses. These 
transactions can be printed on paper, and the paper carrier can be stored in a safe separately.

If you store coins on the balance of a multisignature address the keys to which are lost (one or both), then the coins 
get frozen. However, there is a *locktime* transaction; the one who publishes this transaction will be able to send these 
frozen coins to an active external address, and the coins will be saved.

The above case is a simplified example. In practice, much more complex mechanisms are possible.

### 2-of-3 multisignature

The 2-of-3 scheme implies that any two of the three predetermined keys must be engaged in verification of the two 
signatures provided as proofs of coin ownership. Thus, in order to spend coins, it is necessary to provide two 
signatures. These signatures will be verified with two of the three predetermined public keys.

Suppose there is a group of people who have a common budget. They create a multisignature address with three keys, with 
two signatures being enough to sign a transaction (Fig. 4.44). These coins can be spent with the consent of participants 
who make up the majority of a group; any two of the three participants can make a decision to spend coins. They only 
need to spread the transaction to the network.

![Figure 4.44 - 2-of-3 multisignature](/resources/img/chapter-1/4.5-multisignature-mechanism-and-bitcoin-script/4.44-2-of-3-multisig.png)

A more interesting way of using the 2-of-3 combination is when there is a *wallet service* involved. In this case, you 
should not confuse wallet services with bitcoin wallets that users monitor themselves. The service does not provide a 
full-fledged storage of coins, and neither it owns them but only provides services for convenient work. Schematically, 
it can be represented as follows (Fig. 4.45). The first of the private keys belongs directly to the service; the second 
one is generated by a user (and is only known to a user), and the third key is also generated by a user but stored 
separately and is not used regularly. After that, public keys are calculated from private keys and a MultiSig address is 
created. 

![Figure 4.45 - 2-of-3 multisignature with a wallet service involved](/resources/img/chapter-1/4.5-multisignature-mechanism-and-bitcoin-script/4.45-2-of-3-with-involved-wallet.png)

Imagine that we deal with a web service that allows a client to receive and send coins using a web browser. The browser 
loads a code that implements only a part of the bitcoin wallet functionality: receives UTXOs, generates transactions and 
verifies them with a digital signature. If a user wants to spend coins, she creates a transaction in the browser and 
signs it with her signature (one of the necessary ones).

Note that a user can get a private key either from her password or using a random sequence generator. After that, a user 
encrypts the received private key with her password and stores this key in an encrypted form on the service side. In the 
latter case, a user sends a request to the web service, receives a container with an encrypted key, decrypts it using 
her password (or a hash value of the password) as a key, and thus gains access to her private key.

When a user signs a transaction, it is being sent to the service side so that it would put the second necessary 
signature and send the transaction to the network. The service can clarify the user's action using a second 
authentication channel, which is installed in advance: a phone call, an SMS message, an e-mail message, or any other 
alternative ways of communication (up to a personal visit if that's what the security level requires). After the service 
has verified that the request to sign a transaction is initiated by the registered user, it puts the missing signature 
with its own key. After that, a transaction becomes valid and can be distributed over the network for confirmation.

The third key generally becomes necessary in the event of service denial. A user usually stores this key in a safe 
place. If the service does not respond, a user can sign the transaction with his keys (the second and the third one). 
If the second key is stored on the service side, the protected container with this key will be sent in advance to a user 
through an alternative communication channel (for example, via e-mail). The key is further decrypted. A user receives 
two necessary private keys and enters them in a special software, which the service had provided in advance. After that, 
the software operates autonomously on a user's computer (i.e., without the service being involved). This software should 
only be used in extreme cases when the service gets hacked, damaged, suspended, and so on. Then a user signs a 
transaction with his keys and sends the coins to their destination.

### Advantages of wallet services using 2-of-3 multisignature
This is a reliable storage method because the service does not own all the necessary keys. The service stores only one 
of the keys, which is not enough to take possession of client's coins. Therefore, neither a service nor even a potential 
hacker has access to the coins.

The convenience of this approach is that a user does not need to have secure access to this service. A user can use a 
regular PC or a smartphone, which can even potentially be infected with viruses or controlled by scammers, while the 
data can potentially be compromised or substituted; this is no problem at all since an attacker if accesses the device, 
will only obtain one of the two necessary signatures. Another advantage is that a user does not lose access to his coins 
even in case of service denial.

We have described only some of the possible schemes of using multisignature addresses. Now, let's figure out what 
Bitcoin Script is.

### Bitcoin Script
Bitcoin Script is a stack-based non-Turing-complete language for describing coin spending scenarios. A language which is 
non-Turing-complete has limited functionality and cannot perform unconditional jumps, loops, and recursions; that is the 
script cannot be executed infinitely. This principle allows limiting malicious parties from creating complex 
transactions, thereby slowing down the entire system.

The purpose of Bitcoin Script is to verify that the party who wants to spend coins has all the necessary proofs of 
ownership of these coins. Each transaction contains the fields scriptSig and scriptPubKey. The field scriptPubKey 
contains conditions for spending coins, while the field scriptSig contains the necessary data for unlocking coins. These 
fields are concatenated before the script is executed.

The execution of a script implies the sequential execution of operations on certain data contained in the stack. During 
the execution of each operation, this data is taken from the stack, and a certain action is performed over it, which is 
determined by the corresponding operation code (or simply opcode). The result of execution is pushed to the stack. Coins 
are considered unlocked only if the result of running an entire script on the stack is true. Also, the maximum stack 
capacity when using Bitcoin Script is limited to 520 bytes.

Opcodes constitute some kind of commands of a programming language for Bitcoin transactions [64]. Each operation is 
represented by a set of bits, which is read by the virtual processor and executed. Every opcode consists of two parts: 
the prefix "OP_" and the name of the operation itself. The most popular operations are OP_SHA256, OP_HASH160, OP_EQUALS, 
OP_CHECKSIG, OP_CHECKMULTISIG, OP_CHECKLOCKTIMEVERIFY [64].

### Concept of P2SH addresses and their benefits
BIP16 [65] has introduced a new concept in the Bitcoin protocol—the so-called pay to *script hash* (P2SH). It gives the 
possibility to set the rules for spending coins not with a plain script—where operands and certain data that are 
subsequently executed in the manner described above are written in succession—but with a hash value from the desired 
script, that is a checksum of these operands. It has become possible to specify large and complex conditions for 
spending coins in the output of a transaction while not enlarging the output.

To spend coins from such an output, a user should prove that she knows the terms of spending and, of course, she should 
meet these terms. Hence, a user publishes these conditions, that is the entire script as well as the hash value of the 
script. Now, by comparing the hash value received from the script with what a user has published, anyone can make sure 
whether the terms are correct or not. If you meet the terms, all that would be left for the participants is to verify 
the transaction in terms of the protocol rules.

This Bitcoin improvement proposal was adopted on January 3, 2012. Currently, P2SH is being actively applied and used for 
the implementation of a multisignature address.

Let's take a look at an example of how this works. To figure out how transactions with P2SH are created, we will 
describe concepts such as *redeem script*, *locking script*, and *unlocking script* as well as the rules for filling in 
the corresponding fields.

A *redeem script* contains public keys to which a multisignature address will be attached. In this case, we consider the 
2-of-5 combination of keys. First comes the value 2, which means that it is necessary to provide two signatures for the 
verification with the corresponding public keys. After that, 5 public keys are recorded. Next, the number of public keys 
(that is, the value 5) is specified; when the data is read in reverse order, this value will help to understand how many 
keys have to be read. After this, OP_CHECKMULTISIG, the multisignature verify operation, is specified.

A *locking script* is a script that is specified in the output of a transaction that conducts a payment to a 
multisignature address. Here, the hash value receive operation (e. g., OP_SHA256 or OP_HASH160) is performed. Next comes 
the hash value of a redeem script, which in this case has the size of 20 bytes. Then, the operation to verify the match 
of data with the actual hash value is performed.

An *unlocking script* is a concatenation of scripts in the input of a transaction with scripts in the output of the 
transaction that conducts a payment to a multisignature address. It represents two signatures for spending coins and a 
full redeem script, which will later be fed to the input of a hash function and checked for the compliance with the 
address to which the coins are sent. After that, the script is executed entirely even for the verification of a 
multisignature.

It is important that there are limits on the maximum size of 520 bytes for each of the listed scripts. This number is 
calculated according to the fact that an unlocking script can contain at most 15 signatures, 15 corresponding public 
keys, and several opcodes for the verification of this data. Being rounded up, the resulting value turns out to be 520 
bytes. However, this value is obtained expecting that a 15-of-15 multisignature address is a reasonable limit for the 
practical application. With a large number of signatures, a redeem script correspondingly becomes very large. Spending 
coins from MultiSig or other P2SH addresses can be quite costly for users.

Let's look at the advantages of P2SH. The first one is that such addresses can be encrypted into a habitual form using 
base58, in which their length is 34 characters. In accordance with BIP—that defines the rules for specifying the version 
byte for bitcoin addresses [66] which are encoded with base58Check—the address begins with a specific version byte, 
which is "3". Below is an example of how a P2SH address might look.

**3P14159F73E4gFr7JterCCQh9QjiTjiZrG**

This may not necessarily be a multisignature address. A redeem script can describe other terms of coin spending, 
including the more complicated ones. However, P2SH is not the only method for implementing multisignature in Bitcoin.

Another way is that instead of specifying a hash value of the script in the output that pays to the multisignature 
address, you specify there the script itself; namely, you list all open keys directly in the output and add the 
multisignature check operation. This method, however, has its drawbacks. The first is that public keys, which control 
the addresses on which user's coins are stored, will be publicly available—a user publishes them when coins are sent to 
the address. Another drawback is that for a large transaction size, a sender will have to pay high fees in order to send 
coins to a MultiSig address. It is unlikely that the sender would want to overpay fees only for the fact that the 
recipient wants to use complicated terms of spending coins, which take up a large amount of data.

P2SH allows transferring these fees to the recipient. So if the recipient wants to receive coins to the multisignature 
address, he will pay for confirmation of large transactions, which is a more equitable approach. P2SH also allows 
realizing different combinations of such multisignature (2-of-2, 2-of-3 and others).

### Example of using P2SH for a MultiSig address
Imagine that Alice wants to pay Bob, who only uses a MultiSig address (in fact, any two organizations could take their 
place). To do this Bob generates several private keys locally on his computer and gets corresponding concatenated public 
keys (Fig. 4.46). Commonly, public keys are first encrypted with base58Check, then sorted in alphabetical order, and 
then decrypted and concatenated, eventually forming the redeem script.

![Figure 4.46 - Using of P2SH](/resources/img/chapter-1/4.5-multisignature-mechanism-and-bitcoin-script/4.46-using-of-p2sh.png)

The sorting is worthwhile when you need to recalculate public keys from the private keys. They need to be concatenated 
exactly in the same order because the next step is to create redeem script and its hashing; if exactly the same keys get 
into redeem script but in a different order, you will get another hash value and a different address. This could result 
in misunderstandings. Therefore, before being concatenated, public keys must be sorted according to a certain rule. Most 
often, the sorting by the base58 alphabet is used.

So Bob has calculated the hash value from the redeem script. He can represent it in the form of 20 bytes and send it to 
Alice, saying that this is a P2SH multisignature address. Also, Bob can encrypt this hash value with a versioned byte 
just like a regular address, and simply send it to Alice. Alice, seeing the version byte, will understand that this is a 
multisignature address, so she will create a transaction and fill out its output accordingly for Bob to get his coins. 
Then, she spreads the transaction to the network. They both wait for confirmation; Bob accepts the payment from Alice 
and in return provides a service or, for example, releases goods.

Now, let’s imagine that Bob wants to spend the coins that Alice sent him. While the coins were on Bob's multisignature 
address, full redeem script has not been published. Even Alice does not know what and how many public keys Bob has used. 
She does not know the rules by which this P2SH-address is composed, whether it is a multisignature address or not, etc. 
Accordingly, attacks on the digital signature (on an elliptic curve) are still impossible.

Suppose Bob wants to send a payment to Eva (Fig. 4.47). In this case, she generates a new address and passes it to Bob. 
He creates a transaction prototype. In the input of this transaction, Bob refers to the transaction from which he 
received coins from Alice. At the output, he specifies Eva's address. Now, he must provide proof of ownership of coins 
that he spends, so he takes the necessary number of his private keys (2 if his multisignature address implies the 2-of-5 
case), from which he computes two signatures for the transaction. Then, he adds the full redeem script to the 
transaction input.

![Figure 4.47 - Bob proves his ownership of coins](/resources/img/chapter-1/4.5-multisignature-mechanism-and-bitcoin-script/4.47-bob-proves-the-ownership.png)

Note that Bob should store this redeem script entirely on his computer; either way, he will have to remember the order 
in which he used the public keys for composing this script. He should also remember the sorting rules that he applied 
(in case he did) as well as the multisignature address which is attached to certain private keys. Without knowing this, 
ob would not know which of his private keys he needs to operate with and in which order they have to be fed to the input 
of a hash function in order for him to obtain the desired address.

Finally, Bob has two signatures and a full redeem script. The transaction is considered correct and Bob distributes it 
to the network and waits for confirmation. These are the basic principles of spending coins from a multisignature 
address.

## 4.6 Features of the Segregated Witness update
The Segregated Witness update was probably the most desired and at the same time difficult to achieve in the ten-year 
history of the Bitcoin protocol. It provoked the first division of the community and showed to the whole world the 
non-triviality of implementing *governance* in a decentralized anonymous environment, where participants are prone to 
have conflicting interests. When the community members faced the problem of limited capacity in Bitcoin (3 to 4 
transactions per second), they began looking for the solutions. Opinions about who takes the final responsibility for 
the adoption of new rules—developers, validators, or users—were also divided. It is fair to note that many projects 
after Bitcoin (especially Dash, Cardano, and EOS) have initially included governance mechanisms in the p2p protocol. 
Yet, fortunately or unfortunately, when designing Bitcoin Satoshi did not provide mechanisms for resolving such 
disputes.

> **Issues of the original Bitcoin architecture**
>> * Low capacity of the system
>> * Transaction malleability (mutability of unconfirmed transactions)
>> * Combining business logic and coin ownership proofs
>> * Limitations on implementing off-chain protocols

Segregated Witness has brought great changes to the protocol and significantly improved the format of transactions. In 
this section, we will cover new formats of transactions and addresses, the possibility to separate the data with proofs 
of coin ownership, new formats for input and output scripts, and of course, the impact of this update on the capacity of 
the system.

You might want to see how the Segregated Witness actually looks like and what is its difference with a ordinary 
serialized Bitcoin transaction, which is presented in Figure 4.48.

![Figure 4.48 - Example of a standard serialized Bitcoin transaction](/resources/img/chapter-1/4.6-features-of-the-segregated-witness-update/4.48-serialized-tx.png)

Figure 4.49 shows the Segregated Witness transaction. The difference with this one is that witness data, which is 
detached in the figure, can be separated. Updated nodes receive full data about the transaction, including the witness 
data, while for the nodes which are not updated, witness data is not available, and they consider such a transaction 
correct by default (even if proof of ownership is not correct). We will describe why further in the section.

![Figure 4.49 - Example of a serialized SegWit Bitcoin transaction](/resources/img/chapter-1/4.6-features-of-the-segregated-witness-update/4.49-serialized-sewit-tx.png)

Essentially this is the primary idea of Segregated Witness update—the proof of coin ownership is kept separately from 
the primary transaction data (Fig. 4.50).

![Figure 4.50 - Segregating proof of coin ownership from the rest transaction data](/resources/img/chapter-1/4.6-features-of-the-segregated-witness-update/4.50-segregation-of-coin-ownership.png)

In addition, the SegWit update includes many other improvements. It allows increasing the network capacity, separating 
the proof of coin ownership from the rest transaction data, and eliminating disadvantages of the transaction format 
related to modifying data in the signed transactions (*transaction malleability*) while preserving backward 
compatibility with the previous versions of the protocol. The greatest value of this update is that it allows 
implementing a number of important off-chain solutions on top of the Bitcoin protocol (for more details, see 4.8).

### Backward compatibility and increased capacity 
Since the block size is limited, the number of transactions which can be included to a block is limited as well; it is 
what the system capacity depends on. Certainly, when an issue of increasing capacity appeared, the methods of increasing 
the maximum block size were primarily thought about as the solution. Though, this idea was not supported by the majority 
of the network participants.

Any proposal is being thoroughly checked and tested by the Bitcoin protocol team. An update is only released if the 
community reaches consensus and decides to implement the proposal in the protocol. Let's consider two basic ways of how 
the problem of increasing the capacity of an accounting system can be solved.

*Hardfork*. The simplest approach to updating is to increase the block size literally. One block is assumed to include 
more transactions, resulting in an increased capacity. Such a block, however, will not be accepted by the nodes that 
operate under the old protocol rules, according to which the maximum block size cannot exceed 1 MB. This change requires 
a hardfork, which is organizationally more complex than a softfork.

*Softfork*. Segregated Witness allows solving the problem through a softfork. It allows dividing a block into two parts: 
in the first one, you have transactions and in the second, proofs of coin ownership. New nodes of the network receive 
both parts, while the old ones receive only the part with transactions, which is 1 MB in size. Old nodes cannot receive 
the data with proofs and, accordingly, cannot fully validate the new-format transactions (what they see is that the 
block is correct according to general rules and therefore can be added to the mainchain). This solution allows old nodes 
to participate in reaching consensus while not resorting to a hardfork; also, it motivates the old nodes to gradually 
move towards the new software.

According to the old rules, the maximum block size was limited to 1 MB, and transactions in the block contained built-in 
proofs. New rules also imply the maximum base block size as 1 MB; however, outside the block, there is an additional 
data structure containing proofs, and hence the total size of a new-format block exceeds 1 MB.

For backward compatibility, the protocol rules allow old nodes to work with the new blocks; the difference is that they 
receive blocks in their base configuration, with a maximum size of 1 MB, and they are not able to see the structure 
witness (proof of coin ownership). The new nodes, however, receive full blocks containing both transactions and proofs 
(see Fig. 4.51).

![Figure 4.51 - Result of Segregated Witness update](/resources/img/chapter-1/4.6-features-of-the-segregated-witness-update/4.51-result-of-segwit-update.png)

On the left, you can see how the Bitcoin protocol operated before the activation of Segregated Witness. The block had a 
maximum size of 1 MB and was distributed among different nodes of the network in the same form.

### Innovations of Segregated Witness
The first and the most important innovation of Segregated Witness was the new transaction structure. In addition to the 
already known fields, the new transaction now includes three more: *marker* and *flag*, which are used for versioning 
(in this case, they are strictly specified, but this can change in further protocols), and also the field *witness*. 
*Witness data is a set of proofs of coin ownership that are excluded from the primary transaction part*. Structurally, 
it looks like a set of inputs, where each witness data element corresponds to an input with a certain number. This 
allows comparing the proof with specific spent coins.

Each transaction has its unique identifier, *txid*, by which it can be traced. To receive it, you need to cast the 
transaction to a single data sequence and then get the hash value from this data sequence. With the introduction of 
Segregated Witness, the new identifier, *wtxid*, and the new serialization format appeared. For the old transactions 
(that spend coins without using Segregated Witness), *wtxid* is the same as *txid*. Figure 4.52 shows the transaction 
fields that take part in generating *wtxid* (for default and SegWit transactions respectively).

![Figure 4.52 - Difference in how transaction identifiers are formed](/resources/img/chapter-1/4.6-features-of-the-segregated-witness-update/4.52-diff-between-txs.png)

*Wtxid* is required to build an alternative Merkle tree for proofs. It is built almost the same way as for the usual 
transactions, but here, instead of a transaction hash, the *wtxid* value is applied. Accordingly, the *wtxid* values of 
transactions are pairwise hashed and, as a result, the Merkle root is generated.

Merkle root is inserted into a coinbase transaction, not into a block header. Otherwise, the block structure would 
change, so that nodes supporting the old protocol could not operate with the new-format blocks, and it would hinder 
preserving the backward compatibility. Therefore, Merkle root is inserted into one of the coinbase transaction outputs. 
As soon as all nodes switch to Segregated Witness, this situation may change, and new approaches may possibly be 
considered.

Let's observe the issue of *transaction malleability* in more detail. In Bitcoin, there is a possibility to modify certain 
transaction data while keeping a transaction itself correct (coins will be sent from and to the same addresses) [67]. 
However, minor these modifications may seem, they are enough to cause a change in the hashing result, making it a 
completely different transaction (even though it sends coins from and to the same addresses as the original one). Such 
modifications can only be applied to unconfirmed transactions, which by itself may seem non-threatening. However, for 
unconfirmed transactions, the immutability of data is of no less importance: the undefined probability of such 
modification of transaction data interferes with the implementation of off-chain protocols, which are based on building 
chains of unconfirmed transactions.

How does it work? When a Bitcoin transaction is created, not all data is hashed and signed: namely, you cannot sign the 
scriptSig field because it contains the data of a digital signature generated by a user. This vulnerability allows 
permitting several types of attacks in Bitcoin. Let's consider some of the basic ones.

First of such attacks allows modifying the signature format. In the Bitcoin protocol, the signature format, which is 
placed in scriptSig, has happened to be non-strict and depends on the implementation of OpenSSL, which also does not 
provide a strict format. An outsider can intercept the transaction and slightly modify it while keeping it valid. The 
hash value of the transaction will change so that it will become a completely different transaction.

The second attack affects the scriptSig directly. Since scriptSig is a set of commands by which a user proves the 
ownership of coins, it allows specifying the number of additional operations. It is possible to add several meaningless 
operations to the original data in scriptSig, which will not affect the result of a script but will change the hash 
value of a transaction.

What's wrong with changing the transaction hash value? This question is relevant because one might think that an 
unconfirmed transaction is not trustworthy and you better not rely on it until it is confirmed. However, the issue of 
transaction malleability is more important than it may seem at first glance. Firstly, it makes it possible to create a 
transaction that will be exactly like the original one but because of a changed hash value, network nodes will consider 
it as a different (new) transaction. Therefore, the original transaction will conflict with the alternative one, meaning 
that you cannot ever be sure that your confirmed transaction has the original hash value. Secondly, solving the problem 
of transaction malleability is important for the implementation of protocols which are based on building the chain of 
unconfirmed transactions. If a hash value of at least one transaction from this chain changes, the entire subsequent 
transaction chain will get invalid. Thus, it turns out that the hash value of any transaction can be changed by an 
outsider simply because you don't have to have the access to private keys to change the hash value.

The SegWit update solves issues related to transaction malleability, namely for the new-format transactions, where now 
fields are filled by strict rules. It prescribes setting and serializing the data unambiguously, excluding duality.

### Example of a SegWit transaction
In order to understand what is stored in the corresponding transaction fields, consider an example of a segwit 
transaction confirmed on the Bitcoin network and which is presented in JSON format (Fig. 4.53).

![Figure 4.53 - Real SegWit transaction](/resources/img/chapter-1/4.6-features-of-the-segregated-witness-update/4.53-real-segwit-tx.png)

First, let's pay attention to the fact that the *scriptSig* fields are empty. The corresponding *proofs of coin 
ownership* in this case are placed in additional fields: witness. And just like in normal bitcoin transactions, these 
fields consist of digital signature and public key. The only difference is that the scriptSig field is included in the 
basic structure of the transaction, while the witness field is not. This allows not to transfer this data over the 
network, not to verify and not to store it in memory when it is not needed, because the proofs of ownership of the coins 
has already been verified.

In fact, *witness data* is a separate part of the transaction, which in some cases may not be used or even be completely 
absent without changing the basic structure of the transaction and its hash value. However, when displaying a 
transaction on the screen or in JSON format, it is convenient to show *witness data* in the transaction inputs.

### New concepts of weight and size

The concepts of transaction weight and block weight are another innovations that the Segregated Witness update has 
introduced. Prior to the update, there were only concepts such as the transaction size and block size. Everything 
related to it (for instance, transaction fees) was determined according to the size. The block size was limited to 1 MB. 
With the introduction of Segregated Witness, it became necessary to process old-format transactions (with the "old" 
size) along with new-format ones.

Another important issue to solve was to reduce the cost of processing an additional part of a block (witness data). In 
order to solve it, the concept of transaction weight and corresponding weight units have been introduced. The size of 
the main part of a transaction is now calculated with the coefficient 3, and the size of witness data with the 
coefficient 1. As you might guess, any data included in witness data requires 3 times fewer fees than the basic data of 
a transaction. Such an approach allows validators to determine a more profitable transaction as to the relation between 
space occupied in a block and the reward received. Transaction weight is calculated with the formula below.

**transaction weight = base size * 3 + total size**

* transaction weight is measured in weight units;
* base size is measured in bytes;
* total size is measured in bytes.

In this formula, the base transaction size (that is the transaction size when serialized under the old rules) is 
multiplied by three, and the result is added to the transaction size that is serialized according to the new rules. 
Thus, we get the transaction weight.

Regardless of the rules (old or new) under which old-format transactions are serialized, their size will always stay the 
same, while the weight, accordingly, will be exactly 4 times bigger. For SegWit transactions, the weight is slightly 
less because they do not include proofs of coin ownership.

Along with the weight, the concept of *virtual size* was also introduced; it is calculated by dividing the weight by 4. 
Virtual size is used to calculate fees for transactions and to make it possible for validators to understand the 
profitability of including a particular transaction in their block while using the usual price of the record, which is 
measured in spb (satoshi per byte) units.

**virtual size = weight units / 4**

Since the weight of an old-format transaction is 4 times bigger than its size, the virtual size of a transaction is 
equal to its usual size. Accordingly, for the old-format transactions, fee calculation remains the same, and for the 
new-format ones, it will be slightly less because signatures are put to a separate structure. This is how SegWit 
transactions make it possible to pay lower fees but have the same priority for validators to have them included in the 
block. At the same time, the maximum block size without the witness data (base size) remains 1 MB, while the maximum 
block weight is up to 4 MB.

Now, you may well have a logical question: what is the final size of a block with the witness data? The answer is 
obviously somewhere in the range from 1 MB to 4 MB (conditional weight units), but you cannot actually tell the precise 
value but only approximate, which is about 1.8 MB. How is it obtained? Through an accurate theoretical evaluation. A 
typical block with transactions currently consists of approximately 60% of proofs of coin ownership. Let's calculate the 
weight of a 1 MB block, which consists of 60% of proofs of coin ownership.

**400,000 bytes * 4 = 1,600,000 conditional weight units**

**600,000 bytes * 1 = 600,000 conditional weight units**

**1,600,000 + 600,000 = 2,200,000 conditional weight units** (This value matches the actual size of 1 MB.)

Now, given the maximum weight of the block of 4,000,000 conditional units (defined according to the protocol rules), it 
is possible to determine how much this will increase the final size of the block together with the witness data.

**4,000,000 / 2,200,000 = 1.8** (Hence, the block size will be 1.8 MB.)

Thus, the average effective block size can be assumed to be around 1.8 MB (do not confuse the actual size of the block 
with its weight). In practice, though, this value totally depends on the set of transactions in a particular block.

### SegWit adoption statistics
In November 2018, the number of SegWit transactions exceeded 40% of the total number of transactions on the Bitcoin 
network. Moreover, if you consider the total number of coins transferred on the Bitcoin network, SegWit transactions are 
liable for 55-60%. At the same time, digital wallets (e.g., Electrum, Bitxfy) and other major services operating with 
Bitcoin have implemented Segregated Witness support not long before 2018.

After the update adoption, dynamics of the final block size is also noticeable (Fig. 4.54). At the moments when the flow 
of new transactions increases, almost all blocks exceed 1 MB (sometimes even 2 MB). It is quite obvious that after the 
activation of SegWit, the issue of low capacity in Bitcoin seemed no longer as pressing as before the update [68].

![Figure 4.54 - Graph of how block size has changed after SegWit adoption](/resources/img/chapter-1/4.6-features-of-the-segregated-witness-update/4.54-block-changing-after-adoption.png)

If you look at the dependence of an average transaction fee on the number of new-format transactions, you will also see 
a very strong correlation between these values (Fig. 4.55).

![Figure 4.55 - Dependence of average transaction fee on the number of transactions](/resources/img/chapter-1/4.6-features-of-the-segregated-witness-update/4.55-average-tx-fee.png)

Also, we should not forget that Segregated Witness has enabled the development of the off-chain solutions on top of the 
Bitcoin protocol. To get ahead of the story, we would like to point out that the adoption of Lightning Network (LN) is 
much more difficult than the SegWit implementation. However, the work towards LN adoption is well underway, and there 
are already significant achievements (see 4.8).

**Frequently asked questions**

*– Is it correct to claim that RBF (replace-by-fee) will not work for the Segregated Witness transactions?*

No, it is not. Replace-by-fee will work for the Segregated Witness transactions because it is based not on what the 
spending rules are but on the fact of using the same coins and specifying the sequence number of the transaction input. 
If you increase the input value using the same coins and specify correct proof of coin ownership, then you can replace 
the previous transaction.

*– How can I change the hash of an unconfirmed transaction?*

Transaction hash is the result of computing the hash function of all the data that is stored in the transaction. 
ScriptSig, which is contained in a transaction, participates in the calculation of hash but cannot be signed. Minor 
changes in this field that do not entail changes to the signature verification rules will cause changes to the 
transaction hash value. It means that while the signature and transaction remain valid, the hash of the transaction will 
change.

*– How is witness data stored in a transaction?*

As noted before, Segregated Witness transactions have a new structure: a set of inputs, a set of outputs, and proofs in 
which witness data is stored, respectively. As simple as that: a user has a dataset describing that there are two inputs 
of the transaction (bytes of the first input and bytes of the second input), two outputs, and two more sets of witness 
data, which are also written similarly as a sequence of bytes.

## 4.7 Fees mechanism in Bitcoin
In this section, we will describe the main purpose of fees in Bitcoin, namely how they work and what they influence on. 
We will cover reasons for the volatility of fees, delays in confirming transactions, and approaches to solving these 
problems. In addition, we will touch on the topic of how the Segregated Witness update contributes to lowering the 
transaction costs. In conclusion, we will give some food for thought considering how being friends with validators may 
reduce fees to zero, and what this trend may lead to in future.

According to the Bitcoin protocol rules, in order to confirm a transaction, owners of most computational power must 
create a block containing the transaction and extend the chain based on the block. In other words, several more blocks 
must be built on the basis of the block in which the transaction received the first confirmation (Fig. 4.56). It is 
important that this chain is the longest, and there should not be any competitive chains. The required number of the 
following blocks (transaction confirmations) is determined by the recipient at his own discretion. The recipient uses 
the following rule: *the more confirmations a transaction has, the less likely it is to be canceled later*.

[Figure 4.56] - Inclusion of a transaction in a block 

With the growth of Bitcoin's popularity, the flow of new transactions on the network has significantly increased. The 
maximum block size in Bitcoin is 1 MB, which is strictly defined by the protocol rules. Block creation time is 10 
minutes on average, hence, the capacity is limited (1.7 KB/s). If the flow of new transactions exceeds the network 
capacity, some transactions will not be processed (such a situation is not rare). Which transactions will receive 
confirmation first and which of them will wait—these are the questions that require a clear answer. The basic idea here 
is that transactions compete with each other.

Fees mechanism in Bitcoin is a necessary tool that allows you to pay for the service of a distributed network, whilst 
the service essentially is a reliable data storage. In fact, Bitcoin users pay for each byte of data added to the shared 
database. Due to the fact that the capacity of this database is limited, users compete with each other for the priority 
of record.

When creating transactions, users set a fee of a certain amount of satoshis per one byte of data. Each node-validator 
arranges all unconfirmed transactions in a queue so that those transactions that pay a larger fee per unit of their size 
(weight unit) are confirmed first. This means that those transactions that get to the end of the queue may remain 
unconfirmed for a long time (Fig. 4.57).

[Figure 4.57] - Competition for the space in a block

Another important task that fees mechanism has solved is the so-called *tragedy of the commons*. In the context of 
Bitcoin, it is referred to transaction spam protection. The presence of mandatory fees means that it will be expensive 
for an attacker to flood the network with fake transactions for a long time.

### Volatility of the data recording price 
At times, users of the Bitcoin network face the problem of unusually high fee volatility. For example, in 2017, the 
average price of adding one byte of data to Bitcoin's database fluctuated between 1 and 500 satoshis. The queue of 
unconfirmed transactions ranged from several KiB to tens and even hundreds MiB.

Due to the fact that the price of data recording is apt to change dramatically, users are compelled to compete with each 
other almost blindly. This is because a transaction is practically confirmed on average within 8 minutes, while the fee 
for the processing must be established even before the transaction is signed. Therefore, the issue of a properly 
established fee is still relevant—everyone wants to have their transactions confirmed quickly, that said within minimal 
costs. In 2017, fees for adding an average transaction to the Bitcoin database reached up to $50. Paying such a fee is 
obviously not the best idea especially when you can save up to 90% of that amount by intelligently calculating fee and 
propagation time.

And so, the consequences of a sharp increase in the flow of new transactions are due to the fact that there is a huge 
queue of transactions waiting to be added to the block. Among them, there are transactions in which senders either 
created their transactions at the moment right before the sharp price increase or, simply, did not pay attention to a 
leap in the price of the record. Commonly, the main problem is not due to the users but due to the wallet software or 
the service that manages bitcoins. Moreover, many software products for working with Bitcoin even hide the fee 
management from user.

### Solving the problem of fee volatility 
Instead of using constant or manually updated values, Bitcoin wallets can use special mechanisms to estimate the current 
price of data recording. These mechanisms are implemented in the software of the wallet and allow users to manage the 
priority of data recording intelligently.

What is noteworthy is that it is possible to forecast the price of the record. Such forecasts, however, usually work 
only for very short periods of time (few minutes on average). If the forecast is outdated and the transaction is not 
confirmed, then you will very likely wait for its confirmation for a long time (several hours, sometimes even up to a 
day).

**transaction fee = transaction size * byte price**

Dynamic (or adaptive) fee calculation implies an automatic estimation of the final transaction size in bytes and the 
price of adding one byte of data to the Bitcoin database for the next few hours. For both estimates, there are special 
algorithms for analyzing transactions and activity on the network. After you receive the estimates of transaction size 
and the price of adding one byte of data to the Bitcoin database, you multiply these two values; the result would be the 
amount of bitcoins you need to add as a fee to your transaction.

The advantage of adaptive calculation is that the fee you include is much more accurate; it will be above the minimum 
threshold yet not too high. Moreover, the probability that this transaction gets to the end of the queue of unconfirmed 
transactions is minimal. Nevertheless, there are exceptions, so we will consider in more details what can be done in 
case of an emergency situation.

### Increasing the fee after the transaction has been sent
Bitcoin protocol is currently quite flexible regarding the fees. There are two protocol improvements that allow you to 
increase fees even after the transaction has already been created and sent to the network: *replace-by-fee* [69] and 
*child-pays-for-parent* [70]. Unfortunately, only a few wallets support this functionality, yet more of them appear over 
time.

*RBF (replace-by-fee)* allows replacing an unconfirmed transaction with an alternative transaction with an increased 
fee. This means that a user can again create a transaction where he uses the same coins as in the first attempt but this 
time set a higher fee for the transaction and submit it to the network again. There is a set of rules which determine 
how to create such transactions and how network nodes should do the replacement. These two transactions conflict with 
each other and only one of them will eventually receive confirmation.

*CPFP (child-pays-for-parent)*. In this case, you need to create a new child transaction that will try to spend coins of 
your unconfirmed parent transaction. In this child transaction, you include a fee that is beneficial for validators. But 
in order to confirm it and get the fees, a validator should first confirm the parent transaction and eventually add both 
of them to his block. The method was proposed in August 2016 [71]. This approach has a number of technical nuances and 
organizational requirements for its correct use. One of these requirements is that a user (or the software in the 
automatic mode) should recalculate the fee of a transaction, create and sign a new, alternative transaction, distribute 
it to the network, and continue monitoring its state.

An example of a transaction that is confirmed using the child-pays-for-parent method is demonstrated in Figure 4.58; 
there you can see two corresponding transactions (parent and child) confirmed in one block.

[Figure 4.58] - Child-pays-for-parent mechanism

As you can see the problem of raising a fee can be solved yet has certain inconveniences. Despite the fact that the RBF 
and CPFP options are supporting the protocol for quite a long time now, their adoption in bitcoin wallet software is 
quite slow.

### How does Segregated Witness help to lower fees?
Recall that one of the protocol changes that have been introduced with the Segregated Witness update is the new 
transaction format and its *weight (transaction weight)*. Prior to the advent of Segregated Witness, only the size of a 
transaction was usually considered when calculating a fee, and now both the size and the weight of a transaction are 
important. A new transaction contains proof of coin ownership in a separate structure (witness data). To convert the 
*transaction size (total size)* to its weight, the witness data size is multiplied by a smaller coefficient than the 
other transaction data. The transaction weight is calculated using the formula below.

**transaction weight = base size * 3 + total size**

In this case, *base size* is the transaction size excluding the size of witness data. Any data included in the witness 
data requires 4 times fewer fees than other transaction data. This approach allows validators to determine a more 
profitable transaction regarding the space occupied in the block and the reward received.

About 60% of all transaction data is precisely the proof of coin ownership (this data can be included in the witness 
structure). Accordingly, the weight of new-format transactions significantly decreases, and users pay less for having 
their transactions confirmed. Moreover, these transactions have the same priority for validators as the old transactions 
that pay a higher fee.

Figure 4.59 shows the dependence of the price of adding one byte of data in the shared Bitcoin database from the load 
(flow of unconfirmed transactions) expressed in bytes per second (byte per second). A conclusion is very simple: if the 
flow of new transactions is lower or equal to the capacity of Bitcoin, then the price of record is almost zero. If the 
flow of new transactions exceeds the capacity, then the price increases sharply.

[Figure 4.59] - Dependence of fees and capacity in the Bitcoin system

### Friend miner case
Imagine that you have a friend who is engaged in mining and who controls 10% of the entire processing power of the 
Bitcoin network. This means that he creates one block on average once in 100 minutes. If your friendship is strong 
enough, you can create a transaction with a zero fee and send it to your friend for confirmation (Fig. 4.60).

[Figure 4.60] - Friend miner case

On the average, the first confirmation for your transaction will be received with 100% probability within 100 minutes, 
and full confirmation will be received 50 minutes after the first one. As a result, your transactions will receive full 
confirmation in 150 minutes. If you did not have such a friend, full confirmation of a transaction would be received 
within approximately 60 minutes, but you would have to include the fee payment.

Thus, if the question is about saving money, having a friend miner who controls a significant part of all mining 
capacity in Bitcoin is a fortune. However, even if you do not have such a friend, there may be alternative ways of 
paying for a block space.

### Option of selling places in the confirmation queue
Commonly, a validator operates according to a standard scheme and sorts transactions according to the price of adding 1 
byte of data—it seems that every software for mining is organized this way. However, there possibly may be a way for 
validators to have a better motivation: they can independently formulate a policy of monetizing their activity (i.e., 
they can benefit from an alternative method of adding transactions in the block). Validators may very likely consider 
changing their policies if they find something more beneficial or relevant. This approach assumes that users can pay 
validators for confirmation of their transactions not using a pre-established fee but rather directly—according to their 
own business logic.

In practice, any sufficiently large mining pool can conduct its campaign in order to increase profitability and use 
simple mechanisms for this, for example:

* Determine its priorities for sorting transactions 
* Establish partnerships with certain services (exchanges, stores, browsers, etc.)
* Sell guarantees for a particular space in the future block

Moreover, pools can even pre-sell certificates for free space in their blocks as an obligation on the provision of a 
certain capacity of the system in future (for more details about tokenization, see 6.4).

**Frequently asked questions**

*– How to send a transaction with a zero fee to a friend miner?*

As a regular transaction, it is unlikely to be propagated on the network because a node configuration has a default 
parameter *minRelayFee*, which is not equal to zero. This means that a transaction must necessarily have a fee greater 
than a certain threshold. Only then, nodes will react to the transaction and will pass it further. This is done to 
prevent spamming. Therefore, in such a situation, a user may apply a different approach to transmitting this "unusual" 
transaction to the friend miner. One of the options is to forcibly connect the user's node to the friend node-validator 
by manually entering the network address in the configuration file of the user's node and to set minRelayFee to zero on 
the node-validator. There is a problem, however: a friend node must distinguish "unusual" transactions from the 
transactions of other users. Therefore, another solution is that user wallet and the friend node-validator would use 
their own API.

*– What is the benefit for a validator to take a smaller fee by selling certificates if she can take more bitcoins?*

The benefit is that a validator can sell space in the block in advance. For example, she may need money earlier than she 
will be able to create a block. Moreover, a fixed price can be important for Bitcoin wallet users; sometimes they need 
to know their costs for a few months in advance. Also, this provides a certain guarantee for users that there will be a 
place in future blocks for their transactions for the upfront known price.

## 4.8 Payment channels and Lightning Network

The idea of *payment channels* (and its development in *Lightning Network*) has undoubtedly become revolutionary for the 
world of digital payments. The application of this idea is not limited only to cryptocurrencies. An important question 
for any digital accounting system are the capacity limitations, as well as concerns for user privacy. Before the 
appearance of digital communication systems, these problems were irrelevant—the operation to transfer a paper banknote 
from hand to hand, obviously, did not require accessing a server and was neither tracked nor censored. The appearance of 
digital accounting systems made it possible to transfer funds fast without the physical limits of delivering gold or 
banknotes. However, in digital systems, every new transaction should be added to the database as soon as possible, with 
both the sender and recipient having access to the database—so that both could verify that the payment has been 
completed. In addition, a party who has access to the history of all transactions (a government, for example) has a 
tremendous leverage in influencing people's lives through the ability to analyze this history and track the activities 
of citizens.

The appearance of off-chain payment channels and Lightning Network (LN) [72] allowed for payment networks which are 
scalable, secure, cheap, and that work in a truly p2p manner, preventing breach of privacy. Below we will discuss the 
principles of both mechanisms, their features in the context of Bitcoin, and see how they work.

## What is a payment channel?
For the first time, the idea of payment channels was described by Satoshi Nakamoto in a personal letter to one of the 
active developers of the protocol many years ago. At that time, Bitcoin did not yet receive enough important updates to 
allow for the implementation of reliable payment channels. However later it became possible, and in 2013 developers 
returned to this truly promising idea. We will consider the main methods for implementing payment channels.

*A payment channel is a method that allows carrying out multiple payments without adding transactions for each payment 
into the chain of blocks*. In order to open and close a payment channel, counterparties need to perform two on-chain 
transactions, opening and closing. Channel members interact only with each other [73], and the presence of additional 
validators or trusted third parties is not required.

The process of using a payment channel is similar to that of recording a loan at the everyday level (paper record). The 
difference is that through payment channels there is a guarantee parties can fulfill their obligations—which a paper 
record cannot allow for.

Below, we will consider the principles of payment channels and the protocol which allowed developing and applying this 
concept to improve Bitcoin—namely, Lightning Network.

## Why are payment channels needed? 
Main limitations in Bitcoin are long transaction confirmation time and unpredictable fees which make micropayments 
(e.g., a fraction of a cent) impractical. What are the advantages of a payment channel compared to usual transactions? 
In the payment channel, participants make payments between themselves without publishing transactions on the Bitcoin 
network. The receiving party performs a quick independent verification and accepts the payment. Since only a few parties 
are involved, fees are absent by default. Due to this specific feature, payment channels are also called *micropayment 
channels* [74].

The interaction of channel members is private by default. The details of each micropayment will remain secret from 
everyone else, and only the fact of using the payment channel will be known to the public—namely specific bitcoin 
addresses between which the payment channel is established.

What is noteworthy is that participants of a payment channel should not necessarily trust each other. Security 
mechanisms in the payment channels allow preventing the fraud on the part of interacting parties. The idea is that in a 
fraud scenario, a malicious party would not only lose the coins he tried to illegally possess but, depending on the 
implementation method, also he would lose all the coins on which he opened the channel.

## Payment channel: step-by-step example
In a simplified version, the operation of a payment channel is shown in Figure 4.61. Alice and Bob have Bitcoin wallets 
with additional modules for working with the payment channel. These modules exchange data for making payments (for 
instance, data about coins distribution, relevant data about signatures, etc.).

[Figure 4.61] - Operation of a payment channel

It would be interesting to consider this in more detail and see the very way how coins are distributed within the 
payment channel (Fig. 4.62).

[Figure 4.62] - Interaction within a payment channel 

*Step 0*. To open the channel, Alice and Bob create a multisignature address and both transfer their coins on it, let’s 
say, 5 BTC each. So the multisignature address now stores 10 BTC from which they each “own” 5 BTC (below we will explain 
what does this “own” means).

*Step 1*. Now let’s imagine Alice wants to pay Bob 2 BTC for some service. To do this, Alice creates an off-chain 
transaction that distributes value in the payment channel so that Alice now owns 3 BTC and Bob 7 BTC.

*Step 2*. Alice needs to pay Bob one more time, let’s say 1 BTC. To transfer it to Bob, Alice again creates another 
off-chain transaction that distributes the coins so that Alice now owns 2 BTC and Bob 8 BTC.

*Step 3*. Now Bob decides to pay Alice 7 BTC, and so he creates a new transaction with the corresponding terms of coin 
distribution: Alice owns 9 BTC and Bob 1 BTC.

*Step 4*. After Alice and Bob have performed all the required payments, they close the channel. For this, they publish 
the last transaction (the one that Bob created in step 3) on the Bitcoin network. The input of a resulting transaction 
will be 10 BTC that are stored on the multisignature address; this transaction will pay 9 BTC to Alice and 1 BTC to Bob.

Noteworthy, only two of all transactions took place on the Bitcoin network, the opening transaction (step 0) and the 
closing transaction (step 4). The rest transactions were off-chain and thus were not burdening the Bitcoin network. 
Correspondingly, Alice and Bob didn’t have to pay any fees for them. Inside the payment channel, they were simply 
exchanging with each other the same transaction with the same input but with updated output fields. When the time has 
come, one of them publishes the last updated transaction on the Bitcoin network.

### Features of a payment channel
Payment channels (as a concept) do not have any noteworthy shortcomings compared to usual transactions, but they do have 
certain characteristic features.

A payment channel must be opened and, sooner or later, closed. These processes are performed by separate on-chain 
transactions. Waiting for confirmation and carrying fees is inevitable with these transactions. Note that for an opening 
transaction, it is better to wait for a full transaction confirmation since you have to be sure that the coins on which 
you opened the channel are definitely locked up on a multiSig address.

Inside a particular channel, payments are available only within a predefined amount (following our above example, this 
means that Alice and Bob can interact only within predefined 10 BTC). This amount is a sum of deposits on the 
participants’ mutual multisignature address.

Payment channels can be *unidirectional* and *bidirectional*. It depends on the method of the channel implementation.

The lifetime of the channel and the maximum number of payments in the channel can be unlimited. However, depending on 
the technique, it is possible to set limits on the channel's parameters. Accordingly, the channels can be closed at the 
advent of a certain time or before the scheduled time. In addition, the channel can be closed both by mutual consent of 
the participants and at the request of one of them but with some specific requirements.

### Methods of payment channel implementation 
As we have mentioned in the Alice and Bob example, users have a special module that allows them to interact within a 
payment channel. Depending on the implementation, these modules can support different methods of payment channels 
functioning. We will list some of the most popular ones.

*Spillman-style payment channel* is the simplest version of a one-way channel with a limited lifetime and an unlimited 
number of payments.

Later, another improvement on the Bitcoin protocol was made and *CLTV-style payment channels* became available, which 
are an improvement of the previous method.

*Poon-Dryja payment channel* is a method of bidirectional channels with unlimited time of operation. They require 
several updates of the Bitcoin protocol, which were recently adopted: these channels are used in the design of 
Lightning Network.

*Decker-Wattenhofer duplex payment channel* is an option of using two unidirectional channels simultaneously. It allows 
improving the properties of channels because it forms a whole tree of replaced transactions rather than a consecutive 
chain of replaceable ones.

We will consider in detail the Spillman-style implementation since it is the simplest and the most understandable one. 

### Spillman-style payment channels
*Spillman-style payment channel* is the method of creating unidirectional payment channels, where there is a role of a 
sender and recipient. The operational time of this channel is set arbitrarily by the sender, while the recipient can 
close the channel ahead of the schedule. The basic steps of operation of this channel are shown in the diagram below 
(see Fig. 4.63).

[Figure 4.63] - How Spillman-style payment channels work

For a better understanding, imagine there is a service that trades access to a Wi-Fi access point, and there is a client 
that wants to get a 24 hours access to the network; suppose the service costs one coin. The client does not trust the 
service and therefore wants to pay for the traffic per second.

The service and client decide to open a payment channel for 24 hours with the full payment amount of one coin. The 
service generates a new key pair for the digital signature and passes the public key to the client. The client, in turn, 
generates a new key pair and uses his public key and a public key of the service to create a 2-of-2 multisignature 
address. After that, the client creates a transaction number one, through which he sends 1 BTC to the multisignature 
address. The client signs it but does not send it to the Bitcoin network since the service can potentially deceive him 
and refuse to sign any transactions for the further transfer of one bitcoin.

Therefore, the customer creates a transaction number two, in which 1 BTC is sent from the multisignature address to the 
address that he controls himself; at that, he sets the nLockTime field in such a way that the transaction can be 
confirmed only in 24 hours. He does not sign this transaction but sends it to the service, which, in turn, agrees that 
the client can take the entire coin but no earlier than a day later, and then signs the transaction with its key. The 
service passes the signature to the client, and the client checks it. Now the client has an opportunity to sign the 
transaction with his key and take the coin back guaranteed in case company decides to refuse its service. So basically 
the second transaction acts as an insurance against fraud and it has to be signed even before the funds are sent to the 
service. Such an insuring method is made possible through the proper use of the locktime mechanism. And it is for this 
reason that we can affirm that a user actually “owns” certain amount of coins on the multisignature address right after 
its funding.

The next step is to distribute the first transaction to the Bitcoin network or send it to the service so that it would 
distribute it in case a customer does not have a connection. After the first transaction is confirmed, the payment 
channel is considered open.

In this case, transaction 1 is called a *funding* transaction, and transaction 2 is called a *refunding* transaction.

How is the payment settlement within the payment channel work? Consider the following scheme (Fig. 4.64).

[Figure 4.64] - Coins settlement in the channel after a payment is conducted

In order to send the first payment, a client requests the bitcoin address of the service, which the service controls 
itself. Next, he creates a transaction 3, in which the coin from a multisignature address is distributed between two 
outputs. The first output is a payment to the address of the service for, let's say, 1 second of operation of its WiFi 
access point. The second output is the transfer to the client's address. The client signs the transaction 3 with his own 
key and passes it to the service. The service checks the validity of a transaction and signature and then accepts the 
payment; it can guaranteedly receive the payment for the first second of its traffic if it finally signs this 
transaction in 24 hours. If the service intends to continue providing its services to a customer and receive payments 
in the channel, then it simply saves the transaction 3 locally until the channel is closed.

In order to send next payments, the client changes output values of a transaction number three, re-signs it, and then 
all that he needs to pass to the service is only his signature and the amount of change of the payment. The service also 
checks the received data and saves the new version of a transaction number three—in this version, it receives more coins 
(see Fig. 4.65).

[Figure 4.65] - Publication of results on the Bitcoin network

How do you close the channel? The diagram shows that the service must succeed to publish the latest version of 
transaction 3 on the Bitcoin network before the end of the channel operation time. Otherwise, the sender may cheat and 
withdraw the entire amount to his address by finally signing and publishing transaction 2.

Note that the client can publish the refunding transaction at any time while the channel is open and set in it any 
amount of transfer within one bitcoin (the amount on which the channel was initially opened). If the refunding 
transaction is published on the mainchain and the time set in its locktime field has already come, then the transaction 
will very likely be confirmed. Therefore, the service constantly monitors whether the customer published a refunding 
transaction to the network or not; in case he published, the service breaks the contract with the client closing the 
channel ahead of schedule (the time set in the locktime) by publishing the latest version of transaction number three. 
The third transaction can be considered valid since it is signed by the client. This is how the service can be confident 
that the client will not be able to deceive and steal the money.

### Application of payment channels
There are two ways of applying payment channels. The first and the basic one is to use them for regular payments between 
pre-established parties. And the second is through the commutation of channels, the LN case.

Commutation can be defined as the ability to make payments between users who did not open a payment channel with each 
other directly but have opened channels with other users. In such case, coins are generally transmitted through a chain 
of channels of users who do not know each other.

In the case of LN, there are additional complications and peculiarities. The complications are associated with the 
development of a generally accepted format for the commutation of channels and the nodes communication protocol. It is 
also important that the wallets of particular developers could operate with the wallets of others. Another and probably 
the most crucial difficulty is the routing issue. The problem is that you need to find the shortest way for transferring 
the value while considering the amount of coins and possible restrictions on the transfer in each channel.

### Features of the Bitcoin network and Lightning Network operation 
On the Bitcoin network, full nodes exchange data about transactions, blocks, and network addresses of each other. There 
are also lightweight nodes that only receive the information they need without having to process and store the entire 
history. As a result, a consensus is reached and a shared database is formed.

*Lightning Network* implies a reliable routing of a payment from the sender to the recipient through a set of opened 
*bidirectional payment channels*; moreover, these routing sets can be established through channels of outsiders who know 
neither the sender nor the recipient of a payment. The LN nodes do not exchange transactions and do not reach consensus 
as in Bitcoin. Therefore, each node of LN should obtain up-to-date information about its transactions either directly 
from the Bitcoin network nodes or from the trusted sources.

The idea of LN began to form together with the appearance of payment channels in Bitcoin [73]. At approximately the same 
time when the payment channel idea was first proposed, the Bitcoin Core developers, Peter Todd and Gavin Andresen, 
started considering the creation of payment networks on top of the primary accounting system. However, only in 2015, the 
world saw a work called "The Bitcoin Lightning Network: Scalable Off-Chain Instant Payments" [75].

At the time the LN whitepaper was published, the idea it described was incompatible with the current version of Bitcoin; 
therefore some changes in the protocol were required. The first changes were the addition of operations 
OP_CHECKLOCKTIMEVERIFY and OP_CHECKSEQUENCEVERIFY, which allowed blocking coins for a certain period of time. 
Additionally, the SegWit update was essential, which was activated in summer 2017 (see section 4.6). Officially, the LN 
entered at the alpha stage in January 2017 [76]; its first full implementation appeared at the same time [77; 112].

As mentioned earlier, in summer 2017 the SegWit update was released and became the basis for the introduction of LN. 
Three months later Blockstream announced the first successful transaction through LN [114]. In November, the first 
transfer of coins between the Bitcoin and Litecoin networks was completed. In December, the leading teams of 
Blockstream, Lightning Labs, and ASINQ announced that their implementations were fully compatible.

The edges in Figure 4.66 denote the network connections between nodes of Bitcoin and Lightning networks. The arrows in 
the figure indicate the interaction of these nodes, which is required for the actualization of the states. The nodes of 
LN need to update each other's information about their status and exchange messages to support the operation of payment 
channels.

[Figure 4.66] - Message exchanging between Bitcoin network and Lightning Network

### How does Lightning Network work?
To understand the basic idea of LN, consider the following example. Suppose there is a grocery store in town that 
enables its customers to pay in bitcoins. Moreover, this store gives an opportunity for regular customers to open 
bidirectional payment channels with it so that both could save on fees and transaction confirmation time. Alice and Bob 
are **regular** clients and both have opened the channels with the store.

The idea of LN is that not only Alice and Bob can provide payments to the party with which they have opened the channel 
(in our case, a grocery store), but also they can transfer coins to each other by using this party as an intermediary. 
The important point is that a client does not need to trust this intermediary (in practice, the number of them is most 
often more than one).

As we have described above, specific payment channel implementations are designed in a way to prevent the fraudulent 
behavior at the root (an attempt to steal coins of an interacting party may end up losing all your funds). Lightning 
Network operates on the same principles. Intermediaries who “pass” your funds are technically prevented from the ability 
to steal your coins. This is what makes payments in Lightning Network atomic: the transfer of coins is either performed 
successfully and completely or not performed at all. The basis for such a trustless interaction became the proper 
implementation of a locktime mechanism into the LN protocol (more details will be discussed in the next volume of this 
book).

To have a clearer understanding about how LN works, now consider the example (Fig. 4.67) that shows how coins are 
transferred from Alice to Bob, while they do not have a payment channel opened with each other directly.  

[Figure 4.67] - Simplified example of how nodes are arranged in Lightning Network

The figure schematically presents a small network of 6 users, each with a node. In practice, of course, there could be 
hundreds of them. Suppose that Alice needs to transfer 2 BTC to Bob. In such a case, the software of her node will need 
to find the most optimal way to “pass” the coins. Since nodes in the network constantly synchronize, Alice’s node knows 
the required information about the other opened channels such as the number of coins, the time on which the channel is 
opened, and so on.

The software of Alice’s node determines the routes by which it can transfer 2 BTC to Bob. This process is as follows. In 
the Alice—Ken channel, the state of balances is 6|10, which means that Alice has 6 BTC and Ken has 10 BTC—value transfer 
is possible. In the Ken—Den channel, the state of balances is 1|4, meaning that Ken has 1 BTC and Den has 4 BTC—value 
transfer (2 BTC) is impossible. Noteworthy, if in the Ken—Den channel, the state of balances would be reversed (4|1), 
value transfer could be completed.

[Figure 4.68] - Sending a payment from Alice to Bob

Now, there’s the second path. In the Alice—Eva channel, balances are 17|43—value transfer is possible. In the Eva—Dave 
channel, balances are 4|1—value transfer is possible (as you can see, the fact the Dave has only 1 BTC in the Eva—Dave 
channel doesn’t prevent the transfer of Alice’s 2 BTC because it is only the sender who must have the required amount). 
Eventually, in the Dave—Bob channel, balances are 17|15, meaning that value transfer is possible, and Alice will be able 
to transfer 2 BTC to Bob through two intermediaries, Eva and Dave. Figure 4.69 also vividly shows how the state of 
balances have changed after Alice has transferred her 2 BTC to Bob.

[Figure 4.69] - How the state of channel balances has changed after the payment has been transferred

In fact, in the very same situation, there’s also another possible way to send the payment from Alice to Bob. Alice can 
split the payment into two parts and send the coins separately: 1 BTC through Eva and Dave, and another 1 BTC through 
Ken and Den (Fig. 4.70).

[Figure 4.70] - Sending the same payment but separately

In Figure 4.71, you can see how the state of balances of each channel will change provided that the payment is made in 
this way (if Alice splits her coins and sends them separately).

[Figure 4.71] - How the state of channel balances has changed after the payment has been transferred separately

In practice, the number of nodes and possible routes is much higher.  Moreover, LN is not that uniform as the Bitcoin 
network. This means that there are nodes with the greater and lower load as well as nodes with inconsistent activity. 
In Figure 4.72, you can see the relevant state of LN in December 2018 [78]. 

> **Statistics of the Lightning Network in 2018**
>> * Estimated 4,500 nodes
>> * 14,000 channels opened on average
>> * 479.70 BTC locked on the Bitcoin network (are in the circulation of LN)

[Figure 4.72] - State of Lightning Network in the end of 2018

To open a payment channel, you need to freeze a certain amount of coins; thus sending and accepting payments in channels 
is possible only within a limited, predetermined sum. If an ordinary user divides her coins into several parts and 
correspondingly opens several channels, she will only be able to operate within a limited amount of coins in each of the 
channels. This is why large organizations such as wallet developers, centralized exchanges, or popular merchants will 
serve as *hubs*. They can afford maintaining a huge number of channels opened on large sums and for long periods of time 
without going offline. Ordinary users will very likely open at most one or two payment channels with one of these hubs.

Based on the above-mentioned facts, let's define the basic functions of LN nodes.

> * Routing
> * Opening channels
> * Making transfers between intermediary nodes
> * Closing channels

Routing is one of the most complex functions of LN nodes. Every LN node determines the optimal path of a payment, 
considering factors such as the capacity of a specific channel, the sum on which it is opened, how these parameters 
could possibly change as the payment passes over the network, and a “backup plan” in case some of these channels 
suddenly shut down.

Also, every network node must make sure that it has open channels with other nodes. Some channels are particularly 
important for the network, and the loss of them may cause both malfunctioning and network breaches. To avoid this, every 
node should maintain connections with as many nodes as possible.

Perhaps, the paramount function of the network nodes is to perform payments. The size and number of payments a node is 
able to perform directly depend on the width of channels opened by this node.

Another function of network nodes is to close the payment channels. In LN, there are so-called dominant nodes (with a 
huge number of opened channels). In some sense, these nodes could manipulate the flow of funds by opening or closing 
their payment channels. This issue can be solved by increasing the number of channels between all LN nodes. This 
approach, however, would be quite expensive for the owners of nodes since they pay fees for opening and closing their 
channels, and they should also have a sufficient amount of coins to have it frozen and provide the operation of their 
channel.

**Frequently asked questions**

*– How much are payment transfers in the channels reliable compared to usual Bitcoin transactions?*

The reliability of payments in channels can be compared with ordinary bitcoin transactions; coins will not be taken 
away, and the payment will not be canceled. However, there are a number of peculiarities such as the need to open and 
close the channels on-time, limiting the amount in the channel, the need to constantly synchronize with the Bitcoin 
network, the probability of freezing of coins on a certain period of time, etc.

*– Is the capacity in the Lightning Network limited?*

The fact is that there are no limitations, but there may be delays related to channel processing, network exploration, 
and route planning. It depends on the performance of specific network nodes. It should also be taken into account that 
nodes can unpredictably go offline, and this can entail certain restrictions in making payments to other participants.

*– Should channel members trust each other?*

No, the mechanism of payment channels provides protection from malicious actions of interacting parties according to the 
protocol.

*– What is the use of channels to a person who wants to send only one payment?*

If a person wants to get rid of the last coins and no longer plans to accept and send payments, then it makes no sense 
to open the channel; instead, it would be better to send a regular on-chain transaction. In all other cases, the opening 
of the channel will be useful. For example, it can reduce the fee and the time of transaction confirmation.

*– What is the difference between a sidechain and payment channels?*

A *sidechain* is a separate chain of blocks that accounts for coins from the *mainchain*. Rules in a sidechain may 
differ from the rules in the mainchain and be determined by the new protocol. Sidechain does not have a base currency, 
but it implements the coins transfer mechanism from the main chain of blocks. At the same time, the payment channel is 
not a separate chain of blocks and does not implement the transfer of coins from the mainchain. It is simply a method of 
carrying out multiple payments without having to process transactions by validators (only the two transactions are 
confirmed for each payment channel: an opening one and a closing one).











































