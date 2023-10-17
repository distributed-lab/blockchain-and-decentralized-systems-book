# 13. Development of smart contracts in the Cardano environment

Cardano is not just an open-source protocol and a decentralized accounting system based on blockchain. One of the most 
important features of Cardano is that the protocols underlying the platform are the result of scientific research 
and are extensively documented in a series of scientific publications. Cardano has its own approach to the UTXO model,
asset creation and management, and smart contract mechanisms.

> To learn more about the fundamental architectural decisions and differences of Cardano from other blockchain-based systems,
we recommend familiarizing yourself with the information available at the following links:
https://iohk.io/en/blog/posts/2021/03/11/cardanos-extended-utxo-accounting-model/
https://developers.cardano.org/docs/native-tokens/
https://developers.cardano.org/docs/smart-contracts/

## Native non-fungible tokens

✯ Create and release a native token that will represent a certain value in the real world (such as a concert ticket,
a voucher for a free coffee, etc.) in a digitized form. The token policy should provide for a one-time emission,
meaning that the total number of tokens will be fixed, and all of them will be issued in a single transaction, and
then distributed among recipients.

> More information about working with native tokens and policies can be found at the following links:
https://developers.cardano.org/docs/native-tokens/minting
https://docs.cardano.org/native-tokens/learn*

How to do it? Our robot Mykhailo will help. In this example, a token called "FreeCoffee" with a fixed amount of 1000
tokens will be created.

Let's determine the sequence of actions required to create and issue a native token on Cardano.
1. Create an address and keys for token issuance.
2. Create a token policy file.
3. Generate a token policy identifier.
4. Check the balance of your wallet and obtain a unique UTXO number.
5. Construct a transaction with token issuance.
6. Sign the transaction.
7. Send the signed transaction.

After completing these steps, your native token will be created and issued on the Cardano blockchain. You can use
this token to represent various assets in the real world, such as vouchers, goods or services.

To transfer tokens between addresses, use transactions that include the sender, receiver, and the number  of tokens
to be sent. Don't forget to also send a small amount of ADA to cover fees and support UTXO.

It is important to note that compatible wallets such as Yoroi or Daedalus are recommended for interacting with tokens
on Cardano. These wallets allow you to view, send, and receive native tokens on Cardano, such as your "FreeCoffee"
token. To access tokens in the wallet, you may need to import a private key or restore the wallet from a mnemonic 
phrase associated with the address where you created the token.

You should  have the Cardano development environment installed, including cardano-cli and cardano-node. Make sure that your version of Cardano-CLI is equal to or newer than 1.25.0.

🤖

```
#First create an address and keys for token issuance:
cardano-cli address key-gen \
  --verification-key-file free_coffee.vkey \
  --signing-key-file free_coffee.skey
cardano-cli address build \
  --payment-verification-key-file free_coffee.vkey \
  --out-file free_coffee.addr \
  --mainnet
#Create the token policy file free_coffee_policy.script:
{
  "
keyHash": "KEY_HASH",
"type": "sig"
}
#Replace `KEY_HASH` with the hash of the key. This can be done by executing the following command:
```sh
cardano-cli address key-hash --payment-verification-key-file free_coffee.vkey > keyhash.txt
#Insert the obtained key hash from keyhash.txt instead of #KEY_HASH in the file free_coffee_policy.script.
#Generate the token policy ID:
cardano-cli transaction policyid --script-file free_coffee_policy.script > policyID.txt
#Check the balance of your wallet and get a unique UTXO number:
cardano-cli query utxo \
  --address $(cat free_coffee.addr) \
  --mainnet
#Construct a transaction with the issuance of the "FreeCoffee" token:
cardano-cli transaction build-raw \
  --tx-in TX_IN \
  – tx-out $(cat free_coffee.addr)+CHANGE+1000"FreeCoffee"
--mint="1000 FreeCoffee"
--invalid-hereafter SLOT_NUMBER_PLUS_1000
--fee FEE
--out-file free_coffee_tx.raw
--mainnet
#In this command, replace the following values:
#- `TX_IN` with the unique UTXO number of your wallet.
#- `CHANGE` with your balance minus the transaction cost and #the amount of ADA you want to leave at the address.
#- `SLOT_NUMBER_PLUS_1000` with the current slot plus 1000 (or #more) to set the transaction validity period.
#- `FEE` with the approximate cost of the transaction (check #the current fee recommendations).
#Sign the transaction:
```sh
cardano-cli transaction sign \
  --tx-body-file free_coffee_tx.raw \
  --signing-key-file free_coffee.skey \
  --mainnet \
  --out-file free_coffee_tx.signed
#Submit the signed transaction:
cardano-cli transaction submit \
  --tx-file free_coffee_tx.signed \
  --mainnet
#After successfully executing this command, your native token #"FreeCoffee" will be created and issued. You can check the #balance of the address where you created the token to see the #number of newly created tokens:
cardano-cli query utxo \
  --address $(cat free_coffee.addr) \
  --mainnet
#Now you can create transactions to distribute "FreeCoffee" #tokens among recipients. Note that these tokens can be #used to represent vouchers for free coffee or other assets #in the real world. In this example, we used a one-time #issuance policy, which means that the total number of #"FreeCoffee" tokens will be fixed and all of them will be #issued in one transaction. Additional tokens cannot be #created under this policy after issuance.
```

Thus, you can create and release a native token in Cardano that represents a certain real-world value, such as a 
voucher for a free coffee. Since the token emission policy involves a one-time issuance, you guarantee that the 
number of vouchers is limited and cannot be increased after the initial issuance. However, this implementation has
several drawbacks regarding  security and practical use. This code creates and emits a native Cardano token called
"FreeCoffee". Analyzing the code, several security and practical use issues can be identified.

>Security issues include:
> * Lack of hardware wallet usage: Using a hardware wallet such as Ledger or Trezor can help ensure the security of
private keys. In this scenario, private keys are stored locally, which can increase the risk of loss of funds due
to hacking or fraud.
> * No use of multi-signatures: Multi-signatures can add an additional layer of security as they require transaction
confirmation from multiple parties. In this case, a simple one-way signature is used, which can be a weak point in security.

> Practical use issues include:
> * Suboptimal token emission policy: The current token policy only allows creating "FreeCoffee" tokens once.

> So:
> 1. Security: Keys are generated on the user's device and stored in files (free_coffee.vkey, free_coffee.skey). This creates
a potential risk of private key leakage. It is recommended to store private keys on hardware wallets or in secure
storage devices.
> 2. Centralization: All native "FreeCoffee" tokens are created on one wallet. This may create a problem
of centralization, as control over tokens is concentrated in one entity. It is possible to consider using multi-signature
or distributing control rights over tokens among several participants, which would reduce centralization and increase
decentralization of the system.
> 3. Lack of token distribution mechanism: This code does not contain any instructions on distributing "FreeCoffee"
tokens among participants. You need to develop an additional mechanism for distributing tokens among participants,
considering different scenarios of use.
> 4. Lack of limits on the number of token issuances: The current code allows the owner of the `free_coffee.skey`
key to issue an unlimited number of tokens.
> 5. The code does not provide any means of managing "FreeCoffee" tokens after their issuance, such as the ability
to burn tokens, transfer ownership rights, or pre-establish rules for token exchange. It is recommended to consider
adding additional functions and interactions with tokens that will meet your needs and goals.
> 6. Lack of integration with user interfaces: This code uses only a command-line interface (CLI) to interact with
the Cardano system node. This may be inconvenient for end-users who would like to interact with your tokens through
a graphical interface or web application. You should consider developing and integrating with appropriate user interfaces
to facilitate access to your tokens and improve the overall user experience.

✯ To distribute tokens among recipients, create transactions that send tokens to the addresses of the recipients. For example,
you can create transactions that send 1 "FreeCoffee" token to the address of each recipient. Note that to transfer
tokens, you also need to transfer a small amount of ADA to cover fees and support UTXO.

## Token Bundles
✯ Create a _token bundle_ that will include two or more tokens from the previous task (for example, a concert ticket and 
a voucher for a drink).

> You can learn more about the policies for creating a token bundle at the following link:
https://docs.cardano.org/native-tokens/learn/#tokenbundles

## Native non-fungible tokens
✯ Create and release a native non-fungible token (NFT) that will represent ownership rights to a specific digital collectible
object (such as a unique painting or 3D model).

> More about NFT release can be found at the following link:
https://developers.cardano.org/docs/native-tokens/minting-nfts

## Programming smart contracts in Marlowe language
✯ Create a contract for a simple escrow account for buying and selling with a mediator. After the funds are deposited, 
the buyer must confirm receipt of the goods or dispute the agreement. The mediator can confirm or cancel the complaint. If the complaint is confirmed, the funds are returned to the buyer's address.

> You can learn more about the domain-specific programming language Marlowe by following this link:
https://play.marlowe-finance.io/doc/marlowe/tutorials/index.html
https://iohk.io/en/blog/posts/2022/03/04/diving-deeper-into-the-marlowe-playground/

> As a browser-based development environment, we recommend using Marlowe Playground:
https://play.marlowe-finance.io/

> Smart contracts in the Marlowe programming language can be developed in the Blockly visual environment. Learn more about
it at the following link: https://play.marlowe-finance.io/doc/marlowe/tutorials/playground-blockly.html

✯ Extend the logic of the previous contract by adding the possibility of compensation for defects in the received goods.
The percentage of the cost that will be refunded to the buyer in case of inadequate quality of the received goods
is agreed upon by the participants of the contract according to the 2-of-3 scheme (note that photos or other 
evidence of non-compliance with quality will be transmitted through off-chain channels). In case of confirmation
by at least two out of three participants, the compensation amount will be returned to the buyer's address, and
the rest will be transferred to the seller.

## Programming smart contracts in the Plutus language
✯ Reproduce the contract from the previous task (Programming smart contracts in Marlowe language) using the Plutus programming language.

> Plutus was developed in the functional programming language Haskell, and the smart contracts written in Plutus are 
actually programs in Haskell. If you want to delve deeper into smart contract development in the Cardano environment,
it will be useful to familiarize yourself with the syntax and basic constructs of Haskell:
https://learnxinyminutes.com/docs/haskell/

> More about Plutus and the process of developing smart contracts can be found at the following link:
https://docs.cardano.org/plutus/learn-about-plutus

> As a browser-based development environment, we recommend using Plutus Playground:
https://playground.plutus.iohkdev.io/