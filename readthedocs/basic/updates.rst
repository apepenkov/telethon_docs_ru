=======
События
=======

События - важная тема в платформе обмена сообщениями, такой как Telegram.
В конце концов, вы хотите получать уведомления при получении нового сообщения,
при входе нового участника, когда кто-то печатает и т.п.
Для этоко используйте **events**.

.. important::

    Советуется использовать logging при работе с событиями,
    так как ошибки в обработчиках событий спрятаны по умолчанию.
    Пожалуйста, добавьте следующий фрамент кода на верхнюю строчку
    вашего файла:

    .. code-block:: python

        import logging
        logging.basicConfig(format='[%(levelname) 5s/%(asctime)s] %(name)s: %(message)s',
                            level=logging.WARNING)


Начиная
=======

Начнем с примера автоматизации ответов:

.. code-block:: python

    from telethon import TelegramClient, events

    client = TelegramClient('anon', api_id, api_hash)

    @client.on(events.NewMessage)
    async def my_event_handler(event):
        if 'hello' in event.raw_text:
            await event.reply('hi!')

    client.start()
    client.run_until_disconnected()


Этот код небольшой, но могут быть некоторые неясности.
Давайте разберемся:

.. code-block:: python

    from telethon import TelegramClient, events

    client = TelegramClient('anon', api_id, api_hash)


Это обычное создание (конечно, укажите имя сессии, API ID и hash).
Ничего такого, чего мы еще не знаем.

.. code-block:: python

    @client.on(events.NewMessage)


Этот декоратор Python прикрепится к функции ``my_event_handler``
и это практически значит *on* a `NewMessage
<telethon.events.newmessage.NewMessage>` *event*,
callback-функция, которую вы собираетесь определить, будет названа:

.. code-block:: python

    async def my_event_handler(event):
        if 'hello' in event.raw_text:
            await event.reply('hi!')


Если `NewMessage
<telethon.events.newmessage.NewMessage>` событие происходит,
и ``'hello'`` - это текст сообщения, мы `reply()
<telethon.tl.custom.message.Message.reply>` событию
сообщением ``'hi!'``.

.. note::

    Обработчики событий **должны** быть ``async def``. В конце концов,
    Telethon - это асинхронная библиотека, основанная на `asyncio`,
    что является более безопасным и часто более быстрым подходом к потокам.

    Вы **должны** ``await`` все вызовы методов, которые используют
    сетевые запросы, которых больше всего.


Больше примеров
===============

Отвечать на сообщения приветствием - это весело, но можем ли мы сделать больше?

.. code-block:: python

    @client.on(events.NewMessage(outgoing=True, pattern=r'\.save'))
    async def handler(event):
        if event.is_reply:
            replied = await event.get_reply_message()
            sender = replied.sender
            await client.download_profile_photo(sender)
            await event.respond('Saved your photo {}'.format(sender.username))

Мы также можем получать только ответы. Это событие фильтрует исходящие сообщения
(только те, которые мы отправляем, вызовут метод), затем мы фильтруем
регулярным выражением ``r'\.save'``, который будет соответствовать сообщениям,
начинающимся с ``".save"``.

Внутри метода мы проверяем, отвечает ли событие на другое сообщение, или нет.
Если это так, мы получаем ответное сообщение и отправителя этого сообщения,
и загружаем фото их профиля.

Удалим сообщения, которые содержат «heck». У нас не допускается ругань.

.. code-block:: python

    @client.on(events.NewMessage(pattern=r'(?i).*heck'))
    async def handler(event):
        await event.delete()


С помощью регулярного выражения ``r'(?i).*heck'`` мы сравниваем без учета регистра
"heck" в любом месте сообщения. Регулярные выражения очень мощные, и вы
можете узгать больше https://regexone.com/.

Пока что мы видели только `NewMessage
<telethon.events.newmessage.NewMessage>`, но куда больше
будет рассмотрено позже. Это лишь небольшое введение в события.

Сущности
========

Если вам нужен пользователь или чат, где произошло событие, вы **должны**
использовать следующие методы:

.. code-block:: python

    async def handler(event):
        # Правильно
        chat = await event.get_chat()
        sender = await event.get_sender()
        chat_id = event.chat_id
        sender_id = event.sender_id

        # НЕ ПРАВИЛЬНО. не делайте так.
        chat = event.chat
        sender = event.sender
        chat_id = event.chat.id
        sender_id = event.sender.id

События похожи на сообщения, но не содержат всей информации, которую имеет сообщение!
Когда вы получаете сообщение вручную, оно будет содержать всю необходимую информацию.
Когда вы получаете обновление о сообщении, оно **не** будет содержать всю
информацию, поэтому вам нужно **использовать методы**, а не свойства.

Прежде чем продолжить, убедитесь, что вы понимаете приведенный здесь код!
Как правило, помните, что события нового сообщения ведут себя просто
как объекты сообщений, так что вы можете делать с ними все, что можете
делать с объектом сообщения.
