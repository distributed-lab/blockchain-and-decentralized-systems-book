# 7 EVOLUTION OF DECENTRALIZED TECHNOLOGIES


## 7.1 Bitshares protocol design

The decentralized platform Bitshares implements a cryptocurrency, smart contracts, a trading platform and a lot more interesting functionality. Bitshares is an accounting system that moves between the poles decentralization and capacity [61]. In addition, the Bitshares protocol was taken as a basis in other decentralized systems, such as Steemit and EOS.

The idea of the Bitshares protocol is as follows: a tool is created that enables trading various assets and currencies in a decentralized environment without actually depositing them on the trading floor.

### Purpose of the Bitshares platform

Daniel Larimer (also known as Bytemaster) is the main ideologist and developer of the protocol. The Bitshares platform allows anyone to create user issued assets (UIAs) or digital tokens. Thus, the platform accounts for the base currency (BTS cryptocurrency) and many user issued tokenized assets.

The protocol implements a decentralized exchange where these digital assets can be traded. When designing the accounting system and the consensus mechanism, the developers emphasized system capacity.

Bitshares positions itself as a smart contract platform. It's noteworthy that the smart contracts are predefined herein, and their number is limited (the most popular contracts are implemented). The smart contracts are more energy efficient and, consequently, cheaper in terms of fees.

Another feature of the platform is the support for payments with a high level of user privacy, which can be used optionally. At Bitshares, this technology is called stealth transfers.

### Account model

Now let's look at how the accounts are organized in the Bitshares protocol. It uses elliptic curves cryptography, and the curve itself is exactly the same as in the Bitcoin protocol. The address format uses the hash value of the public key, base 58 number system, the BTS prefix and does not contain a checksum. This format, however, is rarely used since the general platform database is optimized in a way that each object, including the user account, has its own unique identifier, which is an integer of 8 bytes (or 64 bits) in size. Usually, when sending a payment, such an account identifier is used.

In addition, the protocol supports the registration of unique names. Similar functionality was first implemented in the Namecoin protocol. Thus, in Bitshares you can register a line that is convenient for human perception, which will be unique within the corresponding blockchain system, and link to your account in order to use it instead of an account ID.

### Transaction model

Let us examine the Bitshares transaction model in more detail (Fig. 7.1).

<img width="55%" alt="Figure 7.1 – Transaction structure in Bitshares protocol" src="/resources/img/volume-2/7.1-Bitshares-protocol-design/Figure-7.1-Transaction-structure-in-Bitshares-protocol.png"/> 

The figure shows that the transaction body consists of five main fields. The first two fields of the transaction are necessary in order to bind it to a specific block. This is necessary to determine the chain of blocks into which this transaction can be added because according to the rules of the protocol, a transaction cannot be confirmed in that chain to which it is not attached. The *expiration_time* field sets the time until which the transaction can be added to the block. If a transaction was not confirmed before this time, it is considered to be invalid and can no longer be included in the chain of blocks.

The *operations_vector* field is different. Its main feature is that you can put various different operations into it. *Operation* is another key object in the Bitshares protocol. These are  some of the most popular types of operations: *transfer* (transfer), *account_update* (updating an account), *asset_issue* (issuing a tokenized asset) and *order* (trading offer). Each operation has its own format and the required parameters. For example, the *transfer* operation requires the sender’s account, type of asset, transfer amount and recipient’s account. The operations themselves are independent of each other but can only be performed together if the transaction is accepted.

The *extensions* field is necessary so that the current version of the software can process new version transactions, where additional fields are added. Of course, the old software will not know how to correctly verify additional fields of new transactions, but it is able to correctly process transactions according to the old rules.

This is an unsigned transaction format. To correctly sign the transaction, you need to analyze all operations from the *operations_vector* field and make a list of accounts that must confirm the transaction. Then it becomes clear which keys you need to sign the transaction with. All necessary signatures are placed in a separate field – *signatures*. If a minimum of one signature is not provided, then the entire transaction is considered incorrect.

Note that by optimizing the size of identifiers, the final size of a transaction that contains one operation will be approximately 100 bytes. This is indeed a very compact transaction compared to transactions in other protocols.

Regarding the fees, the Bitshares protocol implements a special approach: each operation requires a certain fee, which is paid from the balance of the initiating account at the time of the transaction confirmation. The fee for operations is typically a constant but may vary. For a rough comparison, the fees for ordinary transfers and trade are much lower than the fees for issuing new assets and registering a new account.

### Decentralized asset exchange

Now let's see how the trading of assets works, which are issued and recorded on the Bitshares platform. The user can make a transaction with such an operation, where the user declares that he is ready to exchange one asset for another in a certain ratio for a certain amount.

This transaction is distributed over the network and receives confirmation, after which another user can declare in the same way that he wants to exchange the same assets in the same ratio. At the moment of the second transaction confirmation, the balances of these two users are updated, following the protocol. For example, an actual exchange of assets is made on the basis that both users have signed the corresponding exchange statements.

Since such trading is taking place in a decentralized accounting system, this trading platform is called a decentralized exchange. However, Bitshares does not implement a cross-exchange mechanism, which allows the user to exchange an asset not directly but through the chain of orders.

### Account management flexibility

Another important feature of the Bitshares protocol is its flexible account management. The system of dynamic account permissions enables users to specify account management by several keys, based on the multisignature principle. This is implemented so that each account can be controlled by a balanced combination of other accounts or digital keys.

This approach allows you to create a hierarchical management structure, which organization is similar to a permit system in real life. It turns out that you can organize multi-user management of an account and its balances, where each user will have a certain weight in making a decision. Moreover, for different operations, you can set different criteria. This management mechanism can significantly reduce asset theft risks and the loss of control over an account (Fig. 7.2).

<img width="50%" alt="Figure 7.2 – Dynamic account management scheme" src="/resources/img/volume-2/7.1-Bitshares-protocol-design/Figure-7.2-Dynamic-account-management-scheme.png"/> 

Schematically, it looks like this: at the top of the hierarchy, there is a main account that requires the confirmation of several accounts and keys in order to confirm transactions. Lower ranking accounts - in terms of hierarchy -  are typically called signers. Each signer has its own weight in the respective operation’s confirmation. For example, in the diagram above, signers have a weight of 25, 40, 35, and 40 units, and the necessary threshold for confirming a certain type of operation can be 50, 60, or 70 units. At the same time, for other types of operations, a different distribution of weights and a different threshold value may be required.

How does it work at the transaction level? One of the signers creates a transaction with certain operations and confirms it on behalf of his account. The operations that are included in this transaction are not performed but are waiting for further confirmation. Then other signers see the offer and can either approve or reject it on behalf of their account by using transactions that contain specific operations.

### UIA issuance

Let us examine how user assets are issued on the Bitshares platform in more detail. Anyone can form a transaction by creating a new asset. The user has to pay a certain fee, set the asset parameters, and then issues the corresponding tokenized assets. This protocol functionality is implemented as a pre-installed smart contract with some standard functionality.

Upon the issuer’s request, the KYC (Know Your Customer) procedure can be implemented using a white list mechanism, a list of pre-approved accounts and the issuer‘s additional confirmation, the actual approval. The white list contains accounts that are pre-approved by the issuer to receive and hold the respective asset tokens. An additional confirmation mechanism allows the issuer to control each transaction for the transfer or trade of tokens, which means the issuer can reject or approve each of his assets’ transactions.

In addition, the issuer may restrict the trading of tokenized assets and allow only for the storage and transfer. Alternatively, transfers can be restricted so that only trading is allowed. The issuer may also establish additional fees for transferring and trading these assets.

Moreover, the issuer can activate the function of withdrawing and redistributing tokens. This works in cases where external mechanisms for condemning transactions and rolling back payments are needed. Note that all asset properties and restrictions are determined by setting the smart contract parameters accordingly. Further, the issuer determines which properties can be changed and which ones are permanent. For example, issuing additional  tokens may be limited or may allow issuing new tokens arbitrarily. These contract parameters are visible to all users.

There is another  interesting aspect for assets tokenized on the Bitshares platform: when executing a transaction, the user can pay the transaction fee either with the base currency or with the very token itself. The conversion is based on the exchange rate set by the issuer.

### Voting mechanism

The Bitshares platform employs voting, a mechanism that helps make decisions in a decentralized environment. Committee members, validators (witnesses) and developers (workers) are all elected by this very voting process. The committee members are required to vote in order to change protocol parameters such as fees, number of validators, etc. Validators, or delegates, are responsible for transaction verification and blocks formation. Developers offer software improvements; if they get a sufficient number of votes and adhere to the development deadlines, they receive a reward.

In the voting process, the weight of the vote of each account is proportional to its balance in the base currency. At the same time, a proxy voting option is supported, which allows you to transfer your vote to another voter. This is convenient when the user himself does not know which of the candidates he wants  to vote for, and gives this right to a more competent user. In general, voting is paramount to ensure the reliability of the platform.

### Fees mechanism

Now let's look at the mechanism of paying fees for transactions and rewards of active network participants. There is a *reserve pool*, a balance that does not belong to any platform users, and it can only be managed according to the rules of the protocol. The rules stipulate that the transaction fees from all accounts go to the reserve pool. The accounts of validators and developers are paid a percentage of the reserve pool‘s balance. In addition, there is a referral program that can be superimposed on the collection and distribution of fees (Fig. 7.3).

<img width="50%" alt="Figure 7.3 – Scheme of fees payments and distribution" src="/resources/img/volume-2/7.1-Bitshares-protocol-design/Figure-7.3-Scheme-of-fees-payments-and-distribution.png"/> 

### SmartCoins

Earlier, we explored a smart contract that implements ordinary tokens, now, we discuss another smart contract that implements *market pegged assets* – tokens pegged at an external asset. They are also called SmartCoins. This works on the basis of a *contract for difference*. Accordingly, anyone can issue these tokens simply by freezing a certain amount of, for example, base currency as a security deposit. The collateral ratio is significantly higher than usual, and the recommended value is 2 or more. In a SmartCoin system, everything works according to the principles of margin trading, where margin positions and automatic margin calls are used. In order for the asset price data to appear on the platform in foreign markets, trusted parties are used that feed the data onto the Bitshares blockchain using a special type of transaction. Here, these trusted parties are the validators themselves.

### Block header format

The block header in Bitshares has a simpler structure and smaller size than the block header in Bitcoin (Fig. 7.4).

<img width="55%" alt="Figure 7.4 – Block header in Bitshares protocol" src="/resources/img/volume-2/7.1-Bitshares-protocol-design/Figure-7.4-Block-header-in-Bitshares-protocol.png"/>

The following fields are included: identifier of the previous block (*previous_block_id*), timestamp, creator identifier (*witness_id*), hash value of incoming transactions (*merkle_root*) and extension field to maintain compatibility with new versions of the block header.

The complete block will include the header of the block, the signature of the validator that created this block, and the list of transactions included in this block.

Within the Bitshares protocol, there are 4 concepts that play a key role in the operation of the platform: blocks, transactions, operations and objects. The objects herein are *account*, *asset*, *balance*, *offer*, and other.

### Subset of operations and features of their implementation

Let’s consider in more detail the concept of operation in the context of this protocol. In the Bitshares protocol, at the time of 2019, there are about 40 different types of operations (new operation types can be added during each new protocol update). Operations allow you to initiate some simple changes in the accounting system and enable more complex mechanisms such as predefined smart contracts. As explained above, one transaction can contain several operations that, if successful, will be executed simultaneously.

Here is a short list of the most popular operations:

* *transfer_operation* transfers a specific asset from the balance of one account to another;
* *limit_order_create_operation* creates an asset exchange request object;
* *Limit_order_cancel_operation* cancels the above request;
* *fill_order_operation* executes automatically when two applications correspond each other; it does not require the creation of a transaction; therefore, it is also called a virtual operation;
* *account_create_operation* creates a regular user account;
* *blind_transfer_operation* makes a confidential payment;
* *asset_create_operation* creates an object of a new asset;
* *asset_update_operation* changes the smart contract settings of an existing token;
* *asset_issue_operation* issues tokens of an existing asset;
* *witness_create_operation* creates a validator candidate account.

Regarding statistics, the load on the Bitshares network in terms of the number of transactions is comparable to the load on the Bitcoin or Ethereum network. Moreover, at some points in 2018, the Bitshares platform processed more transactions than Bitcoin and Ethereum combined. The results of load testing show that the maximum capacity of the Bitshares network is hundreds of times higher than that of the Bitcoin and Ethereum networks.

### Database organization

Now, let's look at one important architectural feature of the Bitshares protocol, which helps to achieve such a high capacity (Fig. 7.5).

<img width="50%" alt="Figure 7.5 – Blockchain and ledger database organization" src="/resources/img/volume-2/7.1-Bitshares-protocol-design/Figure-7.5-Blockchain-and-ledger-database-organization.png"/> 

The diagram on the left shows a way to organize a database called blockchain, which organizes the storage of the entire transaction history. On the right is another way to organize the database - ledger. It organizes the storage of the correspondences between identifiers and states (for example, an account and its balance).

The blockchain properties make it easy to check the integrity of the database and the history of its changes. On top of that it easily organizes synchronization and achieves consensus in a decentralized environment.

A ledger is convenient in that it compactly stores the latest state of the database and at the same time provides quick search functionality, reading and updating of records. That is the reason for its higher capacity. Ledger is commonly used in centralized accounting systems.

The idea of Bitshares is to take advantage of both ways of organizing a database at the same time. Thus, a complete network node records transactions simultaneously using two databases organized by blockchain and ledger type, respectively. At the same time, the nodes synchronize data with each other using blocks and update the local copy of the blockchain. They also carry out the verification and acceptance of transactions and focus on the last state of the database that the ledger stores. Moreover, the size of all the data that makes up the ledger is optimized so that the network nodes can keep them in RAM. This allows for the significant speed up in the process of transaction verification and adoption of new transactions.

### Optimization of business-logic execution

Many accounting systems use a general-purpose scripting language to define all operations. These accounting systems ultimately use the “business logic processor” as a virtual machine, and all transactions are defined as scripts that will be run by the virtual machine. This approach uses the mechanisms of synchronization of threads of a real processor and combines them, executing all instructions through a virtual processor. A virtual processor, even with just-in-time compilation, will always be slower than a real processor, but the final rate of calculations is not the only problem of this approach when everything is a script.

When transactions are defined at such a low level, this means that most of the static checks and cryptographic transformations remain in the business logic processing and the overall throughput drops. To increase the performance of the accounting system and quickly execute the business logic, all static checks, including all cryptographic transformations, must be taken out of the scope of the business logic module.

Another optimization step is storing the accounting system final state data in RAM. This means that the "business logic processor" can quickly follow the pointers to the memory area and directly receive the data it needs, and not perform time-consuming database queries. It also means that the data can be accessed without copying, and it can be rearranged location-wise. This optimization provides a performance boost within the use of a database-based approach.

Thus, the creation of an accounting system with high performance does not require complex technologies and separation of processes among network nodes. All that is needed to create a high-performance validator node is the separation of all independent calculations from the main business logic, its execution in one thread without interruptions for synchronization and storage of all verification dependencies in RAM.

### User privacy options

It was noted above that the Bitshares accounting system uses accounts and balances, unlike Bitcoin, where accounting is based on unspent transaction outputs (UTXO). Increasing privacy on the Bitshares platform is not a trivial task since in the context of accounts and balances, it is even easier to deanonymize users than in Bitcoin.

Nonetheless, the developers of Bitshares solved this problem in an interesting way. They implemented a function to specify multiple inputs and outputs in a single operation. Now, you can include many inputs and outputs in one transaction, which complicates the analysis of cash flows and increases users’ privacy.

In addition, these operations use confidential transactions and stealth addresses by default. Confidential transactions hide the amounts of inputs and outputs but use the proof that the sum of the outputs does not exceed the sum of the inputs. Stealth addresses, however, hide the connection between the used public key as the recipient identifier and the specific address that is indicated in the transaction output.

Bitshares defaults to regular (non-confidential) payments, but users can use stealth transfers if they wish. Thus, the accounting of coins on the platform is carried out in two different ways.

Coins can be transferred from one accounting method to another. For this, separate operations are implemented:

* *transfer_to_blind_operation* used to transfer coins from open to hidden;
* *blind_transfer_operation* to transfer coins in a hidden way;
* *transfer_from_blind_operation* to transfer coins from hidden to open.

It is clear that these operations are larger than usual in terms of data volume and, accordingly, require a larger fee.

It is noteworthy that anyone can calculate how many coins went into hidden circulation, so there is the concept of stealth supply (the number of coins in hidden circulation). Regardless, there is one practical flaw in the privacy option at Bitshares at the moment (July 2019). The fact is that the graphical interfaces for work in high privacy mode are still unobvious and inconvenient for a regular user.

**Frequently asked questions**

*– Does Bitshares support the ability to set arbitrary conditions for spending coins, for example, using Bitcoin Script?*

No, the current version of the protocol does not support this functionality. It is unlikely that it will be added in the future because in Bitshares it is most convenient to create new types of operations and introduce them in subsequent updates of the protocol.

## 7.2 Ethereum platform and smart contracts

Ethereum is a decentralized platform designed to execute arbitrarily programmable smart contracts [59]. The Ethereum project has an open-source code and a fairly large community (developers, testers, smart contract developers, users). Ethereum implements a distributed database for storing and synchronizing data on the statuses of user accounts and smart contract accounts, as well as the base currency of the ether platform (cryptocurrency).

The Ethereum platform allows smart contract developers to create custom, decentralized applications with built-in economic features. For smart contracts, the platform implemented its own programming language, Solidity.

### Features of Ethereum platform operation

Let's consider the type of accounts that exist on the Ethereum platform. There are only two types: user accounts and contract accounts. Let's see how they differ.

A user account is only managed by a private digital signature key. The account holder generates his key pair for electronic signature using the ECDSA algorithm. Only transactions signed by this key can change the state of this account.

The smart contract account, however, has a different logic. It can be controlled only using a predetermined program code that completely determines the behavior of the smart contract: how it will manage its coins under certain circumstances, namely under which conditions these coins are distributed as well as by which user this distribution is initiated. If some points are not provided by the developers in the program code, problems may arise. For example, a smart contract can go into some specific state in which it does not accept the initiation of further execution from any of the users. In this case, the coins, in fact, will be frozen because the smart contract does not provide a solution to resolve this state.

### Creating accounts in Ethereum

Noteworthy, Ethereum uses the exact same algorithm and elliptic curve as Bitcoin for digital signatures, but the address is calculated in a slightly different way. 

In this case, the result of double hashing is no longer applied as in Bitcoin, but a one-time hashing is provided by the KECCAK function at a length of 256 bits. The least significant bits are cut off from the received value, namely the 160 least significant bits of the output value of the hash function. As a result, we get an address in Ethereum with a size of 20 bytes.

Note that unlike Bitcoin and many other systems, the account identifier in Ethereum is encoded in hexadecimal without a checksum, where the address is encoded in the base number system 58 with the addition of a checksum. Hence you need to work with account identifiers in Ethereum carefully: even a single error in the identifier leads to the coins’ loss.

Another important feature is that, at the general database level, the user account is created at the moment when the user accepts the first incoming payment.

There is a completely different approach to creating a smart contract account. First, one of the users writes a source code of the smart contract and upon completion, the code is passed through a special compiler for the Ethereum platform, and the byte code for the Ethereum virtual machine is obtained. The received bytecode is fed in a special transaction field. This transaction is verified on behalf of the initiator's account; it is then spread over the network, and it hosts the smart contract code. The fee for the transaction and, accordingly, for the execution of the contract is removed from the balance of the initiator's account.

Each smart contract necessarily contains its own constructor — it may be empty or may have content. After the constructor is executed, a smart contract account identifier is created, which can be used to send coins, call smart contract methods, etc.

Any account contains four fields:

* *nonce* – a counter that displays the number of confirmed transactions initiated by this account;
* *balance* – the number of wei that belongs to the address;
* *storageRoot* – 256-bit hash value of the root node of the Merkle Patricia tree, which covers the contents of an account (in usual accounts, the field is empty in accounts; in the contract accounts, nonetheless,  it stores the root hash value from the state of an account);
* *codeHash* – hash value of an EVM code, that is, the value of the code that will be executed when this account receives a call message. This field cannot be changed after creating an account.

EVM code is a binary code of a smart contract that runs on the Ethereum virtual machine. Typically, a smart contract is written in Solidity, but the virtual machine that executes the contract does not understand this language, so you must first compile the code into machine-readable EVM code.

EVM is stack-based, that is, the data is pushed onto the stack, and the operators [60] operate directly with the values in the stack.

### Call message in Ethereum 

Contracts in Ethereum can send “messages” to other contracts. Messages are virtual objects that are not serialized and exist only on the Ethereum network. Each message contains the following information:

* message sender;
* message recipient;
* number of coins transmitted with the message;
* field for optional data;
* STARTGAS value (the number of maximum steps that are allowed to be performed for a transaction).

A message is very similar to a transaction, except that it is triggered by the contract, not by the user. Messages are generated when the executing contract calls the CALL code that creates and executes the message.

### Fees and gas 

Unlike many other accounting systems, fees for transactions in Ethereum are paid regardless of whether the transaction was successfully completed or not (i.e., whether the code contained in it was fully executed). Even if the transaction is not successfully confirmed, validators have already begun processing it (verified and sent its code for execution to the virtual machine); accordingly, these calculations must be paid.

The maximum transaction fee for a transaction is defined as the product of *gasPrice* and *gasLimit*. The *gasLimit* parameter is determined directly by the sender of the transaction and contains the maximum number of gas units that the user is willing to pay for a transaction. This restriction is necessary due to the nature of smart contracts in Ethereum. Since Ethereum supports Turing-complete smart contracts, it is possible that, due to an error in the code, the contract can be executed endlessly (and spend all the user's coins). More on that, it is impossible to claim exactly how much gas will be spent on the transaction (i.e., correct code completion) before this transaction is de facto processed and confirmed. This is because the branches in the smart contract code may depend on the latest current status of other accounts. At the same time, it is important to understand that if the user does not have enough gas units that he is ready to pay to fulfill the contract, the smart contract will not be fully processed by validators. Accordingly, the transaction will not be confirmed, but the user will still spend coins to pay the fee. Note that all unused gas is converted back to ether and returned to the sender's account balance after the transaction is correctly processed.

If the user does not want to pay a lot for the transaction (or, on the contrary, wants to pay more), then there is the gasPrice parameter, where the transaction initiator determines the amount of wei that he is willing to pay for the gas unit. The higher this value, the more willingly the validators will start processing the transaction, since adding it to the block is much more profitable than adding a transaction with a relatively small gasPrice value. Actual transaction fee is calculated as follows:

**transactionFee = gas * gasPrice.**

### Ethereum transaction structure

To make it clearer, we will begin to review the structure of the Ethereum transaction and the sample smart contract code. The Ethereum transaction consists of the fields shown in Fig. 7.6.

<img width="50%" alt="Figure 7.6 – Ethereum Transaction" src="/resources/img/volume-2/7.2-Ethereum-platform-and-smart-contracts/Figure-7.6-Ethereum-Transaction.png"/> 

The *nonce* field is a sequence number of the transaction relative to the account, which distributes the transaction and is basically its author. This is necessary in order to distinguish duplicate transactions, which is necessary to ensure the same transaction is not accepted twice. Due to this, each transaction has a unique hash value.

Next goes the *gas price* field. Here you can see the price at which the ether base currency is converted to gas, which pays for the execution of the smart contract commands and the allocation of the virtual machine resource. What does it mean?

In Bitcoin, fees are paid in the base currency – in bitcoins. This is possible due to a simple mechanism for calculating their size: we pay strictly for the amount of data contained in the transaction. In Ethereum, the situation is more complicated, because specifying the amount of transaction data isn’t enough. Here, the transaction may still contain the program code that will be run on the virtual machine, and each operation of the virtual machine may have different complexity and execution time. There are also operations that allocate memory for variables and data arrays. They will also have a certain complexity on which their payment will depend. Moreover, at the stage of transaction formation, it is not known how many operations will be executed and what specific operations will be performed at the time of acceptance of this transaction by validators.

The cost of each operation is calculated in the gas equivalent and will be constant. It is introduced specifically to determine the constant cost of each operation. The gas price parameter is set depending on the load on the network. The respective value of the base currency will be converted to gas in order to pay fees.

There is another feature of a transaction in Ethereum: the bytecode that it contains in the virtual machine will be executed until it finishes with some result (successful/unsuccessful) or until a certain number of coins are allocated to pay fees. The *start gas* field (it is often called *gas limit*) is provided in order to avoid a situation where all coins would be spent on a fee from the sender’s account due to some kind of error (for example, an infinite cycle began to run in a virtual machine). It determines the maximum amount of coins that the sender is willing to spend on a specific transaction.

The next field is called *destination address*. This includes the address of the recipient of coins or the address of a specific smart contract, the methods of which will be called. Next is  the value field, which contains the number of coins that are sent to the destination address balance.

Next is a field called *data*. The field not only contains value, but the entire structure that defines the code for a virtual machine. In addition, you can put arbitrary data in this field; for this, there are special rules for filling the structure.

The last transaction field is called *signature*. It simultaneously contains both the digital signature of the initiator of this transaction and the public key with which this signature is verified. From the public key, you can get the account identifier (address) of the sender of this transaction, that is, uniquely identify the sender account in the system itself.

### Transaction processing

After sending a transaction to the network, it enters the mempool of each node, and if the owners of the majority of the processing power of the Ethereum network accept it, then it is included in one of the new blocks. Note that in order to confirm a transaction in Ethereum, a certain number of blocks must be formed based on the one that includes the given transaction. In practice, this number is 6–20 blocks. Strictly speaking, the number of transaction confirmations is determined by the party that will accept the changes made by the transaction to the state of its account. For example, a transaction might add 1000 ether to Alice’s account balance (the amount is quite large), then Alice decides that she will wait for 35 more blocks following the one that confirms this transaction.

Each Ethereum block has at least one transaction – a reward for the formed block. This award, like the fee, is given to the validator node, which was the first to solve the resource-intensive task. Indeed, the award is not issued immediately but after the appearance (formation) of a certain number of blocks.

Also in Ethereum, there is such a thing as *uncle blocks*. Uncle blocks are blocks that were mined and comply with the protocol rules but that due to the network delays (or for other reasons) were received by network nodes later than alternative ones and cannot be added to the main chain. In Bitcoin, such blocks are discarded, but in Ethereum they are taken into account: transactions that do not conflict are confirmed, but the reward for finding an uncle block is less. It’s calculated as follows: 
*(U<sub>n</sub> + 8 − B<sub>n</sub>) ∗ R / 8*, where 
*R* is the base block reward, *U<sub>n</sub>* is the height of the uncle block, and *B<sub>n</sub>* is the height of the last block mined.

### Ethereum block structure

A block in Ethereum consists of the following fields (Table 7.1).

Table 7.1
<img width="50%" alt="Table 7.1" src="/resources/img/volume-2/7.2-Ethereum-platform-and-smart-contracts/Table-7.1.png"/> 

### Ethereum virtual machine 

The EVM is an important component of each node in the Ethereum network, regardless of whether a particular node is a validator or not, since it is responsible for processing the network state and performing all the necessary calculations. The EVM is responsible for processing the following information:

* balances;
* addresses;
* the state of gas;
* smart contract.

Each Ethereum node launches an EVM to maintain an Ethereum network and achieve consensus. The virtual machine must monitor the state of the listed components for successful processing and transaction confirmation, which, in turn, affect the state of a decentralized accounting system as a whole.

### Smart contract source code example

Let's now take a closer look at the simplest smart contract. The graph below shows the source code of a primitive smart contract that can hold user coins and return them on demand.

This smart contract is called Bank, and it performs the following functions: accumulation of coins on its balance sheet (that is, when a transaction is confirmed and such a smart contract is placed, a new account is created that can contain coins on its balance sheet); it remembers users and the distribution of coins between them. It has a method of replenishment, withdrawal, as well as checking the balance of a user (Fig. 7.7).

<img width="50%" alt="Figure 7.7 – Ethereum contract example" src="/resources/img/volume-2/7.2-Ethereum-platform-and-smart-contracts/Figure-7.7-Ethereum-contract-example.png"/> 

Let’s review this code line by line. There are constant fields in this contract. One of the address types is called *owner*. Here, the contract remembers the address of the user who created this smart contract. Next, there is a dynamic structure that preserves the correspondence between user addresses and balances.

After this comes the method *Bank* – the constructor of this contract. Here, the owner variable is assigned the address of the person who posted this smart contract on the platform. The *msg* structure is exactly the data that was transferred to the virtual machine along with the transaction containing the code for this contract.

Accordingly, msg.sender is the address of the transaction initiating the account that hosts this code. He will be the owner of the smart contract.

The method deposit allows transferring a certain number of coins to a contract account in a transaction. A smart contract receiving these coins leaves them on its balance sheet but adds to the balances structure who exactly was the sender of these coins to store the information who they belong to.

The *withdraw* method takes one parameter — the number of coins that the initiator wants to withdraw from this contract. Here, a check is made to see if there are enough coins on the balance of the user who called this method to withdraw funds. If the account has sufficient funds, then the smart contract sends this amount of coins to the user's balance.

The *getMyBalance* method checks the current balance of the user. The target balance is read from the correspondence structure at an address of the calling account. The modifier of this method is called view. It means that the method does not change the variables of its class (contract) in any way and, in fact, is only a reading method. No separate transaction is created to call this method, no commission is paid, and all calculations are performed locally, after which the user receives the result.

There is also the *kill* method. It is needed to deactivate the contract and destroy its corresponding state. An additional check is written here to see if the one who calls this method owns this contract. If so, then the contract is self-destructing, and the self-destruct function takes one parameter – the identifier of the account to which the contract will send all the coins remaining on its balance. In case the contract self-destructs, all remaining coins on the balance will automatically go to the balance of the contract owner.

### Contract execution on the Ethereum platform

Let's consider schematically how smart contracts are executed on the Ethereum platform and how a full network node works.

The full Ethereum network node must have at least four modules (Figure 7.8).

<img width="50%" alt="Figure 7.8 –Ethereum Full Node Modules" src="/resources/img/volume-2/7.2-Ethereum-platform-and-smart-contracts/Figure-7.8-Ethereum-Full-Node-Modules.png"/> 

The first is the P2P networking module – a network connection and work module with other nodes where blocks, transactions, and information about other nodes are exchanged. This component is required for any decentralized accounting system.

Then goes blockchain — a module that stores the chain of blocks, processes alternative chains, chooses a priority branch, supplements blocks, disconnects blocks, does the verification, etc.

The next module is called *Ethereum virtual machine*. This is the virtual machine that accepts bytecode from a transaction in Ethereum. This module processes the current state of a specific account and makes changes to its status based on the received bytecode. The version of the virtual machine on each of the network nodes must be the same to get one result. The calculations that occur on each Ethereum node are exactly the same, but on the network, they occur in an asynchronous mode: some nodes receive and verify some transactions earlier; that means they execute the code included in it, and other nodes execute the code later. Accordingly, after its creation, the transaction is propagated throughout the network; the nodes accept it and at the time of verification (similar to the execution of Bitcoin Script for transactions in Bitcoin), the byte code of the virtual machine is executed.

A transaction is considered verified if all the code contained in it was executed as well as a new state for a certain account was generated, and a new state was saved up to the point when it became clear whether this transaction was applied or not. If the transaction is applied, then this state is considered not only completed but also relevant. Each node has a database that stores the status of each account. Due to the fact that all calculations are the same and the blockchain version is the same, the database containing the status of all accounts for each node will also be the same.

### Limitations of the Ethereum platform

Let's look at the limitations that are inherent in the Ethereum platform (and other similar smart contract platforms), and, at the same time, dispel some myths. Certain restrictions take place due to the presence of a virtual machine at each network node.

> * *Code execution*
> * *Memory allocation*
> * *Access to data only from blockchain*
> * *Sending payments*
> * *Create a new contract*
> * *Call other contracts*

Indeed, arbitrary logical operations can be performed here. However, the fee is paid separately for each operation and for each additionally allocated unit of memory.

The virtual machine can read data from the blockchain database in order to use this data as a trigger for executing one or another logic of smart contracts. A virtual machine can create and send transactions, create new contracts and call methods of other smart contracts that are already published on the network, etc.

The most common myth is that Ethereum smart contracts can use information from any Internet resources in their own conditions. The truth is that the virtual machine cannot send a network request to some external information resources on the Internet, that is, you cannot write a smart contract that will distribute the value between users depending on the weather on the street or on who won the championship, or on any other incident happening in the external world. This is because the data on these incidents is simply not in the database of the platform itself. That means if such data does not appear on the blockchain, then the virtual machine cannot use it as triggers.

### Disadvantages of Ethereum platform

> * *Errors in the code (due to difficulties in the development and testing)*
> * *Virtual machine vulnerabilities*
> * *Difficulty of setting prices for operations*

The first drawback is that there are some difficulties in developing and testing smart contracts in Ethereum. The practice shows that the human factor becomes the cause of a very large percentage of errors. This is true for already written Ethereum smart contracts, which are of medium or high complexity. While the error probability is small for simple smart contracts, complex smart contracts’ errors very often lead to the theft of funds, to their freezing as well as to their premature destruction.

The second drawback is that the virtual machine itself was created by people and cannot be perfect in every way. It can execute arbitrary commands – and this is the vulnerability: you can configure a number of commands in a certain way, put them in a transaction and send them for execution, which will lead to unpredictable consequences. This is a very complex area, but there are already several studies that show that these vulnerabilities exist in the current version of the Ethereum network and they can lead to the failure of many smart contracts.

Another big difficulty, which can be considered a drawback is that it is possible to practically or technically determine some specific order of operations that, when performed together, will very heavily load the virtual machine and slow it down unproportionally to the fee that was paid for performing these operations.

It has already happened in past that various researchers found vulnerabilities during a detailed research of the Ethereum virtual machine. They paid very small fees, but in practice, they greatly slowed down the entire network. This effect was achieved by selecting a certain sequence of virtual machine commands. These problems are very difficult to solve because they need to be found first; secondly you need to adjust the price for performing these operations, and thirdly, carry out a hardfork (simultaneous update of all network nodes to a new version of the software and simultaneous coming into force of all these changes).

As for the Ethereum platform as a whole, a lot of research has been done and a lot of practical experience has been gained (both positive and negative). It was used to create other decentralized smart contract platforms such as Bitshares and EOS. Nevertheless, difficulties and vulnerabilities remain that still have to be addressed.

**Frequently asked questions**

*– If all parties associated with a smart contract want to change the terms, can they cancel this smart contract using multi-signature, and then create a new smart contract with updated conditions?*

The answer is twofold. On the one hand, a smart contract is set once, and in the future, it does not imply any changes; on the other hand, it can have a predefined logic that provides for a complete or partial change in some conditions. That is, if you want to change something in your smart contract after its publication, then you must prescribe the conditions and add replacement functionality, with which you can update an existing contract. Only this way it is possible to organize a contract update. However, any action here shall be carefully designed and tested to avoid mistakes and create corresponding vulnerabilities.

*– Can I add a transaction mediator to a smart contract? What happens if a mediator conspires with one of the participants in a smart contract?*

A mediator is not required in a smart contract, but one or more mediators of a transaction may be envisaged. To do this, you need to include the identifiers of mediators in the contract in advance and prescribe for the individual methods that they will call to influence the progress of the transaction. Mediators are chosen in a way that they are simultaneously trusted by all parties that are involved in the process. Accordingly, participants in the transaction simply will not start an interaction, transfer coins and call contract  methods if they do not trust the  mediator. All risks regarding collusion are mitigated by increasing the number of independent mediators.

*– Is it possible to transfer many different tokens from one address to different target addresses, for example, exchange addresses in order to trade these tokens in one Ethereum transaction?*

It concerns the transaction model in Ethereum and its difference from the model in Bitcoin. This difference is cardinal. In a transaction model in Ethereum, if you just transfer coins, the amount you specified is transferred only from one address to another address without delivery and without the participation of other addresses. That is, Ethereum does not work on the model of unspent outputs (UTXO) but rather on the model of accounts and balances. Therefore, it is impossible to make several payments with one transaction – hence, separate transactions are needed. If payments must be in different tokens, then each of the transactions should call the contract method that deals with an accounting of the corresponding token.

*– In “Limitations of Ethereum platform” it was stated that it is impossible to describe conditions that depend on the data of an external Internet resource. Is it possible to circumvent this limitation?*

The solution here is that the smart contract itself may include one or more so-called oracles. These are trusted parties that collect data on the state of the “outside world” and transfer it to smart contracts through special methods. The contract itself, in turn, must process all the data sets received, taking for truth those that are received from most oracles. An algorithm may be specified in the contract that does not take into account data from some oracles that contradict the majority.

*– Is it possible to create your own platform with the implementation of your own contracts? How difficult is it and what is needed for this?*

Theoretically, this is possible, of course, but for this it is necessary to solve a number of problems related to the following: development of software for network nodes, create user applications, design a motivation system for validators, developing a mechanism for achieving consensus, etc. It is also worth determining if programmable smart contracts or it will be enough to implement several templates; otherwise, each node will have to run its own virtual machine, which would have to be developed separately. One way or another, creating a decentralized smart contract platform from scratch is a very difficult task. First of all, it is worth starting from the requirements that the system shall satisfy; it may turn out to be easier to clone an existing system and configure the settings for specific validators and users.

[CONCLUSION](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-2/en/8-conclusion.md)
