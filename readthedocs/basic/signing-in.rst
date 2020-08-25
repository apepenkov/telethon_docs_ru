.. _signing-in:

====
Вход
====

Перед тем, как работать с Telegram API, вам нужно получить собственный API ID и API HASH:

1. `Войдите в ваш телеграм аккаунт в <https://my.telegram.org/>`_ с
   номером телефона вашей используемой учетной записи.

2. Нажмите на 'API Development tools'.

3. Появится окно *Create new application*. Заполните поля.
   Никаких *URL* вводить не нужно, а только первые два
   поля (*App title* и *Short name*).

4. В конце нажмите *Create application*. Помните, что ваш
   **API hash является секретным**, и Telegram не позволит вам его отозвать.
   Не делитись им!

.. note::

   Эти API ID и API HASH используются *вашим приложением*, а не вашим
   телефонным номером. Вы можете использовать эту комбинацию API ID и API HASH с *любым* номером телефона
   или даже с учетными записями ботов.


Редактирование кода
===================

Это небольшое введение для новичков в программировании на Python в целом.


Мы напишем наш код внутри ``hello.py``, так что вы можете использовать любой текст
редактор, который вам нравится. Чтобы запустить код, используйте ``python3 hello.py`` из
терминала.

.. important::

   Не называйте свой скрипт ``telethon.py``! Python попытается импортировать
   клиент оттуда, и произойдёт ошибка по типу:
   «ImportError: cannot import name 'TelegramClient' ...».


Вход в аккаунт
==============

Наконец-то мы можем написать код для входа в нашу учетную запись!

.. code-block:: python

    from telethon import TelegramClient

    # Используйте свои собственные значения из my.telegram.org
    api_id = 12345
    api_hash = '0123456789abcdef0123456789abcdef'

    # Первый параметр - это имя файла .session (разрешены абсолютные пути)
    with TelegramClient('anon', api_id, api_hash) as client:
        client.loop.run_until_complete(client.send_message('me', 'Hello, myself!'))


В первой строке мы импортируем имя класса, чтобы мы могли создать экземпляр
клиента. Затем мы определяем переменные для хранения нашего API ID и API hash для удобства.

Затем создайте `TelegramClient <telethon.client.telegramclient.TelegramClient>`
и назовите его ``client``. Теперь мы можем использовать переменную client
для всего, что мы хотим сделать, например для отправки сообщения самим себе.

.. note::

    Так как Telethon - асинхронная библиотека, нам нужно ``await``
    функции сопрограмм (далее - coroutine functions) чтобы они сработали
    (или делать цикл до зовершения). Однако в этом небольшом примере
    мы не будем делать ``async def main()``.

    Прочитайте :ref:`mastering-asyncio` Чтобы узнать больше.


Использование блока ``with`` - предпочтительный способ использования библиотеки. оно будет
автоматически `start() <telethon.client.auth.AuthMethods.start>` клиент,
входя или регистрируясь при необходимости.

Если ``.session`` файл уже существует, то вход не потребуется еще раз,
так что учтите это при переименовывании/перемещении файла!


Вход в качестве бота
====================

Вы также можете использовать Telethon для своих ботов (обычных учетных записей ботов, а не пользователей).
Вам по-прежнему понадобится API-ID и hash, но процесс очень похож:


.. code-block:: python

    from telethon.sync import TelegramClient

    api_id = 12345
    api_hash = '0123456789abcdef0123456789abcdef'
    bot_token = '12345:0123456789abcdef0123456789abcdef'

    # Мы должны вручную вызвать "start", если нам нужен точный токен бота.
    bot = TelegramClient('bot', api_id, api_hash).start(bot_token=bot_token)

    # Теперь мы можем использовать client как обычно
    with bot:
        ...


Чтобы получить бот-аккаунт, вам надо написать
`@BotFather <https://t.me/BotFather>`_.


Вход с прокси
=============

Если вам нужно использовать прокси для доступа к Telegram,
вам понадобится  `утсановить PySocks`__ и изменить:

.. code-block:: python

    TelegramClient('anon', api_id, api_hash)

на

.. code-block:: python

    TelegramClient('anon', api_id, api_hash, proxy=(socks.SOCKS5, '127.0.0.1', 4444))

(есстественно, замените IP и порт на IP и порт вашего прокси).

Аргумент ``proxy=`` должен быть tuple, list или dict,
сотстоящий из параметров описанных в `использовании PySocks`__.

.. __: https://github.com/Anorov/PySocks#installation
.. __: https://github.com/Anorov/PySocks#usage-1


Использовние MTProto прокси
===========================

Прокси MTProto - это альтернатива Telegram обычным прокси,
и они работают немного иначе. Доступны следующие протоколы:

* ``ConnectionTcpMTProxyAbridged``
* ``ConnectionTcpMTProxyIntermediate``
* ``ConnectionTcpMTProxyRandomizedIntermediate`` (предпочитается)

На данный момент вам необходимо вручную указать эти специальные режимы подключения.
Если вы хотите использовать прокси MTProto. Ваш код будет выглядеть так:

.. code-block:: python

    from telethon import TelegramClient, connection
    #   мы должны изменить connection    ^^^^^^^^^^

    client = TelegramClient(
        'anon',
        api_id,
        api_hash,

        # Используйте один из доступных режимов подключения.
        # Обычно этот работает с большинством прокси.
        connection=connection.ConnectionTcpMTProxyRandomizedIntermediate,

        # Затем передайте данные прокси в виде tuple:
        #     (хост, порт, прокси secret)
        #
        # Если у прокси нет secret, secret должен быть:
        #     '00000000000000000000000000000000'
        proxy=('mtproxy.example.com', 2002, 'secret')
    )

В будущих обновлениях мы можем упростить использование прокси MTProto.
(например, избежание необходимости вручную передавать ``connection=``).

Короче говоря, тот же код выше, но без комментариев, чтобы было понятнее:

.. code-block:: python

    from telethon import TelegramClient, connection

    client = TelegramClient(
        'anon', api_id, api_hash,
        connection=connection.ConnectionTcpMTProxyRandomizedIntermediate,
        proxy=('mtproxy.example.com', 2002, 'secret')
    )
