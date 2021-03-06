# Go: Часто задаваемые вопросы (FAQ)

_Перевод на русский язык официальных ответов на самые частозадаваемые вопросы о языке программирования Go._

📖 [Версия, доступная на Github Pages](https://akrisanov.github.io/golang_faq_ru/) и удобная для чтения,
например, с экрана мобильного устройства.

## Содержание

* [Происхождение](#происхождение)
  * [Цель проекта](#цель-проекта)
  * [История проекта](#история-проекта)
  * [Происхождение талисмана гофера](#происхождение-талисмана-гофера)
  * [Язык называется Go или Golang?](#язык-называется-go-или-golang)
  * [Зачем вы создали новый язык?](#зачем-вы-создали-новый-язык)
  * [Прорадители языка Go](#прорадители-языка-go)
  * [Основные принципы дизайна](#основные-принципы-дизайна)
* [Использование](#использование)
  * [Использует ли Google Go внутри компании?](#использует-ли-google-go-внутри-компании)
  * [Какие еще компании используют Go?](#какие-еще-компании-используют-go)
  * [Могут ли Go программы быть интегрированы с программами на C/C++?](#могут-ли-go-программы-быть-интегрированы-с-программами-на-cc)
  * [Какие IDE поддерживают Go?](#какие-ide-поддерживают-go)
  * [Поддерживает ли Go сериализацию protocol buffers от Google?](#поддерживает-ли-Go-сериализацию-protocol-buffers-от-google)
  * [Могу ли я перевести домашнюю страницу Go на другой язык?](#могу-ли-я-перевести-домашнюю-страницу-go-на-другой-язык)
* [Дизайн](#дизайн)
  * [Есть ли у Go среда выполнения (runtime)?](#есть-ли-у-go-среда-выполнения-runtime)
  * [Что насчет Unicode идентификаторов?](#что-насчет-unicode-идентификаторов)
  * [Почему в Go нет возможности X?](#почему-в-go-нет-возможности-x)
  * [Почему в Go нет обобщенных типов (дженериков)?](#почему-в-go-нет-обобщенных-типов-дженериков)
  * [Почему в Go нет исключений?](#почему-в-go-нет-исключений)
  * [Почему в Go нет утверждений (assert)?](#почему-в-go-нет-утверждений-assert)
  * [Зачем строить конкурентность на идеях CSP?](#зачем-строить-конкурентность-на-идеях-csp)
  * [Почему горутины вместо потоков?](#почему-горутины-вместо-потоков)
  * [Почему операции с отображениями не определены как атомарные?](#почему-операции-с-отображениями-не-определены-как-атомарные)
  * [Вы примете мои изменения языка?](#вы-примете-мои-изменения-языка)
* [Типы](#типы)
    * [Является ли Go объектно-ориентированным языком?](#является-ли-go-объектно-ориентированным-языком)
    * [Как обеспечить динамическую отправку методов?](#как-обеспечить-динамическую-отправку-методов)
    * [Почему нет типа наследования?](#почему-нет-типа-наследования)
    * [Почему len - это функция, а не метод?](#почему-len-это-функция-а-не-метод)
    * [Почему Go не поддерживает перегрузку методов и операторов?](#почему-go-не-поддерживает-перегрузку-методов-и-операторов)
    * [Почему в Go нет объявлений "implements"?](#почему-в-go-нет-объявлений-implements)
    * [Как я могу гарантировать, что мой тип удовлетворяет требованиям интерфейса?](#как-я-могу-гарантировать-что-мой-тип-удовлетворяет-требованиям-интерфейса)
    * [Почему тип T не удовлетворяет интерфейсу Equals?](#почему-тип-t-не-удовлетворяет-интерфейсу-equals)
    * [Могу ли я преобразовать `[]T` в  `[]interface{}`?](#могу-ли-я-преобразовать-t-в-interface)
    * Могу ли я преобразовать `[]T1` в `[]T2`, если `T1` и `T2` имеют один и тот же базовый тип?
    * Почему моя nil ошибка не равна nil?
    * Почему нет untagged unions, как в C?
    * Почему Go не имеет вариантов?
    * Почему в Go нет ковариантных типов результатов?

## Происхождение

### Цель проекта

Во времена создания Go, всего лишь десятилетие назад, мир программирования отличался от сегодняшнего.
Индустриальное программное обеспечение обычно разрабатывалось на C++ или Java, GitHub не существовал,
большинство компьютеров еще не были многопроцессорными, и кроме Visual Studio и Eclipse было
доступно совсем немного IDE или других инструментов высокого уровня, не говоря уже о бесплатном
доступе к ним.

Между тем, мы были разочарованы излишней сложностью использования языков, с которыми мы работали
при разработке серверного программного обеспечения. С тех пор, как были разработаны такие языки,
как C, C++ и Java, компьютеры стали развиваться невероятно быстро, но сам по себе акт программирования
не продвинулся так далеко. Также стало понятно, что многопроцессорные системы становятся общедоступными,
но большинство языков предоставляют мало возможностей в их эффективном и безопасном программировании.

Мы решили сделать шаг назад и подумать о том, какие основные проблемы будут доминировать в разработке
программного обеспечения в ближайшие годы по мере развития технологий, и как новый язык может помочь
в их преодолении. Например, рост числа многоядерных процессоров привел к тому, что язык должен
обеспечивать полноценную поддержку некоторой конкурентности или параллелизма. А для того, чтобы
сделать управление ресурсами отслеживаемым в большой конкурентной программе, необходима была сборка
мусора или хотя бы какое-то безопасное автоматическое управление памятью. 

Эти размышления привели к [серии дискуссий](https://commandcenter.blogspot.com/2017/09/go-ten-years-and-climbing.html),
из которых Go возник сначала как набор идей и пожеланий, а затем как язык. Главная цель заключалась
в том, чтобы Go делал больше для работающего программиста, предоставляя необходимые инструменты,
автоматизируя рутинные задачи, такие как форматирование кода, и устраняя препятствия при работе над
большими кодовыми базами.

Гораздо более подробное описание целей Go и того, как они осуществляются или, по крайней мере,
пытаются быть достигнуты, можно найти в статье
[«Go в Google: Дизайн языка в Службе разработки программного обеспечения»](https://talks.golang.org/2012/splash.article). 

### История проекта

21 сентября 2007 года [Роберт Гризмер](https://twitter.com/robertgriesemer), [Роб Пайк](https://twitter.com/rob_pike)
и [Кен Томпсон](https://en.wikipedia.org/wiki/Ken_Thompson) начинают зарисовывать цели нового языка
на белой доске. В течение нескольких дней цели были сведены в план для выполнения и помогали получить
представление о том, что это будет. Проектирование продолжалось в режиме неполного рабочего времени
параллельно с несвязанной работой. К январю 2008 года Кен начал работу над компилятором, с помощью
которого можно было бы исследовать идеи; на выходе он генерировал код на Си. К середине года язык
превратился в полноценный проект и достаточно устоялся, чтобы попробовать компилятор в промышленной
разработке. В мае 2008 года [Ян Тейлор](https://twitter.com/ianlancetaylor) самостоятельно начал
работу над GCC фронтендом для Go используя черновик спецификации. [Расс Кокс](https://twitter.com/_rsc)
присоединился к команде в конце 2008 года и помог перенести язык и библиотеки из прототипа в реальность.

Go стал публичным проектом с открытым исходным кодом 10 ноября 2009 года. Бесчисленное количество
людей из сообщества внесли свой вклад в идеи, обсуждения и код. 

Сейчас в мире миллионы Go-программистов – гоферов, и с каждым днем их становится все больше.
Успех Go намного превзошел наши ожидания. 

### Происхождение талисмана гофера

Талисман и логотип были разработаны [Рене Френч](https://reneefrench.blogspot.com/), которая также
сделала дизайн кролика [Glenda](https://9p.io/plan9/glenda.html) для
[Plan 9](https://en.wikipedia.org/wiki/Plan_9_from_Bell_Labs). [Пост в блоге](https://blog.golang.org/gopher)
о гофере объясняет, как он был получен из того, что она использовала для дизайна футболок
[WFMU](https://wfmu.org/) несколько лет назад. На логотип и талисман распространяется лицензия
[Creative Commons Attribution 3.0](https://creativecommons.org/licenses/by/3.0/). 

У гофера есть [макетный лист](https://golang.org/doc/gopher/modelsheet.jpg), иллюстрирующий его
характеристики и то, как их правильно изобразить. Он был впервые показан в
[выступлении](https://www.youtube.com/watch?v=4rw_B4yY69k) Рене на Гоферконе в 2016 году.
У него есть уникальные особенности; он – гофер Go, а не просто какой-нибудь старый
[гофер](https://ru.wikipedia.org/wiki/%D0%93%D0%BE%D1%84%D0%B5%D1%80%D0%BE%D0%B2%D1%8B%D0%B5). 

### Язык называется Go или Golang?

Язык называется Go. Название "golang" возникло из-за доменного имени golang.org – go.org был недоступен.
Однако многие используют имя Golang, и оно иногда удобно в качестве ярлыка. Например, тэг `#golang`
в твиттере. Название же _Go_ теряется.

Примечание: Несмотря на то, что официальный логотип состоит из двух больших букв, название языка
пишется как _Go_, а не _GO_. 

### Зачем вы создали новый язык?

Go родился из неудовлетворенности существующими языками и средами для работы, которую мы выполняли
в Google. Программирование стало слишком сложным, и отчасти в этом был виноват выбор языков.
Приходилось выбирать между быстрой компиляцией, эффективным исполнением или простотой программирования;
все три свойства были недоступны в каком-то одном распространенном языке. Программисты, которые
могли выбирать простоту, а не безопасность и эффективность, перешли на динамически типизированные
языки, такие как Python и JavaScript, а не на C++ или, в меньшей степени, на Java.

Мы были не одни в своих беспокойствах. После многих лет с довольно тихим ландшафтом языков
программирования, Go был одним из первых из нескольких новых языков – Rust, Elixir, Swift и пр. –
которые снова сделали разработку языков программирования активной, почти популярной сферой деятельности.

Go решает описанные выше проблемы, пытаясь совместить простоту программирования на интерпретируемом,
динамически типизированном языке с эффективностью и безопасностью статически типизированного,
компилируемого языка. Он также стремится быть современным, поддерживая сетевые и многоядерные вычисления.
И, наконец, работа с Go должна быть быстрой: сборка большого исполняемого файла на одном компьютере
должна занимать не более нескольких секунд. Для достижения этих целей необходимо было решить ряд
лингвистических задач: выразительная, но легкая система типов; конкурентность и сборка мусора;
жесткая спецификация зависимостей; и так далее. Все это не может быть решено только библиотеками или
инструментами; необходим новый язык.

В статье [Go в Google](https://talks.golang.org/2012/splash.article) обсуждаются предпосылки и
мотивация дизайна языка, а также приводится более подробная информация к многим ответам,
представленным в этом FAQ. 

### Прорадители языка Go

Go в большей степени относится к семейству Cи (основной синтаксис); значительный вклад оказало
семейство языков Pascal/Modula/Oberon (декларации, пакеты), кроме того, некоторые идеи были взяты
из языков, вдохновленных [CSP Тони Хоара](https://ru.wikipedia.org/wiki/%D0%92%D0%B7%D0%B0%D0%B8%D0%BC%D0%BE%D0%B4%D0%B5%D0%B9%D1%81%D1%82%D0%B2%D1%83%D1%8E%D1%89%D0%B8%D0%B5_%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%D0%B4%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5_%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D1%81%D1%81%D1%8B),
таких как Newsqueak и Limbo (конкурентность). Тем не менее, Go – это новый язык в целом.
Во всех отношениях язык был разработан с расчетом на то, что делают программисты и как сделать
программирование, по крайней мере, тот вид программирования, которым мы занимаемся, более эффективным,
а значит, более увлекательным.

### Основные принципы дизайна

Когда Go был спроектирован, Java и C++ были самыми распространенными языками для написания серверов,
по крайней мере в Google. Мы чувствовали, что эти языки требуют учета слишком многих вещей и
повторений. Некоторые программисты отреагировали на это переходом к более динамическим, гибким
языкам, таким как Python, в ущерб эффективности и безопасности типов. Мы сочли, что эффективность,
безопасность и гибкость должны быть доступны в одном языке.

Go пытается уменьшить количество кода. За время его проектирования мы попытались уменьшить
нагромождение и сложность. Нет ни предварительных объявлений, ни заголовочных файлов – все
объявляется ровно один раз. Инициализация является выразительной, автоматической и простой в
использовании. Синтаксис очевиден и легок с точки зрения ключевых слов. Многословность
`foo.Foo* myFoo = new(foo.Foo)` сокращается простым выводом типа с помощью конструкции `:=`
объявления и инициализации. И, пожалуй, самое радикальное, что в языке не существует иерархии типов:
сами типы есть, но они не должны объявлять о своих отношениях. Эти упрощения позволяют Go быть
выразительным и в то же время понятным без ущерба изяществу.

Другим важным принципом является сохранение концепций ортогональными. Методы могут быть реализованы
для любого типа; структуры представляют данные, в то время как интерфейсы представляют абстракции;
и так далее. Ортогональность облегчает понимание того, что происходит, когда вещи соединяются.

## Использование

### Использует ли Google Go внутри компании?

Да. Go широко используется в продакшене внутри Google. Одним из простых примеров является сервер
сайта [golang.org](https://golang.org/). Это просто [godoc](https://golang.org/cmd/godoc) сервер
документации, работающий в продакшен-конфигурации на [Google App Engine](https://developers.google.com/appengine/). 

Более значимым примером является сервер загрузок dl.google.com, который поставляет бинарные файлы
Google Chrome и другие большие инсталляционные материалы, такие как apt-get пакеты.

Go - далеко не единственный язык, используемый в Google, но он является ключевым для ряда областей,
включая надежность и безотказность (SRE) и крупномасштабную обработку данных. 

### Какие еще компании используют Go?

Использование Go растет по всему миру, особенно (и не только) в области облачных вычислений.
Пара крупных проектов в сфере облачной инфраструктуры написанны на Go - это Docker и Kubernetes,
но есть и много других.

Но и не только облака. Go Wiki содержит [страницу](https://github.com/golang/go/wiki/GoUsers),
которая регулярно обновляется и на которой перечислены некоторые из многих компаний, использующих Go.

В Wiki также есть страница со ссылками на [истории успеха](https://github.com/golang/go/wiki/SuccessStories)
компаний и проектов, применяющих Go.

### Могут ли Go программы быть интегрированы с программами на C/C++?

Использование С и Go вместе в одном адресном пространстве возможно, но не является естественным и
может потребовать специального интерфейса программного обеспечения. Кроме того, связывание С с Go
кодом приводит к потере безопасносного управления памятью и стеком, которое обеспечивает Go.
Иногда для решения задачи абсолютно необходимо использование библиотек на С, но это всегда вносит
элемент риска, который отсутствует в чистом Go-коде, поэтому делайте это с осторожностью.

Если вам все-таки необходимо использовать С вместе с Go, то взаимодействие зависит от реализации
компилятора Go. Есть три реализации компилятора Go, поддерживаемые командой языка. Это `gc` -
компилятор по умолчанию, `gccgo` - использующий GCC в качестве бэкенда, и немного менее зрелый
`gollvm` на базе инфраструктуры LLVM.

`Gc` использует другое соглашение о вызовах и линкер, отличные от С, и поэтому не может быть вызван
напрямую из программ на C, или наоборот. Программа cgo предоставляет механизм "интерфейса внешней функции",
позволяющий безопасно вызывать библиотеки C из кода Go. SWIG расширяет эту возможность для
библиотек C++. 

Вы также можете использовать cgo и SWIG с `Gccgo` и `gollvm`. Так как они используют традиционные
API, то также возможно с большой осторожностью компоновать код из этих компиляторов напрямую с
программами на C или C++, скомпилированными с помощью GCC/LLVM. Однако, чтобы сделать это безопасно,
требуется понимание соглашений по вызову для всех задействованных языков, а также понимание
ограничений стека при вызове C или C++ из Go.

### Какие IDE поддерживают Go?

Проект Go не включает в себя собственную IDE, но язык и библиотеки были разработаны так, чтобы
облегчить анализ исходного кода. Как следствие, большинство известных редакторов и IDE хорошо
поддерживают Go, либо напрямую, либо через плагин.

В список известных IDE и редакторов с хорошей поддержкой Go входят Emacs, Vim, VSCode, Atom,
Eclipse, Sublime, IntelliJ (через кастомную версию GoLand), и многие другие. Скорее всего, ваша
любимая среда является продуктивной для программирования на Go.

### Поддерживает ли Go сериализацию protocol buffers от Google?

Отдельный проект с открытым исходным кодом предоставляет необходимый плагин компилятора и библиотеку.
Он доступен по ссылке [github.com/golang/protobuf/](https://github.com/golang/protobuf)

### Могу ли я перевести домашнюю страницу Go на другой язык?

Конечно. Мы призываем разработчиков создавать сайты языка Go на своих родных языках.
Однако, если вы решили добавить логотип или брендинг Google на свой сайт (он не присутствует на
[golang.org](https://golang.org/)), вы должны будете соблюдать правила, изложенные на странице
[www.google.com/permissions/guidelines.html](https://www.google.com/permissions/guidelines.html)

## Дизайн

### Есть ли у Go среда выполнения (runtime)?

Go имеет обширную библиотеку, называемую _runtime_ (среда выполнения), которая является частью
каждой программы на Go. Библиотека выполнения реализует сборку мусора, конкурентность, управление
стеком и другие важные функции языка Go. Несмотря на то, что среда выполнения является более центральной
для языка, в Go она аналогична библиотеке _libc_ языка C.

Тем не менее, важно понимать, что среда выполнения Go не включает в себя виртуальную машину, например,
предоставляемую средой выполнения в Java. Программы на Go заранее компилируются в нативный машинный
код (JavaScript или WebAssembly для некоторых вариантов реализации). Таким образом, хотя этот термин
часто используется для описания виртуальной среды, в которой выполняется программа, в языке Go слово
"runtime" - это просто название, данное библиотеке, предоставляющей критические языковые сервисы. 

### Что насчет Unicode идентификаторов?

При проектировании Go мы хотели убедиться, что он не слишком ASCII-ориентирован, что означало
расширение пространства идентификаторов за пределами 7-битного ASCII. Правило Go – символы,
являющиеся идентификаторами, должны быть буквами или цифрами, как они определены в Юникоде - это
просто для понимания и реализации, но имеет ограничения. Например, составные символы исключены
согласно дизайну, и это не позволяет использовать некоторые языки, такие как деванагари.

У этого правила есть еще одно досадное последствие. Поскольку экспортируемый идентификатор должен
начинаться с заглавной буквы, то идентификаторы, созданные из символов на некоторых языках, могут,
по определению, не экспортироваться. Пока единственным решением является использование чего-то вроде
`X日本語` (заметьте символ `X` перед словом), что явно оставляет желать лучшего.

Начиная с ранней версии языка, была проведена значительная работа по расширению пространства
идентификаторов для удобства работы программистов, использующих родные языки. Вопрос о том, что
именно нужно сделать, остается активной темой обсуждения, и будущая версия языка может быть более
либеральной в своем определении идентификатора. Например, некоторые идеи могут быть взяты на
вооружение из [рекомендаций](http://unicode.org/reports/tr31/) организации Юникода по идентификаторам.
Как бы это ни было реализовано, это должно быть сделано с сохранением (или, возможно, расширением)
того, как регистр букв определяет видимость идентификаторов, что остается одной из наших любимых
особенностей Go.

На данный момент у нас есть простое правило, которое может быть расширено позже без необходимости
нарушения программ – правило, позволяющее избежать ошибок, которые могли бы возникнуть из-за
допущения неоднозначных идентификаторов.

### Почему в Go нет возможности X?

Каждый язык обладает новыми возможностями и не включает в себя чью-либо любимую функциональность.
Go был спроектирован с учетом легкости программирования, скорости компиляции, ортогональности
концепций, а также необходимости поддержки таких функций, как конкурентность и сборка мусора.
Ваша любимая функциональность может отсутствовать, потому что она не подходит, поскольку влияет на
скорость компиляции или понятность дизайна, или потому что это усложнит фундаментальные вещи.

Если вас беспокоит, что в Go отсутствует возможность X, пожалуйста, простите нас и изучите возможности,
которые Go предоставляет. Вы можете обнаружить, что они интересным образом компенсируют отсутствие Х.

### Почему в Go нет обобщенных типов (дженериков)?

Дженерики могут быть добавлены в какой-то момент. Мы не чувствуем в них неотложной необходимости,
хотя, как мы понимаем, некоторые программисты чувствуют.

Go был задуман как язык для написания серверных программ, которые будет легко поддерживать с
течением времени. (См. [эту статью](https://talks.golang.org/2012/splash.article) для дополнительной
информации). Дизайн был сосредоточен на таких вещах, как масштабируемость, читабельность и
конкурентность. Полиморфное программирование тогда казалось несущественным для целей языка и
поэтому было исключено в угоду простоте.

Сейчас язык стал более зрелым, и есть возможность рассмотреть некоторые формы обобщенного
программирования. Тем не менее, остаются некоторые нюансы.

Обобщенные типы удобны, но вместе с ними возникают расходы, сопряжённые со сложностью системы типов
и среды выполнения. Пока мы не нашли дизайн, который придавал бы ценность, пропорциональную сложности.
Хотя мы продолжаем думать об этом. В то же время, встроенные в Go отображения и слайсы, а также
возможность использовать пустой интерфейс для построения контейнеров (с явной распаковкой) позволяют
во многих случаях писать код, в котором пригодились бы дженерики, пусть и не так удобно.

Тема остается открытой. Ознакомиться с несколькими предыдущими неудачными попытками разработки
хорошей реализации обобщенных типов в Go можно в [этом предложении](https://golang.org/issue/15292). 

### Почему в Go нет исключений?

Мы считаем, что связывание исключений с управляющей структурой, как в идиоме `try-catch-finally`,
приводит к запутанному коду. Это также побуждает программистов называть слишком много обычных ошибок,
таких как неспособность открыть файл, исключениями.

Go придерживается другого подхода. Для простой обработки ошибок возвращение нескольких значений в
Go позволяют легко сообщать об ошибке без перегрузки возвращаемого значения.
[Каноническое представление ошибки в сочетании с другими возможностями Go](https://golang.org/doc/articles/error_handling.html)
делает обработку ошибок приятной, но совершенно отличной от обработки ошибок на других языках.

Go также имеет пару встроенных функций для оповещения и восстановления после действительно
исключительных ситуаций. Механизм восстановления выполняется только как часть состояния функции,
разрушаемого после ошибки, что является достаточным для обработки катастрофы, но не требует
дополнительных управляющих структур и при правильном использовании может привести к чистому коду
обработки ошибок.

Подробности см. в статье [Defer, Panic, and Recover](https://golang.org/doc/articles/defer_panic_recover.html).
Кроме того, пост в блоге [Errors are values](https://blog.golang.org/errors-are-values) описывает
один подход к чистой обработке ошибок в Go, демонстрируя, что, поскольку ошибки - это всего лишь значения,
вся мощь Go может быть направлена на их обработку.

### Почему в Go нет утверждений (assert)?

Go не предоставляет утверждений. Они, безусловно, удобны, но наш опыт показывает, что программисты
используют их как костыль, чтобы не думать о правильной обработке ошибок и оповещении о них.
Надлежащая обработка ошибок означает, что сервера продолжают работать вместо того, чтобы выходить
из строя после фатальной ошибки. Правильное информирование об ошибках означает, что ошибки являются
прямолинейными, что избавляет программиста от интерпретации большой трассировки аварии.
Точные ошибки особенно важны, когда программист, видя их, не знаком с кодом.

Мы понимаем, что это спорный вопрос. Есть много вещей в языке Go и библиотеках, которые отличаются
от современных практик просто потому, что мы чувствуем, что иногда стоит попробовать другой подход.

### Зачем строить конкурентность на идеях CSP?

Конкурентное и многопоточное программирование со временем стало отождествляться со сложностью.
Мы считаем, что это отчасти связано с непростыми конструкциями, такими как
[pthreads](https://en.wikipedia.org/wiki/POSIX_Threads), а также с чрезмерным акцентом на
низкоуровневых деталях – мьютексы, переменные состояния и барьеры памяти.
Высокоуровневые интерфейсы позволяют значительно упростить код, даже если под ними все те же
мьютексы и тому подобное.

Одна из наиболее успешных моделей обеспечения высокоуровневой языковой поддержки конкурентности
появилась в взаимодействующих последовательных процессах (Communicating Sequential Processes)
Тони Хоара, или CSP. Оккам и Эрланг - два хорошо известных языка, которые основаны на CSP.
Примитивы конкурентности в Go происходят из другой части семейного дерева, основным вкладом которой
является важное представление о каналах как об объектах первого класса. Опыт работы с несколькими
ранними языками показал, что модель CSP хорошо вписывается в основу процедурного языка.

### Почему горутины вместо потоков?

Горутины позволяют упростить конкурентность. Идея, которая возникла уже давно, заключается в
мультиплексировании независимо выполняющихся функций – корутин – в набор потоков. При блокировке
корутины, например, при вызове блокирующего системного вызова, среда выполнения (рантайм)
автоматически перемещает другие корутины из того же потока операционной системы в другой,
выполняемый поток, чтобы они не были заблокированы. Программист ничего этого не видит, и в этом суть.
Результат, который мы называем горутинами, может быть очень дешевым: у них мало накладных расходов
за пределами памяти для стека, который составляет всего несколько килобайт.

Чтобы сделать стеки маленькими, среда выполнения Go использует ограниченные стеки с возможностью
изменения размера. Новоиспеченной горутине дается несколько килобайт, чего почти всегда достаточно.
Когда это не так, среда выполнения автоматически увеличивает (и урезает) память для хранения стека,
позволяя многим горутинам жить в скромном объеме памяти. Нагрузка на процессор в среднем составляет
около трех дешевых инструкций на вызов одной функции. Вполне практично создавать сотни тысяч горутин
в одном адресном пространстве. Если бы горутины были просто потоками, системные ресурсы иссякли бы
в гораздо меньших пределах.

### Почему операции с отображениями не определены как атомарные?

После долгого обсуждения было решено, что типичное использование отображений не требует безопасного
доступа из нескольких горутин, а в тех случаях, когда это действительно необходимо, отображение,
вероятно, будет является частью какой-то более крупной структуры данных или вычислений, которые уже
были синхронизированы. Поэтому требование, чтобы все операции с отображениями захватывали мьютекс,
замедлит работу большинства программ и добавит безопасности лишь немногим. Однако это было непростое
решение, так как это означает, что неконтролируемый доступ к отображению может привести к аварийному
завершению программы.

Язык не исключает атомарных обновлений отображения. При необходимости, например, при выполнении
ненадежной программы, реализация может блокировать доступ к отображению. 

Доступ к отображениям небезопасен только при обновлении. До тех пор, пока все горутины только
считывают элементы отображения, включая итерацию по нему с использованием цикла с диапазоном, и не
меняют отображение, присваивая новые значения или производя удаление, для них безопасен
одновременный доступ к отображению без необходимости синхронизации.

В качестве помощи в правильном использовании отображения, некоторые реализации языка содержат
специальную проверку, которая автоматически сообщает в рантайме, когда отображение небезопасно
изменено при конкурентном выполнении.

### Вы примете мои изменения языка?

Люди часто предлагают улучшить язык – [список рассылки](https://groups.google.com/group/golang-nuts)
содержит богатую историю таких обсуждений, но очень мало из этих изменений было принято.

Хотя Go является проектом с открытым исходным кодом, язык и библиотеки защищены
[обещанием совместимости](https://golang.org/doc/go1compat.html), которое предотвращает изменения,
ломающие существующие программы, по крайней мере, на уровне исходного кода (программы, возможно,
придется время от времени перекомпилировать, чтобы они оставались в актуальном состоянии).
Если ваше предложение нарушает спецификацию Go 1, мы не сможем даже рассмотреть вашу идею,
независимо от ее значимости. Будущий мажорный релиз Go может быть несовместим с Go 1, и обсуждения
на эту тему только начались, но одно можно с уверенностью сказать наверняка: таких несовместимостей
в процессе будет очень мало. Более того, обещание совместимости побуждает нас обеспечить
автоматический переход для старых программ, чтобы адаптировать их в случае возникновения такой ситуации.

Даже если ваше предложение совместимо со спецификацией Go 1, оно может не соответствовать принципам
дизайна Go. Статья [«Go в Google: Дизайн языка в Службе разработки программного обеспечения»](https://talks.golang.org/2012/splash.article)
объясняет происхождение Go и мотивацию, лежащую в основе его дизайна.

## Типы

### Является ли Go объектно-ориентированным языком?

Да и нет. Хотя в Go есть типы и методы, и он допускает объектно-ориентированный стиль
программирования, в нем нет иерархии типов. Концепция «интерфейс» в Go предоставляет другой подход,
который мы считаем простым в использовании и в некотором роде более общим. Есть также способы
встраивать типы в другие типы, чтобы обеспечить нечто подобное, но не идентичное подклассам.
Более того, методы в Go более общие, чем в C++ или Java: они могут быть определены для любого типа
данных, даже для встроенных, таких как простые "unboxed" (представляющие значения) целые числа.
Они не ограничиваются структурами (классами).

Кроме того, отсутствие иерархии типов делает "объекты" в Go намного легче, чем в таких языках,
как C++ или Java.

### Как обеспечить динамическую отправку методов?

Единственный способ динамической отправки методов - через интерфейс. Методы структуры или любого
другого конкретного типа всегда разрешаются статически.

### Почему нет типа наследования?

Объектно-ориентированное программирование, по крайней мере, в наиболее известных языках,
подразумевает слишком много обсуждений отношений между типами – отношений, которые часто могут быть
выведены автоматически.

Вместо того, чтобы требовать от программиста заранее объявлять, что два типа взаимосвязаны, в Go
тип автоматически удовлетворяет любому интерфейсу, который определяет подмножество его методов.
Помимо сокращения кода, такой подход имеет и реальные преимущества. Типы могут удовлетворять многим
интерфейсам одновременно, без сложностей традиционного множественного наследования.
Интерфейсы могут быть очень легкими - интерфейс с одним или даже без методов может выразить
полезную концепцию. Интерфейсы могут быть добавлены после того, как появится новая идея, или для
тестирования – без аннотации оригинальных типов. Поскольку нет явных отношений между типами и
интерфейсами, нет необходимости управлять иерархией типов или обсуждать ее.

Можно использовать эти идеи, чтобы построить нечто похожее на типобезопасные пайпы Unix.
Например, посмотрите, как `fmt.Fprintf` реализует форматирование печати для любого вывода,
не только для файла, или как пакет `bufio` может быть полностью отделен от ввода/вывода файла,
или как пакеты `image` генерируют сжатые файлы изображений. Все эти идеи исходят из одного
интерфейса `io.Writer`, представляющего собой единый метод `Write`. И это только скольжение по
поверхности. Интерфейсы Go оказывают глубокое влияние на структуру программ.

К этому нужно привыкнуть, но этот неявный стиль зависимости типов - одна из самых продуктивных вещей в Go.

### Почему len - это функция, а не метод?

Мы обсуждали этот вопрос, но решили реализовать `len` и другие как функции, так как на практике этого
было достаточно, и они не усложняли вопросы по интерфейсу (в понимании типов в Go) основных типов.

### Почему Go не поддерживает перегрузку методов и операторов?

Отправка метода упрощается, если не требуется также выполнять сопоставление по типам.
Опыт работы с другими языками показал нам, что иногда полезно иметь различные методы с одним и тем
же именем, но с разными подписями, но на практике это также может быть запутанным и хрупким.
Сопоставление только по названию и требование единообразия типов было основным упрощающим решением
в системе типов Go.

Что касается перегрузки операторов, то это кажется скорее удобством, чем абсолютным требованием.
Опять же, без него все проще.

### Почему в Go нет объявлений "implements"?

*Примечание переводчика: см., например, [интерфейсы и implements в Java](https://javarush.ru/groups/posts/1981-dlja-chego-v-java-nuzhnih-interfeysih).*

Тип в Go удовлетворяет интерфейсу, реализуя методы этого интерфейса, не более того.
Это свойство позволяет определять и использовать интерфейсы без необходимости изменения существующего кода.
Таким образом, обеспечивается своего рода [структурная типизация](https://en.wikipedia.org/wiki/Structural_type_system),
которая способствует разделению ответственности и улучшает повторное использование кода, а также облегчает
разработку на основе паттернов, возникающих по мере его развития.
Семантика интерфейсов – одна из главных причин ощущения гибкости и легкости, которое даёт Go.

См. [вопрос о наследовании типов](#почему-нет-типа-наследования) для дополнительной информации. 

### Как я могу гарантировать, что мой тип удовлетворяет требованиям интерфейса?

Вы можете попросить компилятор проверить, что тип `T` реализует интерфейс `I`, попробовав присвоить
нулевое значение для `T` или указателя на `T` соответственно:

```go
type T struct{}
var _ I = T{}       // Проверка, что T реализует I.
var _ I = (*T)(nil) // Проверка, что *T реализует I.
```

Если `T` (или `*T`) не реализует `I`, то ошибка будет отловлена во время компиляции.

Если вы хотите, чтобы пользователи интерфейса явно объявили, что они его реализуют, вы можете
добавить метод с описательным именем в набор методов этого интерфейса. Например:

```go
type Fooer interface {
    Foo()
    ImplementsFooer()
}
```

В таком случае тип должен реализовать метод `ImplementsFooer`, чтобы быть `Fooer`, четко документируя
факт и объявляя его в формате, доступном для [go doc](https://golang.org/cmd/go/#hdr-Show_documentation_for_package_or_symbol).

```go
type Bar struct{}
func (b Bar) ImplementsFooer() {}
func (b Bar) Foo() {}
```

В большинстве кода такие ограничения не используются, так как они снижают практичность интерфейса.
Иногда, однако, они необходимы для устранения неясностей между похожими интерфейсами.

### Почему тип T не удовлетворяет интерфейсу Equals?

Рассмотрим простой интерфейс для представления объекта, который может сравнивать себя с другим значением:

```go
type Equaler interface {
    Equal(Equaler) bool
}
```

и этот тип, `T`:

```go
type T int
func (t T) Equal(u T) bool { return t == u } // не удовлетворяет Equaler
```

В отличие от аналогичной ситуации в некоторых системах полиморфного типа, `T` не реализует `Equaler`.
Типом аргумента `T.Equal` является `T`, а не буквально требуемый тип `Equaler`.

В Go система типов не повышает аргумент `Equal`, это ответственность программиста, как показывает
тип `T2`, который реализует `Equaler`:   

```go
type T2 int
func (t T2) Equal(u Equaler) bool { return t == u.(T2) }  // удовлетворяет Equaler
```

Хотя даже это не похоже на другие системы типов, потому что в Go любой тип, удовлетворяющий `Equaler`,
может быть передан в качестве аргумента в `T2.Equal`, и во время выполнения мы должны проверить,
что аргумент имеет тип `T2`. Некоторые языки обеспечивают такую гарантию во время компиляции.

```go
type Opener interface {
   Open() Reader
}

func (t T3) Open() *os.File
```

В Go, `T3` не удовлетворяет `Opener`, хотя подобное может быть возможно в другом языке.

В то время как система типов Go действительно делает меньше для программиста в подобных случаях,
отсутствие подтипизации определяет правило об удовлетворении интерфейса очень простыми в изложении:
_совпадают ли названия и подписи функций с именами и подписями интерфейса?_
Правило Go также легко реализовать эффективно. Мы считаем, что эти преимущества компенсируют
отсутствие автоматического повышения типа. Если когда-нибудь Go реализует какую-нибудь форму
полиморфной типизации, то мы ожидаем, что появится способ выразить не только идею из примеров выше,
но также выполнить их статическую проверку.

### Могу ли я преобразовать `[]T` в  `[]interface{}`?

Не напрямую. Это запрещено спецификацией языка, так как эти два типа не имеют одинакового представления в памяти.
Необходимо скопировать элементы по отдельности в целевой срез.
Этот пример преобразует срез `int` в срез `interface{}`:

```go
t := []int{1, 2, 3, 4}
s := make([]interface{}, len(t))
for i, v := range t {
    s[i] = v
}
```
