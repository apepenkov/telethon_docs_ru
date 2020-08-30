=============================
Telegram API на других языках
=============================


Telethon был сделан для **Python**, и, насколько я знаю, не
существует *такого же* порта для других языков. Не смотря на это,
*существуют* другие реализации, сделанные другими крутыми людьми
(тебе нужно быть крутым, чтобы хотя-бы понять офиициальную документацию
Telegram) на разных языках (под Python даже есть несколько библиотек),
список ниже:

C
=

Возможно, самая известная неофициальная реализация с открытым исходным кодом
от `@vysheng <https://github.com/vysheng>`__,
`tgl <https://github.com/vysheng/tgl>`__, и его консольный клиент
`telegram-cli <https://github.com/vysheng/tg>`__. Последующая разработка
переместилась на `BitBucket <https://bitbucket.org/vysheng/tdcli>`__.

C++
===

Новейшая (и оффициальная) библиотека, написанная с нуля, названа
`tdlib <https://github.com/tdlib/td>`__, и её использует Telegram X.
Вы можете найти больше информации в оффициальной документации,
расположенной `здесь <https://core.telegram.org/tdlib/docs/>`__.

JavaScript
==========

`Ali Gasymov <https://github.com/alik0211>`__ сделал `@mtproto/core <https://github.com/alik0211/mtproto-core>`__
библиотеку для браузера и nodejs, утсананвливаемую через`npm <https://www.npmjs.com/package/@mtproto/core>`__.


`painor <https://github.com/painor>`__ - основной автор `gramjs <https://github.com/gram-js/gramjs>`__,
Telegram клиента, сделанного на JavaScript.

Kotlin
======

`Kotlogram <https://github.com/badoualy/kotlogram>`__ - это Telegram
реализация, написанная на Kotlin *один из
`оффициальных <https://blog.jetbrains.com/kotlin/2017/05/kotlin-on-android-now-official/>`__
языков для
`Android <https://developer.android.com/kotlin/index.html>`__) от
`@badoualy <https://github.com/badoualy>`__, в настоящий момент
в бете, но работает.

Language-Agnostic
=================

`Taas <https://www.t-a-a-s.ru/>`__ - это сервис, который позволяет вам использовать Telegram API с
любым HTTP клиентом через API. Используя tdlib, Taas - коммерчиский сервис, но предоставляет бесплатный доступ,
если Вы делаете менее 5000 запросов в месяц.

PHP
===

Реалицазия на PHP доступна благодаря
`@danog <https://github.com/danog>`__ и его проекту
`MadelineProto <https://github.com/danog/MadelineProto>`__, с
прекрасной `онлайн
документацией <https://daniil.it/MadelineProto/API_docs/>`__.

Python
======

Довольно новая (на конец 2017 года) библиотека Telegram, написанная
с нуля на Python от
`@delivrance <https://github.com/delivrance>`__ и его библиотека
`Pyrogram <https://github.com/pyrogram/pyrogram>`__.
На самом деле нет причин выбирать его вместо Telethon,
и было бы немного грустно видеть, как вы уходите, но было бы неплохо знать,
что вы упускаете из библиотеки друг друга, чтобы обе могли быть улучшены.

Rust
====

Библиотека `grammers <https://github.com/Lonami/grammers>`__ сделана
`тем же автором, что и Telethos <https://github.com/Lonami>`__! Если вы
ищите альтернативу Telethon, написанной на Rust, это отличный выбор!


Другая, более старая, реализация (работа в процессе) сделана
`@JuanPotato <https://github.com/JuanPotato>`__ под именем
`Vail <https://github.com/JuanPotato/Vail>`__.
