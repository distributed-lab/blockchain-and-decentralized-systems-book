# TEST QUESTIONS

Which principles constitute the basis of BitTorrent protocol?
* A native distributed peer-to-peer network
* The fee size is proportional to the message size
* Messages are private due to asymmetric encryption
* Outdated messages are not transmitted by the network nodes
* Storing short messages in the blockchain with timestamps
* The PoW difficulty depends on the amount of data transmitted

Which data is stored in torrent files?
* Location of the content’s source nodes 
* Order of split file fragments
* Hash values of file fragments
* Network addresses of the content’s source nodes

Which problem does a distributed hash table (DHT) solve?
* Propagation of hash values of big tables
* Distribution of hash values from the mapping table
* Searching for data in a distributed network by their identifier
* Searching for connectable nodes by the hash value of their addresses in the table

Which disadvantages are inherent in DHT?
* Block generation time depends on the table size
* A large number of requests is required to receive one reply
* Anyone can trace the entire request history of a particular node
* Every network node must store its own copy of the same table

What are the applications for the web-of-trust concept?
* Decentralized identification systems
* Relative reputation systems
* Increasing the trust level of clients of new web services
* Peer-to-peer public key infrastructures

Which features are built in the BitMessage protocol?
* It is impossible to determine the sender’s and recipient’s addresses when the message in the network is captured 
* Messages are included in a Merkle tree structure to validate their integrity
* A message is transmitted from one node to another until it reaches its recipient
* Every node stores all recent messages

Which features are inherent in the IPFS protocol?
* Files are split into parts and stored only by interested nodes
* History of added files is kept in the shared database and its integrity can be audited
* It is possible to add updated files with a link to the previous version
* Every file transmitted over the network has a digital signature of the node that initially published it

What disadvantages does the IPFS protocol have?
* Initial network node synchronization takes a lot of time
* The file availability depends on the network presence of nodes that store it
* If no one downloads the file during a long period of time it gets completely deleted from the system
* System users have no motivation to store and distribute files that they no longer need

What are the fundamentals of blind signatures?
* The signer does not use his private key
* The signer does not see the entire document or its part
* The signer cannot verify his signature
* The signer cannot trace the signed document

What are digital keys used for?
* Data encryption
* Data decryption
* Proof-of-work generation
* Calculation of a digital signature
* Verification of a digital signature

What are digital keys not used for?
* MAC calculation
* MAC verification
* Constructing the Merkle tree
* Calculating a shared secret
* Generating a hierarchical key

What methods of digital key generation exist?
* Random/pseudo-random number generators
* Hashing the current timestamp
* Hashing a password
* Using shared secret generation algorithms

What is the principle behind the man-in-the-middle (MITM) attack?
* It is a passive kind of attack when an attacker eavesdrops on the communication channel between two users
* It is an active kind of attack when an attacker substitutes all the data in the communication channel between two users
* An attacker scans the communication channel to extract confidential data
* All the traffic passes through an attacker while he introduces himself to both participants as a user 

What is a Merkle branch used for?
* To verify whether a specific data fragment matches a Merkle root
* To add new data fragments to an existing Merkle root
* To check whether the number of leaves in the Merkle tree is even
* To add right leaves to the Merkle tree with a specific root

Which statement is true about one-time signature schemes?
* The signer does not see the entire message or some of its parts
* The private key owner has no motivation to reuse it
* To calculate a signature correctly, the private keys of several specific participants are required
* The participant who signed a message remains anonymous within a specific group

Which statement is true about the multisignature scheme?
* The signer does not see the entire message or some of its parts
* The private key owner has no motivation to reuse it
* To calculate a signature correctly, the private keys of several specific participants are required
* The participant who signed a message remains anonymous within a specific group

Which statement is true about the threshold signature scheme?
* The signer does not see the entire message or some of its parts
* The private key owner has no motivation to reuse it
* To calculate a signature correctly, the private keys of several specific participants are required
* The participant who signed a message remains anonymous within a specific group

Which statement is true about the group signature scheme?
* The signer does not see the entire message or some of its parts
* The private key owner has no motivation to reuse it
* To calculate a signature correctly, the private keys of several specific participants are required
* The participant who signed a message remains anonymous within a specific group

Which statement is true about the ring signature scheme?
* The signer does not see the entire message or some of its parts
* The private key owner has no motivation to reuse it
* To calculate a signature correctly, the private keys of several specific participants are required
* The participant who signed a message remains anonymous within a specific group

Which features are related to the blind signature scheme?
* The signer does not see the entire message or some of its parts
* The private key owner has no motivation to reuse it
* To calculate a signature correctly, the private keys of several specific participants are required
* The participant who signed a message remains anonymous within a specific group

Which statement is true about the Bitcoin Script?
* All network nodes must exchange messages in the Bitcoin Script language
* Operations are performed on the data stack
* Bitcoin Script does not support conditional branching
* Arbitrary conditions cannot be implemented

What is the foundation of the public key compression method for ECDSA?
* The ability to remove the redundancy in key data
* The ability to obtain the preimage of a public key
* The ability to calculate the absolute value of the second coordinate of an elliptic curve point when the first one is known
* The ability to calculate the hash value of a public key and use as a condition to spend coins or in balance accounting

What is a compressed public key?
* Elliptic curve coordinates that are added in the Cartesian coordinates
* A prefix and the Y coordinate
* A prefix and the X coordinate
* A public key cannot be compressed

What is a serialized transaction?
* The values of all transaction fields in the byte form that are recorded in a strict order and form a single sequence
* A transaction body with its header, hash value, and timestamp
* A transaction prepared by a wallet to be presented in human-readable format to a user
* A transaction that was successfully verified but has not been confirmed by validators

For which purposes does the Bitcoin testnet exist?
* To test network equipment of centralized exchanges
* To conduct experiments and demonstrations in conditions similar to the real system
* To test and debug the software for wallets, nodes, payment gateways, and other infrastructure elements
* To hide the network address of a transaction sender

How do Bitcoin protocol updates take effect?
Every node downloads the updates from the master server and installs them
The majority of validators update their software
The majority of the computational power is turned to create a block according to new rules

Which methods exist to protect the queuing of distributed accounting systems against a flood attack?
* Adding PoW to requests
* Paying for every request
* Anonymizing the outgoing traffic for 51% of distributed network nodes
* Coding the data with a one-way adder
* Presence of the hash value of the previous block header

What are the advantages of payments in the Lightning Network compared to on-chain transactions?
* Instant confirmation
* Low or zero fees
* Less demanding digital wallet
* Higher privacy level
* No need to back up a digital wallet

Which are the disadvantages of payments in the Lightning Network compared to on-chain transactions?
* Backing up the wallet and moving it to another device is harder
* The need to constantly trace the state of opened channels
* Minimum payment amount limit of 0.001 BTC
* Inability to use the cold storage wallet option

How do the participants of a bidirectional payment channel update the coin distribution between themselves?
* They make one shared transaction with two outputs and new amounts
* One of the participants completes the signature and publishes a refunding transaction
* They generate a new channel state and exchange the secret values to cancel the previous state
* They make one follow-up commitment transaction that cancels the previous transaction

How can a bidirectional payment channel be closed?
* One of the participants can publish the final commitment transaction
* The participants must exchange the secret values and publish two refunding transactions
* The participants can jointly make and publish one refunding transaction
* The participants can jointly cancel the funding transaction and confirm the coin refunding

What is the idea behind an atomic swap?
* The ability to exchange digital assets between two users of two different accounting systems
* The ability to exchange digital assets without depositing them by a trusted third party
* The ability to instantly exchange digital assets with zero or minimum fees
* The ability to directly transfer digital assets from one accounting system to another

What features do accounting systems require to enable adding support of atomic swaps?
* The ability to use multisignature and time locks in conditions of coin spending
* Presence of an order book for storing half-signed transactions
* Presence of a separate field for a payment description
* The ability to lock coins using the hash value of the secret

What is the advantage of using proof-of-stake to reach consensus compared to proof-of-work?
* The block formation frequency can be higher while the efficiency stays the same
* No limitations for the number of validators
* A validator can remain anonymous
It is unprofitable for a validator to perform 51% attacks

Which requirements must an efficient proof-of-stake consensus algorithm meet?
* It’s impossible to predict the order of validators
* Preventing the queue of unconfirmed transactions
* Penalties for creating blocks in alternative chains
* Ensuring that blocks are added in regular intervals

What is the idea behind the consensus mechanism based on DPoS?
* No limitation on the number of validators 
* Validators have different weights in confirming transactions 
* Any base currency owner can participate in the process of choosing validators
* A strict order of validators to form the chain of blocks

Which features are related to BFT-class consensus algorithms?
* A block that has gained the threshold number of validators' votes cannot be replaced
* All validators have equal weight during transaction confirmation
* Only active users can change the current validator group
* The block formation time depends on the coin amount a validator holds in the base currency

Which features are related to the FBA consensus mechanism?
* All the validators have equal weight during the transaction confirmation
* The block formation time depends only on the performance of validators and data transmission channels
* The node owners can choose who they include in their validator list
* There must be sufficient number of trust circle intersections in the network to reach a global finalized state

Which features are related to hashgraph-based consensus?
* All validators are known, and their number is limited
* Even honest validators' hash graphs are never completely equal
* When creating a new event, a validator includes the hash values of any two existing events in it
* Events are added to a graph asynchronously, and every event only has the signature of its creator

What are the advantages of the Grin cryptocurrency, which works according to the MimbleWimble protocol?
* Payment amounts in the transaction body are hidden
* A convenient mechanism to delete redundant history data
* Atomic swaps and Lightning Network support
* Zero fees and 2% cashback for every trade

What are the disadvantages of the Grin cryptocurrency?
* A payment recipient must participate in the transaction creation
* Inability to set additional coin spending terms
* Big transaction size
* Inability to use the hierarchical key generation

Which features are related to the Bitshares protocol?
* Accounting model based on accounts and balances
* Support of the Lightning Network based on bidirectional channels
The ability to issue custom assets
* Flexible permissions customization for account management

What is an Ethereum smart contract?
* A fast transfer with no fees
* A transfer of funds to a general address
* A contract that is written on a language readable by both humans and machines
* Conditions executed by a decentralized computer

What do Ethereum smart contracts enable?
* Executing the conditions which could not be executed before
* Solving common problems while avoiding the need to trust someone
* Describing and executing the contract logic

What are the advantages of Ethereum smart contracts?
* The correct execution is guaranteed by the consensus
* There is no need to involve a third party
* The contract conditions cannot be modified after being signed
* Coins can be obtained out of thin air

Who can create an Ethereum smart contract?
* Only a public key owner
* Only an engineer
* Anyone

Who can execute the code of an Ethereum smart contract?
* Only the lord of consensus
* Only an engineer
* Only a public key owner
* Anyone

Who can view the result of executing an Ethereum smart contract?
* Anyone
* Only a chosen one
* Only an engineer
* The one who has executed its code

[GLOSSARY OF TERMS](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-2/en/10-glossary-of-terms.md)
