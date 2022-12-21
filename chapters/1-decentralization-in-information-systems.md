# 1 Decentralization in information systems

Сentralized systems with their top-down management models have a number of disadvantages  For example, any centralized 
social network such as Facebook can censor all actions of its users and block their accounts if the actions turn out 
to be “illegitimate”. Surely, the decision on the legitimacy of these actions are solely made by the governing party.

The non-transparency of processes in such systems leaves little opportunity for consumers to prove the violation of 
their sensitive data privacy. In financial systems, this non-transparency leads to inability to verify whether a user’s 
final balance matches with the completed transactions.

Such a situation (namely, such features of the traditional architecture) allows the system owner to implement any 
modifications at his own discretion (in particular, with backdating). Obviously, decision-making is fully subjective 
in such systems.

## 1.1 What is decentralization?
The process of *decentralization* is opposite to the process of centralization. It implies that the functions of a 
particular system (storage, computation, decision-making, etc.) are maintained with the responsibility being 
distributed between the system participants. This concept is applied not only in the scope of information technologies. 
It has already been used in such areas of public life as politics, management, jurisprudence, economics, etc.

In the data transmission theory, decentralization is understood as creating conditions under which the need for a 
central server is eliminated and network participants have equal rights. The striking example of decentralization is the 
Internet: in this global network, routers operate independently of one another. In such conditions, the breakdown of one 
of the routers is not critical for users, as there is typically more than one data packet delivery route. The higher the 
*decentralization level* of a particular system is, the bigger the maximum number of denied components that the system 
can sustain is.

A *decentralized system* implies the presence of many independent participants who maintain its operation together. Note 
that such an approach requires participants to take coordinated actions for sufficiently effective interaction without 
a central party. Decentralization as a phenomenon has many advantages. In order to understand them, one should 
consider the basic concepts and definitions and refer to the history of decentralized systems development.

### Concept of decentralization for information systems
First, we should figure out what an information system is. According to ISO/IEC 2382:2015 standard [1], an information 
system is a system designed for collecting, organizing, storing and processing information and the relevant 
organizational resources. In order to work, an information system requires a database as well as the technical tools 
with the appropriate software for accessing the database and managing the data.

There are two main attributes that distinguish a centralized information system from a decentralized one. The first is 
that in a decentralized system, all components are mandatorily decentralized. “So what?” one might say, “Suppose that a 
centralized service implies that replicas of its database are stored by all its users. Why can’t you call such a system 
decentralized?”

Actually, you cannot because there is the second important feature. It lies in the fact that decentralization “breaks 
down” the core of a system. All the *processes* that used to be indivisible—governance, identity management, asset 
management, participants’ communication, decision-making, storage and processing of information, audit—can now be 
executed by many participants in parallel and independently of one another.

### Difference between decentralized and redundant systems
Redundancy is often utilized to enhance the reliability of a particular system. It implies the addition of excessive 
(backup) components to the system. Such an approach assumes that if one of the components fails, the system will not 
stop working but will rather switch to a backup component. Good examples of redundancy in real life would be the 
duplication of some of the human organs or the executive deputies on a governing body.

According to the above-mentioned description, we provide the following definition. *A redundant system is a system with 
backup components, which are redundant regarding their minimum necessary number and perform the same functions as do the 
basic components.*

Obviously, decentralization implies redundancy mandatorily, but redundancy does not necessarily imply decentralization. 
The point is that redundancy can also be applied in centralized systems. For this, it is enough to add backups of key 
components to a system (e. g., an extra database which replicates the data stored). In a decentralized environment, 
redundancy is the mandatory consequence of the fact that every participant of a particular system has an independent 
set of components.

Based on the definitions of the two systems, we can point out two primary distinctions. The first one is that components 
of a *decentralized system*, unlike those of a redundant one, must not be controlled by a central authority. The second 
one is that redundancy is mandatory in a decentralized system. In a redundant system, it is only applied to improve 
particular operating characteristics and is not mandatory.

## 1.2 History of decentralized systems
As far back as the 1970s, during the search for a more reliable way of storing digital data, the use of decentralization 
principles drew attention. One of the first projects was Usenet [2]. The basic operational principle of this protocol 
was the data exchange between servers using a special algorithm that additionally ensured the synchronization of the 
nodes’ states. In this way, each server was an updated local copy of any other node of the network. If one server 
failed, the system would continue operating because the data copies were stored on any other server. As compared to the 
centralized alternatives available at that time, the Usenet approach made it possible to improve the reliability of data 
storage.

The idea implemented in Usenet protocol indicated the new approach to data storage and synchronization. Naturally, it 
also formed the basis for future attempts to implement a reliable method of data exchange in a decentralized 
environment.

Around that time, the File Transfer Protocol (FTP) was proposed [3]. It allowed users to transfer files to one another 
independently and propelled the emergence of decentralized file-sharing networks and messaging protocols. These began to 
actively develop later in the 1990s and included examples such as Topsites, IRC, Napster, etc.

In the early 1980s, the world saw rapid development of network control protocols TCP/IP [4], which led to the appearance 
of the Internet as we know it today. This fact was a revolution in the world of information, with computers becoming 
able to connect to the global digital space. Business greatly benefited from the transition of processes to a digital 
form, while even governments, for the most part, supported this innovation.

The Internet has become a free network for information sharing and a great example for other areas which have started 
applying the principles of decentralization of data search and processing. Now we refer to them as *open APIs and 
sharing economy* [5]. Their main principles are the direct user interaction and shared use of resources, services, 
content, devices, etc. Further, we will consider how such principles have been applied in accounting systems of 
different types.

### Decentralized file-sharing systems
For file-sharing systems and data storage systems, the decentralized approach implies that different network nodes store 
different file fragments (Fig. 1.1).

![Figure 1.1 - A scheme for distribution of file fragments among](/resources/img/chapter-1/1.2-history-of-decentralized-systems/image1.png)

Decentralized applications started to be developed rapidly especially with the invention of services and protocols for 
file sharing (Fig. 1.2).

![Figure 1.2 - Scheme of file sharing in decentralized file-sharing systems](/resources/img/chapter-1/1.2-history-of-decentralized-systems/image3.png)

One of the first such services, Napster, provided a way to exchange MP3 files. At that time, recorded music had been 
mostly available on tapes and disks (of course, it required payment). Therefore, Napster became very popular among 
Internet users as it was free and more convenient. Yet, although users interacted on the *peer-to-peer (p2p)* principle, 
the database with the latest version of files was nevertheless stored on a centralized server. This was subsequently 
discovered to be a weakness, which led to the service being shut down following legal pressure from authorities and 
regulators.

Subsequently, other file-sharing networks appeared such as Gnutella, eDonkey2000, DC++, and I2HUB. Further, however, 
most of them came under attack as regulators were able to influence the organizations that stood behind a particular 
service. In 2005, MetaMachine, which had been developing and supporting eDonkey2000, received a dedicated letter from 
the Recording Industry Association of America (RIAA) with a demand to terminate its file-sharing network. As a result, 
activities carried out with eDonkey2000 could not be considered legal, and the company could have borne serious 
responsibility if it had not shut down the service.

The eDonkey2000 project had to be closed. However, this had demonstrated to the world how business and economic 
relations could be changed. The banning and closure of the p2p projects led to the emergence of new projects that began 
developing decentralized models and creating new possibilities for data management: availability, reliability of 
storage, fault-tolerant operation, etc.

In 2001, BitTorrent, a communication protocol for p2p file-sharing, was introduced. It worked quickly and efficiently 
and was not only fault-tolerant but also independent. Initially, a centralized client software was required, but later 
sophisticated torrent clients appeared, while using VPNs (Virtual Private Networks) increased the level of user 
anonymity. BitTorrent is fairly one of the symbols of the decentralized approach, which has successfully worked so far.

In addition to BitTorrent, there are also many alternative solutions for decentralized data storing. One of them is the 
distributed storage IPFS [11]. Like in BitTorrent, the essence of IPFS’s operation is that users do not download files 
from centralized servers but share them with each other. In fact, it is quite similar to the concept of the World-Wide 
Web: in the system, every file is assigned a unique identifier, which is a hash value of it (for further details, see 
3.1), and Git (version control system) is used to trace the history of each file. Such an approach provides the ability 
to have access to the most relevant version of the content and also ensures its authenticity. A file can be searched 
both by an identifier and by human-readable names, which is performed using the decentralized name system IPNS.

### Decentralized data transmission systems
The more actively people have been using the global network, the more pressing the need for privacy has become. In early 
2002, a project called Tor (The Onion Router) [6] was launched. It is a system of proxy servers that allows setting up 
an anonymous network connection protected from having the data transmission traced. Its implementation allowed users 
from around the world to bypass the traffic blocking of local providers and access data while maintaining privacy [7].

Technologies based on decentralization principles received a strong impulse in their development in particular areas. 
For example, in 2004, the first projects using wireless mesh networks in South Africa were launched. The principle of 
their work is that users themselves maintain data transmission channels and perform network packet routing. In such 
networks, nodes “listen” to each other and if a particular node fails, the ones which were connected to the failed node 
seek for alternative nodes to reconnect. This method of organizing network interaction [8] made the Internet more 
accessible in regions where, for various reasons, centralized providers did not deploy their equipment.

### Decentralized decision-making systems
Another interesting application of decentralization are the systems where decision-making is decentralized. Imagine that 
five people put a cake into a shared safe and agreed to eat it only if the majority agrees. Therefore, they split the 
secret of the code lock and distribute its parts between each other. The cake can only be eaten if the majority of 
participants decides to share their part of the secret and unlock the safe (Fig. 1.3). 

![Figure 1.3 - Decision-making by independent parties](/resources/img/chapter-1/1.2-history-of-decentralized-systems/image4.png)

Nowadays major firms cannot be managed by one person, and even if they do exist, their effectiveness is quite doubtful. 
For this reason, almost any large company in practice has a board of directors, a countless number of advisors, and 
sometimes even opportunity for employees to participate in decision-making. This approach increases the effectiveness of 
a company through the cooperation of people with different views, different methods for evaluating problems, and 
different approaches to solving them. Thus, the final decision can be considered more objective.

In this way, as the two-way communication between the top management and those in the middle and lower levels grows, the 
performance of an organization typically increases. Nowadays more organizations are turning from the hierarchical 
management model to a more decentralized one, which continues to gain traction.

It is also important not to confuse decentralized systems with parallel computing systems. In both systems there are 
many computers connected in one network. But in the first case, the computational problem is solved by each computer 
anew to achieve independence of the calculation results, and in the second case, the task is divided into subtasks and 
each computer solves its part to reduce the time for obtaining results.

Grid systems use distributed computational resources to reach a common goal (Fig. 1.4). The number of nodes in such a 
system can fluctuate from several machines to hundreds and thousands of workstations.

![Figure 1.4 - Scheme for transferring results of calculations in parallel computing systems](/resources/img/chapter-1/1.2-history-of-decentralized-systems/image2.png)

Such an approach was first suggested in 1999 in the publication “The Grid: Blueprint for a new computing infrastructure” 
[9]. That same year the first grid project, SETI@home [10], was launched. Today there are a huge number of similar 
projects such as BOINC, Folding@home, Einstein@Home, etc. Note that the above-mentioned SETI@home project has so far 
been one of the most powerful distributed supercomputers.

### Decentralized payment systems
The next step happened to be the decentralization of payment systems. They were much harder to decentralize for a clear 
reason: people are ultimately anxious about everything directly related to the safety of their money. 

Due to the invention of *money*, the value of private property became much easier to estimate (ability to objectively 
estimate value is one of the fundamental features of money). Money has historically been tied to commodities such as 
gold, cows, furs, shells, cigarettes, etc. The wealth of an individual was based on how much commodity belonged to him. 

Nowadays money has become digital; they have actually become numbers in a database which allow evaluating each others’ 
abilities, power, and status. The problem with this kind of money (so-called *fiat money*) is their non-transparent 
issuance: this process may be performed based on decisions of particular people and not on the overall consensus. Note 
that achieving an overall consensus regarding monetary policy is fairly not a trivial problem.

With the appearance of Bitcoin (the accounting system) in 2009, bitcoin (the base currency) became the first *scarce 
digital asset*. Because of this, the idea to separate money from a state or banks became popular. A similar thing 
happened centuries ago in some of the developed countries with regard to religion and press when they became free 
(almost…) from the state. Nevertheless, the concept of *single national currency* is still explicitly stated in the 
constitutions of many states.

The major question is as follows: is it possible to create an efficient monetary system with initially unlimited 
issuance which is able to simulate the operation of fiat money but is not influenced by particular people? What we can 
say for sure is that the possibility to create digital scarcity (issuance limited with mathematics) and programmable 
rules of work (a constitution guaranteed by cryptography) is revolutionary for many aspects of life and will keep on 
developing and evolving. For this reason, understanding the principles of Bitcoin operation is essential to get yourself 
ready for the new digital world.

In the first volume of the book, we analyze the operating features of a decentralized accounting system with primary 
focus on Bitcoin, since many of the principles which guided its design and implementation are fundamental to any 
decentralized system.

## 1.3 Applying the principles of decentralization
In this subsection, we focus on and formalize the principles of decentralization and specific features of their 
application. These principles have been applied to create well-known and successful applications including data storage 
systems (IPFS), computing systems for load distribution (the BOINC project), mesh networks (Open Garden), 
telecommunications (Tor, I2P), messengers (Tox, BitMessage), and content distribution networks (BitTorrent) as well as 
cryptocurrencies.

### Limitations and issues of centralized systems
The essence of centralized systems is that the management of processes is centralized. In many cases, such an approach 
can provide a system with high efficiency. One of the striking examples is that the protocol rules can be updated simply 
and fast. Suppose a vulnerability is detected in a centralized accounting system, whereas developers discover the source 
of the issue and release an appropriate update. Eventually, all the users who wish to continue using a system have to 
update their software. In this case, the entire process of updating occurs fast and as painless as possible.

> **Operation of traditional payment systems**
> * Database is stored on the main server (cloud or cluster)
> * System is managed in a centralized way 
> * Users send requests to the system for conducting transactions

However, there are certain risks that must be considered when operating in such a system. One of them is the probability 
of complete system failure. In certain situations (turn-off of the primary power source, negligence of staff maintaining 
a system), key components of a system can fail and lead to an overall loss of its operability. The application of the 
redundancy principle could help but not in all cases.

Security is a key component of any system. Regardless of the security measures, centralized systems are yet more 
vulnerable—they are potentially more exposed to various attacks. Decentralized systems, however, have their key 
components spread physically and usually with a different software. It provides a higher level of protection from 
targeted attacks, which are always aimed at a single point.

Traditional financial systems work on the principle that the internal accounting system is secured by means of brute 
force—the protection is only at the perimeter [12]. If you overcome external protection, the database becomes 
vulnerable. This is where the drawbacks lie. Additionally, this approach does not allow users to personally verify their 
data. A user can only send a request to the server that will process it and send back a response (Fig. 1.5).

[Picture 1.5] - Traditional systems protection principle

For a regular user, the internal structure of such a system is non-transparent, and there are no guarantees of its work 
as expected. Also, centralized systems have a single point of failure (Figure 1.5). On the plus side, however, there's 
always an entity that is responsible in case of failure or service denial.

If there is a need to verify the correctness of the final database state, you would have to do it manually or using some 
automation tools; it is very likely that this check will be performed by an auditor. Until 2009 all accounting systems 
had been operating by this principle.

Another disadvantage is the possibility to censor user actions in the system. A centralized system implies complete and 
unconditional trust by its users to the owner and the managers of the system. The issue with such an approach is that 
the owner of a system is able to violate the protocol rules at any moment.

### Application of a decentralized approach
Historically, people have been designing systems that simulate a familiar model of social relations, and therefore most 
of the systems we use are based on the hierarchical principle. There is a considerable number of areas where the 
hierarchical model is well-suited, yet this approach is also related to a number of potential problems.

> **Problems that decentralization solves** 
>> * Possibility of censorship
>> * Presence of a single point of failure
>> * Necessity to trust the owner of a system

> **_NOTE:_** *Censorship in the context of an accounting system means that a particular party (e.g., organization that 
> maintains the system) is able to perform activities such as blocking user accounts and modifying transaction data at 
> its own discretion.*

Generally, decentralization is possible when it comes to the interaction of parties with equal rights. Over the last 
twenty years, there has been active implementation of systems where the interaction of participants is peer-to-peer 
(p2p). It has been made possible to perform a number of processes in a decentralized way while maintaining a sufficient 
level of automation. However, the social aspect of decentralization is much more complex than the technological one, 
which results in a relatively low adoption rate.

### Principles of designing decentralized systems
We will list the main aspects of decentralization associated with new technological and organizational risks in order to 
analyze the features and challenges in designing, developing, and using decentralized systems.

> **Basic principles of decentralization**
>> * Ultimate increase in the level of independence of each system component
>> * Maintaining the balance between performance and resources applied
>> * Preserving the integrity of the entire system [13]

> **_NOTE:_** *Further in the text, we will mostly use the term "data" when referring to a sequence of bytes that can be 
> transmitted, stored, and processed in automated systems and the term "information" when referring to some knowledge 
> that people exchange with each other or remember.*

First of all, let’s focus on the ways users interact within systems. In Figure 1.6, you can see three approaches, which 
fundamentally differ in their design [14].

[Picture 1.6] - Types of systems based on how users interact

Popular social networks are a good example of *centralized* systems. Their main disadvantage is that any issue in their 
databases will affect all the users.

The global banking system may be viewed as *decentralized* since it consists of a number of local centers. For instance, 
if something goes wrong in a bank in Argentina, this will not lead to delays in the payments of banks in Germany.

*Distributed* systems include mesh networks and communication systems based on hand-held radios, where the system is 
supported only by the users' devices, and disabling one device will not affect other users.

> **_NOTE:_** *We will use the general term, a decentralized system, to describe the last two types of systems 
> (decentralized and distributed) since both cases imply the absence of a central node.*

An *accounting system* is a particular case of an automated data processing system which manages a ledger and the 
decision-making process about its updating (Fig. 1.7).

[Picture 1.7] - Accounting system architecture

A *centralized accounting system* includes a database, mechanisms for updating it, and a management center responsible for 
maintaining its operation. A management center may not always provide a sufficient level of security of data processing 
and reliability of decision-making.

In the search for better solutions, the world has come up with an idea to decentralize accounting systems.

### Base architecture of decentralized systems
A *decentralized* system is a particular case of a system which uses the *mechanism for reaching consensus* (agreement) 
between independent parties on how to process and update a shared database (ledger) (Fig. 1.8). All parties have their 
own copies of a database, while the *consensus-reaching algorithm* ensures that all copies are securely synchronized. 
Instead of a single authorized party (*management center*), multiple independent parties (*validators*) take part in 
matching the final state of all copies.

[Picture 1.8] - Decentralized accounting system architecture

### Advantages of decentralized systems
In general, a decentralized system assumes that participants exchange information directly, using p2p protocols, and 
independently store the required data. To do this, they run special software that supports the required p2p protocol of 
a particular decentralized system. In decentralized systems such as cryptocurrencies, all participants are assumed 
(ideally) to store copies of the same database and update them using the consensus algorithms.

> * Fault tolerance 
> * Independence from individual control 
> * Trustless (no need to trust a third party)
> * Permissionless (free use)
> * Persistence (immutability of the final state of a database)
> * Liveness (appending records is guaranteed)
> * Formality of protocols 

Let's start with *fault tolerance*. This feature means that a system remains operable even after one or several its 
components fail. In the traditional client-server architecture, the main efforts are directed at protecting a central 
server. If an attacker takes over the server and gains access to a database, he will be able to rewrite the database for 
his own purposes. Though, an attacker cannot critically harm a decentralized system by taking over one server and 
gaining access to its database.

The principles of decentralization have been successfully used for many years in aviation and space technology as well 
as in the operation of high-precision computing systems. When working in unstable conditions, it is very important that 
the basic systems (e.g. navigation, communications, controls, etc.) work reliably; in such cases, builders of the system 
apply *redundancy*. 

*Independence from individual control* is another distinguishing feature of a decentralized system. If there are a large 
number of independent participants, then no one, not even a very interested party, can influence the decision-making in 
their own favor.

The *trustless* property implies that there is no need to trust the system concerning the aspects of storing and 
processing data. Trustlessness can be achieved through a large number of independent participants that make some joint 
decisions. As the number of independent participants (who are strangers to each other) increases, the probability that 
they collude with each other reduces.

The *permissionless* property means that the system is available for anyone without any additional permissions (no 
hierarchy of permissioned roles). Anyone can read and write data, perform an audit and participate in decision-making. 
However, the permissionless property may also be a disadvantage in certain cases. For example, in cases when decisions 
must be taken by particular experts instead of the public, or when validators are involved in the processing of 
confidential or sensitive data.

*Persistence* allows the system to maintain the final state of its database immutable (even if the entire network fails 
its operation for a moment).

*Liveness* is a property which ensures that appending of records will definitely take place if all honest participants 
want to perform it.

*Formality* of protocols presumes that all participants come up with the same decision if they strictly follow a 
specified algorithm.

> **_NOTE:_** *The advantages of decentralized systems depend on their decentralization level.*

### Limitations of decentralized systems

Along with obvious advantages, which, in fact, depend on the level of decentralization of a system, decentralized 
systems have their limitations.

First of all is the absence of any kind of support service. This means that if a transaction is accidentally sent to the 
network, there will be no one to ask for the refund.

The second limitation is the increased cost of maintaining the system. In a number of systems, their databases only grow 
over time, so their participants have to dedicate more and more resources for data storing and processing.

The third limitation is that some functions, which are available in centralized systems, are hard to implement on 
decentralized platforms. Some examples are getting statistics, or the system state report at a specific point in time.

### Factors slowing down the implementation of decentralized systems
The reliability and stability of the system grow with its level of decentralization, however, the very process of 
decentralization usually meets the following difficulties and limitations.

> * Difficulties in updating the protocol 
> * Responsibility issue 
> * Difficulties in monetization of development 
> * High hardware requirements 
> * Scalability issue

*Difficulties in updating the protocol*. The primary difficulty is that the proposed protocol update requires the 
majority of active users (nodes) to support it (Fig. 1.9). For this, a party proposing an update should prove the 
importance of its implementation to others—which is by itself a difficult task. For example, the transition from IPv4 to 
IPv6 required updates on all devices from all manufacturers, which took much more time than the developers planned. In 
this case, the benefit for users is not noticed immediately.

[Picture 1.9] - Protocol update issue 

*Responsibility issue*. Decisions in decentralized systems are the result of consent by the majority of participants. 
There is no single participant who could cancel a decision that was taken mutually by all participants and impose their 
own. However, if a user becomes a victim of scammers (due to his own carelessness), there is no one to complain to, no 
responsible party, and no guarantees (Fig. 1.10). Here, we are talking about taking risks which are very difficult to 
measure.

[Picture 1.10] - Responsibility issue 

*Difficulties in monetization of development*. It is generally easier to monetize a centralized rather than a 
decentralized system. This is because centralized systems are more manageable and can use legal enforcement since there 
is a designated representative to appear in court. In such circumstances, it is easier to build a business model and 
monetize the project. For example, in decentralized systems, it is quite difficult to introduce effective filtering and 
ensure copyright protection. Therefore, it is difficult for protocol developers of decentralized systems to make a 
profit from their projects.

*Scalability issue*. If the case is about a distributed decision-making mechanism, all participants are required to 
communicate and reach agreement. The capacity of the system decreases with the growth of a number of validators in it 
(Fig. 1.11).

[Picture 1.11] - Example of how capacity decreases as the number of users grows

Decentralized systems also have to *store redundant data amounts and to meet high hardware requirements*. 
These limitations will be considered in detail further (see 5.3).

There are *other issues* that can harm the distribution of decentralized software. For example, network traffic can 
easily be filtered by an Internet service provider. Since a significant amount of software is distributed through 
centralized services such as Microsoft, App Store, Google Play, GitHub etc., they can implement censorship based on 
their own decision (Fig. 1.12). Therefore, applications may be rejected so much as deleted (even in quite a long time 
after the successful release).

[Picture 1.12] - Provider services capable of censoring software for operating in decentralized systems

### Summary
Comparing decentralization with centralization is not always a good idea since everything depends on purpose, 
requirements, and operating conditions.

A centralized system has a number of useful properties: it is easier to manage and usually operated by a known, 
responsible entity; decisions are generally made quickly, so you can build almost any particular business model and 
stick to it, which gives you an advantage when monetizing.

The main goal of decentralization is to implement properties that allow users to interact with each other effectively 
and reliably in a situation where they don’t trust some central party or intermediary. Cryptographic algorithms, 
protocols for mutual decision-making, and blockchain technology have become the building bricks for a truly transparent, 
secure, and sufficiently effective decentralized accounting system.

**Common myths**

*The amount of resources required for the maintenance of a decentralized system constantly increases over time.*

The required resource amount increases only if a decentralized system must preserve the ability to audit the entire 
change history of its shared database. In such a case, what will definitely increase are only disk space requirements, 
while computational power requirements may remain unchanged.