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
<br />
В результате сканирования мы обнаружили: 
- 21 port - FTP (vsftpd 3.0.2)
- 22 port - SSH (OpenSSH 6.7p1)
- 80 port - HTTP (Apache 2.4.10)
<br />
Проверим активный веб-сервис

```sh
http://your_machine_ip
```

![nmap_scan](screenshots/3.png)

[^1]:https://tryhackme.com/room/yearoftherabbit#
