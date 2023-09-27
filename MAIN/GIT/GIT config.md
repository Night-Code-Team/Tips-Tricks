## Глобальный конфиг Gitа

#### Где найти конфиг и настройка пользователя
Для настройки конфига открываем терминал или открываем файл `.gitconfig` (Windows: `C:\username\.gitconfig` | Linux: `~/.gitconfig`)

Пример настройки `.gitconfig`, если файл пуст, то он не настроен:

```git
[user]
	email = useremail@example.com
	name = UserName
[color]
	ui = true
	status = auto
	branch = auto
[core]
	editor = nvim
[init]
	defaultBranch = master
```

#### Настройка пользователя

Для начала нужно в терминал ввести 2 команды, отвечающие за имя и почту (если имя состоит из 2 и более частей, то оборачивается в двойные кавычки `"`):

```zsh
git config --global user.name "My Name"
git config --global user.email myEmail@example.com
```

#### Настройка редактора по умолчанию:
При работе с гитом нередко будет открываться текстовый редактор. Изначально это vim и если хотите его перенастроить, введите команду:

```zsh
git config --global core.editor name_of_editor
```

Если вы хотите добавить опции для запуска редактора, которые он поддерживает, откройте двойные кавычки, введите название (если он встроен в окружение PATH, если нет то используйте прямой путь до исполняемого файла в одинарных кавычках, далее через пробел впишите опции). Кстати в Windows нужно указывать полный путь:

```zsh 
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

#### Название ветки по умолчанию
Если вы хотите изменить название ветки при инициализации, используйте команду:

```zsh
git config --global init.defaultBranch name_of_branch
```

#### Добавим цветов в консоль

Это не обязательный пункт, но если есть желание, используем набор команд:

```zsh
git config --global color.ui true
git config --global color.status auto
git config --global color.branch auto
```

