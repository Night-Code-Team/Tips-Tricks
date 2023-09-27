Это упрощенный модуль для работы с sql базами данных.
Установка не требуется, вшит в python.

Для начала нужно создать соединение с базой данных. Для этого указывается название (а так же и путь расположения, желательно использовать `os.path.join` ).

> [!attention] Внимание! 
> Если `sqlite3` не найдет по пути это название бд, то создаст его сам. Рекомендую проверять наличие `os.path.exists`.

> [!info] Обертка
> Рекомендую оборачивать любую работу с `sqlite3` в блок `try except finally`.  Где в блоке `finally` закрывать соединение с бд.

#### Пример
Вот тестовый блок кода:
```python
import sqlite3

try:
	sqlite_connect = sqlite3.connect('sqlite_python.db')
	cursor = sqlite_connect.cursor()
	print("Успешное подключение к бд")

	# Блок с кодом выполнения

	cursor.close()
except sqlite3.Error as error:
	print("Ошибка в работе с бд", error)
finally:
	if sqlite_connect:
		sqlite_connect.close()
		print("Разорванно соединение с бд")
```

#### Подробно
- `sqlite3.connect()` - выполняет подключение к базе данных, всегда возвращает объект подключения SQLite `Connection`. Не является потокобезопасным - нельзя передавать данные в другой поток.
- `cursor = sqlite_connect.cursor()` - создает объект `Cursor` именно он выполняет заданные команды. Не является потокобезопасным, но можно создавать неограниченное количество курсоров в 1 соединении.
- `cursor.close()` - закрыть курсор во избежании ошибок

#### Команды cursor
- `cursor.execute("запрос")` - выполняет запрос в бд. В качестве аргумента принимает sql-запрос, а возвращает `resultSet` из бд.
- `cursor.executescript(sql_script_file)` - выполняет sql-запрос указанный в файле формата `.sql`, вернет `resultSet` из бд.
- `cursor.fetchall()` - после выполнения метода `execute()` и получения `resultSet` (он будет спрятан в классе `Cursor`, ничего не нужно присваивать) данный метод возвращает результат запроса.

#### Типы данных SQLite3
- `NULL` - значение NULL (python `None`)
- `INTEGER` - числовое значение, занимает 1, 2, 3, 4, 6 и 8 байт в зависимости от величины. (`int`)
- `REAL` - число с плавающей точкой, например 3.14 (`float`)
- `TEXT` - текстовое значение, хранится в кодировке `UTF-8`, `UTF-16BE` или `UTF-16LE` (`str`)
- `BLOB` - бинарные данные, изображение, файлы. Не рекомендуется! (`bytes`)

#### Более подробный пример
Для начала создадим бд:
```python
import sqlite3

try:
	sqlite_connect = sqlite3.connect('sqlite_python.db')
	cursor = sqlite_connect.cursor()
	print("Успешное подключение к бд")

	sql_create = '''CREATE TABLE just_for_test (
				 id INTEGER PRIMARY KEY,
				 name TEXT NOT NULL,
				 email TEXT NOT NULL UNIQUE,
				 date datetime);'''

	cursor.execute(sql_create)
	sqlite_connect.commit()
	cursor.close()
	print("Таблица создана")
except sqlite3.Error as error:
	print("Ошибка в бд", error)
finally:
	if sqlite_connect:
		sqlite_connect.close()
		print("Соединение с бд разорванно")
```

1. `sql_create` - формат создания таблицы
2. `cursor.execute(sql_create)` - выполняем команду создания таблицы
3. `sqlite_connect.commit()` - зафиксировать изменения

Теперь добавим запись:
```python
import sqlite3

try:
	sqlite_connect = sqlite3.connect('sqlite_python.db')
	cursor = sqlite_connect.cursor()
	print("Успешное подключение к бд")

	sql_insert = f'''INSERT INTO just_for_test (id, name, email, date)
				  VALUES (1,
				  'MaxBro',
				  'maxbro',
				  {datetime.date.today()});'''

	cursor.execute(sql_insert)
	sqlite_connect.commit()
	cursor.close()
except sqlite3.Error as error:
	print("Ошибка в бд", error)
finally:
	if sqlite_connect:
		sqlite_connect.close()
		print("Соединение с бд разорванно")
```

`sql_insert` - формат вставки в таблицу значений. Также выполняем `execute`, а после `commit` для сохранения

> [!info] Рекомендации
> Рекомендую `str` или `text` значения при ставки указывать в кавычках \'  \'

Теперь заберем значение из бд:
```python
import sqlite3

try:
	sqlite_connect = sqlite3.connect('sqlite_python.db')
	cursor = sqlite_connect.cursor()
	print("Успешное подключение к бд")

	sql_get = '''SELECT * FROM just_for_test WHERE id = 1'''

	cursor.execute(sql_get)
	print(cursor.fetchall())
	cursor.close()
except sqlite3.Error as error:
	print("Ошибка в бд", error)
finally:
	if sqlite_connect:
		sqlite_connect.close()
		print("Соединение с бд разорванно")
```

Вывод:
```zsh
Успешное подключение к бд
[(1, 'MaxBro', 'maxbro', 1985)]
Соединение с бд разорванно
```

1. `sql_get` - составляем SELECT - запрос в бд
2. `cursor.execute(sql_get)` - выполняем запрос в бд
3. `print(cursor.fetchall())` - выводим запрос в консоль

> [!info] Заметь!
> `fetchall` возвращает список из строк таблицы, подходящих по условиям. А сама запись является кортежем `tuple`

#### Sqlite script
Sqlite3 поддерживает использования заготовленных .sql скриптов. Для этого, например создадим файл `sqlite_commands.sql` добавим туда sql-запрос в котором просто создадим 2 таблицы:
```sql
CREATE TABLE eat (
id INTEGER PRIMARY KEY,
name TEXT NOT NULL,
price REAL NOT NULL
);

CREATE TABLE drinks (
id INTEGER PRIMARY KEY,
name TEXT NOT NULL,
price REAL NOT NULL
);
```

Заходим в наш python скрипт и используем команду `executescript`:
```python
import sqlite3

try:
	sqlite_connect = sqlite3.connect('sqlite_python.db')
	cursor = sqlite_connect.cursor()
	print("Успешное подключение к бд")

	with open('sqlite_commands.sql', 'r') as f:
		sql_script = f.read()

	cursor.executescript(sql_script)
	cursor.close()
	print("Скрипт успешно выполнен")
except sqlite3.Error as error:
	print("Ошибка в бд", error)
finally:
	if sqlite_connect:
		sqlite_connect.close()
		print("Соединение с бд разорванно")
```

#### Исключения
Все ниже описанные исключения являются подклассами `Exeption` 
- `sqlite3.Warning` - обычное предупреждение, можно игнорировать.
- `sqlite3.Error` - базовый подкласс для остальных исключений.
- `sqlite3.DatabaseError` - исключение, сообщающие об ошибках базы данных. Например, если мы откроем файл как sql-база, который не является ею (`.txt`).
- `sqlite3.IntegrityError` - подкласс `sqlite3.DatabaseError`, вызывается если возникают проблемы с отношениями в бд. Например не проходит проверка ключа.
- `sqlite3.ProgrammingError` - подкласс `sqlite3.DatabaseError`, вызывается если программист допустил ошибку. Например, попытался создать таблицу уже с существующим названием.
- `sqlite3.OperationalError` - подкласс `sqlite3.DatabaseError`, вызывается если возникает проблемы в работе бд. Например, разорвалось соединение с бд или проблемы с источником данных.
- `sqlite3.NotSupportedError` - исключение, появляющиеся при использовании неподдерживаемых бд API.

#### Получить список всех изменений
Может быть полезно узнать сколько изменений в бд было сделано, для этого используется команда `Connection.total_changes` - укажет число измененных строк. Обрати внимание: применять метод нужно до закрытия соединения с бд.

На примере предыдущего примера, возьмем только блок `finally`:
```python
finally:
	if sqlite_connect:
		print(sqlite_connect.total_changes)
		sqlite_connect.close()
```

#### Резервное копирование БД
Копирование базы данных осуществляется командой `Connection.backup()`
```python
Connection.backup(target, *, pages=0, progress=None, name="main", sleep=0.250)
```

Параметры:
- `target` - наша цель, куда скопировать данные.
- `pages` - если этот параметр равен 0 или отрицательному числу, база копируется за 1 шаг. Если больше нуля вызывается цикл и копирование идет с заданным количеством страниц за раз.
- `progress` - что сделать чтобы показать прогресс, подробнее в примере ниже.
- `name` - указывает название бд куда совершается копирование (можно и не указывать)
- `sleep` - время между попытками сохранения бд, параметр может быть как целочисленным, так и дробным.

Пример:
```python
def progress(status, remaining, total):
	print(f'Скопировано {total-remaining} из {total}...')


try:
	sqlite_main = sqlite3.connect('sqlite_python.db')
	sqlite_backup = sqlite3.connect('sqlite_python_backup.db')
	print("Успешное соединение с main и backup")

	with sqlite_backup:
		sqlite_main.backup(sqlite_backup, pages=3, progress=progress)

	print("Резервное копирование выполнено успешно")

except sqlite3.Error as error:
	print("Ошибка в бд", error)

finally:
	if sqlite_main or sqlite_backup:
		sqlite_main.close()
		sqlite_backup.close()
		print("Соединение с бд разорванно")
```

1. Подключаемся к двум бд 1 - основная, 2 - наш бекап.
2. Запускаем метод `backup` нашей главной бд в блоке `with`
3. В параметры передаем переменную нашего соединения с бекап бд
4. Указываем что пройдем в 3 страницы
5. Для отметки прогресса создадим функцию, выписывающую в консоль сколько осталось сделать.
6. Запускаем

> [!attention] Осторожно!
> В примере в блоке `finally` я для упрощения не разделил блок `if`. Рекомендуется для каждого `Connection` делать отдельный блок `if`

