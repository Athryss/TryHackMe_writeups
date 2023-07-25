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

Наблюдаем скрытую директорию /WExYY2Cv-qU

Переходим к данной директории

![hidden_dir](screenshots/10.1.png)

Мы можем наблюдать в ней изображение с названием Hot_Babe.png, откроем его

![Hot_babe](screenshots/10.png)

Описание не врало, там действительно находится Hot_Babe

## Компрометация машины

Cкачаем данное изображение и проверим его при помощи утилиты strings

```console
strings Desktop/Hot_Babe.png
```

![strings_in](screenshots/11.png)

Вводим команду, на выходе получаем большое количество ненужных данных, листаем вниз, пока не наткнемся на подсказку

![strings_out](screenshots/12.png)

Мы получили username и много паролей, из этого следует, что нужно провести атаку перебором на ssh, для этого будем использовать инструмент hydra

Но для начала создадим словарь со всеми вариантами пароля

Создаем .txt документ (я назвал его rabbit_worldist) и открываем его с помощью nano.
Вставляем туда наши данные, полученные из strings, сохраняем изменения и закрываем

```console
sudo nano Desktop/rabbit_wordlist.txt
```

![strings_out](screenshots/13.png)

Запускаем hydra, выбираем созданный нами словарь

![hydra](screenshots/14.png)

В результате hydra подобрала нам пароль от ftpuser - 5iez1wGXKfPKQ

Попробуем войти при помощи этих данных

![inside](screenshots/15.png)

Мы внутри
Посмотрим что вокруг нас
![strings_out](screenshots/16.png)
![strings_out](screenshots/17.png)
![strings_out](screenshots/18.png)


[^1]:https://tryhackme.com/room/yearoftherabbit#





