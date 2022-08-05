#### Утилита для контроля активности сервиса tor

#nyx #tor #linux 

Запускаем командой:
`$ nyx –i 9051`
9051 – порт управления, открывается в конфиг.файле `/etc/tor/torrc`
Там же устанавливаем пароль на подключение к интерфейсу управления Tor, предварительно сделав хэш пароля:
`$ tor —hash-password password`
Копируем результат выполнения команды и вставляем его в конфиг.
Главная кнопочка в интерфейсе nyx – клавиша m – вход в меню, откуда можно многое настроить.



#### Мосты в tor

#tor #torbridges #bridges #linux 

Некоторые провайдеры Интернет-услуг блокируют работу Tor. Они могут использовать различные подходы, например, блокировать подключения ко всем IP сети Tor либо анализируя трафик и, если он определяется как принадлежащий сети Tor, блокируют его.
Для обхода такой блокировки можно использовать ретрансляторы. Мост – один из разновидностей ретрансляторов. Поскольку известны не все IP адреса мостов, то есть шанс обойти блокировку Tor’а.
Необходимо получить список актуальных IP адресов. Для этого перейдите по ссылке: https://bridges.torproject.org/options
Выберите obfs4 и нажмите кнопку «Получить мосты!»
Вам будет дан список из трёх адресов. Скопируйте одну из этих строк.
Другим способом получения мостов является отправка электронного письма на адрес bridges@torproject.org. Пожалуйста, обратите внимание на то, что вы должны отправить электронный запрос с использованием одного из перечисленных сервисов: Riseup, Gmail или Yahoo.
Установите необходимые пакеты.
В производных Debian (Kali Linux, Linux Mint, Ubuntu) это делается командой:
$ sudo apt install tor obfs4proxy

В производных Arch Linux (BlackArch, Manjaro) это делается командой:
$ sudo pacman -S tor obfs4proxy

К слову, obfs4proxy нашёлся только в AUR.

Добавляем к файлу `/etc/tor/torrc` следующие строки:

```
ClientTransportPlugin obfs4 exec /usr/bin/obfs4proxy

Bridge obfs4 188.165.0.32:8443 DE67F5AF37709959703D106218D2593A210F86DB cert=kzLn5Wop1AkuYHHCFtcobVtTI3fmoVYX3i3+tBPyUtIL6Fa9B/l5PUMxyr2X8D1di/noRg iat-mode=0

Bridge obfs4 142.47.109.194:8443 924B9FEBC61D1D9835477C7932EC13D18E3E7FA4 cert=HjV8Ss2QZbKe7bmDu1otyfxhSWCGXiKP4VXv3qdKWXKYCDif7XZSZHfH090LdE4dpCAaFQ iat-mode=0

Bridge obfs4 87.233.197.123:4444 8C233B8732BFC22F40956A5E9290A80987CEFB96 cert=42D6Lz7QAF21baswmm53PZ9tLaOhAn1DEGRZKS220ejKZoDHnypYxOPr/FoNNI3SpY8SJQ iat-mode=0

UseBridges 1
```

Далее надо перезапустить Tor:
`$ systemctl restart tor`



#### Запуск и использование minidlna

#multimedia #dlna #minidlna #video #linux 

Запуск сервиса (службы, демона) minidlna
`$ sudo systemctl start minidlna`
Проверить состояние демона minidlna можно на странице http://localhost:8200/ или командой:
`$ sudo systemctl status minidlna`
Файл конфигурации: `/etc/minidlna.conf`
Файл базы данных: `/var/cache/private/minidlna/files.db`

http://itadept.ru/linux-dlna-server-minidlna/
https://clsv.ru/linux/ustanovka_i_nastrojka_minidlna_16



#### Не монтируется раздел NTFS

#ntfs #readonly #mount #linux 

https://zalinux.ru/?p=3305
`$ sudo umount /dev/sda2`
`$ sudo ntfsfix /dev/sda2`
`$ sudo mount /dev/sda2 /run/media/m0d/DATA`



#### Жадный билайн побороли сменой TTL

#ttl #linux 

1. Добавляем новое правило в iptables:
`$ sudo iptables -t mangle -A POSTROUTING -j TTL --ttl-set 65`

2. Правим файл:
   `$ kate /etc/sysctl.d/99-sysctl.conf`

3. Добавляем в этот файл строку:
   `net.ipv4.ip_default_ttl=65`

4. Перезагружаемся?

Результат: из модема (телефона) вылетают пакеты с TTL=64, билайн не определяет, что интернет раздаётся на другое устройство.



#### Не работает микрофон на гарнитуре

#sound #audio #microphone #headset #linux

_Ситуация: работает только встроенный микрофон ноутбука, гарнитура, подключаемая в совмещённый 4-pin аудиоразъём ноутбука, работает только на воспроизведение, микрофона не видит._

Что было сделано:
- установил alsa-tools: `$ sudo pacman -S alsa-tools`
- пробовал переназначать пины в коннекторе `$ hdajackretask`

__Решение:__
1. определил название аудиокарты и подключённых к ней кодеков:
    `$ cat /proc/asound/card*/codec* | grep Codec`

	Результат был такой:

	```
	Codec: Nvidia GPU 94 HDMI/DP  
	Codec: Realtek ALC295
	```

2. Создал файл:
   `$ sudo touch /etc/modprobe.d/alsa-base.conf`
   
3. Добавил туда строку:
   `options snd-hda-intel model=alc295-headset,dell-headset-multi`

4. Перезагрузился.

__Результат:__
В микшере громкости (pavucontrol) на вкладке 'Устройства Ввода' появилась возможность выбрать 'Headset Mictophone' вместо 'Internal Microphone', и микрофон гарнитуры заработал!

[Источник](https://superuser.com/questions/1312970/headset-microphone-not-detected-by-pulse-und-alsa)



#### Расстановка переносов
#typograph #office #libreoffice 

_Ситуация: открываю .docx файл, а оно пишет, мол доустановите пакет словаря для автоматической расстановки переносов (не дословно)._

__Решение:__ через pamac доустановден пакет hyphen-ru.



#### Добавление шрифтов
#typograph #office #libreoffice 

_Ситуация: надо добавить ttf-шрифты в linux._

__Решение:__
`$ mkdir ~/.fonts`
`$ cp /папка_со_шрифтами/windows/Fonts/*.ttf ~/.fonts` ← скопировал все файлы ttf во вновь сознанную папку .fonts
`$ fc-cache -f -v` ← обновил кэш информации о шрифтах в системе


#### Цвета текста в консоли
#LS_COLORS #dircolors #zsh #bash

_Ситуация: в консоли, в выводе команды ls, цвета некоторых папок из-за цветовой схемы нечитаемы._

__Решение:__
1. Вначале правим файл `/usr/share/zsh/manjaro-zsh-config`, где в секции `# File and Dir colors for ls and other outputs` (в самом конце) закоментируем строку `eval "$(dircolors -b)"` и за ней запишем следующее:
```bash
   eval `dircolors ~/.dir_colors`
```
2. Далее правим файл `~/.dir_colors`, в котором находим нужные строки и меняем в них цифровые коды цветов. Соответственно, для начала делаем резервную копию этого файла.

2. Чтобы проверить получившийся результат:
```bash
   $ eval \`dircolors ~/.dir_colors\`
```


#### KDE Cursor switches Style when pointing on Desktop
#kde #cursor

_Ситуация: курсор мыши над рабочим столом KDE и над заголовками окон меняется на курсор из темы KDE_Сlassic._

__Решение__
1. Копируем файл /usr/share/icons/default/index.theme в /home/m0d/.icons/default/
2. Правим /home/m0d/.icons/default/index.theme, где в секции
   [icon theme] правим параметр `Inherits=macOSBigSur`
   где macOSBigSur — название каталога с темой курсоров в каталоге /usr/share/icons/
   
   _Примечание: тема с курсорами может лежать и в каталоге ~/.local/share/icons/, в этом случае делаем символьную ссылку на интересующий нас каталог с темой курсоров в /usr/share/icons/_