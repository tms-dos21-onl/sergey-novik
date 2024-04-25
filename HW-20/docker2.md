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
   PS C:\Users\s.novik> docker run -d -v db:/var/lib/mysql --env MARIADB_ROOT_PASSWORD=1234 mariadb:11.0 
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

    mysql> CREATE TABLE IF NOT EXISTS poets
    -> (poet_id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    -> poet_name VARCHAR(100) NOT NULL);
    Query OK, 0 rows affected (0.04 sec)
    
    mysql> SHOW TABLES;
    +------------------+
    | Tables_in_testdb |
    +------------------+
    | poets            |
    +------------------+
    1 row in set (0.00 sec)    
    mysql> INSERT INTO poets (poet_name)
    -> VALUES('Александр Сергеевич Пушкин'),
    ->       ('Михаил Юрьевич Лермонтов'),
    ->       ('Сергей Александрович Есенин'),
    ->       ('Анна Андреевна Ахматова');
    Query OK, 4 rows affected (0.00 sec)
    Records: 4  Duplicates: 0  Warnings: 0
    
    mysql> SELECT * FROM poets;
    +---------+------------------------------------------------------+
    | poet_id | poet_name                                            |
    +---------+------------------------------------------------------+
    |       1 | Александр Сергеевич Пушкин                           |
    |       2 | Михаил Юрьевич Лермонтов                             |
    |       3 | Сергей Александрович Есенин                          |
    |       4 | Анна Андреевна Ахматова                              |
    +---------+------------------------------------------------------+
    4 rows in set (0.00 sec)
    
    ```
- Выполнить апгрейд MariaDB путем подмены версии используемого Docker образа на 11.1.2.
    ```
     #стопаю контейнер и запускаю новый 
     PS C:\Users\s.novik> docker run -d -v db:/var/lib/mysql --env MARIADB_ROOT_PASSWORD=1234 mariadb:11.1.2
     PS C:\Users\s.novik> docker exec -it 1cad638ddfa5 /bin/bash    
    ```
- Проверить, что версия MariaDB поменялась.
  ```
  root@1cad638ddfa5:/# mysql -u root -p
     mysql> SELECT VERSION();
        +---------------------------------------+
        | VERSION()                             |
        +---------------------------------------+
        | 11.1.2-MariaDB-1:11.1.2+maria~ubu2204 |
        +---------------------------------------+
        1 row in set (0.00 sec)
  ```
- Проверить, что данные остались.
    ```
    mysql> SHOW DATABASES;
        +--------------------+
        | Database           |
        +--------------------+
        | information_schema |
        | mysql              |
        | performance_schema |
        | sys                |
        | testdb             |
        +--------------------+
        5 rows in set (0.01 sec)
    
  mysql> USE testdb;
        Database changed
        mysql> SELECT * FROM poets;
        +---------+------------------------------------------------------+
        | poet_id | poet_name                                            |
        +---------+------------------------------------------------------+
        |       1 | Александр Сергеевич Пушкин                           |
        |       2 | Михаил Юрьевич Лермонтов                             |
        |       3 | Сергей Александрович Есенин                          |
        |       4 | Анна Андреевна Ахматова                              |
        +---------+------------------------------------------------------+
        4 rows in set (0.00 sec)
```
