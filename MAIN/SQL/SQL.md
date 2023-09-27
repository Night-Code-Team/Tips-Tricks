Связанные заметки: [[Postgresql]] [[sqlite3]] [[SQL Нормальные формы]]
Тут используется короткая версия статьи https://habr.com/ru/articles/564390/

## Команды SQL

Команды делятся на 4 раздела
- DDL - язык определения данных (Data Definition Language) - проще создание и работа с таблицами

Команда | Описание
--------- | ----------
СREATE  | Создание таблицы, Представление таблицы или другой объект в БД
ALTER | Модифицирует существующий объект как таблица
DROP | Удаляет существующую таблицу, представление таблицы или другой объект в бд

- DML - язык изменения данных (Data Manipulation Language)

Команда | Описание
--------- | ----------
SELECT  | Выбор значений из одной или нескольких таблиц
INSERT | Добавляет запись
UPDATE | Обновляет запись
DELETE | Удаляет записи

- DCL - язык управления данными (Data Control Language) - это официально, а не официально, управление доступом

Команда | Описание
--------- | ----------
GRANT  | Наделяет пользователя правами
REVOKE | Отменяет права пользователя

- DQL – язык запросов. Вообще команда SELECT больше относится именно к этому разделу, но во многих источниках их ставят в DML

> [!info] Регистр
> В SQL принято именно команды писать заглавными, а название таблиц, данными и другим маленькими. Однако большинство СУБД нечувствительны к регистру.

## Что такое таблица?
Данные хранятся в объектах БД, называемыми таблицами `table`. В 1 бд может быть неограниченное количество таблиц.

id | name | city | age
--|-------|-------|----
1 | Аня | Moscow | 22
2 | Сергей | СПБ | 25

#### Что такое поле?
Таблица состоит из полей (`field`). Поле - это верхняя колонка таблицы (id, name, city, age)

#### Что такое строка?
Строка - `record` или `row` - "единичное вхождение" (`entry`), по факту запись в таблице.

#### Что такое колонка?
Колонка - (`column`) вертикальное вхождение в таблицу. 

## Различие NULL, 0 и  \"\"
**NULL** - это четко нулевое значение, то есть при составлении записи, в колонку не было добавлено какое либо значение. Заметь: пустое - это например пустая строка \"\" но никак не нулевое значение. Что тоже может возникнуть путаница, если мы возьмем 0 - это же является числом. В python представлении sqlite3 `NULL` это тип `None`.

## Ограничения 
Или `constraints` - правила применяемые к данным. Они используются для ограничения данных, которые можно записать в таблицу. Фактически - проверка условий. 
Обычно должны устанавливаться на уровне полей для колонок, но так же можно и на уровне таблиц.

Самые распространенные:
- `NOT NULL` - колонка не может иметь нулевое значение 
- `DEFAULT` - значение колонки по умолчанию
- `UNIQUE` - все значения колонки должны быть уникальными
- `PRIMARY KEY` - первичный или основной ключ, уникальный индикатор записи
- `FOREIGN KEY` - внешний ключ, уникальный идентификатор записи в другой таблице, связанной с текущей
- `CHECK` - все значения колонки должны удовлетворять определенному условию
- `INDEX` - быстрая запись и извлечение данных

Ограничения можно удалить командой ALTER TABLE и DROP CONSTRAINT + название ограничения

## Целостность данных
У каждой СУБД  существуют следующие категории целостности данных:
- целостность объекта (`Entity Integrity`) - в таблице нет дубликатов
- целостность домена (`Domain Integrity`) - фильтрация значений по типу, формату, диапазону
- целостность ссылок (`Referential Integrity`) - строки, которые используются другими записями (например другой таблицей), не могут быть удалены
- целостность, определенная пользователем (`User-Defined Integrity`) - дополнительные правила, созданные пользователями бд

## Нормализация БД
Нормализация - процесс приведения БД к определенному типу форм. 
Тема очень сложная, подробнее в [[SQL Нормальные формы]]

## Синтаксис SQL
Все инструкции SQL должны начинаться с ключевого слова, таких как `SELECT`, INSERT, `UPDATE`, `DELETE`, `ALTER`, `DROP`, `CREATE`, `USE`, `SHOW` и заканчиваться `;` у самого sql это не обязательно, но требуется большинством реализаций СУБД.

#### Выборка `SELECT`:
```sql
SELECT col1, col2, ...colN
FROM tableName;

SELECT DISTINCT col1, col2, ...colN
FROM tableName;

SELECT col1, col2, ...colN
FROM tableName
WHERE condition;

SELECT col1, col2, ...colN
FROM tableName
WHERE condition1 AND|OR condition2;

SELECT col1, col2, ...colN
FROM tableName
WHERE colName IN (val1, val2, ...valN);

SELECT col1, col2, ...colN
FROM tableName
WHERE colName BETWEEN val1 AND val2;

SELECT col1, col2, ...colN
FROM tableName
WHERE colName LIKE pattern;

SELECT col1, col2, ...colN
FROM tableName
WHERE condition
ORDER BY colName [ASC|DESC];

SELECT SUM(colName)
FROM tableName
WHERE condition
GROUP BY colName;

SELECT COUNT(colName)
FROM tableName
WHERE condition;

SELECT SUM(colName)
FROM tableName
WHERE condition
GROUP BY colName
HAVING (function condition);
```

#### Создание таблиц:
```sql
CREATE TABLE tableName (
	col1 datatype,
	col2 datatype,
	...
	colN datatype,
);
```

#### Удаление таблицы
```sql
DROP TABLE tableName;
```

#### Создание индекса
```sql
CREATE UNIQUE INDEX indexName
ON tableName (col1, col2, ...colN);
```

#### Удаление индекса
```sql
ALTER TABLE tableName
DROP INDEX indexName;
```

#### Получаем описание структуры таблицы
```sql
DESC tableName;
```

#### Очистка таблицы
```sql
TRUNCATE TABLE tableName;
```

#### Добавление / удаление / модификация колонки
```sql
ALTER TABLE tableName ADD|DROP|MODIFY colName [datatype];
```

#### Переименование таблицы
```sql
ALTER TABLE tableName RENAME TO newtableName;
```

#### Вставка значений
```sql
INSERT INTO tableName (col1, col2, ...colN)
VALUES (val1, val2, ...valN)
```

#### Обновление записи
```sql
UPDATE tableName 
SET col1 = val1, col2 = val2, ...colN = valN)
[WHERE condition];
```

#### Удаление записи
```sql
DELETE FROM tableName
WHERE condition;
```

#### Создание БД
```sql
CREATE DATABASE [IF NOT EXISTS] dbName;
```

#### Удаление БД
```sql
DROP DATABASE [IF EXISTS] dbName;
```

#### Сохранение
```sql
COMMIT;
```

#### Отмена изменений
```sql
ROLLBACK;
```

## Типы данных

> [!info] Типы данных
> Для каждой реализации СУБД существуют свои типы данных, тут стоит прочитать курс по каждой из них отдельно.

## Операторы

#### Арифметические

Оператор | Описание
-------- | --------
\+ | сложение
\- | вычитание
\* | умножение
\/ | деление
\% | деление возвращающее остаток

#### Сравнение

Оператор | Описание
-------- | --------
\= | равенство
\!= | НЕ равенство значений
\<> | НЕ равенство значений
\> | больше
\< | меньше
\>= | больше или равно
\<= | меньше или равно
\!< | левое не меньше правого
\!> | левое не больше правого

#### Логическое

Оператор | Описание
-------- | --------
ALL | сравнивает все значения
AND | объединяет условия
ANY | сравнивает одно значения с другим если последнее совпадает с условием
BETWEEN | вхождение значения между диапазоном
EXISTS | проверка существования
IN | в списке
LIKE | сравнивает значение с похожими с помощью операторов подстановки
NOT | нет обычно применяется с другими операторами (NOT EXISTS)
OR | одно из условий
IS NULL | ненулевое значение
UNIQUE | определяет уникальность строки

## Выражения

Это комбинация значений, операторов и функций для (оценки) вычисления значений, забора из БД набора данных (применение к данным фильтров).

Таблица **users**

|id|name|age|city|status|
|---|---|---|---|---|
|1|Igor|25|Moscow|active|
|2|Vika|26|Ekaterinburg|inactive|
|3|Elena|27|Ekaterinburg|active|
|4|Oleg|28|Moscow|inactive|

Найдем актив:
```sql
SELECT * FROM users WHERE status = active;
```

|userId|userName|age|city|status|
|---|---|---|---|---|
|1|Igor|25|Moscow|active|
|3|Elena|27|Ekaterinburg|active|

## Создание БД
Для создания используется `CREATE DATABASE`
```sql
CREATE DATABASE dbName;
```
Однако часто видно такой формат:
```sql
CREATE DATABASE IF NOT EXISTS dbName;
```

Список баз данных можно получить командой `SHOW`
```sql
SHOW DATABASES;
```

Если используется несколько баз данных, используется команда `USE`
```sql
USE dbName;
```

## Таблицы SQL

#### Создаем командой:
```sql
CREATE TABLE users (
	userId INT PRIMARY KEY,
	userName VARCHAR(20) NOT NULL,
	age INT NOT NULL,
	city VARCHAR(20),
	status VARCHAR(8),
);
```

Разберем:
1. Создаем таблицу `users`
2. В скобках указываем атрибуты (названия колонок)
3. Около каждой колонки пишем тип переменной
4. Если это необходимо указываем первичный ключ
5. Если строка не должна иметь нулевое значение указываем `NOT NULL`

#### Удаляем командой:
```sql
DROP TABLE users;
```

#### Добавим колонки:
```sql
INSERT INTO users (userId, userName, age, city, status)
VALUES (1, 'Igor', 25, 'Moscow', 'active');
INSERT INTO users (userId, userName, age, city, status)
VALUES (2, 'Vika', 26, 'Ekaterinburg', 'inactive');
INSERT INTO users (userId, userName, age, city, status)
VALUES (3, 'Elena', 27, 'Ekaterinburg', 'active');
```

Или сократим все до 1 операции:
```sql
INSERT INTO users (userId, userName, age, city, status)
VALUES
(1, 'Igor', 25, 'Moscow', 'active'),
(2, 'Vika', 26, 'Ekaterinburg', 'inactive'),
(3, 'Elena', 27, 'Ekaterinburg', 'active');
```

Или используя другую таблицу:
```sql
INSERT INTO tableName [(col1, col2, ...colN)]
	SELECT col1, col2, ...colN
	FROM anotherTable
	[WHERE condition];
```

#### Выборка значений
```sql
SELECT * FROM users;
SELECT userId, userName, age FROM users;
```

- `*` - выводит все колонки
- или можно указать интересующие нас колонки через запятую

#### WHERE
Используется для постановки условий с использованием выше описанных операторов.
```sql
SELECT userId, userName, age
FROM users
WHERE status = 'active';
```

Так же можно указывать несколько условий, например используя `ANY`, `ALL`, `AND`, `OR` и другие:
```sql
SELECT userId, userName, age
FROM users
WHERE status = 'active' AND age >= 25 AND city = 'Moscow'
```

#### Удаляем записи
Удаление записи осуществляется командой `DELETE`
```sql
DELETE FROM users
WHERE status = 'inactive'
```

