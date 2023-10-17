# 1 Decentralization As a Paradigm in Information Systems

## 1.1 Peer-to-peer networks and the BitTorrent protocol

The BitTorrent protocol was one of the first protocols that showed how file-sharing services can be decentralized. It worked much faster than centralized solutions, where the downloading speed was limited by the server performance and connection, and, at the same time, it was more resistant to censorship. This protocol played an essential role in the field of decentralized technologies by showing how to separate the process of storing and transmitting data, all within one system. We will consider the fundamental concepts of the protocol, the properties it provides, and how this technology can be developed and applied in next-generation systems.

### Traditional client-server architecture
To understand how BitTorrent works, let’s see how traditional centralized file sharing systems operate and why this approach is so ineffective. Let’s start by introducing three basic terms: the client, the server, and the client-server architecture. A client is a piece of hardware and/or software that uses services of a server. A server is software and/or hardware that provides data to other software and/or hardware devices (i.e., its clients).

So what does the client-server architecture mean? Let’s describe it using simple language. Imagine some servers that store large datasets. If needed, a client contacts one of these servers requesting a certain data subset. The server processes the client’s request and provides (or does not provide) access to the desired content (Fig. 1.1).
<p align="center">
<img src="/resources/img/volume-2/1.1-Peer-to-peer-networks-and-BitTorrent-protocol/Figure-1.1-Traditional-client-server-architecture.png" alt="Figure 1.1 – Traditional client-server architecture" width="70%"/>
</p>

> **_NOTE:_** *In centralized file sharing systems, the client and server roles are strictly divided and in most cases cannot intersect. More on that, these systems pose some additional limitations and disadvantages.*

> **Issues of centralized systems with the client-server architecture**
> * Subject to censorship
> * Single point of failure
> * Capacity limitation on the server side
> * Difficulty in recovering the lost dataset

One of the most prominent issues is the content censorship on the side of a centralized party that controls servers. For example, in many countries there is strict censorship related to political and social  media; you may not get access to certain content (film, video, etc.) if someone with sufficient influence has decided that you shouldn’t have access to it [1].

The second problem is the storage of large amounts of data in a single center; an attack on this center may result in a complete loss of all the data or restrict access to it for an indefinite period of time. Many services, such as Google, use pre-backup and duplication of the stored data on a large number of servers, which are geographically separated [2]. This approach allows to protect the data from loss. However, note that not every service can afford such protective measures and therefore has to solve the problem of real-time data synchronization between servers. 

The third limitation of centralized services is the data downloading speed. Typically, it depends on a number of factors, such as the type of transfer protocol, server capacity, the number of other clients' active connections, etc.

Since the data is often stored by the server as a single copy in one place, it can greatly reduce the overall system performance if many clients request the same data simultaneously.

It is also worth noting that some content can be stored in a single copy; if the server (which previously stored the requested content) crashes and the content is lost, even Google will not help you return it (Fig. 1.2).

<p align="center">
<img src="/resources/img/volume-2/1.1-Peer-to-peer-networks-and-BitTorrent-protocol/Figure-1.2-Сontent-becomes-unavailable-when-it-is-lost-by-the-server.png" alt="Figure 1.2 – Сontent becomes unavailable when it is lost by the server" width="70%"/>
<p>

### Principles of building a peer-to-peer file sharing network
The architecture of peer-to-peer (p2p) file sharing systems is fundamentally different from traditional ones. Each node in the network is both a server and a client. Every network participant can be involved in the distribution of files to other nodes as well as receiving files from them (Fig. 1.3).

<p align="center">
<img src="/resources/img/volume-2/1.1-Peer-to-peer-networks-and-BitTorrent-protocol/Figure-1.3-Principle-of-operation-of-peer-to-peer-file-sharing-system.png" alt="Figure 1.3 – Principle of operation of peer-to-peer file sharing system" width="70%"/>
<p>

When the node's software finds a network participant with the requested content, it downloads the data directly from this source. When you request part of the content, you are a client. When one of the participants needs to access data that you store, your software will act as a server and share the stored data.

### BitTorrent development history
The BitTorrent protocol was created by Bram Cohen in 2001 [3]. He managed to solve one of the most important tasks that society faced with the advent of the Internet: a reliable transmission of large amounts of data in an environment with low capacity and a periodic denial of service of the transmission channels used.

The BitTorrent company was founded in 2004. The project drew attention in many areas, especially among those involved in the illegal distribution of media products. They turned the BitTorrent protocol into another Napster (a service for sharing mp3 files). Unfortunately, BitTorrent was not as successful as its technology. Several attempts to become a media company have not brought the desired results. One thing is certain: BitTorrent protocol has become a disruptive technology that has shown how a decentralized file sharing system can work.

In 2019, more than 170 million people use the BitTorrent protocol: Facebook [4] and Twitter [5] use BitTorrent to deploy servers, and the Dutch University uses it to update workstations [6]. The BitTorrent protocol is in widespread use due to the fact that it allows reliable data transmission even in networks with unstable connection and low capacity.

### How does BitTorrent work?
The BitTorrent protocol is an open protocol for sharing files in peer-to-peer systems, which implies p2p data transmission between system participants and the absence of a central server party.

In order to access a specific piece of data, a network participant addresses a tracker, a server that monitors computers connected to the network. Its task is to provide IP addresses of active participants in the system with the requested content parts. Note that the participants never download data from a tracker itself: the tracker is only concerned with providing the active nodes' necessary IP addresses.

After receiving a list of active nodes, the user asks them for the necessary data fragments. Note that the participant downloads the data in small parts while checking their integrity along the way. This allows the particular fragments of the requested content to be saved and the missing fragments can either be downloaded from other sources or after reconnecting – even if one of the nodes is shut down. This way, the necessary content can be obtained in small parts and from different sources (Fig. 1.4).

<p align="center">
<img src="/resources/img/volume-2/1.1-Peer-to-peer-networks-and-BitTorrent-protocol/Figure-1.4-Obtaining-data-fragments-from-system-participants.png" alt="Figure 1.4 – Obtaining data fragments from system participants" width="60%"/>
<p>

In order to reach a greater level of decentralization, the mechanism implementing DHT (Distributed Hash Table) has been added to the BitTorrent protocol in 2013. We will examine the operational principles of DHT in more detail in Section 1.2. In a nutshell, this improvement avoids using centralized trackers to search for content sites and makes the interaction of sites more independent and decentralized.

However, most trackers didn’t like this update since it allows eliminating all intermediaries in a decentralized file sharing network. Countering this, trackers adopted the addition of a special flag to the information part of a torrent file. If a user decided to remove this flag and use DHT, the identifier of the torrent file would change completely. This divided the users, since nodes using DHT could not receive the required files from nodes that used trackers. It was also much easier to find torrent files that use trackers – after all, the owners of media sites make money on it. Thus, the centralized trackers are still popular and widely used.

It is important to mention that the BitTorrent protocol stipulates the rewarding of active network participants. The more actively the node participates in the distribution of files, the higher will be its total download speed. On the contrary, users who rarely participate in the distribution of files (download the necessary content and leave the distribution) have a lower speed of downloading the content. The protocol also requires to reserve a part of the channel bandwidth, which does not allow a particular node to only download content, but also makes it distribute the data.

Another feature of the BitTorrent protocol is the primary distribution of rare fragments. For example, if a single node that contains the entire dataset receives requests from others who want to get this dataset, it evenly distributes all pieces of data between the participants and does not transmit the entire sequence to each of them (Fig. 1.5). This feature positively affects the speed since the nodes that received the fragment can distribute it further themselves. Additionally, the storage of all fragments is distributed allowing to avoid a situation when many nodes store only the first fragment, and only one node stores all the other fragments.

<p align="center">
<img src="/resources/img/volume-2/1.1-Peer-to-peer-networks-and-BitTorrent-protocol/Figure-1.5-Arbitrary-transmission-order-of-file-fragments.png" alt="Figure 1.5 – Arbitrary transmission order of file fragments" width="60%"/>
<p>

### Data fragmentation
Many readers are familiar with torrent files (.torrent), but not everyone knows how they are formed and what they contain. We have previously determined that content is downloaded in small portions (fragments). Let’s consider how these fragments are formed and where to get links to download them.

When creating a torrent file, the initial data is divided into small parts (the size can vary from 128 KB to 2–4 MB). The SHA-1 hash value is taken from each received fragment and placed in the torrent file (Fig. 1.6) [7].

<p align="center">
<img src="/resources/img/volume-2/1.1-Peer-to-peer-networks-and-BitTorrent-protocol/Figure-1.6-Creation-of-a-torrent-file.png" alt="Figure 1.6 – Creation of a torrent file" width="70%"/>
<p>

> **_NOTE:_** *when distributing a large amount of data, the fragment size can reach 32–64 MB.*

Also, a link to a tracker (or several trackers) is placed in the torrent file so that the user receiving the source torrent file knows which tracker can provide him with the addresses of the content keepers. The network participant who first published particular data to the network and created the torrent file is called the initial seeder.

When one of the participants has a torrent file and wants to receive the content corresponding to it, he contacts the tracker specified in the file and requests addresses whose nodes store the fragments specified in the torrent file. After the tracker returns a list of addresses to the participant, the node sends requests with hash values of the fragments to the received addresses and waits for someone to return the required fragments. When downloading fragments from the network, the node can quickly and simply check whether the necessary fragments were downloaded, since it has the corresponding hash values. This approach prevents errors when downloading files and checking the integrity of all the received content by checking the integrity of its fragments.

### Protocol limitations
Note that besides its obvious advantages, the BitTorrent protocol also has a number of limitations.

> * Dependence of file download speed on the number of active nodes that contain copies of the file
> * Threats related to the presence of malicious code in the downloaded  fragments 
> * Many nodes use protocol only for downloading while not supporting the network (meaning they do not distribute files)
> * Inability to selectively limit the distribution of content (copyright infringement)

The download speed depends on the number of active system nodes that store the file. If the content is popular, then it is likely to be stored by a large number of network participants and accessing it will not be a problem. However, if there is no particular demand for the content (for example, a textbook on catching caterpillars), and it is stored by only one network node, then any communication failure or shutdown of this node will disable access to the requested content.

When a user receives a torrent file, he only has a set of hash values and information on where to download the necessary content. He can only guess if the downloaded content will be the same as expected. If, alongside with content, you actually manage to download several browsers rather than a piece of malicious code, then it’s already a good start.

Another important issue is the lack of motivation for nodes to maintain the network. Very often the node downloads the requested content fragment and does not remain on the distribution. The BitTorrent protocol provides a mechanism for rewarding active distributors (increasing capacity for receiving content), which turned out to be quite ineffective since rewards are non-monetary. This issue is partially solved in the IPFS protocol, which we will examine in more detail in 1.5.

Last but not least, the issue that mostly concerns copyright holders is copyright infringement by transmitting pirated content. According to statistics, copyright holders lose more than 10 billion US dollars a year solely due to piracy in the music industry. Further, more than 40% of users' software is pirated and more than 75% of all computers in the world have at least one illegally installed program [8].

**Common myths** 

*Trackers and DHT cannot be used simultaneously.*

In fact, each user can simultaneously use both approaches to download different files at the same time. The restriction is imposed only on the use of the two approaches for downloading a single file, since for these cases, the hash values of the torrent files will differ. Note that most trackers check whether the flag is present and add it if it is absent when the torrent appears on their sites.

*Using DHT significantly reduces the performance of BitTorrent.*

This is one of the most common myths. Moreover, many of the readers who used both trackers and a DHT will say that this is not a myth and the system performance is really reduced. However, this has nothing to do with the technology itself. The efficient use of DHT can even increase productivity, but there are a lot of restrictions on the way: the size of the space allocated for building a DHT, the number of system participants, the total size of the content stored, the influence of centralized trackers, etc.

**Frequently asked questions**

*– What is seeding in BitTorrent?*

Seeding is the process of distributing files on the network to other nodes. While downloading fragments of files from the network, every client automatically distributes them to other nodes, thereby increasing the network operation speed. Moreover, each participant can distribute files to maintain the network, even after they have been fully downloaded so that other nodes can download them faster. This process is referred to as seeding.

*– Who are leechers and peers?*

A leecher is a user who downloads larger amounts of information than he distributes on the network. At the moment, the protocol itself does not penalize such behavior, although part of the community is in favor of introducing a penalty. A peer is a user who connects to the participant who distributes a file. After a peer completely downloads the file, he becomes a seeder.

*– Are there options to ensure anonymity of users on BitTorrent network?*

The BitTorrent protocol does not ensure the anonymity of its users. Any user can see the IP addresses of all nodes in the list of their torrent client. Because of this, in some countries, copyright protection organizations can report copyright infringements of individual users to their Internet service providers and have the user prosecuted legally. In order to do that, they obtain  a users' IP address who illegally distributes information limited by copyright.

*– Can Internet service providers limit BitTorrent traffic and how can this be prevented?*

Since BitTorrent accounts for most of the traffic, some Internet service providers restrict (slow down) BitTorrent transmission. To solve this problem, many torrent clients use protocol header encryption and message stream encryption. These traffic masking methods help to make it difficult to detect BitTorrent traffic, thereby preventing providers to do the regulation.

## 1.2 Operation principle and application of DHT
DHT (Distributed Hash Table) is one of the cornerstone protocols of decentralized networks [9]. The protocol was invented in the late 90s to solve the problem of indexing and searching within a large amount of data. Despite the fact that the data itself (e.g., files) can be stored on different servers, the table of data links can potentially be infinitely large, hence excluding the option of storing it in one place. In addition, the protection against attacks on the central node was an important problem. DHT was developed to solve these two problems.

### Problems that DHT solves
DHT solves the problem of finding content by its hash value (InfoHash) in a decentralized network.

> **Conditions under which the DHT protocol works**
> * Participants do not identify each other
> * There can be an unlimited number of participants
> * There is no central server
> * Content is stored by the participants themselves (and it is not known in advance by whom exactly)
> * Every participant can communicate only with a limited number of other participants

For a participant who is searching for data, the task is to get the network address of another participant storing a content part with a specific InfoHash.

The environment in which the content is stored and searched is entirely distributed: many computers that run a specific version of the DHT protocol and conduct independent messaging.

### How does DHT work?
According to the protocol, every system participant has a unique global identifier. This identifier is a number of the same dimension as InfoHash (usually 160 bits or more). The participant chooses the identifier himself, since there is no decision-making center and all participants are independent. To start operating, a new participant needs to have a program with protocol implementation and the network address of at least one more active participant (it is necessary to connect to an existing network).

According to the protocol rules, each participant allocates some memory (5–50 MB by default) for their own table. In this table, he stores the mappings between InfoHash and content as well as between the participant identifiers and their network addresses (Fig. 1.7). 

<p align="center">
<img src="/resources/img/volume-2/1.2-Operation-principle-and-application-of-DHT/Figure-1.7-Network-node-s-table.png" alt="Figure 1.7 – Network node's table" width="60%"/>
<p>

All records in this table are sorted by the first column (identifier or InfoHash), and it is constantly updated according to the following rules:
> 1. If a participant detects a new participant (identifier + network address) when communicating with others, this entry is put into the table.
> 2. If a participant downloads or generates content, then InfoHash of this content is calculated and the appropriate mapping is also put into the table.
> 3. If the table size approaches the limit of allocated memory, then the rows that have identifiers that differ most from the participant’s own identifier are deleted from the table.

Thus, only the top and bottom rows of the table are deleted (Fig. 1.8). During a certain period of time, the participant will form his own table. It will store identifiers that are as close as possible to his own identifier since they are sorted in an increasing order. Accordingly, each participant stores a list of identifiers of other participants and content links that are as close as possible to their own identifier.

<p align="center">
<img src="/resources/img/volume-2/1.2-Operation-principle-and-application-of-DHT/Figure-1.8-Table-row-deletion-scheme.png" alt="Figure 1.8 – Table row deletion scheme" width="60%"/>
<p>

In order to find the necessary content, an active network node needs to know only the corresponding InfoHash. The procedure of searching for specific content by a participant begins with addressing his own table. A row with the identifier closest to the InfoHash is selected from the table. This row will store the network address of the participant who is closest to the desired content. Then the searching participant opens a connection towards this address and sends a request to search the specific InfoHash. In response to the request, he receives either the desired content or the network address of another participant who is even closer to this content (Fig. 1.9). The requests will be repeated until the desired content is received. In the case of failure, requests can be repeated after a certain time period.

<p align="center">
<img src="/resources/img/volume-2/1.2-Operation-principle-and-application-of-DHT/Figure-1.9-Search-request-propagation-scheme.png" alt="Figure 1.9 – Search request propagation scheme" width="60%"/>
<p>

Each participant stores and updates a mapping table locally, as shown in Figure 1.9. It’s possible to quickly search and insert new rows since the list is sorted. Note that for participants with close identifiers, some part of the table may be identical (Fig. 1.10).

<p align="center">
<img src="/resources/img/volume-2/1.2-Operation-principle-and-application-of-DHT/Figure-1.10-State-of-tables-of-participants-with-close-identifiers.png" alt="Figure 1.10 – State of tables of participants with close identifiers" width="70%"/>
<p>

To increase the search reliability and speed, requests are executed in parallel on several near network participants. This search mechanism has a fairly high degree of reliability; it is not censored and well-scalable. The number of network participants and the number of content fragments stored is practically unlimited. There is a large number of modified versions of this protocol aimed at improving its various properties such as search concealing, larger data storage, resistance to Sybil attacks, accelerated search for popular content, and ensuring the integrity of requests and responses.

DHT protocols are an indispensable component of many popular projects: I2P, IPFS, GNUnet, BitTorrent, and Tox.

Before DHT, a similar content search problem was solved by using centralized services or a group of centralized services that synchronized with each other. The centralized service also has a public API for requests and, in most cases, a registered domain name. In fact, such services store full tables of mappings between InfoHash and network addresses, which in turn turned out to be much more than 50 MB in volume. An example of such a service is Torrent Tracker. As it is highlighted above, such trackers are easily censored and can be blocked.

### Issue of content blocking by an attacker
The most common attack on DHT protocols is when an attacker blocks one specific piece of content almost completely.
Let’s suppose that a man named Taras decided to limit access to Satoshi Nakamoto's real photo because receiving such information by an interested party could cause irreparable damage to the Bitcoin network.

To do this, he launches a number of specially modified nodes. The modification here lies in selecting InfoHash that is closest to the targeted content’s InfoHash instead of generating it randomly. Also, modified nodes react differently to search requests on this InfoHash – they return non-existent network addresses. Consequently, a large number of fake nodes surrounds the identifier of Satoshi’s photo; thus, almost all participants looking for this content are being directed to these addresses (Fig. 1.11). This is how access to one specific piece of content can be blocked temporarily.

<p align="center">
<img src="/resources/img/volume-2/1.2-Operation-principle-and-application-of-DHT/Figure-1.11-Scheme-of-blocking-a-specific-piece-of-content-when-being-searched.png" alt="Figure 1.11 – Scheme of blocking a specific piece of content when being searched" width="70%"/>
<p>

**Common myths**

*Using DHT requires a lot of disk space.*

This is not entirely true. It is important to note that the requirement for DHT database size is completely dependent on the number of network nodes. More nodes in a distributed network that use the technology leads to lower requirements in terms of the amount of data stored in the table.

DHT works very slowly, and it is difficult to use in systems that handle large amounts of data.

Searching for content using DHT is, in fact, slower than using traditional methods. However, DHT is more fault-tolerant and is required in decentralized systems in particular. Some examples are BitTorrent and IPFS (see details in 1.5).

*An identifier obtained by hashing the IP address of a node can be considered secure.*

This approach is theoretically vulnerable to IP spoofing. The concept of IP address spoofing is that an attacker modifies the IP packet header in such a way to mimic another node. If an attacker manages to get a response to his request, it means that he can assign himself virtually any IP address. Consequently, building a system whose security relies on the limited space of available IP addresses can be extremely dangerous.

**Frequently asked questions**

*— Is the privacy of DHT users assured?*

No. Since all the hash values of the table are publicly visible, tracking the distribution is a realistic possibility.

*— What benefits did the DHT implementation bring to BitTorrent?*

DHT allows either reducing the load traffic upon trackers. It also eliminates them since it uses the information built into a torrent file. Now, BitTorrent can choose whether to proceed to a tracker or to go through a DHT node-set.

## 1.3 Web-of-trust concept
Today most of the people use the internet. They make purchases without leaving their home, message on social networks, and much more. Asymmetric encryption, digital signatures, and digital key algorithms are widely used to protect the transmitted data.

In this context, it is important to correctly map a specific user to his public key in order to avoid a man-in-the-middle attack when an attacker replaces the transmitted public key with his own key. Such kind of attack allows him to send and sign messages on behalf of the target user.

To ensure the reliability of the usage of digital signatures and other asymmetric algorithms, key certification authorities (CAs) are used. This centralized approach has two key disadvantages: 1) the private key of the root certification authority can be compromised and 2) certification authorities have to be trusted. However, there is a solution — a completely decentralized approach known as the “Web of Trust” (WoT).

### Public key certificate concept

To better understand the WoT approach, let’s consider how the traditional public key infrastructure is organized.

> *Public key infrastructure (PKI) is a set of tools, methods, and policies needed to create and process public key certificates that enable the reliable operation of asymmetric cryptographic algorithms [65].*

> *A public key certificate is a document establishing the association between a public key and its owner; it is signed and issued by one of the CAs [65].*

The structure of the public key certificate according to the X.509 standard [64] is shown in Table 1.1.

<p align="center">
<img src="/resources/img/volume-2/1.3-Web-of-trust-concept/Table-1.1.png" alt="Table 1.1" width="60%"/>
<p>

The main fields in the public key certificate are *subject public key* and *certificate signature*, calculated by the certification authority. Note that with a digital signature, the certification authority confirms that the public key is mapped to a specific person.

### How does hierarchical PKI work?
The hierarchical PKI is a traditional organization model, which assumes that certification authorities (CAs) are connected in a hierarchical tree structure (Fig. 1.12).

<p align="center">
<img src="/resources/img/volume-2/1.3-Web-of-trust-concept/Figure-1.12-Certificate-authorities-hierarchy-scheme.png" alt="Figure 1.12 – Certificate authorities hierarchy scheme" width="70%"/>
<p>

Hierarchical PKI has the following features:

> * Each relationship between a child and parent CA is represented by a separate certificate signed by the parent node (the root certificate is self-signed)
> * All CAs except for the root CA are subordinate to a parent CA
> * CAs may have subordinate CAs and issue certificates to them or to end users
> * All users trust the same root CA

Based on these features, it is quite simple to explain how the hierarchical PKI model works. At the top level, there is the root key certification authority. The root authority has its own key pair that it uses to sign certificates for lower-level certification authorities. At the same time, the root certification authority also has its own public key certificate that it generated for itself.

When receiving a certificate, every CA has the right to independently issue certificates for its children CAs. This is how the certificate chain, where every subordinate one is signed by the parent, continues to the end users.

One of the advantages of this PKI design approach is that it’s simple to build and verify certification chains since every user and CA has a certificate issued by exactly one parent CA. Thus, certificate chains are relatively short and all lead to a single root CA.

To better understand the described structure, let's consider an example in which Alice needs to verify Bob's certificate (in order to make sure that the quarterly report she received was actually signed by Bob). At the same time, Alice’s and Bob’s certificates were issued by different certification centers. This scheme can be represented as follows (Fig. 1.13).

<p align="center">
<img src="/resources/img/volume-2/1.3-Web-of-trust-concept/Figure-1.13-Hierarchical-PKI-scheme.png" alt="Figure 1.13 – Hierarchical PKI scheme" width="50%"/>
<p>

Alice has her own public key certificate, which was signed and is stored by CA1. Bob has a similar certificate (however, it was issued and is stored by CA2). In this case, CA1 and CA2 also have their own certificates that have been issued by the same root CA0.

When Alice wants to verify the authenticity of Bob's public key (the one she used to verify the digital signature value), she receives a public key certificate from him and checks who signed this certificate. Apparently, it was signed by CA2. Since Alice does not personally trust CA2, she also requests the public key certificate, which was issued by the root CA0. Alice trusts CA0 (because this certificate authority issued a certificate to CA1) and makes sure that Bob's public key actually belongs to him. Note that in this example there are two certificates in the chain (we check the certificates of Bob and CA2). If Bob's certificate was signed by CA1, then Alice could directly contact CA1 and immediately verify that the certificate was correct.

An advantage of the hierarchical approach is also the ability to quickly respond to the compromise of an end user's private key (if this happens, a user needs to contact the parent CA).

*Key compromise is the incident when, in fact or suspected, a third party becomes aware of a user's private key.* Compromise also includes the loss of physical control of a key by its owner.

Suppose that the private key that belonged to Bob was stolen by Taras. Now Taras can send fake messages to everyone on behalf of Bob and sign them using Bob's private key. In this case, Bob must immediately inform the CA that his private key has been compromised. CA can quickly react and mark Bob's certificate as invalid and then issue him a new certificate with a new public key.

If Alice receives a message signed with Bob's old key, she addresses it to CA2. As in the previous case, CA2 informs Alice that the certificate of the required public key is no longer valid and it is likely that someone has attempted to outwit her (Fig. 1.14).

<p align="center">
<img src="/resources/img/volume-2/1.3-Web-of-trust-concept/Figure-1.14-Alice-verifies-Bob-s-certificate.png" alt="Figure 1.14 – Alice verifies Bob's certificate" width="50%"/>
<p>

However, what happens if the keys of one of the CAs are being compromised instead of the end user’s keys (for example, CA2)? In this case, CA2 tells the root CA0 that its keys are compromised. CA0 marks the CA2 certificate as invalid and issues it a new certificate with a new public key, thereby returning the compromised authority back to the hierarchy. However, since the old CA2 public key is no longer valid, all certificates issued by this CA become invalid. Therefore, users in a compromised segment lose their ability to use the PKI services (their certificates also become invalid; unless users stop interacting or update the certificates, they may be outwitted by an attacker who has stolen the secret key of CA2).

Let’s now consider what will happen if the private key of the root CA is compromised. The consequences are the same but they impact the system on a global scale: the entire infrastructure must be suspended for some time. Note that it is crucial to notify all the system participants about the compromise of one of the CAs since in this case, an attacker can issue fake certificates, revoke valid certificates, etc.

Hierarchical PKIs have obvious advantages: high speed of building and tracking certificate chains as well as quick reissuance of certificates in case the private keys of both users and CAs are compromised. However, such models have several disadvantages as well.

> **Drawbacks of hierarchical PKI**
> * The necessity to verify the entire certificate chain
> * Compromising the private key of the root CA creates a critical situation since the entire network may break down
> * Issues related to the synchronization of certification authorities among themselves
> * Users do not actually manage their identities
> * Difficulties in system compatibility (signature algorithms, etc.)

### Operational principles of web-of-trust
Web-of-trust is an alternative to the hierarchical PKI model. The concept was first applied for authentication of users' public keys in the PGP protocol in 1991. Moreover, the web-of-trust approach can be used for identification mechanisms in a decentralized environment and rating systems (there are many browser add-ons for online rating of sites that use the web-of-trust mechanism).

To some extent, the basis of the web-of-trust model’s reliable operation is social consensus: no user will trust another user's certificate until this participant receives confirmation of his public key from at least one participant who is in the trust circle of the first user.

Before proceeding directly to the principles of web-of-trust, let’s define two concepts that are pretty closely related to each other and that are the basis of the entire concept — *trust* and *validity*.

*Validity* determines the level of how well one user knows the second user. We denote the validity level from 0 to 1, where 0 means that the first user knows nothing about the second user (hereinafter, he knows nothing about the keys the second user owns), and 1 means he is completely confident that the public key is linked to a specific user.

*Trust* determines the level of trust between users, which means how much one user entrusts the identification of other participants to another user.

So, let's go back to the decentralized PKI. We have a system, where each participant owns a key pair. Additionally, each participant determines his own list of keys of other network participants that he verified (for example, he met with them in a cafe and personally received their public keys) as well as the validity and trust to these users. He signs the received keys with his private key, in other words, generates public key certificates for other users. Each user maintains a table that, approximately, looks as follows (Table 1.2).

<p align="center">
<img src="/resources/img/volume-2/1.3-Web-of-trust-concept/Table-1.2.png" alt="Table 1.2" width="70%"/>
<p>

In this case, the user can securely interact with those participants in the system whose keys are in his list and whose validity level is 1.

But what if you need to communicate with a system participant whose public key you don’t have? In this case, you would need to address the trusted users (whose trust level is higher in comparison) and ask if they have the public key of the desired user in their contact list. If one of the contacts contains a signed public key of the required network participant, then depending on the trust and validity level between the user and  the target participant, one determines how safe it would be to use the provided public key of the target user.

### Example with Italian mafia

To make things clear, let’s consider an example where Italian mafia bosses – Don Corleone, Don Tattaglia, and Don Fanucci – decided to organize a shared business network and use web-of-trust to organize the PKI. Note that the keys, in this case, will be used to encrypt and sign messages.

To do this, they organize a meeting of heads of the families where they exchange public keys (Fig. 1.15).

<p align="center">
<img src="/resources/img/volume-2/1.3-Web-of-trust-concept/Figure-1.15-Exchange-of-public-keys-between-heads-of-the-families.png" alt="Figure 1.15 – Exchange of public keys between heads of the families" width="50%"/>
<p>

Each of the dons forms his own table, which he fills in after the meeting. For example, consider the table (Table 1.3), which was formed by Don Corleone.

<p align="center">
<img src="/resources/img/volume-2/1.3-Web-of-trust-concept/Table-1.3.png" alt="Table 1.3" width="70%"/>
<p>

> **_Note._** *The validity level in this case is set equal to one because the dons exchanged the keys at their meeting, personally. The trust to Don Fanucci was determined by Don Corleone as 0.8 because the corresponding public key was provided without enough respect.*

Now don Corleone can send messages encrypted with the public keys of the remaining heads of the families without worrying that someone else can read them (at the same time, the recipients can authenticate the sender by digital signature value).

But what if one of the Dons needs to contact Sollozzo, whose public key is not in his list? To do this, he addresses Don Tattaglia to find out whether he has the corresponding public key. Don Tattaglia says that such a key exists and is signed by him, whereas its validity and trust are defined as 1 (that is, Don Tattaglia  fully verified and fully trusts Sollozzo). Since Don Corleone fully trusts Don Tattaglia (trust is equal to 1), and Don Tattaglia is sure that the public key belongs to Sollozzo, Don Corleone’s validity to Sollozzo is 1 (and he can confidently encrypt messages using the received key).

However, due to the old disagreements, Don Corleone doesn’t actually trust Sollozzo to identify other network participants and fills his own table (Table 1.4) as follows.

<p align="center">
<img src="/resources/img/volume-2/1.3-Web-of-trust-concept/Table-1.4.png" alt="Table 1.4" width="70%"/>
<p>

Doing this, Don Corleone emphasizes that for him, every certificate issued by Sollozzo will have validity equal to 0.1.

Now let's imagine that there is Don Barzini, whose public key is kept by both Sollozzo and Don Tattaglia (moreover, Sollozzo has the validity of 1, and Don Tattaglia has the validity of 0.3). If Don Corleone wants to write to Don Barzini, he requests the certificates from Sollozzo and Tattaglia, who both send him a corresponding certificate.

After receiving the certificates, Don Corleone calculates the validity. That means, he checks if the level of validity of the received public keys correspond to Don Barzini (by the way, they can be different if, for example, Sollozzo wants to cheat and read the message himself).

The validity of public keys that we don’t have in our own list is defined as follows:
> *validity(PK) = validity(PKi) * trust(PK)*, where
> * *validity(PK)* is the validity of the key not present in the list
> * *trust(PK)* is the level of trust toward an intermediary
> * *validity(PKi)* is the intermediary's validity of the target key

Accordingly, Don Corleone believes that Don Barzini owns the keys with a validity level of 0.24 (Don Tattaglia) and 0.1 (Sollozzo). These values are quite small, and Don Corleone does not dare to encrypt the message with the received keys, so he decides to directly visit Don Barzini.

### Advantages and limitations of web-of-trust technology

The main advantage of the web-of-trust concept is the ability to operate in a fully decentralized environment, where the interacting parties do not want to trust the central certification authority.

Note that when using web-of-trust, the failure of the entire system due to the compromise of the private key of one or more users is impossible. If the web-of-trust is used in a fully distributed environment, then the failure of one node can only affect the ability to interact with this particular node and will not greatly affect the operation of the other nodes between each other.

However, the web-of-trust technology has a number of limitations that impact the operation of the system.

> **Limitations of web-of-trust**
> * *In some cases, it is difficult to find a public key certificate with high validity level*
> * *Rebuilding the network is a complex process that takes a lot of time*
> * *It may take more time to find the necessary certificate*
> * *Storing the certificate database requires a certain amount of space on the nodes*

The first limitation is specific to the web-of-trust protocol. A network that uses web-of-trust will only work well and efficiently if all nodes are honest and have maximum validity and trust. In most cases, the level of users' trust to other users is far from maximum, which leads to a scavenger hunt when deciding which particular key to use.

If one of the user keys is compromised, the rebuilding of the network will take much more time than in the case with a centralized structure. To do this, the user must inform all nodes that store his certificates that he is compromised (more so, he needs to do this in a convincing manner since nodes can perceive it as a malicious message and do nothing). Also, if the user is a new network participant, she needs to behave honestly for a long time so that other nodes would increase the level of trust to her.

The third limitation is related to the difficulty of finding a specific certificate if the network has a large number of participants. This problem can be partially solved using DHT (see details in 1.2), but this approach requires additional software and resources.

The last problem is also related to the increased number of network participants. The larger the network, the bigger the number of certificates each node has to store (network efficiency directly depends on this).

***Common myths***

*Web-of-trust guarantees the ability to find a valid user certificate, but it may take a long time.*

This statement is a myth due to the features of trust between network nodes. It rarely happens that one node absolutely trusts the other node's certificate. The level of trust may be very high, but it is rarely 100%. Thus, it may happen that it’s impossible to find a user certificate with a level of trust that is sufficient for you. For example, a client can configure their software in a way that a certificate is considered valid only if its trust level through intermediaries is 95% or higher. Then there can be a chain leading to a specific certificate through 3–4 nodes with a 95% trust level between them. However, due to the peculiarities of calculation of the trust level between the requesting and the target node, it may turn out that the final trust level will be no more than 85%.

*In a web-of-trust network, you can increase the level of trust to yourself (your rating) by creating many fake identities and, correspondingly, create many lines of trust to yourself with the maximum value of validity.*

You can create fake identities and lines of trust, but this will in no way affect the calculation of your rate by real users since this calculation is carried out in regard to each particular user and their outgoing lines of trust. The opinion of a fake person is not perceived by real users since they did not establish a trust line to them.

***Frequently asked questions***

*— Can one user create several certificates for his different keys?*

It is possible to do so. And if different keys are used in different communication areas (for example, personal and corporate), this will not affect the confirmation level of certificates and will not confuse the interacting parties.

*— Is the web-of-trust protocol subject to the Sybil attack and can this attack affect the acceptance of a certificate with an initially low trust level by a node?*

The protocol does not limit the number of nodes in the network, which means that, theoretically, an attacker can create a large number of "participants" who fully trust his certificate. However, there is no way it can affect other participants in the network since the trust from node to node is one-way. In fact, nodes will not store certificates of new participants if they do not trust them, so the number does not matter.

However, there is a problem when a previously honest node with a high level of trust suddenly begins to behave maliciously and adds certificates of non-existent participants as target users. As a result, the network may stop trusting this user, however, the rebuilding of a system is a complex process and cannot be completed easily; an attacker may take advantage of that.

## 1.4 Overview of the BitMessage protocol
After Edward Snowden reported that more than 1 billion people in 60 countries are being monitored globally [10], the need to create a truly confidential means of communication became evident.

On March 21, 2013, the first beta version of the BitMessage client was released. A key innovation in the BitMessage protocol was the absence of central servers or any center that processes user data. The protocol works in line with the peer-to-peer paradigm meaning that every node is both a client and a server at the same time and there is no single centralized server.

### Operation principles of the protocol

For those who are interested in maintaining personal or corporate privacy, BitMessage is a good alternative to regular messengers.

> * *Support for end-to-end encryption of transmitted messages*
> * *Sender and recipient are not explicitly indicated in a message*
> * *Each node stores all the latest messages on the network*
> * *Proof-of-work as network spam protection*
> * *Support for checking the integrity and authorship of received messages*
> * *Support for public broadcast channels*
> * *Support for private group chats*

The key idea of the protocol is to support end-to-end encryption of transmitted messages. This means that only the sender and the recipient can read the message, and no intermediate links have this capability. To encrypt message data, the AES algorithm with a block length of 256 bits is used, which operates in CBC (Cipher Block Chaining) mode. In this case, the key is calculated as a shared secret between the sender and the receiver according to the ECDH scheme. The sender can receive such a secret autonomously – without interacting with the recipient. To do this, he needs to use his private key and the recipient's public key. The recipient uses his private key and the public key of the sender to be able to get exactly the same secret. Therefore, both participants have the same secret for message encryption. In addition, to improve security, instead of the ECDH algorithm, a shared randomizer can be used to generate a new encryption key for each new message.

> **_Note._** *The CBC mode implies that the encrypted value of the previous block is used to encrypt each subsequent data block.*

BitMessage messages do not specify the identifier of the sender and the recipient of a message. So how does the recipient determine that this particular message is addressed to him? To do this, each network node tries to decrypt all the messages it receives. Since the encryption key is available only to the sender and the recipient of the message, only they can access the message contents.

As noted earlier, the node tries to decrypt each received message. If it succeeds (if the message was actually addressed to it), the recipient can see the contents of this message. But what to do with messages that could not be decrypted? The BitMessage protocol implies that each node stores all received messages for 2 days. This is done so that the recipient of the message can access it even if he is not online at the time of sending the message. When the user connects to the Internet, he asks the neighbour nodes for all messages that were not received previously and tries to decrypt them.

As a network protection against spam, proof-of-work is used. To send a message, the sender must solve a resource-intensive task. The difficulty of proof-of-work depends on the size of the message and the volume of attachments included in the message.

Furthermore, the protocol provides the functionality of using a digital signature to verify the integrity and authorship of every message sent.

The BitMessage protocol allows the creation of broadcast channels and secret chats. Broadcast channels imply that the sender encrypts the message with a hash value of its own public key. Each network member who has the sender’s public key can decrypt such a message. Secret chats involve creating a shared secret for encryption between a group of users. It is important to note that such a secret chat cannot be censored by a third party.

### Addresses in BitMessage
The BitMessage protocol also implies that users have addresses. Let's observe how an address is generated and why it is needed.

Two key pairs are mapped to each BitMessage address: one pair is used to sign messages (ECDSA), and the other one is used to create a shared secret (ECDH). To create a BitMessage address, both public keys are used (Fig. 1.16).

<p align="center">
<img src="/resources/img/volume-2/1.4-Overview-of-the-BitMessage-protocol/Figure-1.16-BitMessage-address-generation-scheme.png" alt="Figure 1.16 – BitMessage address generation scheme" width="60%"/>
<p>

Public keys are concatenated and hashed using SHA512, and the hash value is calculated again for the result, but using RIPEMD160. The output is a 160-bit (20-byte) number. This value is concatenated with Version (protocol version value), Stream (message flow - we will examine it in more detail below) and Checksum (value which helps to prevent errors in writing the address). The "BM" prefix is used to indicate that the address belongs to the BitMessage network. As a result, the BitMessage address looks as follows:

> *BM-BcbRqcFFSQUUmXFKsPJgVQPSiFA3Xash* 

BitMessage supports 2 types of addresses:

> * *Deterministic address*
> * *Random address*

To obtain a deterministic address, the user must set the seed value to generate it. You can draw an analogy to the way a user sets a password to register for the service.

Since the seed is set directly by the user, some benefits and risks are related to this. The advantage is the ease of address recovery. When using another device, the user just needs to enter the seed value (the user will likely not want to determine the random seed value but will set it to a value that is easy for him to remember). After that, the device independently generates an address. The risks include the simplicity of hacking (brute force attack or dictionary attack), since the  seed is not a random value but a user-defined one.

Random addresses use a pseudo-random value as a seed to generate the address, and without knowing this value, the address cannot be recovered. Note that in this case, the user also needs to enter a seed to restore the address, but since a seed is a random value, the convenience of recovery is lost, while the security is increased when interacting with the system.

### Message structure in BitMessage
Next, we take a closer look at the message structure in the BitMessage protocol, shown in Table 1.5.

<p align="center">
<img src="/resources/img/volume-2/1.4-Overview-of-the-BitMessage-protocol/Table-1.5.png" alt="Table 1.5" width="70%"/>
<p>

The message contains 5 fields that are listed in the table. The first of these – *magic* – is a constant number that is used to identify the data stream. With this value, you can determine that the data that follows it belongs to the BitMessage network. The same value is used for the Bitcoin protocol (there, it is defined as 0xD9B4BEF9).

The following field is *command*, which contains the message type. Depending on the type, messages are divided into broadcast (broadcast message), private (private message) or ACK (notification of delivery of a private message). Each of the listed message types has its own purpose, which we will discuss later.

The next field is *length*, which indicates the length of the transmitted message in bytes. The longer the message is, the more resource-intensive the task that must be solved by its sender is. Note that the maximum message size in the latest protocol version is limited to 256 KiB.

The next component of the message is the field *checksum*, which contains the first 4 bytes of the SHA512 hash value of the transmitted data.

The key field of the message is the *payload*, which contains the transmitted data and the nonce that acts as a proof of work. When a message is received, the node first calculates the hash value of the payload, and if the received hash value satisfies the required difficulty parameter, such a message is saved locally and transmitted further over the network. If the hash value does not satisfy the required parameter, this message is deleted and not transmitted.

### Message types in BitMessage

As mentioned earlier, BitMessage supports three types of messages.

> * *Broadcast message*
> * *Private message*
> * *ACK message*

*Broadcast* messages can be decoded by any node whose address list contains the sender’s address. The broadcast message is encrypted with the hash value of the sender's public keys. The node that receives such a message determines that it is a broadcast message (by the field command in the message) and begins calculating the hash values of all addresses in its list one by one. Using the calculated hash value, the node tries to decrypt the received message. If the message could not be decrypted, the next address is taken. If the message is decrypted successfully, it is displayed to the user.

By sending such a message, the sender does not receive delivery notifications. Therefore, resending a broadcast message can only be done manually. By default, broadcast messages are stored by nodes for 2 days.

To send *private* messages, end-to-end encryption is used, which is based on a shared secret between the sender and the recipient. Only the recipient who has the corresponding secret and the sender can read these messages.

By default, nodes store private messages for 2.5 days. If during this time the user to whom a certain message was addressed does not appear on the network, the message will be sent again. For each resending of the message, a PoW recomputation is required. Since the recipient sends a notification to the sender, the software allows relaying a private message automatically.

ACK message is the delivery notification for a private message. The ACK message is sent by the recipient of the private message. This message type is encrypted the same way as the private message, and the nodes store this message for 2.5 days as well.

### Stream in BitMessage

Earlier, we mentioned the meaning of stream in the context of generating a BitMessage address. Now, we will have a closer look at what this value determines and why it is used in the protocol.

The scalability issue is key to the BitMessage protocol. Despite the fact that the protocol can provide a high level of anonymity and confidentiality of transmitted messages, the use of a system in which each network member stores all the data is extremely inefficient. Imagine a situation in which BitMessage will be used by millions of people. In this case, each of the participants will need to store all the messages of the other participants. For obvious reasons, such a system cannot function reliably.

The solution is a tradeoff between the system’s anonymity and scalability, which implies dividing users into so-called streams. Every stream defines a group of users who will receive and store messages corresponding to their stream.

Streams are a binary structure, as in Figure 1.17.

<p align="center">
<img src="/resources/img/volume-2/1.4-Overview-of-the-BitMessage-protocol/Figure-1.17-Scheme-of-the-division-of-network-nodes-into-streams.png" alt="Figure 1.17 – Scheme of the division of network nodes into streams" width="60%"/>
<p>

The stream number is contained in the address of the system participant and determines which of the streams the sender must connect to in order for the message to reach its addressee. Note that each network member stores messages corresponding to its own stream and child streams.

***Frequently asked questions***

*– How many connections does the BitMessage node need to maintain?*

To communicate with the BitMessage network, you must connect to at least one of the nodes. More connections with different nodes of other participants result in a more integrated network and lower probability of message loss. However, the protocol defines a limit regarding the maximum number of outgoing connections: there can be no more than 8. At the same time, the maximum number of incoming connections is tens of times larger.

*– Can I send a message to a user who is currently not connected to the network?*

Yes, the user can send a message to the BitMessage network, regardless of whether the recipient is online. The sent message is stored by all nodes for two days. When reconnecting, the recipient enters the network and downloads all new messages from the connected nodes. If the recipient has not been online for more than two days, then the message is deleted and requires resending. 

*– What is the message delivery time from the sender to the recipient according to the protocol?*

In practice, the message transmission time depends on two values: the time of propagation over the network and the time of waiting in the queue for decrypting the message. From a sender node to a receiver node, a message can go up to 30 seconds over the network depending on the number of intermediary nodes. It will wait for decryption in the queue with all other messages for about 5 seconds on average. Thus, if the recipient node is online and synchronized, it will see the incoming message 20–30 seconds after it is sent.

*— How can a node work passively?*

If it is possible for an attacker to trace the network or eavesdrop on it, the user can work completely passively without sending ACK messages. However, it is much more reasonable for the user to send an ACK message via another node, which may not even be aware of it. For example, Bob, who is afraid that Eve is eavesdropping on his network, will send the message to his friend or random node directly instead of sending Alice an ACK message directly. The latter, in turn, distributes this message and confirms it at the same time.

## 1.5 Architecture and features of the IPFS protocol

In section 1.1, we examined how a decentralized file sharing network can work: it is not regulated by any single party or entity, resistant to censorship, and directly supported by its participants. To organize such a network, the BitTorrent protocol was implemented, which laid down the basic design concepts enabling peer-to-peer transactions on the one hand, but at the same time, it had some limitations, including narrow focus of use.

On October 10, 2018, a new protocol for organizing a decentralized file-sharing network called IPFS was introduced. It applied the key concepts of BitTorrent and added a number of new enhancements to them. Moving forward, IPFS was used as a platform for many decentralized applications. In this subsection, we will elaborate on the basic concepts of IPFS, the advantages of the protocol over other protocols for building decentralized file sharing networks, and how IPFS can be used to implement decentralized applications.

### Basic principles of the protocol
IPFS (InterPlanetary File System) is a decentralized file sharing system protocol. This protocol uses the DHT concept (see 1.2) to distribute content and enable search between participants. IPFS uses MDAG (Merkle directed acyclic graph) to organize a convenient structure of content links. Like the BitTorrent protocol, IPFS does not simply store all files on each system node: each system participant only stores the part of the content that it considers necessary.

> * *Distributed content storing*
> * *Tree structure of content links (similar to a traditional file system)*
> * *Checking and excluding duplicate files*
> * *Ability to version file history*

In addition to content, each node stores its own hash table. This table contains a mapping between a specific identifier and data - either a piece of content or another participant's network address.

A key feature of IPFS, unlike many other protocols, constitutes the support for content versioning. Here is an example: you uploaded a document to a decentralized network and distributed it among other participants. Then you decide to fix some errors and update the document after it was uploaded to the network. In systems such as BitTorrent, it’s impossible to replace the old file with a new one (in theory, this can be done, but only if all nodes agree to delete the old version). Instead, a new document that has no connection to the old one is published on the network. This is a huge inconvenience for applications that rely heavily on working with document versioning. IPFS does not allow replacing old files with new ones as well. After the file was added to the network, it can be deleted only if everyone agrees to do this. However, IPFS supports the versioning of individual files. That means, when you update a certain document, you can associate the new version with the old one. This feature is quite similar to how Google Docs or GitHub work: you can keep the history of changes and restore old copies in case of need.

### How does IPFS work?

IFPS is designed in the same way as BitTorrent, meaning, it provides a model for addressing content, not content sources model. The protocol also implies the division of content into chunks of files (fragments), and the size of one such file cannot exceed 256 KB. Each file has its own identifier – its hash value. At the same time, the protocol does not strictly limit the use of a specific hash function (you can use SHA-1, SHA-256, KECCAK, SHAKE, X11, BLAKE, MD5, etc.). Note that the hash function identifier is used as the file identifier prefix. Thus, the user will be able to make sure that he is downloading the necessary file using the desired hashing algorithm, depending on the identifier.

Each IPFS file has the structure shown in Table 1.6.

<p align="center">
<img src="/resources/img/volume-2/1.5-Architecture-and-features-of-IPFS-protocol/Table-1.6.png" alt="Table 1.6" width="70%"/>
<p>

Files in IPFS can be divided into four basic types.

> **File types in IPFS**
> * *Blob (a dataset)*
> * *List*
> * *Commit (a state)*
> * *Tree*

A *blob* is the end fragment and contains a part of the downloaded file. A *list* contains a list of links to blobs (their identifiers) and/or links to other lists (one list is not enough to place the identifiers of all parts of the file when the file is more than 8 MB). A *tree* is a file that contains links to other trees, lists, and blobs. The difference between a tree and a list is that a list points to links to a single file, while a tree can be compared to a catalog (which contains many different lists and files). A *commit* is a file that refers to the parent file and contains some modifications.

Let's examine the process of sending a file in IPFS . Suppose you want to send a small text file to the IPFS network. If it is less than 256 KB, then you hash the file and send its identifier to the network. Now your text file, i.e. blob, is associated with the received identifier (Fig. 1.18).

<p align="center">
<img src="/resources/img/volume-2/1.5-Architecture-and-features-of-IPFS-protocol/Figure-1.18-Scheme-of-adding-a-small-file-to-the-blob-structure.png" alt="Figure 1.18 – Scheme of adding a small file to the blob structure" width="50%"/>
<p>

Now, consider what a list is in this context. The list is a collection of links to blobs from which you can assemble a complete file.

Assuming you want to send a file to the network that is larger than 256 KB (photo, video, etc.), you would need to split the file into four parts to send it. After you have done this and received the identifiers of each part, they are not connected in any way. For this reason, you create a list in which you specify all fragment identifiers (an analogy can be made with a file .torrent). In this case, in order to access the entire file, the network participant needs to get the list by its identifier (Fig. 1.19).

<p align="center">
<img src="/resources/img/volume-2/1.5-Architecture-and-features-of-IPFS-protocol/Figure-1.19-Scheme-of-adding-a-large-file-to-the-list-structure.png" alt="Figure 1.19 – Scheme of adding a large file to the list structure" width="50%"/>
<p>

Note that the list identifier depends entirely on the blobs identifiers, which, in turn, depend on the contents of the file parts. If one part is modified, this list will not be able to refer to it. However, as we mentioned earlier, IPFS allows versioning; further, we will take a deep dive into how versioning works in IPFS.

Trees are links to a large number of different content pieces. They may contain links to other trees, lists, and files. The tree identifier depends entirely on the identifiers of all its links (therefore, a modification in any of the files will be noticed). Schematically, trees can be represented as follows (Fig. 1.20).

<p align="center">
<img src="/resources/img/volume-2/1.5-Architecture-and-features-of-IPFS-protocol/Figure-1.20-Tree-structure-formation-scheme.png" alt="Figure 1.20 – Tree structure formation scheme" width="50%"/>
<p>

Now let's see what commits are. When a user wants to update one of the files that he has published to the network, he cannot simply delete the old file and add a new one, since the network is decentralized and it is unknown which nodes store this file. Also, the user cannot redefine the link to the old file so that it refers to the new one, because the identifier of each fragment depends entirely on its contents. Therefore, the user has no choice but to add a new file to the network. In this case, it will have a new unique identifier. In the BitTorrent protocol, such two files cannot be connected at the protocol level at all, but only nodes that store files can tell other nodes that they have 2 or more versions of one file in question.

In contrast, IPFS allows you to link all versions of content at the protocol level. For each new version, a commit file is created that references the parent. Note that if some fragments have not been changed, then alternative trees and lists also refer to these fragments. Schematically, a commit is shown in Figure 1.21.

<p align="center">
<img src="/resources/img/volume-2/1.5-Architecture-and-features-of-IPFS-protocol/Figure-1.21-Commit-structure-formation-scheme.png" alt="Figure 1.21 – Commit structure formation scheme" width="50%"/>
<p>

### Usage of IPFS

The listed operation principles allow building a variety of decentralized applications on top of the IPFS network.

> **Decentralized applications on top of IPFS**
>> * *Messengers and e-mail*
>> * *Media platforms*
>> * *Version control systems (Git)*
>> * *Sites*

One of the first applications that was built on top of IPFS was a messenger. Messengers operate in the following way: the user hashes a new message and sends it to the network. All chat messages are linked in a list and its version is updated with each new message (commit). Two prominent examples are Orbit chat [11] and PubSub Chat [66].

Naturally, IPFS was used for multimedia applications that would enable the exchange of video and music. IPFS allows you to create decentralized prototypes of YouTube and Google Play Music, where the servers are the participants themselves. dTube is such an application[12].

The IPFS versioning system allowed the creation of applications that require version control for the first time. Imagine a decentralized GitHub that cannot be censored by any legal authorities or commercial companies. These projects already exist, although they are much less demand in the market  compared to centralized products [13].

Another area of application for IPFS is the creation of decentralized sites. Instead of holding a server with the site, you can create a tree file with links to all its pages. Thus, having the value of the identifier for the tree file of the site, each user can download all of its pages (executable scripts can also be placed in separate files).

> **_Note._** *Most notably, the IPFS site is also available in IPFS.*

### Filecoin

Despite the large number of advantages that a decentralized IPFS system provides, it still has one key limitation that greatly impedes the implementation of all the listed applications – users  lack motivation to store data that is not of interest to them.

Many readers might remember the time  when they downloaded all fragments of a torrent file, terminated the operation of their network node and thus, stopped distributing the stored content. In fact, this was impacting the content that some users still needed. So how do you incentivize users to store files they are actually not interested to store? The Protocol Labs company (by the way, it also proposed IPFS) introduced Filecoin, a project that can solve this problem to some extent. Let's examine the basic concepts of the protocol.

> * *Network participants pay for storing and receiving data*
> * *Network participants receive rewards for storing data*
> * *Orders for the execution of decentralized network services*
> * *A data keeper regularly provides the proof of the fact the data is stored*

The main idea of Filecoin is that system participants who want to store their data or access other data in the system pay for the services using the built-in currency. The task of miners in this case is to ensure the storing of data and their provision upon request for an appropriate fee. Each owner of a miner node personally determines the amount of disk space provided.

The price for storing a fragment of content is determined by supply and demand. Miners send their offers regarding the amount of space provided and the cost of recording. Other users send offers with the amount of coins that they are willing to pay for storing their data (orders). After the offers are matched, participants create a transaction in which the client pays the miner in small parts after a certain period of time. Note that in order to receive a reward, the miner must prove at these time intervals that the content is still stored by him.

### Consensus algorithm in Filecoin

In the Filecoin protocol, the *proof-of-spacetime* consensus algorithm was proposed. In this consensus mechanism, the probability of mining a block by a network participant depends not on the computational power of its node but on the size of the storage that is used to store data (relative to all other network participants). Unlike the classical proof-of-capacity (more details in 5.4), the consensus algorithm in Filecoin implies not storing arbitrary data (useless data, which is necessary only for proving that the storage is full) but the data necessary for users, that is, useful content.

Each network node that is involved in data storage participates in consensus reaching. At the same time, the storage of each miner is publicly available, meaning each network member can get information about the storage space allocated by the miner and record their data there. The storage is also publicly verified: for each segment of the repository, miners must generate proof-of-spacetime to confirm that they really store the relevant data (it is worth noting that the proof does not disclose any information about the stored content - it is a zero-knowledge proof). At any moment, the miner can increase the storage space and provide it for network participants to use it.

### Advantages of Filecoin protocol

The Filecoin protocol is designed in a way that it allows achieving a number of features that are required in a decentralized file sharing system.

> * *Incentivizing data storage and penalizing dishonest system participants*
> * *Each network member can verify that the miner stores certain data (auditability) while not having access to the actual content (zero knowledge)*
> * *Ability to access the data stored over time (retrievability)
> * *Ability to ensure confidentiality of data*
The Filecoin protocol implies rewarding active system participants. First, participants are rewarded for forming blocks. Secondly, reward is paid for the actual storing of useful data.*

***Frequently asked questions***

*– Can the data added to IPFS be deleted and how much time does it take?*

Yes, the data can be deleted, but there are some aspects worth mentioning. Adding and deleting data in a decentralized file sharing system is very different from using a centralized storage system. The peculiarities are that the data is stored by several independent parties, and the data can be deleted only if it has been deleted all data keepers. Most often, data keepers have no reason to store data for which they do not receive money, so the process of deleting data from the network takes up to several minutes or hours. However, if keepers for some reason do not want to delete the data, then the data  will actually remain on the network, and their owner cannot influence its deletion.

*– Can the data added to IPFS be encrypted?*

Yes, the client can encrypt the data before sending it to the network; in fact, data encryption is a common practice on the IPFS network. Nodes do not care in what form and what data is stored. Encrypted data can also be indexed and placed on the network. The only difference is that the ability to access the contents of the stored data is granted only with the appropriate key.

*— Can a private network based on IPFS be deployed?*

Yes, based on IPFS, a private network can be created [14; 15]. To do this, you need your own node that will connect clients to each other (connecting to a public one does not make sense, since in this case, the network will be public) and a swarm key, which will be shared by all network participants. A swarm key is an identifier that allows IPFS nodes to connect to each other. For example, if two nodes try to communicate with each other and one of them has a swarm key, the fact that the other node has a similar key is checked. If the keys are present and coincide, the nodes connect to each other.

[CRYPTOGRAPHY IN DECENTRALIZED SYSTEMS](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-2/en/2-cryptography-in-decentralized-systems.md)
