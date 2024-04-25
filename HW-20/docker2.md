1. Установить Nginx и смонтировать в него:
    ```
    План такой: запустить nginx в docker, посмотреть-забрать конфиг файл nginx, отредактировать его для ssl, примонтировать вместе с ssl;
    ```
- конфигурационный файл с хоста, который выполняет настройку HTTPS для страницы по умолчанию
- директорию с сертификатами
    ```
    docker run -d --name testssl --rm --mount type=bind,source=/c/novik/conf.d,destination=/etc/nginx/conf.d --mount type=bind,source=/c/novik/ssl,destination=/etc/nginx/ssl,ro -p 1234:443 nginx
    ```
2. Запустить 2 Docker контейнера (например, Docker Getting Started и netshoot) с настройками сети по умолчанию и проверить есть ли между ними соединение.
   ```
      docker run -d --rm --name nginxtt nginx
   docker exec -it nginxtt /bin/bash

   root@fb68efb6740f:/# apt-get install net-tools
   root@fb68efb6740f:/# ifconfig
        eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.2  netmask 255.255.0.0  broadcast 172.17.255.255
   _________________________________________________
   
   docker run -d --rm ubuntu/apache2
   docker exec -it 9223b87b845d8 /bin/bash

   root@9223b87b845d:/# apt-get update
   root@9223b87b845d:/# apt-get install net-tools
   root@9223b87b845d:/# ifconfig
        eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.3  netmask 255.255.0.0  broadcast 172.17.255.255

   root@9223b87b845d:/# apt-get install iputils-ping
   root@9223b87b845d:/# ping 172.17.0.2
    PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
    64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.045 ms
    64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.042 ms

   #пингуется, тушим первый контейнер. После этого не пингуется, значит по деволту контейнеры в одной сети и между ними разрешено сетевое взаимодействие
   ```
   
3. Создать именованный Docker volume, который будет использоваться для хранения данных MariaDB. Установить MariaDB версии 11.0 используя ранее созданный volume. Затем:
   ```
   PS C:\Users\s.novik> docker volume create db
   PS C:\Users\s.novik> docker volume ls
    DRIVER    VOLUME NAME
    local     db
   
   PS C:\Users\s.novik> docker pull mariadb:11.0
   PS C:\Users\s.novik> docker run -d --rm -v db:/mnd/base --env MARIADB_ROOT_PASSWORD=1234 mariadb:11.0 
   ```
- Запустить интерактивную сессию Bash в запущенном контейнере при помощи docker exec
    ```
    PS C:\Users\s.novik> docker exec -it 60d31de794b7 /bin/bash
    root@60d31de794b7:/#
    ```
- Проверить версию MariaDB через SQL запрос.
    ```
    root@46455b01b270:/# apt-get update
    root@46455b01b270:/# apt-get install mysql-client
    root@46455b01b270:/# mysql -u root -p
    mysql> SELECT VERSION();
        +---------------------------------------+
        | VERSION()                             |
        +---------------------------------------+
        | 11.0.5-MariaDB-1:11.0.5+maria~ubu2204 |
        +---------------------------------------+
        1 row in set (0.00 sec) 
    ```
- Создать БД, таблицу и запись.
    ```
    mysql> CREATE DATABASE testdb;
    Query OK, 1 row affected (0.00 sec)

    mysql> USE testdb;
    Database changed

    
    ```
- Выполнить апгрейд MariaDB путем подмены версии используемого Docker образа на 11.1.2.
- Проверить, что версия MariaDB поменялась.
- Проверить, что данные остались.
