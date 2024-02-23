1. Распределить основные сетевые протоколы (перечислены ниже) по уровням модели TCP/IP:
  - FTP, HTTP, SSH, RTP, DNS, NTP
  - TCP, UDP
  - ICMP
  
2. Узнать pid процесса и длительность подключения ssh с помощью утилиты ss
   - pid процесса `ss -ltp`:
     ```console
      root@ubuntu-tms:/home/novik# ss -ltp | grep ssh
      LISTEN 0      128        127.0.0.1:6010        0.0.0.0:*    users:(("sshd",pid=1013,fd=9))
      LISTEN 0      128          0.0.0.0:ssh         0.0.0.0:*    users:(("sshd",pid=649,fd=3))
      LISTEN 0      128            [::1]:6010           [::]:*    users:(("sshd",pid=1013,fd=7))
      LISTEN 0      128             [::]:ssh            [::]:*    users:(("sshd",pid=649,fd=4))
     ```
      :heavy_check_mark: PID = 649  
    - длительность подключения `ss -tn -o`:
      ```console
      root@ubuntu-tms:/home/novik# ss -tn -o
      State                Recv-Q                Send-Q                               Local Address:Port                               Peer Address:Port                 Process
      ESTAB                0                     0                                         10.0.2.4:22                                     10.0.2.2:62157                 timer:(keepalive,108min,0)
      ESTAB                0                     48                                        10.0.2.4:22                                     10.0.2.2:62156                 timer:(on,396ms,0)
      root@ubuntu-tms:/home/novik#

      ```
      :heavy_check_mark: Это keepalive, но не продолжительность подключения.
         
   
3. Закрыть все порты для входящих подключений, кроме ssh  
   - в Ubuntu 22 по-умолчанию `ufw` отключен. Добавляем правило для ssh `ufw allow 22/tcp`. Включаем - `ufw enable`. По-умолчанию  ufw находися в "нормально закрытом состоянии" для входящих соединений,
     т.е. все соединеия запрещены кроме явно разрешенных.  
     
4. Установить telnetd на VM, зайти на нее с другой VM с помощью telnet и отловить вводимый пароль и вводимые команды при входе c помощью tcpdump
   - На машине к которой подключаемся устанавливаем telnet `apt install telnetd -y`
   - проверяем его состояние `systemctl status inetd`, открываем порт для входящих соединений `ufw allow 23`
   - подключаемся с другой машины:
     ```console
      root@ubuntu-ansible-srv:/home/novik# telnet 10.0.2.4
      Trying 10.0.2.4...
      Connected to 10.0.2.4.
      Escape character is '^]'.
      Ubuntu 22.04.4 LTS
      ubuntu-tms login: novik
      Password:
      Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-94-generic x86_64)
      ***
      Last login: Fri Feb 23 13:12:49 UTC 2024 from 10.0.2.2 on pts/0
      novik@ubuntu-tms:~$

     ```
6. (***) Открыть порт 222/tcp и обеспечить прослушивание порта с помощью netcat, проверить доступность порта 222 с помощью telnet и nmap.

