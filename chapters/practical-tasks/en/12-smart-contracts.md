# 12. Development of smart contracts in the Ethereum environment

Ethereum is not only an accounting system, but also a platform for executing decentralized computations. The specialized
high-level object-oriented programming language Solidity allows for the creation of smart contracts that are executed
in the decentralized environment of Ethereum. A smart contract is a digital agreement for the redistribution of values
between parties, which involves strict and unambiguous conditions, process automation, and minimization of the involvement
of trusted parties.

What do you need to know about smart contracts?

* They allow parties to interact without the involvement of third parties.

* Auditing smart contracts is easier than with traditional contract systems.

* There are different ways and protocols for implementing smart contracts.

* They allow for the management of digital assets that are accounted for on the platform.

* The initiation of the created contract is performed either manually or automatically.


-> In the _____ section of the third part of the textbook 'Blockchain and Decentralized Systems', you can learn more about
the technology of smart contracts.

> Before developing  smart contracts, we recommend familiarizing yourself with package managers such as _npm_ and
_yarn_. They significantly increase work efficiency and allow for the reproduction of the development environment
independently of the machine you work on.
> 
> In addition, many  libraries for smart contract development are published in the npm repository.
Although they are not mandatory for development, using libraries that the developer community has verified
can help avoid potential errors and significantly reduce development time. The ability to work with tools such
as Truffle, Ganache, and Hardhat can also greatly simplify your life as a smart contract developer.


## Hello, Solidity!

✯ To better understand the environment and the process of developing smart contracts, we suggest starting with the 
traditional "Hello, world!" program.


```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract HelloWorld {
    function hello() public pure returns (string memory) {
        return "Hello, world!";
    }
}
```

This contract can be compiled and deployed on the Ethereum blockchain and will be available for calling through
web interfaces such as Remix or Truffle. Users can call the hello() function for free and receive the response
'Hello, world!'.

**Description of functions:**

`hello()`
A function that returns any text. Ensure that the function does not change the state of the accounting system, and its 
call is free for the user.

> More information about modifiers and the impact of contracts on the state of the accounting system can be found in the
relevant section of the documentation: https://docs.soliditylang.org/en/latest/contracts.html#state-mutability

✯ Compile the contract and deploy it in the Remix VM environment (a virtual machine that runs in the browser and
does not require a wallet). After that, call the hello() function and make sure that the contract works as intended.

> More information about the features and differences between environments can be found at the following link: 
https://remix-ide.readthedocs.io/en/latest/run.html#environment

✯ Make the function accept a parameter named "name" and greet the person whose name is specified in it.

## Decimal division
Often, in the development of smart contracts, you may need to deal with the need to perform certain arithmetic operations,
especially if your contract involves a financial component. Solidity has certain features related to the environment
where these operations are performed. To become more familiar with these features, we propose to perform the following 
task.

✯ Set two numerical variables: **a = 3** and **b = 2**. Divide **a** by **b**. The result should be **a** decimal.

> Some important remarks on this topic:
https://docs.soliditylang.org/en/latest/types.html#fixed-point-numbers
https://docs.soliditylang.org/en/develop/types.html#rational-and-integer-literals

✯ Allow the user to independently set the dividend, divisor, and number of decimal places.

## Contract registry
✯ As part of this task, you need to develop a simple smart contract for tracking bank transactions.

**Objects:**

`TransactionRecord`
The structure has the following fields: payment identifier, sender identifier, receiver identifier, payment amount, 
timestamp, and payment hash value. All identifiers consist of a combination of digits and letters. The payment
hash value is obtained by concatenating the identifiers, amount, and timestamp, and hashing the resulting string
using any available algorithm in Solidity.

`transactions`
Gesh-map (mapping) that allows obtaining fields of the _TransactionRecord_ structure by payment identifier.

**Description of functions:**

`storeTransactionRecord()`
A function that saves payment details in a smart contract.

`getTransactionRecordById()`
A function that allows retrieving payment fields by their identifier.

`getAllTransactionsBySender()`
A function that allows retrieving all payments by the specified sender identifier.

`getAllTransactionsByRecipient()`
A function that allows retrieving all payments by the specified recipient identifier.

> You can find additional information that may be useful at the following links:
https://docs.soliditylang.org/en/v0.8.18/structure-of-a-contract.html#struct-types
https://docs.soliditylang.org/en/v0.8.18/structure-of-a-contract.html#functions

To understand where it is best to start, we suggest implementing our work:

🤖

```solidity
pragma solidity ^0.8.0;

contract BankTransactions {
struct TransactionRecord {
string paymentId;
string senderId;
string recipientId;
uint256 amount;
uint256 timestamp;
bytes32 paymentHash;
}

    mapping(string => TransactionRecord) public transactions;
    mapping(string => string[]) public transactionsBySender;
    mapping(string => string[]) public transactionsByRecipient;

    function storeTransactionRecord(
        string memory _paymentId,
        string memory _senderId,
        string memory _recipientId,
        uint256 _amount
    ) public {
        uint256 _timestamp = block.timestamp;
        bytes32 _paymentHash = keccak256(abi.encodePacked(_paymentId, _senderId, _recipientId, _amount, _timestamp));

        TransactionRecord memory newTransaction = TransactionRecord({
            paymentId: _paymentId,
            senderId: _senderId,
            recipientId: _recipientId,
            amount: _amount,
            timestamp: _timestamp,
            paymentHash: _paymentHash
        });

        transactions[_paymentId] = newTransaction;
        transactionsBySender[_senderId].push(_paymentId);
        transactionsByRecipient[_recipientId].push(_paymentId);
    }

    function getTransactionRecordById(string memory _paymentId) public view returns (TransactionRecord memory) {
return transactions[_paymentId];
}
```

This smart contract implements the `TransactionRecord` structure, as well as corresponding functions for storing
and retrieving transaction information. In the `storeTransactionRecord()` function, a new transaction record is
created and stored in the `transactions` map and added to the `transactionsBySender` and `transactionsByRecipient`
lists.

✯ Using the Remix IDE, deploy the contract on any public test network (such as Goerli).

>To deploy contracts in public accounting systems, both primary and test ones, you need to have a wallet with
a certain amount of coins to cover commission costs. 
> For the easiest integration with Remix, you can install Metamask (https://metamask.io/), although contract 
deployment can be done in various ways. Coins in test accounting systems have no real value, so they can be
obtained for free. 
> The current list of places where you can get test coins for Goerli is available at https://faucetlink.to/goerli.

## Updating smart contracts

After the compiled bytecode of the contract has been distributed and added to the blockchain, it is impossible
to remove it from there. But what if you suddenly notice a critical error in the code that needs to be urgently
fixed? In traditional applications that a user installs on their device or views in a web browser, it is enough 
to update the software version on the server or ask users to download an update. Deploying a new version of 
your smart contract will give you a new copy with a new address and an empty state. However, there are several
approaches to solving this problem, which we propose you familiarize yourself with.

✯ Using the OpenZeppelin library, create a contract (or modify an existing one) that complies with the Universal 
Upgradeable Proxy Standard (UUPS) and perform the upgrade process. Use the OpenZeppelin Upgrades Plugins for
one of the development environments you will be using: Truffle (https://trufflesuite.com/) or Hardhat
(https://hardhat.org/).

> More about OpenZeppelin Upgrades Plugins can be found at:  
https://docs.openzeppelin.com/upgrades-plugins/1.x/

> To work with OpenZeppelin libraries and development environments such as Truffle and Hardhat, it is useful to have 
skills in working with the npm package manager (https://www.npmjs.com/).

✯ Deploy the contract in a public test account system using migration.

> More information about migrations in relevant development environments can be found at the following links:
https://trufflesuite.com/docs/truffle/
https://hardhat.org/hardhat-runner/docs/guides/deploying

✯ Create a new version of the implementation contract, for example, by adding a new or changing an existing function so 
that the change is visible when the contract is executed. Create a migration to update the contract. The address
of the proxy contract should remain unchanged.

✯ Run the accounting system locally in the Ganache simulator (https://trufflesuite.com/docs/ganache/) and create a separate
migration to deploy the contract in it. Test the contract update procedure in the local network.

## Oracles
Smart contracts deployed on the Ethereum accounting system do not have the ability to directly make requests "outside" 
and receive information through APIs. Oracles act as a bridge between the external world and the accounting
system, allowing contracts to obtain information such as current currency exchange rates and make requests to
third-party APIs.

✯ Using the Chainlink Data Feeds library, create a smart contract that will provide the current exchange rate of ETH in
relation to the US dollar upon request from the user. Use the corresponding contract in the accounting system
as the data source for the contract you deploy.

**Description of functions:**

`getLatestRate()`
A function that returns the latest current value of the ETH/USD exchange rate.

> More information about the use of ChainLink data sources can be found at the following link:
https://docs.chain.link/data-feeds/price-feeds

> The list of all data source contracts can be found at the link:
https://docs.chain.link/data-feeds/price-feeds/addresses/

> Where do sources get data from? The answer is here:
https://docs.chain.link/architecture-overview/architecture-decentralized-model/

✯ Add the ability to obtain not only the latest current exchange rate, but also the exchange rate as of a certain round in the past.

✯ Using the ChainLink Any API library, create a contract that receives and stores data from a public API.

**Description of functions:**

`sendRequestAndStoreResponse()`
A function that makes a request to an API through an oracle, saves one of the values from the JSON body of the response (for example, the air temperature in degrees Celsius), and returns it to the caller of the function.

`retrieveData()`
A function that allows obtaining any of the previously saved values by its ordinal number.

> To store a variable, you can use an array of the corresponding type or a hash map. Before proceeding with this task, 
we recommend familiarizing yourself with how the ChainLink basic query model works: https://docs.chain.link/architecture-overview/architecture-request-model/

> You can learn how to make simple API calls through ChainLink oracles here:
https://docs.chain.link/any-api/get-request/examples/single-word-response/

✯ Expand the contract in such a way that it could receive, store, and issue not just one, but several values.

## Metatransactions

Usually, the person who calls the contract function pays the commission themselves (provided that this function changes
the state of the accounting system, not just reads data from it). In some cases, there may be a need to facilitate
the work of users with an application that uses a smart contract by paying the commission for them, and then,
for example, including its cost in the total amount of a bank payment.

✯ Create a contract that changes the state of the system (or use an existing one). Adapt the contract to work with the 
Gas Station Network so that the caller of the function does not have to pay a fee.

> More information about how to do it can be found at the following link:  https://docs.opengsn.org/contracts/

✯  Deploy your contract and **Paymaster** contract in a public test account system. Test the functionality of the contract 
by calling a function from a wallet with no coins.

> The full process of adapting the contract, as well as creating and deploying the Paymaster contract, is described at the following link:  
https://docs.opengsn.org/javascript-client/tutorial.html

✯ Deploy your own Relay Server (https://docs.opengsn.org/relay-server/tutorial.html) and familiarize yourself with the 
process of its operation.
