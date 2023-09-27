0Wayland изначально блокирует работу с проприетарными драйверами от Nvidia и переключается в лучшем случае на X11.

Чтобы исправить это:

1. Заходим в 
```zsh
sudo nano /etc/gdm/custom.conf
```

2. Раскомментируем и изменяем строку
```nano
WaylandEnable=true
```

3. Разрешаем использовать kms-modifiers
```zsh
gsettings set org.gnome.mutter experimental-features '["kms-modifiers"]'
```
 
 4. Открываем файл
```zsh
sudo nano /etc/mkinitcpio.conf
```

5. Дополняем, если уже занята строку
```nano
MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

6. Обновляем конфиг mkinitcpio
```zsh
sudo mkinitcpio -P
```

7. Открываем файл
```zsh
sudo nano /etc/default/grub
```

8. Дополняем через пробел строку `GRUB_CMDLINE_LINUX_DEFAULT` параметром `nvidia-drm.modeset=1` . Например у меня:
```nano
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash udev.log_priority=3 nvidia-drm.modeset=1"
```

9. Обновляем конфиг grub
```zsh
sudo update-grub
```

Говорят это тоже работает но я не проверял:
```zsh
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

10. Проверяем установку модулей 
```zsh
sudo pacman -Syu --needed xorg-xwayland libxcb egl-wayland
```

11. Перезагружаемся с включенными драйверами от Nvidia

### Если это не помогло
Дополнительно можно убрать файл **Правил** Wayland НЕ РЕКОМЕНДУЕТСЯ! Может привести к багам и ошибкам

```zsh
sudo ln -s /dev/null /etc/udev/rules.d/61-gdm.rules
```
