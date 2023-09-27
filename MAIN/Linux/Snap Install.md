Изначально в manjaro и других дистребутивах нет snapa и его нужно правильно настроить

```zsh
sudo pamac install snapd libpamac-snap-plugin
sudo systemctl enable --now snapd.socket
sudo ln -s /var/lib/snapd/snap /snap
```

Так же нужно добавить пакеты для Pamac Store

```zsh
sudo pamac build yay
sudo yay -S pamac-snap-plugin
sudo yay -S pamac-flatpak-plugin
```

После установки `snap`:
```zsh
systemctl enable --now snapd.apparmor
```
