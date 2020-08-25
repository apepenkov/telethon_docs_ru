=============
Быстрый старт
=============

Давайте посмотрим на более подробный пример, чтобы изучить некоторые методы,
которые библиотека может предложить. Это так называемые
"удобные методы", и вы всегда должны, по возможности, использовать их.

.. code-block:: python

    from telethon import TelegramClient

    # не забудьте взять свои данные из my.telegram.org!
    api_id = 12345
    api_hash = '0123456789abcdef0123456789abcdef'
    client = TelegramClient('anon', api_id, api_hash)

    async def main():
        # Получение информации о себе
        me = await client.get_me()

        # "me" - объект пользователя. Вы можете получить красивую информацию
        #  об объекте Telethon с помощью метода "stringify":
        print(me.stringify())

        # Когда вы что-то выводите, вы видите репрезентацию.
        # Вы можете получить доступ ко всем атрибутам объектов Telethon с
        # помощью оператора точки. Например, чтобы получить имя пользователя:
        username = me.username
        print(username)
        print(me.phone)

        # Вы можете распечатать все диалоги, в которых вы участвуете:
        async for dialog in client.iter_dialogs():
            print(dialog.name, 'has ID', dialog.id)

        # Вы можете отправлять сообщения себе ...
        await client.send_message('me', 'Hello, myself!')
        # ...к некоторому Chat ID
        await client.send_message(-100123456, 'Hello, group!')
        # ...вашим контактам
        await client.send_message('+34600123123', 'Hello, friend!')
        # ...или даже по имени пользователя
        await client.send_message('username', 'Testing Telethon!')

        # Конечно, вы можете использовать markdown в своих сообщениях:
        message = await client.send_message(
            'me',
            'This message has **bold**, `code`, __italics__ and '
            'a [nice website](https://example.com)!',
            link_preview=False
        )

        # При отправке сообщения возвращается объект отправленного сообщения,
        # который вы можете использовать
        print(message.raw_text)

        # Вы можете отвечать на сообщения напрямую, если у вас есть объект сообщения
        await message.reply('Cool!')

        # Или отправить файлы, песни, документы, альбомы ...
        await client.send_file('me', '/home/me/Pictures/holidays.jpg')

        # Вы можете получить историю сообщений любого чата:
        async for message in client.iter_messages('me'):
            print(message.id, message.text)

            # Вы также можете загружать медиа из сообщений!
            # Метод вернет путь, по которому был сохранен файл.
            if message.photo:
                path = await message.download_media()
                print('File saved to', path)  # выводится после загрузки

    with client:
        client.loop.run_until_complete(main())


Здесь вы увидите, как залогинитсья, получить информацию о себе, отправить
сообщения, файлы, получить чаты, вывести сообщения и загрузить
файлы.

Вы должны убедиться, что понимаете, что делает показанный здесь код,
обратите внимание на то, как методы вызываются и используются и т. д. до
продолжения. Позже мы увидим все доступные методы.

.. important::

    Заметьте, что Telethon - это асинхронная библиотека, т.е. вы должны
    привыкнуть и немного узнать про `asyncio`. Это вам сильно поможет.
    По-быстрому, это значит, что вы должны писать код внутри ``async def``
    по типу:

.. code-block:: python

    client = ...

    async def do_something(me):
        ...

    async def main():
        # Большая часть вашего кода должна быть здесь
        # Конечно, вы можете создать и использовать свой собственный async def (do_something).
        # Ему нужно быть асинхронным только в том случае, если ему нужно await (ждать) чего-то.
        me = await client.get_me()
        await do_something(me)

    with client:
        client.loop.run_until_complete(main())

     После того, как вы это поймете, вы можете использовать Telethon.sync (что не очень хорошая идея),
     (см .: ref: `compatibility-and-comfort`), но учтите, что вы можете столкнуться с другими проблемами
     (iPython, Anaconda и т. д. имеют некоторые проблемы с этим).
