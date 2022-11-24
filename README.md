# mctf-emergancy-capture
Misc task for [M*CTF](https://mctf.mtuci.ru) Finals 

## Описание
Наши сенсоры зафиксировали атаку на рабочую станцию в сети. 
Найдите файлы, которые могли быть похищены.

## Решение
Лучше всего начать с открытия пункта Статистика -> Иерархия адресов.
Помимо кучи интернет-адресов у нас есть 4 из одной сети. Их и проверим

![image](/images/addrs.png)

### 10.0.2.3
**10.0.2.3** - это DHCP сервер и от него прилетело всего 2 пакета, не интересно

![image](/images/10_0_2_3.png)

### 10.0.2.15
Дальше по колличеству пакетов идёт **10.0.2.15** . 

![image](/images/10_0_2_15.png)
Здесь видно FTP соединение с **10.0.2.5**, посмотрим его (ПКМ->Follow TCP Stream)

![image](images/ftp_stream.png)

Был скачан некий файл с названием *Activator.zip*. Предположим, что **10.0.2.5** - это рабочая станция. У неё очень много соединений, потому рациональнее перейти к **10.0.2.6**.

### 10.0.2.6
Сразу выберем в качестве источника **10.0.2.5**, a в качестве назначения **10.0.2.6**. Здесь какая-то TCP сессия.
![image](/images/10_0_2_6.png)

Сразу откроем TCP поток. 

![image](/images/shell_log.png)

А здесь у нас лог обратной оболочки с **10.0.2.6**. В нём был прочитан файл secret.txt. Секрет в нём похож на флаг, который нужно "повернуть".

С помощью [Cyberchef](https://gchq.github.io/CyberChef/) и опции ROT13 Bruteforce (чтобы не подбирать по одному) расшифровываем флаг.
![image](/images/rot.png)

## Flag 
```
MCTF{St0l3n_sHe1l}
```
