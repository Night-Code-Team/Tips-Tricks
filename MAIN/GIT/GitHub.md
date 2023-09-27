**Связанные заметки:** [[SSH-Ключи]]

# Подключение SSH

У github есть опция подключаться по ssh, для этого необходимо: 

1) Заходим в терминал:

```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

>[!info] ED25519 не поддерживается!
>Тогда используйте ключ **rsa**:
>
>```shell
>ssh-keygen -t rsa -b 4096 -C "your_email@example.com"```

2) Переименуйте ключ после генерации и защитите его правами 600 или 700
3) Добавьте ключ в **ssh-agent**:

```shell
ssh-add way/to/key
```
