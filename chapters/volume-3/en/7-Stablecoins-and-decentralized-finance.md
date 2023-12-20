# [6 BASICS OF POST-QUANTUM CRYPTOGRAPHY](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/6-Basics-of-post-quantum-cryptography.md)

# 7 STABLECOINS AND DECENTRALIZED FINANCE


## 7.1 Approaches to creating stablecoins

_Stablecoin_ or _stable currency_ are definitions that are often used to denote a subset of digital assets having a stable, fixed price tied to a certain _benchmark_. The creation of a stablecoin is relevant since many people want to utilize an asset that will have several properties at the same time: the ability to store and transfer it digitally, a stable price, independence from banks, regulators and their restrictions. This section describes a few main methods to _peg_ the price of a stablecoin to a price of a certain price standard (a _benchmark_).

It should be understood that price stability is a _relative value_. It has no absolute value or universal measure units. In order to better understand this problem, one needs to create a list of common goods and services. For instance, it may include a liter of milk, a kilogram of wheat, and a subway ticket. On one hand, a particular person may consider the cost of these goods and services relatively stable, on the other hand, these goods and services are, to a certain degree, unstable as compared to each other. It follows that there is no universal unit of price measurement, so a stablecoin price can only be pegged to a certain benchmark, whether it’s an already existing product, service or other assets.

There are different ways to peg a stablecoin to a particular benchmark. In order to compare them, it is best to use the below-listed criteria.

> * _Peg stability level regarding the demand and supply_
> * _Peg maintenance cost_
> * _Pegging mechanisms transparency for users_
> * _Difficulty of determining the bounds of the stable peg state_

Generally, a particular stablecoin and its corresponding benchmark are traded on different platforms and in different volumes. Therefore, the demand and supply ratio for the stablecoin at certain time points can vary as compared to the demand and supply ratio for the benchmark. For example, if that stablecoin is pegged to one liter of fuel and there are, at the some time, more people who want to urgently sell this stablecoin than people who want to buy it, then people will naturally create sale offers even at a lower price (below the current price of one liter of fuel). This problem is primary for a stablecoin, and the pegging mechanism must solve it depending on certain assumptions.

Options to peg the stablecoin price to a certain benchmark can be provided via different methods: including centralized and decentralized solutions, with specific, self-executing smart contracts. An important criterion, meanwhile, is the cost of maintaining this system. It includes, for example, the cost of maintaining a sufficient number of independent oracles that monitor price changes of the benchmark and transfer actual data to the stablecoin management system itself, the costs of maintaining the validators of the accounting system, etc. Someone must cover these costs; this may be goods and services distributors, traders, or holders and users.

Transparency of the pegging mechanisms is another important criterion for a stablecoin. In the ideal case, anyone must be able to explore the principle of pegging of a stablecoin to a particular asset, to assure its theoretical operability, and to practically audit the system for making sure that it matches the theoretical model. Otherwise, that stablecoin will not gain trust and be widely spread.

The complexity of determining the boundaries of a stable pegging state is a direct result of the previously discussed criteria. If market participants cannot easily determine or agree on the points at which the binding will lose its stability, it becomes simple for an attacker to spread panic among users. In this way, an attacker can cause the avalanche effect: users themselves will destabilize the stablecoin without any apparent reasons. Thus, a transparent and easily analyzable pegging mechanism is less prone to manipulations and mood swings.

The approaches to creating stablecoins can be divided into the three below-listed basic groups.

> * _Complete collateralization with the corresponding benchmark_
> * _Collateralization with another digital asset for the equivalent price_
> * _No collateralization_


### Methods for pegging with a complete collateralization

The simplest option to create a stablecoin in terms of understanding its principles is to issue digital obligations in a centralized accounting system in which the owner has the corresponding amount of benchmark units (locked in the owner’s storage) for the entire amount of issued obligations. For example, a farmer issues one stablecoin each time he puts one liter of fresh milk into the warehouse. One stablecoin is redeemed each time someone comes to the warehouse and exchanges his digital obligation to a liter of milk.

Such an organization has the following advantages:

* The farm owner divides the flow of selling milk and the one of client service.
* In theory, it is impossible for the client to have a stablecoin while there is no milk in the warehouse.

An interesting feature is that people who believe in the stability of the price of milk can use this stablecoin to store their savings. However, this may not be advantageous for every benchmark. For the situation with milk, the more stablecoins are used to store savings, the more milk just stays in the warehouse and goes bad. Therefore, this approach to creating stablecoins is not suitable when the benchmark is expensive to store or has a short lifespan.

Another disadvantage of this approach is that the peg of a stablecoin to a particular benchmark is maintained by a single party, and the peg cannot be easily verified by anyone. Therefore, one can use such a stablecoin only if they fully trust the system owner. It also follows that stablecoins from different issuers (e. g., different farmers) are generally non-fungible even if they are pegged to one benchmark.

Regarding the methods for pegging with complete collateralization, note also that there are cases when the collateral is stored in a centralized manner (by a single company) and the stablecoin is issued in the same manner (by the same company), but the accounting is done in a decentralized system. In such cases, only one of the processes is transparent and independent; it makes very little sense in the context of the reliability of the entire system.


### Collateralizing the stablecoin with another digital asset

The approach to creating stablecoins where another digital asset is used as collateral can be applied in a permissionless decentralized accounting system. This is the option that we will discuss further. In this case, the process of issuing a stablecoin and storing collateral is controlled by a smart contract. Theoretically, such a stablecoin will be trustless since the system is independent and fully auditable. However, it is important to understand that this approach only works if the stablecoin itself and the digital asset with which it is ensured/supplied are issued and accounted in the same system; i.e., it is impossible to create a stablecoin that is tied to one gram of gold and is provided for an equivalent amount by bitcoin.

The most common way to implement this approach involves the use of the so-called contract for difference (CFD). CFD is a popular stock exchange tool that is used to insure against changes in the asset price. By concluding a CFD, the buyer and seller agree to refund the difference between the current price of a certain amount of the asset units and the price of this amount at the time of completion of the contract.

For example, if the price of one liter of milk in dollars increases by a certain amount, the seller will pay the buyer this amount, and if the price of one liter of milk decreases, then the buyer will pay the difference to the seller. At the same time, they don’t pass the milk to each other and may not even physically have it. One party simply pays another the difference in price that emerged during the life of the contract.

Obviously, in this example, to close (pay off) the contract for the difference in prices, one needs to pay only the amount by which the price of one liter of milk has changed. Thus, if it has changed by 10%, then to repay CFD only 10% of the price of a liter of milk is needed. In practice, the settlement between the parties does not occur after the contract is closed, but exactly at the moment of its closing. Moreover, both parties want to have a guarantee that the opponent will be solvent at the time the contract is closed. Thus, when the contract is opened, the parties freeze a certain amount of funds in the same contract to use it as collateral. For example, the parties create CFD equal to the price of one liter of milk in dollars and deposit 20% of the cost of one liter of milk in dollars as collateral. Such a contract can be closed in two ways: upon the initiative of one party or automatically by a margin call when the price of one liter of milk in dollars changes by 20% or more. The second case means that the difference that needs to be paid is actually equal to the maximum amount that one of the participants is ready to pay.

The amount of collateral usually depends on the volatility of the asset price. For assets with a relatively stable price, this can be several percent. However, for assets with a highly volatile price, it can be up to several tens or even hundreds percent. In any way, the main advantage of CFDs is that it allows one to trade an asset without actually having it.

Now let’s look at how to use CFD in a decentralized accounting system to issue stablecoin, the price of which is pegged to an arbitrary benchmark. The difference here is that the contract is signed not between the two counterparties, but between the merchant and the entire accounting system. To better understand the features of the smart contract in these conditions, consider an example of a certain merchant issuing one stablecoin, which is tied to the price of one liter of milk, where a CowMeow supermarket chain will be the source of the price.

So the merchant is an accounting system user who owns a certain amount of base currency. This currency will be used as collateral for the issued stablecoin. Suppose it is currently known that one liter of milk costs 100 units of the base currency in the CowMeow chain. Therefore, the merchant, when making up a contract for the issuance of one stablecoin, sets the amount of collateral equal to 120 units of the base currency – cost equivalent + 20% of the collateral deposit. After this contract is signed by the merchant and accepted by the accounting system, 120 units of the base currency are removed from its balance sheet and frozen in the contract, and in return, he receives a new stablecoin.

In this accounting system, the stablecoin is a guarantee that its owner at any time can exchange it for the base currency in the equivalent of the price of one liter of milk from the CowMeow chain. The merchant can send this stablecoin to any other user of the system or can create an offer to sell it on the internal exchange of this decentralized accounting system (DEX).

In this system, the internal exchange plays a very important role in guaranteeing the correct closing of contracts in case of a margin call. Suppose a merchant donated his stablecoin to an orphanage as a charity. But while the children went to the nearest CowMeow supermarket for shopping, the price of one liter of milk in this store increased by 20% in the base currency, which was used as collateral for stablecoin. What will happen in the system at this moment? An accounting system needs to know the current price of the benchmark to which stablecoin is attached to quickly process similar situations. Therefore, there are special oracles that monitor the state of foreign markets and record the current state in a joint database of the accounting system. Now a smart contract for executing a margin call needs to use collateral in order to redeem one stablecoin at the current price at the internal exchange and destroy it. At the moment, the price is 120 units of the base currency, which is equal to the total amount of collateral. As a result of the automatic closure of the contract, the merchant receives a notification that his CFD has been paid off, and milk lovers still go to the store with the same stablecoin and can use it even if the price for it in the base currency increases even further.

Let’s now consider what happens if the price of one liter of milk in the base currency decreases. It is worth noting that this does not automatically close the contract. In fact, if the price drops by 20% and will be 80 units while the collateral will remain 120 units, the actual size of the collateral will be 40 units. In such a situation, the merchant can initiate the closure of the contract. Then one stablecoin will be bought and destroyed at the current price on the internal exchange, and the merchant will get back the remaining 40 units of the base currency; if the merchant has the corresponding stablecoin on the balance, it will be destroyed and the entire amount of collateral will be returned to the merchant. The merchant can decide that 40 units of collateral is too much and take back 20 units or change the contract and issue 0.2 stablecoin on the collateral of this collateral.

This approach to creating stablecoin will work only under certain assumptions: in addition to ordinary users, the system must have merchants who open contracts and issue a sufficient amount of stablecoins, there must be a lot of liquidity on the system’s internal exchange that will allow automatic redemption, and many oracles must support the relevance of the information about the value of the benchmark in foreign markets at all times. In addition, it is worth considering other assumptions related to the features of the decentralized accounting system.

A good example of putting this approach into practice is the Bitshares decentralized platform. The most popular stablecoins released on this platform at the time of March 2020 are bitUSD (over 2 million units released) and bitCNY (over 28 million units released), pegged at the price of the US dollar and Chinese yuan respectively.


### Methods for pegging with no collateralization

The distinction of the non-collateralizing pegging methods is that the binding mechanism actually changes the amount of stablecoin in circulation. This, in turn, impacts the demand and supply ratio (for a given stablecoin). Theoretically, the price of a stablecoin supported in this way should fluctuate in a narrow range around the price of the benchmark itself. This approach is similar to how the price of most fiat currencies is maintained by the central banks that issue them. An important feature of this type of stablecoin is that the issuer is not obligated to pay it off at any price, i.e. they can only pay someone or sell it on the exchange.

Consider the example of how the price binding mechanism works in this scenario. Suppose a certain group of farmers decided to launch a joint system for stablecoin accounting. They try to keep its price equal to the cost of one liter of milk in the CowMeow supermarket chain. They issue a certain amount of stablecoins in this system and immediately put them up for sale at the price of a liter of milk in a supermarket.

If the demand for stablecoin turns out to be so large that the price of stablecoin on exchanges exceeds the current price of milk, the farmers make a collaborative decision to issue new stablecoins and put them up for sale at exactly the price of a liter of milk in CowMeow. Thus, the increased demand for stablecoin is satisfied.

If the offer begins to exceed the demand and the price of stablecoin falls below the cost of milk on the shelves of the store, then according to the rules of the accounting system, the reward that stablecoin holders can receive if they freeze it for a while grows. This rule is implemented using a smart contract, which at the request of the user freezes his stablecoin for some period of time and pays him a reward in the base currency. The amount of the reward depends on the time for which stablecoin is blocked and the deviation of the stablecoin price. Thus, the system motivates participants to withdraw stablecoin from circulation when it is necessary to cope with an increased offer. As a result, an increase in remuneration stimulates the demand for stablecoin and reduces its supply.

This approach to creating stablecoins can also be implemented in a decentralized accounting system, but there is one interesting question. Who will be the owners of the stablecoin that has just been issued? And who will get the proceeds from their sale? A good example of applying this approach in practice is the NuShares system, which issues and accounts stablecoin NuBits tied to USD. However, in the spring of 2018, the pegging mechanism did not cope with its task and the price of NuBits collapsed. Nevertheless, this system contains answers to some important questions about the very approach to creating a stablecoin.

In NuShares, the issuance of new NuBits takes place only by decision of the holders of the base currency. NuBits that were issued as a result of voting are immediately sent to the exchange for sale, and the earnings from their sale are used to cover operating expenses, support the project and pay dividends to holders of the base currency.

### \*\*\*Frequently asked questions\*\*\*

_– What will happen if the benchmark to which a particular stablecoin is pegged gets into a long and uncontrolled devaluation?_

Suppose a particular stablecoin is pegged to the cost of buckwheat. Suddenly, scientists prove that buckwheat is very harmful as a food product – and it becomes much cheaper as compared to any other cereal (e. g., rice). At the same time, the mechanism for pegging the cost of a stablecoin to buckwheat keeps working, so the cost of this stablecoin is still equal to the cost of buckwheat relative to rice.

However, the cost of this stablecoin may no longer match the cost of the benchmark if the stablecoin peg mechanism is insufficiently unstable, but this is another issue.


## 7.2 Operating details of stablecoins

Stablecoins are a class of digital assets pegged to one of fiat currencies, most often to the US dollar.

USDT, or Tether, can be considered the first successful stablecoin project. The project was launched in 2014 and until now, with the exception of rare cases, its price has not deviated by more than 0.05–0.1 units from the dollar US (Fig. 7.1) [125].

![Figure 7.1 – Graph of the exchange rate of USDT to the US dollar](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.1-exchange-rate-of-USDT-to-US-dollar.png "Figure 7.1 – Graph of the exchange rate of USDT to the US dollar")

Over the 8 years of its existence, USDT has become popular and the total volume of these stablecoins in circulation has exceeded 80 billion US dollars [125].

![Figure 7.2 – Graph of the amount of USDT issued](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.2-amount-of-USDT-issued.png "Figure 7.2 – Graph of the amount of USDT issued")

Stablecoins are important in the digital asset ecosystem because they actually provide pricing for the most in-demand virtual assets like bitcoin (BTC) and ether (ETH). The initial rejection of the digital economy by the classical market and the extremely limited opportunities of trading for fiat money forced enthusiasts to create a tokenized version of the US dollar. Today there are dozens of different stablecoin projects.

The total volume of issued stablecoins as of May 2022 exceeds 180 billion US dollars [126]. The top 2 stablecoins by market capitalization are Tether (USDT) and Circle (USDC). Both are tokenized versions of the US dollar. The highest by capitalization non-dollar stablecoins pegged to the euro are EURT (Tether) [127] and STASIS EURO [128] with the capitalizations of 250 million USD and 130 million USD, respectively, as of May 2022.

The growing popularity of stablecoins is due to the following factors:

* Stablecoins are open to any user practically 24 hours a day 7 days a week
* Transfers are made in accordance with the rules and the features of the accounting system in which the stablecoin is issued and in circulation, i. e. in the Ethereum system, the transfer is made in a few minutes
* Most often, stablecoins are easy to use in smart contracts and are therefore available in many decentralized financial applications (DeFi DAPPS)

Next, let’s analyze the basic principles of issuance and the pegging mechanisms of stablecoins. Conventionally, all projects presented on the market can be distinguished by the two criteria:

* Issuance rules
* Collateralization method

The corresponding classification of stablecoins is presented in Fig. 7.3.

![Figure 7.3 – Classification of stablecoins](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.3-classification-of-stablecoins.jpg "Figure 7.3 – Classification of stablecoins")


### Centralized stablecoins

The simplest option is a stablecoin issued by a centralized organization and collateralized with a benchmark asset in 1-to-1 ratio. For example, for each issued USDT, USDC and USDP token, there is one US dollar in the issuer’s bank account.

Note that dollar tokens are not always collateralized only with US dollars in a bank account. For example, in the case of Tether, it is about collateralization in the form of reserves with the following composition [129] as of May 2022:

* 83.74 %: cash, its equivalents, and other short-term deposits and commercial papers:
    * 52.41 %: US treasury bills;
    * 36.68 %: commercial papers and certificates of deposit;
    * 6.36 %: cash dollars and bank deposits;
    * 4.55 %: money market funds;
* 6.38 %: other investments (including digital tokens);
* 5.27 %: secured loans (none to affiliated persons);
* 4.61 %: corporate bonds, funds, and precious metals.

That is, strictly speaking, only 6.36 % of the entire USDT collateral is very dollars in the bank account, which is even less than the share of “other investments” (6.38 %). Undoubtedly, the lion’s share of the collateral is made by extremely conservative investments in US treasury bonds, commercial papers and certificates of deposit. Quarterly collateralization audits allow assessing the composition and quality of the collateral and, through public reports, assuring USDT users that the claimed state matches the real one.

The project with the second-highest issuance volume – USDC – is more conservative than USDT. The monthly audit opinion contains the phrase that the total fair value (in US dollars) of assets held in separate accounts is at least equal to the value of 51,388,555,903 USDC tokens in circulation as of March 31, 2022 [130].

Thus, the degree of trust in a particular centralized stablecoin collateralized with US dollars and similar assets comes down to the trust in the issuing organization and the auditors who verify the reports.

A feature of centralized stablecoins is the ability to block funds at specific addresses. That is, the corresponding smart contract has a function that allows the issuer to block funds at a specific address at the request of state authorities.

In the case of USDC, such addresses are published on the website of the issuing company [131].

The advantage of centralized stablecoins is the ease of their issuance and accounting. The disadvantages are their complete dependence on the issuing organization and on the quality and the safety of the collateral as well as centralized control with the ability to block funds at specific addresses.


### Decentralized stablecoins

The next step in the development of stablecoins is decentralized protocols for their issuance and maintenance, built on smart contracts. In this case, there is no single person or organization that would issue stablecoins at its discretion, but there is a system of smart contracts that are responsible for the decentralized issuance and circulation withdrawal of the corresponding tokens.

The earliest decentralized stablecoin project and one of the most successful is the Maker DAO project with the decentralized DAI stablecoin [132]. It originated in 2014 in the form of a decentralized autonomous organization (DAO) launched on the Ethereum accounting system. The governance token in the DAO governance system is MKR. It is utilized for voting and primary decision-making regarding the ways of the project development. Both digital currencies (e.g. ether) and tokens or stablecoins (e.g. USDC) can be used as the collateral for DAI. Depending on the level of risk accepted by the system, the collateralization ratio in digital assets can range from 102 % to 175 % [133]. A special module – PSM USDC (peg stability module) [134] – is used to issue DAI against USDC collateral in 1-to-1 percentage ratio. With it, as of May 2022, 3.8 billion DAI were issued [135].

As of May 2022, the Maker DAO ecosystem has issued 8.5 billion DAI with the collateral worth $13.9 billion [136]. Thus, each DAI is collateralized with $1.64, or by 164 %.

A high-level schematic representation of the Maker DAO protocol is shown in Fig. 7.4 [137].

![Figure 7.4 – The main participants of the Maker DAO system](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.4-main-participants-of-Maker-DAO-system.png "Figure 7.4 – The main participants of the Maker DAO system")

Unlike centralized stablecoins, in Maker DAO, the DAI issuer can be any user who owns the amount of a collateral asset (for example, ether) required for the issuance. To perform the issuance, the user must send ether to a special smart contract of the system, called a vault. The minimum amount of DAI issued for an ETH-A type vault under the conditions of the collateralization ratio of 145 % and the annual fee (the stability fee) of 2.25 % [138] is 15,000 DAI. All risk parameters and fees are approved by MKR token holders’ voting (MKR Governance). Having chosen the permissible amount of DAI issuance, the user sends a transaction that triggers the system’s smart contract; as a result, he receives DAI at his address. At the same time, in the vault, the system locks the amount of ether which matches 145 % of the released stablecoins. If the user wants to withdraw the locked collateral, then he must call a special method of the smart contract and return the debt with the matching amount of DAI, counting the accumulated interest of the stability fee. At the same time, the smart contract redeems the DAI and returns the collateral to the call initiator’s address.

An example of a vault (vault 27448) is presented in Fig. 7.5 [139]. The stETH wrapped asset (these are ether obligations invested in the Lido Ethereum service) is used as collateral. The minimum collateralization ratio is 160 %, the real one is 357.63 %. As soon as the minimum collateralization ratio is reached, the vault is liquidated. That is, in order to avoid liquidation, the vault owner must maintain the ratio above the minimum with some reserve due to the volatility of the collateralized asset against the US dollar. The annual fee is 2.25 %. The issuance in the vault was 8.2 million DAI. The pledge is 10,275 wstETH tokens worth more than $29.3 million USD.

![Figure 7.5 – Example of a vault in Maker DAO](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.5-vault-in-Maker-DAO.png "Figure 7.5 – Example of a vault in Maker DAO")

Let's go ahead from DAI users to another group of Maker DAO system participants – keepers. Keepers are particularly responsible for the process of liquidation of users’ positions, for which the debt amount exceeds the collateral amount defined by the protocol.

Let’s analyze the liquidation process via the following example. Suppose we deposit the ether collateral worth \$ $100,000$ to the ETH-A vault. Against this collateral, we issue 50,000 DAI. Therefore, the collateralization ratio is $\frac{100,000}{50,000} \cdot 100$% $=200$. The minimum collateralization ratio value for an ETH-A vault is 145 %. In a while, the price of ether starts to decrease, and so does the cost of the collateral of the issued DAI. As soon as the value of the collateral becomes lower than the threshold value, namely $50,000 \cdot 145$% =$72,500$ (USD), keepers intervene: they take the collateral and repay the loan in DAI instead of the user. Doing this is profitable for the keeper, because she buys ether for DAI at a large discount, since, as before, at the time of redemption, the ether collateral is 145 % of the cost of the issued DAI. From the 45 % difference, a fee of 13 % is paid for the liquidation of the position, and the rest is taken by the keeper as a reward for timely liquidation. Note that anyone can be a keeper.

There are situations when the collateral cost decreases so rapidly that keepers do not redeem the collateral in time when its cost is from 100 % to 145 % of the cost of DAI. In this case, it is economically infeasible for the keeper to repay the loan instead of the user, since the cost of the collateral that would be received is less than the cost of the DAI that would be given. Therefore, positions are liquidated at the protocol level using the accumulated system surplus, which as of May 2022 is approximately 70 million DAI. The system surplus is accumulated from the users’ fees for DAI issuance (from the stability fees). If these funds are not sufficient, MKR governance tokens are automatically issued and sold for DAI. The funds received from the sale of tokens are used to liquidate positions with a broken ratio of the collateral to the issued DAI.

In addition, the Maker DAO system provides price oracles. These participants provide the system with data on the current cost of all assets available in the system. Exactly on the basis of the information received from price oracles, the liquidation of positions is launched, the maximum amount of DAI that can be issued against a particular collateral in a system’s vault is determined, etc.

There is also the role of developers, who are responsible for improving the protocol and correcting errors. As soon as the governance system decides to make changes to the protocol, developers directly update the software code. The development is financed from the system surplus by decision via the MKR token holders’ voting.

System governance is performed in two main directions: general management and risk management team. Let’s start with the last one.

The risk management team is responsible for the timely change of risk parameters of the protocol, particularly for the assessment of new digital assets that are added as collateral for DAI issuance. For each such asset, the liquidation ratio, the debt ceiling, and the stability fee are determined.

As of May 2022, the system allows utilizing the collaterals listed in Table 7.1 [133]. According to the table, the system accepts 31 assets as a collateral for the maximum amount of 34.6 billion DAI.

![Table 7.1 (1) – Collateral asset parameters in Maker DAO](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/T-7.1-1-en-collateral-asset-parameters-in-Maker-DAO.png "Table 7.1 (1) – Collateral asset parameters in Maker DAO")

![Table 7.1 (2) – Collateral asset parameters in Maker DAO](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/T-7.1-2-en.png "Table 7.1 (2) – Collateral asset parameters in Maker DAO")

For each type of collateral, the risk management team defines the following parameters:

* A debt ceiling is the maximum amount of debt that can be created with one collateral type. The governance system assigns each collateral type a debt ceiling, which is used to ensure sufficient diversification of the system’s collateralization portfolio. Once a collateral type reaches its debt ceiling, it becomes impossible to create new debt until some existing users pay off their debts in whole or in part.
* A stability fee is the system’s annual percentage income, which depends on how much DAI was issued against a collateral of any of the assets in Table 7.1. The stability fee is paid only in DAI and then becomes the income of Maker DAO.
* A liquidation ratio. A low ratio means that collateral price volatility is low; a high ratio means the high collateral price volatility is expected. Keepers cannot redeem the collateral of each particular vault until the collateralization ratio in that vault falls below the defined value.
* A liquidation penalty is a fee that is added to the total amount of unredeemed DAI charged by the vault and subject to liquidation. 

Management decisions are made by MKR token holders via voting. This is the main governance level of the DAO. As an example, we refer to the vote on adding a new CF-DROP asset as collateral. The decision itself was made on July 23, 2021: 155.24 MKR tokens were used to vote for [203].

The governance mechanism involves two types of management [204]:

* Proactive
* Reactive

Proactive management includes discussion, decision-making and automated implementation. Its particular example is the process of accepting a new token as collateral and determining its risk parameters.

Reactive management includes procedural intervention. Its particular example is increasing or decreasing a particular debt ceiling due to an increase or decrease in the collateral liquidity.

MKR token holders’ voting can take two forms:

* Governance polls
* Executive votes

Governance polls allow decisions to be made on one case or a group of cases (for example, on adding new oracles or a new risk group).

Executive votes allow system settings to be adjusted. For example, they may relate to specific risk parameters of a new collateral type.

In general, the governance system makes decisions regarding the following cases:

* Adding a new asset as eligible collateral for DAI issuance with a new set of risk parameters
* Adjusting the risk parameters of one or more existing collateral assets or adding new risk parameters to one or more existing collateral assets
* Allocating funds from system revenues to pay for various infrastructure needs and services, including oracle services and related risk management research
* Adjusting DAI retention rate
* Selecting a set of price oracles
* Selecting a set of emergency price oracles
* Activating the emergency system shutdown
* Restoring the system

Therefore, the responsibility for the operation of Maker DAO rests entirely with the MKR token holders. Within the smart contract, a vote is initiated on the adoption of some decision. Any user having MKR tokens can support this decision by voting for it, and the weight of the vote will be equal to the number of tokens on that user’s balance. The corresponding decision will be made if the total weight of the votes reaches a threshold value (for example, 2 % of all issued MKR tokens). Depending on the decision importance and complexity, the corresponding vote may use a different threshold value for approval.

The adopted decision enters the governance security module [140]. The main function of this module is to enable all participants in the system to double-check the decision and its consequences before its immediate implementation.

The security module encapsulates the updated operating code of the system and stores it for final review within 24 hours. This way, all interested parties (especially those responsible for system shutdown) can test the updated code before implementing it to ensure the integrity of the system.

At the beginning of its development, the Maker DAO project used digital assets, primarily ether, as collateral. The system allows you to invest ether in three types of vaults:

* ETH A: liquidation ratio 145 %, fee 2.25 %
* ETH B: liquidation ratio 130 %, fee 4 %
* ETH C: liquidation ratio 170 %, fee 0.5 %

That is, the lower the liquidation ratio, the higher the fee of the system and vice versa.

In addition to ether, you can use some other digital assets (see Table 7.1): WBTC, stETH, LINK, YFI, MATIC, UNI, MANA, renBTC. The total amount of DAI issued as collateral for digital assets is 21.53 billion DAI.

The next asset class is liquidity tokens, or LP tokens of decentralized exchanges. They can be considered a subspecies of digital assets. They are shares in a liquidity pool, for example, DAI and USDC token pairs with a 0.01 % exchange fee on the Uniswap exchange. This pair is one of the leaders in terms of liquidity on the Uniswap V3 decentralized exchange (its liquidity is $354 million) [141]. Liquidity tokens that can be used as collateral for DAI issuance (see Table 7.1): GUNI-DAI/USDC 0.01 %, GUNI-DAI/USDC 0.05 %, DAI/USDC UNISWAP LP, USDC/ETH UNISWAP LP, WBTC/ETH UNISWAP LP, DAI/ETH UNISWAP LP, WBTC/DAI UNISWAP LP, Curve stETH/ETH, UNI/ETH UNISWAP LP.

When liquidity tokens of pairs of two stablecoins DAI and USDC are used as collateral, 100 DAI can be obtained by pledging 102 units of LP tokens (that is, the liquidation ratio is 102 %). When at least one token in the liquidity pair is a volatile virtual asset, the liquidation ratios are much higher and range from 120 % to 160 %.

The total amount of DAI that can be issued against liquidity tokens is 2.18 billion DAI. The largest part of this amount is DAI, which is issued against the collateral of DAI and USDC LP tokens. Thus, the system incentivizes the increase in available liquidity to exchange DAI for USDC and vice versa.

A special peg stability module (PSM) was developed to ensure the stability of DAI with respect to the benchmark, that is, the value of the US dollar. It allows you to exchange a certain amount of DAI at any moment for the same number of units of a specific stablecoin (such as USDC) with a system fee of 0 %.

Currently, 3 types of stablecoins are available for exchange within the PSM module (see Table 7.1): USDC, USDP and GUSD. The system allows exchanges of DAI for other stablecoins in the amount of up to 10.56 billion DAI. The lion’s share of this limit falls on USDC – 10 billion DAI.

The PSM module allows you to charge the following fees [134]:

* Fee in (tin). The interest fee charged when a collateral asset is exchanged for DAI in PSM
* Fee out (tout). The interest fee charged when PSM exchanges DAI for collateral

As of May 2022, these fees are 0 %.

Note that the volume of transactions that PSM can offer is limited by its debt limit and the current amount of collateral tokens. PSM cannot offer DAI for exchange if the debt limit is reached. Similarly, PSM cannot offer a collateral asset for exchange if PSM does not have collateral tokens. That is, if, for example, USDC PSM has a limit of 1000 USD, then no more than 1000 USDC can be exchanged for 1000 DAI. On the other hand, if users have already exchanged 500 USDC for 500 DAI and there is 500 USDC left in PSM, then users who want to reverse exchange DAI for USDC can exchange no more than 500 DAI for 500 USDC.

Exchange through PSM is possible only between stablecoins pegged to the same benchmark asset.

An important component of the ecosystem of decentralized finance is the so-called lending protocols, or DeFi banks. The largest of them are AAVE and Compound. Their purpose is to provide the ability of decentralized borrowing of some tokens against the collateral of others. For example, you deposit 1 ETH into such a system and you can borrow DAI against this asset. At the same time, in the context of risk management, each asset has its own leverage ratio and its own interest rate curve.

The system works in such a way that the more funds from the DAI liquidity pool are borrowed, the higher the interest rate on such loans. The formulas for increasing rates depending on the utilization ratio of the liquidity pool are detailed in the descriptions of the protocols themselves [142].

The essence of the Direct Deposit Dai Module (D3M) is that when the interest rate in the lending protocol for loans exceeds the value approved by the Maker DAO management decision (4 % for the AAVE protocol [143]), anyone who wants to can call the smart contract lending protocol and balance the amount of DAI in the liquidity pool by taking the required amount of DAI directly from the Maker DAO [144]. At the same time, Maker DAO receives tokens of the liquidity pool of the lending protocol as collateral. In the case of AAVE, these will be aDAI tokens. The process of adding and subtracting liquidity is shown in Fig. 7.6 [145].

![Figure 7.6 – D3M module operation scheme](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.6-D3M-module-operation.jpg "Figure 7.6 – D3M module operation scheme")

In a similar way, liquidity is returned from the lending protocol: when the interest rate on loans falls below 4 %, the smart contract function is called, which allows you to take excess liquidity and repay the Maker DAO loan in favor of the lending protocol [146].

Table 7.1 shows the parameters of the D3M module working with the AAVE protocol. The debt ceiling is 300 million DAI. The actual rate that Maker DAO receives (2.81 %) corresponds to approximately the 4 % lending rate in AAVE.

The general approach of the D3M module and its ability to interact with other lending systems is shown in Fig. 7.7 [147].

![Figure 7.7 – Scheme of D3M module’s interaction with lending protocols](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.7-D3M-module_s-interaction-with-lending-protocols.png "Figure 7.7 – Scheme of D3M module’s interaction with lending protocols")

The advantages of the D3M module are obvious. It allows you to keep the cost of DAI borrowing in the AAVE system at a predictable level of no more than 4 % per annum. This makes DAI an attractive stablecoin for borrowing as compared to its counterparts, where the borrowing rate is variable and can sometimes exceed double digits (which forces borrowers to overpay for loans). Fig. 7.8 shows the dynamics of DAI borrowing rates in the AAVE system before the introduction of the D3M module [148].

![Figure 7.8 – Interest rate of DAI borrowing in the AAVE system](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.8-interest-rate-of-DAI-borrowing-in-AAVE-system.png "Figure 7.8 – Interest rate of DAI borrowing in the AAVE system")

The last class of assets that is permissible as collateral for the issuance of DAI is real world assets (RWA) (see Table 7.1). The total limit for this asset class is 59 million DAI. The issuance of DAI against the collateral of real assets can be considered on the example of the limit of 15 million DAI approved by the RWA001-A management system.

American company “6s Capital LLC” [149] works with several commercial real estate developers throughout the United States. Developers build commercial real estate, which is then leased out on a long-term basis. “6s” provides secured loans of up to 100 % of the construction cost to finance the construction and subsequent acquisition of commercial real estate [150].

“6s” came into being during the banking credit crisis caused by COVID-19: then its subsidiary “6s Development LLC” had problems with banks no longer willing to provide construction loans. “6s Development LLC” has been engaged in construction and long-term commercial leasing for 15 years and has built more than 1,000 properties. None of the projects financed by “6s Development LLC” was in default [150].

MakerDAO provides credit in exchange for a risk-adjusted ability to foreclose on the assets used as collateral. In this context, MakerDAO will use real assets as collateral, and at the same time, it is necessary to determine how the credit position can be liquidated or the collateral collected if the borrower has a problem with returning the DAI provided.

In the real world, it is not possible to use a digital oracle to determine whether a credit default has occurred, which will then trigger a liquidation. “6s” and Maker DAO must first agree on the type of real-world events that could cause “6s” credit to be liquidated. Let the reason for the liquidation be a violation of the terms of lending by “6s”. A digital oracle cannot go to court and file a claim against “6s” for breach of the loan agreement. This can only be done by a person who has the right to sue on behalf of Maker DAO, i.e. the real-world representative of Maker DAO. MakerDAO and “6s” may agree on commitments/requirements/agreements and commit those terms during DAO token voting, but the same agreed terms must then be set forth in a written credit line agreement signed by a real person on behalf of and on behalf of Maker DAO.

So, for the organization of the credit line, the legal structure depicted in Fig. 7.9 [150].

![Figure 7.9 – Legal structure of DAI lending for an RWA collateral](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.9-legal-structure-of-DAI-lending-for-RWA-collateral.png "Figure 7.9 – Legal structure of DAI lending for an RWA collateral")

The legal entity registered in the Cayman Islands is used as a Maker representative. It also acts as a tax agent (Tax Favorable Entity, TFE) acting through a trust company (Trust).

All transactions with DAI in the world are carried out by the representative of the Maker (TFE) through the trust company (Trust).

A lending company (LendCo) is a lender secured with real world assets. LendCo will use the USD received and its available capital to make secured loans to borrowers (BorrowCo).

Credit is issued and returned according to the following scheme (see Fig. 7.10) [150]:

1. New DAIs are issued in favor of TFE.
2. TFE transfers DAI to Trust:
    1. TFE sends DAI to the US dealer broker to its ether address.
    2. TFE instructs the dealer broker to exchange DAI for USD in a trust account with the dealer broker.
    3. TFE instructs the dealer broker to transfer US dollars to the bank account of the Trust Company.
3. The trust company issues a loan in US dollars to the lending company (LendCo).
4. The credit company conducts credit transactions and either repays the loans to the trust company or returns the capital in another credit transaction with the amount approved by the Maker DAO.
5. The loan company repays the loan to the trust company in US dollars. LendCo transfers USD to the trust company’s account at the dealer broker and instructs the dealer to exchange USD for DAI.
6. The trust company returns the TFE’s DAI to the ether address.
7. The TFE returns the DAI to the Maker repository where they are burned.

![Figure 7.10 – Scheme of issuing and returning DAI for an RWA collateral](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.10-issuing-and-returning-DAI-for-RWA-collateral.jpg "Figure 7.10 – Scheme of issuing and returning DAI for an RWA collateral")

Maker DAO controls and approves conditions for the credit company in 5 main areas:

* Cumulative limit of debt (in our case – 15 million DAI)
* Interest rate (3 %)
* Purposes of the loan (Maker DAO can change them at the request of the loan company)
* Collateral requirements for each credit company deal (Maker DAO may change these at the credit company’s request)
* Liquidation conditions

Maker DAO is an innovator and a pioneer in the field of lending against real assets, when stablecoin is not used inside the crypto industry, but gets into the outside world. This allows us to finance the construction of commercial real estate in the US in our example.

For stablecoin DAI , real-world projects are a good way to maintain its peg to the US dollar. If DAI becomes cheaper than the dollar, it is more profitable for real-world borrowers to repay their loan in DAI because they receive real-world income in real US dollars. Otherwise, it is more profitable for borrowers from the real sector to increase the volume of credit and, therefore, receive more dollars per DAI.

The next step in the development of lending to the real sector with the help of DAI is the agreement with “Huntingdon Valley Bank”, which Maker DAO approved on April 18, 2022 [151]. In the future, the credit limit of this small American bank may reach 1 billion DAI [152].

Next, let’s consider the important process from the point of view of loans – the liquidation of pledges. Liquidation is the process of selling collateral to cover the amount of DAI that a user has emitted from their vault. Liquidation ensures that DAI is always secured with the appropriate amount of collateral, by closing vaults whose collateral level is lower than the minimum required collateralization ratio for a particular type of collateral.

A vault can be liquidated if the value of its collateral falls below the minimum required level, which is called the liquidation ratio.

Since the system is decentralized, liquidation is carried out by third parties, system users or a separate group of users called keepers. They buy back the collateral at a discount, redeeming the issued DAI when a particular vault becomes undercollateralized.

Maker DAO oracles provide the system with price data that is used to monitor vaults, and in the event of a violation of the collateralization ratio, any keeper can initiate the liquidation of the vault.

The following formula is used to calculate the collateralization ratio: $R_{Y}= \frac{Y \cdot P_{Y}}{Z} \cdot 100 \%$, where _R<sub>Y</sub>_ is a collateralization ratio, _Y_ is a collateral amount, _P<sub>Y</sub>_ a collateral price, and _Z_ is an issued amount of DAI. For example, a vault with the 150 % collateralization ratio requires at least $1.50 of collateral value per issued unit of DAI.

If the value of the collateral falls to USD 1.49 (or below), it will be liquidated to cover the DAI issued and the liquidation penalty.

A liquidation penalty is a fee paid by owners of vaults when the value of their collateral reaches the liquidation price of the vault.

The liquidation penalty is added to the total amount of outstanding DAI issued by the vault when liquidation occurs. This causes most of the collateral to be sold at auction.

Profits from liquidation fines are used to replenish the system surplus or burn MKR tokens.

The Maker DAO protocol now uses Dutch auction version 2 liquidation. The essence of this auction is that first the highest price of the asset being sold is announced, and then it is gradually reduced until the first buyer agrees to it. After that, an agreement is made [153].

The protocol allows for the redemption of the pledge in parts. Thus, the first buyer agrees to buy back part of the assets put up for auction, and after the deal, the price drops further until the collateral is bought out in full.

Fig. 7.11 shows an auction with a linear function of price decrease (calc) for the collateral in the ETH-A vault: OSM (Oracle Security Module) 200 DAI, buf 20 %, and tail (tau) 21600 seconds. As a result of the auction, it is necessary to sell a pledge (lot) of 347.32 ETH to repay 60,000 DAI. There are two bidders: Alice and Bob. Alice is the first to take 50,000 DAI in exchange for a pledge of 256.41 ETH at a price of 195 DAI per ETH. The price of ETH declines more over time, and Bob takes the remaining 90.91 ETH in exchange for 10,000 DAI at a price of 110 DAI per ETH. If more than 21600 seconds have passed since the start of the auction or if the price has fallen below the set minimum, then the auction will have to be reset and start over [154].

![Figure 7.11 (1) – Auction with a linear function of price decrease](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.11-1-auction-with-linear-function-of-price-decrease.png "Figure 7.11 (1) – Auction with a linear function of price decrease")

![Figure 7.11 (2) – Auction with a linear function of price decrease](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.11-2.png "Figure 7.11 (2) – Auction with a linear function of price decrease")

![Figure 7.11 (3) – Auction with a linear function of price decrease](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.11-3.png "Figure 7.11 (3) – Auction with a linear function of price decrease")

![Figure 7.11 (4) – Auction with a linear function of price decrease](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.11-4.png "Figure 7.11 (4) – Auction with a linear function of price decrease")

![Figure 7.11 (5) – Auction with a linear function of price decrease](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.11-5.png "Figure 7.11 (5) – Auction with a linear function of price decrease")

The Maker DAO protocol implies the so-called emergency shutdown of the system, which provides two main goals. First, it is used during emergency situations as a last resort to protect the system from attacks on its infrastructure and attacks on the target price of DAI. Emergencies can include system control mechanism attacks, code breaches, security breaches, and long-term market irrationality. Second, a system crash can be used to update the Maker DAO protocol. The shutdown process can only be controlled by the Maker Governance system.

MKR token holders can also instantly trigger a system shutdown by depositing 50,000 MKR tokens into the ESM. As soon as users deposit MKR into the failover module, the tokens are immediately burned [155]. This prevents the Governance Security Module from delaying the emergency shutdown. As soon as the required MKR token voting quorum for shutdown is reached, the system shuts down without delay.

Emergency shutdown consists of three phases [132]:

1. The Maker DAO system shuts down, the owners of vaults withdraw their money. After the emergency shutdown of the system is started, further creation of new vaults and operations with existing vaults are prevented, price oracles are frozen. Frozen asset prices ensure that all users can withdraw the net value of their assets to which they are entitled. In effect, this measure allows vault owners to immediately recall excess collateral in vaults that are not being used to secure loans in issued DAI.
2. Auctions for the sale of collateral after a system shutdown are processed. Once the system is launched, all collateral auctions must be completed within a specified amount of time. This time period is set by the management system as slightly longer than the duration of the longest mortgage auction. This ensures that there are no pending auctions at the end of the auction processing period.
3. DAI holders pick up the rest of the pledges. At the end of the auction processing period, DAI holders use their DAI to redeem collateral at a fixed rate that corresponds to the estimated asset value based on the DAI target price. For example, if the ETH/USD price ratio is 200 and the user has 1000 DAI at a target price of 1 USD, the user will be able to claim 5 ETH from Maker DAO after the auction processing period if the system crash is activated. The period of time when DAI can be exchanged for collateral assets is not limited. DAI holders receive a proportional share of each collateral that is secured by the portfolio. DAI holders may risk losing a portion of their collateral, resulting in them not receiving the full value of their DAI at the target price of $1 per DAI. This may happen due to the fact that the owners of the vault deposit boxes, who have the right to collect the excess collateral first, have taken it and the remaining part of the collateral is not enough to fully cover the claims of the DAI holders.

Next, let’s consider the FRAX protocol. This protocol belongs to the category of so-called algorithmic stablecoins. The main task that the FRAX protocol solves is to ensure the stability of the value binding mechanism in case of incomplete collateralization. As of July 4, 2022, stablecoin FRAX is collateralized with USDC at 90 % (see Fig. 7.12) [156].

![Figure 7.12 – Graph of FRAX collateralization with USDC](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.12-FRAX-collateralization-with-USDC.png "Figure 7.12 – Graph of FRAX collateralization with USDC")

The minimum collateralization ratio was 82 % in October 2021. Another stablecoin – USDC – is the collateral for the issuance of FRAX. Thus, in current conditions, 1 FRAX accounts for 0.9 USDC and 0.1 Frax Shares (FXS). FXS is a volatile token that is the driving force behind the FRAX ecosystem.

Simply put, FRAX allows 900 USDC to issue 1000 FRAX, thus earning a bonus of 100 FRAX. Obviously, if FRAX reaches 100 % collateralization, it will not make economic sense to convert, for example, 1000 USDC to 1000 FRAX and pay a conversion fee.

Simplified, the FRAX protocol works as follows (see Fig. 7.13) [157]. When the price of 1 FRAX exceeds 1 USD (e. g., 1.01), anyone can put into the system collateral in the form of USDC according to the current collateralization ratio of 0.9 USDC and FXS token worth 0.1 USD and receive 1 FRAX in return. That is, an arbitrage is possible when an asset in the amount of $1 is given and an asset in the amount of $1.01 is received in return. At the same time, the FXS received by the system is burned, and 1 FRAX is additionally released into circulation. In addition, every hour the collateralization ratio decreases by 0.25 % [158]. When the price of 1 FRAX is less than 1 USD (e. g., 0.99), anyone can exchange 1 FRAX in the system for assets with a total value of 1 USD in the form of USDC collateral and the FXS token that is issued at the time of system exchange. So once again an arbitrage is possible where an asset worth $0.99 is traded for an asset worth $1. At the same time, 1 FRAX is withdrawn from circulation and burned. In addition, every hour the collateralization ratio increases by 0.25 % [158].

![Figure 7.13 – Model of FRAX minting and redeeming](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.13-model-of-FRAX-minting-and-redeeming.png "Figure 7.13 – Model of FRAX minting and redeeming")

There are 4 components involved in the stablecoin minting and redeeming: FRAX itself, collateral in the form of USDC, volatile token FXS, and a collateralization ratio that determines the required amounts of USDC and FXS [157].

The general issuance formula is as follows: $F=YP_{Y}+ZP_{Z}$, where _F_ is an amount of newly issued FRAX tokens, _Y_ is an amount of collateral tokens transferred to the system, _P<sub>Y</sub>_ is a dollar price of one collateral token, _Z_ is an amount of FXS tokens being burned, and _P<sub>Z</sub>_ is a dollar price of one FXS token. The value _Z_ can be calculated from the following equation: $(1-R_{Y})YP_{Y}=R_{Y}ZP_{Z}$, where _R<sub>Y</sub>_ is a collateralization ratio.

Let us have a collateralization ratio of 50 %, 220 USDC ($0.9995/USDC) and FXS tokens priced at $3.50/FXS. Let’s determine the number of FRAX that we can release into circulation. First, let’s calculate the number of FXS that must be burned to issue FRAX:  
$(1-0.5) \cdot 220 \cdot 0.9995=0.5 \cdot Z \cdot 3.5$;  
$Z=62.54$.  
Now let’s calculate the amount of FRAX issuances:  
$F=220 \cdot 0.9995+62.54 \cdot 3.5$;  
$F=437.78$.

In the case of FRAX redemption (burning), the following formula applies [157]: $Y= \frac{FR_{Y}}{P_{Y}}$, where _Y_ is an amount of collateral tokens received for FRAX redemption, _F_ is an amount of FRAX tokens redeemed, _R<sub>Y</sub>_ is a collateralization ratio, and _P<sub>Y</sub>_ is a dollar price of one collateral token. Along with the return of collateral tokens, the system mints FXS tokens according to the following formula: $Z= \frac{F(1-R_{Y})}{P_{Z}}$, where _Z_ is an amount of FXS tokens issued and _P<sub>Z</sub>_ is a dollar price of one FXS token.

For example, when redeeming 170 FRAX with a collateralization ratio of 65 %, prices $1.00/USDC and $3.75/FXS, the user will receive the following number of tokens:  
$Y= \frac{170 \cdot 0.65}{1} =110.5$;  
$Z= \frac{170 \cdot (1-0.65)}{3.75} =15.867$.

veFXS is a system for the time-locking of FXS tokens based on the veCRV Curve engine. Users can lock up their FXS for up to 4 years and get the amount of veFXS (e. g. lock 100 FXS for 4 years and get 400 veFXS). veFXS is a non-transferable token that does not circulate on liquid markets. It’s more like an account-based points system that captures how long a certain amount of FXS tokens are locked within the protocol.

The veFXS balance decreases linearly as the token lock expires, reaching 1 veFXS per 1 FXS with zero lock time remaining. This promotes the long-term interests of community members.

Each veFXS has 1 vote in system management proposals. Placing 1 FXS for a maximum time of 4 years leads to 4 veFXS and therefore increases the number of votes by 4 times. A user can also increase their veFXS balance by locking FXS, extending the lock period, or both. Note that veFXS is non-transferable and that each account can only have one lockout period. This means that one account cannot lock one part of FXS tokens for 2 years and another part for 3 years, etc.

The main recipients of income from the work of Frax are holders of veFXS. The profit received from the so-called AMO controllers (Algorithmic Market Operations controllers), distributed among veFXS owners as income [159].

So-called algorithmic market operations controllers (AMOs) appeared in version 2 of the Frax​​ protocol. Each AMO module is an autonomous smart contract(s) that conducts its own market operations and does not affect the FRAX peg to the US dollar. This means that AMO controllers can perform operations on the open market algorithmically, but they cannot arbitrarily emit uncollateralized FRAX and thus “break” the peg to the dollar [160].

Each AMO controller must meet four main criteria:

* Decollateralization. The part of a strategy that lowers the collateralization ratio.
* Market operations. The part of a strategy that works in equilibrium and changes the collateralization ratio.
* Recollateralization. The part of a strategy that increases the collateralization ratio.
* FXS1559. The AMO balance accounting that determines exactly how much FXS can be burned at a profit above the target collateralization ratio.

If the FRAX price is higher than the USD price, the collateralization ratio is reduced, the FRAX bid is increased as usual, and the AMO controllers continue to operate. If the collateralization ratio decreases to a value at which the peg to the dollar is lost, the AMO controllers stop all market operations, the system begins recollateralization as FRAX is bought back, and the collateralization ratio increases to restore the peg. This allows all AMO controllers to work with natural market forces just like the Frax engine version 1.

Any user can propose an AMO controller. The proposed controller will be integrated into the FRAX ecosystem if the control system approves it.

The operation of the system allows you to profit from the operation of AMO modules. Initially, it was proposed to regularly buy back and burn FXS for the amount of accumulated surpluses. In October 2021, a majority vote of the system management decided to direct all profits to the holders of veFXS tokens [161].

The greater the revenue paid for veFXS, the greater the demand for FXS and the greater the number of ecosystem participants interested in the long-term performance of the system.

The Frax system has launched its FRAX3CRV liquidity metapool on the Curve protocol [162]. This means that Frax’s own address has administrative rights to this Curve metapool, allowing the AMO Curve controller to set and collect administrative fees for the benefit of FXS holders. The AMO module can move free USDC collateral or newly issued FRAX tokens into its own Curve pool to create even more liquidity and generate additional revenue by improving the peg to the dollar.

The general strategy of the AMO controller is to optimize the minimum offer of FRAX Y in such a way that selling all of _Y_ at once to the Curve pool with _Z_ TVL and gain _A_ would affect the price of FRAX by less than _X_ % (_X_ denotes the sensitivity of the collateralization ratio range). That is, the AMO Curve module can put additional FRAX+USDC liquidity into Curve’s own metapool. Since the system triggers recollateralization when the price of FRAX becomes less than $0.99, it is possible to sell some amount of FRAX directly into the Curve pool before the price of FRAX decreases by 1 %. The Frax system can have at least this much algorithmic FRAX in circulation on the open market, as selling this amount at once in the TVL of the Curve metapool will not affect the price enough to cause a change in the collateralization ratio. These amounts are quite large, if you take into account the potential for stablecoins in the Curve metapool to deviate. For example, a 330 million FRAX3Pool (provided the underlying 3Pool is balanced) can support at least 39.2 million FRAX sell orders without a price change of more than $0.01. If the collateralization ratio change threshold is 1 %, the system can have at least 39.2 million free algorithmic FRAX on the open market without losing its peg to the US dollar [163].

In addition, Curve distributes its own CRV tokens as a reward to liquidity providers in metapools. Since Frax is the largest liquidity provider in the FRAX3CRV pool, it can direct all of its FRAX3CRV LP tokens to generate additional revenue in the form of CRV tokens, which in turn can be used to influence decisions within the Curve protocol.

To gain additional income, Curve liquidity tokens are allocated in the Convex system as of July 2022 (Fig. 7.14) [164].

![Figure 7.14 – Assets allocation in Curve AMO and in Convex AMO](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.14-assets-allocation-in-Curve-AMO-and-in-Convex-AMO.png "Figure 7.14 – Assets allocation in Curve AMO and in Convex AMO")

Liquidity AMO allocates FRAX and/or USDC on other decentralized exchanges other than Curve. The work of this AMO controller can be described in 4 points:

* Decollateralization: depositing free collateral and newly issued FRAX in the liquidity pairs of decentralized exchanges (for example, Uniswap version 3)
* Market operations: collecting fees for token exchange transactions on decentralized exchanges
* Recollateralization: the withdrawal of funds from the liquidity pools of decentralized exchanges with the subsequent burning of FRAX and/or the return of USDC to increase the collateralization ratio
* FXS1559: the daily collection of transaction fees and their use in accordance with FXS1559

Liquidity AMOs like Uniswap v3 can issue and allocate FRAX in liquidity pools with other stablecoins. This provides liquidity for other stablecoins that can be exchanged for FRAX. In addition, Liquidity AMO may periodically invoke a fee collection function to profit from exchange transactions in the liquidity pools. The diagram of the distribution of relevant assets as of July 2022 is shown in Fig. 7.15 [164].

![Figure 7.15 – Assets distribution in Liquidity AMO](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.15-assets-distribution-in-Liquidity-AMO.png "Figure 7.15 – Assets distribution in Liquidity AMO")

Investor AMO allows gaining income thanks to the reinvestment of free digital assets that the Frax system has. Initially, with the help of this controller, USDC was invested in the Yearn system; later it also became possible to invest in AAVE, Compound and other systems.

The main condition of this AMO controller’s work is the ability to withdraw invested assets at any moment and, if necessary, direct them to recollateralization. For this reason, funds allocated via Investor AMO are not taken into account when determining the collateralization ratio.

All funds earned through the controller are spent in accordance with FXS1559. The distribution of funds as of July 2022 is shown in Fig. 7.16 [164]. As can be seen in Fig. 7.16, the largest profit is gained in the form of vlCVX tokens, which the system receives from transactions in Curve and uses to generate revenue in Convex, as well as to vote on the redistribution of revenue between Curve pools.

![Figure 7.16 – Assets distribution in Investor AMO](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.16-assets-distribution-in-Investor-AMO.png "Figure 7.16 – Assets distribution in Investor AMO")

Lending AMO releases FRAX in lending systems like Compound, AAVE or CREAM. The mechanism of FRAX issuance is similar to the mechanism of operation of the D3M module in the Maker DAO protocol: it is an additional way of issuing FRAX tokens against the collateral of other digital assets available in the lending systems.

Such an additional issuance mechanism is permissible, since on lending platforms FRAX is always issued with excess collateral, and this does not reduce the collateral factor established by the protocol.

For assets issued on credit, lending platforms utilize a dynamic interest rate that depends on the share of the use of the liquidity pool. Thus, the higher the percentage of the liquidity pool of this or that asset issued on credit, the higher the price paid by the borrowers for this asset. Given the target rate set in the controller, the Frax protocol can emit additional FRAX to the liquidity pool or withdraw the provided liquidity to prevent sharp jumps in the FRAX borrowing price and provide a cheaper borrowing cost than the borrowing cost of competing stablecoins.

All received interest income is spent by the system in accordance with FXS1559. The corresponding distribution of assets as of July 2022 is shown in Fig. 7.17 [164].

![Figure 7.17 – Assets distribution in Lending AMO](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.17-assets-distribution-in-Lending-AMO.png "Figure 7.17 – Assets distribution in Lending AMO")

Let’s consider the risks of the Frax system. FXS is designed as an additional token that holds the value of the system and the collateral of stablecoins. Frax directs seigniorage income (income that the issuer receives from the issue of funds and appropriates as a right of ownership) directly into the FXS token.

The main risk of this model is a death spiral. The essence of this phenomenon is well illustrated by the example of another stablecoin UST (Terra protocol). In the case of mass selling caused by an attack or a general drop in the asset in the market, the stablecoin is depegged from the US dollar. This makes it profitable to burn FRAX at a price of, say, $0.98 and get USDC and FXS for a combined cost of $1 (i. e. making an actual $0.02 profit). The escalation of the death spiral is that the price of FXS declines so rapidly that it becomes impossible to lock in a profit from burning FRAX. Let at time _X_ exchange FRAX for USDC + FXS make sense and make a profit of $0.02. However, at a later point in time _X'_, when you need to lock in a profit, selling FXS may result in a loss of $0.01. In this situation, FRAX is not burned and not withdrawn from circulation, the depeg increases, the token price of FXS decreases rapidly and the value of both assets approaches zero. This is exactly what happened with UST and LUNA tokens in May 2022.

Fortunately, in a Frax system with 90 % collateral, it is unlikely that the price of FRAX will fall further than 0.9 USD, because 1 FRAX will always yield 0.9 USDC, even if the price of FXS drops to zero. However, according to certain calculations, the actual collateralization of FRAX is not 90 %, but 80 % due to funds of the system itself, invested in liquidity pools [165]. In addition, interest in the project is mostly maintained thanks to FXS farming programs, which are distributed as an additional reward in various liquidity pools [166]. Thus, the long-term success of the Frax system depends on its ability to gain critical mass and find real use by the time the sources of artificial incentivization of participants through FXS handouts are exhausted.

Another significant risk is the complete dependence of the system on the USDC. As you know, USDC is a centralized stablecoin, so any address can be blocked at any moment at the discretion of the owner of this system. Therefore, blocking Frax system addresses that hold USDC collateral would cause the system to crash immediately. The same would happen if the USDC itself were to be depegged from the US dollar.

Next, let’s consider the Synthetix synthetic asset protocol. The main asset of the system launched on this protocol is the stablecoin sUSD. The next largest assets are sETH and sBTC. An asset is considered synthetic if it is not collateralized with a real world asset, but only with the SNX token of the system. The Synthetix protocol is similar to the Maker DAO in the sense that in both protocols stablecoins are issued against the collateral of a digital asset, but in the Synthetix protocol only its own SNX token can be used as collateral.

Note that this system works as a full-fledged decentralized organization (DAO) and all decisions are made by voting of SNX token holders.

As of July 2022, a total of more than 100 million sUSD is in circulation [167]. The main mechanism for issuing new sUSD works through the so-called Synthetix staking portal [168]. To issue sUSD, you need to have SNX tokens in one of two accounting systems: Ethereum or Optimistic Ethereum. The option with the second-layer Optimistic Ethereum protocol appeared due to the need to lower the transaction cost of issuing, redeeming and receiving rewards in the Synthetix ecosystem.

The DAO decision sets the collateral C-Ratio. As of July 2022, if this ratio exceeds 400 % (i.e. $4 SNX is pledged for 1 issued sUSD), the user has the right to receive a reward once a week – every Wednesday at 20:00–21:00 Kyiv time. The reward is paid in the form of SNX and sUSD tokens. SNX for reward is issued by the system, and these tokens become available for exchange only 12 months after they are received. Until then rewards can only be used as collateral to issue sUSD. The second part of the weekly reward – sUSD tokens – is available immediately and is a fee that is redistributed from platforms where synthetic assets are traded: “Kwenta Futures”, “Lyra Options”, “Kwenta Spot”, “Curve Cross-Asset Swaps”, etc.

If the value of C-Ratio becomes lower than 400 %, it is impossible to withdraw the already accrued reward and the new reward is not paid. If the accrued reward is not collected within a week after its accrual, then after the next payment period the previous reward is lost. Thus, you should carry out a claim transaction of accrued rewards every week. This feature partially led to the transition of the protocol to staking and issuing sUSD in the Optimistic Ethereum accounting system, where transaction fees are lower.

There are two ways to increase C-Ratio: buying additional SNX tokens and burning sUSD tokens. The position is forcibly liquidated 12 hours after the C-Ratio drops to less than 150 % [169]. At the same time, a penalty of 30 % is charged, which is redistributed as a separate type of reward among all SNX stakers.

Note that, unlike many other protocols, it is not necessary to send SNX tokens to a separate contract in order to contribute them to the stack. The fact that the tokens are at your address means that they will be added to the stake as soon as you have emitted this or that amount of sUSD.

In addition to sUSD, the Synthetix system allows you to buy sBTC, sETH and a number of other synthetic digital assets. The main use of synthetic digital assets is their use in short sales (short) for the sake of earning from the change in the value of a non-synthetic asset (for example, ETH).

The “KWENTA” platform is used for short selling. Let’s say Alice wants to short sell 1 sETH, which for our example is worth $1,000.

Alice deposits 3000 sUSD and borrows 1 sETH on the “KWENTA” platform. The loaned sETH is automatically sold for $1000, which is returned to Alice in her account. To return the deposit, she should buy and return 1 sETH. Let’s analyze how a change in the price of sETH will affect its profit.

After the short sale, Alice has $1,000 in her wallet and owes the system 1 sETH. If the price of sETH decreases to 900 USD, Alice will have a profit of 100 USD by redeeming 1 sETH. However, if the price of sETH increases to $1100, it will lose $100. So Alice earns from the decline in the asset.

If the price of sETH continues to rise, Alice’s position will be threatened with liquidation. When Alice opened a short position, she locked up $3,000 to sell 1 sETH. This means that her C-Ratio is 300 %. The liquidation mechanism is triggered when the C-Ratio drops below 120 %, so if the price of sETH rises to $2500, Alice’s short position will be liquidated.

In general, the functioning of short positions is currently regulated according to SIP-103 [170]. Note that opening a short position is only possible using sUSD – the asset on which the entire Synthetix ecosystem is built.

Another advantage of the Synthetix protocol is that sBTC, sETH and other synthetic assets are purchased for sUSD without slippage, which is typical for transactions on decentralized exchanges. The protocol allows for such purchases and sales based on prices that are transmitted to the system by price oracles. The diagram of this process is shown in Fig. 7.18 [171].

![Figure 7.18 (1) – Synthetix protocol operation scheme](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.18-1-Synthetix-protocol-operation.png "Figure 7.18 (1) – Synthetix protocol operation scheme")

![Figure 7.18 (2) – Synthetix protocol operation scheme](/resources/img/volume-3/7.2-Operating-details-of-stablecoins/F-7.18-2.png "Figure 7.18 (2) – Synthetix protocol operation scheme")

What was probably the most risky is the Terra project, which aimed to create an algorithmic stablecoin with no collateralization at all. Formally, the UST token was collateralized with another token – LUNA. In fact, the LUNA token was collateralized with nothing but the belief that a system crash was impossible. In May 2022, LUNA entered the so-called death spiral. The price of UST dropped from $1 to a few cents, leaving those who previously owned LUNA worth millions of dollars with a few hundred dollars.

Let’s analyze in more detail how the project got into the spiral of death. The UST collateralization mechanism is very similar to how Frax works. However, unlike Frax, where the share of algorithmic support in the form of FXS is only 10–20 %, in the Terra system the share of algorithmic support was 100 %.

According to the design of the project, if the value of UST deviates from 1 USD, arbitrage is possible by buying UST at the price of 0.99 USD and exchanging it for LUNA with a value of 1 USD (as a result, a profit of 0.01 USD is obtained). This model works until the LUNA devaluation rate exceeds the UST depegging rate. If LUNA is down to $0.98 after exchanging UST for LUNA and before the USD profit is locked in, maintain the UST rate by burning it and exchanging it for LUNA. This is the death spiral: further attempts to stabilize the rate with standard protocol methods cause the depreciation of both UST and LUNA.


### Summary about stablecoins

The case with the Terra project clearly showed that only the US Federal Reserve System can practically create (mint) assets out of thin air. All other algorithms and protocols have to assume as collateral either already printed US dollars (as in USDT, USDC) or some existing digital asset (as in Maker DAO). Note that it is unreasonable to consider the Maker DAO system overcollateralized: the more volatile the collateral asset is in relation to the US dollar, the higher the collateralization ratio. If a tokenized dollar is issued against a fiat dollar, obviously the collateral factor is 100 %; if a stablecoin is issued on the basis of a volatile digital asset, the collateralization ratio is 130 % or more, because a buffer is needed, which allows for a sharp devaluation of the collateral to liquidate the position and return the credit issued in stablecoins.

Frax can tentatively be called a dollar-based stablecoin issuing system: FRAX is 90 % collateralized with USDC. The remaining 10 % provides the system’s ability to earn and accumulate intrinsic value over time.

The Synthetix protocol can be considered an acceptable exception. It uses its own SNX token as collateral, but at the same time, firstly, it encourages keeping the level of collateral at least 400 % and, secondly, it has created an ecosystem of financial derivatives in which sUSD is the only permissible collateral. The scope of sUSD is severely limited by the system’s internal need to allocate collateral on the KWENTA platform. So, sUSD is an example of a stablecoin that is issued for the specific needs of the created ecosystem and has a limited circulation.

Experiments with stablecoins continue. Noteworthy are the announcements about the release of their own stablecoins in the AAVE and Curve systems. Given the shutdown of AAVE’s D3M Maker DAO module, launching its own stablecoin is a natural step for AAVE, as billions of USD in assets are locked up within this system. The same applies to Curve, whose business model is fundamentally built on liquidity pools to exchange one stablecoin for another.

The company “Distributed Lab” has developed its DL DeFi Core lending protocol with an integrated stablecoin. The model used in DL DeFi Core allows you to collect virtual digital assets in liquidity pools and issue stablecoins with a collateralization ratio set in relation to the total amount of collateral allocated in the system. This approach notably outperforms Maker DAO, which has long used the same model but issues DAI for each of its collateral assets separately with different collateralization ratios and position liquidation parameters. Thus, DL DeFi Core, unlike Maker DAO, allows you to improve the collateralization ratio by placing any eligible asset in the system as collateral, and avoid forced liquidation of the position.


## 7.3 Overview of DeFi protocols

There are many financial instruments in the world that have been created and improved for dozens of years: lending, derivatives, exchanges. At the same time, with the recent but rapid development of decentralized technologies, they began to gradually appear in the decentralized environment and take on a new form. For the nascent ecosystem, the collective acronym DeFi was even coined, derived from the term “decentralized finance”. DeFi covers a number of projects, technologies and financial instruments that are only available in decentralized accounting systems.

The main components of the DeFi ecosystem, which will be discussed further:

* Decentralized exchanges
* Decentralized lending and borrowing protocols
* Stablecoins
* Derivative financial instruments (derivatives)
* Yield aggregators
* DEX aggregators


### Decentralized exchange protocols

In Subsection 1.4 of this book, we have discussed decentralized exchange technologies. All of them share the same working principle: order matching is done using the order book, asset exchange is peer-to-peer, and asset prices are formed off-chain.

Next, decentralized exchanges built on the basis of an automated market maker (AMM) will be considered. AMM is a mechanism that allows for on-chain pricing. For this, a special smart contract (liquidity pool smart contract) is created for each pair of assets, which serves as a boiler account and also calculates prices using formulas that take into account supply and demand. In addition, after each exchange of one asset for another, a new price ratio is determined.

As a result, each user can interact directly with the smart contract of the liquidity pool, and not with specific orders from counterparties. Examples of such exchanges are Uniswap and Curve. Let’s consider these 2 exchanges in more detail.


### Uniswap protocol

Uniswap is a decentralized exchange protocol deployed on the Ethereum system. It was Uniswap that proved the concept of decentralized exchanges based on AMM, which was described by Vitalik Buterin [172]. The first implementation was proposed by Hayden Adams, who at the time of submitting his proposal wanted to practice development on Solidity, and eventually received a grant from the Ethereum Foundation [173].

There are currently 3 versions of the protocol. Let’s consider each of them in more detail.

Let’s start with Uniswap V1. This protocol contains the following main components [174]:

* Exchange smart contracts that implement the liquidity pool contract for the ERC20 token and ETH pair. This approach allows you to exchange any ERC20 token using ETH as an intermediate asset. The price ratio between the token and ETH is determined by the formula specified in the exchange smart contract [175].
* Factory smart contract that allows creating new exchange smart contracts. It is also a public registry of all available exchange smart contracts.

Now let’s consider what is available to users in the first version of the protocol.

First of all, how is liquidity provided? Each user can contribute their ERC20 tokens as well as their equivalent amount of ETH to the liquidity pool at the current pool rate. In exchange for invested tokens, the user receives from the system the number of LP tokens that corresponds to his investment (they are also often called liquidity provider tokens) [176].

Why is this necessary? In the simplest case, if the system wants to ensure an even distribution of rewards from exchanges among all liquidity providers, it must create a huge number of individual transactions. Obviously, the size of the rewards will not cover the fees for new transactions. Therefore, rewards are accumulated at the address of the smart contract, and then only at the request of the user, the funds are withdrawn together with the provided liquidity.

The described mechanism works on the principle of a constant increase in the exchange rate between LP tokens and the underlying asset. The exchange rate itself is calculated on the basis of all loan commissions that have entered the system. The principle of operation of the protocol can be seen in Fig. 7.19 [181].

![Figure 7.19 – Uniswap protocol operation scheme](/resources/img/volume-3/7.3-Overview-of-DeFi-protocols/F-7.19-Uniswap-protocol-operation.png "Figure 7.19 – Uniswap protocol operation scheme")

Also, any willing user can make an exchange by paying a fee. In the Uniswap V1 protocol, exchange can be of two types:

* ERC20 token for ETH
* ERC20 token to another ERC20 token

Let’s consider how the exchange price is calculated. Importantly, the price ratio is recalculated by the smart contract every time an exchange is executed. The main rule that determines the price ratio of two assets is described as follows: when there is more asset _X_ in the liquidity pool and less asset _Y_, this means that the demand for asset _Y_ becomes greater and its price relative to asset _X_ increases.

From a mathematical point of view, this mechanism is described by the following invariant: $xy=k$, where _k_ is a constant value. (In this context, an invariant is an expression characterized by invariance.) This means that regardless of changes in the quantity of each asset in the liquidity pool, the product of their quantities must be unchanged. This approach guarantees liquidity in the pool. The Uniswap pricing scheme is shown in Fig. 7.20 [181].

![Figure 7.20 – Pricing in Uniswap](/resources/img/volume-3/7.3-Overview-of-DeFi-protocols/F-7.20-pricing-in-Uniswap.png "Figure 7.20 – Pricing in Uniswap")

Consider an example of how the exchange rate will change as a result of the exchange of two assets. To do this, use the following notations:

* _x_ is the number of units of asset _X_ in the liquidity pool.
* _y_ is the number of units of asset _Y_ in the liquidity pool.
* _Δx_ is the number of units of the sold asset.
* _Δy_ is the number of units of the purchased asset.
* _x'_ is the number of units of asset _X_ in the liquidity pool after the conclusion of the agreement.
* _y'_ is the number of units of asset _Y_ in the liquidity pool after the conclusion of the agreement.

The main rule that determines the price of an asset is as follows: $xy=(x+Δx)(y-Δy)$. Then the number of tokens remaining in the liquidity pool is calculated according to the following formulas: $x'=x+Δx=x(1+α)= \frac{x}{1-β}$, $y'=y-Δy= \frac{y}{1+α} =y(1-β)$, where $α= \frac{Δx}{x}$ and $β= \frac{Δy}{y}$. At the same time, it is possible to derive formulas for the cost of exchanging a certain number of tokens _X_ for tokens _Y_: $Δx= \frac{xβ}{1-β}$, $Δy= \frac{yα}{1+α}$.

In addition, to carry out the exchange, the user is obliged to pay a fee, which will then be paid to the liquidity provider. A separate interest fee is set for each liquidity pool, which depends on the volatility of the asset. For more volatile assets, the fee is higher, and for stablecoins it is often 0.3%. The main rule for determining the fee is as follows: $0 \lt p \lt 1$, where _p_ is a fee percentage. Then obtain the following formulas: $x'=x+Δx=x(1+α)= \frac{x(1+β( \frac{1}{γ} -1))}{1-β}$, $y'=y-Δy= \frac{y}{1+αγ} =y(1-β)$, where $α= \frac{Δx}{x}$, $β= \frac{Δy}{y}$, and $γ=1-p$. Hence, we can calculate the cost of exchanging a certain number of tokens _X_ for tokens _Y_ with the fee taken into account: $Δx= \frac{xβ}{γ(1-β)}$, $Δy= \frac{yαγ}{1+αγ}$.

New exchange smart contracts are created using the Factory smart contract. Each unique ERC20 token can be bound to only one exchange smart contract [177].

You can see the entire list of available liquidity pools, as well as their parameters, using the web interface. Figure 7.21 shows the top liquidity pools in Uniswap on August 15, 2022 [178].

![Figure 7.21 – Top liquidity pools in Uniswap](/resources/img/volume-3/7.3-Overview-of-DeFi-protocols/F-7.21-top-liquidity-pools-in-Uniswap.png "Figure 7.21 – Top liquidity pools in Uniswap")

Uniswap V1 provides the following functions to support the execution of exchange operations [179]:

* createExchange – creating a new liquidity pool
* addLiquidity – providing liquidity to the pool
* removeLiquidity – removing deposited funds from the pool

For the direct exchange of assets, there are the functions named according to the following template: ‹ethToToken | tokenToEth | tokenToToken›‹Swap | Transfer›‹Input | Output›, where:

* ethToToken denotes that ETH is exchanged for ERC20.
* tokenToEth denotes that ERC20 is being exchanged for ETH.
* tokenToToken denotes that an ERC20 to ERC20 exchange is taking place.
* Swap denotes that purchased tokens will be sent to the same address from which they were sold.
* Transfer denotes that purchased tokens will be sent to another address specified by the user in the input parameters of the called smart contract function.
* Input denotes that the user sets exactly the number of tokens to be sold, while the number of tokens to be bought may vary depending on the exchange rate.
* Output  denotes that the user specifies exactly the number of tokens to be bought, while the number of tokens to be sold may vary depending on the exchange rate.

As an example, let’s consider the function ethToTokenTransferInput. Upon calling this function, the user must determine the following items:

* the exact amount of ETH for sale
* the minimum amount of ERC20
* the address of the recipient of the purchased ERC20 tokens

After this transaction has been made, the outcomes are as follows:

* The sold ETH will be credited to the liquidity pool address.
* The amount of ERC20 tokens equivalent to the specified amount of ETH will be credited to the recipient’s address. If the amount of ERC20 is less than the minimum amount, the transaction will not be made.

In addition, there are functions that can return price ratios between different pairs of assets. They are as follows [180]:

* getEthToTokenInputPrice
* getEthToTokenOutputPrice
* getTokenToEthInputPrice
* getTokenToEthOutputPrice

Next, let’s consider the features of the Uniswap V2 protocol. While version 1 was aimed at proof-of-concept creation, the goal of version 2 is to further develop the idea as well as to enhance the convenience and the security of the protocol. Version 2 introduced the following updates [182]:

* Ability to create liquidity pools for pairs of ERC20 tokens, as well as perform direct (without an intermediate asset) exchanges
* Decentralized price oracles (instead of Uniswap V1’s centralized oracles)

The next version of the protocol – Uniswap V3 – implied the solution of the following issue: in the previous protocol versions, when exchanging stablecoins, the price calculation mechanism works in such a way that 1 USDT may be purchased for 1.05 USDC [183]. Due to this, a too big and unprofitable difference comes up for big amounts, since both stablecoins pegged to the same benchmark asset should ideally be exchanged in 1 to 1 ratio. This happens due to the inefficiency of the constant-product invariant for exchanging stablecoins pegged to the same asset. Therefore, the defining innovation of the Uniswap V3 protocol is the mechanism of concentrated liquidity, which is likely to have solved the above-described issue [184].

What makes the mechanism work? Every user, when providing liquidity, can determine the range in which his liquidity can be used. The protocol, in turn, motivates users to concentrate all the liquidity which they provide in a particular price range depending on the liquidity pool (for example, from $0.99 to $1.01 for the USDT–USDC pool). As a result, a bigger reward is received by those users in whose liquidity usage range most exchanges have been made. Since the exchange of stablecoins satisfies both sides of the exchange (those who want to buy USDT and those who want to buy USDC), the exchange rate between the two stablecoins remains close to ​​1 to 1 ratio.

The obvious advantage for users who perform exchanges is the greater amount of liquidity in a particular price range.


### Curve protocol

Curve is a decentralized exchange protocol [185], the principle of which is very close to Uniswap. Its main feature is the focus on the stablecoins market.

For the same goal for which Uniswap has created the concentrated liquidity mechanism, the Curve team has developed its own mechanism for exchanging stablecoins – StableSwap [186].

To understand how this mechanism works, first consider the constant-sum (constant-price) invariant: $x+y=k$. With such an invariant, stablecoins are exchanged at the 1-to-1 rate, since the total amount of two assets before the exchange is equal to the amount after the exchange: $x+y=(x+Δx)+(y-Δy)$. It also follows that the amounts of the purchased and the sold asset are always equal: $Δx=-Δy$.

Although this method seems good for working with stablecoins, it still entails the following disadvantages:

* A situation may arise when there are no coins left in the liquidity pool for exchange. In this case, the system can suffer huge losses.
* The number of stablecoins supported by the platform may be drastically decreased due to “unstability” or unpopularity of particular stablecoins.

As a result, the Curve team decided to combine the constant-sum invariant and the Uniswap invariant (see Fig. 7.22) in order to obtain a solution with which the depeg of stablecoins is unlikely, the exchange price is acceptable, and the system always has a certain amount of liquidity.

![Figure 7.22 – Comparison of invariants](/resources/img/volume-3/7.3-Overview-of-DeFi-protocols/F-7.22-comparison-of-invariants.png "Figure 7.22 – Comparison of invariants")

Let’s consider how the merging of invariants is carried out mathematically. The invariants generally look like this: constant sum $\displaystyle\sum_{i}{x_{i}} =D$, constant product $\displaystyle\prod_{i}{x_{i}} =( \frac{D}{n} )^{n}$, where _D_ is the total number of coins at the stage when they had equal prices and _n_ is the number of coins supported by the pool.

To join two graphs, the following formula is applied: $χD^{n-1} \displaystyle\sum_{i}{x_{i}} + \displaystyle\prod_{i}{x_{i}} = χD^{n} + ( \frac{D}{n} )^{n}$, where _χ_ is a dynamic coefficient indicating direction to which the invariant tends. If $χ \to 0$, Stableswap turns to the constant-product invariant; if $χ \to \infin$, then to a constant-sum one. This coefficient is defined as follows: $χ= \frac{A \displaystyle\prod_{i}{x_{i}}}{( \frac{D}{n} )^{n}}$, where _A_ is an amplification coefficient for the constant-sum component of the merged invariant.

If we plug the expression for _χ_ to the invariant formula, we obtain the following: $An^{n} \displaystyle\sum_{i}{x_{i}} +D=ADn^{n}+ \frac{D^{n+1}}{n^{n} \displaystyle\prod_{i}{x_{i}}}$. Further, the system is only to maintain this equation.

Examine the Curve interface (see Fig. 7.23) as well as the parameters that will be useful in the context of providing liquidity to the liquidity pool.

![Figure 7.23 – The Curve interface](/resources/img/volume-3/7.3-Overview-of-DeFi-protocols/F-7.23-Curve-interface.png "Figure 7.23 – The Curve interface")

In addition to the previously discussed parameters, the following ones are required to select a liquidity pool:

* base vAPY is the annual percentage of the reward, calculated based on the current daily interest and the volume.
* rewards tAPR is the annual reward in system tokens, calculated based on the current daily interest.


### Borrowing and lending protocols

Before understanding how decentralized lending systems work, let’s consider how lending and borrowing work in real life. Take a bank as an example: some of the bank’s customers open a deposit and later can receive a certain interest, some other customers take out loans and later pay interest. Hence, the bank at any moment has the funds to give loans, whereas the margin between the interest paid by borrowers and the interest paid to depositors remains with the bank as profit. The scheme of bank lending is shown in Figure 7.24.

![Figure 7.24 – Scheme of lending in centralized financial institutions](/resources/img/volume-3/7.3-Overview-of-DeFi-protocols/F-7.24-lending-in-centralized-financial-institutions.jpg "Figure 7.24 – Scheme of lending in centralized financial institutions")

This approach has an obvious drawback: the bank cannot give loans to just anyone, since otherwise it risks never receiving the depositors’ money. Therefore, there are two options for the bank: to give loans against a collateral of something valuable or to give loans based on the borrower’s credit history.

Consider a specific example using collateral. Alice wants to enter university, and she needs to pay $10,000 for the first semester of study. The problem is that Alice does not have that much money. She decides that the best option is to take a loan from a bank and repay the debt some time after her graduation.

The bank, in turn, cannot trust Alice, since her credit history is empty, and the bank is not convinced by her intention to become, say, the greatest dentist in the world. There is another option: Alice can pledge something valuable to strengthen her obligation to pay off the debt afterward.

Alice has recently inherited her grandmother’s necklace, which has the estimated value of 100,000 dollars. Just selling it is unacceptable for her.

Further, there are four possible options:

* Alice repays the debt and collects her pledge.
* Alice does not repay the debt. This case is favorable for the bank, because it will have the right to keep the pledge, which is worth much more than the debt amount.
* Alice can try to trick the bank by pledging a fake necklace. However, the bank does not trust Alice and can verify whether the piece of jewelry is authentic. Moreover, the bank can publicly claim that Alice is inclined to cheat.
* Alice leaves the pledge, but in some time the price of the necklace drops to $5,000. For her, not repaying the loan is more profitable, and for the bank, it is better to get rid of the pledge before it completely devalues.

Now let’s consider how the decentralized lending protocols have been developed. First of all, the p2p borrowing/lending approach was tested. An example of such a protocol is ETHLend (currently the project is stopped) [187].

ETHLend is a decentralized p2p lending application (DApp) running on the Ethereum system. This protocol allowed the borrower and the lender to determine important details of the loan without the need for an intermediary. This approach was assumed to eliminate the situation where only a bank gives credits and accepts deposits as well as to create competition between lenders, leading to the most competitive interest rate.

In 2018, ETHLend was renamed to Aave, which means “ghost” in Finnish. The team that created the protocol explains that this name symbolizes an attempt to create a transparent and open decentralized lending system.


### AAVE protocol

AAVE is a decentralized lending protocol [188] that allows its users to borrow funds and receive interest for lending funds.

A significant difference of the AAVE architecture from ETHLend is that a liquidity pool is created to ensure the system’s operation with each ERC20 token. DEXs experienced a similar stage of development: when order books disappeared, what appeared were liquidity pools, to where users could add assets supported by the system and from where users could take credits. Thus, there is no need to match demand and supply. The advantage of this solution is that in terms of security, the solution remains reliable. It does not require trust, because the underlying smart contracts are publicly available for audit.

Let’s examine the flow of providing liquidity to the pool. Imagine such a situation: Alice has recently purchased 1 ETH, and she decides to make a deposit for which she can receive interest. She becomes a lender in the borrowing/lending protocol by depositing the desired amount of the asset to the liquidity pool (Fig. 7.25). As a result, Alice receives the correspoding _interest bearing tokens_ (in our case, ibETH). When Alice wants to withdraw her 1 ETH along with the accumulated interest, she can simply exchange the interest bearing tokens back.

![Figure 7.25 – AAVE lending scheme](/resources/img/volume-3/7.3-Overview-of-DeFi-protocols/F-7.25-AAVE-lending.png "Figure 7.25 – AAVE lending scheme")

At this time, another protocol user borrows Alice’s funds. Further, when the borrower repays his debt, he will also pay the interest, which will be credited to Alice (Fig. 7.26).

![Figure 7.26 – Loan repayment scheme](/resources/img/volume-3/7.3-Overview-of-DeFi-protocols/F-7.26-loan-repayment.png "Figure 7.26 – Loan repayment scheme")

Noteworthy, the reward is accumulated due to frequent fluctuations of the exchange rate of the original asset to the corresponding interest bearing ones. The exchange rate changes due to the fact that when returning funds, borrowers also pay interest [189].

Consider an example of how it works. A new ETH liquidity pool is created. Alice and Eve deposit 100 ETH each to it, and they receive 100 ibETH each in return. The initial price ratio is 1:1, because the pool has no profit yet: the amount of ibETH issued is equal to the amount of ETH.

Let Bob need a loan of 100 ETH and let the annual interest rate defined in the smart contract of the protocol be 5%. So, Bob borrows 100 ETH, and he is going to repay the entire debt as well as to pay the interest in a year. That is, he will have to give 105 ETH. Let the initial ETH amount in the pool be 200 coins. In a year the ETH amount will become 205 coins. This means that the exchange rate will be as follows: 1 ibETH for 1.025 ETH.

Imagine that a year has passed and Alice now wants to withdraw her tokens. If she exchanges her ibETH back, she will receive ~102.5 ETH, i. e., her profit will be 2.5 ETH.

The advantage of using this method is obvious: transaction costs of regular interest payments are reduced, since the payment can be made only when the interest recipient requests it.

Another important point is how the interest rate is set. In the previous simplified example, we knew in advance that the borrower would have to pay 5%. In fact, the interest rate (_R_(_t<sub>i</sub>_)) at every time moment is dynamic (see Fig. 7.27). It is calculated according to the following formula: 

$$
\begin{cases}
R(t_{i})=α+U_{i}β & \text{ if } U_{i} \leq U' \cr \\
R(t_{i})=α+U'β+γ(U_{i}-U') & \text{ if } U_{i} \gt U'
\end{cases} $$ 

where _α_ is the basic percentage for one block, _β_ is the percentage which ensures the growth of the exchange rate, _U<sub>i</sub>_ is the value of the demand and supply ratio in the pool for a time point _t<sub>i</sub>_, _U'_ is the optimum value of the demand and supply ratio (it is required to control the availability of funds in the pool), and _γ_ is the “jump” multiplier which determines the graph slope change when $U_{i}=U'$.

The parameters _α_, _β_, _γ_, and _U'_ can be initially set by the protocol’s creator and afterward modified by specially assigned system administrators or by governance mechanisms. The parameter _U_, in turn, is calculated according to the following formula: $U= \frac{A_{\text{bor}}}{A_{\text{av}}+A_{\text{bor}}}$, where _A<sub>bor</sub>_ is the total amount of funds borrowed from the liquidity pool and _A<sub>av</sub>_ is the leftover of funds in the liquidity pool.

Fig. 7.27 shows an example with the following ready-set parameters: $α=2.5$, $β=0.05$, $γ=3$, and $U'=70$.

![Figure 7.27 – Graph of the interest rate calculation function](/resources/img/volume-3/7.3-Overview-of-DeFi-protocols/F-7.27-interest-rate-calculation-function.png "Figure 7.27 – Graph of the interest rate calculation function")

Further, using the interest rates calculated for every specific time point, you can calculate the number of coins that the borrower must ultimately return. In order to effectively accumulate block interest rates, it is very important to create a mechanism that normalizes debts, i. e., indicates them as for the time point when the liquidity pool was created (_t<sub>0</sub>_). Otherwise, you will have to store every block interest rate change as well as the time of every new borrowing.

Let’s define the formula for a normalized debt: $N= \frac{A_{\text{dep}}}{R(t)}$, where _N_ is the normalized debt, _A<sub>dep</sub>_ is a deposit amount, and _R_(_t_) is the cumulative interest rate. Now, define the concept of a cumulative interest rate: $R(t)= \displaystyle\prod_{i=0}^{n}{R(t_{i})}$, where _n_ is the number of blocks (of specific time points). It considers all changes in the block coefficient and allows calculating the final coefficient, which summarizes the interest for the entire borrowing period. In order to calculate the final amount to be returned, the following formula is applied: $A_{\text{ret}}=N(1+R(t))$, where _A<sub>ret</sub>_ is the return amount.

Let’s consider the flow of loan taking. The main principle is that a loan is taken against another asset as a collateral (assets that may be used as collaterals are determined by the protocol’s creators or by the governance module). This allows protecting the platform from situations in which a user may not repay the debt. However, there is another issue – collateral depreciation. If the collateral deposited by a user begins to depreciate sharply and the platform does not manage to liquidate it, the platform will stay in loss. Therefore, to track the price of a user’s collateral, the system utilizes price oracles.

There is a mechanism that helps to prevent collateral deprecation: the user cannot borrow an amount greater than the equivalent collateral amount multiplied by some security factor $0 \lt f_{\text{sec}} \lt 1$, which determines the size of the “cushion”. This factor can be set by the system’s creator, and afterward it can be changed by administrators or by the governance module. The security factor reflects the volatility of a specific asset. Therefore, every asset is assumed to have its own value of this factor.

If the cost of the collateral decreases and stops covering the loan, liquidation comes into play. Liquidation is the process of selling the borrower’s collateral when the collateral cost tends to no longer cover the debt cost. In addition to the above-described security factor, most often the liquidation threshold values ​​are set for every asset. This threshold determines when liquidation begins.

Most often, such liquidation is beneficial for both the platform and the liquidator (the user who repays the loan and takes the collateral), since the platform can sell the user’s collateral with a discount.

Imagine the following example: Alice has left a collateral of 0.033 ETH equivalent to $100 and has borrowed 70 DAI equivalent to $70 (the security factor equals 0.7). However, the ETH liquidation threshold equals 0.8. This means that if the price of 0.033 ETH decreases to $80, any user will be able to initiate the liquidation and redeem the collateral with a discount. Let the discount percentage be 6.25%. Then 0.033 ETH will be sold at the price of $75, which implies the $5 discount for the liquidator and the $5 profit for the platform.


### Compound protocol

Compound is a decentralized application in the Ethereum accounting system [190]. Its main purpose is the organization of decentralized lending and borrowing.

The operating principle of Compound is the same as that of AAVE, but there are several differences as follows:

* AAVE offers flash loans to users. They allow borrowing funds without collateral, but the borrowed funds must be returned to the system in the same block in which the loan has been taken. Despite this great advantage, this functionality often becomes vulnerable in similar protocols. A recent attack on this mechanism happened on April 17, 2022; 182 million dollars were stolen [191].
* Compound and AAVE offer different collateral ratios for different assets, whereas the protocols’ APYs are often close in value.
* Unlike Compound, which supports only Ethereum, AAVE also works in the following accounting systems: Polygon, Avalanche, Fantom, Harmony, and Optimism.
* As of August 15, 2022, AAVE’s TVL is $12.5 billion (including all networks), while Compound’s one is $3.03 billion.

Ultimately, both protocols offer their users a well-developed decentralized borrowing functionality.


### Yearn protocol

For users who understand how DeFi works, there are many opportunities to earn via the right investment of funds – via either providing liquidity to the DEX protocol or borrowing one asset in order to provide liquidity to another liquidity pool with a higher interest rate. With the emergence of more and more new DeFi protocols, it becomes even more difficult to track all profitable paths manually. As a result, there is a need for a solution that will automate this process.

In February 2020, Andre Cronje tried to implement this idea in the form of the project iearn.finance [192]. Initially, income optimization was implemented only within the protocols such as Fulcrum, Aave, Compound, and dYdX, but later the number of supported DeFi protocols and assets increased and the project was renamed to Yearn.

How does Yearn work? For each asset, its respective yVault (Yearn vault) is created. A yVault is a smart contract that maintains a pool of tokens as well as manages tokens according to the strategies implemented in the smart contract [193].

Yearn vaults have the following properties:

* The income will accumulate in the token which is deposited to a yVault. This allows having compound interest in a yVault.
* yVault can have many strategies active at the same time. If necessary, yVault may change the funds distribution in its strategies.
* Unlike in many other income aggregators, no deposit/withdrawal fees are charged.

Hence, any user can simply deposit his funds in some yVault and later withdraw them along with the accumulated income (Fig. 7.28).

![Figure 7.28 – Yearn operation scheme](/resources/img/volume-3/7.3-Overview-of-DeFi-protocols/F-7.28-Yearn-operation.jpg "Figure 7.28 – Yearn operation scheme")

There is a respective yVault for every asset. Every yVault can maintain its own range of strategies [194]. Generally, the strategies can be divided into the following categories:

* Stable yVaults are ones in which stablecoins, such as USDT, sUSD, DAI, TUSD, USDC, or LUSD, can be deposited.
* DeFi Tokens are yVaults in which tokens of various DeFi protocols, such as UNI, SNX, AAVE, or COMP, can be deposited.
* Curve Pools are yVaults in which the Curve system’s liquidity tokens can be deposited.
* Retired yVaults are ones that are stopping their operation or have already stopped it.

For each strategy, its respective smart contract is created. Let’s consider the most common strategies:

* Aave Lender Optimizer [195] deposits tokens to AAVE to receive interest and earn stkAAVE tokens. After tokens are withdrawn, earned stkAAVE are sold in exchange for a specific yVault token, which is afterward invested back in the strategy.
* Aave Borrow [196] deposits DAI to AAVE to receive interest and earn AAVE tokens. Once unlocked, earned tokens are collected and sold for more DAI, which are invested back in the strategy. This strategy also borrows tokens against DAI. Borrowed tokens are deposited to the respective yVault to receive income.
* Kashi Lender [197] deposits USDC to Sushi via Kashi to receive interest and earn SUSHI tokens. Earned tokens are collected and sold for USDC, which are invested back in the strategy.
* Curve Yield Seeker [198] deposits WBTC to Curve Finance to earn CRV. Earned tokens are collected and sold for extra WBTC, which are invested back in the strategy. The strategy automatically switches to the most profitable Curve pool.
* Maker Delegate [199] deposits UNI to MakerDAO and mints DAI. The minted DAI are then deposited to the DAI yVault for receive income.
* Curve Boost [200] deposits steCRV to Curve Finance and stakes them to collect all available tokens and earn increased CRV reward due to CRV boost lock. Earned tokens are collected and sold for extra steCRV, which are invested back in the strategy.
* Convex Reinvest [201] deposits eCRV to Convex Finance to earn CRV and CVX (and any other available tokens). Earned tokens are collected and sold for more eCRV, which are invested back in the strategy.

Regarding the reliability of the system, note that there has already been a case of hacking. In that case, 11 million USD were stolen, but the attacker managed to collect only 2.8 million USD [202]. The attack was based on exploiting flash loans.

# [CONCLUSION](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/en/Z.1-Conclusion.md)
