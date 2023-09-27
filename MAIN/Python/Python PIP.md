#### Зачем вообще это нужно?
Модуль pip занимается установкой и управлением установленных пакетов
pip уже автоматически должен находиться в системе вместе с питоном при установки, если такого модуля нет, переустанавливаем python.

#### Обновление
PIP часто обновляется и чтобы не переустанавливать каждый раз весь python используем команду:
```zsh
python -m pip install -U pip
```

#### Установка пакета
Чтобы установить  пакет используется:
```zsh
python -m pip install package_name
```

#### Модификаторы команды INSTALL
- `-U` или `--upgrade` - обновить данный пакет до последней версии
-  `-r` или `--requirement` + `requirements.txt`  установить все модули приведенные в файле `requirements.txt`. Например:
```txt
numpy==1.25.1
pandas==2.0.3
```
- `--force-reinstall` - при обновлении, переустановить пакет, даже если он последней версии, помогает если пакет сломан.
- `--no-deps` - не устанавливать зависимости пакета
- `--dry-run` - ничего не установится, но в консоли будет выглядеть как например все установится
- `-I` или `--ignore-installed` - установит пакеты поверх тех что есть в системе

#### Удаление пакета
Чтобы удалить установленный пакет используется:
```zsh
python -m pip uninstall package_name
```

#### Модификаторы команды UNINSTALL
-  `-r` или `--requirement` + `requirements.txt`  удалить все модули приведенные в файле `requirements.txt`. Например:
```txt
numpy
pandas
```
- `-y` или `--yes` - не запрашивать подтверждение на удаление

#### Список всех пакетов
Вывод списка установленных пакетов:
```zsh
python -m pip list
```

#### Узнать информацию по установленным пакетам
Команда:
```zsh
python -m pip show [opt] package_name
```
- `-f` или `--files` - узнать весь список файлов для каждого пакета

#### Вывести список пакетов в формате `requirements.txt`
Команда:
```zsh
python -m pip freeze [opt]
```
- `-r`  или `--requirement` + `requirements.txt`  - вывести в файл

#### Проверить пакеты
Если показалось что пакет сломан можно использовать команду:
```zsh
python -m pip check
```

#### Скачать пакет без зависимостей
Если мы хотим уставить пакет из локального проекта или не устанавливать зависимости.
Команда:
```zsh
python -m pip download
```
-  `-r` или `--requirement` + `requirements.txt`  установить все пакеты приведенные в файле `requirements.txt`
- `--no-deps` - без зависимостей
- `-d` или `-dest` установить пакет в определенную директорию

#### Найти пакет
Чтобы найти пакет содержащий название:
```zsh
python -m pip search name
```

### Комбинации
Как обновить все установленные пакеты?
```zsh
python -m pip freeze > requirements.txt
python -m pip install -r requirements.txt --upgrade
```