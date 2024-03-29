#### Нормализация
Нормализация - это процесс приведение к 1 из нормальных форм. Фактически это "создание в бд здравого смысла", избавление от избыточности данных, создание правильных связей между полями и таблицами.

#### Избыточность данных
Это перегрузка данных, хранение одних и тех же данных несколько раз в 1 таблице и другие. Это приводит к проблемам обработки и изменения таких бд, а так же к увеличению занимаемого места.

Пример:

id | name | age | city
-- | ---- | ---- | ----
1 | Anna | 18 | Moscow
2 | Oleg | 34 | Moscow
3 | Jack | 24 | NY
4 | Denis | 15 | Moscow
5 | Sasha | 56 | Moscow
6 | Dasha | 35 | NY

В этом примере избыточностью можно назвать поле `city` так как в случае изменения названия города нам придется редактировать всю таблицу, а не 1 поле, если мы вынесем название городов в другую таблицу.

#### Денормализация БД
Процесс обратный нормализации, то есть наоборот создать избыточность данных. Иногда это может быть наоборот полезным, упрощая работу с бд и уменьшает занимаемое пространства на диске. Да отдельная таблица с id городов может занимать больше места чем несколько раз записать название города в бд. Все очень относительно.

#### Декомпозиция БД
Это процесс разбиения 1 таблицы на несколько. Часто считается процессом **нормализации**, хотя таковым не является.
При декомпозиции мы обычно заменяем строковые значения в отдельные отношения. Это может иметь проектные основания, но связь с нормализацией только в том что вводит **транзитивные зависимости**. 
Этот процесс часто приводит к излишнему усложнению многих запросов.

> [!info] Транзитивные зависимости
> Зависимость, двух атрибутов между собой через третий атрибут. A > B и B > C


#### Типы нормальных форм:
- Первая нормальная форма
- Вторая нормальная форма
- Третья нормальная форма
- Нормальная форма Бойса-Кодда
- Четвертая нормальная форма
- Пятая нормальная форма
- Доменно-ключевая нормальная форма
- Шестая нормальная форма

Форм много, но фактически говорят только о первых трех.

#### Реляционная модель
Определение нормальных форм есть в терминах реляционных моделей:
- Отношения - таблицы
- Атрибуты - поля таблицы
- Кортежи - строки таблицы

## Первая нормальная форма
Правила:
- нет дублирующихся строк
- все атрибуты **атомарны**
- нет повторяющихся атрибутов с одинаковым смыслом

> [!info] Атомарные данные (Atomic data)
> В базах данных это атрибуты, которые хранят единственное значение и не являются списками или множествами значений.

Пример:

os | phone
-- | -----
Android | Google Pixel 4, Mi A4, Google Pixel 6
iOS | iPhone15

Эта таблица не является нормальной по первой форме так как атрибут `Android` содержит список. 

Привидением в нормальную форму можно назвать вот такое решение задачи:

os | phone
-- | -----
Android | Google Pixel 4
Android | Mi A4
Android | Google Pixel 6
iOS | iPhone15

> [!attention] Декомпозиция
> В данном примере, использование привело бы только к усложнению использования таблицы.

## Вторая нормальная форма
Правила:
- отношение находится в первой нормальной форме
- есть первичный ключ
- все не ключевые атрибуты функционально зависят от ключа целиком, но не от его части

Пример:

owner | phone | screen_size
----- | ----- | -----------
Max | Google Pixel 6 | 6.7
Max | iPhone 13 | 6.06
Anna | i Phone 24 | 10

Таблица не является нормальной по второй форме так как атрибут **screen_size** не зависит от **owner** а лишь относится к атрибуту **phone**.

Тут применение декомпозиции является адекватным решением проблемы, соответственно мы разбиваем эту таблицу на две где у первой первичный ключ будет **owner** и атрибут **phone**, а у второй будет первичным ключом будет **phone** и атрибут **screen_size**.

owner | phone
----- | -----
Max | Google Pixel 6
Max | iPhone 13
Anna | i Phone 24

phone | screen_size
----- | -----------
Google Pixel 6 | 6.7
iPhone 13 | 6.06
i Phone 24 | 10

## Третья нормальная форма
Правила:
- Отношение находится во второй нормальной форме
- Неключевые атрибуты напрямую зависят только от первичного ключе, но не от других атрибутов

phone | os | os_maintainer
----- | -- | -------------
Google Pixel 6 Pro | Android | Google
Google Pixel 7 | Android | Google
Samsung S22 | Android | Google
iPhone13 | iOS | Apple

Разберем отношения атрибутов в данном "отношении":
1. **phone** является основным атрибутом, то есть первичным ключом.
2. **os** это атрибут зависящий от первичного ключа **phone**
3. **os_maintainer** это атрибут зависящий от атрибута **os** и он не как не относится к первичному ключу

То есть в примере образуются **транзитивные отношения атрибутов**, что является нарушением правила третий нормальной формы.

Решением данной проблемы является декомпозиция таблицы:

phone | os
----- | --
Google Pixel 6 Pro | Android
Google Pixel 7 | Android
Samsung S22 | Android
iPhone13 | iOS

os | os_maintainer
-- | -------------
Android | Google
iOS | Apple

## Нормальная форма Бойса - Кодда
Более строгая версия **третьей нормальной формы**.

https://neerc.ifmo.ru/wiki/index.php?title=%D0%9D%D0%BE%D1%80%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5_%D1%84%D0%BE%D1%80%D0%BC%D1%8B:_%D1%82%D1%80%D0%B5%D1%82%D1%8C%D1%8F_%D0%B8_%D0%91%D0%BE%D0%B9%D1%81%D0%B0-%D0%9A%D0%BE%D0%B4%D0%B4%D0%B0

## Четвертая нормальная форма
