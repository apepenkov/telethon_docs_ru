=====
Тесты
=====

Telethon использует `Pytest <https://pytest.org/>`__, для тестов, `Tox
<https://tox.readthedocs.io/en/latest/>`__ для установки окружения, и
`pytest-asyncio <https://pypi.org/project/pytest-asyncio/>`__ с `pytest-cov
<https://pytest-cov.readthedocs.io/en/latest/>`__ для asyncio
`coverage <https://coverage.readthedocs.io/>`__ интеграции.

Хотя чтение полной документации по ним, вероятно, является хорошей идеей,
там много текса, поэтому для удобства ниже приводится краткое описание этих инструментов.

Краткое введение в Pytest
=========================

`Pytest <https://pytest.org/>`__  - это инструмент для обнаружения и запуска тестов Python,
также он допускает многократное модульное использование кода настройки теста с
использованием приспособлений.

Большинство тестов Pytest будут выглядеть примерно так::

    from module import my_thing, my_other_thing

    def test_my_thing(fixture):
        assert my_thing(fixture) == 42

    @pytest.mark.asyncio
    async def test_my_thing(event_loop):
        assert await my_other_thing(loop=event_loop) == 42

Обратите внимание:

 1. Тест импортирует конкретную функцийю. Цель модульных тестов - тестировать,
    работает ли реализация модуля, например функции или класса.

    It's role is not so much to test that components interact well with each
    other. I/O, such as connecting to remote servers, should be avoided. This
    helps with quickly identifying the source of an error, finding silent
    breakage, and makes it easier to cover all possible code paths.

    System or integration tests can also be useful, but are currently out of
    scope of Telethon's automated testing.

 2. A function ``test_my_thing`` is declared. Pytest searches for files
    starting with ``test_``, classes starting with ``Test`` and executes any
    functions or methods starting with ``test_`` it finds.

 3. The function is declared with a parameter ``fixture``. Fixtures are used to
    request things required to run the test, such as temporary directories,
    free TCP ports, Connections, etc. Fixtures are declared by simply adding
    the fixture name as parameter. A full list of available fixtures can be
    found with the ``pytest --fixtures`` command.

 4. The test uses a simple ``assert`` to test some condition is valid.  Pytest
    uses some magic to ensure that the errors from this are readable and easy
    to debug.

 5. The ``pytest.mark.asyncio`` fixture is provided by ``pytest-asyncio``. It
    starts a loop and executes a test function as coroutine. This should be
    used for testing asyncio code. It also declares the ``event_loop``
    fixture, which will request an ``asyncio`` event loop.

Brief Introduction to Tox
=========================

`Tox <https://tox.readthedocs.io/en/latest/>`__ is a tool for automated setup
of virtual environments for testing. While the tests can be run directly by
just running ``pytest``, this only tests one specific python version in your
existing environment, which will not catch e.g. undeclared dependencies, or
version incompatabilities.

Tox environments are declared in the ``tox.ini`` file. The default
environments, declared at the top, can be simply run with ``tox``. The option
``tox -e py36,flake`` can be used to request specific environments to be run.

Brief Introduction to Pytest-cov
================================

Coverage is a useful metric for testing. It measures the lines of code and
branches that are exercised by the tests. The higher the coverage, the more
likely it is that any coding errors will be caught by the tests.

A brief coverage report can be generated with the ``--cov`` option to ``tox``,
which will be passed on to ``pytest``. Additionally, the very useful HTML
report can be generated with ``--cov --cov-report=html``, which contains a
browsable copy of the source code, annotated with coverage information for each
line.
