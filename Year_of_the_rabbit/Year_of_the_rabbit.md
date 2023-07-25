# Year of the Rabbit[^1]
## Questions:
1. What is the user flag? <br />
1. What is the root flag? <br />
## Сканирование
Сканируем машину при помощи Nmap. <br />
```console
nmap -A -sV your_machine_ip
```
![nmap_scan](screenshots/1.png)

В результате сканирования мы обнаружили: 
- 21 port - FTP (vsftpd 3.0.2)
- 22 port - SSH (OpenSSH 6.7p1)
- 80 port - HTTP (Apache 2.4.10)

## Web-ресурсы 

Проверим активный веб-сервис

```sh
http://your_machine_ip
```

![http_check](screenshots/3.png)

Обнаруживаем стандартную страницу Apache, пока-что ничего нас интересующего

Произдведем поиск директорий при помощи gobuster

```console
gobuster dir -r -k -x .php,.txt,.html -r -k --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt --url your_machine_ip
```

![gobuster](screenshots/2.png)

Достаточно быстро находим директорию /assets, проверим её

```sh
http://your_machine_ip/assets
```

![assets_check](screenshots/4.1.png)

В данной директории находятся:
- RickRolled.mp4 - Видеозапись с понятным содержанием, не трогаем пока-что, т.к. не быть закрикроленными
- style.css - CSS документ

Откроем style.css

[^1]:https://tryhackme.com/room/yearoftherabbit#

![style_check](screenshots/4.png)

Наблюдаем  подсказку посмотреть страницу /sup3r_s3cr3t_fl4g.php

Переходим на неё

```sh
http://your_machine_ip/sup3r_s3cr3t_fl4g.php
```

![style_check](screenshots/5.png)

Но что мы тут видим?

![style_check](screenshots/6.png)

Если присмотреться, то можно заметить что происходит переадресация с http://your_machine_ip/sup3r_s3cr3t_fl4g.php на http://your_machine_ip/sup3r_s3cret_fl4g, что достаточно интересно, попробуем изучить данный вопрос повнимательнее при помощи burp suite

Заходим в burp suite, выбираем proxy - intercept и включаем перехват 

```sh
http://your_machine_ip/sup3r_s3cr3t_fl4g.php
```

![burp_1](screenshots/7.png)

Вводим нашу ссылку

![burp_2](screenshots/8.png)

Сначала ничего интересного

![burp_2](screenshots/9.png)

А вот тут уже что-то есть



