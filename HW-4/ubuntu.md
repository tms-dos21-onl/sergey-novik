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
   
6. Убить процесс, запущенный в фоновом режиме.
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
8. Написать свой сервис под управлением systemd, добавить его в автозагрузку (можно использовать процесс из п.2).
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
   WantedBy=multi-user.target   #указывает после чего запускается наш сервис
   ```
   
   
10. Посмотреть логи своего сервиса.
    
12. (**) Написать скрипт, который выводит следующую информацию (можно переиспользовать предыдущее дз):  
- кол-во процессов запущенных из под текущего пользователя
- load average за 15 минут
- кол-во свободной памяти
- свободное место в рутовом разделе /
  
8. (**) Добавить в cron задачу, которая будет каждые 10 минут писать в файл результаты выполнения скрипта из п.7
9. (***) Сделать п. 5 для Prometheus Node Exporter

** и *** не обязательны к выполнению. Задачи на интерес
