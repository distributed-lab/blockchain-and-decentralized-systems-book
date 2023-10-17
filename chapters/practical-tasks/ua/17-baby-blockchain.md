# 17. Реалізація власного блокчейну

Основною метою цього розділу є розуміння базових засад функціонування технології блокчейн. Усвідомлення настає за 
рахунок реалізації власної мінімальної версії облікової системи (без децентралізації та алгоритму досягнення консенсусу, 
лише базові механіки обліку).

-> У п’ятому розділі частини першої навчального посібника «Блокчейн і децентралізовані системи» ми досить детально 
розкрили основні аспекти технології блокчейн.

> Примітка. Для успішного розуміння та виконання завдань цього розділу, бажано ознайомитися із додатковими матеріалами:
> https://bitcoin.org/bitcoin.pdf – Bitcoin White paper.
> https://andersbrownworth.com/blockchain/hash – Blockchain demo.

Для реалізації власної облікової системи на базі технології blockchain, необхідно визначити / обрати під яку сферу 
застосування ваша реалізація буде використана.

Для цього необхідно сформувати коротке технічне завдання (ТЗ) для обраного кейсу.

Як вибрати сферу застосування? Ми надаємо кілька варіантів для вас, де застосування / використання технології 
блокчейн – доцільно:
* реєстри; 
* соціальні мережі; 
* системи обміну даними; 
* аукціони; 
* цифрові активи та валюти; 
* платформи голосування; 
* системи управління сертифікатами; 
* системи цифрової ідентифікації; 
* банки; 
* біржі та сервіси обміну активами; 
* прийняття рішень для управління системою (Government).

Технічне завдання відповідно до обраної вами сфери застосування має містити вимоги для певного програмного виробу, 
програми або набору програм (продукт), які виконують певні функції у конкретному оточенні.

Такий документ має надавати відповіді на наступні запитання:
* Огляд та призначення системи/продукту.
* Зміст системи (кордону системи).
* Взаємодія (потенційна) продукту (з іншими продуктами та компонентами).
* Функції продукту (короткий опис).
* Вимоги до безпеки.
* Характеристики користувачів (хто є кінцевим користувачем системи).
* Обмеження.

Для програмної реалізації практичного завдання можна використовувати будь-яку зручну для вас мову програмування. 
Завдання полягає у реалізації мінімальної версії блокчейну. У вас є можливість самостійно вирішити, у якому вигляді 
виконувати завдання, але в будь-якому випадку нижче буде представлений рекомендований підхід та поради при розробці 
рішення.

## Діаграма класів

На першому етапі рекомендовано розробити (використати) діаграму класів, яку представлено на рисунку 1.

Наведемо опис набору класів, які рекомендовано використовувати:

`Hash`: клас-обгортка для використання геш-функції.

`Signature`: клас-обгортка для використання цифрового підпису (будь-який алгоритм).

`KeyPair`: клас, який призначено для роботи із ключами.

`Account`: клас, який призначено для роботи із гаманцем, створення операцій та підпису даних.

`Operation`: клас, який дозволяє створити операцію платежу.

`Transaction`: клас, який дозволяє створити транзакцію, що містить платежі користувачів.

`Block`: клас, що формує блок із транзакціями.

`Blockchain`: клас, що дозволяє створити ланцюжок блоків, базу даних монет та існуючих транзакцій.

`Node`: клас, який дозволяє підтримувати один вузол (навіть централізований варіант), що обробляє ланцюжок блоків, має 
мережеві інтерфейси та працює з певними налаштуваннями.

`UserApplication`: клас, що дозволяє користувачам запускати програму, яка підтримуватиме цифровий гаманець із певними 
налаштуваннями та базою даних.

`Wallet`: клас, який дозволяє генерувати та зберігати ключі, формувати нові транзакції та перевіряти статуси транзакцій.

![Img](/resources/img/practical-volume/17/2-classchart.png)

Більш детально з UML-діаграмою ви можете ознайомитись за посиланням:

https://miro.com/app/board/uXjVOctudsU=/?invite_link_id=391702096318

Нижче наведено детальний опис кожного із заявлених класів. За потреби, ви самостійно можете доповнювати той або інший клас 
своїми методами та об’єктами.

## Клас KeyPair 
На рисунку 2 представлено клас KeyPair та відповідні об’єкти та методи.

![Img](/resources/img/practical-volume/17/3-keypair.png)

**Об'єкти:**

`privateKey`: велике натуральне число, що потрібне для підпису операцій.

`publicKey`: точка на еліптичній кривій (у разі використання підписів на еліптичних кривих).

**Опис методів:**

`genKeyPair()`
функція, що виконує генерацію ключів та повертає об’єкт класу KeyPair.

**Опціональні методи (можуть спростити процес тестування):**

`printKeyPair()`
функція для виведення об'єктів ключової пари на екран.

> Зауваження та пропозиції:
> * Для спрощення завдання можна уникнути реалізації цього класу. Цей клас необхідний для підпису операцій (механізм 
перевірки яких дозволяє довести володіння обліковим записом та балансом). Якщо ваше рішення не передбачає 
криптографічних підписів транзакцій/операцій, ви можете пропустити це. 
> * Відкритий ключ може бути публічним об'єктом, причому доступ до особистого ключа варто зробити тільки для методів 
даного класу (private modifier). Можна використати будь-який алгоритм цифрового підпису.

## Клас Signature

На рисунку 3 представлено клас Signature та його методи.

![Img](/resources/img/practical-volume/17/4-signature.png)

**Опис методів:**

`signData()`
функція, що дозволяє виконати обчислення підпису. На вхід функція приймає особистий ключ та повідомлення, підпис якого 
буде виконуватися. Повертає дані цифрового підпису у вигляді масиву байтів. В залежності від алгоритму та бібліотеки, 
що використовується, типи вхідних та вихідних даних можуть відрізнятися.

`verifySignature()`
булева функція, що дозволяє виконати верифікацію підпису. На вхід функція приймає значення підпису, відкритого ключа та 
повідомлення. Виходом є значення true / false, залежно від результату верифікації.

**Опціональні методи (можуть спростити процес тестування):**

`printSignature()`
функція для виведення значення підпису на екран.

> Зауваження та пропозиції:
> * Функція підпису не є обов'язковою. Якщо ваша облікова система не передбачає криптографічного підпису транзакцій, 
можна не реалізовувати цей клас.
> * Згенеровані ключі (клас KeyPair) повинні відповідати обраному для використання алгоритму цифрового підпису.
> * Можна реалізувати свій алгоритм підпису, але задля економії часу рекомендується взяти заздалегідь реалізовану 
бібліотеку для підпису.

## Клас Account

На рисунку 4 представлено клас Account та його методи.

![Img](/resources/img/practical-volume/17/5-account.png)

**Об'єкти:**

`accountID`
унікальне значення, яке призначено для ідентифікації облікового запису в межах системи. Часто може бути відкритим ключем 
або його геш-значенням.

`publicKeys`
масив, в якому зберігаються об’єкти відкритих ключів, що належать одному обліковому запису. В більшості випадків 
достатньо одного відкритого ключа.

`balance`
кількість монет (ціле беззнакове число), що належать одному обліковому запису.

**Опис методів:**

`createAccount()`
функція, для створення акаунту в обліковій системі. Повертає об'єкт класу Account. Виконується присвоєння відкритого 
ключа (ключів) та призначення його обліковому запису.

`addKeyToWallet()`
функція, яка дозволяє додати до аккаунту новий відкритий ключ та використовувати його в подальшому для підпису певних 
операцій, які ініціюються від імені цього облікового запису. Функція не повертає значення.

`updateBalance()`
функція, що оновлює стан балансу користувача. На вхід подається ціле значення. Функція не повертає значення.

`createPaymentOp()`
функція, що дозволяє створити операцію платежу від імені даного облікового запису отримувачу. На вхід подається об’єкт 
облікового запису, на який буде створено платіж, сума переказу та індекс ключа в гаманці.

`getBalance()`
функція, що дозволяє отримати поточний баланс користувача цього акаунту. Повертає ціле значення.

`printBalance()`
функція, що дозволяє виводити на екран стан баланса користувача. Функція не повертає значення.

`signData()`
функція, що дозволяє користувачеві підписати довільні дані. На вхід приймає повідомлення та індекс відкритого ключа в 
масиві. Повертає значення цифрового підпису.

**Опціональні методи (можуть спростити процес тестування):**

`toString()`
функція, що дозволяє сформувати строку із об’єктом облікового запису. Повертає об’єкт класу String.

`print()`
функція, що виводить на екран усі об’єкти класу Account. Функція не повертає значення.

> Зауваження та пропозиції:
> * Якщо ваша реалізація облікової системи не призначена для зберігання фінансових транзакцій, а зберігає довільні 
записи, клас Account може не містити об'єкт з балансом.
> * Наявність класу Account не є обов'язковою. Дані можна додавати в блоки без ідентифікації їх походження.
> * Будьте уважні з реалізацією методу updateBalance().*

## Клас Operation

На рисунку 5 представлено клас Operation та його методи.

![Img](/resources/img/practical-volume/17/6-operation.png)

**Об'єкти:**

`sender`
обліковий запис відправника платежу.

`receiver`
обліковий запис отримувача платежу.

`amount`
сума переказу.

`signature`
дані підпису, що згенеровано відправником платежу.

**Опис методів:**

`createOperation()`
функція, що дозволяє створити операцію з усіма необхідними деталями та підписом. На вхід приймає облікові записи 
відправника та отримувача коштів, суму переказу та підпис тих даних, що було описано. Повертає об’єкт Operation.

`verifyOperation()`
функція, що виконує перевірку операції. До основних перевірок, які можна віднести до запропонованої імплементації, 
відносяться: перевірка суми переказу (сума не повинна перевищувати баланс відправника), перевірка підпису (за допомогою 
відкритого ключа відправника платежу). Функція повертає true/false в залежності від результатів перевірки операції.

**Опціональні методи (можуть спростити процес тестування):**

`toString()`
функція, що дозволяє сформувати строку з об’єктом операції. Повертає об’єкт класу String.

`printKeyPair()`
функція для відображення об’єктів операції. Функція не повертає значення.

> Зауваження та пропозиції:
> * Операція може містити додаткові поля, наприклад, коментарі і т.д. Логіка верифікації додаткових полів визначається 
виключно автором реалізації.
> * Описана імплементація містить тільки перевірку унікальності транзакції, перевірка унікальності операції відсутня. 
Слід зауважити, що в даному випадку зловмисник може створити нову транзакцію, додати до неї дану операцію та 
витратити монети з чужого облікового запису. Щоб не забути про таку вразливість під час реалізації, уявіть, що це може 
бути ваш обліковий запис.
> * У разі, якщо до конкретного облікового запису належить гаманець з великою кількістю ключів, не забудьте перевірити 
все на відповідність підпису.

## Клас Transaction
На рисунку 6 представлено клас Transaction та його методи.

![Img](/resources/img/practical-volume/17/7-transaction.png)

**Об'єкти:**

`transactionID`
унікальний ідентифікатор транзакції (геш-значення всіх інших полів транзакції).

`setOfOperations`
набір операцій платежів, які підтверджуються в даній транзакції.

`nonce`
значення для захисту від дублювання транзакцій з однаковими операціями.

**Опис методів:**

`createOperation()`
функція, яка дозволяє створити транзакція з усіма необхідними елементами. На вхід приймає список операцій і nonce. 
Повертає об’єкт типу Transaction.

**Опціональні методи (можуть спростити процес тестування):**

`toString()`
функція, що дозволяє сформувати строку з об’єктом транзакції. Повертає об’єкт класу String.

`printKeyPair()`
функція для відображення об'єктів транзакції. Функція не повертає нічого.

> Зауваження та пропозиції:
> * У випадку, якщо не забезпечити контроль дублювання транзакції, валідатори можуть не визначити, що необхідну 
транзакцію вже було додано до історії. Для цього можна використовувати значення nonce, яке впливає на формування 
ідентифікатора транзакції (геш-значення всіх інших полів транзакції).

## Клас Hash

На рисунку 7 представлено клас Hash, що містить лише один метод – SHA256

![Img](/resources/img/practical-volume/17/8-hash.png)

**Опис методів:**

`SHA256()`
Функція, яка приймає на вхід массив даних і повертає геш-значення цих даних у вигляді масиву байтів (в даному випадку в 
якості алгоритма гешування використовується функція SHA2 на довжині 256 бітів).

> Зауваження та пропозиції:
> * Ви можете використовувати будь-який алгоритм гешування, який вважаєте за необхідне. Рекомендовано використовувати 
вже існуючі бібліотеки. Розробка власної версії алгоритму гешування вітається, проте потребує чималого часу.

## Клас Block
На рисунку 8 представлено клас Block та його методи.

![Img](/resources/img/practical-volume/17/9-block.png)

**Об'єкти:**

`blockID`
унікальний ідентифікатор блоку (геш-значення всіх інших даних).

`prevHash`
ідентифікатор попереднього блоку (необхідний для забезпечення перевірки цілісності історії).

`setOfTransactions`
список транзакцій, який підтверджується в даному блоку.

**Опис методів:**

`createBlock()`
функція, що дозволяє створити блок з усіма необхідними елементами. На вхід приймає список транзакцій та ідентифікатор 
попереднього блоку. Повертає об’єкт Block.

**Опціональні методи (можуть спростити процес тестування):**

`toString()`
функція, що дозволяє створити строку з об’єктів блоку. Повертає об’єкт класу Block.

`printKeyPair()`
функція для відображення об’єктів блоку. Функція не повертає нічого.

## Клас Blockchain

На рисунку 9 представлено клас Blockchain та його методи.

![Img](/resources/img/practical-volume/17/10-blockchain.png)

**Об'єкти:**

`coinDatabase`
таблиця, яка відображає поточний стан балансів в системі. В якості ключа використовується ідентифікатор облікового 
запису, в якості значення – баланс користувача.

`blockHistory`
масив, який зберігає всі блоки, які було додано в історію.

`txDatabase`
масив, який зберігає всі транзакції в історії. Його буде використано для більш швидкого доступу під час перевірки 
існування транзакції в історії (захист від дублювання).

`faucetCoins`
ціле значення, яке визначає кількість монет для тестування.

**Опис методів:**

`initBlockchain()`
функція, що виконує ініціалізацію блокчейну. Насправді, відбувається створення генезіс-блоку та додавання його до 
історії.

`getTokenFromFaucet()`
функція, що дозволяє отримати невелику кількість монет для тестів. Оновлює стан coins Database і балансу облікового 
запису, який цей метод викликав.

`checkBlock()`
функція, що виконує перевірку блоку з усіма транзакціями, що входять до його складу.

`connectBlock()`
функція, що дозволяє приєднати блок до локального ланцюжку блоків і оновити поточний стан облікової системи на цьому 
вузлі.

`getTokenFromFaucet()`
функція, яка дозволяє отримати поточний стан облікових записів та балансів.

**Опціональні методи (можуть спростити процес тестування):**

`toString()`
функція, яка формує строку з об’єктів блокчейну. Повертає об’єкт класу String, що знадобиться при відлагодженні 
програми.

`print()`
функція для виведення головних елементів класу на екран, що знадобиться для логування роботи вузла.

> Зауваження та пропозиції:
> * Функція checkBlock повинна містити набір наступних перевірок:*
> 1. Перевірку того, що блок містить посилання на останній актуальний блок в історії;*
> 2. Перевірка того, що транзакції у блоці ще не було додано в історію;*
> 3. Перевірка на те, що блок не містить транзакцій, які можуть конфліктувати.*
> 4. Перевірка кожної операції в транзакції:
>    1. Перевірка підпису;
>    2. Перевірка, що операція не використовує більше монет, ніж зберігається на балансі облікового запису відправника.

## Клас Node

На рисунку 10 представлено клас Node та його методи.

![Img](/resources/img/practical-volume/17/11-node.png)

**Об'єкти:**

`blockchain`
обʼєкт класу Blockchain зазначає, що вузол буде обробляти певний ланцюжок блоків.

`connections`
цей обʼєкт має відповідати за перелік усіх інших вузлів мережі. Відстежувати їх статуси, і відкривати пряме мережеве 
зʼєднання з певною кількістю вузлів з цього переліку. Отримувати і передавати мережею дані щодо існуючих вузлів, для 
процессу розвідування мережі.

`settings`
масив або відображення, що потрібне для зберігання налаштувань з якими працює поточний вузол. Наприклад бажана 
кількість мережевих зʼєднаннь, максимально дозволене навантаження на центральний процесор і оперативну паʼять, тощо.

**Опис методів:**

`startNode()`
функція, запускає процеси усіх потрібних модулів повного вузла мережі. Наприклад, мережевий модуль, модуль обробки 
ланцюжка блоків, модуль взаємодії з користувачем / адміністратором вузла.

`startNetworking()`
функція, запускає мережевий модуль та взаємодію з іншими вузлами мережі.

`updateConfiguration()`
функція, що дозволяє оновити налаштування поточного вузла.

`stopNode()`
функція, яка завершує виконання процесів і модулів поточного вузла, зберігає все необхідне на диск, для можливості 
коректного запуску в майбутньому.

**Опціональні методи (можуть спростити процес тестування):**

`getStatus()`
функція, яка повертає дані про поточний стан вузла. Наприклад, висота останнього відомого блоку, його геш-значення, 
поточну кількість мережевих зʼєднаннь, обсяг використовуємої оперативної памʼяті, обсяг використаного дискового 
простору, тощо.

`rescanBlockchain()`
функція для ініціювання перевірки всього ланцюжка блоків від початку до останнього відомого, з метою виправлення можливо 
накопичених помилок.

## Клас UserApplication

На рисунку 11 представлено клас UserApplication та його методи.

![Img](/resources/img/practical-volume/17/12-app.png)

**Об'єкти:**

`wallet`
обʼєкт класу Wallet зазначає, що застосунок буде обробляти певний цифровий гаманець.

`connection`
обʼєкт який відповідає за підтримку зʼєднання гаманця з повним вузлом мережі для синхронізації свого стану і 
розповсюдження створених транзакцій.

`txDatabase`
база даних що використовується для зберігання транзакцій, що мають відношення саме до поточного гаманця. Зберігання 
інших даних гаманця (ключі, адреси, налаштування) може виконуватись в цій або окремій базі даних.

`settings`
обʼєкт в оперативній паʼяті, що зберігає, оновлює і застосовує поточні налаштування застосунку користувача.

**Опис методів:**

`startApp()`
функція, запускає процеси усіх потрібних модулів застосунка користувача. Наприклад, мережевий модуль, модуль 
синхронізації транзакцій, модуль взаємодії з користувачем цифрового гаманця.

`startNetworking()`
функція, що запускає модуль взаємодії з довіреним вузлом мережі.

`updateConfiguration()`
функція, що дозволяє виконати оновлення налаштувань застосунка за ініціативою користувача.

`stopApp()`
функція, яка завершує виконання процесів і модулів застосунка користувача, зберігає все необхідне на диск, для 
можливості коректного запуску в майбутньому.

**Опціональні методи (можуть спростити процес тестування):**

`getStatus()`
функція, яка повертає дані про поточний стан застосунка. Наприклад, висота блоку до якого була успішно виконана 
синхронізація, кількість транзакцій користувача що отримали повне підтвердження, статус зʼєднання з повним вузлом 
мережі, обсяг використовуємої оперативної памʼяті, обсяг використаного дискового простору, тощо.

`rescanWallet()`
функція що видаляє усі відомі застосунку транзакції і ініціює сканування всього ланцюжка блоків від початку до 
останнього відомого повному вузлу мережі, з метою поновій знайти всі транзакції що відноситься до поточного гаманця.

## Клас Wallet
На рисунку 12 представлено клас Wallet та його методи.

![Img](/resources/img/practical-volume/17/13-wallet.png)

**Об'єкти:**

`account`
обʼєкт класу Account потрібен для зазначення, що гаманець працюватиме з певним ідентифікатором і певними відкритими 
ключами.

`userTxes`
масив транзакцій або відображення геш-значень до транзакцій, який зберігає всі транзакції, які мають відношення до 
поточного акаунту.

**Опис методів:**

`generateNewKeyPair()`
функція, що генерує нову ключову пару, відправляє відкритий ключ на додавання в акаунт користувача транзакцією, а 
особистий ключ записує у окреме захищене сховище. Додатково може ініціювати пошук транзакцій що відноситься до нового 
відкритого ключа.

`createTransaction()`
функція, що формує, заповнює, підписує та ініціює розповсюдження транзакції через повний вузол мережі.

`getBalance()`
функція, що звертається до історії транзакцій поточного гаманця, робить розрахунок поточного балансу і повертає у 
вигляді числа. Або робить відповідний запит повному вузлу мережі для отримання поточного балансу за ідентифікатором 
акаунту.

`rescanTransactions()`
функція, що оновлює дані щодо статусу підтвержденності відомих гаманцю транзакцій, отримуючи дані від повного вузла 
мережі.

**Опціональні методи (можуть спростити процес тестування):**

`getStatus()`
функція, яка повертає важливу статистику і поточний стан гаманця, наприклад кількість транзакцій в історії користувача, 
кількість ключових пар гаманця тощо.

`makeWalletBackup()`
функція, що повертає (або записує у файл) дані особистих ключів гаманця у відкритому або захищеному вигляді.

`restoreFromBackup()`
функція, що приймає або зчитує з файлу дані особистих ключів і зберігає у відповідному захищеному сховищі. Додатково 
може ініціювати сканування гаманця для знаходження повʼязаних з новими ключами транзакції.

*Зауваження та пропозиції:*
* *Функція підпису не є обов'язковою. Якщо ваша облікова система не передбачає криптографічного підпису транзакції, можна 
не реалізовувати метод signData().*

Для розуміння того, як це може бути реалізовано, наводимо приклад простої реалізації власного блокчейну для кейсу «електронне голосування» на мові програмування Go, який створено за допомогою штучного інтелекту:

🤖

```go
package main
import (
  "crypto/sha256"
  "encoding/hex"
  "fmt"
  "strconv"
  "strings"
  "time"
)
const difficulty = 3
// Block represents each 'item' in the blockchain
type Block struct {
  Index      int
  Timestamp  string
  Vote       string
  Hash       string
  PrevHash   string
  Difficulty int
  Nonce      int
}
// Blockchain is a series of validated Blocks
var Blockchain []Block

// HashBlock generates a hash of all block information
func HashBlock(block Block) string {
  record := strconv.Itoa(block.Index) + block.Timestamp + block.Vote + block.PrevHash + strconv.Itoa(block.Nonce)
  hash := sha256.Sum256([]byte(record))
  return hex.EncodeToString(hash[:])
}
// CreateNewBlock creates a new block in the blockchain
func CreateNewBlock(vote string) Block {
  var newBlock Block
  t := time.Now()
  newBlock.Index = len(Blockchain)
  newBlock.Timestamp = t.String()
  newBlock.Vote = vote
  newBlock.PrevHash = Blockchain[len(Blockchain)-1].Hash
  newBlock.Difficulty = difficulty
  newBlock.Nonce = 0
  newBlock.Hash = calculateBlockHash(newBlock)
  return newBlock
}
// IsBlockValid returns if the block is valid by checking index, and comparing the hash of the previous block
func IsBlockValid(newBlock, oldBlock Block) bool {
  if oldBlock.Index+1 != newBlock.Index {
    return false
  }
  if oldBlock.Hash != newBlock.PrevHash {
    return false
  }
  if calculateBlockHash(newBlock) != newBlock.Hash {
    return false
  }
  return true
}
// ReplaceChain replaces the blockchain with a new chain
func ReplaceChain(newBlocks []Block) {
  if len(newBlocks) > len(Blockchain) {
    Blockchain = newBlocks
  }
}
// make a vote
func makeVote(vote string) {
  newBlock := CreateNewBlock(vote)
  if IsBlockValid(newBlock, Blockchain[len(Blockchain)-1]) {
    Blockchain = append(Blockchain, newBlock)
  }
}
func calculateBlockHash(block Block) string {
  for block.Nonce < 1000000000 {
    hash := HashBlock(block)
    if strings.HasPrefix(hash, strings.Repeat("0", block.Difficulty)) {
      return hash
    }
    block.Nonce++
  }
  return ""
}
func main() {
  // create the genesis block
  t := time.Now()
  difficulty := 3
  genesisBlock := Block{0, t.String(), "", "", "", difficulty, 0}
  genesisBlock.Hash = HashBlock(genesisBlock)
  Blockchain = append(Blockchain, genesisBlock)

  // create 10 dummy votes
  for i := 0; i < 10; i++ {
    vote := "vote" + strconv.Itoa(i)
    makeVote(vote)
    fmt.Printf("New block added to the blockchain with vote: %s\n", vote)
    fmt.Printf("Hash: %s\n", Blockchain[len(Blockchain)-1].Hash)
    fmt.Println("\n-------------------\n")
  }
}
```

> У цьому коді параметр difficulty  встановлює кількість нулів, які мають бути на початку гешу, щоб він вважався дійсним. 
Функція CalculateBlockHash виконує PoW, збільшуючи nonce, доки не буде знайдено геш, який відповідає критеріям 
складності. Кожного разу, коли проводиться голосування, функція makeVote створює новий блок і виконує PoW перед 
додаванням його в блокчейн, якщо він дійсний.

> Аналіз типових недоліків, які присутні в цій реалізації має допомогти вам не повторити їх. Отже, що тут не так?
> 1. Функція HashBlock – не використовує параметр difficulty для формування підсумкового гешу. Це ніби не критично з точки 
зору безпеки, але гарним рішенням це точно не є.
> 2. Параметр difficulty не змінюється залежно від частоти знаходження блоків. Це означає, що буде величезна кількість 
orphan блоків у разі великого значення hashpower. Це дійсно може перерости у проблему, яка полягає у тому, що може 
з’явитися  велика кількість «форків», і, як наслідок – складність їх резолвінгу.
> 3. Принципово неправильне використання difficulty, який жорстко прописаний у коді. На них повинен спиратися консенсус, 
а дана реалізація цього не підтримує.
> 4. IsBlockValid – дуже цікава функція з точки зору її імплементації саме штучним інтелектом. Загалом добре, що вона є, 
але перевірок має бути значно більше для реального використання. Так саме, й щодо timestamp, який знаходиться в певному 
діапазоні. Difficulty і Nonce параметри знову ж таки повинні перевірятися саме тут, але оскільки вони фіктивні, звісно 
не перевіряються.
> 5. Replace chain реалізований неправильно. Він повинен орієнтуватися на складність ланцюжка, а не на його довжину.
> 6. Загальне зауваження. Реалізація в такому вигляді описує модель 1 голос = 1 блок. При такому підході проголосувати 
може тільки творець блоку. Та слід зауважати, що немає функції підрахунку голосів. Таким чином, можна сказати, що це 
більш нагадує реєстр, а не систему голосування.

Для порівняння, які аспекти можуть бути враховані людиною під час виконання такого навчального проекту, як приклад, 
надаємо посилання на репозиторій з реалізацією власного блокчейна для платформи голосування:

https://github.com/yehor-podporinov/baby_blockchain
https://GitHub/distributedlab.

✯ Спробуйте самостійно, посилаючись на рекомендації, що зазначені у цьому розділі, виконати всі етапи реалізації. 
Акцентуємо увагу, що дані рекомендації є лише зразком архітектури і покривають лише 20% всіх класів і методів, що 
потрібні для функціонування облікової системи і застосунків для її використання.

Додатково рекомендуємо наступні кейси для реалізації:
* Управління та відстеження ланцюжків поставок. 
* Транскордонні платежі та грошові перекази.
* Безпечне зберігання та обмін медичними записами.
* Децентралізоване зберігання файлів.
* Децентралізоване керування ідентифікацією.
* Рішення для боротьби з підробками для предметів розкоші.
* Володіння нерухомістю та управління транзакціями.
* Децентралізовані ринки прогнозів.
* Децентралізовані автономні організації (DAO).