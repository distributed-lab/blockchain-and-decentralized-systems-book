# [3 APPLYING THE CONCEPTS OF SHARDING, OFF-CHAIN, AND DAG](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/3-Applying-the-concepts-of-sharding-off-chain-and-DAG.md)

# 4 PRIVACY AND ANONYMITY ON THE INTERNET


## 4.1 Operating principles and application of Tor

This subsection discusses the most popular approaches to increasing the level of anonymity of a network user by hiding certain data from other users or outside observers.

The concept of privacy includes the two main components: untraceability and anonymity. Untraceability implies that it is impossible to detect an association between the actions of a particular network user. Anonymity means the inability to reliably identify a user in this network.

At the same time, it should be understood that the concept of user privacy includes the following different aspects: technical (the association of user actions with specific hardware or software), social (the association of a user with his social circle and interests), time-related (the association of user actions with the time of day), behavioral (the association of a user with a text input manner, languages ​​used, etc.). Therefore, anonymity does not have a defined threshold, units, or absolute value.

In practice, it all boils down to that, on the one hand, people invent analysis methods that allow making an association between actions and users, while on the other hand, people improve the methods to hide such associations, thus increasing the complexity and cost of analysis.

It is worth noting the difference between _anonymity_ and _pseudonymity_. A network user is considered _anonymous_ if it is impossible (or at least very difficult in practice) to determine the user’s network address, location, personal data or assign to that user an identifier by which that particular user can be identified in future. If user actions are associated with an abstract identifier, then it is considered _pseudonymous_. For example, if a user has created an account using fake data and uses it constantly, then he is no longer anonymous. At best, he is pseudonymous [82].


### Approaches to anonymizing a network user

> * _Sending traffic through a proxy server_
> * _Using onion routing_
> * _Using onion routing and a dedicated browser_
> * _High latency data distribution systems_

Transferring traffic through a proxy server, using an SSH tunnel, connecting via a VPN or similar intermediaries allows hiding the network address of a user from the resource that the user accesses. A third-party observer who sees the traffic between the user and the proxy server cannot determine which resource the user is accessing.

The disadvantage of this approach is that the intermediary itself, who acts as a proxy server, knows which resources all its users connect to. In addition, users usually set up a connection with one proxy server and then use it for a long time, thus allowing a third-party observer and the resource itself to deduce that all actions are performed by the same person without knowing the actual network address of the user. This approach is also ineffective against a global observer who, by analyzing the traffic throughout the network, would simultaneously see a correlation in time and packet size between the sender and receiver – even despite the fact they are not sent directly.

The _onion routing_ or its modifications allow users to hide the network addresses of the recipient and sender not only from an outside observer but even from intermediaries. Also, the presence of a large number of nodes from which the user can choose intermediaries allows the user to rearrange the route as often as the user wants and even use several different chains of intermediaries simultaneously for different connections. Nevertheless, this approach is still ineffective against the global observer, and it does not protect the user from intentional attempts that online resources perform to assign an identifier based on browsing data such as the browser name and version, script execution characteristics, screen resolution, saving and using data about previous sessions, etc.

Using the previous approach together with an anonymity-concerned browser protects the user from different ways of identification by the resource that the user accesses. Such a browser does not provide real data about the operating system, the browser, enabled plug-ins, and real screen resolution. It also allows completely disabling script execution and not saving data about previous sessions. The task of such specialized browsers is to make all their users “look the same”. Obviously, this browser is useless if the user visits resources that require registration and uses the same account(s) over and over again. For example, the browser’s “identity shield” is useless if a user repeatedly accesses resources requiring the sign-up and utilizes the same account. 

The approach that involves using data distribution systems with long delays allows users to hide the connection between the sender and the recipient as well as data transmitted between them. Furthermore, even a global observer cannot see a correlation in the transmission of data between participants of such a system. The basic principle of this approach is that data is transmitted through intermediaries, but the entire transmission route is not known in advance. One message can be divided into several components, each transmitted independently from the others.

In order to transmit parts of different messages between intermediaries, they are randomly combined and wait for a random amount of time while the recipient receives them in an asynchronous order. The transmission time of one message from the sender to the recipient can reach 30 minutes. Obviously, the communication that is organized in such a way is not a general-purpose data transmission system. It cannot be used to access traditional HTTP servers, make audio/video calls or chat in real time. Protocols that use this approach in order to increase anonymity usually implement an isolated communication environment like e-mail, public forums, or microblogs.


### Operation principle of onion routing

The concept of onion routing was first proposed in 1995 by the US Navy. Initially, these studies were funded by grants from the US government. The source code for the modern version of Tor (short for _The Onion Routing_ project) software was published under a free license in October 2003, so that everyone could check the absence of errors and backdoors. Later, a non-profit organization was founded that has led the development of the Tor Project. This organization was funded by various sponsors. In order to become completely independent from the US government budgeting, in 2015 the project began to accept donations from individuals.

The main task of onion routing is to hide from an outside observer the fact that one network user communicated with another. The idea is to transfer data packets not directly from the sender to the recipient but through a chain of several intermediaries, where intermediaries know only the part which is necessary to transmit the packet but not the entire path of a particular packet.

The name “onion routing” comes from the use of “multilayer” encryption. Each protective “layer” of a data packet is associated with a specific intermediary node and can be “peeled” only by the respective intermediary. Since data itself and the fact of its transmission on the network are obscured from an outside observer, the Tor network and similar networks are called _dark networks_.

Let’s look at how onion routing works using an example of data transfer between Alice and Bob. Those who want to use onion routing, including Alice and Bob, start using the same protocol to form a peer-to-peer network that uses the Internet as the transport. The nodes of such a network have keys for asymmetric encryption and exchange network addresses and public keys of each other so that everyone knows everyone else and everyone can connect and send a data packet that only the recipient can decrypt.

Alice encrypts the message she wants to send to Bob using Bob’s public key. Next, Alice forms a data packet _P1_ in which she places an encrypted message and indicates Bob’s address as the recipient (Fig. 4.1). Alice now selects a random network node _R1_ and encrypts packet _P1_ with the public key of node _R1_. Next, Alice forms packet _P2_, in which she places the encrypted _P1_ and indicates the address _R1_ as the recipient. In the same way, Alice selects nodes _R2_, _R3_ and forms packets _P3_ and _P4_.

![Figure 4.1 – Data encryption scheme for onion routing](/resources/img/volume-3/4.1-Operating-principles-and-application-of-Tor/F-4.1-data-encryption-for-onion-routing.png "Figure 4.1 – Data encryption scheme for onion routing")

As a result of the “multilayer” encryption of Alice’s message, Bob receives data packet _P4_, which is called _onion_ due to its multilayer structure. Alice sends this packet to node _R3_ via the usual Internet (Fig. 4.2). Node _R3_ can decrypt the received packet and receive packet _P3_, which is addressed to node _R2_. In the same way, node _R2_ sends packet _P2_, and node _R1_ sends packet _P1_. As a result, Bob decrypts the data of packet _P1_ with his private key and reads the message. Note that to send his reply to Alice, Bob can either build the route through a new set of intermediaries or use the route built by Alice (that depends on a specific protocol).

![Figure 4.2 – Data transmission scheme for onion routing](/resources/img/volume-3/4.1-Operating-principles-and-application-of-Tor/F-4.2-data-transmission-for-onion-routing.png "Figure 4.2 – Data transmission scheme for onion routing")

In this scenario, an outside observer who can view the data packets transmitted via the Internet cannot see the communication between Alice and Bob. An outside observer can see packets _P1_, _P2_, _P3_ and _P4_ but cannot link them together since they contain different ciphertexts, and the keys are unknown. Each of the intermediary nodes also cannot reveal the fact of Alice and Bob’s communication since they know only the addresses of neighbor nodes. The fact of message transfer between Alice and Bob can be proved only if _all_ involved intermediaries conspire and publish all the intermediate data that they processed. However, this is made unlikely to happen by choosing intermediaries randomly from many independent nodes.

When such approaches to routing are used, different disclosure methods, based on analyzing timings of data packet appearance on the network, data packet sizes, etc., can be implemented. However, protocols implementing such routing can use mechanisms to significantly complicate packet analysis and route determination. These mechanisms include composite packets, dynamic routes, etc. Also, different protocols are configured for a different number of randomly selected intermediaries; their number is usually two or three.


### Tor application features

In 2019, there were more than two million users in the Tor network and more than 7,000 active nodes involved in traffic transmission. They are located around the world and supported by volunteers who agree to contribute part of their channel capacity to maintain the network.

Tor capacity and anonymity levels depend on the number of nodes: the bigger the number is, the better. This is because the throughput of each node is limited, and a large number of nodes is far more difficult to analyze.

By default, traffic of Tor users passes through 3 randomly selected nodes. So, the traffic is routed in the following manner:

* Client
* Guard relay
* Middle relay
* Exit relay
* Destination

First, the user encrypts the data so that only the exit relay can decrypt it. The resulting ciphertext is then encrypted so only the middle relay can decrypt it. After that, the received ciphertext is encrypted again so that only the guard relay can decrypt it (Fig. 4.3).

![Figure 4.3 – Data transmission process in Tor](/resources/img/volume-3/4.1-Operating-principles-and-application-of-Tor/F-4.3-data-transmission-in-Tor.png "Figure 4.3 – Data transmission process in Tor")

In general, launching and maintaining a network node that acts as a guard or middle relay transmitting traffic to other users is considered safe. It is difficult to accuse the operator of this node of distributing prohibited content or similarly unlawful or illegal actions since he receives and sends data only in the encrypted form and does not have keys to receive or decode the original data.

In this context, operators of exit relays have a much greater responsibility and are at a far greater risk. If an attacker performs illegal actions using the Tor network, then these actions will not be associated with the computer of the attacker, but with the exit relay that the attacker chose. For ordinary people, this means that starting and maintaining the Tor exit node can lead to somewhat unpleasant situations.

Another problem is that if the sender and the recipient do not additionally ensure the confidentiality of the transmitted data (for example, use HTTP or FTP), then the exit relay will see this data as plain text.


### Obtaining a list of Tor nodes

After the Tor network client has launched, it needs to get the lists of all guard, middle, and exit relays. Compilation, updating, and distribution of these lists are handled by a special service consisting of many servers that are supported by the protocol developers. Lists with addresses of Tor network nodes must be publicly available – anyone can get them in practice. However, what relates to it is an issue of blocking.

Interested parties can use the lists of all nodes’ addresses to block the use of Tor. In theory, there are two options for this: block traffic from the exit relays or block traffic directed to the guard relays.

The first option is implemented directly on the side of web services. To do this, it is enough for them to fetch the list of Tor exit relays and block all traffic from them. In such a case, some web services are completely inaccessible from the Tor network, and it is practically impossible to fight this.

The second option is far worse: blocking traffic directed to incoming nodes would prevent Tor from being used at all. If such a scenario is implemented by ISPs around the world, Tor will become useless. But Tor developers came up with a good solution to this problem – users can connect to the guard relays indirectly, through _bridges_.

Bridges in the context of Tor are nodes that act as intermediaries between the user and the Tor network, that is, they are extra links between the client and the guard relay. Bridges are equipped by users who cannot connect to the Tor guard relay directly, or who want to hide the fact of using Tor from an outside observer. The list of bridges’ network addresses is also compiled and updated by specialized developers’ servers; it is never made fully available to the public, though. Anyone can send a request or send an email to receive information about bridges, but the service returns the addresses of only a few bridges at a time. This approach prevents bridges from being entirely blocked and still lets the user requesting the bridges’ addresses access the network.


### Methods for deanonymizing dark network users

In the process of deanonymizing dark network users, the attacker’s goal is to determine an association either between the data and the sender, or between the data and the recipient, or between the sender and the recipient. In general, an attacker can perform the following actions to achieve this goal:

* View the entire traffic of a chosen network participant
* Monitor traffic of two suspicious parties simultaneously
* Create, modify, delete, or delay traffic
* Maintain multiple intermediary nodes
* Compromise or gain control of multiple existing intermediaries

Let’s examine a method of deanonymizing a user based on the correlation of data packets over time since it is simple and effective for most networks with low latencies. In this case, an attacker can abstract from the features of the anonymization system and perceive it as a “black box”. In this case, it is enough for him to observe all incoming  and outgoing traffic of the suspicious parties (for instance, the user and the resource) and perform a non-complex analysis of it (Fig. 4.4). As a result, the attacker can determine whether the suspicious parties communicate with each other or not (for example, whether a particular user receives service from a particular resource or not).

![Figure 4.4 – Scheme of an attacker connecting to the suspicious parties](/resources/img/volume-3/4.1-Operating-principles-and-application-of-Tor/F-4.4-attacker-connecting-to-suspicious-parties.png "Figure 4.4 – Scheme of an attacker connecting to the suspicious parties")

According to this method, an attacker must record the exact time when each sent or received packet of two suspicious parties appears on the network. Depending on the order and intensity of interaction between the parties, the attacker can collect data for analysis for several minutes or hours. The data analysis implies finding matches between the group of sent packets and the group of packets received in a particular time gap (Fig. 4.5), i. e., correlating the “send” and “receive” timings. For example, a user sends a request to a resource consisting of seven consecutive packets, and after 20 milliseconds the resource receives seven consecutive packets. At the same time, the attacker does not analyze the contents of the packets and the addresses specified in their headers, since the parties are assumed to use multilayer encryption and a chain of intermediaries.

![Figure 4.5 – Scheme of detecting packet correlation by time](/resources/img/volume-3/4.1-Operating-principles-and-application-of-Tor/F-4.5-detecting-packet-correlation-by-time.png "Figure 4.5 – Scheme of detecting packet correlation by time")

To confirm the fact of the interaction between the suspicious parties, an attacker must find many matches between the groups of sent and received packets. However, it is also worth considering that the suspicious parties can exchange packets not only with each other but also with other network participants, thus increasing the number of false matches or reducing the number of true matches. However, by actively intervening in the packet exchange, the attacker can confirm his guesses. To do this, the attacker needs to monitor changes in the packet stream of the receiver while at the same time selectively deleting or delaying part of the sender packets.

There is no universal method of deanonymization for all dark networks as well as no method that would guarantee results within a certain timeframe. We can only say that every attack requires significant resources. For example, for the attack with malicious intermediaries, it is necessary to launch and maintain a substantial number of fake nodes of the anonymizing network. And for an attack based on detection of time correlation, an ordinary observer needs to take significant time to check all suspicious parties in pairs.

Theoretically, an attacker may not be an ordinary observer but the so-called _global observer_ – the one that simultaneously monitors all data transmission channels. A global observer can inspect any suspicious parties simultaneously. Dark networks with low latencies usually do not consider a global observer in their threat model; such an observer is practically unlikely to exist [83].

### \*\*\*Frequently asked questions\*\*\*

_– What is the average data transmission performance available for a Tor network client?_

The average transmission time for a file with 50 kiB size (a page of text) is 0.5–2.5 seconds, and for a file with 5 MiB size (a photo) is 5–20 seconds, depending on the selected intermediary nodes.


## 4.2 Tox design 

Tox is one of the most popular decentralized messenger protocols. Its main advantages are confidentiality, integrity, and authenticity of transmitted messages. Furthermore, it hides the relationship between the user’s network address and their messages.

The starting point for the creation of Tox was the disclosure of information about NSA surveillance programs by Edward Snowden. After documents revealing references of Microsoft’s massive collaboration with government agencies and the details of Skype wiretapping were published, the discussion started about creating a secure messenger that would have at least the same functionalities as Skype and would be simple enough to use for an average user [86].

Over time, many applications running under the Tox protocol and supporting popular operating systems appeared. This subsection discusses the most prominent features of the structure and implementation of Tox.


### How Tox accounts are designed

Any Tox user can have more than one account. A user’s long-term public key acts as a unique identifier for his account. The user generates a corresponding key pair for digital signatures using the ED25519 algorithm (the length of the private and public keys is 256 bits). The user can attach a name (or pseudonym), a photo, description, and similar data to his account. This data is stored by the application of the user and transmitted only to his contacts (each time he connects to the contact and updates the data).

A contact in Tox is an account of another user with whom one exchanged long-term public keys. The important step for adding a contact is to confirm the request in an application that processes the corresponding accounts. In order to detect another user on the network and send him a request to add to contacts, one needs to know his Tox ID.

A Tox ID is generated from the following data: a _long-term public key_, a _nospam value_, and _checksum_. Tox ID is represented with one hexadecimal number (Fig. 4.6).

![Figure 4.6 – Tox ID structure](/resources/img/volume-3/4.2-Tox-design/F-4.6-Tox-ID-structure.png "Figure 4.6 – Tox ID structure")

The _public key_ is used for mutual authentication of users and installation of session keys. Throughout its lifecycle, a particular account has the same long-term key pair. If a user wants to change a long-term key pair, he needs to create a new account, send the new Tox ID to other users and re-add them to the contact list.

The _nospam_ value is a part of Tox ID that is used to protect against spam attacks that send a large number of requests for adding a contact. It takes up 4 bytes and can be changed at any time. If an attacker knows the victim’s long-term public key and the actual _nospam_ value, then he can send a large number of correct requests for adding contacts in the network. The victim’s application will be forced to process them all. In this scenario, the victim can resolve it by simply generating a new _nospam_ value and waiting for requests to add a contact using the new Tox ID. Meanwhile, all requests to the old Tox ID will be invalid.

The checksum value takes up 2 bytes and is necessary to detect errors after transmitting and entering Tox ID. The checksum is calculated as a sequential XOR of 36 bytes of the data (32 bytes of the public key + 4 bytes of the _nospam_ value) that are divided into blocks 2 bytes each [87].


### Node and application types

Each Tox node is identified by a network address, port number, and public key. All existing nodes can be divided into two types: _bootstrap nodes_ and _client nodes_. A _bootstrap node_ only deals with network maintenance  (it connects to other nodes, transfers their data, supports DHT, etc.) and does not directly interact with user applications. A _client node_, in addition to maintaining the network, also acts as a user application or chatbot that connects to contacts and processes messages.

Generally, an outside observer cannot determine the type of a particular network node since it is impossible to determine whether it transmits its own messages or broadcasts messages from other nodes. In this context, note an important feature: the application and the node use different key pairs: one for onion routing, the other for contact authorization.

To connect to the Tox network, the user needs to have data to connect to at least one active node, which will help reveal the rest. By default, a list of public bootstrap nodes that run on the Internet is used to perform this function. Communication with other nodes is necessary to maintain DHT, as well as to search for and connect to users from a user’s contact list.

Tox can also work quite efficiently on the intranet or in an isolated local network. To do this, the _libtoxcore library_ implements package sending to broadcast addresses, which allows users to connect to a private Tox network without access to the Internet (provided that there is a node in the network segment). Thus, even two Tox nodes are enough to ensure the interaction of local clients [88].


### Connection of parties

Tox nodes operate on the network publicly: their public keys and network addresses are available to anyone, whereas each node is ready to respond to a request from any other node. Unlike the node, the user application is designed to _not_ disclose user information. For example, it is not enough for a third-party observer to know the Tox ID of a particular user to determine the user’s network address and vice versa. Therefore, a direct connection cannot be used even to exchange requests to add a contact and transfer data to authenticate an already known contact. Tox applications use the onion routing mechanism to hide the network addresses of users.

Let’s examine the process of establishing communication between Tox users. For example, Alice and Bob know each other’s Tox IDs and have launched Tox applications but have not yet established a connection. Each of them has to announce their appearance online so that contacts can detect them. To do this, Alice and Bob randomly select one node – _A_ and _B_, respectively – in such a way that the public key of node _A_ has a value close to Alice’s public key, and the public key of node _B_ has a value close to Bob’s public key (Fig. 4.7). Next, Alice and Bob use onion routing to transmit values of their public keys to nodes _A_ and _B_, respectively, so that they can accept and transmit the connection request without knowing the correspondence between the counterparty’s public key and his/her network address.

![Figure 4.7 – Building a route to announce the public key](/resources/img/volume-3/4.2-Tox-design/F-4.7-building-route-to-announce-public-key.png "Figure 4.7 – Building a route to announce the public key")

User applications regularly check the status of their contacts (online/offline) by sending requests to search for their public keys in DHT. Let’s suppose Bob makes this request using Alice’s key and it leads him to node _A_. Then Bob forms a connection request with Alice and, using onion routing, passes it to node _A_, which, in turn, sends it to Alice (Fig. 4.8).

![Figure 4.8 – Sending a request through the node announcing the key](/resources/img/volume-3/4.2-Tox-design/F-4.8-sending-request-through-node-announcing-key.png "Figure 4.8 – Sending a request through the node announcing the key")

After receiving the connection request, Alice performs its identification and authentication. Having determined that this request was sent by Bob, she forms a response and uses onion routing to send it to node _B_, which forwards it to Bob. Bob, in turn, checks the answer, making sure that it was generated by Alice (Fig. 4.9).

![Figure 4.9 – Responding through the node announcing a key](/resources/img/volume-3/4.2-Tox-design/F-4.9-responding-through-node-announcing-key.png "Figure 4.9 – Responding through the node announcing a key")

When sending such requests and responses, participants exchange session public keys and can also disclose their own network addresses to each other to establish a direct connection. If participants do not want to share their network addresses, they can use nodes supporting TCP relay (Fig. 4.10).

![Figure 4.10 – Connection options for parties after mutual authentication](/resources/img/volume-3/4.2-Tox-design/F-4.10-connection-options-for-parties.png "Figure 4.10 – Connection options for parties after mutual authentication")


### TCP relay nodes

One of the problems for most P2P applications is that a significant part of users only support outgoing connections. It means that knowing the address of the desired node and the port number that it “listens” to, one can initiate a connection with it. However, due to network equipment settings, firewall restrictions, operating system, etc., nodes themselves cannot accept connections from those who wish to connect to them. In addition, there may be reasons why a user cannot process UDP traffic. In decentralized systems, this poses a real issue: if two participants need to connect to exchange the data (to make an audio or video call, for example) and none of them can accept the connection from the counterparty, they can only connect through an intermediary. However, in a decentralized system there cannot be only a single node to which a significant part of all users connect. Tox developers added the TCP relay feature to the node software that allows any network node capable of accepting incoming connections to act as an intermediary between other users. Now users who cannot connect directly can choose their own intermediary.

This works as follows. When the Tox client starts, it attempts to connect to the necessary nodes directly by using UDP. Regardless of the result (i.e., even if the connection is successful), it connects to one of the nodes with the TCP relay function, which will be ready to serve as a “failsafe” if problems with the direct connection arise. If the client cannot open a direct connection, it will use TCP relay by default, but still continue attempts to connect directly.

In turn, a feature of nodes supporting TCP relay is that they try to use commonly known port numbers: 80 (HTTP), 443 (HTTPS) and 3389 (RDP). This makes it difficult to track and filter Tox traffic, at the same time allowing it to pass through most firewalls that restrict specific port numbers [89].


### Using DHT to find contacts in the network

The DHT module uses a separate key pair, generating a new pair each time it launches. The corresponding public key is the node identifier in DHT, i.e., the correspondence between this key, address and port of the node is saved in a distributed table as a separate line.

When the application works, it monitors the state of all user contacts. For each contact with which there is no connection, the application looks for its Tox ID in DHT. The search result is the network address of the node that was selected to announce the public key of this contact.

If the connection with the contact is successfully established, the contact is set online and the user can start exchanging text messages and files or initiate a call. When the contact disconnects, it is immediately set offline. Noteworthy, by default, messages are stored only by the parties themselves, and their transmission is possible only when there is a connection between the parties.

### \*\*\*Frequently asked questions\*\*\*

_– How much traffic does the Tox node consume, not including the application itself?_

If you separately run such a node (Tox-bootstrap), then its traffic consumption will be about 250 kbit/s, which is about 2.5 GiB per day.

_– Is it possible to determine the type of messages transmitted between users by analyzing the size and frequency of packet transmission between them?_

This analysis method is quite effective and can even be used against protocols using onion routing. To complicate the process of tracking transmitted message type by packet size, Tox network members add a random number of random bytes to each packet. Therefore, the same message is transmitted between the transit nodes in packets that have different sizes.

_– Tox ID is quite a long string and it’s inconvenient to enter it from memory or print on a business card. What is the best way to solve this issue?_

The Tox community has approved and implemented the DNS Discovery standard, which allows associating Tox IDs with shorter names. Using it, users can register addresses in the standard e-mail address format like username@example.com.


## 4.3 Invisible Internet project

Invisible Internet project (I2P) is a peer-to-peer overlay data transferring network with a high level of user anonymity operating without central servers. I2P was launched in 2003 with the goal to create an anonymous channel for data propagation with low latency. As compared to alternative means, it was expected to achieve greater distributiveness and better resistance to attacks. The I2P project also implies that the traffic is transmitted through a chain of intermediate nodes. However, the methods for finding nodes and organizing the data packet flow are different.

The I2P network is primarily focused on transferring data between its participants. Therefore, both parties (for example, the user and the resource) must use special software – an _I2P router_. I2P users are also able to connect to resources located on the regular Internet; however, such kind of connection works only through the correspondingly configured nodes (_outproxy_), which are more rare.


### Garlic routing

Garlic routing technology was proposed by Michael Friedman in 2000; it is a modification of the onion routing [61]. In onion routing, each packet between a sender and receiver is transmitted separately by intermediaries. The idea of garlic routing is that such a packet will not be transmitted individually, but in a group with other packets that have to be sent to the same node at the same time, as if a garlic bulb containing many separate cloves is transferred from one place to another.

Thus, network nodes exchange so-called _garlics_ consisting of packets from different parties that have the same part of the route. In this case, such packets are called _garlic cloves_. Fig. 4.11 shows a simplified diagram of how the garlic is processed by a network node.

Bob receives the garlic from Alice with cloves C, E, and F. They are to be transmitted to Carol, Eva, and Frank respectively. At the same time, Bob receives a garlic from Dave, where clove B is for Bob himself, and cloves F, E, and C are to be transmitted to Frank, Eva, and Carol respectively. Having these two garlics, Bob unpacks them and finds out the recipients of each clove. Having done that, he forms three new garlics. Bob, in turn, wants to send Eva (or send through her) his own data packet, so he adds another clove to the garlic that was formed for Eve.

![Figure 4.11 – A simplified scheme of garlic routing](/resources/img/volume-3/4.3-Invisible-Internet-project/F-4.11-simplified-garlic-routing.png "Figure 4.11 – A simplified scheme of garlic routing")

When creating a clove, a multilayer encryption scheme is used. If Alice wants to transmit the message to Bob, she encrypts it so that only Bob can decrypt it and then adds Bob’s address to the received ciphertext (Fig. 4.12). Alice encrypts the received packet in such a way that only the intermediary can decrypt it, and she adds the address of the intermediary to the received ciphertext. In this way, layers are added for each intermediary and only then the clove is included in the garlic. The garlic itself is encrypted so that only the recipient node can decrypt it.

![Figure 4.12 – Scheme of multilayer encryption in garlic routing](/resources/img/volume-3/4.3-Invisible-Internet-project/F-4.12-multilayer-encryption-in-garlic-routing.png "Figure 4.12 – Scheme of multilayer encryption in garlic routing")

Noteworthy, an outside observer does not see the cloves of garlic transmitted between the nodes of the network; he can only see the whole garlic. Therefore, an outside observer does not know whether someone is a recipient or the transit node for a particular clove. The transit node also does not know its place in the intermediary chain for the clove. To complicate the process of tracing the full route for a specific clove by the size of the garlics, a random number of random bytes is added to each clove when it is formed [84].


### Selecting intermediate nodes and creating tunnels

An important object in I2P is the _tunnel_. A tunnel is an oriented data transmission path through several intermediary nodes. Data can be sent through a tunnel only in one direction, so users usually open two types of tunnels: _outbound_ tunnels and _inbound_ tunnels.

In order for Alice to send a message to Bob (Fig. 4.13), she must have an outbound tunnel consisting of intermediaries selected by her, and Bob, in turn, must have an inbound tunnel consisting of intermediaries selected by him. At the same time, Alice does not know Bob’s network address on the usual Internet; she only knows his address on the I2P network, from which she finds the network address of the _gateway_ of Bob’s current inbound tunnel.

Next, Alice forms a clove that will be used to transmit a message through her outbound tunnel to the gateway of Bob’s inbound tunnel. The gateway to Bob’s inbound tunnel, in turn, will form a clove that will transmit a message to Bob through his inbound tunnel.

![Figure 4.13 – Transmitting messages through tunnels in I2P](/resources/img/volume-3/4.3-Invisible-Internet-project/F-4.13-transmitting-messages-through-tunnels-in-I2P.png "Figure 4.13 – Transmitting messages through tunnels in I2P")

In order for Bob to reply to Alice’s message, he needs to have an outbound tunnel, know the network address of Alice’s inbound tunnel gateway and, based on the reply message, form a corresponding clove.

Thus, messages from Alice to Bob and from Bob to Alice are transmitted through completely different routes. Network users can have several inbound and outbound tunnels at the same time (for instance, a separate one for each application or party). The default tunnel lifetime is 10 minutes. This means that regardless of the duration of the parties’ interaction, if it exceeds this 10-minute limit the connection through the current intermediaries will be interrupted, other intermediary nodes will be selected, new tunnels will be created, and communication will resume through the new tunnels.


### Addressing and searching for nodes

The full address of the I2P network resource includes the following data: the public key for asymmetric encryption, the digital signature public key, and certificate data. This data is usually concatenated and encoded in base64, resulting in a string with the length of 516 characters. Shorter addresses are also used; theу are calculated as a hash value of the full resource address encoded in base32. The short version of the address is 52 characters long.

Of course, it is not enough to know the short address of the resource (base32) to connect to it. The full address (base64) must be found given its hash value. Searching for new intermediary nodes, full resource addresses (corresponding public keys), as well as gateway network addresses through which users can send requests to these resources is performed through a distributed database – _NetDB_. This database processes related requests by dividing them into two groups: _RouterInfo_ and _LeaseSet_.

_RouterInfo_ allows receiving data necessary to use other network nodes as transit nodes: network addresses, public keys, and certain statistical data.

_LeaseSet_ allows receiving the data necessary to connect to a specific network node as a resource (party); the fact of communication with this node is to be hidden from the others. In this case, NetDB returns the network address of the recipient’s incoming tunnel gateway, the expiration time of this tunnel, and the recipient’s public keys.

All requests and responses in NetDB are encrypted. This is necessary so that an outside observer cannot find out what kind of data a particular user is looking for. But note also that the hosts transmit RouterInfo directly to each other, whereas LeaseSet is transmitted only through tunnels. It allows avoiding correlation between the network address of the node and the address of the resource at which it is actually located, as well as between the network address of the user and the address of the resource that he is interested in. Due to this, it is considered impossible to establish the actual location of resources and users in the I2P network.

To create her own inbound and outbound tunnels, Alice sends requests to NetDB and collects RouterInfo. Thus, she forms a list of nodes (network addresses and public keys) that she can use as intermediaries for her tunnels.

When Alice wants to send a message to Bob, she first searches NetDB to find Bob’s LeaseSet and get information about Bob’s current inbound tunnels. She then selects one of her outbound tunnels and sends a message through it with instructions for the endpoint of the outbound tunnel to forward the message to one of Bob’s inbound tunnel gateways.

To reply to Alice, Bob also needs to first contact NetDB and get her LeaseSet. But when Alice sends the first message to Bob, she can reduce the response time by adding her current LeaseSet to the message; then Bob will not have to access NetDB when he decides to reply [85].

### \*\*\*Frequently asked questions\*\*\*

_– What is the average capacity available for an I2P network client?_

The average transfer time of a 50 KiB file (a page of text) is 3 to 7 seconds, and transferring a file of several megabytes may take up to several minutes.


## 4.4 Steganographic methods of hiding information

Steganography is a method of storing or transmitting data that allows users to hide its presence among other information that is also stored or transmitted. In this case, the hidden data is called a _message_, and the data that is used to hide the message is called a _container_. A _steganographic system_ (a _stegosystem_) consists of a set of algorithms used to embed, detect, and extract messages from certain container types.

A simple and clear example of a stegosystem is the use of invisible ink and paper with some irrelevant text. In this case, the sender puts his message on top of the visible text (or between the lines) and transfers the container to the recipient as a regular book, a newspaper, or a magazine. The recipient must know that this container has the message embedded in it and how to extract it (Fig. 4.14), i. e., how to properly handle the paper (change the temperature, apply a special reagent, use specific lighting, etc.).

![Figure 4.14 – Example of hidden message detection](/resources/img/volume-3/4.4-Steganographic-methods-of-information-hiding/F-4.14-hidden-message-detection.png "Figure 4.14 – Example of hidden message detection")

While cryptography allows hiding _contents_ of messages from unauthorized parties, steganography allows hiding _the fact that the message exists_. Thus, the message is usually embedded in the container in unencrypted form. However, outsiders may assume that the parties use steganography and attempt to detect and extract messages. Therefore, in a number of stegosystems, it makes sense to embed the message in an encrypted form since it makes it harder or even impossible to detect the message in the container.

For some stegosystems, this works in such a way that even if the third party correctly extracts the message from the container, it will be unreadable. Moreover, the message cannot be distinguished from the noise that is present in such containers even if they do not contain an embedded message.


### Operating peculiarities and advantages

When talking about steganography, one can say that it ensures the safety of the message by following the “security through obscurity” principle, which literally means security due to unawareness. The same principle ensures that software with closed source code is protected from hacking: if an attacker does not know the source code of the program, it is much more difficult for him to find vulnerabilities.

Why is hiding the fact that a message exists with steganography better than hiding the contents of a message using cryptography? The answer to this question may not be entirely obvious. If certain data is protected only by cryptographic methods, it is easy to detect (retrieve, intercept) them. In most cases, knowing the protocol used for encryption and the parties involved is enough to determine the specific algorithm and the party that has the secret. Therefore, if an attacker discovers the existence of encrypted data but cannot read it, he can affect the party that has the secret using different methods, including abusive ones. In case of steganography, it is much more troublesome for an attacker to even detect the presence of the data seeked for.

The main characteristics of a stegosystem are _degree of stealth_, _container capacity_, and _complexity of embedding/extracting_. Degree of stealth determines how difficult it is for a third party to distinguish between a filled container and an empty one (i.e., to detect the presence of a message in it). The container capacity determines the ratio of the container size to the maximum message size that can be embedded in this container. In other words, how efficiently the data channel will be used (for example, every 100 bytes of the container contain 1 byte of a hidden message). Stegosystem complexity determines the number of calculations required to embed and extract the message from the container (for example, the number of elementary operations that need to be performed to embed a message with 100 bytes in size in the container).


### Classic steganography

Steganography was used long before our era to send secret messages about military attacks, etc. For example, wax tablets were used as containers with the message being applied to a wooden substrate that was then covered with wax. Another option involved using clay tablets with the message being cut out on them and covered with another layer of clay with some irrelevant message. Also, there are mentions that the shaved heads of slaves were used as steganographic containers. The message was written or tattooed on the head and after the hair grew back, the slave was sent to the recipient who shaved their head again and read the message.

Classic steganography methods also include writing on the side of a card deck arranged in a specific order. It is also possible to use various stencils that, when put over text, leave only relevant words.

Classic steganography also includes a number of physical methods for hiding messages. The most popular of them uses optical reduction of images or text to apply it to the so-called _microdot_. Microdots creation is similar to photos creation with photosensitive film. Microdots usually had a circular shape with a diameter of several tenths of a millimeter, which allowed them to be embedded in the paper, glued under the postage stamp of a mail letter, transferred by pigeon mail, etc. The recipient usually used a microscope to read the message from the microdot. The main advantage of a microdot is that an uninformed observer cannot detect it even if it is simply attached to a sheet of paper with text, because it looks like an ordinary ink dot [90].


### Computer steganography

This field of steganography is based on using features of computer platforms that allow saving data in such a way that when browsing the file system with ordinary browsers, it will not be detected. Below we discuss some examples.

Many file formats include fields reserved for future format updates. This approach provides compatibility between software versions. So, when these fields are not used, they are filled with zeros. A user can create many such files in the file system. Each of them will be useless in a separate form but will not cause any suspicion. Further, using special software, a user can embed the message (or other files) into the unused fields of these files. The disadvantage of this method is the low degree of hiding.

There was also a method of hiding information in unused areas of floppy disks. The name of the method speaks for itself: hidden data is placed in unused parts of the disk by recording onto the zero track or after the flag indicating the end of recorded data.

Use of special fields and formatting options allows users to add a hidden message to text documents so that it will not be visible during normal display of the document on the screen. For example, a message may be written in white font on a white background in the footnote field. The disadvantage of this method is the low container capacity.

Using unfilled file system clusters makes it possible to hide quite large amounts of data simply and efficiently. In most file systems, a file written to a disk occupies a number of clusters (minimum addressable memory cells). Typically, the cluster size is between 2 kB and 32 kB. Thus, to store a file of 1 kB in size, a memory amount equal to one cluster is allocated on the disk. In this cluster, 1 kB is used to store the file itself, while the remaining 15 kB can be used by special software to store hidden data. Again, this method has a low degree of hiding.


### Digital steganography

The _digital steganography_ field is based on embedding messages in digital multimedia files or streams (images, audio, video). In this case, embedding a message in such a container leads to its distortion. Therefore, for each type of container defined by compression and encoding formats special embedding and extraction algorithms must be used in order to keep distortions below the sensitivity threshold of an average person. Thus, the process of embedding messages does not lead to noticeable changes in multimedia containers. In addition, digitized objects that were initially analog always contain some amounts of noise which increases the degree of stealth of such stegosystems.

Let’s dive into more detail on one of the simplest methods to embed a message in an image. This method is based on modification of the least significant bits. Most often, the image is encoded as a group of pixels, each of which is set by the red, green and blue components (RGB). The brightness of each component can have one of 256 values, meaning that it is set by one byte of the data. The _least significant bits_ (LSB) method involves replacing one or more bits of the pixels with bits of the embedded message in this case. The embedding process is shown in Fig. 4.15.

![Figure 4.15 – Scheme of embedding a message in an image](/resources/img/volume-3/4.4-Steganographic-methods-of-information-hiding/F-4.15-embedding-message-in-image.png "Figure 4.15 – Scheme of embedding a message in an image")

In this example, the message is divided into portions 2 bits each. Then each portion is embedded in one of the components of the pixel. For example, we want to embed 10 10 01 into pixel #103 with the component values R: 35, G: 118, and B: 242. Thus, the modified pixel will have component values R: 34, G: 118, and B: 241. If you replace two least significant bits of each color component its value in the worst case will increase or decrease by 3 units out of 256 possible. Therefore, it is impossible to visually distinguish a modified image from the original. However, if more than 2 bits of data are changed in each color component, this will lead to an image distortion that will be noticeable to the naked eye.

It is also quite simple to estimate the container volume. If 2 bits of data are embedded in each color component, each pixel will contain 6 bits. Therefore, having an image with a resolution of 200 by 100 pixels, a message with a length of 120,000 bits can be embedded.

Obviously, the least significant bits method is applied to image formats without compression or with lossless compression. There are other methods for embedding messages in images that are resistant to transcoding, compression, geometric transformation and some other modifications of the container image.

There are a number of modifications to this method that increase resistance to manual and automated embedded data detection. An improvement in the approach is that the message is not embedded directly in the pixels, but as the sum of the values ​​for a group of pixels. The method of dividing the image into groups is set in advance and is known only to the sender and recipient. An image can be divided into simple groups of pixels, such as rows, columns or squares, or into more complex ones, such as crosses or intersecting matrices. In this case, the sender changes each group of pixels in such a way that the sum of all pixels of the group is equal to the next part of the embedded message. The recipient, in turn, splits images into the same groups, calculates their sums and composes the message from them. Another feature is that if one embeds the same message in the same container, the filled containers will be different, even if one uses the same method of splitting into groups of pixels.

To embed messages in audio files or audio streams the following methods are usually used: echo methods, phase coding, and spread-spectrum method. Echo methods are based on the fact that the sound wave is superimposed on itself with a small shift in time and reduced amplitude. On the one hand, this echo is quite easy to detect; on the other hand, the common digital audio signal always contains an echo that appears for natural reasons at the time of recording through the microphone.

In this case, an echo is characterized by an initial amplitude, an impedance degree, and a delay, which are selected in such a way that the echo remains undetectable to human perception. Using several different delay values, the message is embedded in the audio signal. For example, an echo with a delay of 0.001 s denotes a zero bit of a message and an echo with a delay of 0.002 s denotes a one bit.


### Hiding messages with hash steganography

This steganography method is special because it does not require changing the container, and the procedure for embedding and extracting a message is quite different from the methods discussed above. Its operating principle is that a sender selects a container (a picture, an audio file, a text file, or some other) the hash value of which matches the message to be transmitted. Of course, the sender and the recipient must agree in advance on the data transfer/communication channel and the hash function used. To extract the message, the recipient simply needs to calculate the hash value of the container.

The longer the hash value of the hash function, the more difficult it is to select a container that will match the desired message when hashed. For example, for the output value of a hash function with the length of 8 bits there are 256 possible values, and for a value with the length of 16 bits there are 65536. In addition, usually the message’s length is much longer than the length of the hash value. Therefore, hash steganography involves using a group of ordered containers to transmit a single message. Furthermore, the hash function can be used even with a long output value, but only a part of it will be used.

Let’s examine the procedure for embedding and extracting a hidden message with hash steganography using an example. Suppose Alice and Bob agree that they will use hash steganography with 128-bit SHA-1 hash function and use the first byte of the output value; photos will be used as containers, and Alice’s personal photo album on a social network will serve as a communication/data transfer channel. Now if Alice needs to send Bob a message of 44 bytes long, from the whole set of photos that she has not yet published, she selects the one with the first byte of the hash value that matches her message and publishes it. In the same way, Alice selects a photo for the second byte of the message and so on, until all 44 photos are published in the correct order (Fig. 4.16). Obviously, extracting the message is a much simpler process for Bob. To receive the message, he simply needs to calculate hash values ​​of Alice’s photos in the order they were published.

![Figure 4.16 – Operational scheme of hash steganography](/resources/img/volume-3/4.4-Steganographic-methods-of-information-hiding/F-4.16-operation-of-hash-steganography.png "Figure 4.16 – Operational scheme of hash steganography")

However, Alice’s task can be slightly optimized. Instead of searching through photos with brute force, she can go through the entire set of unpublished photos only once and classify them according to the first byte of the hash value. In other words, Alice creates a table where each row contains photos. The first byte of their respective hash values equals the row number (Fig. 4.17). Now, to transmit each next byte of a hidden message it is enough to publish a photo from the appropriate table row. In order to avoid repetition of photos in the album and avoid unwanted attention, Alice removes the photo from the table right after publishing it. This means that when actively transmitting messages hidden in this manner, Alice needs to regularly refill her table with new photos.

![Figure 4.17 – Table of photos prepared by Alice](/resources/img/volume-3/4.4-Steganographic-methods-of-information-hiding/F-4.17-prepared-table-of-photos.png "Figure 4.17 – Table of photos prepared by Alice")

The advantage of hash steganography is a very high degree of hiding, since the containers do not require modification and can be arbitrary files. This allows selecting virtually any container depending on the particular situation. The disadvantage of this method is the low capacity of the container: a single container can actually contain no more than a few bytes of a hidden message.


### Application peculiarities and possible attacks

To protect the copyright of digital content, _digital fingerprint_ and _digital watermarking_ systems are often used. These methods use steganography to seamlessly embed author identifiers, digital signatures or other marks into digital media.

There are also suggestions that governmental agents and members of terrorist groups use steganography to communicate on the Internet. This may be due to the fact that when such containers as images are published on open resources, it does not raise suspicion compared to the use of secure messengers or dark networks.

By attacks on steganographic systems we usually mean attempts to detect, extract, or substitute the embedded message. The corresponding activity is called _steganalysis_. Attacks on stegosystems may be based on a known embedding technique, a known embedded message, or a known empty container.

### \*\*\*Frequently asked questions\*\*\*

_– What images are the most suitable for embedding messages?_

For LSB-based embedding methods, monotonous, low-contrasting images with repeating or uniform textures (images of sand, sky, grass, trees, sea, and so on) are way better than high-contrasting ones (images with text, diagrams, and so on). In addition, one should keep in mind that the human eye is most sensitive to shifts in the green color spectrum. By contrast, its least sensitivity is observed regarding the blue color spectrum. Thus, one can embed more data into the blue channel (according to the RGB color model) of an image than into its green channel, and the filled container will remain visually indistinguishable from the original image.

# [5 ROLE OF CRYPTOGRAPHIC COMMITMENTS IN ACCOUNTING SYSTEMS](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/5-Role-of-cryptographic-commitments-in-accounting-systems.md)
