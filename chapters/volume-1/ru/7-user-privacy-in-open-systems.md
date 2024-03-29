# ПРИВАТНОСТЬ ПОЛЬЗОВАТЕЛЕЙ В ОТКРЫТЫХ СИСТЕМАХ


## 7.1 Приватность как понятие в цифровом мире

В начале ХХI века онлайн-платежи считались делом рискованным и обычно совершались с большой осторожностью. Использование реальных имен на онлайн-ресурсах было немыслимо, а анонимность деятельности в Интернете считалась почти абсолютной.

В некоторых странах и сейчас безопасность и благополучие пользователей зависят от степени защищенности их персональных данных или степени их анонимности в Сети. Профессор Пенсильванского университета Дэвид Кросби (David Crosbie) отметил, что внедрение по-настоящему анонимных платежей может улучшить ситуацию в развивающихся странах [99]. Именно в развивающихся странах защищенность данных пользователей имеет наибольшее значение, поскольку уровень доверия населения к органам законодательной, судебной и исполнительной власти зачастую очень низкий, а политика приватности не всегда добросовестно соблюдается при обработке персональных данных. В развитых странах зачастую существует отлаженная и устоявшаяся инфраструктура, где обеспечение информационной безопасности имеет свои особенности регулирования (например GDPR [125]).

Тема приватности и способов ее обеспечения достаточно объемна. Начать ее изучение лучше всего с понимания базовых определений.

### Важность сохранения приватности

На практике в открытых системах соблюдение приватности имеет огромное значение. Предположим, на одной улице есть пять ресторанов. Они конкурируют за поток посетителей и каждый стремится получить больший доход. Сведения о внутренних процессах ценообразования и доходах остаются тайной для конкурентов. Однако давайте представим, что будет, если кто-то опубликует сведения обо всех внутренних транзакциях и другую документацию этих пяти ресторанов в открытом доступе.

Менее успешные конкуренты могут руководствоваться нечестными намерениями и просто навредить наиболее успешному конкуренту материально, например подстроить пожар. При условии, что наиболее успешный ресторан был самым привлекательным для посетителей, поскольку там готовили вкусные блюда из качественных продуктов, от разорения этого заведения никто не выиграет, кроме менее успешных конкурентов.

Вопрос приватности актуален и в области научных исследований. Может случиться так, что группа ученых, которые сделали значимые открытия, пожелают остаться анонимными (как в случае с Bitcoin), например, объясняя это тем, что они работали в большой команде, каждый участник которой сделал свой вклад в открытие. В связи с этим может стать актуальным вопрос о возможностях легального анонимного финансирования научных исследований для развития независимых и свободных от цензуры исследовательских институтов.

Сегодня использование аналитических возможностей позволяет компаниям не только учитывать привычки пользователей, но и влиять на их поведение. Все это стало возможным благодаря обработке данных о покупках и других действиях пользователей в Сети. Эти данные приносят компаниям огромные доходы.

В качестве примера можно привести нашумевший случай в сети магазинов Target [75], когда один мужчина пришел в магазин и был очень возмущен тем, что его дочери на почту приходит большое количество купонов на памперсы и детскую одежду, хотя она еще даже школу не закончила и о детях думать ей очень рано. Менеджер магазина согласился, что это странно, и принес свои извинения. Все бы ничего, но девушка и вправду оказалась беременной. Как бы это странно ни выглядело, система прогнозирования беременности, которую разработал аналитик Target Эндрю Поул (Andrew Pole), позволила торговой компании узнать о беременности девушки намного раньше ее отца, анализируя предпочтения в покупках.

Эта история может вызвать разные эмоции, но можно отметить, что скорее всего конкретному человеку не понравится, если он узнает, что значительная часть его покупок была спровоцирована именно таким образом. Более того, если проанализировать полученные данные, то многие из них могут оказаться персональными. А если учесть, что этот человек мог не давать согласия на их обработку, то логично предположить, что он будет ощущать себя обманутым. Скорее всего, человек захочет ограничить доступ к данным, благодаря которым какая-то компания получит возможность манипулировать его поведением путем «выгодных предложений» в нужный момент.

Поэтому вопрос о приватности в цифровом мире занимает одно из первых мест среди тех, на которые следует обращать внимание. А учитывая развитие технологий и аналитических инструментов, он становится все актуальнее.

### Составляющие приватности

*Понятие приватность включает две основные составляющие: неотслеживаемость (untraceability) и анонимность (anonymity)*. Неотслеживаемость подразумевает невозможность отнести группу действий к некоторому пользователю в сети. Анонимность связана с невозможностью достоверно установить личность пользователя в этой сети. На рис. 7.1 приведен пример типичного человека, который желает защитить себя и, следовательно, свою приватность в сети.

![Рисунок 7.1 – Обеспечение анонимности пользователя в сети](/resources/img/volume-1/7.1-privacy-as-a-concept-in-the-digital-world/7.1-principles-of-user-privacy.png)

Понятие анонимность подразумевает наличие социального и технического компонента. Социальная анонимность включает сведения, которые может указывать пользователь о своей личности. Техническая анонимность куда сложнее, и основные усилия по обеспечению анонимности прилагаются именно в этой области.

Что же касается повышения уровня приватности, то это может обеспечиваться специальными механизмами для сокрытия следующих атрибутов активности пользователей:

* Прямой связи между всеми действиями, сообщениями, транзакциями
* Версий приложений и операционной системы
* Локализации приложений
* Временных меток сообщений и запросов
* Идентификаторов (имени пользователя, адреса электронной почты, номера телефона)
* Самого факта действия
* Местоположения устройств
* Сетевых адресов устройств

Некоторые реализации браузера Tor и клиента BitTorrent уже давно поддерживают расширенные опции приватности, которые помогают скрывать некоторые из перечисленных атрибутов. Методики могут быть действительно специфическими, например, использование случайных портов на сетевом уровне, высокая избыточность запросов, изменение разрешения экрана по умолчанию.

Как упоминалось ранее, Bitcoin показал, что финансовая система может быть прозрачной и поддающейся проверке для всех участников сети при сохранении определенного уровня приватности пользователей. Тем не менее, стало ясно, что для некоторых вариантов использования уровень прозрачности, обеспечиваемый технологией blockchain, является неприемлемым. Таким образом, новые методы для обеспечения более высокого уровня приватности пользователей были предложены и даже реализованы в некоторых проектах криптовалют. Эти методы эффективно решали вопросы приватности транзакций, оставляя их неизменяемыми и публично проверяемыми.

## 7.2 Приватность в цифровых валютах

Как упоминалось в разделе о платежных каналах (см. 4.8), развитие цифровых валют осложняется проблемами масштабируемости и приватности пользователей. Более того, любая система учета активов, использующая открытую базу данных, сталкивается с проблемой приватности [100]. А если речь идет о криптовалютах, то там принцип полной прозрачности истории транзакций вообще является базовым. Этот подраздел посвящен тому, как в таких условиях достичь приватности пользователей.

Далее мы ознакомимся с моделью приватности, принципами ее обеспечения, разберем несколько уже реализованных на практике подходов, а также проанализируем их достоинства и недостатки.

> **Данные о транзакциях, которые следует скрывать в первую очередь**
>> * *История происхождения монет* 
>> * *Суммы платежей* 
>> * *Адреса (идентификаторы) пользователей*
>> * *Сетевые адреса пользовательских устройств* 
>> * *Метаданные (время распространения в сети, версия кошелька, описание платежа и т. п.)*

В некоторых цифровых валютах используется специальная структура транзакций и модель учета монет, которая позволяет скрывать данные о некоторых деталях операций (табл. 7.1). Для сокрытия или запутывания определенных данных разные валюты применяют разные методы.

![Таблица 7.1 – Аспекты приватности в цифровых валютах](/resources/img/volume-1/7.2-privacy-in-digital-currencies/table-7.1-privacy.png)

### Слепые подписи

Рассмотрим один из механизмов, который позволяет нескольким сторонам взаимодействовать и обеспечивать при этом необходимый уровень приватности. Речь идет о слепой подписи.

*Слепая подпись (blind signature) – разновидность цифровой подписи, особенность которой состоит в том, что подписывающая сторона не может точно знать, что содержит подписываемый документ* [105]. Шаги взаимодействия участников следующие (рис. 7.2):

> 1. Автор выполняет ослепление (blinding) документа своим секретом и передает ослепленный документ подписанту.
> 2. Подписант с помощью своего ключа подписывает ослепленный документ и возвращает значение подписи автору.
> 3. Автор выполняет снятие ослепления (unblinding) подписанного документа своим секретом и предъявляет документ проверяющему.
> 4. Проверяющий видит обычный документ с обычной подписью, которая проверяется открытым ключом подписанта.

![Рисунок 7.2 – Схема использования слепой подписи](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.2-bling-sig.png)

Наиболее широко схемы слепых подписей применяются в сферах цифровых валют (для выпуска анонимных цифровых банкнот) и тайного голосования (для анонимного получения права голоса).

### Приватность в Биткоине по умолчанию

В Биткоине обеспечивается свойство анонимность, но его очень легко потерять на практике [101]. Не достигается в полной мере и свойство неотслеживаемость. Мы можем проанализировать граф транзакций и сделать заключение об их «происхождении» из определенных анонимных кошельков [102]. Если хотя бы один адрес был скомпрометирован в контексте анонимности, то можно установить и связь с определенными личностями. Самая простая реализация биткоин-кошелька способна обеспечить только минимальный уровень приватности.

Допустим, для каждого входящего платежа или сдачи пользователь создает новый адрес. Аудитор, анализирующий граф транзакций, в этом случае уже не может достоверно связать конкретные факты, которые касаются действий пользователей и распределения валюты между ними. Но даже в такой ситуации уровень приватности пользователя не так высок, как может показаться.

Обычно в Биткоине приватность пользователя зависит от его контрагентов. Получатель платежа будет знать историю происхождения монет, а отправитель монет будет знать, кому он их передает (адрес получателя), и, более того, сможет отслеживать историю дальнейшей передачи монет. Иначе говоря, генерация новых адресов усложняет возможность отслеживания цепочек, но не делает их абсолютно неотслеживаемыми. Существует ряд метаданных, которые могут оказаться доступными посторонним: характер сделки, данные о кошельке, данные о нахождении пользователя и т. д. Кроме того, существуют методы, алгоритмы, анализаторы, которые позволяют построить дерево из цепочек, найти зависимости по времени, по IP-адресам, по способу построения транзакций и с определенной вероятностью могут дать достаточно приближенную к реальности картину распределения монет [103].

Следует понимать, что если пользователь хочет потратить монеты, которые привязаны к нескольким разным адресам, то транзакция, которая будет содержать множество входов, принадлежащих одному пользователю, может полностью деанонимизировать его. При этом до появления такой транзакции каждая отдельная монета (непотраченный выход) могла быть не связана ни с остальными монетами пользователя, ни с самим пользователем.

Один из простейших способов запутывания истории транзакций – использование т. н. *миксеров (mixers)* [104]. Они позволяют смешать вместе монеты отдельных пользователей. Для этого составляется одна транзакция, а на ее вход подаются монеты от разных владельцев. Монеты со всех входов суммируются, при этом смешиваясь, а потом распределяются между всеми выходами, в которых снова указываются получатели. В таком случае невозможно однозначно определить историю передачи монеты, потому что истории смешались вместе, а потом каким-то образом разошлись. Такое смешивание в общих транзакциях можно проводить даже несколько раз подряд. Например, можно провести 7 таких транзакций подряд. В итоге мы получим монеты с неоднозначной историей. Однако этот способ не единственный и имеет определенные недостатки. Например, некоторые сервисы не принимают монеты, которые прошли через миксер.

Стоит также понимать, что миксеры являются организационным методом повышения уровня приватности. Поэтому соответствующие сервисы обычно требуют от пользователя полного доверия как в отношении сохранности монет, так и в отношении сохранения анонимности пользователя.

Чтобы обеспечить максимальный уровень приватности, нужно понять, какие же данные о транзакции следует скрывать прежде всего. Сюда относятся данные о происхождении монет, которые непосредственно влияют на свойство *взаимозаменяемость (fungibility)*, очень важного для денег и ценностей. На уровне протокола Bitcoin это свойство обеспечивается (все монеты одинаковые, и правила их обработки одинаковые для всех), но на практике fungibility легко нарушить. Например, некоторые мерчанты могут анализировать историю происхождения принимаемых монет и отклонять платежи, если она вызывает у них сомнения.

Следующее, что имеет смысл скрывать, это суммы платежей, адреса отправителя и получателя в теле транзакции. Важно также скрыть сетевые адреса пользователей, что обычно достигается при помощи даркнетов, где используются такие протоколы, как Freenet, Tor и I2P. Однако остается вопрос: как же скрыть суммы, историю и адреса?

### CoinJoin

Самый простой метод для запутывания графа транзакций называется *CoinJoin*. Его суть состоит в создании совместной транзакции, вследствие чего происхождение отправляемых монет становится неоднозначным. Формируется группа из пользователей, которые создают общую транзакцию, в рамках которой одновременно совершается несколько платежей. То есть пользователям не нужно создавать отдельные транзакции.

Впервые такую идею предложил Грегори Максвелл (Gregory Maxwell) в 2013 году на популярном форуме BitcoinTalk [106]. С тех пор было предложено и разработано большое количество модификаций данного метода. Каждая из них улучшала определенные свойства платежей. Давайте поговорим о том, как CoinJoin работает в чистом виде, а после рассмотрим несколько самых интересных его модификаций.

Представьте группу из трех пользователей, в которой каждый хочет приобрести товар в интернет-магазине. При этом для каждого из них магазин свой. Они создают одну транзакцию на три входа, по одному от каждого пользователя, три выхода, по одному на каждый интернет-магазин. Помимо этого, создается еще три выхода для сдачи (рис. 7.3). Далее все выходы перемешиваются между собой случайным образом. Каждый пользователь перепроверяет полученную транзакцию и подписывает соответствующий ему вход. В случае успеха транзакция считается правильной, распространяется по сети и получает подтверждение.

![Рисунок 7.3 – Схема использования CoinJoin транзакции](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.3-coinjoin.png)

На рисунке 7.4 изображены два графа транзакций: верхний и нижний. Для верхнего графа характерно то, что каждая транзакция имеет один или два выхода, а транзакции нижнего графа могут иметь три и больше выходов. Нижний граф более запутан и сложнее поддается анализу.

![Рисунок 7.4 – Графы транзакций с использованием CoinJoin и без такового](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.4-coinjoin-graph.png)

Чтобы применить метод CoinJoin, например, для отправки биткоинов, сначала нужно сформировать группу пользователей, которые одновременно хотят воспользоваться этим же методом. Далее они должны взаимодействовать, чтобы сформировать совместную транзакцию. На практике результирующие транзакции могут иметь десятки входов и выходов (иногда больше). Изображенный на плоскости граф таких транзакций получится очень запутанным (рис. 7.4). Монета, которая прошла цепочку таких транзакций, имеет тысячи возможных вариантов происхождения. Сложно определить среди всех вариантов настоящий.

Далее рассмотрим различные модификации метода CoinJoin, в которых создатели стремятся достичь большей приватности при помощи различных способов.

### Chaumian CoinJoin

Одна из модификаций CoinJoin называется *Chaumian CoinJoin*. Предложил ее тот же Gregory Maxwell. Здесь задействуется централизованный оператор и используется слепая подпись. Оператор нужен, чтобы выполнить перемешивание входов и выходов, после чего составить конечную транзакцию. При этом оператор не может украсть монеты или нарушить приватность перемешивания благодаря слепой подписи.

Пользователь предварительно ослепляет данные до их передачи оператору (при использовании механизма слепой подписи). Когда оператор подписывает эти данные, у него нет доступа к фактическому содержимому. Подписанные данные возвращаются пользователю, после чего он убирает ослепление и все выглядит, как обычная цифровая подпись.

Как проходит взаимодействие между пользователем и оператором при формировании общей транзакции? Каждый пользователь заранее подготавливает вход, где тратятся принадлежащие ему монеты, адрес для получения сдачи, а также ослепленный адрес для отправки платежа, после чего объединяет эти данные в одну последовательность и передает оператору (рис. 7.5).

![Рисунок 7.5 – Взаимодействие пользователей при использовании Chaumian CoinJoin](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.5-chaumian-coinjoin.png)

Оператор проверяет вход и сумму платежа, подписывает выходной адрес и возвращает подпись пользователю. При этом у оператора нет доступа к адресу, на который пользователь хочет отправить платеж, поскольку он ослеплен. Далее, пользователь убирает ослепление с выходного адреса, анонимно переподключается к оператору и передает ему подписанный выходной адрес. Оператор в свою очередь проверяет, что он действительно подписывал этот адрес своим ключом и соответствующий вход у него уже есть, но при этом он не может знать, какой вход соответствует какому выходу. После того, как все пользователи выполнили такие действия, они снова анонимно переподключаются к оператору и предоставляют подписи, которые подтверждают владение монетами на входе общей транзакции. Готовую транзакцию распространяют в сети для подтверждения; это делают либо сами пользователи, либо оператор.

В таком случае ни пользователи, ни сам оператор не могут деанонимизировать монеты на выходных адресах. Формирование транзакции при нормальных условиях занимает не более одной минуты. Отметим, что пользователи должны взаимодействовать через анонимные сети передачи данных, такие как Tor, I2P или Bitmessage.

Среди пользователей могут быть недобросовестные, цель которых – нарушить процесс создания общей транзакции любыми способами. Существует целый список возможных сценариев поведения пользователей, в том числе и мошеннический. Разработан целый ряд механизмов защиты для противостояния неблагоприятным сценариям, которые позволяют честным пользователям гарантированно сформировать конечную транзакцию. Механизмы защиты используют таймауты, отслеживание непотраченных выходов и т. д.

### CoinShuffle

Модификацию *CoinShuffle* предложили в 2014 году [107]. Здесь уже нет центрального оператора, и это стало преимуществом. Пользователи самостоятельно формируют совместную транзакцию, общаясь между собой. При этом приватность перемешивания выходных адресов и соответствующих монет обеспечивается даже для самих участников транзакции.

Еще одно преимущество этого метода состоит в том, что пользователям не обязательно использовать дополнительные сети для анонимизации трафика, так как все необходимые свойства будут достигнуты при использовании одинакового однорангового протокола взаимодействия пользователей.

В этом случае применяется асимметричное шифрование, где задействуется пара ключей (открытый и личный). Сообщение шифруется с помощью открытого ключа, а расшифровать его может только владелец личного ключа. Для коммуникации между участниками используется протокол DiceMix, также предусмотрено противостояние нарушителям. Давайте на схемах разберемся, как работает CoinShuffle в упрощенном варианте.

Представим небольшую группу пользователей: хитрая Алиса, мудрый Боб, бородатый Чарли и оранжевый Дэйв. У каждого из них есть одна непотраченная монета (один непотраченный выход) в учетной системе Bitcoin на адресах A, B, C и D, соответственно (рис. 7.6).

![Рисунок 7.6 – Группа пользователей, которая будет принимать участие в формировании транзакции](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.6-group-for-shared-tx.png)

Каждый хочет потратить монету и при этом скрыть историю ее происхождения. Для этого каждый участник группы узнает адрес, на который должна быть отправлена его монета, но не разглашает этот адрес остальным участникам.

Далее каждый генерирует новую пару ключей для асимметричного шифрования, после чего участники группы обмениваются открытыми ключами шифрования между собой, причем новый открытый ключ подписывается личным ключом, который соответствует адресу с непотраченной монетой. Таким же образом будут подписываться все сообщения участников при последующем взаимодействии. Это был первый этап.

Участники перемешиваются и образуют очередь. Алиса будет первой, потому что она хитрая, Боб второй, потому что он мудрый, и т. д. Теперь Алиса берет А’ и шифрует направленно на Дэйва, используя открытый ключ Дейва соответственно. Получившийся шифротекст Алиса снова шифрует, причем направленно на Чарли. Этот шифротекст снова шифруется, но уже направленно на Боба (рис. 7.7).

![Рисунок 7.7 – Процесс взаимодействия пользователей](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.7-users-interaction-coin-shuffle.png)

Результат шифрования Алиса передает Бобу. Боб расшифровывает своим личным ключом полученное сообщение. Затем берет B’ и шифрует направленно на Дейва, потом на Чарли и добавляет в список. Этот список он перемешивает случайным образом и передает Чарли. Чарли в свою очередь расшифровывает элементы списка своим личным ключом, добавляет C’, зашифрованный направлено на Дейва, в список и перемешивает все элементы списка случайным образом. Список передается Дэйву, который его дешифрует, получает открытые данные адресов для отправки монет, добавляет адрес D’, перемешивает случайным образом и на основании этих адресов, известных входов транзакции и сумм формирует общую транзакцию.

Дейв распространяет заготовку транзакции остальным участникам группы. Далее, каждый проверяет, есть ли в выходах транзакции нужный ему адрес и совпадает ли сумма. Если условие выполнено, то участник подписывает транзакцию, подтверждая владение монетами своего входа (рис. 7.8). Участники обмениваются подписями и если транзакция набирает все необходимые подписи, то может быть распространена по сети для подтверждения.

![Рисунок 7.8 – Формирование совместной транзакции](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.8-formation-of-shared-tx.png)

Если кто-то из участников начинает отклоняться от основного сценария взаимодействия, то остальные могут совместно проанализировать историю взаимодействия и вывести нарушителей из группы, чтобы повторить все без них. Это важная особенность.

Отметим, что готовые реализации CoinShuffle уже есть. На практике они эффективно работают даже на группах из нескольких десятков пользователей. В настоящий момент ожидается интеграция этого протокола в некоторые биткоин-кошельки, в том числе в мобильные.

### Недостатки метода CoinJoin

Очевидно, что для формирования транзакции необходимо более сложное off-chain взаимодействие, т. е. необходимо организовать формирование групп и взаимодействие пользователей между собой. Но более весомый недостаток состоит в том, что CoinJoin в чистом виде не скрывает суммы платежей. Как результат, он уязвим к *CoinJoin sudoku analysis*, который основан на сопоставлении сумм на выходах транзакций и позволяет распутать историю происхождения монет после многократного ее запутывания. Да, с этой проблемой можно бороться, например, использовать для выходных значений транзакций только определенные суммы (0,1 ВТС, 1 ВТС, 10 ВТС и т. п.), но это создает дополнительные сложности и ограничения.

Другой способ, который решает проблему открытости сумм платежей, – *confidential transactions*. Однако прежде чем перейти к нему, предлагаем рассмотреть основные принципы доказательств с нулевым разглашением.

### Концепция zero-knowledge proof

*Доказательство с нулевым разглашением (zero-knowledge proof, ZKP) – это метод, который позволяет одной стороне доказать знание секрета второй стороне без разглашения данных самого секрета*.

Для построения схем таких доказательств могут использоваться различные математические подходы. Однако схема может считаться zero-knowledge proof, только если она обеспечивает свойства ниже.

> **Properties of zero-knowledge proofs**
>> * *Completeness (полнота)*
>> * *Soundness (корректность)*
>> * *Zero knowledge (нулевое знание)*

*Completeness* предполагает, что если проверяющий и доказывающий честно следуют протоколу, то доказывающий гарантированно может убедить проверяющего в корректности утверждения (за исключением очень малой вероятности).

*Soundness* значит, что если утверждение неверное, то доказывающий не сможет убедить проверяющего в своей правоте (за исключением очень малой вероятности).

*Zero knowledge* предполагает, что во время доказательства проверяющий не может узнать ничего о утверждении кроме факта, что оно верное/неверное.

Существует два варианта ZKP: интерактивный и неинтерактивный. Интерактивный вариант предполагает, что доказывающий и проверяющий общаются непосредственно между собой. В ходе общения проверяющий все больше и больше убеждается в том, что утверждение доказывающего верно. Отметим, что при таком подходе никакую третью сторону нельзя убедить в том, что утверждение является верным, используя те же самые доказательства.

В случае неинтерактивного протокола доказывающий по предварительно определенной схеме формирует отдельный набор данных. Этот набор – фактическое доказательство утверждения, которое не имеет конкретного адресата. Имея такое доказательство, кто угодно может в любой момент времени убедиться в том, верно ли утверждение доказывающего.

Для хорошего понимания того, как работают ZKPs, предлагаем рассмотреть следующий пример с раскрашиванием узлов графа.

Допустим, Алиса захотела проверить своего друга Боба на сообразительность и предложила ему решить интересную задачу. Для этого она нарисовала несколько точек и соединила их линиями следующим образом (рис. 7.9).

![Рисунок 7.9 – Положение узлов графа](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.9-graph-nodes.png)

После этого Алиса дала Бобу три маркера разного цвета и предложила ему попытаться раскрасить узлы графа так, чтобы не существовало ни одного соединения между узлами одинакового  цвета.

Боб очень старался, потратил целых 4 минуты, пытаясь найти решение, после чего заявил, что эту задачу решить невозможно. Алиса ответила, что решение есть и она может доказать это Бобу. При этом Алиса придумала такой способ доказательства, при котором она не раскрывала, как именно должен быть раскрашен граф.

Итак, Алиса попросила Боба выйти из комнаты, после чего раскрасила граф так, как на рис. 7.10.

![Рисунок 7.10 – Состояние графа после раскраски](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.10-the-second-state-of-the-graph.png)

На каждую точку графа Алиса поставила колпак, который скрывал цвет узла, и попросила Боба войти обратно в комнату. Когда Боб вошел, он увидел следующее (рис. 7.11).

![Рисунок 7.11 – Сокрытие цветов узлов графа](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.11-the-hidden-graph.png)

Естественно, Боб поинтересовался, каким образом колпаки могут помочь Алисе с доказательствами. Тогда Алиса попросила Боба поднять два любых колпака, узлы под которыми соединены между собой. Когда Боб поднял колпаки, он увидел, что соединенные узлы имеют разный цвет (рис. 7.12). 

![Figure 7.12 — Bob verifying Alice’s statement](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.12-verification-of-the-graph.png)

Конечно, Боб сразу не захотел сдаваться и сказал, что вдруг Алисе повезло и другие соединенные между собой узлы могли быть одного цвета. Алиса снова выпроводила Боба из комнаты и перекрасила граф снова, но перемешав цвета. Боб заходит в комнату, поднимает два соседних колпака и, естественно, убеждается, что под ними – узлы разного цвета.

Так как Боб не особо упорный, то уже после 10-й попытки он все же согласился, что эта задача имеет решение и Алиса знает его. Самое интересное в этом примере (как и в концепции ZKP) то, что Алиса сумела доказать Бобу, что она знает решение задачи, не раскрыв Бобу, каким именно образом она раскрасила граф.

### Confidential Transactions

Особенность метода *confidential transactions (CTs)* [94] заключается в том, что он полностью скрывает фактические суммы на входах и выходах транзакции от третьих лиц. Каждый может проверить, что сумма всех выходов не превышает сумму всех входов, – необходимое условие для верификации и подтверждения транзакции.

Это стало возможным благодаря использованию вышеописанного подхода, а именно ZKP.

> * *Суммы платежей скрыты для всех, кроме отправителя и получателя* 
> * *Транзакция содержит данные, достаточные для ее верификации и подтверждения* 
> * *Используются доказательства с нулевым разглашением*

Для доказательства того, что сумма выходов не превышает сумму входов, используется *обязательство Педерсена (Pedersen commitment)*, которое базируется на преобразованиях в группе точек эллиптической кривой. Для борьбы с неконтролируемой эмиссией монет в этой схеме обязательно применяется доказательство использования допустимых сумм на выходе транзакции. Чтобы проверить, что были использованы неотрицательные суммы, которые не превышают порядок базовой точки, применяются т. н. *доказательства диапазона (range proofs)*.

И все было бы хорошо, но создание этих самых range proofs весьма затратно с точки зрения вычислительных ресурсов. К тому же, они имеют очень большой объем. Теоретически интегрировать confidential transactions в протокол биткоина можно, но никто особо не спешит это делать из-за их большого объема. Тем не менее, уже есть работающие учетные системы, где confidential transactions успешно применяются.

### Ring Confidential Transactions

Следующий метод называется *ring confidential transactions*. По сути, это гибрид CTs и механизма кольцевой подписи. Ring confidential transactions предполагают использование обязательств Педерсена для сокрытия сумм платежей и кольцевых подписей для запутывания истории происхождения монет. Это значит, что отправитель платежа во входе своей транзакции ссылается не на один конкретный выход (UTXO), а сразу на несколько. Далее с помощью кольцевой подписи он доказывает, что ему принадлежат монеты одного из нескольких выходов, но не разглашается, какого конкретно. Из этого следует, что невозможно однозначно отследить историю происхождения монет.

Применение кольцевых подписей таким образом (рис. 7.13) было впервые предложено в одном из стандартов *CryptoNote* [108], на базе которого работают несколько криптовалют. Ring confidential transactions позволяют создавать транзакции со множеством входов и выходов, где невозможно однозначно отследить происхождение каждого входа, суммы платежей скрыты, а взаимодействие с другими пользователями для создания транзакции не требуется.

![Рисунок 7.13 – Использование кольцевой подписи для ring confidential transactions](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.13-ring-signature.png)

### MimbleWimble

*MimbleWimble – это формат и протокол формирования транзакций и блоков цифровой валюты, который позволяет обеспечить высокий уровень приватности пользователей, опираясь только на криптографические примитивы.* В своем нынешнем виде MimbleWimble не совместимый с протоколом Bitcoin и может быть реализован только в виде sidechain или в отдельной учетной системе. Главные особенности MimbleWimble следующие:

>> * *Сравнительно высокий уровень приватности пользователей* 
>> * *Поддерживаются транзакции только с повышенным уровнем приватности* 
>> * *Эффективная агрегация данных о непотраченных монетах* 
>> * *Сравнительно низкий темп увеличения размера базы данных* 

Криптовалюта Grin – первый проект, который реализует протокол MimbleWimble. Первая тестовая сеть (testnet) этого проекта была запущена 16 ноября 2017 года, и ее целью было нахождение недостатков в самой концепции MimbleWimble. 26 марта 2018 года протокол был доработан и запущена testnet2, по окончании работы которой в проект снова было добавлено огромное количество технических обновлений, а также были исправлены критические уязвимости протокола. 7 июля 2018 года была запущена testnet3, а для завершения тестирования цифровой валюты 17 октября 2018 года была запущена testnet4.

15 января 2019 года в криптовалюте Grin начала работать основная сеть (mainnet). На рис. 7.14 приведены данные соответствующего genesis block. В качестве постоянной тестовой сети была параллельно запущена еще одна сеть проекта – floonet.

![Рисунок 7.14 – Genesis block проекта Grin](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.14-grin-genesis-block.png)

### Stealth Addresses

Этот метод подразумевает расчет скрытых адресов, на которые будут отправляться монеты. Впервые эту идею описал Питер Тодд (Peter Todd). Метод stealth addresses требует от отправителя платежа создания случайного одноразового адреса для каждой транзакции. В случае проведения нескольких транзакций на один stealth address, в цепочке блоков будет содержаться данные о нескольких платежах на совершенно разные адреса [110].

Подобный подход позволяет сделать невозможным связывание транзакций с конкретным идентификатором пользователя. В качестве идентификаторов пользователей используются открытые ключи: если вы хотите принимать платежи, то вам нужно огласить свой открытый ключ. При этом только владелец stealth address может с помощью своего личного ключа просмотреть все транзакции, адресованные ему. Отправитель использует свою пару ключей и открытый ключ получателя, чтобы рассчитать новый одноразовый открытый ключ, который уже будет указан в транзакции в качестве адреса.

> * Адрес, на который отправляются монеты, могут знать только отправитель и получатель
> * Сторонний наблюдатель не может ни подтвердить, ни опровергнуть связь между идентификатором пользователя и адресом назначения платежа

В качестве примера можно привести интернет-магазин, который использует stealth address для приема платежей. В этом случае для каждого входящего платежа будет формироваться новый одноразовый адрес, что сильно затрудняет отслеживание деятельности интернет-магазина (рис. 7.15).

![Рисунок 7.15 – Использование stealth address магазином для приема платежей](/resources/img/volume-1/7.2-privacy-in-digital-currencies/7.15-stealth-addresses.png)

### Концепция гомоморфного шифрования

В контексте асимметричной криптографии также нельзя не упомянуть о *гомоморфном шифровании (homomorphic encryption). Гомоморфная криптосистема – это система, которая позволяет проведение некоторых действий с открытым текстом путем проведения операций с шифротекстом*. То есть, используя такие системы, вы можете позволить другой стороне обрабатывать ваши важные (sensitive) данные и при этом не беспокоиться о их утечке. Например, вы можете зашифровать данные о своих доходах и расходах и отправить их бухгалтеру для подсчета текущего баланса, причем ему не будет нужно расшифровывать данные, ведь все операции он будет выполнять с шифротекстом. В результате, бухгалтер вернет вам зашифрованный документ, содержащий результат, доступ к которому можно будет получить используя тот же ключ, который использовался для шифрования изначального документа.

**Часто задаваемые вопросы**

*– Возможно ли использовать stealth addresses в Биткоине?*

Да, вы уже сейчас можете их использовать, обновления протокола для этого не требуется. Но для более широкой адаптации этой функциональности необходимо строго специфицировать порядок вычислений и форматы данных, чтобы все кошельки могли работать друг с другом и, соответственно, добавить эту функцию в сами кошельки. Для введения этой спецификации Peter Todd уже создал отдельный BIP, но он еще на стадии рассмотрения.

*– Эффективно ли использовать CoinJoin в чистом виде для биткоинов?*

Нет, в чистом виде это малоэффективно, потому что такие транзакции поддаются простому анализу по суммам платежей. Как вариант, можно использовать одинаковые суммы для всех участников схемы запутывания, при этом нужно избегать доверенных миксеров, которые могут либо украсть монеты, либо нарушить приватность.

*– Можно ли применять рассмотренные выше методы для обеспечения приватности в цифровых валютах, таких как Ethereum, Ripple и Stellar*

Нет, это не так. Ethereum, Ripple и Stellar используют совершенно другую модель транзакций и другой способ учета монет, к которым нельзя применить такие методы обеспечения приватности. Вы, конечно, можете попытаться искусственно интегрировать stealth addresses или confidential transactions, но это будет крайне малоэффективно с точки зрения производительности системы. Причина состоит в том, что в Биткоине учет осуществляется на основе непотраченных выходов, а перечисленные в вопросе валюты используют балансы и аккаунты.

[ЗАКЛЮЧЕНИЕ](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-1/ru/8-conclusion.md)   