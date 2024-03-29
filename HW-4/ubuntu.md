1. Отобразить все процессы в системе.
   ```bash
   ps -eH | less
   ```
    - но лучше так, показывает втом числе и отключенные процессы:
   
   ```bash
   ps -aux | less
   ```
2. Запустить бесконечный процесс в фоновом режиме используя nohup.
   ```bash
   nohup ping 8.8.8.8 &
   ```
   
3. Убедиться, что процесс продолжил работу после завершения сессии.
   ```bash
   ps -aux | grep ping  #проверяем после перелогина по ssh
   ```
   - видим что процесс еще выполняется:  
   ```console
   root@ubuntu-tms:/home/novik# ps -aux | grep ping
   root        1047  0.1  0.0   7720  1316 ?        S    11:50   0:00 ping 8.8.8.8
   root        1186  0.0  0.1   6480  2228 pts/1    S+   11:51   0:00 grep --color=auto ping
   ```
   
4. Убить процесс, запущенный в фоновом режиме.
   ```bash
   kill 1047   # id процесса можно получить через ps
   ```
   - результат:
   ```console
   root@ubuntu-tms:/home/novik# ps -aux | grep ping
   root        1047  0.1  0.0   7720  1316 ?        S    11:50   0:00 ping 8.8.8.8
   root        1186  0.0  0.1   6480  2228 pts/1    S+   11:51   0:00 grep --color=auto ping
   root@ubuntu-tms:/home/novik# kill 1047
   root@ubuntu-tms:/home/novik# ps -aux | grep ping
   root        1188  0.0  0.1   6480  2400 pts/1    S+   11:52   0:00 grep --color=auto ping
   root@ubuntu-tms:/home/novik#
   ```
5. Написать свой сервис под управлением systemd, добавить его в автозагрузку (можно использовать процесс из п.2).
   - мой сервис будет неприрывно пинговать 8.8.8.8:
   ```console
   touch my-services.sh   #скрипт для выполнения ping 8.8.8.8
   chmod +x my-services.sh   #права на запуск
   nano /etc/systemd/system/my-services.service    #создаем сервисный файл
   ```
   - в my-services.service пишем:
   ```bash
   [Service]
   ExecStart=/home/novik/my-services.sh  
   User=root
   Group=root

   [Install]
   WantedBy=multi-user.target   #уровень запуска
   ```
   - запуск и проверка:
   ```console
   systemctl start my-services    # запуск сервиса
   systemctl enable my-services   # включение автозагрузки сервиса, если ошибок нет - значит все работает
   ```
   
6. Посмотреть логи своего сервиса.
   - посмотреть текущее состояние:
     
    ```console  
    root@ubuntu-tms:/home/novik# systemctl status my-services
      ● my-services.service
        Loaded: loaded (/etc/systemd/system/my-services.service; enabled; vendor preset:>
        Active: active (running) since Mon 2024-02-19 12:32:22 UTC; 22min ago
      Main PID: 619 (my-services.sh)
         Tasks: 2 (limit: 2220)
        Memory: 1.8M
           CPU: 1.686s
        CGroup: /system.slice/my-services.service
                ├─619 /bin/bash /home/novik/my-services.sh
                └─630 ping 8.8.8.8
      Feb 19 12:54:45 ubuntu-tms my-services.sh[630]: 64 bytes from 8.8.8.8: icmp_seq=1341 >
      Feb 19 12:54:46 ubuntu-tms my-services.sh[630]: 64 bytes from 8.8.8.8: icmp_seq=1342   
    ```


   - посмотреть логи сервиса:
   ```console
   journalctl -eu my-services.service
   ```
7. (**) Написать скрипт, который выводит следующую информацию (можно переиспользовать предыдущее дз):  
   - кол-во процессов запущенных из под текущего пользователя
   - load average за 15 минут
   - кол-во свободной памяти
   - свободное место в рутовом разделе /
     ```bash
      #!/bin/bash
      
      sumps=$(ps -U $USER | wc -l)
      echo 'Количество процессов запущенных от' $USER '=' $sumps
      
      la=$(cat /proc/loadavg | awk '{print $3}')
      echo 'Load Average за последние 15 минут =' $la
      
      mem=$(free -m | awk '/Mem/ {print $4}')
      echo 'Свободная ОЗУ:' $mem'Мб'
      
      hdd=$(df -h | awk '/\/$/{print $4}')
      echo 'Свободное место в рутовом разделе / =' $hdd
     ```
   - рузультат:
     ```console
      novik@ubuntu-tms:~$ ./hw4.7.sh
      Количество процессов запущенных от novik = 17
      Load Average за последние 15 минут = 0.00
      Свободная ОЗУ: 1263Мб
      Свободное место в рутовом разделе / = 14G
      novik@ubuntu-tms:~$
     ```
8. (**) Добавить в cron задачу, которая будет каждые 10 минут писать в файл результаты выполнения скрипта из п.7
   - создадим скрипт для запуска предыдущего скрпта):
     ```bash
     #!/bin/bash
      date >> /home/novik/sysstat.log    #записывае в лог время
      
      /bin/bash /home/novik/hw4.7.sh >> /home/novik/sysstat.log 
      
      echo -e "\n" >> /home/novik/sysstat.log    #добавляем пустую строку, для удобства
     ```
   - в консоли вводим `crontab -e` и добавляем строчку:
     ```bash
     */10 * * * * /home/novik/monit.sh
     ```
9. (***) Сделать п. 5 для Prometheus Node Exporter
    - копируем в браузере ссылку, скачиваем в консоли через winget:
      `wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz`
    - распаковываем архив `tar xvfz node_exporter-1.7.0.linux-amd64.tar.gz`
    - копируем содержимое юнита из п.5, меням только путь к исполняемому файлу. Запускаем юнит.
      <details><summary>Результат</summary>
      
        ![image](https://github.com/tms-dos21-onl/sergey-novik/assets/77771829/f7804dc8-d3a0-49d4-8e62-87503f3178df)


      </details>
      <br>

** и *** не обязательны к выполнению. Задачи на интерес
