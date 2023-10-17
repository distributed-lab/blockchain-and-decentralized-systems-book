# 17. Implementation of Own Blockchain

The main goal of this section is to understand the basic principles of blockchain technology. Understanding comes through
implementing  a personal minimum version of an accounting system (without decentralization and consensus
algorithm, only basic accounting mechanics).

-> In the fifth chapter of the first part of the textbook 'Blockchain and Decentralized Systems', we have thoroughly 
covered the main aspects of blockchain technology.

> Note. To successfully understand and complete the tasks of this section, it is desirable to familiarize yourself with 
additional materials
> https://bitcoin.org/bitcoin.pdf – Bitcoin White paper.

> https://andersbrownworth.com/blockchain/hash – Blockchain demo.

To implement your own accounting system based on blockchain technology, you should  determine/choose
the field in which your implementation will be used.

To do this, you need to create a brief technical specification (TS) for the selected use case.

How to choose the application field? We offer several options for you where the use of blockchain technology is appropriate.

* registries;
* social networks;
* data exchange systems;
* auctions;
* digital assets and currencies;
* voting platforms;
* certificate management systems;
* digital identification systems;
* banks;
* exchanges and asset exchange services;
* decision-making for system management (Government).


The technical task, according to the chosen field of application, should contain requirements for a
certain software product, program, or set of programs that perform specific functions in a particular
environment.

Such a document should provide answers to the following questions:
* Overview and purpose of the system/product.
* Content of the system (system boundaries).
* Interaction (potential) of the product (with other products and components).
* Functions of the product (brief description).
* Safety requirements.
* Characteristics of users (who is the end user of the system).
* Limitations.

For the practical task implementation, you can use any programming language that is convenient for you. The task involves
implementing **a minimal version of a blockchain**. You have the opportunity to decide for yourself in
what form to perform the task, but in any case, the recommended approach and tips for developing a
solution will be presented below.

## Class diagram

At the first stage, it is recommended to develop (use) a class diagram, which is presented in the figure below.
Let us provide a description of the set of classes that are recommended to be used:


`hash`: a wrapper class for using a hash function.

`Signature`: a wrapper class for using a digital signature (any algorithm).

`KeyPair`: a class designed for working with keys.

`Account`: a class designed for working with a wallet, creating operations, and signing data.

`Operation`: a class that allows creating a payment operation.

`Transaction`: a class that allows creating a transaction containing user payments.

`Block`: a class that forms a block with transactions.

`Blockchain`: a class that allows creating of a chain of blocks, a database of coins, and existing transactions.

`Node`: a class that allows maintaining a single node (even a centralized one) that processes the chain of blocks, has network
interfaces, and works with certain settings.

`UserApplication`: a class that allows users to launch a program that supports a digital wallet with certain settings
and a database.

`Wallet`: a class that allows generating and storing keys, creating new transactions, and verifying transaction statuses.


![Img](/resources/img/practical-volume/17/2-classchart.png)

You can learn more about the UML diagram by following the link:

https://miro.com/app/board/uXjVOctudsU=/?invite_link_id=391702096318

Below is a detailed description of each of the declared classes. If necessary, you can independently supplement any class
with your own methods and objects.

## Class KeyPair
Figure 2 shows the KeyPair class and its corresponding objects and methods.

![Img](/resources/img/practical-volume/17/3-keypair.png)

**Оbjects:**

`privateKey`: A large natural number required for signing operations.

`publicKey`: Point on an elliptic curve (in the case of using signatures on elliptic curves).

**Description of methods:**

`genKeyPair()`
The function that generates keys and returns an object of the KeyPair class.

**Оptional methods (can simplify the testing process):**

`printKeyPair()`
Function for displaying key-value pairs on the screen.

> Comments and suggestions:
> * To simplify the task, it is possible to avoid implementing this class. This class is necessary for signing operations
(a mechanism for verifying which allows proving ownership of the account and balance). You can skip this step if your
decision does not involve cryptographic signatures of transactions/operations.
> * The public key can be a public object and access to the private key should only be made for methods of this class
(private modifier). Any digital signature algorithm can be used.


## Class Signature

In Figure 3, the Signature class and its methods are presented.

![Img](/resources/img/practical-volume/17/4-signature.png)

**Description of methods:**

`signData()`
A function that allows for signature computation. The function takes as input a private key and a message to be signed.
It returns the digital signature data as an array of bytes. Depending on the algorithm and library used, input and
output data types may differ.

`verifySignature()`
This is a boolean function that allows signature verification. The function takes as input the signature value, the 
public key, and the message. The output is either true or false, depending on the verification result.


**Optional methods (can simplify the testing process):**

`printSignature()`
Function for displaying the value of a caption on the screen.

> Comments and suggestions:
> * The signature function is not mandatory. If your accounting system does not provide for cryptographic signatures
of transactions, this class can be skipped.
> * The generated keys (KeyPair class) should correspond to the chosen digital signature algorithm.
> * It is possible to implement your own signature algorithm, but for the sake of time efficiency, it is recommended
> * to use a pre-implemented library for signature.


## Class Account

Figure 4 shows the Account class and its methods.

![Img](/resources/img/practical-volume/17/5-account.png)

**Objects:**

`accountID`
Unique value assigned to identify an account within the system. It is often a public key or its hash value.

`publicKeys`
An array that stores objects of public keys belonging to one account. In most cases, one public key is sufficient.

`balance`
The number of coins (an unsigned integer) that belong to one account.

**Description of methods:**

`createAccount()`
A function for creating an account in the accounting system. Returns an object of the Account class. The assignment
of the public key(s) and its assignment to the account is performed.

`addKeyToWallet()`
A function that allows adding a new public key to the account and using it in the future to sign certain
operations initiated on behalf of this account. The function does not return a value.

`updateBalance()`
A function that updates the user's balance state. An integer value is passed as input to the function. The 
function does not return a value.

`createPaymentOp()`
A function that allows creating a payment operation on behalf of a given account to the recipient. The input
is an account object to which the payment will be made, the transfer amount, and the key index in the wallet.

`getBalance()`
A function that allows obtaining the current balance of the user of this account. Returns an integer value.

`printBalance()`
A function that allows displaying the user's balance state on the screen. The function does not return a value.

`signData()`
The function that allows the user to sign arbitrary data. It takes a message and the public key index in the 
array as input and returns the value of the digital signature.

**Optional methods (can simplify the testing process):**

`toString()`
A function that allows you to create a string with an account object. Returns an object of the String class.

`print()`
A function that displays all objects of the Account class on the screen. The function does not return a value.

> Comments and proposals:
> * If your implementation of the accounting system is not intended for storing financial transactions and
stores arbitrary records, the Account class may not include an object with a balance.
> * The presence of the Account class is not mandatory. Data can be added to blocks without identifying their
origin.
> * Be careful with the implementation of the updateBalance() method.


## Class Operation

Figure 5 shows the class Operation and its methods.

![Img](/resources/img/practical-volume/17/6-operation.png)

**Objects:**

`sender`
Sender's payment account.

`receiver`
Payment recipient account.

`amount`
transfer amount.

`signature`
Signature data is generated by the payment sender.


**Description of methods:**

`createOperation()`
A function that allows creating an operation with all necessary details and signature. It takes the sender and
receiver's account, transfer amount, and signature of the described data as input. Returns an Operation object.

`verifyOperation()`
A function that performs an operation check. The main checks that can be attributed to the proposed implementation
include: verification of the transfer amount (the amount should not exceed the sender's balance), and signature
verification (using the sender's payment public key). The function returns true/false depending on the results
of the operation check.

**Optional methods (can simplify the testing process):**

`toString()`
A function that allows you to create a string with an operation object. Returns an object of the String class.

`printKeyPair()`
Function for displaying operation objects. The function does not return a value.

> Comments and suggestions: 
> * An operation may contain additional fields, such as comments, etc. The logic for verifying additional fields
is determined solely by the implementation author.
> * The described implementation only includes checking the uniqueness of the transaction, and checking the
uniqueness of the operation is absent. It should be noted that in this case, an attacker can create a new 
transaction, add this operation to it, and spend coins from someone else's account. To avoid forgetting such
vulnerability during implementation, imagine it could be your own account.
> * If a wallet with many  keys belongs to a specific account, do not forget to verify everything for
compliance with the signature.


## Class Transaction
The class Transaction and its methods are presented in Figure 6.

![Img](/resources/img/practical-volume/17/7-transaction.png)

**Objects:**

`transactionID`
Unique transaction identifier (hash value of all other fields of the transaction).

`setOfOperations`
A set of payment operations that are confirmed in the given transaction.

`nonce`
meaning for protection against duplication of transactions with identical operations.

**Description of methods:**

`createOperation()`
A function that allows creating a transaction with all the necessary elements. It takes a list of operations and _nonce_ 
as input and returns an object of type Transaction.

**Optional methods (can simplify the testing process):**

`toString()`
A function that allows to formation of a string with a transaction object. Returns an object of the String class.

`printKeyPair()`
A function for displaying transaction objects. The function does not return anything.

> Comments and suggestions:
> * If duplication control of the transaction is not ensured, validators may not recognize that the necessary transaction
has already been added to the history. To address this, nonce values can be used, which affect the formation of the
transaction identifier (hash value of all other transaction fields).

## Class Hash

In Figure 7, the Hash class is presented, which contains only one method - **SHA256**.

![Img](/resources/img/practical-volume/17/8-hash.png)

**Description of methods:**

`SHA256()`
A function that takes an input array of data and returns the hash value of this data as an array of bytes (in this case,
the SHA2 function with a length of 256 bits is used as the hashing algorithm).

>Comments and suggestions:
> * You can use any hashing algorithm that you deem necessary. It is recommended to use existing libraries. Developing 
your own version of a hashing algorithm is welcomed, but requires a considerable amount of time.

## Class Block
Figure 8 shows the Block class and its methods.

![Img](/resources/img/practical-volume/17/9-block.png)

**Objects:**

`blockID`
Unique identifier of the block (hash value of all other data).

`prevHash`
Previous block identifier (needed to ensure the integrity of the history).

`setOfTransactions`
List of transactions confirmed in this block.

**Description of methods:**

`createBlock()`
A function that allows creating a block with all necessary elements. It takes a list of transactions and the identifier
of the previous block as input. It returns a Block object.

**Optional methods (can simplify the testing process):**

`toString()`
A function that allows creating a string from block objects. Returns an object of the Block class.

`printKeyPair()`
Function for displaying objects of the block. The function does not return anything.

## Class Blockchain

The class Blockchain and its methods are presented in Figure 9.

![Img](/resources/img/practical-volume/17/10-blockchain.png)

**Objects:**

`coinDatabase`
A table that displays the current balance status in the system. The account identifier is used as the key, and the user's
balance is used as the value

`blockHistory`
An array that stores all the blocks that have been added to the history.

`txDatabase`
An array that stores all transactions in history. It will be used for faster access when verifying the existence of a 
transaction in history (protection against duplication).

`faucetCoins`
An integer value that determines the number of coins for testing.

**Description of methods:**

`initBlockchain()`
The function that initializes the blockchain. In fact, the genesis block is created and added to the history.

`getTokenFromFaucet()`
A function that allows obtaining a small number  of coins for testing purposes. It updates the state of the coins database
and the balance of the account that called this method.

`checkBlock()`
A function that performs a check of a block with all the transactions that are included in it.

`connectBlock()`
A function that allows to attach a block to the local chain of blocks and update the current state of the accounting 
system on this node.

`getTokenFromFaucet()`
A function that allows obtaining the current state of accounts and balances.

**Optional methods (can simplify the testing process):**

`toString()`
A function that creates a string from blockchain objects. It returns an object of the String class, which will be useful for debugging the program.

`print()`
A function for displaying the main elements of a class on the screen, which will be needed for logging the node's work.Comments and suggestions:

> Comments and suggestions:
> * The `checkBlock` function should contain a set of the following checks:
> 1. Verification that the block contains a reference to the latest valid block in the history;
> 2. Verification that the transactions in the block have not yet been added to the history;
> 3. Verification that the block does not contain conflicting transactions.
> 4. Verification of each operation in the transaction:
>    1. Signature verification;
>    2. Verification that the operation does not use more coins than are stored in the sender's account balance.
  

## Class Node

The class Node and its methods are presented in the figure 10.

![Img](/resources/img/practical-volume/17/11-node.png)

**Objects:**

`blockchain`
The object of the Blockchain class indicates that the node will process a certain chain of blocks.

`connections`
This object is responsible for maintaining a list of all other nodes in the network. It tracks their statuses and establishes
direct network connections with a certain number of nodes from this list. It receives and transmits data about existing
nodes through the network for the purpose of network reconnaissance.

`settings`
An array or a set that is necessary for storing the settings with which the current node operates. For example, the desired
number of network connections, the maximum allowed load on the central processor and RAM, etc.

**Description of methods:**

`startNode()`
The function launches the processes of all the necessary modules of the full network node. For example, the network module,
the blockchain processing module, and the user/admin interaction module of the node.

`startNetworking()`
The function launches the network module and interacts with other nodes in the network.

`updateConfiguration()`
The function that allows updating the settings of the current node.

`stopNode()`
A function that terminates the execution of processes and modules of the current node, and saves all necessary data to 
disk for the possibility of correct launch in the future.

**Optional methods (can simplify the testing process):**

`getStatus()`
A function that returns data about the current state of a node. For example, the height of the latest known block,
its hash value, the current number of network connections, the amount of used RAM, the amount of used disk space, and
so on.

`rescanBlockchain()`
A function to initiate a check of the entire blockchain from the beginning to the last known block, in order to correct
any possible accumulated errors.

## Class UserApplication

The figure 11 shows the UserApplication class and its methods.

![Img](/resources/img/practical-volume/17/12-app.png)

**Objects:**

`wallet`
The Wallet class object indicates that the application will handle a certain digital wallet.

`connection`
The object responsible for supporting the connection of the wallet to the full node of the network for synchronizing its
state and propagating created transactions.

`txDatabase`
The database is used to store transactions that are related specifically to the current wallet. Storage of other wallet
data (keys, addresses, settings) can be performed in this or a separate database.

`settings`
The object in the operational memory that stores, updates, and applies the current settings of the user's application.

**Description of methods:**

`startApp()`
The function launches processes of all necessary modules of the user application. For example, the network module, 
transaction synchronization module, and module for interacting with the user's digital wallet.

`startNetworking()`
A function that launches the module for interacting with a trusted node of the network.

`updateConfiguration()`
A function that allows the user to initiate updates to the application settings.

`stopApp()`
The function that terminates the execution of processes and modules of the user's application, saves all necessary data 
to disk for the possibility of correct future launch.

**Optional methods (can simplify the testing process):**

`getStatus()`
A function that returns data about the current state of the application. For example, the height of the successfully 
synchronized block, the number of user transactions that received full confirmation, the status of the connection to
the full network node, the amount of used RAM, the amount of used disk space, and so on.

`rescanWallet()`
The function that deletes all known application transactions and initiates a scan of the entire blockchain from the beginning
to the last known full node of the network in order to recover all transactions related to the current wallet.

## Class Wallet
The class Wallet and its methods are presented in Figure 12.

![Img](/resources/img/practical-volume/17/13-wallet.png)

**Objects:**

`account`
The object of the Account class is needed to specify that the wallet will work with a certain identifier and certain 
public keys.

`userTxes`
An array of transactions or a mapping of hash values to transactions that stores all transactions related to the current
account.

**Description of methods:**

`generateNewKeyPair()`
The function generates a new key pair, sends the public key to be added to the user's account through a transaction,
and stores the private key in a separate secure repository. Additionally, it can initiate a search for transactions
related to the new public key.

`createTransaction()`
A function that creates, fills, signs, and initiates the propagation of a transaction through a full node of the network.

`getBalance()`
The function that accesses the transaction history of the current wallet calculates the current balance and returns it 
as a number. Alternatively, it makes a corresponding request to the full network node to obtain the current balance
for the account identifier.

`rescanTransactions()`
A function that updates data on the confirmation status of known transaction wallets by receiving data from a full node
of the network.

**Optional methods (can simplify the testing process):**

`getStatus()`
A function that returns important statistics and the current state of the wallet, such as the number of transactions in 
the user's history, the number of key pairs in the wallet, and so on.

`makeWalletBackup()`
Function that returns (or writes to a file) personal wallet keys data in an open or secure format.

`restoreFromBackup()`
A function that receives or reads personal key data from a file and stores it in a corresponding secure repository. 
Additionally, it can initiate a wallet scan to find transaction-related new keys.

> Comments and suggestions: 
> * The signature function is not mandatory. If your accounting system does not provide for the cryptographic signature of
transactions, the `signData()` method can be left unimplemented.

To understand how this can be implemented, we provide an example of a simple implementation of a custom blockchain for 
the **"e-voting"** use case in the Go programming language, created with the help of artificial intelligence.

🤖

```go
package main
import (
  "crypto/sha256"
  "encoding/hex"
  "fmt"
  "strconv"
  "strings"
  "time"
)
const difficulty = 3
// Block represents each 'item' in the blockchain
type Block struct {
  Index      int
  Timestamp  string
  Vote       string
  Hash       string
  PrevHash   string
  Difficulty int
  Nonce      int
}
// Blockchain is a series of validated Blocks
var Blockchain []Block

// HashBlock generates a hash of all block information
func HashBlock(block Block) string {
  record := strconv.Itoa(block.Index) + block.Timestamp + block.Vote + block.PrevHash + strconv.Itoa(block.Nonce)
  hash := sha256.Sum256([]byte(record))
  return hex.EncodeToString(hash[:])
}
// CreateNewBlock creates a new block in the blockchain
func CreateNewBlock(vote string) Block {
  var newBlock Block
  t := time.Now()
  newBlock.Index = len(Blockchain)
  newBlock.Timestamp = t.String()
  newBlock.Vote = vote
  newBlock.PrevHash = Blockchain[len(Blockchain)-1].Hash
  newBlock.Difficulty = difficulty
  newBlock.Nonce = 0
  newBlock.Hash = calculateBlockHash(newBlock)
  return newBlock
}
// IsBlockValid returns if the block is valid by checking index, and comparing the hash of the previous block
func IsBlockValid(newBlock, oldBlock Block) bool {
  if oldBlock.Index+1 != newBlock.Index {
    return false
  }
  if oldBlock.Hash != newBlock.PrevHash {
    return false
  }
  if calculateBlockHash(newBlock) != newBlock.Hash {
    return false
  }
  return true
}
// ReplaceChain replaces the blockchain with a new chain
func ReplaceChain(newBlocks []Block) {
  if len(newBlocks) > len(Blockchain) {
    Blockchain = newBlocks
  }
}
// make a vote
func makeVote(vote string) {
  newBlock := CreateNewBlock(vote)
  if IsBlockValid(newBlock, Blockchain[len(Blockchain)-1]) {
    Blockchain = append(Blockchain, newBlock)
  }
}
func calculateBlockHash(block Block) string {
  for block.Nonce < 1000000000 {
    hash := HashBlock(block)
    if strings.HasPrefix(hash, strings.Repeat("0", block.Difficulty)) {
      return hash
    }
    block.Nonce++
  }
  return ""
}
func main() {
  // create the genesis block
  t := time.Now()
  difficulty := 3
  genesisBlock := Block{0, t.String(), "", "", "", difficulty, 0}
  genesisBlock.Hash = HashBlock(genesisBlock)
  Blockchain = append(Blockchain, genesisBlock)

  // create 10 dummy votes
  for i := 0; i < 10; i++ {
    vote := "vote" + strconv.Itoa(i)
    makeVote(vote)
    fmt.Printf("New block added to the blockchain with vote: %s\n", vote)
    fmt.Printf("Hash: %s\n", Blockchain[len(Blockchain)-1].Hash)
    fmt.Println("\n-------------------\n")
  }
}
```

> In this code, the difficulty parameter sets the number of zeros that must be at the beginning of the hash for it to
be considered valid. The CalculateBlockHash function performs PoW by incrementing the nonce until a hash is found that
meets the difficulty criteria. Each time a vote is cast, the makeVote function creates a new block and performs PoW
before adding it to the blockchain if it is valid.

> Analysis of typical shortcomings present in this implementation should help you avoid repeating them. So, what's wrong here?
> 1. The HashBlock function does not use the difficulty parameter to form the summary hash. This is not critical from
a security standpoint, but it is definitely not a good solution.
> 2. The difficulty parameter does not change depending on the frequency of block discovery. This means that there
will be a huge number of orphan blocks in case of a large hashpower value. This can indeed become a problem, which 
consists in the fact that many  "forks" can appear, and as a result - the difficulty of resolving them.
> 3. The fundamentally incorrect use of difficulty, which is hard-coded in the code. Consensus should rely on them, and
this implementation does not support it.
> 4. IsBlockValid is a very interesting function in terms of its implementation by artificial intelligence. In general,
it is good that it exists, but there should be significantly more checks for real use. The same goes for the timestamp,
which is in a certain range. Difficulty and Nonce parameters should be checked here again, but since they are fictitious,
they are not checked, of course.
> 5. Replace chain is implemented incorrectly. It should be oriented to the complexity of the chain, not its length.
> 6. General comment. The implementation in this form describes the model 1 voice = 1 block. With this approach, only
the block creator can vote. However, it should be noted that there is no vote counting function. Thus, it can be said
that this is more like a registry than a voting system.

For comparison, to illustrate which aspects can be taken into account by a person during the implementation of an educational
project, we provide a reference to a repository with the implementation of a custom blockchain for a voting platform:
https://github.com/yehor-podporinov/baby_blockchain

✯ Try to independently perform all stages of implementation, referring to the recommendations outlined in this section.
It should be noted that these recommendations are only an example of architecture and cover only 20% of all classes
and methods necessary for the functioning of the accounting system and applications for its use.

We also recommend the following cases for implementation:
* Management and tracking of supply chains.
* Cross-border payments and money transfers.
* Secure storage and exchange of medical records.
* Decentralized file storage.
* Decentralized identity management.
* Solutions for combating counterfeiting of luxury goods.
* Property ownership and transaction management.
* Decentralized prediction markets.
* Decentralized autonomous organizations (DAOs).
