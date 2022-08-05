#linux #linuxadmin
#### Настраиваем Syncthing. Синяя изолента в мелком бизнесе и дома
https://habr.com/ru/post/350892/

Подобный софт не должен работать от имени root. Все созданные в результате синхронизации файлы должны принадлежать локальному пользователю. Поэтому есть два варианта — автоматический запуск как системный сервис либо в качестве пользовательского сервиса. Второй вариант будет работать только тогда, когда пользователь залогинился через ssh или авторизировался в локальной системе. Нас интересует системный демон и для этого лучше всего подойдет глубоко любимый общественностью systemd. Пути могут немного отличаться в разных дистрибутивах. Данный мануал применим к Debian и Ubuntu 16.04 Server. Для начала создаем юнит:  
  

```
sudo nano /etc/systemd/system/syncthing@.service
```

  
И вносим туда следующее содержимое:  

```
[Unit]  
Description=Syncthing - Open Source Continuous File Synchronization for %I  
Documentation=man:syncthing(1)  
After=network.target  
Wants=syncthing-inotify@.service  
  
[Service]  
User=%i  
ExecStart=/usr/bin/syncthing -no-browser -no-restart -logflags=0  
Restart=on-failure  
SuccessExitStatus=3 4  
RestartForceExitStatus=3 4  
  
[Install]  
WantedBy=multi-user.target  
``` 
  
Теперь остается лишь активировать сервис от имени нужного пользователя и можно настраивать ноду.  
  

```
sudo systemctl enable syncthing@username.service
sudo systemctl start syncthing@username.service
```

`SRSST7U-UXSDLJH-TUAEQGK-PXVGWUP-4W7UE2I-UPL2CI2-YRDYQHG-PDDBFQL`
