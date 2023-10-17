# [ABOUT DISTRIBUTED LAB](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/0.2-About-Distributed-Lab.md)

# 1 APPLYING DECENTRALIZED APPROACHES TO DIFFERENT SYSTEM DESIGNS

Over time, the decentralization principles are increasingly used in the designing of information systems for various purposes. Decentralized technologies are used in the building of network protocols and overlay network protocols, in data exchange and messaging systems, in search and financial systems, etc.

How does decentralization affect architectural changes in user-familiar systems? What are the difficulties of the designing of mesh networks and why are they better than the traditional Internet? Is authentication necessary for a user and how to distribute the identification process between independent parties? What processes can be distributed in an auctions system? Is there any sense in the decentralized exchanges and what protocols for their implementation exist? We give answers to these questions in this section of the book.


## 1.1 Operational principles and development of mesh networks

A mesh network is the physical topology of the data transmission network, which implies that each workstation (a computer, a smartphone, a TV set, etc.) has direct connections with other workstations and acts as an independent switch taking part in transmitting their traffic.

> **_Distinctive features of mesh networks_**  
> * _Increased fault tolerance level_
> * _Blocking- and filtering-proof (i.e. impossible to block and/or filter)_
> * _No subscription fees_
> * _Permissionless (i.e. no permission required to join)_

The mesh network topology implies that a node (a user device) has several physical connections with other nodes, that is, it is connected in a wired or wireless way directly, without intermediaries. It is due to such redundancy and low probability of simultaneous disruption of several connections that the fault tolerance level of the entire network increases [1].

A mesh network node receives and sends data through its “neighbors” instead of connecting to a centralized provider (Fig. 1.1). Thus, part of the node’s traffic passes through one of its neighbors, but instead of demanding payment for a service, the neighbor forwards a part of its traffic through this node. In practice, anyone can use the services of such a network without a subscription fee by paying only for the purchase of a network device and for the maintenance of its operation.

![Figure 1.1 – Data transmission between mesh network participants](/resources/img/volume-3/1.1-Operation-principles-and-development-of-mesh-networks/F-1.1-data-transmission.png "Figure 1.1 – Data transmission between mesh network participants")

Due to the fact that the entire network is supported exclusively by independent individuals, blocking specific nodes or filtering traffic are quite complex processes. Here, the question may arise: will the network organized in this way within a residential area or an entire city be legal? Of course, it all depends on the situation and the legislation in force in a particular territory, but in most cases, the answer is quite simple: as long as the participants in such a network, while placing and deploying their equipment, do not violate private and public property rights as well as the restrictions on frequency and power of electromagnetic radiation, they are unlikely to have legal issues.

In special-purpose networks, for example, between military or space objects, node addresses are assigned in a centralized way. In order to join the network, one needs to go through a special registration procedure and get permission. By contrast, protocols of general mesh networks generally have the permissionlessness feature. In such networks, addresses are either assigned automatically, or a node generates its address by itself. In order to become an equal node in a mesh network, no permissions are required.


### Global goal of mesh networks

There are several protocols for designing mesh networks, each of which is aimed at providing specific properties and functions in solving local communication problems that the traditional topology and centralized providers cannot cope with.

However, the global idea of introducing mesh networks is to create an alternative and self-sufficient Internet where every data packet, when transmitted from a sender’s node to a recipient’s node, is cryptographically protected; every participant acts as an ISP (Internet service provider); getting an IP address is as simple as generating a random number; many alternative routes between any two nodes exist; and words such as “denial of service” and “the network is unavailable” are even unfamiliar to users. Thus, for users, the mesh network version of the Internet will be much more private and protected from filtering of transmitted content [2].

The conceptual difficulty of creating a global mesh network lies in the fact that in order to launch it, one needs to cover the major part of the planet with devices that are ready to work under a single protocol at any time. As long as the critical mass of such devices has not been reached, the comparative benefit of the mesh network is extremely small. As a result, in order to create an alternative global network based on the mesh network principle, ordinary people first need to believe in this idea, start supporting such networks on their devices, and wait for the network’s expansion. When the critical mass of independent users is reached and both local and global coverage is stable, then such a network can be considered an alternative Internet and centralized ISPs can be abandoned.

The schematic difference between the traditional Internet and the alternative one can be seen in Fig. 1.2. In the traditional Internet, there are network connection providers through which the traffic of local user groups passes. In addition, there are companies that provide communication channels and packet routing between local groups and continents. This centralization of traffic control leads to two main critical flaws of the traditional Internet, the first being the ability to block specific users or specific content and the second being the potential  denial of service.

![Figure 1.2 – Connection of users in the traditional Internet and in the alternative Internet](/resources/img/volume-3/1.1-Operation-principles-and-development-of-mesh-networks/F-1.2-connection-of-users.png "Figure 1.2 – Connection of users in the traditional Internet and in the alternative Internet")

In practice, there are cities and even countries that are connected to the rest of the network with only one physical channel. There are historic cases when such a communication channel failed to regularly operate for a day or even longer. As a result, the work of many business processes and the normal lives of people were interrupted leading to massively negative consequences, including public disturbance.

In turn, the alternative Internet assumes that the entire communication channel is maintained by the users themselves. In this case, the level of decentralization of traffic control is maximum, and the probabilities of blocking and denial of service are minimal (close to non-existent levels).

> **_Popular protocols for designing mesh networks_**  
> * _DTN_
> * _NETSUKUKU_
> * _B.A.T.M.A.N._
> * _OSPF_
> * _CJDNS_
> * _Yggdrasil_

DTN (Delay-Tolerant Networking) is one of the earliest alternatives to traditional data transmission networks. Initially, its task was to ensure reliable data delivery in a network with a long signal propagation time, long waiting in a queue for sending or a temporary connection disruption between nodes. The development of DTN began in 1970s, when laptop computers began to appear and researchers wondered about the reliability of the networks in which its participants do not have permanent connection [3].

The nodes of such a network had to store the received data packets until they were transmitted to the next node. As compared to traditional networks, the nodes of such a network provide much longer storage of packets and can write them to disk to wait in the queue for sending. This technology has the most potential for application in space communications. Therefore, the delays in DTN are understood not only as delays caused by transit nodes or capacity limitations of a communication channel, but also as additional delays in signal transmission not  depending on the amount of transmitted data (Fig. 1.3). Such delays may depend on signal propagation speed in the transmission medium, signal coverage distance, and communication channel quality (if the channel is poor, there will be data losses, which will require additional time to retransmit the data).

![Figure 1.3 – Example of data saving by DTN nodes](/resources/img/volume-3/1.1-Operation-principles-and-development-of-mesh-networks/F-1.3-data-saving-by-DTN-nodes.png "Figure 1.3 – Example of data saving by DTN nodes")

In DTN networks, packets that have already passed part of their route are saved not only in spite of long delays but also in spite of disruptions of data transmission channels due to their storage by intermediate nodes. Such a disruption can occur due to the range limits of wireless communications, sparseness of mobile nodes, lack of energy resources, or interference. When a disrupted channel between nodes is restored, the saved packets simply continue on their way instead of being  resent.

The CJDNS protocol is a more recent project, its first implementations appeared in 2012 [4]. CJDNS is supported by the open community and primarily serves ordinary users’ needs. As compared to other protocols (Table 1.1) [5], it implements encryption of data packets between sender and recipient as well as data encryption during transmission between all transit nodes. In addition, it is possible to combine several CJDNS-based private mesh networks into one large mesh network as well as to connect nodes via the traditional Internet.

The CJDNS protocol assumes that each node generates its own cryptographic keys for asymmetric encryption and its network address which is associated with the public key and is in the range of private IPv6 addresses. For a physical connection with the “neighbors”, Wi-Fi (in both access point mode and client mode) or Ethernet can be used. The number of nodes in such a network can reach tens of billions, so the routing table is organized according to DHT (distributed hash table) principles. Thus, the network node stores only a part of the table and can effectively build a route for any packet by communicating with neighboring nodes.

Another interesting fact is that in 2017 the CJDNS community split and some developers began work on a similar project – Yggdrasil [6]. The goals of the project and the principles for reaching them are the same, but the Yggdrasil developers offer a more decentralized way of packet routing, which allows to completely avoid centralized directory nodes.

Unlike most other protocols, B.A.T.M.A.N. (Better Approach To Mobile Adhoc Networking) uses MAC addresses of node network interfaces as host identifiers. In a network operating on this protocol, each node stores two routing tables: a local one and a global one. The local table stores information about nodes with which there is a direct physical connection, whereas the global table contains the most relevant information obtained from local tables of the neighbor nodes. During its operation, each node regularly sends broadcast messages with data about its address and its local routing table. Noteworthy, B.A.T.M.A.N. protocol is supported by the open community; at the time of 2019 it is actively developed and has integration into the Linux kernel.

The development of Netsukuku protocol has already ceased, but it also has interesting features. For example, all nodes of a network working on this protocol are treated hierarchically. Each 256 nodes form a so-called group node (gnode), and 256 group nodes, in turn, make up a higher-level group (ggnode) and so on. Netsukuku protocol logically treats any group node as a physical one, so routing can work the same way at all hierarchy levels. The advantage of this approach is that with each route search, at most 256 nodes must be operated/interacted with; this greatly simplifies the process. To exchange data on the network state, the nodes use a TP package (trace path package), where there is no specified recipient, but there is data about all intermediaries which it passed through en route to the sender. According to the protocol, one of the nodes acts as the initiator and sends such a packet to all its neighbors; the neighbors append the data about themselves to the received packet and transmit the appended packet further, but no one transmits the same packet twice. As a result, each node that receives a TP package receives the full route to the sender node as well as to each of the intermediary nodes.

The OSPF (Open Shortest Path First) protocol also uses dynamic routing, but it is based on link-state technology. According to this technology, each router seeks not only to know the best routes to each node, but also to have a complete network map with all existing connections between other routers. To find the shortest path, the OSPF protocol uses the Dijkstra algorithm. Table 1.1 provides a comparison of the mentioned protocols for mesh network design [5].

![Table 1.1 – Comparative characteristics of the existing protocols for mesh network design](/resources/img/volume-3/1.1-Operation-principles-and-development-of-mesh-networks/T-1.1-en-characteristics-of-existing-protocols.png "Table 1.1 – Comparative characteristics of the existing protocols for mesh network design")


### Applying mesh networks in practice

The most common and popular version of mesh networks uses wireless technology. In this case, workstations can change their location relative to other workstations and create new physical connections on the fly. Meanwhile, wired connections do not allow one workstation to easily connect to several others and reconnect if necessary.

The need to build mesh networks originally arose in the military, where there is often a need for autonomous communication in the field. Therefore, self-organizing networks have become a staple solution for creating large-area zones with continuous and reliable coverage. Such a network is easily scalable when adding new nodes and is fault-tolerant in case of loss of particular network elements.

Due to the many positive properties, over the time mesh networks have spread to other application areas as well.

> **_Areas of mesh networks application_**  
> * _Military (connection between computers in field operations)_
> * _Satellite communication (the 66 satellites of the Iridium constellation work as a single mesh network)_ [7]
> * _Communication in large corporate environments_
> * _Communication in areas with undeveloped infrastructure_

There are still cities and regions in the world which are not covered by the centralized network connection providers and where local people decide to provide themselves with a network connection by joining together in a mesh network. A good example is the Guifi project. It originated in the early 2000s in Catalonia and Valencia regions of Spain when local residents got tired of waiting for a normal ISP to emerge. Currently, it is one of the largest mesh networks in the world: it consists of more than 35 thousand active nodes. The network is being developed on a voluntary basis and connecting to Guifi is completely free of charge [8].

The Guifi network has so-called “islands” – groups of nodes located close to each other and having a high degree of connectivity (Fig. 1.4). Each “island” is a network that interconnects users of a district, municipality, or city. To connect clients Wi-Fi routers with special firmware are used. Communication between these “islands” can be provided, in particular, through the traditional Internet (VPNs or proxy servers). In addition, nodes connected to the traditional Internet give other Guifi users access to it.

![Figure 1.4 – Map of Guifi mesh network in cities of Spain](/resources/img/volume-3/1.1-Operation-principles-and-development-of-mesh-networks/F-1.4-map-of-mesh-network.png "Figure 1.4 – Map of Guifi mesh network in cities of Spain")

The capacity of each specific node in each case is different: for some nodes, it does not even exceed 1 Mbit/s, but in many areas (especially in the mountains), this is still the best (and sometimes the only) way to connect to the Internet.


### Features of packet routing on a network

The whole set of routing algorithms can be divided into two classes: reactive and proactive. Reactive routing algorithms lay the route for a data packet from a sender node to a receiver node only when necessary; this creates a certain delay in the transmission of the first packet. The basic advantage of this class of algorithms is the absence of background network load.

The proactive (table-driven) approach implements tables of actual routes  for constant background exchange of data on network topology changes between nodes as well as for periodic distribution of special service packets. Since the proactive algorithm always has a ready route to any potentially reachable node, additional delays when sending data packets are excluded. It should be noted though that this advantage is achieved due to background traffic, which leads to a decrease in the total network performance as compared to using reactive algorithms.

The efficiency of both proactive and reactive algorithms decreases sharply with an increase in the frequency of network topology changes. The decrease in the efficiency of reactive algorithms is explained by the fact that cached packet transport routes quickly become obsolete due to destruction of the connections that make them up. In the marginal case, application of a reactive algorithm becomes impossible due to the fact that packet delivery routes will become obsolete during their construction.

The use of proactive algorithms in a network with frequent topology changes will lead to a sharp increase in the volume of service traffic to maintain the relevance of routing tables; in the marginal case, this leads to situations when all of the network capacity is used only for service data transmission. Therefore, neither proactive nor reactive algorithms in their pure form can be used to organize the process of routing data packets on networks with a frequently changing topology. They try to build the full path from a sender node to a receiver node, but on a large network with constant changes long routes have an extremely short lifetime. This effectively means that when constructing efficient routing algorithms the local properties of a network must be taken into account. The local approach provides the following two advantages: resources saving when responding to a network topology change and simplicity of obtaining the current state of neighbor nodes as compared to obtaining the state of other network nodes.

Another important principle for constructing an effective routing algorithm for networks with a frequently changing topology is to minimize the response to topology changes. This can be achieved by sending service messages only to a small number of nodes located as close as possible to the point where the topology changed. Changing the whole network routing nature is not required. Thus, each transmitted data packet has the full route, but it is not required to be fully complied with. Therefore, when communication disruptions occur or an intermediary node disconnects, a local group of the nearest nodes solves the issue.


### Disadvantages of mesh networks

> * _The initial launch of a mesh network is relatively complex_
> * _A network works effectively only with a big number of nodes_
> * _Unpredictable network capacity and packet delivery delays_
> * _Problems of building optimal routes_
> * _Mesh networks are ineffective for accessing centralized services located in the global network_

A mesh network only works efficiently with good connection quality between nodes. Every node must have a physical connection with several other nodes, and there must be a sufficient number of connections between the “islands” of the network. Therefore, for the initial launch of the network, it is necessary to first reach the critical mass of ready-configured devices.

In dynamic mesh networks (where nodes move unpredictably in space and can disconnect at any moment) it is impossible to predict the capacity and the time of packet transmission between two network nodes. This means that when downloading or uploading a file, a user cannot get an estimate of the remaining time, and during an audio call or video streaming pauses may occur even if adaptive media streaming protocols are used, since they cannot select the optimal parameters for the connection the characteristics of which are impossible to predict.

The routing problem also remains relevant. There are several approaches to building routes in dynamic networks, but every approach has its own drawbacks: high memory consumption, long operation time, dependence on a single directory node, choosing the optimal route using only one criterion, etc. Since there can be no ideal solution, researchers are still looking for the optimal balance between the existing advantages and weaknesses.

It also should be understood that a mesh network is most effective for independent intercommunication of private nodes. For example, a mesh network is well suited for designing a corporate IP telephony system. However, if every member of some user group connects only to “facebook.com”, there is little sense in such a join.


### \*\*\*Common myths\*\*\*

_A mesh network has no access to the Internet._

Sometimes it doesn’t have the access, because some protocols for organizing mesh networks do not support the ability to broadcast packages to a global network and vice versa. However, this does not mean that such mesh networks cannot have access to the Internet. To connect to the global network, what is used are special proxy or VPN servers, which are supported by the mesh networks and which allow using the Internet within mesh networks.


### \*\*\*Frequently asked questions\*\*\*

_– How does one merge two mesh networks?_

There may be several fundamentally different scenarios. If the networks use a single protocol that initially supports network merging, then one common node is enough. If the networks use a single protocol that does _not_ support network merging, then it is necessary to manually configure at least one common router and translator of network addresses. If networks use _different_ protocols, then network merging may not be possible at all due to the use of different address formats, data packet formats, end-to-end encryption, and other interaction features.

_– Can mesh networks and related technologies be banned by regulators?_

Restricting the use of technology itself is highly improper because it can be used for quite a number of very different purposes. Some may use mesh networks to transmit forbidden content or to hide criminal activity traces, while others can use them to achieve noble goals. For example, it is convenient for ISPs and mobile operators to use mesh networks to simplify the setup of their equipment through self-organization. It is convenient for network administrators to use mesh networks to create a wireless network with a large coverage area (shopping centers, office buildings, etc.). In the scope of the Internet of things, the use of mesh networks is also very important for detecting nearby devices and interacting with them. 


## 1.2 Decentralized digital identity systems

For each of us, a set of certain attributes (data) that uniquely distinguishes one’s identity from the others can be gathered. These can be both physical characteristics (e.g. fingerprints, retinal pattern) and their digital analogs (e.g. e-mail address, the digital signature public key) (Fig. 1.5). All of this is identity data, or personally identifiable information (PII) [9].

![Figure 1.5 – User identity data](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.5-user-identity-data.png "Figure 1.5 – User identity data")

When a user wants to use a service for the first time (for example, open a bank account, create an e-mail account, get a biometric passport, etc.) either in digital or physical form, that service will most likely ask him to provide a set of his identification data. Based on that data, the service will create an account in its local database where it will record the correspondence/interaction between the user identification data and the _local identifier_. This is how a person will obtain a _digital identity_ [10].

The process of collecting unique data – in this case, personal data – is called _identification_. The identification procedure in most cases is necessary in order for a service to understand with whom it interacts, since a set of permissions or rights that are available only to a specific user is often bound to an account that is assigned to that specific user [11].

> _Note. Hereinafter, the services that provide identification services will be defined as “identity providers”._

Having passed the identification procedure, the user is issued a unique identifier in the system (Fig. 1.6). During further interaction with that system, the user will not need to re-provide his identification data since he will be “recognized” by the previously issued local identifier, which may be an account number in a specific bank, an e-mail address that was used to register in the system, or a biometric passport. The procedure for verifying whether the user is who he claims to be is called _authentication_ [12].  

![Figure 1.6 – User identification procedure implemented by different service providers](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.6-user-identification-procedure.png "Figure 1.6 – User identification procedure implemented by different service providers")

At this point, it is important to highlight one of the limitations of this approach: the databases of identity providers are _not_ connected and are _not_ synchronized with each other. As a result, a particular unique identifier can only be used in the system that issued that identifier. This leads to a situation where one physical identity corresponds to many digital identities (Figure 1.7).

![Figure 1.7 – User identifiers in different accounting systems](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.7-user-identifiers.png "Figure 1.7 – User identifiers in different accounting systems")

Although this paradigm is ubiquitously applied at present, it entails particular inconvenience to both users and providers.

> **_Disadvantages of existing identity systems_**  
> * _Irrational resource usage_
> * _Non-compliance with personal data processing policies and regulations_
> * _Risk of personal data theft/loss potentially resulting in identity theft_
> * _Non-compliance with password policy_

The first drawback is that the user is forced to spend time registering (signing up) for each new service, filling out identical (or almost identical) registration forms, creating new logins and passwords, passing the “captcha”, confirming registration by e-mail, etc. This process is generally quite frustrating and inconvenient, especially if in the end one needs to use the service only once (for example, to format an image, process a file or upload a song) or it turns out that this is not exactly the service that the user needs.

As we mentioned earlier, the identification information includes the user’s personal data, which must be collected, processed and stored in compliance with personal data regulations, such as GDPR for EU countries  [13] or other similar regulatory documents  regulating personal data collection and processing in other jurisdictions. Most recent of these regulations introduce the principle of _direct personal data control_: the owner (the personal data subject) has to have control of her personal data. However, the situation is completely different in practice: most users do not even imagine what happens to their personal data after they have agreed with the data processing terms (Fig. 1.8). Further to this, most users do not review the personal data processing principles established by online services.

![Figure 1.8 – Example of a personal data processing “agreement”](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.8-personal-data-processing-agreement.png "Figure 1.8 – Example of a personal data processing “agreement”")

Most often, during the identification procedure, PII is transmitted as plain text. They are processed in the same form by the relevant identity provider. This entails an increased probability of personal data interception by an adversary.

In addition, most users have a large number of identifiers and corresponding passwords required to authenticate in various systems. Imagine that you are standing in front of a closed door with a bunch of keys and cannot remember which key suits that door (Fig. 1.9). Users try to simplify their lives and more often than not use the same password to access various services. In this scenario, compromising the password to one of the services may potentially result in unauthorized access to many independent systems by malicious third parties.

One should also consider the fact that the password policy for each provider is different and in addition to the requirement for the user to keep the safety of her passwords, she must update (change) them on a regular basis [14].

![Figure 1.9 – Traditional process of gaining access to a system](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.9-traditional-access-gaining.png "Figure 1.9 – Traditional process of gaining access to a system")

A logical solution may be to reuse previously collected identification data. For example, a user who already has a Google account wants to use the service draw.io to create a diagram. However, instead of filling the registration form and going through the identification procedure again, he may ask Google to provide the necessary information to the service. Below we discuss how this interaction can be made possible.


### Design and operational principles of the OAuth protocol

The _OAuth protocol_ [15] provides a solution that allows third-party applications to gain limited, controlled access to controlled resources (for example, personal user data).

In the second-generation of the OAuth protocol, four system roles can be distinguished:

> * _Resource owner_
> * _Resource server_
> * _Client_
> * _Authorization server_

A _resource owner_ is actually the user herself. She can provide the permission for other parties (_clients_) to gain access to the resource.

A _client_ is an application/service that can generate requests on behalf of a resource owner to receive data which belong to the resource owner.

A _resource server_ is a server on which a protected resource is stored (for example, a Google server that stores user data).

An _authorization server_ is a server that issues access tokens for a client after his successful authentication (confirmation of rights to access a resource).

The basic idea of OAuth protocol is to use a special-purpose token instead of user credentials (username and password) to access a protected resource. The token here is a digital string that contains access attributes. With these attributes, it is easy to determine which information was granted access to, for how long, and to whom exactly. 

Access tokens are issued to third-party clients by the authorization server with the approval of the resource owner. The client uses the token to gain access to protected resources hosted on the resource server side (Fig. 1.10).

> _Note. The interaction between the authorization server and the resource server is beyond the scope of this protocol. The authorization server can be the same server as the resource server or a separate object. One authorization server can issue access tokens that are accepted by several resource servers._

![Figure 1.10 – Overview of OAuth protocol](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.10-OAuth-protocol.png "Figure 1.10 – Overview of OAuth protocol")

According to the OAuth 2.0 protocol, we assume the following interaction order:

1. The client generates an authorization request and sends it to the owner. An authorization request can be made directly to the resource owner (as shown in Fig. 1.10) or indirectly through the authorization server.
2. The resource owner transmits an authorization grant to the client, which contains confirmation that the client actually has the permission to gain access to the resource.
3. The client requests an access token on the authorization server. To confirm that the client acts on behalf of the resource owner, the authorization grant obtained in the previous step is included in the request. Here, the authorization grant confirms the successful authentication of the user on the side of the authorization server. Noteworthy, the authorization server does not know at this step which application (client) they are issued to, but it knows exactly that the client received the necessary permission from the owner.
4. The authorization server checks the client’s permissions (i.e., understands that it is the resource owner who grants the permission to this client) and the authorization grant. If the authorization grant is valid, the server issues an access token.
5. After authentication, the client requests a protected resource from the resource server by using the access token. 
6. The resource server checks the access token and, if it is valid, processes the request. At this step, the resource server knows exactly to which application the access token is issued and is sure that this happens with the consent of the data owner.

Thus, it is enough for the user to submit the set of personal data to one service only once, after which it will be possible to issue access tokens to different parts of that set to different clients. For example, using the exact same principle as described above, the user can allow an application on his smartphone to save photos to Google Drive automatically.


### OpenID and OpenID Connect protocols

OpenID utilizes the concept of creating a single accounting system for various Internet resources, which would save users from having to constantly fill out registration forms (pass the identification procedure) and, accordingly, accumulate new login–password pairs.

The idea is to store a single account on one service (one of the OpenID providers) and use it to register for the others that support the OpenID protocol.

> _Note. The list of OpenID providers includes Google, Microsoft, Symantec, Verizon, Oracle, and VMware._

The protocol involves the following three parties:

* A user who wants to use his OpenID identifier
* An Internet service that a user wants to access
* An OpenID provider who has previously performed user authentication procedure

The first and second generations of OpenID protocols [16] were aimed exclusively at authenticating the user on the side of one of the OpenID providers. The authentication result was transmitted directly to the relying party – the service which the user wanted to interact with (Fig. 1.11).

![Figure 1.11 – Operation of OpenID protocol](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.11-OpenID-protocol.png "Figure 1.11 – Operation of OpenID protocol")

The procedure for establishing a communication channel between an Internet service and an OpenID provider (shown in step 3) is carried out according to the internal check_Id algorithm, which is a two-factor authentication protocol that allows the interacting parties to verify the authenticity of each other. Optionally, it is possible to establish a common secret between an Internet service and a provider using the Diffie–Hellman protocol (in this case, by using a MAC code, the Internet service can authenticate the provider without additional requests for information about its certificate).

The result of the protocol operation is that the service makes sure that the user is who she claims to be because one of the trusted identity providers confirmed this. At the same time, no other information about the user is disclosed. Note that neither OpenID 1.0 nor OpenID 2.0 were compatible with the OAuth protocol.

The third-generation protocol, called OpenID Connect [17], is an add-on to the OAuth 2.0 authorization protocol. OpenID Connect allows internet services to verify a user’s identity based on the authentication performed by the authorization server (Fig. 1.12).

Let us examine the following roles in the OpenID Connect protocol:

* End user is an object that gains access to a client’s resource
* Relying party (client, RP) is a service that, according to OAuth 2.0 protocol, requires the end user’s authentication and the access to some of the information about him from an OpenID provider
* OpenID provider (OP) is an authorization server that can authenticate an end user and provide the relying party with information about the authentication event and the end user

![Figure 1.12 – Operation of OpenID Connect protocol](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.12-OpenID-Connect-protocol.png "Figure 1.12 – Operation of OpenID Connect protocol")

Generally, interaction within the framework of the OpenID Connect protocol occurs as follows:

1. The client sends an authentication request to an OpenID provider.
2. The user passes the authentication procedure on the side of the OpenID provider, after which the user tells which client must be provided access to personal information.
3. Two tokens (an ID token and an access token) are issued, which are included in AuthResponse. An ID token carries information about the authentication event – a set of information about which user initiated the issuance of tokens for which client side on the server. An access token is exactly the same token as described above in the OAuth protocol.
4. Using the received access token, the relying party sends a request for information about the user, which is stored on the OpenID provider’s side.
5. The OpenID provider checks the access token and, if it is valid, processes the request.


### Limitations of discussed protocols

The proposed protocols, of course, simplify the interaction of users and identity providers but several key issues remain:

> * _Lack of direct control of a user’s personal data by a user_
> * _The problem of trust to centralized providers_
> * _Vulnerabilities related to login–password authentication_
> * _Risks of using sessions_
> * _Lack of identification events synchronization_

Both approaches described above involve storing sensitive user information (including PII) on one resource. Accordingly, ensuring security completely depends on the security policies of a given resource, and the resource itself actually owns this data. As a result, the user still cannot fully control her personal data.

The second limitation is the need to trust centralized identity providers. The client receives information from a particular provider and considers it correct. Actually, there is no objectivity in the process of resolving the problem of trusting a particular identity.

The login–password authentication method is not reliable enough. Password compromise (users do not always use sufficiently strong passwords) leads to the compromise of _all_ personal data. Moreover, as most users do not like to invent new passwords or fail to replace them on a regular basis, compromising one password opens a large number of  “doors”.

OAuth protocol vulnerabilities are also related to the session nature of the protocol. If an attacker manages to intercept an Authorization Grant during such a session, he will be able to implement a man-in-the-middle attack.

A user can have several OpenID identifiers that will not be associated with each other. Thus, the problem of the existence of multiple digital identities has yet to be solved.


### Principles of building the global identity system

Below we discuss a theoretical model of a global decentralized identification system. The basic principles of such a system would be as follows:

> * _The user directly controls her own identification data_
> * _A user’s identity contains a digital digest of personal data to ensure the verification of their integrity_
> * _Linking identification data with keys that belong to the user_
> * _Distributed storage of identification data by providers who verified this data_
> * _Identification event synchronization between third-party providers_

The basic role of the global digital identity system is to connect the global user identifier with his public key and the personal data that belong to this user through checking this connection by several identity providers (Fig. 1.13).

![Figure 1.13 – Concept of the global digital identity system](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.13-global-digital-identity-system.png "Figure 1.13 – Concept of the global digital identity system")

The most secure authentication and authorization approach is that for each accounting system a user interacts with the right to perform operations authorized for the identifier (and to access authorized data) is verified by using the ownership of the public key which the identifier is bound to. Public key ownership is verified by calculating and further verifying the digital signature.

The accounting system that a user currently wants to interact with, when receiving a request from him, can address the global digital identification system and retrieve the current public key corresponding to a specific identifier, and then verify the request signature (Fig. 1.14).

![Figure 1.14 – Verification of the link of identification data with a public key when the request is received](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.14-verification-of-link-of-identification-data.png "Figure 1.14 – Verification of the link of identification data with a public key when the request is received")

Storing sensitive information in plaintext requires expensive protection methods, but rather than storing information itself it is possible to store its digital digest, by which it is easy to determine that the user actually has access to and control over her personal data. 

A user must be able to store different sets of identification data in different accounting systems and, if necessary, provide the information requested by a third party without creating a new set.

Let us imagine a user who has accounts in several accounting systems that store the sets of personal data they need (Fig. 1.15).

![Figure 1.15 – Data stored by different identity providers](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.15-data-stored-by-different-identity-providers.png "Figure 1.15 – Data stored by different identity providers")

For example, a user wants to get an electronic insurance policy for a new car. For this, the insurance company requires the following dataset: the email address to which the policy must be sent, passport data, confirmation of availability of sufficient funds for making monthly payments, the driver’s license data. 

Currently, even using the protocols described above, there is no user-friendly way to remotely provide the insurance company with the necessary dataset. Even if the insurance company accepts OpenID identifiers, after passing the authentication procedure the user will need to “manually” provide the required set of documents. The OAuth protocol also does not provide a significant advantage, since in order to use it the user will need to authorize all of his client applications (if any) in the accounting system of the insurance company.

In the concept of a global identification system, the user will essentially need to make and issue to the insurance company a set of permissions indicating in which of the existing accounting systems the parts of the required dataset are stored (Fig. 1.16). Such a set of permissions can be signed with the user’s private key so that each of the identity providers can reliably verify that the request is executed with the permission of the data owner (user).

![Figure 1.16 – Retrieving information from different identity providers](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.16-retrieving-information.png "Figure 1.16 – Retrieving information from different identity providers")

At the same time, requests from the insurance company are sent only to those accounting systems that the user specified. Thus, the user did not have to make and provide the necessary set of information by himself, nor did he have to create an extra account in the new accounting system.


### Extending capabilities of the global identity system with blockchain technology

Networking between identity providers using blockchain technology can create a distributed database with timestamps for each transaction made. Transactions in such a system may be events, where each event is the moment the user passes the identification procedure at one of the identity providers. This makes transactions easily traceable, irreversible, and also prevents fraud, abuse and other types of manipulation.

After performing the user identification procedure, the provider adds the corresponding record to the blockchain. If the user needs to access another service, the user connects to it using his own identifier. 

The system, in turn, requests information about which of the other providers has already identified the user. The owner of each system independently determines how much he trusts the identification events (and, accordingly, the results of these events) issued by another system. If the level of trust is maximum, then the system provides the user with a service based on her identifier. If the trust of the system (or set of systems) is lower, then the client can request the user’s data and make sure that they correspond to those stored in the distributed ledger.

Noteworthy, personal data are accessed only after the user’s approval. If the data provided are sufficient, the system gives access to the user; if not (for example, additional data are necessary), the system identifies the individual user separately with the corresponding record in the ledger.

Below is a diagram showing how an identification procedure (also known as “know your customer,” or KYC) can be organized in such a system (Fig. 1.17).

![Figure 1.17 – Passing identification procedure with user adding the corresponding event to the system](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.17-passing-identification-procedure.png "Figure 1.17 – Passing identification procedure with user adding the corresponding event to the system")

1. Alice sends her public key and the data necessary for passing KYC procedure to the bank, which is the initial identity provider.
2. The bank places the received personal user data in a reliable KYC storage in compliance with the GDPR requirements. Note that KYC data must be encrypted by the bank, and only the bank can directly access the data in the storage. In order for a third-party organization to obtain KYC data, it must contact the bank, which, before providing this data, will request user approval.
3. The KYC provider gains access to the user data and performs the KYC procedure (this provider can be both the bank itself and a party that the bank trusts to perform this procedure).
4. After that, the KYC provider transfers the results of the KYC procedure to the bank. If the procedure fails, the bank informs the user about it, whereas the user adds the missing data and repeats step 1.
5. If the KYC procedure was successfully completed, the bank creates a transaction that contains the user’s public key (which is signed by the bank key) and the hash value of all user data (as the proof of storing this data). The bank also adds to the transaction the information that the bank confirms this data. Node validators process the transaction and add it to the blockchain.
6. The bank returns Alice a signed transaction. Alice can verify that the corresponding record has been added to the blockchain and that the data in the transaction matches the data she provided.
7. If Bob wants to change his public key, he informs his identity authority about it (in our example, a pension fund certification authority).
8. The pension fund creates a transaction that contains information about the cancellation of a previously valid certificate, and adds it to the blockchain. Validators process the transaction and add it to the blockchain. As a result, the previously installed public key becomes invalid for all participants in the system while a new key is issued to Bob. 

Now let’s consider how it is possible to organize the use of an identifier to access the services of other organizations (Fig. 1.18).

![Figure 1.18 – Gaining access to services using the received identifier](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.18-gaining-access-to-services.png "Figure 1.18 – Gaining access to services using the received identifier")

1. Previously, a bank added Alice’s identification event to the distributed database, as discussed above.
2. Alice wants to interact with a pension fund. She is not registered in it, but the pension fund is also a node in the identification system. She forms a request to the pension fund, which includes her identifier (public key), and signs the request with a private key, which confirms her ownership of the identifier.
3. The pension fund verifies the confirmation of Alice’s identifier in the distributed database. If the pension fund trusts the bank regarding the identification and certification of users, it provides the corresponding service to Alice.
4. If the pension fund does not trust the bank enough (for example, the bank is a young company), it can request Alice’s identification data from the bank (on the condition that Alice gives her consent).
5. In this case, the bank extracts data from the storage and transfers them to the pension fund. Note that Alice must provide permission to access her personal data during the formation of a request to the pension fund.
6. The pension fund receives data and can verify their authenticity (a hash of the data stored in a distributed database), after which the fund provides Alice access to its services.
7. The pension fund is required to store this information, so it must immediately receive it and save it in its storage.


### Digital identity for IoT

There are nine basic uses of IoT (Figure 1.19) [78].

![Figure 1.19 – Internet of Things application areas](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.19-IoT-application-areas.png "Figure 1.19 – Internet of Things application areas")

By using blockchain technology, it is possible to organize an identification system suitable for everyone: in the area of e-health for reliable exchange of information about the components of medical equipment, in the manufacturing industry for authentication of the data on related components and manufacturers, etc. As an example, let’s look at how an identification system containing information about car sensors can be designed (Fig. 1.20).

![Figure 1.20 – Using an identification system for designing IoT infrastructure for car sensors](/resources/img/volume-3/1.2-Decentralized-digital-identity-systems/F-1.20-identification-system-with-IoT-infrastructure.png "Figure 1.20 – Using an identification system for designing IoT infrastructure for car sensors")

1. Sensor manufacturers place identifiers and public keys of their products in an identification system.
2. Sensors transmit information from the onboard computer of the car (while signing all the messages transmitted).
3. The on-board computer addresses the distributed ledger and verifies the validity of the sensor’s public keys.
4. Based on the information received, the onboard computer gives commands to the car’s systems.
5. Suppose Eve wants to send false information on behalf of one of the sensors.
6. The computer requests a public key that matches the identifier received. Since the system does not have Eve’s identity (and if it is present, then another public key value is bound to her), the onboard computer will not process the received request.

### \*\*\*Frequently asked questions\*\*\*

_– Can a decentralized system support multi-factor authentication? How will this work if I want to use some knowledge as the first factor and my biometric data as the second factor?_

A decentralized authentication system can support multi-factor authentication, and the process will be divided into several stages. For example, at stage 1 you authenticate with the first provider by entering the secret PIN. However, for a decentralized system, confirmation from only one provider will not be enough, therefore, you will need to additionally pass biometric authentication with the participation of the second provider that stores your biometric data. Thus, only after passing several checks and confirmations the decentralized system will be sure that you are the user who you pretend to be.

_– Which consensus reaching algorithms can be applied for the identity systems using blockchain technology?_

Since the users of these systems pass identification, it is a permissioned environment. For consensus reaching in such an environment, the algorithms such as BFT, FBA, DPoS, or PoA are utilized.


## 1.3 Decentralized e-voting platforms

In the simplest case, voting can be defined as a way of making a collective decision, which implies the formation of a common opinion based on the counting of votes of members of a particular group. In this subsection we will examine several ways of organizing e-voting, the advantages and disadvantages of the described approaches as well as how these approaches are used in practice. 


### Challenges of traditional approaches to voting

Traditional approaches to voting can be considered quite effective and secure only when it comes to polling residents of one house regarding the color of the front door or choosing a team captain when engaging in sports. In the context of larger and more demanding events, such as nation-wide elections or referendums, far more complex and robust solutions are required; until a certain time there were no better alternative ways to organize voting in certain areas. 

Such approaches have the following disadvantages and limitations:

> * _Non-transparency of vote counting_
> * _Possibility of having fake voters_
> * _Pseudo-anonymity of voters_
> * _Possibility of vote forging in ballots_

The first limitation is that the vote counting is performed in a trusted closed environment. The voter is forced to trust those who form and announce the voting results and cannot independently verify that his/her vote has been counted correctly (Fig. 1.21).

![Figure 1.21 – Non-transparency of vote counting procedure](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.21-non-transparency-of-vote-counting-procedure.png "Figure 1.21 – Non-transparency of vote counting procedure")

Speaking about the second restriction, an analogy with “dead souls” comes up: the voter database may not be up-to-date at the time of performing the voting and there is a risk that non-existent users may vote. This means that a reliable source of voter data is required. Its role can be performed by a separate authority (as implemented now) or an identification system (how this should be implemented).

Due to the third restriction of traditional approaches, a voter’s identity may be disclosed and the voter can be subjected to pressure so that he/she votes for the “right” candidate (Fig. 1.22). This pressure can also be applied in the form of bribes, threats of physical violence, damage to professional reputation, etc.

![Figure 1.22 – Pseudo-anonymity of voter](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.22-pseudo-anonymity-of-voter.png "Figure 1.22 – Pseudo-anonymity of voter")

The fourth restriction is related to the fact that after the vote some of the ballots remain empty since a certain percentage of voters do not want to participate; by filling them out, one can create votes “from air”. It is also worth noting that by ruining the valid ballots the interested party can get rid of genuine votes. In addition, there have been cases where fading ink pens were used at a polling station [18].

Note that a lot of votings nowadays still take place in the analog (manual) form. In addition to the inefficiency and insecurity, organizing such voting is very expensive. However, there are electronic voting systems; a few examples of such systems are discussed below.


### E-voting in Estonia

In 2005, Estonia became the first country in the world to hold nationwide elections using e-voting, and in 2007 it was used during parliamentary elections [19]. The ability to vote electronically was introduced in addition to the traditional voting method (i.e. at the polling station with a paper ballot).

The I-Voting (Internet voting) is a system that allows citizens to vote from anywhere in the world by using special software. Its introduction solved the problem that exists when using any remote voting method, including traditional postal ballots – the possibility of forced or bribed voting. Estonia’s solution is to allow voters to enter the system and vote several times during the pre-voting period. Since each vote cancels the previous one, the voter can change his/her vote, but every time a vote is cast again the system asks the user to confirm this action [19].

The infrastructure of the I-Voting system is shown in Fig. 1.23. It includes the following components:

* Voting client
* Verification app
* Vote forwarding server
* Log server
* Vote storage server
* Vote counting server
* Hardware security module (HSM)

![Figure 1.23 – I-Voting system infrastructure](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.23-I-Voting-system-infrastructure.png "Figure 1.23 – I-Voting system infrastructure")

To participate in I-Voting, one has to download the client application to the computer; it contains an election-specific public key for vote encryption (PK<sub>elect</sub>) and a TLS certificate for the forwarding server. During the pre-voting period, the voter enters the system using an ID card or Mobile-ID. 

The client software verifies the server’s identity using the previously mentioned certificate, and the server, based on the voter’s public key, confirms his/her right to vote and displays the corresponding list of candidates (_C_) for him.

The voter makes a choice _c_. After that, the client pads the vote _c_ using the RSA-OAEP [20] algorithm and encrypts it with the election’s public key (PK<sub>elect</sub>). For the encrypted vote (_b_), the user calculates the signature (_σ_) using his private key (SK<sub>voter</sub>).

The server receives the encrypted and signed vote _v_, associates it with a unique identifier _x_ and returns _x_ to the client (Fig. 1.24) [21].

![Figure 1.24 – Vote casting process](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.24-vote casting.png "Figure 1.24 – Vote casting process")

The voter can verify if his/her vote has been correctly recorded using special software. By scanning the QR code that the client shows for voting, it receives _r_ and _x_. The application sends _x_ to the forwarding server and receives _b_ and _C_ from it. For each vote option _c'_, it calculates the encrypted form and compares it with the previously received _b_; if there is a match, then the application shows the voter _c'_, otherwise it reports an error. The server allows performing this verification three times per one vote within thirty minutes after casting the vote (Fig. 1.25) [21].

![Figure 1.25 – Vote verification process](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.25-vote-verification.png "Figure 1.25 – Vote verification process")

> _Note. During the procedure described above, a voter can verify that he/she did not make a mistake when choosing a voting option, thus confirming the vote. Nevertheless, the voter cannot be sure that his/her vote will subsequently be correctly tallied._

After the online voting has ended, the storage server re-verifies the signatures of the encrypted votes and cancels invalid and revoked votes. The voter’s digital signature (_σ_) is separated from the vote (_b_) before it arrives at the vote counting server, which ensures voter anonymity (that is, the storage server does not transmit digital signatures to the counting server). A set of anonymous encrypted votes (_B_) is then transferred to the counting server. The counting server uses a hardware security module that contains the election private key (SK<sub>elect</sub>) to decrypt the votes and counts the votes for each candidate (_counts_[_c_]). The result of the counting of electronic votes (_counts_) is combined with the result of manual voting and published as the overall election result [21]. These actions are carried out by the election committee (Fig. 1.26).

![Figure 1.26 – Vote counting process](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.26-vote-counting.png "Figure 1.26 – Vote counting process")

E-voting results are not published until the end of voting on election day (day of manual voting). After the votes have been tallied (usually the next day), the votes are retallied to verify their integrity. In order for the vote tally to be verified publicly, e-votes are mixed and reordered. During the data audit, the auditor also checks the integrity of the ballot box, whether or not the recast votes were canceled and the anonymization of votes. Like the auditor, outside observers can also perform similar verification procedures [22].


### E-voting in Switzerland

Since 2003, more than 200 attempts have been made in Switzerland to introduce e-voting in various cantons. However, e-voting has not yet become a permanent voting method; in November 2018, less than 4% of the registered population had the right to vote electronically. For comparison: in 2017 31% of all votes in Estonia were cast electronically [23].

Over the years, two systems have been tested. The first one was developed by the canton of Geneva. Several other cantons also tried to introduce it. However, in 2018, it was announced that further development and support of this system will be stopped due to the high maintenance costs [23].

The second system – Swiss Post – is the development of the Spanish group Scytl. In two cantons, the system has been used since 2016 and 2017, respectively, both for federal voting and for local elections [24].

E-voting using the Swiss Post system is designed as an additional voting channel, which is offered along with voting by mail and manual voting. The voting process is illustrated in Fig. 1.27; it consists of the following steps:

1. Forming ballots (determining the voting subject and the list of vote options).
2. Access codes and confirmation codes generation.
3. Preparing and delivering voting materials.
4. Encrypting ballot boxes and splitting keys.
5. Casting votes (filling ballots and sending them).
6. Delivering the sealed electronic ballot box.
7. Decrypting the ballot box and tallying electronic votes.
8. Publication of results and statistics.

The Swiss Post system (marked in yellow) interacts directly with the voter (green) in steps 3 and 5 and with the government authority (blue) in steps 1, 4, and 6.

![Figure 1.27 – Voting procedure using the Swiss Post system](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.27-voting-with-Swiss-Post-system.png "Figure 1.27 – Voting procedure using the Swiss Post system")

By October 2019, Switzerland did not have a permanent e-voting system introduced. At present the government only announced plans that at least 18 out of 26 cantons would provide an opportunity to vote electronically in future parliamentary elections [23].


### Decentralized e-voting approach

A decentralized approach can improve the voting process by increasing the reliability, security, and resilience of a voting system. There are several important properties that this approach can bring:

> * _Ability to verify correctness of the vote tally_
> * _System fault tolerance_
> * _Voter anonymity_
> * _Ability to verify authenticity of votes_
> * _Protection from vote forging_

Below we discuss an example of decentralized voting.


### Example of a voting system with no central authority

We will examine a protocol that does not use the central organ created by M. Merritt in 1982 [25]. Suppose Alice, Bob, Carol, and Dave vote on something. Let each voter have a pair of keys (public and private), and let their public keys be known to each other (Fig. 1.28).

![Figure 1.28 – Voting participants](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.28-voting-participants.png "Figure 1.28 – Voting participants")

Let _E_ be an encryption function, _R_ a random string, and _V_ a string containing the vote (for example, the candidate number). For convenience, we introduce the following stages in this protocol: vote configuration, vote submission, and vote confirmation and tallying.

At the stage of vote configuration, each voter performs the following actions (Fig. 1.29):

1. Adds a random string _R<sub>0</sub>_ (salt) to his vote.
2. Encrypts the result of step 1 with Dave’s public key.
3. Encrypts the result of step 2 with Carol’s public key.
4. Encrypts the result of step 3 with Bob’s public key.
5. Encrypts the result of step 4 with Alice’s public key.
6. Adds a random string _R<sub>1</sub>_ to the result of step 5 and encrypts the result with Dave’s public key.
7. Adds a random string _R<sub>2</sub>_ to the result of step 6 and encrypts the result with Carol’s public key.
8. Adds a random string _R<sub>3</sub>_ to the result of step 7 and encrypts the result with Bob’s public key.
9. Adds a random string _R<sub>4</sub>_ to the result of step 8 and encrypts the result with Alice’s public key.

![Figure 1.29 – Vote configuration scheme](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.29-vote-configuration.png "Figure 1.29 – Vote configuration scheme")

The voter’s final message will look like this: $E_{\text{A}}(R_{4} \Vert E_{\text{B}}(R_{3} \Vert E_{\text{C}}(R_{2} \Vert E_{\text{D}}(R_{1} \Vert E_{\text{A}}(E_{\text{B}}(E_{\text{C}}(E_{\text{D}}(V \Vert R_{0}))))))))$. Each voter saves all intermediate results at each step of the calculations. These results will be used in further steps of the protocol to confirm that a particular voter’s vote will be counted.

The stage of vote submission implies the following sequence of actions (Fig. 1.30):

1. Each voter sends an encrypted vote to Alice.
2. Alice decrypts all votes using her private key, removes all random strings at this level, mixes all the votes and sends the result to Bob. Now each vote will look like this: $E_{\text{B}}(R_{3} \Vert E_{\text{C}}(R_{2} \Vert E_{\text{D}}(R_{1} \Vert E_{\text{A}}(E_{\text{B}}(E_{\text{C}}(E_{\text{D}}(V \Vert R_{0})))))))$.
3. Bob decrypts all votes using his private key, checks if his vote is among the received votes, removes all random strings at this level, mixes the votes and sends the result to Carol. Now each vote will look like this: $E_{\text{C}}(R_{2} \Vert E_{\text{D}}(R_{1} \Vert E_{\text{A}}(E_{\text{B}}(E_{\text{C}}(E_{\text{D}}(V \Vert R_{0}))))))$.
4. Carol decrypts all votes using her private key, checks if her vote is among the received votes, removes all random strings at this level, mixes the votes and sends the result to Dave. Now each vote will look like this: $E_{\text{D}}(R_{1} \Vert E_{\text{A}}(E_{\text{B}}(E_{\text{C}}(E_{\text{D}}(V \Vert R_{0})))))$.
5. Dave decrypts all votes using his private key, checks if his vote is among the received votes, removes all random strings at this level, mixes the vote and sends the result to Alice. Now each vote will look like this: $E_{\text{A}}(E_{\text{B}}(E_{\text{C}}(E_{\text{D}}(V \Vert R_{0}))))$.

![Figure 1.30 – Vote submission scheme](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.30-vote-submission.png "Figure 1.30 – Vote submission scheme")

Further actions relate to the stage of vote confirmation and tallying (Fig. 1.31):

1. Alice decrypts all votes using her private key, checks if her vote is among the received votes, signs all the votes and sends the result to Bob, Carol, and Dave. Now each vote will look like this: $S_{\text{A}}(E_{\text{B}}(E_{\text{C}}(E_{\text{D}}(V \Vert R_{0}))))$.
2. Bob verifies and removes Alice’s signatures. He decrypts all votes using his private key, checks if his vote is among the received votes, signs all the votes and sends the result to Alice, Carol, and Dave. Now each vote will look like this: $S_{\text{B}}(E_{\text{C}}(E_{\text{D}}(V \Vert R_{0})))$.
3. Carol verifies and removes Bob’s signatures. She decrypts all votes using her private key, checks if her vote is among the received votes, signs all the votes and sends the result to Alice, Bob, and Dave. Now each vote will look like this: $S_{\text{C}}(E_{\text{D}}(V \Vert R_{0}))$.
4. Dave verifies and removes Carol’s signatures. He decrypts all votes using his private key, checks if his vote is among the received votes, signs all the votes and sends the result to Alice, Bob, and Carol. Now each vote will look like this: $S_{\text{D}}(V \Vert R_{0})$.
5. Everyone checks Dave’s signature. They make sure that their votes are among the received votes by finding one’s random string.
6. Everyone removes random strings from each vote and tallies the votes. This way, each participant (voter) obtains his/her own voting result.

![Figure 1.31 – Vote confirmation and tallying scheme](/resources/img/volume-3/1.3-Decentralized-e-voting-platforms/F-1.31-vote-confirmation-and-tallying.png "Figure 1.31 – Vote confirmation and tallying scheme")

Alice, Bob, Carol, and Dave will immediately find out if any one of them tried to cheat. In order to detect fraud, there is no need for any central authority. To see how this works, here is an example of a fraud attempt.

If someone tries to add a vote, Alice will discover this attempt in the second stage of voting, when she receives more votes than the number of people participating in the voting. If Alice tries to add a vote, Bob will also discover this in the second stage of voting.

Substitution of one vote for another is another fraudulent option. Since votes are encrypted with various public keys, everyone can create as many valid votes as needed. At each stage (2 and 3), vote substitution at different steps is detected differently.

If someone replaces one vote with another at the third stage of voting, his/her actions will be detected immediately. At every step, votes are signed and sent to all voters. If one or several voters discover(s) that his/her/their vote is no longer among the set of votes, he / she / they immediately stop(s) implementation of the protocol. Since the votes are signed at every step and everyone can go back a few steps in the third part of the protocol, it is easy to find the cheater who replaced the votes.

Replacing one vote with another in the second part of the protocol is more exquisite. Alice cannot make a substitution because Bob, Carol, and Dave will discover it in the next steps. If Bob replaces the votes of Carol and Dave (remember, he does not know who each vote belongs to), Carol or Dave will notice this further at their respective steps. They will not know who substituted their votes (although it must be someone who has already processed the votes), but they will know that their votes have been replaced. If Bob was lucky and managed to change Alice’s vote, she will not notice it until the third part of the protocol. Then she will discover that her vote disappeared but will not be able to find out who changed her vote. At the second stage, the votes are mixed and are not signed at each step, so no one can execute the protocol in the reverse direction and determine who replaced the votes.

Another form of fraud is an attempt to find out who voted for whom. Because of the votes mixing in the second part, no one can go back and tie the votes to the voters. Removing random strings in the second part is also crucial for preserving anonymity: if the strings were not removed, the mixing of votes could be inverted by re-encrypting the received votes with the public key of the person who mixed them. When the protocol stops, the confidentiality of the votes will be preserved.

Moreover, due to the initial random string (_R<sub>0</sub>_) even identical votes are encrypted differently at each step of the protocol. Thus, no one can know the value of the vote at the last step of the third stage.

What are the problems of this protocol? Firstly, it needs cumbersome calculations. In the given example, only four people take part in the voting, but it is already complicated. Such a protocol will not work in real elections with tens or hundreds of thousands of voters.

Secondly, Dave will know the election results before the others. Although he cannot influence the result, he gets a certain advantage.

Thirdly, one participant can copy the vote of another participant without even knowing its contents in advance. To understand why this might be a problem, consider the voting example for the three voters – Alice, Bob, and Eve. Suppose Eve does not care about the voting result, but she wants to know how Alice voted, and for this, she copies her vote. As the result, the majority of the votes will be guaranteed for the answer option for which Alice voted: either 3 out of 3 if Bob voted for the same answer, or 2 out of 3 otherwise.


### Using blockchain technology for an e-voting system

In order to apply decentralized e-voting to regular elections, we need a protocol that allows us to ensure the transparency of voting and the anonymity of users at the same time. Using blockchain as a technology on which a decentralized system can (not must) be based can provide the following features:

> * _A voter can verify at any time that his/her vote has been counted correctly_
> * _Presence of the voting right proof and verification of the vote integrity due to using digital keys and signatures_
> * _Vote forging is impossible because it will be immediately detected in the vote submission history_

The first and most important advantage of using blockchain to configure a voting platform is that a voter can really make sure that his/her vote has been counted correctly and that the voting results are based on the transactions made. In this case, specific methods can be used to ensure the anonymity of voters (for example, ring signatures) while preserving the verifiability of the voting results.

Using cryptographic signatures and blockchain allows us to ensure the verification of authenticity and integrity of all sent transactions. It is important to provide a reliable source of information about voters’ public keys. Having a reliable public key infrastructure, every user will be able to verify that the votes came from existing people, while information about the vote sender can be hidden.

Votes in this case also cannot be backdated, since such a change will be visible to all network participants who store the transaction history (possibly even to lightweight nodes). Should such a voting system  provide the opportunity of re-voting, it will only be possible by submitting a new transaction.

Noteworthy, the platforms which can be called decentralized ones primarily aim to provide the ability to verify the results and the fact whether the specific participant’s vote has been tallied. These are the features not provided by traditional voting systems.

### \*\*\*Frequently asked questions\*\*\*

_– What other methods to ensure user privacy in e-voting systems exist?_

As one of the alternative options for ensuring privacy, the ring signature mechanism can be used. If a set of users’ public keys is known in advance, a voter can use the public keys from this set and his private key to generate a ring signature of a vote. Note that when verifying this type of signature, the verifier can make sure that the signature was calculated by one of the group members but cannot determine by whom exactly.

_– Are the e-voting systems in Estonia and Switzerland decentralized? If they aren't, why are they considered out of the context of traditional approaches to voting?_

Both discussed systems are not decentralized; both imply the presence of the central authority that receives and tallies votes (the election committee). They combine traditional methods for the designing of voting with the ability to vote without paper ballots and physical presence at a polling station (with the voting in electronic form).


## 1.4 Decentralized exchange technologies

The exchange is a platform where buyers and sellers can make deals with each other. The exchange can focus on trading goods (commodity exchange), securities (stock exchange), currency (currency exchange), hiring workers (labor exchange) and other activities. With the emergence of cryptocurrencies and other digital assets the need arose for introduction to the market of platforms that support exchange of such assets for fiat currencies and mutual exchange.

Centralized exchanges were the first to appear. They act as an intermediary and are responsible for the storage and management of all funds that are traded on the platform. The first exchange which supported bitcoin – Bitcoinmarket.com – appeared on March 17, 2010 [26].

> **_Challenges of centralized exchanges_**  
> * _Risks of platform hacking_
> * _The keys to coins are controlled by the exchange_
> * _Risk of regulatory bans/sanctions_
> * _Need for users to fully trust the exchange_

The basic disadvantage is the risk of platform hacking. Centralized exchanges are more vulnerable to scammers and hackers since they are a good target: all funds are stored centrally (plus, all transactions made on the platform are irreversible and rather difficult to trace without special tools). This simplifies the matter for an attacker (instead of conducting expensive attacks on end users with little benefit, he attacks the exchange itself), thus making centralized exchanges prime hacking targets. In Fig. 1.32, the consequences of the most famous attacks on centralized exchanges are provided [27–40].

![Figure 1.32 – Attacks on centralized exchanges and consequent losses](/resources/img/volume-3/1.4-Decentralized-exchange-technologies/F-1.32-attacks-on-centralized-exchanges.png "Figure 1.32 – Attacks on centralized exchanges and consequent losses")

The second disadvantage is that the keys to coins are controlled by the exchange, which means that the user must fully trust it. That is why a centralized exchange can limit users when trying to withdraw funds from the platform. Very often, users spend a lot of effort and money to restore access to coins after receiving the “Withdrawal of funds is temporarily disabled for this account” message [41].

In addition, there exists the risk of government (and not only government) bans or sanctions. The servers of the exchange may be arrested, and the exchange itself may block the accounts of residents of certain countries due to political or regulatory issues. The hardest bans and sanctions were introduced by the governments of China and Korea [42]. For example, in November 2019 the Chinese authorities closed the BISS cryptocurrency exchange due to violation of state laws and arrested ten people suspected of involvement in its operations [43].

Also, centralized exchanges work only with verified users, so at this stage the anonymity property can and will be mandatorily lost. Users must comply with KYC requirements, which include providing identification documents.

From here, another risk arises – leakage of personally identifiable information (PII) through centralized exchanges. In 2018, information about users of Bittrex, Poloniex, Bitfinex, and Binance centralized exchanges was put on sale by hackers [44]. The above-mentioned facts indicate that it has become much more dangerous for individuals to use centralized exchanges.


### Operational principles of decentralized exchanges

An alternative that solves the discussed issues are _decentralized exchanges_, sometimes shortened to DEX (Fig. 1.33). These platforms allow users to keep control over funds and are only involved in matching orders of users who want to exchange (sometimes they also ensure the security of these user exchanges). The difference between the operating models of centralized and decentralized exchanges is somewhat similar to the difference between centralized (Kazaa, Napster) and decentralized (BitTorrent) peer-to-peer file-sharing systems.

![Figure 1.33 – Difference in operation of centralized and decentralized exchanges](/resources/img/volume-3/1.4-Decentralized-exchange-technologies/F-1.33-difference-in-operation-of-exchanges.png "Figure 1.33 – Difference in operation of centralized and decentralized exchanges")

Unlike a centralized exchange, a decentralized exchange does not store keys to user funds but is only a platform for making transactions without intermediaries, that is, there is no centralized party that controls and ensures the operation of a decentralized exchange. There are several approaches to designing decentralized exchanges.

> * _Escrow_
> * _Atomic swap_
> * _Smart contracts_
> * _Internal exchanges_

It should be understood that decentralized exchanges are not one specific technology. There are many exchanges and many respective technologies. They should be primarily distinguished by the method for organizing processes such as order book creation, order book storage, order book access control, and _order matching_. There are decentralized exchanges in which users themselves perform some part of the processes. For example, such processes can be the public key exchange to create intermediate addresses or the audit of smart contracts that ensure completion/cancellation of an exchange. Based on this, DEX can be divided at least into two groups as follows:

* Ones with an on-chain order book
* Ones with an off-chain order book

In the ideal case, a decentralized exchange could work with an unlimited number of digital currencies, with an unlimited number of users (traders) and would allow trustless exchanges. Such a decentralized exchange would be the only one, because a bigger number of ones would not be needed. In this context, the most suitable technology is atomic swap combined with an open order book creation system. However, this is only the ideal view for a decentralized exchange; in practice, people are only trying to achieve something similar.


### Escrow

An _escrow account_ is a special account for making secure settlements between a buyer and a seller. It is also called a _conditional account _because funds from it can be transferred only after certain conditions are met. Under the escrow procedure the buyer puts money into an escrow account, and the seller can withdraw it only after he/she fulfills the  conditions set forth in the contract. The security of this approach lies with an independent intermediary - an _escrow agent_ - that monitors the fulfillment of the conditions and that is, in traditional finance, the owner/operator of the discussed escrow account (this is not always the case with cryptocurrencies).

Such accounts are most often used when exchanging cryptocurrencies for fiat currencies, but they can also be used when exchanging one cryptocurrency for another. An exchange takes place according to this principle: cryptocurrency is held in escrow until payment is made. For example, two people who want to exchange dollars and bitcoins find each other. In this case, an intermediary is selected who will monitor whether a particular deal is made fairly for both parties. Together they create a 2-of-3 MultiSig address (an escrow account). The first key will belong to the one who wants to get dollars, the second key to the one who wants to get bitcoins, and the third to the intermediary. To complete the transaction, 2 signatures are enough: either the parties agree among themselves, or, if there is a dispute, the intermediary accepts the position of one of the parties and helps resolve the dispute (for an appropriate fee). One of the exchanges that implements this technology is BitSquare.


### Atomic swap

_Atomic swap_ is a technology that allows exchanging one digital asset for another without centralized intermediaries. There is a prerequisite for making an atomic swap: the currencies exchanged must have the same hashing algorithm and must support hash time locked contracts (HTLC). We already examined in detail how atomic swap works in the second volume of the book.

Not all cryptocurrencies support atomic swaps, which is the disadvantage of this technology. The systems that currently support them include Bitcoin, Ethereum, and other similar systems. Note that there is still no single standard for atomic swaps. All systems which currently support atomic swaps use cryptography and smart contracts without a single standard.

> _Note. The first mention of this technology dates back to 2013. However, only on September 20, 2017 Decred developers created the first working smart contract using SCRIPT to allow an atomic swap between Decred and Litecoin_ [45]_._

An example of an exchange that uses these technologies is BarterDEX [46]. BarterDEX is a decentralized exchange built on the Komodo platform which uses atomic swap technology.

> _Note. Komodo uses the delayed proof-of-work (dPoW) consensus reaching algorithm. dPoW includes a mechanism that notarizes blocks in a chain. This algorithm implies storing transaction backups in the selected blockchain, so that, if there is an attack on Komodo, the entire transaction history can be restored. This feature is achieved by adding the current system state “snapshot” to a block._

BarterDEX exchange supports more than 100 different coins and tokens, including Bitcoin-based currencies, Ethereum-based currencies, and tokens issued on the Ethereum platform. A limitation of this platform is the inability to conduct exchanges between digital and fiat currencies.

An important feature of the platform is that the order book is also decentralized. For the operation of the order book, BarterDEX maintains a custom peer-to-peer network that uses two types of nodes: a _full-relay node_ and a _non-relay node_.

The main difference between these types of nodes is that a full-relay node is usually a user with high sales volume who provides liquidity in exchange for being a trading hub in the network. This allows him/her to complete transactions faster than his trading competitors. Non-relay nodes are end-user nodes that use BarterDEX when exchanging one digital asset for another.


### 0x protocol

The _0x protocol_ project was founded in October 2016 [47]. 0x is not a platform that implements a decentralized exchange. It is an open permissionless protocol (contract) for a decentralized exchange that operates within Ethereum.

All exchanges are made _off-chain_. Only the result of the interaction in the form of a transaction is recorded in the chain itself The chain records contain only data on ownership rights changes (other data do not appear on the chain).

The exchange flow implies the following steps:

1. A user creates an order, describes the desired exchange rate and its expiration time (after which the order cannot be completed) and then signs it with his private key.
2. Next, the user needs to find another user who he/she can exchange with. If such a user is predetermined, then a 0x order can be simply sent directly (i.e. by email, via a chat, or at an over-the-counter platform). If a counterparty has not been found yet, then the order is sent to a _relayer_. Relayers maintain an order book outside the blockchain. They allow users to find, create, execute, or cancel orders. The relayer can also communicate with other relayer nodes and create a set of orders to increase liquidity (Fig. 1.34).

![Figure 1.34 – Sending orders to relayer nodes](/resources/img/volume-3/1.4-Decentralized-exchange-technologies/F-1.34-sending-orders-to-relayer-nodes.png "Figure 1.34 – Sending orders to relayer nodes")

3. As soon as someone finds an order and wants to make an exchange on it, he sends an order to the blockchain along with the amount he wants to fulfill. Unlike an exchange, the relayer does not make or ensure the deal - it can only facilitate bidding by “broadcasting” user orders to the network.
4. The buyer sends his signature to the decentralized exchange smart contract along with the signature of the order creator.
5. The contract verifies the digital signatures of users and fulfills all conditions of the deal. If everything is correct, then the assets involved will be atomically swapped between two users. If any of the order conditions are not fulfilled, the application for the order execution will be rejected.

> _Note. As we mentioned earlier, in 0x there are the following two types of orders: a broadcast order and a point-to-point order. The first one is similar to the usual order on the exchange: a user sends an offer to the network indicating that the user is ready to buy (sell) a certain amount of tokens at the specified price. The second one implies the existence of a preliminary sale-and-purchase agreement between the holders of two specific accounts. This method allows exchanging through quite a number of channels, including email and instant messengers. The deal initiator sends his request to a relayer node, and only the owner of the account specified in it can reply to it._


### Internal exchanges

An _internal exchange_ is an exchange that operates within an accounting system and allows users to exchange digital assets that are issued in this system. Exchanges of this type work on platforms such as Stellar and Bitshares.

The Stellar system can be used to store and transfer ownership rights of any type, such as dollars, euros, bitcoins, stocks, gold, and others. Within it, any asset can be exchanged for any other.

Imagine that a user wants to exchange BTC for ETH.

1. A user creates an offer, which is then checked against the order book for the specified asset pair. 
2. If there is a match, then a deal is made. Otherwise, the offer is saved in the order book and remains there until the corresponding order is created or until it is canceled by the user. 
3. It is possible that the exchange will be completed in more than one operation. If the order book exchange has a very large spread or does not exist at all, it can be more beneficial if an exchange is made in several moves (Fig. 1.35). For example, if we want to exchange BTC for ETH, then we can first exchange BTC for XLM, and just after that proceed with XLM for ETH.

![Figure 1.35 – Exchange through several intermediaries](/resources/img/volume-3/1.4-Decentralized-exchange-technologies/F-1.35-exchange-through-several-intermediaries.png "Figure 1.35 – Exchange through several intermediaries")

These ways of converting assets can contain up to 6 hops, but the entire payment is atomic – either it will be successful or none of the transfers will be completed (the payment sender will never be left with an asset that he does not need). The process of finding the best payment path is called _pathfinding_. Pathfinding includes viewing current orders and searching for a series of exchanges (hops) that would ensure the most profitable deal.

> _Note. The internal exchange in Bitshares has its own on-chain order book. To make an exchange, a transaction must be created where a user declares that he is ready to exchange one asset for another. Then this transaction is propagated over the network and receives confirmation, after which another user can declare in the same way that he wants to exchange the same assets at the same ratio. At the moment of confirmation of the second transaction, according to the protocol, the balances of both users are updated. We examined the operation principles of Bitshares in Volume 2 of the book._

### \*\*\*Frequently asked questions\*\*\*

_– Why are the internal exchanges of the accounting systems such as Bitshares and Stellar also called decentralized exchanges (DEX), although they exist only within one accounting system and do not maintain assets of other systems?_

The reason for this is that the internal exchange option appeared much earlier than the atomic swap method and the idea of trustless exchanges between different accounting systems. Moreover, if all processes of an internal exchange are decentralized, then it qualifies as a DEX by definition. It should also be noted that an internal exchange has a number of advantages as compared to an exchange based on atomic swaps. These include shorter confirmation time, no need for direct interaction between the deal participants, deal execution without user participation, and – typically – a fewer number of transactions and fees to complete a single deal.

_– What is the thing that, when an exchange is made through Stellar, ensures that the user will not be left with an intermediate currency that he does not need?_

_Batching_ is the concept of joining several operations in one transaction. This is the guarantee that when executing a series of operations, if one operation fails upon submission to the network all other operations in the transaction automatically fail [80]. For example, if we want to exchange BTC for ETH, then if there is no possibility to make an exchange directly, you can first exchange, for example, BTC for XLM, and just after that proceed with XLM for ETH. In this case, these operations are batched, and if one of them is not executed, then the rest of them will be canceled.

_– Does decentralized exchanges have native tokens or cryptocurrencies?_

Currently, there are several exchanges that use their native token to pay commission fees when making transactions on their platforms. Examples of platforms using their native token within their own ecosystem are the following exchanges: Binance (Binance Coin), Bitfinex (UNUS SED LEO), Huobi (Huobi Token), OKEx (OKB), Kucoin (KuCoin shares), Exmo (EXMO Coin), BitMax (BitMax Token), Bibox (BibBox Token), FTX (FTX Token), and Gate.io (GateChain Token).

_– Suppose we deposit some amount of USDT of one system (Omni Layer) to some exchange and then withdraw the corresponding amount of USDT of another system (Ethereum). Are the assets considered fungible in this case?_

According to the same scheme, you can exchange $100 for a brick with a stranger outdoors. However, after such an exchange, a dollar and a brick do not become fungible, do they? There is an opinion that digital assets cannot be considered completely fungible if they are minimum distinguishable from each other. If at least one party of the exchange does not agree to exchange the A<sub>1</sub> asset for the A<sub>2</sub> asset, which is minimum distinguishable from A<sub>1</sub>, then there is actually no fungibility between these assets.


## 1.5 Decentralized auction

An auction is a specially designed market for selling goods, services, or assets where (in its classic form) participants themselves propose the price (place bids) they think is suitable to buy goods, and the participant who has made the highest bid becomes the buyer (winner). Thus, the main task of the auction is not to complete deals but to determine the winner and the price.

Initially, the design of such markets was that sellers and buyers got together, inspected the goods, and held an auction for each particular product. The reliability of this design consists in the openness of the actions of participants, i.e. everyone sees (hears) the bid of each person directly and can be sure about the correctness of determining the winner and the price (Fig. 1.36).

![Figure 1.36 – Scheme of interaction between offline auction participants](/resources/img/volume-3/1.5-Decentralized-auction/F-1.36-interaction-in-offline-auction.png "Figure 1.36 – Scheme of interaction between offline auction participants")


### Operation principles of an online auction

With the spread of the Internet, online auctions have begun to emerge as well. This approach has the following advantages: one can participate in the auction remotely; the duration of the auction can be significantly longer; parallel auctions can be held for many products simultaneously.

However, until 2015 online auctions were created exclusively as centralized solutions. On such platforms, apart from auction participants (sellers and buyers), an _organizer_ is actually needed. The role of an organizer is that he accepts bids from participants and at the end of the auction announces the winner (Fig. 1.37). In this case, the organizer is the exact decision-making center, which does not imply the ability to audit every user request. The obvious drawback of this design is that the participants cannot be sure about or confirm the correctness of the decisions made by the organizer.

![Figure 1.37 – Scheme of interaction between centralized online auction participants](/resources/img/volume-3/1.5-Decentralized-auction/F-1.37-interaction-in-centralized-online-auction.png "Figure 1.37 – Scheme of interaction between centralized online auction participants")

The simplest scenario of fraudulent abuse of this system can be projected  as follows. Let’s say Anatoliy is the organizer of an online auction where a large piece of land with a nice brick house near the central park will soon be put up for sale. Taras knows about this. Taras also knows that such a house can be easily sold for 100,000 coins. Therefore, before the auction of the house begins, Taras comes to Anatoliy and asks him to set up his platform so that it does not accept bids above 30,000 coins. Taras offers Anatoliy a reward of 10,000 coins if he declares Taras the winner of the auction with the opportunity to buy a plot with a house for 30,000 coins. In this case, at the end of the auction, Taras can buy a plot with a house at a lower, pre-determined price, and the seller will not even know that there were people who wanted to pay more.

Despite the fact that the organizer of the centralized online auction is not an intermediary and does not participate in the deal itself, he can choose the winner to his liking and thereby affect the price of the goods. Therefore, centralized online auctions require trust from both sellers and buyers. The problem of trust as well as the problem of interaction between users of different platforms can be solved using a decentralized approach.


### Operational principles of a decentralized online auction

The task of ensuring the fault tolerance of the auction when accepting bids from participants can be achieved through the use of a shared database that is protected from modifications. For example, if all participants publish product data and bids to the Bitcoin database, then the history of their actions will remain integral and protected from modifications, and since the database is open, each participant can independently determine the winner.

At first glance, it might seem that this would be enough for the normal operation of the auction and the role of an organizer is already unnecessary. But in practice, this method will not work because it is not protected from participants who are not going to buy goods but only inflate the price with fake bids.

Next, let’s consider one of the approaches to designing a decentralized online auction which is successfully applied in practice [48]. This approach entails that there exists a large number of independent platforms where a seller can register and create a lot for the sale of goods, and a buyer can register, prove his solvency, and make bids. As a rule, different platforms have different owners, different applications and user interfaces, different localization, different methods of proving solvency, different sets of supported product categories, etc. But, at the same time all these platforms support a single protocol for the unified accounting of lots and bids. They are actual validator nodes of a decentralized accounting system, to which they record the actions of their users. In this case, a lot created by a user of one platform will be available to users of all other platforms, and a bid made by a user of one platform will be saved by all platforms and visible to all users (Fig. 1.38).

![Figure 1.38 – Scheme of interaction between decentralized online auction participants](/resources/img/volume-3/1.5-Decentralized-auction/F-1.38-interaction-in-decentralized-online-auction.png "Figure 1.38 – Scheme of interaction between decentralized online auction participants")

This approach uses the BFT-based algorithm as a mechanism for achieving consensus, where the weight of each validator (platform) is proportional to the number of its users. Thus, the better service the site provides, the more users it will have, and the bigger its weight in reaching consensus similarly to proof-of-stake-based consensus mechanisms. If users notice that the owner/operator of their platform does not correctly handle their requests or denies service, they switch to another platform, thereby reducing the weight of the “bad” platforms and increasing the weight of the “good” ones.

To protect the system from fake users and bids, digital receipts certified by the bank are used. When registering on the site, a user provides a receipt signed with a digital signature of his bank, which confirms his/her solvency. All sites publish such receipts in a united database to confirm the number of their users.

In terms of ease of use, a decentralized auction built using this approach is comparable to a centralized solution – an application with a regular account. In terms of capabilities and reliability, it has several advantages: lots and bids are synchronized between all platforms, and probability of collusion or abuse is minimized.

Another distinguishing feature of this approach is that owners of long-existing centralized online auction platforms can move to a new platform relatively easily. To do this, one does not need to build the entire infrastructure from scratch or transfer users to new, unfamiliar applications, since it is enough to integrate a network node into the existing infrastructure for the system to work. To commence synchronization, platform owners only need to exchange network addresses and public keys.

### \*\*\*Common myths\*\*\*

_A decentralized auction guarantees that the winner receives the corresponding goods or service._

A decentralized auction can only guarantee that anyone who wants can independently audit the accounting system and get the same result as other honest auditors. In fact, the result of the auction operation is not a completed transaction but a determined winner and a determined price. Furthermore, the winner and the seller act independently and can use the results of the auction operation to protect their rights.

### \*\*\*Frequently asked questions\*\*\*

_– How does one deal with possible overloads of a decentralized accounting system which processes actions of all its users regarding all its lots?_

There may simultaneously exist several separate decentralized accounting systems, each of which processes auctions for particular types of goods or services. Furthermore, a platform can synchronize with several accounting systems simultaneously, and its users may not even know that their actions are recorded in different systems. This eliminates the existence of a single overloaded accounting system and saves platforms from processing data on goods that are not interesting to its users. For example, Alice’s platform is synchronized with the accounting systems of auctions for car sales, home furniture, and winter clothes, whereas Bob’s platform is synchronized with the accounting systems of auctions for car sales, computers, and pets.

_– Can any user place a lot on a marketplace?_

Yes, any user can. Typically, all digital auctions are located on digital exchanges and each participant is identified to participate in auctions. However, before placing the lot, it is necessary to confirm its existence. This can be implemented using digital certificates. A certificate issuer must be trusted by a particular exchange, containing the platform which has a marketplace and on which an auction is held.

# [2 SCHNORR SIGNATURES AND THEIR APPLICATION](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/2-Schnorr-signatures-and-their-application.md)
