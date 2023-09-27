## Установка
1. Проверим существование командой:

```zsh
postgresql --version
```

2. Если отсутствует ответ:

```zsh
sudo pacman -S postgresql
```

(Так же рекомендую установить и настроить текстовый редактор)

3. Перезагрузка
4. Проверим работу postgres командой
```zsh
systemctl status postgresql
```

Должно ответить:

```zsh
postgresql.service - PostgreSQL database server
Loaded: loaded (/usr/lib/systemd/system/postgresql.service; disabled; vendor preset: disabled)
Active: ==inactive (dead)==
```

5. Логинимся в postgres как супер-пользователь:
```zsh
sudo su - postgres
```

6. Находясь в интерфейсе postgres, инициализируем хранилище баз данных:
```zsh
sudo initdb --locale en_US.UTF-8 -D /var/lib/postgres/data
```

7. Выходим из интерфейса postgres 
```zsh
exit
```

8. Начинаем работу postgres
```zsh
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

9. Проверяем работу сервера:
```zsh
systemctl status postgresql
```

Должен вернуть примерно:
```zsh
postgresql.service - PostgreSQL database server 
	Loaded: loaded (/usr/lib/systemd/system/postgresql.service; enabled; vendor preset: disabled)
	Active: ==active (running)== since Wed 2021-09-08 15:14:51 UTC; 9s ago
Main PID: 6214 (postgres)
Tasks: 7 (limit: 4682)
Memory: 15.6M
CGroup: /system.slice/postgresql.service ├─6214 /usr/bin/postgres -D /var/lib/postgres/data ├─6217 "postgres: checkpointer " "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ├─6218 "postgres: background writer " "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ├─6219 "postgres: walwriter " "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ├─6220 "postgres: autovacuum launcher " "" "" "" "" "" "" "" "" "" "" "" "" "" ├─6221 "postgres: stats collector " "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" └─6222 "postgres: logical replication launcher " "" "" "" "" 

Sep 08 15:14:50 arch-linux systemd[1]: Starting PostgreSQL database server... Sep 08 15:14:51 arch-linux postgres[6214]: 2021-09-08 15:14:51.132 UTC [6214] LOG: starting PostgreSQL 13.3 on x86_64-pc-linux-gnu, compiled by gcc (GCC) 11.1.0, 64-bit
Sep 08 15:14:51 arch-linux postgres[6214]: 2021-09-08 15:14:51.136 UTC [6214] LOG: listening on IPv6 address "::1", port 5432
Sep 08 15:14:51 arch-linux postgres[6214]: 2021-09-08 15:14:51.136 UTC [6214] LOG: listening on IPv4 address "127.0.0.1", port 5432
Sep 08 15:14:51 arch-linux postgres[6214]: 2021-09-08 15:14:51.138 UTC [6214] LOG: listening on Unix socket "/run/postgresql/.s.PGSQL.5432"
Sep 08 15:14:51 arch-linux postgres[6216]: 2021-09-08 15:14:51.145 UTC [6216] LOG: database system was shut down at 2021-09-08 15:14:30 UTC
Sep 08 15:14:51 arch-linux postgres[6214]: 2021-09-08 15:14:51.154 UTC [6214] LOG: database system is ready to accept connections
Sep 08 15:14:51 arch-linux systemd[1]: Started PostgreSQL database server.
```

10. Если хотим залогиниться как супер пользователь `postgres`:
```zsh
sudo su - postgres
```

11. Создадим базу данных:
```sql
CREATE DATABASE testdb;
```

12. Создадим своего пользователя:
```sql
CREATE USER username WITH ENCRYPTED PASSWORD 'password';
```

13. Выходим:
```zsh
\q
```

14. Запускаем бд от имени `user`:
```zsh
psql -U username -d testdb
```
