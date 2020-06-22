# Описание
[Discord Bot](https://discordapp.com/) написанный на `php`, который по средствам [REST API](https://discordapp.com/developers/docs/intro) и `WebSocket` ведёт расписание событий к канале гильдии. Участники в канале гильдии отправляют ему сообщения в определённом формате, он их обрабатывает и удаляет. Из обработанных сообщений **формируется расписание**, которое отображается в виде сводного сообщения в этом же канале. Закреплённые сообщения он не обрабатывает и не удаляет, в них можно написать инструкции для пользователей. В первую очередь скрипт предназначен для использования в планировщике `cron` на `php` хостинге.
# Формат сообщений
Чтобы создать, записаться или выписаться из рейда необходимо в канале, который контролирует бот, написать сообщение в следующем формате:
```
<action> <role> <date> <time> <raid> [<user>] <addition> <comment>
```
- `<action>` - Действие, которое нужно выполнить.
    - **+ (записаться)** - Записаться или создать рейд.
    - **\* (изменить)** - Изменить запись в рейде.
    - **- (выписаться)** - Выписаться из рейда.
- `<role>` - Роль участника рейда.
    - **tank (танк)** - Держит удар.
    - **heal (хил)** - Восстанавливает здоровье.
    - **dd (дд)** - Наносит урон.
- `<date>` - Дата рейда, например: **20.02.2020**, **завтра**, **вторник**.
- `<time>` - Время рейда, например: **18:00**.
- `<raid>` - Идентификатор рейда, например: **vHRC**.
- `<user>` - Для записи других участников, в формате **@id**
- `<addition>` - Дополнительная опция.
    - **leader (лидер)** - Записаться как лидер.
    - **member (участник)** - Записаться как участник.
    - **once (однократно)** - Без повторений.
    - **daily (ежедневно)** - Повторять ежедневно.
    - **weekly (еженедельно)** - Повторять еженедельно.

- `<comment>` - Комментарий к рейду.
## Примеры сообщений
```
+ дд 18.01.2020 18:00 vHRC
+ танк завтра 15:00 vHOF
+хил завтра 20:00 nCR лидер Комментарий
+ дд сегодня 21:00 vSS 640 CP и 45K DPS
- танк 18.01 18:00
-хил
```
## Права доступа
Чтобы бот начал контролировать канал и обрабатывать сообщения в нём, непосредственно в этом канале ему нужно выдать права: **Читать сообщения**, **Отправлять сообщения**, **Управлять сообщениями** и **Читать историю сообщений**. Бот игнорирует унаследованные права от роли, категории и гильдии, т.к. он удаляет сообщения в контролируемом канале.

Чтобы пользователи могли создавать рейды, а не только записываться в имеющиеся, непосредственно в этом канале у них (или у их роли или у `@everyone`) должно быть право **Прикреплять файлы**.

Чтобы пользователи могли записывать других участников, непосредственно в этом канале у них (или у их роли или у `@everyone`) должно быть право **Встраивать ссылки**.
# Использование
После **публикации** на `php` хостинге и заполнения в файле `php/api.php` значений `MY-APP-TOKEN`, `MY-DISCORD-BOT-ID`, `MY-DISCORD-BOT-TOKEN` и перейдите в браузере или в планировщике `cron` по следующему адресу:
```
php/api.php?method=<method>&token=<token>[&format=<format>][... &param=<param>]
```
- `<method>` - Имя метода, который нужно выполнить.
- `<token>` - Заданный вами `MY-APP-TOKEN`.
- `<format>` - Формат результата работы, поддерживается `json`, `xml` и `http`.
- ... `<param>` - Параметры для метода.
## Поддерживаемые методы
`discord.connect` - Выполнить подключение по `WebSocket` и в синхронном режиме обрабатывать все поступающие сообщения в контролируемых каналах всех гильдий.

`discord.guild` - Выполнить подключение по `REST API` и разово обработать все поступившие сообщения в контролируемых каналах заданной гильдий.
- `<guild>` - Цифровой идентификатор гильдии в Discord.

`discord.channel` - Выполнить подключение по `REST API` и разово обработать все поступившие сообщения в указанном канале заданной гильдий.
- `<guild>` - Цифровой идентификатор гильдии в Discord.
- `<channel>` - Цифровой идентификатор канала в гильдии.

`discord.message` - Выполнить подключение по `REST API` и разово обработать конкретное сообщение в указанном канале заданной гильдий.
- `<guild>` - Цифровой идентификатор гильдии в Discord.
- `<channel>` - Цифровой идентификатор канала в гильдии.
- `<message>` - Цифровой идентификатор сообщения в канале.
## Примечание
Выражаю огромную благодарность всем, кто мне помогает с разработкой бота. А также отдельное спасибо [paragi](https://github.com/paragi) за рабочую реализацию протокола [WebSocket](https://github.com/paragi/PHP-websocket-client) на `php`. Для полноценной работы, по мимо вышеупомянутого набора функций `webSocketClient-1.0.inc.php`, требуется библиотека `phpEasy-0.3.inc.php`, класс `File-0.1.inc.php` и подкласс `FileStorage-0.5.inc.php`. К сожалению, я сейчас не готов опубликовать эти наработки в открытый доступ. Поэтому, к сожалению, у вас не получиться запустить бота на вашем `php` хостинге, т.к. в данном репозитории отсутствуют вышеупомянутые библиотеки и классы. Но вы можете посмотреть его работу в [этой](https://discord.gg/J8smX6R) **демонстрационной** гильдии.