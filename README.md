# je3-project

[Ссылка на Схему базы данных](https://app.sqldbm.com/MySQL/Edit/p43934/)

[Ссылка на статические данные для наполнения таблиц в нашей базе данных](google.com)

[Ссылка на Project Management app](https://trello.com/b/KkkeYEnU/game-project)


# Технологии:
# BE
---------------------
- [Maven](https://maven.apache.org/)
- [JUnit](https://junit.org/junit4/)
- [Hamcrest](http://hamcrest.org/JavaHamcrest/)
- [OkHttp](http://square.github.io/okhttp/)
- [MySql](https://www.mysql.com/)
- [JDBC](https://docs.oracle.com/javase/tutorial/jdbc/basics/index.html)
- [SQL](https://www.w3schools.com/sql/)
- [REST](https://www.codecademy.com/articles/what-is-rest)
- [Jetty (Web server and javax.servlet container)](https://www.eclipse.org/jetty/)
- [Jersey (JAX-RS)](https://jersey.github.io/)

# FE
---------------------
- [JavaScript](https://www.w3schools.com/jS/default.asp)
- [Ajax](https://www.w3schools.com/jS/js_ajax_intro.asp)
- [Json](https://www.w3schools.com/js/js_json_intro.asp)
- [HTML](https://www.w3schools.com/html/default.asp)
- [CSS](https://www.w3schools.com/css/default.asp)

# Краткое описание проекта:
=========================== 
```
Упрощенная RTS игра в которую можно играть вдвоем по сети. 

Остовные правила:

Используя карты нужно достичь максимального показателя определенного ресурса(например Власти) или добиться нулевого показателя этого ресурса у противника.

Карты являются единственной возможностью управлять процессом игры и они могут влиять(плюсовать и минусовать) на "Ресурсы" "Здания" и "Апгрейды" собственные и противника.

Чтобы сделать любой экшен(действие) в игре например: купить здание, проапгрейдить здание, пополнить ресурс, нанести удар по зданиям апгрейдам или ресурсам противника, обменять один ресурс на другой и т.д. нужно применять доступные карты.

Эти или какие либо иные придуманые экшены будут реализовываться по средствам описания действия той или иной карты. 

Применение карты выражается в плюсовании или минусовании набраных Ресурсов Зданий и Апгрейдов в таблицах - User_Building, User_Resource, User_Upgrade.

В списке карт каждая карта может появиться тогда когда для нее есть все ее требуемые "Ресурсы" "Здания" и "Апгрейды".

После использования карты мы создаем запрос в REST API нашего серверсайда и вычитаем все расходники из таблиц:

User_Building, User_Resource, User_Upgrade, для себя и для противника.
```

# Описание игры:
=========================== 
```
Игра состоит из 4-х html страниц(с которых будут делаться все наши запросы в наш REST сервис с использованием JavaScript+Ajax+Json):

Станица Login-а или  Create new User.

Страница списка всех доступных игровых комнат (Room-ов) в которых можно играть, каждый элемент списка состоит из "номер комнаты", "названия комнаты", "количесво игроков в комнате". и кнопки:
"Join Room" - если в выбраной комнате есть один игрок то сразу начать игру, если вы в комнате один то занять место в ожидании когда прийдет другой игрок и стартанет игру.

Страница где и происходит сам бой между игроками куда мы попадаем после старта игры. Состоит из следующих компонентов:

- "Список Построеных Зданий" (table Building and User_Building)

- "Список Полученых Ресурсов" (table Resource and User_Resource)

- "Список Полученых Апгрейдов" (table Upgrade and User_Upgrade)

- "Список Построеных Зданий Противника" (table Building and User_Building)

- "Список Полученых Ресурсов Противника" (table Resource and User_Resource)

- "Список полученых Апгрейдов Противника" (table Upgrade and User_Upgrade)

- "Список Карт текущего игрока(список карт противника не показываем)" (tables Сard and Card_Product возможно еще что-то)

Страница достижений. Таблица в которой отображаются все Ачивки набраные в играх всех игроков упорядоченая по убыванию достижений. (edited)
```

# Краткая справка по таблицам
============================

`User` - таблица в которой сохранены все зарегистрированые в игре пользователи

`Room` - таблица в которой хранятся все комнаты - комныты нужны чтобы обьединять пары игроков для проведения игр.

`Message` - таблица в которой хранятся все сообщения которые создаются игроками в чате

`Building` - таблица в которой хранится статическая информация о здании - название и описание (билдинги позволяют производить ресурсы - больше билдингов больше ресурсов)

`User_Building` - таблица котороя отображает сколько билдингов пользователь уже нажил в течении одной игры, обнуляется на старте игры

`Building_Resource` - важная таблица которая содержит информацию о том какие ресурсы и сколько каждое здание производит а какие потребляет дастигается за счет положительных и отрицательных значений количества, таблица обновляется после каждого применения соответствующих карт

`Resource` - таблица в которой хранится статическая информация о ресурсе - название и описание (ресурсы это основные элементы количество которых надо накапливаь в игре)

`User_Resource` - отображает сколько пользователь заработал в текущей игре ресурсов - таблица постоянно обновляется так как здания потребляют и производят ресурсы каждую минуту динамически, обнуляется на старте игры

`Upgrade` - таблица в которой хранится статическая информация об апгрейде - название и описание (апгрейды улучшают производство зданий на N процентов)

`User_Upgrade` - отображает какие апргрейды игрок приобрел в течении игры - эти апгрейды в свою очередь влияют на количество ресурсов произведенных зданиями в зависимости от указаных процентов, обнуляется на старте игры

`Upgrade_Building` - отображает все необходимые связи указывая у каких зданий производство ресурсов улучшается на какой процент

`Achievement` - таблица в которой хранится статическая информация о ачивменте - название и описание (просто награды типа звезд и медалей за достижения)

`User_Achievement` - отображает все достижения сделаные пользователем за все игры которые он когда либо сиграл - нужна для показания успеха игрока и сравнения с успехами других в одной теблице в фигишном UI на фронтенде, таблица обнуляется только когда создается новый пользователь

`Trigger_Achievement` - условия при достижении которыйх зачисляется ачивка на вашего пользователя

`Notification` - таблица в которой хранится статическая информация о нотификейшене - название и описание (нужна для придания игре динамики по смыслу тоже самое что и ачивменты)

`User_Notification` - таблица содержит список показанных и не показанных текстовых сообщений, таблица обнуляется при старте игры

`Trigger_Notification` - условия при достижении которых вам будет доступно текстовое сообщение, если был сделан реквест на получение доступных сообщений то все сообщени помечаются как прочитаные и не показываются при следующем реквесте

`Card` - таблица в которой хранится статическая информация о картах - название и описание (основная еденица управления процессом игры)

`Card_Impact` - самая сложная таблица в которой хранится информация о том сколько карта тратит ресурсов а также на какте здания и апгрейды влияет - причем карта может влиять не только на твои задния ресурсы и апгрейды но и на вражеские

`Card_Group` - таблица в которой хранится список групп на которые деляться карты. Например некоторые возможные названия групп: 'Покупка зданий', 'Покупка ресурсов', 'Атакова противника', 'Апгрейд зданий'