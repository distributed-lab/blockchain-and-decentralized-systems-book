# 2 History and operational principles of Bitcoin

## 2.1 What is Bitcoin?
Essentially, Bitcoin is a protocol that implements an independent currency and payment system. Other digital money or 
payment systems (e.g., WebMoney and PayPal) operate with existing (*fiat*) currencies controlled by government entities 
(e.g., dollar, euro, pound). Bitcoin is both a separate currency and a payment system that manages its own currency. 
Note that it is an independent payment system: there is no central entity that controls its operation.

> **_NOTE:_** *We will use Bitcoin to refer to the accounting system or the protocol and bitcoin to refer to the 
> currency or a coin.*

When people try to understand Bitcoin, they often turn to Wikipedia, which defines it as a peer-to-peer payment system 
that uses its own coins as value units, and cryptographic methods are used to ensure the operation and protection of the 
system. For a person who doesn't have any experience with payment systems, this definition is complex and 
incomplete—especially if they're hearing about Bitcoin for the first time. Therefore it is necessary to define Bitcoin 
with more familiar words. The best analogy that appears in publications on this subject is the analogy with "digital 
gold" (Fig. 2.1).

[Figure 2.1] - Bitcoin is a digital gold analog

It is fair to assume that the creator of Bitcoin actually aimed to reproduce all the properties of gold such as a 
limited supply, mining, independence from a single organization, and the impossibility of artificial reproduction. The 
fact that Bitcoin reproduces the qualities of a naturally occurring mineral in a digital form can be considered a major 
breakthrough in computer science.

Using Bitcoin, one can make a payment to anyone and anywhere. All that is required is access to the Internet, a digital 
wallet, and a receiver's address. Common disadvantages for international money transfers are simply absent. No 
additional permissions are required to make a payment.

Therefore Bitcoin became well known and well regarded in communities with people who held firm beliefs in anarchism, 
liberal ideas, and complete privacy; later computer geeks (cypherpunks, hackers) as well as scientists—who did research 
and conducted experiments on Bitcoin—have joined them. Soon the price of bitcoin attracted the attention of 
entrepreneurs and speculative investors. Finally, the key motivation for using Bitcoin across the board was the ability 
for regular people to conduct permissionless payments without having third parties involved and without the need to 
register.

Payment systems such as PayPal or WebMoney are managed by companies which can be exposed to hackers or government 
demands, leading to information leaks, server shutdowns, and the freezing of accounts. This is not the case with 
Bitcoin: there is no, say, Bitcoin LLC which can freeze accounts or individually confirm transactions. Bitcoin 
transactions are not censored. This property is very important, and we will later discuss how it is achieved.

### History of Bitcoin creation
We do not know who invented Bitcoin, though it is known that it was someone with the pseudonym Satoshi Nakamoto. Perhaps 
this is an individual, but there are suggestions that it could be a group of people. Satoshi registered the bitcoin.org 
domain in 2008, released the first article, and published the initial version of the source code of the protocol. Until 
2011, messages under the pseudonym of Satoshi Nakamoto appeared on forums and in email lists. Later he wrote that he had 
decided to focus on other things and ceased public communication.

Nevertheless, it is worth noting that Satoshi's idea of creating Bitcoin didn't come out of nowhere. In his paper 
"Bitcoin: A Peer-to-Peer Electronic Cash System" [15] (Fig. 2.2), Satoshi mentioned two earlier key projects (previous 
attempts to create an independent digital currency): Adam Back's HashCash [16] and Wei Dai's B-Money [17]. HashCash 
first introduced the proof-of-work approach, originally created to combat spam in emails, while B-Money introduced a 
network model for distributing transaction data, plus the use of cryptographic signatures to send money. 

[Figure 2.2] - Fragment of the first publication about Bitcoin

Thus, Satoshi used existing concepts to implement Bitcoin and solved the issues of earlier attempts to create digital 
money. It is fair to assume that not many would have recognized an embryonic formation of an independent digital 
currency from the above-mentioned projects.

> **Background to the invention of Bitcoin**
>> * David Chaum—DigiCash [18] (1989)
>> * Adam Back—HashCash (1997)
>> * Nick Szabo—BitGold [19] (1998)
>> * Wei Dai—B-Money (1998)
>> * Hal Finney—RPoW as e-money [20] (2004)

Bitcoin is the first successful implementation of a decentralized accounting system. There were other attempts before 
it: for example, the company DigiCash, founded by David Chaum in 1989, was the first to offer digital payments with a 
special currency called Ecash. Users of this payment system were anonymous and banks could not track their accounts. 
The project was supported by several innovative banks in the US and one in Finland; since it was very difficult to 
convince more banks and merchants to accept anonymous currency, the project was canceled.

> **Key dates**
>> * Bitcoin whitepaper release—October 31, 2008
>> * Genesis block creation—January 3, 2009
>> * First bitcoin exchange—February 2010
>> * P2SH adoption—January 5, 2012
>> * Bitcoin Cash fork—August 1, 2017
>> * Segregated Witness adoption—August 24, 2017
>> * Lightning Network launch—March 15, 2018

One of the first steps in the development of Bitcoin was the publication of the Bitcoin White Paper on October 31, 2008. 
On January 3, 2009, the first source code was published and the first block (the genesis block) was created—the 
first 50 bitcoins appeared. This day can, therefore, be considered as the launch date of the system. Just over a 
year later, came the first exchange of bitcoins for fiat currency.

Bitcoin Cash fork, which took place on August 1, 2017, was the first splitting of the primary system (for further 
details, see section 6.1). Another important event was the introduction of a crucial update for Bitcoin, Segregated 
Witness, which has implemented certain improvements which opened up a number of formerly impossible opportunities for 
Bitcoin (for further details, see section 4.6). And on March 15, 2018, the revolutionary and transforming instant 
payment system on top of Bitcoin, Lightning Network, was launched.

> **Statistics of the Bitcoin network in 2018**
>> * Estimated 10 million unique users
>> * On average, 300,000 payments in bitcoins per day
>> * Daily trading volume equivalent to $15 billion

In 2018, Bitcoin is used by millions of people worldwide and hundreds of companies accept bitcoins as a payment method. 
Many projects are being developed on the basis of Bitcoin, and some countries (most notably Japan) recognize and 
acknowledge bitcoin as a legal means of payment.

### Issues that Bitcoin can solve
The first reason Bitcoin became popular is that it implements a decentralized financial system independent of 
governments and banks. It is not subject to inflation in a traditional sense of this term (there is no entity or a 
particular person who can print more Bitcoins on their own). It does not allow freezing, reverting, or canceling 
transactions, and it relies only on mathematics and not on the whims of people. In addition, bitcoin payments are more 
private compared to digital currencies that existed before.

> * Financial instrument that operates without restrictions
> * No complex registration processes
> * Can be used anonymously
> * Resistant to censorship

These properties attract a different group of users. For example, businesses can be sure that when a buyer doesn't like 
the purchased goods, she cannot just automatically demand a refund. Another issue which became very painful for PayPal 
business users was when their accounts and tens of thousands of dollars were blocked for several months for such reasons 
as "A complaint was filed against you". This is impossible in Bitcoin, and it is the reason why it became attractive for 
business.

The non-transparency of the existing financial system is another argument in favor of Bitcoin. History provides numerous 
cases when governments have issued money without good reason, which led to hyperinflation and a drop in the standard of 
living. This happened in the 1990s in the countries of the former Soviet Union, where the personal wealth of citizens 
was devalued to virtually nothing due to reasons beyond their control. Such events clearly explain why people prefer to 
manage their finances independently of governments or banks, where possible.

In fact, the very first block of transactions in Bitcoin included links to news which described the printing of a large 
amount of cash as an attempt to deal with the 2008 financial crisis [21]. Non-transparent payment systems caused 
the dissatisfaction of users during the crisis period. And that is where the conspiracy theory lies: its supporters 
argue that banks do whatever they want with their clients' money (this actually takes place in some countries). In 
practice, this happens more "decently": the bank takes fiat money from the client and, in return, gives an obligation to 
return it at the client's request; a little later the bank may refuse its obligations. The essence of the problem is 
that for the client proving this refusal is wrong and returning the money is a very complex task.

> **Issues of existing payment systems**
>> * Users do not control their funds
>> * Fund transfers may take days or weeks to settle
>> * Payments can be canceled or voided

It turned out to be very difficult to implement a system in which people could independently manage their own money. 
However, over time, the demand for this has been increasing, and in 2009, a real solution was presented in the form of 
Bitcoin. It did not take long for this to capture the attention of the public.

### Main principles of Bitcoin operation
A concise formulation of the basic principle on which Bitcoin functions would sound something like this: *each user runs 
the same version of the software which has the same state, and, as a result, they maintain a decentralized system*.

> * Copy of the database is stored by all users
> * Users process transactions themselves
> * All users follow the same rules
> * Users only trust what they can verify themselves

Connection and disconnection of participants occur independently and do not affect the work of other network members. 
Everyone keeps and updates the database and the corresponding software independently. All the rules of work are stored 
by everyone, and all users double-check each other. The Bitcoin network of users can be represented as in Figure 2.3.

[Figure 2.3] - Network of Bitcoin users exemplified

A particular user may process any network transactions on her own. By using the software, she may confirm a particular 
transaction which she considers correct. As a result, each of the users has the set of transactions which he has 
verified and determined as correct. Based on this, each network node builds the shared database consisting of the 
transactions agreed on by the majority of users. As soon as the transaction enters this database, it is considered to be 
confirmed.

### Issuance in Bitcoin
*Issuance* (in the context of money) *is the process of putting cash or non-cash money into circulation*. Issuance in 
Bitcoin is different from that in traditional financial systems. Instead, it occurs through the creation of coins by an 
algorithm. The basic principle is that the entire process is completely decentralized and is based on mathematical 
algorithms which are publicly published and cannot be manipulated. This is how absolute transparency in the distribution 
of coins is achieved.

> * The process of issuance is decentralized
> * Issuance rules are controlled mathematically
> * Issuance is performed by the users themselves
> * New coins are distributed among active participants

According to the protocol rules, any user can perform issuance, while all the network members verify the compliance with 
these rules. In short, new bitcoins are distributed among the users who are actively involved in maintaining the 
accounting system. They are called validators. This means that there can be a lot of participants in the system and each 
of them can receive new coins.

> **Rules for the issuance of bitcoins**
>> * New coins appear on average every 10 minutes
>> * Every 4 years the number of new coins is reduced by half
>> * Absolute total number of bitcoins which will ever be issued is 20,999,999.9769

The issuance of coins is determined by the rules that are established in the software of the network *node* (a computer 
that runs Bitcoin software). Satoshi established that new bitcoins appear on the average every 10 minutes. At first, it 
was 50 coins, and since then the number of new coins gets halved approximately every 4 years. At the moment, more than 
10 years have passed since the launch date of Bitcoin—and the rate of issuance of coins has significantly decreased.

In Bitcoin, it is programmed that the maximum number of coins is about 21 million. Why 21 million? No one knows the 
exact reason, and it really does not matter; this is what was determined by Satoshi. Each bitcoin can be divided into 
100,000,000 parts, the smallest unit being called satoshi (by the pseudonym of the protocol's creator). Therefore, you 
do not have to have a whole coin: you can own a fraction of one.

***1 BTC = 100,000,000 satoshis***

According to the calculations, the absolute number of 21,000,000 bitcoins will be reached in approximately 2140. The 
precise number will be a little bit less than 21 million bitcoin or 2,099,999,997,690,000 satoshis.

Now, let's see how the number of issued coins changes over time. The graph in Figure 2.4 shows the change in the number 
of coins in existence during a certain time. We can see that it resembles a logarithmic curve. In 2018, the number of 
mined coins exceeded 17 million BTC.

[Figure 2.4] - Graph of the change in the amount of issued bitcoins over time

> **Properties of bitcoin coins**
>> * Scarcity (limited amount)
>> * Discreteness (coins are divisible)
>> * Censorship resistance (independence from any company or state)
>> * Immutability (inability to forge coins)

Bitcoin allows paying any amount to anyone for anything to any place in the world. In addition, bitcoin as a currency 
has a number of properties that make it suitable as a means of payment, which at some point can also become a store of 
wealth.

### How is the bitcoin price established?
We believe that the primary reason why many people’s interest is actively drawn by cryptocurrencies is the increase in 
their price. One of the first questions is how the price is actually determined. The price of bitcoin is determined 
strictly by the law of supply and demand between buyers and sellers. There is no entity that decides that today the 
price of bitcoin will be X and tomorrow it will be Y.

For the most part, the price is established on exchanges which allow people to buy or sell coins using an order book, as 
they do for stocks and other financial instruments. Globally, there are many different trading platforms with different 
users and order books. Therefore, the price of bitcoin can be different across exchanges because it is determined by the 
demand and supply ratio created by users of a specific site (Fig. 2.5).

[Figure 2.5] - Demand and supply ratio for bitcoins

In Figure 2.6, there is the graph of the price of one bitcoin (1 BTC) from the launch date of Bitcoin, namely from 2009 
to 2018 [22].

[Figure 2.6] - Graph of the change in value of a bitcoin over time

In the early days of Bitcoin, the price of bitcoin did not use to be determined, as there used to be no public 
exchanges. Later people started exchanging bitcoins for other currencies in a certain ratio. The first purchase of a 
real product (two pizzas) for bitcoins was made in 2010 [23] at the rate of a few cents per bitcoin. The price began to 
grow, and in 2011 it reached an early peak of $33 but then proceeded to fall to $2. The record price in 2014 was around 
$1,100. After another fall to $200, the price was comparatively stable throughout 2015, then began to grow gradually. 
In 2017, the period of its exponential increase began, and the price reached $20,000.

Concerning the establishment of the price, it is fair to note that bitcoins which cannot be accessed or which are not 
used for some reason can play a significant role in the overall picture. For example, the fact that by November 2018 
about 3 to 4 million bitcoins had been blocked due to key losses has a major influence on the price [24]. It is known 
that Satoshi has about a million bitcoins, none of which have ever been spent. If evidence were to come to light that 
the private keys have been destroyed and the corresponding coins are out of circulation, the price of bitcoin could have 
changed significantly.

We cannot say for sure whether Satoshi has blocked the possibility of spending his bitcoins. If he still has access to 
those bitcoins, then he can use them, and, if keys have been lost, it is possible that someone else might find and use 
them. Only Satoshi can prove or disprove it. It remains possible that he may appear tomorrow and declare that he will 
control the price of bitcoin using his vast supply, similar to the principles used by central banks to manage the supply 
and value of fiat currencies.

Noteworthy, the price of bitcoin is inessential in some use cases. For instance, if someone needs to transfer money from 
Ukraine to China, she can use Bitcoin as a transitional “layer”—buy bitcoins in Kyiv for hryvnias (the local currency) 
and sell them in Shanghai for yuans. Here, it does not matter whether 1 BTC costs $200 or $10,000.

> **_NOTE:_** *It is not always viable to use bitcoins as a payment/settlement method for real business (mostly due to 
> the price volatility), but they can be very helpful in the context of international transactions.*

The advantage of Bitcoin is the irreversibility of payments and the ability to “program” behavior. These features are 
crucial in situations when buyers do not trust sellers. For example, take online auctions: the seller is unwilling to 
ship the item to the buyer before the buyer’s payment, and on the other hand, the buyer is reluctant to send the money 
before having his goods shipped.

A simple solution for them can be that instead of paying the full amount using a bank transfer, a buyer makes a partial 
prepayment in bitcoins (which is instant and supports trustless escrow, see 4.5) and afterward sends the remaining money 
via bank transfer. Once the seller has received the prepayment in bitcoins (as an option, it may cover the shipping 
costs), he ships the item. After the item arrives and the buyer makes the bank transfer, the seller returns the 
prepayment.

In the example above, neither the bitcoin price nor its volatility is an issue. Ultimately, if you imagine a world where 
many cryptocurrencies provide the same level of *censorship resistance* and *transactions irreversibility*, then their price 
gets irrelevant for people who use them.

> **_NOTE:_** *Censorship resistance stems from true decentralization, and it is what creates real demand for bitcoin as 
> an asset.*
> 
> *It is very easy to copy the Bitcoin codebase and create another cryptocurrency that would also be limited in the 
> issuance or could even have a higher level of anonymity. However, it won’t necessarily be as valuable as Bitcoin 
> because the only distinguishing factor is the level of censorship resistance and not the amount of coins in existence 
> or some additional functionality.*

### Concept of trust in Bitcoin
The key feature of Bitcoin is the trustless interaction of all its users. Every single person can verify that the final 
state of the system is reached with no violations of the protocol rules. Trust between participants of the system is 
excluded.

*Trust only to mathematics* is one of the key advances in a cryptocurrency. We will repeat this statement more than once 
in this book. Nevertheless, it is not comprehensive, and things are slightly more complicated in practice.

Regarding the interaction of a specific user with the Bitcoin network, a number of aspects should be considered which 
make the user trust somebody. For example, when you trust that your bitcoin wallet balance is correct, this means that 
you trust:
* Those who recommended you to install this particular wallet (friends, Internet forums, etc.)
* That the wallet you use is developed and implemented correctly 
* That your operating system is not hacked 
* That hardware of your computer is not hacked 
* That your wallet is connected to authentic Bitcoin nodes (you are not connected to malicious nodes)
* That the implementation of a cryptographic library has no backdoors 
* That the cryptographic algorithm has no built-in backdoors

All the above-mentioned is not imaginary (there are actually much more possible threats). Behind each of the items, 
there is a real case that at some point in time happened to somebody. This shows that concepts such as *absolute 
security* and *full trustlessness* in a system are only theoretical. Decentralization is just increasing the probability 
that the system is not compromised. In the context of Bitcoin, this means that every user can independently verify the 
correctness of any transaction or block—he does not have to trust the data provided by a third party.

### Limitations of Bitcoin
Along with its advantages, Bitcoin also has limitations as compared to the digital currencies which are managed in a 
centralized way.

> * Limited capacity
> * High system maintenance costs
> * Long transaction confirmation
> * Public availability of transaction data
> * High fees and their volatility

In the following subsections, we will consider each limitation in detail and describe the respective solutions (see 
4.6–4.8).

> **_NOTE:_** *Note. The above-mentioned limitations do not make Bitcoin incomplete. They are the outcome of a 
> **tradeoff** between properties such as efficiency, security, and censorship resistance. For the same reason, it is 
> incorrect to compare the capacity of digital currencies that operate under different security assumptions (such as 
> Ethereum and Ripple).*

### What does decentralization in Bitcoin mean?
So, you now have a basic grasp of decentralization as a notion and also some of the key aspects of Bitcoin operation. 
This is why, before proceeding to further sections, we would like to repeat some of the specific peculiarities of 
Bitcoin, namely what does it mean when one says that Bitcoin is decentralized. Essentially, this means that there is no 
particular party responsible for a particular process in the system.

> **There is no designated party/organization that is**
>> * Responsible for Bitcoin operation
>> * In charge of system management
>> * Collecting fees
>> * Processing transactions
>> * Exclusively storing the history of transactions
>> * Making refunds
>> * Enforcing court orders

**Frequently asked questions**

*– Are 21 million bitcoins enough for mass adoption?*

Some might argue that 21 million coins are not enough for a large number of users. However, each coin can be split into 
at least one hundred million pieces and, if necessary, it is potentially possible to release an update and increase the 
coin divisibility.

*– Is it possible to "deactivate" Bitcoin?*

Theoretically, the possibility exists. In practice, the operation of Bitcoin is maintained by the participants whose 
equipment is located in different parts of the world. To participate, one needs a computer and Internet access. 
Therefore, in order to perform a "shutdown", it would be necessary to ban the use of computing equipment and data 
transmission channels. Moreover, it would also be necessary to enforce this prohibition. This would require coordinated 
and rapid actions by various political forces, including those opposing each other. This is extremely hard to get done.

*– What if I want to modify the Bitcoin protocol?*

This is quite common, so there is a conventional way of making suggestions for protocol improvement. The corresponding 
repository is called "Bitcoin Improvement Proposal" (BIP); in it, members of the community discuss new proposals. Anyone 
can add their proposal to the repository, and if the community and validators agree with the update, they will switch to 
a new version of the protocol.

*– Does the issuance of coins depend on the demand on them?*

There is no such dependency. Here, it is worth mentioning whether the market price of a coin depends on its mining 
difficulty: in general, it doesn’t. What one can only say surely is that the prime cost of a coin (the price of its 
mining) will be unlikely to be higher than its market price.

## 2.2 How to use Bitcoin?
In this subsection, we explore the core properties of digital wallets for utilizing Bitcoin: their types, special 
aspects of their use, features of their implementations, and the impact of these features on the level of security.

> **Most common operations with wallets**
>> * Balance check
>> * User keys generating and storing
>> * Payment sending and receiving

To illustrate the idea of a digital wallet, see Figure 2.7; here you can see a screenshot of a mobile application 
running an implementation of a bitcoin wallet. On the main screen (the one on the left in the figure), you can see the 
current balance, the list of transactions, the amount of each payment, and the state of their confirmation. You can also 
switch to another view (the one on the right) to accept and send payments.

[Figure 2.7] - User interface of a particular bitcoin wallet

### Addresses in Bitcoin
A bitcoin address is an identifier of the recipient. In order to send a payment, the sender needs to know the 
destination address. Therefore, it is important for the recipient to provide his address in advance. A regular bitcoin 
address looks like this:

**1BooKnbm48Eabw3FdPgTSudt9u4YTWKBvf**

In some sense, a bitcoin address is an alias of a user, which is not tied to any identification data—it (address) is 
relatively anonymous. This fact, however, does not mean that, when operating with Bitcoin, you stay anonymous by default 
(to learn more, see section 7.1).

An important feature of Bitcoin is that users create their bitcoin addresses on their own (this also applies to other 
cryptocurrencies) by launching a special software that generates a new address. The number of addresses that one user 
can create is unlimited, meaning you can generate even a billion ones and use each for accepting and spending coins.

*Each address has a certain confidential data value available only to the owner of the wallet—a private key*. It is used 
to prove ownership of coins at the time they are sent.

An address can be calculated mathematically from the private key, so if someone takes possession of your private key, he 
can access the coins without even knowing the address.

One key difference of cryptocurrency from the traditional financial system is that the number of addresses is 
practically unlimited. You can send coins to any of them, whereas in the banking system, accounts always belong to some 
organization and must have an owner.

### Transactions in Bitcoin
*A transaction is a set of digital data that initiates the update of the database (the sending of bitcoins from one 
address to another)*. It is a simplified definition; the transaction structure is actually more complex (for more 
details, see 2.3). Note that transaction is created and sent directly by the user's digital wallet, so it is the process 
which even digital currency developers cannot interfere with. Figure 2.8 shows the screenshots of the interface parts of 
a particular mobile wallet [25] which allow providing your requisites and send a payment, respectively.

[Figure 2.8] - Sending and receiving funds via mobile wallet 

> **Basic types of bitcoin wallets**
>> * Software wallet
>> * Hardware wallet
>> * Centralized storage

### Software wallets
Some people use software wallets to store their bitcoins. Software wallets can be launched on a smartphone, laptop, or a 
desktop computer. *A software wallet is an application that processes keys and transactions*. This application connects 
to the Bitcoin network through *trusted nodes or centralized services* or is a network node itself.

Note that it is recommended to only use the software wallets of developers you trust. This cautionary point is made due 
to the fact that developers can introduce vulnerabilities into the application code that would allow them to later block 
or steal the coins. Unintentional mistakes can also be made in their code that hackers can later exploit.

Therefore, it is recommended to use open source software wallets. This solution allows you to audit the wallet's source 
code and prepare it to run on your device.

However, software developers are not the only ones who should be trusted by a software wallet user. In addition, a user 
needs to trust the services which distribute this software (application stores, etc.), the developers of the operating 
system, and the developers of the user's device: backdoors can potentially be implemented at any of these stages.

### Hardware wallets
We pay special attention to hardware wallets. Such a wallet is a computer that can only perform a narrow set of 
operations and with limited control interfaces. This is done to ultimately complicate possible virus injections into a 
device as well as eliminate device hacking in general. Typically, the basic functions are as follows: private key 
generation, private key storing, and transaction signing.

The basic configuration of a wallet does not have any additional interfaces; it is simply a compact device. Most often, 
it can be connected to a regular PC and operated with via special software. Also, more advanced models are currently 
available—the hardware wallet may have a screen, a keyboard, a battery, a fingerprint scanner, and modules for wireless 
communication (Fig. 2.9). In some cases, there is even a camera for scanning the recipient's address. Such wallets 
minimize the risk of theft of private keys but do not protect the owner from their loss. Therefore, many hardware 
wallets provide a backup of private keys.

[Figure 2.9] - Examples of hardware wallets

Once there was an interesting story: one person bought on eBay a used Ledger hardware wallet from an unknown 
seller [26]. In the box with the wallet, there was a sheet of paper (Fig. 2.10) with a recorded mnemonic phrase with the 
note: "Use this phrase to restore your wallet." The new owner of the device entered the suggested words, and a few days 
later it turned out that all his bitcoins were gone. How did this happen? The seller of the wallet (the previous owner) 
knew this mnemonic phrase. Therefore, he could access the addresses that the new wallet owner used. In  other words, 
what the previous owner said was: "Put your money in the vault for which I have a key." Thus, the buyer of the device 
has irretrievably lost all his coins. The mistake was made as the device buyer did not generate new keys.

[Figure 2.10] - Mnemonic phrase for restoring a specific wallet 

In theory, you can imagine a situation where the private key of one user appears to be the same as the one of another 
user. But in practice, it is extremely unlikely. This is because the amount of possible private keys is 2<sup>256</sup>. 
This number is greater than the number of atoms in the universe. Therefore, for a mnemonic phrase with a length of 24 
words, the probability of this coincidence is almost zero.

> **_NOTE:_** *If the software that generates private keys is initialized based on some obvious data, such as a user's 
> password or the current time, then the probability of guessing and matching keys increases.*


