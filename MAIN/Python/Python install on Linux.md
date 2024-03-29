На linux нельзя так просто проставить exe-файл и чтобы безопасно его поставить нужно пройтись по командам:

Потребуется установить пакет **make**.
```zsh
sudo pamac build make
```

Так же стоит проверить существование пакета питона с нашим названием:
```zsh
python3.11 -V
```
Если эта команда что-то вернет питон нужно будет переименовать!!!

Можно узнать расположение пакета командой:
```zsh
which python3.11
```

1. Скачиваем Source Code Python последней версии с официального сайта
2. В папке "Загрузки" получаем архив **Python-x.x.x.tar.xz**
3. Открываем консоль и переходим в раздел загрузок
```zsh
cd Загрузки
```

4. Вводим команду чтобы распаковать архив
```zsh
tar -xvf Python-x.x.x.tar.xz
```

5. Проверяем что файл распакован, в загрузках должна появиться папка с таким же названием
6. Переходим в новую папку
```zsh
cd Python-x.x.x.tar.xz
```

7. Проверяем наличие `./configure` командой `ls -a`
8. Вводим настройки конфигурации. Префикс `--prefix=` указывает путь куда будет установлен пакет. Рекомендую сторонний софт ставить в папку `/opt`
```zsh
./configure --prefix=/opt/python3.11 --enable-optimizations
```

9. Собираем и проводим настройку питона. Вместо X указываем количество ядер к которым будет допуск у пакета
```zsh
make -jX
```

10. Если все успешно:
```zsh
sudo make altinstall
```

11. Добавляем установленный питон к модулю PATH
```zsh
nano ~/.zshrc
```

12. В конце добавляем строку:
```nano
export PATH=/opt/python3.11/bin:$PATH
```
