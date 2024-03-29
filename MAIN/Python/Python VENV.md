#### Зачем вообще это нужно?
Виртуальная среда Python - это изолированная рабочая среда. 

Преимущества:
- Локальная установка модулей
- Экспорт рабочей среды
- Выполнение кода внутри окружения

Создать виртуальное окружение командой
```zsh
python3.11 -m venv name_of_venv
```

где:
- `python3.11` - рабочая версия питона
- `-m venv` - использование модуля venv
- `name_of_venv` - название виртуальной среды, обычно используется стандартное название `venv`

#### Активация
Linux:
```zsh
source name_of_venv/bin/activate
```
Windows:
```pws
name_of_venv/scripts/activate
```

#### Выход из виртуальной среды
Linux & Windows:
```zsh
deactivate
```

#### Экспорт пакетов
При использовании виртуальной среды python можно удобно экспортировать данные о установленных модулях, командой:
```zsh
python3.11 -m pip freeze > requirements.txt
```
