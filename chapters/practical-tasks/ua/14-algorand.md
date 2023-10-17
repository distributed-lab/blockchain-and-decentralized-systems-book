# 14. Розробка смарт-контрактів у середовищі Algorand

Algorand – це облікова система та платформа виконання смарт-контрактів на основі технології блокчейн. Виконання 
запрограмованої логіки відбувається у віртуальній машині Algorand (Algorand Virtual Machine – AVM), яка подібна до 
Ethereum Virtual Machine. Однак, на відміну від Ethereum, Algorand дозволяє створювати ще й stateless смарт-контракти 
(які не мають власного стану). Їх називають смарт-підписи (smart signatures). Для розробки смарт-контрактів та 
смарт-підписів використовується спеціалізована мова програмування TEAL.

## Algorand Standard Token
✯ Використовуючи SDK для будь-якої з доступних мов програмування (Python, JavaScript, Go, Java), створіть fungible токен 
за специфікацією Algorand Standard Token (ASA). Деталі токену вкажіть на свій розсуд. Переведіть токен між двома 
адресами, виконайте процес заморозки та, коли попередні операції будуть виконані, знищіть токен.

> Детальніше про SDK та ASA можна дізнатися за посиланнями:
https://developer.algorand.org/docs/sdks/
https://developer.algorand.org/docs/get-details/asa/

Розглянемо приклад використання Python Algorand SDK для створення ASA та виконання потрібних операцій.

🤖

```python
import json
from algosdk import account, mnemonic
from algosdk.v2client import algod
from algosdk.future.transaction import AssetConfigTxn, AssetTransferTxn, AssetFreezeTxn

# Замініть на реальні значення
algod_address = "http://localhost:4001"
algod_token = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
#Налаштування клієнта Algod
algod_client = algod.AlgodClient(algod_token, algod_address)

#Створення облікових записів
creator_private_key, creator_address = account.generate_account()
receiver_private_key, receiver_address = account.generate_account()
freeze_private_key, freeze_address = account.generate_account()

#Відображення мнемонічної фрази облікових записів
print("Creator mnemonic:", mnemonic.from_private_key(creator_private_key))
print("Receiver mnemonic:", mnemonic.from_private_key(receiver_private_key))
print("Freeze mnemonic:", mnemonic.from_private_key(freeze_private_key))

#Отримання параметрів транзакції
params = algod_client.suggested_params()
#Створення ASA
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
#Підписання та відправлення транзакції створення ASA
signed_txn = txn.sign(creator_private_key)
txid = algod_client.send_transaction(signed_txn)
print("Transaction ID:", txid)

#Очікування підтвердження транзакції
wait_for_confirmation(algod_client, txid)

#Отримання інформації про створений актив
try:
asset_info = algod_client.asset_info(txn.asset_id())
print(json.dumps(asset_info, indent=2))
except Exception as e:
print(e)
#Передача токенів між адресами
transfer_txn = Asset
TransferTxn(
sender=creator_address,
sp=params,
receiver=receiver_address,
amt=5000,
index=txn.asset_id()
)

#Підписання та відправлення транзакції передачі
signed_transfer_txn = transfer_txn.sign(creator_private_key)
transfer_txid = algod_client.send_transaction(signed_transfer_txn)
print("Transfer Transaction ID:", transfer_txid)

#Очікування підтвердження транзакції передачі
wait_for_confirmation(algod_client, transfer_txid)
#Заморожування активів
freeze_txn = AssetFreezeTxn(
sender=freeze_address,
sp=params,
index=txn.asset_id(),
target=receiver_address,
new_freeze_state=True
)

#Підписання та відправлення транзакції заморожування
signed_freeze_txn = freeze_txn.sign(freeze_private_key)
freeze_txid = algod_client.send_transaction(signed_freeze_txn)
print("Freeze Transaction ID:", freeze_txid)

#Заморожування активів
freeze_txn = AssetFreezeTxn(
sender=freeze_address,
sp=params,
index=txn.asset_id(),
target=receiver_address,
new_freeze_state=True
)

#Підписання та відправлення транзакції заморожування
signed_freeze_txn = freeze_txn.sign(freeze_private_key)
freeze_txid = algod_client.send_transaction(signed_freeze_txn)
print("Freeze Transaction ID:", freeze_txid)

#Очікування підтвердження транзакції заморожування
wait_for_confirmation(algod_client, freeze_txid)

#Знищення ASA
destroy_txn = AssetConfigTxn(
sender=creator_address,
sp=params,
index=txn.asset_id(),
strict_empty_address_check=False,
manager="", # Empty address
reserve="", # Empty address
freeze="", # Empty address
clawback="" # Empty address
)
#Підписання та відправлення транзакції знищення
signed_destroy_txn = destroy_txn.sign(creator_private_key)
destroy_txid = algod_client.send_transaction(signed_destroy_txn)
print("Destroy Transaction ID:", destroy_txid)

#Очікування підтвердження транзакції знищення
wait_for_confirmation(algod_client, destroy_txid)
Визначення функції wait_for_confirmation
def wait_for_confirmation(client, txid):
"""
Wait until the transaction is confirmed or rejected, or until 'timeout'
number of rounds have passed.
Args:
txid (str): the transaction ID to wait for
"""
last_round = client.status()["last-round"]
while True:
txinfo = client.pending_transaction_info(txid)
if txinfo.get("confirmed-round", 0) > 0:
print("Transaction confirmed in round", txinfo["confirmed-round"])
break
elif txinfo["pool-error"]:
raise Exception("Transaction rejected pool error:", txinfo["pool-error"])
client.status_after_block(last_round + last_round += 1)


#Виконання коду
if name == "main":
main()
```
Цей приклад демонструє створення fungible токену за допомогою Algorand Standard Token (ASA) та виконання ряду операцій, 
таких як передача токенів між адресами, заморожування активів та знищення ASA.

Переконайтеся, що ви замінили `algod_address` та `algod_token` на реальні значення вашого вузла. Цей приклад 
використовує Python SDK для взаємодії з системою Algorand. Після виконання цього коду, ви зможете створити ASA, 
перевести токени, заморозити токени та знищити ASA.

## Escrow-контракт
✯ Створіть stateless-контракт, який буде виступати в ролі простого escrow-рахунку – будь-хто може перевести монети на 
рахунок контракту, але тільки його власник може їх вивести.

> В рамках цього завдання ви познайомитеся з новою мовою програмування – TEAL. Ця мова була створена спеціально для 
розробки смарт-контрактів, які виконуються у AVM. Детальніше ознайомитися з AVM і TEAL, а також знайти підказки для 
виконання завдання можна за наступними посиланнями:
https://developer.algorand.org/docs/get-details/dapps/avm/
https://developer.algorand.org/docs/get-details/dapps/avm/teal/

> Дізнатися більше про роботу зі смарт-підписами можна за посиланнями:
https://developer.algorand.org/docs/get-details/dapps/smart-contracts/frontend/smartsigs/
https://developer.algorand.org/docs/get-details/dapps/smart-contracts/smartsigs/modes/#delegated-approval

> Для виконання цього та наступних практичних завдань ми рекомендуємо використовувати локальне середовище розробки 
Algorand Sandbox: https://github.com/algorand/sandbox.

## Hash Lock Contract
✯ Створіть смарт-контракт, на якому міститься певна кількість монет, які може розблокувати лише той, хто знає 
відповідний рядок тексту. Геш-значення рядку, закодоване алгоритмом Base64, задається у коді контракту. Контракт повинен 
приймати один аргумент – рядок тексту для розблокування монет. При перевірці відповідності геш-значення рядка тексту до 
заздалегіть встановленної константи, так само спочатку загешуйте, а потім закодуйте його. Якщо отримане значення 
відповідає заданій константі, контракт повинен розблокувати монети та перевести їх на адресу того, хто надав правильну 
відповідь.

Згенерувати секрет можна за допомогою такої команди:

`python3 -c "import hashlib;import base64;print(base64.b64encode(hashlib.sha256(str(СЕКРЕТНИЙ_РЯДОК).encode('utf-8')).digest()).decode('utf-8'))"`

✯ Скомпілюйте контракт, розгорніть його та поповніть будь-якою кількістю монет на власний розсуд, після чого перевірте 
правильність його роботи, надавши спочатку невірний секретний рядок, а потім вірний.

> Важливо! Памʼятайте, що під час виклику смарт-контракту аргументи повинні передаватися закодованими в Base64.  Для 
кодування рядка можна скористатися наступною командою в терміналі вашої ОС:

`echo -n "СЕКРЕТНИЙ_РЯДОК" | base64`

> Для компіляції і перевірки контрактів, відправки транзакцій та інших розповсюджених завдань Algorand має свій інтерфейс 
командного рядка – GOAL. Дізнатися про GOAL та команди, які вам знадобляться можна за посиланнями:
https://developer.algorand.org/docs/clis/goal/goal/
https://developer.algorand.org/docs/clis/goal/clerk/clerk/

✯ Розширте розроблений Hash Lock контракт до HTLC (Hash Time Lock Contract). До умов витрати монет за прообразом 
геш-значення додати умову блокування за часом, так щоб можна було в коді контракту встановити проміжок часу, протягом 
якого, з моменту підтвердження транзакції блокування монет, неможливо їх розблокувати. А коли час вже минув будь-хто 
маючи прообраз геш-значення (рядок тексту) може розблокувати монети.