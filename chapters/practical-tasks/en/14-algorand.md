# 14. Development of smart contracts in the Algorand environment

Algorand is an accounting system and smart contract execution platform based on blockchain technology. The programmed
logic is executed in the Algorand Virtual Machine (AVM), which is similar to the Ethereum Virtual Machine. However,
unlike Ethereum, Algorand also allows for creating  stateless smart contracts (which do not have their own state).
These are called smart signatures. The specialized programming language TEAL is used to develop smart contracts and
smart signatures.

## Algorand Standard Token
✯ Using the SDK for any available programming language (Python, JavaScript, Go, Java), create a fungible token according
to the Algorand Standard Token (ASA) specification. Specify the details of the token at your discretion. Transfer
the token between two addresses, perform the freezing process, and when the previous operations are completed, destroy
the token.

> You can learn more about SDK and ASA through the following links:
https://developer.algorand.org/docs/sdks/
https://developer.algorand.org/docs/get-details/asa/

Let's consider an example of using the Python Algorand SDK to create ASAs and perform necessary operations.

🤖

```python
import json
from algosdk import account, mnemonic
from algosdk.v2client import algod
from algosdk.future.transaction import AssetConfigTxn, AssetTransferTxn, AssetFreezeTxn

# Replace with real values
algod_address = "http://localhost:4001"
algod_token = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"

# Algod client setup
algod_client = algod.AlgodClient(algod_token, algod_address)

# Account creation
creator_private_key, creator_address = account.generate_account()
receiver_private_key, receiver_address = account.generate_account()
freeze_private_key, freeze_address = account.generate_account()

# Display account mnemonics
print("Creator mnemonic:", mnemonic.from_private_key(creator_private_key))
print("Receiver mnemonic:", mnemonic.from_private_key(receiver_private_key))
print("Freeze mnemonic:", mnemonic.from_private_key(freeze_private_key))

# Get transaction parameters
params = algod_client.suggested_params()

# Create ASA
asa_details = {
    "asset_name": "ExampleToken",
    "unit_name": "EXT",
    "total": 1000000,
    "decimals": 0,
    "default_frozen": False,
    "manager": creator_address,
    "reserve": creator_address,
    "freeze": freeze_address,
    "clawback": creator_address,
    "url": "https://example.com",
    "metadata_hash": b"1234"
}

txn = AssetConfigTxn(
    sender=creator_address,
    sp=params,
    total=asa_details["total"],
    default_frozen=asa_details["default_frozen"],
    unit_name=asa_details["unit_name"],
    asset_name=asa_details["asset_name"],
    manager=asa_details["manager"],
    reserve=asa_details["reserve"],
    freeze=asa_details["freeze"],
    clawback=asa_details["clawback"],
    url=asa_details["url"],
    decimals=asa_details["decimals"],
    metadata_hash=asa_details["metadata_hash"]
)

# Sign and send ASA creation transaction
signed_txn = txn.sign(creator_private_key)
txid = algod_client.send_transaction(signed_txn)
print("Transaction ID:", txid)

# Wait for confirmation of ASA creation transaction
wait_for_confirmation(algod_client, txid)

# Get information about created asset
try:
    asset_info = algod_client.asset_info(txn.asset_id())
    print(json.dumps(asset_info, indent=2))
except Exception as e:
    print(e)

# Transfer tokens between addresses
transfer_txn = AssetTransferTxn(
    sender=creator_address,
    sp=params,
    receiver=receiver_address,
    amt=5000,
    index=txn.asset_id()
)

# Sign and send transfer transaction
signed_transfer_txn = transfer_txn.sign(creator_private_key)
transfer_txid = algod_client.send_transaction(signed_transfer_txn)
print("Transfer Transaction ID:", transfer_txid)

# Wait for confirmation of transfer transaction
wait_for_confirmation(algod_client, transfer_txid)

# Freeze assets
freeze_txn = AssetFreezeTxn(
    sender=freeze_address,
    sp=params,
    index=txn.asset_id(),
    target=receiver_address,
    new_freeze_state=True
)

# Sign and send freeze transaction
signed_freeze_txn = freeze_txn.sign(freeze_private_key)
freeze_txid = algod_client.send_transaction(signed_freeze_txn)
print("Freeze Transaction ID:", freeze_txid)

# Wait for confirmation of freeze transaction
wait_for_confirmation(algod_client, freeze_txid)

# Destroy ASA
destroy_txn = AssetConfigTxn(
    sender=creator_address,
    sp=params,
    index=txn.asset_id(),
    strict_empty_address_check=False,
    manager="",  # Empty address
    reserve="",  # Empty address
    freeze="",  # Empty address
    clawback=""  # Empty address
)

# Sign and send destroy transaction
signed_destroy_txn = destroy_txn.sign(creator_private_key)
destroy_txid = algod_client.send_transaction(signed_destroy_txn)
print("Destroy Transaction ID:", destroy_txid)

# Wait for confirmation of destroy transaction
wait_for_confirmation(algod_client, destroy_txid)

# Define wait_for_confirmation function
def wait_for_confirmation(client, txid):
   
# Wait until the transaction is confirmed or rejected, or until 'timeout'
    number of rounds have passed.
    Args:
        txid (str): the transaction ID to wait for
   
    last_round = client.status()["last-round"]
    while True:
        txinfo = client.pending_transaction_info(txid)
        if txinfo.get("confirmed-round", 0) > 0:
            print("Transaction confirmed in round", txinfo["confirmed-round"])
            break
        elif txinfo["pool-error"]:
            raise Exception("Transaction rejected pool error:", txinfo["pool-error"])
        client.status_after_block(last_round + last_round += 1)

# Execute code
if __name__ == "main":
    main()
```
This example demonstrates the creation of a fungible token using Algorand Standard Token (ASA) and performing a series
of operations such as transferring tokens between addresses, freezing assets, and destroying ASA. 

Ensure you have replaced `algod_address` and `algod_token` with the actual values of your node. This example uses
Python SDK to interact with the Algorand system. After executing this code, you will be able to create ASA, transfer
tokens, freeze tokens, and destroy ASA.

## Escrow-contract
✯ Create a stateless contract that will act as a simple escrow account - anyone can transfer coins to the contract account,
but only its owner can withdraw them.

> As part of this task, you will be introduced to a new programming language - TEAL. This language was specifically created
for developing smart contracts that are executed on AVM. You can learn more about AVM and TEAL, as well as find 
hints for completing the task, at the following links:
https://developer.algorand.org/docs/get-details/dapps/avm/
https://developer.algorand.org/docs/get-details/dapps/avm/teal/

>You can learn more about working with smart signatures by following the links:
https://developer.algorand.org/docs/get-details/dapps/smart-contracts/frontend/smartsigs/
https://developer.algorand.org/docs/get-details/dapps/smart-contracts/smartsigs/modes/#delegated-approval

> To perform this and future practical tasks, we recommend using the Algorand Sandbox local development environment.
https://github.com/algorand/sandbox.

## Hash Lock Contract
✯ Create a smart contract that contains a certain amount of coins that can only be unlocked by someone who knows the
corresponding text string. The hash value of the string, encoded with the Base64 algorithm, is specified in the
contract code. The contract should accept one argument - the text string to unlock the coins. When checking the
hash value of the text string against a pre-set constant, also hash and encode it first. If the resulting value
matches the specified constant, the contract should unlock the coins and transfer them to the address of the 
person who provided the correct answer.

You can generate a secret using the following command:

`python3 -c "import hashlib;import base64;print(base64.b64encode(hashlib.sha256(str(SECRET_LINE).encode('utf-8')).digest()).decode('utf-8'))`

✯ Compile the contract, deploy it, and fund it with any amount of coins at your discretion, then verify its correctness
by providing an incorrect secret string first and then the correct one.

> Important! Remember that during the invocation of a smart contract, arguments must be passed encoded in Base64. 
To encode a string, you can use the following command in the terminal of your OS:

`echo -n "SECRET_LINE" | base64`

> Algorand has its own command-line interface, GOAL, for compiling and verifying contracts, sending transactions, and other
distributed tasks. You can learn about GOAL and the commands you will need at the following links:
https://developer.algorand.org/docs/clis/goal/goal/
https://developer.algorand.org/docs/clis/goal/clerk/clerk/

✯ Extend the developed Hash Lock contract to HTLC (Hash Time Lock Contract). Add a time lock condition to the
conditions for spending coins based on the hash value, so that a time interval can be set in the contract code 
during which it is impossible to unlock the coins from the moment the coin-locking transaction is confirmed. 
And when the time has elapsed, anyone with the hash value (text string) can unlock the coins.