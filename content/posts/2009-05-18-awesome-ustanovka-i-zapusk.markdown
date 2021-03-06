---
title: "awesome: установка и запуск"
date: "2009-05-18T00:00:00+0400"
tags:
  - awesome
  - wm
  - archlinux
keywords: awesome,wm,archlinux,install,config
---
Давно уже собирался, да все никак не мог собраться описать про тайловый менеджер окон awesome, который довольно давно использую в своей практике. А после того, как опробовал довольно большое число различных оконных менеджеров и окружений, могу смело утверждать, что это лучший оконный менеджер!

В этой статье рассмотрим установку и запуск данного оконного менеджера.

Итак, для начала установим awesome и slim. Последний -- это легкий логин-менеджер, который является альтернативой gdm/kdm, но без зависимостей. При этом очень функционален и красив. Даем команду:

```bash
$ yaourt -S awesome slim wicked-git
```

И теперь приступаем к изменению конфигов. Для начала разберемся со slim. Для запуска slim при старте системы существует два способа, описанных в wiki, однако я рекомендую только один -- это прописать slim в строке DAEMONS файла /etc/rc.conf, у меня так:

```text
DAEMONS=(syslog-ng crond hal iptables network !netfs hddtemp sensors pdnsd alsa mpd mpdscribble xinetd preload fam slim)
```

Почему только один? Потому что прописывая slim в пятом уровне запуска получаем затем проблему при работе в первой системной консоли...

При входе в систему slim читает файл ~/.xinitrc и выполняет прописанные там команды, запуская указанный там оконный менеджер. В моем случае он выглядит так:

```bash
#!/bin/bash
#
# .xsession/.xinitrc

export G_FILENAME_ENCODING=@locale
export G_BROKEN_FILENAMES=1

export OOO_FORCE_DESKTOP=gnome
export GDK_USE_XFT=1
export EDITOR=emacsclient

##export LC_CTYPE=ru_RU.utf8
export XMODIFIERS=@im=SCIM
export GTK_IM_MODULE="scim"
export QT_IM_MODULE="scim"
scim -d &amp;

if [ -f ~/.Xmodmap ]; then
xmodmap ~/.Xmodmap
fi

#if [ -f ~/.Xdefaults ]; then
xrdb ~/.Xdefaults
#fi

# D-bus
if which dbus-launch &gt;/dev/null &amp;&amp; test -z "$DBUS_SESSION_BUS_ADDRESS"; then
eval `dbus-launch --sh-syntax --exit-with-session`
fi

xset r rate 350 30
xset -dpms &amp;
xset s off &amp;
xset fp+ /usr/share/fonts/local/
xset fp+ /usr/share/fonts/artwiz-fonts/
xset fp+ ~/.fonts/snap-11/
xset fp rehach

eval `cat ~/.fehbg` &amp;
thunar --daemon &amp;
autocutsel -f &amp;
lomoco -g &amp;
unclutter -idle 20 &amp;

xcompmgr -a &amp;
exec ck-launch-session awesome
```

В самом начале файла задаются основные переменные, необходимые для корректной работы ряда программ, затем запускается SCIM (переключение раскладок), считываются файлы конфигураций клавиатуры и xorg, запускается d-bus, устанавливаются основные параметры работы xorg (мне удобнее это делать тут, чем менять конфигурацию самих иксов). Затем запускаются основные программы, которые используются в качестве сервисов, это установка фона, демон автоматического монтирования, синхронизация буфера обмена, демон мыши логитех и скрытие курсора мыши, после чего запускаем композитный менеджер и вызываем наш любимый awesome.

Обращаю внимание на то, что для корректной работы awesome с `policykit` необходимо при запуске использовать `ck-launch-session`.

С запуском более или менее разобрались, теперь переходим к самому интересному, это к настройке самого авесома. По сути после установки можно уже его запускать и посмотреть на его возможности со стандартными настройками. По мне -- не очень удобно.

Создаем в домашней папке каталог `~/.config/awesome` и копируем туда файл `/etc/xdg/awesome/rc.lua`, после чего начинаем его усиленно менять.

Можно сразу создать в этом же каталог папку `~/.config/awesome/themes/` и скопировать туда файл `/usr/share/awesome/themes/sky/theme` переименовав его при этом в sky. Дефолтная тема темная, а sky наоборот -- светлая... После копирования можно изменять саму тему, изменяя содержимое файла. В основном тут задаются шрифты, основное окружение, его цвета. Не забываем создать строку:

```lua
wallpaper_cmd = eval `cat ~/.fehbg`
```

если ее еще не было, эта строка дублирует строку установки фона из файла `~/.xinitrc`. Не помешает прописать ее и тут. Понятно, что для использования данной команды должен стоять пакет feh.

С темой более менее разобрались, переходим к правке основного файл конфигурации rc.lua...

Это читаем в следующей статье...
