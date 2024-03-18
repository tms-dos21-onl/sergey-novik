1. Вывести в консоль список всех пользователей системы.
   ```console
   cat /etc/passwd
   ```
 
2. Найти и вывести в консоль домашние каталоги для текущего пользователя и root.
   ```console
   root@ubuntu-tms:/app# echo ~
   /root
   root@ubuntu-tms:/app# echo ~novik
   /home/novik
   root@ubuntu-tms:/app#

   ```
3. Создать Bash скрипт get-date.sh, выводящий текущую дату.
   ```console
   novik@ubuntu-tms:~$ nano get-date.sh
   novik@ubuntu-tms:~$ ls -l
   -rw-rw-r-- 1 novik novik  19 Mar 18 13:48 get-date.sh
   novik@ubuntu-tms:~$ chmod +x get-date.sh
   novik@ubuntu-tms:~$ ls -l
   -rwxrwxr-x 1 novik novik  19 Mar 18 13:48 get-date.sh
   novik@ubuntu-tms:~$ ./get-date.sh
   Mon Mar 18 01:49:56 PM UTC 2024
   novik@ubuntu-tms:~$ cat get-date.sh
   #!/bin/bash
   
   date
   novik@ubuntu-tms:~$

   ```
   
4. Запустить скрипт через ./get-date.sh и bash get-date.sh. Какой вариант не работает? Сделать так, чтобы оба варианта работали.
   ```console
   novik@ubuntu-tms:~$ bash get-date.sh
   Mon Mar 18 01:53:28 PM UTC 2024
   novik@ubuntu-tms:~$ ./get-date.sh
   Mon Mar 18 01:53:38 PM UTC 2024
   novik@ubuntu-tms:~$

   #оба варианта сразу работают

   ```
5. Создать пользователей alice и bob с домашними директориями и установить /bin/bash в качестве командной оболочки по умолчанию.
   ```console
   root@ubuntu-tms:/home/novik# useradd  -m alice -s /bin/bash
   root@ubuntu-tms:/home/novik# useradd  -m bob -s /bin/bash
   root@ubuntu-tms:/home/novik#
   root@ubuntu-tms:/home/novik#
   root@ubuntu-tms:/home/novik# ls -l /home/
   total 20
   drwxr-x--- 2 alice    alice    4096 Mar 18 13:59 alice
   drwxr-x--- 2 backend  backend  4096 Mar  7 12:13 backend
   drwxr-x--- 2 bob      bob      4096 Mar 18 13:59 bob
   drwxr-x--- 3 frontend frontend 4096 Mar  7 13:31 frontend
   drwxr-x--- 5 novik    novik    4096 Mar 18 13:49 novik
   root@ubuntu-tms:/home/novik#

   ```
   
6. Запустить интерактивную сессию от пользователя alice. Создать файл secret.txt с каким-нибудь секретом в домашней директории при помощи текстового редактора nano.
   ```console
   root@ubuntu-tms:/home/novik# su - alice
   alice@ubuntu-tms:~$ pwd
   /home/alice
   alice@ubuntu-tms:~$ nano secret.txt

   ```
7. Вывести права доступа к файлу secret.txt.
   ```console
   alice@ubuntu-tms:~$ ls -l
   total 4
   -rw-rw-r-- 1 alice alice 9 Mar 18 14:20 secret.txt
   alice@ubuntu-tms:~$

   ``` 
8. Выйти из сессии от alice и открыть сессию от bob. Вывести содержимое файла /home/alice/secret.txt созданного ранее не прибегая к команде sudo. В случае, если это не работает, объяснить.
   ```console
   
   ``` 
13. Создать файл secret.txt с каким-нибудь секретом в каталоге /tmp при помощи текстового редактора nano.
14. Вывести права доступа к файлу secret.txt. Поменять права таким образом, чтобы этот файл могли читать только владелец и члены группы, привязанной к файлу.
15. Выйти из сессии от bob и открыть сессию от alice. Вывести содержимое файла /tmp/secret.txt созданного ранее не прибегая к команде sudo. В случае, если это не работает, объяснить.
16. Добавить пользователя alice в группу, привязанную к файлу /tmp/secret.txt.
17. Вывести содержимое файла /tmp/secret.txt.
18. Скопировать домашнюю директорию пользователя alice в директорию /tmp/alice с помощью rsync.
19. Скопировать домашнюю директорию пользователя alice в директорию /tmp/alice на другую VM по SSH с помощью rsync. Как альтернатива, можно скопировать любую папку с хоста на VM по SSH.
20. Удалить пользователей alice и bob вместе с домашними директориями.
21. С помощью утилиты htop определить какой процесс потребляет больше всего ресурсов в системе.
22. Вывести логи сервиса Firewall с помощью journalctl не прибегая к фильтрации с помощью grep.
