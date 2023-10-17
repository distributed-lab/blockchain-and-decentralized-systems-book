# 11. Interacting with the Bitcoin node structure

To complete this practical task, your PC or laptop must have any distribution of the Linux operating system installed
(either virtually or physically) and the corresponding software (automatic startup configuration script).

The automatic startup configuration script refers to the [dlab](https://github.com/OlKurbatov/dlab_tool)utility, which 
was specially written for this practical task and is available for download at the link (`git pull`). The script above 
performs the following functions:
* Automatic configuration of private networks for various protocols that use blockchain technology (with several
deployment options);
* Automatic launch and connection of additional nodes to the private network as desired by the student;
* Execution of load tests for maximum approximation to the functioning conditions  of a real decentralized accounting
system.

https://github.com/OlKurbatov/dlab_tool

This script allows skipping the manual network setup stage and immediately starting to familiarize oneself with the
protocol.

After you have a working Linux distribution with installed software ([dlab](https://github.com/OlKurbatov/dlab_tool)), you should perform the following steps 
to configure a private Bitcoin network:
1. Open the terminal 
2. Call the configuration script with the command `dlab` from https://github.com/OlKurbatov/dlab_tool 
>>NOTE: TO CORRECTLY
STOP NODES IN THE CONSOLE WINDOW OF THE CONFIGURATION SCRIPT, USE THE KEY COMBINATION `ctrl+C`.
3. Choose the type of network (in this case, Bitcoin) and press "Enter".

![Network Type](/resources/img/practical-volume/11/4-network_type.png)

4. Choose the type of configuration (in this case, Multiple nodes on the local machine).

![Config Type](/resources/img/practical-volume/11/5-configtype.png)

5. The script configures two nodes and opens their consoles (each terminal window represents the interface of one 
node, and in each terminal window you can find two tabs: node console - `node_console` and client console - `cli-console`).
Now you have a fully functional "Bitcoin private test network" Bitcoin network consisting of two nodes (full node).
Later, to add nodes, you need to return to the configuration script and select the `1. Add new node` option. All 
the commands listed below should be executed in the client console tab (the node console is purely informative).

> Attention! The network is operating in regtest (Regression test mode), which means that all operations (transactions, etc.)
are applied to a test version of the blockchain. And this means:
> 1. All the coins you mine cannot be spent in the mainnet (the main blockchain of the bitcoin cryptocurrency).
> 2. When you send test coins to another address (another node/wallet), you do not suffer any material loss from this 
operation.
> 3. The difficulty of mining coins is so low that you have the right to create 1000 blocks (this process will 
take about 15-20 seconds) and dispose of the mined funds. But unfortunately, only in your regtest network.


6. After setting up the network, go to the client console of any node and check the status of the local blockchain
using the command `getblockchaininfo`. 

In response to this command, you will receive a conclusion of this type:

![ВSummary](/resources/img/practical-volume/11/6-summary.png)

The full content may vary slightly for you:

```json
{
  "chain": "regtest",
  "blocks": 0,
  "headers": 0,
  "bestblockhash": "0f9188f13cb7b2c71f2a335e3a4fc328bf5beb436012afca590b1a11466e2206",
  "difficulty": 4.656542373906925e-10,
  "mediantime": 1296688602,
  "verificationprogress": 1,
  "initialblockdownload": true,
  "chainwork": "0000000000000000000000000000000000000000000000000000000000000002",
  "size_on_disk": 293,
  "pruned": false,
  "softforks": [
	{
  	"id": "bip34",
  	"version": 2,
  	"reject": {
    	"status": false
  	}
	},
	{
  	"id": "bip66",
  	"version": 3,
  	"reject": {
    	"status": false
  	}
	},
	{
  	"id": "bip65",
  	"version": 4,
  	"reject": {
    	"status": false
  	}
	}
  ],
  "bip9_softforks": {
	"csv": {
  	"status": "defined",
  	"startTime": 0,
  	"timeout": 9223372036854775807,
  	"since": 0
	},
	"segwit": {
  	"status": "active",
  	"startTime": -1,
  	"timeout": 9223372036854775807,
  	"since": 0
	}
  },
  "warnings": ""
}
```
Main elements:

`chain` - network operation mode;

`blocks` - the number of blocks in your local chain;

`headers` - the number of confirmed block headers in the local chain;

`bestblockhash` - the hash value of the header of the last confirmed block;

`difficulty` - the current difficulty of block formation;

`mediantime` - the average time it takes to confirm 11 blocks before the last block in the blockchain. Used to 
verify transaction confirmation time according to BIP113;

`Verificationprogress` - an estimate of the percentage of transaction blocks that have been verified so far, starting
from 0.0 and increasing to 1.0 for full verification. It may slightly exceed 1.0 when fully synchronized to account
for transactions in the mempool that were verified before being included in a block;

`chainwork` - the predicted number of block header hashes verified from the genesis block to this block, encoded
as big-endian hex;

`initialblockdownload` - a network synchronization method used by a node;

`pruned` - indicates that blocks are subject to pruning.


7. In order to check the status of the connections to other nodes (nodes that are connected to you), use the `getaddednodeinfo` command.

![Connection State](/resources/img/practical-volume/11/7-connection-state.png)

Main elements:

`addednode` - IP address of the added node;

`connected` - connection status (true if the node is currently connected, and false if it is not);

`address` - one of the wallet addresses on this node;

`connected` - possible values: false (no connection), inbound (if some node has an inbound connection to this node),
outbound (if this node has an outbound connection).


8. Obtaining information about a website. Using the command `-getinfo`, you will receive information about the basic 
parameters of the node.

![Get info](/resources/img/practical-volume/11/8-getinfo.png)

Main recurring elements:

`version` - the version of Bitcoin software (Bitcoin Core);

`protocolversion` - the version of the protocol used by this node;

`walletversion` - the version of the wallet for this node. Only displayed if the node software supports this
function;

`blocks` - the number of formed blocks in the local blockchain;

`timeoffset` - the difference with the system clock in seconds;

`connections` - the number of open connections (incoming and outgoing);

`proxy` - address:port of the proxy server;

`difficulty` - the difficulty of creating a block;

`testnet` - returns true if the node is in testnet and false if in mainnet or regtest;

`keypoololdest` - the date when the oldest key in the wallet key pool was created; useful only for scanning blocks
created from this date for transactions;

`keypoolsize` - the number of keys in the wallet key pool;

`paytxfee` - the minimum fee per kilobyte of transaction;

`relayfee` - the minimum fee that a low-priority transaction must pay for this node to accept it into its mempool.
 
10. The formation of a new address occurs in the following way.

Using the `getnewaddress` command, we create an address.
The result will be a payment-receiving address. Payments sent to this address will be credited to this node's
wallet .


![Generate new address](/resources/img/practical-volume/11/9-getnewaddress.png)

10. Creating 1 block.

To create a block, use the command `bitcoin-cli generate [n]`, where n is the number of blocks to be created. 
In response, the command will output an array of hash values of the headers of the created blocks (using hash,
you can associate corresponding transactions with a certain block).

![Block generate](/resources/img/practical-volume/11/10-blockgen.png)

Switch to the console of another node (another console window, the first tab `node_consoleX`) and make sure that it has
received the block created on the first node.

![Block confirmation](/resources/img/practical-volume/11/11-block-receiving.png)

11. Перевірка інформації про щойно створений блок.

Checking information about a newly created block.
Using the command `getblock <block header hash>` we obtain a representation of the block with the specified hash,
which is outputted when using `generate <n>`.

![Receiving a block](/resources/img/practical-volume/11/12-getblock.png)

Main elements:

`hash` - the hash value of the block header;

`confirmations` - the number of blocks confirmed since the transaction was published;

`size` - the size of the block in bytes;

`strippedsize` - the size of the block without witness data;

`weight` - the weight of the block, which is a measure of the block's size, including segwit data 
(see Block_weight for more details);

`height` - the height of the block in the block chain;

`version` - the version of the block being used;

`versionHex` - the version of this block encoded in hexadecimal format;

`merkleroot` - the Merkle root for this block, encoded in hexadecimal format;

`tx:[]` - an array of transactions contained in the block;

`time` - the time when the block was formed;

`nonce` - the solution to the PoW problem specifically for this block;

`bits` - indicating the target threshold that the header of this block had to pass;

`difficulty` - the difficulty for creating this block;

`chainwork` - the predicted number of hash values of block headers that miners had to verify from the genesis block
to this block, encoded in hexadecimal format;
`Previousblouckhash` - the hash of the header of the previous block. Not returned for the genesis block;

`nextblockhash` - the hash of the next block in the best block chain, if known, encoded in hexadecimal format.


12. To receive a reward for creating the first block of the Bitcoin protocol, 100 confirmations are required. To do this,
it is necessary to form 100 blocks. Do this (see point 5). After that, check:
* Has the second node accepted your created blocks?

![Block check](/resources/img/practical-volume/11/13-accept-100-blocks.png)

* Using the `getbalance` command, check if you have received a reward for creating a block (the command must be executed
* on the node from which the block was created).

![Block check](/resources/img/practical-volume/11/14-getbalance.png)

13. To send a transaction, a Bitcoin address is required from the receiving side, and some balance on the sending 
side. To satisfy these conditions, follow these steps:
* Select a node that will receive the coins and execute the command from point 4 on it (remember the received address);
* Choose a node that will send the coins (in fact, the coins are only on the address of one node, but you can always
repeat points 5, 7 and receive the coins at the address of another node);
* On the sending node, execute the command `sendtoaddress <bitcoin address>`, where <bitcoin address> is the address
of the receiving party.
* The received hash (TXID of the transaction) is remembered, we will need it later.

![Block check](/resources/img/practical-volume/11/15-tx.png)

14. Creating a single block (on any node) to confirm a transaction (see point 5) and checking the balance on the receiving
side using the `getbalance` command.

![Block check](/resources/img/practical-volume/11/16-getbalance-after-tx.png)

15. ПView information about a transaction using the command `gettransaction <TXID>` (on any node) using the TXID obtained
during the transaction. (TXID from step 8)

![Block check](/resources/img/practical-volume/11/17-gettx.png)

`amount` - the number of coins spent;

`confirmations` - the number of transaction confirmations;

`blockhash` - the hash of the block in the local chain to which the transaction belongs;

`blockindex` - the index of the transaction in the block;

`blocktime` - the time in the block header (only for confirmed transactions);

`txid` - the unique identifier of the transaction;

`address` - the address to which the funds were sent;

`category` - the transaction category with respect to the node (in case of "send").


16. To check the synchronization process, go to the console window where the configuration script is running, select
the `1. Add new node` option, and press `Enter`. A console window for the new node will appear (with the same tabs
`node_console` and `cli-console`).

Go to the newly created console window:

* Go to the `node_console` tab and track the synchronization process;
* Go to the `cli-console` tab and execute the command `getblockchaininfo | grep blocks`. The height should be the
same as on other nodes.

17. In order to view the _mempool_ function, it is necessary to conduct several transactions between any nodes 
(you can create new nodes in any quantity) without confirming these transactions, that is, without creating blocks.
And execute the command `getmempoolinfo`.

![Block check](/resources/img/practical-volume/11/18-getmempool.png)

`size` - the number of transactions in the mempool;

`bytes` - the total number of bytes of transactions;

`usage` - the amount of memory used to store the mempool data (including all transaction data and service data);

`maxmempool` - the maximum memory usage for the mempool in bytes;

`mempoolminfee` - the lowest fee per kilobyte paid by any transaction to enter the mempool.


18. Using the command `generate 1`, we create one block for transaction confirmation and again check the state of the mempool.

![Block check](/resources/img/practical-volume/11/19-getmempool2.png)

As we can see, transactions from the mempool have disappeared. You can execute the `getbalance` command to make sure that
the transactions have been included in a block and confirmed.

>Additionally, we recommend familiarizing yourself with:
>https://bitcoin.org/bitcoin.pdf – White paper

>https://bitcoin.org/en/developer-guide#block-chain-overview – Bitcoin Developer Guide

>https://en.bitcoin.it/wiki/Main_Page – Bitcoin Wiki

>https://bitcoin.org/en/developer-reference#version – Bitcoin Core APIs