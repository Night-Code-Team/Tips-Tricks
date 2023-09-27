#### Что это такое?
Используется для упаковывания python приложений в **exe-файл** 

#### Требуется пакет **pyinstaller**

```zsh
python -m pip install --upgrade pyinstaller
```

#### Пример конвертации программы:

```zsh
pyinstaller "Ссылка на скрипт" --onefile --noconsole -i "Где иконка?"
```

#### Дополнительные параметры:
По типу файловой системы:
- `--onefile` - записать все в 1 **exe-file**. Если приложение использует свою внутреннюю память. Например, картинки или конфиги. Это может увеличить время загрузки приложения. Так же эти файлы развертываются в специальную среду, чтобы получить к этому доступ используйте `path_r` из модуля `myfunctools.func.filemanage.path`
```python
def pjoin_r(*args: str) -> str:
    """Соединяет аргументы в путь до файла или папки"""
    return resource_path(join(*args))  


def resource_path(relative_path):
    """ Get absolute path to resource, works for dev and for PyInstaller """
    try:
        # PyInstaller creates a temp folder and stores path in _MEIPASS
        base_path = sys._MEIPASS
    except Exception:
        base_path = abspath(".")
    return join(base_path, relative_path)
```
- `--onedir` - вывод запишется в 1 папку. Файлов очень много, но работает быстро.

#### Без консоли
- `--noconsole` - не показывается консоль

#### Иконка
- `-i "way_to_icon.ico"` - подключить иконку