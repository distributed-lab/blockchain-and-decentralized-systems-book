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

![Figure 2.1 - Bitcoin is a digital gold analog](/resources/img/volume-1/2.1-what-is-bitcoin/2.1-gold.png)

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

![Figure 2.2 - Fragment of the first publication about Bitcoin](/resources/img/volume-1/2.1-what-is-bitcoin/2.2-bitcoin-wp.png)

Thus, Satoshi used existing concepts to implement Bitcoin and solved the issues of earlier attempts to create digital 
money. It is fair to assume that not many would have recognized an embryonic formation of an independent digital 
currency from the above-mentioned projects.

> **Background to the invention of Bitcoin**
>> * *David Chaum—DigiCash* [18] *(1989)*
>> * *Adam Back—HashCash (1997)*
>> * *Nick Szabo—BitGold* [19] *(1998)*
>> * *Wei Dai—B-Money (1998)*
>> * *Hal Finney—RPoW as e-money* [20] *(2004)*

Bitcoin is the first successful implementation of a decentralized accounting system. There were other attempts before 
it: for example, the company DigiCash, founded by David Chaum in 1989, was the first to offer digital payments with a 
special currency called Ecash. Users of this payment system were anonymous and banks could not track their accounts. 
The project was supported by several innovative banks in the US and one in Finland; since it was very difficult to 
convince more banks and merchants to accept anonymous currency, the project was canceled.

> **Key dates**
>> * *Bitcoin whitepaper release—October 31, 2008*
>> * *Genesis block creation—January 3, 2009*
>> * *First bitcoin exchange—February 2010*
>> * *P2SH adoption—January 5, 2012*
>> * *Bitcoin Cash fork—August 1, 2017*
>> * *Segregated Witness adoption—August 24, 2017*
>> * *Lightning Network launch—March 15, 2018*

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
>> * *Estimated 10 million unique users*
>> * *On average, 300,000 payments in bitcoins per day*
>> * *Daily trading volume equivalent to $15 billion*

In 2018, Bitcoin is used by millions of people worldwide and hundreds of companies accept bitcoins as a payment method. 
Many projects are being developed on the basis of Bitcoin, and some countries (most notably Japan) recognize and 
acknowledge bitcoin as a legal means of payment.

### Issues that Bitcoin can solve

The first reason Bitcoin became popular is that it implements a decentralized financial system independent of 
governments and banks. It is not subject to inflation in a traditional sense of this term (there is no entity or a 
particular person who can print more Bitcoins on their own). It does not allow freezing, reverting, or canceling 
transactions, and it relies only on mathematics and not on the whims of people. In addition, bitcoin payments are more 
private compared to digital currencies that existed before.

> * *Financial instrument that operates without restrictions*
> * *No complex registration processes*
> * *Can be used anonymously*
> * *Resistant to censorship*

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
>> * *Users do not control their funds*
>> * *Fund transfers may take days or weeks to settle*
>> * *Payments can be canceled or voided*

It turned out to be very difficult to implement a system in which people could independently manage their own money. 
However, over time, the demand for this has been increasing, and in 2009, a real solution was presented in the form of 
Bitcoin. It did not take long for this to capture the attention of the public.

### Main principles of Bitcoin operation

A concise formulation of the basic principle on which Bitcoin functions would sound something like this: *each user runs 
the same version of the software which has the same state, and, as a result, they maintain a decentralized system*.

> * *Copy of the database is stored by all users*
> * *Users process transactions themselves*
> * *All users follow the same rules*
> * *Users only trust what they can verify themselves*

Connection and disconnection of participants occur independently and do not affect the work of other network members. 
Everyone keeps and updates the database and the corresponding software independently. All the rules of work are stored 
by everyone, and all users double-check each other. The Bitcoin network of users can be represented as in Figure 2.3.

![Figure 2.3 - Network of Bitcoin users exemplified](/resources/img/volume-1/2.1-what-is-bitcoin/2.3-network-of-users.png)

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

> * *The process of issuance is decentralized*
> * *Issuance rules are controlled mathematically*
> * *Issuance is performed by the users themselves*
> * *New coins are distributed among active participants*

According to the protocol rules, any user can perform issuance, while all the network members verify the compliance with 
these rules. In short, new bitcoins are distributed among the users who are actively involved in maintaining the 
accounting system. They are called validators. This means that there can be a lot of participants in the system and each 
of them can receive new coins.

> **Rules for the issuance of bitcoins**
>> * *New coins appear on average every 10 minutes*
>> * *Every 4 years the number of new coins is reduced by half*
>> * *Absolute total number of bitcoins which will ever be issued is 20,999,999.9769*

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

![Figure 2.4 - Graph of the change in the amount of issued bitcoins over time](/resources/img/volume-1/2.1-what-is-bitcoin/2.4-graph-of-issuance.png)

> **Properties of bitcoin coins**
>> * *Scarcity (limited amount)*
>> * *Discreteness (coins are divisible)*
>> * *Censorship resistance (independence from any company or state)*
>> * *Immutability (inability to forge coins)*

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

![Figure 2.5 - Demand and supply ratio for bitcoins](/resources/img/volume-1/2.1-what-is-bitcoin/2.5-demand-and-supply.png)

In Figure 2.6, there is the graph of the price of one bitcoin (1 BTC) from the launch date of Bitcoin, namely from 2009 
to 2018 [22].

![Figure 2.6 - Graph of the change in value of a bitcoin over time](/resources/img/volume-1/2.1-what-is-bitcoin/2.6-value-change.png)

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

> * *Limited capacity*
> * *High system maintenance costs*
> * *Long transaction confirmation*
> * *Public availability of transaction data*
> * *High fees and their volatility*

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
>> * *Responsible for Bitcoin operation*
>> * *In charge of system management*
>> * *Collecting fees*
>> * *Processing transactions*
>> * *Exclusively storing the history of transactions*
>> * *Making refunds*
>> * *Enforcing court orders*

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
>> * *Balance check*
>> * *User keys generating and storing*
>> * *Payment sending and receiving*

To illustrate the idea of a digital wallet, see Figure 2.7; here you can see a screenshot of a mobile application 
running an implementation of a bitcoin wallet. On the main screen (the one on the left in the figure), you can see the 
current balance, the list of transactions, the amount of each payment, and the state of their confirmation. You can also 
switch to another view (the one on the right) to accept and send payments.

![Figure 2.7 - User interface of a particular bitcoin wallet](/resources/img/volume-1/2.2-how-to-use-bitcoin/2.7-user-interface.png)

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

![Figure 2.8 - Sending and receiving funds via mobile wallet](/resources/img/volume-1/2.2-how-to-use-bitcoin/2.8-spend-and-receive.png)

> **Basic types of bitcoin wallets**
>> * *Software wallet*
>> * *Hardware wallet*
>> * *Centralized storage*

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

![Figure 2.9 - Examples of hardware wallets](/resources/img/volume-1/2.2-how-to-use-bitcoin/2.9-hardware-wallets.png)

Once there was an interesting story: one person bought on eBay a used Ledger hardware wallet from an unknown 
seller [26]. In the box with the wallet, there was a sheet of paper (Fig. 2.10) with a recorded mnemonic phrase with the 
note: "Use this phrase to restore your wallet." The new owner of the device entered the suggested words, and a few days 
later it turned out that all his bitcoins were gone. How did this happen? The seller of the wallet (the previous owner) 
knew this mnemonic phrase. Therefore, he could access the addresses that the new wallet owner used. In  other words, 
what the previous owner said was: "Put your money in the vault for which I have a key." Thus, the buyer of the device 
has irretrievably lost all his coins. The mistake was made as the device buyer did not generate new keys.

![Figure 2.10 - Mnemonic phrase for restoring a specific wallet](/resources/img/volume-1/2.2-how-to-use-bitcoin/2.10-mnemonic.png)

In theory, you can imagine a situation where the private key of one user appears to be the same as the one of another 
user. But in practice, it is extremely unlikely. This is because the amount of possible private keys is 2<sup>256</sup>. 
This number is greater than the number of atoms in the universe. Therefore, for a mnemonic phrase with a length of 24 
words, the probability of this coincidence is almost zero.

> **_NOTE:_** *If the software that generates private keys is initialized based on some obvious data, such as a user's 
> password or the current time, then the probability of guessing and matching keys increases.*

### Centralized storages

There are online services that implement the basic functions of digital wallets but store private keys on their own 
servers. Their users have a false feeling that they have a bitcoin wallet, whereas it is actually a particular remote 
service that performs operations and stores coins instead of the user himself. Obviously, by using such services, you do 
not control your coins and must completely trust the third-party organization.

One of the largest providers of centralized storage is Coinbase. In fact, it is a kind of bitcoin bank, where a user 
registers as with PayPal (login, password, etc). After that, a user is given an address for the deposit of bitcoins and 
an application for remote management of her coins. A user in fact only has a promise from the centralized service to 
keep funds safe. Using centralized wallets, your ability to manage funds is limited: you can only send a request for a 
withdrawal of bitcoins, and then “hope” that it will be processed.

Most exchanges store coins of users on the principle of centralized storage, hence they require a maximum level of trust 
from their users.

### Wallet backup

Since the private key is the only way to prove ownership of coins and spend them, *the owner of coins is determined by 
the knowledge of the private key*. All bitcoin wallets implement the following functions: generation of addresses for 
coin receiving, displaying of balances and transaction history, payment sending, creation of a *wallet backup*, and 
wallet restoring from a backup.

There was an interesting incident in the life of a man named James. He had a hard drive storing the keys with access to 
7,500 BTC. Since, at that time, the price of bitcoins was low, James threw the hard drive away. When the price of the 
coin increased, James realized how much money he had lost and started looking for his hard drive at the dump. He even 
appealed to the municipality, so that the local authorities would allow him to search the dump, but he was refused. 
Ultimately, James spent a long time pursuing various options to find the hard drive holding his coins, but all efforts 
proved unsuccessful [27].

We deliberately emphasize the fact that making a backup copy of your wallet is mandatory and something you should carry 
out even before you receive the first payment. For this functionality, there is a technical specification supported by 
different developers of the wallet software. A backup copy can generally be restored in wallets from different 
developers.

If a user is wondering whether or not it makes sense to make a backup copy of the wallet after each payment, the answer 
will depend on the implementation of a specific wallet. Usually, the wallet generates new addresses and corresponding 
keys for each payment, meaning that you have to re-transfer them to a backup copy each time. However, there are wallets 
that use the so-called deterministic key generation (*deterministic wallets*). In this case, one backup is enough.

For the deterministic wallets, there is the concept of the main secret of the wallet. All new keys are mathematically 
generated from this main secret. This means that users can make a backup copy of the main secret of the wallet only once 
and then, using it, restore the access to coins on another device and/or in other software at any time. Usually, special 
encoding is used by the wallet to allow users to represent the main secret with an easy-to-remember mnemonic phrase.

An example of a mnemonic phrase is illustrated in Figure 2.11. In practice, it is convenient to write a mnemonic phrase 
on a piece of paper and store it in a very safe place and, when necessary, enter it into another wallet. To store keys 
conveniently, you can use password managers. They allow a user to remember from 4 to 6 words instead of writing down the 
entire mnemonic phrase. A user will further need those particular words to open a password manager and access all her 
private keys.

![Figure 2.11 - Example of a mnemonic phrase](/resources/img/volume-1/2.2-how-to-use-bitcoin/2.11-mnemonic-example.png)

**Common myths**

*If you store private keys on your device, they cannot be stolen.*

The most common way to steal coins is to steal private keys using a software virus. Therefore, it is not recommended to 
store the private keys which big coin amounts are tied to on unprotected devices.

*If you store private keys on a device with no internet access, you need not worry about security.*

This approach can actually improve the security of key storage and protect against most remote attacks, but this does 
not mean that the private key stored in this way is ultimately secured.

*Quantum computers can easily hack the Bitcoin encryption system.*

In fact, Bitcoin does not use encryption at all. It uses digital signatures and hash functions in a combination that 
makes them not vulnerable to attacks by a quantum computer (see 3.2).

**Frequently asked questions**

*– Are there any organizations that resolve disputes regarding transactions?*

There is no such centralized organization. Disputes regarding transactions are resolved at the Bitcoin protocol level. 
However, there may be disputes outside the protocol during its usage. To solve them, the so-called escrow agents are 
used. This means that during a transaction, coins are frozen between the sender and the recipient. The decision on how 
to distribute these coins is made with the mediator’s participation. In this case, the mediator must be trusted by both 
parties of the transaction.

*– Why can't you personalize a wallet—bind it to a specific person?*

Bitcoin as a payment system protocol does not require user identification. Moreover, this is not an exclusive 
requirement for verifying transactions (for this, a digital signature is enough). The one who generates such signatures 
is the owner. And so, the signature can be owned by a person, a group of several people, even a robot, etc. As for 
personalization, there is no identity data in the Bitcoin database, but the entire transaction history is publicly 
available. This means that if you manage to associate a particular address with a particular person, then you can 
deanonymize some users with a certain probability.

*– If several billion new users get wallets, will Bitcoin slow down?*

If you just open a wallet and create many addresses, this will not affect the network and the database. What does matter 
is the number of transactions. So, it's not about the number of users but the scale of their activity.

*- Is it possible to create wallets that work without servers and a center (peer-to-peer)?*

Yes, it is possible. However, in this case, the requirements for the mobile device increase: internet connectivity, long 
recovery on another device, power consumption, and device memory.

## 2.3 Concept of transaction in Bitcoin

In this subsection, we describe some general ideas around the concept of transaction in Bitcoin: what a bitcoin 
transaction is, how it is verified in a shared database, how a user proves her ownership of the coins that she tries to 
spend, how fees are determined and set in Bitcoin, and what conflicting transactions are.

### What is a bitcoin transaction?

As we noted earlier, *a transaction is a set of digital data which is used to update a particular database (which 
reflects coin transferring from one address to another)*. A transaction is especially what determines the transfer 
amount and the recipient’s address as well as the terms required to access the transferred coins.

> **Lifecycle of a transaction**
>> * *Creation*
>> * *Propagation*
>> * *Verification*
>> * *Inclusion in a block (validation)*
>> * *Rejection (in case the block is invalidated later)*

Let's explore what data makes up a bitcoin transaction. Any transaction in Bitcoin contains the origin of coins that are 
being spent (i.e., references to transactions where these coins were received), proof of coin ownership, addresses of 
new owners (more broadly, conditions under which the coins can be spent), and the transfer amounts.

> **Data in the body of a transaction**
>> * *Origin of coins that are being spent*
>> * *Proof of coin ownership*
>> * *Address for the transfer (spending conditions)*
>> * *Transfer amounts*

> **_NOTE:_** *In the simplest case, an address is associated with one pair of keys (the public and private key), which 
> is used to generate and verify the digital signature. A private key is used to authenticate transactions and is only 
> known to the address owner. The number of all possible addresses is huge, and the number of possible private keys is 
> even greater. Therefore, guessing a private key to someone else's address is almost impossible (given that key 
> generation is a truly random process). It is important not to confuse the concept of address with the concept of 
> account since there are no accounts in the Bitcoin protocol.*

The analogy with a usual receipt might help to better understand the idea of a bitcoin transaction. In one sense, 
regular money is a receipt. Each receipt contains data: who received the payment, from whom, for what, the payment 
amount, date, and signature (Fig. 2.12).

![Figure 2.12 - Example of a traditional receipt](/resources/img/volume-1/2.3-concept-of-transaction-in-bitcoin/2.12-traditional-receipt.png)

A bitcoin transaction, just like the lawful receipt, must meet specific requirements to be considered correct. First, it 
must transfer coins that the sender owns. Secondly, these coins can be spent only once.

*Verification is the process of checking the data (transactions, blocks, etc.) according to the protocol rules.*

During the verification process, each transaction is checked to ensure that the sender owns the coins that she tries to 
spend. Usually, the originator of a transaction proves the ownership of coins by using a digital signature. In addition, 
it is necessary to verify that a transaction spends existing coins for the first time and only once.

The authenticity of the paper receipt is easy to verify, while arguably digital receipts could be copied many times, 
making it difficult to determine the original. Suppose that Alice has given a receipt to Bob. If Bob turns out to be a 
fraud, you can assume that he may come with his, say, five friends, all with the same receipts, and all five would 
demand money from Alice. Thus, if each receipt would have had the authenticity of the original one, then Alice would 
definitely find herself in an awkward situation. In paper form, it is relatively easy to distinguish a copy from the 
original; in digital form, it is not. This and similar problems obviously demand their solutions.

In Bitcoin, the solution is that transactions are joined in the structure units called blocks. A block is a unit of data 
consisting of a header and a body that represents a set of transactions (generally, non-empty). Blocks are linked to 
each other with hash values (for further details, refer to 3.1) and in this way stored in a shared database. This 
approach ensures the immutability of a database of all transactions.

### Verification of transactions

Paper checks (Fig. 2.13) are still common in the US and many other countries: people receive their salary in checks, pay 
for rent, cash them out in banks, etc. On the check, you can see its serial number (12982 as in Figure 2.13), which is 
unique for each check (you cannot cash out two checks of the same serial number).

![Figure 2.13 - Example of a paper check](/resources/img/volume-1/2.3-concept-of-transaction-in-bitcoin/2.13-paper-check.png)

How does Bitcoin solve the problem of protection against copying of original "receipts”, in its case called 
transactions? In the checkbook, each check has an order number, which is its unique identifier. But in a *decentralized 
environment*, where Bitcoin functions, it is impossible to enumerate transactions since all participants work 
asynchronously. Therefore, in order to distinguish one transaction from another, in Bitcoin there is a globally unique 
transaction identifier (*txid/wtxid*)—the so-called *hash value* calculated from the transaction data (for further 
details, see 3.1). If multiple transactions appear to have the same hash value, only one of them will be considered 
correct. However, there can be exceptional situations, which will be described separately.

An interesting feature of Bitcoin is that it is possible in any transaction to show "where" the coins came from (there 
is a reference to the previous transaction with its hash value). This is how the origin of transmitted coins is verified 
in Bitcoin. You can schematically see how a new transaction spends the coins received from the previous transaction in 
Figure 2.14.

![Figure 2.14 - How Bitcoin transactions work](/resources/img/volume-1/2.3-concept-of-transaction-in-bitcoin/2.14-how-transactions-work.png)

> **Basic stages of transaction verification**
>> * *Verification of the condition that the spent coins exist in the accounting system*
>> * *Verification of the condition that the coins are spent for the first time and not more than once*
>> * *Verification of the proof of coin ownership submitted by the sender (initiator of a transaction)*

Here is how it works: in order to spend the coins, a user must indicate where he received them from and prove the 
ownership. If the origin of coins does not raise additional issues—there is no other transaction that spends the same 
coins and the user has proved his ownership—then all that remains is to wait for the confirmation of this transaction by 
other members of the system.

The process of transaction confirmation demands that participants first check them and then mutually agree which 
transactions they consider correct. This means that a transaction in order to be confirmed should receive a consent of 
*the majority of active participants* (see 2.5). Anyone can participate in the process of confirming transactions in 
Bitcoin (Fig. 2.15).

![Figure 2.15 - How nodes interact in the Bitcoin network](/resources/img/volume-1/2.3-concept-of-transaction-in-bitcoin/2.15-nodes-iteraction.png)

### Concept of fee in Bitcoin

The transaction model in Bitcoin assumes transaction fees which are paid in bitcoins. Fees are included by the sender 
during the creation of a transaction and should, by default, be above a certain threshold. Although in practice, users 
can set a zero fee and such a transaction will be considered correct. The fee is technically an additional reward for 
the participants who confirm transactions.

With the growing popularity of Bitcoin, the flow of new transactions in the network has increased significantly. In 
Bitcoin, the block size was limited under the rules of the protocol (the maximum base size of a block is 1 MB). 
Sometimes there are situations when the flow of new transactions exceeds the capacity in Bitcoin. In this case, each 
network node arranges all unconfirmed transactions into a queue, where transactions that pay higher fees are at the 
front. In fact, transactions in Bitcoin can be considered as a data record in a shared database. The price of this data 
record is the ratio of a fee defined in a transaction and the size of a transaction in bytes (or its size in *weight 
units*; see 4.6). So transactions at the end of the queue may remain unconfirmed for longer. Therefore, this provokes a 
hardly predictable market for adding a unit of data to the Bitcoin database—namely, how much you should pay to have your 
transaction confirmed within a reasonable time (see 4.7).

The principles of decentralization applied to defining fees in bitcoin transactions can be illustrated by an example: 
you can compare a potentially confirmed transaction in Bitcoin to a political party that wants to enter parliament. 
Which of the deputies gets to the upper part of the election list depends on the amount of his "contribution" 
(unfortunately, such an approach is very popular in many countries). The deputies will be "sorted" by the total of this 
"contribution"; those who offered less will not succeed. Most likely they will have to wait for the next parliamentary 
elections and try again. Due to the high volatility of fees in Bitcoin, this became a problem as the transaction sent to 
the network could remain unconfirmed for a long time (or even forever). However, this problem was later solved (for more 
details about fee volatility and the solution to this problem, see 4.7).

> *Transaction pays a fee per each unit of its weight*
> *Maximum base size of a block is 1 MB*
> *Validators sort transactions by descending price of data records*
> *Transactions with a low price of data recording may remain unconfirmed for a longer time (or even forever)*

### Concept of conflicting transactions

A well-formed chain of blocks contains transactions that do not conflict with each other. You may wonder what a 
conflicting transaction is. If John transferred 5 coins to Mary in one transaction and later sent the same 5 coins in 
another transaction to Paul, then it is obvious that both transactions cannot be recorded in one version of history. 
Therefore, only one of these transactions will receive full confirmation. The question is: who will get these 5 
coins—Mary or Paul? Until the point of full confirmation, it is impossible to answer the question as to which 
transaction will definitely be accepted. You can only suppose that the transaction with a higher chance of confirmation 
is the one which was distributed over the network first or, even more likely, the one that pays a higher fee.

**Common myths**

*The Bitcoin protocol source code is closed and only the creator can make changes.*

Quite the opposite: there are several implementations of one protocol in different programming languages; the source 
code of most of them is open and available for study, modification, etc.

*Ordinary engineers and developers cannot suggest improving the Bitcoin protocol or develop their own wallet.*

Anyone can offer a protocol modification and even program his own network client or application. The existing Bitcoin 
community is open to new proposals and primarily consists of independent enthusiasts. Though, it is generally the more 
experienced participants who offer the most worthwhile protocol modifications.

**Frequently asked questions**

*– Is it necessary to connect to the Bitcoin network to generate a new address?*

No, you do not need a network connection for this. Generation of keys for a digital signature and the generation of 
addresses is performed locally. This process does not depend on other users.

*– What happens if private keys of two different users match?*

The probability of such a coincidence is extremely small yet exists, so it is usually neglected. But if due to a 
generation error or some other unlikely reason this happens, then both users will, in fact, see one amount on the 
balance sheet and have access to the same coins, which can be spent only once.

*– Who establishes the fee for the transaction?*

The amount of the fee is specified in the body of the transaction itself. It is determined by the one who creates and 
signs the transaction. According to the Bitcoin protocol rules, a zero-fee transaction is correct. However, the software 
of most network nodes will simply ignore the transaction if it is below the necessary minimum (which can change over 
time).

*– Is it true that somehow one can analyze the history of the origin of all coins and violate the privacy of some 
transactions in Bitcoin?*

Yes, there is a practice of creating such organizations that analyze a large number of direct and indirect data 
regarding the transactions in Bitcoin. And these organizations can with some probability say which coins belong to whom 
and for what purpose they are used. In turn, services that take bitcoins, for example, centralized exchanges, can apply 
to such organizations for information. Services that value their reputation, such as Kraken, do not accept payment if 
the history of the *coins origin* is doubtful (see 4.1). However, there are approaches that allow increasing the level of 
privacy in Bitcoin: cryptocurrencies such as Monero and ZCash place greater emphasis on the anonymity of users 
(see 7.2).

## 2.4 High-level architecture of Bitcoin

In this subsection, we will cover how Bitcoin works, review the features of a shared database and collective processing 
of transactions, describe the basic principle of reaching consensus in Bitcoin, and also compare Bitcoin with 
traditional payment systems.

The architecture of Bitcoin is heavily affected by the need to work in an extremely hostile Internet environment, which 
is anonymous, where law enforcement barely works, which is full of hackers, and where there are no responsible parties 
nor any guarantees. Therefore, its launch has proved that it is possible to create a financial system with an open 
database that anyone can use without registration. The basic principles are as follows: a copy of the database is stored 
by all users; users follow the same rules and trust only what they can verify themselves. This is the essence of 
*decentralization in accounting systems*.

### Architecture of Bitcoin
Each user has their local copy of the database organized as a chain of strictly ordered sets of transactions—blocks. In 
Bitcoin, transactions are considered unconfirmed until they are added to the block. After this block is confirmed by 
honest participants, it is impossible to modify or remove the transaction from the shared database undetected. 
Schematically, this data organization can be represented as a chain of blocks, in which each block is a set of 
transactions (see Fig. 2.16). Satoshi called this format of data storage *block chain*; later it was called 
*blockchain*. 

![Figure 2.16 - How a blockchain-based database is arranged](/resources/img/volume-1/2.4-high-level-architecture-of-bitcoin/2.16-blockchain-based-database.png)

The blockchain technology allows organizing the database in such a way that all the blocks are linked to each other (The 
green block in Figure 2.16 is a so-called *genesis block*; for further details, see section 4.3). Since blocks are 
linked together, it is impossible to change the content of one block without affecting all the subsequent ones.

Who verifies the transactions and how do they get onto the chain of blocks? Any user can create a correct (in terms of 
the protocol) transaction, send it to the network, and it will be transmitted to all other participants. Then, each 
validator independently verifies the validity of a transaction and adds it to their block for confirmation; when the 
transaction has been added to one of the new blocks, it can be considered confirmed. Each new block in the chain is 
created as a result of parallel and independent work by each participant (Fig. 2.17).

![Figure 2.17 - Users working on the continuation of the chain of blocks](/resources/img/volume-1/2.4-high-level-architecture-of-bitcoin/2.17-users-work-on-the-chain.png)

Who creates blocks and chooses which transactions to include in them? In Figure 2.18, you can see that each block is 
created by one of the users and hence extends the main chain. The creator proposes her block to other network nodes. 
They verify this block and add it to their database copies if this block complies with the protocol rules. If the 
majority of participants add the proposed block, it gets to the chain, and the transactions in the block become 
confirmed. It should be understood that just one block is added to the chain. All the other blocks which have been 
generated by other participants but have not got to the chain are discarded. The transactions of a discarded block 
(an *orphan block*) are not confirmed and are available for future confirmation. This sequence of actions is required to 
achieve consensus in a *decentralized environment*.

![Figure 2.18 - How users create blocks](/resources/img/volume-1/2.4-high-level-architecture-of-bitcoin/2.18-how-users-create-block.png)

### Processes in the Bitcoin accounting system

> *Audit of the accounting system*
> *Validation of transactions*
> *Update management (governance)*

*Audit* is the process of verifying that the current state of the database fully complies the conducted transactions.

*Validation of transactions* (also known as mining or the process of block creation) is the process of verifying 
transactions for the compliance with the protocol rules and of adding them to a block (and subsequently to the database 
of the system).

*Update management (governance)* is the process of affirming the functionality of the system and, at best, transforming 
all nodes of a system to a uniform operation model.

### Roles of participants in the Bitcoin accounting system

Bitcoin allows every participant to manage the above-mentioned processes and hence perform any of the below roles.

> *User*
> *Auditor*
> *Validator*
> *Developer*
> *QA engineer*

Another feature of Bitcoin is that every system participant decides himself (in a *permissionless* manner) which role he 
will perform.

### Conditions in which the consensus in Bitcoin is reached

As described above, before making any changes to the general state of the chain of blocks, participants must come to an 
agreement. If participants have checked the block for correctness, and everyone agrees to add it to their chain, then 
the block is added, and all the subsequent blocks will be created on its basis. Coming to an agreement is very often 
difficult yet a crucial task. Without a solution, you wouldn't achieve reliable operation of a *decentralized accounting 
system*. Therefore, one of its crucial components is the *consensus mechanism*.

*Consensus is the state of users' agreement regarding the transactions that they consider correct, which is developed 
during the "discussion"* (exchange of messages about transactions, blocks, state of nodes). Reaching consensus is the 
goal of all honest participants in Bitcoin, but obviously, you wouldn’t blindly rely on the fact that most Bitcoin 
participants are honest. For this reason, the Bitcoin protocol is designed in such a way that consensus can be reached 
in the system even under the following conditions.

> *Number of network nodes is unknown*
> *Participants can be anonymous*
> *Validators can be anonymous*
> *System participants do not trust each other*
> *There is no single governing authority*
> *Number of malicious nodes is unknown*

Ideally, everyone should agree with the list of confirmed transactions. However, in practice, there are a number of 
difficulties during the reconciliation. As we have already stated, Bitcoin is designed to securely operate in an 
extremely hostile Internet environment. Here, all participants are anonymous and do not trust each other; their number 
is generally unknown and is very likely large; each user's vote can be easily forged (if you consider the traditional 
approach to voting). Directly on the network, there is a computer program which is usually controlled by a person. 
However, it may be that particular software isn't run by an honest participant but rather is a bot controlled by a 
malicious person. Moreover, it may even happen that for a hundred or a thousand users there will be only one real 
person.

### How is the consensus in Bitcoin reached?

Having determined the complexity of the task—reaching overall consensus in the environment which is trustless and where 
any participant may potentially be malicious—we can cover the key question, how.

The principle of reaching consensus in a decentralized accounting system can be explained in the following example. 
Imagine playing a card game. In this game, there is no "dealer" responsible for controlling the game: all participants 
know the rules and are obliged to adhere to them. All moves (in our case, transactions) are public and checked by all 
participants of the game. In case one player cheats, this will be immediately noticed by other players, and the cheater 
will be excluded (Fig. 2.19). The distribution of new cards from the deck is carried out by the same principles. Rules 
determine the order in which players pick up cards as well as the number of these cards.

![Figure 2.19 - Card game as an example of reaching consensus in an auditable environment](/resources/img/volume-1/2.4-high-level-architecture-of-bitcoin/2.19-poker.png)

Let's draw a consensus schematically (see Fig. 2.20). All users store their local copy of the database, which is 
organized using blockchain technology. The state of unanimous approval means that copies of all honest users are 
identical after they have been synchronized (i.e., database copies of all participants have the same blocks of data 
forming identical chains).

![Figure 2.20 - Case in which the system participants have reached consensus](/resources/img/volume-1/2.4-high-level-architecture-of-bitcoin/2.20-consensus-reaching.png)

> **_NOTE:_** *In the context of Bitcoin, the concepts such as shared database, chain of blocks, and transaction history 
> have the same meaning.*

### Bitcoin as compared to traditional payment systems

> *Bitcoin does not require registration*
> *Transaction does not contain any users’ personal  data*
> *Payment address can be used only once*

Very often people wonder how confidential their bitcoin transactions are. In fact, everyone can see all the transactions 
between all the addresses, but it is difficult to identify who is behind each bitcoin address as well as track the 
transaction history of a specific user. For a better understanding of privacy in Bitcoin, let's draw an analogy with 
traditional financial systems (Table 2.1).

Table 2.1 — Traditional payment systems as compared to Bitcoin

|Traditional payment systems| Bitcoin |
|---|---------|
|Non-transparent transaction history and limited access to audit|Transparent transaction history; all changes are evident to everyone|
|Centralized decision-making regarding the changes (governance)|Collective update of the database with transactions|
|Requires full trust to the responsible organization|Trust only to mathematics and the program code|
|Possibility to restore access to an account |Self-management of access keys|
|Risk of bankruptcy and denial of service|Each user bears all the risks related to their keys and software management |

The bank performs the functions of storing and accounting. It takes money and valuables from customers and in return 
promises to remember how much and to whom it owes. Generally, this approach is convenient since access to money is 
performed through a credit card, and only the owner has the ability to use this card. This saves the customer from 
having to carry cash, while the lost cards can easily be replaced. However, the funds of customers are managed entirely 
by the bank and subject to their centralized decisions. A client will have problems in the case that a bank "forgets" 
how much and to whom it owes the money. This is a typical example of a *trade-off between convenience and security*.

Before Bitcoin, this was essentially the only possible way to perform financial relations in society—users could only 
hope that banks would fulfill their obligations. So banks have been increasingly adopting this specific scheme and over 
time developed a lot of different implementations of it.

Bitcoin operates without the restrictions common in traditional payment systems. In one sense, Bitcoin complements an 
established financial system; it shows that alternative architectures are possible as well as an accounting system 
without any managing authority.

**Common myths**

*The creator of Bitcoin can change the currency rules or steal users' coins.*

This is not true. The creator of Bitcoin has the same capabilities as other users.

*To send and receive payments in Bitcoin, you need to register.*

In Bitcoin, there is no database of registered users and no decision-making center. Each transaction is validated 
separately, and anyone can verify a digital signature. Typically, centralized systems use session authentication, which 
means that once a user is verified, he does not need to redo this process for each next action. Meanwhile, in Bitcoin, 
a user is assumed to generate a new address for each new payment.

**Frequently asked questions**

*– What does the bitcoin coin look like?*

Bitcoin coins do not have a physical representation but are defined abstractly. This abstract concept quite clearly 
indicates the properties of the digital currency because the shared database has the record of all actions performed in 
it—the need for physical coins is simply absent.

*– What happens if a transaction is disputed in court, and it orders the transaction to be canceled?*

There is no any global jurisdiction on Bitcoin, as there is no law which rules over the open source protocol. Since 
decisions in Bitcoin are taken in a decentralized way through a consensus, a single action will not affect the overall 
state of the database. The court, of course, can decide to cancel the transaction but most likely will not be able to 
implement it. The only exception is the case in which the majority of participants would support the court order. 
Though, such an arbitrage practice can hardly be imagined nowadays.

## 2.5 Confirmation of transactions in Bitcoin

We have mentioned heretofore that Bitcoin can work reliably, given the anonymity of participants and the potential 
presence of an unknown number of cybercriminals. In this section, we will try to describe in detail how it is made 
possible. In addition, we will explain the issues related to full confirmation of transactions (namely, how the 
transaction that has been previously confirmed can then again become unconfirmed) and why finality of transactions is 
impossible to attain in such conditions [28]. Also, we will consider how Bitcoin implements transaction confirmation in 
a *decentralized environment*.

Among the unconfirmed transactions, any user can choose those that she considers correct, then merge them into a block, 
and offer this block to all other nodes of the network. The block can be adopted or rejected by the decision of the 
majority. If the validator worked honestly, verified transactions according to the protocol rules, and proposed the next 
block of the general chain earlier than the rest, then other honest participants will accept this block.

### Creation of blocks with transactions

A correctly created block consists of transactions that have not been confirmed in the previous blocks and do not 
conflict with each other. Each block necessarily contains the hash value of the previous block. In this way, each new 
block confirms that not only are the transactions in it correct and confirmed but also those in the previous blocks to 
which it refers. Another feature is that the protocol regulates the difficulty of a resource-intensive problem for block 
creation in such a way that a new block is added to the shared database on average every 10 minutes.

> **A correctly created block**
>> * *Contains correct transactions which have not been confirmed so far and do not conflict with each other*
>> * *Contains a hash value of the previous block*

How is the new block created? A particular user's computer stores the entire chain of blocks that has been verified 
using the Bitcoin software and is considered correct. Coin transfers between users in the system occur constantly and 
new unconfirmed transactions are propagated across all nodes. Each user's computer verifies the flow of new transactions 
and adds to the block those it considers correct (Fig. 2.21). Further, it refers to the previous block by indicating a 
link to it, so that the operational history is clear—and this is how the integrity of the database is maintained. Next, 
the computer suggests its created block as a continuation of the general chain to other nodes.

![Figure 2.21 - Verification of received transactions](/resources/img/volume-1/2.5-confirmation-of-transactions-in-bitcoin/2.21-transaction-verification.png)

What happens if one user runs a network node that is cheating and offering fake blocks to other users? Even worse, what 
if it were an organized group of intruders launching a whole botnet of modified network nodes that ignore the protocol 
rules? How can honest users achieve consensus in such a situation? This obvious risk was solved by Satoshi Nakamoto, and 
we will explain how, next.

> **Consensus regarding the new transactions**
>> * *Any user can choose among the unconfirmed transactions the correct ones and add them to a block*
>> * *The proposed block will be accepted or rejected by the decision of the majority of validators*

### Requirements for the new blocks
The solution to the potential problem of fake blocks is as follows: the block is considered correct if a given amount of 
computing resources was spent on its creation. This means that a user must provide a solution to a *resource-intensive 
task* so that all other participants can check and accept his block.

In this way, a bad actor cannot trick the rest of the participants with a large number of fake blocks. The task which 
needs to be performed also addresses two other considerations: how frequently new blocks should appear and how to 
prioritize the order in which blocks are added to the shared database. It works like this: everyone starts creating a 
new block, yet only the one who solves the resource-intensive task first gets the right to offer his block to the rest.

So, the more computational power a user has, the more often she becomes the first to finish the task. The probability 
for a particular participant becoming the first to complete is the percentage of the total system computational 
resources which a particular participant has.

> **Block creation flow**
>> * *A solution to a resource-intensive task must be provided*
>> * *The one who has solved the task first notifies everyone else by sending them his proposed block*
>> * *The probability of being first depends on both the share of participant's resources as compared to all resources* 
>> * *Involved in the system and delays in data transmission channels*

That's how we come to the very essence of mining in Bitcoin, which in practice means the solving of a resource-intensive 
task. This task is the same difficulty for all nodes in the network. Mining is essential for Bitcoin: honest users are 
involved in mining to maintain the reliability of the transaction confirmation process. Those who control more computing 
power create blocks more often. Let’s see how it works in the example below.

### Principles of competition between users

Suppose that there are 10 validators operating in the system. Each of them has one mining computer, which calculates the 
same number of hash values per one unit of time (Fig. 2.22). Let's take this computing power as 1, and thus the total 
power of the system will be 10. Each validator, accordingly, controls 1/10 of this capacity and has a 0.1 probability of 
creating the block first.

![Figure 2.22 - Accounting system consisting of ten validators](/resources/img/volume-1/2.5-confirmation-of-transactions-in-bitcoin/2.22-ten-validators.png)

Now, let's assume that Alice has noticed that mining appears to be more beneficial than ever before and therefore buys a 
couple more computers (Fig. 2.23). After that, the total power of the system will be 12 (10 + 2 = 12). Her share is now 
3/12 (or 1/4), while each other validator’s share remains equal to 1/12. Therefore, the probability that Alice finds the 
new block first is 0.25, while for the other validators, it will be equal to 0.08(3). In this way, even with a smaller 
fraction of processing power, a validator can nevertheless create the new block first, but the probability will be 
lower.

![Figure 2.23 - Network consisting of 10 validators, where one has acquired additional computational power](/resources/img/volume-1/2.5-confirmation-of-transactions-in-bitcoin/2.23-ten-validators-with-diff-power.png)

However, what would happen if an attacker solves the resource-intensive task? What if his block contains transactions 
which are correct in terms of the protocol, yet honest users will still disagree with this block? Sounds confusing, 
right? After all, members of the network are anonymous and you cannot say in advance who is honest and who is malicious.

### Propagation of blocks

Before we answer how disagreements regarding a specific block are solved, let's observe how blocks are propagated over 
the network. So, one of the network nodes finds a solution to the problem and creates a block. Then, a block is 
broadcast to the network. That is, the node which has created a block transmits it to all the nodes to which it is 
connected.

After that, the nodes which have received a block verify whether or not it matches the protocol rules. There are two 
possible scenarios. In the first one, a node agrees that the block is valid and can be added to the chain of blocks; it 
saves a copy of the block and propagates it further over the network. What happens in the second scenario, in which a 
node disagrees that a block received is valid?

### Resolving disagreements

To answer the above question, we will consider how disagreements regarding confirmation of transactions are solved. If 
one participant does not agree with the block of the other, it is possible, according to the protocol rules, to create 
an alternative block at the same height of the chain of blocks.

> *Disagreed participant creates an alternative block*
> *Alternative blocks can include the same transactions*
> *Network nodes keep both alternative versions*
> *Each of the participants creates blocks, extending one particular version of the chain*
> *The longest chain "wins" (the one which has taken the largest amount of work to be created)*

*The height of the block is the sequence number of the block in the chain starting from the genesis block* (Fig. 2.24).

*Genesis block is a block with a zero height.*

![Figure 2.24 - Numeration of blocks (0 – genesis block)](/resources/img/volume-1/2.5-confirmation-of-transactions-in-bitcoin/2.24-block-num.png)

Therefore, a disagreement between at least two participants may entail a situation where we have two blocks created both 
referring to the same previous block. Such blocks can include the same or conflicting transactions. In this case, all 
the other nodes of the network can save both suggested options if they consider both blocks correct, but each of them 
should nevertheless determine which block is the basis to start creating the next one. In this way, different  
participants make their choice, creating subsequent blocks on top of one of the existing blocks, and specifying a link 
to it in their new block to extend a particular version of the chain. The rules of the protocol state that out of the 
two correct versions, the priority should nevertheless be given to the one for the creation of which more computational 
resources have been spent (i.e., the version which is supported by the majority of participants).

The process of resolving disagreements can be shown schematically (Fig. 2.25). There is a network of nodes. These nodes, 
in their local database copies, may store two alternative blocks at the same height. Let's say that users on the left 
(in the figure) have decided to select the top block as the main one, and users on the right selected the bottom one. 
Everyone continues to work on creating the next block supporting different versions.

![Figure 2.25 - Numeration of blocks (0 – genesis block)](/resources/img/volume-1/2.5-confirmation-of-transactions-in-bitcoin/2.25-disagreement.png)

Then, a certain user has created and proposed a new block that refers to the block which is at the top of the two 
alternative blocks (according to the above figure). Her proposal was accepted by all other participants of the network. 
Even those who originally chose another alternative block, have checked and accepted a longer chain (Fig. 2.26).

![Figure 2.26 - Network state after disagreements are resolved](/resources/img/volume-1/2.5-confirmation-of-transactions-in-bitcoin/2.26-disagreement-resolved.png)

This is the so-called *rule of the longest chain*. It says that among all chains a participant considers correct, he 
must choose the longest one. In other words, he chooses the chain where  more work has been spent on its creation.

Furthermore, an honest participant will switch to the longest chain only if it was created according to the protocol 
rules. This means that anyone trying to disrupt the chain is prevented from violating the Bitcoin rules, even when 
having a lot of computational power.

### Concept of a full transaction confirmation

Probably the most important matter in Bitcoin is the process of confirming transactions. *A transaction can be 
considered fully confirmed if it is included in the longest chain and there are 5 blocks after the block in which this 
transaction is contained*. In other words, you need to wait for at least 6 confirmations, not 1. Since a new block is 
created on average once every 10 minutes, full confirmation of a transaction takes approximately one hour (however, in 
some situations, one may want to wait much longer to reduce the risk of an attack).

The answer to the question why exactly is it 6 confirmations lies in the mathematical calculation: if there are two 
alternative chains and one is ahead of the other by 5 blocks given that the same amount of processing power was invested 
in both chains, the probability that the shorter chain outruns the longer one is very small. In his article [15], 
Satoshi Nakamoto mathematically proves this on the basis of the following proposition.

[Formula]

where p is the probability that the next block will be created by an honest node; q is the probability that the next 
block will be created by an attacker; and qz is the probability that an attacker will someday catch up with the main 
chain if he started his, alternative chain *z* blocks back.

For example, if an attacker possesses 10% of the processing power of all validators, the probability of a successful 
attack is less than 0.1% (on the assumption that honest nodes operate on the network with an equally fast message 
delivery).

### Reward for block creation 

Another important issue is the motivation for users to solve resource-intensive tasks, create new blocks, and confirm 
transactions, thus leaving no chances for intruders.

We have already described the processes such as block creation, coin issuance, and the imposition of fees for 
transaction confirmation. In Bitcoin, these processes are very closely related to each other. The fact is that, 
according to the rules of the protocol, the creator of a block can send to her own address a certain number of coins, 
which she literally took from nowhere, meaning that these coins did not exist previously—this is how coin issuance 
works. The total amount of a reward is calculated as the issued coins plus the fees for all transactions included in the 
block.

**reward = new (issued) coins + fees**

In 2018, the reward for creating one block is 12.5 coins plus fees. Note that, for security reasons, a validator does 
not receive this reward immediately after the block is created. There is a special parameter called *coinbase 
maturity* [29]. It indicates the minimum number of transaction confirmations for which the validator was rewarded. 
In Bitcoin, this parameter has a value of 100, which means that after the block is created, the reward (fees + issued 
coins) becomes available only after 99 further blocks confirm this block.

As noted earlier, a new block appears on average every 10 minutes regardless of the total computational power of the 
system. This is achieved by using the *difficulty parameter*, which is calculated by each node independently by the 
known algorithm and used to specify the requirements for solving the resource-intensive task. Over time this parameter 
is recalculated according to the change in computing power of the system.

All network nodes create a new block collectively in 10 minutes. For an individual participant, creating it may take 
hours, days, or even years. But according to the theory of probability, one block will be created on average every 10 
minutes provided that all validators work on one chain of blocks. This also means that turning off half the power of the 
system at some moment will increase the average block creation period to approximately 20 minutes. It will remain 
extended until the difficulty parameter is recalculated.

It is believed that only those who control more than 50% of the capacity can influence the processing of transactions. 
Bitcoin can be considered secure when most of the computational power of the system is controlled by honest validators. 
The confidence in Bitcoin as an accounting system, therefore, depends on the fact that thousands of people are unlikely 
to collude against a particular user (the opposite of a centralized system where censorship is easy to implement and 
hide). This means that trusting Bitcoin, a user is confident that thousands of people will not disagree with him at the 
same time. An attack in which an adversary controls more than half of the computational power of the system is called a 
*51% attack* (for more details, see 4.2). Bitcoin has never been exposed to this in practice (as of 2020). However, 
there are striking examples of 51% attacks in several digital currencies such as Bitcoin Gold and ZenCash, which cost 
more than 18 million US dollars in total.

### Influence of network disruptions on Bitcoin

Let's start by imagining the following fictional situation. One day, the sea beavers got drunk with pirate rum and cut 
all the fiber-optic cables, which passed on the bottom of the oceans and connected all continents into a global network. 
Data transmission channels between continents refused to operate, but nodes on each continent continued working 
correctly in their subnets. However, users of different subnets could not synchronize in order to exchange transactions 
and blocks. Therefore, on each continent validators formed different versions of the chain of blocks.

It is obvious that the majority of Bitcoin network nodes use a global network to communicate with each other, while the 
population of entrepreneurs-miners is unevenly distributed on the surface of the continents (Fig. 2.27). What can happen 
to Bitcoin if the Internet fails between six continents?

![Figure 2.27 - Exemplified network connection between continents](/resources/img/volume-1/2.5-confirmation-of-transactions-in-bitcoin/2.27-network-connection.png)

While Dr. Powderpill was grinding the teeth of all the sea beavers to prevent them from breaking the cables for a second 
time, and Inspector Gadget was mending the fiber optic cables with his electrical tape, validators continued creating 
blocks (Fig. 2.28). Since the computational power of validator groups on different continents is different and the 
difficulty parameter had not yet been updated, the period of block creation for each subnet significantly exceeded the 
default parameter, which is 10 minutes. Moreover, this period was different for each continent, which led to the 
creation of alternative chains of different lengths.

![Figure 2.28 - Non-uniformity of the database state on different continents](/resources/img/volume-1/2.5-confirmation-of-transactions-in-bitcoin/2.28-non-uniformity.png)

By the time Dr. Powderpill and Inspector Gadget finished their work and data transfer channels were restored, the 
Bitcoin network nodes from different continents started synchronizing with each other. Then it turned out that there are 
several different versions of the chain of blocks, all with a different length (Fig. 2.29). Moreover, different chains 
now contained different transactions, and only the subsets of these transactions could partially intersect.

In accordance with Bitcoin protocol rules, all nodes will eventually choose the longest chain and consider it as the 
primary one (*the mainchain*). Another interesting fact is that the transactions that were already confirmed but 
included in alternative chains have lost their confirmation status. Nevertheless, full nodes of the network continue to 
store and synchronize them, and validator-nodes, as always, are interested in including them in their blocks and receive 
the rewards, provided that these transactions do not conflict with those already confirmed in the mainchain.

![Figure 2.29 - Process of choosing the longest chain of blocks](/resources/img/volume-1/2.5-confirmation-of-transactions-in-bitcoin/2.29-choosing-the-longest-chain.png)

Therefore, transactions of the *orphan blocks* get to the list of unconfirmed transactions but will very likely be 
confirmed later. And even though some of the validators will not receive a reward for solving the resource-intensive 
task since there is a possibility that the transaction from the orphan block (which they created) will be confirmed in 
the mainchain by a different validator. Thus only one validator will receive a reward even though both worked with the 
same diligence.

In the situation described, there are risks of losing coins not only for Bitcoin validators (who lose both the reward 
for created blocks and transaction fees) but also for regular users who accept payments in bitcoins. These risks are due 
to the fact that the computational power of Bitcoin was used just partially to maintain the reliability of the currency. 
For example, users may not notice the split of the global network into subnets and continue receiving payments in 
bitcoins, which means that there is a risk that they will later find out that their balance is zero.

Pirates of the Caribbean Sea could well carry out a similar deception. Suppose that Captain Jack Sparrow somehow 
obtained a representation of a private key. On the address controlled by this private key—even before pirates taught 
beavers to drink rum and be mad—someone sent some bitcoins, which remained unspent. Once the beavers had turned the 
global network into six local ones, Captain Jack, without wasting time, decided to install a diesel engine on his Black 
Pearl in the port of the closest continent and arrange this payment in bitcoins. Then the ship, under a favorable wind, 
swiftly sailed to the second continent to refill with diesel fuel.

Pirates don't have a lot of time for their deception to work before communication is restored; on the second continent, 
no one knows about the first transaction by Captain Jack Sparrow. Arriving at the second continent, Jack again uses the 
private key to pay for refilling the pirate ship with bitcoins (Fig. 2.30). The local tanker waits for the first 
confirmation and accepts the coins that were actually spent on the first continent, buying the diesel engine.

![Figure 2.30 - Captain Jack Sparrow spending the same coins several times](/resources/img/volume-1/2.5-confirmation-of-transactions-in-bitcoin/2.30-captain-jack-sparrow.png)

Meanwhile, Dr. Powderpill and Inspector Gadget are promising to eliminate the threat and solve the communication problem 
very soon. Knowing how persistent Dr. Powderpill and Inspector Gadget are at their work, the pirates have in advance 
prepared the schedule for a swift around-the-world travel. Having a representation of a private key and a fast ship with 
a full tank of diesel fuel, they are ready to visit other continents to fill the hold with rum and buy a manual on 
studying quantum computing.

Obviously, after the nodes were synchronized, only one of Jack's transactions was confirmed—the one which paid the 
merchant from the continent with the biggest part of the Bitcoin computational power. The pirates had given the deceived 
traders a good lesson—check the network healthiness before accepting a transaction. The captain and his team are 
analyzing the events and think about improving their strategy—now they consider using a quantum computer.

**Common myths**

*Creation of a block in Bitcoin always takes 10 minutes.*

In practice, a block can be created in a split second or in an hour and a half. Time of creation of the next block is 
almost unpredictable. However, there is an algorithm that adapts difficulty so that blocks appear on average every 10 
minutes.

*If someone controls the majority of mining power, then he may violate the rules of the protocol.*

The greatest harm that the owner of most of the power can inflict is actually a successful attempt to double-spend his 
coins and also filtering or blocking someone else's transactions upon confirmation. This is only possible when an 
attacker has appropriate equipment. He cannot steal coins or perform an arbitrary issuance.

**Frequently asked questions**

*– What is the minimum number of confirmations a transaction should receive to be added to the chain of blocks?*

One confirmation. But in order to consider a transaction fully confirmed, there has to be more: 6 confirmations by 
default. Some services accept payments with two or three confirmations. In general, the number of confirmations depends 
on the amount of payment (and a risk of a double-spending attack; for further details, see 4.2) and can even exceed 100.

*– What is the probability that a correctly created transaction will be confirmed after it is sent to the network?*

The probability that a correctly created transaction is confirmed is close to 1 if it includes a sufficient fee and the 
process of creating new blocks is truly decentralized (is not censored).

*– Is it required to set up a full node and in what cases is it necessary?*

A full Bitcoin node is required if you want to accept and send payments and see the current version of the database 
without having to trust other nodes. Light clients, web wallets, mobile wallets, which do not store the entire chain of 
blocks, use trusted nodes to obtain up-to-date data. In other words, a node launched by a service itself keeps track of 
the current state of all transactions and current distribution of coins, while the light client (that trusts this node) 
receives this state to make the payments. In some cases having trustees is unacceptable; therefore you will set up your 
own full node, make an audit of its software, and no longer depend on the third-party services.

*– What is a fork? What kinds of forks are there?*

There are two different meanings of fork: the branching of the chain of blocks and the copying of the source code. In 
the context of blockchain, forks can either be planned or unplanned. Forks of the source code are usually performed 
either for the development of a separate project or in order to make some changes to the protocol for the purpose of 
testing them. At the same time, forks should not be confused with concepts such as *softfork* and *hardfork*: these are 
types of protocol updates (for further details, see 6.1).

*– Who in the Bitcoin system makes the decision to increase or decrease the difficulty?*

No one does. The difficulty is determined by the same rules, which are laid down in the software of each node of the 
network. Each participant updates the difficulty parameter in her software according to these rules. As a result, this 
parameter will be the same for all; the decision center is not required. This is the most important principle in the 
work of decentralized systems.

*– If 1 hour is initially set in the system for transaction confirmation, how could Bitcoin be used as a means of 
payment?*

In fact, 1 hour is only a recommendation that is better to adhere to by default. If you go deeper into detail, it turns 
out that the waiting time before accepting payment is determined by the recipient. And the larger is the transaction 
amount, the longer the recipient is interested in waiting. According to a rough estimate, one transaction confirmation 
is sufficient for accepting a payment of $1; for the amount of $1,000,000, it is 16 confirmations (for further details, 
see 4.2) [30]. In addition, there are methods for making payments through the so-called *payment channels*, where they are 
confirmed much faster than usual (*on-chain*) transactions, and the confirmation time does not depend on the amount of 
payment and the established fee (for more details, see 4.8).

[CRYPTOGRAPHY AND KEYS MANAGEMENT](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-1/en/3-cryptography-and-keys-management.md)