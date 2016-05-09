# Два столпа JavaScript
## Часть 2: функциональное программирование

[Два столпа JavaScript часть 1: наследование через прототипы][pillars-part-1]. Из вступления:

> JavaScript является одним из наиболее важных ЯП всех времен не просто из-за своей популярности, а потому что он популяризует **две черезвычайно важные для развития всей науки программирования парадигмы**:
* **Наследование через прототипы** (объекты не содержащие классов, делегирование прототипов более известное как OLOO — Objects Linking to Other Objects)
* **Функциональное программирование** (с помощью лямбда-выражений и замыканий)

> Вместе я называю эти парадигмы **двумя столпами JavaScript** и, к моему стыду, они меня полностью совратили. Теперь мне не хочется программировать на языках в которых нет их реализации.

### Сегодня мы создаем будущее

> «Я перемещаюсь туда где шайба должна быть, а не туда где она уже была» ~ Уэйн Грецки

Современный высокотехнологичный мир построен на культуре непрерывных инноваций. Мы создаем крутые штуки вместе, потому что кооперация необходима для создания новых технологий. Каждый новый прорыв позволяет нам делать то о чем мы даже не мечтали пару лет назад. Добро пожаловать в мир технологий.

Не только сами технологии меняются быстро, скорость изменения технологий тоже [постоянно увеличивается][singularity] Процесс создания приложений пять лет назад сильно отличается от того, чем мы занимаемся сегодня. Скорее всего, вы используете инструменты которых не существовало 2 года назад. Вернитесь на 15-20 лет, и убедитесь что даже подходы к разработке радикально изменились ([гибкая][agile] модель заменила [каскадную][waterfall]).

Тратили ли вы последние пару лет на изучение Angular? Если ответ «да» — я вам сочуствую. Большинство из этих знаний неприменимо к Angular 2.0 или React. И нам не угнаться за скоростью изменений, если мы будем продолжать делать то что привыкли.

Если мы собираемся выжить в качестве инженеров то, в конечном счете, мы должны адаптироваться, и **адаптировать свой код**.

И это надо сделать быстро, потому что технологии которые мы будем использовать в следующем году, вероятно, будут в два раза сложнее текущих. Учитывая что эволюция IoT и AI и не думает останавливаться, нагрузка на наши приложения продолжит расти в геометрической прогрессии. Программам завтрашнего дня придется быть более масштабируемыми, гибкими, конкурентноспособными, производительными и умными чем сейчас.

Единственная возможность выдержать все возрастающую сложность заключается в том чтобы уменьшить сложность понимания программ. Для поддержки гигантских приложений завтра, мы должны научиться создавать более выразительный код сегодня. Короче, нам надо научиться писать программы о которых будет проще рассуждать, легче дебажить, создавать, анализировать и понимать.

> Процедурное программирование и ООП сами по себе не приведут нас к задуманным целям.

В течении следующих нескольких лет **процесс разработки ПО кардинально изменится**, и будет сильно отличаться от того чем мы занимались последние 30 лет. За этими изменениями последуют фундаментальные прорывы в технологиях создания ПО, масштабируемости процессов и приложений, контроле качества.

> В ближайшее время разработчики, разбирающиеся в функциональном программировании, будут сильно востребованы.

Этот процесс уже коснулся таких организаций как Netflix, Facebook и Microsoft, и это случилось не просто так.

В дзен-буддизме для понимания сути учения предлагают читать [коаны][koan]. В каждый коан заложено логическое противоречие, попытки его понять приводят к осмыслению всех заложенных идей, что, в конце концов, ведет к повышению мастерства и просветлению. Современную версию такого коана я привел в своей книге «[Programming JavaScript Applications][book-orelly]":

> Почтенный мастер Ку Чи шел со своим учеником, Антоном. Надеясь вступить с мастером в дискуссию, Антон сказал: «Учитель, я слышал, что объекты это очень хорошая вещь — это правда?» Ку Чи с сожалением посмотрел на своего ученика и ответил: «Мой глупый ученик, объекты — это замыкания для бедных. [ссылка]"
Антон поклонился и вернулся в свою келью изучать замыкания. Он внимательно прочитал всю серию работ «Lambda: The Ultimate…"[ссылка], потом все остальные статьи по этой теме, и создал небольшой интерпретатор с использованием объектов эмулирующих функционал замыканий. Он многому научился, и стал искать встречи с мастером чтобы похвастатья.
На своей следующей прогулке с Ку Чи, Антон сказал: «Учитель, я усердно изучал этот вопрос, и теперь понимаю, что объекты действительно на самом деле являются замыканиями для бедных.» Ку Чи ударил Антона своей палкой и сказал: «Когда же ты научишься?  Замыкания — это объекты для бедных". В этот самый момент Антон достиг просветления.
>
> ~ [Anton Van Straaten][anton-van-staaten]

Как и объекты, замыкания предлагают механизм сохранения состояния. В JavaScript замыкание создается всякий раз, когда функция получает доступ к переменной, определенной вне непосредственной области видимости функции. Замыкание создать легко: просто объявите функцию внутри другой функции, и верните ее как результат первой функции. Переменные, используемые внутренней функцией, будут доступны даже после того как внешняя функция закончит работу.

К примеру, вы можете использовать замыкания для создания приватных данных используя следующую функцию-фабрику:
```javascript
var counter = function counter() {
  var count = 0;
  return {
    getCount: function getCount() {
      return count;
    },
    increment: function increment() {
      count += 1;
    }
  };
};
```

В первой части «[Столпов][pillars-part-1]» я не только описал некоторые из основных недостатков ООП, но также заметил проблеск надежды на светлое будущее: **наследование через прототипы** — *построение новых экземпляров объектов из более мелких объектов, вместо классов-родителей*.

Во второй части мы будем обсуждать совершенно иную парадигму программирования, играющую гораздо большую роль в современных языках, добавляющую функционал который уменьшает лишнюю работу, синтаксический шум и копипасту: **функциональное программирование** *(далее по тексту — «ФП")*.

> «… шаг вперед это иногда два шага назад.» ~ какой-то мудрый мужик

У самых истоков компьютерной революции, за десятилетия до изобретения микропроцессоров, человек по имени **Алонзо Чёрч** опубликовал новаторскую работу в области теоретической информатики. Возможно, вы слышали о его ученике и соратнике, **Алане Тьюринге**. Вместе они создали теорию под названием [Тезич Чёрча-Тюринга][church-turing-tesis], описывающую вычислимые (посредством математических алгоритмов) функции.

Из этой работы вышли две основополагающие модели вычисления. Знаменитая [**машина Тьюринга**][turing-machine], и [**лямбда-исчисление**][labmda-calculus].

Сегодня, принципы машины Тьюринга лежат в основе всех языков программирования. Язык или набор инструкций для процессора (или виртуальной машины) называется тьюринг-полным если на нем можно реализовать абстрактную машину Тьюринга. Первым известным примером тьюринг-полной системы было лямбда-исчисление, описаное Алонзо Чёрчем в 1936.

Лямбда-исчисление вдохновило на создание одного из первых высокоуровневых языков программирования, и второго старейшего из используемых на текущее время: Lisp.

Lisp был (и остается) чрезвычайно влиятельным и популярным языком в научном мире, кроме этого его использовали в **системах с непрерывным потоком данных**, таких как масштабируемые системы обработки векторной графики. Изначальная спецификация Lisp была разработана в 1958, и этот язык до сих пор встроен в большое количество сложных приложений. Примечательно, что почти все популярные САПР поддерживают AutoLISP, диалект Lisp, используемый в AutoCAD, который, в свою очередь, вместе с многочисленными клонами, до сих пор является самой широко используемой САПР в мире.

> Непрерывные данные: данные которые могут принимать любое значение на непрерывной шкале и их легче измерить чем подсчитать (примеры — температура и давление). В противоположность дискретным данным, которые могут быть подсчитаны и легко проиндексированы. Все значения приходят из известного набора допустимых значений. В частности, частота звучания скрипки может быть представлена непрерывным набором данных, потому что смычок может находиться в любом месте грифа в любой момент времени. В противоположность, звуки, полученные с помощью пианино, наоборот являются дискретными, так как каждая нота настраивается на определенную клавишу, и невозможно извлечь звуки отличные от тех что уже настроены. Обычно на клавишах пианино 88 нот. Количество нот же, которые может производить скрипка, подсчитать невозможно, потому что скрипач производит их с помощью скольжения смычком вверх-вниз между разными нотами.

Lisp и его производные породили целое семейство **функциональных языков программирования**, таких как: Curry, Haskell, Erlang, Clojure, ML, OCaml и т.д.

Не менее популярный, JavaScript взрастил целое поколение программистов использующих концепцию функционального программирования.

ФП естественно для JavaScript, поскольку язык обладает всеми необходимыми особенностями: функции первого порядка, замыкания и простой лямбда-синтаксис.

JavaScript позволяет легко присваивать функции переменным, передавать их в другие функции, возвращать функции из функций, выполнять композицию функций, и так далее. Этот язык также предлагает иммутабельные структуры данных и функции, которые позволяют легко возвращать новые объекты и массивы вместо изменения тех, которые передаются в качестве аргументов.

*Почему это так важно?*

Словарь ФП содержит множество сложных определений, но, в целом, суть функционального программирования проста: **создание больших программ с помощью маленьких повторно используемых предсказуемых чистых функций**.

Чистые функции обладают определенными свойствами, которые делают их максимально повторно используемыми и крайне полезными в большом количестве приложений:

**Идемпотентность:** При одинаковых входных параметрах, чистая функция всегда вернет тот же самый результат, независимо от того сколько раз она была вызвана. Возможно, вы слышали это слово при описании HTTP GET запросов.
**Идемпотентность важна при создании RESTful-сервисов** в вебе, но она также полезна в программах которые зависят от времени и порядка операций — **без нее точно не обойтись при параллельных вычислениях и горизонтальном масштабировании**.

Из-за этого свойства, чистые функции отлично подходят когда необходимо работать с непрерывными наборами данных. Например, постепенное «проявление/затухание» сигнала в видео или аудио. Как правило, подобные алгоритмы выполняются в виде линейных или логарифмических кривых вне зависимости от частоты кадров или частоты дискретизации, и используются в последней точке непрерывных данных(аудио/видеосигнал) для получения дискретных значений. В графике, аналогичные функции применяются для масштабирования векторных изображений, таких как SVG, шрифты и файлы Adobe Illustrator. Ключевые кадры, или контрольные точки, располагаются по отношению друг к другу относительно или находятся в разных местах по времени, а не  фиксированно с известным положением пикселей, кадров и прочих единиц измерения.

Так как идемпотентные функции не зависят от времени или размера данных, можно рассматривать непрерывные данные как неограниченный (практически бесконечный) поток, что дает нам возможность свободно масштабировать его по времени, разрешению пикселей, громкости звука, и тому подобным непрерывным характеристикам.

**Свобода от побочных эффектов:** Чистые функции могут безопасно применяться без каких-либо побочных эффектов, это значит что они не изменяют переданные в них аргументы и не производят никаких внешних действий кроме возвращения значений: не создают исключений, не порождают событий, не работают напрямую с вводом-выводом устройств, сетью, консолью, дисплеем и прочее.

Из-за отсутствия общей области видимости и побочных эффектов, чистые функции гораздо реже конфликтуют друг другом и остальным кодом, вызывают меньше ошибок в несвязанных частях программы.

Если перефразировать в терминах ООП, **чистые функции, по сравнению с другими методами, гораздо прочнее инкапсулируют содержимое**, при этом обеспечивая остальные преимущества ООП: возможность поменять реализацию без влияния на остальную часть программы, самодокументированный публичный интерфейс (я говорю о входных параметрах функций), независимость от внешнего кода, возможность свободно перемещать код между файлами, модулями, проектами.

*Чистые функции это ответ ФП на ООП-проблему гориллы и банана:*
> «Проблема объектно-ориентированных языков в том что они тащут за собой всю неявную среду.  Вы хотите получить банан, но кроме банана в нагрузку получаете гориллу, и все чертовы джунгли»  ~ Джо Армстронг

Очевидно, что большинство программ передает какие-то данные, поэтому комплексные программы обычно не могут быть созданы с использованием одних **только** чистых функций, но использовать чистые функции везде где это рационально — отличная идея.

#### Программируем в функциональном стиле
ФП предоставляет несколько стандартных типов функций, подходящих для использование в любом месте. Можно получить множество вариантов применения, соединяя эти функции в различной последовательности. В основном, данный список перекочевал без изменений из документации Haskell. Haskell до сих пор остается одним из самых популярных функциональных языков,    однако приведенные ниже функции или похожие на них вы будете встречать во многих популярных JavaScript-библиотеках:

###### Базовые методы работы со списками:
* `head()` — получить первый элемент
* `tail()` — получить все элементы кроме первого
* `last()` — получить последний элемент
* `length()` — вернуть количество элементов

###### Утверждения / сравнения:
* `equal()`
* `greaterThan()`
* `lessThan()`

###### Поиск по спискам:
* `find() ([х]) -> y` — взять список «х» и вернуть первый элемент, соответствующий условию
* `filter () ([х]) -> [у]` — взять список «х» и вернуть все элементы, соответствующие условию

###### Преобразование списков:
* `map() ([х]) -> [у]` — взять список «х» и применить преобразование к каждому элементу в этом списке, вернуть новый список «y"
* `reverse() ([1, 2, 3]) -> [3, 2, 1]`
* `reduce() ([х], function [, accumulator]) -> y` — применить функцию к каждому элементу списка «x» и объединить результат в одно значение
* `any()` — `истина`, если любое из значений соответствует условию
* `all()` — `истина`, если все значения соответствуют условию
* `sum()` — сумма всех значений
* `product()` — произведение всех значений
* `maximum()` — максимальное значение
* `minimum()` — минимальное значение
* `concat()` — вернуть список в котором соединены все переданные списки
###### Итераторы / Генераторы / Коллекторы (бесконечные списки)
* `sample()` — возвращает текущее значение непрерывного источника (температура, формы ввода текста, переключатель, и т.д)
* `repeat() — (1) -> [1, 1, 1, 1, 1, 1, …]`
* `cycle() / loop()` — вернуться и начать все заново когда будет достигнут конец списка

Некоторые из этих функций были добавлены в JavaScript при принятии стандарта ECMAScript5, например как дополнительные методы массива:
```javascript
// в новом ES6 синтаксе «() =>» эквивалентно «function () {}"
var foo = [1, 2, 3, 4, 5];
var bar = foo.map( (n) => n + 1 ); // [2, 3, 4, 5, 6]
var baz = bar.filter( (n) => n >=3 && n <=5); // [3,4,5]
var bif = baz.reduce( (n, el) => n += el); // 12
```

#### Списки в роли общего интерфейса
Вы уже заметили, что большинство описанных выше функций работают со **списками элементов**. После того как вы начнете писать код в функциональном стиле, вы станете замечать, что практически все является **списком**, **элементом** списка, **условием** для проверки значений списка, или **преобразованием** списка. Такой способ мышления закладывает основу для повторно используемого кода.

**Общие функции** — функции, способные работать с разными типами данных. В JavaScript большинство функций, которые работают с коллекциями, являются общими. К примеру, вы можете использовать одну и ту же функцию для строк и массивов, потому что строка может быть представлена как массив символов.

Процесс внедрения в функцию поддержи работы с разными типами данных называется «поднятие» (**lifting**).

Он основан на том, что все эти данные обрабатываются списком, то есть **список как тип данных является единым интерфейсом взаимодействия**. В обсуждал лифтинг в лекции «Static Types are Overrated» на Fluent 2014.

#### Как прекратить обращать внимание на мелочи

В объектно-ориентированом и империативном программировании мы контролируем всё. Состояния, счетчики итерации, синхронизацию событий по времени, и каллбеки. А что если бы я вам сказал, что вся эта работа совершенно не нужна, что вы могли бы буквально удалять целые куски кода ваших программ?

**Реактивное программирование** использует функциональные методы `map`, `filter` и `reduce` для создания и обработки потоков данных,распространяемых внутри системы. Когда ввод X изменяется, вывод Y автоматически обновляется в ответ, именно поэтому мы и используем термин «реактивное", то *есть основанное на реакции*.

В OOП вы можете создать какой-нибудь объект (скажем, форму ввода), превратить этот объект в эмиттер событий, настроить слушателя который будет что-то делать когда событие произойдет.

При использовании реактивного программирования, вы вместо этого определите события данных в декларативной манере, при этом самая сложная часть будет возложена на стандартные функции, так что вам не придется изобретать колесо каждый раз.

**Представляйте каждый список потоком данных**: массив это поток значений в порядке их положения по индексу, поле ввода формы может быть потоком дискретных значений при каждом изменении текста, кнопка это поток нажатий.

> Поток данных это список значений, распределенный на временной шкале.

В понятиях Rx (реактивные расширения) вы создаете **наблюдаемые потоки**, а затем обрабатываете их используя набор стандартных функций которые мы рассмотрели выше.

Подобно потокам в node.js, наблюдаемые потоки могут дополнительно содержать **механизм буферизации**. Как и при использовании **[генераторов][es-generators]**, ваша программа без потерь данных может брать значения из наблюдаемых потоков в своем собственном темпе, тогда когда будет готова их обработать и не раньше.

Представьте, вы хотите отсортировать все ваши социальные медиа-каналы по определенному хэш-тегу. Вы можете собрать список списков, объединить их в порядке поступления, а затем использовать любую комбинацию из описанных выше функций для их обработки.

Пример того как это может выглядеть:
```javascript
var messages = merge(twitter, fb, gplus, github)
messages.map(normalize) // преобразуем сообщения в единый формат
  .filter(selectHashtag) // отбираем сообщения с нужным нам тегом
  .pipe(process.stdout) // посылаем результат в консоль
```

#### Еще больше асинхронности
Возможно, вы слышали об [обещаниях][es-promises]? **Обещание** это объект, предоставляющий стандартный интерфейс для работы со значениями, которые могут или не могут быть доступны во время вызова обещания. Другими словами, обещание возвращает вместо нужного значения — себя, и сообщает когда приходит нужный результат:

```javascript
fetchFutureStockPrices().then(becomeAMillionaire);
```

Ну, на самом деле это работает не совсем так, потому что `.then()` будет вызвано только когда цены на акции станут доступны, поэтому когда `becomeAMillionaire` наконец-то выполнится — цены на «будущие» акции» уже станут настоящими.

В общем случае, обещание это поток который сообщает только одно значение (или ошибку). Наблюдаемые значения могут заменить обещания, плюс обеспечить весь функционал стандартных утилит к которым вы привыкли если используете библиотеки наподобие Underscore или Lo-Dash.

Сейчас отличное время узнать больше о функциональном программировании, преимуществах чистых функций, ленивом вычислении, и о многом другом. И я *обещаю*, что вы еще не раз услышите об этих темах в течение ближайших лет.

Приближаясь к 2015 году *(прим. пер.: статья писалась в 2014 году)*, мы возвращаемся к корням информатики заложенным в 1930-х — чтобы легче управлять программным обеспечением будущего.

#### Информация для дальнейшего ознакомления
Мне очень понравилась лекция технического руководителя Netflix Джафара Хусайна на конференции Scale@2014. Она называется «Asynchronous Programming at Netflix» ([видео][youtube-netflix]).

Джеймс Коглан провел отличную беседу, затрагивающую такие ключевые темы как роль обещаний и ленивые вычисления ([видео][youtube-coglan])

Если вы готовы заняться реактивным программированием, ознакомьтесь с [Reactive Extensions for JavaScript][rxjs], [The Reactive Manifesto][reactive-manifesto], и поработайте с [этими][rx-lessons] интерактивными уроками.

Для понимания функциональных потоков в node.js ознакомьтесь со статьей «[How I Want to Write Node: Stream All the Things!]» и библиотекой [Highland.js][highland].

Для детального понимания концепций реактивности, ознакомьтесь с отличной работой «[General Theory of Reactivity][gtor]» Криса Ковальски.

Ну и, конечно: [узнайте все об обоих столпах с серией моих платных курсов][pillar-courses] :)

[pillars-part-1]: http://frontender.info/the-two-pillars-of-javascript/
[singularity]: https://ru.wikipedia.org/wiki/Технологическая_сингулярность
[waterfall]: https://ru.wikipedia.org/wiki/Каскадная_модель
[agile]: https://ru.wikipedia.org/wiki/Гибкая_методология_разработки
[koan]: https://ru.wikipedia.org/wiki/Коан
[book-orelly]: http://learn-javascript.ericelliott.me/programming-javascript-applications/
[anton-van-staaten]: http://people.csail.mit.edu/gregs/ll1-discuss-archive-html/msg03277.html
[church-turing-tesis]: https://ru.wikipedia.org/wiki/Тезис_Чёрча_—_Тьюринга
[turing-machine]: https://ru.wikipedia.org/wiki/Машина_Тьюринга
[labmda-calculus]: https://ru.wikipedia.org/wiki/Лямбда-исчисление
[es-generators]: https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Iterators_and_Generators
[es-promises]: https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Promise
[youtube-netflix]: https://www.youtube.com/watch?v=gawmdhCNy-A
[youtube-coglan]: https://www.youtube.com/watch?v=XcS-LdEBUkE
[rxjs]: https://github.com/Reactive-Extensions/RxJS
[reactive-manifesto]: http://www.reactivemanifesto.org
[rx-lessons]: http://reactivex.io/learnrx
[node-streams]: http://caolanmcmahon.com/posts/how_i_want_to_write_node_stream_all_the_things_new/
[highland]: http://highlandjs.org
[gtor]: https://github.com/kriskowal/gtor
[pillar-courses]: https://ericelliottjs.com