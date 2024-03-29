# [4 МЕХАНИЗМЫ ОБЕСПЕЧЕНИЯ КОНФИДЕНЦИАЛЬНОСТИ В ИНТЕРНЕТЕ](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/ru/4-Privacy-and-anonymity-on-the-Internet.md)

# 5 РОЛЬ КРИПТОГРАФИЧЕСКИХ ОБЯЗАТЕЛЬСТВ В УЧЕТНЫХ СИСТЕМАХ

Низкий уровень приватности представляет серьезную угрозу безопасности учетной системы. Например, ваши конкуренты могут узнать обо всех проведенных вами сделках, изучить детали вашего бизнеса и т. д. В Bitcoin, например, вопрос обеспечения анонимности частично решается путем присвоения адресов. Если третья сторона не знает, что именно вам принадлежит тот или иной адрес, ее возможность влиять на вас заметно снижается.

Однако сеть подразумевает взаимодействие сторон. В результате этого взаимодействия стороны узнают об адресах/аккаунтах друг друга. Значит, третья сторона, с которой вы взаимодействуете, может просмотреть информацию о ваших предыдущих действиях и получить необходимую ей информацию, так как детали всех транзакций по умолчанию являются открытыми.

Средства, за счет которых можно повысить уровень приватности, зачастую очень дорого обходятся системе (снижая производительность и ее пропускную способность). Однако для некоторых платформ приватность – необходимое требование, экономить на котором не стоит. В этом разделе мы рассмотрим принципы построения confidential transaction (CT) как одного из методов обеспечения конфиденциальности (с возможностью публичного аудита) в открытых системах (рис. 5.1).

![Рисунок 5.1 – Раскрытие деталей транзакции сторонним наблюдателем и использование CT для скрытия этих деталей](/resources/img/volume-3/5.0/F-5.1-disclosure-of-transaction-details-and-using-CT.png "Рисунок 5.1 – Раскрытие деталей транзакции сторонним наблюдателем и использование CT для скрытия этих деталей")


## 5.1 Методы построения криптографических обязательств

В этом разделе мы рассмотрим функционирование фундаментальных основ доказательств с нулевым разглашением – _криптографических обязательств_.

Криптографические обязательства обычно используются тогда, когда вы хотите зафиксировать некоторое значение и через определенное время при его разглашении доказать, что именно оно было зафиксировано. Например, вы хотите сделать ставку на одну из лошадей, которая, как вы считаете, прибежит к финишу первой. При этом вы не хотите, чтобы до момента окончания забега кто-то узнал, на какую лошадь вы поставили, и хотите иметь возможность доказать (по итогу забега), что вы поставили на конкретную лошадь.

Криптографические обязательства могут работать только благодаря существованию однонаправленных функций: у доказывающей стороны должна быть возможность предоставить выходное значение однонаправленной функции, которое само по себе не раскрывает входное сообщение (проверяющая сторона будет использовать такую же функцию для проверки знания) [91].

С этой ролью, например, отлично справляются хэш-функции. Пользователь может сделать ставку на лошадь под номером 2, предоставив хэш-значение этого числа. После того как лошадь побеждает в забеге, пользователь разглашает секрет, а проверяющий убеждается, что это значение соответствует ранее предоставленному обязательству (рис. 5.2).

![Рисунок 5.2 – Использование обязательства для номера лошади](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.2-using-commitment-to-bet.png "Рисунок 5.2 – Использование обязательства для номера лошади")


### Добавление случайности как компонента обязательств

Описанный выше подход не очень безопасен, так как каждый раз, предоставляя обязательство знания 2, пользователь будет предоставлять одно и то же хэш-значение. Соответственно, при помощи этого проверяющий сможет раскрыть знание пользователя.

> _Замечание. Если набор возможных вариантов знания небольшой (например, в забеге участвует только 10 лошадей), то, получив хэш-значение от определенного числа, проверяющий может быстро перебрать все варианты и раскрыть значение секрета. Однако даже если учесть, что количество вариантов большое (например, 2<sup>256</sup>), то обязательства для одних и тех же знаний не будут отличаться друг от друга. Если мы предоставляем хэш-значение от 2, проверяющий может сохранить это соответствие (обязательство–знание). Если через некоторое время мы предоставляем обязательство для этого же знания, проверяющий может быстро раскрыть секрет._

Какой есть способ решить это? Все очень просто: представьте, что доказывающий отправляет проверяющему не только значение 2, а еще несколько случайных байтов, дополненных к нему (рис. 5.3). Так как хэш-функция устойчива к нахождению прообраза и коллизии, то мы можем использовать это свойство для того, чтобы проверяющий не раскрыл знание, имея только обязательства, и доказывающий не смог это значение изменить [91].

![Рисунок 5.3 – Добавление случайности при формировании обязательств](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.3-adding-randomness-to-commitments.png "Рисунок 5.3 – Добавление случайности при формировании обязательств")

Хотя хэш-функции и могут обеспечить эти свойства, они не имеют ничего общего с гомоморфизмом, который является очень важным свойством в доказательствах с нулевым разглашением. 

> _Замечание. Напомним, что гомоморфизм заключается в возможности проводить некоторые математические операции с обязательствами и при этом обеспечивать коммутативность и ассоциативность этих операций. Если же использовать в качестве обязательств хэш-значения, то_ $\mathrm{hash} (a)+ \mathrm{hash} (b) \neq \mathrm{hash} (a+b)$_; это значит, что мы не можем проводить операции над обязательствами для получения корректного результата_ [92]_._


### Обязательства Педерсена

В качестве обязательств мы можем использовать обязательства Педерсена [93]. В них роль однонаправленной функции выполняет операция умножения точки эллиптической кривой на скаляр. Обязательства Педерсена можно представить в виде следующего уравнения: $C=rH+aG$, где _a_ – секрет (знание), _r_ – случайное значение, а _G_ и _H_ – генераторы группы точек (точки на кривой). Здесь _r_ используется так же, как и в хэш-функции в примере выше. Кроме того, у доказывающего не должно быть возможности установить зависимость _H_ от _G_ (далее детально рассмотрим, почему так).

Обязательства Педерсена могут обеспечить _необратимость_ (невозможность получить секрет, имея только обязательство), _доказуемость_ (имея обязательство, проверяющий может с высоким уровнем достоверности проверить правильность знания) и _гомоморфизм_. Необратимость и доказуемость обеспечивается за счет сложности задачи дискретного логарифмирования. Гомоморфизм заключается в следующем (рис. 5.4): можно сформировать _одно обязательство_ (одно значение точки), чтобы доказать выполнение _набора обязательств_ (при условии, что избыточные значения также предоставлены корректно).

![Рисунок 5.4 – Суть гомоморфизма в обязательствах Педерсена](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.4-homomorphism-in-Pedersen-commitments.png "Рисунок 5.4 – Суть гомоморфизма в обязательствах Педерсена")

Теперь доказывающий может предоставить проверяющему одно обязательство, после чего – набор знаний, который удовлетворяет ему (рис. 5.5). Например, доказывающий хочет одновременно поставить на 10 различных скачек, предоставив одно значение.

![Рисунок 5.5 – Предоставление единого обязательства для большого набора знаний](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.5-single-commitment-to-set-of-knowledge-values.png "Рисунок 5.5 – Предоставление единого обязательства для большого набора знаний")

> _Замечание. В этом подразделе мы рассматриваем обязательства Педерсена, которые используют эллиптическую криптографию. Однако еще существует «классическая» реализация этой схемы обязательств. Согласно протоколу параметрами выступают h и g (уже не точки эллиптической кривой, а большие простые числа). Доказывающий владеет секретом a и генерирует случайное значение r. Доказательство представляет собой значение_ $C=g^{a}h^{r}$_. Проверяющий, получая значения a и r, таким же образом может проверить, что обязательство удовлетворено._


### Подмена знаний и подходы «nothing up my sleeve»

Как мы отмечали ранее, важна случайность значения точки _H_. Если доказывающий может установить зависимость между точками _H_ и _G_, то он может формировать поддельные доказательства, которые удовлетворяют обязательству.

Это свойство заключается в том, что обязательство представляет собой точку на кривой, которая была получена в результате сложения двух точек – $rH$ и $aG$. _H_ – точка, производная от _G_: $H=xG$, где _х_ – неизвестное значение. В итоге, обязательство выглядит так: $C=rH+aG=rxG+aG=(rx+a)G$.

Доказывающему известны значения _r_ и _a_. Если бы ему было известно и значение _х_, то он мог бы изменять _а_, _r_ и _х_ так, чтобы они удовлетворяли обязательству. Однако поскольку доказывающий не знает _х_, он не может подобрать такое $x' \neq x$ при помощи которого можно было бы сформировать поддельное доказательство.

Промоделируем такую атаку на нашем примере со ставками. Итак, точка _H_ не случайна: доказывающий знает значение секрета, которое при умножении на _G_ дает значение _H_ (например $H=3G$). Доказывающий хочет поставить на лошадь под номером 2 (значение _a_). Он генерирует случайное значение $r=2$ и передает проверяющему обязательство (рис. 5.6-А).

![Рисунок 5.6-А – Предоставление обязательства проверяющему](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.6-A-providing-of-commitment-to-verifier.png "Рисунок 5.6-А – Предоставление обязательства проверяющему")

Внезапно в скачках выигрывает лошадь под номером 5. Если бы доказывающий не знал _х_, то он точно потерял бы деньги. Но так как он знает _x_, он отправляет проверяющему поддельные значения $a=5$ и $r=1$. Как видим на рис. 5.6-Б, проверяющий принимает доказательства, хотя изначально доказывающий предоставлял обязательства для абсолютно других значений. Таким образом, важно, чтобы значение _H_ задавалось случайно.

![Рисунок 5.6-Б – Успешное использование поддельных доказательств](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.6-B-successful-use-of-fake-commitments.png "Рисунок 5.6-Б – Успешное использование поддельных доказательств")

Для доказательства того, что _H_ не было задано специальным образом, существует набор методов «nothing up my sleeve» [94; 95]. Этот набор довольно широкий, но основные принципы можно объяснить на следующем примере. 

Представьте, что вы просто берете хэш-значение любых данных и проверяете, является ли это значение точкой на эллиптической кривой. Если да, то вы можете задать его как _H_. При этом, хотя вы и знаете изначальный набор данных, вы не можете найти _x_. Это очень похоже на создание адреса, с которого нельзя потратить монеты (non-spendable address).

На практике для получения точки _H_ чаще всего берут хэш-значение от точки _G_ и конкатенируют со счетчиком _i_ до момента, пока не будет найдена точка на кривой (точнее, действительная координата _x_): $x_{H}= \mathrm{hash} (G \Vert i)$.

> _Замечание. Для проверки факта, действительно ли координата х выбрана правильно, необходимо вычислить соответствующее целое значение у из уравнения эллиптической кривой:_ $y^{2}=ax^{3}+bx+c$_. Полученные координаты и будут координатами точки H._


### Одно обязательство для вектора значений

Одним из интересных свойств обязательств Педерсена также является тот факт, что размер обязательств не зависит от того, скрываем ли мы один секрет либо вектор значений. В любом случае обязательство является единым значением точки на кривой (рис. 5.7-А) [91].

![Рисунок 5.7-А – Обязательство является единым значением точки, вне зависимости от объема скрываемых знаний](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.7-A-commitment-is-single-point-value.png "Рисунок 5.7-А – Обязательство является единым значением точки, вне зависимости от объема скрываемых знаний")

В данном случае эти обязательства могут быть проверены, если доказывающая сторона предоставит _r_ и все значения вектора _a̅_. Но повторим, что при этом, вне зависимости от количества элементов в векторе, обязательство будет одной точкой.

Более того, мы можем создать такие же обязательства для большого количества различных векторов, при этом сохранив размер этих обязательств (рис. 5.7-Б).

![Рисунок 5.7-Б – Приблизительный вид обязательства для различных векторов значений](/resources/img/volume-3/5.1-Methods-of-building-cryptographic-commitments/F-5.7-B-commitment-for-different-value-vectors.png "Рисунок 5.7-Б – Приблизительный вид обязательства для различных векторов значений")

> **_Свойства обязательств Педерсена_**  
> * _Perfect hiding_
> * _Computational binding_
> * _Equivocality_

_Perfect hiding_ значит, что третья сторона не может получить секрет, имея только соответствующее ему обязательство. Даже если она найдет значения, которые удовлетворяют обязательству, она не сможет быть уверена в том, что это будет именно тот секрет, которым обладает доказывающий.

_Computational binding_ подразумевает, что злоумышленник не может подобрать другие данные, удовлетворяющие конкретным обязательствам, не решив задачу дискретного логарифмирования в группе точек эллиптической кривой (при условии, что он не имеет дополнительных данных об используемых параметрах).

_Equivocality_ является некоторым ограничением этой схемы обязательств и вытекает из предыдущих свойств. Оно подразумевает, что определенным обязательствам могут удовлетворять разные наборы данных и доказывающий может убедить проверяющего, что знает секрет, без знания такового, если генерация параметров не была случайной [91].


### Схема обязательств Эль-Гамаля

Схема обязательств Эль-Гамаля [96] также довольно часто используется, но при этом обладает немного другими свойствами, в отличие от обязательств Педерсена. Рассмотрим, как она работает.

У нас есть два случайных генератора группы: _g_ и _h_. Доказывающий хочет предоставить обязательства к секрету _a_. Изначально он также генерирует случайное значение _r_, при помощи которого добавляет случайности обязательству. Сами обязательства представляют собой пару значений и вычисляются следующим образом: $C=(g^{r},ah^{r})$.

Для доказательства знания пользователю необходимо предоставить значения _a_ и _r_. Проверяющий таким же образом вычисляет обязательство. Если вычисленное обязательство равно полученному ранее, знание секрета доказано.

> **_Свойства обязательств Эль-Гамаля_**  
> * _Perfect binding_
> * _Computational hiding_
> * _Extractability_

_Perfect binding_ указывает на то, что обязательства строго привязаны к исходному секрету, то есть атакующий не сможет подобрать другой секрет, который удовлетворяет доказательству.

_Computational hiding_ значит, что злоумышленник не сможет различить два обязательства для двух различных сообщений (при условии, что он не сможет решить задачу факторизации).

_Extractability_ предполагает, что если злоумышленник решит задачу факторизации, то он сможет раскрыть секрет для любого обязательства. 


## 5.2 Протокол идентификации Шнорра как схема интерактивного доказательства с нулевым разглашением

Чтобы разобраться, как работают интерактивные доказательства с нулевым разглашением и как в них используются схемы обязательств (рассмотренные в предыдущем подразделе), рассмотрим одну из интерактивных схем доказательств – схему идентификации Шнорра. Схема идентификации очень похожа на схему подписи Шнорра, за исключением того, что она проходит интерактивно (т. е. схема подписи не требует коммуникации между подписантом и проверяющим в ходе проверки подписи) и сообщения не будут использоваться.


### Схема идентификации Шнорра

Согласно этому протоколу доказывающий владеет некоторым секретом _x_ и соответствующим обязательством $P=xG$. Здесь связь между обязательством и секретом точно такая же, как между открытым и личным ключом в схеме одноименной подписи. Доказывающий хочет предоставить доказательство знания _x_, при этом не раскрывая его значения [97].

Алгоритм состоит из четырех этапов. На первом этапе доказывающий генерирует новый секрет _r_ и формирует соответствующее ему обязательство $R=rG$. Открытое значение передается проверяющему. В ответ на это проверяющий генерирует случайный скаляр _e_ и возвращает его доказывающему (второй этап протокола). На третьем этапе при помощи секрета _x_, а также значений _r_ и _e_ доказывающий формирует доказательство знания секрета и отправляет его проверяющему. На последнем этапе проверяющий верифицирует полученное доказательство и убеждается в том, что доказывающий знает секрет (если проверка завершена успешно). Описание протокола представлено на рис. 5.8.

![Рисунок 5.8 – Схема идентификации Шнорра](/resources/img/volume-3/5.2-Schnorr-identification-protocol-as-an-interactive-zero-knowledge-proof-scheme/F-5.8-Schnorr-identification-scheme.png "Рисунок 5.8 – Схема идентификации Шнорра")

1. Доказывающий генерирует случайное значение _r_ и вычисляет $R=rG$. После этого он передает _R_ проверяющему. Проверяющий не может вычислить _r_ (решить задачу дискретного логарифмирования в группе точек эллиптической кривой).
2. Проверяющий генерирует открытый параметр _e_ и передает его доказывающему.
3. Доказывающий вычисляет $s=r+ex$ и передает его проверяющему. Имея только значения _s_ и _e_, проверяющий не может получить секрет _x_. 
4. Проверяющий верифицирует доказательство знания _х_ согласно выражениям на рис. 5.9.

![Рисунок 5.9 – Проверка доказательства проверяющим](/resources/img/volume-3/5.2-Schnorr-identification-protocol-as-an-interactive-zero-knowledge-proof-scheme/F-5.9-proof-verification.png "Рисунок 5.9 – Проверка доказательства проверяющим")

Как видим, этот протокол удовлетворяет требованиям протоколов доказательства с нулевым разглашением:

* Доказывающий гарантировано убеждает проверяющего, что он владеет необходимым секретом (_x_).
* Если доказывающий не владеет секретом, он не может убедить проверяющего в обратном.
* В ходе доказательства знания секрета не раскрывается сам секрет.


### Подделка доказательства доказывающим

Если доказывающий знает значение _e_ (скаляр, который сгенерировал проверяющий) до начала взаимодействия с проверяющим, он может подделывать доказательства, не зная значения секрета. Давайте разберем, как это работает (рис. 5.10).

![Рисунок 5.10 – Процесс подделки доказательства](/resources/img/volume-3/5.2-Schnorr-identification-protocol-as-an-interactive-zero-knowledge-proof-scheme/F-5.10-proof-forging.png "Рисунок 5.10 – Процесс подделки доказательства")

1. Доказывающий генерирует случайное значение _s_ и вычисляет $S=sG$. После этого он вычисляет $R=sG-eP$ (он может это сделать, так как знает значение _e_)
2. Доказывающий передает проверяющему _R_.
3. Проверяющий возвращает значение _e_. После этого доказывающий отправляет проверяющему заранее сгенерированное _s_.
4. Проверяющий делает такую же проверку, как в предыдущем примере, и убеждается, что доказывающий знает секрет (хотя на самом деле это не так).

Однако особенность этой схемы состоит не в том, что доказывающий может обмануть проверяющего, заранее узнав значение _e_ (если проверяющий знает правильный порядок взаимодействия, то он не даст себя обмануть таким образом), а то, что этот протокол интерактивный. То есть третья сторона не может убедиться в том, что итерации протокола были выполнены корректно, и в том, что проверяющий заранее не раскрыл доказывающему значение _e_. Такой протокол может использоваться только между двумя сторонами (для проверки, что доказывающий владеет секретом в конкретный момент времени), и доказывающий и проверяющий не могут убедить любую третью сторону в корректности проведенных действий.


### Преобразование интерактивного протокола в неинтерактивный

В протоколе идентификации, описанном выше, случайное значение _e_, на котором основывается безопасность данной схемы, предоставляет проверяющий. Следовательно, от проверяющего непосредственно зависит, сможет ли доказывающий эти доказательства подделать [91].

Мы можем изменить источник случайности, используя для этого, например, хэш-функцию. В неинтерактивном протоколе _e_ уже не генерирует проверяющий: оно вычисляется как $e= \mathrm{hash} (R)$.

Что это дает? Ранее доказывающий вычислял значение _R_ на основе сгенерированного _s_ и мог сделать правильные вычисления, только если предварительно получал доступ к _e_. Теперь же _e_ зависит от _R_ и не может быть вычислено раньше, чем _R_. Это, в свою очередь, предотвращает потенциальную подделку доказательств знания.


## 5.3 Использование обязательств Педерсена для доказательств с нулевым разглашением

В предыдущих разделах мы по отдельности рассмотрели, что такое обязательства Педерсена и как работает протокол идентификации Шнорра. Теперь рассмотрим вариант, когда проверяющий интерактивно взаимодействует с доказывающим в режиме реального времени (как в случае интерактивного протокола Шнорра), но при этом в качестве обязательств используются обязательства Педерсена.

Доказательство знания будет предоставляться после того, как доказывающий сгенерирует набор обязательств для каждого из _m_ следующих векторов: $C_{i}=r_{i}H+ \overline{x_{i}G}$, $i \in \{ 1,2, \ldots ,m \}$. После того как доказывающий передаст проверяющему набор обязательств $(C_{1},C_{2}, \ldots ,C_{m})$, он сможет за один шаг доказать, что знает изначальные векторы-секреты.

Сначала доказывающий формирует новое обязательство _C<sub>0</sub>_ для случайного значения _C<sub>0</sub>_ и отправляет его проверяющему. После этого проверяющий генерирует случайное значение _e_ (скаляр) и возвращает его доказывающему. Доказывающий формирует пару значений $(z,s)$ и отправляет ее проверяющему. Эти значения вычисляются так, как показано на рис. 5.11-А.

![Рисунок 5.11-А – Интерактивный протокол доказательства](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.11-A-interactive-proof-protocol.png "Рисунок 5.11-А – Интерактивный протокол доказательства")

> _Замечание. Шаги итерации в этом протоколе схожи с шагами протокола идентификации Шнорра, а значит можно предположить, что и здесь возможна атака подмены обязательства (далее мы рассмотрим детальнее, как это работает)._

Векторы _x<sub>i</sub>_ скрыты от проверяющего, так как сложение элементов не раскрывает каждый из них по отдельности: $z_{i}=x_{0,i}+ex_{1,i}+e^{2}x_{2,i}+ \ldots +e^{m}x_{m,i}$. Полученные обязательства проверяющий верифицирует согласно выражению на рис. 5.11-Б. Если равенство выполнено, то проверяющий убеждается, что доказывающий знает весь набор секретов.

![Рисунок 5.11-Б – Проверка доказательств](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.11-B-verification-of-proofs.png "Рисунок 5.11-Б – Проверка доказательств")

Приведем пример, который показывает, почему такие доказательства работают. Для него сделаем одно упрощение: каждый вектор-секрет будет просто одним значением. То есть мы будем работать с обязательствами Педерсена, относящимися к сокрытию одного секрета. Предположим, Алиса отправляет Бобу 3 обязательства _C<sub>1</sub>_, _C<sub>2</sub>_, _C<sub>3</sub>_, которые соответствуют значениям _a<sub>1</sub>_, _a<sub>2</sub>_, _a<sub>3</sub>_, которые она знает (рис. 5.12-А).

![Рисунок 5.12-А – Отправка Алисой набора обязательств](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.12-A-Alice-sends-set-of-commitments.png "Рисунок 5.12-А – Отправка Алисой набора обязательств")

После этого она хочет доказать знание этих значений. Для этого она отправляет Бобу новое обязательство  _C<sub>0</sub>_ для случайного значения  _a<sub>0</sub>_. Боб возвращает Алисе скаляр _е_. Далее Алиса вычисляет и отправляет Бобу пару значений $(z,s)$ (рис. 5.12-Б).

![Рисунок 5.12-Б – Процесс доказательства владения знаниями](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.12-B-knowledge-proof.png "Рисунок 5.12-Б – Процесс доказательства владения знаниями")

В свою очередь, Боб может проверить знание, имея только значения _z_ и _s_, согласно расчету на рис. 5.12-В.

![Рисунок 5.12-В – Проверка правильности доказательств](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.12-C-proof-verification.png "Рисунок 5.12-В – Проверка правильности доказательств")

Итак, мы оказываемся в ситуации, напоминающей использование протокола идентификации Шнорра. Единственное отличие состоит в том, что на последнем шаге итерации в протоколе Шнорра передается лишь значение скаляра _s_, а в рассматриваемом протоколе – пара значений $(z,s)$. Для рассмотренного примера _z_ также был единым скаляром, но он может быть вектором значений.

Поэтому если доказывающему заранее известно значение _e_, он может подделывать доказательства. Для этого он генерирует пару случайных значений $(z,s)$, после чего вычисляет значение обязательства _C<sub>0</sub>_  следующим образом: $C_{0} = (sH+aG)- \displaystyle\sum_{i=1}^{m}{e^{i}C_{i}}$.

Рассмотрим, как это будет работать для нашего примера. Алиса отправляет Бобу 3 обязательства _C<sub>1</sub>_, _C<sub>2</sub>_, _C<sub>3</sub>_, соответствующие значениям _a<sub>1</sub>_, _a<sub>2</sub>_, _a<sub>3</sub>_. После этого Алиса случайно генерирует $(z,s)$. Имея значение _е_, Алиса формирует _C<sub>0</sub>_ и отправляет его проверяющему (рис. 5.13-А).

![Рисунок 5.13-А – Процесс подделки доказательств](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.13-A-proof-forging.png "Рисунок 5.13-А – Процесс подделки доказательств")

В свою очередь, Боб для проверки знания проводит вычисление, как на рис. 5.13-Б. Как видим, Алиса, не владея секретом, но заранее имея значение _е_, может подделывать доказательства для обязательств.

![Рисунок 5.13-Б – Успешная проверка поддельного доказательства](/resources/img/volume-3/5.3-Using-Pedersen-commitments-for-zero-knowledge-proofs/F-5.13-B-successful-verification-of-fake-proof.png "Рисунок 5.13-Б – Успешная проверка поддельного доказательства")


### Использование обязательств Педерсена в confidential transactions

Для того, чтобы определить, как обязательства Педерсена могут использоваться в confidential transactions (CT), сначала нужно определить, какие детали транзакций необходимо скрывать. Предположим, что нам нужно скрыть суммы на входе (_a_) и выходе (_b_) транзакции, а также сумму комиссии (_c_). Чтобы транзакция считалась правильной и ее могли подтвердить валидаторы, должно выполняться следующее условие: $a-b-c=0$. Для скрытия каждого значения используем обязательства Педерсена: $A=r_{A}H+aG$, $B=r_{B}H+bG$, $C=r_{C}H+cG$. Так как обязательства являются гомоморфными, над ними можно провести следующее преобразование: $A-B-C=0G+(r_{A}-r_{B}-r_{C})H$.

Если доказывающий предоставит доказательство того, что он знает итоговое значение $(r_{A}-r_{B}-r_{C})$ (может использоваться либо схема идентификации Шнорра, либо одноименная подпись), то проверяющий может убедиться, что входы и выходы транзакции корректные.

В то же время возникает необходимость доказательства того, что значения _b_ (сумма выхода) и _с_ (комиссия) неотрицательные. Для этого используются доказательства диапазона (range proofs), которые могут основываться на обязательствах Педерсена.


### Использование обязательств Педерсена для range proofs

Доказательства диапазона используются для проверки того, что конкретное число пребывает в заранее определенных границах. В нашем случае нужно доказать, что значения _b_ и _с_ неотрицательные, а также что сумма этих значений не превышает максимально допустимое _n_ (значение модуля). При этом очень важно не раскрыть сами значения [98].

Мы можем представить _b_ и _с_ в виде битовой последовательности. Если длина модуля равна $k+2$ бит, то максимальная длина суммы b и с не должна превышать $k+1$ бит, а размер каждого из значений – _k_ бит. Соответственно, задача состоит в доказательстве того, что _b_ и _c_ – _k_-битные последовательности. 

В качестве примера сформируем такое доказательство для _b_. Его можно представить в виде следующего полинома: $b=b_{0}+2b_{1}+2^{2}b_{2}+ \ldots +2^{k-1}b_{k-1}$. У нас уже есть обязательство для _b_, которое представлено как $B=r_{B}H+bG$. Так же мы можем сформировать аналогичные обязательства для каждого слагаемого элемента полинома _b_. Для этого мы генерируем новый набор значений $(r_{B,0},r_{B,1}, \ldots ,r_{B,k-2})$ и определяем $r_{B,k-1}=r_{B}- \displaystyle\sum_{i=0}^{k-2}{r_{B,i}}$. Далее, используя эти значения, мы формируем обязательства для каждого элемента _b<sub>i</sub>_ полинома _b_: $P_{i}=r_{B,i}H+2^{i}b_{i}G$. Так как обязательства гомоморфные, выполняется следующее равенство: $P= \displaystyle\sum_{i=0}^{k-1}{P_{i}}$.

Таким образом мы можем доказать, что размер _b_ не превышает _k_ бит. Однако мы еще не доказали, что каждое _b<sub>i</sub> – действительно конкретный бит (0 либо 1). Для доказательства этого может использоваться механизм кольцевых подписей [98].

# [6 ОСНОВЫ ПОСТКВАНТОВОЙ КРИПТОГРАФИИ](https://github.com/distributed-lab/blockchain-and-decentralized-systems-book/blob/main/chapters/volume-3/ru/6-Basics-of-post-quantum-cryptography.md)
