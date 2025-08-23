---
title: Python
nav_order: 1
layout: default
---

# Питання з Python
- TOC
{:toc}

## Загальне

<hr>

* [Ключові характеристики Python](general.html#ключові-характеристики-python)
* [Компіляція vs. інтерпретація](general.html#компіляція-vs-інтерпретація)
* [Імперативне vs. декларативне програмування](general.html#імперативне-vs-декларативне-програмування)
* [Статична vs. динамічна типізація](general.html#статична-vs-динамічна-типізація)
* [Що таке інтроспекція?](general.html#що-таке-інтроспекція)
* [Що таке рефлексія?](general.html#що-таке-рефлексія)

## Функції

<hr>
### Загальне

* [Що таке функція в Python?](functions.html#що-таке-функція-в-python)
* [Параметр vs. аргумент](functions.html#параметр-vs-аргумент)
* [Як передаються аргументи у функцію в Python?](functions.html#як-передаються-аргументи-у-функцію-в-python)
* [Які є різновиди параметрів у функціях?](functions.html#які-є-різновиди-параметрів-у-функціях)
* [Як працюють *args і **kwargs?](functions.html#як-працюють-args-і-kwargs)
* [Що таке обов'язкові та необов'язкові параметри/аргументи?](functions.html#що-таке-обовязкові-та-необовязкові-параметриаргументи)
* [Порядок визначення параметрів функції](functions.html#порядок-визначення-параметрів-функції)
* [Навіщо використовуються оператори / та * у списку параметрів?](functions.html#навіщо-використовуються-оператори--та--у-списку-параметрів)
* [Параметри за замовчуванням для змінних типів](functions.html#параметри-за-замовчуванням-для-змінних-типів)

### Вкладені функції, lambda-функції та замикання

* [Що таке вкладена (inner) функція?](functions.html#що-таке-вкладена-inner-функція)
* [Що таке замикання (closure)?](functions.html#що-таке-замикання-closure)
* [Що таке lambda-функція?](functions.html#що-таке-lambda-функція)
* [Чи можна створити замикання за допомогою lambda?](functions.html#чи-можна-створити-замикання-за-допомогою-lambda)
* [Які підводні камені при використанні замикань у циклах?](functions.html#які-підводні-камені-при-використанні-замикань-у-циклах)

### Scopes

* [Що таке область видимості (scope)?](functions.html#що-таке-область-видимості-scope)
* [Порядок розв'язання імен у Python (LEGB)](functions.html#порядок-розвязання-імен-у-python-legb)
* [Навіщо потрібно ключові слова global та nonlocal?](functions.html#навіщо-потрібно-ключові-слова-global-та-nonlocal)

### Decorators

* [Що таке декоратор у Python?](functions.html#що-таке-декоратор-у-python)
* [Як визначити декоратор з аргументами?](functions.html#як-визначити-декоратор-з-аргументами)
* [Який порядок застосування декораторів?](functions.html#який-порядок-застосування-декораторів)
* [Для чого використовувати functools.wraps?](functions.html#для-чого-використовувати-functoolswraps)

### Рекурсія

* [Що таке рекурсія?](functions.html#що-таке-рекурсія)
* [Чи завжди можна рекурсивний виклик замінити циклом?](functions.html#чи-завжди-можна-рекурсивний-виклик-замінити-циклом)

## Протоколи

<hr>
### Загальне

* [Що таке протокол у Python?](protocols.html#що-таке-протокол-у-python)
* [Що таке subtyping у Python?](protocols.html#що-таке-subtyping-у-python)

### Iterators та Iterables

* [Що таке Iterable у Python?](protocols.html#що-таке-iterable-у-python)
* [Що таке Iterator у Python?](protocols.html#що-таке-iterator-у-python)
* [За що відповідає функція next() у Python?](protocols.html#за-що-відповідає-функція-next-у-python)

### Generators

* [Що таке генератор у Python?](protocols.html#що-таке-генератор-у-python)
* [Iterator vs. Iterable vs. Generator](protocols.html#iterator-vs-iterable-vs-generator)
* [Що таке генераторні вирази (generator expressions)?](protocols.html#що-таке-генераторні-вирази-generator-expressions)
* [Що таке yield from і як його використовують?](protocols.html#що-таке-yield-from-і-як-його-використовують)
* [Чи можна перебирати елементи генератора кілька разів?](protocols.html#чи-можна-перебирати-елементи-генератора-кілька-разів)
* [Навіщо потрібні методи send(), throw() і close()?](protocols.html#навіщо-потрібні-методи-send-throw-і-close)

### Context Managers

* [Що таке контекстний менеджер у Python?](protocols.html#що-таке-контекстний-менеджер-у-python)
* [Навіщо потрібні контекстні менеджери?](protocols.html#навіщо-потрібні-контекстні-менеджери)
* [Які стандартні приклади контекстних менеджерів є у Python?](protocols.html#які-стандартні-приклади-контекстних-менеджерів-є-у-python)
* [Як обробляються винятки всередині блоку with?](protocols.html#як-обробляються-винятки-всередині-блоку-with)
* [Що таке витік пам'яті?](protocols.html#що-таке-витік-памяті)
* [У чому різниця між 'try/finally' та контекстного менеджера ('with') у Python?](protocols.html#у-чому-різниця-між-tryfinally-та-контекстного-менеджера-with-у-python)
* [Що таке асинхронний контекстний менеджер?](protocols.html#що-таке-асинхронний-контекстний-менеджер)

### Sequences та Mappings

* [Що таке Sequence у Python?](protocols.html#що-таке-sequence-у-python)
* [Для чого потрібен клас collections.abc.Sequence?](protocols.html#для-чого-потрібен-клас-collectionsabcsequence)
* [Чим відрізняється Sequence від MutableSequence у Python?](protocols.html#чим-відрізняється-sequence-від-mutablesequence-у-python)
* [Що таке Mapping у Python?](protocols.html#що-таке-mapping-у-python)
* [Чим відрізняється Mapping від MutableMapping у Python?](protocols.html#чим-відрізняється-mapping-від-mutablemapping-у-python)